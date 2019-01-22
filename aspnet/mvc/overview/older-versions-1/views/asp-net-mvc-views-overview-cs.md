---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: Panoramica (c#) delle visualizzazioni ASP.NET MVC | Microsoft Docs
author: StephenWalther
description: Che cos'è una visualizzazione MVC ASP.NET e in che cosa differisce da una pagina HTML? In questa esercitazione, Stephen Walther presenta le viste e viene illustrato come è possibile t...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: a8e64a99549584f150d64d909ac97210257b1147
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444129"
---
<a name="aspnet-mvc-views-overview-c"></a>Panoramica (c#) delle visualizzazioni ASP.NET MVC
====================
da [Stephen Walther](https://github.com/StephenWalther)

> Che cos'è una visualizzazione MVC ASP.NET e in che cosa differisce da una pagina HTML? In questa esercitazione, Stephen Walther presenta le viste e viene illustrato come è possibile sfruttare i vantaggi di visualizzare i dati e gli helper HTML all'interno di una vista.


Lo scopo di questa esercitazione è fornire una breve introduzione a visualizzazioni ASP.NET MVC, visualizzare i dati e gli helper HTML. Al termine di questa esercitazione, è necessario comprendere come creare nuove viste, passare i dati da un controller a una visualizzazione e usare gli helper HTML per generare il contenuto in una vista.

## <a name="understanding-views"></a>Informazioni sulle viste

Per ASP.NET o pagine ASP, ASP.NET MVC non include tutto ciò che corrisponde direttamente a una pagina. In un'applicazione ASP.NET MVC, non esiste una pagina su disco che corrisponde al percorso nell'URL digitato nella barra degli indirizzi del browser. L'elemento più simile a una pagina in un'applicazione ASP.NET MVC è un elemento chiamato un' *vista*.

In un'applicazione ASP.NET MVC, le richieste in ingresso browser vengono mappate alle azioni del controller. Un'azione del controller potrebbe restituire una visualizzazione. Tuttavia, un'azione del controller potrebbe eseguire un altro tipo di azione, ad esempio è il reindirizzamento a un'altra azione del controller.

L'elenco 1 contiene un controller semplice denominato HomeController. La classe HomeController espone due azioni del controller denominate Details() e Index ().

**Listato 1 - HomeController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

È possibile richiamare la prima azione, l'azione Index (), digitare l'URL seguente nella barra degli indirizzi del browser:

/Home/Index

È possibile richiamare la seconda azione, l'azione Details(), immettendo questo indirizzo nel browser:

/ Home/dettagli

L'azione Index () restituisce una visualizzazione. La maggior parte delle azioni che creano restituirà le visualizzazioni. Tuttavia, un'azione può restituire altri tipi di risultati dell'azione. Ad esempio, l'azione Details() restituisce un RedirectToActionResult che reindirizza le richieste in ingresso per l'azione Index ().

L'azione Index () contiene la seguente riga di codice:

View();

Questa riga di codice restituisce una vista in cui deve trovarsi nel percorso seguente nel server web:

\Views\Home\Index.aspx

Il percorso della visualizzazione viene dedotto dal nome del controller e il nome dell'azione del controller.

Se si preferisce, possono essere esplicite sulla vista. La riga di codice seguente restituisce una visualizzazione denominata Fred:

Visualizzazione (Fred);

Quando viene eseguita questa riga di codice, viene restituita una visualizzazione dal percorso seguente:

\Views\Home\Fred.aspx

> [!NOTE] 
> 
> Se si prevede di creare unit test per l'applicazione ASP.NET MVC è una buona idea definire esplicito i nomi delle visualizzazioni. In questo modo, è possibile creare uno unit test per verificare che la vista previsto è stata restituita da un'azione del controller.


## <a name="adding-content-to-a-view"></a>Aggiunta di contenuto a una vista

Una vista è uno standard (documento HTML che può contenere gli script X). È utilizzare script per aggiungere contenuto dinamico a una visualizzazione.

Ad esempio, la visualizzazione nel listato 2 consente di visualizzare la data e ora correnti.

**Listato 2 - \Views\Home\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

Si noti che il corpo della pagina HTML nel listato 2 contiene lo script seguente:

&lt;% Response.Write(DateTime.Now);%&gt;

Si usano i delimitatori di script &lt;% e %&gt; per contrassegnare l'inizio e alla fine di uno script. Questo script viene scritto nel linguaggio c#. Visualizza la data e ora correnti, chiamare il metodo Response per il rendering del contenuto nel browser. I delimitatori di script &lt;% e %&gt; può essere utilizzato per eseguire una o più istruzioni.

Poiché si chiama spesso Response, Microsoft fornisce un collegamento è per chiamare il metodo Response. La visualizzazione nel listato 3 Usa i delimitatori &lt;% = % e&gt; come collegamento per la chiamata a Response.

**Listing 3 - Views\Home\Index2.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

È possibile usare qualsiasi linguaggio .NET per generare il contenuto dinamico in una vista. In genere, si userà Visual Basic .NET o C# per scrivere il controller e visualizzazioni.

## <a name="using-html-helpers-to-generate-view-content"></a>Utilizzo di helper HTML per generare Visualizza contenuto

Per renderne più semplice aggiungere contenuto a una vista, è possibile sfruttare i vantaggi di un elemento denominato un' *HTML Helper*. Un HTML Helper, è in genere, un metodo che genera una stringa. È possibile usare gli helper HTML per generare gli elementi HTML standard, ad esempio caselle di testo, collegamenti, elenchi a discesa e caselle di riepilogo.

Ad esempio, la visualizzazione nel listato 4 sfrutta le tre gli helper HTML, gli helper BeginForm() TextBox() e Password(): per generare un account di accesso formano (vedere la figura 1).

**Listato 4 - \Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]


[![La finestra di dialogo Nuovo progetto](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)

**Figura 01**: Un modulo di accesso standard ([fare clic per visualizzare l'immagine con dimensioni normali](asp-net-mvc-views-overview-cs/_static/image2.png))


Tutti i metodi helper HTML vengono chiamati nella proprietà Html della visualizzazione. Ad esempio, si esegue il rendering una casella di testo chiamando il metodo Html.TextBox().

Si noti che userai i delimitatori di script &lt;% = % e&gt; quando si chiama Html.TextBox() sia Html.Password() helper. Questi helper è sufficiente restituiscono una stringa. È necessario chiamare Response per eseguire il rendering di stringa al browser.

Uso di metodi HTML Helper è facoltativo. Semplificano la vita, riducendo la quantità di codice HTML e script che è necessario scrivere. La visualizzazione nel listato 5 esegue il rendering esattamente dello stesso formato di visualizzazione nel listato 4 senza l'utilizzo di helper HTML.

**Listato 5 - \Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

È inoltre la possibilità di creare il proprio gli helper HTML. Ad esempio, è possibile creare un metodo helper GridView() che visualizza automaticamente un set di record del database in una tabella HTML. In questo argomento verrà esaminata in esercitazioni **creazione di helper HTML personalizzati**.

## <a name="using-view-data-to-pass-data-to-a-view"></a>Utilizzando i dati della visualizzazione per passare dati a una vista

Visualizzare i dati è possibile per passare i dati da un controller a una visualizzazione. Pensare a visualizzare i dati, ad esempio un pacchetto che si invia tramite posta elettronica. Tutti i dati passati da un controller a una visualizzazione devono essere inviati tramite questo pacchetto. Ad esempio, il controller nel listato 6 aggiunge un messaggio per visualizzare i dati.

**Listato 6 - ProductController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

Il controller di proprietà ViewData rappresenta una raccolta di coppie nome / valore. Nel listato 6, il metodo Index () aggiunge un elemento alla raccolta di dati di visualizzazione denominata messaggio con il valore Hello World!. Quando la vista viene restituita dal metodo Index (), i dati di visualizzazione vengono passati automaticamente alla visualizzazione.

La visualizzazione nel listato 7 recupera il messaggio da visualizzare i dati ed esegue il rendering il messaggio al browser.

**Listing 7 -- \Views\Product\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

Si noti che la vista si avvale del metodo Helper HTML Html.Encode() durante il rendering del messaggio. L'Helper HTML Html.Encode() codifica i caratteri speciali, ad esempio &lt; e &gt; in caratteri che possono essere visualizzati in una pagina web. Ogni volta che si esegue il rendering del contenuto per l'invio di un utente a un sito Web, è consigliabile codificare il contenuto per impedire attacchi injection JavaScript.

(Perché è stato creato il messaggio di che noi stessi nel ProductController, abbiamo bisogno codificare il messaggio. Tuttavia, è un'operazione consigliabile chiamare sempre il metodo Html.Encode() quando visualizzare il contenuto recuperato da visualizzare i dati all'interno di una vista).

Nel listato 7, abbiamo sfruttato visualizzare i dati di passare un messaggio stringa semplice da un controller a una visualizzazione. È anche possibile usare Visualizza dati per trasmettere altri tipi di dati, ad esempio una raccolta di record del database, da un controller a una visualizzazione. Ad esempio, se si desidera visualizzare il contenuto della tabella Products del database in una vista, quindi si passa la raccolta di database i record nella visualizzazione dei dati.

È possibile scegliere di passare i dati di visualizzazione fortemente tipizzata da un controller a una visualizzazione. In questo argomento verrà esaminata in esercitazioni **Understanding fortemente tipizzate visualizzare i dati e viste**.

## <a name="summary"></a>Riepilogo

Questa esercitazione è fornita una breve introduzione a visualizzazioni ASP.NET MVC, visualizzare i dati e gli helper HTML. Nella prima sezione, è stato descritto come aggiungere nuove visualizzazioni per il progetto. Si è appreso che è necessario aggiungere una visualizzazione nella cartella corretta per chiamarla da un controller specifico. Successivamente, abbiamo discusso l'argomento di helper HTML. Si è appreso come helper HTML consentono di generare in modo semplice il contenuto HTML standard. Infine, si è appreso come sfruttare i vantaggi di visualizzare i dati per passare i dati da un controller a una visualizzazione.

> [!div class="step-by-step"]
> [avanti](creating-custom-html-helpers-cs.md)
