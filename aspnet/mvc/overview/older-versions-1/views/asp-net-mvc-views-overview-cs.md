---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: ASP.NET MVC Visualizza panoramica (c#) | Documenti Microsoft
author: StephenWalther
description: Che cos'è una visualizzazione MVC ASP.NET e differenza da una pagina HTML? In questa esercitazione, Stephen Walther vengono introdotte le visualizzazioni e viene illustrato come è possibile t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 5217994168ebac32a4a9754ae09e63e120804813
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870271"
---
<a name="aspnet-mvc-views-overview-c"></a>ASP.NET MVC Visualizza panoramica (c#)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> Che cos'è una visualizzazione MVC ASP.NET e differenza da una pagina HTML? In questa esercitazione, Stephen Walther vengono introdotte le visualizzazioni e viene illustrato come è possibile usufruire di visualizzare i dati e gli helper HTML all'interno di una vista.


Lo scopo di questa esercitazione consiste nel fornire una breve introduzione alle visualizzazioni ASP.NET MVC, visualizzare i dati e gli helper HTML. Al termine di questa esercitazione, è necessario comprendere come creare nuove visualizzazioni, passare una visualizzazione dati da un controller e utilizzare l'helper HTML per generare il contenuto in una vista.

## <a name="understanding-views"></a>Informazioni sulle viste

Per le pagine ASP o ASP.NET, MVC ASP.NET non include tutto ciò che corrisponde direttamente a una pagina. In un'applicazione ASP.NET MVC, non c'è una pagina su disco che corrisponde al percorso nell'URL digitato nella barra degli indirizzi del browser. La cosa più vicina a una pagina in un'applicazione ASP.NET MVC è un elemento denominato un *vista*.

MVC ASP.NET vengono eseguito il mapping alle richieste del browser di applicazione, in ingresso per le azioni del controller. Un'azione del controller potrebbe restituire una visualizzazione. Tuttavia, un'azione del controller potrebbe eseguire un altro tipo di azione, ad esempio si reindirizzamento a un'altra azione del controller.

Elenco 1 contiene un controller semplice denominato HomeController. La classe HomeController espone due azioni del controller denominate Index () e Details().

**Elenco 1 - HomeController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

È possibile richiamare la prima azione, l'azione Index (), digitando il seguente URL nella barra degli indirizzi del browser:

/Home/Index

È possibile richiamare la seconda azione, l'azione Details(), digitando l'indirizzo nel browser:

/ Home/dettagli

L'azione Index () restituisce una visualizzazione. La maggior parte delle azioni create restituirà le visualizzazioni. Tuttavia, un'azione può restituire altri tipi di risultati dell'azione. Ad esempio, l'azione Details() restituisce un RedirectToActionResult che reindirizza la richiesta in ingresso per l'azione Index ().

L'azione Index () contiene la seguente riga di codice:

View();

Questa riga di codice restituisce una visualizzazione che deve trovarsi nel percorso seguente sul server web:

\Views\Home\Index.aspx

Il percorso della visualizzazione viene dedotto dal nome del controller e il nome dell'azione del controller.

Se si preferisce, possono essere esplicite sulla visualizzazione. La riga di codice seguente restituisce una visualizzazione denominata Fred:

Visualizzazione (Fred);

Quando viene eseguita questa riga di codice, viene restituita una visualizzazione dal percorso seguente:

\Views\Home\Fred.aspx

> [!NOTE] 
> 
> Se si prevede di creare unit test per l'applicazione ASP.NET MVC è consigliabile essere espliciti sui nomi di visualizzazione. In questo modo, è possibile creare uno unit test per verificare che la vista previsto è stata restituita da un'azione del controller.


## <a name="adding-content-to-a-view"></a>Aggiunta di contenuto a una vista

Una vista è uno standard (documento HTML che può includere script X). Gli script consentono di aggiungere contenuto dinamico a una vista.

Ad esempio, la visualizzazione nel listato 2 Visualizza la data e ora correnti.

**Il listato 2 - \Views\Home\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

Si noti che il corpo della pagina HTML nel listato 2 contiene lo script seguente:

&lt;% Response.Write(DateTime.Now);%&gt;

Utilizzare i delimitatori di script &lt;% e %&gt; per contrassegnare l'inizio e alla fine di uno script. Questo script viene scritto in c#. Visualizza la data e ora correnti, chiamare il metodo Response per il rendering del contenuto nel browser. I delimitatori di script &lt;% e %&gt; può essere usato per eseguire una o più istruzioni.

Poiché si effettuano chiamate Response spesso, Microsoft offre un scelta rapida per chiamare il metodo Response. La visualizzazione nel listato 3 utilizza i delimitatori &lt;% = % e&gt; come scelta rapida per la chiamata a Response.

**Elenco di 3 - Views\Home\Index2.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

È possibile utilizzare qualsiasi linguaggio .NET per generare il contenuto dinamico in una vista. In genere, si ll utilizzare Visual Basic .NET o Visual c# per scrivere il controller e visualizzazioni.

## <a name="using-html-helpers-to-generate-view-content"></a>Utilizzando l'helper HTML per generare il contenuto di visualizzazione

Per rendere più semplice aggiungere contenuto a una vista, è possibile sfruttare cosiddetto un *HTML Helper*. Un HTML Helper, è in genere, un metodo che genera una stringa. È possibile utilizzare l'helper HTML per generare elementi HTML standard, ad esempio caselle di testo, collegamenti, elenchi a discesa e caselle di riepilogo.

Ad esempio, la visualizzazione nel listato 4 sfrutta i vantaggi dei tre helper HTML, gli helper BeginForm() TextBox() e Password()-consente di generare un account di accesso formano (vedere Figura 1).

**Listato 4 - \Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]


[![La finestra di dialogo Nuovo progetto](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)

**Figura 01**: un form di accesso standard ([fare clic per visualizzare l'immagine ingrandita](asp-net-mvc-views-overview-cs/_static/image2.png))


Tutti i metodi helper HTML sono chiamati sulla proprietà della visualizzazione Html. Ad esempio, si esegue il rendering una casella di testo chiamando il metodo Html.TextBox().

Si noti che utilizzare i delimitatori di script &lt;% = % e&gt; quando si chiama Html.TextBox() sia Html.Password() helper. Questi helper limitino a restituire una stringa. È necessario chiamare Response per eseguire il rendering di stringa per il browser.

Utilizzo di metodi HTML Helper è facoltativo. Essi semplificano la vita riducendo la quantità di HTML e script che è necessario scrivere. La visualizzazione nel listato 5 esegue il rendering lo stesso formato esatto di visualizzazione nel listato 4 senza utilizzare l'helper HTML.

**Nel listato 5 - \Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

È inoltre la possibilità di creare la propria helper HTML. Ad esempio, è possibile creare un metodo di supporto GridView() che visualizza automaticamente un set di record del database in una tabella HTML. In questo argomento vengono illustrati nell'esercitazione **la creazione di helper HTML personalizzati**.

## <a name="using-view-data-to-pass-data-to-a-view"></a>Utilizzando i dati della visualizzazione di passare dati a una vista

Visualizzare i dati consentono di passare i dati da un controller a una visualizzazione. Pensare a visualizzare i dati come un pacchetto che si invia tramite posta elettronica. Tutti i dati passati da un controller a una vista devono essere inviati tramite questo pacchetto. Ad esempio, il controller nel listato 6 aggiunge un messaggio per visualizzare i dati.

**Elenco 6 - ProductController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

Il controller di proprietà ViewData rappresenta una raccolta di coppie nome / valore. Nel listato 6, il metodo Index () aggiunge un elemento per la raccolta di dati di visualizzazione denominata messaggio con il valore di Hello World!. Quando la vista viene restituita dal metodo Index (), i dati della visualizzazione viene passati automaticamente alla visualizzazione.

La visualizzazione nel listato 7 recupera il messaggio dai dati di visualizzazione e visualizza il messaggio nel browser.

**Elenco 7 - \Views\Product\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

Si noti che la vista consente di sfruttare il metodo Helper HTML Html.Encode() quando il rendering del messaggio. L'Helper HTML Html.Encode() codifica i caratteri speciali, ad esempio &lt; e &gt; in caratteri che possono essere visualizzati in una pagina web. Ogni volta che si esegue il rendering del contenuto per l'invio di un utente a un sito Web, si consiglia di codificare il contenuto per impedire attacchi intrusivi nel codice JavaScript.

(Perché abbiamo creato il messaggio effettuata nel ProductController, non abbiamo t devono necessariamente codificare il messaggio. Tuttavia, è consigliabile chiamare sempre il metodo Html.Encode() quando visualizzare il contenuto recuperato da visualizzare i dati all'interno di una vista.)

Nel listato 7, abbiamo sfruttato Visualizza dati per passare un messaggio stringa semplice da un controller a una visualizzazione. È possibile utilizzare dati di visualizzazione per trasmettere altri tipi di dati, ad esempio una raccolta di record del database, da un controller a una vista. Ad esempio, se si desidera visualizzare il contenuto della tabella Products del database in una vista, quindi passare la raccolta di database nella visualizzazione registra dati.

È inoltre possibile passare i dati di visualizzazione fortemente tipizzata da un controller a una vista. In questo argomento vengono illustrati nell'esercitazione **dati di visualizzazione di informazioni sui fortemente tipizzati e viste**.

## <a name="summary"></a>Riepilogo

In questa esercitazione viene fornita una breve introduzione a ASP.NET MVC viste, visualizzare i dati e gli helper HTML. Nella prima sezione, è stato descritto come aggiungere nuove visualizzazioni per il progetto. Si è appreso che è necessario aggiungere una visualizzazione nella cartella corretta per chiamarlo da un controller specifico. Successivamente, abbiamo parlato l'argomento di helper HTML. Si è appreso come helper HTML consentono di generare facilmente il contenuto HTML standard. Infine, è stato descritto come sfruttare i vantaggi dei dati di visualizzazione per passare dati da un controller di una vista.

> [!div class="step-by-step"]
> [avanti](creating-custom-html-helpers-cs.md)
