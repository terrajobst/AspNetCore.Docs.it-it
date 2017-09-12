---
title: Host in un servizio Windows
author: tdykstra
description: Informazioni su come pubblicare un'applicazione ASP.NET Core in un servizio Windows.
keywords: Hosting ASP.NET Core, il servizio di Windows,
ms.author: tdykstra
manager: wpickett
ms.date: 03/30/2017
ms.topic: article
ms.assetid: d9a65066-d7cb-47df-b046-64629c4d2c6f
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/windows-service
ms.openlocfilehash: 33a4eca48a04f9b29c60a446f4191d39d21e7e7d
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a>Ospitare un'applicazione ASP.NET Core in un servizio Windows

Da [Tom Dykstra](https://github.com/tdykstra)

È il modo consigliato per ospitare un'applicazione ASP.NET Core in Windows, quando non si utilizza IIS per l'esecuzione in un [servizio Windows](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications). In questo modo è possibile avviare automaticamente dopo il riavvio di arresti anomali del sistema, senza attendere che un utente per l'accesso.

[Consente di visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) vedere il [passaggi successivi](#next-steps) per istruzioni su come eseguirlo.

## <a name="prerequisites"></a>Prerequisiti

* L'app deve essere eseguita nel runtime di .NET framework.  Nel *csproj* file, specificare i valori appropriati per [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) e [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog). Di seguito è riportato un esempio:

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  Quando si crea un progetto in Visual Studio, usare il **ASP.NET Core applicazione (.NET Framework)** modello.

* Se l'applicazione riceve richieste da internet (non solo da una rete interna), è necessario utilizzare il [WebListener](xref:fundamentals/servers/weblistener) server web anziché [Kestrel](xref:fundamentals/servers/kestrel).  Kestrel deve essere utilizzata con IIS per le distribuzioni di bordo.  Per ulteriori informazioni, vedere [quando utilizzare Kestrel con un proxy inverso](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

## <a name="getting-started"></a>Per iniziare

Questa sezione illustra le modifiche minime necessarie per configurare un progetto ASP.NET Core esistente per l'esecuzione in un servizio.

* Installare il pacchetto NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

* Apportare le modifiche seguenti in `Program.Main`:
  
  * Chiamare `host.RunAsService` anziché `host.Run`.
  
  * Se il codice chiama `UseContentRoot`, utilizzare un percorso per il percorso di pubblicazione invece di`Directory.GetCurrentDirectory()` 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* Pubblicare l'applicazione in una cartella.

  Utilizzare [dotnet pubblicare](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) o [profilo di pubblicazione di Visual Studio](xref:publishing/web-publishing-vs) che pubblica una cartella.

* Verificare la creazione e avvio del servizio.

  Aprire una finestra del prompt dei comandi di amministratore da utilizzare il [sc.exe](https://technet.microsoft.com/library/bb490995) strumento da riga di comando per creare e avviare un servizio.  
  
  Il nome del servizio MyService, pubblicare l'app per `c:\svc`e l'app stessa denominato AspNetCoreService, i comandi sono analogo al seguente:

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```
  Il `binPath` valore è il percorso file eseguibile dell'applicazione, inclusi il nome del file eseguibile stesso.

  ![Finestra della console, creare e avviare l'esempio](windows-service/_static/create-start.png)

  Questi comandi terminato, è possibile esplorare per lo stesso percorso quando si esegue come un'applicazione console (per impostazione predefinita, `http://localhost:5000`)

  ![In esecuzione in un servizio](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a>Fornire un modo per l'esecuzione all'esterno di un servizio

È più semplice testare ed eseguire il debug durante l'esecuzione all'esterno di un servizio, pertanto è facoltativa per aggiungere il codice che chiama `host.RunAsService` solo in determinate condizioni.  Ad esempio, è possibile eseguire come un'applicazione console se viene visualizzato un `--console` argomento della riga di comando o se il debugger è collegato.

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a>Gestire l'arresto e avvio degli eventi

Se si desidera gestire `OnStarting`, `OnStarted`, e `OnStopping` degli eventi, apportare le modifiche aggiuntive seguenti:

* Creare una classe che deriva da `WebHostService`.

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* Creare un metodo di estensione per `IWebHost` che passa personalizzato `WebHostService` a `ServiceBase.Run`.

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* In `Program.Main` modifica chiami il nuovo metodo di estensione anziché `host.RunAsService`.

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

Se personalizzato `WebHostService` codice deve ottenere un servizio dall'inserimento di dipendenze (ad esempio un logger), è possibile ottenere dal `Services` proprietà di `IWebHost`.

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a>Passaggi successivi

Il [applicazione di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) che accompagna questo articolo è un'applicazione web MVC semplice che è stata modificata come illustrato negli esempi di codice precedente.  Per eseguirlo in un servizio, effettuare i passaggi seguenti:

* Pubblicare *c:\svc*.

* Aprire una finestra di amministratore.

* Immettere i comandi seguenti:

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * In un browser, passare alla indirizzo http://localhost:5000/ per verificare che sia in esecuzione.

Se l'app non viene avviato fino come previsto durante l'esecuzione in un servizio, un modo rapido per rendere accessibili i messaggi di errore consiste nell'aggiungere un provider di log, ad esempio il [provider registro eventi di Windows](xref:fundamentals/logging#eventlog).

## <a name="acknowledgments"></a>Riconoscimenti

In questo articolo è stato scritto con l'aiuto di origini che sono stati già pubblicate. Il primo e più utili di essi sono questi stati:

* [Hosting ASP.NET Core come servizio Windows](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [Come ospitare il ASP.NET Core in un servizio Windows](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
