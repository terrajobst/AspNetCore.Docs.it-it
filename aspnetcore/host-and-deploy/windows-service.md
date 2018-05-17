---
title: Host ASP.NET Core in un servizio Windows
author: rick-anderson
description: Informazioni su come pubblicare un'applicazione ASP.NET Core in un servizio Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 29f83ee585c73aeb57a09f70ea8e28650c05ce69
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2018
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Host ASP.NET Core in un servizio Windows

[Tom Dykstra](https://github.com/tdykstra)

Il metodo consigliato per ospitare un'applicazione ASP.NET Core in Windows senza utilizza IIS, è possibile eseguirlo in un [servizio Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Quando è ospitato come servizio Windows, l'app è possibile eseguire automaticamente inizio dopo aver riavviato e si blocca senza intervento umano.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample)). Per istruzioni su come eseguire l'app di esempio, vedere l'esempio *README.md* file.

## <a name="prerequisites"></a>Prerequisiti

* L'app deve essere eseguita nel runtime di .NET Framework. Nel *csproj* file, specificare i valori appropriati per [TargetFramework](/nuget/schema/target-frameworks) e [RuntimeIdentifier](/dotnet/articles/core/rid-catalog). Di seguito è riportato un esempio:

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  Quando si crea un progetto in Visual Studio, usare il **ASP.NET Core applicazione (.NET Framework)** modello.

* Se l'applicazione riceve richieste da Internet (non solo da una rete interna), è necessario utilizzare il [HTTP.sys](xref:fundamentals/servers/httpsys) server web (noto in precedenza come [WebListener](xref:fundamentals/servers/weblistener) per le app di ASP.NET Core 1. x) anziché [Kestrel](xref:fundamentals/servers/kestrel). È consigliabile IIS per l'utilizzo come server proxy inverso per le distribuzioni di bordo con Kestrel. Per altre informazioni, vedere [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Quando usare Kestrel con un proxy inverso).

## <a name="get-started"></a>Introduzione

Questa sezione illustra le modifiche minime necessarie per configurare un progetto ASP.NET Core esistente per l'esecuzione in un servizio.

1. Installare il pacchetto NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

2. Apportare le modifiche seguenti in `Program.Main`:

   * Chiamare `host.RunAsService` anziché `host.Run`.

   * Se il codice chiama `UseContentRoot`, utilizzare un percorso per il percorso di pubblicazione anziché `Directory.GetCurrentDirectory()`.

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

3. Pubblicare l'app in una cartella. Utilizzare [dotnet pubblicare](/dotnet/articles/core/tools/dotnet-publish) o [profilo di pubblicazione di Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) che pubblica una cartella.

4. Verificare la creazione e avvio del servizio.

   Aprire una shell dei comandi con privilegi di amministratore per utilizzare il [sc.exe](https://technet.microsoft.com/library/bb490995) strumento da riga di comando per creare e avviare un servizio. Se il servizio è denominato MyService, pubblicata `c:\svc`, e denominata AspNetCoreService, i comandi sono:

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   Il `binPath` valore rappresenta il percorso dell'eseguibile dell'app, che include il nome del file eseguibile.

   ![Finestra della console, creare e avviare l'esempio](windows-service/_static/create-start.png)

   Questi comandi terminato, scegliere il percorso stesso come quando viene eseguito come un'applicazione console (per impostazione predefinita, `http://localhost:5000`):

   ![In esecuzione in un servizio](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a>Fornire un modo per l'esecuzione all'esterno di un servizio

È più semplice testare ed eseguire il debug durante l'esecuzione all'esterno di un servizio, pertanto è facoltativa per aggiungere il codice che chiama `RunAsService` solo in determinate condizioni. Ad esempio, eseguire l'app come un'applicazione console con un `--console` argomento della riga di comando o se il debugger è collegato:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a>Gestire l'arresto e avvio degli eventi

Per gestire `OnStarting`, `OnStarted`, e `OnStopping` degli eventi, apportare le modifiche aggiuntive seguenti:

1. Creare una classe che deriva da `WebHostService`:

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. Creare un metodo di estensione per `IWebHost` che passa l'oggetto personalizzato `WebHostService` a `ServiceBase.Run`:

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. In `Program.Main`, chiamare il nuovo metodo di estensione, `RunAsCustomService`, invece di `RunAsService`:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

Se l'oggetto personalizzato `WebHostService` codice richiede un servizio dall'inserimento di dipendenze (ad esempio un logger), scaricarlo dal `Services` proprietà di `IWebHost`:

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Scenari con server proxy e servizi di bilanciamento del carico

Servizi che interagiscono con le richieste da Internet o da una rete aziendale e si trovano dietro un proxy o il bilanciamento del carico potrebbero richiedere un'ulteriore configurazione. Per altre informazioni, vedere [Configurare ASP.NET Core per l'utilizzo di server proxy e servizi di bilanciamento del carico](xref:host-and-deploy/proxy-load-balancer).

## <a name="acknowledgments"></a>Ringraziamenti

In questo articolo è stato scritto con l'aiuto di origini pubblicate:

* [Hosting ASP.NET Core come servizio Windows](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [Come ospitare il ASP.NET Core in un servizio Windows](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
