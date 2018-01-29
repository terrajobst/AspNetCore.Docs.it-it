---
title: Host ASP.NET Core in Windows con IIS
author: guardrex
description: Informazioni su come ospitare app ASP.NET Core in Windows Server Internet Information Services (IIS).
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/index
ms.openlocfilehash: 18c7448ad79891d04eca1e939a0aeeabe417bde8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>Host ASP.NET Core in Windows con IIS

Di [Luke Latham](https://github.com/guardrex) e [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>Sistemi operativi supportati

Sono supportati i sistemi operativi seguenti:

* Windows 7 e versioni più recenti
* Windows Server 2008 R2 e più recenti &#8224;

&#8224;A livello concettuale, la configurazione di IIS descritta in questo documento è valida anche per l'hosting delle app ASP.NET Core in Nano Server IIS. Per istruzioni specifiche vedere [ASP.NET Core con IIS in Nano Server](xref:tutorials/nano-server).

Il [server HTTP.sys](xref:fundamentals/servers/httpsys) (chiamato in precedenza [WebListener](xref:fundamentals/servers/weblistener)) non funziona in una configurazione proxy inverso con IIS. È necessario usare il [server Kestrel](xref:fundamentals/servers/kestrel).

## <a name="iis-configuration"></a>Configurazione di IIS

Abilitare il ruolo **Server Web (IIS)** e stabilire i servizi di ruolo.

### <a name="windows-desktop-operating-systems"></a>Sistemi operativi desktop di Windows

Passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo). Aprire il gruppo per **Internet Information Services** e **Strumenti di gestione Web**. Selezionare la casella per **Console di gestione IIS**. Selezionare la casella per **Servizi World Wide Web**. Accettare le funzionalità predefinite per **Servizi World Wide Web** o personalizzare le funzionalità IIS.

![La console di gestione IIS e servizi World Wide Web sono selezionati nelle funzionalità di Windows.](index/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a>Sistemi operativi Windows Server

Per i sistemi operativi del server, usare la procedura guidata **Aggiungi ruoli e funzionalità** tramite il menù **Gestisci** o il collegamento in **Server Manager**. Nel passaggio **Ruoli del server** selezionare la casella per **Server Web (IIS)**.

![Il ruolo Server Web IIS viene selezionato nel passaggio dei ruoli del server Selezionare.](index/_static/server-roles-ws2016.png)

Nel passaggio **Servizi ruolo** selezionare i servizi ruolo IIS desiderati o accettare i servizi ruolo predefiniti specificati.

![I servizi ruolo predefiniti vengono selezionati nel passaggio Selezionare i servizi ruolo.](index/_static/role-services-ws2016.png)

Procedere con il passaggio **Conferma** per installare il ruolo del server web e i servizi. Dopo l'installazione del ruolo Server Web (IIS) non è necessario riavviare il server o IIS.

## <a name="install-the-net-core-windows-server-hosting-bundle"></a>Installare l'aggregazione di Hosting di.NET Core Windows Server

1. Installare l'[aggregazione di Hosting di.NET Core Windows Server](https://aka.ms/dotnetcore-2-windowshosting) nel sistema di hosting. L'aggregazione installa il runtime di .NET Core, la libreria di .NET Core e il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). Il modulo crea il proxy inverso tra IIS e il server Kestrel. Se nel sistema non è presente una connessione a Internet, ottenere e installare [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) prima di installare l'aggregazione di Hosting di .NET Core Windows Server.

2. Riavviare il sistema o eseguire **net stop was /y** seguito da **net start w3svc** da un prompt dei comandi per visualizzare una modifica al percorso di sistema.

> [!NOTE]
> Per informazioni sulla configurazione condivisa di IIS, vedere [Modulo di ASP.NET Core con configurazione condivisa di IIS](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Installare la Distribuzione Web quando si pubblica con Visual Studio

Quando si distribuiscono app ai server con [Distribuzione Web](/iis/publish/using-web-deploy/introduction-to-web-deploy), installare la versione più recente di Distribuzione Web nel server. Per installare Distribuzione Web usare l'[Installazione guidata piattaforma Web (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) o ottenere direttamente un programma di installazione in [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717). Il metodo preferito consiste nell'usare WebPI. WebPI offre un'installazione autonoma e una configurazione per i provider di hosting.

## <a name="application-configuration"></a>Configurazione dell'applicazione

### <a name="enabling-the-iisintegration-components"></a>Abilitare i componenti IISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Un file *Program.cs* tipico chiama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) per avviare la configurazione di un host. `CreateDefaultBuilder` configura [Kestrel](xref:fundamentals/servers/kestrel) come server Web e abilita l'integrazione IIS configurando il percorso base e la porta per il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module):

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Includere una dipendenza dal pacchetto [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) nelle dipendenze dell'applicazione. Incorporare il middleware di integrazione IIS nell'app aggiungendo il metodo di estensione *UseIISIntegration* a *WebHostBuilder*:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

Sono necessari sia `UseKestrel` sia `UseIISIntegration`. Il fatto che il codice chiami `UseIISIntegration` non influisce sulla portabilità del codice stesso. Se l'app non viene eseguita dietro IIS (ad esempio, se l'app viene eseguita direttamente in Kestrel), `UseIISIntegration` non esegue nessuna operazione.

---

Per altre informazioni sull'hosting, vedere [Hosting in ASP.NET Core](xref:fundamentals/hosting).

### <a name="iis-options"></a>Opzioni IIS

Per configurare le opzioni di IIS includere una configurazione del servizio per `IISOptions` in `ConfigureServices`:

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| Opzione                         | Impostazione predefinita | Impostazione |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | Se è `true`, il middleware di autenticazione imposta `HttpContext.User` e risponde alle richieste generiche. Se è `false`, il middleware di autenticazione fornisce solo un'identità (`HttpContext.User`) e risponde alle richieste solo in base a un’istruzione esplicita di `AuthenticationScheme`. Per il funzionamento di `AutomaticAuthentication` l’autenticazione di Windows deve essere abilitata in IIS. |
| `AuthenticationDisplayName`    | `null`  | Imposta il nome visualizzato dagli utenti nelle pagine di accesso. |
| `ForwardClientCertificate`     | `true`  | Se è `true` ed è presente l’intestazione della richiesta `MS-ASPNETCORE-CLIENTCERT`, `HttpContext.Connection.ClientCertificate` viene popolato. |

### <a name="webconfig"></a>web.config

Lo scopo principale del file *web.config* è la configurazione del [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). e può fornire facoltativamente altre impostazioni di configurazione di IIS. La creazione, la trasformazione e la pubblicazione di *web.config* vengono gestite da .NET Core Web SDK (`Microsoft.NET.Sdk.Web`), impostato all'inizio del file di progetto `<Project Sdk="Microsoft.NET.Sdk.Web">`. Per impedire che l'SDK trasformi il file *web.config*, aggiungere la proprietà **\<IsTransformWebConfigDisabled >** al file di progetto con un'impostazione di `true`:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Se nel progetto è presente un file *web.config*, il file viene trasformato con il valore di *processPath* e gli *argomenti* corretti per la configurazione del [modulo di ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), quindi viene spostato nell'[output pubblicato](xref:host-and-deploy/directory-structure). La trasformazione non modifica le impostazioni di configurazione di IIS incluse nel file.

### <a name="webconfig-location"></a>Percorso di web.config

Le app .NET core sono ospitate tramite un proxy inverso tra IIS e il server Kestrel. Per creare il proxy inverso, il file *web.config* deve essere presente nel percorso radice del contenuto (in genere il percorso base dell'app) dell'app distribuita, che è il percorso fisico del sito Web reso disponibile a IIS. Il file *web.config* deve essere presente nella radice dell'app per abilitare la pubblicazione di più app mediante Distribuzione Web.

Nel percorso fisico dell'app sono presenti file riservati, incluse le sottocartelle quali *\<assembly_name>.runtimeconfig.json*, *\<assembly_name>.xml* (commenti in formato documentazione XML) e *\<assembly_name>.deps.json*. Quando il file *web.config* è presente e configura il sito, IIS limita la disponibilità di questi file riservati. **Di conseguenza è importante che il file *web.config* non sia inavvertitamente rinominato o rimosso dalla distribuzione.**

## <a name="create-the-iis-website"></a>Creare il sito Web IIS

1. Nel sistema IIS di destinazione creare una cartella per contenere le cartelle e i file pubblicati dell'app, descritti in [Struttura della directory](xref:host-and-deploy/directory-structure).

2. All'interno della cartella creare una cartella *logs* per contenere i log stdout quando è abilitata la registrazione stdout. Se l'app viene distribuita con una cartella *logs* nel payload, ignorare questo passaggio. Per istruzioni su come impostare MSBuild per la creazione della cartella *logs*, vedere l'argomento [Struttura di directory](xref:host-and-deploy/directory-structure).

3. In **Gestione IIS**, creare un nuovo sito Web. Specificare un **Nome del sito** e impostare il **Percorso fisico** per la cartella di distribuzione dell'app. Fornire la configurazione dell'**Associazione** e creare il sito Web.

4. Impostare il pool di applicazioni su **Nessun codice gestito**. ASP.NET Core viene eseguito in un processo separato e gestisce il runtime.

5. Aprire la finestra **Aggiungi sito Web**.

   ![Selezionare "Aggiungi sito Web" dal menu di scelta rapida dei siti.](index/_static/add-website-context-menu-ws2016.png)

6. Configurare il sito Web.

   ![Specificare il nome del sito, il percorso fisico e il nome Host nel passaggio Aggiungi sito Web.](index/_static/add-website-ws2016.png)

7. Nel riquadro **Pool di applicazioni** aprire la finestra **Modifica Pool di applicazioni** facendo clic con il pulsante destro del mouse sul pool di app del sito Web e selezionando **Impostazioni Basic** dal menu a comparsa.

   ![Selezionare le Impostazioni Basic dal menu di scelta rapida del Pool di applicazioni.](index/_static/apppools-basic-settings-ws2016.png)

8. Impostare la **versione CLR.NET** su **Nessun codice gestito**.

   ![Non impostare il Codice gestito per la versione CLR.NET.](index/_static/edit-apppool-ws2016.png)
     
    Nota: l'impostazione della **versione CLR.NET** su **Nessun codice gestito** è facoltativa. ASP.NET Core non si basa sul caricamento del CLR del desktop.

9. Confermare che l'identità del modello del processo disponga delle autorizzazioni appropriate.

   Se l'identità predefinita del pool di app (**Modello di processo** > **Identità**) viene modificata da **ApplicationPoolIdentity** a un'altra identità, verificare che la nuova identità disponga delle autorizzazioni necessarie per l'accesso alla cartella dell'app, al database e alle altre risorse richieste.
   
## <a name="deploy-the-app"></a>Distribuire l'app

Distribuire l'app nella cartella creata nel sistema IIS di destinazione. Il metodo consigliato per la distribuzione è [Distribuzione Web](/iis/publish/using-web-deploy/introduction-to-web-deploy).

Verificare che l'app pubblicata per la distribuzione non sia in esecuzione. I file nella cartella *publish* sono bloccati durante l'esecuzione dell'app. I file bloccati non possono essere sovrascritti. Per rilasciare i file bloccati in una distribuzione, arrestare il pool di applicazioni:

* Manualmente in Gestione IIS sul server.
* Usando Distribuzione Web e facendo riferimento a `Microsoft.NET.Sdk.Web` nel file di progetto. Nella radice della directory dell'app web viene posizionato un file *app_offline.htm*. Quando il file è presente, il modulo ASP.NET Core arresta normalmente l'app e rende disponibile il file *app_offline.htm* durante la distribuzione. Per altre informazioni, vedere la [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module#appofflinehtm).
* Usare PowerShell per arrestare e riavviare il pool di applicazioni (richiede PowerShell 5 o versione successiva):
  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

### <a name="web-deploy-with-visual-studio"></a>Distribuzione web con Visual Studio

Vedere l'argomento [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) (Profili di pubblicazione Visual Studio per la distribuzione di app ASP.NET Core) per informazioni su come creare un profilo di pubblicazione da usare con Distribuzione Web. Se il provider di hosting rende disponibile un Profilo di pubblicazione o il supporto per crearne uno, scaricare il profilo e importarlo usando la finestra di dialogo **Pubblica** di Visual Studio.

![Pagina della finestra di dialogo Pubblica](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Distribuzione Web all'esterno di Visual Studio

È anche possibile usare [Distribuzione Web](/iis/publish/using-web-deploy/introduction-to-web-deploy) all'esterno di Visual Studio dalla riga di comando. Per altre informazioni sulla distribuzione, vedere [Strumento Distribuzione Web](/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Alternative a Distribuzione web

Usare uno dei metodi disponibili, ad esempio Xcopy, Robocopy o PowerShell per spostare l'app nel sistema host. Gli utenti di Visual Studio possono usare gli [Esempi di pubblicazione](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).

## <a name="browse-the-website"></a>Esplorare il sito Web

![Il browser Microsoft Edge ha caricato la pagina di avvio IIS.](index/_static/browsewebsite.png)

## <a name="data-protection"></a>Protezione dei dati

La protezione dei dati viene usata da diversi middlewares di ASP.NET, inclusi quelli usati nell'autenticazione. Anche se le DPAPI (Data Protection API) non vengono chiamate dal codice dell'utente, è consigliabile configurare Protezione dati per la creazione di un archivio chiavi permanente con uno script di distribuzione o nel codice dell'utente. Se non si configura la protezione dei dati, le chiavi vengono mantenute in memoria ed eliminate al riavvio dell'app.

Se il gruppo di chiavi viene archiviato in memoria, quando l'app viene riavviata:

* Tutti i token di autenticazione dei moduli non sono più validi. 
* Gli utenti devono ripetere l'accesso alla richiesta successiva. 
* Tutti i dati protetti con il gruppo di chiavi non possono più essere decrittografati.

Per configurare la protezione dei dati in IIS è necessario usare **uno** degli approcci seguenti:

### <a name="create-a-data-protection-registry-hive"></a>Creare un Hive del Registro di sistema di protezione dei dati

Le chiavi di protezione dei dati usate dalle app ASP.NET vengono archiviate in hive del registro esterni alle app. Per mantenere le chiavi per una determinata app, creare un hive del registro per il pool dell'app.

Per le installazioni IIS autonome non web farm è possibile usare lo [script PowerShell Data Protection Provision-AutoGenKeys.ps1 (Provisioning di protezione dati-AutoGenKeys.ps1)](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) per ogni pool di app usato con un'app ASP.NET Core. Questo script crea una chiave del Registro di sistema speciale nel registro HKLM che viene inclusa nell'elenco di controllo di accesso (ACL) solo per l'account del processo di lavoro. Le chiavi vengono crittografate a riposo tramite DPAPI con una chiave a livello del computer.

In scenari di web farm un'app può essere configurata in modo da usare un percorso UNC per archiviare il relativo gruppo di chiavi di protezione dei dati. Per impostazione predefinita, le chiavi di protezione dei dati non vengono crittografate. Assicurarsi che le autorizzazioni file per una condivisione di questo tipo siano limitate all'app che viene eseguita come account di Windows. È anche possibile usare un certificato X509 per la protezione delle chiavi a riposo. Considerare l'implementazione di un meccanismo per consentire agli utenti di caricare i certificati: inserire i certificati nell'archivio certificati attendibili dell'utente e assicurarsi che siano disponibili in tutti i computer in cui viene eseguita l'app dell'utente. Vedere [Configurazione della protezione dati](xref:security/data-protection/configuration/overview) per altri dettagli.

### <a name="configure-the-iis-application-pool-to-load-the-user-profile"></a>Configurare il Pool di applicazioni IIS per caricare il profilo utente

Questa impostazione si trova nella sezione **Modello del processo** in **Impostazioni avanzate** per il pool di app. Impostare Carica profilo utente su `True`. Questa operazione archivia le chiavi nella directory del profilo utente e le mantiene protette tramite DPAPI con una chiave specifica per l'account utente usato per il pool di app.

### <a name="use-the-file-system-as-a-key-ring-store"></a>Usare il file system come archivio del gruppo di chiavi

Modificare il codice dell'app per [usare il file system come archivio del gruppo di chiavi](xref:security/data-protection/configuration/overview). Usare un certificato X509 per proteggere le chiavi e verificare che sia un certificato attendibile. Se si tratta di un certificato autofirmato, inserirlo nell'archivio radice attendibile.

Quando si usa IIS in una web farm:

* Usare una condivisione di file a cui possono accedere tutti i computer.
* Distribuire un certificato X509 in ogni computer. Configurare [protezione dei dati nel codice](xref:security/data-protection/configuration/overview).

### <a name="set-a-machine-wide-policy-for-data-protection"></a>Impostare criteri a livello di computer per la protezione dei dati

Il sistema di protezione dei dati ha un supporto limitato per l'impostazione di [criteri a livello di computer](xref:security/data-protection/configuration/machine-wide-policy) predefiniti per tutte le app che usano le API di protezione dei dati. Vedere la documentazione sulla [protezione dei dati](xref:security/data-protection/index) per maggiori dettagli.

## <a name="configuration-of-sub-applications"></a>Configurazione delle sotto-applicazioni

Le sotto-app aggiunte sotto l'app radice non devono includere il modulo ASP.NET Core come gestore. Se il modulo viene aggiunto come gestore nel file *web.config* di una sotto-app, quando si prova a esplorare la sotto-app si riceve un messaggio 500.19 (Errore interno del server) che segnala errori nel file di configurazione. Nell'esempio seguente viene illustrato il contenuto di un file *web.config* pubblicato per una sotto-app di ASP.NET Core:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Se si ospita una sotto-app non ASP.NET Core in un'app ASP.NET Core, è necessario rimuovere in modo esplicito il gestore ereditato nel file *web.config* della sotto-app:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Per altre informazioni sulla configurazione del modulo di ASP.NET Core, vedere l'argomento [Introduzione al modulo di ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) e la [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

## <a name="configuration-of-iis-with-webconfig"></a>Configurazione di IIS con web.config

La configurazione di IIS è influenzata dalla sezione **\<system.webServer>** di *web.config* per le funzionalità IIS valide per una configurazione proxy inverso. Se IIS è configurato a livello di sistema per l'uso della compressione dinamica, tale impostazione può essere disattivata per un'app mediante l'elemento **\<urlCompression>** nel file *web.config* dell'app stessa. Per altre informazioni, vedere il [riferimento per la configurazione di \<system.webServer>](https://docs.microsoft.com/iis/configuration/system.webServer/), la [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) e [Uso dei moduli IIS con ASP.NET Core](xref:host-and-deploy/iis/modules). Se è necessario impostare variabili di ambiente per singole app in esecuzione in pool di app isolate (supportati in IIS 10.0+), vedere la sezione *Comando AppCmd.exe* dell'argomento [Variabili di ambiente \<environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) nella documentazione di riferimento di IIS.

## <a name="configuration-sections-of-webconfig"></a>Sezioni di configurazione di web.config

Le sezioni di configurazione di app ASP.NET Framework in *web.config* non vengono usate dalle app ASP.NET Core per la configurazione:

* **\<system.web>**
* **\<appSettings>**
* **\<connectionStrings>**
* **\<location>**

Le applicazioni ASP.NET Core vengono configurate tramite altri provider di configurazione. Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration/index).

## <a name="application-pools"></a>Pool di applicazioni

Quando si ospitano più siti Web in un unico sistema, isolare le app le une dalle altre eseguendo ogni app nel pool di app corrispondente. La finestra di dialogo **Aggiungi sito Web** di IIS ha questa configurazione come impostazione predefinita. Quando si specifica un valore in **Nome del sito**, il testo viene automaticamente trasferito alla casella di testo **Pool di applicazioni**. Quando si aggiunge il sito Web viene creato un nuovo pool di app con il nome del sito.

## <a name="application-pool-identity"></a>Identità del pool di applicazione

Un account di identità del pool di app consente di eseguire un'app in un account univoco, senza dover creare e gestire domini o account locali. In IIS 8.0+ il processo di lavoro amministrazione IIS (WAS) crea un account virtuale con il nome del nuovo pool di app ed esegue i processi di lavoro del pool di applicazioni inclusi nell'account per impostazione predefinita. Nella Console di gestione IIS in **Impostazioni avanzate** per il pool di app verificare che **Identità** sia impostata su **ApplicationPoolIdentity**:

![Finestra di dialogo Impostazioni avanzate del pool di applicazione](index/_static/apppool-identity.png)

Il processo di gestione IIS crea un identificatore sicuro con il nome del pool di app nel sistema di sicurezza di Windows. È possibile proteggere le risorse con questa identità, che tuttavia non è un account utente reale e non viene visualizzata nella Console di gestione utenti di Windows.

Se il processo di lavoro IIS richiede l'accesso con privilegi elevati all'app, modificare l'elenco di controllo di accesso (ACL) della directory contenente l'app:

1. Aprire Esplora risorse, quindi spostarsi nella directory.

2. Fare clic con il pulsante destro del mouse sulla directory e scegliere **Proprietà**.

3. Nella scheda **Sicurezza** selezionare il pulsante **Modifica** e quindi il pulsante **Aggiungi**.

4. Fare clic sul pulsante **Percorsi** e verificare che il sistema sia selezionato.

5. Immettere **IIS AppPool\DefaultAppPool** nella casella di testo **Immettere i nomi degli oggetti da selezionare**.

   ![Selezionare la finestra di dialogo degli utenti o dei gruppi per la cartella dell'app](index/_static/select-users-or-groups-1.png)

6. Fare clic sul pulsante **Controlla nomi**. Scegliere **OK**.

   ![Selezionare la finestra di dialogo degli utenti o dei gruppi per la cartella dell'app](index/_static/select-users-or-groups-2.png)

Questa operazione può essere eseguita al prompt dei comandi usando lo strumento **ICACLS**:

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="additional-resources"></a>Risorse aggiuntive

* [Risolvere i problemi di ASP.NET Core in IIS](xref:host-and-deploy/iis/troubleshoot)
* [Errori comuni di Azure App Service e IIS con ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Introduzione al modulo di ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Utilizzo dei moduli IIS con ASP.NET Core](xref:host-and-deploy/iis/modules)
* [Introduzione a ASP.NET Core](../index.md)
* [Il sito IIS ufficiale di Microsoft](https://www.iis.net/)
* [Libreria di Microsoft TechNet: Windows Server](https://docs.microsoft.com/windows-server/windows-server-versions)
