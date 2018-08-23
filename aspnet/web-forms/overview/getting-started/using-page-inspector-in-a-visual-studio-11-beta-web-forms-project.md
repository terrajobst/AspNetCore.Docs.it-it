---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Utilizzo di controllo pagina per Visual Studio 2012 in ASP.NET Web Forms | Microsoft Docs
author: rick-anderson
description: Controllo pagina per Visual Studio 2012 è uno strumento di sviluppo web con un browser integrato. Selezionare qualsiasi elemento nel browser integrato e controllo pagina...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: d2c377f8466f8f324b75ce60860aa00c11bc0ffe
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838075"
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a><span data-ttu-id="39a21-104">Utilizzo di controllo pagina per Visual Studio 2012 in Web Form ASP.NET</span><span class="sxs-lookup"><span data-stu-id="39a21-104">Using Page Inspector for Visual Studio 2012 in ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="39a21-105">da Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="39a21-105">by Tim Ammann</span></span>

> <span data-ttu-id="39a21-106">Controllo pagina per Visual Studio 2012 è uno strumento di sviluppo web con un browser integrato.</span><span class="sxs-lookup"><span data-stu-id="39a21-106">Page Inspector for Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="39a21-107">Selezionare un elemento nel browser integrato e controllo pagina evidenzia immediatamente l'origine e CSS dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="39a21-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="39a21-108">È possibile esplorare tutte le pagine nell'applicazione, rapidamente trovare le origini di markup sottoposto a rendering e usare gli strumenti del browser direttamente all'interno dell'ambiente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="39a21-108">You can browse any page in your application, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> <span data-ttu-id="39a21-109">Questa esercitazione shwos come abilitare la modalità controllo rapidamente individuare e modificare le regole CSS e il testo all'interno del progetto web.</span><span class="sxs-lookup"><span data-stu-id="39a21-109">This tutorial shwos how to enable Inspection Mode and then quickly locate and edit CSS rules and text within your web project.</span></span> <span data-ttu-id="39a21-110">L'esercitazione Usa un progetto di applicazione Web Form, ma è anche possibile utilizzare controllo pagina per progetti di siti Web e [MVC](https://go.microsoft.com/?linkid=9802002) applicazioni.</span><span class="sxs-lookup"><span data-stu-id="39a21-110">The tutorial uses a Web Forms Application Project, but you can also use Page Inspector for Web Site projects and [MVC](https://go.microsoft.com/?linkid=9802002) applications.</span></span>
> 
> <span data-ttu-id="39a21-111">L'esercitazione include le sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="39a21-111">The tutorial has the following sections:</span></span>
> 
> [<span data-ttu-id="39a21-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="39a21-112">Prerequisites</span></span>](#_1_prerequisites)
> 
> [<span data-ttu-id="39a21-113">Creare un'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="39a21-113">Create a Web Application</span></span>](#_2_creating_a)
> 
> [<span data-ttu-id="39a21-114">Utilizzare controllo pagina per visualizzare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="39a21-114">Use Page Inspector to View the Application</span></span>](#_3_using_page)
> 
> [<span data-ttu-id="39a21-115">Abilitare la modalità controllo</span><span class="sxs-lookup"><span data-stu-id="39a21-115">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> 
> [<span data-ttu-id="39a21-116">Utilizzare controllo pagina per apportare modifiche al Markup</span><span class="sxs-lookup"><span data-stu-id="39a21-116">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> 
> [<span data-ttu-id="39a21-117">Modalità controllo e la finestra HTML</span><span class="sxs-lookup"><span data-stu-id="39a21-117">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> 
> [<span data-ttu-id="39a21-118">Anteprima modifiche CSS nella finestra stili</span><span class="sxs-lookup"><span data-stu-id="39a21-118">Preview CSS Changes in the Styles Window</span></span>](#_7_previewing_css)
> 
> [<span data-ttu-id="39a21-119">Sincronizzazione automatica CSS</span><span class="sxs-lookup"><span data-stu-id="39a21-119">CSS Auto Sync</span></span>](#css_auto_sync)
> 
> [<span data-ttu-id="39a21-120">Tramite la selezione colori CSS</span><span class="sxs-lookup"><span data-stu-id="39a21-120">Using the CSS Color Picker</span></span>](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="39a21-121">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="39a21-121">Prerequisites</span></span>

- <span data-ttu-id="39a21-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) oppure [Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="39a21-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="39a21-123">Per ottenere la versione più recente di controllo pagina, usare [instalace Webové Platformy](https://go.microsoft.com/fwlink/?LinkId=255386) per installare Azure SDK per .NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="39a21-123">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="39a21-124">Page Inspector è abbinato a Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="39a21-124">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="39a21-125">La versione più recente è 1.3x.</span><span class="sxs-lookup"><span data-stu-id="39a21-125">The latest version is 1.3.</span></span> <span data-ttu-id="39a21-126">Per verificare quale versione hanno, eseguire Visual Studio e selezionare **informazioni su Microsoft Visual Studio** dal **Guida** menu.</span><span class="sxs-lookup"><span data-stu-id="39a21-126">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="39a21-127">Creare un'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="39a21-127">Create a Web Application</span></span>

<span data-ttu-id="39a21-128">In primo luogo, si creerà un'applicazione web che verrà utilizzato il controllo di pagina con.</span><span class="sxs-lookup"><span data-stu-id="39a21-128">First, you will create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="39a21-129">In Visual Studio, scegliere **File** &gt; **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="39a21-129">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="39a21-130">A sinistra, espandere **Visual c#**, selezionare **Web**, quindi selezionare **applicazione Web Form ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="39a21-130">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET Web Forms Application**.</span></span>

![Nuova applicazione Web Form](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

<span data-ttu-id="39a21-132">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="39a21-132">Click **OK**.</span></span>

<span data-ttu-id="39a21-133">L'applicazione viene aperto in **origine** visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="39a21-133">The application opens in **Source** view.</span></span>

![Nuova applicazione Web Form nella visualizzazione origine](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

<span data-ttu-id="39a21-135">Dopo aver creato un'applicazione di usare, è possibile utilizzare controllo pagina per esaminare e modificarlo.</span><span class="sxs-lookup"><span data-stu-id="39a21-135">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a><span data-ttu-id="39a21-136">Utilizzare controllo pagina per visualizzare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="39a21-136">Use Page Inspector to View the Application</span></span>

<span data-ttu-id="39a21-137">Successivamente, si visualizzeranno l'applicazione con controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="39a21-137">Next, you will view the application with Page Inspector.</span></span> <span data-ttu-id="39a21-138">Nelle **Esplora soluzioni**, fare clic con il pulsante destro del progetto e quindi scegliere **Visualizza in controllo pagina**.</span><span class="sxs-lookup"><span data-stu-id="39a21-138">In **Solution Explorer**, right click the project, and then choose **View in Page Inspector**.</span></span>

![Visualizza in controllo pagina](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

<span data-ttu-id="39a21-140">Per impostazione predefinita, quando si avvia controllo pagina per la prima volta, è ancorata come una piccola finestra sul lato sinistro dell'ambiente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="39a21-140">By default, when Page Inspector launches for the first time, it is docked as a narrow window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="39a21-141">Lasciarla ancorata al lato sinistro e impostarla su una larghezza che ritiene adatto per l'utente oppure ancorarla in una delle aree degli strumenti in alto, nella parte inferiore o destro:</span><span class="sxs-lookup"><span data-stu-id="39a21-141">Leave it docked on the left side and set it to a width that is comfortable for you, or dock it in one of the tool areas on the top, bottom, or right:</span></span>

![Posizioni di ancoraggio di controllo pagina](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

<span data-ttu-id="39a21-143">Se si disancorare la finestra di controllo pagina, è possibile inserirlo all'esterno di Visual Studio o anche in un secondo monitor se presente.</span><span class="sxs-lookup"><span data-stu-id="39a21-143">If you undock the Page Inspector window, you can place it outside Visual Studio, or even on a second monitor if you have one.</span></span> <span data-ttu-id="39a21-144">Tuttavia, in ordine ALT + TAB tra controllo pagina e Visual Studio quando la finestra di controllo pagina è ancorata, passare a **degli strumenti** &gt; **opzioni** &gt;  **Ambiente** &gt; **schede e Windows**, quindi in **scheda anche**, deselezionare la casella di controllo denominato **finestre degli strumenti finestra mobile restano sempre in cima il finestra principale**:</span><span class="sxs-lookup"><span data-stu-id="39a21-144">However, in order to ALT+TAB between Page Inspector and Visual Studio when the Page Inspector window is undocked, go to **Tools** &gt; **Options** &gt; **Environment** &gt; **Tabs and Windows**, and under **Tab Well**, clear the check box called **Floating tool windows always stay on top of the main window**:</span></span>

![Deselezionare la casella di controllo di windows degli strumenti a virgola mobile per ALT + TAB tra Visual Studio e la finestra di controllo pagina non ancorata](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

<span data-ttu-id="39a21-146">Il riquadro superiore della finestra di controllo pagina Mostra la pagina corrente in una finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="39a21-146">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="39a21-147">Nel riquadro inferiore mostra la pagina nel markup HTML a sinistra e alcune schede a destra che consentono di controllare diversi aspetti della pagina.</span><span class="sxs-lookup"><span data-stu-id="39a21-147">The bottom pane shows the page in HTML markup on the left, and some tabs on the right that let you inspect different aspects of the page.</span></span> <span data-ttu-id="39a21-148">Il riquadro inferiore è simile al [strumenti di sviluppo F12](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="39a21-148">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span> <span data-ttu-id="39a21-149">(Tuttavia, diversamente da strumenti di sviluppo, è possibile utilizzare controllo pagina direttamente da Visual Studio.)</span><span class="sxs-lookup"><span data-stu-id="39a21-149">(However, unlike the developer tools, you can use Page Inspector right within Visual Studio.)</span></span>

![Controllo pagina](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

<span data-ttu-id="39a21-151">In questa esercitazione si userà il riquadro del browser di controllo pagina e il **HTML** e **stili** schede che consentono rapidamente passare e apportare modifiche all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="39a21-151">In this tutorial, you will use the Page Inspector browser pane, and the **HTML** and **Styles** tabs to help you rapidly navigate and make changes to the application.</span></span>

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a><span data-ttu-id="39a21-152">Abilitare la modalità controllo</span><span class="sxs-lookup"><span data-stu-id="39a21-152">Enable Inspection Mode</span></span>

<span data-ttu-id="39a21-153">Successivamente, si noterà come modalità di controllo di controllo pagina funziona.</span><span class="sxs-lookup"><span data-stu-id="39a21-153">Next, you will see how Page Inspector's Inspection Mode works.</span></span> <span data-ttu-id="39a21-154">Nella finestra di controllo pagina, scegliere il **Inspect** pulsante.</span><span class="sxs-lookup"><span data-stu-id="39a21-154">In the Page Inspector window, click the **Inspect** button.</span></span>

![Esaminare l'elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

<span data-ttu-id="39a21-156">Per visualizzare le modalità di controllo in azione, spostare il puntatore del mouse su parti diverse della pagina nella finestra del browser di controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="39a21-156">To see inspection mode in action, move your mouse over different parts of the page within the Page Inspector browser window.</span></span> <span data-ttu-id="39a21-157">Come avviene, il puntatore del mouse diventa un segno più grande e viene evidenziato l'elemento di sotto:</span><span class="sxs-lookup"><span data-stu-id="39a21-157">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Posizionare il mouse su div.content wrapper](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

<span data-ttu-id="39a21-159">Quando si sposta il puntatore del mouse, tenere presente che</span><span class="sxs-lookup"><span data-stu-id="39a21-159">As you move the mouse pointer, note that</span></span>

- <span data-ttu-id="39a21-160">Il contenuto in **origine** visualizzazione cambia per mostrare il markup corrispondente all'elemento selezionato nella pagina.</span><span class="sxs-lookup"><span data-stu-id="39a21-160">The content in **Source** view changes to show the markup corresponding to the selected element on the page.</span></span> <span data-ttu-id="39a21-161">Il markup pertinente viene evidenziato.</span><span class="sxs-lookup"><span data-stu-id="39a21-161">The relevant markup is highlighted.</span></span> <span data-ttu-id="39a21-162">Se l'origine è in un altro file, tale file viene aperto nella visualizzazione origine con il markup pertinente evidenziato.</span><span class="sxs-lookup"><span data-stu-id="39a21-162">If the source is in another file, that file is opened in Source view with the relevant markup highlighted.</span></span>

- <span data-ttu-id="39a21-163">Il markup visualizzato nei **HTML** consente anche di passare in controllo pagina in modo da corrispondere all'elemento selezionato nella pagina.</span><span class="sxs-lookup"><span data-stu-id="39a21-163">The markup displayed in the **HTML** tab in Page Inspector also changes to correspond to the selected element on the page.</span></span> <span data-ttu-id="39a21-164">Nel **HTML** scheda, viene illustrato il markup pertinente.</span><span class="sxs-lookup"><span data-stu-id="39a21-164">In the **HTML** tab, the relevant markup is outlined.</span></span>

- <span data-ttu-id="39a21-165">Il **stili** scheda Mostra le regole CSS pertinenti per la selezione corrente.</span><span class="sxs-lookup"><span data-stu-id="39a21-165">The **Styles** tab shows the CSS rules relevant to the current selection.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="39a21-166">Utilizzare controllo pagina per apportare modifiche al Markup</span><span class="sxs-lookup"><span data-stu-id="39a21-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="39a21-167">A questo punto verrà visualizzato come è possibile utilizzare controllo pagina per trovare e apportare modifiche al markup o la cui posizione potrebbe non essere immediatamente ovvia come testo.</span><span class="sxs-lookup"><span data-stu-id="39a21-167">Now you will see how you can use Page Inspector to find and make changes to markup or text whose location might not be immediately obvious.</span></span>

<span data-ttu-id="39a21-168">PUT Page Inspector in modalità controllo e quindi scorrere fino alla fine della home page.</span><span class="sxs-lookup"><span data-stu-id="39a21-168">Put Page Inspector in Inspection Mode and then scroll to the bottom of the home page.</span></span>

<span data-ttu-id="39a21-169">Non appena si immette l'area di piè di pagina, controllo pagina apre il *Site. master* file di layout nella **origine** vista in una scheda temporanea a destra delle altre schede ed evidenzia la sezione del master pagina che si Hai selezionato.</span><span class="sxs-lookup"><span data-stu-id="39a21-169">As soon as you enter the footer area, Page Inspector opens the *Site.Master* layout file in **Source** view in a temporary tab to the right of the other tabs and highlights the section of the master page that you have selected.</span></span> <span data-ttu-id="39a21-170">Vengono visualizzati come controllo pagina è possibile trovare e visualizzare il contenuto in una pagina che in realtà potrebbe provenire da un file diverso da quello che è stata aperta originariamente.</span><span class="sxs-lookup"><span data-stu-id="39a21-170">This shows you how Page Inspector can find and display content on a page that might actually come from a different file than the one you originally opened.</span></span>

![Piè di pagina vengono evidenziate in modalità controllo](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

<span data-ttu-id="39a21-172">Nella finestra del browser di controllo pagina, spostare il puntatore del mouse sulla riga con il copyright <a id="a"> </a>notare.</span><span class="sxs-lookup"><span data-stu-id="39a21-172">In the Page Inspector browser window, move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span>

<span data-ttu-id="39a21-173">Nel *Site. master* pagina, la riga corrispondente viene evidenziato.</span><span class="sxs-lookup"><span data-stu-id="39a21-173">In the *Site.Master* page, the corresponding line is highlighted.</span></span>

![Riga di piè di pagina informazioni sul copyright evidenziata](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

<span data-ttu-id="39a21-175">Aggiungere testo alla fine della riga nel *Site. master* file.</span><span class="sxs-lookup"><span data-stu-id="39a21-175">Add some text to the end of the line in the *Site.Master* file.</span></span>

<span data-ttu-id="39a21-176">&lt;p&gt;&amp;duplicarlo; &lt;%: % DateTime.Now.Year&gt; -Rocks l'applicazione ASP.NET!&lt; / p&gt;</span><span class="sxs-lookup"><span data-stu-id="39a21-176">&lt;p&gt;&amp;copy; &lt;%: DateTime.Now.Year %&gt; - My ASP.NET Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="39a21-177">A questo punto, premere Ctrl + Alt + INVIO o fare clic sulla barra di aggiornamento per visualizzare i risultati nella finestra del browser di controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="39a21-177">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![Funziona l'applicazione ASP.NET!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

<span data-ttu-id="39a21-179">Si potrebbe avrai pensato che era il piè di pagina sul *default. aspx* pagina, ma si è dimostrata nella pagina di layout master e controllo pagina è stato trovato per te.</span><span class="sxs-lookup"><span data-stu-id="39a21-179">You might have thought that the footer was on the *Default.aspx* page, but it turned out to be in the master layout page, and Page Inspector found it for you.</span></span>

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="39a21-180">Modalità controllo e la finestra HTML</span><span class="sxs-lookup"><span data-stu-id="39a21-180">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="39a21-181">Successivamente, si avrà un'occhiata alla finestra HTML e il relativo mapping elementi per l'utente.</span><span class="sxs-lookup"><span data-stu-id="39a21-181">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="39a21-182">Inserire controllo pagina in modalità controllo.</span><span class="sxs-lookup"><span data-stu-id="39a21-182">Put Page Inspector in Inspection Mode.</span></span>

![Esaminare l'elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

<span data-ttu-id="39a21-184">Fare clic sulla parte superiore della pagina con la dicitura "inserire qui il logo".</span><span class="sxs-lookup"><span data-stu-id="39a21-184">Click the top part of the page that says "your logo here".</span></span> <span data-ttu-id="39a21-185">Se si sta esaminando un particolare elemento in modo più dettagliato, in modo che la visualizzazione nella finestra del browser non è più viene modificato quando si sposta il puntatore del mouse.</span><span class="sxs-lookup"><span data-stu-id="39a21-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="39a21-186">Ora passare il puntatore del mouse per il **HTML** finestra.</span><span class="sxs-lookup"><span data-stu-id="39a21-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="39a21-187">Quando si sposta il puntatore del mouse, controllo pagina vengono illustrati l'elemento all'interno di **HTML** finestra ed evidenzia l'elemento corrispondente nella finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="39a21-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Finestra HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

<span data-ttu-id="39a21-189">Come in precedenza, controllo pagina apre il *Site. master* file per l'utente in una scheda temporanea. Fare clic sulla scheda Site. master, e il markup corrispondente viene evidenziato nel &lt;intestazione&gt; sezione:</span><span class="sxs-lookup"><span data-stu-id="39a21-189">As before, Page Inspector opens the *Site.Master* file for you in a temporary tab. Click the Site.Master tab, and the corresponding markup is highlighted in the &lt;header&gt; section:</span></span>

![Markup evidenziato](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="39a21-191">Anteprima modifiche CSS nella finestra stili</span><span class="sxs-lookup"><span data-stu-id="39a21-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="39a21-192">Successivamente, si vedrà come usare il controllo pagina **stili** finestra per visualizzare in anteprima le modifiche a CSS.</span><span class="sxs-lookup"><span data-stu-id="39a21-192">Next, you will see how you can use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="39a21-193">Scegliere il **Inspect** pulsante per attivare controllo pagina in modalità controllo.</span><span class="sxs-lookup"><span data-stu-id="39a21-193">Click the **Inspect** button to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="39a21-194">Nella finestra del browser di controllo pagina, spostare il puntatore del mouse sopra la sezione "Home Page" finché il **div.content wrapper** etichetta viene visualizzata.</span><span class="sxs-lookup"><span data-stu-id="39a21-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Passare il mouse sugli elementi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

<span data-ttu-id="39a21-196">Fare clic su una sola volta all'interno della sezione div.content wrapper, e quindi spostare il puntatore del mouse per il **stili** finestra.</span><span class="sxs-lookup"><span data-stu-id="39a21-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="39a21-197">Nel selettore della classe wrapper. Content .featured, deselezionare e selezionare la casella di controllo per la proprietà di colore di sfondo.</span><span class="sxs-lookup"><span data-stu-id="39a21-197">Under the .featured .content-wrapper class selector, clear and select the checkbox for the background-color property.</span></span>

![Colore di sfondo chiaro](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

<span data-ttu-id="39a21-199">Si noti come la modifica include l'anteprima immediatamente nella finestra del browser di controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="39a21-199">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="39a21-200">Selezionare la casella di controllo di nuovo, quindi fare doppio clic sul valore della proprietà e modificarlo in base ai `red`.</span><span class="sxs-lookup"><span data-stu-id="39a21-200">Select the checkbox again, then double-click the property value and change it to `red`.</span></span> <span data-ttu-id="39a21-201">La modifica viene immediatamente:</span><span class="sxs-lookup"><span data-stu-id="39a21-201">The change shows immediately:</span></span>

![Colore di sfondo rosso](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

<span data-ttu-id="39a21-203">Il **stili** rende finestra è più facile testare e visualizzare in anteprima CSS viene modificato prima di procedere effettivamente le modifiche apportate allo stile della finestra stessa.</span><span class="sxs-lookup"><span data-stu-id="39a21-203">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="39a21-204">Sincronizzazione automatica CSS</span><span class="sxs-lookup"><span data-stu-id="39a21-204">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="39a21-205">Questa funzionalità richiede la versione 1.3 di controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="39a21-205">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="39a21-206">La funzionalità di sincronizzazione automatica CSS consente di modificare direttamente un file CSS e visualizzare le modifiche immediatamente nel browser di controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="39a21-206">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="39a21-207">Fare clic su **Inspect** inserire controllo pagina in modalità controllo.</span><span class="sxs-lookup"><span data-stu-id="39a21-207">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="39a21-208">Nel browser di controllo pagina, spostare il puntatore del mouse sopra la sezione "Home Page" finché il **div.content wrapper** etichetta viene visualizzata.</span><span class="sxs-lookup"><span data-stu-id="39a21-208">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="39a21-209">Fare clic su una sola volta per selezionare questo elemento.</span><span class="sxs-lookup"><span data-stu-id="39a21-209">Click once to select this element.</span></span>

<span data-ttu-id="39a21-210">Il **stili** finestra Mostra tutte le regole CSS per questo elemento.</span><span class="sxs-lookup"><span data-stu-id="39a21-210">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="39a21-211">Scorrere verso il basso il selettore di classe find .featured. Content-wrapper.</span><span class="sxs-lookup"><span data-stu-id="39a21-211">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="39a21-212">Fare clic su .featured. Content-"wrapper".</span><span class="sxs-lookup"><span data-stu-id="39a21-212">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="39a21-213">Controllo pagina apre il file CSS che definisce questo stile (CSS) ed evidenzia il CSS corrispondente.</span><span class="sxs-lookup"><span data-stu-id="39a21-213">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![File CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

<span data-ttu-id="39a21-215">A questo punto modificare il valore per `background-color` su "red".</span><span class="sxs-lookup"><span data-stu-id="39a21-215">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="39a21-216">La modifica viene visualizzata immediatamente nel browser di controllo pagina.</span><span class="sxs-lookup"><span data-stu-id="39a21-216">The change appears immediately in the Page Inspector browser.</span></span>

![Browser di controllo pagina](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a><span data-ttu-id="39a21-218">Tramite la selezione colori CSS</span><span class="sxs-lookup"><span data-stu-id="39a21-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="39a21-219">Successivamente, imparerai a utilizzare controllo pagina per trovare rapidamente e modificare il CSS relativo testo evidenziato nell'applicazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="39a21-219">Next, you'll learn how to use Page Inspector to quickly find and change the CSS for highlighted text in the default application.</span></span> <span data-ttu-id="39a21-220">In questo esempio, si è deciso di non desidera che l'evidenziazione blu e per modificarlo in un altro colore.</span><span class="sxs-lookup"><span data-stu-id="39a21-220">In this example, you've decided that you don't like the blue highlighting and want to change it to another color.</span></span>

<span data-ttu-id="39a21-221">Scegliere il **Inspect** pulsante.</span><span class="sxs-lookup"><span data-stu-id="39a21-221">Click the **Inspect** button.</span></span>

![Esaminare l'elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

<span data-ttu-id="39a21-223">Nella finestra del browser di controllo pagina, spostare il puntatore del mouse sulla evidenziata "video, esercitazioni ed esempi di" testo in modo che il file CSS "mark" etichetta viene visualizzata.</span><span class="sxs-lookup"><span data-stu-id="39a21-223">In the Page Inspector browser window, move the mouse pointer over the highlighted "videos, tutorials, and samples" text so that the CSS "mark" label appears.</span></span>

![Passare il mouse sull'elemento mark](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

<span data-ttu-id="39a21-225">Fare clic sul testo per selezionarlo.</span><span class="sxs-lookup"><span data-stu-id="39a21-225">Click the text to select it.</span></span> <span data-ttu-id="39a21-226">Il selettore di mark CSS corrispondente viene visualizzato in fondo il **stili** finestra.</span><span class="sxs-lookup"><span data-stu-id="39a21-226">The corresponding CSS mark selector appears at the bottom of the **Styles** window.</span></span>

![selettore di Mark nella finestra stili](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

<span data-ttu-id="39a21-228">Fare clic sul selettore di mark.</span><span class="sxs-lookup"><span data-stu-id="39a21-228">Click the mark selector.</span></span> <span data-ttu-id="39a21-229">Verrà visualizzata la *CSS* file per l'applicazione web.</span><span class="sxs-lookup"><span data-stu-id="39a21-229">This opens the *Site.css* file for the web application.</span></span> <span data-ttu-id="39a21-230">Fare clic sulla scheda Site. CSS e viene evidenziato il CSS corrispondente per il selettore:</span><span class="sxs-lookup"><span data-stu-id="39a21-230">Click the Site.css tab, and the corresponding CSS for the selector is highlighted:</span></span>

![selettore di Mark nel foglio di stile](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

<span data-ttu-id="39a21-232">Selezionare e rimuovere la riga con la proprietà di colore di sfondo.</span><span class="sxs-lookup"><span data-stu-id="39a21-232">Select and remove the line with the background-color property.</span></span>

<span data-ttu-id="39a21-233">Si userà ora la nuova selezione di colori di Visual Studio 2012 CSS per scegliere un nuovo colore per il **contrassegnare** proprietà background-color.</span><span class="sxs-lookup"><span data-stu-id="39a21-233">You will now use the new Visual Studio 2012 CSS color picker to choose a new color for the **mark** background-color property.</span></span>

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a><span data-ttu-id="39a21-234">Tramite la selezione colori CSS di Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="39a21-234">Using the Visual Studio 2012 CSS Color Picker</span></span>

<span data-ttu-id="39a21-235">Editor CSS in Visual Studio 2012 è una selezione colori che consente di scegliere e inserire i colori.</span><span class="sxs-lookup"><span data-stu-id="39a21-235">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="39a21-236">Include una semplice barra dei colori e una selezione di "pop-down" che offre un controllo più preciso.</span><span class="sxs-lookup"><span data-stu-id="39a21-236">It has a simple color bar and a "pop-down" picker that offers finer control.</span></span>

<span data-ttu-id="39a21-237">La selezione colori include una tavolozza di colori standard supporta i nomi dei colori standard, codici hash, i colori RGB, RGBA, HSL e HSLA e gestisce un elenco dei colori utilizzati più di recente nel documento.</span><span class="sxs-lookup"><span data-stu-id="39a21-237">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="39a21-238">Nella riga in cui è stata la proprietà di colore di sfondo, digitare "bc" e premere la freccia verso il basso una sola volta.</span><span class="sxs-lookup"><span data-stu-id="39a21-238">On the line where the background-color property was, type "bc" and press the down arrow once.</span></span>

<span data-ttu-id="39a21-239">Quando si digita il primo carattere di ogni parola in una proprietà separati da trattini, ad esempio "background-color", IntelliSense Filtra l'elenco per visualizzare solo le proprietà che corrispondono a:</span><span class="sxs-lookup"><span data-stu-id="39a21-239">When you type the first character of each word in a hyphen-separated property like "background-color", IntelliSense filters the list for you to show only the properties that match:</span></span>

![Valori di IntelliSense filtrati](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

<span data-ttu-id="39a21-241">A questo punto digitare due punti.</span><span class="sxs-lookup"><span data-stu-id="39a21-241">Now type a colon.</span></span> <span data-ttu-id="39a21-242">Quando esegue questa operazione, viene inserito il nome della proprietà completa del colore di sfondo.</span><span class="sxs-lookup"><span data-stu-id="39a21-242">When you do, the full background-color property name is inserted.</span></span> <span data-ttu-id="39a21-243">Tipo di **#** oppure **rgb (**, e viene visualizzata la barra di selezione colore:</span><span class="sxs-lookup"><span data-stu-id="39a21-243">Type **#** or **rgb(**, and the color picker bar appears:</span></span>

![La barra di selezione colori CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

<span data-ttu-id="39a21-245">Per vedere come funziona la barra di selezione colore, fare clic sul relativo colori con il puntatore del mouse o premere il tasto freccia giù e quindi utilizzare i tasti freccia sinistra e destra per attraversare i colori.</span><span class="sxs-lookup"><span data-stu-id="39a21-245">To see how the color picker bar works, click its colors with the mouse pointer, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="39a21-246">Quando si visita un colore, viene visualizzato in anteprima il valore corrispondente per la proprietà background-color:</span><span class="sxs-lookup"><span data-stu-id="39a21-246">When you visit a color, the corresponding value for the background-color property is previewed:</span></span>

![valore della proprietà di colore di sfondo visualizzata in anteprima](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

<span data-ttu-id="39a21-248">A questo punto, è possibile premere INVIO per selezionare il valore e quindi un punto e virgola (;) per completare la voce di CSS.</span><span class="sxs-lookup"><span data-stu-id="39a21-248">At this point, you could press Enter to select the value and then a semicolon (;) to complete the CSS entry.</span></span> <span data-ttu-id="39a21-249">Per il momento, andare nella sezione successiva in modo che è possibile vedere come funziona il pop-down di selezione colore.</span><span class="sxs-lookup"><span data-stu-id="39a21-249">For now, go on to the next section so that you can see how the color picker pop-down works.</span></span>

#### <a name="using-the-color-picker-pop-down"></a><span data-ttu-id="39a21-250">Tramite la selezione di colore Pop-Down</span><span class="sxs-lookup"><span data-stu-id="39a21-250">Using the Color Picker Pop-Down</span></span>

<span data-ttu-id="39a21-251">Quando la barra dei colori non dispone del colore esatto che si sta cercando, è possibile usare la selezione colori popup a discesa.</span><span class="sxs-lookup"><span data-stu-id="39a21-251">When the color bar doesn't have the exact color that you're looking for, you can use the color picker pop-down.</span></span>

<span data-ttu-id="39a21-252">Per aprirlo, fare clic sulle virgolette doppie all'estremità destra della barra dei colori o premere una o due volte il tasto freccia giù sulla tastiera.</span><span class="sxs-lookup"><span data-stu-id="39a21-252">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Selezione di colore CSS Pop-Down](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

<span data-ttu-id="39a21-254">Fare clic su un altro colore nella barra verticale sul lato destro.</span><span class="sxs-lookup"><span data-stu-id="39a21-254">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="39a21-255">Mostra una sfumatura di colore nella finestra principale.</span><span class="sxs-lookup"><span data-stu-id="39a21-255">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="39a21-256">Scegliere un colore direttamente dalla barra degli strumenti verticale premendo INVIO oppure fare clic su qualsiasi punto nella finestra principale di scegliere con precisione maggiore.</span><span class="sxs-lookup"><span data-stu-id="39a21-256">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="39a21-257">Se è presente un colore sullo schermo del computer che si desidera utilizzare (non deve necessariamente essere all'interno dell'interfaccia utente di Visual Studio), è possibile acquisire il relativo valore con lo strumento contagocce in basso a destra.</span><span class="sxs-lookup"><span data-stu-id="39a21-257">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="39a21-258">È anche possibile modificare l'opacità di colore spostando il dispositivo di scorrimento nella parte inferiore della selezione colori.</span><span class="sxs-lookup"><span data-stu-id="39a21-258">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="39a21-259">In questo modo le modifiche dei colori dei valori di valori RGBA perché il formato RGBA può rappresentare l'opacità.</span><span class="sxs-lookup"><span data-stu-id="39a21-259">Doing so changes color values to RGBA values because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="39a21-260">Dopo aver scelto un colore, premere INVIO e quindi digitare un punto e virgola per completare la voce di colore di sfondo nel *CSS* file.</span><span class="sxs-lookup"><span data-stu-id="39a21-260">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="39a21-261">La barra di aggiornamento di controllo pagina</span><span class="sxs-lookup"><span data-stu-id="39a21-261">The Page Inspector Update Bar</span></span>

<span data-ttu-id="39a21-262">Controllo pagina immediatamente rileva la modifica per il *CSS* file (o a qualsiasi file nell'applicazione) e viene visualizzato un avviso in una barra di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="39a21-262">Page Inspector immediately detects the change to the *Site.css* file (or to any file in the application) and displays an alert in an update bar.</span></span>

![Barra di aggiornamento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

<span data-ttu-id="39a21-264">Per salvare tutti i file e aggiornare il browser di controllo pagina, premere Ctrl + Alt + INVIO o fare clic sulla barra di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="39a21-264">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="39a21-265">La modifica il colore di evidenziazione viene visualizzata nel browser:</span><span class="sxs-lookup"><span data-stu-id="39a21-265">The change in the highlight color appears in the browser:</span></span>

![Colore dei lati illuminati modificato](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a><span data-ttu-id="39a21-267">Si noti che facilmente aggiornato browser di controllo pagina direttamente dall'interno dell'ambiente Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="39a21-267">Notice that you conveniently refreshed the Page Inspector browser right from within the Visual Studio environment.</span></span> <span data-ttu-id="39a21-268">Utilizzo di controllo pagina invece di un browser esterno consente di rimanere nell'editor, quando si sviluppano le applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="39a21-268">Using Page Inspector instead of an external browser lets you stay in the editor when you develop your web applications.</span></span>

## <a name="see-also"></a><span data-ttu-id="39a21-269">Vedere anche</span><span class="sxs-lookup"><span data-stu-id="39a21-269">See Also</span></span>

<span data-ttu-id="39a21-270">[Introduzione a controllo pagina](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (video di Channel 9)</span><span class="sxs-lookup"><span data-stu-id="39a21-270">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)</span></span>

<span data-ttu-id="39a21-271">[I messaggi di errore di controllo pagina](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="39a21-271">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
