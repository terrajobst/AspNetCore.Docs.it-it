---
title: Middleware di ASP.NET Core
author: rick-anderson
description: Informazioni sul middelware di ASP.NET Core e la pipeline delle richieste.
manager: wpickett
ms.author: riande
ms.date: 01/22/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/middleware/index
ms.openlocfilehash: 3312b27f936340a73243224c1a716fe421f178bc
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="aspnet-core-middleware"></a>Middleware di ASP.NET Core

[Rick Anderson](https://twitter.com/RickAndMSFT) e [Steve Smith](https://ardalis.com/)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-middleware"></a>Che cos'è il middleware?

Il middleware è un software che viene assemblato in una pipeline dell'applicazione per gestire richieste e risposte. Ogni componente:

* Sceglie se passare la richiesta al componente successivo nella pipeline.
* Può eseguire le operazioni prima e dopo aver chiamato il componente successivo nella pipeline. 

Per compilare la pipeline delle richieste vengono usati i delegati di richiesta. I delegati di richiesta gestiscono ogni richiesta HTTP.

I delegati di richiesta vengono configurati tramite i metodi di estensione [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) e [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions). È possibile specificare un singolo delegato di richiesta inline come metodo anonimo (chiamato middleware inline) o definirlo in una classe riutilizzabile. Le classi riutilizzabili o i metodi anonimi inline costituiscono il *middleware* o sono *componenti middleware*. Ogni componente del middleware è responsabile della chiamata del componente seguente nella pipeline o del corto circuito della catena, se necessario.

In [Eseguire la migrazione di moduli HTTP in middleware](xref:migration/http-modules) sono descritte le differenze tra le pipeline delle richieste in ASP.NET Core e in ASP.NET 4.x e sono riportati altri esempi di middleware.

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a>Creazione di una pipeline middleware con IApplicationBuilder

La pipeline delle richieste ASP.NET Core è costituita da una sequenza di delegati di richiesta, chiamati uno dopo l'altro, come illustrato nella figura seguente (le frecce nere indicano il thread di esecuzione):

![Modello di elaborazione di richiesta che visualizza una richiesta in arrivo, l'elaborazione tramite tre middleware e la risposta che esce dall'applicazione. Ogni middleware esegue la relativa logica e passa la richiesta al middleware successivo in corrispondenza dell'istruzione next(). Dopo che il terzo middleware ha elaborato la richiesta, la richiesta torna ai due middleware precedenti in ordine inverso per un'ulteriore elaborazione dopo le istruzioni next() prima di uscire dall'applicazione sotto forma di risposta al client.](index/_static/request-delegate-pipeline.png)

Ogni delegato può eseguire operazioni prima e dopo il delegato successivo. Un delegato può anche decidere di non passare una richiesta al delegato successivo e creare un corto circuito della pipeline delle richieste. Il corto circuito è spesso opportuno poiché evita l'esecuzione di operazioni non necessarie. Ad esempio, il middleware dei file statici può restituire una richiesta di un file statico ed effettuare il corto circuito della pipeline rimanente. I delegati che gestiscono le eccezioni devono essere chiamati nella prima parte della pipeline in modo che possano individuare le eccezioni che si verificano nelle parti successive della pipeline.

L'app di ASP.NET Core più semplice imposta un delegato di richiesta singolo che gestisce tutte le richieste. In questo caso non è inclusa una pipeline di richieste effettiva. Al contrario, viene chiamata una singola funzione anonima in risposta a ogni richiesta HTTP.

[!code-csharp[](index/sample/Middleware/Startup.cs)]

Il primo delegato [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) termina la pipeline.

È possibile concatenare più delegati di richiesta insieme a [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions). Il parametro `next` rappresenta il delegato successivo nella pipeline. (Tenere presente che è possibile eseguire il corto circuito della pipeline *non* chiamando il parametro *successivo*). In genere è possibile eseguire un'azione prima e dopo il delegato successivo, come illustra l'esempio seguente:

[!code-csharp[](index/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> Non chiamare `next.Invoke` dopo aver inviato la risposta al client. Le modifiche apportate a `HttpResponse` dopo l'avvio della risposta generano un'eccezione. Ad esempio, le modifiche come l'impostazione delle intestazioni, del codice di stato e così via, generano un'eccezione. Scrivere nel corpo della risposta dopo aver chiamato `next`:
> - Può causare una violazione del protocollo. Ad esempio, scrivere un contenuto che supera il valore `content-length` specificato.
> - Può danneggiare il formato del corpo. Ad esempio, scrivere un piè di pagina HTML in un file CSS.
>
> [HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) è un suggerimento utile per indicare se le intestazioni sono state inviate e/o se è stato scritto un contenuto nel corpo.

## <a name="ordering"></a>Ordinazioni

L'ordine in cui vengono aggiunti i componenti middleware nel metodo `Configure` definisce l'ordine in cui sono chiamati nelle richieste e l'ordine inverso per la risposta. Questo ordinamento è fondamentale per la sicurezza, le prestazioni e la funzionalità.

Il metodo Configure (illustrato di seguito) aggiunge i componenti middleware seguenti:

1. Gestione errori/eccezioni
2. File server statico
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

Nel codice precedente poiché `UseExceptionHandler` è il primo componente middleware aggiunto alla pipeline, il componente consente di rilevare le eccezioni che si verificano nelle chiamate successive.

Il middleware dei file statici viene chiamato nella prima parte della pipeline in moda che possa gestire le richieste ed eseguire un corto circuito senza passare attraverso i componenti rimanenti. Il middleware dei file statici **non** offre controlli di autorizzazione. I file serviti dal sever, inclusi i file in *wwwroot*, sono disponibili pubblicamente. Vedere [Usare file statici](xref:fundamentals/static-files) per un approccio per proteggere i file statici.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)


Se la richiesta non è gestita dal middleware dei file statici, viene passata al middleware Identity (`app.UseAuthentication`), che esegue l'autenticazione. Identity non esegue il corto circuito di richieste non autenticate. Sebbene Identity esegua l'autenticazione delle richieste, l'autorizzazione (e il rifiuto) si verifica solo dopo che MVC seleziona una pagina Razor o un controller specifico e un'azione.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Se la richiesta non è gestita dal middleware dei file statici, viene passata al middleware Identity (`app.UseIdentity`), che esegue l'autenticazione. Identity non esegue il corto circuito di richieste non autenticate. Sebbene Identity esegua l'autenticazione delle richieste, l'autorizzazione (e il rifiuto) si verifica solo dopo che MVC seleziona un controller specifico e un'azione.

-----------

L'esempio seguente illustra un ordinamento del middleware nel quale le richieste dei file statici vengono gestite dal middleware dei file statici prima del middleware di compressione delle risposte. I file statici non vengono compressi con questo tipo di ordinamento del middleware. Le risposte MVC da [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) possono essere compresse.

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

### <a name="use-run-and-map"></a>Use, Run e Map

Configurare la pipeline HTTP usando `Use`,`Run` e `Map`. Il metodo `Use` può eseguire il corto circuito della pipeline, ovvero non chiamare un delegato di richiesta `next`. `Run` è una convenzione e alcuni componenti del middleware possono esporre metodi `Run[Middleware]` che vengono eseguiti al termine della pipeline.

Le estensioni `Map*` vengono usate come convenzione per la diramazione della pipeline. [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) crea un ramo nella pipeline delle richieste in base alle corrispondenze del percorso della richiesta specificato. Se il percorso della richiesta inizia con il percorso specificato, il ramo viene eseguito.

[!code-csharp[](index/sample/Chain/StartupMap.cs?name=snippet1)]

La tabella seguente visualizza le richieste e le risposte da `http://localhost:1234` usando il codice precedente:

| Richiesta | Risposta |
| --- | --- |
| localhost:1234 | Hello from non-Map delegate.  |
| localhost:1234/map1 | Map Test 1 |
| localhost:1234/map2 | Map Test 2 |
| localhost:1234/map3 | Hello from non-Map delegate.  |

Quando si usa `Map`, i segmenti di percorso corrispondenti vengono rimossi da `HttpRequest.Path` e aggiunti a `HttpRequest.PathBase` per ogni richiesta.

[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) crea un ramo nella pipeline delle richieste in base al risultato del predicato specificato. È possibile usare qualsiasi predicato di tipo `Func<HttpContext, bool>` per mappare le richieste a un nuovo ramo della pipeline. Nell'esempio seguente viene usato un predicato per rilevare la presenza di una variabile di stringa di query `branch`:

[!code-csharp[](index/sample/Chain/StartupMapWhen.cs?name=snippet1)]

La tabella seguente visualizza le richieste e le risposte da `http://localhost:1234` usando il codice precedente:

| Richiesta | Risposta |
| --- | --- |
| localhost:1234 | Hello from non-Map delegate.  |
| localhost:1234/?branch=master | Ramo usato = master|

`Map` supporta l'annidamento, ad esempio:

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

`Map` può anche trovare la corrispondenza di più segmenti contemporaneamente, ad esempio:

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a>Middleware incorporato

ASP.NET Core viene dato con i componenti middleware seguenti e una descrizione dell'ordine in cui devono essere aggiunti:

| Middleware | Descrizione | Ordinamento |
| ---------- | ----------- | ----- |
| [Autenticazione](xref:security/authentication/identity) | Offre il supporto dell'autenticazione. | Prima di `HttpContext.User`. Terminale per i callback OAuth. |
| [CORS](xref:security/cors) | Configura la condivisione di risorse tra le origini (CORS). | Prima dei componenti che usano CORS. |
| [Diagnostica](xref:fundamentals/error-handling) | Configura la diagnostica. | Prima dei componenti che generano errori. |
| [ForwardedHeaders/HttpOverrides](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | Inoltra le intestazioni proxy nella richiesta corrente. | Prima dei componenti che usano i campi aggiornati, ad esempio Scheme, Host, ClientIP, Method. |
| [Memorizzazione nella cache delle risposte](xref:performance/caching/middleware) | Offre il supporto per la memorizzazione delle risposte nella cache. | Prima dei componenti che richiedono la memorizzazione nella cache. |
| [Compressione delle risposte](xref:performance/response-compression) | Offre il supporto per la compressione delle risposte. | Prima dei componenti che richiedono la compressione. |
| [RequestLocalization](xref:fundamentals/localization) | Offre il supporto per la localizzazione. | Prima dei componenti sensibili alla localizzazione. |
| [Routing](xref:fundamentals/routing) | Definisce e vincola le route di richiesta. | Terminale per le route corrispondenti. |
| [Sessione](xref:fundamentals/app-state) | Offre il supporto per la gestione delle sessioni utente. | Prima dei componenti che richiedono la Sessione. |
| [File statici](xref:fundamentals/static-files) | Offre il supporto per la gestione di file statici e l'esplorazione directory. | Terminale se una richiesta corrisponde ai file. |
| [Riscrittura dell'URL](xref:fundamentals/url-rewriting) | Offre il supporto per la riscrittura degli URL e il reindirizzamento delle richieste. | Prima dei componenti che usano l'URL. |
| [Oggetti WebSocket](xref:fundamentals/websockets) | Abilita il protocollo WebSocket. | Prima dei componenti necessari per accettare le richieste WebSocket. |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a>Scrittura di middleware

Il middleware è in genere incapsulato in una classe ed esposto con un metodo di estensione. Si consideri il middleware seguente, che specifica le impostazioni cultura per la richiesta corrente dalla stringa di query:

[!code-csharp[](index/sample/Culture/StartupCulture.cs?name=snippet1)]

Nota: il codice di esempio precedente viene usato per illustrare la creazione di un componente middleware. Vedere [Globalizzazione e localizzazione](xref:fundamentals/localization) per il supporto di localizzazione incorporato di ASP.NET Core.

È possibile testare il middleware passando le impostazioni cultura, ad esempio `http://localhost:7997/?culture=no`.

Il codice seguente sposta il delegato middleware in una classe:

[!code-csharp[](index/sample/Culture/RequestCultureMiddleware.cs)]

> [!NOTE]
> In ASP.NET Core 1. x il nome del metodo `Task` del middleware deve essere `Invoke`. In ASP.NET Core 2.0 o versioni successive il nome può essere `Invoke` o `InvokeAsync`.

Il metodo di estensione seguente espone il middleware tramite [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):

[!code-csharp[](index/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

Il codice seguente chiama il middleware da `Configure`:

[!code-csharp[](index/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

Il middleware deve seguire il [principio delle dipendenze esplicite](http://deviq.com/explicit-dependencies-principle/) esponendo le dipendenze nel costruttore. Il middleware viene costruito una volta per ogni *durata applicazione*. Se è necessario condividere servizi con il middleware all'interno di una richiesta, vedere *Dipendenze per richiesta* di seguito.

I componenti middleware possono risolvere le dipendenze dall'inserimento di dipendenze mediante i parametri del costruttore. [`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) può anche accettare direttamente parametri aggiuntivi.

### <a name="per-request-dependencies"></a>Dipendenze per richiesta

Poiché il middleware viene creato all'avvio dell'app e non per richiesta, i servizi di durata *con ambito* usati dai costruttori del middleware non vengono condivisi con altri tipi di inserimento di dipendenze durante ogni richiesta. Se è necessario condividere un servizio *con ambito* tra il proprio middleware e altri tipi, aggiungere i servizi alla firma del metodo `Invoke`. Il metodo `Invoke` può accettare parametri aggiuntivi popolati dall'inserimento delle dipendenze. Ad esempio:

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

## <a name="additional-resources"></a>Risorse aggiuntive

* [Eseguire la migrazione di moduli HTTP in middleware](xref:migration/http-modules)
* [Avvio dell'applicazione](xref:fundamentals/startup)
* [Funzionalità di richiesta](xref:fundamentals/request-features)
* [Attivazione del middleware basata sulla factory](xref:fundamentals/middleware/extensibility)
* [Attivazione del middleware con un contenitore di terze parti](xref:fundamentals/middleware/extensibility-third-party-container)
