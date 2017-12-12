---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
title: Miglioramento dei dettagli e i metodi di eliminazione (VB) | Documenti Microsoft
author: Rick-Anderson
description: "In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: c5c14ef0-c128-4dc1-8c01-7f0fdb09e411
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 24c986f7ec8376bc997f1ebc575338772507cbc9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="improving-the-details-and-delete-methods-vb"></a>Miglioramento dei dettagli e i metodi di eliminazione (VB)
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

> In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul seguente collegamento: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti utilizzando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supportano runtime + tools)
> 
> Se si utilizza Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul seguente collegamento: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un progetto di Visual Web Developer con codice sorgente VB.NET complemento è disponibile in questo argomento. [Scaricare la versione di Visual Basic.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce c#, passare il [c# versione](../cs/improving-the-details-and-delete-methods.md) di questa esercitazione.


In questa parte dell'esercitazione, si renderanno alcuni miglioramenti generato automaticamente `Details` e `Delete` metodi. Queste modifiche non sono necessari, ma con pochi automaticamente piccole parti di codice, è possibile migliorare facilmente l'applicazione.

## <a name="improving-the-details-and-delete-methods"></a>Miglioramento dei dettagli e i metodi di eliminazione

Quando scaffolding di `Movie` controller MVC ASP.NET generato codice che ha lavorato efficace, ma che può essere reso più affidabile con poche modifiche minori.

Aprire il `Movie` controller e modificare il `Details` metodo restituendo `HttpNotFound` quando non vengono trovato un filmato. È inoltre necessario modificare il `Details` per impostare un valore predefinito per l'ID che viene passato al metodo. (Sono state apportate modifiche simili per il `Edit` metodo [parte 6](examining-the-edit-methods-and-edit-view.md) di questa esercitazione.) Tuttavia, è necessario modificare il tipo restituito del `Details` metodo `ViewResult` per `ActionResult`, perché il `HttpNotFound` metodo non restituisce un `ViewResult` oggetto. Nell'esempio seguente viene modificato `Details` metodo.

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample1.vb)]

Codice innanzitutto semplifica la ricerca di dati utilizzando il `Find` metodo. Un'importante funzionalità di sicurezza è incorporata nel metodo è che il codice verifica che il `Find` metodo ha trovato un filmato prima che il codice tenta di utilizzarlo. Ad esempio, un pirata informatico potrebbe causare errori nel sito modificando l'URL creato per i collegamenti da `http://localhost:xxxx/Movies/Details/1` esempio `http://localhost:xxxx/Movies/Details/12345` (o altri valori che non rappresentano un filmato effettivo). In caso contrario il controllo per un filmato null, ciò potrebbe causare un errore di database.

Analogamente, modificare il `Delete` e `DeleteConfirmed` metodi per specificare un valore predefinito per il parametro ID e restituire `HttpNotFound` quando non vengono trovato un filmato. L'aggiornamento `Delete` metodi di `Movie` controller vengono mostrate di seguito.

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample2.vb)]

Si noti che il `Delete` metodo non comporta l'eliminazione dei dati. L'esecuzione di un'operazione di eliminazione in risposta a una richiesta GET (o l'esecuzione di un'operazione di modifica, di creazione o di qualsiasi altra azione che modifica i dati) introduce un problema di sicurezza. Per ulteriori informazioni, vedere il post di blog di Stephen Walther [ASP.NET MVC suggerimento #46-non usare eliminare collegamenti poiché creano problemi di sicurezza](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

Il metodo `HttpPost` che elimina i dati è denominato `DeleteConfirmed` per fornire al metodo HTTP POST un nome o una firma univoca. Le firme dei due metodi sono illustrate di seguito:

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample3.vb)]

Common language runtime (CLR) richiede metodi di overload per avere una firma univoca (stesso nome, elenco di parametri diverso). Tuttavia, in questo caso è necessario due metodi di eliminazione: uno per GET - e uno per POST che richiedono entrambi la stessa firma. Entrambi devono accettare un singolo intero come parametro.

Per ordinare la natura, è possibile eseguire alcune operazioni. Uno consiste nell'assegnare i metodi di nomi diversi. Ovvero è stato fatto in egli sopra riportato. Tuttavia in questo modo si introduce un piccolo problema: ASP.NET esegue il mapping dei segmenti di un URL ai metodi di azione in base al nome e, se si rinomina un metodo, generalmente il routing non è in grado di trovare questo metodo. La soluzione è mostrata nell'esempio e consiste nell'aggiungere l'attributo `ActionName("Delete")` al metodo `DeleteConfirmed`. Questo in modo efficace esegue il mapping per il sistema di routing in modo che un URL che includa */Delete/*disponibili per un POST di richiesta di `DeleteConfirmed` metodo.

Un altro modo per evitare un problema con metodi che hanno firme e i nomi identici consiste nel modificare artificialmente la firma del metodo POST per includere un parametro non utilizzato. Ad esempio, alcuni sviluppatori aggiungono un parametro di tipo `FormCollection` che viene passato al metodo POST e quindi semplicemente non usa il parametro:

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample4.vb)]

## <a name="wrapping-up"></a>Conclusione

Ora è un'applicazione ASP.NET MVC completa che archivia i dati in un database di SQL Server Compact. È possibile creare, leggere, aggiornare, eliminare e cercare film.

![](improving-the-details-and-delete-methods/_static/image1.png)

In questa esercitazione di base ottenuto è iniziato le controller, associarli a viste e al passaggio dati hardcoded. È quindi possibile creare e progettato un modello di dati. Code First di Entity Framework creato un database dal modello di dati in tempo reale e il sistema lo scaffolding di ASP.NET MVC generati automaticamente i metodi di azione e le visualizzazioni per operazioni CRUD di base. Si aggiunge un modulo di ricerca che consentono agli utenti di eseguire ricerche nel database. Modificato il database per includere una nuova colonna di dati e quindi aggiornata due pagine per creare e visualizzare questi nuovi dati. È stato aggiunto convalida contrassegnando il modello di dati con gli attributi di `DataAnnotations` dello spazio dei nomi. La convalida risulta viene eseguito sul client e nel server.

Se si desidera distribuire l'applicazione, è utile per test prima dell'applicazione sul server locale di IIS 7. È possibile utilizzare questo [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) collegamento per abilitare l'impostazione di IIS per le applicazioni ASP.NET. Vedere i collegamenti di distribuzione seguenti:

- [Mappa del contenuto di distribuzione di ASP.NET](https://msdn.microsoft.com/en-us/library/dd394698.aspx)
- [L'attivazione di IIS 7. x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Distribuzione di progetti applicazione Web](https://msdn.microsoft.com/en-us/library/dd394698.aspx)

Si consiglia per continuare con il livello intermedio ora [la creazione di un modello di dati di Entity Framework per un'applicazione MVC ASP.NET](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) e [negozio MVC](../../mvc-music-store/mvc-music-store-part-1.md) esercitazioni, per esplorare il [ASP.NET articoli su MSDN](https://msdn.microsoft.com/en-us/library/gg416514(VS.98).aspx)e per estrarre i video e le risorse in molti [https://asp.net/mvc](https://asp.net/mvc) per ulteriori informazioni su ASP.NET MVC. Il [forum di ASP.NET MVC](https://forums.asp.net/1146.aspx) sono un ottimo punto di porre domande.

Usufruisci!

-Scott Hanselman ([http://hanselman.com](http://hanselman.com) e [ @shanselman ](http://twitter.com/shanselman) su Twitter) e Rick Anderson [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

>[!div class="step-by-step"]
[Precedente](adding-validation-to-the-model.md)
