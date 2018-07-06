---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
title: Panoramica (VB) delle visualizzazioni ASP.NET MVC | Microsoft Docs
author: StephenWalther
description: Che cos'è una visualizzazione MVC ASP.NET e in che cosa differisce da una pagina HTML? In questa esercitazione, Stephen Walther presenta le viste e viene illustrato come è possibile t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: c28ba88d-3a93-47f5-a306-049bd766714d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 12039a799b71c88e012831f6eefb01c668142a75
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369273"
---
<a name="aspnet-mvc-views-overview-vb"></a><span data-ttu-id="d6e3e-104">Panoramica (VB) delle visualizzazioni ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="d6e3e-104">ASP.NET MVC Views Overview (VB)</span></span>
====================
<span data-ttu-id="d6e3e-105">da [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="d6e3e-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="d6e3e-106">Che cos'è una visualizzazione MVC ASP.NET e in che cosa differisce da una pagina HTML?</span><span class="sxs-lookup"><span data-stu-id="d6e3e-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="d6e3e-107">In questa esercitazione, Stephen Walther presenta le viste e viene illustrato come è possibile sfruttare i vantaggi di visualizzare i dati e gli helper HTML all'interno di una vista.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>


<span data-ttu-id="d6e3e-108">Lo scopo di questa esercitazione è fornire una breve introduzione a visualizzazioni ASP.NET MVC, visualizzare i dati e gli helper HTML.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="d6e3e-109">Al termine di questa esercitazione, è necessario comprendere come creare nuove viste, passare i dati da un controller a una visualizzazione e usare gli helper HTML per generare il contenuto in una vista.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="d6e3e-110">Informazioni sulle viste</span><span class="sxs-lookup"><span data-stu-id="d6e3e-110">Understanding Views</span></span>

<span data-ttu-id="d6e3e-111">A differenza di ASP.NET o pagine ASP, ASP.NET MVC non include tutto ciò che corrisponde direttamente a una pagina.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-111">Unlike ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="d6e3e-112">In un'applicazione ASP.NET MVC, non esiste una pagina su disco che corrisponde al percorso nell'URL digitato nella barra degli indirizzi del browser.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="d6e3e-113">L'elemento più simile a una pagina in un'applicazione ASP.NET MVC è un elemento chiamato un' *vista*.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="d6e3e-114">In un'applicazione ASP.NET MVC, le richieste in ingresso browser vengono mappate alle azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-114">In an ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="d6e3e-115">Un'azione del controller potrebbe restituire una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-115">A controller action might return a view.</span></span> <span data-ttu-id="d6e3e-116">Tuttavia, un'azione del controller potrebbe eseguire un altro tipo di azione, ad esempio è il reindirizzamento a un'altra azione del controller.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="d6e3e-117">L'elenco 1 contiene un controller semplice denominato HomeController.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="d6e3e-118">La classe HomeController espone due azioni del controller denominate Details() e Index ().</span><span class="sxs-lookup"><span data-stu-id="d6e3e-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="d6e3e-119">**Listato 1 - HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="d6e3e-119">**Listing 1 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample1.vb)]

<span data-ttu-id="d6e3e-120">È possibile richiamare la prima azione, l'azione Index (), digitare l'URL seguente nella barra degli indirizzi del browser:</span><span class="sxs-lookup"><span data-stu-id="d6e3e-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="d6e3e-121">/ Home/Index</span><span class="sxs-lookup"><span data-stu-id="d6e3e-121">/Home/Index</span></span>

<span data-ttu-id="d6e3e-122">È possibile richiamare la seconda azione, l'azione Details(), immettendo questo indirizzo nel browser:</span><span class="sxs-lookup"><span data-stu-id="d6e3e-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="d6e3e-123">/ Home/dettagli</span><span class="sxs-lookup"><span data-stu-id="d6e3e-123">/Home/Details</span></span>

<span data-ttu-id="d6e3e-124">L'azione Index () restituisce una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-124">The Index() action returns a view.</span></span> <span data-ttu-id="d6e3e-125">La maggior parte delle azioni che creano restituirà le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-125">Most actions that you create will return views.</span></span> <span data-ttu-id="d6e3e-126">Tuttavia, un'azione può restituire altri tipi di risultati dell'azione.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="d6e3e-127">Ad esempio, l'azione Details() restituisce un RedirectToActionResult che reindirizza le richieste in ingresso per l'azione Index ().</span><span class="sxs-lookup"><span data-stu-id="d6e3e-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="d6e3e-128">L'azione Index () contiene la seguente riga di codice:</span><span class="sxs-lookup"><span data-stu-id="d6e3e-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="d6e3e-129">View()</span><span class="sxs-lookup"><span data-stu-id="d6e3e-129">View()</span></span>

<span data-ttu-id="d6e3e-130">Questa riga di codice restituisce una vista in cui deve trovarsi nel percorso seguente nel server web:</span><span class="sxs-lookup"><span data-stu-id="d6e3e-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="d6e3e-131">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="d6e3e-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="d6e3e-132">Il percorso della visualizzazione viene dedotto dal nome del controller e il nome dell'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="d6e3e-133">Se si preferisce, possono essere esplicite sulla vista.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="d6e3e-134">La riga di codice seguente restituisce una visualizzazione denominata Fred:</span><span class="sxs-lookup"><span data-stu-id="d6e3e-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="d6e3e-135">Visualizzazione (Fred)</span><span class="sxs-lookup"><span data-stu-id="d6e3e-135">View( Fred )</span></span>

<span data-ttu-id="d6e3e-136">Quando viene eseguita questa riga di codice, viene restituita una visualizzazione dal percorso seguente:</span><span class="sxs-lookup"><span data-stu-id="d6e3e-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="d6e3e-137">\Views\Home\Fred.aspx</span><span class="sxs-lookup"><span data-stu-id="d6e3e-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="d6e3e-138">Se si prevede di creare unit test per l'applicazione ASP.NET MVC è una buona idea definire esplicito i nomi delle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="d6e3e-139">In questo modo, è possibile creare uno unit test per verificare che la vista previsto è stata restituita da un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>


## <a name="adding-content-to-a-view"></a><span data-ttu-id="d6e3e-140">Aggiunta di contenuto a una vista</span><span class="sxs-lookup"><span data-stu-id="d6e3e-140">Adding Content to a View</span></span>

<span data-ttu-id="d6e3e-141">Una vista è uno standard (documento HTML che può contenere gli script X).</span><span class="sxs-lookup"><span data-stu-id="d6e3e-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="d6e3e-142">È utilizzare script per aggiungere contenuto dinamico a una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="d6e3e-143">Ad esempio, la visualizzazione nel listato 2 consente di visualizzare la data e ora correnti.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="d6e3e-144">**Listato 2 - \Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="d6e3e-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample2.aspx)]

<span data-ttu-id="d6e3e-145">Si noti che il corpo della pagina HTML nel listato 2 contiene lo script seguente:</span><span class="sxs-lookup"><span data-stu-id="d6e3e-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="d6e3e-146">&lt;% Response.Write(DateTime.Now)%&gt;</span><span class="sxs-lookup"><span data-stu-id="d6e3e-146">&lt;% Response.Write(DateTime.Now)%&gt;</span></span>

<span data-ttu-id="d6e3e-147">Si usano i delimitatori di script &lt;% e %&gt; per contrassegnare l'inizio e alla fine di uno script.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="d6e3e-148">Questo script viene scritto in Visual basic.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-148">This script is written in Visual basic.</span></span> <span data-ttu-id="d6e3e-149">Visualizza la data e ora correnti, chiamare il metodo Response per il rendering del contenuto nel browser.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="d6e3e-150">I delimitatori di script &lt;% e %&gt; può essere utilizzato per eseguire una o più istruzioni.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="d6e3e-151">Poiché si chiama spesso Response, Microsoft fornisce un collegamento è per chiamare il metodo Response.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="d6e3e-152">La visualizzazione nel listato 3 Usa i delimitatori &lt;% = % e&gt; come collegamento per la chiamata a Response.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="d6e3e-153">**Listato 3 - Views\Home\Index2.aspx**</span><span class="sxs-lookup"><span data-stu-id="d6e3e-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample3.aspx)]

<span data-ttu-id="d6e3e-154">È possibile usare qualsiasi linguaggio .NET per generare il contenuto dinamico in una vista.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="d6e3e-155">In genere, si userà Visual Basic .NET o c# per scrivere il controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-155">Normally, you�ll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="d6e3e-156">Utilizzo di helper HTML per generare Visualizza contenuto</span><span class="sxs-lookup"><span data-stu-id="d6e3e-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="d6e3e-157">Per renderne più semplice aggiungere contenuto a una vista, è possibile sfruttare i vantaggi di un elemento denominato un' *HTML Helper*.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="d6e3e-158">Un HTML Helper, è in genere, un metodo che genera una stringa.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="d6e3e-159">È possibile usare gli helper HTML per generare gli elementi HTML standard, ad esempio caselle di testo, collegamenti, elenchi a discesa e caselle di riepilogo.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="d6e3e-160">Ad esempio, la visualizzazione nel listato 4 sfrutta le tre gli helper HTML, gli helper BeginForm() TextBox() e Password(): per generare un account di accesso formano (vedere la figura 1).</span><span class="sxs-lookup"><span data-stu-id="d6e3e-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="d6e3e-161">**Listato 4 - \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="d6e3e-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample4.aspx)]


<span data-ttu-id="d6e3e-162">[![La finestra di dialogo Nuovo progetto](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d6e3e-162">[![The New Project dialog box](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="d6e3e-163">**Figura 01**: un modulo di accesso standard ([fare clic per visualizzare l'immagine con dimensioni normali](asp-net-mvc-views-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="d6e3e-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-vb/_static/image2.png))</span></span>


<span data-ttu-id="d6e3e-164">Tutti i metodi helper HTML vengono chiamati nella proprietà Html della visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="d6e3e-165">Ad esempio, si esegue il rendering una casella di testo chiamando il metodo Html.TextBox().</span><span class="sxs-lookup"><span data-stu-id="d6e3e-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="d6e3e-166">Si noti che userai i delimitatori di script &lt;% = % e&gt; quando si chiama Html.TextBox() sia Html.Password() helper.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="d6e3e-167">Questi helper è sufficiente restituiscono una stringa.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-167">These helpers simply return a string.</span></span> <span data-ttu-id="d6e3e-168">È necessario chiamare Response per eseguire il rendering di stringa al browser.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="d6e3e-169">Uso di metodi HTML Helper è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="d6e3e-170">Semplificano la vita, riducendo la quantità di codice HTML e script che è necessario scrivere.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="d6e3e-171">La visualizzazione nel listato 5 esegue il rendering esattamente dello stesso formato di visualizzazione nel listato 4 senza l'utilizzo di helper HTML.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="d6e3e-172">**Listato 5 - \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="d6e3e-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample5.aspx)]

<span data-ttu-id="d6e3e-173">È inoltre la possibilità di creare il proprio gli helper HTML.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="d6e3e-174">Ad esempio, è possibile creare un metodo helper GridView() che visualizza automaticamente un set di record del database in una tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="d6e3e-175">In questo argomento verrà esaminata in esercitazioni **creazione di helper HTML personalizzati**.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="d6e3e-176">Utilizzando i dati della visualizzazione per passare dati a una vista</span><span class="sxs-lookup"><span data-stu-id="d6e3e-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="d6e3e-177">Visualizzare i dati è possibile per passare i dati da un controller a una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="d6e3e-178">Pensare a visualizzare i dati, ad esempio un pacchetto che si invia tramite posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="d6e3e-179">Tutti i dati passati da un controller a una visualizzazione devono essere inviati tramite questo pacchetto.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="d6e3e-180">Ad esempio, il controller nel listato 6 aggiunge un messaggio per visualizzare i dati.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="d6e3e-181">**Listato 6 - ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="d6e3e-181">**Listing 6 - ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample6.vb)]

<span data-ttu-id="d6e3e-182">Il controller di proprietà ViewData rappresenta una raccolta di coppie nome / valore.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="d6e3e-183">Nel listato 6, il metodo Index () aggiunge un elemento alla raccolta di dati di visualizzazione denominata messaggio con il valore Hello World!.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="d6e3e-184">Quando la vista viene restituita dal metodo Index (), i dati di visualizzazione vengono passati automaticamente alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="d6e3e-185">La visualizzazione nel listato 7 recupera il messaggio da visualizzare i dati ed esegue il rendering il messaggio al browser.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="d6e3e-186">**Listato 7 - \Views\Product\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="d6e3e-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample7.aspx)]

<span data-ttu-id="d6e3e-187">Si noti che la vista si avvale del metodo Helper HTML Html.Encode() durante il rendering del messaggio.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="d6e3e-188">L'Helper HTML Html.Encode() codifica i caratteri speciali, ad esempio &lt; e &gt; in caratteri che possono essere visualizzati in una pagina web.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="d6e3e-189">Ogni volta che si esegue il rendering del contenuto per l'invio di un utente a un sito Web, è consigliabile codificare il contenuto per impedire attacchi injection JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="d6e3e-190">(Perché è stato creato il messaggio di noi nel ProductController, non abbiamo t realmente necessario codificare il messaggio.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-190">(Because we created the message ourselves in the ProductController, we don�t really need to encode the message.</span></span> <span data-ttu-id="d6e3e-191">Tuttavia, è un'operazione consigliabile chiamare sempre il metodo Html.Encode() quando visualizzare il contenuto recuperato da visualizzare i dati all'interno di una vista).</span><span class="sxs-lookup"><span data-stu-id="d6e3e-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="d6e3e-192">Nel listato 7, abbiamo sfruttato visualizzare i dati di passare un messaggio stringa semplice da un controller a una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="d6e3e-193">È anche possibile usare Visualizza dati per trasmettere altri tipi di dati, ad esempio una raccolta di record del database, da un controller a una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="d6e3e-194">Ad esempio, se si desidera visualizzare il contenuto della tabella Products del database in una vista, quindi si passa la raccolta di database i record nella visualizzazione dei dati.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="d6e3e-195">È possibile scegliere di passare i dati di visualizzazione fortemente tipizzata da un controller a una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="d6e3e-196">In questo argomento verrà esaminata in esercitazioni **Understanding fortemente tipizzate visualizzare i dati e viste**.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="d6e3e-197">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="d6e3e-197">Summary</span></span>

<span data-ttu-id="d6e3e-198">Questa esercitazione è fornita una breve introduzione a visualizzazioni ASP.NET MVC, visualizzare i dati e gli helper HTML.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="d6e3e-199">Nella prima sezione, è stato descritto come aggiungere nuove visualizzazioni per il progetto.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="d6e3e-200">Si è appreso che è necessario aggiungere una visualizzazione nella cartella corretta per chiamarla da un controller specifico.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="d6e3e-201">Successivamente, abbiamo discusso l'argomento di helper HTML.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="d6e3e-202">Si è appreso come helper HTML consentono di generare in modo semplice il contenuto HTML standard.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="d6e3e-203">Infine, si è appreso come sfruttare i vantaggi di visualizzare i dati per passare i dati da un controller a una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d6e3e-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d6e3e-204">[Precedente](passing-data-to-view-master-pages-cs.md)
> [Successivo](creating-custom-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d6e3e-204">[Previous](passing-data-to-view-master-pages-cs.md)
[Next](creating-custom-html-helpers-vb.md)</span></span>
