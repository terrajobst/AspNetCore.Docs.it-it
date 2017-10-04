---
title: Analisi dei dettagli e dei metodi di eliminazione
author: rick-anderson
description: Metodo e vista del controller Details in una semplice app ASP.NET Core MVC.
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: 870192b4-8d4f-45c7-8c14-83d02bc0ad79
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: 28ed7a7a56415d7eb675c06353fb9a8f65fb571f
ms.sourcegitcommit: c9658c0db446f7cb2e443f62b00cf773bed545fa
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/30/2017
---
# <a name="examining-the-details-and-delete-methods"></a>Analisi dei dettagli e dei metodi di eliminazione

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Aprire il controller di film ed esaminare il metodo `Details`:

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

Il motore di scaffolding MVC che ha creato questo metodo di azione aggiunge un commento che mostra una richiesta HTTP che richiama il metodo. In questo caso si tratta di una richiesta GET con tre segmenti di URL, il controller `Movies`, il metodo `Details` e un valore `id`. Tenere presente che questi segmenti vengono definiti nel file *Startup.cs*.

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

Entity Framework semplifica la ricerca di dati usando il metodo `SingleOrDefaultAsync`. Un'importante funzionalità di sicurezza integrata nel metodo è che il codice verifica che il metodo di ricerca abbia trovato un film prima di tentare di eseguire una qualsiasi operazione su di esso. Ad esempio, un hacker potrebbe introdurre errori nel sito modificando l'URL creato dai collegamenti di `http://localhost:xxxx/Movies/Details/1` in qualcosa di simile a `http://localhost:xxxx/Movies/Details/12345` o usando altri valori che non rappresentano un film effettivo. Se non è stato cercato un film con valore null, l'app genera un'eccezione.

Esaminare i metodi `Delete` e `DeleteConfirmed`.

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]

Si noti che il metodo `HTTP GET Delete` non elimina il film specificato, ma restituisce una vista del film in cui è possibile inviare (HttpPost) la richiesta di eliminazione. L'esecuzione di un'operazione di eliminazione in risposta a una richiesta GET (o l'esecuzione di un'operazione di modifica, di creazione o di qualsiasi altra azione che modifica i dati) introduce un problema di sicurezza.

Il metodo `[HttpPost]` che elimina i dati è denominato `DeleteConfirmed` per fornire al metodo HTTP POST un nome o una firma univoca. Le firme dei due metodi sono illustrate di seguito:

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


Il Common Language Runtime (CLR) richiede metodi in rapporto di overload per disporre di una firma di parametro univoca, ovvero lo stesso nome di metodo ma un elenco diverso di parametri. Tuttavia, qui è necessario usare due `Delete` metodi: uno per GET e uno per POST, entrambi con la stessa firma di parametro. Entrambi devono accettare un singolo intero come parametro.

Per risolvere questo problema esistono due approcci. Una possibilità è assegnare nomi diversi ai metodi. Questa operazione è stata eseguita dal meccanismo di scaffolding nell'esempio precedente. Tuttavia in questo modo si introduce un piccolo problema: ASP.NET esegue il mapping dei segmenti di un URL ai metodi di azione in base al nome e, se si rinomina un metodo, generalmente il routing non è in grado di trovare questo metodo. La soluzione è mostrata nell'esempio e consiste nell'aggiungere l'attributo `ActionName("Delete")` al metodo `DeleteConfirmed`. Questo attributo esegue il mapping per il sistema di routing in modo che un URL che includa /Delete/ per una richiesta POST troverà il metodo `DeleteConfirmed`.

Un'altra alternativa comune per i metodi con nomi e firme identici consiste nel modificare artificialmente la firma del metodo POST in modo da includere un parametro aggiuntivo (non usato). Questa azione è stata eseguita in un post precedente quando è stato aggiunto il parametro `notUsed`. È possibile eseguire la stessa operazione qui per il metodo `[HttpPost] Delete`:

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a>Pubblicare in Azure

Per istruzioni su come pubblicare l'app in Azure, vedere [Pubblicare un'app Web ASP.NET Core in Servizio app di Azure con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).

Grazie aver completato questa introduzione ad ASP.NET Core MVC. Si apprezza l'invio di commenti. [Getting started with MVC and EF Core](xref:data/ef-mvc/intro) (Introduzione a MVC ed Entity Framework Core) è un ottimo completamento per questa esercitazione.

>[!div class="step-by-step"]
[Precedente](validation.md)
