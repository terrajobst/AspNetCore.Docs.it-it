---
title: Ospitare ASP.NET Core in un servizio Windows
author: guardrex
description: Informazioni su come ospitare un'app ASP.NET Core in un servizio Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 4aded0b87ca14a5c09844cc378efb1ac0c12a289
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342156"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Ospitare ASP.NET Core in un servizio Windows

Di [Luke Latham](https://github.com/guardrex) e [Tom Dykstra](https://github.com/tdykstra)

È possibile ospitare un'app ASP.NET Core in Windows senza usare IIS come [servizio Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Quando è ospitata come servizio Windows, l'app può essere avviata automaticamente dopo riavvii e arresti anomali senza alcun intervento manuale.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="get-started"></a>Introduzione

Le modifiche minime seguenti sono necessarie per configurare un progetto ASP.NET Core esistente per l'esecuzione in un servizio:

1. Nel file di progetto:

   1. Verificare la presenza dell'identificatore di runtime o aggiungerlo al **\<PropertyGroup >** che contiene il framework di destinazione:

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

   1. Aggiungere un riferimento al pacchetto per [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

1. Modificare `Program.Main` nel modo seguente:

   * Chiamare [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) invece di `host.Run`.

   * Chiamare [UseContentRoot](xref:fundamentals/host/web-host#content-root) e usare un percorso di pubblicazione dell'app invece di `Directory.GetCurrentDirectory()`.

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,12)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. Pubblicare l'app. Usare [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) o un [profilo di pubblicazione di Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles). Quando si usa Visual Studio, selezionare **FolderProfile**.

   Per pubblicare l'app di esempio dalla riga di comando, eseguire il comando seguente in una finestra della console dalla cartella di progetto:

   ```console
   dotnet publish --configuration Release
   ```

1. Usare lo strumento da riga di comando [sc.exe](https://technet.microsoft.com/library/bb490995) per creare il servizio. Il valore `binPath` rappresenta il percorso dell'eseguibile dell'app, che include il nome del file eseguibile. **Lo spazio tra il segno di uguale e le virgolette che indicano l'inizio del percorso è obbligatorio.**

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   Per creare un servizio pubblicato nella cartella del progetto, usare il percorso della cartella *publish*. Nell'esempio seguente:

   * Il progetto si trova nella cartella `c:\my_services\AspNetCoreService`.
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
   
      1. Usare l'opzione [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) sul comando `dotnet publish`. Se si usa Visual Studio, configurare il **Percorso di destinazione** nella pagina delle proprietà di pubblicazione **FolderProfile** prima di selezionare il pulsante **Pubblica**.
   1. Creare il servizio con il comando `sc.exe` usando il percorso della cartella di output. Includere il nome dell'eseguibile del servizio nel percorso fornito a `binPath`.

1. Avviare il servizio con il comando `sc start <SERVICE_NAME>`.

   Per avviare il servizio app di esempio, usare il comando seguente:

   ```console
   sc start MyService
   ```

   L'avvio del servizio richiede alcuni secondi.

1. Il comando `sc query <SERVICE_NAME>` può essere utilizzato per controllare lo stato del servizio:

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

## <a name="provide-a-way-to-run-outside-of-a-service"></a>Fornire un modo per l'esecuzione all'esterno di un servizio

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

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=14)]

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

## <a name="additional-resources"></a>Risorse aggiuntive

* [Configurazione dell'endpoint Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (include la configurazione HTTPS e il supporto SNI)
* <xref:fundamentals/host/web-host>
