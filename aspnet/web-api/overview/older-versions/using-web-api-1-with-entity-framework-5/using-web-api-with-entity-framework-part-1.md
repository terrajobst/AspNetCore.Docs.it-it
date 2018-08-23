---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Parte 1: Panoramica e creazione del progetto | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0e4021402e8deccd2395f23b6b512679b5e9d281
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836179"
---
<a name="part-1-overview-and-creating-the-project"></a><span data-ttu-id="68965-102">Parte 1: Panoramica e creazione del progetto</span><span class="sxs-lookup"><span data-stu-id="68965-102">Part 1: Overview and Creating the Project</span></span>
====================
<span data-ttu-id="68965-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="68965-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="68965-104">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="68965-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

<span data-ttu-id="68965-105">Entity Framework è un framework di mapping relazionale a oggetti /.</span><span class="sxs-lookup"><span data-stu-id="68965-105">Entity Framework is an object/relational mapping framework.</span></span> <span data-ttu-id="68965-106">Viene eseguito il mapping di oggetti di dominio nel codice le entità in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="68965-106">It maps the domain objects in your code to entities in a relational database.</span></span> <span data-ttu-id="68965-107">Nella maggior parte, non è necessario preoccuparsi che il livello di database, perché Entity Framework si occupa di esso per te.</span><span class="sxs-lookup"><span data-stu-id="68965-107">For the most part, you do not have to worry about the database layer, because Entity Framework takes care of it for you.</span></span> <span data-ttu-id="68965-108">Il codice lo modifica gli oggetti e le modifiche vengono mantenute per un database.</span><span class="sxs-lookup"><span data-stu-id="68965-108">Your code manipulates the objects, and changes are persisted to a database.</span></span>

## <a name="about-the-tutorial"></a><span data-ttu-id="68965-109">In merito all'esercitazione</span><span class="sxs-lookup"><span data-stu-id="68965-109">About the Tutorial</span></span>

<span data-ttu-id="68965-110">In questa esercitazione si creerà un'applicazione semplice archivio.</span><span class="sxs-lookup"><span data-stu-id="68965-110">In this tutorial, you will create a simple store application.</span></span> <span data-ttu-id="68965-111">Esistono due parti principali per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="68965-111">There are two main parts to the application.</span></span> <span data-ttu-id="68965-112">Gli utenti normali possono visualizzare i prodotti e creano gli ordini:</span><span class="sxs-lookup"><span data-stu-id="68965-112">Normal users can view products and create orders:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

<span data-ttu-id="68965-113">Gli amministratori possono creare, eliminare o modificare i prodotti:</span><span class="sxs-lookup"><span data-stu-id="68965-113">Administrators can create, delete, or edit products:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="68965-114">Competenze</span><span class="sxs-lookup"><span data-stu-id="68965-114">Skills You'll Learn</span></span>

<span data-ttu-id="68965-115">Ecco cosa si apprenderà:</span><span class="sxs-lookup"><span data-stu-id="68965-115">Here's what you'll learn:</span></span>

- <span data-ttu-id="68965-116">Come usare Entity Framework con ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="68965-116">How to use Entity Framework with ASP.NET Web API.</span></span>
- <span data-ttu-id="68965-117">Come usare Knockout. js per creare un client dell'interfaccia utente dinamico.</span><span class="sxs-lookup"><span data-stu-id="68965-117">How to use knockout.js to create a dynamic client UI.</span></span>
- <span data-ttu-id="68965-118">Come usare l'autenticazione basata su form con l'API Web per autenticare gli utenti.</span><span class="sxs-lookup"><span data-stu-id="68965-118">How to use forms authentication with Web API to authenticate users.</span></span>

<span data-ttu-id="68965-119">Sebbene in questa esercitazione è autonoma, è possibile leggere prima di tutto le esercitazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="68965-119">Although this tutorial is self-contained, you might want to read the following tutorials first:</span></span>

- [<span data-ttu-id="68965-120">Prima API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="68965-120">Your First ASP.NET Web API</span></span>](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [<span data-ttu-id="68965-121">Creazione di un'API Web che supporta operazioni CRUD</span><span class="sxs-lookup"><span data-stu-id="68965-121">Creating a Web API that Supports CRUD Operations</span></span>](../creating-a-web-api-that-supports-crud-operations.md)

<span data-ttu-id="68965-122">Conoscenza degli [ASP.NET MVC](../../../../mvc/index.md) è inoltre utile.</span><span class="sxs-lookup"><span data-stu-id="68965-122">Some knowledge of [ASP.NET MVC](../../../../mvc/index.md) is also helpful.</span></span>

## <a name="overview"></a><span data-ttu-id="68965-123">Panoramica</span><span class="sxs-lookup"><span data-stu-id="68965-123">Overview</span></span>

<span data-ttu-id="68965-124">A livello generale, ecco l'architettura dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="68965-124">At a high level, here is the architecture of the application:</span></span>

- <span data-ttu-id="68965-125">ASP.NET MVC genera le pagine HTML per il client.</span><span class="sxs-lookup"><span data-stu-id="68965-125">ASP.NET MVC generates the HTML pages for the client.</span></span>
- <span data-ttu-id="68965-126">API Web ASP.NET espone le operazioni CRUD sui dati (i prodotti e ordini).</span><span class="sxs-lookup"><span data-stu-id="68965-126">ASP.NET Web API exposes CRUD operations on the data (products and orders).</span></span>
- <span data-ttu-id="68965-127">Entity Framework traduce i modelli c# usati dalle API Web nelle entità di database.</span><span class="sxs-lookup"><span data-stu-id="68965-127">Entity Framework translates the C# models used by Web API into database entities.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

<span data-ttu-id="68965-128">Il diagramma seguente illustra come gli oggetti di dominio sono rappresentati a vari livelli dell'applicazione: il livello di database, il modello a oggetti e infine il formato wire, che consente di trasmettere i dati al client tramite HTTP.</span><span class="sxs-lookup"><span data-stu-id="68965-128">The following diagram shows how the domain objects are represented at various layers of the application: The database layer, the object model, and finally the wire format, which is used to transmit data to the client via HTTP.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="68965-129">Creare il progetto di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="68965-129">Create the Visual Studio Project</span></span>

<span data-ttu-id="68965-130">È possibile creare il progetto esercitazione utilizzando la versione completa di Visual Studio o Visual Web Developer Express.</span><span class="sxs-lookup"><span data-stu-id="68965-130">You can create the tutorial project using either Visual Web Developer Express or the full version of Visual Studio.</span></span>

<span data-ttu-id="68965-131">Dal **avviare** pagina, fare clic su **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="68965-131">From the **Start** page, click **New Project**.</span></span>

<span data-ttu-id="68965-132">Nel **modelli** riquadro, selezionare **modelli installati** ed espandere le **Visual c#** nodo.</span><span class="sxs-lookup"><span data-stu-id="68965-132">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="68965-133">Sotto **Visual c#**, selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="68965-133">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="68965-134">Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="68965-134">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="68965-135">Denominare il progetto "ProductStore" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="68965-135">Name the project "ProductStore" and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

<span data-ttu-id="68965-136">Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo, seleziona **applicazione Internet** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="68965-136">In the **New ASP.NET MVC 4 Project** dialog, select **Internet Application** and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

<span data-ttu-id="68965-137">Il modello "Applicazione Internet" consente di creare un'applicazione MVC ASP.NET che supporta l'autenticazione basata su form.</span><span class="sxs-lookup"><span data-stu-id="68965-137">The "Internet Application" template creates an ASP.NET MVC application that supports forms authentication.</span></span> <span data-ttu-id="68965-138">Se si esegue l'applicazione a questo punto, ha già alcune funzionalità:</span><span class="sxs-lookup"><span data-stu-id="68965-138">If you run the application now, it already has some features:</span></span>

- <span data-ttu-id="68965-139">Nuovi utenti possono registrare facendo clic sul collegamento "Register" nell'angolo superiore destro.</span><span class="sxs-lookup"><span data-stu-id="68965-139">New users can register by clicking the "Register" link in the upper right corner.</span></span>
- <span data-ttu-id="68965-140">Gli utenti registrati possono accedere facendo clic sul collegamento "Accedi".</span><span class="sxs-lookup"><span data-stu-id="68965-140">Registered users can log in by clicking the "Log in" link.</span></span>

<span data-ttu-id="68965-141">Le informazioni di appartenenza sono persistente in un database che viene creato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="68965-141">Membership information is persisted in a database that gets created automatically.</span></span> <span data-ttu-id="68965-142">Per altre informazioni sull'autenticazione basata su form in ASP.NET MVC, vedere [procedura dettagliata: uso autenticazione basata su form in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span><span class="sxs-lookup"><span data-stu-id="68965-142">For more information about forms authentication in ASP.NET MVC, see [Walkthrough: Using Forms Authentication in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span></span>

## <a name="update-the-css-file"></a><span data-ttu-id="68965-143">Aggiornare il File CSS</span><span class="sxs-lookup"><span data-stu-id="68965-143">Update the CSS File</span></span>

<span data-ttu-id="68965-144">Questo passaggio è generico; serve, ma verranno effettuate le pagine di eseguire il rendering, come le schermate precedenti.</span><span class="sxs-lookup"><span data-stu-id="68965-144">This step is cosmetic, but it will make the pages render like the earlier screen shots.</span></span>

<span data-ttu-id="68965-145">In Esplora soluzioni espandere la cartella del contenuto e aprire il file denominato CSS.</span><span class="sxs-lookup"><span data-stu-id="68965-145">In Solution Explorer, expand the Content folder and open the file named Site.css.</span></span> <span data-ttu-id="68965-146">Aggiungere gli stili CSS seguenti:</span><span class="sxs-lookup"><span data-stu-id="68965-146">Add the following CSS styles:</span></span>

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [<span data-ttu-id="68965-147">avanti</span><span class="sxs-lookup"><span data-stu-id="68965-147">Next</span></span>](using-web-api-with-entity-framework-part-2.md)
