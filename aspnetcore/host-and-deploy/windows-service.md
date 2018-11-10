---
title: Ospitare ASP.NET Core in un servizio Windows
author: guardrex
description: Informazioni su come ospitare un'app ASP.NET Core in un servizio Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/30/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 11913019bfe5d06c259b806fce9cc580a8280ad5
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758193"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Ospitare ASP.NET Core in un servizio Windows

Di [Luke Latham](https://github.com/guardrex) e [Tom Dykstra](https://github.com/tdykstra)

È possibile ospitare un'app ASP.NET Core in Windows senza usare IIS come [servizio Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Quando è ospitata come servizio Windows, l'app viene avviata automaticamente dopo ogni riavvio.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="convert-a-project-into-a-windows-service"></a>Convertire un progetto in un servizio Windows

Sono necessarie le modifiche minime seguenti per configurare un progetto ASP.NET Core esistente da eseguire come servizio:

1. Nel file di progetto:

   * Verificare la presenza di un [identificatore di runtime](/dotnet/core/rid-catalog) di Windows o aggiungerlo al `<PropertyGroup>` che contiene il framework di destinazione:

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.2</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      Per eseguire la pubblicazione per più identificatori di runtime:

      * Specificare gli identificatori di runtime in un elenco delimitato da punto e virgola.
      * Usare il nome di proprietà `<RuntimeIdentifiers>` (plurale).

      Per altre informazioni, vedere il [Catalogo RID di .NET Core](/dotnet/core/rid-catalog).

   * Aggiungere un riferimento al pacchetto per [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).

1. Modificare `Program.Main` nel modo seguente:

   * Chiamare [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) invece di `host.Run`.

   * Chiamare [UseContentRoot](xref:fundamentals/host/web-host#content-root) e usare un percorso di pubblicazione dell'app invece di `Directory.GetCurrentDirectory()`.

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

1. Pubblicare l'app usando [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), un [profilo di pubblicazione di Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) o Visual Studio Code. Quando si usa Visual Studio, selezionare **FolderProfile** e configurare il **Percorso di destinazione** prima di selezionare il pulsante **Pubblica**.

   Per pubblicare l'app di esempio usando gli strumenti dell'interfaccia della riga di comando, eseguire il comando [dotnet publish](/dotnet/core/tools/dotnet-publish) da un prompt dei comandi nella cartella del progetto. L'identificatore di runtime deve essere specificato nella proprietà `<RuntimeIdenfifier>` (o `<RuntimeIdentifiers>`) del file di progetto. Nell'esempio seguente, l'app viene pubblicata nella configurazione per il rilascio del runtime `win7-x64` in una cartella creata in *c:\\svc*:

   ```console
   dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
   ```

1. Creare un account utente per il servizio con il comando `net user`:

   ```console
   net user {USER ACCOUNT} {PASSWORD} /add
   ```

   Per l'app di esempio, creare un account utente con il nome `ServiceUser` e una password. Nel comando seguente sostituire `{PASSWORD}` con una [password complessa](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).

   ```console
   net user ServiceUser {PASSWORD} /add
   ```

   Se è necessario aggiungere l'utente a un gruppo, usare il comando `net localgroup`, dove `{GROUP}` è il nome del gruppo:

   ```console
   net localgroup {GROUP} {USER ACCOUNT} /add
   ```

   Per altre informazioni, vedere [Service User Accounts](/windows/desktop/services/service-user-accounts) (Account utente di servizio).

1. Concedere l'accesso per scrittura/lettura/esecuzione alla cartella dell'app usando il comando [icacls](/windows-server/administration/windows-commands/icacls):

   ```console
   icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
   ```

   * `{PATH}` &ndash; Percorso della cartella dell'app.
   * `{USER ACCOUNT}` &ndash; Account utente (SID).
   * `(OI)` &ndash; Il flag Eredità oggetto propaga le autorizzazioni ai file subordinati.
   * `(CI)` &ndash; Il flag Eredità contenitore propaga le autorizzazioni alle cartelle subordinate.
   * `{PERMISSION FLAGS}` &ndash; Imposta le autorizzazioni di accesso dell'app.
     * Scrittura (`W`)
     * Lettura (`R`)
     * Esecuzione (`X`)
     * Completo (`F`)
     * Modifica (`M`)
   * `/t` &ndash; Applicare in modo ricorsivo alle cartelle e ai file subordinati esistenti.

   Per l'app di esempio pubblicata nella cartella *c:\\svc* e l'account `ServiceUser` con autorizzazioni di scrittura/lettura/esecuzione, usare il comando seguente:

   ```console
   icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
   ```

   Per altre informazioni, vedere [icacls](/windows-server/administration/windows-commands/icacls).

1. Usare lo strumento da riga di comando [sc.exe](https://technet.microsoft.com/library/bb490995) per creare il servizio. Il valore `binPath` rappresenta il percorso dell'eseguibile dell'app, che include il nome del file eseguibile. **Lo spazio tra il segno di uguale e il carattere di virgoletta di ogni parametro e valore è obbligatorio.**

   ```console
   sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
   ```

   * `{SERVICE NAME}` &ndash; Nome da assegnare al servizio in [Gestione controllo servizi](/windows/desktop/services/service-control-manager).
   * `{PATH}` &ndash; Percorso dell'eseguibile del servizio.
   * `{DOMAIN}` (o se il computer non è aggiunto a un dominio, il nome del computer locale) e `{USER ACCOUNT}` &ndash; il dominio (o nome del computer locale) e account utente con cui viene eseguito il servizio. **Non** omettere il parametro `obj`. Il valore predefinito per `obj` è l'account [LocalSystem](/windows/desktop/services/localsystem-account). L'esecuzione di un servizio nel contesto dell'account `LocalSystem` presenta rischi significativi per la sicurezza. Eseguire sempre un servizio con un account utente con privilegi limitati nel server.
   * `{PASSWORD}` &ndash; Password dell'account utente.

   Nell'esempio seguente:

   * Il servizio è denominato **MyService**.
   * Il servizio pubblicato risiede nella cartella *c:\\svc*. L'eseguibile dell'app è denominato *AspNetCoreService.exe*. Il valore `binPath` è racchiuso tra virgolette semplici (").
   * Il servizio viene eseguito nel contesto dell'account `ServiceUser`. Sostituire `{DOMAIN}` con il dominio dell'account utente o il nome del computer locale. Racchiudere il valore `obj` tra virgolette semplici ("). Esempio: se il sistema host è un computer locale denominato `MairaPC`, impostare `obj` su `"MairaPC\ServiceUser"`.
   * Sostituire `{PASSWORD}` con la password dell'account utente. Il valore `password` è racchiuso tra virgolette semplici (").

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
   ```

   > [!IMPORTANT]
   > Assicurarsi che siano presenti gli spazi tra i segni di uguale e i valori dei parametri.

1. Avviare il servizio con il comando `sc start {SERVICE NAME}`.

   Per avviare il servizio app di esempio, usare il comando seguente:

   ```console
   sc start MyService
   ```

   L'avvio del servizio richiede alcuni secondi.

1. Per verificare lo stato del servizio, usare il comando `sc query {SERVICE NAME}`. Lo stato viene indicato con uno dei valori seguenti:

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   Usare il comando seguente per verificare lo stato del servizio app di esempio:

   ```console
   sc query MyService
   ```

1. Quando il servizio è nello stato `RUNNING` e se il servizio è un'app Web, selezionare l'app dal suo percorso (per impostazione predefinita, `http://localhost:5000`, che reindirizza a `https://localhost:5001` quando si utilizza [il middleware di reindirizzamento HTTPS](xref:security/enforcing-ssl)).

   Per il servizio app di esempio, selezionare l'app in `http://localhost:5000`.

1. Arrestare il servizio con il comando `sc stop {SERVICE NAME}`.

   Per arrestare il servizio app di esempio, usare il comando seguente:

   ```console
   sc stop MyService
   ```

1. Dopo il breve lasso di tempo necessario per arrestare il servizio, disinstallare il servizio con il comando `sc delete {SERVICE NAME}`.

   Verificare lo stato del servizio app di esempio:

   ```console
   sc query MyService
   ```

   Quando il servizio app di esempio è nello stato `STOPPED`, usare il comando seguente per disinstallare il servizio app di esempio:

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a>Eseguire l'app fuori da un servizio

È più semplice testare ed eseguire il debug durante l'esecuzione all'esterno di un servizio, pertanto può essere utile aggiungere codice che chiama `RunAsService` solo in determinate condizioni. Ad esempio, l'app può essere eseguita come un'app console con un argomento della riga di comando `--console` o se il debugger è collegato:

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

Poiché la configurazione di ASP.NET Core richiede coppie nome-valore per gli argomenti della riga di comando, l'opzione `--console` viene rimossa prima che gli argomenti vengono passati a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

> [!NOTE]
> `isService` non viene passato da `Main` in `CreateWebHostBuilder` perché la firma di `CreateWebHostBuilder` deve essere `CreateWebHostBuilder(string[])` per garantire il corretto funzionamento dei [test di integrazione](xref:test/integration-tests).

## <a name="handle-stopping-and-starting-events"></a>Gestire gli eventi di arresto e avvio

Per gestire gli eventi [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) e [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), apportare le modifiche aggiuntive seguenti:

1. Creare una classe che derivi dalla classe [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. Creare un metodo di estensione per [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) che passi l'oggetto `WebHostService` personalizzato a [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. In `Program.Main` chiamare il nuovo metodo di estensione, `RunAsCustomService`, anziché [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > `isService` non viene passato da `Main` in `CreateWebHostBuilder` perché la firma di `CreateWebHostBuilder` deve essere `CreateWebHostBuilder(string[])` per garantire il corretto funzionamento dei [test di integrazione](xref:test/integration-tests).

Se il codice `WebHostService` personalizzato richiede un servizio dall'inserimento di dipendenze, ad esempio un logger, ottenerlo dalla proprietà [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Scenari con server proxy e servizi di bilanciamento del carico

I servizi che interagiscono con le richieste da Internet o da una rete aziendale e si trovano dietro un proxy o un servizio di bilanciamento del carico potrebbero richiedere una configurazione aggiuntiva. Per ulteriori informazioni, vedere <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-https"></a>Configurare HTTPS

Per configurare il servizio con un endpoint sicuro:

1. Creare un certificato X.509 per il sistema di hosting usando i meccanismi di acquisizione e distribuzione dei certificati della piattaforma.
1. Specificare una [configurazione dell'endpoint HTTPS del server Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) per usare il certificato.

L'uso del certificato di sviluppo ASP.NET Core HTTPS per proteggere un endpoint del servizio non è supportato.

## <a name="current-directory-and-content-root"></a>Directory corrente e radice del contenuto

La directory di lavoro corrente restituita chiamando `Directory.GetCurrentDirectory()` per un servizio Windows è la cartella *C:\\WINDOWS\\system32*. La cartella *system32* non è un percorso appropriato per archiviare i file di un servizio, ad esempio i file di impostazioni. Usare uno degli approcci seguenti per gestire e accedere alle risorse e ai file di impostazioni di un servizio con [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) quando si usa un'interfaccia [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):

* Usare il percorso radice del contenuto. L'elemento `IHostingEnvironment.ContentRootPath` è lo stesso percorso fornito all'argomento `binPath` durante la creazione del servizio. Invece di usare `Directory.GetCurrentDirectory()` per creare i percorsi dei file di impostazioni, usare il percorso radice del contenuto e mantenere i file nella radice del contenuto dell'app.
* Archiviare i file in un percorso appropriato nel disco. Specificare un percorso assoluto con `SetBasePath` alla cartella contenente i file.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Configurazione dell'endpoint Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (include la configurazione HTTPS e il supporto SNI)
* <xref:fundamentals/host/web-host>
