---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programmazione ASP.NET Web Pages (Razor) tramite Visual Studio | Documenti Microsoft
author: tfitzmac
description: Questa appendice spiega come è possibile utilizzare Visual Studio 2010 o Visual Web Developer 2010 Express al programma ASP.NET Web Pages con sintassi Razor.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2014
ms.topic: article
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: eb17c8cc1fab5b552c8495e74bb86ae9dbc5b972
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896582"
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>Programmazione di pagine Web ASP.NET (Razor) utilizzando Visual Studio
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo viene illustrato come è possibile utilizzare Visual Studio o Visual Web Developer Express al programma di siti Web di ASP.NET Web Pages (Razor).
> 
> Si apprenderà
> 
> - Cosa è necessario installare (eventuale) per l'utilizzo con ASP.NET Web Pages nella versione di Visual Studio.
> - Come aggiungere il supporto per le pagine Web ASP.NET in Visual Web Developer 2010 Express.
> - Come utilizzare le funzionalità in Visual Studio per utilizzare le pagine ASP.NET Razor, tra cui IntelliSense e il debugger.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> In questa esercitazione si integra inoltre con ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010 e WebMatrix 2.


È possibile programmare ASP.NET Web pages con sintassi Razor utilizzando WebMatrix o molti altri editor di codice. È inoltre possibile utilizzare Microsoft Visual Studio è un ambiente completo di sviluppo integrato (IDE) che fornisce un potente set di strumenti per la creazione di molti tipi di applicazioni (non solo i siti Web). Per utilizzare le pagine ASP.NET Razor, è possibile utilizzare una delle edizioni di Visual Studio complete o il gratuito [Visual Studio Express per Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.

Due funzionalità particolarmente utile fornite da Visual Studio per la programmazione con le pagine web ASP.NET Razor sono:

- *IntelliSense*. La funzionalità IntelliSense integrata in Visual Studio è più completa di IntelliSense in WebMatrix.
- *Debugger*. Il debugger consente di risolvere i problemi relativi al codice per l'arresto di un programma mentre è in esecuzione, esaminare le variabili e avanzando nel codice riga per riga.

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>In Visual Studio con diverse versioni di ASP.NET Web Pages

Visual Studio 2012 e Visual Studio 2013 includono il supporto per ASP.NET Web Pages. (I pacchetti che sono necessari per supportare le pagine Web ASP.NET vengono installati quando si installa Visual Studio).

Visual Studio 2010 non include il supporto per impostazione predefinita per le pagine Web ASP.NET. Per utilizzare le pagine Web ASP.NET con Visual Studio 2010, è necessario installare il pacchetto di ASP.NET MVC. Per ottenere ASP.NET Web Pages 2, si installa ASP.NET MVC 4.

Nella tabella seguente viene riepilogato il supporto per le pagine Web ASP.NET in versioni diverse di Visual Studio.

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **ASP.NET Web Pages 2** | Installare ASP.NET MVC 4 | (Inclusi) | (Inclusi) |
| **ASP.NET Web Pages 3** |  | Aggiornamento ASP.NET Web Pages 3 NuGet tramite | (Inclusi) |

Per lavorare con Visual Studio 2010, vedere [installazione del supporto per pagine Web ASP.NET in Visual Studio 2010](#vs2010support).

## <a name="launching-visual-studio-from-webmatrix"></a>Avvio di Visual Studio da WebMatrix

Se è stato avviato un progetto in WebMatrix e si desidera passare a Visual Studio, WebMatrix ti offre un pulsante per aprire facilmente il progetto in Visual Studio. È necessario disporre di Visual Studio installata nel computer in uso per questo pulsante deve essere abilitata. La figura seguente mostra il pulsante in WebMatrix.

![Avviare Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

Quando si fa clic sul pulsante, il progetto è aperto in Visual Studio. È possibile passare avanti e indietro tra WebMatrix e Visual Studio senza problemi. Se tutti i file sono stati modificati in altro ambiente e devono essere ricaricati per ottenere le modifiche più recenti si riceverà la notifica.

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>La creazione del sito ASP.NET Razor in Visual Studio

Per creare un sito Web ASP.NET Razor in Visual Studio:

1. Avviare Visual Studio o Visual Web Developer.
2. Nel **File** menu, fare clic su **nuovo sito Web**.

    ![Crea nuovo sito web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. Nel **nuovo sito Web** finestra di dialogo, selezionare la lingua da utilizzare (Visual c# o Visual Basic).
4. Selezionare il **sito Web ASP.NET (Razor)** modello.

    ![sito Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. Fare clic su **OK**.

Il nuovo progetto esiste e viene popolato con alcune pagine web predefinito per iniziare.

### <a name="using-intellisense"></a>Using IntelliSense

Dopo aver creato un sito, è possibile visualizzare il funzionamento di IntelliSense in Visual Studio.

1. Nel sito Web appena creato, aprire il *cshtml* pagina.
2. Dopo il `<h3>` tag della pagina, digitare `@ServerInfo.` (incluso il punto). Si noti come IntelliSense consente di visualizzare i metodi disponibili per il `ServerInfo` helper in un elenco a discesa. 

    ![intellisense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. Selezionare il `GetHtml` metodo dall'elenco e quindi premere INVIO. IntelliSense automaticamente nel metodo. (Come con qualsiasi metodo in c#, è necessario aggiungere `()` caratteri dopo il metodo.)  
   Il codice completo per il `GetHtml` metodo è simile alla seguente:  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. Premere Ctrl + F5 per eseguire la pagina. Questo è l'aspetto delle pagine quando visualizzata in un browser: 

    ![pagina predefinita nel browser](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. Chiudere il browser.

### <a name="using-the-debugger"></a>Utilizzo del Debugger

1. Nella parte superiore del *cshtml* pagina dopo la riga che inizia con `Page.Title`, aggiungere la riga di codice seguente: 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. Margine grigio dell'editor a sinistra del codice, fare clic su Avanti questa nuova riga per aggiungere un *punto di interruzione*. Un punto di interruzione è un indicatore che indica al debugger per interrompere l'esecuzione del programma a questo punto è possibile visualizzare ciò che accade.

    ![Set di punti di interruzione](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. Rimuovere la chiamata al `ServerInfo.GetHtml` (metodo) e aggiungere una chiamata al `@myTime` variabile al suo posto. Questa chiamata consente di visualizzare il valore di tempo corrente restituito da una nuova riga di codice.
4. Premere F5 per eseguire la pagina nel debugger. La pagina si interrompe al punto di interruzione impostati. Nella figura seguente mostra la pagina aspetto nell'editor con il punto di interruzione (giallo). 

    ![punto di interruzione di debug](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. Nella barra degli strumenti di Debug, scegliere il **Esegui istruzione** pulsante (o premere F11) per eseguire la riga successiva del codice. Ogni volta che si fa clic su questo pulsante, l'esecuzione si arriva alla riga successiva del codice.

    ![Esegui istruzione pulsante](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. Esaminare il valore del `myTime` variabile posizionando il puntatore del mouse su di esso o controllando i valori visualizzati di **variabili locali** e **Stack di chiamate** windows. In Visual Studio è visualizzato il valore della variabile.

    ![valore di ora Mostra](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. Al termine esaminando la variabile e di avanzamento tramite codice, premere F5 per continuare l'esecuzione della pagina senza interrompere l'esecuzione in ciascuna riga. Una volta terminata l'esecuzione passo passo tutto il codice, il browser visualizza la pagina.

Per ulteriori informazioni sul debugger e su come eseguire il debug di codice in Visual Studio, vedere [procedura dettagliata: debug di pagine Web in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>Utilizzando Razor nei progetti ASP.NET MVC in Visual Studio

La sintassi Razor viene ampiamente utilizzata anche in progetti ASP.NET MVC. MVC è un modo potente, basato su modelli per creare siti Web dinamici. Se il sito di ASP.NET Web Pages diventa difficile da gestire, è consigliabile convertirlo in un'applicazione MVC ASP.NET. Per un esempio di creazione di un'applicazione MVC, vedere [Introduzione a ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>Installazione del supporto per le pagine Web ASP.NET in Visual Studio 2010

In questa sezione viene illustrato come installare Visual Web Developer Express 2010 e gli strumenti di pagine Web ASP.NET per Visual Studio.

1. Se si dispone già l'installazione guidata piattaforma Web, è possibile scaricarlo dall'URL seguente:

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. Eseguire l'installazione guidata piattaforma Web.
3. Fare clic su di **prodotti** scheda.

    ![Scheda prodotti WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. Cercare **ASP.NET MVC 4** (per ASP.NET Web Pages 2) e quindi fare clic su **Aggiungi**. Questi prodotti includono strumenti di Visual Studio per la creazione di siti Web di ASP.NET Razor.

    ![Opzioni di installazione di WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. Fare clic su **installare** per completare l'installazione.
