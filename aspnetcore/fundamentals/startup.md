---
title: Avvio dell'applicazione in ASP.NET Core
author: ardalis
description: Illustra la classe di avvio in ASP.NET Core.
keywords: ASP.NET Core, avvio, il metodo di configurazione, ConfigureServices (metodo)
ms.author: tdykstra
manager: wpickett
ms.date: 02/29/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: 69af91de6d2c48af58bc10a32d8857af18a41b6a
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="application-startup-in-aspnet-core"></a>Avvio dell'applicazione in ASP.NET Core

Da [Steve Smith](https://ardalis.com/) e [Tom Dykstra](https://github.com/tdykstra/)

La `Startup` classe consente di configurare servizi e delle pipeline delle richieste dell'applicazione. 

## <a name="the-startup-class"></a>La classe di avvio

Le applicazioni ASP.NET Core richiedono un `Startup` classe. Per convenzione, il `Startup` classe è denominata "Avvio". Specificare il nome di classe di avvio nel `Main` programma [WebHostBuilderExtensions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions) [ `UseStartup<TStartup>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) metodo. Vedere [Hosting](xref:fundamentals/hosting) per altre informazioni, vedere `WebHostBuilder`, che viene eseguito prima `Startup`.

È possibile definire separato `Startup` classi per diversi ambienti e verrà selezionato uno in fase di esecuzione appropriato. Se si specifica `startupAssembly` nel [configurazione WebHost](https://docs.microsoft.com/aspnet/core/fundamentals/hosting?tabs=aspnetcore2x#configuring-a-host) o opzioni di hosting verranno caricare l'assembly di avvio, cercare un `Startup` o `Startup[Environment]` tipo. La classe il cui corrispondenze suffisso di nome sarà valutato l'ambiente corrente, pertanto se si esegue l'app nel *sviluppo* ambiente e include sia un `Startup` e un `StartupDevelopment` (classe), la `StartupDevelopment` classe sarà utilizzato. Vedere [FindStartupType](https://github.com/aspnet/Hosting/blob/rel/1.1.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs) in `StartupLoader` e [utilizzo di più ambienti](environments.md#startup-conventions).

In alternativa, è possibile definire fisse `Startup` classe che verrà utilizzato indipendentemente dall'ambiente chiamando `UseStartup<TStartup>`. Si tratta dell'approccio consigliato.

Il `Startup` costruttore della classe può accettare le dipendenze che vengono fornite tramite [inserimento di dipendenze](xref:fundamentals/dependency-injection). Un approccio comune consiste nell'utilizzare `IHostingEnvironment` impostare [configurazione](xref:fundamentals/configuration) origini.

Il `Startup` classe deve includere un `Configure` metodo e può includere facoltativamente un `ConfigureServices` (metodo), che vengono chiamati quando l'avvio dell'applicazione. La classe può anche includere [versioni specifiche dell'ambiente di questi metodi](xref:fundamentals/environments#startup-conventions). `ConfigureServices`, se presente, viene chiamato prima `Configure`.

Informazioni su [la gestione delle eccezioni durante l'avvio dell'applicazione](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Il metodo ConfigureServices

Il [ConfigureServices](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.startupbase#Microsoft_AspNetCore_Hosting_StartupBase_ConfigureServices_Microsoft_Extensions_DependencyInjection_IServiceCollection_) è facoltativo; ma se utilizzato, viene chiamato prima di `Configure` metodo dall'host web. L'host web può configurare alcuni servizi prima ``Startup`` metodi vengono chiamati (vedere [hosting](xref:fundamentals/hosting)). Per convenzione, [opzioni di configurazione](xref:fundamentals/configuration) sono impostate in questo metodo.

Per le funzionalità che richiedono l'installazione sostanziale esistono `Add[Service]` metodi di estensione in [IServiceCollection](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.iservicecollection). In questo esempio il modello di sito web predefinito configurato l'applicazione per utilizzare i servizi per Entity Framework, l'identità e MVC:

[!code-csharp[Principale](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

Aggiunta di servizi per il contenitore dei servizi sono disponibili all'interno dell'applicazione tramite [inserimento di dipendenze](xref:fundamentals/dependency-injection).

## <a name="services-available-in-startup"></a>Servizi disponibili nella finestra di avvio

Inserimento di dipendenze di ASP.NET Core fornisce servizi durante l'avvio di un'applicazione. È possibile richiedere questi servizi, includendo l'interfaccia appropriata come parametro nel `Startup` costruttore della classe o il relativo `Configure` metodo. Il `ConfigureServices` metodo accetta solo un `IServiceCollection` parametro (ma può essere qualsiasi servizio registrato recuperare dalla raccolta, pertanto i parametri aggiuntivi non sono necessari).

Di seguito sono riportati alcuni dei servizi in genere richiesti da `Startup` metodi:

* Nel costruttore: `IHostingEnvironment`,`ILogger<Startup>`
* In `ConfigureServices`:`IServiceCollection`
* In `Configure`: `IApplicationBuilder`, `IHostingEnvironment`,`ILoggerFactory`

Tutti i servizi aggiunti dal ``WebHostBuilder`` ``ConfigureServices`` metodo può essere richiesto dal ``Startup`` costruttore della classe o il relativo ``Configure`` metodo. Utilizzare `WebHostBuilder` per fornire qualsiasi servizio, è necessario durante `Startup` metodi.

## <a name="the-configure-method"></a>Metodo Configure

Il `Configure` consente di specificare la modalità con cui l'applicazione ASP.NET di risposta alle richieste HTTP. La pipeline delle richieste è configurata tramite l'aggiunta di [middleware](middleware.md) componenti da un `IApplicationBuilder` istanza fornito dall'inserimento di dipendenze.

Nell'esempio seguente il modello di sito web predefinito, i diversi metodi di estensione vengono utilizzati per configurare la pipeline con il supporto per [BrowserLink](http://vswebessentials.com/features/browserlink), pagine di errore, file statici, ASP.NET MVC e identità.

[!code-csharp[Principale](../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

Ogni `Use` metodo di estensione aggiunge un [middleware](xref:fundamentals/middleware) componente alla pipeline delle richieste. Ad esempio, il `UseMvc` metodo di estensione aggiunge la [routing](routing.md) middleware alla pipeline delle richieste e configura [MVC](xref:mvc/overview) come gestore predefinito.

Per ulteriori informazioni su come usare `IApplicationBuilder`, vedere [Middleware](xref:fundamentals/middleware).

Servizi aggiuntivi, ad esempio `IHostingEnvironment` e `ILoggerFactory` può anche essere specificato nella firma del metodo, nel qual caso questi servizi sono [inserito](dependency-injection.md) se sono disponibili. 

## <a name="additional-resources"></a>Risorse aggiuntive

* [Utilizzo di più ambienti](xref:fundamentals/environments)
* [Middleware](xref:fundamentals/middleware)
* [Registrazione](xref:fundamentals/logging)
* [Configurazione](xref:fundamentals/configuration)
