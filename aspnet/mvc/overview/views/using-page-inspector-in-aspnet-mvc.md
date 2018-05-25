---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: Utilizzo di controllo pagina in ASP.NET MVC | Documenti Microsoft
author: rick-anderson
description: Controllo pagina in Visual Studio 2012 è uno strumento di sviluppo web con un browser integrato. Selezionare un elemento nel browser integrato e controllo pagina i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5b443963a089f96a9dab11b7db4a25451075d6be
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="using-page-inspector-in-aspnet-mvc"></a><span data-ttu-id="f55aa-104">Utilizzo di Controllo pagina in ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="f55aa-104">Using Page Inspector in ASP.NET MVC</span></span>
====================
<span data-ttu-id="f55aa-105">da Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="f55aa-105">by Tim Ammann</span></span>

> <span data-ttu-id="f55aa-106">Controllo pagina in Visual Studio 2012 è uno strumento di sviluppo web con un browser integrato.</span><span class="sxs-lookup"><span data-stu-id="f55aa-106">Page Inspector in Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="f55aa-107">Selezionare un elemento nel browser integrato e controllo pagina evidenzia immediatamente l'origine dell'elemento e CSS.</span><span class="sxs-lookup"><span data-stu-id="f55aa-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="f55aa-108">È possibile passare qualsiasi visualizzazione MVC, rapidamente trovare le origini di markup sottoposto a rendering e gli strumenti del visualizzatore a destra all'interno dell'ambiente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f55aa-108">You can browse any MVC view, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> [<span data-ttu-id="f55aa-109">Guardare il Video</span><span class="sxs-lookup"><span data-stu-id="f55aa-109">Watch the Video</span></span>](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> <span data-ttu-id="f55aa-110">In questa esercitazione viene illustrato come abilitare la modalità di controllo e quindi individuare e modificare rapidamente markup e CSS all'interno del progetto web.</span><span class="sxs-lookup"><span data-stu-id="f55aa-110">This tutorial shows how to enable Inspection Mode, and then quickly locate and edit markup and CSS within your web project.</span></span> <span data-ttu-id="f55aa-111">L'esercitazione Usa un progetto MVC, ma è anche possibile usare il controllo pagina per [Web Form](https://go.microsoft.com/?linkid=9802001) e altre applicazioni ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f55aa-111">The tutorial uses an MVC Project, but you can also use Page Inspector for [Web Forms](https://go.microsoft.com/?linkid=9802001) and other ASP.NET applications.</span></span>
> 
> <span data-ttu-id="f55aa-112">L'esercitazione include le sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="f55aa-112">The tutorial has the following sections:</span></span>
> 
> - [<span data-ttu-id="f55aa-113">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="f55aa-113">Prerequisites</span></span>](#_1_prerequisites)
> - [<span data-ttu-id="f55aa-114">Creare un'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="f55aa-114">Create a Web Application</span></span>](#_2_creating_a)
> - [<span data-ttu-id="f55aa-115">Utilizza controllo pagina per selezionare una vista</span><span class="sxs-lookup"><span data-stu-id="f55aa-115">Use Page Inspector to Browse to a View</span></span>](#_3_using_page)
> - [<span data-ttu-id="f55aa-116">Abilitare la modalità di controllo</span><span class="sxs-lookup"><span data-stu-id="f55aa-116">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> - [<span data-ttu-id="f55aa-117">Utilizza controllo pagina per apportare modifiche al Markup</span><span class="sxs-lookup"><span data-stu-id="f55aa-117">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> - [<span data-ttu-id="f55aa-118">Modalità di ispezione e la finestra di HTML</span><span class="sxs-lookup"><span data-stu-id="f55aa-118">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> - [<span data-ttu-id="f55aa-119">Anteprima modifiche CSS nella finestra stili</span><span class="sxs-lookup"><span data-stu-id="f55aa-119">Preview CSS Changes in the Styles window</span></span>](#_7_previewing_css)
> - [<span data-ttu-id="f55aa-120">Sincronizzazione automatica CSS</span><span class="sxs-lookup"><span data-stu-id="f55aa-120">CSS Auto Sync</span></span>](#css_auto_sync)
> - [<span data-ttu-id="f55aa-121">Mediante il selettore del colore CSS</span><span class="sxs-lookup"><span data-stu-id="f55aa-121">Using the CSS Color Picker</span></span>](#css_color_picker)
> - [<span data-ttu-id="f55aa-122">Mapping di elementi della pagina dinamica per JavaScript</span><span class="sxs-lookup"><span data-stu-id="f55aa-122">Mapping Dynamic Page Elements to JavaScript</span></span>](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="f55aa-123">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="f55aa-123">Prerequisites</span></span>

- <span data-ttu-id="f55aa-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) o [Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="f55aa-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="f55aa-125">Per ottenere la versione più recente di controllo pagina, utilizzare [installazione guidata piattaforma Web](https://go.microsoft.com/fwlink/?LinkId=255386) per installare Windows Azure SDK per .NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="f55aa-125">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Windows Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="f55aa-126">Controllo pagina viene fornito con Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="f55aa-126">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="f55aa-127">La versione più recente è 1.3.</span><span class="sxs-lookup"><span data-stu-id="f55aa-127">The latest version is 1.3.</span></span> <span data-ttu-id="f55aa-128">Per verificare la versione hanno, eseguire Visual Studio e selezionare **su Microsoft Visual Studio** dal **Guida** menu.</span><span class="sxs-lookup"><span data-stu-id="f55aa-128">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="f55aa-129">Creare un'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="f55aa-129">Create a Web Application</span></span>

<span data-ttu-id="f55aa-130">Innanzitutto, creare un'applicazione web che verrà utilizzato il controllo pagina con.</span><span class="sxs-lookup"><span data-stu-id="f55aa-130">First, create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="f55aa-131">In Visual Studio, scegliere **File** &gt; **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="f55aa-131">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="f55aa-132">A sinistra, espandere **Visual c#** selezionare **Web**, quindi selezionare **applicazione Web di ASP.NET MVC4**.</span><span class="sxs-lookup"><span data-stu-id="f55aa-132">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span>

![Nuova applicazione MVC ASP.NET](using-page-inspector-in-aspnet-mvc/_static/image2.png)

<span data-ttu-id="f55aa-134">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="f55aa-134">Click **OK**.</span></span>

<span data-ttu-id="f55aa-135">Nel **nuovo progetto ASP.NET MVC 4** nella finestra di dialogo **applicazione Internet**.</span><span class="sxs-lookup"><span data-stu-id="f55aa-135">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="f55aa-136">Lasciare **Razor** come motore di visualizzazione predefinito.</span><span class="sxs-lookup"><span data-stu-id="f55aa-136">Leave **Razor** as the default view engine.</span></span>

![Nuovo progetto ASP.NET MVC - applicazione Internet](using-page-inspector-in-aspnet-mvc/_static/image4.png)

<span data-ttu-id="f55aa-138">L'applicazione verrà visualizzata in **origine** visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f55aa-138">The application opens in **Source** view.</span></span>

![Nuova applicazione MVC ASP.NET nella visualizzazione origine](using-page-inspector-in-aspnet-mvc/_static/image6.png)

<span data-ttu-id="f55aa-140">Dopo aver creato un'applicazione da utilizzare, è possibile utilizzare controllo pagina per esaminare e modificare.</span><span class="sxs-lookup"><span data-stu-id="f55aa-140">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a><span data-ttu-id="f55aa-141">Utilizza controllo pagina per selezionare una vista</span><span class="sxs-lookup"><span data-stu-id="f55aa-141">Use Page Inspector to Browse to a View</span></span>

<span data-ttu-id="f55aa-142">In Visual Studio 2012, è possibile fare doppio clic qualsiasi visualizzazione nel progetto, seleziona **Visualizza in controllo pagina**, controllo pagina verrà scoprire la route e visualizzare la pagina.</span><span class="sxs-lookup"><span data-stu-id="f55aa-142">In Visual Studio 2012, you can right-click any view in your project, select **View in Page Inspector**, and Page Inspector will figure out the route and display the page.</span></span>

<span data-ttu-id="f55aa-143">In **Esplora**, espandere il **viste** cartella e quindi la **Home** cartella.</span><span class="sxs-lookup"><span data-stu-id="f55aa-143">In **Solution Explorer**, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="f55aa-144">Fare clic con il pulsante destro del file cshtml e scegliere **Visualizza in controllo pagina**.</span><span class="sxs-lookup"><span data-stu-id="f55aa-144">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

![Visualizzare cshtml in controllo pagina](using-page-inspector-in-aspnet-mvc/_static/image8.png)

<span data-ttu-id="f55aa-146">Per impostazione predefinita, controllo pagina è ancorato come finestra sul lato sinistro dell'ambiente Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f55aa-146">By default, Page Inspector is docked as a window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="f55aa-147">Se si preferisce, è possibile ancorare la finestra in un' posizione o disancorare la finestra.</span><span class="sxs-lookup"><span data-stu-id="f55aa-147">If you prefer, you can dock it elsewhere, or undock the window.</span></span> <span data-ttu-id="f55aa-148">Vedere [procedura: disporre e ancorare le finestre](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span><span class="sxs-lookup"><span data-stu-id="f55aa-148">See [How to: Arrange and Dock Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span></span>

<span data-ttu-id="f55aa-149">Nel riquadro superiore della finestra di controllo pagina Mostra la pagina corrente in una finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="f55aa-149">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="f55aa-150">Nel riquadro inferiore mostra la pagina nel markup HTML, con alcune schede che consentono di controllare aspetti diversi della pagina.</span><span class="sxs-lookup"><span data-stu-id="f55aa-150">The bottom pane shows the page in HTML markup, along with some tabs that let you inspect different aspects of the page.</span></span> <span data-ttu-id="f55aa-151">Il riquadro inferiore è simile al [strumenti di sviluppo F12](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="f55aa-151">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span>

![Applicazione ASP.NET MVC in controllo pagina](using-page-inspector-in-aspnet-mvc/_static/image10.png)

<span data-ttu-id="f55aa-153">In questa esercitazione si utilizzerà il **HTML** e **stili** schede per passare rapidamente e apportare modifiche all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f55aa-153">In this tutorial, you will use the **HTML** and **Styles** tabs to navigate quickly and make changes to the application.</span></span>

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a><span data-ttu-id="f55aa-154">Modalità EnableInspection</span><span class="sxs-lookup"><span data-stu-id="f55aa-154">EnableInspection Mode</span></span>

<span data-ttu-id="f55aa-155">Per impostare controllo pagina in modalità di controllo, fare clic su di **controlla** pulsante.</span><span class="sxs-lookup"><span data-stu-id="f55aa-155">To put Page Inspector into Inspection Mode, click the **Inspect** button.</span></span> <span data-ttu-id="f55aa-156">In modalità di controllo, quando si posiziona il puntatore del mouse su qualsiasi parte della pagina di cui è stato eseguito rendering, il markup di origine corrispondente o il codice viene evidenziata.</span><span class="sxs-lookup"><span data-stu-id="f55aa-156">In Inspection Mode, when you hold the mouse pointer over any part of the rendered page, the corresponding source markup or code is highlighted.</span></span>

![Attiva/Disattiva modalità di controllo](using-page-inspector-in-aspnet-mvc/_static/image12.png)

<span data-ttu-id="f55aa-158">Spostare il puntatore del mouse su parti diverse della pagina in controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="f55aa-158">Now move your mouse over different parts of the page within Page Inspector.</span></span> <span data-ttu-id="f55aa-159">Quando si esegue, il puntatore del mouse diventa un segno più grande e viene evidenziato l'elemento sotto:</span><span class="sxs-lookup"><span data-stu-id="f55aa-159">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Mouse div.content wrapper](using-page-inspector-in-aspnet-mvc/_static/image14.png)

<span data-ttu-id="f55aa-161">Quando si sposta il puntatore del mouse, Visual Studio consente di evidenziare la sintassi Razor corrispondente nel file di origine.</span><span class="sxs-lookup"><span data-stu-id="f55aa-161">As you move the mouse pointer, Visual Studio highlights the corresponding Razor syntax in the source file.</span></span> <span data-ttu-id="f55aa-162">Se l'elemento HTML proviene da un altro file di origine, Visual Studio apre automaticamente il file.</span><span class="sxs-lookup"><span data-stu-id="f55aa-162">If the HTML element comes from another source file, Visual Studio automatically opens the file.</span></span>

<span data-ttu-id="f55aa-163">In controllo pagina, il **HTML** scheda Mostra il codice HTML che è stato generato dalla sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="f55aa-163">In Page Inspector, the **HTML** tab shows the HTML that was generated from the Razor syntax.</span></span> <span data-ttu-id="f55aa-164">Quando si sposta il puntatore del mouse, vengono evidenziati gli elementi HTML.</span><span class="sxs-lookup"><span data-stu-id="f55aa-164">As you move the mouse pointer, the HTML elements are highlighted.</span></span> <span data-ttu-id="f55aa-165">Il **stili** scheda Visualizza le regole CSS per l'elemento.</span><span class="sxs-lookup"><span data-stu-id="f55aa-165">The **Styles** tab shows the CSS rules for the element.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="f55aa-166">Utilizza controllo pagina per apportare modifiche al Markup</span><span class="sxs-lookup"><span data-stu-id="f55aa-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="f55aa-167">Controllo pagina consente di trovare markup il cui percorso potrebbe non essere evidente.</span><span class="sxs-lookup"><span data-stu-id="f55aa-167">Page Inspector lets you find markup whose location might not be obvious.</span></span> <span data-ttu-id="f55aa-168">È quindi possibile modificare il markup e visualizzare le modifiche risultante.</span><span class="sxs-lookup"><span data-stu-id="f55aa-168">Then you can modify the markup and see the resulting changes.</span></span>

<span data-ttu-id="f55aa-169">Per verificarlo, fare clic su **controlla** e quindi scorrere fino alla fine della pagina nella finestra di controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="f55aa-169">To see this, click **Inspect** and then scroll to the bottom of the page in the Page Inspector window.</span></span>

<span data-ttu-id="f55aa-170">Quando si sposta il puntatore del mouse nell'area di piè di pagina, vengono aperti il \_file cshtml ed evidenzia la sezione della pagina di layout che è stato selezionato.</span><span class="sxs-lookup"><span data-stu-id="f55aa-170">When you move the mouse pointer into the footer area, Page Inspector opens the \_Layout.cshtml file and highlights the section of the layout page that you have selected.</span></span> <span data-ttu-id="f55aa-171">Come si può notare, il piè di pagina sono è definito nel file di layout e non la vista stessa.</span><span class="sxs-lookup"><span data-stu-id="f55aa-171">As you can see, the footer are is defined in the layout file, and not the view itself.</span></span>

![Piè di pagina](using-page-inspector-in-aspnet-mvc/_static/image16.png)

<span data-ttu-id="f55aa-173">Spostare il puntatore del mouse sulla linea con il copyright <a id="a"> </a>nota.</span><span class="sxs-lookup"><span data-stu-id="f55aa-173">Now move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span> <span data-ttu-id="f55aa-174">Nel \_cshtml pagina, la riga corrispondente viene evidenziato.</span><span class="sxs-lookup"><span data-stu-id="f55aa-174">In the \_Layout.cshtml page, the corresponding line is highlighted.</span></span>

![Riga di piè di pagina copyright evidenziata](using-page-inspector-in-aspnet-mvc/_static/image18.png)

<span data-ttu-id="f55aa-176">Aggiungere testo alla fine della riga di \_file cshtml.</span><span class="sxs-lookup"><span data-stu-id="f55aa-176">Add some text to the end of the line in the \_Layout.cshtml file.</span></span>

<span data-ttu-id="f55aa-177">&lt;p&gt;&amp;copiare; @DateTime.Now.Year -Applicazione ASP.NET MVC Massi!  &lt; /p&gt;</span><span class="sxs-lookup"><span data-stu-id="f55aa-177">&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET MVC Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="f55aa-178">A questo punto, premere Ctrl + Alt + Invio oppure fare clic sulla barra di aggiornamento per visualizzare i risultati nella finestra del browser di controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="f55aa-178">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![L'applicazione ASP.NET funziona](using-page-inspector-in-aspnet-mvc/_static/image20.png)

<span data-ttu-id="f55aa-180">Si potrebbe pensare che il piè di pagina definite nelle cshtml, ma è stato eseguito nel \_cshtml e controllo pagina ha rilevato che per l'utente.</span><span class="sxs-lookup"><span data-stu-id="f55aa-180">You might have thought that the footer defined in Index.cshtml, but it turned out to be in the \_Layout.cshtml, and Page Inspector found it for you.</span></span>

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="f55aa-181">Modalità di ispezione e la finestra di HTML</span><span class="sxs-lookup"><span data-stu-id="f55aa-181">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="f55aa-182">Successivamente, si avrà un rapido controllo di finestra di HTML e come viene eseguito il mapping elementi per l'utente.</span><span class="sxs-lookup"><span data-stu-id="f55aa-182">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="f55aa-183">Fare clic su **controlla** inserire controllo pagina in modalità controllo.</span><span class="sxs-lookup"><span data-stu-id="f55aa-183">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="f55aa-184">Fare clic sulla parte superiore della pagina su cui è indicato "Il logohere".</span><span class="sxs-lookup"><span data-stu-id="f55aa-184">Click the top part of the page that says "Your logohere".</span></span> <span data-ttu-id="f55aa-185">Si sta esaminando un particolare elemento in modo più dettagliato, pertanto la visualizzazione nella finestra del browser non cambierà quando si sposta il puntatore del mouse.</span><span class="sxs-lookup"><span data-stu-id="f55aa-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="f55aa-186">Spostare il puntatore del mouse per il **HTML** finestra.</span><span class="sxs-lookup"><span data-stu-id="f55aa-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="f55aa-187">Quando si sposta il puntatore del mouse, controllo pagina vengono descritti l'elemento all'interno di **HTML** finestra ed evidenziare l'elemento corrispondente nella finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="f55aa-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Finestra HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

<span data-ttu-id="f55aa-189">Come in precedenza, vengono aperti il \_file cshtml automaticamente in una scheda temporanea. Fare clic su di \_scheda temporanea cshtml e il markup corrispondente verrà evidenziato nel &lt;intestazione&gt; sezione automaticamente:</span><span class="sxs-lookup"><span data-stu-id="f55aa-189">As before, Page Inspector opens the \_Layout.cshtml file for you in a temporary tab. Click the \_Layout.cshtml temporary tab, and the corresponding markup will be highlighted in the &lt;header&gt; section for you:</span></span>

![Markup evidenziato](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="f55aa-191">Anteprima modifiche CSS nella finestra stili</span><span class="sxs-lookup"><span data-stu-id="f55aa-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="f55aa-192">Successivamente, si utilizzerà il controllo pagina **stili** finestra per visualizzare in anteprima le modifiche a CSS.</span><span class="sxs-lookup"><span data-stu-id="f55aa-192">Next, you will use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="f55aa-193">Fare clic su **controlla** inserire controllo pagina in modalità controllo.</span><span class="sxs-lookup"><span data-stu-id="f55aa-193">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="f55aa-194">Nella finestra del browser di controllo pagina, spostare il puntatore del mouse sopra la sezione "Home Page" finché il **div.content wrapper** etichetta viene visualizzata.</span><span class="sxs-lookup"><span data-stu-id="f55aa-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Mouse div.content wrapper](using-page-inspector-in-aspnet-mvc/_static/image26.png)

<span data-ttu-id="f55aa-196">Fare clic una volta all'interno della sezione div.content wrapper, e quindi spostare il puntatore del mouse per il **stili** finestra.</span><span class="sxs-lookup"><span data-stu-id="f55aa-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="f55aa-197">Il **stili** finestra Visualizza tutte le regole CSS per questo elemento.</span><span class="sxs-lookup"><span data-stu-id="f55aa-197">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="f55aa-198">Scorrere fino a individuare il wrapper. Content .featured selettore di classe.</span><span class="sxs-lookup"><span data-stu-id="f55aa-198">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="f55aa-199">A questo punto deselezionare la casella di controllo per la proprietà del colore di sfondo.</span><span class="sxs-lookup"><span data-stu-id="f55aa-199">Now clear the checkbox for the background-color property.</span></span>

![Colore di sfondo cancella](using-page-inspector-in-aspnet-mvc/_static/image28.png)

<span data-ttu-id="f55aa-201">Si noti come la modifica immediata anteprime nella finestra del browser di controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="f55aa-201">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="f55aa-202">Selezionare di nuovo la casella di controllo, quindi fare doppio clic sul valore della proprietà e modificarla in rosso.</span><span class="sxs-lookup"><span data-stu-id="f55aa-202">Select the checkbox again, then double-click the property value and change it to red.</span></span> <span data-ttu-id="f55aa-203">La modifica viene immediatamente:</span><span class="sxs-lookup"><span data-stu-id="f55aa-203">The change shows immediately:</span></span>

![Colore di sfondo rosso](using-page-inspector-in-aspnet-mvc/_static/image30.png)

<span data-ttu-id="f55aa-205">Il **stili** rende finestra è più facile di test e visualizzare in anteprima CSS modifiche prima di applicare le modifiche allo stile di finestra stessa.</span><span class="sxs-lookup"><span data-stu-id="f55aa-205">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="f55aa-206">Sincronizzazione automatica CSS</span><span class="sxs-lookup"><span data-stu-id="f55aa-206">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="f55aa-207">Questa funzionalità richiede la versione 1.3 del controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="f55aa-207">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="f55aa-208">La funzionalità di sincronizzazione automatica CSS consente di modificare direttamente un file CSS e visualizzare le modifiche immediatamente nel browser di controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="f55aa-208">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="f55aa-209">Fare clic su **controlla** inserire controllo pagina in modalità controllo.</span><span class="sxs-lookup"><span data-stu-id="f55aa-209">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="f55aa-210">Nel browser di controllo pagina, spostare il puntatore del mouse sopra la sezione "Home Page" finché il **div.content wrapper** etichetta viene visualizzata.</span><span class="sxs-lookup"><span data-stu-id="f55aa-210">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="f55aa-211">Fare clic per selezionare questo elemento.</span><span class="sxs-lookup"><span data-stu-id="f55aa-211">Click once to select this element.</span></span>

<span data-ttu-id="f55aa-212">Il **stili** finestra Visualizza tutte le regole CSS per questo elemento.</span><span class="sxs-lookup"><span data-stu-id="f55aa-212">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="f55aa-213">Scorrere fino a individuare il wrapper. Content .featured selettore di classe.</span><span class="sxs-lookup"><span data-stu-id="f55aa-213">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="f55aa-214">Fare clic su ".featured. Content-wrapper".</span><span class="sxs-lookup"><span data-stu-id="f55aa-214">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="f55aa-215">Controllo pagina consente di aprire il file CSS che definisce questo stile (Site.css) ed evidenzia il CSS corrispondente.</span><span class="sxs-lookup"><span data-stu-id="f55aa-215">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

<span data-ttu-id="f55aa-216">Modificare il valore per `background-color` per "red".</span><span class="sxs-lookup"><span data-stu-id="f55aa-216">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="f55aa-217">La modifica viene visualizzata immediatamente nel browser di controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="f55aa-217">The change appears immediately in the Page Inspector browser.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a><span data-ttu-id="f55aa-218">Mediante il selettore del colore CSS</span><span class="sxs-lookup"><span data-stu-id="f55aa-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="f55aa-219">L'editor CSS in Visual Studio 2012 è una selezione di colori che rende più semplice scegliere e inserire i colori.</span><span class="sxs-lookup"><span data-stu-id="f55aa-219">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="f55aa-220">La selezione colori include una tavolozza di colori standard supporta nomi di colori standard, codici hash, i colori RGB, RGBA, HSL e HSLA e gestisce un elenco dei colori utilizzati più di recente nel documento.</span><span class="sxs-lookup"><span data-stu-id="f55aa-220">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="f55aa-221">Nella sezione precedente, è stato modificato il valore di `background-color` proprietà.</span><span class="sxs-lookup"><span data-stu-id="f55aa-221">In the previous section, you changed the value of the `background-color` property.</span></span> <span data-ttu-id="f55aa-222">Per richiamare il selettore di colore, posizionare il cursore dopo il nome della proprietà e il tipo **#** o **rgb (**.</span><span class="sxs-lookup"><span data-stu-id="f55aa-222">To invoke the color picker, place the insertion point after the property name and type **#** or **rgb(**.</span></span>

![Barra di selezione dei colori CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

<span data-ttu-id="f55aa-224">Fare clic su un colore per selezionarlo oppure premere il tasto freccia giù e quindi utilizzare i tasti freccia sinistra e destra per attraversare i colori.</span><span class="sxs-lookup"><span data-stu-id="f55aa-224">Click on a color to select it, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="f55aa-225">Quando si visita un colore, il valore esadecimale corrispondente viene visualizzato in anteprima:</span><span class="sxs-lookup"><span data-stu-id="f55aa-225">When you visit a color, the corresponding hex value is previewed:</span></span>

![valore della proprietà colore di sfondo visualizzato in anteprima](using-page-inspector-in-aspnet-mvc/_static/image38.png)

<span data-ttu-id="f55aa-227">Se la barra dei colori non è il colore esatto desiderato, è possibile utilizzare il colore selezione pop verso il basso.</span><span class="sxs-lookup"><span data-stu-id="f55aa-227">If the color bar doesn't have the exact color you want, you can use the color picker pop-down.</span></span> <span data-ttu-id="f55aa-228">Per aprirlo, fare clic sulla doppia freccia di espansione all'estremità destra della barra dei colori o premere una o due volte il tasto freccia giù sulla tastiera.</span><span class="sxs-lookup"><span data-stu-id="f55aa-228">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Selezione colori CSS Pop-Down](using-page-inspector-in-aspnet-mvc/_static/image40.png)

<span data-ttu-id="f55aa-230">Fare clic su un colore dalla barra verticale a destra.</span><span class="sxs-lookup"><span data-stu-id="f55aa-230">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="f55aa-231">Nella finestra principale indica una sfumatura per il colore specifico.</span><span class="sxs-lookup"><span data-stu-id="f55aa-231">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="f55aa-232">Scegliere un colore direttamente dalla barra verticale premendo INVIO oppure fare clic su qualsiasi punto nella finestra principale di scegliere con maggiore precisione.</span><span class="sxs-lookup"><span data-stu-id="f55aa-232">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="f55aa-233">Se è presente un colore sullo schermo del computer che si desidera utilizzare (non è necessario all'interno dell'interfaccia utente di Visual Studio), è possibile acquisire il relativo valore utilizzando il contagocce in basso a destra.</span><span class="sxs-lookup"><span data-stu-id="f55aa-233">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="f55aa-234">È inoltre possibile modificare l'opacità di un colore spostando il dispositivo di scorrimento nella parte inferiore della selezione colori.</span><span class="sxs-lookup"><span data-stu-id="f55aa-234">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="f55aa-235">In tal modo le modifiche dei colori valori RGBA, perché il formato RGBA può rappresentare l'opacità.</span><span class="sxs-lookup"><span data-stu-id="f55aa-235">Doing so changes color values to RGBA values, because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="f55aa-236">Dopo aver scelto un colore, premere INVIO e quindi digitare un punto e virgola per completare l'immissione di colore di sfondo nel *Site* file.</span><span class="sxs-lookup"><span data-stu-id="f55aa-236">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="f55aa-237">La barra di aggiornamento del controllo pagina</span><span class="sxs-lookup"><span data-stu-id="f55aa-237">The Page Inspector Update Bar</span></span>

<span data-ttu-id="f55aa-238">Controllo pagina rileva immediatamente la modifica di *Site* file e visualizza un avviso in una barra di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="f55aa-238">Page Inspector immediately detects the change to the *Site.css* file and displays an alert in an update bar.</span></span>

![Barra di aggiornamento](using-page-inspector-in-aspnet-mvc/_static/image42.png)

<span data-ttu-id="f55aa-240">Per salvare tutti i file e aggiornare il browser di controllo pagina, premere Ctrl + Alt + Invio oppure fare clic sulla barra di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="f55aa-240">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="f55aa-241">La modifica il colore di evidenziazione viene visualizzato nel browser.</span><span class="sxs-lookup"><span data-stu-id="f55aa-241">The change in the highlight color appears in the browser.</span></span>

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a><span data-ttu-id="f55aa-242">Mapping di elementi della pagina dinamica per JavaScript</span><span class="sxs-lookup"><span data-stu-id="f55aa-242">Mapping Dynamic Page Elements to JavaScript</span></span>

<span data-ttu-id="f55aa-243">Nelle applicazioni web moderne, gli elementi della pagina sono spesso generati dinamicamente con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f55aa-243">In modern web applications, elements in the page are often generated dynamically with JavaScript.</span></span> <span data-ttu-id="f55aa-244">Ciò significa che non sono presenti markup statico (HTML o Razor) che corrisponde a questi elementi di pagina.</span><span class="sxs-lookup"><span data-stu-id="f55aa-244">That means there is no static markup (either HTML or Razor) that corresponds to these page elements.</span></span>

<span data-ttu-id="f55aa-245">Con la versione 1.3, controllo pagina può ora mappare gli elementi che sono stati aggiunti dinamicamente alla pagina al codice JavaScript corrispondente.</span><span class="sxs-lookup"><span data-stu-id="f55aa-245">With version 1.3, Page Inspector can now map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span> <span data-ttu-id="f55aa-246">Per illustrare questa funzionalità, si userà il [modello di applicazione a pagina singola (SPA)](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="f55aa-246">To demonstrate this feature, we'll use the [Single Page Application (SPA) template](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f55aa-247">Il modello di SPA richiede il [ASP.NET e Web strumenti 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) aggiornare.</span><span class="sxs-lookup"><span data-stu-id="f55aa-247">The SPA template requires the [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) update.</span></span>


<span data-ttu-id="f55aa-248">In Visual Studio, scegliere **File** &gt; **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="f55aa-248">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="f55aa-249">A sinistra, espandere **Visual c#** selezionare **Web**, quindi selezionare **applicazione Web di ASP.NET MVC4**.</span><span class="sxs-lookup"><span data-stu-id="f55aa-249">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span> <span data-ttu-id="f55aa-250">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="f55aa-250">Click **OK**.</span></span>

<span data-ttu-id="f55aa-251">Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo Seleziona **applicazione a pagina singola**.</span><span class="sxs-lookup"><span data-stu-id="f55aa-251">In the **New ASP.NET MVC 4 Project** dialog, select **Single Page Application**.</span></span>

<span data-ttu-id="f55aa-252">In Esplora soluzioni, espandere il **viste** cartella e quindi la **Home** cartella.</span><span class="sxs-lookup"><span data-stu-id="f55aa-252">In Solution Explorer, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="f55aa-253">Fare clic con il pulsante destro del file cshtml e scegliere **Visualizza in controllo pagina**.</span><span class="sxs-lookup"><span data-stu-id="f55aa-253">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

<span data-ttu-id="f55aa-254">In primo luogo che è visualizzato nel browser di controllo pagina è una pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="f55aa-254">The first thing that is displayed in the Page Inspector browser is a login page.</span></span> <span data-ttu-id="f55aa-255">Fare clic su "Sign Up" e creare un nome utente e una password.</span><span class="sxs-lookup"><span data-stu-id="f55aa-255">Click "Sign Up" and create a user name and password.</span></span> <span data-ttu-id="f55aa-256">Quando si esegue l'iscrizione, l'applicazione esegue l'accesso e crea un elenco di attività con alcuni elementi di esempio.</span><span class="sxs-lookup"><span data-stu-id="f55aa-256">Once you sign up, the application logs you in and creates a to-do list with some sample items.</span></span>

<span data-ttu-id="f55aa-257">Fare clic su **controlla** inserire controllo pagina in modalità controllo.</span><span class="sxs-lookup"><span data-stu-id="f55aa-257">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span> <span data-ttu-id="f55aa-258">Nel browser di controllo pagina, fare clic su uno degli elementi di attività da eseguire.</span><span class="sxs-lookup"><span data-stu-id="f55aa-258">In the Page Inspector browser, click on one of the to-do items.</span></span> <span data-ttu-id="f55aa-259">Si noti che invece viene evidenziato in blu, l'elemento viene evidenziato in arancione, con "JS" accanto al nome dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="f55aa-259">Notice that instead of being highlighted in blue, the element is highlighted in orange, with "JS" next to the element name.</span></span> <span data-ttu-id="f55aa-260">Indica che l'elemento è stato creato in modo dinamico tramite script.</span><span class="sxs-lookup"><span data-stu-id="f55aa-260">This indicates that the element was created dynamically through script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

<span data-ttu-id="f55aa-261">Inoltre, è presente un carattere di sottolineatura arancione il **Stack di chiamate** scheda. Ciò indica che il **Stack di chiamate** riquadro è disponibili altre informazioni sull'elemento.</span><span class="sxs-lookup"><span data-stu-id="f55aa-261">In addition, an orange underline appears on the **Call Stack** tab. This indicates that the **Call Stack** pane has more information about the element.</span></span>

<span data-ttu-id="f55aa-262">Fare clic su di **Stack di chiamate** scheda. Il **Stack di chiamate** riquadro mostra lo stack di chiamate per la chiamata di JavaScript che ha creato l'elemento.</span><span class="sxs-lookup"><span data-stu-id="f55aa-262">Click on the **Call Stack** tab. The **Call Stack** pane shows the call stack for the JavaScript call that created the element.</span></span> <span data-ttu-id="f55aa-263">Chiamate a librerie esterne, ad esempio jQuery sono compressi, in modo che è possibile visualizzare facilmente le chiamate per lo script dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f55aa-263">Calls to external libraries such as jQuery are collapsed, so that you can easily see the calls to your application script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

<span data-ttu-id="f55aa-264">Per visualizzare lo stack completo, incluse le chiamate a librerie esterne, è possibile espandere i nodi con etichettati "Librerie esterne":</span><span class="sxs-lookup"><span data-stu-id="f55aa-264">To see the full stack, including calls to external libraries, you can expand the nodes labeled "External Libraries":</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

<span data-ttu-id="f55aa-265">Se si fa clic su un elemento nello stack di chiamate, Visual Studio apre il file di codice ed evidenzia script corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="f55aa-265">If you click an item in the call stack, Visual Studio opens the code file and highlights the corresponding script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a><span data-ttu-id="f55aa-266">Vedere anche</span><span class="sxs-lookup"><span data-stu-id="f55aa-266">See Also</span></span>

<span data-ttu-id="f55aa-267">[Introduzione ad ASP.NET MVC 4 con Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (sito Web ASP.net)</span><span class="sxs-lookup"><span data-stu-id="f55aa-267">[Intro to ASP.NET MVC 4 with Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net website)</span></span>

<span data-ttu-id="f55aa-268">[Introduzione a controllo pagina](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (video di Channel 9)</span><span class="sxs-lookup"><span data-stu-id="f55aa-268">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)</span></span>

<span data-ttu-id="f55aa-269">[Messaggi di errore di Page Inspector](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="f55aa-269">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
