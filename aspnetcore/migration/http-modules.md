---
title: La migrazione di gestori HTTP e moduli al middleware di ASP.NET Core.
author: rick-anderson
description: 
manager: wpickett
ms.author: tdykstra
ms.date: 12/07/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/http-modules
ms.openlocfilehash: 7f08e155491b56933ae183818e9b9ee562ad8286
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/02/2018
---
# <a name="migrating-http-handlers-and-modules-to-aspnet-core-middleware"></a>La migrazione di gestori HTTP e moduli al middleware di ASP.NET Core. 

Da [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)

In questo articolo viene illustrato come eseguire la migrazione di ASP.NET esistente [moduli e i gestori da System. webServer](https://docs.microsoft.com/iis/configuration/system.webserver/) per ASP.NET Core [middleware](xref:fundamentals/middleware/index).

## <a name="modules-and-handlers-revisited"></a>I moduli e i gestori aggiornamento

Prima di procedere al middleware di ASP.NET Core, verrà innanzitutto riepilogo funzionano dei gestori e moduli HTTP:

![Gestore di moduli](http-modules/_static/moduleshandlers.png)

**I gestori sono:**

   * Le classi che implementano [IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)

   * Utilizzato per gestire le richieste con un nome file specificato o l'estensione, ad esempio *.report*

   * [Configurato](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/) in *Web. config*

**I moduli sono:**

   * Le classi che implementano [IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)

   * Viene richiamata per ogni richiesta

   * In grado di corto circuito (interrompere ulteriore elaborazione di una richiesta)

   * In grado di aggiungere alla risposta HTTP o crearne di propri

   * [Configurato](https://docs.microsoft.com//iis/configuration/system.webserver/modules/) in *Web. config*

**L'ordine in cui i moduli di elaborare le richieste in ingresso è determinato da:**

   1. Il [ciclo di vita dell'applicazione](https://msdn.microsoft.com/library/ms227673.aspx), ovvero gli eventi una serie viene generato da ASP.NET: [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest)e così via. Ogni modulo è possibile creare un gestore per uno o più eventi.

   2. Per lo stesso evento, l'ordine in cui la configurazione di tali in *Web. config*.

Oltre a moduli, è possibile aggiungere i gestori per gli eventi del ciclo di vita per il *Global.asax.cs* file. Questi gestori vengono eseguiti dopo i gestori di moduli configurati.

## <a name="from-handlers-and-modules-to-middleware"></a>Da gestori e i moduli al middleware.

**Middleware sono più semplici rispetto a gestori e i moduli HTTP:**

   * I moduli, i gestori, *Global.asax.cs*, *Web. config* (ad eccezione di configurazione di IIS) e il ciclo di vita dell'applicazione sono stati eliminati

   * I ruoli di moduli e i gestori sono stati eseguiti dai middleware

   * Middleware vengono configurati utilizzando codice anziché in *Web. config*

   * [Creazione di rami pipeline](xref:fundamentals/middleware/index#middleware-run-map-use) consente di inviare richieste al middleware specifico, dipende non solo l'URL ma anche delle intestazioni di richiesta, le stringhe di query, ecc.

**Middleware sono molto simili ai moduli:**

   * Richiamato in linea di massima per ogni richiesta

   * In grado di corto circuito, una richiesta da [non passare la richiesta al middleware successivo](#http-modules-shortcircuiting-middleware)

   * In grado di creare i propri risposta HTTP

**Middleware e i moduli vengono elaborati in un ordine diverso:**

   * Ordine di middleware è in base all'ordine in cui viene inserito nella pipeline delle richieste, mentre l'ordine dei moduli si basa principalmente sulla [ciclo di vita dell'applicazione](https://msdn.microsoft.com/library/ms227673.aspx) eventi

   * Ordine del middleware per le risposte è l'opposto rispetto a quello per le richieste, mentre l'ordine dei moduli è lo stesso per le richieste e risposte

   * Vedere [creazione di una pipeline middleware con IApplicationBuilder](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder)

![Middleware](http-modules/_static/middleware.png)

Si noti come nell'immagine precedente, il middleware di autenticazione short-circuited la richiesta.

## <a name="migrating-module-code-to-middleware"></a>Migrazione del codice modulo al middleware.

Un modulo HTTP esistente avrà un aspetto simile al seguente:

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

Come illustrato nel [Middleware](xref:fundamentals/middleware/index) pagina, un middleware di ASP.NET Core è una classe che espone un `Invoke` acquisire metodo un `HttpContext` e restituendo un `Task`. Il middleware nuovo sarà simile al seguente:

<a name="http-modules-usemiddleware"></a>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

Il modello di middleware precedente è stato effettuato dalla sezione su [scrittura middleware](xref:fundamentals/middleware/index#middleware-writing-middleware).

Il *MyMiddlewareExtensions* classe helper rende più semplice configurare il middleware del `Startup` classe. Il `UseMyMiddleware` metodo aggiunge la classe middleware alla pipeline delle richieste. Servizi richiesti dal middleware ottengano inseriti nel costruttore del middleware.

<a name="http-modules-shortcircuiting-middleware"></a>

Il modulo potrebbe terminare una richiesta, ad esempio, se l'utente non autorizzato:

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

Un tipo di middleware viene gestita tramite la chiamata non `Invoke` nel middleware successivo nella pipeline. Tenere presente che questo non termina completamente la richiesta, in quanto middlewares precedente ancora da richiamare quando la risposta procede attraverso la pipeline.

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

Quando si esegue la migrazione di funzionalità del modulo per il middleware di nuovo, è possibile che il codice non viene compilato perché la `HttpContext` classe è stata modificata in modo significativo in ASP.NET Core. [In un secondo momento](#migrating-to-the-new-httpcontext), si vedrà come eseguire la migrazione alla nuova ASP.NET Core HttpContext.

## <a name="migrating-module-insertion-into-the-request-pipeline"></a>Inserimento di modulo migrazione nella pipeline delle richieste

I moduli HTTP vengono in genere aggiunti per la richiesta di pipeline utilizzando *Web. config*:

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

Il problema, convertire [aggiunta del nuovo middleware](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder) alla pipeline delle richieste nel `Startup` classe:

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

La posizione esatta nella pipeline di punto di inserimento del nuovo middleware dipende dall'evento che gestito come modulo (`BeginRequest`, `EndRequest`e così via) e il relativo ordine nell'elenco dei moduli in *Web. config*.

Come già non indicato, vi è alcun ciclo di vita delle applicazioni in ASP.NET Core e l'ordine in cui vengono elaborate le risposte dal middleware è diverso da quello usato dai moduli. Questo potrebbe rendere la decisione di ordinamento più complessa.

Se l'ordinamento diventa un problema, è possibile suddividere un modulo in più componenti middleware che possono essere ordinati in modo indipendente.

## <a name="migrating-handler-code-to-middleware"></a>Migrazione del codice del gestore al middleware.

Un gestore HTTP è simile alla seguente:

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

Nel progetto ASP.NET Core, è necessario convertire in un tipo di middleware simile al seguente:

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

Questo middleware è molto simile per il middleware corrispondente ai moduli. L'unica differenza è che qui non è presente alcuna chiamata a `_next.Invoke(context)`. Che è utile, perché il gestore non è alla fine della pipeline di richieste, in modo che vi sia alcun middleware successivo da richiamare.

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a>La migrazione di inserimento gestore nella pipeline delle richieste

Configurazione di un gestore HTTP viene eseguita in *Web. config* e simile al seguente:

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

È possibile convertire questo valore tramite l'aggiunta del nuovo middleware gestore alla pipeline delle richieste nel `Startup` (classe), simile al middleware convertito da moduli. Il problema con questo approccio è che invierebbe tutte le richieste per il nuovo middleware gestore. Tuttavia, si desidera solo le richieste con una determinata estensione per raggiungere il middleware. Che fornisce la stessa funzionalità che si aveva con il gestore HTTP.

Una soluzione consiste nel creare un ramo della pipeline per le richieste con una determinata estensione utilizzando il `MapWhen` metodo di estensione. A tale scopo, lo stesso `Configure` metodo in cui si aggiunge il middleware di altri:

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

`MapWhen` utilizza questi parametri:

1. Un'espressione lambda che accetta il `HttpContext` e restituisce `true` se la richiesta deve essere inoltrato al ramo. Ciò significa che è possibile creare rami di richieste di base non solo l'estensione, ma anche le intestazioni di richiesta, i parametri di stringa di query e così via.

2. Un'espressione lambda che accetta un `IApplicationBuilder` e aggiunge il middleware di tutti per il ramo. Ciò significa che è possibile aggiungere altro middleware per il ramo davanti del middleware di gestore.

Middleware aggiunto alla pipeline prima che il ramo verrà richiamato in tutte le richieste; il ramo avrà alcun effetto su di essi.

## <a name="loading-middleware-options-using-the-options-pattern"></a>Il caricamento di opzioni del middleware usando il modello di opzioni

Alcuni gestori e i moduli sono disponibili opzioni di configurazione che vengono archiviate in *Web. config*. Tuttavia, in ASP.NET Core viene utilizzato un nuovo modello di configurazione al posto di *Web. config*.

Il nuovo [sistema di configurazione](xref:fundamentals/configuration/index) fornisce le seguenti opzioni per risolvere il problema:

* Inserire direttamente le opzioni in middleware, come illustrato nel [nella sezione successiva](#loading-middleware-options-through-direct-injection).

* Utilizzare il [modello opzioni](xref:fundamentals/configuration/options):

1.  Creare una classe che contenga le opzioni del middleware, ad esempio:

    [!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2.  Archiviare i valori di opzione

    Il sistema di configurazione consente di archiviare i valori di opzione in qualsiasi punto desiderato. Tuttavia, più siti utilizzare *appSettings. JSON*, pertanto l'utente verrà reindirizzato a tale approccio:

    [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

    *MyMiddlewareOptionsSection* qui è un nome di sezione. Non deve essere identico al nome della classe di opzioni.

3. Associare i valori dell'opzione con la classe di opzioni

    Il modello di opzioni Usa framework della ASP.NET Core dipendenza attacco intrusivo nel codice per associare il tipo di opzioni (ad esempio `MyMiddlewareOptions`) con un `MyMiddlewareOptions` oggetto con le opzioni.

    Aggiornamento del `Startup` classe:

    1.  Se si utilizza *appSettings. JSON*, aggiungerlo al generatore di configurazione nel `Startup` costruttore:

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

    2.  Configurare il servizio di opzioni:

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    3.  Associare le opzioni disponibili con la classe di opzioni:

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4.  Inserire le opzioni nel costruttore del middleware. È simile per l'inserimento di opzioni in un controller.

  [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

  Il [UseMiddleware](#http-modules-usemiddleware) metodo di estensione che aggiunge il middleware per la `IApplicationBuilder` si occupa di inserimento di dipendenze.

  Questo non è limitato a `IOptions` oggetti. In questo modo può essere inserito da qualsiasi altro oggetto che richiede il middleware.

## <a name="loading-middleware-options-through-direct-injection"></a>Il caricamento di opzioni del middleware tramite inserimento diretto

Il modello di opzioni presenta il vantaggio che crea separato accoppiamento tra i valori delle opzioni e utenti. Una volta associato a una classe di opzioni i valori delle opzioni effettivo, qualsiasi altra classe può accedere alle opzioni tramite il framework di inserimento di dipendenza. Non è necessario passare intorno ai valori di opzioni.

Questo modo si ottengono tuttavia se si desidera utilizzare il middleware stesso due volte, con opzioni diverse. Ad esempio un'autorizzazione middleware utilizzato in diversi rami che consente di ruoli diversi. È possibile associare i due oggetti diverse opzioni con la classe di opzioni di uno.

La soluzione consiste nell'ottenere gli oggetti di opzioni con i valori effettivi opzioni la `Startup` classe e passare direttamente in ogni istanza del middleware.

1.  Aggiungere una seconda chiave da *appSettings. JSON*

    Per aggiungere un secondo set di opzioni per il *appSettings. JSON* file, utilizzare una nuova chiave per identificarla in modo univoco:

    [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2.  Recuperare i valori delle opzioni e passarle al middleware. Il `Use...` metodo di estensione (che aggiunge il middleware alla pipeline) è un punto logico per passare i valori di opzione: 

    [!code-csharp[](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

4.  Abilitare il middleware accettare un parametro di opzioni. Fornire un overload di `Use...` metodo di estensione (che accetta il parametro di opzioni e lo passa al `UseMiddleware`). Quando `UseMiddleware` viene chiamato con parametri, passa i parametri del costruttore del middleware quando viene creata un'istanza dell'oggetto middleware.

    [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

    Si noti come questo include l'oggetto di opzioni in un `OptionsWrapper` oggetto. Implementa `IOptions`, come previsto dal costruttore del middleware.

## <a name="migrating-to-the-new-httpcontext"></a>La migrazione a nuovi HttpContext

Illustrato in precedenza che il `Invoke` del middleware metodo accetta un parametro di tipo `HttpContext`:

```csharp
public async Task Invoke(HttpContext context)
```

`HttpContext` è stato modificato in modo significativo in ASP.NET Core. In questa sezione viene illustrato come convertire le proprietà utilizzate più di frequente di [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext) al nuovo `Microsoft.AspNetCore.Http.HttpContext`.

### <a name="httpcontext"></a>HttpContext

**HttpContext. Items** viene convertito in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

**ID richiesta univoco (non controparte System.Web.HttpContext)**

Fornisce un id univoco per ogni richiesta. Molto utile da includere nei file registro.

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a>HttpContext.Request

**HttpContext.Request.HttpMethod** viene convertito in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

**HttpContext.Request.QueryString** viene convertito in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

**HttpContext.Request.Url** e **HttpContext.Request.RawUrl** Traduci in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

**HttpContext.Request.IsSecureConnection** viene convertito in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

**HttpContext.Request.UserHostAddress** viene convertito in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

**HttpContext.Request.Cookies** viene convertito in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

**HttpContext.Request.RequestContext.RouteData** viene convertito in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

**HttpContext.Request.Headers** viene convertito in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

**HttpContext.Request.UserAgent** viene convertito in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

**HttpContext.Request.UrlReferrer** viene convertito in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

**HttpContext.Request.ContentType** viene convertito in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

**HttpContext.Request.Form** viene convertito in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> Leggere i valori del form solo se il tipo di contenuto sub *x-www-form-urlencoded* o *form-data*.

**HttpContext.Request.InputStream** viene convertito in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> Utilizzare questo codice solo in un middleware di tipo di gestore, alla fine di una pipeline.
>
>È possibile leggere il corpo non elaborato, come illustrato in precedenza una sola volta per ogni richiesta. Tentativo di leggere il corpo dopo la prima lettura middleware leggerà un corpo vuoto.
>
>Questo non si applica alla lettura di un modulo come illustrato in precedenza, perché questa operazione viene effettuata da un buffer.

### <a name="httpcontextresponse"></a>HttpContext.Response

**HttpContext.Response.Status** e **HttpContext.Response.StatusDescription** Traduci in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

**HttpContext.Response.ContentEncoding** e **HttpContext.Response.ContentType** Traduci in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

**HttpContext.Response.ContentType** sul proprio anche viene convertito in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

**HttpContext.Response.Output** viene convertito in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

**HttpContext.Response.TransmitFile**

Disporre di un file è descritto [qui](../fundamentals/request-features.md#middleware-and-request-features).

**HttpContext.Response.Headers**

L'invio delle intestazioni di risposta è complicato dal fatto che se si imposta dopo qualsiasi elemento è stato scritto per il corpo della risposta, non verrà inviati.

La soluzione consiste nell'impostare un metodo di callback che verrà chiamato destra prima della scrittura dell'inizio della risposta. Questo è più adatto all'inizio del `Invoke` metodo del middleware. È il metodo di callback che imposta le intestazioni di risposta.

Il codice seguente imposta un metodo di callback chiamato `SetHeaders`:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

Il `SetHeaders` metodo di callback è analogo al seguente:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

**HttpContext.Response.Cookies**

Si spostano i cookie nel browser in un *Set-Cookie* intestazione della risposta. Di conseguenza, inviare cookie richiede il callback stesso utilizzato per l'invio delle intestazioni di risposta:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

Il `SetCookies` metodo di callback avrà un aspetto simile al seguente:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a>Risorse aggiuntive

* [Panoramica di moduli HTTP e i gestori HTTP](/iis/configuration/system.webserver/)
* [Configurazione](xref:fundamentals/configuration/index)
* [Avvio dell'applicazione](xref:fundamentals/startup)
* [Middleware](xref:fundamentals/middleware/index)
