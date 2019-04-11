---
title: Esaminare i metodi Details e Delete di un'app ASP.NET Core
author: rick-anderson
description: Informazioni sul metodo e sulla vista del controller Details in un'app ASP.NET Core MVC di base.
ms.author: riande
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: 056017ea4f4073fa0b1cd747d06775b2a33616cf
ms.sourcegitcommit: 1a7000630e55da90da19b284e1b2f2f13a393d74
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/04/2019
ms.locfileid: "59012669"
---
# <a name="examine-the-details-and-delete-methods-of-an-aspnet-core-app"></a>Esaminare i metodi Details e Delete di un'app ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Aprire il controller di film ed esaminare il metodo `Details`:

[!code-csharp[](start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_details)]

Il motore di scaffolding MVC che ha creato questo metodo di azione aggiunge un commento che mostra una richiesta HTTP che richiama il metodo. In questo caso si tratta di una richiesta GET con tre segmenti di URL, il controller `Movies`, il metodo `Details` e un valore `id`. Tenere presente che questi segmenti vengono definiti nel file *Startup.cs*.

[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

Entity Framework semplifica la ricerca di dati usando il metodo `FirstOrDefaultAsync`. Un'importante funzionalità di sicurezza integrata nel metodo è che il codice verifica che il metodo di ricerca abbia trovato un film prima di tentare di eseguire una qualsiasi operazione su di esso. Ad esempio, un hacker potrebbe introdurre errori nel sito modificando l'URL creato dai collegamenti di `http://localhost:xxxx/Movies/Details/1` in qualcosa di simile a `http://localhost:xxxx/Movies/Details/12345` o usando altri valori che non rappresentano un film effettivo. Se non è stato cercato un film con valore Null, l'app genera un'eccezione.

Esaminare i metodi `Delete` e `DeleteConfirmed`.

[!code-csharp[](start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_delete)]

Si noti che il metodo `HTTP GET Delete` non elimina il film specificato, ma restituisce una vista del film in cui è possibile inviare (HttpPost) la richiesta di eliminazione. L'esecuzione di un'operazione di eliminazione in risposta a una richiesta GET (o l'esecuzione di un'operazione di modifica, di creazione o di qualsiasi altra azione che modifica i dati) introduce un problema di sicurezza.

Il metodo `[HttpPost]` che elimina i dati è denominato `DeleteConfirmed` per fornire al metodo HTTP POST un nome o una firma univoca. Le firme dei due metodi sono illustrate di seguito:

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]

Il Common Language Runtime (CLR) richiede metodi in rapporto di overload per disporre di una firma di parametro univoca, ovvero lo stesso nome di metodo ma un elenco diverso di parametri. Tuttavia, qui è necessario usare due `Delete` metodi: uno per GET e uno per POST, entrambi con la stessa firma di parametro. Entrambi devono accettare un singolo intero come parametro.

Per risolvere questo problema esistono due approcci. Una possibilità è assegnare nomi diversi ai metodi. Questa operazione è stata eseguita dal meccanismo di scaffolding nell'esempio precedente. In questo modo, tuttavia, si introduce un piccolo problema: ASP.NET esegue il mapping dei segmenti di un URL ai metodi di azione in base al nome e, se si rinomina un metodo, in genere il routing non è in grado di trovarlo. La soluzione è mostrata nell'esempio e consiste nell'aggiungere l'attributo `ActionName("Delete")` al metodo `DeleteConfirmed`. Questo attributo esegue il mapping per il sistema di routing in modo che un URL che includa /Delete/ per una richiesta POST troverà il metodo `DeleteConfirmed`.

Un'altra alternativa comune per i metodi con nomi e firme identici consiste nel modificare artificialmente la firma del metodo POST in modo da includere un parametro aggiuntivo (non usato). Questa azione è stata eseguita in un post precedente quando è stato aggiunto il parametro `notUsed`. È possibile eseguire la stessa operazione qui per il metodo `[HttpPost] Delete`:

```csharp
// POST: Movies/Delete/6
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a>Pubblicare in Azure

Per informazioni sulla distribuzione in Azure, vedere [Esercitazione: Creare un'app Web .NET Core e database SQL nel Servizio app di Azure](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb).

> [!div class="step-by-step"]
> [Precedente](validation.md)
