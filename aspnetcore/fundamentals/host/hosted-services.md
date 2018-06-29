---
title: Attività in background con servizi ospitati in ASP.NET Core
author: guardrex
description: Informazioni su come implementare attività in background con servizi ospitati in ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
uid: fundamentals/host/hosted-services
ms.openlocfilehash: e5455e553cba817dce811391d4a909e501a20d9a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273781"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>Attività in background con servizi ospitati in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

In ASP.NET Core le attività in background possono essere implementate come *servizi ospitati*. Un servizio ospitato è una classe con logica di attività in background che implementa l'interfaccia [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice). In questo argomento vengono forniti tre esempi di servizio ospitato:

* Attività in background eseguita su un timer.
* Servizio ospitato che attiva un servizio con ambito. Il servizio con ambito può utilizzare l'inserimento di dipendenze.
* Attività in background in coda che vengono eseguite in sequenza.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.1"

L'app di esempio è disponibile in due versioni:

* Host Web &ndash; L'host Web è utile per l'hosting di app Web. Il codice di esempio in questo argomento è tratto dalla versione host Web dell'esempio. Per altre informazioni, vedere l'argomento [Host Web](xref:fundamentals/host/web-host).
* Host generico &ndash; L'host generico è una novità di ASP.NET Core 2.1. Per altre informazioni, vedere l'argomento [Host generico](xref:fundamentals/host/generic-host).

::: moniker-end

## <a name="ihostedservice-interface"></a>Interfaccia IHostedService

I servizi ospitati implementano l'interfaccia [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice). L'interfaccia definisce due metodi riservati agli oggetti gestiti dall'host:

* [StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync): metodo chiamato dopo l'avvio del server e l'attivazione di [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted). `StartAsync` contiene la logica per avviare l'attività in background.

* [StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync): metodo attivato quando l'host sta eseguendo un arresto normale. `StopAsync` contiene la logica per porre fine all'attività in background ed eliminare le eventuali risorse non gestite. Se l'app si arresta in modo imprevisto, ad esempio, il processo dell'app ha esito negativo, il metodo `StopAsync` potrebbe non essere chiamato.

Il servizio ospitato viene attivato una volta all'avvio dell'app e all'arresto normale al momento della chiusura dell'applicazione. Quando [IDisposable](/dotnet/api/system.idisposable) viene implementato, le risorse possono essere eliminate quando viene eliminato il contenitore dei servizi. Se viene generato un errore durante l'esecuzione dell'attività in background, deve essere chiamato `Dispose` anche se `StopAsync` non viene chiamato.

## <a name="timed-background-tasks"></a>Attività in background programmate

Un'attività programmata in background utilizza la classe [System.Threading.Timer](/dotnet/api/system.threading.timer). Il timer attiva il metodo `DoWork` dell'attività. Il timer viene disabilitato con `StopAsync` ed eliminato quando il contenitore dei servizi è eliminato con `Dispose`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

::: moniker range=">= aspnetcore-2.1"

Il servizio registrato in `Startup.ConfigureServices` con il metodo di estensione `AddHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Il servizio è registrato in `Startup.ConfigureServices`:

[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Utilizzo di un servizio con ambito in un'attività in background

Per utilizzare i servizi con ambito all'interno di un `IHostedService`, creare un ambito. Non viene creato automaticamente alcun ambito per un servizio ospitato.

Il servizio dell'attività in background con ambito contiene la logica dell'attività in background. Nell'esempio seguente [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) viene inserito nel servizio:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

Il servizio ospitato crea un ambito per risolvere il servizio dell'attività in background con ambito e chiamare il relativo metodo `DoWork`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

::: moniker range=">= aspnetcore-2.1"

I servizi vengono registrati in `Startup.ConfigureServices`. L'implementazione di `IHostedService`viene registrata con il metodo di estensione `AddHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

I servizi sono registrati in `Startup.ConfigureServices`:

[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

::: moniker-end

## <a name="queued-background-tasks"></a>Attività in background in coda

Una coda di attività in background si basa su .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([provvisoriamente pianificato per essere incorporato in ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

In `QueueHostedService` le attività in background (`workItem`) nella coda vengono rimosse dalla coda ed eseguite:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

::: moniker range=">= aspnetcore-2.1"

I servizi vengono registrati in `Startup.ConfigureServices`. L'implementazione di `IHostedService`viene registrata con il metodo di estensione `AddHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

I servizi sono registrati in `Startup.ConfigureServices`:

[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

::: moniker-end

Nella classe modello della pagina di indice `IBackgroundTaskQueue` viene inserito nel costruttore e assegnato a `Queue`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

Quando si seleziona il pulsante **Aggiungi attività** nella pagina di indice, viene eseguito il metodo `OnPostAddTask`. `QueueBackgroundWorkItem` viene chiamato per accodare l'elemento di lavoro:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a>Risorse aggiuntive

* [Implementare attività in background in microservizi con IHostedService e la classe BackgroundService](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [System.Threading.Timer](/dotnet/api/system.threading.timer)
