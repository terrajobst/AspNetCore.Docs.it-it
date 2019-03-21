---
title: Eseguire la migrazione di moduli e gestori HTTP in middleware di ASP.NET Core
author: rick-anderson
description: ''
ms.author: tdykstra
ms.date: 12/07/2016
uid: migration/http-modules
ms.openlocfilehash: 516230a66ee3edba986c91d79684256aa8e4c994
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209846"
---
# <a name="migrate-http-handlers-and-modules-to-aspnet-core-middleware"></a>Eseguire la migrazione di moduli e gestori HTTP in middleware di ASP.NET Core

Da [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)

Questo articolo illustra come eseguire la migrazione ASP.NET esistenti [moduli HTTP e i gestori da System. webServer](/iis/configuration/system.webserver/) ad ASP.NET Core [middleware](xref:fundamentals/middleware/index).

## <a name="modules-and-handlers-revisited"></a>I moduli e gestori Rivisitazione della

Prima di procedere al middleware di ASP.NET Core, è opportuno innanzitutto riepilogare come funzionano i gestori e moduli HTTP:

![Gestore di moduli](http-modules/_static/moduleshandlers.png)

**I gestori sono:**

* Le classi che implementano [IHttpHandler](/dotnet/api/system.web.ihttphandler)

* Consente di gestire le richieste con un nome file specificato o l'estensione, ad esempio *.report*

* [Configurata](/iis/configuration/system.webserver/handlers/) in *Web. config*

**I moduli sono:**

* Le classi che implementano [IHttpModule](/dotnet/api/system.web.ihttpmodule)

* Richiamato per ogni richiesta

* In grado di corto circuito (interrompere ulteriore elaborazione di una richiesta)

* Possibilità di aggiungere alla risposta HTTP, o crearne di propri

* [Configurata](/iis/configuration/system.webserver/modules/) in *Web. config*

**L'ordine in cui i moduli elaborino le richieste in ingresso è determinato da:**

1. Il [ciclo di vita dell'applicazione](https://msdn.microsoft.com/library/ms227673.aspx), ovvero un eventi della serie generato da ASP.NET: [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest), etc. Ogni modulo è possibile creare un gestore per uno o più eventi.

2. Per lo stesso evento, l'ordine in cui configurarli nel *Web. config*.

Oltre ai moduli, è possibile aggiungere gestori per gli eventi del ciclo di vita per le *Global.asax.cs* file. Dopo i gestori nei moduli configurati, eseguire questi gestori.

## <a name="from-handlers-and-modules-to-middleware"></a>Da gestori e moduli in middleware

**Middleware sono più semplici rispetto a gestori e moduli HTTP:**

* I moduli, gestori *Global.asax.cs*, *Web. config* (ad eccezione di configurazione di IIS) e il ciclo di vita dell'applicazione sono stati eliminati

* I ruoli di entrambi i moduli e gestori sono stati eseguiti al middleware

* Middleware vengono configurati utilizzando codice anziché in *Web. config*

* [Creazione di rami pipeline](xref:fundamentals/middleware/index#use-run-and-map) consente di inviare richieste al middleware specifico, basato sull'URL non solo ma anche in intestazioni della richiesta, le stringhe di query, e così via.

**Middleware sono molto simili ai moduli:**

* Richiamato in linea di principio per ogni richiesta

* In grado di corto circuito da una richiesta, [non trasmettere la richiesta al middleware successivo](#http-modules-shortcircuiting-middleware)

* In grado di creare le proprie risposte HTTP

**Middleware e i moduli vengono elaborati in un ordine diverso:**

* Ordine del middleware è in base all'ordine in cui viene inserito nel pipeline delle richieste, mentre l'ordine dei moduli si basa principalmente sulla [ciclo di vita dell'applicazione](https://msdn.microsoft.com/library/ms227673.aspx) eventi

* Ordine del middleware per le risposte è l'opposto rispetto a quello per le richieste, mentre l'ordine dei moduli è lo stesso per le richieste e risposte

* Vedere [creare una pipeline middleware con IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)

![Middleware](http-modules/_static/middleware.png)

Si noti come nell'immagine precedente, il middleware di autenticazione bloccata la richiesta.

## <a name="migrating-module-code-to-middleware"></a>Migrazione del codice modulo al middleware

Un modulo HTTP esistente avrà un aspetto simile al seguente:

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

Come illustrato nel [Middleware](xref:fundamentals/middleware/index) pagina, un middleware di ASP.NET Core è una classe che espone un `Invoke` prendendo metodo un `HttpContext` e restituendo un `Task`. Il middleware nuovo avrà un aspetto simile al seguente:

<a name="http-modules-usemiddleware"></a>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

Il modello di middleware precedente è stato creato nella sezione [scrittura di middleware](xref:fundamentals/middleware/write).

Il *MyMiddlewareExtensions* rende più semplice configurare il middleware nella classe helper di `Startup` classe. Il `UseMyMiddleware` metodo aggiunge una classe middleware alla pipeline delle richieste. I servizi richiesti dal middleware ottengano inseriti nel costruttore del middleware.

<a name="http-modules-shortcircuiting-middleware"></a>

Il modulo potrebbe infatti terminare una richiesta, se l'utente non è stata autorizzata, ad esempio:

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

Un tipo di middleware viene gestita tramite la chiamata non `Invoke` il middleware successivo nella pipeline. Tenere presente che questo non termina completamente la richiesta, poiché il middleware precedente verrà ancora richiamata quando la risposta arrivi attraverso la pipeline.

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

Quando si esegue la migrazione delle funzionalità del modulo per il middleware di nuovo, è possibile che il codice non viene compilato perché il `HttpContext` classe ha subito modifiche significative in ASP.NET Core. [In un secondo momento](#migrating-to-the-new-httpcontext), scoprirai come eseguire la migrazione al nuovo ASP.NET Core HttpContext.

## <a name="migrating-module-insertion-into-the-request-pipeline"></a>La migrazione, l'inserimento modulo nella pipeline delle richieste

In genere vengono aggiunti i moduli HTTP per la richiesta di pipeline tramite *Web. config*:

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

This per convertire [aggiungere il middleware di nuovo](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) alla pipeline delle richieste nel `Startup` classe:

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

Punto nella pipeline in cui si inserisce il middleware di nuovo varia a seconda dell'evento che gestito come modulo (`BeginRequest`, `EndRequest`e così via) e l'ordine nell'elenco dei moduli nel *Web. config*.

Come già non indicato, non contiene alcun ciclo di vita dell'applicazione ASP.NET Core e l'ordine in cui le risposte vengono elaborate dal middleware differente da quello usato dai moduli. Ciò potrebbe rendere la decisione di ordinamento più impegnativo.

Se l'ordinamento diventa un problema, è possibile suddividere un modulo in più componenti middleware che possono essere ordinati in modo indipendente.

## <a name="migrating-handler-code-to-middleware"></a>Migrazione del codice del gestore in middleware

Un gestore HTTP simile al seguente:

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

Nel progetto ASP.NET Core, è necessario convertire in un middleware simile al seguente:

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

Questo middleware è molto simile al middleware corrispondente ai moduli. L'unica vera differenza è che seguito non vi è alcuna chiamata a `_next.Invoke(context)`. Che ha senso, perché il gestore di è alla fine della pipeline delle richieste, pertanto sarà presente alcun middleware successivo da richiamare.

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a>La migrazione, l'inserimento del gestore nella pipeline delle richieste

Configurazione di un gestore HTTP viene eseguita in *Web. config* e ha un aspetto simile al seguente:

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

È possibile convertire questo aggiungendo il middleware di gestore di nuovo alla pipeline delle richieste nel `Startup` (classe), simile al middleware convertito da moduli. Il problema con questo approccio è che lo invierà tutte le richieste per il middleware di gestore di nuovo. Tuttavia, è necessario solo alle richieste con una determinata estensione di raggiungere il middleware. Che consente la stessa funzionalità che è necessario con il gestore HTTP.

Una soluzione consiste nel creare un ramo della pipeline per le richieste con una determinata estensione, usando il `MapWhen` metodo di estensione. A tale scopo, lo stesso `Configure` metodo in cui si aggiunge l'altro middleware:

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

`MapWhen` utilizza questi parametri:

1. Un'espressione lambda che accetta il `HttpContext` e restituisce `true` se la richiesta deve passare al ramo. Ciò significa che è possibile creare una diramazione delle richieste non solo in base all'estensione, ma anche le intestazioni di richiesta parametri della stringa di query, e così via.

2. Un'espressione lambda che accetta un `IApplicationBuilder` e aggiunge il middleware di tutti per il ramo. Ciò significa che è possibile aggiungere altro middleware per il ramo davanti il middleware di gestore.

Middleware aggiunto alla pipeline prima che il ramo verrà richiamato in tutte le richieste; il ramo avrà alcun effetto su di essi.

## <a name="loading-middleware-options-using-the-options-pattern"></a>Le opzioni del middleware adottando il modello di opzioni di caricamento

Alcuni moduli e gestori hanno opzioni di configurazione che vengono archiviate nel *Web. config*. Tuttavia, in ASP.NET Core viene usato un nuovo modello di configurazione al posto di *Web. config*.

Il nuovo [sistema di configurazione](xref:fundamentals/configuration/index) fornisce le seguenti opzioni per risolvere il problema:

* Inserire direttamente le opzioni nel middleware, come illustrato nella [nella sezione successiva](#loading-middleware-options-through-direct-injection).

* Usare la [modello di opzioni](xref:fundamentals/configuration/options):

1. Creare una classe che contenga le opzioni del middleware, ad esempio:

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2. I valori delle opzioni di Store

   Il sistema di configurazione consente di archiviare i valori delle opzioni in qualsiasi punto desiderato. Tuttavia, i siti più utilizzare *appSettings. JSON*, pertanto è possibile adottare un tale approccio:

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

   *MyMiddlewareOptionsSection* seguito è riportato un nome di sezione. Non deve necessariamente corrispondere al nome della classe di opzioni.

3. Associare i valori delle opzioni con la classe di opzioni

    Il modello di opzioni Usa i framework di inserimento delle dipendenze di ASP.NET Core per associare il tipo di opzioni (ad esempio `MyMiddlewareOptions`) con un `MyMiddlewareOptions` oggetto con le opzioni effettive.

    Aggiornamento di `Startup` classe:

   1. Se si usa *appSettings. JSON*, aggiungerlo al generatore di configurazione nel `Startup` costruttore:

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

   2. Configurare il servizio di opzioni:

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

   3. Associare le opzioni disponibili con la classe di opzioni:

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4. Inserire le opzioni nel costruttore del middleware. È simile all'inserimento di opzioni in un controller.

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

   Il [UseMiddleware](#http-modules-usemiddleware) metodo di estensione che aggiunge il middleware per la `IApplicationBuilder` si occupa di inserimento delle dipendenze.

   Non è limitato a `IOptions` oggetti. Qualsiasi altro oggetto che richiede il middleware può essere inserito in questo modo.

## <a name="loading-middleware-options-through-direct-injection"></a>Il caricamento di opzioni del middleware tramite l'inserimento diretto

Il modello di opzioni presenta il vantaggio che crea loose accoppiamento tra i valori delle opzioni e i relativi utenti. Una volta associato a una classe di opzioni i valori effettivi di opzioni, nessun'altra classe può accedere alle opzioni tramite il framework di inserimento delle dipendenze. Non è necessario passare intorno ai valori di opzioni.

Questo modo si ottengono tuttavia se si desidera usare il middleware stesso due volte, con opzioni diverse. Ad esempio un middleware di autorizzazione usato in diversi rami che consente diversi ruoli. È possibile associare due oggetti opzioni diverse con la classe di uno opzioni.

La soluzione consiste nell'ottenere gli oggetti di opzioni con i valori di opzioni effettivo nel `Startup` classe e passarle direttamente a ogni istanza del proprio middleware.

1. Aggiungere una seconda chiave da *appSettings. JSON*

   Per aggiungere un secondo set di opzioni per la *appSettings. JSON* file, usare una nuova chiave per identificarlo in modo univoco:

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2. Recuperare i valori delle opzioni e passarle al middleware. Il `Use...` metodo di estensione (che aggiunge il middleware alla pipeline) è una posizione logica per passare i valori delle opzioni: 

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

3. Abilitare il middleware di accettare un parametro di opzioni. Fornire un overload del `Use...` metodo di estensione (che accetta il parametro options e lo passa al `UseMiddleware`). Quando si `UseMiddleware` viene chiamato con parametri, passa i parametri al costruttore del middleware quando si crea un'istanza dell'oggetto di middleware.

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

   Si noti come si esegue il wrapping l'oggetto di opzioni in un `OptionsWrapper` oggetto. Ciò implementa `IOptions`, come previsto dal costruttore del middleware.

## <a name="migrating-to-the-new-httpcontext"></a>Eseguire la migrazione a nuovo HttpContext

Illustrato in precedenza che il `Invoke` metodo nel proprio middleware accetta un parametro di tipo `HttpContext`:

```csharp
public async Task Invoke(HttpContext context)
```

`HttpContext` è stato modificato in modo significativo in ASP.NET Core. In questa sezione spiega come traslare le proprietà più comunemente utilizzate del [System.Web.HttpContext](/dotnet/api/system.web.httpcontext) al nuovo `Microsoft.AspNetCore.Http.HttpContext`.

### <a name="httpcontext"></a>HttpContext

**HttpContext. Items** si traduce in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

**ID univoco della richiesta (nessuna controparte System.Web.HttpContext)**

Fornisce un id univoco per ogni richiesta. Molto utili da includere nei log.

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a>HttpContext.Request

**HttpContext.Request.HttpMethod** si traduce in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

**HttpContext.Request.QueryString** translates to:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

**HttpContext.Request.Url** e **HttpContext.Request.RawUrl** Traduci da:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

**HttpContext.Request.IsSecureConnection** translates to:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

**HttpContext.Request.UserHostAddress** si traduce in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

**HttpContext.Request.Cookies** si traduce in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

**HttpContext.Request.RequestContext.RouteData** si traduce in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

**HttpContext.Request.Headers** si traduce in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

**HttpContext.Request.UserAgent** si traduce in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

**HttpContext.Request.UrlReferrer** translates to:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

**HttpContext.Request.ContentType** translates to:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

**HttpContext.Request.Form** si traduce in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> Leggere i valori del form solo se il tipo di contenuto sub *x-www-form-urlencoded* oppure *form-data*.

**HttpContext.Request.InputStream** translates to:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> Usare questo codice solo in un middleware di tipo gestore, alla fine di una pipeline.
>
>È possibile leggere il corpo non elaborato, come illustrato in precedenza una sola volta per ogni richiesta. Tentativo di leggere il corpo dopo la prima lettura middleware leggerà un corpo vuoto.
>
>Ciò non si applica alla lettura di un modulo come illustrato in precedenza, perché l'operazione viene eseguita da un buffer.

### <a name="httpcontextresponse"></a>HttpContext.Response

**HttpContext.Response.Status** e **HttpContext.Response.StatusDescription** Traduci da:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

**HttpContext.Response.ContentEncoding** e **HttpContext.Response.ContentType** Traduci da:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

**HttpContext.Response.ContentType** sul proprio anche si traduce in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

**HttpContext.Response.Output** si traduce in:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

**HttpContext.Response.TransmitFile**

Servizio backup di un file viene illustrata [qui](../fundamentals/request-features.md#middleware-and-request-features).

**HttpContext.Response.Headers**

L'invio delle intestazioni di risposta è ulteriormente complicata dal fatto che se si impostano dopo qualsiasi elemento è stato scritto nel corpo della risposta, non verrà inviati.

La soluzione consiste nell'impostare un metodo di callback che verrà chiamato a destra prima della scrittura nei avviata risposta. Questa operazione viene eseguita meglio all'inizio del `Invoke` metodo nel proprio middleware. È questo metodo di callback che consente di impostare le intestazioni della risposta.

Il codice seguente imposta un metodo di callback chiamato `SetHeaders`:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

Il `SetHeaders` metodo di callback sarà analogo al seguente:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

**HttpContext.Response.Cookies**

I cookie verranno trasmessi al browser in un *Set-Cookie* intestazione della risposta. Di conseguenza, l'invio di cookie richiede il callback stesso come quelle utilizzate per l'invio delle intestazioni di risposta:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

Il `SetCookies` metodo di callback sarebbe simile al seguente:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a>Risorse aggiuntive

* [Panoramica di moduli HTTP e gestori HTTP](/iis/configuration/system.webserver/)
* [Configurazione](xref:fundamentals/configuration/index)
* [Avvio dell'applicazione](xref:fundamentals/startup)
* [Middleware](xref:fundamentals/middleware/index)
