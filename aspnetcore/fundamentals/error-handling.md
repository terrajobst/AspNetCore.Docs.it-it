---
title: Gestire gli errori in ASP.NET Core
author: ardalis
description: Informazioni su come gestire gli errori nelle applicazioni ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/error-handling
ms.openlocfilehash: 3ff3a17d14d9ed7c438399191ffe3cf93d555d49
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/17/2018
---
# <a name="handle-errors-in-aspnet-core"></a>Gestire gli errori in ASP.NET Core

Di [Steve Smith](https://ardalis.com/) e [Tom Dykstra](https://github.com/tdykstra/)

Questo articolo descrive gli approcci comuni per la gestione degli errori nelle app ASP.NET Core.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="the-developer-exception-page"></a>Pagina delle eccezioni per gli sviluppatori

Per configurare un'app in modo che visualizzi una pagina contenente informazioni dettagliate sulle eccezioni, installare il pacchetto NuGet `Microsoft.AspNetCore.Diagnostics` e aggiungere una riga al [metodo Configure della classe Startup](xref:fundamentals/startup):

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

Inserire `UseDeveloperExceptionPage` prima di qualsiasi middleware in cui si desidera rilevare le eccezioni, ad esempio `app.UseMvc`.

>[!WARNING]
> Abilitare la pagina delle eccezioni per gli sviluppatori **solo quando l'app è in esecuzione nell'ambiente di sviluppo** per non condividere pubblicamente le informazioni dettagliate sulle eccezioni quando l'app è in esecuzione nell'ambiente di produzione. [Altre informazioni sulla configurazione degli ambienti](xref:fundamentals/environments).

Per visualizzare la pagina delle eccezioni per gli sviluppatori, eseguire l'applicazione di esempio con l'ambiente impostato su `Development` e aggiungere `?throw=true` all'URL di base dell'app. La pagina include diverse schede con informazioni sull'eccezione e sulla richiesta. La prima scheda include un'analisi dello stack. 

![Analisi dello stack](error-handling/_static/developer-exception-page.png)

La scheda successiva visualizza i parametri della stringa di query, se presente.

![Parametri della stringa di query](error-handling/_static/developer-exception-page-query.png)

Questa richiesta non include cookie. Se li includesse, verrebbero visualizzati nella scheda **Cookie**. È possibile visualizzare le intestazioni passate nell'ultima scheda.

![Intestazioni](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>Configurazione di una pagina di gestione delle eccezioni personalizzata

Può essere utile configurare una pagina di gestione delle eccezioni da usare quando l'app non è in esecuzione nell'ambiente `Development`.

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

In un'app MVC non decorare esplicitamente il metodo dell'azione di gestione degli errori con attributi di metodo HTTP, ad esempio `HttpGet`. L'uso di verbi espliciti può impedire ad alcune richieste di raggiungere il metodo.

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a>Configurazione delle tabelle codici di stato

Per impostazione predefinita, un'app non offre una tabella codici di stato completa per i codici di stato HTTP, ad esempio *404 Non trovato*. Per includere tabelle codici di stato, configurare il middleware delle tabelle codici di stato aggiungendo una riga al metodo `Startup.Configure`:

```csharp
app.UseStatusCodePages();
```

Per impostazione predefinita, il middleware delle tabelle codici di stato aggiunge gestori di solo testo semplici per i codici di stato comuni, ad esempio 404:

![Pagina 404](error-handling/_static/default-404-status-code.png)

Il middleware supporta vari metodi di estensione. Un metodo accetta un'espressione lambda:

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

Un altro metodo accetta un tipo di contenuto e una stringa di formato:

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

Sono disponibili anche metodi di estensione di reindirizzamento e ripetizione dell'esecuzione. Il metodo di reindirizzamento invia un codice di stato 302 al client:

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

Il metodo di ripetizione dell'esecuzione restituisce il codice di stato originale al client, ma esegue anche il gestore per l'URL di reindirizzamento:

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

## <a name="exception-handling-code"></a>Codice di gestione delle eccezioni

Il codice in pagine di gestione delle eccezioni può generare eccezioni. È spesso consigliabile che le pagine di errore di produzione contengano esclusivamente contenuto statico.

Tenere anche presente che dopo che le intestazioni per una risposta sono state inviate, non è possibile modificare il codice di stato della risposta né eseguire pagine delle eccezioni o gestori. La risposta deve essere completata o la connessione deve essere interrotta.

## <a name="server-exception-handling"></a>Gestione delle eccezioni del server

Oltre alla logica di gestione delle eccezioni nell'app, il [server](xref:fundamentals/servers/index) che ospita l'app esegue una parte della gestione delle eccezioni. Se rileva un'eccezione prima dell'invio delle intestazioni, il server invia una risposta *500 Errore interno del server* senza corpo. Se il server rileva un'eccezione dopo l'invio delle intestazioni, il server chiude la connessione. Le richieste che non sono gestite dall'app vengono gestite dal server. Tutte le eccezioni vengono gestite dalla gestione delle eccezioni del server. Eventuali pagine di errore personalizzate, middleware di gestione delle eccezioni o filtri configurati non hanno effetto su questo comportamento.

## <a name="startup-exception-handling"></a>Gestione delle eccezioni durante l'avvio

Solo il livello di hosting può gestire le eccezioni che si verificano durante l'avvio dell'app. Con [Host Web](xref:fundamentals/host/web-host) è possibile [configurare il comportamento dell'host in risposta agli errori durante l'avvio](xref:fundamentals/host/web-host#detailed-errors) usando `captureStartupErrors` e le chiavi `detailedErrors`.

L'hosting può visualizzare solo una pagina di errore per un errore di avvio acquisito se l'errore si verifica dopo l'associazione indirizzo host/porta. Se un'associazione ha esito negativo per qualsiasi ragione, il livello di hosting registra un'eccezione critica, il processo dotnet viene interrotto e non viene visualizzata alcuna pagina di errore quando l'app è in esecuzione nel server [Kestrel](xref:fundamentals/servers/kestrel).

Se durante l'esecuzione su [IIS](/iis) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) non è possibile avviare il processo, viene restituito un codice *502.5 Errore del processo* dal [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). Seguire i consigli sulla risoluzione dei problemi in [Risolvere i problemi di ASP.NET Core in IIS](xref:host-and-deploy/iis/troubleshoot).

## <a name="aspnet-mvc-error-handling"></a>Gestione degli errori di ASP.NET MVC

Le app [MVC](xref:mvc/overview) includono alcune opzioni aggiuntive per la gestione degli errori, ad esempio la configurazione di filtri delle eccezioni e l'esecuzione della convalida del modello.

### <a name="exception-filters"></a>Filtri eccezioni

I filtri delle eccezioni possono essere configurati a livello globale o per singolo controller o azione in un'app MVC. Questi filtri gestiscono tutte le eccezioni non gestite che si verificano durante l'esecuzione di un'azione del controller o di un altro filtro e non vengono chiamate e non vengono chiamate in altro modo. Per altre informazioni sui filtri delle eccezioni, vedere [Filtri](xref:mvc/controllers/filters).

>[!TIP]
> I filtri delle eccezioni sono utili per intercettare le eccezioni che si verificano all'interno di azioni MV, ma non sono flessibili come il middleware di gestione degli errori. Per i casi generici è preferibile usare il middleware e usare invece i filtri solo quando è necessario gestire gli errori *in modo diverso* in base all'azione MVC scelta.

### <a name="handling-model-state-errors"></a>Gestione degli errori dello stato del modello

La [convalida del modello](xref:mvc/models/validation) avviene prima di richiamare ogni azione del controller ed è responsabilità del metodo di azione controllare `ModelState.IsValid` e rispondere in modo appropriato.

Alcune app scelgono di seguire una convenzione standard per gestire gli errori di convalida del modello. In tal caso un [filtro](xref:mvc/controllers/filters) può essere l'elemento adatto in cui implementare tali criteri. È consigliabile testare il comportamento delle azioni con gli stati del modello non validi. Altre informazioni sono disponibili in [Test della logica dei controller](xref:mvc/controllers/testing).



