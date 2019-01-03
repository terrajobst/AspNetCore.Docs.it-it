---
title: Host ASP.NET Core in Windows con IIS
author: guardrex
description: Informazioni su come ospitare app ASP.NET Core in Windows Server Internet Information Services (IIS).
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: host-and-deploy/iis/index
ms.openlocfilehash: 4356d986731f915c2e76a4c4863f951572820de0
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637878"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>Host ASP.NET Core in Windows con IIS

Di [Luke Latham](https://github.com/guardrex)

[Installare il bundle di hosting .NET Core](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a>Sistemi operativi supportati

Sono supportati i sistemi operativi seguenti:

* Windows 7 o versione successiva
* Windows Server 2008 R2 o versioni successive

Il [server HTTP.sys](xref:fundamentals/servers/httpsys) (chiamato in precedenza WebListener) non funziona in una configurazione proxy inverso con IIS. È necessario usare il [server Kestrel](xref:fundamentals/servers/kestrel).

Per informazioni sull'hosting in Azure, vedere <xref:host-and-deploy/azure-apps/index>.

## <a name="supported-platforms"></a>Piattaforme supportate

Sono supportate le app pubblicate per la distribuzione a 32 bit (x86) e a 64 bit (x64). Distribuire un'app a 32 bit, a meno che l'app:

* Non richieda lo spazio indirizzi di memoria virtuale più grande disponibile per un'app a 64 bit.
* Non richieda le dimensioni maggiori dello stack IIS.
* Non abbia dipendenze native a 64 bit.

## <a name="application-configuration"></a>Configurazione dell'applicazione

### <a name="enable-the-iisintegration-components"></a>Abilitare i componenti IISIntegration

::: moniker range=">= aspnetcore-2.1"

Un file *Program.cs* tipico chiama <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> per avviare la configurazione di un host:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Un file *Program.cs* tipico chiama <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> per avviare la configurazione di un host:

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

**Modello di hosting in-process**

`CreateDefaultBuilder` chiama il metodo `UseIIS` per avviare [CoreCLR](/dotnet/standard/glossary#coreclr) ed eseguire l'hosting dell'app nel processo di lavoro IIS (*w3wp.exe* o *iisexpress.exe*). I test delle prestazioni indicano che l'hosting di un'app .NET Core in-process offre una velocità effettiva delle richieste significativamente superiore rispetto all'hosting dell'app out-of-process o all'inoltro delle richieste al server [Kestrel](xref:fundamentals/servers/kestrel).

**Modello di hosting out-of-process**

Per l'hosting out-of-process con IIS, `CreateDefaultBuilder` configura il server [Kestrel](xref:fundamentals/servers/kestrel) come server Web e abilita l'integrazione di IIS configurando il percorso base e la porta per il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

Il modulo ASP.NET Core genera una porta dinamica da assegnare al processo back-end. `CreateDefaultBuilder` chiama il metodo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*>. `UseIISIntegration` configura Kestrel per l'ascolto sulla porta dinamica all'indirizzo IP localhost (`127.0.0.1`). Se la porta dinamica è 1234, Kestrel è in ascolto su `127.0.0.1:1234`. Questa configurazione sostituisce le altre configurazioni URL fornite da:

* `UseUrls`
* [API Listen di Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [Configurazione](xref:fundamentals/configuration/index) (o [opzione URL della riga di comando](xref:fundamentals/host/web-host#override-configuration))

Le chiamate a `UseUrls` o all'API `Listen` di Kestrel non sono necessarie quando si usa il modulo. In caso di chiamata di `UseUrls` o `Listen`, Kestrel resta in ascolto sulla porta specificata solo quando si esegue l'app senza IIS.

Per altre informazioni sui modelli di hosting in-process e out-of-process, vedere [Modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) e [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

::: moniker-end

::: moniker range="= aspnetcore-2.1"

`CreateDefaultBuilder` configura il server [Kestrel](xref:fundamentals/servers/kestrel) come server Web e abilita l'integrazione di IIS configurando il percorso base e la porta per il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

Il modulo ASP.NET Core genera una porta dinamica da assegnare al processo back-end. `CreateDefaultBuilder` chiama il metodo [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration). `UseIISIntegration` configura Kestrel per l'ascolto sulla porta dinamica all'indirizzo IP localhost (`127.0.0.1`). Se la porta dinamica è 1234, Kestrel è in ascolto su `127.0.0.1:1234`. Questa configurazione sostituisce le altre configurazioni URL fornite da:

* `UseUrls`
* [API Listen di Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [Configurazione](xref:fundamentals/configuration/index) (o [opzione URL della riga di comando](xref:fundamentals/host/web-host#override-configuration))

Le chiamate a `UseUrls` o all'API `Listen` di Kestrel non sono necessarie quando si usa il modulo. In caso di chiamata di `UseUrls` o `Listen`, Kestrel resta in ascolto sulla porta specificata solo quando si esegue l'app senza IIS.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

`CreateDefaultBuilder` configura il server [Kestrel](xref:fundamentals/servers/kestrel) come server Web e abilita l'integrazione di IIS configurando il percorso base e la porta per il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

Il modulo ASP.NET Core genera una porta dinamica da assegnare al processo back-end. `CreateDefaultBuilder` chiama il metodo [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration). `UseIISIntegration` configura Kestrel per l'ascolto sulla porta dinamica all'indirizzo IP localhost (`localhost`). Se la porta dinamica è 1234, Kestrel è in ascolto su `localhost:1234`. Questa configurazione sostituisce le altre configurazioni URL fornite da:

* `UseUrls`
* [API Listen di Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [Configurazione](xref:fundamentals/configuration/index) (o [opzione URL della riga di comando](xref:fundamentals/host/web-host#override-configuration))

Le chiamate a `UseUrls` o all'API `Listen` di Kestrel non sono necessarie quando si usa il modulo. In caso di chiamata di `UseUrls` o `Listen`, Kestrel resta in ascolto sulla porta specificata solo quando si esegue l'app senza IIS.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Includere una dipendenza dal pacchetto [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) nelle dipendenze dell'applicazione. Usare il middleware di integrazione IIS aggiungendo il metodo di estensione [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) a [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder):

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

[UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) e [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) sono entrambi obbligatori. Il fatto che il codice chiami `UseIISIntegration` non influisce sulla portabilità del codice stesso. Se l'app non viene eseguita dietro IIS (ad esempio se viene eseguita direttamente in Kestrel), `UseIISIntegration` non esegue alcuna operazione.

Il modulo ASP.NET Core genera una porta dinamica da assegnare al processo back-end. `UseIISIntegration` configura Kestrel per l'ascolto sulla porta dinamica all'indirizzo IP localhost (`localhost`). Se la porta dinamica è 1234, Kestrel è in ascolto su `localhost:1234`. Questa configurazione sostituisce le altre configurazioni URL fornite da:

* `UseUrls`
* [Configurazione](xref:fundamentals/configuration/index) (o [opzione URL della riga di comando](xref:fundamentals/host/web-host#override-configuration))

Non è necessaria una chiamata a `UseUrls` quando si usa il modulo. In caso di chiamata di `UseUrls`, Kestrel resta in ascolto sulla porta specificata solo quando si esegue l'app senza IIS.

Se `UseUrls` viene chiamato in un'app ASP.NET Core 1.0, chiamarlo **prima** della chiamata di `UseIISIntegration` in modo che la porta configurata del modulo non venga sovrascritta. Questo ordine di chiamata non è obbligatorio con ASP.NET 1.1 Core perché l'impostazione del modulo ha la precedenza su `UseUrls`.

::: moniker-end

Per altre informazioni sull'hosting, vedere [Hosting in ASP.NET Core](xref:fundamentals/host/index).

### <a name="iis-options"></a>Opzioni IIS

::: moniker range=">= aspnetcore-2.2"

**Modello di hosting in-process**

Per configurare le opzioni di IIS Server, includere una configurazione del servizio per [IISServerOptions](/dotnet/api/microsoft.aspnetcore.builder.iisserveroptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices). L'esempio seguente disabilita AutomaticAuthentication:

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

| Opzione                         | Impostazione predefinita | Impostazione |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | Se `true`, IIS Server imposta l'utente `HttpContext.User` autenticato tramite l'[autenticazione di Windows](xref:security/authentication/windowsauth). Se `false`, il server fornisce solo un'identità per `HttpContext.User` e risponde alle richieste esplicite di `AuthenticationScheme`. Per il funzionamento di `AutomaticAuthentication` l’autenticazione di Windows deve essere abilitata in IIS. Per altre informazioni, vedere [Autenticazione di Windows](xref:security/authentication/windowsauth). |
| `AuthenticationDisplayName`    | `null`  | Imposta il nome visualizzato dagli utenti nelle pagine di accesso. |

**Modello di hosting out-of-process**

::: moniker-end

Per configurare le opzioni di IIS, includere una configurazione del servizio per [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices). Nell'esempio seguente si impedisce all'app di popolare `HttpContext.Connection.ClientCertificate`:

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| Opzione                         | Impostazione predefinita | Impostazione |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | Se `true`, il middleware di integrazione IIS imposta `HttpContext.User` autenticato tramite l'[autenticazione di Windows](xref:security/authentication/windowsauth). Se `false`, il middleware fornisce solo un'identità per `HttpContext.User` e risponde solo alle richieste esplicite di `AuthenticationScheme`. Per il funzionamento di `AutomaticAuthentication` l’autenticazione di Windows deve essere abilitata in IIS. Per altre informazioni, vedere l'argomento [Autenticazione di Windows](xref:security/authentication/windowsauth). |
| `AuthenticationDisplayName`    | `null`  | Imposta il nome visualizzato dagli utenti nelle pagine di accesso. |
| `ForwardClientCertificate`     | `true`  | Se è `true` ed è presente l’intestazione della richiesta `MS-ASPNETCORE-CLIENTCERT`, `HttpContext.Connection.ClientCertificate` viene popolato. |

### <a name="proxy-server-and-load-balancer-scenarios"></a>Scenari con server proxy e servizi di bilanciamento del carico

Il middleware di integrazione di IIS, che consente di configurare il middleware delle intestazioni inoltrate, e il modulo ASP.NET Core sono configurati per inoltrare lo schema (HTTP/HTTPS) e l'indirizzo IP remoto di origine della richiesta. Potrebbero essere necessari interventi di configurazione aggiuntivi per le app ospitate dietro ulteriori server proxy e servizi di bilanciamento del carico. Per altre informazioni, vedere [Configurare ASP.NET Core per l'utilizzo di server proxy e servizi di bilanciamento del carico](xref:host-and-deploy/proxy-load-balancer).

### <a name="webconfig-file"></a>File web.config

Il file *web.config* configura il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module). La creazione, la trasformazione e la pubblicazione del file *web.config* sono operazioni gestite da una destinazione MSBuild (`_TransformWebConfig`) quando viene pubblicato il progetto. Questa destinazione si trova tra le destinazioni Web SDK (`Microsoft.NET.Sdk.Web`). L'SDK è impostato all'inizio del file di progetto:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

Se nel progetto non è presente un file *web.config*, il file viene creato con un valore *processPath* e *argomenti* corretti per la configurazione del [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module). Il file viene quindi spostato nell'[output pubblicato](xref:host-and-deploy/directory-structure).

Se nel progetto è presente un file *web.config*, il file viene trasformato con il valore di *processPath* e gli *argomenti* corretti per la configurazione del modulo ASP.NET Core, quindi viene spostato nell'output pubblicato. La trasformazione non modifica le impostazioni di configurazione di IIS incluse nel file.

Il file *web.config* può fornire ulteriori impostazioni di configurazione di IIS che controllano i moduli IIS attivi. Per informazioni sui moduli IIS in grado di elaborare le richieste con le app di ASP.NET Core, vedere l'argomento [Moduli IIS](xref:host-and-deploy/iis/modules).

Per impedire che Web SDK trasformi il file *web.config*, usare la proprietà **\<IsTransformWebConfigDisabled >** nel file di progetto:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Quando si disabilita la trasformazione del file in Web SDK, il valore di *processPath* e gli *argomenti* devono essere impostati manualmente dallo sviluppatore. Per altre informazioni, vedere la [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

### <a name="webconfig-file-location"></a>Posizione del file web.config

Per configurare correttamente il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module), il file *web.config* deve essere presente nel percorso radice del contenuto (in genere il percorso base dell'app) dell'app distribuita. Corrisponde al percorso fisico del sito Web fornito a IIS. Il file *web.config* deve essere presente nella radice dell'app per abilitare la pubblicazione di più app mediante Distribuzione Web.

Nel percorso fisico dell'app sono presenti file riservati, come *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (commenti in formato documentazione XML) e *\<assembly>.deps.json*. Quando il file *web.config* è presente e il sito viene avviato normalmente, IIS non fornisce questi file riservati se vengono richiesti. Se il file *web.config* non è presente, ha un nome non corretto o non è in grado di configurare il sito per l'avvio normale, IIS potrebbe rendere disponibili pubblicamente i file riservati.

**Il file *web.config* deve essere sempre presente nella distribuzione, deve essere denominato correttamente ed essere in grado di configurare il sito per l'avvio normale. Non rimuovere mai il file *web.config* da una distribuzione in ambiente di produzione.**

## <a name="iis-configuration"></a>Configurazione di IIS

**Sistemi operativi Windows Server**

Abilitare il ruolo del server **Server Web (IIS)** e stabilire i servizi di ruolo.

1. Usare la procedura guidata **Aggiungi ruoli e funzionalità** accessibile tramite il menu **Gestisci** o il collegamento in **Server Manager**. Nel passaggio **Ruoli del server** selezionare la casella per **Server Web (IIS)**.

   ![Il ruolo Server Web IIS viene selezionato nel passaggio dei ruoli del server Selezionare.](index/_static/server-roles-ws2016.png)

1. Dopo il passaggio **Funzionalità**, viene caricato il passaggio**Servizi ruolo** per Server Web (IIS). Selezionare i servizi ruolo IIS desiderati o accettare i servizi ruolo predefiniti specificati.

   ![I servizi ruolo predefiniti vengono selezionati nel passaggio Selezionare i servizi ruolo.](index/_static/role-services-ws2016.png)

   **Autenticazione di Windows (facoltativa)**  
   Per abilitare l'autenticazione di Windows, espandere i nodi seguenti: **Server Web** > **Sicurezza**. Selezionare la funzionalità **Autenticazione di Windows**. Per altre informazioni, vedere [Autenticazione di Windows\<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) e [Configurare l'autenticazione Windows](xref:security/authentication/windowsauth).

   **WebSockets (facoltativo)**  
   WebSockets è supportato con ASP.NET Core 1.1 o versioni successive. Per abilitare WebSockets, espandere i nodi seguenti: **Server Web** > **Sviluppo applicazioni**. Selezionare la funzionalità **Protocollo WebSocket**. Per altre informazioni, vedere [Oggetti WebSocket](xref:fundamentals/websockets).

1. Procedere con il passaggio **Conferma** per installare il ruolo del server web e i servizi. Dopo l'installazione del ruolo **Server Web (IIS)** non è necessario riavviare il server o IIS.

**Sistemi operativi desktop di Windows**

Abilitare **Console di gestione IIS** e **Servizi Web**.

1. Passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo).

1. Aprire il nodo **Internet Information Services**. Aprire il nodo **Strumenti di gestione Web**.

1. Selezionare la casella per **Console di gestione IIS**.

1. Selezionare la casella per **Servizi World Wide Web**.

1. Accettare le funzionalità predefinite per **Servizi World Wide Web** o personalizzare le funzionalità IIS.

   **Autenticazione di Windows (facoltativa)**  
   Per abilitare l'autenticazione di Windows, espandere i nodi seguenti: **Servizi Web** > **Sicurezza**. Selezionare la funzionalità **Autenticazione di Windows**. Per altre informazioni, vedere [Autenticazione di Windows\<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) e [Configurare l'autenticazione Windows](xref:security/authentication/windowsauth).

   **WebSockets (facoltativo)**  
   WebSockets è supportato con ASP.NET Core 1.1 o versioni successive. Per abilitare WebSockets, espandere i nodi seguenti: **Servizi Web** > **Funzionalità per lo sviluppo di applicazioni**. Selezionare la funzionalità **Protocollo WebSocket**. Per altre informazioni, vedere [Oggetti WebSocket](xref:fundamentals/websockets).

1. Se l'installazione di IIS richiede un riavvio, riavviare il sistema.

![La console di gestione IIS e servizi World Wide Web sono selezionati nelle funzionalità di Windows.](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a>Installare il bundle di hosting .NET Core

Installare il *bundle di hosting .NET Core* nel sistema di hosting. L'aggregazione installa il runtime di .NET Core, la libreria di .NET Core e il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module). Il modulo consente l'esecuzione delle app ASP.NET Core dietro IIS. Se il sistema non ha una connessione Internet, ottenere e installare [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) prima di installare il bundle di hosting .NET Core.

> [!IMPORTANT]
> Se il bundle di hosting viene installato prima di IIS, è necessario riparare l'installazione del bundle. Eseguire di nuovo il programma di installazione del bundle di hosting dopo l'installazione di IIS.

### <a name="direct-download-current-version"></a>Download diretto (versione corrente)

Scaricare il programma di installazione mediante il collegamento seguente:

[Programma di installazione del bundle di hosting .NET Core corrente (download diretto)](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a>Versioni precedenti del programma di installazione

Per ottenere una versione precedente del programma di installazione:

1. Passare agli [archivi dei download per .NET](https://www.microsoft.com/net/download/archives).
1. In **.NET Core**, selezionare la versione di .NET Core.
1. Nella colonna **Run apps - Runtime** (App di esecuzione - Runtime), trovare la riga della versione di runtime di .NET Core desiderata.
1. Scaricare il programma di installazione tramite il collegamento **Runtime & Hosting Bundle** (Runtime e bundle di hosting).

> [!WARNING]
> Alcuni programmi di installazione contengono versioni che hanno terminato il loro ciclo di vita e non sono più supportate da Microsoft. Per altre informazioni, vedere i [criteri di supporto](https://www.microsoft.com/net/download/dotnet-core/2.0).

### <a name="install-the-hosting-bundle"></a>Installare il bundle di hosting

1. Eseguire il programma di installazione nel server. Quando si esegue il programma di installazione da un prompt dei comandi di amministratore sono disponibili le opzioni seguenti:

   * `OPT_NO_ANCM=1` &ndash; Ignora l'installazione del modulo ASP.NET Core.
   * `OPT_NO_RUNTIME=1` &ndash; Ignora l'installazione del runtime .NET Core.
   * `OPT_NO_SHAREDFX=1` &ndash; Ignora l'installazione del framework condiviso di ASP.NET (runtime ASP.NET).
   * `OPT_NO_X86=1` &ndash; Ignora l'installazione dei runtime x86. Usare questa opzione se si è certi che non verrà eseguito l'hosting di app a 32 bit. Se non esiste alcuna possibilità che in futuro venga eseguito l'hosting di app sia a 32 che a 64 bit, non usare questa opzione e installare entrambi i runtime.
1. Riavviare il sistema o eseguire **net stop was /y** seguito da **net start w3svc** da un prompt dei comandi. Il riavvio di IIS rileva una modifica alla variabile di ambiente di sistema PATH apportata dal programma di installazione.

Se il programma di installazione del bundle di hosting Windows rileva che è necessario reimpostare IIS per poter completare l'installazione, procede con la reimpostazione. Se il programma di installazione attiva una reimpostazione di IIS, tutti i siti Web e i pool di app IIS vengono riavviati.

> [!NOTE]
> Per informazioni sulla configurazione condivisa di IIS, vedere [Modulo di ASP.NET Core con configurazione condivisa di IIS](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Installare la Distribuzione Web quando si pubblica con Visual Studio

Quando si distribuiscono app ai server con [Distribuzione Web](/iis/publish/using-web-deploy/introduction-to-web-deploy), installare la versione più recente di Distribuzione Web nel server. Per installare Distribuzione Web usare l'[Installazione guidata piattaforma Web (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) o ottenere direttamente un programma di installazione in [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717). Il metodo preferito consiste nell'usare WebPI. WebPI offre un'installazione autonoma e una configurazione per i provider di hosting.

## <a name="create-the-iis-site"></a>Creare il sito IIS

1. Nel sistema host creare una cartella per contenere le cartelle e i file pubblicati dell'app. Un layout di distribuzione di un'app è descritto nell'argomento [Struttura della directory](xref:host-and-deploy/directory-structure).

1. All'interno della nuova cartella creare una cartella *logs* per contenere i log stdout del modulo ASP.NET Core quando è abilitata la registrazione stdout. Se l'app viene distribuita con una cartella *logs* nel payload, ignorare questo passaggio. Per istruzioni su come impostare MSBuild per la creazione automatica della cartella *logs* quando il progetto viene compilato in locale, vedere l'argomento [Struttura della directory](xref:host-and-deploy/directory-structure).

   > [!IMPORTANT]
   > usare il log stdout solo per la risoluzione dei problemi di avvio delle app. Non usarlo mai per la registrazione delle app di routine. Non esiste alcun limite per le dimensioni dei file di log o il numero di file di log che è possibile creare. Il pool di applicazioni deve avere accesso in scrittura alla posizione in cui vengono scritti i log. Tutte le cartelle nel percorso della posizione del log devono essere già esistenti. Per altre informazioni sul log stdout, vedere [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) (Creazione e reindirizzamento dei log). Per informazioni sulla registrazione in un'app ASP.NET Core, vedere l'argomento [Registrazione](xref:fundamentals/logging/index).

1. In **Gestione IIS** aprire il nodo del server nel pannello **Connessioni**. Fare clic con il pulsante destro del mouse sulla cartella **Siti**. Scegliere **Aggiungi sito Web** dal menu di scelta rapida.

1. Specificare un **Nome del sito** e impostare il **Percorso fisico** per la cartella di distribuzione dell'app. Specificare la configurazione in **Binding** e creare il sito Web scegliendo **OK**:

   ![Specificare il nome del sito, il percorso fisico e il nome Host nel passaggio Aggiungi sito Web.](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > Le associazioni con caratteri jolly di livello superiore (`http://*:80/` e `http://+:80`) **non** devono essere usate, poiché possono introdurre vulnerabilità a livello di sicurezza nell'app. Questo concetto vale sia per i caratteri jolly sicuri che vulnerabili. Usare nomi host espliciti al posto di caratteri jolly. L'associazione con caratteri jolly del sottodominio (ad esempio, `*.mysub.com`) non costituisce un rischio per la sicurezza se viene controllato l'intero dominio padre (a differenza di `*.com`, che è vulnerabile). Vedere la [sezione 5.4 di RFC7230](https://tools.ietf.org/html/rfc7230#section-5.4) per altre informazioni.

1. Nel nodo del server selezionare **Pool di applicazioni**.

1. Fare clic con il pulsante destro del mouse sul pool di applicazioni del sito e scegliere **Impostazioni di base** dal menu di scelta rapida.

1. Nella finestra **Modifica pool di applicazioni** impostare **Versione .NET CLR** su **Nessun codice gestito**.

   ![Impostare Nessun codice gestito per la versione .NET CLR.](index/_static/edit-apppool-ws2016.png)

    ASP.NET Core viene eseguito in un processo separato e gestisce il runtime. ASP.NET Core non si basa sul caricamento del CLR del desktop. L'impostazione della **versione .NET CLR** su **Nessun codice gestito** è facoltativa.

1. *ASP.NET Core 2.2 o versione successiva*: per una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd) a 64 bit (x64) che usa il [modello di hosting in-process](xref:fundamentals/servers/index#in-process-hosting-model), disabilitare il pool di app per i processi a 32 bit (x86).

   Nella barra laterale **Azioni** del **pool di applicazioni** di Gestione IIS selezionare **Impostazioni predefinite pool di applicazioni** o **Impostazioni avanzate**. Individuare **Attiva applicazioni a 32 bit** e impostare il valore su `False`. Questa impostazione non viene applicata alle app distribuite per l'[hosting out-of-process](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).

1. Confermare che l'identità del modello del processo disponga delle autorizzazioni appropriate.

   Se l'identità predefinita del pool di app (**Modello di processo** > **Identità**) viene modificata da **ApplicationPoolIdentity** a un'altra identità, verificare che la nuova identità disponga delle autorizzazioni necessarie per l'accesso alla cartella dell'app, al database e alle altre risorse richieste. Ad esempio, il pool di applicazioni richiede l'accesso in lettura e scrittura alle cartelle in cui l'app legge e scrive i file.

**Configurazione dell'autenticazione di Windows (facoltativa)**  
Per altre informazioni, vedere [Configurare l'autenticazione Windows](xref:security/authentication/windowsauth).

## <a name="deploy-the-app"></a>Distribuire l'app

Distribuire l'app nella cartella creata nel sistema host. Il metodo consigliato per la distribuzione è [Distribuzione Web](/iis/publish/using-web-deploy/introduction-to-web-deploy).

### <a name="web-deploy-with-visual-studio"></a>Distribuzione web con Visual Studio

Vedere l'argomento [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) (Profili di pubblicazione Visual Studio per la distribuzione di app ASP.NET Core) per informazioni su come creare un profilo di pubblicazione da usare con Distribuzione Web. Se il provider di hosting fornisce un profilo di pubblicazione o il supporto per crearne uno, scaricare il profilo e importarlo usando la finestra di dialogo **Pubblica** di Visual Studio.

![Pagina della finestra di dialogo Pubblica](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Distribuzione Web all'esterno di Visual Studio

È anche possibile usare [Distribuzione Web](/iis/publish/using-web-deploy/introduction-to-web-deploy) all'esterno di Visual Studio dalla riga di comando. Per altre informazioni sulla distribuzione, vedere [Strumento Distribuzione Web](/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Alternative a Distribuzione web

Usare uno dei metodi disponibili, ad esempio la copia manuale, Xcopy, Robocopy o PowerShell, per spostare l'app nel sistema host.

Per altre informazioni sulla distribuzione di ASP.NET Core in IIS, vedere la sezione [Risorse di distribuzione per amministratori IIS](#deployment-resources-for-iis-administrators).

## <a name="browse-the-website"></a>Esplorare il sito Web

![Il browser Microsoft Edge ha caricato la pagina di avvio IIS.](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a>File di distribuzione bloccati

I file nella cartella di distribuzione sono bloccati durante l'esecuzione dell'app. I file bloccati non possono essere sovrascritti durante la distribuzione. Per rilasciare i file bloccati in una distribuzione, arrestare il pool di applicazioni usando **uno** dei metodi seguenti:

* Usare Distribuzione Web e fare riferimento a `Microsoft.NET.Sdk.Web` nel file di progetto. Nella radice della directory dell'app web viene posizionato un file *app_offline.htm*. Quando il file è presente, il modulo ASP.NET Core arresta normalmente l'app e rende disponibile il file *app_offline.htm* durante la distribuzione. Per altre informazioni, vedere la [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).
* Arrestare manualmente il pool di applicazioni in Gestione IIS sul server.
* Usare PowerShell per eliminare *app_offline.html* (è richiesto PowerShell 5 o versione successiva):

  ```PowerShell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a>Protezione dei dati

Lo [stack di protezione dei dati di ASP.NET Core](xref:security/data-protection/introduction) viene usato da diversi [middleware](xref:fundamentals/middleware/index) di ASP.NET Core, inclusi quelli usati nell'autenticazione. Anche se le DPAPI (Data Protection API) non vengono chiamate dal codice dell'utente, è consigliabile configurare la protezione dati per la creazione di un [archivio di chiavi](xref:security/data-protection/implementation/key-management) crittografiche permanente con uno script di distribuzione o nel codice dell'utente. Se non si configura la protezione dei dati, le chiavi vengono mantenute in memoria ed eliminate al riavvio dell'app.

Se il gruppo di chiavi viene archiviato in memoria quando l'app viene riavviata:

* Tutti i token di autenticazione basati su cookie vengono invalidati. 
* Gli utenti devono ripetere l'accesso alla richiesta successiva. 
* Tutti i dati protetti con il gruppo di chiavi non possono più essere decrittografati. Possono essere inclusi i [token CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) e i [cookie TempData di ASP.NET Core MVC](xref:fundamentals/app-state#tempdata).

Per configurare la protezione dati in IIS in modo da rendere permanente il gruppo di chiavi, usare **uno** degli approcci seguenti:

* **Creare chiavi del Registro di sistema di protezione dati**

  Le chiavi di protezione dati usate dalle app ASP.NET Core vengono archiviate nel Registro di sistema esterno alle app. Per mantenere le chiavi per una determinata app, creare chiavi del Registro di sistema per il pool di applicazioni.

  Per le installazioni IIS autonome non web farm è possibile usare lo [script PowerShell Data Protection Provision-AutoGenKeys.ps1 (Provisioning di protezione dati-AutoGenKeys.ps1)](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) per ogni pool di app usato con un'app ASP.NET Core. Questo script crea una chiave del Registro di sistema nel registro HKLM accessibile solo all'account del processo di lavoro del pool di applicazioni dell'app. Le chiavi vengono crittografate a riposo tramite DPAPI con una chiave a livello del computer.

  In scenari di web farm un'app può essere configurata in modo da usare un percorso UNC in cui archiviare il proprio gruppo di chiavi di protezione dati. Per impostazione predefinita, le chiavi di protezione dati non vengono crittografate. Assicurarsi che le autorizzazioni file per la condivisione di rete siano limitate all'account di Windows in cui viene eseguita l'app. È possibile usare un certificato X509 per la protezione delle chiavi inattive. Prendere in considerazione un meccanismo per consentire agli utenti di caricare i certificati: Posizionare i certificati nell'archivio certificati attendibili dell'utente e assicurarsi che siano disponibili in tutti i computer in cui viene eseguita l'app dell'utente. Vedere [Configurare la protezione dati di ASP.NET Core](xref:security/data-protection/configuration/overview) per altri dettagli.

* **Configurare il pool di applicazioni IIS per caricare il profilo utente**

  Questa impostazione si trova nella sezione **Modello del processo** in **Impostazioni avanzate** per il pool di app. Impostare Carica profilo utente su `True`. Questa operazione archivia le chiavi nella directory del profilo utente e le mantiene protette tramite DPAPI con una chiave specifica dell'account utente usato dal pool di applicazioni.

* **Usare il file system come archivio del gruppo di chiavi**

  Modificare il codice dell'app per [usare il file system come archivio del gruppo di chiavi](xref:security/data-protection/configuration/overview). Usare un certificato X509 per proteggere il gruppo di chiavi e verificare che sia un certificato attendibile. Se il certificato è autofirmato, inserirlo nell'archivio radice attendibile.

  Quando si usa IIS in una web farm:

  * Usare una condivisione di file a cui possono accedere tutti i computer.
  * Distribuire un certificato X509 in ogni computer. Configurare [protezione dei dati nel codice](xref:security/data-protection/configuration/overview).

* **Impostare criteri a livello di computer per la protezione dei dati**

  Il sistema di protezione dei dati ha un supporto limitato per l'impostazione di [criteri a livello di computer](xref:security/data-protection/configuration/machine-wide-policy) predefiniti per tutte le app che usano le API di protezione dei dati. Per ulteriori informazioni, vedere <xref:security/data-protection/introduction>.

## <a name="virtual-directories"></a>Directory virtuali

Le [directory virtuali IIS](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) non sono supportate con le app ASP.NET Core. Un'app può essere ospitata come [applicazione secondaria](#sub-applications).

## <a name="sub-applications"></a>Applicazioni secondarie

Un'app ASP.NET Core può essere ospitata come [applicazione secondaria di IIS (app secondaria)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications). Il percorso dell'app secondaria diventa parte dell'URL dell'app radice.

::: moniker range="< aspnetcore-2.2"

Un'app secondaria non deve includere il modulo ASP.NET Core come gestore. Se il modulo viene aggiunto come gestore nel file *web.config* di un'app secondaria, quando si prova a esplorare l'app secondaria si riceve un messaggio 500.19 *(errore interno del server)* che segnala errori nel file di configurazione.

L'esempio seguente illustra un file *web.config* pubblicato per un'app secondaria di ASP.NET Core:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Quando si ospita un'app secondaria non ASP.NET Core in un'app ASP.NET Core, è necessario rimuovere in modo esplicito il gestore ereditato nel file *web.config* dell'app secondaria:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

I collegamenti ad asset statici all'interno dell'app secondaria devono usare la notazione con tilde-barra (`~/`). La notazione con tilde-barra attiva un [helper tag](xref:mvc/views/tag-helpers/intro) per anteporre la base del percorso dell'app secondaria al collegamento relativo sottoposto a rendering. Per un'app secondaria in `/subapp_path`, il rendering di un'immagine collegata con `src="~/image.png"` viene eseguito come `src="/subapp_path/image.png"`. Il middleware dei file statici dell'app radice non elabora la richiesta di file statici. La richiesta viene elaborata dal middleware dei file statici dell'app secondaria.

Se l'attributo `src` di un asset statico viene impostato su un percorso assoluto (ad esempio, `src="/image.png"`), il rendering del collegamento viene eseguito senza la base del percorso dell'app secondaria. Il middleware dei file statici dell'app radice prova a servire l'asset da [webroot](xref:fundamentals/index#web-root-webroot) dell'app radice con conseguente risposta *404 - Non trovato*, a meno che l'asset statico non sia disponibile dal'app radice.

Per ospitare un'app ASP.NET Core come app secondaria in un'altra app ASP.NET Core:

1. Definire un pool di app per l'app secondaria. Impostare la **versione di CLR.NET** su **No Managed Code** (Nessun codice gestito).

1. Aggiungere il sito radice in Gestione IIS con l'applicazione secondaria in una cartella all'interno del sito principale.

1. Fare clic con il pulsante destro del mouse sulla cartella dell'applicazione secondaria in Gestione IIS e scegliere **Converti in applicazione**.

1. Nella finestra di dialogo **Aggiungi applicazione** usare il pulsante **Seleziona** per **Pool di applicazioni** per assegnare il pool di app creato per l'app secondaria. Scegliere **OK**.

L'assegnazione di un pool di app separato all'app secondaria è un requisito quando si usa il modello di hosting in-process.

Per altre informazioni sul modello di hosting in-process e sulla configurazione del modulo ASP.NET Core, vedere <xref:host-and-deploy/aspnet-core-module> e <xref:host-and-deploy/aspnet-core-module>.

## <a name="configuration-of-iis-with-webconfig"></a>Configurazione di IIS con web.config

La configurazione di IIS è influenzata dalla sezione `<system.webServer>` di *web.config* per gli scenari IIS funzionali per le app ASP.NET Core con il modulo ASP.NET Core. Ad esempio, la configurazione di IIS è funzionale per la compressione dinamica. Se IIS è configurato a livello di server per l'uso della compressione dinamica, l'elemento `<urlCompression>` nel file *web.config* dell'app può disabilitare questa impostazione per un'app ASP.NET Core.

Per altre informazioni, vedere la [guida di riferimento per la configurazione di \<system.webServer>](/iis/configuration/system.webServer/), la [guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) e [Moduli IIS con ASP.NET Core](xref:host-and-deploy/iis/modules). Per impostare variabili di ambiente per singole app in esecuzione in pool di applicazioni isolate (supportati per IIS 10.0 o versioni successive), vedere la sezione *Comando AppCmd.exe* dell'argomento [Variabili di ambiente \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) nella documentazione di riferimento di IIS.

## <a name="configuration-sections-of-webconfig"></a>Sezioni di configurazione di web.config

Le sezioni di configurazione di app ASP.NET 4.x in *web.config* non vengono usate dalle app ASP.NET Core per la configurazione:

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

Le applicazioni ASP.NET Core vengono configurate tramite altri provider di configurazione. Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration/index).

## <a name="application-pools"></a>Pool di applicazioni

::: moniker range=">= aspnetcore-2.2"

L'isolamento dei pool di app è determinato dal modello di hosting:

* Hosting in-process &ndash; Le app devono essere eseguite in pool di app separati.
* Hosting out-of-process &ndash; È consigliabile isolare le app le une dalle altre eseguendo ogni app nel relativo pool di applicazioni.

Nella finestra di dialogo **Aggiungi sito Web** è selezionato per impostazione predefinita un singolo pool di app per ogni app. Quando si specifica un valore in **Nome del sito**, il testo viene automaticamente trasferito alla casella di testo **Pool di applicazioni**. Quando si aggiunge il sito viene creato un nuovo pool di applicazioni con il nome del sito.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Quando si ospitano più siti Web in un server, è consigliabile isolare le app le une dalle altre eseguendo ogni app nel relativo pool di applicazioni. La finestra di dialogo **Aggiungi sito Web** di IIS ha questa configurazione come impostazione predefinita. Quando si specifica un valore in **Nome del sito**, il testo viene automaticamente trasferito alla casella di testo **Pool di applicazioni**. Quando si aggiunge il sito viene creato un nuovo pool di applicazioni con il nome del sito.

::: moniker-end

## <a name="application-pool-identity"></a>Identità del pool di applicazione

Un account di identità del pool di app consente di eseguire un'app in un account univoco, senza dover creare e gestire domini o account locali. In IIS 8.0 o versioni successive il processo di lavoro amministrazione IIS (WAS) crea un account virtuale con il nome del nuovo pool di applicazioni ed esegue i processi di lavoro del pool di applicazioni inclusi nell'account per impostazione predefinita. Nella Console di gestione IIS in **Impostazioni avanzate** per il pool di app verificare che **Identità** sia impostata su **ApplicationPoolIdentity**:

![Finestra di dialogo Impostazioni avanzate del pool di applicazione](index/_static/apppool-identity.png)

Il processo di gestione IIS crea un identificatore sicuro con il nome del pool di app nel sistema di sicurezza di Windows. Le risorse possono essere protette mediante questa identità, che tuttavia non è un account utente reale e non viene visualizzata nella console di gestione utenti di Windows.

Se il processo di lavoro IIS richiede l'accesso con privilegi elevati all'app, modificare l'elenco di controllo di accesso (ACL) della directory contenente l'app:

1. Aprire Esplora risorse, quindi spostarsi nella directory.

1. Fare clic con il pulsante destro del mouse sulla directory e scegliere **Proprietà**.

1. Nella scheda **Sicurezza** selezionare il pulsante **Modifica** e quindi il pulsante **Aggiungi**.

1. Fare clic sul pulsante **Percorsi** e verificare che il sistema sia selezionato.

1. Immettere **IIS AppPool\\<nome_pool_applicazioni>** nell'area **Immettere i nomi degli oggetti da selezionare**. Fare clic sul pulsante **Controlla nomi**. Per *DefaultAppPool* controllare i nomi usando **IIS AppPool\DefaultAppPool**. Quando il pulsante **Controlla nomi** è selezionato, nell'area dei nomi degli oggetti viene indicato un valore di **DefaultAppPool**. Non è possibile immettere il nome del pool di applicazioni direttamente nell'area dei nomi degli oggetti. Usare il formato **IIS AppPool\\<nome_pool_applicazioni>** durante il controllo dei nomi degli oggetti.

   ![Finestra di dialogo Seleziona utenti o gruppi per la cartella dell'app: il nome del pool di applicazioni "DefaultAppPool" viene aggiunto in coda a "IIS AppPool\" nell'area dei nomi degli oggetti prima di selezionare "Controlla nomi".](index/_static/select-users-or-groups-1.png)

1. Scegliere **OK**.

   ![Finestra di dialogo Seleziona utenti o gruppi per la cartella dell'app: dopo aver selezionato "Controlla nomi", il nome dell'oggetto "DefaultAppPool" viene visualizzato nell'area dei nomi degli oggetti.](index/_static/select-users-or-groups-2.png)

1. Le autorizzazioni di lettura ed esecuzione devono essere concesse per impostazione predefinita. Fornire autorizzazioni aggiuntive in base alle necessità.

L'accesso può essere concesso anche dal prompt dei comandi usando lo strumento **ICACLS**. Usando *DefaultAppPool* come esempio, viene eseguito il comando seguente:

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

Per altre informazioni, vedere l'argomento [icacls](/windows-server/administration/windows-commands/icacls).

## <a name="http2-support"></a>Supporto per HTTP/2

::: moniker range=">= aspnetcore-2.2"

[HTTP/2](https://httpwg.org/specs/rfc7540.html) è supportato con ASP.NET Core negli scenari di distribuzione IIS seguenti:

* In-Process
  * Windows Server 2016/Windows 10 o versioni successive; IIS 10 o versioni successive
  * Connessione TLS 1.2 o successiva
* Out-of-process
  * Windows Server 2016/Windows 10 o versioni successive; IIS 10 o versioni successive
  * Le connessioni server perimetrali pubbliche usano HTTP/2, mentre la connessione con proxy inverso al [server Kestrel](xref:fundamentals/servers/kestrel) usa HTTP/1.1.
  * Connessione TLS 1.2 o successiva

Per una distribuzione In-Process, se viene stabilita una connessione HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) corrisponde a `HTTP/2`. Per una distribuzione out-of-process, se viene stabilita una connessione HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) corrisponde a `HTTP/1.1`.

Per altre informazioni sui modelli di hosting in-process e out-of-process, vedere l'argomento <xref:host-and-deploy/aspnet-core-module> e <xref:host-and-deploy/aspnet-core-module>.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[HTTP/2](https://httpwg.org/specs/rfc7540.html) è supportato per le distribuzioni out-of-process che soddisfano i requisiti seguenti:

* Windows Server 2016/Windows 10 o versioni successive; IIS 10 o versioni successive
* Le connessioni server perimetrali pubbliche usano HTTP/2, mentre la connessione con proxy inverso al [server Kestrel](xref:fundamentals/servers/kestrel) usa HTTP/1.1.
* Framework di destinazione: non applicabile alle distribuzioni out-of-process, in quanto la connessione HTTP/2 è gestita interamente da IIS.
* Connessione TLS 1.2 o successiva

Se viene stabilita una connessione HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) corrisponde a `HTTP/1.1`.

::: moniker-end

HTTP/2 è abilitato per impostazione predefinita. Se non viene stabilita una connessione HTTP/2, la connessione esegue il fallback a HTTP/1.1. Per altre informazioni sulla configurazione di HTTP/2 con le distribuzioni IIS, vedere [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis) (HTTP/2 in IIS).

## <a name="deployment-resources-for-iis-administrators"></a>Risorse di distribuzione per amministratori IIS

Informazioni approfondite su IIS nella documentazione di IIS.  
[Documentazione di ISS](/iis)

Informazioni sui modelli di distribuzione di app .NET Core.  
[Distribuzione di applicazioni .NET Core](/dotnet/core/deploying/)

Informazioni su come il modulo ASP.NET Core consente al server Web Kestrel di usare IIS o IIS Express come server proxy inverso.  
[Modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)

Informazioni su come configurare il modulo di ASP.NET Core per l'hosting di app ASP.NET Core.  
[Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)

Informazioni sulla struttura di directory delle app ASP.NET Core pubblicate.  
[Struttura di directory](xref:host-and-deploy/directory-structure)

Individuare i moduli IIS attivi e inattivi per le app ASP.NET Core e come gestire i moduli IIS.  
[Moduli IIS](xref:host-and-deploy/iis/troubleshoot)

Informazioni su come diagnosticare i problemi delle distribuzioni IIS di app ASP.NET Core.  
[Risolvere i problemi](xref:host-and-deploy/iis/troubleshoot)

Riconoscere errori comuni dell'hosting di app ASP.NET Core in IIS.  
[Errori comuni di Azure App Service e IIS](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:test/troubleshoot>
* [Introduzione a ASP.NET Core](xref:index)
* [Il sito IIS ufficiale di Microsoft](https://www.iis.net/)
* [Libreria di contenuti tecnici di Windows Server](/windows-server/windows-server)
* [HTTP/2 in IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
