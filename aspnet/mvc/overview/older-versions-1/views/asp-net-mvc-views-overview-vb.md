---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
title: ASP.NET MVC Visualizza panoramica (VB) | Documenti Microsoft
author: StephenWalther
description: "Che cos'è una visualizzazione MVC ASP.NET e differenza da una pagina HTML? In questa esercitazione, Stephen Walther vengono introdotte le visualizzazioni e viene illustrato come è possibile t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: c28ba88d-3a93-47f5-a306-049bd766714d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: c85b969aa4457d0326b4a16da218db9e11d01e10
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-views-overview-vb"></a><span data-ttu-id="e613b-104">ASP.NET MVC Visualizza panoramica (VB)</span><span class="sxs-lookup"><span data-stu-id="e613b-104">ASP.NET MVC Views Overview (VB)</span></span>
====================
<span data-ttu-id="e613b-105">da [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="e613b-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="e613b-106">Che cos'è una visualizzazione MVC ASP.NET e differenza da una pagina HTML?</span><span class="sxs-lookup"><span data-stu-id="e613b-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="e613b-107">In questa esercitazione, Stephen Walther vengono introdotte le visualizzazioni e viene illustrato come è possibile usufruire di visualizzare i dati e gli helper HTML all'interno di una vista.</span><span class="sxs-lookup"><span data-stu-id="e613b-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>


<span data-ttu-id="e613b-108">Lo scopo di questa esercitazione consiste nel fornire una breve introduzione alle visualizzazioni ASP.NET MVC, visualizzare i dati e gli helper HTML.</span><span class="sxs-lookup"><span data-stu-id="e613b-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="e613b-109">Al termine di questa esercitazione, è necessario comprendere come creare nuove visualizzazioni, passare una visualizzazione dati da un controller e utilizzare l'helper HTML per generare il contenuto in una vista.</span><span class="sxs-lookup"><span data-stu-id="e613b-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="e613b-110">Informazioni sulle viste</span><span class="sxs-lookup"><span data-stu-id="e613b-110">Understanding Views</span></span>

<span data-ttu-id="e613b-111">A differenza delle pagine ASP o ASP.NET, MVC ASP.NET non include tutto ciò che corrisponde direttamente a una pagina.</span><span class="sxs-lookup"><span data-stu-id="e613b-111">Unlike ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="e613b-112">In un'applicazione ASP.NET MVC, non c'è una pagina su disco che corrisponde al percorso nell'URL digitato nella barra degli indirizzi del browser.</span><span class="sxs-lookup"><span data-stu-id="e613b-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="e613b-113">La cosa più vicina a una pagina in un'applicazione ASP.NET MVC è un elemento denominato un *vista*.</span><span class="sxs-lookup"><span data-stu-id="e613b-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="e613b-114">In un'applicazione ASP.NET MVC, alle richieste del browser in ingresso vengono mappate alle azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="e613b-114">In an ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="e613b-115">Un'azione del controller potrebbe restituire una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="e613b-115">A controller action might return a view.</span></span> <span data-ttu-id="e613b-116">Tuttavia, un'azione del controller potrebbe eseguire un altro tipo di azione, ad esempio si reindirizzamento a un'altra azione del controller.</span><span class="sxs-lookup"><span data-stu-id="e613b-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="e613b-117">Elenco 1 contiene un controller semplice denominato HomeController.</span><span class="sxs-lookup"><span data-stu-id="e613b-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="e613b-118">La classe HomeController espone due azioni del controller denominate Index () e Details().</span><span class="sxs-lookup"><span data-stu-id="e613b-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="e613b-119">**Elenco 1 - HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="e613b-119">**Listing 1 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample1.vb)]

<span data-ttu-id="e613b-120">È possibile richiamare la prima azione, l'azione Index (), digitando il seguente URL nella barra degli indirizzi del browser:</span><span class="sxs-lookup"><span data-stu-id="e613b-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="e613b-121">/ Home/Index</span><span class="sxs-lookup"><span data-stu-id="e613b-121">/Home/Index</span></span>

<span data-ttu-id="e613b-122">È possibile richiamare la seconda azione, l'azione Details(), digitando l'indirizzo nel browser:</span><span class="sxs-lookup"><span data-stu-id="e613b-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="e613b-123">/ Home/dettagli</span><span class="sxs-lookup"><span data-stu-id="e613b-123">/Home/Details</span></span>

<span data-ttu-id="e613b-124">L'azione Index () restituisce una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="e613b-124">The Index() action returns a view.</span></span> <span data-ttu-id="e613b-125">La maggior parte delle azioni create restituirà le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="e613b-125">Most actions that you create will return views.</span></span> <span data-ttu-id="e613b-126">Tuttavia, un'azione può restituire altri tipi di risultati dell'azione.</span><span class="sxs-lookup"><span data-stu-id="e613b-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="e613b-127">Ad esempio, l'azione Details() restituisce un RedirectToActionResult che reindirizza la richiesta in ingresso per l'azione Index ().</span><span class="sxs-lookup"><span data-stu-id="e613b-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="e613b-128">L'azione Index () contiene la seguente riga di codice:</span><span class="sxs-lookup"><span data-stu-id="e613b-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="e613b-129">View()</span><span class="sxs-lookup"><span data-stu-id="e613b-129">View()</span></span>

<span data-ttu-id="e613b-130">Questa riga di codice restituisce una visualizzazione che deve trovarsi nel percorso seguente sul server web:</span><span class="sxs-lookup"><span data-stu-id="e613b-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="e613b-131">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="e613b-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="e613b-132">Il percorso della visualizzazione viene dedotto dal nome del controller e il nome dell'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="e613b-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="e613b-133">Se si preferisce, possono essere esplicite sulla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="e613b-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="e613b-134">La riga di codice seguente restituisce una visualizzazione denominata Fred:</span><span class="sxs-lookup"><span data-stu-id="e613b-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="e613b-135">Visualizzazione (Fred)</span><span class="sxs-lookup"><span data-stu-id="e613b-135">View( Fred )</span></span>

<span data-ttu-id="e613b-136">Quando viene eseguita questa riga di codice, viene restituita una visualizzazione dal percorso seguente:</span><span class="sxs-lookup"><span data-stu-id="e613b-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="e613b-137">\Views\Home\Fred.aspx</span><span class="sxs-lookup"><span data-stu-id="e613b-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="e613b-138">Se si prevede di creare unit test per l'applicazione ASP.NET MVC è consigliabile essere espliciti sui nomi di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="e613b-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="e613b-139">In questo modo, è possibile creare uno unit test per verificare che la vista previsto è stata restituita da un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="e613b-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>


## <a name="adding-content-to-a-view"></a><span data-ttu-id="e613b-140">Aggiunta di contenuto a una vista</span><span class="sxs-lookup"><span data-stu-id="e613b-140">Adding Content to a View</span></span>

<span data-ttu-id="e613b-141">Una vista è uno standard (documento HTML che può includere script X).</span><span class="sxs-lookup"><span data-stu-id="e613b-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="e613b-142">Gli script consentono di aggiungere contenuto dinamico a una vista.</span><span class="sxs-lookup"><span data-stu-id="e613b-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="e613b-143">Ad esempio, la visualizzazione nel listato 2 Visualizza la data e ora correnti.</span><span class="sxs-lookup"><span data-stu-id="e613b-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="e613b-144">**Elenco di 2 - \Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="e613b-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample2.aspx)]

<span data-ttu-id="e613b-145">Si noti che il corpo della pagina HTML nel listato 2 contiene lo script seguente:</span><span class="sxs-lookup"><span data-stu-id="e613b-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="e613b-146">&lt;% Response.Write(DateTime.Now) %&gt;</span><span class="sxs-lookup"><span data-stu-id="e613b-146">&lt;% Response.Write(DateTime.Now)%&gt;</span></span>

<span data-ttu-id="e613b-147">Utilizzare i delimitatori di script &lt;% e %&gt; per contrassegnare l'inizio e alla fine di uno script.</span><span class="sxs-lookup"><span data-stu-id="e613b-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="e613b-148">Questo script viene scritto in Visual basic.</span><span class="sxs-lookup"><span data-stu-id="e613b-148">This script is written in Visual basic.</span></span> <span data-ttu-id="e613b-149">Visualizza la data e ora correnti, chiamare il metodo Response per il rendering del contenuto nel browser.</span><span class="sxs-lookup"><span data-stu-id="e613b-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="e613b-150">I delimitatori di script &lt;% e %&gt; può essere usato per eseguire una o più istruzioni.</span><span class="sxs-lookup"><span data-stu-id="e613b-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="e613b-151">Poiché si effettuano chiamate Response spesso, Microsoft offre un scelta rapida per chiamare il metodo Response.</span><span class="sxs-lookup"><span data-stu-id="e613b-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="e613b-152">La visualizzazione nel listato 3 utilizza i delimitatori &lt;% = % e&gt; come scelta rapida per la chiamata a Response.</span><span class="sxs-lookup"><span data-stu-id="e613b-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="e613b-153">**Elenco di 3 - Views\Home\Index2.aspx**</span><span class="sxs-lookup"><span data-stu-id="e613b-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample3.aspx)]

<span data-ttu-id="e613b-154">È possibile utilizzare qualsiasi linguaggio .NET per generare il contenuto dinamico in una vista.</span><span class="sxs-lookup"><span data-stu-id="e613b-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="e613b-155">In genere, si ll utilizzare Visual Basic .NET o Visual c# per scrivere il controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="e613b-155">Normally, you�ll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="e613b-156">Utilizzando l'helper HTML per generare il contenuto di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="e613b-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="e613b-157">Per rendere più semplice aggiungere contenuto a una vista, è possibile sfruttare cosiddetto un *HTML Helper*.</span><span class="sxs-lookup"><span data-stu-id="e613b-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="e613b-158">Un HTML Helper, è in genere, un metodo che genera una stringa.</span><span class="sxs-lookup"><span data-stu-id="e613b-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="e613b-159">È possibile utilizzare l'helper HTML per generare elementi HTML standard, ad esempio caselle di testo, collegamenti, elenchi a discesa e caselle di riepilogo.</span><span class="sxs-lookup"><span data-stu-id="e613b-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="e613b-160">Ad esempio, la visualizzazione nel listato 4 sfrutta i vantaggi dei tre helper HTML, gli helper BeginForm() TextBox() e Password()-consente di generare un account di accesso formano (vedere Figura 1).</span><span class="sxs-lookup"><span data-stu-id="e613b-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="e613b-161">**Elenco di 4 - \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="e613b-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample4.aspx)]


<span data-ttu-id="e613b-162">[![La finestra di dialogo Nuovo progetto](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e613b-162">[![The New Project dialog box](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="e613b-163">**Figura 01**: un form di accesso standard ([fare clic per visualizzare l'immagine ingrandita](asp-net-mvc-views-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="e613b-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-vb/_static/image2.png))</span></span>


<span data-ttu-id="e613b-164">Tutti i metodi helper HTML sono chiamati sulla proprietà della visualizzazione Html.</span><span class="sxs-lookup"><span data-stu-id="e613b-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="e613b-165">Ad esempio, si esegue il rendering una casella di testo chiamando il metodo Html.TextBox().</span><span class="sxs-lookup"><span data-stu-id="e613b-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="e613b-166">Si noti che utilizzare i delimitatori di script &lt;% = % e&gt; quando si chiama Html.TextBox() sia Html.Password() helper.</span><span class="sxs-lookup"><span data-stu-id="e613b-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="e613b-167">Questi helper limitino a restituire una stringa.</span><span class="sxs-lookup"><span data-stu-id="e613b-167">These helpers simply return a string.</span></span> <span data-ttu-id="e613b-168">È necessario chiamare Response per eseguire il rendering di stringa per il browser.</span><span class="sxs-lookup"><span data-stu-id="e613b-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="e613b-169">Utilizzo di metodi HTML Helper è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="e613b-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="e613b-170">Essi semplificano la vita riducendo la quantità di HTML e script che è necessario scrivere.</span><span class="sxs-lookup"><span data-stu-id="e613b-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="e613b-171">La visualizzazione nel listato 5 esegue il rendering lo stesso formato esatto di visualizzazione nel listato 4 senza utilizzare l'helper HTML.</span><span class="sxs-lookup"><span data-stu-id="e613b-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="e613b-172">**Elenco di 5 - \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="e613b-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample5.aspx)]

<span data-ttu-id="e613b-173">È inoltre la possibilità di creare la propria helper HTML.</span><span class="sxs-lookup"><span data-stu-id="e613b-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="e613b-174">Ad esempio, è possibile creare un metodo di supporto GridView() che visualizza automaticamente un set di record del database in una tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="e613b-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="e613b-175">In questo argomento vengono illustrati nell'esercitazione **la creazione di helper HTML personalizzati**.</span><span class="sxs-lookup"><span data-stu-id="e613b-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="e613b-176">Utilizzando i dati della visualizzazione di passare dati a una vista</span><span class="sxs-lookup"><span data-stu-id="e613b-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="e613b-177">Visualizzare i dati consentono di passare i dati da un controller a una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="e613b-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="e613b-178">Pensare a visualizzare i dati come un pacchetto che si invia tramite posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="e613b-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="e613b-179">Tutti i dati passati da un controller a una vista devono essere inviati tramite questo pacchetto.</span><span class="sxs-lookup"><span data-stu-id="e613b-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="e613b-180">Ad esempio, il controller nel listato 6 aggiunge un messaggio per visualizzare i dati.</span><span class="sxs-lookup"><span data-stu-id="e613b-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="e613b-181">**Elenco di 6 - ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="e613b-181">**Listing 6 - ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample6.vb)]

<span data-ttu-id="e613b-182">Il controller di proprietà ViewData rappresenta una raccolta di coppie nome / valore.</span><span class="sxs-lookup"><span data-stu-id="e613b-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="e613b-183">Nel listato 6, il metodo Index () aggiunge un elemento per la raccolta di dati di visualizzazione denominata messaggio con il valore di Hello World!.</span><span class="sxs-lookup"><span data-stu-id="e613b-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="e613b-184">Quando la vista viene restituita dal metodo Index (), i dati della visualizzazione viene passati automaticamente alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="e613b-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="e613b-185">La visualizzazione nel listato 7 recupera il messaggio dai dati di visualizzazione e visualizza il messaggio nel browser.</span><span class="sxs-lookup"><span data-stu-id="e613b-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="e613b-186">**Elenco 7 - \Views\Product\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="e613b-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample7.aspx)]

<span data-ttu-id="e613b-187">Si noti che la vista consente di sfruttare il metodo Helper HTML Html.Encode() quando il rendering del messaggio.</span><span class="sxs-lookup"><span data-stu-id="e613b-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="e613b-188">L'Helper HTML Html.Encode() codifica i caratteri speciali, ad esempio &lt; e &gt; in caratteri che possono essere visualizzati in una pagina web.</span><span class="sxs-lookup"><span data-stu-id="e613b-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="e613b-189">Ogni volta che si esegue il rendering del contenuto per l'invio di un utente a un sito Web, si consiglia di codificare il contenuto per impedire attacchi intrusivi nel codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e613b-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="e613b-190">(Perché abbiamo creato il messaggio effettuata nel ProductController, non abbiamo t devono necessariamente codificare il messaggio.</span><span class="sxs-lookup"><span data-stu-id="e613b-190">(Because we created the message ourselves in the ProductController, we don�t really need to encode the message.</span></span> <span data-ttu-id="e613b-191">Tuttavia, è consigliabile chiamare sempre il metodo Html.Encode() quando visualizzare il contenuto recuperato da visualizzare i dati all'interno di una vista.)</span><span class="sxs-lookup"><span data-stu-id="e613b-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="e613b-192">Nel listato 7, abbiamo sfruttato Visualizza dati per passare un messaggio stringa semplice da un controller a una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="e613b-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="e613b-193">È possibile utilizzare dati di visualizzazione per trasmettere altri tipi di dati, ad esempio una raccolta di record del database, da un controller a una vista.</span><span class="sxs-lookup"><span data-stu-id="e613b-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="e613b-194">Ad esempio, se si desidera visualizzare il contenuto della tabella Products del database in una vista, quindi passare la raccolta di database nella visualizzazione registra dati.</span><span class="sxs-lookup"><span data-stu-id="e613b-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="e613b-195">È inoltre possibile passare i dati di visualizzazione fortemente tipizzata da un controller a una vista.</span><span class="sxs-lookup"><span data-stu-id="e613b-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="e613b-196">In questo argomento vengono illustrati nell'esercitazione **dati di visualizzazione di informazioni sui fortemente tipizzati e viste**.</span><span class="sxs-lookup"><span data-stu-id="e613b-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="e613b-197">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="e613b-197">Summary</span></span>

<span data-ttu-id="e613b-198">In questa esercitazione viene fornita una breve introduzione a ASP.NET MVC viste, visualizzare i dati e gli helper HTML.</span><span class="sxs-lookup"><span data-stu-id="e613b-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="e613b-199">Nella prima sezione, è stato descritto come aggiungere nuove visualizzazioni per il progetto.</span><span class="sxs-lookup"><span data-stu-id="e613b-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="e613b-200">Si è appreso che è necessario aggiungere una visualizzazione nella cartella corretta per chiamarlo da un controller specifico.</span><span class="sxs-lookup"><span data-stu-id="e613b-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="e613b-201">Successivamente, abbiamo parlato l'argomento di helper HTML.</span><span class="sxs-lookup"><span data-stu-id="e613b-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="e613b-202">Si è appreso come helper HTML consentono di generare facilmente il contenuto HTML standard.</span><span class="sxs-lookup"><span data-stu-id="e613b-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="e613b-203">Infine, è stato descritto come sfruttare i vantaggi dei dati di visualizzazione per passare dati da un controller di una vista.</span><span class="sxs-lookup"><span data-stu-id="e613b-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="e613b-204">[Precedente](passing-data-to-view-master-pages-cs.md)
[Successivo](creating-custom-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e613b-204">[Previous](passing-data-to-view-master-pages-cs.md)
[Next](creating-custom-html-helpers-vb.md)</span></span>
