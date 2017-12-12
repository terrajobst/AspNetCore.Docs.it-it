---
title: Middleware ASP.NET Core
author: rick-anderson
description: Informazioni su ASP.NET Core middleware e la pipeline della richiesta.
keywords: ASP.NET Core, Middleware, pipeline, delegato
ms.author: riande
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.assetid: db9a86ab-46c2-40e0-baed-86e38c16af1f
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware
ms.openlocfilehash: ad8d207b1e6de396f16d098fb07ddc89bea2c520
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-core-middleware-fundamentals"></a>Nozioni fondamentali di Middleware di ASP.NET Core

<a name="fundamentals-middleware"></a>

Da [Rick Anderson](https://twitter.com/RickAndMSFT) e [Steve Smith](https://ardalis.com/)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-middleware"></a>Che cos'è middleware

Middleware è un software che viene assemblata in una pipeline dell'applicazione per gestire le richieste e risposte. Ogni componente:

* Sceglie se passare la richiesta al componente successivo nella pipeline.
* Puoi eseguire lavoro prima e dopo aver richiamato il componente successivo nella pipeline. 

Richiesta delegati vengono utilizzati per compilare la pipeline della richiesta. I delegati di richiesta di gestire ciascuna richiesta HTTP.

Richiedere i delegati vengono configurati utilizzando [eseguire](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [mappa](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), e [utilizzare](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) metodi di estensione. Un delegato di singola richiesta può essere specificato nella riga come un metodo anonimo (denominato middleware in linea) o può essere definito in una classe riutilizzabile. Queste classi riutilizzabili e i metodi anonimi in linea sono *middleware*, o *componenti middleware*. Ogni componente del middleware nella pipeline delle richieste è responsabile per richiamare il componente successivo nella pipeline o la catena di corto circuito se appropriato.

[Moduli HTTP con la migrazione al Middleware](../migration/http-modules.md) viene illustrata la differenza tra le versioni precedente e pipeline di richiesta in ASP.NET Core e fornisce altri esempi di middleware.

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a>Creazione di una pipeline middleware con IApplicationBuilder

La pipeline delle richieste ASP.NET Core è costituito da una sequenza di delegati di richiesta, chiamata uno dopo l'altro, perché questo diagramma mostra (il thread di esecuzione segue frecce nere):

![Modello di elaborazione di richiesta che mostra una richiesta in arrivo, l'elaborazione tramite tre middlewares e la risposta e l'applicazione. Ogni middleware esegue la logica e passa la richiesta al middleware successivo in corrispondenza dell'istruzione Next. Dopo il terzo middleware elabora la richiesta, è la mano attraverso il due middlewares precedenti per un'elaborazione aggiuntiva dopo istruzioni Next a sua volta prima di chiudere l'applicazione in risposta al client.](middleware/_static/request-delegate-pipeline.png)

Delegati possono eseguire operazioni prima e dopo che il delegato successivo. Un delegato può anche decidere di non passare una richiesta per il delegato successivo, che viene chiamato la pipeline di richieste di corto circuito. Corto circuito è spesso consigliabile in quanto evita di lavoro non necessario. Ad esempio, il middleware di file statici può restituire una richiesta di un file statico e il resto della pipeline di corto circuito. Gestione delle eccezioni delegati devono essere chiamato nelle prime fasi della pipeline, in modo che può intercettare le eccezioni generate in fasi successive della pipeline.

L'app di ASP.NET Core più semplice possibile imposta un delegato singola richiesta che gestisce tutte le richieste. In questo caso non include una pipeline della richiesta effettiva. Al contrario, una singola funzione anonima viene chiamata in risposta a ogni richiesta HTTP.

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

Il primo [app. Eseguire](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegato termina la pipeline.

È possibile concatenare più delegati richiesta insieme a [app. Utilizzare](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions). Il `next` parametro rappresenta il delegato successivo nella pipeline. (Tenere presente che è possibile corto circuito di pipeline da *non* la chiamata di *Avanti* parametro.) In genere è possibile eseguire azioni prima e dopo che il delegato successivo, come illustrato in questo esempio:

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> Non chiamare `next.Invoke` dopo aver inviata la risposta al client. Passa a `HttpResponse` dopo la risposta è stato avviato, verrà generata un'eccezione. Ad esempio, le modifiche, ad esempio l'impostazione di intestazioni, codice di stato e così via, genererà un'eccezione. Scrivere il corpo della risposta dopo la chiamata `next`:
> - Può causare una violazione del protocollo. Ad esempio, la scrittura di più le `content-length`.
> - Potrebbe danneggiare il formato del corpo. Ad esempio, la scrittura di un piè di pagina HTML in un file CSS.
>
> [HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) è un suggerimento utile per indicare se le intestazioni sono state inviate e/o ha scritto il corpo.

## <a name="ordering"></a>Ordinazioni

L'ordine in cui vengono aggiunti i componenti middleware di `Configure` metodo definisce l'ordine in cui vengono richiamati per le richieste e l'ordine inverso per la risposta. Questo ordinamento è fondamentale per la sicurezza, prestazioni e funzionalità.

Il metodo Configure (mostrato sotto) consente di aggiungere i componenti middleware seguenti:

1. Gestione degli errori di eccezione /
2. Server di file statici
3. Autenticazione
4. MVC

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

Nel codice precedente, `UseExceptionHandler` è il primo componente middleware aggiunto alla pipeline, pertanto, consente di rilevare le eccezioni che si verificano nelle chiamate successive.

Il middleware del file statico viene chiamato nelle prime fasi della pipeline, pertanto può gestire le richieste e di corto circuito senza passare attraverso i componenti rimanenti. Fornisce il middleware di file statici **non** controlli di autorizzazione. Qualsiasi file forniti, tra cui quelle contenute *wwwroot*, sono disponibili pubblicamente. Vedere [utilizzo di file statici](xref:fundamentals/static-files) per un approccio proteggere i file statici.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)


Se la richiesta non è gestita dal middleware del file statico, viene passato al middleware di identità (`app.UseAuthentication`), che esegue l'autenticazione. Identità non di corto circuito le richieste non autenticate. Anche se le richieste vengono autenticate identità, autorizzazione (e rifiuto) si verifica solo dopo aver MVC consente di selezionare una pagina Razor o controller e un'azione specifica.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Se la richiesta non è gestita dal middleware del file statico, viene passato al middleware di identità (`app.UseIdentity`), che esegue l'autenticazione. Identità non di corto circuito le richieste non autenticate. Anche se le richieste vengono autenticate identità, autorizzazione (e rifiuto) si verifica solo dopo aver MVC consente di selezionare un controller specifico e l'azione.

-----------

Nell'esempio seguente viene illustrato un tipo di middleware ordine in cui le richieste di file statici vengono gestite dal middleware del file statico prima il middleware di compressione di risposta. File statici non vengono compressi con questo tipo di ordinamento del middleware. Le risposte MVC [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) possono essere compressi.

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a>Utilizzare, eseguire ed eseguire il mapping

Configurare la pipeline HTTP usando `Use`, `Run`, e `Map`. Il `Use` metodo possibile di corto circuito la pipeline (ovvero, se non chiama un `next` delegato richiesta). `Run`è una convenzione e possono esporre alcuni componenti middleware `Run[Middleware]` metodi che eseguono alla fine della pipeline.

`Map*`le estensioni vengono utilizzate come una convenzione per la diramazione la pipeline. [Mappa](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branch, la pipeline di richieste in base alle corrispondenze del percorso di richiesta specificata. Se il percorso della richiesta inizia con il percorso specificato, viene eseguito il ramo.

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

La tabella seguente mostra le richieste e risposte da `http://localhost:1234` utilizzando il codice precedente:

| Richiesta | Risposta |
| --- | --- |
| localhost:1234 | Hello dal delegato non mappa.  |
| localhost:1234 / map1 | Eseguire il mapping di Test 1 |
| localhost:1234 / map2 | Test mappa 2 |
| localhost:1234 / map3 | Hello dal delegato non mappa.  |

Quando `Map` è utilizzato, ai segmenti di percorso corrispondenti vengono rimossi dal `HttpRequest.Path` e accodato a `HttpRequest.PathBase` per ogni richiesta.

[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branch, la pipeline di richieste in base al risultato del predicato specificato. Un predicato di tipo `Func<HttpContext, bool>` può essere usato per eseguire il mapping delle richieste in un nuovo ramo della pipeline. Nell'esempio seguente, un predicato viene usato per rilevare la presenza di una variabile di stringa di query `branch`:

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

La tabella seguente mostra le richieste e risposte da `http://localhost:1234` utilizzando il codice precedente:

| Richiesta | Risposta |
| --- | --- |
| localhost:1234 | Hello dal delegato non mappa.  |
| localhost:1234 /? ramo = master | Ramo utilizzato = master|

`Map`supporta la nidificazione, ad esempio:

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

`Map`può anche corrispondere più segmenti in una sola volta, ad esempio:

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a>Middleware incorporato

ASP.NET Core viene fornito con i componenti middleware seguenti:

| Middleware | Descrizione |
| ----- | ------- |
| [Autenticazione](xref:security/authentication/identity) | Fornisce supporto per l'autenticazione. |
| [CORS](xref:security/cors) | Configura la condivisione delle risorse Multiorigine. |
| [Memorizzazione nella cache delle risposte](xref:performance/caching/middleware) | Fornisce supporto per la memorizzazione nella cache le risposte. |
| [Compressione di risposta](xref:performance/response-compression) | Fornisce il supporto per la compressione delle risposte. |
| [Routing](xref:fundamentals/routing) | Definisce e vincola una route richiesta. |
| [Sessione](xref:fundamentals/app-state) | Fornisce il supporto per la gestione delle sessioni utente. |
| [File statici](xref:fundamentals/static-files) | Fornisce il supporto per la gestione di file statici e di esplorazione directory. |
| [Middleware di riscrittura URL](xref:fundamentals/url-rewriting) | Fornisce supporto per la riscrittura degli URL e reindirizzamento delle richieste. |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a>Middleware di scrittura

Middleware è in genere incapsulato in una classe ed esposto con un metodo di estensione. Si consideri il middleware seguente, che imposta le impostazioni cultura per la richiesta corrente dalla stringa di query:

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

Nota: Il codice di esempio precedente viene utilizzato per illustrare la creazione di un componente del middleware. Vedere [ globalizzazione e localizzazione](xref:fundamentals/localization) per il supporto di localizzazione predefinite di ASP.NET Core.

È possibile testare il middleware passando le impostazioni cultura, ad esempio `http://localhost:7997/?culture=no`.

Il codice seguente sposta il middleware di delegato a una classe:

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

Il seguente metodo di estensione espone il middleware tramite [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

Il codice seguente viene chiamato il middleware da `Configure`:

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

Middleware deve seguire il [principio dipendenze esplicite](http://deviq.com/explicit-dependencies-principle/) esponendo le relative dipendenze nel relativo costruttore. Middleware viene costruito una volta per ogni *durata applicazione*. Vedere *dipendenze richiesta* sotto se è necessario condividere servizi con middleware all'interno di una richiesta.

I componenti middleware per risolvere le dipendenze dall'inserimento di dipendenze mediante i parametri del costruttore. [`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)può anche accettare parametri aggiuntivi direttamente.

### <a name="per-request-dependencies"></a>Le dipendenze per ogni richiesta

Poiché middleware viene creato all'avvio dell'app, non per singola richiesta, *con ambito* servizi di durata utilizzati dal middleware costruttori non vengono condivise con altri tipi di dipendenza inserito durante ogni richiesta. Se è necessario condividere una *con ambito* tra del middleware e altri tipi di servizio, aggiungere questi servizi per il `Invoke` firma del metodo. Il `Invoke` metodo può accettare parametri aggiuntivi che vengono popolati dall'inserimento di dipendenze. Ad esempio:

```c#
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="resources"></a>Risorse

* [Codice di esempio utilizzati in questo documento](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [Migrazione di moduli HTTP in middleware](../migration/http-modules.md)
* [Avvio dell'applicazione](startup.md)
* [Funzionalità di richiesta](request-features.md)
