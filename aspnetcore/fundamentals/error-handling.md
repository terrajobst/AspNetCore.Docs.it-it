---
title: Gestire gli errori in ASP.NET Core
author: ardalis
description: Informazioni su come gestire gli errori nelle app ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/05/2018
uid: fundamentals/error-handling
ms.openlocfilehash: d1e94fdc89fbebc264dc001bbf35666af16f4799
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50208031"
---
# <a name="handle-errors-in-aspnet-core"></a>Gestire gli errori in ASP.NET Core

Di [Steve Smith](https://ardalis.com/) e [Tom Dykstra](https://github.com/tdykstra/)

Questo articolo descrive gli approcci comuni per la gestione degli errori nelle app ASP.NET Core.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="the-developer-exception-page"></a>Pagina delle eccezioni per gli sviluppatori

::: moniker range=">= aspnetcore-2.1"

Per configurare un'app in modo che sia visualizzata una pagina contenente informazioni dettagliate sulle eccezioni, usare la *pagina delle eccezioni per gli sviluppatori*. La pagina è disponibile nel pacchetto [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) che si trova nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Aggiungere una riga al metodo `Startup.Configure`:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Per configurare un'app in modo che sia visualizzata una pagina contenente informazioni dettagliate sulle eccezioni, usare la *pagina delle eccezioni per gli sviluppatori*. La pagina è disponibile nel pacchetto [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) che si trova nel [metapacchetto Microsoft.AspNetCore.All](xref:fundamentals/metapackage). Aggiungere una riga al metodo `Startup.Configure`:

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Per configurare un'app in modo che sia visualizzata una pagina contenente informazioni dettagliate sulle eccezioni, usare la *pagina delle eccezioni per gli sviluppatori*. La pagina diventa disponibile aggiungendo una riferimento al pacchetto [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) nel file di progetto. Aggiungere una riga al metodo `Startup.Configure`:

::: moniker-end

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

Posizionare la chiamata a [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) davanti al middleware in cui si vogliono rilevare le eccezioni, ad esempio `app.UseMvc`.

> [!WARNING]
> Abilitare la pagina delle eccezioni per gli sviluppatori **solo quando l'app è in esecuzione nell'ambiente di sviluppo**. per non condividere pubblicamente le informazioni dettagliate sulle eccezioni quando l'app è in esecuzione nell'ambiente di produzione. [Altre informazioni sulla configurazione degli ambienti](xref:fundamentals/environments).

Per visualizzare la pagina delle eccezioni per gli sviluppatori, eseguire l'app di esempio con l'ambiente impostato su `Development` e aggiungere `?throw=true` all'URL di base dell'app. La pagina include diverse schede con informazioni sull'eccezione e sulla richiesta. La prima scheda include un'analisi dello stack:

![Analisi dello stack](error-handling/_static/developer-exception-page.png)

La scheda successiva visualizza i parametri della stringa di query, se presenti:

![Parametri della stringa di query](error-handling/_static/developer-exception-page-query.png)

Se la richiesta contiene cookie, verranno visualizzati nella scheda **Cookie**. Le intestazioni vengono visualizzate nell'ultima scheda:

![Intestazioni](error-handling/_static/developer-exception-page-headers.png)

## <a name="configure-a-custom-exception-handling-page"></a>Configurare una pagina di gestione delle eccezioni personalizzata

Configurare una pagina di gestione delle eccezioni da usare quando l'app non è in esecuzione nell'ambiente `Development`:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

In un'app Razor Pages il modello Razor Pages [dotnet new](/dotnet/core/tools/dotnet-new) offre una pagina Errore e una classe di errore `PageModel` nella cartella *Pages* (Pagine).

In un'app MVC non decorare il metodo dell'azione di gestione degli errori con attributi di metodo HTTP, ad esempio `HttpGet`. I verbi espliciti impediscono ad alcune richieste di raggiungere il metodo. Consentire l'accesso anonimo al metodo in modo che gli utenti non autenticati possano ricevere la visualizzazione degli errori.

Ad esempio, il metodo gestore errori seguente viene fornito dal modello MVC [dotnet new](/dotnet/core/tools/dotnet-new) e visualizzato nel controller Home:

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configure-status-code-pages"></a>Configurare le tabelle codici di stato

Per impostazione predefinita, un'app non offre una tabella codici di stato completa per i codici di stato HTTP, ad esempio *404 Non trovato*. Per specificare le tabelle codici di stato, usare il middleware delle tabelle codici di stato.

::: moniker range=">= aspnetcore-2.1"

Il middleware è disponibile nel pacchetto [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) che si trova nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Il middleware è disponibile nel pacchetto [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) che si trova nel [metapacchetto Microsoft.AspNetCore.All](xref:fundamentals/metapackage).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Il middleware diventa disponibile aggiungendo un riferimento al pacchetto [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) nel file di progetto.

::: moniker-end

Aggiungere una riga al metodo `Startup.Configure`:

```csharp
app.UseStatusCodePages();
```

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> deve essere chiamato prima dei middleware di gestione richiesta nella pipeline (ad esempio, middleware dei file statici e middleware MVC).

Per impostazione predefinita, il middleware delle pagine dei codici di stato aggiunge gestori di solo testo per i codici di stato comuni, ad esempio 404:

![Pagina 404](error-handling/_static/default-404-status-code.png)

Il middleware supporta vari metodi di estensione. Un metodo accetta un'espressione lambda:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

Un altro metodo accetta un tipo di contenuto e una stringa di formato:

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

Sono disponibili anche metodi di estensione di reindirizzamento e ripetizione dell'esecuzione. Il metodo di reindirizzamento invia un codice di stato *302 - Trovato* al client e reindirizza il client al modello di URL di posizione specificato. Il modello può includere un segnaposto `{0}` per il codice di stato. Gli URL che iniziano con `~` vengono fatti precedere dal percorso di base. Un URL che non inizia con `~` viene usato così com'è.

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

Il metodo di riesecuzione restituisce il codice di stato originale al client e specifica che il corpo della risposta deve essere generato rieseguendo la pipeline della richiesta e usando un percorso alternativo. Questo percorso può contenere un segnaposto `{0}` per il codice di stato:

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

Le tabelle codici di stato possono essere disabilitate per richieste specifiche in un metodo del gestore Razor Pages o in un controller MVC. Per disabilitare le tabelle codici di stato, provare a recuperare [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) dalla raccolta [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) della richiesta e disabilitare la funzionalità, se disponibile:

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

Se si usa un overload `UseStatusCodePages*` che punta a un endpoint all'interno dell'app, creare una visualizzazione MVC o una pagina Razor per l'endpoint. Ad esempio, il modello [dotnet new](/dotnet/core/tools/dotnet-new) per un'app Razor Pages produce la pagina e la classe di modelli di pagina seguenti:

*Error.cshtml*:

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

## <a name="exception-handling-code"></a>Codice di gestione delle eccezioni

Il codice in pagine di gestione delle eccezioni può generare eccezioni. È spesso consigliabile che le pagine di errore di produzione contengano esclusivamente contenuto statico.

Tenere anche presente che dopo che le intestazioni per una risposta sono state inviate, non è possibile modificare il codice di stato della risposta né eseguire pagine delle eccezioni o gestori. La risposta deve essere completata o la connessione deve essere interrotta.

## <a name="server-exception-handling"></a>Gestione delle eccezioni del server

Oltre alla logica di gestione delle eccezioni nell'app, il [server](xref:fundamentals/servers/index) che ospita l'app esegue una parte della gestione delle eccezioni. Se rileva un'eccezione prima dell'invio delle intestazioni, il server invia una risposta *500 Errore interno del server* senza corpo. Se il server rileva un'eccezione dopo l'invio delle intestazioni, il server chiude la connessione. Le richieste che non sono gestite dall'app vengono gestite dal server. Tutte le eccezioni vengono gestite dalla gestione delle eccezioni del server. Eventuali pagine di errore personalizzate, middleware di gestione delle eccezioni o filtri configurati non hanno effetto su questo comportamento.

## <a name="startup-exception-handling"></a>Gestione delle eccezioni durante l'avvio

Solo il livello di hosting può gestire le eccezioni che si verificano durante l'avvio dell'app. Con [Host Web](xref:fundamentals/host/web-host) è possibile [configurare il comportamento dell'host in risposta agli errori durante l'avvio](xref:fundamentals/host/web-host#detailed-errors) usando `captureStartupErrors` e le chiavi `detailedErrors`.

L'hosting può visualizzare solo una pagina di errore per un errore di avvio acquisito se l'errore si verifica dopo l'associazione indirizzo host/porta. Se un'associazione ha esito negativo per qualsiasi ragione, il livello di hosting registra un'eccezione critica, il processo dotnet viene interrotto e non viene visualizzata alcuna pagina di errore quando l'app è in esecuzione nel server [Kestrel](xref:fundamentals/servers/kestrel).

Se durante l'esecuzione su [IIS](/iis) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) non è possibile avviare il processo, viene restituito un codice *502.5 Errore del processo* dal [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). Per informazioni sulla risoluzione dei problemi di avvio durante l'hosting con IIS, vedere <xref:host-and-deploy/iis/troubleshoot>. Per informazioni sulla risoluzione dei problemi di avvio con il servizio app di Azure, vedere <xref:host-and-deploy/azure-apps/troubleshoot>.

## <a name="aspnet-core-mvc-error-handling"></a>Gestione degli errori di ASP.NET Core MVC

Le app [MVC](xref:mvc/overview) includono alcune opzioni aggiuntive per la gestione degli errori, ad esempio la configurazione di filtri delle eccezioni e l'esecuzione della convalida del modello.

### <a name="exception-filters"></a>Filtri eccezioni

I filtri delle eccezioni possono essere configurati a livello globale o per singolo controller o azione in un'app MVC. Questi filtri gestiscono tutte le eccezioni non gestite che si verificano durante l'esecuzione di un'azione del controller o di un altro filtro e non vengono chiamati in altro modo. Per altre informazioni, vedere [Filtri](xref:mvc/controllers/filters).

> [!TIP]
> I filtri delle eccezioni sono utili per intercettare le eccezioni che si verificano all'interno di azioni MV, ma non sono flessibili come il middleware di gestione degli errori. In genere è preferibile usare il middleware. Usare invece i filtri solo quando è necessario gestire gli errori *in modo diverso* in base all'azione MVC scelta.

### <a name="handling-model-state-errors"></a>Gestione degli errori dello stato del modello

La [convalida del modello](xref:mvc/models/validation) avviene prima di richiamare ogni azione del controller ed è responsabilità del metodo di azione controllare `ModelState.IsValid` e rispondere in modo appropriato.

Alcune app scelgono di seguire una convenzione standard per gestire gli errori di convalida del modello. In tal caso un [filtro](xref:mvc/controllers/filters) può essere l'elemento adatto in cui implementare i criteri. È consigliabile testare il comportamento delle azioni con gli stati del modello non validi. Altre informazioni sono disponibili in [Test della logica dei controller](xref:mvc/controllers/testing).

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
