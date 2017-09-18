---
title: Host ASP.NET Core in Windows con IIS
author: guardrex
description: Configurazione di Windows Server Internet Information Services (IIS) e distribuzione di applicazioni di ASP.NET Core.
keywords: ASP.NET Core, servizi di informazioni internet, iis, server windows, aggregazione di hosting, modulo di asp.net core, distribuzione web
ms.author: riande
manager: wpickett
ms.date: 03/13/2017
ms.topic: article
ms.assetid: a4449ad3-5bad-410c-afa7-dc32d832b552
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/iis
ms.openlocfilehash: 351f3519643bc88fc3dd1c4fbac1c144c6837523
ms.sourcegitcommit: 0a70706a3814d2684f3ff96095d1e8291d559cc7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/22/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-windows-with-iis-and-deploy-to-it"></a>Configurare un ambiente di hosting per ASP.NET Core in Windows con IIS e distribuirlo

Di [Luke Latham](https://github.com/GuardRex) e [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>Sistemi operativi supportati

Sono supportati i sistemi operativi seguenti:

* Windows 7 e versioni più recenti

* Windows Server 2008 R2 e più recenti &#8224;

&#8224; Concettualmente, la configurazione di IIS descritta in questo documento si applica anche per l'hosting delle applicazioni ASP.NET Core in Nano Server IIS, ma fare riferimento a [ASP.NET Core con IIS su Nano Server](xref:tutorials/nano-server) per istruzioni specifiche.

Il [server WebListener](xref:fundamentals/servers/weblistener) non funzionerà in una configurazione di proxy inverso con IIS. È necessario usare il [server Kestrel](xref:fundamentals/servers/kestrel).

## <a name="iis-configuration"></a>Configurazione di IIS

Abilitare il ruolo **Server Web (IIS)** e stabilire i servizi di ruolo.

### <a name="windows-desktop-operating-systems"></a>Sistemi operativi desktop di Windows

Passare a **Pannello di controllo > Programmi > Programmi e funzionalità > Disattivazione o attivare le funzionalità di Windows** (a sinistra dello schermo). Aprire il gruppo per **Internet Information Services** e **Strumenti di gestione Web**. Selezionare la casella per **Console di gestione IIS**. Selezionare la casella per **Servizi World Wide Web**. Accettare le funzionalità predefinite per **servizi World Wide Web** o personalizzare le funzionalità IIS in base alle esigenze.

![La console di gestione IIS e servizi World Wide Web sono selezionati nelle funzionalità di Windows.](iis/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a>Sistemi operativi Windows Server

Per i sistemi operativi del server, usare la procedura guidata **Aggiungi ruoli e funzionalità** tramite il menù **Gestisci** o il collegamento in **Server Manager**. Nel passaggio **Ruoli del server** selezionare la casella per **Server Web (IIS)**.

![Il ruolo Server Web IIS viene selezionato nel passaggio dei ruoli del server Selezionare.](iis/_static/server-roles-ws2016.png)

Nel passaggio **Servizi ruolo** selezionare i servizi ruolo IIS desiderato o accettare i servizi ruolo predefiniti forniti.

![I servizi ruolo predefiniti vengono selezionati nel passaggio Selezionare i servizi ruolo.](iis/_static/role-services-ws2016.png)

Procedere con il passaggio **Conferma** per installare il ruolo del server web e i servizi. Un riavvio del server/IIS non è necessario dopo l'installazione del ruolo Server Web (IIS).

## <a name="install-the-net-core-windows-server-hosting-bundle"></a>Installare l'aggregazione di Hosting di.NET Core Windows Server

1. Installare l'[aggregazione di Hosting di.NET Core Windows Server](https://aka.ms/dotnetcore.2.0.0-windowshosting) nel sistema di hosting. L'aggregazione installerà il Runtime di .NET Core, la libreria di .NET Core e il [Modulo di ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). Il modulo crea il proxy inverso tra IIS e il server Kestrel. Nota: se il sistema non dispone di una connessione a Internet, ottenere e installare *[Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840)*  prima di installare l'aggregazione di Hosting di .NET Core Windows Server.

2. Riavviare il sistema o eseguire **net stop was /y** seguito da **net start w3svc** da un prompt dei comandi per visualizzare una modifica al percorso di sistema.

> [!NOTE]
> Se si usa una configurazione condivisa di IIS, vedere [Modulo di ASP.NET Core con configurazione condivisa di IIS](xref:hosting/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Installare la Distribuzione Web quando si pubblica con Visual Studio

Se si prevede di distribuire le applicazioni con Distribuzione Web in Visual Studio, installare la versione più recente di Distribuzione Web nel sistema di hosting. Per installare Distribuzione Web, è possibile usare il [Programma di installazione dela piattaforma Web (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) o ottenere direttamente da un programma di installazione di [Microsoft Download Center](https://www.microsoft.com/search/result.aspx?q=webdeploy&form=dlc). Il metodo preferito consiste nell'usare WebPI. WebPI offre un'installazione autonoma e una configurazione per i provider di hosting.

## <a name="application-configuration"></a>Configurazione dell'applicazione

### <a name="enabling-the-iisintegration-components"></a>Abilitare i componenti IISIntegration

Includere una dipendenza sul pacchetto *Microsoft.AspNetCore.Server.IISIntegration* nelle dipendenze dell'applicazione. Incorporare il middleware di IIS Integration nell'applicazione aggiungendo l'estensione *. UseIISIntegration()* del metodo di estensione *WebHostBuilder()*. Si noti che la chiamata del codice *. UseIISIntegration()* non influisce sulla portabilità del codice.

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseContentRoot(Directory.GetCurrentDirectory())
    .UseIISIntegration()
    .UseStartup<Startup>()
    .Build();
```

### <a name="setting-iisoptions-for-the-iisintegration-service"></a>Impostare IISOptions per il servizio IISIntegration

Per configurare le opzioni del servizio *IISIntegration*, includere una configurazione del servizio per *IISOptions* in *ConfigureServices*.

```csharp
services.Configure<IISOptions>(options => {
  ...
});
```

| Opzione | Impostazione|
| --- | --- | 
| AutomaticAuthentication | Se true, il middleware di autenticazione modificherà l'utente della richiesta in arrivo e risponderà alle sfide generiche. Se false, il middleware di autenticazione fornirà solo l'identità e risponderà alle sfide quando richiesto esplicitamente da theAuthenticationScheme |
| ForwardClientCertificate | Se true e se l'intestazione `MS-ASPNETCORE-CLIENTCERT` della richiesta è presente, il `ITLSConnectionFeature` verrà popolato. |
| ForwardWindowsAuthentication | Se true, il middleware di autenticazione eseguirà un tentativo di eseguire l'autenticazione usando l'autenticazione della finestra dell'intestazione della piattaforma. Se false, il middleware di autenticazione non verrà aggiunto. |

### <a name="webconfig"></a>web.config

Il file *web.config* configura il modulo di ASP.NET Core e fornisce altre configurazioni di IIS. La creazione, trasformazione e pubblicazione *web.config* viene gestita da `Microsoft.NET.Sdk.Web`, che viene incluso quando si imposta l'SDK del progetto nella parte superiore del file *csproj*, `<Project Sdk="Microsoft.NET.Sdk.Web">`. Per impedire che la destinazione MSBuild trasformi il file *web.config*, aggiungere la proprietà  **\<IsTransformWebConfigDisabled >** al file di progetto con un'impostazione di `true`:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Se non si dispone di un file *web.config* nel progetto quando si pubblica con *dotnet.publish* o con la pubblicazione con Visual Studio, il file viene creato automaticamente nell'output pubblicato. Se si dispone del file nel progetto, esso viene trasformato con i corretti *processPath* e *argomenti* per configurare il modulo di ASP.NET Core e spostato nell'output pubblicato. La trasformazione non tocca le impostazioni di configurazione di IIS incluse nel file.

## <a name="create-the-iis-website"></a>Creare il sito Web IIS

1. Nel sistema IIS di destinazione creare una cartella per contenere le cartelle e i file pubblicati dell'applicazione, che sono descritti in [Struttura della Directory](xref:hosting/directory-structure).

2. All'interno della cartella creata creare una cartella dei *registri* per contenere i registri applicazioni (se si prevede di abilitare la registrazione). Se si prevede di distribuire l'applicazione con una cartella *registri* nel payload, è possibile saltare questo passaggio.

3. In **Gestione IIS**, creare un nuovo sito Web. Fornire un **Nome del sito** e impostare il **Percorso fisico** alla cartella di distribuzione dell'applicazione che è stata creata. Fornire la configurazione dell'**Associazione** e creare il sito Web.

4. Impostare il pool di applicazioni su **Nessun codice gestito**. ASP.NET Core viene eseguito in un processo separato e gestisce il runtime.

5. Aprire la finestra **Aggiungi sito Web**.

   ![Fare clic su Aggiungi sito Web dal menu di scelta rapida dei siti.](iis/_static/add-website-context-menu-ws2016.png)

6. Configurare il sito Web.

   ![Specificare il nome del sito, il percorso fisico e il nome Host nel passaggio Aggiungi sito Web.](iis/_static/add-website-ws2016.png)

7. Nel riquadro **Pool di applicazioni** aprire la finestra **Modifica Pool di applicazioni** facendo clic col tasto destro sul pool di applicazioni del sito Web e selezionando **Impostazioni Basic...**  dal menu a comparsa.

   ![Selezionare le Impostazioni Basic dal menu di scelta rapida del Pool di applicazioni.](iis/_static/apppools-basic-settings-ws2016.png)

8. Impostare la **versione CLR.NET** su **Nessun codice gestito**.

   ![Non impostare il Codice gestito per la versione CLR.NET.](iis/_static/edit-apppool-ws2016.png)
     
    Nota: l'impostazione della **versione CLR.NET** su **Nessun codice gestito** è facoltativa. ASP.NET Core non si basa sul caricamento del CLR del desktop.

9. Confermare che l'identità del modello del processo disponga delle autorizzazioni appropriate.

    Se si modifica l'identità predefinita del pool di applicazioni (**Modello di processo** > **identità**) da **ApplicationPoolIdentity** a un'altra identità, verificare che la nuova identità disponga delle autorizzazioni necessarie per accedere alla cartella dell'applicazione, al database e alle altre risorse richieste.
   
## <a name="deploy-the-application"></a>Distribuire l'applicazione
Distribuire l'applicazione nella cartella creata nel sistema IIS di destinazione. Distribuzione Web è il metodo consigliato per la distribuzione. Le alternative a Distribuzione Web sono elencate di seguito.

Verificare che l'app pubblicata per la distribuzione non sia in esecuzione. I file nella cartella *publish* sono bloccati durante l'esecuzione dell'app. La distribuzione non viene eseguita perché non è possibile copiare i file bloccati.

### <a name="web-deploy-with-visual-studio"></a>Distribuzione web con Visual Studio
Vedere l'argomento [Creare profili di pubblicazione per Visual Studio e MSBuild, per distribuire le app ASP.NET Core](xref:publishing/web-publishing-vs#publish-profiles) per informazioni su come creare un profilo di pubblicazione da usare con Distribuzione Web. Se il provider di hosting fornisce un Profilo di pubblicazione o il supporto per crearne uno, scaricare il profilo e importarlo usando la finestra di dialogo di Visual Studio **Pubblica**.

![Pagina della finestra di dialogo Pubblica](iis/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Distribuzione Web all'esterno di Visual Studio
È anche possibile usare Distribuzione Web all'esterno di Visual Studio dalla riga di comando. Per altre informazioni sulla distribuzione, vedere [Strumento Distribuzione Web](https://technet.microsoft.com/library/dd568996(WS.10).aspx).

### <a name="alternatives-to-web-deploy"></a>Alternative a Distribuzione web
Se non si desidera usare Distribuzione Web o non si usa Visual Studio, è possibile usare uno dei diversi metodi per spostare l'applicazione nel sistema di hosting, ad esempio Xcopy, Robocopy o PowerShell. Gli utenti di Visual Studio possono usare gli [Esempi di pubblicazione](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).

## <a name="browse-the-website"></a>Esplorare il sito Web
![Il browser Microsoft Edge ha caricato la pagina di avvio IIS.](iis/_static/browsewebsite.png)
   
>[!WARNING]
> Le applicazioni .NET core sono ospitate tramite un proxy inverso tra IIS e il server Kestrel. Per creare il proxy inverso, il file *web.config* deve essere presente nel percorso radice del contenuto (in genere il percorso di base dell'app) dell'applicazione distribuita, che è il percorso fisico del sito Web fornito a IIS. I file riservati esistono nel percorso fisico dell'app, incluse le sottocartelle, ad esempio *my_application.runtimeconfig.json*, *my_application.xml* (commenti della documentazione XML), e *my_application.deps.json*. Il file *web.config* è necessario per creare il proxy inverso in Kestrel, che impedisce a IIS di usare questi e altri file riservati. **Pertanto, è importante che il file *web.config* non sia mai accidentalmente rinominato o rimosso dalla distribuzione.**

## <a name="data-protection"></a>Protezione dei dati

Un'applicazione ASP.NET Core archivierà il gruppo di chiavi in memoria alla condizione seguente:

* Un sito Web è ospitato dietro a IIS.
* Lo stack di protezione dei dati non è stato configurato per archiviare il gruppo di chiavi in un archivio permanente.

Se il gruppo di chiavi verrà archiviato in memoria, quando l'app viene riavviata:

* Tutti i token di autenticazione dei moduli non saranno validi. 
* Gli utenti dovranno accedere di nuovo per la richiesta successiva. 
* Tutti i dati che sono stati protetti con il gruppo di chiavi non saranno più protetti.

> [!WARNING]
> La protezione dei dati viene usata da diversi middlewares di ASP.NET, inclusi quelli usati nell'autenticazione. Anche se non si chiamano in modo specifico le API di protezione dei dati dal proprio codice è necessario configurare la protezione dei dati con uno script di distribuzione o nel proprio codice. Se non si configura la protezione dei dati, per impostazione predefinita, le chiavi verranno conservate in memoria ed eliminate quando l'app viene riavviata. Il riavvio invaliderà i cookie scritti per l'autenticazione dei cookie e gli utenti dovranno accedere di nuovo.

Per configurare la protezione dei dati in IIS è necessario usare uno degli approcci seguenti:

* Eseguire uno [script di powershell](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) per creare le voci del Registro di sistema appropriato (ad esempio, `.\Provision-AutoGenKeys.ps1 DefaultAppPool`). Questo archivierà le chiavi nel registro, protetto tramite DPAPI con una chiave del livello del computer.
* Configurare il Pool di applicazioni IIS per caricare il profilo utente. Questa impostazione è nella sezione **Modello del processo** in **Impostazioni avanzate** per il pool di applicazioni. Impostare **Caricamento del profilo utente** su `True`. Questo archivierà le chiavi nella directory del profilo utente e le terrà protette tramite DPAPI con una chiave specifica per l'account utente usato per il pool di applicazioni.
* Modificare il codice dell'applicazione per [usare il file system come archivio del gruppo di chiavi](https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/overview). Usare un certificato X509 per proteggere le chiavi e assicurarsi che sia un certificato attendibile. Ad esempio, in caso di un certificato autofirmato, è necessario inserirlo nell'archivio di Radice attendibile.

Quando si usa IIS in una web farm:

* Usare una condivisione di file a cui possano accedere tutti i computer.
* Distribuire un certificato X509 in ogni computer.  Configurare [protezione dei dati nel codice](https://docs.asp.net/en/latest/security/data-protection/configuration/overview.html).

### <a name="1-create-a-data-protection-registry-hive"></a>1. Creare un Hive del Registro di sistema di protezione dei dati

Le chiavi di protezione dei dati usate dalle applicazioni ASP.NET vengono archiviate nell'hive del registro esterno alle applicazioni. Per mantenere le chiavi per una determinata applicazione, è necessario creare un hive del registro per il pool di applicazioni dell'applicazione.

Per le installazioni autonome IIS, è possibile usare lo [script PowerShell AutoGenKeys.ps1 - Effettuare il provisioning di protezione dei dati](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) per ogni pool di applicazioni usato con un'applicazione ASP.NET Core. Questo script creerà una chiave del registro speciale nel registro HKLM che ACLed solo per l'account del processo di lavoro. Le chiavi vengono crittografate a riposo tramite la DPAPI.

In scenari di web farm un'applicazione può essere configurata per usare un percorso UNC per archiviare il relativo gruppo di chiavi di protezione dei dati. Per impostazione predefinita, le chiavi di protezione dei dati non vengono crittografate. È necessario assicurarsi che le autorizzazioni del file per una condivisione di questo tipo siano limitate per l'applicazione che viene eseguita come l'account di Windows. Inoltre è possibile scegliere di proteggere le chiavi a riposo tramite un certificato X509. Si consiglia di prendere in considerazione un meccanismo per consentire agli utenti di caricare i certificati, inserirli nell'archivio dei certificati attendibili dell'utente e assicurarsi che siano disponibili in tutti i computer in cui verrà eseguita l'applicazione dell'utente. Vedere [Configurazione della protezione dati](xref:security/data-protection/configuration/overview#data-protection-configuring) per altri dettagli.

### <a name="2-configure-the-iis-application-pool-to-load-the-user-profile"></a>2. Configurare il Pool di applicazioni IIS per caricare il profilo utente
Questa impostazione è nella sezione Modello del processo in Impostazioni avanzate per il pool di applicazioni. Impostare il Caricamento del profilo utente su True. Questo archivierà le chiavi nella directory del profilo utente e le terrà protette tramite DPAPI con una chiave specifica per l'account utente usato per il pool di applicazioni.

### <a name="3-machine-wide-policy-for-data-protection"></a>3. Criteri a livello di computer per la protezione dei dati

Il sistema di protezione dei dati ha un supporto limitato per impostazione predefinita per i [criteri a livello di computer](xref:security/data-protection/configuration/machine-wide-policy#data-protection-configuration-machinewidepolicy) per tutte le applicazioni che usano le API di protezione dei dati. Vedere la documentazione sulla [protezione dei dati](xref:security/data-protection/index) per maggiori dettagli.

## <a name="configuration-of-sub-applications"></a>Configurazione delle sotto-applicazioni

Sotto applicazioni aggiunte sotto l'applicazione della radice non devono includere il modulo di ASP.NET Core come gestore. Se si aggiunge il modulo come un gestore in un file della sotto applicazione *web.config*, si riceve un riferimento 500.19 (errore del server interno) al file di configurazione non corretto quando si tenta di passare alla sotto app. Nell'esempio seguente viene illustrato il contenuto di un file pubblicato *web.config* per una sotto app di ASP.NET Core:

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

Se si intende ospitare una sotto app di ASP.NET Core sotto un'app ASP.NET Core, è necessario rimuovere in modo esplicito il gestore ereditato nel file della sub app *web.config*:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
        arguments=".\MyApp.dll" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Per altre informazioni sulla configurazione del modulo di ASP.NET Core con il file *web.config*, vedere l'argomento[Introduzione al modulo di ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) e il [riferimento alla configurazione del modulo di ASP.NET Core](xref:hosting/aspnet-core-module).

## <a name="configuration-of-iis-with-webconfig"></a>Configurazione di IIS con web.config

La configurazione di IIS è comunque influenzata dalla sezione `<system.webServer>` di *web.config* per tali funzionalità IIS che si applicano a una configurazione di proxy inverso. Ad esempio, è possibile disporre della IIS configurata a livello di sistema per usare la compressione dinamica, ma è possibile disabilitare questa impostazione per un'app con l'elemento `<urlCompression>` nel file dell'app *web.config*. Per altre informazioni, vedere il riferimento alla configurazione [ per `<system.webServer>`](https://www.iis.net/configreference/system.webserver), [Riferimento di configurazione del modulo di ASP.NET Core](xref:hosting/aspnet-core-module), e [Uso dei moduli di IIS con ASP.NET Core](xref:hosting/iis-modules). Se è necessario impostare le variabili di ambiente per le singole app in esecuzione nel pool di applicazioni isolate (supportate in IIS 10.0+), vedere la sezione *Comando AppCmd.exe* dell'argomento [Variabili di ambiente \<environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) nella documentazione di riferimento.

## <a name="configuration-sections-of-webconfig"></a>Sezioni di configurazione di web.config

A differenza delle applicazioni .NET Framework che sono configurate con gli elementi `<system.web>`, `<appSettings>`, `<connectionStrings>`, e `<location>` in *web.config*, le app ASP.NET Core vengono configurate tramite altri provider di configurazione. Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration).

## <a name="application-pools"></a>Pool di applicazioni

Quando si ospitano più siti Web in un unico sistema, è necessario isolare le applicazioni le une dalle altre eseguendo ciascuna app nel proprio pool di applicazioni. La finestra di dialogo di IIS **Aggiungi sito Web** ha per impostazione predefinita questo comportamento. Quando si fornisce un **Nome del sito**, il testo viene automaticamente trasferito alla casella di testo **Pool di applicazioni**. Verrà creato un nuovo pool di applicazioni usando il nome del sito quando si aggiunge il sito Web.

## <a name="application-pool-identity"></a>Identità del pool di applicazione

Un account di identità di pool di applicazioni consente di eseguire un'applicazione in un account univoco senza la necessità di creare e gestire i domini o gli account locali. In IIS 8.0 + il processo di lavoro amministrazione IIS (WAS) crea un account virtuale con il nome del nuovo pool di applicazioni e l'esecuzione di processi di lavoro del pool di applicazioni con questo account per impostazione predefinita. Nella Console di gestione IIS in Impostazioni avanzate per il pool di applicazioni, verificare che l'identità venga impostata per usare **ApplicationPoolIdentity** come illustrato nell'immagine seguente.

![Finestra di dialogo Impostazioni avanzate del pool di applicazione](iis/_static/apppool-identity.png)

Il processo di gestione IIS crea un identificatore sicuro con il nome del pool di applicazioni nel sistema di sicurezza di Windows. Le risorse possono essere protette tramite questa identità. Tuttavia, questa identità non è un account utente reale e non verrà visualizzata nella Console di gestione utenti di Windows.

Se è necessario concedere l'accesso elevato del processo di lavoro IIS all'applicazione, sarà necessario modificare l'elenco di controllo di accesso (ACL) per la directory contenente l'applicazione.

1. Aprire Esplora risorse, quindi spostarsi nella directory.

2. Fare clic con il pulsante destro sulla directory e fare clic su **Proprietà**.

3. Sotto la scheda **Sicurezza**, fare clic sul pulsante **Modifica** pulsante e quindi il pulsante **Aggiungi**.

4. Fare clic su di **percorsi** pulsante e assicurarsi di selezionare il sistema.

5. Immettere **IIS AppPool\DefaultAppPool** nella casella di testo **Immettere i nomi degli oggetti da selezionare**.

  ![Selezionare la finestra di dialogo degli utenti o dei gruppi per la cartella dell'applicazione](iis/_static/select-users-or-groups-1.png)

6. Fare clic sul pulsante **Controlla nomi** e quindi fare clic su **OK**.

  ![Selezionare la finestra di dialogo degli utenti o dei gruppi per la cartella dell'applicazione](iis/_static/select-users-or-groups-2.png)

È possibile farlo anche tramite un prompt dei comandi usando lo strumento **ICACLS**:

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="troubleshooting-tips"></a>Suggerimenti per la risoluzione dei problemi

Per diagnosticare problemi con le distribuzioni di IIS, studiare l'output del browser, esaminare il registro dell'**applicazione** del sistema tramite **Visualizzatore eventi**e abilitare la registrazione`stdout`. Il registro del **Modulo di ASP.NET Core** verrà trovato nel percorso fornito nell'attributo *stdoutLogFile* dell'elemento `<aspNetCore>` in *web.config*. Le cartelle nel percorso fornito nel valore dell'attributo devono essere presenti nella distribuzione. È anche necessario impostare *stdoutLogEnabled = "true"*. Le applicazioni che usano l'SDK `Microsoft.NET.Sdk.Web` per creare il file *web.config* useranno come impostazione definita *stdoutLogEnabled* su *false*, pertanto è necessario specificare manualmente il file *web.config* o modificare il file per permettere la `stdout` registrazione.

Molti errori comuni non vengono visualizzati nel browser, nel Registro dell'applicazione e nel Registro del modulo di ASP.NET Core fino a quando il modulo *startupTimeLimit* (impostazione predefinita: 120 secondi) e *startupRetryCount* (valore predefinito: 2) non sono stati superati. Pertanto, attendere sei minuti completi prima di dedurre che il modulo non è riuscito ad avviare un processo per l'applicazione.

Un modo veloce per determinare se l'applicazione funziona correttamente è quello di eseguire l'applicazione direttamente su Kestrel. Se è stata pubblicata l'applicazione come una distribuzione dipendente dal framework, eseguire **dotnet my_application.dll** nella cartella di distribuzione, che è il percorso fisico di IIS per l'applicazione. Se è stata pubblicata l'applicazione come distribuzione indipendente, eseguire l'eseguibile dell'applicazione direttamente da un prompt dei comandi, **my_application.exe**, nella cartella di distribuzione. Se Kestrel è in ascolto sulla porta predefinita 5000, si deve essere in grado di passare all'applicazione in `http://localhost:5000/`. Se l'applicazione risponde in genere in corrispondenza dell'indirizzo endpoint di Kestrel, è più probabile che il problema sia correlato con la configurazione di IIS ASP.NET Core modulo-Kestrel ed è meno probabile che sia all'interno dell'applicazione stessa.

Un modo per determinare se il proxy inverso IIS al server Kestrel funziona correttamente è quello di eseguire una semplice richiesta di file statici per un foglio di stile, uno script o un'immagine dai file statici dell'applicazione in *wwwroot* tramite [Middleware del File statico](xref:fundamentals/static-files). Se l'applicazione può essere attiva per file statici, ma le visualizzazioni MVC e altri endpoint hanno esito negativo, è meno probabile che il problema sia relazionato con la configurazione di IIS ASP.NET Core modulo-Kestrel e più probabile che sia all'interno dell'applicazione stessa (ad esempio, il routing MVC o l'errore del server 500 Internal).

Quando viene avviato normalmente Kestrel dietro IIS, ma l'app non verrà eseguita nel sistema dopo averla correttamente eseguita in locale, è possibile aggiungere temporaneamente una variabile di ambiente per *web.config* per impostare il `ASPNETCORE_ENVIRONMENT` su `Development`. Fino a quando non si esegue l'override dell'ambiente nell'avvio dell'app, la [pagina d'eccezione dello sviluppatore](xref:fundamentals/error-handling) verrà visualizzata quando l'app viene eseguita nel sistema. Impostare la variabile di ambiente per `ASPNETCORE_ENVIRONMENT` in questo modo è consigliato solo per i sistemi del processo di gestione temporanea/ di test che non sono esposti a Internet. Assicurarsi di rimuovere la variabile di ambiente dal file *web.config* al termine del file. Per informazioni sull'impostazione delle variabili di ambiente tramite *web.config* per il proxy inverso, vedere [elemento figlio environmentVariables di aspNetCore](xref:hosting/aspnet-core-module#setting-environment-variables).

Nella maggior parte dei casi l'abilitazione della registrazione dell'applicazione per ricevere assistenza nella risoluzione dei problemi con l'applicazione o il proxy inverso. Per altre informazioni, vedere [Registrazione](xref:fundamentals/logging).

L'ultimo suggerimento di risoluzione dei problemi è relativo alle app che non vengono eseguite dopo l'aggiornamento di .NET Core SDK nelle versioni computer o pacchetto di sviluppo all'interno dell'app. In alcuni casi i pacchetti incoerenti possono interrompere un'app quando si eseguono aggiornamenti principali. È possibile risolvere la maggior parte di questi problemi eliminando le cartelle `bin` e `obj` nel progetto, cancellando le caches del pacchetto in `%UserProfile%\.nuget\packages\` e `%LocalAppData%\Nuget\v3-cache`, ripristinando il progetto e confermando che la distribuzione precedente sul sistema è stata eliminata completamente prima di distribuire nuovamente l'app.

>[!TIP]
> Un modo pratico per cancellare le cache del pacchetto per ottenere lo strumento `NuGet.exe` da [NuGet.org](https://www.nuget.org/), aggiungerlo al percorso del sistema ed eseguire `nuget locals all -clear` da un prompt dei comandi.

## <a name="common-errors"></a>Errori comuni

Quello che segue non è un elenco completo di errori. Qualora si incontrasse un errore che non è presente qui, lasciare un messaggio di errore dettagliato nella sezione commenti.

### <a name="installer-unable-to-obtain-vc-redistributable"></a>Il programma di installazione non riesce ad ottenere VC++ Ridistribuibile

* **Eccezione del programma di installazione:** 0x80072efd o 0x80072f76 - Errore non specificato

* **Installer Log Exception&#8224;:** Error 0x80072efd oppure0x80072f76: Impossibile eseguire i file EXE del pacchetto

  &#8224; Il log si trova in C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.

Risoluzione dei problemi:

* Se il sistema non ha accesso a Internet durante l'installazione del server che ospita l'aggregazione, questa eccezione si verificherà quando il programma di installazione non riesce ad ottenere il *Microsoft Visual C++ 2015 Redistributable*. È possibile ottenere un programma di installazione da [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840). Se il programma di installazione non riesce, il runtime .NET Core necessario per le distribuzioni dipendenti dal framework host potrebbe non essere ricevuto. Se si prevede di ospitare le distribuzioni dipendenti da framework, verificare che il runtime sia installato nelle funzionalità &amp; Programmi. È possibile ottenere un programma di installazione di runtime da [Download .NET](https://www.microsoft.com/net/download/core). Dopo aver installato il runtime, riavviare il sistema o riavviare IIS eseguendo **net stop was /y** seguito da **net start w3svc** da un prompt dei comandi.

### <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>L'aggiornamento del sistema operativo ha rimosso il modulo di ASP.NET Core a 32 bit

* **Registro dell'applicazione:** Il modulo DLL**C:\WINDOWS\system32\inetsrv\aspnetcore.dll** non è riuscito a caricare. L'errore è nei dati.

Risoluzione dei problemi:

* I file non-OS nella directory **C:\Windows\SysWOW64\inetsrv** non vengono conservati durante un aggiornamento del sistema operativo. Se si dispone di un Modulo di ASP.NET Core installato prima di un aggiornamento del sistema operativo e quindi si prova a eseguire qualsiasi pool di applicazioni in modalità a 32 bit dopo un aggiornamento del sistema operativo, si verificherà questo problema. Dopo un aggiornamento del sistema operativo, ripristinare il Modulo di ASP.NET Core. Vedere [Installare l'aggregazione di Hosting di.NET Core Windows Server](#install-the-net-core-windows-server-hosting-bundle). Selezionare **Ripara** quando si esegue il programma di installazione.

### <a name="platform-conflicts-with-rid"></a>La piattaforma è in conflitto con RID

* **Browser:** errore HTTP 502.5 - errore del processo

* **Registro dell'applicazione:** l'applicazione "MACHINE/WEBROOT/APPHOST/MY_APPLICATION" con radice fisica "C:\{PATH}\' Impossibile avviare il processo con la riga di comando""C:\\{PATH}\my_application.{exe|dll}" ", ErrorCode = "0x80004005 : ff.

* **Registro del modulo ASP.NET Core:** Eccezione non gestita: System.BadImageFormatException: Impossibile caricare il file o l'assembly "my_application.dll". Tentativo di caricare un programma con un formato non corretto.

Risoluzione dei problemi:

* Confermare che l'applicazione venga eseguita in locale su Kestrel. Un errore del processo può essere il risultato di un problema all'interno dell'applicazione. Per altre informazioni, vedere [Consigli per la risoluzione dei problemi](#troubleshooting-tips).

* Confermare di non aver impostato un `<PlatformTarget>` nel *csproj* che è in conflitto con il RID. Ad esempio, non specificare un `<PlatformTarget>` di `x86` e pubblicare con un RID di `win10-x64`, tramite l'uso di *dotnet publish - c Release -r win10-x64* o impostando `<RuntimeIdentifiers>` nel *csproj* su `win10-x64`. Il progetto verrà pubblicato senza avviso o errore, ma con esito negativo in merito alle eccezioni registrate in precedenza nel sistema.

* Se questa eccezione si verifica per una distribuzione di App di Azure durante l'aggiornamento di un'applicazione e la distribuzione di assembly più recenti, eliminare manualmente tutti i file dalla distribuzione precedente. Le assembly incompatibili con il tempo di ritardo possono causare una eccezione `System.BadImageFormatException` quando si distribuisce un'app aggiornata.

### <a name="uri-endpoint-wrong-or-stopped-website"></a>Endpoint dell'URI non corretto o sito web arrestato

* **Browser:** ERR_CONNECTION_REFUSED

* **Registro dell'applicazione:** Nessuna voce

* **Registro del modulo di ASP.NET Core:** il file di registro non è stato creato

Risoluzione dei problemi:

* Confermare che si usa l'endpoint URI corretto per l'applicazione. Controllare le associazioni.

* Confermare che il sito Web IIS non si trovi nello stato *In arresto*.

### <a name="corewebengine-or-w3svc-server-features-disabled"></a>Le funzionalità del server CoreWebEngine o W3SVC sono disabilitate

* **Eccezione del sistema operativo:** per usare il modulo di ASP.NET Core è necessario installare le funzionalità di IIS 7.0 CoreWebEngine e W3SVC.

Risoluzione dei problemi:

* Confermare che siano state abilitati il ruolo e le funzionalità appropriate. Vedere [Configurazione di IIS](#iis-configuration).

### <a name="incorrect-website-physical-path-or-application-missing"></a>Percorso fisico del sito Web non corretto o applicazione mancante

* **Browser:** 403 Non consentito: accesso negato **- OPPURE -** 403.14 Non consentito - Il server Web è configurato per non visualizzare il contenuto di questa directory.

* **Registro dell'applicazione:** Nessuna voce

* **Registro del modulo di ASP.NET Core:** il file di registro non è stato creato

Risoluzione dei problemi:

* Controllare il sito Web IIS **Impostazioni di base** e la cartella dell'applicazione fisica. Confermare che l'applicazione si trovi nella cartella nel sito Web IIS **Percorso fisico**.

### <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>Ruolo non corretto, modulo non installato o autorizzazioni non corrette

* **Browser:** 500.19 Errore interno al server – non è possibile accedere alla pagina richiesta perché i dati di configurazione per la pagina non sono validi.

* **Registro dell'applicazione:** Nessuna voce

* **Registro del modulo di ASP.NET Core:** il file di registro non è stato creato

Risoluzione dei problemi:

* Confermare che sia stato abilitato il ruolo appropriato. Vedere [Configurazione di IIS](#iis-configuration).

* Controllare **Programmi &amp; Funzionalità** e confermare che il **Modulo di Microsoft ASP.NET Core** sia stato installato. Se il **Modulo di Microsoft ASP.NET Core** non è presente nell'elenco dei programmi installati, installare il modulo. Vedere [Installare l'aggregazione di Hosting di.NET Core Windows Server](#install-the-net-core-windows-server-hosting-bundle).

* Assicurarsi che il **Pool di applicazioni > Modello di processo > Identità** sia impostato su **ApplicationPoolIdentity** o che un'identità personalizzata disponga delle autorizzazioni appropriate per accedere alla cartella di distribuzione dell'applicazione.

### <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>ProcessPath non corretto, variabile di percorso mancante, aggregazione di hosting non installata, sistema/IIS non riavviato, VC Redistributable non installato o violazione dell'accesso a dotnet.exe

* **Browser:** errore HTTP 502.5 - errore del processo

* **Registro dell'applicazione:** l'applicazione "MACHINE/WEBROOT/APPHOST/MY_APPLICATION" con radice fisica "C:\\PATH}\' Impossibile avviare il processo con la riga di comando"".\my_application.exe" ", ErrorCode = "0x80070002 : 0.

* **Registro del modulo di ASP.NET Core:** il file di registro è stato creato ma è vuoto

Risoluzione dei problemi:

* Confermare che l'applicazione venga eseguita in locale su Kestrel. Un errore del processo può essere il risultato di un problema all'interno dell'applicazione. Per altre informazioni, vedere [Consigli per la risoluzione dei problemi](#troubleshooting-tips).

* Controllare che l'attributo *processPath* nell'elemento `<aspNetCore>` in *web.config* confermi che si tratta di *dotnet* per una distribuzione dipendente da framework o *.\my_application.exe* per una distribuzione indipendente.

* Per una distribuzione dipendente dal framework, *dotnet.exe* potrebbe non essere accessibile tramite le impostazioni del percorso. Confermare che *C:\Program Files\dotnet\* esiste nelle impostazioni del percorso di sistema.

* Per una distribuzione dipendente da framework, *dotnet.exe* potrebbe non essere accessibile per l'identità dell'utente del Pool di applicazioni. Confermare che l'identità dell'utente di AppPool disponga dell'accesso alla directory *C:\Program Files\dotnet*. Verificare che non siano presenti regole di negazione configurate per l'identità dell'utente di AppPool in *C:\Program Files\dotnet* e nelle directory dell'applicazione.

* È possibile aver distribuito una distribuzione dipendente dal framework e installato .NET Core senza riavviare IIS. Riavviare il server o riavviare IIS eseguendo **net stop was /y** seguito da **net start w3svc** da un prompt dei comandi.

* È possibile aver distribuito una distribuzione dipendente dal framework senza installare il runtime .NET Core nel sistema di hosting. Se si sta tentando di eseguire una distribuzione in una distribuzione dipendente dal framework e non è stato installato il runtime .NET Core, eseguire il **Programma di installazione dell'aggregazione di hosting .NET Core Windows Server** nel sistema. Vedere [Installare l'aggregazione di Hosting di.NET Core Windows Server](#install-the-net-core-windows-server-hosting-bundle). Se si sta cercando di installare il runtime .NET Core in un sistema senza una connessione ad Internet, ottenere il runtime da [NET.Downloads](https://www.microsoft.com/net/download/core) ed eseguire il programma di installazione dell'aggregazione di hosting per installare il modulo di ASP.NET Core. Completare l'installazione riavviando il sistema o riavviando IIS eseguendo **net stop was /y** seguito da **net start w3svc** da un prompt dei comandi.

* È possibile aver distribuito una distribuzione dipendente dal framework e installato .NET Core senza riavviare il sistema/IIS. Riavviare il sistema o riavviare IIS eseguendo **net stop was /y** seguito da **net start w3svc** da un prompt dei comandi.

* È possibile aver distribuito una distribuzione dipendente dal framework e *Microsoft Visual C++ 2015 Redistributable (x64)* non è installato nel sistema. È possibile ottenere un programma di installazione da [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).

### <a name="incorrect-arguments-of-aspnetcore-element"></a>Argomenti non corretti dell'elemento \<aspNetCore\>

* **Browser:** errore HTTP 502.5 - errore del processo

* **Registro dell'applicazione:** l'applicazione "MACHINE/WEBROOT/APPHOST/MY_APPLICATION" con radice fisica "C:\\PATH}\' Impossibile avviare il processo con la riga di comando'"dotnet\my_application.dll", ErrorCode = "0x80004005 : 80008081.

* **Registro di modulo di ASP.NET Core:** l'esecuzione dell'applicazione non esiste: "PATH\my_application.dll"

Risoluzione dei problemi:

* Confermare che l'applicazione venga eseguita in locale su Kestrel. Un errore del processo può essere il risultato di un problema all'interno dell'applicazione. Per altre informazioni, vedere [Consigli per la risoluzione dei problemi](#troubleshooting-tips).

* Esaminare l'attributo *argomenti* nell'elemento `<aspNetCore>` in *web.config* per confermare che si tratta di (a) *.\my_application.dll* per una distribuzione dipendente dal framework. o (b) non è presente, una stringa vuota (*argomenti=""*), o un elenco di argomenti dell'applicazione (*argomenti="arg1, arg2,..."*) per una distribuzione indipendente.

### <a name="missing-net-framework-version"></a>Versione di .NET Framework non presente

* **Browser:** 502.3 Gateway non valido - si è verificato un errore di connessione durante il tentativo di routing della richiesta.

* **Registro dell'applicazione:** ErrorCode = l'applicazione "MACHINE/WEBROOT/APPHOST/MY_APPLICATION" con radice fisica "C:\\PATH}\' Impossibile avviare il processo con la riga di comando""dotnet\my_application.dll", ErrorCode = "0x80004005 : 80008081.

* **Registro di modulo di ASP.NET Core:** Metodo, file o eccezione dell'assembly mancanti. Il metodo, il file o l'assembly specificati nell'eccezione sono un metodo, un file o un'assembly .NET Framework.

Risoluzione dei problemi:

* Installare la versione di .NET Framework mancante dal sistema.

* Per una distribuzione dipendente dal framework, verificare di avere il runtime corretto installato nel sistema. Per esempio, se si aggiorna un progetto da 1.0 a 1.1, distribuire il sistema di hosting e ricevere questa eccezione, assicurarsi di installare il framework 1.1 nel sistema di hosting.

### <a name="stopped-application-pool"></a>Pool di applicazioni arrestato

* **Browser:** 503 Servizio non disponibile

* **Registro dell'applicazione:** Nessuna voce

* **Registro del modulo di ASP.NET Core:** il file di registro non è stato creato

Risoluzione dei problemi

* Confermare che il pool di applicazioni non si trovi nello stato *In arresto*.

### <a name="iis-integration-middleware-not-implemented"></a>Middleware di integrazione di IIS non implementato

* **Browser:** errore HTTP 502.5 - errore del processo

* **Registro dell'applicazione:** l'applicazione "MACHINE/WEBROOT/APPHOST/MY_APPLICATION" con radice fisica "C:\\{PATH}\' ha creato un processo con la riga di comando" "C:\\{PATH}\my_application.{exe|dll}" ma ha danneggiato o non ha risposto o non è stata in ascolto sulla porta specificata "{PORT}", ErrorCode = "0x800705b4"

* **Registro di modulo di ASP.NET Core:** Il file di registro ha creato e mostra il normale funzionamento.

Risoluzione dei problemi

* Confermare che l'applicazione venga eseguita in locale su Kestrel. Un errore del processo può essere il risultato di un problema all'interno dell'applicazione. Per altre informazioni, vedere [Consigli per la risoluzione dei problemi](#troubleshooting-tips).

* Verificare che si sia fatto un corretto riferimento al middleware di integrazione IIS chiamando il metodo *.UseIISIntegration()* nell'applicazione *WebHostBuilder()*.

### <a name="sub-application-includes-a-handlers-section"></a>La sotto-applicazione include una sezione \<gestori\>

* **Browser:** errore HTTP 500.19 - errore del server interno

* **Registro dell'applicazione:** Nessuna voce

* **Registro di modulo di ASP.NET Core:** Il file di registro ha creato e mostra il normale funzionamento per l'applicazione della radice. Il file di registro non è stato creato per la sotto-applicazione.

Risoluzione dei problemi

* Verificare che il file *web.config* della sotto-applicazione non includa una sezione `<handlers>`.

### <a name="application-configuration-general-issue"></a>Problema generale della configurazione dell'applicazione

* **Browser:** errore HTTP 502.5 - errore del processo

* **Registro dell'applicazione:** l'applicazione "MACHINE/WEBROOT/APPHOST/MY_APPLICATION" con radice fisica "C:\\{PATH}\' ha creato un processo con la riga di comando" "C:\\{PATH}\my_application.{exe|dll}" ma ha danneggiato o non ha risposto o non è stata in ascolto sulla porta specificata "{PORT}", ErrorCode = "0x800705b4"

* **Registro del modulo di ASP.NET Core:** il file di registro è stato creato ma è vuoto

Risoluzione dei problemi

* Questa eccezione generale indica che non è stato possibile avviare il processo, probabilmente a causa di un problema di configurazione dell'applicazione. Facendo riferimento alla [Struttura della Directory](xref:hosting/directory-structure), verificare che i file e le cartelle distribuiti dell'applicazione siano appropriati e che i file di configurazione dell'applicazione siano presenti e contengano le impostazioni corrette per l'app e l'ambiente. Per altre informazioni, vedere [Consigli per la risoluzione dei problemi](#troubleshooting-tips).

## <a name="resources"></a>Risorse

* [Introduzione al modulo di ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)

* [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:hosting/aspnet-core-module)

* [Utilizzo dei moduli IIS con ASP.NET Core](xref:hosting/iis-modules)

* [Introduzione a ASP.NET Core](../index.md)

* [Il sito IIS ufficiale di Microsoft](http://www.iis.net/)

* [Libreria di Microsoft TechNet: Windows Server](https://technet.microsoft.com/library/bb625087.aspx)
