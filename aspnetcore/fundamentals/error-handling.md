---
title: Gestire gli errori in ASP.NET Core
author: tdykstra
description: Informazioni su come gestire gli errori nelle app ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/05/2019
uid: fundamentals/error-handling
ms.openlocfilehash: d809c70b3fae6b2d21d5ec0871298d905b873d5d
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665363"
---
# <a name="handle-errors-in-aspnet-core"></a>Gestire gli errori in ASP.NET Core

Di [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex) e [Steve Smith](https://ardalis.com/)

Questo articolo descrive gli approcci comuni per la gestione degli errori nelle app ASP.NET Core.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="developer-exception-page"></a>Pagina delle eccezioni per gli sviluppatori

Per configurare un'app in modo da visualizzare una pagina contenente informazioni dettagliate sulle eccezioni delle richieste, usare la *pagina delle eccezioni per gli sviluppatori*. La pagina è disponibile nel pacchetto [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) che si trova nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Aggiungere una riga al metodo `Startup.Configure` quando l'app è in esecuzione nell'[ambiente](xref:fundamentals/environments) di sviluppo:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseDeveloperExceptionPage)]

Posizionare la chiamata a <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> davanti al middleware in cui si vogliono rilevare le eccezioni.

> [!WARNING]
> Abilitare la pagina delle eccezioni per gli sviluppatori **solo quando l'app è in esecuzione nell'ambiente di sviluppo**. per non condividere pubblicamente le informazioni dettagliate sulle eccezioni quando l'app è in esecuzione nell'ambiente di produzione. Per altre informazioni sulla configurazione di ambienti, vedere <xref:fundamentals/environments>.

Per visualizzare la pagina delle eccezioni per gli sviluppatori, eseguire l'app di esempio con l'ambiente impostato su `Development` e aggiungere `?throw=true` all'URL di base dell'app. La pagina include le informazioni seguenti sull'eccezione e sulla richiesta:

* Analisi dello stack
* Parametri della stringa di query (se presenti)
* Cookie (se presenti)
* Intestazioni

## <a name="configure-a-custom-exception-handling-page"></a>Configurare una pagina di gestione delle eccezioni personalizzata

Quando l'app non è in esecuzione nell'ambiente di sviluppo, chiamare il metodo di estensione <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> per aggiungere il middleware di gestione delle eccezioni. Il middleware:

* Intercetta le eccezioni.
* Registra le eccezioni.
* Esegue nuovamente la richiesta in una pipeline alternativa per la pagina o il controller indicati. La richiesta non viene eseguita nuovamente se la risposta è stata avviata.

Nell'esempio seguente dall'app di esempio, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> aggiunge il middleware di gestione delle eccezioni in ambienti non di sviluppo. Il metodo di estensione specifica una pagina di errore o un controller nell'endpoint `/Error` per le richieste rieseguite dopo l'intercettazione e la registrazione delle eccezioni:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseExceptionHandler1)]

Il modello di app Razor Pages fornisce una pagina di errore (*.cshtml*) e una classe <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> (`ErrorModel`) nella cartella Pages.

In un'app MVC, il metodo del gestore errori seguente è incluso nel modello di app MVC e viene visualizzato nel controller Home:

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

Non decorare il metodo dell'azione di gestione degli errori con attributi di metodo HTTP, ad esempio `HttpGet`. I verbi espliciti impediscono ad alcune richieste di raggiungere il metodo. Consentire l'accesso anonimo al metodo in modo che gli utenti non autenticati possano ricevere la visualizzazione degli errori.

## <a name="access-the-exception"></a>Accedere all'eccezione

Usare <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> per accedere all'eccezione o al percorso della richiesta originale in un controller o in una pagina:

* Il percorso è disponibile dalla proprietà <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature.Path>.
* Leggere <xref:System.Exception?displayProperty=fullName> dalla proprietà [IExceptionHandlerFeature.Error](xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature.Error) ereditata.

```csharp
// using Microsoft.AspNetCore.Diagnostics;

var exceptionHandlerPathFeature = 
    HttpContext.Features.Get<IExceptionHandlerPathFeature>();
var path = exceptionHandlerPathFeature?.Path;
var error = exceptionHandlerPathFeature?.Error;
```

> [!WARNING]
> **Non** fornire informazioni sugli errori sensibili da <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> o <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> ai client. Fornire informazioni sugli errori rappresenta un rischio di sicurezza.

## <a name="configure-custom-exception-handling-code"></a>Configurare codice di gestione delle eccezioni personalizzato

Un'alternativa al fornire un endpoint per gli errori con una [pagina di gestione delle eccezioni personalizzata](#configure-a-custom-exception-handling-page) consiste nel fornire un funzione lambda a <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>. L'uso di una funzione lambda con <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> consente l'accesso all'errore prima di restituire la risposta.

L'app di esempio illustra il codice di gestione delle eccezioni personalizzato in `Startup.Configure`. Generare un'eccezione tramite il collegamento **Throw Exception** (Genera eccezione) nella pagina Index. Viene eseguita la funzione lambda seguente:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseExceptionHandler2)]

> [!WARNING]
> **Non** fornire informazioni sugli errori sensibili da <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> o <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> ai client. Fornire informazioni sugli errori rappresenta un rischio di sicurezza.

## <a name="configure-status-code-pages"></a>Configurare le tabelle codici di stato

Per impostazione predefinita, un'app ASP.NET Core non offre una tabella codici di stato per i codici di stato HTTP, ad esempio *404 - Non trovato*. L'app restituisce un codice di stato e un corpo della risposta vuoto. Per specificare le tabelle codici di stato, usare il middleware delle tabelle codici di stato.

Il middleware è disponibile nel pacchetto [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) che si trova nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Aggiungere una riga al metodo `Startup.Configure`:

```csharp
app.UseStatusCodePages();
```

Chiamare il metodo <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> prima del middleware di gestione delle richieste (ad esempio, middleware dei file statici e middleware MVC).

Per impostazione predefinita, il middleware delle pagine dei codici di stato aggiunge gestori di solo testo per i codici di stato comuni, ad esempio *404 - Non trovato*:

```
Status Code: 404; Not Found
```

Il middleware supporta diversi metodi di estensione che consentono di personalizzarne il comportamento.

Un overload di <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> accetta un'espressione lambda, che è possibile usare per elaborare la logica di gestione degli errori personalizzata e per scrivere manualmente la risposta:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

Un overload di <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> accetta un tipo di contenuto e una stringa di formato, utilizzabili per personalizzare il tipo di contenuto e il testo della risposta:

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

### <a name="redirect-and-re-execute-extension-methods"></a>Reindirizzare e rieseguire i metodi di estensione

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:

* Invia un codice di stato *302 - Trovato* al client.
* Reindirizza il client al percorso specificato nel modello di URL.

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> viene in genere usato quando l'app:

* Deve reindirizzare il client a un endpoint diverso, in genere nei casi in cui un'altra app elabora l'errore. Per le app Web, la barra degli indirizzi del browser del client riflette l'endpoint reindirizzato.
* Non deve conservare e restituire il codice di stato originale con la risposta di reindirizzamento iniziale.

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:

* Restituisce il codice di stato originale al client.
* Genera il corpo della risposta eseguendo nuovamente la pipeline delle richieste con un percorso alternativo.

```csharp
app.UseStatusCodePagesWithReExecute("/Error/{0}");
```

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> viene in genere usato quando l'app deve:

* Elaborare la richiesta senza il reindirizzamento a un endpoint diverso. Per le app Web, la barra degli indirizzi del browser del client riflette l'endpoint richiesto in origine.
* Conservare e restituire il codice di stato originale con la risposta.

I modelli possono includere un segnaposto (`{0}`) per il codice di stato. Il modello deve iniziare con una barra rovesciata (`/`). Quando si usa un segnaposto, verificare che l'endpoint (pagina o controller) possa elaborare il segmento del percorso. Ad esempio, una pagina Razor per gli errori deve accettare il valore del segmento di percorso facoltativo con la direttiva `@page`:

```cshtml
@page "{code?}"
```

Le tabelle codici di stato possono essere disabilitate per richieste specifiche in un metodo del gestore Razor Pages o in un controller MVC. Per disabilitare le tabelle codici di stato, provare a recuperare <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> dalla raccolta [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) della richiesta e disabilitare la funzionalità, se disponibile:

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

Per usare un overload <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> che punta a un endpoint all'interno dell'app, creare una visualizzazione MVC o una pagina Razor per l'endpoint. Ad esempio, il modello di app Razor Pages produce la pagina e la classe di modelli di pagina seguenti:

*Error.cshtml*:

::: moniker range=">= aspnetcore-2.2"

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to the <strong>Development</strong> environment displays 
    detailed information about the error that occurred.
</p>
<p>
    <strong>The Development environment shouldn't be enabled for deployed 
    applications.</strong> It can result in displaying sensitive information 
    from exceptions to end users. For local debugging, enable the 
    <strong>Development</strong> environment by setting the 
    <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to 
    <strong>Development</strong> and restarting the app.
</p>
```

*Error.cshtml.cs*:

```csharp
[ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to <strong>Development</strong> environment will display more detailed 
    information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications
    </strong>, as it can result in sensitive information from exceptions being 
    displayed to end users. For local debugging, development environment can be 
    enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment 
    variable to <strong>Development</strong>, and restarting the application.
</p>
```

*Error.cshtml.cs*:

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, 
        NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

## <a name="exception-handling-code"></a>Codice di gestione delle eccezioni

Il codice in pagine di gestione delle eccezioni può generare eccezioni. È spesso consigliabile che le pagine di errore di produzione contengano esclusivamente contenuto statico.

Inoltre, tenere presente che dopo l'invio delle intestazioni di risposta:

* L'app non può modificare il codice di stato della risposta.
* Non è possibile eseguire pagine o gestori delle eccezioni. La risposta deve essere completata o la connessione deve essere interrotta.

## <a name="server-exception-handling"></a>Gestione delle eccezioni del server

Oltre alla logica di gestione delle eccezioni nell'app, l'[implementazione del server](xref:fundamentals/servers/index) può gestire alcune eccezioni. Se rileva un'eccezione prima dell'invio delle intestazioni di risposta, il server invia una risposta *500 - Errore interno del server* senza corpo. Se il server rileva un'eccezione dopo l'invio delle intestazioni di risposta, il server chiude la connessione. Le richieste che non sono gestite dall'app vengono gestite dal server. Qualsiasi eccezione che si verifica quando il server sta gestendo la richiesta viene gestita dalla gestione delle eccezioni del server. Eventuali pagine di errore personalizzate dell'app, middleware di gestione delle eccezioni e filtri non hanno effetto su questo comportamento.

## <a name="startup-exception-handling"></a>Gestione delle eccezioni durante l'avvio

Solo il livello di hosting può gestire le eccezioni che si verificano durante l'avvio dell'app. Con [Host Web](xref:fundamentals/host/web-host) è possibile [configurare il comportamento dell'host in risposta agli errori durante l'avvio](xref:fundamentals/host/web-host#detailed-errors) usando `captureStartupErrors` e le chiavi `detailedErrors`.

L'hosting può visualizzare solo una pagina di errore per un errore di avvio acquisito se l'errore si verifica dopo l'associazione indirizzo host/porta. Se qualsiasi associazione non riesce per qualsiasi motivo:

* Il livello di hosting registra un'eccezione critica.
* Si verifica un arresto anomalo del processo di dotnet.
* Non viene visualizzata alcuna pagina di errore quando l'app è in esecuzione nel server [Kestrel](xref:fundamentals/servers/kestrel).

Se durante l'esecuzione su [IIS](/iis) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) il processo non può essere avviato, viene restituito un codice *502.5 Errore del processo* dal [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module). Per ulteriori informazioni, vedere <xref:host-and-deploy/iis/troubleshoot>. Per informazioni sulla risoluzione dei problemi di avvio con il servizio app di Azure, vedere <xref:host-and-deploy/azure-apps/troubleshoot>.

## <a name="aspnet-core-mvc-error-handling"></a>Gestione degli errori di ASP.NET Core MVC

Le app [MVC](xref:mvc/overview) includono alcune opzioni aggiuntive per la gestione degli errori, ad esempio la configurazione di filtri delle eccezioni e l'esecuzione della convalida del modello.

### <a name="exception-filters"></a>Filtri eccezioni

I filtri delle eccezioni possono essere configurati a livello globale o per singolo controller o azione in un'app MVC. Questi filtri gestiscono tutte le eccezioni non gestite che si verificano durante l'esecuzione di un'azione del controller o di un altro filtro e non vengono chiamati in altro modo. Per ulteriori informazioni, vedere <xref:mvc/controllers/filters#exception-filters>.

> [!TIP]
> I filtri delle eccezioni sono utili per intercettare le eccezioni che si verificano all'interno di azioni MVC, ma non sono flessibili come il middleware di gestione degli errori. È consigliabile usare il middleware. Usare i filtri solo quando è necessario eseguire la gestione degli errori *in modo diverso* in base all'azione MVC scelta.

### <a name="handle-model-state-errors"></a>Gestire gli errori di stato del modello

La [convalida del modello](xref:mvc/models/validation) avviene prima di richiamare ogni azione del controller ed è responsabilità del metodo di azione controllare [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) e rispondere in modo appropriato.

Alcune app scelgono di seguire una convenzione standard per gestire gli errori di [convalida del modello](xref:mvc/models/validation). In tal caso un [filtro](xref:mvc/controllers/filters) può essere l'elemento adatto in cui implementare i criteri. È consigliabile testare il comportamento delle azioni con gli stati del modello non validi. Per ulteriori informazioni, vedere <xref:mvc/controllers/testing>.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
