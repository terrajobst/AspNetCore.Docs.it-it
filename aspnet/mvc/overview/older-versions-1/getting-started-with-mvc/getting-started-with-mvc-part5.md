---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Accesso ai dati del modello da un Controller | Documenti Microsoft
author: shanselman
description: Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Creare un'applicazione web semplice la lettura e scrittura da un database.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 2ba1b73f40a920e27e4a03d9f703e62054d3f25c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="1ee68-104">Accesso ai dati del modello da un Controller</span><span class="sxs-lookup"><span data-stu-id="1ee68-104">Accessing your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="1ee68-105">da [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="1ee68-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="1ee68-106">Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1ee68-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="1ee68-107">Si creerà un'applicazione web semplice la lettura e scrittura da un database.</span><span class="sxs-lookup"><span data-stu-id="1ee68-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="1ee68-108">Visitare il [area risorse di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.</span><span class="sxs-lookup"><span data-stu-id="1ee68-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="1ee68-109">In questa sezione verrà per creare una nuova classe MoviesController e scrivere codice che recupera i dati di film e visualizzarlo nel browser utilizzando un modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="1ee68-109">In this section we are going to create a new MoviesController class, and write some code that retrieves our Movie data and displays it back to the browser using a View template.</span></span>

<span data-ttu-id="1ee68-110">Fare clic con il pulsante destro sulla cartella controller e rendere un nuovo MoviesController.</span><span class="sxs-lookup"><span data-stu-id="1ee68-110">Right click on the Controllers folder and make a new MoviesController.</span></span>

<span data-ttu-id="1ee68-111">[![Aggiungi Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1ee68-111">[![Add Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span></span>

<span data-ttu-id="1ee68-112">Verrà creato un nuovo file "MoviesController.cs" sotto la cartella \Controllers all'interno del progetto.</span><span class="sxs-lookup"><span data-stu-id="1ee68-112">This will create a new "MoviesController.cs" file underneath our \Controllers folder within our project.</span></span> <span data-ttu-id="1ee68-113">Si aggiorna il MovieController per recuperare l'elenco di film dal database appena compilato.</span><span class="sxs-lookup"><span data-stu-id="1ee68-113">Let's update the MovieController to retrieve the list of movies from our newly populated database.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

<span data-ttu-id="1ee68-114">Stiamo eseguendo una query LINQ in modo che è possibile recuperare solo filmati rilasciati dopo estate del 1984.</span><span class="sxs-lookup"><span data-stu-id="1ee68-114">We are performing a LINQ query so that we only retrieve movies released after the summer of 1984.</span></span> <span data-ttu-id="1ee68-115">È necessario un modello di visualizzazione per il rendering di questo elenco di film nuovamente, pertanto nel metodo e scegliere Aggiungi visualizzazione per la sua creazione.</span><span class="sxs-lookup"><span data-stu-id="1ee68-115">We'll need a View template to render this list of movies back, so right-click in the method and select Add View to create it.</span></span>

<span data-ttu-id="1ee68-116">Nella finestra di dialogo Aggiungi visualizzazione è verrà indicato che viene passato un elenco&lt;Movies.Models.Movie&gt; per il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="1ee68-116">Within the Add View dialog we'll indicate that we are passing a List&lt;Movies.Models.Movie&gt; to our View template.</span></span> <span data-ttu-id="1ee68-117">A differenza di ore precedenti è utilizzata la finestra di dialogo Aggiungi visualizzazione e si sceglie di creare un modello "Vuoto", questa volta che è possibile indicare che si vuole che Visual Studio per automaticamente "lo scaffolding" un modello di visualizzazione per noi con contenuto predefinito.</span><span class="sxs-lookup"><span data-stu-id="1ee68-117">Unlike the previous times we used the Add View dialog and chose to create an "Empty" template, this time we'll indicate that we want Visual Studio to automatically "scaffold" a view template for us with some default content.</span></span> <span data-ttu-id="1ee68-118">È questo scopo, selezionare l'elemento "List" in "menu a discesa di contenuto di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="1ee68-118">We'll do this by selecting the "List" item within the "View content dropdown menu.</span></span>

<span data-ttu-id="1ee68-119">Tenere presente che dopo avere creato una nuova classe, è necessario compilare l'applicazione per poter essere visualizzato nella finestra di dialogo Aggiungi visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="1ee68-119">Remember, when you have a created a new class you'll need to compile your application for it to show up in the Add View Dialog.</span></span>

![Aggiungi visualizzazione](getting-started-with-mvc-part5/_static/image3.png)

<span data-ttu-id="1ee68-121">Fare clic su Aggiungi e il sistema genera automaticamente il codice per una vista per noi che consente di visualizzare l'elenco di film.</span><span class="sxs-lookup"><span data-stu-id="1ee68-121">Click add and the system will automatically generate the code for a View for us that displays our list of movies.</span></span> <span data-ttu-id="1ee68-122">Ciò è utile per modificare il &lt;h2&gt; diretto al simile al seguente "Elenco di film personali", come abbiamo fatto in precedenza con la visualizzazione di Hello World.</span><span class="sxs-lookup"><span data-stu-id="1ee68-122">This is a good time to change the &lt;h2&gt; heading to something like "My Movie List" like we did earlier with the Hello World view.</span></span>

<span data-ttu-id="1ee68-123">[![Filmati - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="1ee68-123">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span></span>

<span data-ttu-id="1ee68-124">Eseguire l'applicazione e visitare /Movies nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="1ee68-124">Run your application and visit /Movies in the address bar.</span></span> <span data-ttu-id="1ee68-125">Ora progettuali recuperati dati dal database di utilizzando una query di base all'interno del Controller e i dati restituiti a una vista che viene a conoscenza film.</span><span class="sxs-lookup"><span data-stu-id="1ee68-125">Now we've retrieved data from the database using a basic query inside the Controller and returned the data to a View that knows about Movies.</span></span> <span data-ttu-id="1ee68-126">Tale visualizzazione attraversa l'elenco di film quindi crea una tabella di dati per Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1ee68-126">That View then spins through the list of Movies and creates a table of data for us.</span></span>

<span data-ttu-id="1ee68-127">[![Elenco di film - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="1ee68-127">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span></span>

<span data-ttu-id="1ee68-128">È non implementa la funzionalità di modifica, dettagli e Delete con questa applicazione - è necessario il collegamento predefinito che il modello di scaffolding creato.</span><span class="sxs-lookup"><span data-stu-id="1ee68-128">We won't be implementing Edit, Details and Delete functionality with this application - so we don't need the default links that the scaffold template created for us.</span></span> <span data-ttu-id="1ee68-129">Aprire il file /Movies/Index.aspx e rimuoverli.</span><span class="sxs-lookup"><span data-stu-id="1ee68-129">Open up the /Movies/Index.aspx file and remove them.</span></span>

<span data-ttu-id="1ee68-130">Ecco il codice sorgente per il modello di visualizzazione aggiornato aspetto dopo aver effettuato le modifiche:</span><span class="sxs-lookup"><span data-stu-id="1ee68-130">Here is the source code for what our updated View template should look like once we make these changes:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

<span data-ttu-id="1ee68-131">La creazione collegamenti non necessari, pertanto è verranno eliminati in questo esempio.</span><span class="sxs-lookup"><span data-stu-id="1ee68-131">It's creating links that we won't need, so we'll delete them for this example.</span></span> <span data-ttu-id="1ee68-132">Si manterrà il nuovo collegamento, tuttavia, che è successiva!</span><span class="sxs-lookup"><span data-stu-id="1ee68-132">We will keep our Create New link though, as that's next!</span></span> <span data-ttu-id="1ee68-133">Ecco l'aspetto dell'app a tale colonna rimossa.</span><span class="sxs-lookup"><span data-stu-id="1ee68-133">Here's what our app looks like with that column removed.</span></span>

<span data-ttu-id="1ee68-134">[![Elenco di film - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="1ee68-134">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span></span>

<span data-ttu-id="1ee68-135">È ora disponibile un elenco semplice dei dati del film.</span><span class="sxs-lookup"><span data-stu-id="1ee68-135">We now have a simple listing of our movie data.</span></span> <span data-ttu-id="1ee68-136">Tuttavia, se si fa clic sul collegamento "Crea nuovo", si verrà generato un errore perché non è collegato.</span><span class="sxs-lookup"><span data-stu-id="1ee68-136">However, if we click the "Create New" link, we'll get an error as it's not hooked up!</span></span> <span data-ttu-id="1ee68-137">Si implementa un metodo di azione Create e consentire agli utenti di immettere nuovi film nel database.</span><span class="sxs-lookup"><span data-stu-id="1ee68-137">Let's implement a Create Action method and enable a user to enter new movies in our database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1ee68-138">[Precedente](getting-started-with-mvc-part4.md)
> [Successivo](getting-started-with-mvc-part6.md)</span><span class="sxs-lookup"><span data-stu-id="1ee68-138">[Previous](getting-started-with-mvc-part4.md)
[Next](getting-started-with-mvc-part6.md)</span></span>
