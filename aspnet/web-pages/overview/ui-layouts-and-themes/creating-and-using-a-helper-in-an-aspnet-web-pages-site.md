---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Creazione e utilizzo di un Helper in un Web ASP.NET le pagine del sito (Razor) | Microsoft Docs
author: tfitzmac
description: Questo articolo descrive come creare un helper in un sito Web di ASP.NET Web Pages (Razor). Un helper è un componente riutilizzabile che include il codice e markup per le prestazioni...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5e73beabf973a20335112a0d7dc5ecb6b8817744
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394612"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="65997-104">Creazione e uso di un Helper in un sito ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="65997-104">Creating and Using a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="65997-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="65997-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="65997-106">Questo articolo descrive come creare un helper in un sito Web di ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="65997-106">This article describes how to create a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="65997-107">Oggetto *helper* è un componente riutilizzabile che include il codice e markup per eseguire un'attività che potrebbe essere noioso o complessi.</span><span class="sxs-lookup"><span data-stu-id="65997-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="65997-108">**Che cosa si apprenderà come:**</span><span class="sxs-lookup"><span data-stu-id="65997-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="65997-109">Come creare e usare un semplice helper.</span><span class="sxs-lookup"><span data-stu-id="65997-109">How to create and use a simple helper.</span></span>
> 
> <span data-ttu-id="65997-110">Queste sono le funzionalità ASP.NET introdotte nell'articolo:</span><span class="sxs-lookup"><span data-stu-id="65997-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="65997-111">Il `@helper` sintassi.</span><span class="sxs-lookup"><span data-stu-id="65997-111">The `@helper` syntax.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="65997-112">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="65997-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="65997-113">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="65997-113">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="65997-114">Questa esercitazione si integra inoltre con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="65997-114">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="65997-115">Panoramica degli helper</span><span class="sxs-lookup"><span data-stu-id="65997-115">Overview of Helpers</span></span>

<span data-ttu-id="65997-116">Se è necessario eseguire le stesse attività in diverse pagine del sito, è possibile usare un file di supporto.</span><span class="sxs-lookup"><span data-stu-id="65997-116">If you need to perform the same tasks on different pages in your site, you can use a helper.</span></span> <span data-ttu-id="65997-117">Pagine Web ASP.NET include una serie di helper, ed esistono molte altre che è possibile scaricare e installare.</span><span class="sxs-lookup"><span data-stu-id="65997-117">ASP.NET Web Pages includes a number of helpers, and there are many more that you can download and install.</span></span> <span data-ttu-id="65997-118">(Un elenco degli helper incorporato in ASP.NET Web Pages è disponibile nel [riferimento rapido API ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202907).) Se nessuno degli helper esistenti soddisfano le esigenze, è possibile creare il proprio supporto.</span><span class="sxs-lookup"><span data-stu-id="65997-118">(A list of the built-in helpers in ASP.NET Web Pages is listed in the [ASP.NET API Quick Reference](https://go.microsoft.com/fwlink/?LinkId=202907).) If none of the existing helpers meet your needs, you can create your own helper.</span></span>

<span data-ttu-id="65997-119">Un helper consente di usare un blocco di codice comune su più pagine.</span><span class="sxs-lookup"><span data-stu-id="65997-119">A helper lets you use a common block of code across multiple pages.</span></span> <span data-ttu-id="65997-120">Si supponga che nella pagina spesso necessario creare un elemento di nota che è separato paragrafi normali.</span><span class="sxs-lookup"><span data-stu-id="65997-120">Suppose that in your page you often want to create a note item that's set apart from normal paragraphs.</span></span> <span data-ttu-id="65997-121">Forse la nota viene creata come un `<div>` elemento che ha l'aspetto di una casella con un bordo.</span><span class="sxs-lookup"><span data-stu-id="65997-121">Perhaps the note is created as a `<div>` element that's styled as a box with a border.</span></span> <span data-ttu-id="65997-122">Anziché aggiungere questo stesso markup per una pagina ogni volta che si desidera visualizzare una nota, è possibile creare un pacchetto del markup come supporto.</span><span class="sxs-lookup"><span data-stu-id="65997-122">Rather than add this same markup to a page every time you want to display a note, you can package the markup as a helper.</span></span> <span data-ttu-id="65997-123">È quindi possibile inserire la nota con una singola riga di codice ovunque ti serve.</span><span class="sxs-lookup"><span data-stu-id="65997-123">You can then insert the note with a single line of code anywhere you need it.</span></span>

<span data-ttu-id="65997-124">Uso di un helper simile al seguente rende il codice in ognuna delle pagine più semplici e facili da leggere.</span><span class="sxs-lookup"><span data-stu-id="65997-124">Using a helper like this makes the code in each of your pages simpler and easier to read.</span></span> <span data-ttu-id="65997-125">Inoltre rende più semplice manutenzione del sito, poiché se è necessario modificare l'aspetto di note, è possibile modificare il markup in un'unica posizione.</span><span class="sxs-lookup"><span data-stu-id="65997-125">It also makes it easier to maintain your site, because if you need to change how the notes look, you can change the markup in one place.</span></span>

## <a name="creating-a-helper"></a><span data-ttu-id="65997-126">Creazione di un Helper</span><span class="sxs-lookup"><span data-stu-id="65997-126">Creating a Helper</span></span>

<span data-ttu-id="65997-127">Questa procedura illustra come creare l'helper che crea la nota, come descritto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="65997-127">This procedure shows you how to create the helper that creates the note, as just described.</span></span> <span data-ttu-id="65997-128">Questo è un esempio semplice, ma l'helper personalizzato può includere qualsiasi markup e codice ASP.NET che è necessario.</span><span class="sxs-lookup"><span data-stu-id="65997-128">This is a simple example, but the custom helper can include any markup and ASP.NET code that you need.</span></span>

1. <span data-ttu-id="65997-129">Nella cartella radice del sito Web, creare una cartella denominata *App\_codice*.</span><span class="sxs-lookup"><span data-stu-id="65997-129">In the root folder of the website, create a folder named *App\_Code*.</span></span> <span data-ttu-id="65997-130">Si tratta di un nome di cartella riservato in ASP.NET in cui è possibile inserire codice per i componenti come helper.</span><span class="sxs-lookup"><span data-stu-id="65997-130">This is a reserved folder name in ASP.NET where you can put code for components like helpers.</span></span>
2. <span data-ttu-id="65997-131">Nel *App\_codice* creare una nuova cartella *. cshtml* file e assegnargli il nome *MyHelpers.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="65997-131">In the *App\_Code* folder create a new *.cshtml* file and name it *MyHelpers.cshtml*.</span></span>
3. <span data-ttu-id="65997-132">Sostituire il contenuto esistente con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="65997-132">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="65997-133">Il codice Usa il `@helper` sintassi per dichiarare un nuovo helper denominato `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="65997-133">The code uses the `@helper` syntax to declare a new helper named `MakeNote`.</span></span> <span data-ttu-id="65997-134">Questo particolare helper consente di passare un parametro denominato `content` che può contenere una combinazione di testo e markup.</span><span class="sxs-lookup"><span data-stu-id="65997-134">This particular helper lets you pass a parameter named `content` that can contain a combination of text and markup.</span></span> <span data-ttu-id="65997-135">L'helper inserisce la stringa nella nota del corpo tramite la `@content` variabile.</span><span class="sxs-lookup"><span data-stu-id="65997-135">The helper inserts the string into the note body using the `@content` variable.</span></span>

    <span data-ttu-id="65997-136">Si noti che il file è denominato *MyHelpers.cshtml*, ma l'helper è denominato `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="65997-136">Notice that the file is named *MyHelpers.cshtml*, but the helper is named `MakeNote`.</span></span> <span data-ttu-id="65997-137">È possibile inserire più helper personalizzati in un unico file.</span><span class="sxs-lookup"><span data-stu-id="65997-137">You can put multiple custom helpers into a single file.</span></span>
4. <span data-ttu-id="65997-138">Salvare e chiudere il file.</span><span class="sxs-lookup"><span data-stu-id="65997-138">Save and close the file.</span></span>

## <a name="using-the-helper-in-a-page"></a><span data-ttu-id="65997-139">Usando l'Helper in una pagina</span><span class="sxs-lookup"><span data-stu-id="65997-139">Using the Helper in a Page</span></span>

1. <span data-ttu-id="65997-140">Nella cartella radice, creare un nuovo file vuoto denominato *TestHelper.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="65997-140">In the root folder, create a new blank file called *TestHelper.cshtml*.</span></span>
2. <span data-ttu-id="65997-141">Aggiungere al file il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="65997-141">Add the following code to the file:</span></span>

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    <span data-ttu-id="65997-142">Per chiamare il supporto è stato creato, usare `@` aggiungendo il nome del file in cui l'helper è, un punto e quindi il nome di supporto.</span><span class="sxs-lookup"><span data-stu-id="65997-142">To call the helper you created, use `@` followed by the file name where the helper is, a dot, and then the helper name.</span></span> <span data-ttu-id="65997-143">(Se si dispone di più cartelle *App\_codice* cartella, è possibile utilizzare la sintassi `@FolderName.FileName.HelperName` per chiamare l'helper all'interno di qualsiasi nidificate a livello di cartella).</span><span class="sxs-lookup"><span data-stu-id="65997-143">(If you had multiple folders in the *App\_Code* folder, you could use the syntax `@FolderName.FileName.HelperName` to call your helper within any nested folder level).</span></span> <span data-ttu-id="65997-144">Il testo che aggiunta in virgolette doppie all'interno delle parentesi è il testo che l'helper verrà visualizzato come parte della nota nella pagina web.</span><span class="sxs-lookup"><span data-stu-id="65997-144">The text that you add in quotation marks within the parentheses is the text that the helper will display as part of the note in the web page.</span></span>
3. <span data-ttu-id="65997-145">Salvare la pagina ed eseguirlo in un browser.</span><span class="sxs-lookup"><span data-stu-id="65997-145">Save the page and run it in a browser.</span></span> <span data-ttu-id="65997-146">L'helper che genera l'errore nota elemento a destra in cui è stato chiamato l'helper: tra i due paragrafi.</span><span class="sxs-lookup"><span data-stu-id="65997-146">The helper generates the note item right where you called the helper: between the two paragraphs.</span></span>

    ![Screenshot che mostra la pagina nel browser e modalità di generazione di markup che inserisce una casella intorno al testo specificato dell'helper.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a><span data-ttu-id="65997-148">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="65997-148">Additional Resources</span></span>


<span data-ttu-id="65997-149">[Menu orizzontale come helper Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span><span class="sxs-lookup"><span data-stu-id="65997-149">[Horizontal menu as a Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span></span> <span data-ttu-id="65997-150">In questo intervento sul blog di Mike Pope viene illustrato come creare un menu orizzontale come helper con markup, CSS e codice.</span><span class="sxs-lookup"><span data-stu-id="65997-150">This blog entry by Mike Pope shows how to create a horizontal menu as a helper using markup, CSS, and code.</span></span>

<span data-ttu-id="65997-151">[Uso di HTML5 in ASP.NET Web Pages gli helper per WebMatrix e ASP.NET MVC 3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span><span class="sxs-lookup"><span data-stu-id="65997-151">[Leveraging HTML5 in ASP.NET Web Pages Helpers for WebMatrix and ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span></span> <span data-ttu-id="65997-152">Questo post di blog da Sam Abraham Mostra un helper che esegue il rendering di un HTML5 `Canvas` elemento.</span><span class="sxs-lookup"><span data-stu-id="65997-152">This blog entry by Sam Abraham shows a helper that renders an HTML5 `Canvas` element.</span></span>

<span data-ttu-id="65997-153">[La differenza tra @Helpers e @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span><span class="sxs-lookup"><span data-stu-id="65997-153">[The Difference Between @Helpers and @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span></span> <span data-ttu-id="65997-154">Viene descritto questo intervento sul blog di Mike Brind `@helper` sintassi e `@function` sintassi e il loro utilizzo.</span><span class="sxs-lookup"><span data-stu-id="65997-154">This blog entry by Mike Brind describes `@helper` syntax and `@function` syntax and when to use each.</span></span>
