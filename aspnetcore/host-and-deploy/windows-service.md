---
title: Ospitare ASP.NET Core in un servizio Windows
author: guardrex
description: Informazioni su come ospitare un'app ASP.NET Core in un servizio Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/25/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: f9b1c3fbfafa839c116688e0ac63804afcd5dbe0
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206673"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Ospitare ASP.NET Core in un servizio Windows

Di [Luke Latham](https://github.com/guardrex) e [Tom Dykstra](https://github.com/tdykstra)

È possibile ospitare un'app ASP.NET Core in Windows senza usare IIS come [servizio Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Quando è ospitata come servizio Windows, l'app viene avviata automaticamente dopo ogni riavvio.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="convert-a-project-into-a-windows-service"></a>Convertire un progetto in un servizio Windows

Sono necessarie le modifiche minime seguenti per configurare un progetto ASP.NET Core esistente da eseguire come servizio:

1. Nel file di progetto:

   * Verificare la presenza di un [identificatore di runtime](/dotnet/core/rid-catalog) di Windows o aggiungerlo al `<PropertyGroup>` che contiene il framework di destinazione:

      ::: moniker range=">= aspnetcore-2.1"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="= aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="< aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      Per eseguire la pubblicazione per più identificatori di runtime:

      * Specificare gli identificatori di runtime in un elenco delimitato da punto e virgola.
      * Usare il nome di proprietà `<RuntimeIdentifiers>` (plurale).

      Per altre informazioni, vedere il [Catalogo RID di .NET Core](/dotnet/core/rid-catalog).

   * Aggiungere un riferimento al pacchetto per [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).

1. Modificare `Program.Main` nel modo seguente:

   * Chiamare [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) invece di `host.Run`.

   * Chiamare [UseContentRoot](xref:fundamentals/host/web-host#content-root) e usare un percorso di pubblicazione dell'app invece di `Directory.GetCurrentDirectory()`.

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. Pubblicare l'app. Usare [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) o un [profilo di pubblicazione di Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles). Quando si usa Visual Studio, selezionare **FolderProfile**.

   Per pubblicare l'app di esempio usando gli strumenti dell'interfaccia della riga di comando, eseguire il comando [dotnet publish](/dotnet/core/tools/dotnet-publish) da un prompt dei comandi nella cartella del progetto. L'identificatore di runtime deve essere specificato nella proprietà `<RuntimeIdenfifier>` (o `<RuntimeIdentifiers>`) del file di progetto. Nell'esempio seguente, l'app viene pubblicata nella configurazione per il rilascio del runtime `win7-x64`:

   ```console
   dotnet publish --configuration Release --runtime win7-x64
   ```

1. Usare lo strumento da riga di comando [sc.exe](https://technet.microsoft.com/library/bb490995) per creare il servizio. Il valore `binPath` rappresenta il percorso dell'eseguibile dell'app, che include il nome del file eseguibile. **Lo spazio tra il segno di uguale e le virgolette che indicano l'inizio del percorso è obbligatorio.**

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   Per creare un servizio pubblicato nella cartella del progetto, usare il percorso della cartella *publish*. Nell'esempio seguente:

   * Il progetto si trova nella cartella *c:\\my_services\\AspNetCoreService*.
   * Il progetto viene pubblicato nella configurazione `Release`.
   * Il moniker framework di destinazione (TFM) è `netcoreapp2.1`.
   * L'identificatore relativo (RID) è `win7-x64`.
   * L'eseguibile dell'app è denominato *AspNetCoreService.exe*.
   * Il servizio è denominato **MyService**.

   Esempio:

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```

   > [!IMPORTANT]
   > Verificare che sia presente lo spazio tra l'argomento `binPath=` e il relativo valore.

   Per pubblicare e avviare il servizio da una cartella diversa:

      * Usare l'opzione [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) sul comando `dotnet publish`. Se si usa Visual Studio, configurare il **Percorso di destinazione** nella pagina delle proprietà di pubblicazione **FolderProfile** prima di selezionare il pulsante **Pubblica**.
      * Creare il servizio con il comando `sc.exe` usando il percorso della cartella di output. Includere il nome dell'eseguibile del servizio nel percorso fornito a `binPath`.

1. Avviare il servizio con il comando `sc start <SERVICE_NAME>`.

   Per avviare il servizio app di esempio, usare il comando seguente:

   ```console
   sc start MyService
   ```

   L'avvio del servizio richiede alcuni secondi.

1. Per verificare lo stato del servizio, usare il comando `sc query <SERVICE_NAME>`. Lo stato viene indicato con uno dei valori seguenti:

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

1. Arrestare il servizio con il comando `sc stop <SERVICE_NAME>`.

   Per arrestare il servizio app di esempio, usare il comando seguente:

   ```console
   sc stop MyService
   ```

1. Dopo il breve lasso di tempo necessario per arrestare il servizio, disinstallare il servizio con il comando `sc delete <SERVICE_NAME>`.

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

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

Poiché la configurazione di ASP.NET Core richiede coppie nome-valore per gli argomenti della riga di comando, l'opzione `--console` viene rimossa prima che gli argomenti vengono passati a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

> [!NOTE]
> `isService` non viene passato da `Main` in `CreateWebHostBuilder` perché la firma di `CreateWebHostBuilder` deve essere `CreateWebHostBuilder(string[])` per garantire il corretto funzionamento dei [test di integrazione](xref:test/integration-tests).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a>Gestire gli eventi di arresto e avvio

Per gestire gli eventi [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) e [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), apportare le modifiche aggiuntive seguenti:

1. Creare una classe che derivi dalla classe [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. Creare un metodo di estensione per [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) che passi l'oggetto `WebHostService` personalizzato a [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. In `Program.Main` chiamare il nuovo metodo di estensione, `RunAsCustomService`, anziché [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > `isService` non viene passato da `Main` in `CreateWebHostBuilder` perché la firma di `CreateWebHostBuilder` deve essere `CreateWebHostBuilder(string[])` per garantire il corretto funzionamento dei [test di integrazione](xref:test/integration-tests).

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

Se il codice `WebHostService` personalizzato richiede un servizio dall'inserimento di dipendenze, ad esempio un logger, ottenerlo dalla proprietà [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Scenari con server proxy e servizi di bilanciamento del carico

I servizi che interagiscono con le richieste da Internet o da una rete aziendale e si trovano dietro un proxy o un servizio di bilanciamento del carico potrebbero richiedere una configurazione aggiuntiva. Per ulteriori informazioni, vedere <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-https"></a>Configurare HTTPS

Specificare una [configurazione dell'endpoint HTTPS del server Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).

## <a name="current-directory-and-content-root"></a>Directory corrente e radice del contenuto

La directory di lavoro corrente restituita chiamando `Directory.GetCurrentDirectory()` per un servizio Windows è la cartella *C:\\WINDOWS\\system32*. La cartella *system32* non è un percorso appropriato per archiviare i file di un servizio, ad esempio i file di impostazioni. Usare uno degli approcci seguenti per gestire e accedere alle risorse e ai file di impostazioni di un servizio con [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) quando si usa un'interfaccia [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):

* Usare il percorso radice del contenuto. L'elemento `IHostingEnvironment.ContentRootPath` è lo stesso percorso fornito all'argomento `binPath` durante la creazione del servizio. Invece di usare `Directory.GetCurrentDirectory()` per creare i percorsi dei file di impostazioni, usare il percorso radice del contenuto e mantenere i file nella radice del contenuto dell'app.
* Archiviare i file in un percorso appropriato nel disco. Specificare un percorso assoluto con `SetBasePath` alla cartella contenente i file.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Configurazione dell'endpoint Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (include la configurazione HTTPS e il supporto SNI)
* <xref:fundamentals/host/web-host>
