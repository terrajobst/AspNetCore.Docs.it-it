---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Creazione e utilizzo di un Helper in un Web ASP.NET, le pagine del sito (Razor) | Documenti Microsoft
author: tfitzmac
description: In questo articolo viene descritto come creare un supporto in un sito Web ASP.NET Web Pages (Razor). Un helper è un componente riutilizzabile che include codice e markup perf...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5d0c1ae09d8fbc91ff76cd4045d439abafee7736
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530000"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="9f559-104">Creazione e utilizzo di un Helper in un sito ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="9f559-104">Creating and Using a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="9f559-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="9f559-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="9f559-106">In questo articolo viene descritto come creare un supporto in un sito Web ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="9f559-106">This article describes how to create a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="9f559-107">Oggetto *helper* è un componente riutilizzabile che include codice e markup per eseguire un'attività che potrebbe essere noiosa o complessi.</span><span class="sxs-lookup"><span data-stu-id="9f559-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="9f559-108">**Illustra quanto segue:**</span><span class="sxs-lookup"><span data-stu-id="9f559-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="9f559-109">Come creare e utilizzare un helper semplice.</span><span class="sxs-lookup"><span data-stu-id="9f559-109">How to create and use a simple helper.</span></span>
> 
> <span data-ttu-id="9f559-110">Queste sono le funzionalità ASP.NET introdotte nell'articolo:</span><span class="sxs-lookup"><span data-stu-id="9f559-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="9f559-111">Il `@helper` sintassi.</span><span class="sxs-lookup"><span data-stu-id="9f559-111">The `@helper` syntax.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="9f559-112">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="9f559-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="9f559-113">Pagine Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="9f559-113">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="9f559-114">In questa esercitazione si integra inoltre con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="9f559-114">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="9f559-115">Panoramica di helper</span><span class="sxs-lookup"><span data-stu-id="9f559-115">Overview of Helpers</span></span>

<span data-ttu-id="9f559-116">Se è necessario eseguire le stesse operazioni in diverse pagine del sito, è possibile utilizzare un file di supporto.</span><span class="sxs-lookup"><span data-stu-id="9f559-116">If you need to perform the same tasks on different pages in your site, you can use a helper.</span></span> <span data-ttu-id="9f559-117">Esistono molti altri che è possibile scaricare e installare ASP.NET Web Pages include un numero di helper.</span><span class="sxs-lookup"><span data-stu-id="9f559-117">ASP.NET Web Pages includes a number of helpers, and there are many more that you can download and install.</span></span> <span data-ttu-id="9f559-118">(Un elenco degli helper incorporato in ASP.NET Web Pages è disponibile nel [ASP.NET riferimento rapido di API](https://go.microsoft.com/fwlink/?LinkId=202907).) Se nessuno degli helper esistente alle proprie esigenze, è possibile creare la propria helper.</span><span class="sxs-lookup"><span data-stu-id="9f559-118">(A list of the built-in helpers in ASP.NET Web Pages is listed in the [ASP.NET API Quick Reference](https://go.microsoft.com/fwlink/?LinkId=202907).) If none of the existing helpers meet your needs, you can create your own helper.</span></span>

<span data-ttu-id="9f559-119">Un helper consente di usare un blocco di codice comune in più pagine.</span><span class="sxs-lookup"><span data-stu-id="9f559-119">A helper lets you use a common block of code across multiple pages.</span></span> <span data-ttu-id="9f559-120">Si supponga che nella pagina è spesso necessario creare un elemento di nota che è separato paragrafi normali.</span><span class="sxs-lookup"><span data-stu-id="9f559-120">Suppose that in your page you often want to create a note item that's set apart from normal paragraphs.</span></span> <span data-ttu-id="9f559-121">Ad esempio la nota viene creata come un `<div>` elemento che ha l'aspetto di una casella con un bordo.</span><span class="sxs-lookup"><span data-stu-id="9f559-121">Perhaps the note is created as a `<div>` element that's styled as a box with a border.</span></span> <span data-ttu-id="9f559-122">Anziché aggiungere il markup stesso a una pagina ogni volta che si desidera visualizzare una nota, è possibile comprimere il markup come supporto.</span><span class="sxs-lookup"><span data-stu-id="9f559-122">Rather than add this same markup to a page every time you want to display a note, you can package the markup as a helper.</span></span> <span data-ttu-id="9f559-123">È quindi possibile inserire la nota con una singola riga di codice in qualsiasi punto è necessario.</span><span class="sxs-lookup"><span data-stu-id="9f559-123">You can then insert the note with a single line of code anywhere you need it.</span></span>

<span data-ttu-id="9f559-124">Utilizzando un supporto di questo rende il codice in ognuna delle pagine più semplice e più facile da leggere.</span><span class="sxs-lookup"><span data-stu-id="9f559-124">Using a helper like this makes the code in each of your pages simpler and easier to read.</span></span> <span data-ttu-id="9f559-125">Rende inoltre più semplice gestire il sito, poiché se è necessario modificare l'aspetto delle note, è possibile modificare il markup in un'unica posizione.</span><span class="sxs-lookup"><span data-stu-id="9f559-125">It also makes it easier to maintain your site, because if you need to change how the notes look, you can change the markup in one place.</span></span>

## <a name="creating-a-helper"></a><span data-ttu-id="9f559-126">Creazione di un Helper</span><span class="sxs-lookup"><span data-stu-id="9f559-126">Creating a Helper</span></span>

<span data-ttu-id="9f559-127">In questa procedura viene illustrato come creare l'helper che crea la nota, come descritto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="9f559-127">This procedure shows you how to create the helper that creates the note, as just described.</span></span> <span data-ttu-id="9f559-128">Questo è un esempio semplice, ma l'helper personalizzato può includere qualsiasi markup e codice ASP.NET che è necessario.</span><span class="sxs-lookup"><span data-stu-id="9f559-128">This is a simple example, but the custom helper can include any markup and ASP.NET code that you need.</span></span>

1. <span data-ttu-id="9f559-129">Nella cartella radice del sito Web, creare una cartella denominata *App\_codice*.</span><span class="sxs-lookup"><span data-stu-id="9f559-129">In the root folder of the website, create a folder named *App\_Code*.</span></span> <span data-ttu-id="9f559-130">Questo è un nome di cartella riservato in ASP.NET in cui è possibile inserire codice per i componenti come helper.</span><span class="sxs-lookup"><span data-stu-id="9f559-130">This is a reserved folder name in ASP.NET where you can put code for components like helpers.</span></span>
2. <span data-ttu-id="9f559-131">Nel *App\_codice* cartella creare un nuovo *. cshtml* file e denominarlo *MyHelpers.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9f559-131">In the *App\_Code* folder create a new *.cshtml* file and name it *MyHelpers.cshtml*.</span></span>
3. <span data-ttu-id="9f559-132">Sostituire il contenuto esistente con il seguente:</span><span class="sxs-lookup"><span data-stu-id="9f559-132">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="9f559-133">Il codice Usa il `@helper` sintassi per dichiarare un nuovo supporto denominato `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="9f559-133">The code uses the `@helper` syntax to declare a new helper named `MakeNote`.</span></span> <span data-ttu-id="9f559-134">Questo particolare supporto consente di passare un parametro denominato `content` che può contenere una combinazione di testo e markup.</span><span class="sxs-lookup"><span data-stu-id="9f559-134">This particular helper lets you pass a parameter named `content` that can contain a combination of text and markup.</span></span> <span data-ttu-id="9f559-135">L'helper inserisce la stringa nel corpo nota usando il `@content` variabile.</span><span class="sxs-lookup"><span data-stu-id="9f559-135">The helper inserts the string into the note body using the `@content` variable.</span></span>

    <span data-ttu-id="9f559-136">Si noti che il file è denominato *MyHelpers.cshtml*, ma l'helper è denominata `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="9f559-136">Notice that the file is named *MyHelpers.cshtml*, but the helper is named `MakeNote`.</span></span> <span data-ttu-id="9f559-137">È possibile inserire più helper personalizzati in un singolo file.</span><span class="sxs-lookup"><span data-stu-id="9f559-137">You can put multiple custom helpers into a single file.</span></span>
4. <span data-ttu-id="9f559-138">Salvare e chiudere il file.</span><span class="sxs-lookup"><span data-stu-id="9f559-138">Save and close the file.</span></span>

## <a name="using-the-helper-in-a-page"></a><span data-ttu-id="9f559-139">Utilizzando l'Helper in una pagina</span><span class="sxs-lookup"><span data-stu-id="9f559-139">Using the Helper in a Page</span></span>

1. <span data-ttu-id="9f559-140">Nella cartella radice, creare un nuovo file vuoto denominato *TestHelper.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9f559-140">In the root folder, create a new blank file called *TestHelper.cshtml*.</span></span>
2. <span data-ttu-id="9f559-141">Aggiungere al file il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9f559-141">Add the following code to the file:</span></span>

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    <span data-ttu-id="9f559-142">Per chiamare il supporto è stato creato, utilizzare `@` aggiungendo il nome del file in cui l'helper è un punto, quindi il nome di supporto.</span><span class="sxs-lookup"><span data-stu-id="9f559-142">To call the helper you created, use `@` followed by the file name where the helper is, a dot, and then the helper name.</span></span> <span data-ttu-id="9f559-143">(Se si dispone di più cartelle *App\_codice* cartella, è possibile utilizzare la sintassi `@FolderName.FileName.HelperName` per chiamare il supporto all'interno di qualsiasi annidate a livello di cartella).</span><span class="sxs-lookup"><span data-stu-id="9f559-143">(If you had multiple folders in the *App\_Code* folder, you could use the syntax `@FolderName.FileName.HelperName` to call your helper within any nested folder level).</span></span> <span data-ttu-id="9f559-144">Il testo che aggiunta virgolette all'interno delle parentesi è il testo da Visualizza come parte della nota nella pagina web l'helper.</span><span class="sxs-lookup"><span data-stu-id="9f559-144">The text that you add in quotation marks within the parentheses is the text that the helper will display as part of the note in the web page.</span></span>
3. <span data-ttu-id="9f559-145">Salvare la pagina ed eseguirlo in un browser.</span><span class="sxs-lookup"><span data-stu-id="9f559-145">Save the page and run it in a browser.</span></span> <span data-ttu-id="9f559-146">L'helper genera l'elemento di nota a destra in cui è stato chiamato l'helper: tra i due paragrafi.</span><span class="sxs-lookup"><span data-stu-id="9f559-146">The helper generates the note item right where you called the helper: between the two paragraphs.</span></span>

    ![Screenshot che illustra la pagina nel browser e la modalità di generazione di codice che inserisce una casella di testo specificato l'helper.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a><span data-ttu-id="9f559-148">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="9f559-148">Additional Resources</span></span>


<span data-ttu-id="9f559-149">[Menu orizzontale come supporto Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span><span class="sxs-lookup"><span data-stu-id="9f559-149">[Horizontal menu as a Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span></span> <span data-ttu-id="9f559-150">Questo post di blog da Mike Pope viene illustrato come creare un menu orizzontale come supporto tramite markup, CSS e codice.</span><span class="sxs-lookup"><span data-stu-id="9f559-150">This blog entry by Mike Pope shows how to create a horizontal menu as a helper using markup, CSS, and code.</span></span>

<span data-ttu-id="9f559-151">[Sfruttando HTML5 in ASP.NET Web Pages helper per WebMatrix e ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span><span class="sxs-lookup"><span data-stu-id="9f559-151">[Leveraging HTML5 in ASP.NET Web Pages Helpers for WebMatrix and ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span></span> <span data-ttu-id="9f559-152">Questo post di blog da Sam Abraham Mostra un helper che esegue il rendering di un HTML5 `Canvas` elemento.</span><span class="sxs-lookup"><span data-stu-id="9f559-152">This blog entry by Sam Abraham shows a helper that renders an HTML5 `Canvas` element.</span></span>

<span data-ttu-id="9f559-153">[La differenza tra @Helpers e @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span><span class="sxs-lookup"><span data-stu-id="9f559-153">[The Difference Between @Helpers and @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span></span> <span data-ttu-id="9f559-154">Viene descritto questo post di blog da Mike Brind `@helper` sintassi e `@function` sintassi e il loro utilizzo.</span><span class="sxs-lookup"><span data-stu-id="9f559-154">This blog entry by Mike Brind describes `@helper` syntax and `@function` syntax and when to use each.</span></span>
