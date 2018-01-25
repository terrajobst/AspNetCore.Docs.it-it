---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Con Page Inspector per Visual Studio 2012, in ASP.NET Web Forms | Documenti Microsoft
author: rick-anderson
description: "Controllo pagina per Visual Studio 2012 è uno strumento di sviluppo web con un browser integrato. Selezionare un elemento nel browser integrato e controllo pagina..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: ca8a3c194577766e56d0604323fef567d539316c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a><span data-ttu-id="8d401-104">Con Page Inspector per Visual Studio 2012 in Web Form ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8d401-104">Using Page Inspector for Visual Studio 2012 in ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="8d401-105">da Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="8d401-105">by Tim Ammann</span></span>

> <span data-ttu-id="8d401-106">Controllo pagina per Visual Studio 2012 è uno strumento di sviluppo web con un browser integrato.</span><span class="sxs-lookup"><span data-stu-id="8d401-106">Page Inspector for Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="8d401-107">Selezionare un elemento nel browser integrato e controllo pagina evidenzia immediatamente l'origine dell'elemento e CSS.</span><span class="sxs-lookup"><span data-stu-id="8d401-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="8d401-108">È possibile passare qualsiasi pagina dell'applicazione, rapidamente trovare le origini di markup sottoposto a rendering e gli strumenti del visualizzatore a destra all'interno dell'ambiente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8d401-108">You can browse any page in your application, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> <span data-ttu-id="8d401-109">Questa esercitazione shwos come abilitare la modalità di controllo e quindi individuare e modificare rapidamente le regole CSS e testo all'interno del progetto web.</span><span class="sxs-lookup"><span data-stu-id="8d401-109">This tutorial shwos how to enable Inspection Mode and then quickly locate and edit CSS rules and text within your web project.</span></span> <span data-ttu-id="8d401-110">L'esercitazione Usa un progetto di applicazione Web Form, ma è anche possibile utilizzare controllo pagina per i progetti di sito Web e [MVC](https://go.microsoft.com/?linkid=9802002) applicazioni.</span><span class="sxs-lookup"><span data-stu-id="8d401-110">The tutorial uses a Web Forms Application Project, but you can also use Page Inspector for Web Site projects and [MVC](https://go.microsoft.com/?linkid=9802002) applications.</span></span>
> 
> <span data-ttu-id="8d401-111">L'esercitazione include le sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="8d401-111">The tutorial has the following sections:</span></span>
> 
> [<span data-ttu-id="8d401-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="8d401-112">Prerequisites</span></span>](#_1_prerequisites)
> 
> [<span data-ttu-id="8d401-113">Creare un'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="8d401-113">Create a Web Application</span></span>](#_2_creating_a)
> 
> [<span data-ttu-id="8d401-114">Utilizza controllo pagina per visualizzare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="8d401-114">Use Page Inspector to View the Application</span></span>](#_3_using_page)
> 
> [<span data-ttu-id="8d401-115">Abilitare la modalità di controllo</span><span class="sxs-lookup"><span data-stu-id="8d401-115">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> 
> [<span data-ttu-id="8d401-116">Utilizza controllo pagina per apportare modifiche al Markup</span><span class="sxs-lookup"><span data-stu-id="8d401-116">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> 
> [<span data-ttu-id="8d401-117">Modalità di ispezione e la finestra di HTML</span><span class="sxs-lookup"><span data-stu-id="8d401-117">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> 
> [<span data-ttu-id="8d401-118">Anteprima modifiche CSS nella finestra stili</span><span class="sxs-lookup"><span data-stu-id="8d401-118">Preview CSS Changes in the Styles Window</span></span>](#_7_previewing_css)
> 
> [<span data-ttu-id="8d401-119">Sincronizzazione automatica CSS</span><span class="sxs-lookup"><span data-stu-id="8d401-119">CSS Auto Sync</span></span>](#css_auto_sync)
> 
> [<span data-ttu-id="8d401-120">Mediante il selettore del colore CSS</span><span class="sxs-lookup"><span data-stu-id="8d401-120">Using the CSS Color Picker</span></span>](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="8d401-121">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="8d401-121">Prerequisites</span></span>

- <span data-ttu-id="8d401-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) o [Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="8d401-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="8d401-123">Per ottenere la versione più recente di controllo pagina, utilizzare [installazione guidata piattaforma Web](https://go.microsoft.com/fwlink/?LinkId=255386) per installare Azure SDK per .NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="8d401-123">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="8d401-124">Controllo pagina viene fornito con Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="8d401-124">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="8d401-125">La versione più recente è 1.3.</span><span class="sxs-lookup"><span data-stu-id="8d401-125">The latest version is 1.3.</span></span> <span data-ttu-id="8d401-126">Per verificare la versione hanno, eseguire Visual Studio e selezionare **su Microsoft Visual Studio** dal **Guida** menu.</span><span class="sxs-lookup"><span data-stu-id="8d401-126">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="8d401-127">Creare un'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="8d401-127">Create a Web Application</span></span>

<span data-ttu-id="8d401-128">Creare innanzitutto un'applicazione web che verrà utilizzato il controllo pagina con.</span><span class="sxs-lookup"><span data-stu-id="8d401-128">First, you will create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="8d401-129">In Visual Studio, scegliere **File** &gt; **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="8d401-129">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="8d401-130">A sinistra, espandere **Visual c#**selezionare **Web**, quindi selezionare **applicazioni Web Form ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="8d401-130">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET Web Forms Application**.</span></span>

![Nuova applicazione Web Form](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

<span data-ttu-id="8d401-132">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="8d401-132">Click **OK**.</span></span>

<span data-ttu-id="8d401-133">L'applicazione verrà visualizzata in **origine** visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="8d401-133">The application opens in **Source** view.</span></span>

![Nuova applicazione di Web Form in visualizzazione origine](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

<span data-ttu-id="8d401-135">Dopo aver creato un'applicazione da utilizzare, è possibile utilizzare controllo pagina per esaminare e modificare.</span><span class="sxs-lookup"><span data-stu-id="8d401-135">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a><span data-ttu-id="8d401-136">Utilizza controllo pagina per visualizzare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="8d401-136">Use Page Inspector to View the Application</span></span>

<span data-ttu-id="8d401-137">Successivamente, si visualizzeranno l'applicazione con controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="8d401-137">Next, you will view the application with Page Inspector.</span></span> <span data-ttu-id="8d401-138">In **Esplora**, fare clic con il pulsante destro del progetto e quindi scegliere **Visualizza in controllo pagina**.</span><span class="sxs-lookup"><span data-stu-id="8d401-138">In **Solution Explorer**, right click the project, and then choose **View in Page Inspector**.</span></span>

![Visualizza in controllo pagina](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

<span data-ttu-id="8d401-140">Per impostazione predefinita, quando viene avviata controllo pagina per la prima volta, è ancorata come una piccola finestra sul lato sinistro dell'ambiente Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8d401-140">By default, when Page Inspector launches for the first time, it is docked as a narrow window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="8d401-141">Lasciarla ancorata a sinistra e impostarla su una larghezza che secondo necessità per l'utente o in una delle aree strumento ancorarla nella parte superiore, inferiore o destro:</span><span class="sxs-lookup"><span data-stu-id="8d401-141">Leave it docked on the left side and set it to a width that is comfortable for you, or dock it in one of the tool areas on the top, bottom, or right:</span></span>

![Posizione di ancoraggio di controllo pagina](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

<span data-ttu-id="8d401-143">Se si disancorare la finestra di controllo pagina, è possibile inserirlo all'esterno di Visual Studio o in un secondo monitor se si dispone di uno.</span><span class="sxs-lookup"><span data-stu-id="8d401-143">If you undock the Page Inspector window, you can place it outside Visual Studio, or even on a second monitor if you have one.</span></span> <span data-ttu-id="8d401-144">Tuttavia, in ordine a ALT + TAB tra Visual Studio e controllo pagina quando la finestra di controllo pagina è ancorata, passare a **strumenti** &gt; **opzioni** &gt;  **Ambiente** &gt; **schede e finestre**e in **scheda anche**, deselezionare la casella di controllo denominata **sempre le finestre degli strumenti mobile in cima il finestra principale**:</span><span class="sxs-lookup"><span data-stu-id="8d401-144">However, in order to ALT+TAB between Page Inspector and Visual Studio when the Page Inspector window is undocked, go to **Tools** &gt; **Options** &gt; **Environment** &gt; **Tabs and Windows**, and under **Tab Well**, clear the check box called **Floating tool windows always stay on top of the main window**:</span></span>

![Deselezionare la casella di windows mobile strumento per ALT + TAB tra Visual Studio e la finestra di controllo pagina non ancorata](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

<span data-ttu-id="8d401-146">Nel riquadro superiore della finestra di controllo pagina Mostra la pagina corrente in una finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="8d401-146">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="8d401-147">Nel riquadro inferiore mostra la pagina nel markup HTML a sinistra e alcune schede a destra che consentono di controllare aspetti diversi della pagina.</span><span class="sxs-lookup"><span data-stu-id="8d401-147">The bottom pane shows the page in HTML markup on the left, and some tabs on the right that let you inspect different aspects of the page.</span></span> <span data-ttu-id="8d401-148">Il riquadro inferiore è simile al [strumenti di sviluppo F12](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="8d401-148">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span> <span data-ttu-id="8d401-149">(Tuttavia, diversamente da strumenti di sviluppo, è possibile utilizzare il controllo pagina destra all'interno di Visual Studio.)</span><span class="sxs-lookup"><span data-stu-id="8d401-149">(However, unlike the developer tools, you can use Page Inspector right within Visual Studio.)</span></span>

![Controllo pagina](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

<span data-ttu-id="8d401-151">In questa esercitazione si utilizzerà il riquadro del browser di controllo pagina e **HTML** e **stili** schede che consentono di effettuare rapidamente passare e apportare modifiche all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8d401-151">In this tutorial, you will use the Page Inspector browser pane, and the **HTML** and **Styles** tabs to help you rapidly navigate and make changes to the application.</span></span>

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a><span data-ttu-id="8d401-152">Abilitare la modalità di controllo</span><span class="sxs-lookup"><span data-stu-id="8d401-152">Enable Inspection Mode</span></span>

<span data-ttu-id="8d401-153">Successivamente, verrà visualizzato come modalità di controllo di controllo pagina funziona.</span><span class="sxs-lookup"><span data-stu-id="8d401-153">Next, you will see how Page Inspector's Inspection Mode works.</span></span> <span data-ttu-id="8d401-154">Nella finestra di controllo pagina, fare clic su di **controlla** pulsante.</span><span class="sxs-lookup"><span data-stu-id="8d401-154">In the Page Inspector window, click the **Inspect** button.</span></span>

![Controllare l'elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

<span data-ttu-id="8d401-156">Per visualizzare le modalità di controllo in azione, spostare il puntatore del mouse su parti diverse della pagina nella finestra del browser di controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="8d401-156">To see inspection mode in action, move your mouse over different parts of the page within the Page Inspector browser window.</span></span> <span data-ttu-id="8d401-157">Quando si esegue, il puntatore del mouse diventa un segno più grande e viene evidenziato l'elemento sotto:</span><span class="sxs-lookup"><span data-stu-id="8d401-157">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Mouse div.content wrapper](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

<span data-ttu-id="8d401-159">Quando si sposta il puntatore del mouse, si noti che</span><span class="sxs-lookup"><span data-stu-id="8d401-159">As you move the mouse pointer, note that</span></span>

- <span data-ttu-id="8d401-160">Il contenuto di **origine** visualizzare cambia per mostrare il markup corrispondente all'elemento selezionato nella pagina.</span><span class="sxs-lookup"><span data-stu-id="8d401-160">The content in **Source** view changes to show the markup corresponding to the selected element on the page.</span></span> <span data-ttu-id="8d401-161">Il markup pertinente viene evidenziato.</span><span class="sxs-lookup"><span data-stu-id="8d401-161">The relevant markup is highlighted.</span></span> <span data-ttu-id="8d401-162">Se l'origine è in un altro file, il file è aperto nella visualizzazione origine con il markup pertinente evidenziato.</span><span class="sxs-lookup"><span data-stu-id="8d401-162">If the source is in another file, that file is opened in Source view with the relevant markup highlighted.</span></span>

- <span data-ttu-id="8d401-163">Il markup visualizzato nel **HTML** scheda in controllo pagina viene modificata anche in modo che corrisponda all'elemento selezionato nella pagina.</span><span class="sxs-lookup"><span data-stu-id="8d401-163">The markup displayed in the **HTML** tab in Page Inspector also changes to correspond to the selected element on the page.</span></span> <span data-ttu-id="8d401-164">Nel **HTML** scheda, viene indicato il codice pertinente.</span><span class="sxs-lookup"><span data-stu-id="8d401-164">In the **HTML** tab, the relevant markup is outlined.</span></span>

- <span data-ttu-id="8d401-165">Il **stili** scheda Visualizza le regole CSS pertinenti per la selezione corrente.</span><span class="sxs-lookup"><span data-stu-id="8d401-165">The **Styles** tab shows the CSS rules relevant to the current selection.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="8d401-166">Utilizza controllo pagina per apportare modifiche al Markup</span><span class="sxs-lookup"><span data-stu-id="8d401-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="8d401-167">Verrà visualizzata come è possibile utilizzare controllo pagina per trovare e apportare modifiche al markup o testo la cui posizione potrebbe non essere immediatamente evidente.</span><span class="sxs-lookup"><span data-stu-id="8d401-167">Now you will see how you can use Page Inspector to find and make changes to markup or text whose location might not be immediately obvious.</span></span>

<span data-ttu-id="8d401-168">Inserire il controllo pagina in modalità controllo e quindi scorrere fino alla fine della home page.</span><span class="sxs-lookup"><span data-stu-id="8d401-168">Put Page Inspector in Inspection Mode and then scroll to the bottom of the home page.</span></span>

<span data-ttu-id="8d401-169">Non appena si immette l'area di piè di pagina, vengono aperti il *Site. master* file di layout in **origine** vista in una scheda temporanea a destra delle altre schede ed evidenzia la sezione del master pagina che si stato selezionato.</span><span class="sxs-lookup"><span data-stu-id="8d401-169">As soon as you enter the footer area, Page Inspector opens the *Site.Master* layout file in **Source** view in a temporary tab to the right of the other tabs and highlights the section of the master page that you have selected.</span></span> <span data-ttu-id="8d401-170">Verrà visualizzata come controllo pagina è possibile trovare e visualizzare il contenuto in una pagina che effettivamente potrebbe provenire da un file diverso da quello che aperto originariamente.</span><span class="sxs-lookup"><span data-stu-id="8d401-170">This shows you how Page Inspector can find and display content on a page that might actually come from a different file than the one you originally opened.</span></span>

![Caratteristiche salienti di piè di pagina in modalità controllo](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

<span data-ttu-id="8d401-172">Nella finestra del browser di controllo pagina, spostare il puntatore del mouse sulla linea con il copyright <a id="a"> </a>nota.</span><span class="sxs-lookup"><span data-stu-id="8d401-172">In the Page Inspector browser window, move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span>

<span data-ttu-id="8d401-173">Nel *Site. master* pagina, la riga corrispondente viene evidenziato.</span><span class="sxs-lookup"><span data-stu-id="8d401-173">In the *Site.Master* page, the corresponding line is highlighted.</span></span>

![Riga di piè di pagina copyright evidenziata](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

<span data-ttu-id="8d401-175">Aggiungere testo alla fine della riga di *Site. master* file.</span><span class="sxs-lookup"><span data-stu-id="8d401-175">Add some text to the end of the line in the *Site.Master* file.</span></span>

<span data-ttu-id="8d401-176">&lt;p&gt;&amp;copiare; &lt;%: % DateTime.Now.Year&gt; -My Massi applicazione ASP.NET!&lt; / p&gt;</span><span class="sxs-lookup"><span data-stu-id="8d401-176">&lt;p&gt;&amp;copy; &lt;%: DateTime.Now.Year %&gt; - My ASP.NET Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="8d401-177">A questo punto, premere Ctrl + Alt + Invio oppure fare clic sulla barra di aggiornamento per visualizzare i risultati nella finestra del browser di controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="8d401-177">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![L'applicazione ASP.NET funziona](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

<span data-ttu-id="8d401-179">Si potrebbe pensare che il piè di pagina è stato nel *Default.aspx* pagina, ma non è risultato per essere nella pagina di layout master e controllo pagina è stato trovato per l'utente.</span><span class="sxs-lookup"><span data-stu-id="8d401-179">You might have thought that the footer was on the *Default.aspx* page, but it turned out to be in the master layout page, and Page Inspector found it for you.</span></span>

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="8d401-180">Modalità di ispezione e la finestra di HTML</span><span class="sxs-lookup"><span data-stu-id="8d401-180">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="8d401-181">Successivamente, si avrà un rapido controllo di finestra di HTML e come viene eseguito il mapping elementi per l'utente.</span><span class="sxs-lookup"><span data-stu-id="8d401-181">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="8d401-182">Inserire il controllo pagina in modalità controllo.</span><span class="sxs-lookup"><span data-stu-id="8d401-182">Put Page Inspector in Inspection Mode.</span></span>

![Controllare l'elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

<span data-ttu-id="8d401-184">Fare clic sulla parte superiore della pagina su cui è indicato "inserire qui il logo".</span><span class="sxs-lookup"><span data-stu-id="8d401-184">Click the top part of the page that says "your logo here".</span></span> <span data-ttu-id="8d401-185">Si sta esaminando un particolare elemento in modo più dettagliato, pertanto la visualizzazione nella finestra del browser non cambierà quando si sposta il puntatore del mouse.</span><span class="sxs-lookup"><span data-stu-id="8d401-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="8d401-186">Spostare il puntatore del mouse per il **HTML** finestra.</span><span class="sxs-lookup"><span data-stu-id="8d401-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="8d401-187">Quando si sposta il puntatore del mouse, controllo pagina vengono descritti l'elemento all'interno di **HTML** finestra ed evidenziare l'elemento corrispondente nella finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="8d401-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Finestra HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

<span data-ttu-id="8d401-189">Come in precedenza, vengono aperti il *Site. master* file in una scheda temporanea. Fare clic sulla scheda Site. master, e il markup corrispondente viene evidenziato nel &lt;intestazione&gt; sezione:</span><span class="sxs-lookup"><span data-stu-id="8d401-189">As before, Page Inspector opens the *Site.Master* file for you in a temporary tab. Click the Site.Master tab, and the corresponding markup is highlighted in the &lt;header&gt; section:</span></span>

![Markup evidenziato](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="8d401-191">Anteprima modifiche CSS nella finestra stili</span><span class="sxs-lookup"><span data-stu-id="8d401-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="8d401-192">Successivamente, verrà visualizzato come è possibile utilizzare il controllo pagina **stili** finestra per visualizzare in anteprima le modifiche a CSS.</span><span class="sxs-lookup"><span data-stu-id="8d401-192">Next, you will see how you can use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="8d401-193">Fare clic su di **controlla** pulsante da inserire controllo pagina in modalità di controllo.</span><span class="sxs-lookup"><span data-stu-id="8d401-193">Click the **Inspect** button to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="8d401-194">Nella finestra del browser di controllo pagina, spostare il puntatore del mouse sopra la sezione "Home Page" finché il **div.content wrapper** etichetta viene visualizzata.</span><span class="sxs-lookup"><span data-stu-id="8d401-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Passaggio del mouse sugli elementi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

<span data-ttu-id="8d401-196">Fare clic una volta all'interno della sezione div.content wrapper, e quindi spostare il puntatore del mouse per il **stili** finestra.</span><span class="sxs-lookup"><span data-stu-id="8d401-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="8d401-197">Nel selettore di classe .featured. Content wrapper, deselezionare e selezionare la casella di controllo per la proprietà del colore di sfondo.</span><span class="sxs-lookup"><span data-stu-id="8d401-197">Under the .featured .content-wrapper class selector, clear and select the checkbox for the background-color property.</span></span>

![Colore di sfondo cancella](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

<span data-ttu-id="8d401-199">Si noti come la modifica immediata anteprime nella finestra del browser di controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="8d401-199">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="8d401-200">Selezionare la casella di controllo di nuovo, quindi fare doppio clic sul valore della proprietà e impostarla su `red`.</span><span class="sxs-lookup"><span data-stu-id="8d401-200">Select the checkbox again, then double-click the property value and change it to `red`.</span></span> <span data-ttu-id="8d401-201">La modifica viene immediatamente:</span><span class="sxs-lookup"><span data-stu-id="8d401-201">The change shows immediately:</span></span>

![Colore di sfondo rosso](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

<span data-ttu-id="8d401-203">Il **stili** rende finestra è più facile di test e visualizzare in anteprima CSS modifiche prima di applicare le modifiche allo stile di finestra stessa.</span><span class="sxs-lookup"><span data-stu-id="8d401-203">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="8d401-204">Sincronizzazione automatica CSS</span><span class="sxs-lookup"><span data-stu-id="8d401-204">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="8d401-205">Questa funzionalità richiede la versione 1.3 del controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="8d401-205">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="8d401-206">La funzionalità di sincronizzazione automatica CSS consente di modificare direttamente un file CSS e visualizzare le modifiche immediatamente nel browser di controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="8d401-206">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="8d401-207">Fare clic su **controlla** inserire controllo pagina in modalità controllo.</span><span class="sxs-lookup"><span data-stu-id="8d401-207">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="8d401-208">Nel browser di controllo pagina, spostare il puntatore del mouse sopra la sezione "Home Page" finché il **div.content wrapper** etichetta viene visualizzata.</span><span class="sxs-lookup"><span data-stu-id="8d401-208">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="8d401-209">Fare clic per selezionare questo elemento.</span><span class="sxs-lookup"><span data-stu-id="8d401-209">Click once to select this element.</span></span>

<span data-ttu-id="8d401-210">Il **stili** finestra Visualizza tutte le regole CSS per questo elemento.</span><span class="sxs-lookup"><span data-stu-id="8d401-210">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="8d401-211">Scorrere fino a individuare il wrapper. Content .featured selettore di classe.</span><span class="sxs-lookup"><span data-stu-id="8d401-211">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="8d401-212">Fare clic su ".featured. Content-wrapper".</span><span class="sxs-lookup"><span data-stu-id="8d401-212">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="8d401-213">Controllo pagina consente di aprire il file CSS che definisce questo stile (Site.css) ed evidenzia il CSS corrispondente.</span><span class="sxs-lookup"><span data-stu-id="8d401-213">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![File CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

<span data-ttu-id="8d401-215">Modificare il valore per `background-color` per "red".</span><span class="sxs-lookup"><span data-stu-id="8d401-215">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="8d401-216">La modifica viene visualizzata immediatamente nel browser di controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="8d401-216">The change appears immediately in the Page Inspector browser.</span></span>

![Browser di controllo pagina](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a><span data-ttu-id="8d401-218">Mediante il selettore del colore CSS</span><span class="sxs-lookup"><span data-stu-id="8d401-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="8d401-219">Successivamente, si apprenderà come usare controllo pagina per trovare rapidamente e modificare il codice CSS per il testo evidenziato nell'applicazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="8d401-219">Next, you'll learn how to use Page Inspector to quickly find and change the CSS for highlighted text in the default application.</span></span> <span data-ttu-id="8d401-220">In questo esempio, si è deciso di non desidera che l'evidenziazione blu e modificarla in un altro colore.</span><span class="sxs-lookup"><span data-stu-id="8d401-220">In this example, you've decided that you don't like the blue highlighting and want to change it to another color.</span></span>

<span data-ttu-id="8d401-221">Fare clic su di **controlla** pulsante.</span><span class="sxs-lookup"><span data-stu-id="8d401-221">Click the **Inspect** button.</span></span>

![Controllare l'elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

<span data-ttu-id="8d401-223">Nella finestra del browser di controllo pagina, spostare il puntatore del mouse su evidenziata "video, esercitazioni ed esempi" viene visualizzato il testo in modo che il codice CSS "contrassegna" etichetta.</span><span class="sxs-lookup"><span data-stu-id="8d401-223">In the Page Inspector browser window, move the mouse pointer over the highlighted "videos, tutorials, and samples" text so that the CSS "mark" label appears.</span></span>

![Passare il mouse sopra l'elemento contrassegno](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

<span data-ttu-id="8d401-225">Fare clic sul testo per selezionarlo.</span><span class="sxs-lookup"><span data-stu-id="8d401-225">Click the text to select it.</span></span> <span data-ttu-id="8d401-226">Il selettore di contrassegno CSS corrispondente viene visualizzato in fondo il **stili** finestra.</span><span class="sxs-lookup"><span data-stu-id="8d401-226">The corresponding CSS mark selector appears at the bottom of the **Styles** window.</span></span>

![selettore di spunta nella finestra stili](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

<span data-ttu-id="8d401-228">Fare clic sul selettore di contrassegno.</span><span class="sxs-lookup"><span data-stu-id="8d401-228">Click the mark selector.</span></span> <span data-ttu-id="8d401-229">Verrà visualizzata la *Site* file per l'applicazione web.</span><span class="sxs-lookup"><span data-stu-id="8d401-229">This opens the *Site.css* file for the web application.</span></span> <span data-ttu-id="8d401-230">Fare clic sulla scheda Site e per il selettore CSS corrispondente viene evidenziato:</span><span class="sxs-lookup"><span data-stu-id="8d401-230">Click the Site.css tab, and the corresponding CSS for the selector is highlighted:</span></span>

![Contrassegna selezione nel foglio di stile](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

<span data-ttu-id="8d401-232">Selezionare e rimuovere la riga con la proprietà del colore di sfondo.</span><span class="sxs-lookup"><span data-stu-id="8d401-232">Select and remove the line with the background-color property.</span></span>

<span data-ttu-id="8d401-233">Verrà utilizzata la nuova selezione colori di Visual Studio 2012 CSS per scegliere un nuovo colore per il **contrassegnare** proprietà colore di sfondo.</span><span class="sxs-lookup"><span data-stu-id="8d401-233">You will now use the new Visual Studio 2012 CSS color picker to choose a new color for the **mark** background-color property.</span></span>

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a><span data-ttu-id="8d401-234">Mediante il selettore di colore CSS di Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="8d401-234">Using the Visual Studio 2012 CSS Color Picker</span></span>

<span data-ttu-id="8d401-235">L'editor CSS in Visual Studio 2012 è una selezione di colori che rende più semplice scegliere e inserire i colori.</span><span class="sxs-lookup"><span data-stu-id="8d401-235">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="8d401-236">Dispone di una semplice barra dei colori e un selettore "pop-down" che offre un controllo più preciso.</span><span class="sxs-lookup"><span data-stu-id="8d401-236">It has a simple color bar and a "pop-down" picker that offers finer control.</span></span>

<span data-ttu-id="8d401-237">La selezione colori include una tavolozza di colori standard supporta nomi di colori standard, codici hash, i colori RGB, RGBA, HSL e HSLA e gestisce un elenco dei colori utilizzati più di recente nel documento.</span><span class="sxs-lookup"><span data-stu-id="8d401-237">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="8d401-238">Nella riga in cui è stata la proprietà del colore di sfondo, digitare "bc" e premere la freccia rivolta verso il basso di una volta.</span><span class="sxs-lookup"><span data-stu-id="8d401-238">On the line where the background-color property was, type "bc" and press the down arrow once.</span></span>

<span data-ttu-id="8d401-239">Quando si digita il primo carattere di ogni parola in una proprietà separati da trattini, ad esempio "background-color", IntelliSense consente di filtrare l'elenco per visualizzare solo le proprietà che corrispondono a:</span><span class="sxs-lookup"><span data-stu-id="8d401-239">When you type the first character of each word in a hyphen-separated property like "background-color", IntelliSense filters the list for you to show only the properties that match:</span></span>

![IntelliSense filtrato i valori](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

<span data-ttu-id="8d401-241">Digitare i due punti.</span><span class="sxs-lookup"><span data-stu-id="8d401-241">Now type a colon.</span></span> <span data-ttu-id="8d401-242">Quando si esegue l'operazione, viene inserito il nome completo del colore di sfondo della proprietà.</span><span class="sxs-lookup"><span data-stu-id="8d401-242">When you do, the full background-color property name is inserted.</span></span> <span data-ttu-id="8d401-243">Tipo  **#**  o **rgb (**, e viene visualizzata la barra di selezione colore:</span><span class="sxs-lookup"><span data-stu-id="8d401-243">Type **#** or **rgb(**, and the color picker bar appears:</span></span>

![Barra di selezione dei colori CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

<span data-ttu-id="8d401-245">Per verificare il funzionamento di barra di selezione dei colori, fare clic sui colori con il puntatore del mouse o premere il tasto freccia giù e quindi utilizzare i tasti freccia sinistra e destra per attraversare i colori.</span><span class="sxs-lookup"><span data-stu-id="8d401-245">To see how the color picker bar works, click its colors with the mouse pointer, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="8d401-246">Quando si visita un colore, il valore corrispondente per la proprietà del colore di sfondo viene visualizzato in anteprima:</span><span class="sxs-lookup"><span data-stu-id="8d401-246">When you visit a color, the corresponding value for the background-color property is previewed:</span></span>

![valore della proprietà colore di sfondo visualizzato in anteprima](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

<span data-ttu-id="8d401-248">A questo punto, è possibile premere INVIO per selezionare il valore e quindi un punto e virgola (;) per completare la voce CSS.</span><span class="sxs-lookup"><span data-stu-id="8d401-248">At this point, you could press Enter to select the value and then a semicolon (;) to complete the CSS entry.</span></span> <span data-ttu-id="8d401-249">Per il momento, andare alla sezione successiva, in modo da poter visualizzare il funzionamento di selezione colore pop-down.</span><span class="sxs-lookup"><span data-stu-id="8d401-249">For now, go on to the next section so that you can see how the color picker pop-down works.</span></span>

#### <a name="using-the-color-picker-pop-down"></a><span data-ttu-id="8d401-250">Tramite la selezione colore Pop-Down</span><span class="sxs-lookup"><span data-stu-id="8d401-250">Using the Color Picker Pop-Down</span></span>

<span data-ttu-id="8d401-251">Quando la barra dei colori non è il colore esatto che si sta cercando, è possibile utilizzare il selettore di colore pop-down.</span><span class="sxs-lookup"><span data-stu-id="8d401-251">When the color bar doesn't have the exact color that you're looking for, you can use the color picker pop-down.</span></span>

<span data-ttu-id="8d401-252">Per aprirlo, fare clic sulla doppia freccia di espansione all'estremità destra della barra dei colori o premere una o due volte il tasto freccia giù sulla tastiera.</span><span class="sxs-lookup"><span data-stu-id="8d401-252">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Selezione colori CSS Pop-Down](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

<span data-ttu-id="8d401-254">Fare clic su un colore dalla barra verticale a destra.</span><span class="sxs-lookup"><span data-stu-id="8d401-254">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="8d401-255">Nella finestra principale indica una sfumatura per il colore specifico.</span><span class="sxs-lookup"><span data-stu-id="8d401-255">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="8d401-256">Scegliere un colore direttamente dalla barra verticale premendo INVIO oppure fare clic su qualsiasi punto nella finestra principale di scegliere con maggiore precisione.</span><span class="sxs-lookup"><span data-stu-id="8d401-256">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="8d401-257">Se è presente un colore sullo schermo del computer che si desidera utilizzare (non è necessario all'interno dell'interfaccia utente di Visual Studio), è possibile acquisire il relativo valore utilizzando il contagocce in basso a destra.</span><span class="sxs-lookup"><span data-stu-id="8d401-257">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="8d401-258">È inoltre possibile modificare l'opacità di un colore spostando il dispositivo di scorrimento nella parte inferiore della selezione colori.</span><span class="sxs-lookup"><span data-stu-id="8d401-258">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="8d401-259">In tal modo le modifiche dei colori valori RGBA perché il formato RGBA può rappresentare l'opacità.</span><span class="sxs-lookup"><span data-stu-id="8d401-259">Doing so changes color values to RGBA values because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="8d401-260">Dopo aver scelto un colore, premere INVIO e quindi digitare un punto e virgola per completare l'immissione di colore di sfondo nel *Site* file.</span><span class="sxs-lookup"><span data-stu-id="8d401-260">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="8d401-261">La barra di aggiornamento del controllo pagina</span><span class="sxs-lookup"><span data-stu-id="8d401-261">The Page Inspector Update Bar</span></span>

<span data-ttu-id="8d401-262">Controllo pagina rileva immediatamente la modifica di *Site* file (o a qualsiasi file nell'applicazione) e viene visualizzato un avviso in una barra di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="8d401-262">Page Inspector immediately detects the change to the *Site.css* file (or to any file in the application) and displays an alert in an update bar.</span></span>

![Barra di aggiornamento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

<span data-ttu-id="8d401-264">Per salvare tutti i file e aggiornare il browser di controllo pagina, premere Ctrl + Alt + Invio oppure fare clic sulla barra di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="8d401-264">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="8d401-265">La modifica il colore di evidenziazione viene visualizzato nel browser:</span><span class="sxs-lookup"><span data-stu-id="8d401-265">The change in the highlight color appears in the browser:</span></span>

![Colore di evidenziazione modificato](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a><span data-ttu-id="8d401-267">Si noti che è facilmente aggiornato il browser di controllo pagina direttamente da all'interno dell'ambiente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8d401-267">Notice that you conveniently refreshed the Page Inspector browser right from within the Visual Studio environment.</span></span> <span data-ttu-id="8d401-268">Con Page Inspector anziché un browser esterno consente di rimanere nell'editor, quando si sviluppano le applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="8d401-268">Using Page Inspector instead of an external browser lets you stay in the editor when you develop your web applications.</span></span>

## <a name="see-also"></a><span data-ttu-id="8d401-269">Vedere anche</span><span class="sxs-lookup"><span data-stu-id="8d401-269">See Also</span></span>

<span data-ttu-id="8d401-270">[Introduzione a controllo pagina](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (video di Channel 9)</span><span class="sxs-lookup"><span data-stu-id="8d401-270">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)</span></span>

<span data-ttu-id="8d401-271">[Messaggi di errore di Page Inspector](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="8d401-271">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
