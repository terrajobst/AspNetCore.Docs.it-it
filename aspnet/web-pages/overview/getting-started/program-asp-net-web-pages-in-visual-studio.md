---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programmazione di ASP.NET Web Pages (Razor) con Visual Studio | Microsoft Docs
author: tfitzmac
description: In questa appendice spiega come è possibile usare Visual Studio 2010 o Visual Web Developer 2010 Express al programma ASP.NET Web Pages con sintassi Razor.
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: f3c1a74b23a0d9535256caa660408701062fe21c
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795447"
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>Programmazione di ASP.NET Web Pages (Razor) con Visual Studio
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come è possibile usare Visual Studio o Visual Web Developer Express al programma siti Web di ASP.NET Web Pages (Razor).
>
> Contenuto dell'esercitazione
>
> - Che cosa è necessario installare (se qualsiasi elemento) per l'utilizzo con ASP.NET Web Pages nella versione di Visual Studio.
> - Come aggiungere il supporto per ASP.NET Web Pages a Visual Web Developer 2010 Express.
> - Come usare le funzionalità in Visual Studio per lavorare con le pagine Razor di ASP.NET, tra cui IntelliSense e il debugger.
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
>
>
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>
>
> Questa esercitazione funziona anche con ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010 e WebMatrix 2.


È possibile programmare ASP.NET Web pages con sintassi Razor usando WebMatrix o molti altri editor di codice. È inoltre possibile utilizzare Microsoft Visual Studio che è un ambiente completo di sviluppo integrato (IDE) che fornisce un potente set di strumenti per la creazione di molti tipi di applicazioni (non solo siti Web). Per usare le pagine Razor di ASP.NET, è possibile usare [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).

Due funzionalità particolarmente utile fornite da Visual Studio per la programmazione con le pagine web ASP.NET Razor sono:

- *IntelliSense*. La funzionalità IntelliSense incorporata in Visual Studio è più completa di IntelliSense in WebMatrix.
- *Debugger*. Il debugger consente di risolvere i problemi del codice mediante l'arresto di un programma mentre è in esecuzione, esaminano le variabili e scorrere il codice riga per riga.

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>Usare Visual Studio con versioni diverse delle pagine Web ASP.NET

Per sviluppare le app web ASP.NET in Visual Studio 2017, installare il **sviluppo ASP.NET e web** carico di lavoro.

Visual Studio 2012 e Visual Studio 2013 includono il supporto per ASP.NET Web Pages. (I pacchetti che sono necessari per supportare le pagine Web ASP.NET vengono installati quando si installa Visual Studio).

Visual Studio 2010 non include il supporto per impostazione predefinita per ASP.NET Web Pages. Per usare ASP.NET Web Pages con Visual Studio 2010, è necessario installare il pacchetto di ASP.NET MVC. Per ottenere ASP.NET Web Pages 2, si installa ASP.NET MVC 4.

La tabella seguente riepiloga il supporto per ASP.NET Web Pages in versioni diverse di Visual Studio.

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **ASP.NET Web Pages 2** | Installare ASP.NET MVC 4 | (Inclusi) | (Inclusi) |
| **Pagine Web ASP.NET 3** |  | Aggiornamento ad ASP.NET Web Pages 3 tramite NuGet | (Inclusi) |

Per lavorare con Visual Studio 2010, vedere [installazione del supporto per pagine Web ASP.NET in Visual Studio 2010](#vs2010support).

## <a name="launching-visual-studio-from-webmatrix"></a>Avviare Visual Studio da WebMatrix

Se si è iniziato un progetto in WebMatrix e tornare a Visual Studio, WebMatrix ti offre un pulsante per aprire facilmente il progetto in Visual Studio. È necessario disporre di Visual Studio installata nel computer di questo pulsante di abilitazione. L'immagine seguente mostra il pulsante in WebMatrix.

![Avviare Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

Quando si fa clic sul pulsante, il progetto viene aperto in Visual Studio. È possibile passare alternativamente tra WebMatrix e Visual Studio senza problemi. Verrà informati se tutti i file sono stati modificati in altro ambiente e devono essere ricaricati per ottenere le modifiche più recenti.

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>La creazione del sito ASP.NET Razor in Visual Studio

Per creare un sito Web di Razor di ASP.NET in Visual Studio:

1. Aprire Visual Studio.
2. Nel **File** menu, fare clic su **nuovo sito Web**.

    ![Crea nuovo sito web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. Nel **nuovo sito Web** finestra di dialogo, selezionare la lingua da utilizzare (Visual c# o Visual Basic).
4. Selezionare il **sito Web ASP.NET (Razor)** modello.

    ![sito Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. Fare clic su **OK**.

Il nuovo progetto esiste e viene popolato con alcune pagine web predefiniti che consentono di iniziare.

### <a name="using-intellisense"></a>Using IntelliSense

Ora che è stato creato un sito, è possibile visualizzare il funzionamento di IntelliSense in Visual Studio.

1. Nel sito Web appena creata, aprire il *cshtml* pagina.
2. Dopo il `<h3>` tag nella pagina digitare `@ServerInfo.` (incluso il punto). Si noti come IntelliSense visualizza i metodi disponibili per il `ServerInfo` helper in un elenco a discesa.

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. Selezionare il `GetHtml` metodo dall'elenco e quindi premere INVIO. IntelliSense viene compilato automaticamente nel metodo. (Come con qualsiasi metodo in c#, è necessario aggiungere `()` caratteri dopo il metodo.) Il codice completo per il `GetHtml` metodo simile al seguente:

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. Premere CTRL+F5 per eseguire la pagina. Questo è l'aspetto delle pagine quando visualizzata in un browser:

    ![pagina predefinita nel browser](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. Chiudere il browser.

### <a name="using-the-debugger"></a>Uso del Debugger

1. In cima il *default. cshtml* pagina, dopo la riga che inizia con `Page.Title`, aggiungere la riga di codice seguente:

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. Sul margine grigio dell'editor a sinistra del codice, fare clic accanto a questa nuova riga per aggiungere un *punto di interruzione*. Un punto di interruzione è un indicatore che indica al debugger di interrompere l'esecuzione del programma a questo punto, è possibile visualizzare ciò che accade.

    ![Imposta punto di interruzione](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. Rimuovere la chiamata ai `ServerInfo.GetHtml` metodo e aggiungere una chiamata al `@myTime` variabile al suo posto. Questa chiamata consente di visualizzare il valore di tempo corrente restituito dalla nuova riga di codice.
4. Premere F5 per eseguirla nel debugger. La pagina si interrompe al punto di interruzione impostato. L'immagine seguente mostra la pagina aspetto nell'editor con il punto di interruzione (giallo).

    ![punto di interruzione di debug](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. Nella barra degli strumenti di Debug, scegliere il **Esegui istruzione** (o preme F11) per eseguire la riga successiva del codice. Ogni volta che si fa clic su questo pulsante, l'esecuzione si arriva alla riga successiva del codice.

    ![Eseguire l'istruzione pulsante](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. Esaminare il valore della `myTime` variabile posizionando il puntatore del mouse su di esso o esaminando i valori visualizzati nei **variabili locali** e **Stack di chiamate** windows. Visual Studio visualizza il valore della variabile.

    ![valore di ora Show](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. Dopo aver completato la variabile di analisi e l'avanzamento tramite codice, premere F5 per continuare l'esecuzione della pagina senza interruzione in corrispondenza di ogni riga. Al termine scorrere tutto il codice, il browser visualizza la pagina.

Per altre informazioni sul debugger e su come eseguire il debug di codice in Visual Studio, vedere [procedura dettagliata: debug di pagine Web in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>Uso di Razor in progetti ASP.NET MVC con Visual Studio

La sintassi Razor anche è ampiamente usata in progetti ASP.NET MVC. MVC è un modo potente, basato su modelli per creare siti Web dinamici. Se il sito Web ASP.NET Web Pages è più difficile da gestire, è possibile provare a convertirlo in un'applicazione ASP.NET MVC. Per un esempio di creazione di un'applicazione MVC, vedere [Introduzione a ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>Installazione del supporto per le pagine Web ASP.NET in Visual Studio 2010

In questa sezione viene illustrato come installare Visual Web Developer Express 2010 e gli strumenti di pagine Web ASP.NET per Visual Studio.

1. Se si ha già l'installazione guidata piattaforma Web, scaricarlo dall'URL seguente:

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. Eseguire l'installazione guidata piattaforma Web.
3. Scegliere il **prodotti** scheda.

    ![Scheda prodotti WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. Cercare **ASP.NET MVC 4** (per ASP.NET Web Pages 2) e quindi fare clic su **Add**. Questi prodotti includono gli strumenti di Visual Studio per la creazione di siti Web di ASP.NET Razor.

    ![Opzioni di installazione di WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. Fare clic su **installare** per completare l'installazione.
