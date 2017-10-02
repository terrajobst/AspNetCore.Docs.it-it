---
title: Gestione degli errori in ASP.NET Core
author: ardalis
description: Informazioni su come gestire gli errori nelle applicazioni ASP.NET Core.
keywords: ASP.NET Core, gestione degli errori, la gestione delle eccezioni
ms.author: tdykstra
manager: wpickett
ms.date: 11/30/2016
ms.topic: article
ms.assetid: 4db51023-c8a6-4119-bbbe-3917e272c260
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/error-handling
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: de2ba0ff9ad17c198c06b510ecfb49f808721bdf
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/01/2017
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a>Introduzione alla gestione degli errori in ASP.NET Core

Da [Steve Smith](https://ardalis.com/) e [Tom Dykstra](https://github.com/tdykstra/)

Questo articolo descrive appoaches comuni di gestione degli errori nelle applicazioni ASP.NET Core.

[Consente di visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([come scaricare](xref:tutorials/index#how-to-download-a-sample))

## <a name="the-developer-exception-page"></a>La pagina di eccezione per sviluppatori

Per configurare un'app per visualizzare una pagina che visualizza informazioni dettagliate sulle eccezioni, installare il `Microsoft.AspNetCore.Diagnostics` NuGet pacchetto e aggiungere una riga per il [configurare metodo nella classe di avvio](startup.md):

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

Inserire `UseDeveloperExceptionPage` prima middleware che si desidera intercettare le eccezioni, ad esempio `app.UseMvc`.

>[!WARNING]
> Abilitare la pagina di eccezione developer **solo quando l'app è in esecuzione nell'ambiente di sviluppo**. Non si desidera condividere informazioni dettagliate sulle eccezioni pubblicamente quando viene eseguita l'app nell'ambiente di produzione. [Ulteriori informazioni sulla configurazione di ambienti](environments.md).

Per visualizzare la pagina di eccezione per sviluppatori, eseguire l'applicazione di esempio con l'ambiente impostato su `Development`e aggiungere `?throw=true` all'URL di base dell'app. La pagina include diverse schede con le informazioni sull'eccezione e la richiesta. La prima scheda include un'analisi dello stack. 

![Analisi dello stack](error-handling/_static/developer-exception-page.png)

La scheda successiva mostra la query parametri stringa, se presente.

![Parametri di stringa di query](error-handling/_static/developer-exception-page-query.png)

Questa richiesta non dispone di tutti i cookie, ma in caso contrario, verranno visualizzate sul **cookie** scheda. È possibile visualizzare le intestazioni che sono state passate l'ultima scheda.

![Intestazioni](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>Configurazione di un'eccezione personalizzata la pagina di gestione

È consigliabile configurare una pagina di gestore di eccezioni da utilizzare quando l'app non è in esecuzione `Development` ambiente.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

In un'applicazione MVC, non in modo esplicito decorare il metodo di azione del gestore di errori con gli attributi del metodo HTTP, ad esempio `HttpGet`. Utilizzando i verbi espliciti potrebbe impedire alcune richieste raggiungano il metodo.

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a>Configurazione delle tabelle codici di stato

Per impostazione predefinita, l'applicazione non fornirà una tabella codici di stato per i codici di stato HTTP, ad esempio 500 (errore Server interno) o 404 (non trovato). È possibile configurare il `StatusCodePagesMiddleware` aggiungendo una riga per il `Configure` metodo:

```csharp
app.UseStatusCodePages();
```

Per impostazione predefinita, questo middleware aggiunge gestori di solo testo semplici per i codici di stato comuni, ad esempio 404:

![pagina 404](error-handling/_static/default-404-status-code.png)

Il middleware supporta diversi metodi di estensione differente. Uno accetta un'espressione lambda, un altro accetta una stringa di formato e tipo di contenuto.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

Sono disponibili metodi di estensione di reindirizzamento. Uno invia al client un codice di 302 stato e uno restituisce al client il codice di stato originale, ma esegue anche il gestore per l'URL di reindirizzamento.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

Se è necessario disabilitare i codici di stato per determinate richieste, è possibile farlo:

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a>Codice di gestione delle eccezioni

Codice in pagine di gestione delle eccezioni può generare eccezioni. È spesso consigliabile per le pagine di errore di produzione costituita da contenuto esclusivamente statico.

Inoltre, tenere presente che dopo le intestazioni di risposta sono state inviate, è possibile modificare il codice di stato della risposta, né le pagine di eccezione o gestori di eseguire. La risposta deve essere completata o interrotta la connessione.

## <a name="server-exception-handling"></a>Gestione delle eccezioni

Oltre alla logica dell'app, gestione delle eccezioni di [server](servers/index.md) ospita l'app esegue alcune eccezioni. Se il server rileva un'eccezione prima che le intestazioni vengono inviate, il server invia una risposta 500 Errore interno del Server senza il corpo. Se il server memorizza nella cache dopo l'invio delle intestazioni di un'eccezione, il server chiude la connessione. Le richieste che non sono gestite dalla tua app vengono gestite dal server. Qualsiasi eccezione che si verifica viene gestita dall'eccezione del server la gestione. Qualsiasi configurato pagine di errore personalizzate o middleware di gestione delle eccezioni o i filtri non influisce su tale comportamento.

## <a name="startup-exception-handling"></a>Gestione delle eccezioni di avvio

Il livello di hosting può gestire le eccezioni che si verificano durante l'avvio dell'app. È possibile [configurare il comportamento dell'host in risposta agli errori durante l'avvio](hosting.md#detailed-errors) utilizzando `captureStartupErrors` e `detailedErrors` chiave.

Hosting può mostrare solo una pagina di errore per un errore di avvio acquisita se l'errore si verifica dopo host binding di porta e indirizzo. Se un'associazione non riesce per qualsiasi motivo, il livello di hosting registra un'eccezione critica, l'arresto del processo dotnet, e non viene visualizzata alcuna pagina di errore.

## <a name="aspnet-mvc-error-handling"></a>Gestione degli errori di ASP.NET MVC

[MVC](../mvc/index.md) App disponibili alcune opzioni aggiuntive per la gestione degli errori, ad esempio la configurazione dei filtri di eccezione e l'esecuzione di convalida del modello.

### <a name="exception-filters"></a>Filtri eccezioni

I filtri eccezioni possono essere configurati a livello globale o in base al controller o per ogni azione in un'applicazione MVC. Questi filtri di gestire qualsiasi eccezione non gestita che si verifica durante l'esecuzione di un'azione del controller o un altro filtro e non vengono chiamati in caso contrario. Ulteriori informazioni sui filtri di eccezioni in [filtri](../mvc/controllers/filters.md).

>[!TIP]
> I filtri eccezioni sono ideali per l'intercettazione delle eccezioni che si verificano all'interno di azioni di MVC, ma non sono quanto più flessibili middleware di gestione degli errori. Preferiti middleware nel caso generale e utilizzare i filtri in cui è necessario solo per eseguire la gestione degli errori *in modo diverso* in base a quale azione MVC è stato scelto.

### <a name="handling-model-state-errors"></a>Lo stato del modello di gestione degli errori

[La convalida del modello](../mvc/models/validation.md) si verifica prima di ogni azione del controller richiamato ed è responsabilità del metodo di azione per controllare `ModelState.IsValid` e rispondere nel modo appropriato.

Alcune applicazioni è possibile scegliere di seguire una convenzione standard per la gestione di errori di convalida del modello, nel qual caso un [filtro](../mvc/controllers/filters.md) può essere una posizione appropriata per implementare questi criteri. È consigliabile testare il comportamento delle azioni con gli stati di modello non valido. Altre informazioni, vedere [logica del controller di test](../mvc/controllers/testing.md).



