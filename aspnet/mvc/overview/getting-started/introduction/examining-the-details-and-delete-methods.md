---
uid: mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
title: Esaminare i dettagli e i metodi Delete | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: f1d2a916-626c-4a54-8df4-77e6b9fff355
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 30fafd228dd43624cb2e6886fe1c1dda5bbcb60e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399490"
---
<a name="examining-the-details-and-delete-methods"></a>Esaminare i dettagli e i metodi di eliminazione
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

In questa parte dell'esercitazione, si esamineranno generato automaticamente `Details` e `Delete` metodi.

## <a name="examining-the-details-and-delete-methods"></a>Esaminare i dettagli e i metodi di eliminazione

Aprire il `Movie` controller ed esaminare il `Details` (metodo).

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

Il motore di scaffolding MVC che ha creato questo metodo di azione aggiunge un commento che mostra una richiesta HTTP che richiama il metodo. In questo caso è una `GET` richiesta con tre segmenti di URL, il `Movies` controller, il `Details` metodo e un `ID` valore.

Codice prima di tutto semplifica la ricerca di dati usando il `Find` (metodo). Una caratteristica importante della sicurezza integrata nel metodo è che il codice verifica che il `Find` metodo ha trovato un film prima che il codice prova a eseguire qualsiasi operazione con esso. Ad esempio, un hacker potrebbe introdurre errori nel sito modificando l'URL creato dai collegamenti di `http://localhost:xxxx/Movies/Details/1` su un valore come `http://localhost:xxxx/Movies/Details/12345` (o un altro valore che non rappresenta un film effettivo). Se non è stato cercato un film con valore null, un film con valore null restituirà un errore di database.

Esaminare i metodi `Delete` e `DeleteConfirmed`.

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

Si noti che HTTP GET `Delete` metodo non viene eliminato il film specificato, restituisce una visualizzazione del film in cui è possibile inviare (`HttpPost`) l'eliminazione. L'esecuzione di un'operazione di eliminazione in risposta a una richiesta GET (o l'esecuzione di un'operazione di modifica, di creazione o di qualsiasi altra azione che modifica i dati) introduce un problema di sicurezza. Per altre informazioni, vedere l'intervento nel blog di Stephen Walther [46 & suggerimento di ASP.NET MVC, non usare i collegamenti di eliminazione poiché creano le falle](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

Il metodo `HttpPost` che elimina i dati è denominato `DeleteConfirmed` per fornire al metodo HTTP POST un nome o una firma univoca. Le firme dei due metodi sono illustrate di seguito:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

Il Common Language Runtime (CLR) richiede metodi in rapporto di overload per disporre di una firma di parametro univoca, ovvero lo stesso nome di metodo ma un elenco diverso di parametri. Tuttavia, in questo caso è necessario due metodi di eliminazione: uno per GET e uno per POST che prevedono la stessa firma del parametro. Entrambi devono accettare un singolo intero come parametro.

Per ordinare la natura, è possibile eseguire un paio di cose. Uno consiste nell'assegnare nomi diversi metodi. Questa operazione è stata eseguita dal meccanismo di scaffolding nell'esempio precedente. Tuttavia in questo modo si introduce un piccolo problema: ASP.NET esegue il mapping dei segmenti di un URL ai metodi di azione in base al nome e, se si rinomina un metodo, generalmente il routing non è in grado di trovare questo metodo. La soluzione è mostrata nell'esempio e consiste nell'aggiungere l'attributo `ActionName("Delete")` al metodo `DeleteConfirmed`. Ciò in modo efficace esegue il mapping per il sistema di routing in modo che un URL che includa */Delete/* per una richiesta POST troverà richiesta il `DeleteConfirmed` (metodo).

Un altro modo comune per evitare un problema con i metodi che hanno firme e i nomi identici consiste nel modificare artificialmente la firma del metodo POST per includere un parametro non utilizzato. Ad esempio, alcuni sviluppatori di aggiungono un tipo di parametro `FormCollection` che viene passato al metodo POST e quindi semplicemente non usa il parametro:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>Riepilogo

Ora è un'applicazione ASP.NET MVC completa che archivia dati in un database locale. È possibile creare, leggere, aggiornare, eliminare e cercare i film.

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>Passaggi successivi

Dopo aver compilato e testato un'applicazione web, il passaggio successivo è per renderlo disponibile ad altre persone per l'utilizzo su Internet. A tale scopo, è necessario distribuirla in un provider di hosting web. Microsoft offre l'hosting web gratuito per fino a 10 siti web in un [account Azure gratuito](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). È consigliabile quindi seguire l'esercitazione [distribuire un'app di App ASP.NET MCV sicura con appartenenza, OAuth e Database SQL in Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Un'ottima esercitazione sia a livello di intermedia di Tom Dykstra [creazione di un modello di dati di Entity Framework per un'applicazione ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). [StackOverflow](http://stackoverflow.com/help) e il [forum su ASP.NET MVC](https://forums.asp.net/1146.aspx) sono un'ottima colloca per porre domande. Seguire [me](https://twitter.com/RickAndMSFT) su twitter in modo che è possibile ottenere gli aggiornamenti nella mio esercitazioni più recenti.

Commenti e suggerimenti sono Benvenuti.

— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)  
— [Scott Hanselman](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)

> [!div class="step-by-step"]
> [Precedente](adding-validation.md)
