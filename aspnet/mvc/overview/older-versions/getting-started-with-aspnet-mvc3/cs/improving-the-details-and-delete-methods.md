---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
title: Miglioramento dei dettagli e dei metodi di eliminazione (c#) | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 3f42edd9-c5b8-4712-9055-970f7d38e350
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 255374eb21568d05569f8af6727ad4b558acfc2f
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576443"
---
<a name="improving-the-details-and-delete-methods-c"></a>Miglioramento dei dettagli e dei metodi di eliminazione (c#)
====================
da [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > È disponibile una versione aggiornata di questa esercitazione [qui](../../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e vengono illustrate altre funzionalità.
> 
> 
> Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul collegamento seguente: [instalace Webové Platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime e strumenti di supportano)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul collegamento seguente: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un progetto di Visual Web Developer con codice sorgente c# è disponibile a complemento di questo argomento. [Scaricare la versione c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce Visual Basic, passare al [versione Visual Basic](../vb/intro-to-aspnet-mvc-3.md) di questa esercitazione.


In questa parte dell'esercitazione, si apporteranno alcuni miglioramenti a generato automaticamente `Details` e `Delete` metodi. Queste modifiche non sono necessarie, ma con solo poche piccole parti di codice, è possibile migliorare facilmente l'applicazione.

## <a name="improving-the-details-and-delete-methods"></a>Miglioramento dei dettagli e dei metodi di eliminazione

Quando sottoposto a scaffolding di `Movie` controller ASP.NET MVC ha generato codice che funzionava bene, ma che possono essere resi più affidabile con solo alcune piccole modifiche.

Aprire il `Movie` controller e modificare i `Details` metodo restituendo `HttpNotFound` quando non viene trovato un film. È anche necessario modificare il `Details` metodo per impostare un valore predefinito per l'ID che viene passato al metodo. (Sono state apportate modifiche simili per il `Edit` nel metodo [parte 6](examining-the-edit-methods-and-edit-view.md) di questa esercitazione.) Tuttavia, è necessario modificare il tipo restituito del `Details` metodo da `ViewResult` per `ActionResult`, perché il `HttpNotFound` metodo non restituisce un `ViewResult` oggetto. Nell'esempio seguente viene modificato `Details` (metodo).

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample1.cs)]

Codice prima di tutto semplifica la ricerca di dati usando il `Find` (metodo). Un'importante funzionalità di sicurezza che abbiamo creato nel metodo è che il codice verifica che il `Find` metodo ha trovato un film prima che il codice prova a eseguire qualsiasi operazione con esso. Ad esempio, un hacker potrebbe introdurre errori nel sito modificando l'URL creato dai collegamenti di `http://localhost:xxxx/Movies/Details/1` su un valore come `http://localhost:xxxx/Movies/Details/12345` (o un altro valore che non rappresenta un film effettivo). In caso contrario il controllo di un film con valore null, ciò potrebbe comportare un errore di database.

Analogamente, sostituire il `Delete` e `DeleteConfirmed` metodi per specificare un valore predefinito per il parametro ID e restituire `HttpNotFound` quando non viene trovato un film. Aggiornato `Delete` metodi di `Movie` controller sono illustrate di seguito.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample2.cs)]

Si noti che il `Delete` metodo non comporta l'eliminazione dei dati. L'esecuzione di un'operazione di eliminazione in risposta a una richiesta GET (o l'esecuzione di un'operazione di modifica, di creazione o di qualsiasi altra azione che modifica i dati) introduce un problema di sicurezza. Per altre informazioni, vedere l'intervento nel blog di Stephen Walther [46 & suggerimento di ASP.NET MVC, non usare i collegamenti di eliminazione poiché creano le falle](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

Il metodo `HttpPost` che elimina i dati è denominato `DeleteConfirmed` per fornire al metodo HTTP POST un nome o una firma univoca. Le firme dei due metodi sono illustrate di seguito:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample3.cs)]

Common language runtime (CLR) richiede metodi di overload per avere una firma univoca (stesso nome, un elenco diverso di parametri). Tuttavia, in questo caso è necessario due metodi di eliminazione: uno per GET e uno per POST che richiedono entrambi la stessa firma. Entrambi devono accettare un singolo intero come parametro.

Per ordinare la natura, è possibile eseguire un paio di cose. Uno consiste nell'assegnare nomi diversi metodi. Questo è ciò che abbiamo introdotto in egli sopra riportato. Tuttavia in questo modo si introduce un piccolo problema: ASP.NET esegue il mapping dei segmenti di un URL ai metodi di azione in base al nome e, se si rinomina un metodo, generalmente il routing non è in grado di trovare questo metodo. La soluzione è mostrata nell'esempio e consiste nell'aggiungere l'attributo `ActionName("Delete")` al metodo `DeleteConfirmed`. Ciò in modo efficace esegue il mapping per il sistema di routing in modo che un URL che includa <em>/Delete/</em>per una richiesta POST troverà richiesta il `DeleteConfirmed` (metodo).

Un altro modo per evitare un problema con i metodi che hanno firme e i nomi identici consiste nel modificare artificialmente la firma del metodo POST per includere un parametro non utilizzato. Ad esempio, alcuni sviluppatori di aggiungono un tipo di parametro `FormCollection` che viene passato al metodo POST e quindi semplicemente non usa il parametro:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="wrapping-up"></a>Conclusioni

Ora è un'applicazione ASP.NET MVC completa che archivia dati in un database di SQL Server Compact. È possibile creare, leggere, aggiornare, eliminare e cercare i film.

![](improving-the-details-and-delete-methods/_static/image1.png)

Questa esercitazione di base tutto quello che ti a rendere i controller, associandole con viste e passano dati hardcoded. È quindi possibile creato e progettato un modello di dati. Code First di Entity Framework ha creato un database dal modello di dati in tempo reale e il sistema di scaffolding di ASP.NET MVC generati automaticamente i metodi di azione e visualizzazioni per operazioni CRUD di base. È stata quindi aggiunta una maschera di ricerca che consentono agli utenti di eseguire ricerche nel database. È stato modificato il database in modo da includere una nuova colonna di dati e quindi aggiornata due pagine per creare e visualizzare questi nuovi dati. La convalida è stato aggiunto, contrassegnando il modello di dati con gli attributi dal `DataAnnotations` dello spazio dei nomi. La convalida risulta viene eseguito sul client e nel server.

Se si desidera distribuire l'applicazione, è utile per primo test l'applicazione sul server IIS 7 locale. È possibile usare questo [instalace Webové Platformy](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) collegamento per abilitare l'impostazione di IIS per le applicazioni ASP.NET. Vedere i collegamenti di distribuzione seguenti:

- [Mappa del contenuto di distribuzione di ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx)
- [L'attivazione di IIS 7.x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Distribuzione di progetti applicazione Web](https://msdn.microsoft.com/library/dd394698.aspx)

Adesso ti suggeriamo per proseguire con il livello intermedio [creazione di un modello di dati di Entity Framework per un'applicazione ASP.NET MVC](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) e [MVC Music Store](../../mvc-music-store/mvc-music-store-part-1.md) esercitazioni e per esplorare il [ASP.NET articoli su MSDN](https://msdn.microsoft.com/library/gg416514(VS.98).aspx)e per controllare il video e le risorse a molti [ https://asp.net/mvc ](https://asp.net/mvc) per altre informazioni su ASP.NET MVC. Il [forum di ASP.NET MVC](https://forums.asp.net/1146.aspx) sono un luogo ideale per porre domande.

Buona lettura.

-Scott Hanselman ([ http://hanselman.com ](http://hanselman.com) e [ @shanselman ](http://twitter.com/shanselman) su Twitter) e Rick Anderson [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

> [!div class="step-by-step"]
> [Precedente](adding-validation-to-the-model.md)
