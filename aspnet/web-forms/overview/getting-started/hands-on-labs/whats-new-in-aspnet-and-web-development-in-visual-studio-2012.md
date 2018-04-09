---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: Novità di ASP.NET e lo sviluppo Web in Visual Studio 2012 | Documenti Microsoft
author: rick-anderson
description: La nuova versione di Visual Studio introduce una serie di miglioramenti finalizzate soprattutto a migliorare l'esperienza e le prestazioni quando si lavora con tecnologie Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 00b43cc548df44edded925521991a095ed856494
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a><span data-ttu-id="fd584-103">Novità di ASP.NET e lo sviluppo Web in Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="fd584-103">What's New in ASP.NET and Web Development in Visual Studio 2012</span></span>
====================
<span data-ttu-id="fd584-104">Da [categorie Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="fd584-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="fd584-105">La nuova versione di Visual Studio introduce una serie di miglioramenti finalizzate soprattutto a migliorare l'esperienza e le prestazioni quando si lavora con tecnologie Web.</span><span class="sxs-lookup"><span data-stu-id="fd584-105">The new version of Visual Studio introduces a number of enhancements focused on improving the experience and performance when working with Web technologies.</span></span> <span data-ttu-id="fd584-106">Editor di Visual Studio per CSS, JavaScript e HTML sono completamente rinnovato per includere numerose facilita il codice più richiesta, ad esempio IntelliSense e il rientro automatico.</span><span class="sxs-lookup"><span data-stu-id="fd584-106">Visual Studio Editors for CSS, JavaScript and HTML have been completely revamped to include many of the most in-demand code aids, such as IntelliSense and automatic indentation.</span></span> <span data-ttu-id="fd584-107">Relativamente alle prestazioni, aggregazione e minimizzazione ora sono integrati, come tempo di caricamento di funzionalità incorporate per ridurre facilmente la pagina.</span><span class="sxs-lookup"><span data-stu-id="fd584-107">Regarding performance, bundling and minification are now integrated as built-in features to easily reduce page load time.</span></span>
> 
> <span data-ttu-id="fd584-108">Visual Studio consente di lavorare con le tecnologie del sito Web più recenti.</span><span class="sxs-lookup"><span data-stu-id="fd584-108">Visual Studio enables you to work with the latest website technologies.</span></span> <span data-ttu-id="fd584-109">È possibile utilizzare frammenti di codice CSS3 tra browser per verificare che il sito funziona indipendentemente dalla piattaforma client sfruttando anche i nuovi elementi HTML5 e le funzionalità.</span><span class="sxs-lookup"><span data-stu-id="fd584-109">You can use cross-browser CSS3 Snippets to make sure your site works regardless of the client platform while also taking advantage of the new HTML5 elements and features.</span></span>
> 
> <span data-ttu-id="fd584-110">Scrittura e la profilatura del codice JavaScript deve essere più semplice con questa versione di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fd584-110">Writing and profiling JavaScript code should be easier with this Visual Studio version.</span></span> <span data-ttu-id="fd584-111">Gli elenchi di IntelliSense, funzionalità di documentazione e navigazione XML integrate sono ora disponibili per il codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fd584-111">IntelliSense lists, integrated XML documentation and navigation features are now available for JavaScript code.</span></span> <span data-ttu-id="fd584-112">È ora disponibile il catalogo JavaScript a portata di mano.</span><span class="sxs-lookup"><span data-stu-id="fd584-112">You now have the JavaScript catalog at your fingertips.</span></span> <span data-ttu-id="fd584-113">Inoltre, è possibile controllare la conformità di ECMAScript5 con gli script e rilevare gli errori di sintassi in fase iniziale.</span><span class="sxs-lookup"><span data-stu-id="fd584-113">Additionally, you can check ECMAScript5 compliance with your scripts and detect syntax errors at an early stage.</span></span>
> 
> <span data-ttu-id="fd584-114">Ultimo, ma non meno importante, questa versione di Visual Studio implementa minimizzazione e aggregazione predefiniti.</span><span class="sxs-lookup"><span data-stu-id="fd584-114">Last but not least, this Visual Studio version implements built-in bundling and minification.</span></span> <span data-ttu-id="fd584-115">Il file di script e fogli di stile verranno compressa e compressi in modo che il sito è più veloce.</span><span class="sxs-lookup"><span data-stu-id="fd584-115">Your script files and style sheets will be packed and compressed so that the site performs faster.</span></span>
> 
> <span data-ttu-id="fd584-116">Questa esercitazione illustra i miglioramenti e nuove funzionalità descritte in precedenza tramite l'applicazione di modifiche di lieve entità a un'applicazione Web di esempio fornita nella cartella di origine.</span><span class="sxs-lookup"><span data-stu-id="fd584-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="fd584-117">Tutto il codice di esempio e i frammenti di codice sono inclusi nel Web categorie Training Kit, disponibile all'indirizzo [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="fd584-117">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="fd584-118">Obiettivi</span><span class="sxs-lookup"><span data-stu-id="fd584-118">Objectives</span></span>

<span data-ttu-id="fd584-119">In questo laboratorio pratico, si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="fd584-119">In this hands on lab, you will learn how to:</span></span>

- <span data-ttu-id="fd584-120">Utilizzare le nuove funzionalità e miglioramenti nell'editor CSS</span><span class="sxs-lookup"><span data-stu-id="fd584-120">Use the new features and improvements in the CSS editor</span></span>
- <span data-ttu-id="fd584-121">Utilizzare le nuove funzionalità e miglioramenti nell'editor HTML</span><span class="sxs-lookup"><span data-stu-id="fd584-121">Use the new features and improvements in the HTML editor</span></span>
- <span data-ttu-id="fd584-122">Usare le nuove funzionalità e miglioramenti nell'editor JavaScript</span><span class="sxs-lookup"><span data-stu-id="fd584-122">Use the new features and improvements in the JavaScript editor</span></span>
- <span data-ttu-id="fd584-123">Configurare l'aggregazione e riduzione</span><span class="sxs-lookup"><span data-stu-id="fd584-123">Configure and use bundling and minification</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="fd584-124">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="fd584-124">Prerequisites</span></span>

- <span data-ttu-id="fd584-125">[Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (lettura [appendice](#AppendixA) per istruzioni su come installarlo).</span><span class="sxs-lookup"><span data-stu-id="fd584-125">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>
- <span data-ttu-id="fd584-126">[Windows PowerShell](https://support.microsoft.com/kb/968930/) (per gli script di installazione - già installati in Windows 8 e Windows Server 2008 R2)</span><span class="sxs-lookup"><span data-stu-id="fd584-126">[Windows PowerShell](https://support.microsoft.com/kb/968930/) (for setup scripts - already installed on Windows 8 and Windows Server 2008 R2)</span></span>
- <span data-ttu-id="fd584-127">[Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - o un browser compatibile con HTML5</span><span class="sxs-lookup"><span data-stu-id="fd584-127">[Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - or an HTML5 compliant browser</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="fd584-128">Esercizi</span><span class="sxs-lookup"><span data-stu-id="fd584-128">Exercises</span></span>

<span data-ttu-id="fd584-129">Questo laboratorio pratico include gli esercizi seguenti:</span><span class="sxs-lookup"><span data-stu-id="fd584-129">This hands on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="fd584-130">Esercizio 1: Novità nell'Editor CSS</span><span class="sxs-lookup"><span data-stu-id="fd584-130">Exercise 1: What's New in the CSS Editor</span></span>](#Exercise1)
2. [<span data-ttu-id="fd584-131">Esercizio 2: Novità nell'Editor HTML</span><span class="sxs-lookup"><span data-stu-id="fd584-131">Exercise 2: What's New in the HTML Editor</span></span>](#Exercise2)
3. [<span data-ttu-id="fd584-132">Esercizio 3: Novità nell'Editor JavaScript</span><span class="sxs-lookup"><span data-stu-id="fd584-132">Exercise 3: What's New in the JavaScript Editor</span></span>](#Exercise3)
4. [<span data-ttu-id="fd584-133">Esercizio 4: Bundling and Minification</span><span class="sxs-lookup"><span data-stu-id="fd584-133">Exercise 4: Bundling and Minification</span></span>](#Exercise4)

<span data-ttu-id="fd584-134">Tempo stimato per completare questa esercitazione: **60 minuti**.</span><span class="sxs-lookup"><span data-stu-id="fd584-134">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a><span data-ttu-id="fd584-135">Esercizio 1: Novità nell'Editor CSS</span><span class="sxs-lookup"><span data-stu-id="fd584-135">Exercise 1: What's New in the CSS Editor</span></span>

<span data-ttu-id="fd584-136">Gli sviluppatori Web devono avere familiari con molte delle difficoltà relative alla modifica di CSS.</span><span class="sxs-lookup"><span data-stu-id="fd584-136">Web developers should be familiar with many of the difficulties related to CSS editing.</span></span> <span data-ttu-id="fd584-137">Uno dei principali problemi di applicazione di stili CSS è la compatibilità browser.</span><span class="sxs-lookup"><span data-stu-id="fd584-137">One of the biggest issues of CSS styling is cross-browser compatibility.</span></span> <span data-ttu-id="fd584-138">Accade spesso che, dopo l'applicazione di stili al sito, noterai che l'aspetto diverso se si apre in un altro browser o nel dispositivo.</span><span class="sxs-lookup"><span data-stu-id="fd584-138">It often happens that, after applying styles to your site, you notice that it looks different if you open it in another browser or device.</span></span> <span data-ttu-id="fd584-139">Pertanto, si potrebbe impiegano molto tempo in risolvere tali problemi visual per tenere presente che, quando si apportano infine funziona in un browser, viene suddiviso in altre.</span><span class="sxs-lookup"><span data-stu-id="fd584-139">Therefore, you may spend a considerable time on fixing those visual issues to realize that, when you finally make it work in one browser, it is broken in the others.</span></span>

<span data-ttu-id="fd584-140">Visual Studio ora include funzionalità che consentono agli sviluppatori di accedere, gestire e organizzare in modo efficace i fogli di stile CSS.</span><span class="sxs-lookup"><span data-stu-id="fd584-140">Visual Studio now includes features that help developers access, work and organize CSS style sheets effectively.</span></span> <span data-ttu-id="fd584-141">In questa esercitazione, si soddisferà le nuove funzionalità per un'organizzazione efficace e l'edizione, nonché i frammenti di codice CSS3 per la compatibilità browser.</span><span class="sxs-lookup"><span data-stu-id="fd584-141">Throughout this exercise, you will meet the new features for an effective organization and edition, as well as the CSS3 Code Snippets for cross-browser compatibility.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a><span data-ttu-id="fd584-142">Attività 1 - nuove funzionalità dell'Editor</span><span class="sxs-lookup"><span data-stu-id="fd584-142">Task 1 - New Editor Features</span></span>

<span data-ttu-id="fd584-143">In questa attività, sarà possibile osservare le nuove funzionalità dell'Editor CSS.</span><span class="sxs-lookup"><span data-stu-id="fd584-143">In this task, you will discover the new features of the CSS Editor.</span></span> <span data-ttu-id="fd584-144">Questo nuovo editor consente di aumentare la produttività, sfruttando il rientro smart nuovo, i commenti del codice migliore e l'elenco di IntelliSense migliorato.</span><span class="sxs-lookup"><span data-stu-id="fd584-144">This new editor will help you increase your productivity by taking advantage of the new smart indentation, the improved code comments and the enhanced IntelliSense list.</span></span>

1. <span data-ttu-id="fd584-145">Avviare **Visual Studio** e aprire il **WhatsNewASPNET.sln** soluzione si trova nel **Source\WhatsNewASPNET** cartella di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="fd584-145">Start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="fd584-146">In Esplora soluzioni, aprire il **Site.css** file si trova sotto il **stili** cartella.</span><span class="sxs-lookup"><span data-stu-id="fd584-146">In Solution Explorer, open the **Site.css** file located under the **Styles** folder.</span></span> <span data-ttu-id="fd584-147">Verificare che il **Editor di testo** strumenti sono visibili nella barra degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="fd584-147">Make sure the **Text Editor** tools are visible on the toolbar.</span></span> <span data-ttu-id="fd584-148">A tale scopo, selezionare il **vista** | **barre degli strumenti** opzione di menu, quindi individuare il **Editor di testo** opzioni.</span><span class="sxs-lookup"><span data-stu-id="fd584-148">To do that, select the **View** | **Toolbars** menu option, and check the **Text Editor** options.</span></span> <span data-ttu-id="fd584-149">Si noterà che, poiché questa nuova versione, il **commento** pulsante (![pulsante Commento](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) e **Rimuovi commento** pulsante (![rimuovere il commento-pulsante](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) sono abilitate anche per l'editor CSS.</span><span class="sxs-lookup"><span data-stu-id="fd584-149">You will notice that, since this new version, the **Comment** button (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) and the **Uncomment** button (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png) ) are also enabled for the CSS editor.</span></span>

    <span data-ttu-id="fd584-150">![Abilitazione di Editor e strumenti CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "abilitazione Editor e strumenti CSS")</span><span class="sxs-lookup"><span data-stu-id="fd584-150">![Enabling Editor and CSS Tools](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Enabling Editor and CSS Tools")</span></span>

    <span data-ttu-id="fd584-151">*Abilitazione di Editor e strumenti CSS*</span><span class="sxs-lookup"><span data-stu-id="fd584-151">*Enabling Editor and CSS Tools*</span></span>
3. <span data-ttu-id="fd584-152">Scorrere il codice e selezionare qualsiasi definizione di classe CSS.</span><span class="sxs-lookup"><span data-stu-id="fd584-152">Scroll the code and select any CSS class definition.</span></span> <span data-ttu-id="fd584-153">Fare clic su di **commento** (![pulsante Commento](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) pulsante per commentare le righe selezionate.</span><span class="sxs-lookup"><span data-stu-id="fd584-153">Click the **Comment** (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) button to comment the selected lines.</span></span> <span data-ttu-id="fd584-154">Quindi, fare clic su di **Rimuovi commento** (![pulsante rimuovere il commento](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) pulsante per annullare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="fd584-154">Then, click the **Uncomment** (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) button to undo the changes.</span></span>
4. <span data-ttu-id="fd584-155">Fare clic su di **comprimere** (![comprimere](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) e **Espandi** (![espandere](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) pulsanti si trovano sul margine sinistro del testo.</span><span class="sxs-lookup"><span data-stu-id="fd584-155">Click the **Collapse** (![collapse](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) and **Expand** (![expand](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) buttons located on the left margin of the text.</span></span> <span data-ttu-id="fd584-156">Si noti che è ora possibile nascondere gli stili che non consentono di disporre di una visualizzazione più chiara.</span><span class="sxs-lookup"><span data-stu-id="fd584-156">Notice that you can now hide the styles you don't use to have a cleaner view.</span></span>

    <span data-ttu-id="fd584-157">![Compressione di classi CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "classi CSS compressione")</span><span class="sxs-lookup"><span data-stu-id="fd584-157">![Collapsing CSS classes](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "Collapsing CSS classes")</span></span>

    <span data-ttu-id="fd584-158">*Compressione di classi CSS*</span><span class="sxs-lookup"><span data-stu-id="fd584-158">*Collapsing CSS classes*</span></span>
5. <span data-ttu-id="fd584-159">Assicurarsi che sia abilitata la funzionalità smart rientro.</span><span class="sxs-lookup"><span data-stu-id="fd584-159">Make sure that the smart indentation feature is enabled.</span></span> <span data-ttu-id="fd584-160">Selezionare il **strumenti** | **opzioni** opzione di menu e quindi selezionare il **Editor di testo** | **CSS**  |  **Formattazione** pagina nel riquadro a sinistra della schermata.</span><span class="sxs-lookup"><span data-stu-id="fd584-160">Select the **Tools** | **Options** menu option, and then select the **Text Editor** | **CSS** | **Formatting** page in the left pane of the screen.</span></span> <span data-ttu-id="fd584-161">Controllare il **rientro gerarchico** opzione.</span><span class="sxs-lookup"><span data-stu-id="fd584-161">Check the **Hierarchical indentation** option.</span></span>

    <span data-ttu-id="fd584-162">![L'abilitazione di rientro gerarchico](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "abilitazione rientro gerarchico")</span><span class="sxs-lookup"><span data-stu-id="fd584-162">![Enabling hierarchical indentation](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "Enabling hierarchical indentation")</span></span>

    <span data-ttu-id="fd584-163">*L'abilitazione di rientro gerarchico*</span><span class="sxs-lookup"><span data-stu-id="fd584-163">*Enabling hierarchical indentation*</span></span>
6. <span data-ttu-id="fd584-164">Individuare la definizione di classe principale (.main) e aggiungere uno stile per gli elementi div.</span><span class="sxs-lookup"><span data-stu-id="fd584-164">Locate the main class definition (.main) and append a style to the div elements.</span></span> <span data-ttu-id="fd584-165">Si noterà che il codice consente di allineare automaticamente, rendere gli utenti per trovare l'elemento padre, le classi a colpo d'occhio.</span><span class="sxs-lookup"><span data-stu-id="fd584-165">You will notice that the code aligns automatically, helping users to find the parent classes at a glance.</span></span>

    <span data-ttu-id="fd584-166">CSS</span><span class="sxs-lookup"><span data-stu-id="fd584-166">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    <span data-ttu-id="fd584-167">![Allineamento gerarchico in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "allineamento gerarchico in CSS")</span><span class="sxs-lookup"><span data-stu-id="fd584-167">![Hierarchical alignment in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "Hierarchical alignment in CSS")</span></span>

    <span data-ttu-id="fd584-168">*Allineamento gerarchico in CSS*</span><span class="sxs-lookup"><span data-stu-id="fd584-168">*Hierarchical alignment in CSS*</span></span>
7. <span data-ttu-id="fd584-169">All'interno di **.main div** classe, posizionare il cursore alla fine di **border: 0px;** e premere **invio** per visualizzare l'elenco di IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="fd584-169">Inside **.main div** class, locate the cursor at the end of **border: 0px;** and press **Enter** to display the IntelliSense list.</span></span> <span data-ttu-id="fd584-170">Iniziare a digitare **top** e notare come l'elenco viene filtrato durante la digitazione.</span><span class="sxs-lookup"><span data-stu-id="fd584-170">Start typing **top** and notice how the list is filtered as you type.</span></span> <span data-ttu-id="fd584-171">Nell'elenco vengono visualizzati gli elementi che contengono **top** in qualsiasi parte della parola (nelle versioni precedenti di Visual Studio, l'elenco viene filtrato per gli elementi che *iniziare* con il termine).</span><span class="sxs-lookup"><span data-stu-id="fd584-171">The list will display the elements that contain **top** at any part of the word (In prior versions of Visual Studio, the list is filtered by the items that *begin* with the term).</span></span>

    <span data-ttu-id="fd584-172">![Miglioramenti di IntelliSense in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "miglioramenti di IntelliSense in CSS")</span><span class="sxs-lookup"><span data-stu-id="fd584-172">![IntelliSense enhancements in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "IntelliSense enhancements in CSS")</span></span>

    <span data-ttu-id="fd584-173">*Miglioramenti di IntelliSense in CSS*</span><span class="sxs-lookup"><span data-stu-id="fd584-173">*IntelliSense enhancements in CSS*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a><span data-ttu-id="fd584-174">Attività 2: la selezione colori</span><span class="sxs-lookup"><span data-stu-id="fd584-174">Task 2 - The Color Picker</span></span>

<span data-ttu-id="fd584-175">In questa attività, sarà possibile osservare che la nuova selezione colori CSS integrato in Visual Studio IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="fd584-175">In this task, you will discover the new CSS Color Picker integrated into Visual Studio IntelliSense.</span></span>

1. <span data-ttu-id="fd584-176">In **Site,** individuare la definizione di classe di intestazione (.header) e posizionare il cursore accanto a **colore di sfondo** attributo, tra il &quot;:&quot; e &quot; # &quot; caratteri nella riga di codice **.**</span><span class="sxs-lookup"><span data-stu-id="fd584-176">In **Site.css,** locate the header class definition (.header) and place the cursor next to **background-color** attribute, between the &quot;:&quot; and &quot;#&quot; characters on that line of code **.**</span></span>

    <span data-ttu-id="fd584-177">![Individuazione del cursore](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "individuazione il cursore")</span><span class="sxs-lookup"><span data-stu-id="fd584-177">![Locating the cursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "Locating the cursor")</span></span>

    <span data-ttu-id="fd584-178">*Individuazione del cursore*</span><span class="sxs-lookup"><span data-stu-id="fd584-178">*Locating the cursor*</span></span>
2. <span data-ttu-id="fd584-179">Eliminare il **virgola** (:) e la scrittura di nuovo per visualizzare la selezione colori.</span><span class="sxs-lookup"><span data-stu-id="fd584-179">Delete the **colon** (:) and write it again to display the color picker.</span></span> <span data-ttu-id="fd584-180">Si noti che i colori di primo che verranno visualizzati i colori più frequenti del sito.</span><span class="sxs-lookup"><span data-stu-id="fd584-180">Notice that the first colors you will see are the most frequent colors of your site.</span></span> <span data-ttu-id="fd584-181">Se si sceglie il colore bianco, il codice di colore HTML (#fff) sostituirà il codice colore corrente nel foglio di stile.</span><span class="sxs-lookup"><span data-stu-id="fd584-181">If you click the white color, its HTML color code (#fff) will replace the current color code in the stylesheet.</span></span>

    <span data-ttu-id="fd584-182">![Selezione dei colori](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "selezione dei colori")</span><span class="sxs-lookup"><span data-stu-id="fd584-182">![Color picker](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Color picker")</span></span>

    <span data-ttu-id="fd584-183">*Selezione dei colori*</span><span class="sxs-lookup"><span data-stu-id="fd584-183">*Color picker*</span></span>
3. <span data-ttu-id="fd584-184">Premere il **Espandi** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) pulsante sul selettore di colore per visualizzare la sfumatura di colore e quindi trascinare il cursore sfumato per selezionare un colore diverso.</span><span class="sxs-lookup"><span data-stu-id="fd584-184">Press the **Expand** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) button on the color picker to display the color gradient, and then drag the gradient cursor to select a different color.</span></span> <span data-ttu-id="fd584-185">Successivamente, fare clic su di **contagocce** e selezionare qualsiasi colore dalla schermata.</span><span class="sxs-lookup"><span data-stu-id="fd584-185">After that, click the **Eyedropper** button and select any color from the screen.</span></span> <span data-ttu-id="fd584-186">Si noti che il valore di colore di sfondo cambia dinamicamente mentre si sposta il cursore.</span><span class="sxs-lookup"><span data-stu-id="fd584-186">Notice that background color value changes dynamically while you move the cursor.</span></span>

    <span data-ttu-id="fd584-187">![Sfumatura di colore selezione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "sfumatura di colore selezione")</span><span class="sxs-lookup"><span data-stu-id="fd584-187">![Color picker gradient](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Color picker gradient")</span></span>

    <span data-ttu-id="fd584-188">*Sfumatura di colore selezione*</span><span class="sxs-lookup"><span data-stu-id="fd584-188">*Color picker gradient*</span></span>
4. <span data-ttu-id="fd584-189">Nel **opacità** dispositivo di scorrimento, spostare il selettore al centro della barra per ridurre l'opacità.</span><span class="sxs-lookup"><span data-stu-id="fd584-189">In the **Opacity** slider, move the selector to the center of the bar to reduce the opacity.</span></span> <span data-ttu-id="fd584-190">Si noti che il valore di colore di sfondo viene visualizzata la scala RGBA.</span><span class="sxs-lookup"><span data-stu-id="fd584-190">Notice that background-color value now changes its scale to RGBA.</span></span>

    <span data-ttu-id="fd584-191">![Selezione dei colori opacità](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "selezione colori opacità")</span><span class="sxs-lookup"><span data-stu-id="fd584-191">![Color picker Opacity](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Color picker Opacity")</span></span>

    <span data-ttu-id="fd584-192">*Selezione dei colori opacità*</span><span class="sxs-lookup"><span data-stu-id="fd584-192">*Color picker Opacity*</span></span>

    > [!NOTE]
    > <span data-ttu-id="fd584-193">La definizione di colore RGBA (rosso, verde, blu e alfa) in CSS3 consente di definire il valore di opacità del colore per un singolo elemento.</span><span class="sxs-lookup"><span data-stu-id="fd584-193">The RGBA (Red, Green, Blue, Alpha) color definition in CSS3 enables you to define the color opacity value for a single item.</span></span> <span data-ttu-id="fd584-194">A differenza di **opacità -** un attributo CSS simile **-** colori RGBA sono inoltre compatibili con i browser più recenti.</span><span class="sxs-lookup"><span data-stu-id="fd584-194">Unlike **opacity -** a similar CSS attribute **-** RGBA colors are also compatible with the latest browsers.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a><span data-ttu-id="fd584-195">Attività 3 - frammenti di codice compatibile di CSS</span><span class="sxs-lookup"><span data-stu-id="fd584-195">Task 3 - CSS Compatible Code Snippets</span></span>

<span data-ttu-id="fd584-196">In questa attività si apprenderà come usare i frammenti di codice CSS3 compatibile tra browser per implementare alcune funzionalità del sito Web.</span><span class="sxs-lookup"><span data-stu-id="fd584-196">In this task, you will learn how to use cross-browser compatible CSS3 snippets in order to implement some features in your website.</span></span>

1. <span data-ttu-id="fd584-197">Nel **Site.css** file, individuare il **intestazione** CSS (.header) di definizione di classe e posizionare il cursore sotto il **/ \*radius bordo\* /** segnaposto per aggiungere un nuovo frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="fd584-197">In the **Site.css** file, locate the **header** CSS class definition (.header) and place the cursor below the **/\*border radius\*/** placeholder to add a new snippet.</span></span> <span data-ttu-id="fd584-198">Premere **invio** per visualizzare l'elenco di IntelliSense e il tipo **radius** per filtrare l'elenco.</span><span class="sxs-lookup"><span data-stu-id="fd584-198">Press **Enter** to display the IntelliSense list and type **radius** to filter the list.</span></span> <span data-ttu-id="fd584-199">Selezionare il **border-radius** opzione dall'elenco con doppio clic e quindi premere il **scheda** chiave per inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="fd584-199">Select the **border-radius** option from the list with a double click, and then press the **TAB** key to insert the snippet.</span></span> <span data-ttu-id="fd584-200">Quindi, immettere una dimensione di radius in pixel e quindi premere **invio**.</span><span class="sxs-lookup"><span data-stu-id="fd584-200">Then, type a radius size in pixels and press **Enter**.</span></span> <span data-ttu-id="fd584-201">Ad esempio, digitare **15px**.</span><span class="sxs-lookup"><span data-stu-id="fd584-201">For instance, type **15px**.</span></span>

    <span data-ttu-id="fd584-202">Gli attributi CSS3 aggiunti per il frammento di codice verranno eseguito il rendering di bordi arrotondati nella maggior parte dei browser conformità HTML5, tra cui Mozilla e browser basato su WebKit.</span><span class="sxs-lookup"><span data-stu-id="fd584-202">The CSS3 attributes added by the snippet will render rounded borders in most HTML5 compliance browsers, including Mozilla and WebKit-based browsers.</span></span>

    <span data-ttu-id="fd584-203">![Usando un frammento di codice border-radius](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "usando un frammento di codice border-radius")</span><span class="sxs-lookup"><span data-stu-id="fd584-203">![Using a border-radius snippet](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "Using a border-radius snippet")</span></span>

    <span data-ttu-id="fd584-204">*Usando un frammento di codice border-radius*</span><span class="sxs-lookup"><span data-stu-id="fd584-204">*Using a border-radius snippet*</span></span>
2. <span data-ttu-id="fd584-205">Applicare lo stesso **bordo** frammenti di codice lo stile della pagina (.page).</span><span class="sxs-lookup"><span data-stu-id="fd584-205">Apply the same **border** snippets in the page style (.page).</span></span>

    <span data-ttu-id="fd584-206">CSS</span><span class="sxs-lookup"><span data-stu-id="fd584-206">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. <span data-ttu-id="fd584-207">Premere **F5** per eseguire la soluzione.</span><span class="sxs-lookup"><span data-stu-id="fd584-207">Press **F5** to run the solution.</span></span> <span data-ttu-id="fd584-208">Si noti che ogni pagina ora arrotondati i bordi.</span><span class="sxs-lookup"><span data-stu-id="fd584-208">Notice that each page now has rounded borders.</span></span>

    <span data-ttu-id="fd584-209">![Gli angoli arrotondati](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "gli angoli arrotondati")</span><span class="sxs-lookup"><span data-stu-id="fd584-209">![Rounded corners](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "Rounded corners")</span></span>

    <span data-ttu-id="fd584-210">*Angoli arrotondati*</span><span class="sxs-lookup"><span data-stu-id="fd584-210">*Rounded corners*</span></span>
4. <span data-ttu-id="fd584-211">Chiudere il browser e tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fd584-211">Close the browser and return to Visual Studio.</span></span>
5. <span data-ttu-id="fd584-212">Aprire il **Custom.css** file si trova sotto il **stili** cartella e posizionare il cursore all'interno di **div.images ul li img** definizione di classe.</span><span class="sxs-lookup"><span data-stu-id="fd584-212">Open the **Custom.css** file located under the **Styles** folder and place the cursor inside **div.images ul li img** class definition.</span></span>
6. <span data-ttu-id="fd584-213">Premere INVIO per visualizzare l'elenco di IntelliSense, tipo **ombreggiatura di casella** e premere il **scheda** tasto due volte per inserire il frammento di codice predefinito shadow nella definizione di classe.</span><span class="sxs-lookup"><span data-stu-id="fd584-213">Press enter to display the IntelliSense list, type **box-shadow** and press the **TAB** key twice to insert the default shadow code snippet inside the class definition.</span></span> <span data-ttu-id="fd584-214">Impostare i valori di ombreggiatura **10px 10px 5px 888 #**.</span><span class="sxs-lookup"><span data-stu-id="fd584-214">Set the shadow values to **10px 10px 5px #888**.</span></span> <span data-ttu-id="fd584-215">Digitare quindi **border-radius** e inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="fd584-215">Then, type **border-radius** and insert the code snippet.</span></span> <span data-ttu-id="fd584-216">Tipo **15px** per impostare dimensioni radius e premere **invio**.</span><span class="sxs-lookup"><span data-stu-id="fd584-216">Type **15px** to set radius size and press **ENTER**.</span></span>

    <span data-ttu-id="fd584-217">![Arrotondare gli angoli con ombreggiatura](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "rinnovato con ombreggiatura")</span><span class="sxs-lookup"><span data-stu-id="fd584-217">![Rounded corners with shadow](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "Rounded corners with shadow")</span></span>

    <span data-ttu-id="fd584-218">*Angoli arrotondati con ombreggiatura*</span><span class="sxs-lookup"><span data-stu-id="fd584-218">*Rounded corners with shadow*</span></span>

    > [!NOTE]
    > <span data-ttu-id="fd584-219">In questo momento, l'attributo shadow viene inserita con il prefisso corrispondente (moz, webkit, o) per supportare Mozilla e i browser Webkit (Chrome, Safari, Konkeror).</span><span class="sxs-lookup"><span data-stu-id="fd584-219">At this moment, the shadow attribute is inserted with the corresponding prefix (moz, webkit, o) to support Mozilla and Webkit (Chrome, Safari, Konkeror) browsers.</span></span>
7. <span data-ttu-id="fd584-220">Creare una nuova classe **div.images ul li img:hover** sotto il **div.images ul li img** definizione di classe e posizionare il cursore all'interno delle parentesi **.**</span><span class="sxs-lookup"><span data-stu-id="fd584-220">Create a new class **div.images ul li img:hover** below the **div.images ul li img** class definition and place the cursor inside the brackets **.**</span></span>

    <span data-ttu-id="fd584-221">CSS</span><span class="sxs-lookup"><span data-stu-id="fd584-221">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. <span data-ttu-id="fd584-222">Tipo **trasformare** e premere il **scheda** chiave due volte per inserire il frammento di trasformazione.</span><span class="sxs-lookup"><span data-stu-id="fd584-222">Type **transform** and press the **TAB** key twice in order to insert the transform snippet.</span></span> <span data-ttu-id="fd584-223">Quindi, immettere **rotate(-15deg)** per modificare il valore dell'angolo di rotazione quando si passa le immagini.</span><span class="sxs-lookup"><span data-stu-id="fd584-223">Then, enter **rotate(-15deg)** to change the rotation angle value when images are hovered.</span></span>

    <span data-ttu-id="fd584-224">CSS</span><span class="sxs-lookup"><span data-stu-id="fd584-224">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. <span data-ttu-id="fd584-225">Premere **F5** per eseguire la soluzione e passare alla pagina di CSS3.</span><span class="sxs-lookup"><span data-stu-id="fd584-225">Press **F5** to run the solution and browse to the CSS3 page.</span></span> <span data-ttu-id="fd584-226">Si noti che le immagini sono gli angoli arrotondati e casella ombreggiature.</span><span class="sxs-lookup"><span data-stu-id="fd584-226">Notice that the images have rounded corners and box shadows.</span></span> <span data-ttu-id="fd584-227">Posizionare il mouse sopra le immagini ed esaminarne le operazioni di rotazione.</span><span class="sxs-lookup"><span data-stu-id="fd584-227">Hover the mouse over the images and watch them rotate.</span></span>

    <span data-ttu-id="fd584-228">![Trasformare la rotazione di un'immagine di frammento](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "frammento trasformazione la rotazione di un'immagine")</span><span class="sxs-lookup"><span data-stu-id="fd584-228">![Transform snippet rotating an image](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "Transform snippet rotating an image")</span></span>

    <span data-ttu-id="fd584-229">*Trasformare la rotazione di un'immagine del frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="fd584-229">*Transform snippet rotating an image*</span></span>

    > [!NOTE]
    > <span data-ttu-id="fd584-230">Se si utilizza Internet Explorer 10 e non è possibile visualizzare le ombreggiature, verificare che sia impostata la modalità documento degli standard di Internet Explorer 10.</span><span class="sxs-lookup"><span data-stu-id="fd584-230">If you are using Internet Explorer 10 and cannot see the shadows, make sure the document mode is set to IE10 standards.</span></span> <span data-ttu-id="fd584-231">Premere **F12** aprire Strumenti di sviluppo di Internet Explorer e fare clic su **modalità documento** per passare alla finestra standard di IE10.</span><span class="sxs-lookup"><span data-stu-id="fd584-231">Press **F12** to open Internet Explorer developer tools and click **Document Mode** to change to IE10 standards.</span></span>

    ![about-us](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a><span data-ttu-id="fd584-233">Esercizio 2: Novità nell'Editor HTML</span><span class="sxs-lookup"><span data-stu-id="fd584-233">Exercise 2: What's New in the HTML Editor</span></span>

<span data-ttu-id="fd584-234">Visual Studio è un editor HTML migliorato.</span><span class="sxs-lookup"><span data-stu-id="fd584-234">Visual Studio has an improved HTML editor.</span></span> <span data-ttu-id="fd584-235">Alcuni dei miglioramenti inclusi in questa versione sono rientro intelligente nei documenti HTML, frammenti di codice HTML5, inizio HTML e corrispondenza dei tag di fine e la convalida HTML.</span><span class="sxs-lookup"><span data-stu-id="fd584-235">Some of the enhancements included in this version are smart indentation in HTML documents, HTML5 snippets, HTML start and end tag matching, and HTML validation.</span></span> <span data-ttu-id="fd584-236">In questa esercitazione, verrà visualizzato come queste modifiche migliorano la proprietà quando si utilizza il codice del sito Web.</span><span class="sxs-lookup"><span data-stu-id="fd584-236">Throughout this exercise, you will see how these changes improve your fluency when working in the website markup.</span></span>

<span data-ttu-id="fd584-237">Come l'editor CSS, editor HTML è stata migliorata.</span><span class="sxs-lookup"><span data-stu-id="fd584-237">Like the CSS editor, the HTML editor was also improved.</span></span> <span data-ttu-id="fd584-238">Molti di questi miglioramenti sono più piccole che semplificano la vita dello sviluppatore Web.</span><span class="sxs-lookup"><span data-stu-id="fd584-238">Most of these improvements are small ones that make the Web developer's life easier.</span></span> <span data-ttu-id="fd584-239">Elementi come più frammenti di codice per HTML5, rientro smart tag di inizio e fine di corrispondenza quando si modifica e destinazione del documento HTML DOCTYPE convalida sono riportati alcuni di questi miglioramenti.</span><span class="sxs-lookup"><span data-stu-id="fd584-239">Things like more code snippets for HTML5, smart indentation, matching start and end tags when editing and validation targeting the HTML document DOCTYPE are some of these improvements.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a><span data-ttu-id="fd584-240">Attività 1 - convalida migliorate DOCTYPE</span><span class="sxs-lookup"><span data-stu-id="fd584-240">Task 1 - Improved DOCTYPE Validation</span></span>

<span data-ttu-id="fd584-241">Editor HTML è ora la possibilità di controllare il tipo di documento della pagina, anche se potrebbe essere la definizione della pagina master.</span><span class="sxs-lookup"><span data-stu-id="fd584-241">The HTML editor now has the ability to check the DOCTYPE of your page, even though the definition might be in the master page.</span></span> <span data-ttu-id="fd584-242">In base al tipo di documento della pagina, l'editor HTML verrà convalidato con il set corretto di regole e verrà filtrato l'elenco di IntelliSense, prendere in considerazione gli elementi di tipo di documento.</span><span class="sxs-lookup"><span data-stu-id="fd584-242">Depending on the DOCTYPE of your page, the HTML editor will validate with the correct set of rules and will filter the IntelliSense list considering the DOCTYPE elements.</span></span>

<span data-ttu-id="fd584-243">In questa attività si modificherà la dichiarazione DOCTYPE di una pagina per vedere come il comportamento dell'editor HTML cambia di conseguenza.</span><span class="sxs-lookup"><span data-stu-id="fd584-243">In this task, you will change the DOCTYPE of a page to see how the HTML editor behavior changes accordingly.</span></span>

1. <span data-ttu-id="fd584-244">Se non è già aperto, avviare **Visual Studio** e aprire il **WhatsNewASPNET.sln** soluzione si trova nel **Source\WhatsNewASPNET** cartella di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="fd584-244">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="fd584-245">Aprire il **Site. master** pagina.</span><span class="sxs-lookup"><span data-stu-id="fd584-245">Open the **Site.Master** page.</span></span>
3. <span data-ttu-id="fd584-246">Si noti lo Schema di destinazione per la convalida della barra degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="fd584-246">Notice the Target Schema for Validation Toolbar.</span></span> <span data-ttu-id="fd584-247">Il comportamento dell'editor HTML (convalida, IntelliSense, e così via) verrà modificato in modo corretto in base al tipo di documento selezionato.</span><span class="sxs-lookup"><span data-stu-id="fd584-247">The way the HTML editor behaves (Validation, IntelliSense, etc.) will properly change to fit the Doctype selected.</span></span>

    <span data-ttu-id="fd584-248">![Utilizza Doctype nella barra degli strumenti modifica dell'origine HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Doctype utilizzare nella barra degli strumenti modifica dell'origine HTML")</span><span class="sxs-lookup"><span data-stu-id="fd584-248">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="fd584-249">*Utilizza Doctype nella barra degli strumenti modifica dell'origine HTML*</span><span class="sxs-lookup"><span data-stu-id="fd584-249">*Use Doctype in HTML Source Editing toolbar*</span></span>
4. <span data-ttu-id="fd584-250">Modificare lo Schema di destinazione in formato HTML 4.01.</span><span class="sxs-lookup"><span data-stu-id="fd584-250">Change the Target Schema to HTML 4.01.</span></span>

    <span data-ttu-id="fd584-251">![Modifica tipo di documento nella barra degli strumenti modifica dell'origine HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "modifica Doctype nella barra degli strumenti modifica dell'origine HTML")</span><span class="sxs-lookup"><span data-stu-id="fd584-251">![Changing Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "Changing Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="fd584-252">*Modifica tipo di documento nella barra degli strumenti modifica dell'origine HTML*</span><span class="sxs-lookup"><span data-stu-id="fd584-252">*Changing Doctype in HTML Source Editing toolbar*</span></span>
5. <span data-ttu-id="fd584-253">Posizionare il cursore sotto il **corpo** elemento e iniziare a digitare il nome di un elemento HTML5 (ad esempio, **video**).</span><span class="sxs-lookup"><span data-stu-id="fd584-253">Place the cursor under the **body** element, and start typing the name of an HTML5 element (for example, **video**).</span></span> <span data-ttu-id="fd584-254">Si noti che l'elemento non è disponibile nell'elenco di IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="fd584-254">Notice that the element is not available in the IntelliSense list.</span></span>

    <span data-ttu-id="fd584-255">![Elementi HTML5 non elencati](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "elementi HTML5 non elencati")</span><span class="sxs-lookup"><span data-stu-id="fd584-255">![HTML5 elements not listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5 elements not listed")</span></span>

    <span data-ttu-id="fd584-256">*Elementi HTML5 non elencati*</span><span class="sxs-lookup"><span data-stu-id="fd584-256">*HTML5 elements not listed*</span></span>
6. <span data-ttu-id="fd584-257">Annullare le modifiche allo Schema di destinazione per la convalida della barra degli strumenti, selezione del tipo di documento: XHTML5 nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="fd584-257">Undo the changes to the Target Schema for Validation Toolbar, picking DOCTYPE: XHTML5 from the dropdown list.</span></span>

    <span data-ttu-id="fd584-258">![Utilizza Doctype nella barra degli strumenti modifica dell'origine HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Doctype utilizzare nella barra degli strumenti modifica dell'origine HTML")</span><span class="sxs-lookup"><span data-stu-id="fd584-258">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="fd584-259">*Reimpostare Doctype nella barra degli strumenti modifica dell'origine HTML*</span><span class="sxs-lookup"><span data-stu-id="fd584-259">*Reset Doctype in HTML Source Editing toolbar*</span></span>
7. <span data-ttu-id="fd584-260">Posizionare il cursore sotto il **corpo** elemento e iniziare a digitare di nuovo un elemento HTML5 (ad esempio, come **video**).</span><span class="sxs-lookup"><span data-stu-id="fd584-260">Place the cursor under the **body** element and start typing an HTML5 element again (For example, like **video**).</span></span> <span data-ttu-id="fd584-261">Si noti che gli elementi di HTML5 sono ora disponibili nell'elenco di IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="fd584-261">Notice that the HTML5 elements are now available in the IntelliSense list.</span></span>

    <span data-ttu-id="fd584-262">![Elementi HTML5 elencati](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "elementi HTML5 elencati")</span><span class="sxs-lookup"><span data-stu-id="fd584-262">![HTML5 elements being listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5 elements being listed")</span></span>

    <span data-ttu-id="fd584-263">*Elementi HTML5 elencati*</span><span class="sxs-lookup"><span data-stu-id="fd584-263">*HTML5 elements being listed*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a><span data-ttu-id="fd584-264">Attività 2: inizio/fine tag aggiornamento automatico</span><span class="sxs-lookup"><span data-stu-id="fd584-264">Task 2 - Start/End Tags Automatic Update</span></span>

<span data-ttu-id="fd584-265">Ora, Visual Studio aggiorna il codice HTML di apertura o chiusura di tag dell'elemento che si sta modificando in modo che corrisponda a loro.</span><span class="sxs-lookup"><span data-stu-id="fd584-265">Visual Studio now updates the HTML opening or closing tags of the element that you are editing to match each other.</span></span> <span data-ttu-id="fd584-266">Questa nuova funzionalità la produttività quando si modifica il tag HTML.</span><span class="sxs-lookup"><span data-stu-id="fd584-266">This new feature will improve your productivity when editing HTML tags.</span></span>

1. <span data-ttu-id="fd584-267">Nel **Default.aspx** pagina, aggiungere un **H3** elemento con un titolo (ad esempio, Visual Studio 2012 funziona).</span><span class="sxs-lookup"><span data-stu-id="fd584-267">On the **Default.aspx** page, add an **H3** element with a title (for example, Visual Studio 2012 Rocks!).</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
~~~
2. <span data-ttu-id="fd584-268">Modifica il **H3** tag e il tipo **H2** o **H1.**</span><span class="sxs-lookup"><span data-stu-id="fd584-268">Change the **H3** tag and type **H2** or **H1.**</span></span>

    <span data-ttu-id="fd584-269">Si noti che il tag di fine viene automaticamente aggiornato.</span><span class="sxs-lookup"><span data-stu-id="fd584-269">Notice that the end tag automatically updates.</span></span> <span data-ttu-id="fd584-270">È inoltre possibile modificare il tag di fine per verificare che il tag di inizio Aggiorna di conseguenza troppo.</span><span class="sxs-lookup"><span data-stu-id="fd584-270">You can also modify the end tag to see that the start tag updates accordingly too.</span></span>

    <span data-ttu-id="fd584-271">![L'aggiornamento automatico del tag di fine](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "aggiornamento automatico del tag di fine")</span><span class="sxs-lookup"><span data-stu-id="fd584-271">![Automatic update of the end tag](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "Automatic update of the end tag")</span></span>

    <span data-ttu-id="fd584-272">*Aggiornamento automatico del tag di fine*</span><span class="sxs-lookup"><span data-stu-id="fd584-272">*Automatic update of the end tag*</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a><span data-ttu-id="fd584-273">Attività 3 - nuovi frammenti di codice HTML5</span><span class="sxs-lookup"><span data-stu-id="fd584-273">Task 3 - New HTML5 Code Snippets</span></span>

<span data-ttu-id="fd584-274">Visual Studio ora include diversi frammenti di codice HTML5.</span><span class="sxs-lookup"><span data-stu-id="fd584-274">Visual Studio now includes several HTML5 code snippets.</span></span> <span data-ttu-id="fd584-275">In questa attività, si utilizzerà alcuni di questi frammenti di codice.</span><span class="sxs-lookup"><span data-stu-id="fd584-275">In this task, you will use some of these snippets.</span></span>

1. <span data-ttu-id="fd584-276">Aggiungere una nuova cartella denominata **audio** alla radice della cartella del sito web.</span><span class="sxs-lookup"><span data-stu-id="fd584-276">Add a new folder named **audio** to the root of the web site folder.</span></span> <span data-ttu-id="fd584-277">Aprire Esplora risorse e copiare tutti i file audio nel **audio** cartella la **WhatsNewASPNET.sln** soluzione.</span><span class="sxs-lookup"><span data-stu-id="fd584-277">Open Windows Explorer and copy any audio file into the **audio** folder of the **WhatsNewASPNET.sln** solution.</span></span>
2. <span data-ttu-id="fd584-278">Nel **Default.aspx** pagina, posizionare il cursore sotto il Massi Web11!!</span><span class="sxs-lookup"><span data-stu-id="fd584-278">In the **Default.aspx** page, locate the cursor under the Web11 Rocks!!</span></span> <span data-ttu-id="fd584-279">Intestazione.</span><span class="sxs-lookup"><span data-stu-id="fd584-279">Header.</span></span> <span data-ttu-id="fd584-280">Tipo **audio** e premere il tasto TAB.</span><span class="sxs-lookup"><span data-stu-id="fd584-280">Type **audio** and press the TAB key.</span></span>

    <span data-ttu-id="fd584-281">Il nuovo editor HTML include frammenti di codice per il contenuto di HTML5.</span><span class="sxs-lookup"><span data-stu-id="fd584-281">The new HTML editor includes code snippets for HTML5 content.</span></span> <span data-ttu-id="fd584-282">Ricordarsi di utilizzare la definizione del tipo di documento appropriata per abilitare i frammenti di codice HTML5.</span><span class="sxs-lookup"><span data-stu-id="fd584-282">Remember to use the proper DOCTYPE definition to enable the HTML5 snippets.</span></span>

    <span data-ttu-id="fd584-283">![Inserimento di frammenti di codice HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "inserimento di frammenti di codice HTML5")</span><span class="sxs-lookup"><span data-stu-id="fd584-283">![Inserting HTML5 Code Snippets](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Inserting HTML5 Code Snippets")</span></span>

    <span data-ttu-id="fd584-284">*Inserimento di frammenti di codice HTML5*</span><span class="sxs-lookup"><span data-stu-id="fd584-284">*Inserting HTML5 Code Snippets*</span></span>
3. <span data-ttu-id="fd584-285">Aggiornare l'origine audio in modo che punti a un file audio esistente.</span><span class="sxs-lookup"><span data-stu-id="fd584-285">Update the audio source to point to an existing audio file.</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

> [!NOTE]
> You will need to add the audio file to the solution.
~~~
4. <span data-ttu-id="fd584-286">Premere **F5** per eseguire il sito e riprodurre l'audio.</span><span class="sxs-lookup"><span data-stu-id="fd584-286">Press **F5** to run the site and play the audio.</span></span>

    <span data-ttu-id="fd584-287">![Esecuzione del controllo audio](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "esecuzione del controllo audio")</span><span class="sxs-lookup"><span data-stu-id="fd584-287">![Running the audio control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "Running the audio control")</span></span>

    <span data-ttu-id="fd584-288">*Esecuzione del controllo audio*</span><span class="sxs-lookup"><span data-stu-id="fd584-288">*Running the audio control*</span></span>

    > [!NOTE]
    > <span data-ttu-id="fd584-289">È anche possibile provare più frammenti di codice inclusi in Visual Studio, ad esempio video, nella figura, e così via.</span><span class="sxs-lookup"><span data-stu-id="fd584-289">You can also try more snippets included in Visual Studio, such as video, figure, etc.</span></span>
5. <span data-ttu-id="fd584-290">A questo punto, provare a inserire un controllo in una parte della pagina.</span><span class="sxs-lookup"><span data-stu-id="fd584-290">Now, try to insert a control in some part of the page.</span></span> <span data-ttu-id="fd584-291">Ad esempio, il tentativo di inserire un **GridView** (controllo), ma anziché digitare  **&lt;Gri,** inizia a digitare  **&lt;GV**.</span><span class="sxs-lookup"><span data-stu-id="fd584-291">For example, try to insert a **GridView** control, but instead of typing **&lt;Gri,** start typing **&lt;GV**.</span></span> <span data-ttu-id="fd584-292">Si noti che l'elenco di IntelliSense mostra il **asp: GridView** controllo.</span><span class="sxs-lookup"><span data-stu-id="fd584-292">Notice that the IntelliSense list shows the **asp:GridView** control.</span></span>

    <span data-ttu-id="fd584-293">IntelliSense nell'Editor HTML offre ora ricerca iniziali maiuscole, nonché parziale corrispondente (il recupero di tutti gli elementi che contiene il termine).</span><span class="sxs-lookup"><span data-stu-id="fd584-293">IntelliSense in the HTML Editor now provides title-casing search, as well as partial matching (retrieving all elements that contains the term).</span></span>

    <span data-ttu-id="fd584-294">![Inserimento di un controllo GridView con IntelliSense Elenca](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "inserimento di un controllo GridView con gli elenchi di IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="fd584-294">![Inserting a GridView with IntelliSense lists](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Inserting a GridView with IntelliSense lists")</span></span>

    <span data-ttu-id="fd584-295">*Inserimento di un controllo GridView con gli elenchi di IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="fd584-295">*Inserting a GridView with IntelliSense lists*</span></span>

    <span data-ttu-id="fd584-296">Se si digita  **&lt;griglia** verranno visualizzati tutti gli elementi che corrispondono al termine, ma Visual Studio verrà suggerito il **gridview** controllo:</span><span class="sxs-lookup"><span data-stu-id="fd584-296">If you type **&lt;grid** you will get all the items that match the term, but Visual Studio will suggest the **gridview** control:</span></span>

    <span data-ttu-id="fd584-297">![Inserimento di un controllo GridView con gli elenchi di IntelliSense e di corrispondenza parziale](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "inserimento di un controllo GridView con corrispondenza parziale e gli elenchi di IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="fd584-297">![Inserting a GridView with IntelliSense lists and partial matching](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Inserting a GridView with IntelliSense lists and partial matching")</span></span>

    <span data-ttu-id="fd584-298">*Inserimento di un controllo GridView con corrispondenza parziale e gli elenchi di IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="fd584-298">*Inserting a GridView with IntelliSense lists and partial matching*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a><span data-ttu-id="fd584-299">Attività 4: Editor HTML Smart tag</span><span class="sxs-lookup"><span data-stu-id="fd584-299">Task 4 - HTML Editor Smart Tags</span></span>

<span data-ttu-id="fd584-300">Un altro miglioramento all'Editor HTML è la funzionalità Smart tag.</span><span class="sxs-lookup"><span data-stu-id="fd584-300">Another improvement in the HTML Editor is the Smart Tags feature.</span></span> <span data-ttu-id="fd584-301">Gli smart tag rendono più semplice eseguire attività di sviluppo comuni o ripetitive in base a livello di controllo.</span><span class="sxs-lookup"><span data-stu-id="fd584-301">Smart tags make it easy to perform common or repetitive development tasks on a per-control basis.</span></span> <span data-ttu-id="fd584-302">Questa funzionalità era già disponibile nella finestra di progettazione HTML, ma non nell'Editor HTML.</span><span class="sxs-lookup"><span data-stu-id="fd584-302">This feature was already available in the HTML Designer, but not in the HTML Editor.</span></span>

1. <span data-ttu-id="fd584-303">Aprire **Site. master** e individuare il **asp: Menu** elemento.</span><span class="sxs-lookup"><span data-stu-id="fd584-303">Open **Site.Master** and locate the **asp:Menu** element.</span></span> <span data-ttu-id="fd584-304">Posizionare il cursore sul tag di inizio e si noti che l'icona piccola visualizzata nella parte inferiore dell'elemento - fare clic per aprire il menu smart tag.</span><span class="sxs-lookup"><span data-stu-id="fd584-304">Place the cursor on the start tag and notice that the small glyph displayed at the bottom of the element - click it to open the smart tasks menu.</span></span> <span data-ttu-id="fd584-305">Si noti che si può accedere rapidamente ad alcune attività correlate al controllo Menu.</span><span class="sxs-lookup"><span data-stu-id="fd584-305">Notice that you have quick access to some tasks related to the Menu control.</span></span>

    <span data-ttu-id="fd584-306">![Attività per il controllo Menu Smart](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Smart attività per il controllo Menu")</span><span class="sxs-lookup"><span data-stu-id="fd584-306">![Smart tasks for the Menu control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Smart tasks for the Menu control")</span></span>

    <span data-ttu-id="fd584-307">*Smart task per il controllo Menu*</span><span class="sxs-lookup"><span data-stu-id="fd584-307">*Smart tasks for the Menu control*</span></span>

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a><span data-ttu-id="fd584-308">Attività 5 - rientri intelligenti</span><span class="sxs-lookup"><span data-stu-id="fd584-308">Task 5 - Smart Indentation</span></span>

<span data-ttu-id="fd584-309">Una delle procedure consigliate in formato HTML è il rientro di elementi annidati per mantenere il codice come leggibili.</span><span class="sxs-lookup"><span data-stu-id="fd584-309">One of the best practices in HTML is indenting the nested elements to keep the code readable.</span></span> <span data-ttu-id="fd584-310">In Visual Studio 2012, si noterà che l'editor rientra automaticamente gli elementi durante la scrittura di codice.</span><span class="sxs-lookup"><span data-stu-id="fd584-310">In Visual Studio 2012, you will notice that the editor automatically indents the elements while you are writing the code.</span></span>

> [!NOTE]
> <span data-ttu-id="fd584-311">Nelle versioni precedenti di Visual Studio, rientri intelligenti erano disponibili nell'editor XML, ma non nell'editor HTML.</span><span class="sxs-lookup"><span data-stu-id="fd584-311">In previous version of Visual Studio, smart indentation was available in the XML editor but not in the HTML editor.</span></span>


1. <span data-ttu-id="fd584-312">Assicurarsi che la configurazione di rientri in Editor HTML è impostata su rientri intelligenti.</span><span class="sxs-lookup"><span data-stu-id="fd584-312">Make sure that the Indenting configuration on the HTML Editor is set to Smart Indentation.</span></span> <span data-ttu-id="fd584-313">A tale scopo, selezionare il **strumenti | Opzioni** opzione di menu e quindi selezionare il **Editor di testo | HTML | Schede** pagina nel riquadro a sinistra della schermata.</span><span class="sxs-lookup"><span data-stu-id="fd584-313">To do that, select the **Tools | Options** menu option and then select the **Text Editor | HTML | Tabs** page in the left pane of the screen.</span></span> <span data-ttu-id="fd584-314">Selezionare l'opzione rientri intelligenti.</span><span class="sxs-lookup"><span data-stu-id="fd584-314">Select the Smart indentation option.</span></span>

    <span data-ttu-id="fd584-315">![Impostazioni dell'Editor HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "impostazioni dell'Editor HTML")</span><span class="sxs-lookup"><span data-stu-id="fd584-315">![HTML Editor settings](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML Editor settings")</span></span>

    <span data-ttu-id="fd584-316">*Impostazioni dell'Editor HTML*</span><span class="sxs-lookup"><span data-stu-id="fd584-316">*HTML Editor settings*</span></span>
2. <span data-ttu-id="fd584-317">Nel **Default.aspx** pagina, rimuovere tutto il contenuto sotto l'elemento audio.</span><span class="sxs-lookup"><span data-stu-id="fd584-317">On the **Default.aspx** page, remove all the content under the audio element.</span></span>
3. <span data-ttu-id="fd584-318">Posizionare il cursore alla fine dell'apertura **audio** elemento e premere **invio**.</span><span class="sxs-lookup"><span data-stu-id="fd584-318">Place the cursor at the end of the opening **audio** element and hit **ENTER**.</span></span>

    <span data-ttu-id="fd584-319">Si noti che la nuova posizione del cursore è un livello di rientro aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="fd584-319">Notice that the new position of cursor has an additional indentation level.</span></span>

    <span data-ttu-id="fd584-320">![Rientro nell'Editor HTML per Smart](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "Smart rientro nell'Editor HTML")</span><span class="sxs-lookup"><span data-stu-id="fd584-320">![Smart indentation in the HTML Editor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "Smart indentation in the HTML Editor")</span></span>

    <span data-ttu-id="fd584-321">*Rientro smart nell'Editor HTML*</span><span class="sxs-lookup"><span data-stu-id="fd584-321">*Smart indentation in the HTML Editor*</span></span>
4. <span data-ttu-id="fd584-322">Ripristinare il tag con il contenuto è stato rimosso o chiudere audio **Default.aspx** senza salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="fd584-322">Restore the audio tag with the content you have removed, or close **Default.aspx** without saving the changes.</span></span>

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a><span data-ttu-id="fd584-323">Attività 6 - Estrai nel controllo utente</span><span class="sxs-lookup"><span data-stu-id="fd584-323">Task 6 - Extract to User Control</span></span>

<span data-ttu-id="fd584-324">Gli strumenti di Refactoring inclusi in Visual Studio, ad esempio l'estrazione di una parte di codice a una funzione, sono importanti funzionalità che facilitano l'analisi utilizzo software e il refactoring del codice esistente.</span><span class="sxs-lookup"><span data-stu-id="fd584-324">The Refactoring tools included in Visual Studio, such as extracting a portion of code to a function, are great features that facilitate the improvement and the refactoring the existing code.</span></span> <span data-ttu-id="fd584-325">La controparte per le pagine ASP.NET sarebbe l'estrazione del codice HTML a un controllo utente.</span><span class="sxs-lookup"><span data-stu-id="fd584-325">The counterpart for ASP.NET pages would be the extraction of HTML code to a User Control.</span></span> <span data-ttu-id="fd584-326">Eseguire l'operazione manualmente implica alcuni passaggi, ad esempio la creazione di un nuovo controllo utente, lo spostamento di sezione di codice per il controllo utente, la registrazione di un prefisso di tag per il controllo utente e, infine, la creazione del controllo di utente nelle pagine.</span><span class="sxs-lookup"><span data-stu-id="fd584-326">Doing it manually would involve several steps, like creating a new User Control, moving the code section to the User Control, registering a tag prefix for the User Control, and, finally, instantiating the User Control on the pages.</span></span> <span data-ttu-id="fd584-327">A questo punto, il nuovo *Estrai nel controllo utente* strumento esegue automaticamente tutti questi passaggi per l'utente.</span><span class="sxs-lookup"><span data-stu-id="fd584-327">Now, the new *Extract to User Control* tool automatically performs all those steps for you.</span></span>

<span data-ttu-id="fd584-328">In questa attività, si utilizzerà l'estrazione di nuovo all'operazione contesto controllo utente per generare un nuovo controllo utente dal codice selezionato.</span><span class="sxs-lookup"><span data-stu-id="fd584-328">In this task, you will use the new Extract to User Control contextual operation to generate a new user control from the selected code.</span></span>

1. <span data-ttu-id="fd584-329">Nel **Default.aspx** pagina, selezionare il **H2** e **audio** elementi.</span><span class="sxs-lookup"><span data-stu-id="fd584-329">On the **Default.aspx** page, select the **H2** and **audio** elements.</span></span>
2. <span data-ttu-id="fd584-330">Fare clic e selezionare **Estrai nel controllo utente**.</span><span class="sxs-lookup"><span data-stu-id="fd584-330">Right click and select **Extract to User Control**.</span></span>

    <span data-ttu-id="fd584-331">![Estrarre all'opzione di menu controllo utente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "estrarre all'opzione di menu controllo utente")</span><span class="sxs-lookup"><span data-stu-id="fd584-331">![Extract to User Control menu option](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Extract to User Control menu option")</span></span>

    <span data-ttu-id="fd584-332">*Estrarre all'opzione di menu controllo utente*</span><span class="sxs-lookup"><span data-stu-id="fd584-332">*Extract to User Control menu option*</span></span>
3. <span data-ttu-id="fd584-333">Digitare un nome per il nuovo controllo utente.</span><span class="sxs-lookup"><span data-stu-id="fd584-333">Type a name for the new user control.</span></span> <span data-ttu-id="fd584-334">Ad esempio, **Jukebox.ascx**, quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="fd584-334">For instance, **Jukebox.ascx**, and then click **OK**.</span></span>

    <span data-ttu-id="fd584-335">![Salvare il controllo utente estratti](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "il salvataggio del controllo utente estratti")</span><span class="sxs-lookup"><span data-stu-id="fd584-335">![Saving the extracted user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "Saving the extracted user control")</span></span>

    <span data-ttu-id="fd584-336">*Salvare il controllo utente estratti*</span><span class="sxs-lookup"><span data-stu-id="fd584-336">*Saving the extracted user control*</span></span>
4. <span data-ttu-id="fd584-337">Si noti che il codice selezionato è stato estratto in un controllo utente e il percorso originale del codice selezionato è stato sostituito con un'istanza del nuovo controllo utente.</span><span class="sxs-lookup"><span data-stu-id="fd584-337">Notice that the selected code was extracted to a user control and the original location of the selected code was replaced with an instance of the new user control.</span></span>

    <span data-ttu-id="fd584-338">![Pagina aggiornati automaticamente per usare il nuovo controllo utente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "pagina aggiornati automaticamente per usare il nuovo controllo utente")</span><span class="sxs-lookup"><span data-stu-id="fd584-338">![Page automatically updated to use the new user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "Page automatically updated to use the new user control")</span></span>

    <span data-ttu-id="fd584-339">*Pagina aggiornati automaticamente per usare il nuovo controllo utente*</span><span class="sxs-lookup"><span data-stu-id="fd584-339">*Page automatically updated to use the new user control*</span></span>
5. <span data-ttu-id="fd584-340">Premere **F5** per eseguire la pagina e verificare il corretto funzionamento di controllo.</span><span class="sxs-lookup"><span data-stu-id="fd584-340">Press **F5** to run the page and verify that the control works.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a><span data-ttu-id="fd584-341">Esercizio 3: Novità nell'Editor JavaScript</span><span class="sxs-lookup"><span data-stu-id="fd584-341">Exercise 3: What's New in the JavaScript Editor</span></span>

<span data-ttu-id="fd584-342">La scrittura o modifica del codice JavaScript non è un compito facile, soprattutto quando l'avvio dell'applicazione per l'aumento delle dimensioni e si troverà la gestione di file lunghi e centinaia di funzioni.</span><span class="sxs-lookup"><span data-stu-id="fd584-342">Writing or editing JavaScript code is not an easy task, especially when your application starts to grow in size and you find yourself dealing with long files and hundreds of functions.</span></span> <span data-ttu-id="fd584-343">Gli sviluppatori di script in genere necessario eseguire un lavoro aggiuntivo per mantenere la leggibilità di codice e spostarsi tra i file.</span><span class="sxs-lookup"><span data-stu-id="fd584-343">Script developers usually have to do some extra work to maintain code legibility and navigate across files.</span></span> <span data-ttu-id="fd584-344">Con l'inclusione di librerie JavaScript come jQuery, spostamento di script è diventata una richiesta a causa della lunghezza del codice.</span><span class="sxs-lookup"><span data-stu-id="fd584-344">With the inclusion of JavaScript libraries like jQuery, script navigation has become a challenge itself because of the code length.</span></span>

<span data-ttu-id="fd584-345">Visual Studio ha rinnovato l'editor JavaScript con la promessa affinché la modalità di codice accessibile e organizzati.</span><span class="sxs-lookup"><span data-stu-id="fd584-345">Visual Studio has renewed the JavaScript editor with the promise to make the code mode accessible and organized.</span></span> <span data-ttu-id="fd584-346">Molte funzionalità di Visual Studio che esiste già nell'editor di codice c# o Visual Basic vengono ora implementate nell'editor JavaScript: Vai a definizione, il rientro automatico, documentazione e la convalida durante la scrittura.</span><span class="sxs-lookup"><span data-stu-id="fd584-346">Many Visual Studio features that already existed in C# or VB editors are now implemented in the JavaScript editor: Go To Definition, automatic indentation, documentation and validation when you are writing.</span></span> <span data-ttu-id="fd584-347">Con l'elenco di IntelliSense rinnovato è il catalogo della funzione JavaScript a portata di mano.</span><span class="sxs-lookup"><span data-stu-id="fd584-347">With the renewed IntelliSense list you will have the JavaScript function catalog at your fingertips.</span></span>

<span data-ttu-id="fd584-348">In questo esercizio, si apprenderà alcune delle nuove funzionalità e miglioramenti dell'editor JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fd584-348">In this exercise, you will learn some of the new features and improvements of JavaScript editor.</span></span> <span data-ttu-id="fd584-349">Verrà sfogliare i file di esempio e individuare ognuna delle nuove caratteristiche che consentono la programmazione JavaScript più efficiente all'interno di Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="fd584-349">You will browse sample files and discover each of the new characteristics that will make your JavaScript programming more efficient within Visual Studio 2012.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a><span data-ttu-id="fd584-350">Attività 1: nuove funzionalità di Editor JavaScript</span><span class="sxs-lookup"><span data-stu-id="fd584-350">Task 1 - JavaScript Editor New Features</span></span>

<span data-ttu-id="fd584-351">Questa attività verrà presentate alcune delle nuove funzionalità JavaScript editor concentrarsi sull'organizzazione del codice e riportare una migliore esperienza utente.</span><span class="sxs-lookup"><span data-stu-id="fd584-351">This task will introduce you to some of the new JavaScript editor features, which focus on organizing your code and bringing a better user experience.</span></span>

1. <span data-ttu-id="fd584-352">Se non è già aperto, avviare **Visual Studio** e aprire il **WhatsNewASPNET.sln** soluzione si trova nel **Source\WhatsNewASPNET** cartella di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="fd584-352">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="fd584-353">Premere **F5** per eseguire l'applicazione, quindi fare clic sul collegamento JavaScript nella barra di spostamento.</span><span class="sxs-lookup"><span data-stu-id="fd584-353">Press **F5** to run the application, then click the JavaScript link in the navigation bar.</span></span> <span data-ttu-id="fd584-354">Aggiornare la pagina più volte e verificare come l'incremento del contatore.</span><span class="sxs-lookup"><span data-stu-id="fd584-354">Refresh the page several times and check how the counter increments.</span></span>

    <span data-ttu-id="fd584-355">![Contatore di pagine](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "contatore di pagine")</span><span class="sxs-lookup"><span data-stu-id="fd584-355">![Page counter](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "Page counter")</span></span>

    <span data-ttu-id="fd584-356">*Contatore di pagine*</span><span class="sxs-lookup"><span data-stu-id="fd584-356">*Page counter*</span></span>
3. <span data-ttu-id="fd584-357">Chiudere il browser e tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fd584-357">Close the browser and go back to Visual Studio.</span></span>
4. <span data-ttu-id="fd584-358">Aprire il **JavaScript.aspx** pagina e individuare il **&lt;script&gt;** blocco (mostrato sotto).</span><span class="sxs-lookup"><span data-stu-id="fd584-358">Open the **JavaScript.aspx** page and locate the **&lt;script&gt;** block (shown below).</span></span>

    <span data-ttu-id="fd584-359">Il codice seguente usa l'archiviazione locale HTML5 per archiviare un *pageLoadCount* variabile che archivia il numero di volte in cui la pagina è stata visitata dall'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="fd584-359">The following code uses HTML5 local storage to store a *pageLoadCount* variable that stores the number of times the page has been visited by the current user.</span></span> <span data-ttu-id="fd584-360">Archiviazione locale è un database di chiave-valore sul lato client introdotto con lo standard HTML5.</span><span class="sxs-lookup"><span data-stu-id="fd584-360">Local Storage is a client-side key-value database introduced with the HTML5 standard.</span></span> <span data-ttu-id="fd584-361">I dati vengono salvati nel computer locale, all'interno del browser dell'utente.</span><span class="sxs-lookup"><span data-stu-id="fd584-361">The data is saved on the local machine, inside the user's browser.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="fd584-362">Verificare che il tipo di documento è impostato su XHTML5 prima di procedere con i passaggi successivi.</span><span class="sxs-lookup"><span data-stu-id="fd584-362">Ensure the DOCTYPE is set to XHTML5 before proceeding with the next steps.</span></span>
5. <span data-ttu-id="fd584-363">Modificare il codice e sono incluse le funzionalità di HTML5, come archiviazione locale e i relativi metodi interni che IntelliSense per JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fd584-363">Edit the code and notice that IntelliSense for JavaScript includes HTML5 features, like local storage, and their inner methods.</span></span>

    <span data-ttu-id="fd584-364">![Le funzionalità di HTML5 JavaScript di JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5 JavaScript le funzionalità di JavaScript")</span><span class="sxs-lookup"><span data-stu-id="fd584-364">![HTML5 JavaScript features in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5 JavaScript features in JavaScript")</span></span>

    <span data-ttu-id="fd584-365">*Funzionalità di HTML5 JavaScript di JavaScript*</span><span class="sxs-lookup"><span data-stu-id="fd584-365">*HTML5 JavaScript features in JavaScript*</span></span>
6. <span data-ttu-id="fd584-366">Fare clic su qualsiasi parentesi quadra aperta (**{**) da cui la creazione di script del codice e si noti che le parentesi quadre sono evidenziate.</span><span class="sxs-lookup"><span data-stu-id="fd584-366">Click any opening bracket (**{**) from the scripting code and notice that the brackets are highlighted.</span></span>

    <span data-ttu-id="fd584-367">![Le parentesi quadre sono evidenziate](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "tra parentesi vengono evidenziate")</span><span class="sxs-lookup"><span data-stu-id="fd584-367">![Brackets are highlighted](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "Brackets are highlighted")</span></span>

    <span data-ttu-id="fd584-368">*Le parentesi quadre sono evidenziate*</span><span class="sxs-lookup"><span data-stu-id="fd584-368">*Brackets are highlighted*</span></span>
7. <span data-ttu-id="fd584-369">Rimuovere la funzione **testAutoAlign()** (selezionare le tre righe, è possibile utilizzare **CTRL** + **K**; **CTRL** + **U**) e individuare il cursore all'interno del codice di funzione.</span><span class="sxs-lookup"><span data-stu-id="fd584-369">Uncomment the function **testAutoAlign()** (select the three lines and you can use **CTRL** + **K**; **CTRL** + **U**) and locate the cursor inside the function code.</span></span> <span data-ttu-id="fd584-370">Premere INVIO per aggiungere una seconda riga.</span><span class="sxs-lookup"><span data-stu-id="fd584-370">Press enter to append a second line.</span></span> <span data-ttu-id="fd584-371">Si noti che il codice è ora **allineato** e **rientrato automaticamente**.</span><span class="sxs-lookup"><span data-stu-id="fd584-371">Notice that the code is now **aligned** and **auto-indented**.</span></span>

    <span data-ttu-id="fd584-372">![Il codice JavaScript viene automaticamente allineati](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "codice JavaScript viene allineato automaticamente")</span><span class="sxs-lookup"><span data-stu-id="fd584-372">![JavaScript code is auto aligned](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript code is auto aligned")</span></span>

    <span data-ttu-id="fd584-373">*Il codice JavaScript viene allineato automaticamente*</span><span class="sxs-lookup"><span data-stu-id="fd584-373">*JavaScript code is auto aligned*</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a><span data-ttu-id="fd584-374">Attività 2 - convalida JavaScript</span><span class="sxs-lookup"><span data-stu-id="fd584-374">Task 2 - Validating JavaScript</span></span>

<span data-ttu-id="fd584-375">In questa attività, sarà possibile osservare la convalida di JavaScript nuovi per lo standard di ECMAScript5.</span><span class="sxs-lookup"><span data-stu-id="fd584-375">In this task, you will discover the new JavaScript validation for the ECMAScript5 standard.</span></span> <span data-ttu-id="fd584-376">Questa funzionalità consente di scrivere codice JavaScript conforme, durante i problemi di scripting impedendo prima della distribuzione del sito.</span><span class="sxs-lookup"><span data-stu-id="fd584-376">This feature will help you to write compliant JavaScript code, while preventing scripting issues before site deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="fd584-377">Visual Studio 2010 implementate conformità ECMAStript3, mentre Visual Studio 2012 fornisce ECMAScript5 conformità.</span><span class="sxs-lookup"><span data-stu-id="fd584-377">Visual Studio 2010 implemented ECMAStript3 compliance, while Visual Studio 2012 provides ECMAScript5 compliance.</span></span>


1. <span data-ttu-id="fd584-378">Aprire **ECMA5script5.js** sotto il **Scripts\custom** cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="fd584-378">Open **ECMA5script5.js** located under the **Scripts\custom** project folder.</span></span> <span data-ttu-id="fd584-379">Verrà ora testato convalida per ECMAScript5 standard.</span><span class="sxs-lookup"><span data-stu-id="fd584-379">You will now test validation for ECMAScript5 standard.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    <span data-ttu-id="fd584-380">È possibile estrarre il &quot; **utilizzare strict** &quot; direzione nella prima riga del file, che consente di ECMAScript5 **modalità strict**.</span><span class="sxs-lookup"><span data-stu-id="fd584-380">You can check out the &quot; **use strict** &quot; direction in the first line of the file, which enables ECMAScript5 **strict mode**.</span></span> <span data-ttu-id="fd584-381">Questa modalità è composta da un sottoinsieme del linguaggio che si illustra le ambiguità dalla versione di precedente e aggiunge alcune nuove funzionalità, ad esempio getter e setter, supporto della libreria per JSON e più completa reflection alle proprietà dell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="fd584-381">This mode consists in a subset of the language that clarifies ambiguities from the past edition, and adds some new features, such as getters and setters, library support for JSON, and more complete reflection on object properties.</span></span>
2. <span data-ttu-id="fd584-382">Aprire il **elenco errori** se non è ancora aperto (**vista** menu | **Elenco errori**).</span><span class="sxs-lookup"><span data-stu-id="fd584-382">Open the **Error List** if not already opened (**View** menu | **Error List**).</span></span> <span data-ttu-id="fd584-383">Si noti il **funzione** dichiarazione è sottolineata.</span><span class="sxs-lookup"><span data-stu-id="fd584-383">Notice the **function** declaration is underlined.</span></span> <span data-ttu-id="fd584-384">Questo avviene perché in ECMA5 funzioni standard non possono essere annidate all'interno delle strutture di linguaggio.</span><span class="sxs-lookup"><span data-stu-id="fd584-384">This is because in ECMA5 standard functions cannot be nested inside language structures.</span></span> <span data-ttu-id="fd584-385">L'errore elenco riportato di seguito si visualizzeranno i dettagli dell'avviso.</span><span class="sxs-lookup"><span data-stu-id="fd584-385">In the error list below you will see the warning details.</span></span>

    <span data-ttu-id="fd584-386">![Messaggio di errore di convalida JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "messaggio di errore di convalida JavaScript")</span><span class="sxs-lookup"><span data-stu-id="fd584-386">![JavaScript validation error message](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript validation error message")</span></span>

    <span data-ttu-id="fd584-387">*Messaggio di errore di convalida JavaScript*</span><span class="sxs-lookup"><span data-stu-id="fd584-387">*JavaScript validation error message*</span></span>
3. <span data-ttu-id="fd584-388">Impostare come commento il **&quot;utilizzare strict&quot;** direzione e notare che scompaiono errori, ma vengono mantenuti gli avvisi.</span><span class="sxs-lookup"><span data-stu-id="fd584-388">Comment out the **&quot;use strict&quot;** direction and notice that errors disappear, but the warnings remain.</span></span>
4. <span data-ttu-id="fd584-389">Nell'ultima riga del file, scrivere qualsiasi stringa come **&quot;test&quot;** (che includono le virgolette per indicare che è sotto forma di stringa).</span><span class="sxs-lookup"><span data-stu-id="fd584-389">In the last line of the file, write any string like **&quot;test&quot;** (include the quotation marks to indicate it is as string).</span></span> <span data-ttu-id="fd584-390">Scrivere un periodo accanto la stringa da visualizzare l'elenco di IntelliSense e selezionare il **trim** opzione.</span><span class="sxs-lookup"><span data-stu-id="fd584-390">Write a period next to the string to display the IntelliSense list, and select the **trim** option.</span></span>

    <span data-ttu-id="fd584-391">Nello standard ECMAScript5, variabili e i valori stringa dispongono anche di metodi di stringa definiti, ad esempio trim, lettere maiuscole, ricerca e sostituzione.</span><span class="sxs-lookup"><span data-stu-id="fd584-391">In ECMAScript5 standard, string values and variables also have string methods defined, like trim, uppercase, search and replace.</span></span>

    <span data-ttu-id="fd584-392">![Elenco di IntelliSense in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "elenco IntelliSense in JavaScript")</span><span class="sxs-lookup"><span data-stu-id="fd584-392">![IntelliSense list in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "IntelliSense list in JavaScript")</span></span>

    <span data-ttu-id="fd584-393">*Elenco di IntelliSense in JavaScript*</span><span class="sxs-lookup"><span data-stu-id="fd584-393">*IntelliSense list in JavaScript*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a><span data-ttu-id="fd584-394">Attività 3 - documentazione XML per JavaScript</span><span class="sxs-lookup"><span data-stu-id="fd584-394">Task 3 - XML Documentation for JavaScript</span></span>

<span data-ttu-id="fd584-395">In questa attività verranno analizzate le funzionalità di Visual Studio per la documentazione XML in JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fd584-395">In this task, you will explore Visual Studio features for XML documentation in JavaScript.</span></span> <span data-ttu-id="fd584-396">Verrà visualizzato che l'elenco di JavaScript IntelliSense Mostra ora la documentazione XML di ogni funzione.</span><span class="sxs-lookup"><span data-stu-id="fd584-396">You will see the JavaScript IntelliSense list now shows the XML documentation of each function.</span></span> <span data-ttu-id="fd584-397">Inoltre, si noterà che la funzionalità di navigazione in JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fd584-397">Additionally, you will discover the navigation feature in JavaScript.</span></span>

1. <span data-ttu-id="fd584-398">Aprire **XMLDoc.js** file si trova in **script/personalizzato** cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="fd584-398">Open **XMLDoc.js** file located in **Scripts/custom** project folder.</span></span> <span data-ttu-id="fd584-399">Questo file contiene la documentazione XML in tutte le funzioni JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fd584-399">This file contains XML documentation on each of the JavaScript functions.</span></span>

    <span data-ttu-id="fd584-400">![Documentazione XML JavaScript integrato a IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML documentazione integrata in IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="fd584-400">![JavaScript XML documentation integrated to IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML documentation integrated to IntelliSense")</span></span>

    <span data-ttu-id="fd584-401">*Documentazione XML JavaScript integrata in IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="fd584-401">*JavaScript XML documentation integrated to IntelliSense*</span></span>
2. <span data-ttu-id="fd584-402">Di seguito **aggiungere** funzionare in **XMLDoc.js** file, creare una nuova funzione denominata **test**.</span><span class="sxs-lookup"><span data-stu-id="fd584-402">Below **add** function in **XMLDoc.js** file, create a new function named **test**.</span></span>
3. <span data-ttu-id="fd584-403">Nel **test** funzione, chiamare il **moltiplicare** funzione che riceve due parametri.</span><span class="sxs-lookup"><span data-stu-id="fd584-403">In the **test** function, call the **multiply** function that receives two parameters.</span></span> <span data-ttu-id="fd584-404">Si noti la casella della descrizione comando viene visualizzato il **moltiplicare** funzione documentazione.</span><span class="sxs-lookup"><span data-stu-id="fd584-404">Notice the tooltip box is showing the **multiply** function documentation.</span></span>

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    <span data-ttu-id="fd584-405">![Documentazione XML per le funzioni JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "documentazione XML per le funzioni JavaScript")</span><span class="sxs-lookup"><span data-stu-id="fd584-405">![XML documentation for JavaScript functions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "XML documentation for JavaScript functions")</span></span>

    <span data-ttu-id="fd584-406">*Documentazione XML per le funzioni JavaScript*</span><span class="sxs-lookup"><span data-stu-id="fd584-406">*XML documentation for JavaScript functions*</span></span>
4. <span data-ttu-id="fd584-407">Completare l'istruzione di chiamata di funzione e tipo di un *punto* per aprire l'elenco di IntelliSense nel valore restituito.</span><span class="sxs-lookup"><span data-stu-id="fd584-407">Complete the function call statement and type a *dot* to open the IntelliSense list on the returned value.</span></span> <span data-ttu-id="fd584-408">Si noti che il rilevamento di Visual Studio il **restituire** valore nella documentazione, considerando il valore come un numero.</span><span class="sxs-lookup"><span data-stu-id="fd584-408">Notice that Visual Studio is detecting the **return** value in the documentation, treating the value as a number.</span></span>

    <span data-ttu-id="fd584-409">![Documentazione XML per i tipi restituiti](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "documentazione XML per i tipi restituiti")</span><span class="sxs-lookup"><span data-stu-id="fd584-409">![XML documentation for return types](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "XML documentation for return types")</span></span>

    <span data-ttu-id="fd584-410">*Documentazione XML per i tipi restituiti*</span><span class="sxs-lookup"><span data-stu-id="fd584-410">*XML documentation for return types*</span></span>
5. <span data-ttu-id="fd584-411">A questo punto, inserire una chiamata di add (funzione).</span><span class="sxs-lookup"><span data-stu-id="fd584-411">Now, insert a call to add function.</span></span> <span data-ttu-id="fd584-412">Si noti che l'editor JavaScript supporta ora gli overload della funzione.</span><span class="sxs-lookup"><span data-stu-id="fd584-412">Notice that the JavaScript editor now supports function overloads.</span></span> <span data-ttu-id="fd584-413">Quando si scrive un nome di funzione, sarà possibile selezionare uno degli overload disponibili specificato nella documentazione.</span><span class="sxs-lookup"><span data-stu-id="fd584-413">When you write a function name, you will be able to select any of the available overloads specified in the documentation.</span></span>

    <span data-ttu-id="fd584-414">![Documentazione XML per gli overload](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "documentazione XML per gli overload")</span><span class="sxs-lookup"><span data-stu-id="fd584-414">![XML documentation for overloads](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "XML documentation for overloads")</span></span>

    <span data-ttu-id="fd584-415">*Documentazione XML per gli overload*</span><span class="sxs-lookup"><span data-stu-id="fd584-415">*XML documentation for overloads*</span></span>
6. <span data-ttu-id="fd584-416">Aprire **GotoDefinition.js** file e individuare il **$().html()** chiamata di funzione.</span><span class="sxs-lookup"><span data-stu-id="fd584-416">Open **GotoDefinition.js** file and locate the **$().html()** function call.</span></span> <span data-ttu-id="fd584-417">Posizionare il cursore su **html**.</span><span class="sxs-lookup"><span data-stu-id="fd584-417">Locate the cursor on **html**.</span></span>
7. <span data-ttu-id="fd584-418">Premere **F12** e passare alla definizione.</span><span class="sxs-lookup"><span data-stu-id="fd584-418">Press **F12** and navigate to the definition.</span></span> <span data-ttu-id="fd584-419">Nota è possibile accedere e individuare il codice JavaScript senza la **trovare** strumento.</span><span class="sxs-lookup"><span data-stu-id="fd584-419">Notice you can now access and browse your JavaScript code without using the **Find** tool.</span></span>
8. <span data-ttu-id="fd584-420">Posizionare il cursore sull'istruzione jQuery prima il blocco della firma nella parte inferiore del file di codice.</span><span class="sxs-lookup"><span data-stu-id="fd584-420">Locate the cursor on the jQuery instruction prior to the signature block at the bottom of the code file.</span></span> <span data-ttu-id="fd584-421">Premere **F12**.</span><span class="sxs-lookup"><span data-stu-id="fd584-421">Press **F12**.</span></span> <span data-ttu-id="fd584-422">Passare il file della libreria jQuery.</span><span class="sxs-lookup"><span data-stu-id="fd584-422">You will navigate to the jQuery library file.</span></span> <span data-ttu-id="fd584-423">Si noti è inoltre possibile navigare in tutti i file di jQuery utilizzando **F12**.</span><span class="sxs-lookup"><span data-stu-id="fd584-423">Notice you can also navigate across the jQuery files using **F12**.</span></span>

    <span data-ttu-id="fd584-424">![Passare alle definizioni di jQuery](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "spostarsi alle definizioni di jQuery")</span><span class="sxs-lookup"><span data-stu-id="fd584-424">![Navigating to jQuery definitions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "Navigating to jQuery definitions")</span></span>

    <span data-ttu-id="fd584-425">*Passare alle definizioni di jQuery*</span><span class="sxs-lookup"><span data-stu-id="fd584-425">*Navigating to jQuery definitions*</span></span>

> [!NOTE]
> <span data-ttu-id="fd584-426">Assicurarsi che GotoDefinition.js siano presenti errori di sintassi prima di salvare il file.</span><span class="sxs-lookup"><span data-stu-id="fd584-426">Make sure that GotoDefinition.js has no syntax errors before saving the file.</span></span>


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a><span data-ttu-id="fd584-427">Esercizio 4: Bundling and Minification</span><span class="sxs-lookup"><span data-stu-id="fd584-427">Exercise 4: Bundling and Minification</span></span>

<span data-ttu-id="fd584-428">Quante volte si i siti Web include più JavaScript o CSS file?</span><span class="sxs-lookup"><span data-stu-id="fd584-428">How many times do your websites include more than one JavaScript or CSS file?</span></span> <span data-ttu-id="fd584-429">Si tratta di uno scenario molto comune in cui aggregazione e minimizzazione consentono di ridurre le dimensioni del file e rendere il sito risultano più veloci.</span><span class="sxs-lookup"><span data-stu-id="fd584-429">This is a very common scenario where bundling and minification can help to reduce the file size and make the site perform faster.</span></span> <span data-ttu-id="fd584-430">La nuova funzionalità di aggregazione in ASP.NET 4.5 comprime un set di file JS o CSS in un singolo elemento e la riduzione del contenuto (ad esempio la rimozione degli spazi vuoti non necessari, rimozione di commenti, riducendo gli identificatori) riduce le dimensioni.</span><span class="sxs-lookup"><span data-stu-id="fd584-430">The new bundling feature in ASP.NET 4.5 packs a set of JS or CSS files into a single element, and reduces its size by minifying the content ( i.e. removing not required blank spaces, removing comments, reducing identifiers ).</span></span>

<span data-ttu-id="fd584-431">Come aggregare e riduzione in ASP.NET 4.5 viene eseguita in fase di esecuzione, in modo che il processo può identificare l'agente utente (ad esempio Internet Explorer, Mozilla e così via) e di conseguenza, migliorare la compressione includendo il browser utente (ad esempio, la rimozione ciò che è specifico di Mozilla Quando la richiesta proviene da Internet Explorer).</span><span class="sxs-lookup"><span data-stu-id="fd584-431">Bundling and minification in ASP.NET 4.5 is performed at runtime, so that the process can identify the user agent (for example IE, Mozilla, etc), and thus, improve the compression by targeting the user browser (for instance, removing stuff that is Mozilla specific when the request comes from IE).</span></span>

<span data-ttu-id="fd584-432">In questo esercizio, si apprenderà come abilitare e utilizzare i diversi tipi di aggregazione e riduzione in ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="fd584-432">In this exercise, you will learn how to enable and use the different types of bundling and minification in ASP.NET 4.5.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a><span data-ttu-id="fd584-433">Attività 1: installare l'aggregazione e minimizzazione pacchetto NuGet</span><span class="sxs-lookup"><span data-stu-id="fd584-433">Task 1 - Installing the Bundling and Minification Package from NuGet</span></span>

1. <span data-ttu-id="fd584-434">Se non è già aperto, avviare **Visual Studio** e aprire il **WhatsNewASPNET.sln** soluzione si trova nel **Source\WhatsNewASPNET** cartella di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="fd584-434">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="fd584-435">Aprire la Console di gestione pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="fd584-435">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="fd584-436">A tale scopo, utilizzare il menu **vista** | **altre finestre** | **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="fd584-436">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>

    <span data-ttu-id="fd584-437">![Apertura di file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole manager pacchetto](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "apertura della console di gestione pacchetti")</span><span class="sxs-lookup"><span data-stu-id="fd584-437">![Opening the package manager file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Opening the package manager console")</span></span>

    <span data-ttu-id="fd584-438">*Aprire la console di gestione pacchetti*</span><span class="sxs-lookup"><span data-stu-id="fd584-438">*Opening the package manager console*</span></span>
3. <span data-ttu-id="fd584-439">Nel **Console di gestione pacchetti,** tipo **Install-Package Microsoft.Web.Optimization** e premere **invio**.</span><span class="sxs-lookup"><span data-stu-id="fd584-439">In the **Package Manager Console,** type **Install-Package Microsoft.Web.Optimization** and press **ENTER**.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a><span data-ttu-id="fd584-440">Attività 2: Bundle predefinita</span><span class="sxs-lookup"><span data-stu-id="fd584-440">Task 2 - Default Bundles</span></span>

<span data-ttu-id="fd584-441">Il modo più semplice da utilizzare come aggregare e riduzione è consentire di bundle predefinita.</span><span class="sxs-lookup"><span data-stu-id="fd584-441">The simplest way to use bundling and minification is to enable the default bundles.</span></span> <span data-ttu-id="fd584-442">Questo metodo utilizza le convenzioni per consentono di fare riferimento alla versione minimizzata e aggregata per i file JS e CSS in una cartella.</span><span class="sxs-lookup"><span data-stu-id="fd584-442">This method uses conventions to let you reference the bundled and minified version for the JS and CSS files in a folder.</span></span>

<span data-ttu-id="fd584-443">In questa attività si apprenderà come abilitare e fare riferimento ai file in bundle e minimizzati JS e CSS e visualizzare l'output risultante.</span><span class="sxs-lookup"><span data-stu-id="fd584-443">In this task, you will learn how to enable and reference the bundled and minified JS and CSS files and view the resulting output.</span></span>

1. <span data-ttu-id="fd584-444">Se non è già aperto, avviare **Visual Studio** e aprire il **WhatsNewASPNET.sln** soluzione si trova nel **Source\WhatsNewASPNET** cartella di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="fd584-444">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="fd584-445">Nel **Esplora**, espandere il **stili**, **Scripts\custom** e **Scripts\bundle** cartelle.</span><span class="sxs-lookup"><span data-stu-id="fd584-445">In the **Solution Explorer**, expand the **Styles**, **Scripts\custom** and **Scripts\bundle** folders.</span></span>

    <span data-ttu-id="fd584-446">Si noti che l'applicazione utilizza i file più CSS e JS.</span><span class="sxs-lookup"><span data-stu-id="fd584-446">Notice that the application is using more than one CSS and JS file.</span></span>

    <span data-ttu-id="fd584-447">![I file più fogli di stile e JavaScript nell'applicazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "più fogli di stile e JavaScript file nell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="fd584-447">![Multiple Stylesheets and JavaScript files in the application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "Multiple Stylesheets and JavaScript files in the application")</span></span>

    <span data-ttu-id="fd584-448">*Più file di JavaScript e fogli di stile nell'applicazione*</span><span class="sxs-lookup"><span data-stu-id="fd584-448">*Multiple Stylesheets and JavaScript files in the application*</span></span>
3. <span data-ttu-id="fd584-449">Aprire il **Global.asax.cs** file.</span><span class="sxs-lookup"><span data-stu-id="fd584-449">Open the **Global.asax.cs** file.</span></span>

    <span data-ttu-id="fd584-450">Si noti che il nuovo **Microsoft.Web.Optimization** dello spazio dei nomi viene impostata come commento all'inizio del file.</span><span class="sxs-lookup"><span data-stu-id="fd584-450">Notice that the new **Microsoft.Web.Optimization** namespace is commented out at the beginning of the file.</span></span> <span data-ttu-id="fd584-451">Rimuovere il commento di utilizzando una direttiva per includere le caratteristiche di aggregazione e la riduzione.</span><span class="sxs-lookup"><span data-stu-id="fd584-451">Uncomment the using directive to include the bundling and minification features.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
~~~
4. <span data-ttu-id="fd584-452">Individuare il **applicazione\_avviare** metodo.</span><span class="sxs-lookup"><span data-stu-id="fd584-452">Locate the **Application\_Start** method.</span></span>

    <span data-ttu-id="fd584-453">In questo metodo, rimuovere il commento la chiamata di EnableDefaultBundles come illustrato nel frammento riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="fd584-453">In this method, uncomment the EnableDefaultBundles call as shown in the snippet below.</span></span> <span data-ttu-id="fd584-454">Ciò consente di fare riferimento a una raccolta di bundle di file CSS in una cartella utilizzando il percorso alla cartella, più il &quot;CSS&quot; o &quot;JS&quot; suffisso.</span><span class="sxs-lookup"><span data-stu-id="fd584-454">This enables us to reference a bundled collection of CSS files in a folder by using the path to that folder, plus the &quot;CSS&quot; or the &quot;JS&quot; suffix.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
~~~
5. <span data-ttu-id="fd584-455">Aprire il **Optimization.aspx** file e individuare il controllo contenuto di **HeadContent**.</span><span class="sxs-lookup"><span data-stu-id="fd584-455">Open the **Optimization.aspx** file and locate the content control for **HeadContent**.</span></span>

    <span data-ttu-id="fd584-456">Si noti i file CSS e i file JS presentare un unico tag di cui si fa riferimento.</span><span class="sxs-lookup"><span data-stu-id="fd584-456">Notice the CSS files and the JS files have a single referenced tag.</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

> [!NOTE]
> This code is for demo purposes. Ideally, you will reference the bundles in the Site.Master file. In this sample code, you will find that some of the bundled files are also being referenced by the Site.Master file, making this last reference redundant.
~~~
6. <span data-ttu-id="fd584-457">Si noti che i collegamenti utilizza le convenzioni di aggregazione di **href** attributo per ottenere i file di tutti i CSS o JS dagli stili e Scripts\custom cartella rispettivamente.</span><span class="sxs-lookup"><span data-stu-id="fd584-457">Notice that the links are using the bundling conventions in the **href** attribute to get all the CSS or JS files from the Styles and Scripts\custom folder respectively.</span></span>

    <span data-ttu-id="fd584-458">È possibile utilizzare il percorso **personalizzato/script/JS** come illustrato di seguito per aggregare e minimizzare tutti i file JS all'interno di un **script/personalizzato** cartella.</span><span class="sxs-lookup"><span data-stu-id="fd584-458">You can use the path **Scripts/custom/JS** as shown below to bundle and minify all the JS files inside a **Scripts/custom** folder.</span></span> <span data-ttu-id="fd584-459">Questo è il comportamento predefinito con i bundle predefinita.</span><span class="sxs-lookup"><span data-stu-id="fd584-459">This is the default behavior with the default bundles.</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
~~~
7. <span data-ttu-id="fd584-460">Aprire il **Styles\Site.css** file.</span><span class="sxs-lookup"><span data-stu-id="fd584-460">Open the **Styles\Site.css** file.</span></span>

    <span data-ttu-id="fd584-461">Si noti che il file CSS originale contiene rientro di codice, spazi vuoti e commenti che ampliare il file.</span><span class="sxs-lookup"><span data-stu-id="fd584-461">Notice that the original CSS file contains indented code, blank spaces and comments that enlarge the file.</span></span> <span data-ttu-id="fd584-462">(Anche il file JavaScript contiene spazi vuoti e commenti).</span><span class="sxs-lookup"><span data-stu-id="fd584-462">(Also the JavaScript file contains blank spaces and comments).</span></span>

    <span data-ttu-id="fd584-463">![Uno dei CSS originale file nella cartella Scripts](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "uno del foglio di stile CSS originale file nella cartella Scripts")</span><span class="sxs-lookup"><span data-stu-id="fd584-463">![One of the original CSS files in the Scripts folder](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "One of the original CSS files in the Scripts folder")</span></span>

    <span data-ttu-id="fd584-464">*Uno dei file CSS originali nella cartella Scripts*</span><span class="sxs-lookup"><span data-stu-id="fd584-464">*One of the original CSS files in the Scripts folder*</span></span>
8. <span data-ttu-id="fd584-465">Premere **F5** per eseguire l'applicazione e spostarsi tra le **ottimizzazione** pagina.</span><span class="sxs-lookup"><span data-stu-id="fd584-465">Press **F5** to run the application and navigate to the **Optimization** page.</span></span>
9. <span data-ttu-id="fd584-466">Fare clic su di **Bundle CSS** collegamento per scaricare e aprire il file.</span><span class="sxs-lookup"><span data-stu-id="fd584-466">Click on the **CSS Bundle** link to download and open the file.</span></span>

    <span data-ttu-id="fd584-467">Estrarre il file aggregato minimizzato.</span><span class="sxs-lookup"><span data-stu-id="fd584-467">Check out the minified bundled file.</span></span> <span data-ttu-id="fd584-468">Si noterà che sono stati rimossi gli spazi vuoti, commenti e i caratteri di rientro, la generazione di un file più piccolo.</span><span class="sxs-lookup"><span data-stu-id="fd584-468">You will notice that all the blank spaces, comments and indentation characters have been removed, generating a smaller file.</span></span>

    <span data-ttu-id="fd584-469">![I file CSS di bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "file CSS in bundle")</span><span class="sxs-lookup"><span data-stu-id="fd584-469">![Bundled CSS files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "Bundled CSS files")</span></span>

    <span data-ttu-id="fd584-470">*File di bundle CSS*</span><span class="sxs-lookup"><span data-stu-id="fd584-470">*Bundled CSS files*</span></span>
10. <span data-ttu-id="fd584-471">Fare clic su di **Bundle JS** collegamento per aprire il file JavaScript in bundle.</span><span class="sxs-lookup"><span data-stu-id="fd584-471">Now click the **JS Bundle** link to open the JavaScript bundled file.</span></span> <span data-ttu-id="fd584-472">È possibile ignorare l'avviso di explorer.</span><span class="sxs-lookup"><span data-stu-id="fd584-472">You can safely disregard the explorer warning.</span></span> <span data-ttu-id="fd584-473">Notare i file JavaScript sotto il **personalizzato** cartella sono anche in bundle e minimizzare.</span><span class="sxs-lookup"><span data-stu-id="fd584-473">Notice the JavaScript files under the **custom** folder are also bundled and minified.</span></span>

    <span data-ttu-id="fd584-474">![I file JavaScript di bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "file JavaScript in bundle")</span><span class="sxs-lookup"><span data-stu-id="fd584-474">![Bundled JavaScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "Bundled JavaScript files")</span></span>

    <span data-ttu-id="fd584-475">*File di bundle JavaScript*</span><span class="sxs-lookup"><span data-stu-id="fd584-475">*Bundled JavaScript files*</span></span>

    <span data-ttu-id="fd584-476">L'abilitazione della compressione per i file JS o CSS è molto più complessa in una versione precedente di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fd584-476">Enabling compression for CSS or JS files was much more complicated in previous ASP.NET version.</span></span> <span data-ttu-id="fd584-477">A questo punto, come si è visto, è sufficiente aggiungere una riga di *Global. asax* per abilitare la creazione di bundle di file e quindi fare riferimento ai file in dotazione dal sito.</span><span class="sxs-lookup"><span data-stu-id="fd584-477">Now, as you have seen, you just need to add one line in the *Global.asax* file to enable bundling, and then reference the bundled files from your site.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a><span data-ttu-id="fd584-478">Attività 3 - Bundle statici</span><span class="sxs-lookup"><span data-stu-id="fd584-478">Task 3 - Static Bundles</span></span>

<span data-ttu-id="fd584-479">L'approccio bundle statico consente di personalizzare il set di file di bundle, il riferimento e il metodo minimizzazione che verrà utilizzato.</span><span class="sxs-lookup"><span data-stu-id="fd584-479">The static bundle approach allows you to customize the set of files to bundle, the reference and the minification method that will be used.</span></span>

<span data-ttu-id="fd584-480">In questa attività si configurerà un bundle statico per definire un set specifico di file da aggregare e minimizzare.</span><span class="sxs-lookup"><span data-stu-id="fd584-480">In this task, you will configure a static bundle to define a specific set of files to bundle and minify.</span></span>

1. <span data-ttu-id="fd584-481">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="fd584-481">Close the browser.</span></span>
2. <span data-ttu-id="fd584-482">Aprire il **Global.asax.cs** file e individuare il **applicazione\_avviare** metodo.</span><span class="sxs-lookup"><span data-stu-id="fd584-482">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
3. <span data-ttu-id="fd584-483">Rimuovere il commento il codice di aggregazione statici come illustrato nel codice seguente.</span><span class="sxs-lookup"><span data-stu-id="fd584-483">Uncomment the static bundle code as shown in the code below.</span></span>

    <span data-ttu-id="fd584-484">Si sta definendo un raggruppamento statico che verrà fatto riferimento con il &quot; **~/StaticBundle** &quot; percorso virtuale e utilizzare **JsMinify** per la riduzione di tutti i file specificati con il **AddFile** metodo.</span><span class="sxs-lookup"><span data-stu-id="fd584-484">You are defining a static bundle that will be referenced with the &quot;**~/StaticBundle**&quot; virtual path and use **JsMinify** for minification of all the specified files with the **AddFile** method.</span></span> <span data-ttu-id="fd584-485">Infine, si aggiunge il bundle statico per il **BundleTable** e abilitarlo.</span><span class="sxs-lookup"><span data-stu-id="fd584-485">Finally, you are adding the static bundle to the **BundleTable** and enabling it.</span></span>

    <span data-ttu-id="fd584-486">Si noti che i file non si trovano nella stessa posizione; si tratta di un altro vantaggio rispetto l'aggregazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="fd584-486">Notice that the files are not located in the same place; this is another advantage over the default bundling.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
~~~
4. <span data-ttu-id="fd584-487">Aprire il **Optimization.aspx** file.</span><span class="sxs-lookup"><span data-stu-id="fd584-487">Open the **Optimization.aspx** file.</span></span>

    <span data-ttu-id="fd584-488">Si noti che il collegamento a **statico Bundle JS** sta utilizzando il percorso in cui è stato dichiarato quando è stato configurato il bundle statico nel file Global.asax.cs: **/StaticBundle**.</span><span class="sxs-lookup"><span data-stu-id="fd584-488">Notice that the link to **Static JS Bundle** is using the path you have declared when you configured the static bundle in the Global.asax.cs file: **/StaticBundle**.</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
~~~
5. <span data-ttu-id="fd584-489">Premere **F5** per eseguire l'applicazione e quindi passare il **ottimizzazione** pagina.</span><span class="sxs-lookup"><span data-stu-id="fd584-489">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
6. <span data-ttu-id="fd584-490">Fare clic su di **statico Bundle JS** collegamento per aprire il file.</span><span class="sxs-lookup"><span data-stu-id="fd584-490">Click on the **Static JS Bundle** link to open the file.</span></span>

    <span data-ttu-id="fd584-491">Si noti che il minimizzata in bundle file JavaScript sono riportato l'output per tutti i file JavaScript configurato nel file di bundle statico nel percorso &quot;/StaticBundle&quot;.</span><span class="sxs-lookup"><span data-stu-id="fd584-491">Notice that the minified bundled JavaScript file is the output for all the JavaScript files configured in the static bundle file under the path &quot;/StaticBundle&quot;.</span></span>

    <span data-ttu-id="fd584-492">![Bundle di file statici JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "bundle di file JavaScript statico")</span><span class="sxs-lookup"><span data-stu-id="fd584-492">![Static JavaScript files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "Static JavaScript files bundle")</span></span>

    <span data-ttu-id="fd584-493">*Aggregano i file JavaScript statici*</span><span class="sxs-lookup"><span data-stu-id="fd584-493">*Static JavaScript files bundle*</span></span>
7. <span data-ttu-id="fd584-494">Chiudere il browser e tornare a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fd584-494">Close the browser and return to Visual Studio.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a><span data-ttu-id="fd584-495">Attività 4: Bundle cartella dinamica</span><span class="sxs-lookup"><span data-stu-id="fd584-495">Task 4 - Dynamic Folder Bundles</span></span>

<span data-ttu-id="fd584-496">In questa attività si apprenderà come configurare bundle cartella dinamica.</span><span class="sxs-lookup"><span data-stu-id="fd584-496">In this task, you will learn how to configure dynamic folder bundles.</span></span> <span data-ttu-id="fd584-497">La potenza di aggregazione dinamica è che possibile includere statico JavaScript, nonché altri file in linguaggi che viene compilato in JavaScript e pertanto richiedono alcune operazioni di elaborazione prima che venga eseguita l'aggregazione.</span><span class="sxs-lookup"><span data-stu-id="fd584-497">The power of dynamic bundling is that you can include static JavaScript, as well as other files in languages that compiles into JavaScript, and thus, require some processing before the bundling is executed.</span></span>

<span data-ttu-id="fd584-498">In questo esempio, si apprenderà come usare il **DynamicFolderBundle** classe per creare un bundle dinamico per i file scritti in CofeeScript.</span><span class="sxs-lookup"><span data-stu-id="fd584-498">In this example, you will learn how to use the **DynamicFolderBundle** class to create a dynamic bundle for files written in CofeeScript.</span></span> <span data-ttu-id="fd584-499">CofeeScript è un linguaggio di programmazione che viene compilato in JavaScript e fornisce una sintassi più semplice per la scrittura di codice JavaScript, migliorando la leggibilità e brevità JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fd584-499">CofeeScript is a programming language that compiles into JavaScript and provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability.</span></span>

1. <span data-ttu-id="fd584-500">Aprire il **Global.asax.cs** file e individuare il **applicazione\_avviare** metodo.</span><span class="sxs-lookup"><span data-stu-id="fd584-500">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
2. <span data-ttu-id="fd584-501">Rimuovere il commento di codice dinamico bundle come illustrato nel codice seguente.</span><span class="sxs-lookup"><span data-stu-id="fd584-501">Uncomment the dynamic bundle code as shown in the code below.</span></span>

    <span data-ttu-id="fd584-502">Si sta definendo un bundle della cartella dinamica che utilizzerà il **CoffeeMinify** processore minimizzazione personalizzato che si applica solo ai file con il &quot; **.coffee** &quot; (estensione CoffeeScript file).</span><span class="sxs-lookup"><span data-stu-id="fd584-502">You are defining a dynamic folder bundle that will use the **CoffeeMinify** custom minification processor that will only apply to the files with the &quot;**.coffee**&quot; extension (CoffeeScript files).</span></span> <span data-ttu-id="fd584-503">Si noti che è possibile utilizzare un criterio di ricerca per selezionare i file di bundle all'interno di una cartella, ad esempio '\*.coffee'.</span><span class="sxs-lookup"><span data-stu-id="fd584-503">Notice that you can use a search pattern to select the files to bundle within a folder, like '\*.coffee'.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
~~~
3. <span data-ttu-id="fd584-504">Aprire la Console di gestione pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="fd584-504">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="fd584-505">A tale scopo, utilizzare il menu **vista** | **altre finestre** | **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="fd584-505">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>
4. <span data-ttu-id="fd584-506">Nel **Console di gestione pacchetti,** tipo **Install-Package CoffeeSharp** e premere **invio**.</span><span class="sxs-lookup"><span data-stu-id="fd584-506">In the **Package Manager Console,** type **Install-Package CoffeeSharp** and press **ENTER**.</span></span>
5. <span data-ttu-id="fd584-507">Fare clic su di **Mostra tutti i file** pulsante il **Esplora** finestra</span><span class="sxs-lookup"><span data-stu-id="fd584-507">Click the **Show All Files** button in the **Solution Explorer** window</span></span>

    <span data-ttu-id="fd584-508">![Visualizzazione di tutti i file](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "che mostra tutti i file")</span><span class="sxs-lookup"><span data-stu-id="fd584-508">![Showing all files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "Showing all files")</span></span>

    <span data-ttu-id="fd584-509">*Visualizzazione di tutti i file*</span><span class="sxs-lookup"><span data-stu-id="fd584-509">*Showing all files*</span></span>
6. <span data-ttu-id="fd584-510">Fare clic con il pulsante destro il **CoffeeMinify.cs** del file nel **Esplora** e selezionare **Includi nel progetto**</span><span class="sxs-lookup"><span data-stu-id="fd584-510">Right click the **CoffeeMinify.cs** file in the **Solution Explorer** and select **Include in Project**</span></span>

    <span data-ttu-id="fd584-511">![Includere il file CoffeeMinify.cs nel progetto](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "includere il file CoffeeMinify.cs nel progetto")</span><span class="sxs-lookup"><span data-stu-id="fd584-511">![Include the CoffeeMinify.cs file in the project](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "Include the CoffeeMinify.cs file in the project")</span></span>

    <span data-ttu-id="fd584-512">*Includere il file CoffeeMinify.cs nel progetto*</span><span class="sxs-lookup"><span data-stu-id="fd584-512">*Include the CoffeeMinify.cs file in the project*</span></span>
7. <span data-ttu-id="fd584-513">Aprire il **CoffeeMinify.cs** file.</span><span class="sxs-lookup"><span data-stu-id="fd584-513">Open the **CoffeeMinify.cs** file.</span></span>

    <span data-ttu-id="fd584-514">Questa classe eredita da JsMinify da minimizzare l'output JavaScript risultante dalla compilazione di codice CoffeeScript.</span><span class="sxs-lookup"><span data-stu-id="fd584-514">This class inherits from JsMinify to minify the JavaScript output resulting from the CoffeeScript code compilation.</span></span> <span data-ttu-id="fd584-515">Chiama il compilatore di CoffeeScript per generare il codice JavaScript prima e quindi invia al metodo JsMinify.Process per minimizzare il codice risultante.</span><span class="sxs-lookup"><span data-stu-id="fd584-515">It calls the CoffeeScript compiler to generate the JavaScript code first, and then it sends it to the JsMinify.Process method to minify the resulting code.</span></span>


~~~
[!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
~~~
8. <span data-ttu-id="fd584-516">Aprire il **Script1.coffee** e **Script2.coffee** i file dal **/bundle di script** cartella.</span><span class="sxs-lookup"><span data-stu-id="fd584-516">Open the **Script1.coffee** and **Script2.coffee** files from the **Scripts/bundle** folder.</span></span>

    <span data-ttu-id="fd584-517">Questi file includerà il codice CoffeScript da compilare durante l'esecuzione di aggregazione con la classe CoffeeMinify.</span><span class="sxs-lookup"><span data-stu-id="fd584-517">These files will include the CoffeScript code to be compiled while performing the bundling with the CoffeeMinify class.</span></span>

    <span data-ttu-id="fd584-518">Per motivi di semplicità, file CoffeeScript forniti sono inclusi solo il codice di esempio CoffeeScript.</span><span class="sxs-lookup"><span data-stu-id="fd584-518">For simplicity purposes, the CoffeeScript files provided are only including CoffeeScript sample code.</span></span> <span data-ttu-id="fd584-519">I commenti vengono esclusi dal processo di JsMinify.</span><span class="sxs-lookup"><span data-stu-id="fd584-519">The comments are excluded by the JsMinify process.</span></span>

    <span data-ttu-id="fd584-520">![File CoffeeScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "file CoffeeScript")</span><span class="sxs-lookup"><span data-stu-id="fd584-520">![CoffeeScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript files")</span></span>

    <span data-ttu-id="fd584-521">*File CoffeeScript*</span><span class="sxs-lookup"><span data-stu-id="fd584-521">*CoffeeScript files*</span></span>

    > [!NOTE]
    > <span data-ttu-id="fd584-522">[CofeeScript](https://github.com/jashkenas/coffeescript/) fornisce una sintassi più semplice per la scrittura di codice JavaScript, migliorando la leggibilità e brevità JavaScript, nonché l'aggiunta di altre funzionalità come matrice comprensione e criteri di ricerca.</span><span class="sxs-lookup"><span data-stu-id="fd584-522">[CofeeScript](https://github.com/jashkenas/coffeescript/) provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability, as well as adding other features like array comprehension and pattern matching.</span></span>
9. <span data-ttu-id="fd584-523">Aprire il **Optimization.aspx** file e individuare i collegamenti di bundle.</span><span class="sxs-lookup"><span data-stu-id="fd584-523">Open the **Optimization.aspx** file and locate the bundle links.</span></span>

    <span data-ttu-id="fd584-524">Si noti che il collegamento a **dinamica Bundle JS** fa riferimento il **/bundle di script** cartella utilizzando il **/caffè** suffisso è configurato per il bundle della cartella dinamica.</span><span class="sxs-lookup"><span data-stu-id="fd584-524">Notice that the link to **Dynamic JS Bundle** is referencing the **Scripts/bundle** folder by using the **/Coffee** suffix you configured for the dynamic folder bundle.</span></span>


~~~
[!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
~~~
10. <span data-ttu-id="fd584-525">Premere **F5** per eseguire l'applicazione e quindi passare il **ottimizzazione** pagina.</span><span class="sxs-lookup"><span data-stu-id="fd584-525">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
11. <span data-ttu-id="fd584-526">Fare clic su di **dinamica Bundle JS** collegamento per aprire il file generato.</span><span class="sxs-lookup"><span data-stu-id="fd584-526">Click on the **Dynamic JS Bundle** link to open the generated file.</span></span>

    <span data-ttu-id="fd584-527">Si noti che il contenuto che è stato incluso in questo pacchetto contiene solo **.coffee** file.</span><span class="sxs-lookup"><span data-stu-id="fd584-527">Notice that the content that was included in this bundle only contains **.coffee** files.</span></span> <span data-ttu-id="fd584-528">È anche possibile vedere che il codice di CoffeeScript è stato compilato in JavaScript e le righe di commento è stato rimosso.</span><span class="sxs-lookup"><span data-stu-id="fd584-528">You can also see that the CoffeeScript code was compiled to JavaScript and the commented-out lines has been removed.</span></span>

    <span data-ttu-id="fd584-529">![Aggregano il file JS dinamico](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "bundle file JS dinamica")</span><span class="sxs-lookup"><span data-stu-id="fd584-529">![Dynamic JS files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "Dynamic JS files bundle")</span></span>

    <span data-ttu-id="fd584-530">*Aggregano il file JS dinamico*</span><span class="sxs-lookup"><span data-stu-id="fd584-530">*Dynamic JS files bundle*</span></span>

> [!NOTE]
> <span data-ttu-id="fd584-531">Inoltre, è possibile distribuire l'applicazione per siti Web di Azure seguenti [pubblicazione appendice b: un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="fd584-531">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="fd584-532">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="fd584-532">Summary</span></span>

<span data-ttu-id="fd584-533">Questa esercitazione consente di comprendere cosa è nuovo in ASP.NET e lo sviluppo Web in Visual Studio 2012 e come sfruttare i numerosi miglioramenti a livello di Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="fd584-533">This lab helps you to understand what New in ASP.NET and Web Development in Visual Studio 2012 is and how to take advantage of the variety of enhancements in Visual Studio 2012.</span></span>

<span data-ttu-id="fd584-534">Completando questa pratica, apprese illustrato come utilizzare le nuove funzionalità e miglioramenti nell'editor di Visual Studio 2012 per CSS, JavaScript e HTML.</span><span class="sxs-lookup"><span data-stu-id="fd584-534">By completing this Hands-On Lab, you have learnt how to use the new features and improvements in Visual Studio 2012 Editors for CSS, JavaScript and HTML.</span></span> <span data-ttu-id="fd584-535">Inoltre, apprese la modalità di implementazione di aggregazione incorporata e riduzione in Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="fd584-535">In addition, you have learnt how Visual Studio 2012 implements built-in bundling and minification.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="fd584-536">Appendice a: installazione di Visual Studio Express 2012 per Web</span><span class="sxs-lookup"><span data-stu-id="fd584-536">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="fd584-537">È possibile installare **Microsoft Visual Studio Express 2012 per Web** o un altro &quot;Express&quot; versione utilizzando il **[installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="fd584-537">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="fd584-538">Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web di Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="fd584-538">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="fd584-539">Passare a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="fd584-539">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="fd584-540">In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="fd584-540">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="fd584-541">Fare clic su **installa**.</span><span class="sxs-lookup"><span data-stu-id="fd584-541">Click on **Install Now**.</span></span> <span data-ttu-id="fd584-542">Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.</span><span class="sxs-lookup"><span data-stu-id="fd584-542">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="fd584-543">Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="fd584-543">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="fd584-544">![Installa Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="fd584-544">![Install Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="fd584-545">*Installa Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="fd584-545">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="fd584-546">Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="fd584-546">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettare le condizioni di licenza](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    <span data-ttu-id="fd584-548">*Accettare le condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="fd584-548">*Accepting the license terms*</span></span>
5. <span data-ttu-id="fd584-549">Attendere finché non viene completato il processo di download e l'installazione.</span><span class="sxs-lookup"><span data-stu-id="fd584-549">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    <span data-ttu-id="fd584-551">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="fd584-551">*Installation progress*</span></span>
6. <span data-ttu-id="fd584-552">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="fd584-552">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    <span data-ttu-id="fd584-554">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="fd584-554">*Installation completed*</span></span>
7. <span data-ttu-id="fd584-555">Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="fd584-555">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="fd584-556">Per aprire Visual Studio Express per Web, passare al **avviare** schermata e iniziare la scrittura &quot; **VS Express**&quot;, quindi fare clic su di **Visual Studio Express per Web** il riquadro.</span><span class="sxs-lookup"><span data-stu-id="fd584-556">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express per il riquadro Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    <span data-ttu-id="fd584-558">*VS Express per il riquadro Web*</span><span class="sxs-lookup"><span data-stu-id="fd584-558">*VS Express for Web tile*</span></span>

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="fd584-559">Appendice b: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="fd584-559">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="fd584-560">Questa appendice mostra come creare un nuovo sito web dal portale di gestione di Windows Azure e pubblicare l'applicazione, ottenute seguendo il lab, sfruttando la funzionalità di pubblicazione distribuzione Web fornita da Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="fd584-560">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="fd584-561">Attività 1 - Creazione di un nuovo sito Web da Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fd584-561">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="fd584-562">Passare al [il portale di gestione di Windows Azure](https://manage.windowsazure.com/) e accedere utilizzando le credenziali di Microsoft associate alla sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="fd584-562">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="fd584-563">Con Windows Azure è possibile ospitare gratuito di 10 siti Web ASP.NET e quindi aumentare in base al traffico.</span><span class="sxs-lookup"><span data-stu-id="fd584-563">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="fd584-564">È possibile iscriversi [qui](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="fd584-564">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="fd584-565">![Accedere al portale Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "accedere al portale Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="fd584-565">![Log on to Windows Azure portal](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="fd584-566">*Accedere al portale di gestione di Azure*</span><span class="sxs-lookup"><span data-stu-id="fd584-566">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="fd584-567">Fare clic su **nuovo** sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="fd584-567">Click **New** on the command bar.</span></span>

    <span data-ttu-id="fd584-568">![Creazione di un nuovo sito Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "creando un nuovo sito Web")</span><span class="sxs-lookup"><span data-stu-id="fd584-568">![Creating a new Web Site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="fd584-569">*Creazione di un nuovo sito Web*</span><span class="sxs-lookup"><span data-stu-id="fd584-569">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="fd584-570">Fare clic su **calcolo** | **sito Web**.</span><span class="sxs-lookup"><span data-stu-id="fd584-570">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="fd584-571">Selezionare quindi **creazione rapida** opzione.</span><span class="sxs-lookup"><span data-stu-id="fd584-571">Then select **Quick Create** option.</span></span> <span data-ttu-id="fd584-572">Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.</span><span class="sxs-lookup"><span data-stu-id="fd584-572">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="fd584-573">Un sito Web di Windows Azure è l'host per un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire.</span><span class="sxs-lookup"><span data-stu-id="fd584-573">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="fd584-574">L'opzione Creazione rapida consente di distribuire un'applicazione web completa per il sito Web Microsoft Azure dall'esterno al portale.</span><span class="sxs-lookup"><span data-stu-id="fd584-574">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="fd584-575">Non include i passaggi per configurare un database.</span><span class="sxs-lookup"><span data-stu-id="fd584-575">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="fd584-576">![Creazione di un nuovo sito Web utilizzando Creazione rapida](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "creando un nuovo sito Web utilizzando Creazione rapida")</span><span class="sxs-lookup"><span data-stu-id="fd584-576">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="fd584-577">*Creazione di un nuovo sito Web utilizzando Creazione rapida*</span><span class="sxs-lookup"><span data-stu-id="fd584-577">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="fd584-578">Attendere finché il nuovo **sito Web** viene creato.</span><span class="sxs-lookup"><span data-stu-id="fd584-578">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="fd584-579">Una volta creato il sito Web, fare clic sul collegamento sotto il **URL** colonna.</span><span class="sxs-lookup"><span data-stu-id="fd584-579">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="fd584-580">Verificare il funzionamento del nuovo sito Web.</span><span class="sxs-lookup"><span data-stu-id="fd584-580">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="fd584-581">![Esplorazione per il nuovo sito web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "esplorazione per il nuovo sito web")</span><span class="sxs-lookup"><span data-stu-id="fd584-581">![Browsing to the new web site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="fd584-582">*Esplorazione per il nuovo sito web*</span><span class="sxs-lookup"><span data-stu-id="fd584-582">*Browsing to the new web site*</span></span>

    <span data-ttu-id="fd584-583">![Sito Web in esecuzione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "sito Web in esecuzione")</span><span class="sxs-lookup"><span data-stu-id="fd584-583">![Web site running](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "Web site running")</span></span>

    <span data-ttu-id="fd584-584">*Sito Web in esecuzione*</span><span class="sxs-lookup"><span data-stu-id="fd584-584">*Web site running*</span></span>
6. <span data-ttu-id="fd584-585">Tornare al portale e fare clic sul nome del sito web sotto il **nome** colonna per visualizzare le pagine di gestione.</span><span class="sxs-lookup"><span data-stu-id="fd584-585">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="fd584-586">![Aprire le pagine di gestione del sito web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "apertura delle pagine di gestione del sito web")</span><span class="sxs-lookup"><span data-stu-id="fd584-586">![Opening the web site management pages](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="fd584-587">*Aprire le pagine di gestione del sito Web*</span><span class="sxs-lookup"><span data-stu-id="fd584-587">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="fd584-588">Nel **Dashboard** nella pagina di **riepilogo rapido** fare clic su di **Scarica profilo di pubblicazione** collegamento.</span><span class="sxs-lookup"><span data-stu-id="fd584-588">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="fd584-589">Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in un sito Web di Windows Azure per ogni metodo di pubblicazione abilitato.</span><span class="sxs-lookup"><span data-stu-id="fd584-589">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="fd584-590">Il profilo di pubblicazione contiene gli URL, le credenziali dell'utente e le stringhe di database necessari per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="fd584-590">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="fd584-591">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di queste applicazioni per pubblicazione di applicazioni web per siti Web di Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="fd584-591">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="fd584-592">![Profilo di pubblicazione del sito web di download](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "profilo di pubblicazione del sito web di download")</span><span class="sxs-lookup"><span data-stu-id="fd584-592">![Downloading the web site publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="fd584-593">*Profilo di pubblicazione del sito Web di download*</span><span class="sxs-lookup"><span data-stu-id="fd584-593">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="fd584-594">Scaricare il file del profilo di pubblicazione in un percorso noto.</span><span class="sxs-lookup"><span data-stu-id="fd584-594">Download the publish profile file to a known location.</span></span> <span data-ttu-id="fd584-595">In questo esercizio si ulteriormente verrà visualizzato come utilizzare questo file per pubblicare un'applicazione web a un siti Web di Windows Azure da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fd584-595">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="fd584-596">![Salvare il file del profilo di pubblicazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "il salvataggio del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="fd584-596">![Saving the publish profile file](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "Saving the publish profile")</span></span>

    <span data-ttu-id="fd584-597">*Salvare il file del profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="fd584-597">*Saving the publish profile file*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="fd584-598">Attività 2: configurare il Server di Database</span><span class="sxs-lookup"><span data-stu-id="fd584-598">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="fd584-599">Se l'applicazione viene utilizzato SQL Server database, è necessario creare un server di Database SQL.</span><span class="sxs-lookup"><span data-stu-id="fd584-599">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="fd584-600">Se si desidera distribuire un'applicazione semplice che non utilizza SQL Server è possibile ignorare questa attività.</span><span class="sxs-lookup"><span data-stu-id="fd584-600">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="fd584-601">È necessario un server di Database SQL per archiviare il database dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fd584-601">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="fd584-602">È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure all'indirizzo **database Sql** | **server** | **del Server Dashboard**.</span><span class="sxs-lookup"><span data-stu-id="fd584-602">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="fd584-603">Se non si dispone di un server creato, è possibile creare tramite il **Aggiungi** pulsante sulla barra dei comandi.</span><span class="sxs-lookup"><span data-stu-id="fd584-603">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="fd584-604">Annotare il **nome server e URL, il nome di accesso di amministratore e la password**, come verranno utilizzati in operazioni successive.</span><span class="sxs-lookup"><span data-stu-id="fd584-604">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="fd584-605">Non creare il database, come verrà creato in una fase successiva.</span><span class="sxs-lookup"><span data-stu-id="fd584-605">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="fd584-606">![Dashboard del Server Database SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "Dashboard del Server Database SQL")</span><span class="sxs-lookup"><span data-stu-id="fd584-606">![SQL Database Server Dashboard](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="fd584-607">*Dashboard del Server Database SQL*</span><span class="sxs-lookup"><span data-stu-id="fd584-607">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="fd584-608">Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server di **indirizzi IP consentiti**.</span><span class="sxs-lookup"><span data-stu-id="fd584-608">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="fd584-609">A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP da **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo.</span><span class="sxs-lookup"><span data-stu-id="fd584-609">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes.</span></span> <span data-ttu-id="fd584-610">Immettere un nome per la regola e fare clic su di ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) pulsante.</span><span class="sxs-lookup"><span data-stu-id="fd584-610">Enter a name for the rule and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) button.</span></span>

    ![Aggiunta indirizzo IP del Client](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    <span data-ttu-id="fd584-612">*Aggiunta indirizzo IP del Client*</span><span class="sxs-lookup"><span data-stu-id="fd584-612">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="fd584-613">Una volta il **indirizzo IP del Client** viene aggiunto agli indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="fd584-613">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confermare le modifiche](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    <span data-ttu-id="fd584-615">*Confermare le modifiche*</span><span class="sxs-lookup"><span data-stu-id="fd584-615">*Confirm Changes*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="fd584-616">Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web</span><span class="sxs-lookup"><span data-stu-id="fd584-616">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="fd584-617">Tornare alla soluzione ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="fd584-617">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="fd584-618">Nel **Esplora**, fare clic sul progetto sito web e selezionare **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="fd584-618">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="fd584-619">![La pubblicazione dell'applicazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "la pubblicazione dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="fd584-619">![Publishing the Application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "Publishing the Application")</span></span>

    <span data-ttu-id="fd584-620">*Pubblicazione del sito web*</span><span class="sxs-lookup"><span data-stu-id="fd584-620">*Publishing the web site*</span></span>
2. <span data-ttu-id="fd584-621">Importare il profilo di pubblicazione che è stato salvato nella prima attività.</span><span class="sxs-lookup"><span data-stu-id="fd584-621">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="fd584-622">![L'importazione del profilo di pubblicazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "l'importazione del profilo di pubblicazione")</span><span class="sxs-lookup"><span data-stu-id="fd584-622">![Importing the publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importing the publish profile")</span></span>

    <span data-ttu-id="fd584-623">*L'importazione di profilo di pubblicazione*</span><span class="sxs-lookup"><span data-stu-id="fd584-623">*Importing publish profile*</span></span>
3. <span data-ttu-id="fd584-624">Fare clic su **convalidare la connessione**.</span><span class="sxs-lookup"><span data-stu-id="fd584-624">Click **Validate Connection**.</span></span> <span data-ttu-id="fd584-625">Una volta completata la convalida, fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="fd584-625">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="fd584-626">Dopo aver visualizzato un segno di spunta verde visualizzata accanto al pulsante convalida connessione, la convalida è stata completata.</span><span class="sxs-lookup"><span data-stu-id="fd584-626">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="fd584-627">![Convalida della connessione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "convalida della connessione")</span><span class="sxs-lookup"><span data-stu-id="fd584-627">![Validating connection](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Validating connection")</span></span>

    <span data-ttu-id="fd584-628">*Convalida della connessione*</span><span class="sxs-lookup"><span data-stu-id="fd584-628">*Validating connection*</span></span>
4. <span data-ttu-id="fd584-629">Nel **impostazioni** nella pagina il **database** fare clic sul pulsante accanto casella di testo della connessione di database (ad esempio **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="fd584-629">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="fd584-630">![Configurazione della distribuzione Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "configurazione della distribuzione Web")</span><span class="sxs-lookup"><span data-stu-id="fd584-630">![Web deploy configuration](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web deploy configuration")</span></span>

    <span data-ttu-id="fd584-631">*Configurazione della distribuzione Web*</span><span class="sxs-lookup"><span data-stu-id="fd584-631">*Web deploy configuration*</span></span>
5. <span data-ttu-id="fd584-632">Configurare la connessione al database come segue:</span><span class="sxs-lookup"><span data-stu-id="fd584-632">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="fd584-633">Nel **nome Server** digitare l'URL server di Database SQL utilizzando il *tcp:* prefisso.</span><span class="sxs-lookup"><span data-stu-id="fd584-633">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="fd584-634">In **nome utente** digitare il nome di accesso di amministratore di server.</span><span class="sxs-lookup"><span data-stu-id="fd584-634">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="fd584-635">In **Password** digitare la password dell'account di accesso amministratore server.</span><span class="sxs-lookup"><span data-stu-id="fd584-635">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="fd584-636">Digitare un nuovo nome di database, ad esempio: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="fd584-636">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="fd584-637">![Configurazione di stringa di connessione di destinazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "configurazione stringa di connessione di destinazione")</span><span class="sxs-lookup"><span data-stu-id="fd584-637">![Configuring destination connection string](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="fd584-638">*Configurazione di stringa di connessione di destinazione*</span><span class="sxs-lookup"><span data-stu-id="fd584-638">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="fd584-639">Fare quindi clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="fd584-639">Then click **OK**.</span></span> <span data-ttu-id="fd584-640">Quando viene richiesto di creare il database fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="fd584-640">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="fd584-641">![Creazione del database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "creazione della stringa di database")</span><span class="sxs-lookup"><span data-stu-id="fd584-641">![Creating the database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "Creating the database string")</span></span>

    <span data-ttu-id="fd584-642">*Creazione del database*</span><span class="sxs-lookup"><span data-stu-id="fd584-642">*Creating the database*</span></span>
7. <span data-ttu-id="fd584-643">La stringa di connessione che si utilizzerà per connettersi al Database SQL di Windows Azure viene visualizzata all'interno di casella di testo di connessione predefinito.</span><span class="sxs-lookup"><span data-stu-id="fd584-643">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="fd584-644">Scegliere quindi **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="fd584-644">Then click **Next**.</span></span>

    <span data-ttu-id="fd584-645">![Stringa di connessione che punta al Database SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "stringa di connessione che punta al Database SQL")</span><span class="sxs-lookup"><span data-stu-id="fd584-645">![Connection string pointing to SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="fd584-646">*Stringa di connessione che punta al Database SQL*</span><span class="sxs-lookup"><span data-stu-id="fd584-646">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="fd584-647">Nel **anteprima** pagina, fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="fd584-647">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="fd584-648">![Pubblicare l'applicazione web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "pubblicare l'applicazione web")</span><span class="sxs-lookup"><span data-stu-id="fd584-648">![Publishing the web application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "Publishing the web application")</span></span>

    <span data-ttu-id="fd584-649">*Pubblicare l'applicazione web*</span><span class="sxs-lookup"><span data-stu-id="fd584-649">*Publishing the web application*</span></span>
9. <span data-ttu-id="fd584-650">Una volta terminato il processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.</span><span class="sxs-lookup"><span data-stu-id="fd584-650">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="fd584-651">![Applicazione pubblicata in Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "applicazione pubblicata in Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="fd584-651">![Application published to Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="fd584-652">*Applicazione pubblicata in Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="fd584-652">*Application published to Windows Azure*</span></span>
