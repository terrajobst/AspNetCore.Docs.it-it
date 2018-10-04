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
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="d3845-103">Programmazione di ASP.NET Web Pages (Razor) con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3845-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>
====================
<span data-ttu-id="d3845-104">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d3845-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d3845-105">Questo articolo illustra come è possibile usare Visual Studio o Visual Web Developer Express al programma siti Web di ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="d3845-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
>
> <span data-ttu-id="d3845-106">Contenuto dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="d3845-106">What you'll learn</span></span>
>
> - <span data-ttu-id="d3845-107">Che cosa è necessario installare (se qualsiasi elemento) per l'utilizzo con ASP.NET Web Pages nella versione di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d3845-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="d3845-108">Come aggiungere il supporto per ASP.NET Web Pages a Visual Web Developer 2010 Express.</span><span class="sxs-lookup"><span data-stu-id="d3845-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="d3845-109">Come usare le funzionalità in Visual Studio per lavorare con le pagine Razor di ASP.NET, tra cui IntelliSense e il debugger.</span><span class="sxs-lookup"><span data-stu-id="d3845-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d3845-110">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="d3845-110">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="d3845-111">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="d3845-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="d3845-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d3845-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="d3845-113">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="d3845-113">WebMatrix 3</span></span>
>
>
> <span data-ttu-id="d3845-114">Questa esercitazione funziona anche con ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010 e WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="d3845-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>


<span data-ttu-id="d3845-115">È possibile programmare ASP.NET Web pages con sintassi Razor usando WebMatrix o molti altri editor di codice.</span><span class="sxs-lookup"><span data-stu-id="d3845-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="d3845-116">È inoltre possibile utilizzare Microsoft Visual Studio che è un ambiente completo di sviluppo integrato (IDE) che fornisce un potente set di strumenti per la creazione di molti tipi di applicazioni (non solo siti Web).</span><span class="sxs-lookup"><span data-stu-id="d3845-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="d3845-117">Per usare le pagine Razor di ASP.NET, è possibile usare [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span><span class="sxs-lookup"><span data-stu-id="d3845-117">To work with ASP.NET Razor pages, you can use [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span></span>

<span data-ttu-id="d3845-118">Due funzionalità particolarmente utile fornite da Visual Studio per la programmazione con le pagine web ASP.NET Razor sono:</span><span class="sxs-lookup"><span data-stu-id="d3845-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="d3845-119">*IntelliSense*.</span><span class="sxs-lookup"><span data-stu-id="d3845-119">*IntelliSense*.</span></span> <span data-ttu-id="d3845-120">La funzionalità IntelliSense incorporata in Visual Studio è più completa di IntelliSense in WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="d3845-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="d3845-121">*Debugger*.</span><span class="sxs-lookup"><span data-stu-id="d3845-121">*Debugger*.</span></span> <span data-ttu-id="d3845-122">Il debugger consente di risolvere i problemi del codice mediante l'arresto di un programma mentre è in esecuzione, esaminano le variabili e scorrere il codice riga per riga.</span><span class="sxs-lookup"><span data-stu-id="d3845-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="d3845-123">Usare Visual Studio con versioni diverse delle pagine Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d3845-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="d3845-124">Per sviluppare le app web ASP.NET in Visual Studio 2017, installare il **sviluppo ASP.NET e web** carico di lavoro.</span><span class="sxs-lookup"><span data-stu-id="d3845-124">To develop ASP.NET web apps in Visual Studio 2017, install the **ASP.NET and web development** workload.</span></span>

<span data-ttu-id="d3845-125">Visual Studio 2012 e Visual Studio 2013 includono il supporto per ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="d3845-125">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="d3845-126">(I pacchetti che sono necessari per supportare le pagine Web ASP.NET vengono installati quando si installa Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="d3845-126">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="d3845-127">Visual Studio 2010 non include il supporto per impostazione predefinita per ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="d3845-127">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="d3845-128">Per usare ASP.NET Web Pages con Visual Studio 2010, è necessario installare il pacchetto di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d3845-128">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="d3845-129">Per ottenere ASP.NET Web Pages 2, si installa ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="d3845-129">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="d3845-130">La tabella seguente riepiloga il supporto per ASP.NET Web Pages in versioni diverse di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d3845-130">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="d3845-131">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="d3845-131">Visual Studio 2010</span></span> | <span data-ttu-id="d3845-132">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="d3845-132">Visual Studio 2012</span></span> | <span data-ttu-id="d3845-133">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d3845-133">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="d3845-134">**ASP.NET Web Pages 2**</span><span class="sxs-lookup"><span data-stu-id="d3845-134">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="d3845-135">Installare ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="d3845-135">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="d3845-136">(Inclusi)</span><span class="sxs-lookup"><span data-stu-id="d3845-136">(Included)</span></span> | <span data-ttu-id="d3845-137">(Inclusi)</span><span class="sxs-lookup"><span data-stu-id="d3845-137">(Included)</span></span> |
| <span data-ttu-id="d3845-138">**Pagine Web ASP.NET 3**</span><span class="sxs-lookup"><span data-stu-id="d3845-138">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="d3845-139">Aggiornamento ad ASP.NET Web Pages 3 tramite NuGet</span><span class="sxs-lookup"><span data-stu-id="d3845-139">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="d3845-140">(Inclusi)</span><span class="sxs-lookup"><span data-stu-id="d3845-140">(Included)</span></span> |

<span data-ttu-id="d3845-141">Per lavorare con Visual Studio 2010, vedere [installazione del supporto per pagine Web ASP.NET in Visual Studio 2010](#vs2010support).</span><span class="sxs-lookup"><span data-stu-id="d3845-141">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="d3845-142">Avviare Visual Studio da WebMatrix</span><span class="sxs-lookup"><span data-stu-id="d3845-142">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="d3845-143">Se si è iniziato un progetto in WebMatrix e tornare a Visual Studio, WebMatrix ti offre un pulsante per aprire facilmente il progetto in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d3845-143">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="d3845-144">È necessario disporre di Visual Studio installata nel computer di questo pulsante di abilitazione.</span><span class="sxs-lookup"><span data-stu-id="d3845-144">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="d3845-145">L'immagine seguente mostra il pulsante in WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="d3845-145">The following image shows the button in WebMatrix.</span></span>

![Avviare Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="d3845-147">Quando si fa clic sul pulsante, il progetto viene aperto in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d3845-147">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="d3845-148">È possibile passare alternativamente tra WebMatrix e Visual Studio senza problemi.</span><span class="sxs-lookup"><span data-stu-id="d3845-148">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="d3845-149">Verrà informati se tutti i file sono stati modificati in altro ambiente e devono essere ricaricati per ottenere le modifiche più recenti.</span><span class="sxs-lookup"><span data-stu-id="d3845-149">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="d3845-150">La creazione del sito ASP.NET Razor in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3845-150">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="d3845-151">Per creare un sito Web di Razor di ASP.NET in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="d3845-151">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="d3845-152">Aprire Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d3845-152">Open Visual Studio.</span></span>
2. <span data-ttu-id="d3845-153">Nel **File** menu, fare clic su **nuovo sito Web**.</span><span class="sxs-lookup"><span data-stu-id="d3845-153">In the **File** menu, click **New Web Site**.</span></span>

    ![Crea nuovo sito web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="d3845-155">Nel **nuovo sito Web** finestra di dialogo, selezionare la lingua da utilizzare (Visual c# o Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="d3845-155">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="d3845-156">Selezionare il **sito Web ASP.NET (Razor)** modello.</span><span class="sxs-lookup"><span data-stu-id="d3845-156">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![sito Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="d3845-158">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="d3845-158">Click **OK**.</span></span>

<span data-ttu-id="d3845-159">Il nuovo progetto esiste e viene popolato con alcune pagine web predefiniti che consentono di iniziare.</span><span class="sxs-lookup"><span data-stu-id="d3845-159">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="d3845-160">Using IntelliSense</span><span class="sxs-lookup"><span data-stu-id="d3845-160">Using IntelliSense</span></span>

<span data-ttu-id="d3845-161">Ora che è stato creato un sito, è possibile visualizzare il funzionamento di IntelliSense in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d3845-161">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="d3845-162">Nel sito Web appena creata, aprire il *cshtml* pagina.</span><span class="sxs-lookup"><span data-stu-id="d3845-162">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="d3845-163">Dopo il `<h3>` tag nella pagina digitare `@ServerInfo.` (incluso il punto).</span><span class="sxs-lookup"><span data-stu-id="d3845-163">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="d3845-164">Si noti come IntelliSense visualizza i metodi disponibili per il `ServerInfo` helper in un elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="d3845-164">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span>

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="d3845-166">Selezionare il `GetHtml` metodo dall'elenco e quindi premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="d3845-166">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="d3845-167">IntelliSense viene compilato automaticamente nel metodo.</span><span class="sxs-lookup"><span data-stu-id="d3845-167">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="d3845-168">(Come con qualsiasi metodo in c#, è necessario aggiungere `()` caratteri dopo il metodo.) Il codice completo per il `GetHtml` metodo simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="d3845-168">(As with any method in C#, you must add `()` characters after the method.) The completed code for the `GetHtml` method looks like the following example:</span></span>

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="d3845-169">Premere CTRL+F5 per eseguire la pagina.</span><span class="sxs-lookup"><span data-stu-id="d3845-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="d3845-170">Questo è l'aspetto delle pagine quando visualizzata in un browser:</span><span class="sxs-lookup"><span data-stu-id="d3845-170">This is what the page looks like when displayed in a browser:</span></span>

    ![pagina predefinita nel browser](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="d3845-172">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="d3845-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="d3845-173">Uso del Debugger</span><span class="sxs-lookup"><span data-stu-id="d3845-173">Using the Debugger</span></span>

1. <span data-ttu-id="d3845-174">In cima il *default. cshtml* pagina, dopo la riga che inizia con `Page.Title`, aggiungere la riga di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="d3845-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span>

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="d3845-175">Sul margine grigio dell'editor a sinistra del codice, fare clic accanto a questa nuova riga per aggiungere un *punto di interruzione*.</span><span class="sxs-lookup"><span data-stu-id="d3845-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="d3845-176">Un punto di interruzione è un indicatore che indica al debugger di interrompere l'esecuzione del programma a questo punto, è possibile visualizzare ciò che accade.</span><span class="sxs-lookup"><span data-stu-id="d3845-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![Imposta punto di interruzione](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="d3845-178">Rimuovere la chiamata ai `ServerInfo.GetHtml` metodo e aggiungere una chiamata al `@myTime` variabile al suo posto.</span><span class="sxs-lookup"><span data-stu-id="d3845-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="d3845-179">Questa chiamata consente di visualizzare il valore di tempo corrente restituito dalla nuova riga di codice.</span><span class="sxs-lookup"><span data-stu-id="d3845-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="d3845-180">Premere F5 per eseguirla nel debugger.</span><span class="sxs-lookup"><span data-stu-id="d3845-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="d3845-181">La pagina si interrompe al punto di interruzione impostato.</span><span class="sxs-lookup"><span data-stu-id="d3845-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="d3845-182">L'immagine seguente mostra la pagina aspetto nell'editor con il punto di interruzione (giallo).</span><span class="sxs-lookup"><span data-stu-id="d3845-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span>

    ![punto di interruzione di debug](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="d3845-184">Nella barra degli strumenti di Debug, scegliere il **Esegui istruzione** (o preme F11) per eseguire la riga successiva del codice.</span><span class="sxs-lookup"><span data-stu-id="d3845-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="d3845-185">Ogni volta che si fa clic su questo pulsante, l'esecuzione si arriva alla riga successiva del codice.</span><span class="sxs-lookup"><span data-stu-id="d3845-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![Eseguire l'istruzione pulsante](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="d3845-187">Esaminare il valore della `myTime` variabile posizionando il puntatore del mouse su di esso o esaminando i valori visualizzati nei **variabili locali** e **Stack di chiamate** windows.</span><span class="sxs-lookup"><span data-stu-id="d3845-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="d3845-188">Visual Studio visualizza il valore della variabile.</span><span class="sxs-lookup"><span data-stu-id="d3845-188">Visual Studio display the value of the variable.</span></span>

    ![valore di ora Show](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="d3845-190">Dopo aver completato la variabile di analisi e l'avanzamento tramite codice, premere F5 per continuare l'esecuzione della pagina senza interruzione in corrispondenza di ogni riga.</span><span class="sxs-lookup"><span data-stu-id="d3845-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="d3845-191">Al termine scorrere tutto il codice, il browser visualizza la pagina.</span><span class="sxs-lookup"><span data-stu-id="d3845-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="d3845-192">Per altre informazioni sul debugger e su come eseguire il debug di codice in Visual Studio, vedere [procedura dettagliata: debug di pagine Web in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span><span class="sxs-lookup"><span data-stu-id="d3845-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="d3845-193">Uso di Razor in progetti ASP.NET MVC con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3845-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="d3845-194">La sintassi Razor anche è ampiamente usata in progetti ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d3845-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="d3845-195">MVC è un modo potente, basato su modelli per creare siti Web dinamici.</span><span class="sxs-lookup"><span data-stu-id="d3845-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="d3845-196">Se il sito Web ASP.NET Web Pages è più difficile da gestire, è possibile provare a convertirlo in un'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d3845-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="d3845-197">Per un esempio di creazione di un'applicazione MVC, vedere [Introduzione a ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="d3845-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="d3845-198">Installazione del supporto per le pagine Web ASP.NET in Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="d3845-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="d3845-199">In questa sezione viene illustrato come installare Visual Web Developer Express 2010 e gli strumenti di pagine Web ASP.NET per Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d3845-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="d3845-200">Se si ha già l'installazione guidata piattaforma Web, scaricarlo dall'URL seguente:</span><span class="sxs-lookup"><span data-stu-id="d3845-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="d3845-201">Eseguire l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="d3845-201">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="d3845-202">Scegliere il **prodotti** scheda.</span><span class="sxs-lookup"><span data-stu-id="d3845-202">Click the **Products** tab.</span></span>

    ![Scheda prodotti WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="d3845-204">Cercare **ASP.NET MVC 4** (per ASP.NET Web Pages 2) e quindi fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="d3845-204">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="d3845-205">Questi prodotti includono gli strumenti di Visual Studio per la creazione di siti Web di ASP.NET Razor.</span><span class="sxs-lookup"><span data-stu-id="d3845-205">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![Opzioni di installazione di WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="d3845-207">Fare clic su **installare** per completare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="d3845-207">Click **Install** to complete the installation.</span></span>
