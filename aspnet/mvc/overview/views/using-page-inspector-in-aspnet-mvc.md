---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: Utilizzo di controllo pagina in ASP.NET MVC | Microsoft Docs
author: rick-anderson
description: Controllo pagina in Visual Studio 2012 è uno strumento di sviluppo web con un browser integrato. Selezionare qualsiasi elemento nel browser integrato e controllo pagina i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 032be78e744bdf2c74337c72afb4efb7471ae4f9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399143"
---
<a name="using-page-inspector-in-aspnet-mvc"></a><span data-ttu-id="824fd-104">Utilizzo di Controllo pagina in ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="824fd-104">Using Page Inspector in ASP.NET MVC</span></span>
====================
<span data-ttu-id="824fd-105">da Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="824fd-105">by Tim Ammann</span></span>

> <span data-ttu-id="824fd-106">Controllo pagina in Visual Studio 2012 è uno strumento di sviluppo web con un browser integrato.</span><span class="sxs-lookup"><span data-stu-id="824fd-106">Page Inspector in Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="824fd-107">Selezionare un elemento nel browser integrato e controllo pagina evidenzia immediatamente l'origine e CSS dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="824fd-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="824fd-108">È possibile esplorare tutte le visualizzazioni MVC, rapidamente trovare le origini di markup sottoposto a rendering e usare gli strumenti del browser direttamente all'interno dell'ambiente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="824fd-108">You can browse any MVC view, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> [<span data-ttu-id="824fd-109">Guarda il Video</span><span class="sxs-lookup"><span data-stu-id="824fd-109">Watch the Video</span></span>](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> <span data-ttu-id="824fd-110">Questa esercitazione illustra come abilitare la modalità controllo e quindi individuare e modificare rapidamente CSS e markup all'interno del progetto web.</span><span class="sxs-lookup"><span data-stu-id="824fd-110">This tutorial shows how to enable Inspection Mode, and then quickly locate and edit markup and CSS within your web project.</span></span> <span data-ttu-id="824fd-111">L'esercitazione Usa un progetto MVC, ma è anche possibile utilizzare controllo pagina per [Web Form](https://go.microsoft.com/?linkid=9802001) e altre applicazioni ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="824fd-111">The tutorial uses an MVC Project, but you can also use Page Inspector for [Web Forms](https://go.microsoft.com/?linkid=9802001) and other ASP.NET applications.</span></span>
> 
> <span data-ttu-id="824fd-112">L'esercitazione include le sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="824fd-112">The tutorial has the following sections:</span></span>
> 
> - [<span data-ttu-id="824fd-113">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="824fd-113">Prerequisites</span></span>](#_1_prerequisites)
> - [<span data-ttu-id="824fd-114">Creare un'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="824fd-114">Create a Web Application</span></span>](#_2_creating_a)
> - [<span data-ttu-id="824fd-115">Utilizzare controllo pagina per passare a una vista</span><span class="sxs-lookup"><span data-stu-id="824fd-115">Use Page Inspector to Browse to a View</span></span>](#_3_using_page)
> - [<span data-ttu-id="824fd-116">Abilitare la modalità controllo</span><span class="sxs-lookup"><span data-stu-id="824fd-116">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> - [<span data-ttu-id="824fd-117">Utilizzare controllo pagina per apportare modifiche al Markup</span><span class="sxs-lookup"><span data-stu-id="824fd-117">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> - [<span data-ttu-id="824fd-118">Modalità controllo e la finestra HTML</span><span class="sxs-lookup"><span data-stu-id="824fd-118">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> - [<span data-ttu-id="824fd-119">Anteprima modifiche CSS nella finestra stili</span><span class="sxs-lookup"><span data-stu-id="824fd-119">Preview CSS Changes in the Styles window</span></span>](#_7_previewing_css)
> - [<span data-ttu-id="824fd-120">Sincronizzazione automatica CSS</span><span class="sxs-lookup"><span data-stu-id="824fd-120">CSS Auto Sync</span></span>](#css_auto_sync)
> - [<span data-ttu-id="824fd-121">Tramite la selezione colori CSS</span><span class="sxs-lookup"><span data-stu-id="824fd-121">Using the CSS Color Picker</span></span>](#css_color_picker)
> - [<span data-ttu-id="824fd-122">Mapping di elementi della pagina dinamica a JavaScript</span><span class="sxs-lookup"><span data-stu-id="824fd-122">Mapping Dynamic Page Elements to JavaScript</span></span>](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="824fd-123">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="824fd-123">Prerequisites</span></span>

- <span data-ttu-id="824fd-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) oppure [Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="824fd-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="824fd-125">Per ottenere la versione più recente di controllo pagina, usare [instalace Webové Platformy](https://go.microsoft.com/fwlink/?LinkId=255386) per installare Windows Azure SDK per .NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="824fd-125">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Windows Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="824fd-126">Page Inspector è abbinato a Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="824fd-126">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="824fd-127">La versione più recente è 1.3x.</span><span class="sxs-lookup"><span data-stu-id="824fd-127">The latest version is 1.3.</span></span> <span data-ttu-id="824fd-128">Per verificare quale versione hanno, eseguire Visual Studio e selezionare **informazioni su Microsoft Visual Studio** dal **Guida** menu.</span><span class="sxs-lookup"><span data-stu-id="824fd-128">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="824fd-129">Creare un'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="824fd-129">Create a Web Application</span></span>

<span data-ttu-id="824fd-130">Innanzitutto, creare un'applicazione web che verrà utilizzato il controllo di pagina con.</span><span class="sxs-lookup"><span data-stu-id="824fd-130">First, create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="824fd-131">In Visual Studio, scegliere **File** &gt; **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="824fd-131">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="824fd-132">A sinistra, espandere **Visual c#**, selezionare **Web**, quindi selezionare **applicazione Web di ASP.NET MVC4**.</span><span class="sxs-lookup"><span data-stu-id="824fd-132">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span>

![Nuova applicazione MVC ASP.NET](using-page-inspector-in-aspnet-mvc/_static/image2.png)

<span data-ttu-id="824fd-134">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="824fd-134">Click **OK**.</span></span>

<span data-ttu-id="824fd-135">Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo **applicazione Internet**.</span><span class="sxs-lookup"><span data-stu-id="824fd-135">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="824fd-136">Lasciare **Razor** come motore di visualizzazione predefinito.</span><span class="sxs-lookup"><span data-stu-id="824fd-136">Leave **Razor** as the default view engine.</span></span>

![Nuovo progetto ASP.NET MVC - applicazione Internet](using-page-inspector-in-aspnet-mvc/_static/image4.png)

<span data-ttu-id="824fd-138">L'applicazione viene aperto in **origine** visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="824fd-138">The application opens in **Source** view.</span></span>

![Nuova applicazione MVC ASP.NET nella visualizzazione origine](using-page-inspector-in-aspnet-mvc/_static/image6.png)

<span data-ttu-id="824fd-140">Dopo aver creato un'applicazione di usare, è possibile utilizzare controllo pagina per esaminare e modificarlo.</span><span class="sxs-lookup"><span data-stu-id="824fd-140">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a><span data-ttu-id="824fd-141">Utilizzare controllo pagina per passare a una vista</span><span class="sxs-lookup"><span data-stu-id="824fd-141">Use Page Inspector to Browse to a View</span></span>

<span data-ttu-id="824fd-142">In Visual Studio 2012, è possibile fare doppio clic qualsiasi visualizzazione nel progetto, seleziona **Visualizza in controllo pagina**, controllo pagina verrà capire la route e visualizzare la pagina.</span><span class="sxs-lookup"><span data-stu-id="824fd-142">In Visual Studio 2012, you can right-click any view in your project, select **View in Page Inspector**, and Page Inspector will figure out the route and display the page.</span></span>

<span data-ttu-id="824fd-143">Nelle **Esplora soluzioni**, espandere il **viste** cartella e quindi il **Home** cartella.</span><span class="sxs-lookup"><span data-stu-id="824fd-143">In **Solution Explorer**, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="824fd-144">Fare clic con il pulsante destro il file index. cshtml e scegliere **Visualizza in controllo pagina**.</span><span class="sxs-lookup"><span data-stu-id="824fd-144">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

![Visualizzazione index. cshtml in controllo pagina](using-page-inspector-in-aspnet-mvc/_static/image8.png)

<span data-ttu-id="824fd-146">Per impostazione predefinita, controllo pagina è ancorato come finestra sul lato sinistro dell'ambiente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="824fd-146">By default, Page Inspector is docked as a window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="824fd-147">Se si preferisce, è possibile ancorarlo altrove o disancorare la finestra.</span><span class="sxs-lookup"><span data-stu-id="824fd-147">If you prefer, you can dock it elsewhere, or undock the window.</span></span> <span data-ttu-id="824fd-148">Visualizzare [procedura: disporre e ancorare Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span><span class="sxs-lookup"><span data-stu-id="824fd-148">See [How to: Arrange and Dock Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span></span>

<span data-ttu-id="824fd-149">Il riquadro superiore della finestra di controllo pagina Mostra la pagina corrente in una finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="824fd-149">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="824fd-150">Nel riquadro inferiore mostra la pagina nel markup HTML con alcune schede che consentono di controllare diversi aspetti della pagina.</span><span class="sxs-lookup"><span data-stu-id="824fd-150">The bottom pane shows the page in HTML markup, along with some tabs that let you inspect different aspects of the page.</span></span> <span data-ttu-id="824fd-151">Il riquadro inferiore è simile al [strumenti di sviluppo F12](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="824fd-151">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span>

![Applicazione ASP.NET MVC in controllo pagina](using-page-inspector-in-aspnet-mvc/_static/image10.png)

<span data-ttu-id="824fd-153">In questa esercitazione si userà il **HTML** e **stili** schede per passare rapidamente e apportare modifiche all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="824fd-153">In this tutorial, you will use the **HTML** and **Styles** tabs to navigate quickly and make changes to the application.</span></span>

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a><span data-ttu-id="824fd-154">Modalità EnableInspection</span><span class="sxs-lookup"><span data-stu-id="824fd-154">EnableInspection Mode</span></span>

<span data-ttu-id="824fd-155">Per attivare la modalità controllo di controllo pagina, scegliere il **Inspect** pulsante.</span><span class="sxs-lookup"><span data-stu-id="824fd-155">To put Page Inspector into Inspection Mode, click the **Inspect** button.</span></span> <span data-ttu-id="824fd-156">In modalità controllo, quando si posiziona il puntatore del mouse su un punto qualsiasi della pagina di cui è stato eseguito rendering, il codice o markup di origine corrispondente viene evidenziato.</span><span class="sxs-lookup"><span data-stu-id="824fd-156">In Inspection Mode, when you hold the mouse pointer over any part of the rendered page, the corresponding source markup or code is highlighted.</span></span>

![Modalità controllo Attiva/disattiva](using-page-inspector-in-aspnet-mvc/_static/image12.png)

<span data-ttu-id="824fd-158">A questo punto passa il mouse su parti diverse della pagina in controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="824fd-158">Now move your mouse over different parts of the page within Page Inspector.</span></span> <span data-ttu-id="824fd-159">Come avviene, il puntatore del mouse diventa un segno più grande e viene evidenziato l'elemento di sotto:</span><span class="sxs-lookup"><span data-stu-id="824fd-159">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Posizionare il mouse su div.content wrapper](using-page-inspector-in-aspnet-mvc/_static/image14.png)

<span data-ttu-id="824fd-161">Quando si sposta il puntatore del mouse, Visual Studio evidenzia la sintassi Razor corrispondente nel file di origine.</span><span class="sxs-lookup"><span data-stu-id="824fd-161">As you move the mouse pointer, Visual Studio highlights the corresponding Razor syntax in the source file.</span></span> <span data-ttu-id="824fd-162">Se l'elemento HTML proviene da un altro file di origine, Visual Studio apre automaticamente il file.</span><span class="sxs-lookup"><span data-stu-id="824fd-162">If the HTML element comes from another source file, Visual Studio automatically opens the file.</span></span>

<span data-ttu-id="824fd-163">In controllo pagina, il **HTML** scheda Mostra il codice HTML generato dalla sintassi Razor.</span><span class="sxs-lookup"><span data-stu-id="824fd-163">In Page Inspector, the **HTML** tab shows the HTML that was generated from the Razor syntax.</span></span> <span data-ttu-id="824fd-164">Quando si sposta il puntatore del mouse, vengono evidenziati gli elementi HTML.</span><span class="sxs-lookup"><span data-stu-id="824fd-164">As you move the mouse pointer, the HTML elements are highlighted.</span></span> <span data-ttu-id="824fd-165">Il **stili** scheda Mostra le regole CSS per l'elemento.</span><span class="sxs-lookup"><span data-stu-id="824fd-165">The **Styles** tab shows the CSS rules for the element.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="824fd-166">Utilizzare controllo pagina per apportare modifiche al Markup</span><span class="sxs-lookup"><span data-stu-id="824fd-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="824fd-167">Page Inspector consente di trovare markup la cui posizione potrebbe non essere evidente.</span><span class="sxs-lookup"><span data-stu-id="824fd-167">Page Inspector lets you find markup whose location might not be obvious.</span></span> <span data-ttu-id="824fd-168">È possibile modificare il markup e visualizzare le modifiche risultante.</span><span class="sxs-lookup"><span data-stu-id="824fd-168">Then you can modify the markup and see the resulting changes.</span></span>

<span data-ttu-id="824fd-169">Per verificarlo, fare clic su **Inspect** e quindi scorrere fino alla parte inferiore della pagina nella finestra di controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="824fd-169">To see this, click **Inspect** and then scroll to the bottom of the page in the Page Inspector window.</span></span>

<span data-ttu-id="824fd-170">Quando si sposta il puntatore del mouse nell'area di piè di pagina, controllo pagina apre il \_file di layout. cshtml ed evidenzia la sezione della pagina di layout che è stata selezionata.</span><span class="sxs-lookup"><span data-stu-id="824fd-170">When you move the mouse pointer into the footer area, Page Inspector opens the \_Layout.cshtml file and highlights the section of the layout page that you have selected.</span></span> <span data-ttu-id="824fd-171">Come si può notare, sono il piè di pagina è definita nel file di layout e non la visualizzazione stessa.</span><span class="sxs-lookup"><span data-stu-id="824fd-171">As you can see, the footer are is defined in the layout file, and not the view itself.</span></span>

![Piè di pagina](using-page-inspector-in-aspnet-mvc/_static/image16.png)

<span data-ttu-id="824fd-173">Ora passare il puntatore del mouse sulla riga con il copyright <a id="a"> </a>notare.</span><span class="sxs-lookup"><span data-stu-id="824fd-173">Now move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span> <span data-ttu-id="824fd-174">Nel \_pagina layout. cshtml, la riga corrispondente viene evidenziato.</span><span class="sxs-lookup"><span data-stu-id="824fd-174">In the \_Layout.cshtml page, the corresponding line is highlighted.</span></span>

![Riga di piè di pagina informazioni sul copyright evidenziata](using-page-inspector-in-aspnet-mvc/_static/image18.png)

<span data-ttu-id="824fd-176">Aggiungere testo alla fine della riga di \_file di layout. cshtml.</span><span class="sxs-lookup"><span data-stu-id="824fd-176">Add some text to the end of the line in the \_Layout.cshtml file.</span></span>

<span data-ttu-id="824fd-177">&lt;p&gt;&amp;duplicarlo; @DateTime.Now.Year -Applicazione ASP.NET MVC Rocks!  &lt; /p&gt;</span><span class="sxs-lookup"><span data-stu-id="824fd-177">&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET MVC Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="824fd-178">A questo punto, premere Ctrl + Alt + INVIO o fare clic sulla barra di aggiornamento per visualizzare i risultati nella finestra del browser di controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="824fd-178">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![Funziona l'applicazione ASP.NET!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

<span data-ttu-id="824fd-180">Si potrebbe avrai pensato che il piè di pagina definite in Index. cshtml, ma si è scoperto nel \_layout. cshtml e controllo pagina ha rilevato che per l'utente.</span><span class="sxs-lookup"><span data-stu-id="824fd-180">You might have thought that the footer defined in Index.cshtml, but it turned out to be in the \_Layout.cshtml, and Page Inspector found it for you.</span></span>

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="824fd-181">Modalità controllo e la finestra HTML</span><span class="sxs-lookup"><span data-stu-id="824fd-181">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="824fd-182">Successivamente, si avrà un'occhiata alla finestra HTML e il relativo mapping elementi per l'utente.</span><span class="sxs-lookup"><span data-stu-id="824fd-182">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="824fd-183">Fare clic su **Inspect** inserire controllo pagina in modalità controllo.</span><span class="sxs-lookup"><span data-stu-id="824fd-183">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="824fd-184">Fare clic sulla parte superiore della pagina con la dicitura "Il logohere".</span><span class="sxs-lookup"><span data-stu-id="824fd-184">Click the top part of the page that says "Your logohere".</span></span> <span data-ttu-id="824fd-185">Se si sta esaminando un particolare elemento in modo più dettagliato, in modo che la visualizzazione nella finestra del browser non è più viene modificato quando si sposta il puntatore del mouse.</span><span class="sxs-lookup"><span data-stu-id="824fd-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="824fd-186">Ora passare il puntatore del mouse per il **HTML** finestra.</span><span class="sxs-lookup"><span data-stu-id="824fd-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="824fd-187">Quando si sposta il puntatore del mouse, controllo pagina vengono illustrati l'elemento all'interno di **HTML** finestra ed evidenzia l'elemento corrispondente nella finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="824fd-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Finestra HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

<span data-ttu-id="824fd-189">Come in precedenza, controllo pagina apre il \_file di layout. cshtml per l'utente in una scheda temporanea. Fare clic sui \_layout. cshtml scheda temporanea e il markup corrispondente verrà evidenziato nel &lt;intestazione&gt; sezione per l'utente:</span><span class="sxs-lookup"><span data-stu-id="824fd-189">As before, Page Inspector opens the \_Layout.cshtml file for you in a temporary tab. Click the \_Layout.cshtml temporary tab, and the corresponding markup will be highlighted in the &lt;header&gt; section for you:</span></span>

![Markup evidenziato](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="824fd-191">Anteprima modifiche CSS nella finestra stili</span><span class="sxs-lookup"><span data-stu-id="824fd-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="824fd-192">Successivamente, si userà la Page Inspector **stili** finestra per visualizzare in anteprima le modifiche a CSS.</span><span class="sxs-lookup"><span data-stu-id="824fd-192">Next, you will use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="824fd-193">Fare clic su **Inspect** inserire controllo pagina in modalità controllo.</span><span class="sxs-lookup"><span data-stu-id="824fd-193">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="824fd-194">Nella finestra del browser di controllo pagina, spostare il puntatore del mouse sopra la sezione "Home Page" finché il **div.content wrapper** etichetta viene visualizzata.</span><span class="sxs-lookup"><span data-stu-id="824fd-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Posizionare il mouse su div.content wrapper](using-page-inspector-in-aspnet-mvc/_static/image26.png)

<span data-ttu-id="824fd-196">Fare clic su una sola volta all'interno della sezione div.content wrapper, e quindi spostare il puntatore del mouse per il **stili** finestra.</span><span class="sxs-lookup"><span data-stu-id="824fd-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="824fd-197">Il **stili** finestra Mostra tutte le regole CSS per questo elemento.</span><span class="sxs-lookup"><span data-stu-id="824fd-197">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="824fd-198">Scorrere verso il basso il selettore di classe find .featured. Content-wrapper.</span><span class="sxs-lookup"><span data-stu-id="824fd-198">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="824fd-199">A questo punto deselezionare la casella di controllo per la proprietà di colore di sfondo.</span><span class="sxs-lookup"><span data-stu-id="824fd-199">Now clear the checkbox for the background-color property.</span></span>

![Colore di sfondo chiaro](using-page-inspector-in-aspnet-mvc/_static/image28.png)

<span data-ttu-id="824fd-201">Si noti come la modifica include l'anteprima immediatamente nella finestra del browser di controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="824fd-201">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="824fd-202">Selezionare la casella di controllo di nuovo, quindi fare doppio clic sul valore della proprietà e modificarlo in rosso.</span><span class="sxs-lookup"><span data-stu-id="824fd-202">Select the checkbox again, then double-click the property value and change it to red.</span></span> <span data-ttu-id="824fd-203">La modifica viene immediatamente:</span><span class="sxs-lookup"><span data-stu-id="824fd-203">The change shows immediately:</span></span>

![Colore di sfondo rosso](using-page-inspector-in-aspnet-mvc/_static/image30.png)

<span data-ttu-id="824fd-205">Il **stili** rende finestra è più facile testare e visualizzare in anteprima CSS viene modificato prima di procedere effettivamente le modifiche apportate allo stile della finestra stessa.</span><span class="sxs-lookup"><span data-stu-id="824fd-205">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="824fd-206">Sincronizzazione automatica CSS</span><span class="sxs-lookup"><span data-stu-id="824fd-206">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="824fd-207">Questa funzionalità richiede la versione 1.3 di controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="824fd-207">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="824fd-208">La funzionalità di sincronizzazione automatica CSS consente di modificare direttamente un file CSS e visualizzare le modifiche immediatamente nel browser di controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="824fd-208">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="824fd-209">Fare clic su **Inspect** inserire controllo pagina in modalità controllo.</span><span class="sxs-lookup"><span data-stu-id="824fd-209">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="824fd-210">Nel browser di controllo pagina, spostare il puntatore del mouse sopra la sezione "Home Page" finché il **div.content wrapper** etichetta viene visualizzata.</span><span class="sxs-lookup"><span data-stu-id="824fd-210">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="824fd-211">Fare clic su una sola volta per selezionare questo elemento.</span><span class="sxs-lookup"><span data-stu-id="824fd-211">Click once to select this element.</span></span>

<span data-ttu-id="824fd-212">Il **stili** finestra Mostra tutte le regole CSS per questo elemento.</span><span class="sxs-lookup"><span data-stu-id="824fd-212">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="824fd-213">Scorrere verso il basso il selettore di classe find .featured. Content-wrapper.</span><span class="sxs-lookup"><span data-stu-id="824fd-213">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="824fd-214">Fare clic su .featured. Content-"wrapper".</span><span class="sxs-lookup"><span data-stu-id="824fd-214">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="824fd-215">Controllo pagina apre il file CSS che definisce questo stile (CSS) ed evidenzia il CSS corrispondente.</span><span class="sxs-lookup"><span data-stu-id="824fd-215">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

<span data-ttu-id="824fd-216">A questo punto modificare il valore per `background-color` su "red".</span><span class="sxs-lookup"><span data-stu-id="824fd-216">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="824fd-217">La modifica viene visualizzata immediatamente nel browser di controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="824fd-217">The change appears immediately in the Page Inspector browser.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a><span data-ttu-id="824fd-218">Tramite la selezione colori CSS</span><span class="sxs-lookup"><span data-stu-id="824fd-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="824fd-219">Editor CSS in Visual Studio 2012 è una selezione colori che consente di scegliere e inserire i colori.</span><span class="sxs-lookup"><span data-stu-id="824fd-219">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="824fd-220">La selezione colori include una tavolozza di colori standard supporta i nomi dei colori standard, codici hash, i colori RGB, RGBA, HSL e HSLA e gestisce un elenco dei colori utilizzati più di recente nel documento.</span><span class="sxs-lookup"><span data-stu-id="824fd-220">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="824fd-221">Nella sezione precedente, è stato modificato il valore di `background-color` proprietà.</span><span class="sxs-lookup"><span data-stu-id="824fd-221">In the previous section, you changed the value of the `background-color` property.</span></span> <span data-ttu-id="824fd-222">Per richiamare la selezione colori, posizionare il cursore dopo il nome della proprietà e digitare **#** oppure **rgb (**.</span><span class="sxs-lookup"><span data-stu-id="824fd-222">To invoke the color picker, place the insertion point after the property name and type **#** or **rgb(**.</span></span>

![La barra di selezione colori CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

<span data-ttu-id="824fd-224">Fare clic su un colore per selezionarlo oppure premere il tasto freccia giù e quindi utilizzare i tasti freccia sinistra e destra per attraversare i colori.</span><span class="sxs-lookup"><span data-stu-id="824fd-224">Click on a color to select it, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="824fd-225">Quando si visita un colore, viene visualizzato in anteprima il corrispondente valore esadecimale:</span><span class="sxs-lookup"><span data-stu-id="824fd-225">When you visit a color, the corresponding hex value is previewed:</span></span>

![valore della proprietà di colore di sfondo visualizzata in anteprima](using-page-inspector-in-aspnet-mvc/_static/image38.png)

<span data-ttu-id="824fd-227">Se la barra dei colori non è il colore esatto desiderato, è possibile usare la selezione di colore pop-down.</span><span class="sxs-lookup"><span data-stu-id="824fd-227">If the color bar doesn't have the exact color you want, you can use the color picker pop-down.</span></span> <span data-ttu-id="824fd-228">Per aprirlo, fare clic sulle virgolette doppie all'estremità destra della barra dei colori o premere una o due volte il tasto freccia giù sulla tastiera.</span><span class="sxs-lookup"><span data-stu-id="824fd-228">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Selezione di colore CSS Pop-Down](using-page-inspector-in-aspnet-mvc/_static/image40.png)

<span data-ttu-id="824fd-230">Fare clic su un altro colore nella barra verticale sul lato destro.</span><span class="sxs-lookup"><span data-stu-id="824fd-230">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="824fd-231">Mostra una sfumatura di colore nella finestra principale.</span><span class="sxs-lookup"><span data-stu-id="824fd-231">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="824fd-232">Scegliere un colore direttamente dalla barra degli strumenti verticale premendo INVIO oppure fare clic su qualsiasi punto nella finestra principale di scegliere con precisione maggiore.</span><span class="sxs-lookup"><span data-stu-id="824fd-232">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="824fd-233">Se è presente un colore sullo schermo del computer che si desidera utilizzare (non deve necessariamente essere all'interno dell'interfaccia utente di Visual Studio), è possibile acquisire il relativo valore con lo strumento contagocce in basso a destra.</span><span class="sxs-lookup"><span data-stu-id="824fd-233">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="824fd-234">È anche possibile modificare l'opacità di colore spostando il dispositivo di scorrimento nella parte inferiore della selezione colori.</span><span class="sxs-lookup"><span data-stu-id="824fd-234">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="824fd-235">In questo modo le modifiche dei colori dei valori di valori RGBA, perché il formato RGBA può rappresentare l'opacità.</span><span class="sxs-lookup"><span data-stu-id="824fd-235">Doing so changes color values to RGBA values, because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="824fd-236">Dopo aver scelto un colore, premere INVIO e quindi digitare un punto e virgola per completare la voce di colore di sfondo nel *CSS* file.</span><span class="sxs-lookup"><span data-stu-id="824fd-236">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="824fd-237">La barra di aggiornamento di controllo pagina</span><span class="sxs-lookup"><span data-stu-id="824fd-237">The Page Inspector Update Bar</span></span>

<span data-ttu-id="824fd-238">Controllo pagina immediatamente rileva la modifica per il *CSS* file e viene visualizzato un avviso in una barra di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="824fd-238">Page Inspector immediately detects the change to the *Site.css* file and displays an alert in an update bar.</span></span>

![Barra di aggiornamento](using-page-inspector-in-aspnet-mvc/_static/image42.png)

<span data-ttu-id="824fd-240">Per salvare tutti i file e aggiornare il browser di controllo pagina, premere Ctrl + Alt + INVIO o fare clic sulla barra di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="824fd-240">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="824fd-241">La modifica il colore di evidenziazione viene visualizzata nel browser.</span><span class="sxs-lookup"><span data-stu-id="824fd-241">The change in the highlight color appears in the browser.</span></span>

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a><span data-ttu-id="824fd-242">Mapping di elementi della pagina dinamica a JavaScript</span><span class="sxs-lookup"><span data-stu-id="824fd-242">Mapping Dynamic Page Elements to JavaScript</span></span>

<span data-ttu-id="824fd-243">Nelle applicazioni web moderne, gli elementi della pagina vengono spesso generati in modo dinamico con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="824fd-243">In modern web applications, elements in the page are often generated dynamically with JavaScript.</span></span> <span data-ttu-id="824fd-244">Ciò significa che non sono presenti markup statici, HTML o Razor, che corrisponde a questi elementi di pagina.</span><span class="sxs-lookup"><span data-stu-id="824fd-244">That means there is no static markup (either HTML or Razor) that corresponds to these page elements.</span></span>

<span data-ttu-id="824fd-245">Con la versione 1.3, controllo pagina può ora eseguire il mapping di elementi che sono stati aggiunti in modo dinamico alla pagina di tornare al codice JavaScript corrispondente.</span><span class="sxs-lookup"><span data-stu-id="824fd-245">With version 1.3, Page Inspector can now map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span> <span data-ttu-id="824fd-246">Per illustrare questa funzionalità, si userà il [modello di applicazione a pagina singola (SPA)](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="824fd-246">To demonstrate this feature, we'll use the [Single Page Application (SPA) template](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span></span>

> [!NOTE]
> <span data-ttu-id="824fd-247">Il modello di applicazione a singola pagina richiede la [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) aggiornare.</span><span class="sxs-lookup"><span data-stu-id="824fd-247">The SPA template requires the [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) update.</span></span>


<span data-ttu-id="824fd-248">In Visual Studio, scegliere **File** &gt; **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="824fd-248">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="824fd-249">A sinistra, espandere **Visual c#**, selezionare **Web**, quindi selezionare **applicazione Web di ASP.NET MVC4**.</span><span class="sxs-lookup"><span data-stu-id="824fd-249">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span> <span data-ttu-id="824fd-250">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="824fd-250">Click **OK**.</span></span>

<span data-ttu-id="824fd-251">Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo, seleziona **applicazione a pagina singola**.</span><span class="sxs-lookup"><span data-stu-id="824fd-251">In the **New ASP.NET MVC 4 Project** dialog, select **Single Page Application**.</span></span>

<span data-ttu-id="824fd-252">In Esplora soluzioni espandere la **viste** cartella e quindi la **Home** cartella.</span><span class="sxs-lookup"><span data-stu-id="824fd-252">In Solution Explorer, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="824fd-253">Fare clic con il pulsante destro il file index. cshtml e scegliere **Visualizza in controllo pagina**.</span><span class="sxs-lookup"><span data-stu-id="824fd-253">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

<span data-ttu-id="824fd-254">Il primo elemento visualizzato nel browser di controllo pagina è una pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="824fd-254">The first thing that is displayed in the Page Inspector browser is a login page.</span></span> <span data-ttu-id="824fd-255">Fare clic su "Iscrizione" e creare un nome utente e password.</span><span class="sxs-lookup"><span data-stu-id="824fd-255">Click "Sign Up" and create a user name and password.</span></span> <span data-ttu-id="824fd-256">Quando si effettua l'iscrizione, l'applicazione esegue l'accesso e crea un elenco di cose da fare con alcuni elementi di esempio.</span><span class="sxs-lookup"><span data-stu-id="824fd-256">Once you sign up, the application logs you in and creates a to-do list with some sample items.</span></span>

<span data-ttu-id="824fd-257">Fare clic su **Inspect** inserire controllo pagina in modalità controllo.</span><span class="sxs-lookup"><span data-stu-id="824fd-257">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span> <span data-ttu-id="824fd-258">Nel browser di controllo pagina, fare clic su uno degli elementi di attività da eseguire.</span><span class="sxs-lookup"><span data-stu-id="824fd-258">In the Page Inspector browser, click on one of the to-do items.</span></span> <span data-ttu-id="824fd-259">Si noti che invece viene evidenziato in blu, l'elemento viene evidenziato in arancione, con "JS" accanto al nome dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="824fd-259">Notice that instead of being highlighted in blue, the element is highlighted in orange, with "JS" next to the element name.</span></span> <span data-ttu-id="824fd-260">Ciò indica che l'elemento è stato creato in modo dinamico tramite script.</span><span class="sxs-lookup"><span data-stu-id="824fd-260">This indicates that the element was created dynamically through script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

<span data-ttu-id="824fd-261">Inoltre, in cui viene visualizzata una sottolineatura arancione nel **Stack di chiamate** scheda. Ciò indica che il **Stack di chiamate** riquadro contiene ulteriori informazioni sull'elemento.</span><span class="sxs-lookup"><span data-stu-id="824fd-261">In addition, an orange underline appears on the **Call Stack** tab. This indicates that the **Call Stack** pane has more information about the element.</span></span>

<span data-ttu-id="824fd-262">Fare clic sui **Stack di chiamate** scheda. Il **Stack di chiamate** riquadro mostra lo stack di chiamate per la chiamata di JavaScript che ha creato l'elemento.</span><span class="sxs-lookup"><span data-stu-id="824fd-262">Click on the **Call Stack** tab. The **Call Stack** pane shows the call stack for the JavaScript call that created the element.</span></span> <span data-ttu-id="824fd-263">Le chiamate a librerie esterne, ad esempio jQuery vengono compresse, in modo che è possibile visualizzare facilmente le chiamate per lo script di applicazione.</span><span class="sxs-lookup"><span data-stu-id="824fd-263">Calls to external libraries such as jQuery are collapsed, so that you can easily see the calls to your application script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

<span data-ttu-id="824fd-264">Per visualizzare lo stack completo, comprese chiamate alle librerie esterne, è possibile espandere i nodi con etichettati "Librerie esterne":</span><span class="sxs-lookup"><span data-stu-id="824fd-264">To see the full stack, including calls to external libraries, you can expand the nodes labeled "External Libraries":</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

<span data-ttu-id="824fd-265">Se si fa clic su un elemento nello stack di chiamate, Visual Studio apre il file di codice e viene evidenziato lo script corrispondente.</span><span class="sxs-lookup"><span data-stu-id="824fd-265">If you click an item in the call stack, Visual Studio opens the code file and highlights the corresponding script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a><span data-ttu-id="824fd-266">Vedere anche</span><span class="sxs-lookup"><span data-stu-id="824fd-266">See Also</span></span>

<span data-ttu-id="824fd-267">[Introduzione ad ASP.NET MVC 4 con Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (sito Web ASP.net)</span><span class="sxs-lookup"><span data-stu-id="824fd-267">[Intro to ASP.NET MVC 4 with Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net website)</span></span>

<span data-ttu-id="824fd-268">[Introduzione a controllo pagina](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (video di Channel 9)</span><span class="sxs-lookup"><span data-stu-id="824fd-268">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)</span></span>

<span data-ttu-id="824fd-269">[I messaggi di errore di controllo pagina](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="824fd-269">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
