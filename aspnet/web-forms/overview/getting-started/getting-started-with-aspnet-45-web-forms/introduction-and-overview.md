---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Introduzione a 4,7 Web Form ASP.NET e Visual Studio 2017 | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni dettagliata insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET con Microsoft Visual Studio e ASP.NET 4.7
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: fb41ce72e9454d8d670a0b95234d2bc3f909f0ee
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341553"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a><span data-ttu-id="51a74-103">Introduzione a Web Form ASP.NET 4.5 e Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="51a74-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2017</span></span>
====================

<span data-ttu-id="51a74-104">[Scaricare progetto di esempio Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [Scarica l'E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="51a74-104">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

<span data-ttu-id="51a74-105">Questa serie di esercitazioni illustra come compilare un'applicazione Web Form ASP.NET con ASP.NET 4.5 e Microsoft Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="51a74-105">This tutorial series shows you how to build an ASP.NET Web Forms application with ASP.NET 4.5 and Microsoft Visual Studio 2017.</span></span> 

## <a name="introduction"></a><span data-ttu-id="51a74-106">Introduzione</span><span class="sxs-lookup"><span data-stu-id="51a74-106">Introduction</span></span>

<span data-ttu-id="51a74-107">Questa serie di esercitazioni illustra come creare un'applicazione Web Form ASP.NET mediante ASP.NET 4.5 e Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="51a74-107">This tutorial series guides you through creating an ASP.NET Web Forms application using Visual Studio 2017 and ASP.NET 4.5.</span></span> <span data-ttu-id="51a74-108">Si creerà un'applicazione denominata **Wingtip Toys** : un sito web StoreFront-semplificata la vendita in linea degli oggetti.</span><span class="sxs-lookup"><span data-stu-id="51a74-108">You'll create an application named **Wingtip Toys** - a simplified storefront web site selling items online.</span></span> <span data-ttu-id="51a74-109">Durante la serie, vengono evidenziate nuove funzionalità di ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="51a74-109">During the series, new ASP.NET 4.5 features are highlighted.</span></span>

### <a name="target-audience"></a><span data-ttu-id="51a74-110">Destinatari</span><span class="sxs-lookup"><span data-stu-id="51a74-110">Target audience</span></span>

<span data-ttu-id="51a74-111">Gli sviluppatori al Web Form ASP.NET sono i destinatari di questa serie di esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="51a74-111">Developers new to ASP.NET Web Forms are the target audience for this tutorial series.</span></span>

<span data-ttu-id="51a74-112">È necessario avere una certa conoscenza nelle aree seguenti:</span><span class="sxs-lookup"><span data-stu-id="51a74-112">You should have some knowledge in the following areas:</span></span>

- <span data-ttu-id="51a74-113">Programmazione orientata agli oggetti (OOP) e lingue</span><span class="sxs-lookup"><span data-stu-id="51a74-113">Object-oriented programming (OOP) and languages</span></span>
- <span data-ttu-id="51a74-114">Sviluppo di applicazioni Web (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="51a74-114">Web development (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="51a74-115">database relazionali</span><span class="sxs-lookup"><span data-stu-id="51a74-115">Relational databases</span></span>
- <span data-ttu-id="51a74-116">Architettura a più livelli</span><span class="sxs-lookup"><span data-stu-id="51a74-116">N-tier architecture</span></span>

<span data-ttu-id="51a74-117">Per esaminare queste aree, è consigliabile esaminare il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="51a74-117">To review these areas, consider studying the following content:</span></span>

- [<span data-ttu-id="51a74-118">Introduzione a Visual c#</span><span class="sxs-lookup"><span data-stu-id="51a74-118">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="51a74-119">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="51a74-119">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="51a74-120">Database relazionale</span><span class="sxs-lookup"><span data-stu-id="51a74-120">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="51a74-121">Architettura a più livelli</span><span class="sxs-lookup"><span data-stu-id="51a74-121">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="51a74-122">Funzionalità dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="51a74-122">Application features</span></span>

<span data-ttu-id="51a74-123">Le funzionalità di Web Form ASP.NET, presentate in questa serie includono:</span><span class="sxs-lookup"><span data-stu-id="51a74-123">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="51a74-124">Il progetto di applicazione Web (non il progetto sito Web)</span><span class="sxs-lookup"><span data-stu-id="51a74-124">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="51a74-125">Web Form</span><span class="sxs-lookup"><span data-stu-id="51a74-125">Web Forms</span></span>
- <span data-ttu-id="51a74-126">Pagine master, configurazione</span><span class="sxs-lookup"><span data-stu-id="51a74-126">Master Pages, Configuration</span></span>
- <span data-ttu-id="51a74-127">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="51a74-127">Bootstrap</span></span>
- <span data-ttu-id="51a74-128">Entity Framework Code First, Local DB</span><span class="sxs-lookup"><span data-stu-id="51a74-128">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="51a74-129">Convalida delle richieste</span><span class="sxs-lookup"><span data-stu-id="51a74-129">Request Validation</span></span>
- <span data-ttu-id="51a74-130">Controlli dati fortemente tipizzati</span><span class="sxs-lookup"><span data-stu-id="51a74-130">Strongly-typed Data Controls</span></span>
- <span data-ttu-id="51a74-131">Associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="51a74-131">Model Binding</span></span>
- <span data-ttu-id="51a74-132">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="51a74-132">Data Annotations</span></span>
- <span data-ttu-id="51a74-133">Provider di valori</span><span class="sxs-lookup"><span data-stu-id="51a74-133">Value Providers</span></span>
- <span data-ttu-id="51a74-134">SSL e OAuth</span><span class="sxs-lookup"><span data-stu-id="51a74-134">SSL and OAuth</span></span>
- <span data-ttu-id="51a74-135">ASP.NET Identity, la configurazione e autorizzazione</span><span class="sxs-lookup"><span data-stu-id="51a74-135">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="51a74-136">Convalida discreta</span><span class="sxs-lookup"><span data-stu-id="51a74-136">Unobtrusive Validation</span></span>
- <span data-ttu-id="51a74-137">Routing</span><span class="sxs-lookup"><span data-stu-id="51a74-137">Routing</span></span>
- <span data-ttu-id="51a74-138">Gestione degli errori di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="51a74-138">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="51a74-139">Attività e gli scenari di applicazione</span><span class="sxs-lookup"><span data-stu-id="51a74-139">Application scenarios and tasks</span></span>

<span data-ttu-id="51a74-140">Serie di esercitazioni attività includono:</span><span class="sxs-lookup"><span data-stu-id="51a74-140">Tutorial series tasks include:</span></span>

- <span data-ttu-id="51a74-141">Creazione, la revisione e l'esecuzione di un nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="51a74-141">Creating, reviewing, and running a new project</span></span>
- <span data-ttu-id="51a74-142">Creazione di una struttura di database</span><span class="sxs-lookup"><span data-stu-id="51a74-142">Creating a database structure</span></span>
- <span data-ttu-id="51a74-143">L'inizializzazione e il seeding di un database</span><span class="sxs-lookup"><span data-stu-id="51a74-143">Initializing and seeding a database</span></span>
- <span data-ttu-id="51a74-144">Personalizzazione dell'interfaccia utente con gli stili, grafica e una pagina master</span><span class="sxs-lookup"><span data-stu-id="51a74-144">Customizing the UI with styles, graphics, and a master page</span></span>
- <span data-ttu-id="51a74-145">Aggiunta di pagine e navigazione</span><span class="sxs-lookup"><span data-stu-id="51a74-145">Adding pages and navigation</span></span>
- <span data-ttu-id="51a74-146">Visualizzazione dettagli di menu e i dati del prodotto</span><span class="sxs-lookup"><span data-stu-id="51a74-146">Displaying menu details and product data</span></span>
- <span data-ttu-id="51a74-147">Creazione di un carrello acquisti</span><span class="sxs-lookup"><span data-stu-id="51a74-147">Creating a shopping cart</span></span>
- <span data-ttu-id="51a74-148">Supporto SSL aggiunta e OAuth</span><span class="sxs-lookup"><span data-stu-id="51a74-148">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="51a74-149">Aggiunta di un metodo di pagamento</span><span class="sxs-lookup"><span data-stu-id="51a74-149">Adding a payment method</span></span>
- <span data-ttu-id="51a74-150">Tra cui un ruolo di amministratore e un utente all'applicazione</span><span class="sxs-lookup"><span data-stu-id="51a74-150">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="51a74-151">Limitare l'accesso a pagine specifiche e cartella</span><span class="sxs-lookup"><span data-stu-id="51a74-151">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="51a74-152">Caricare un file all'applicazione web</span><span class="sxs-lookup"><span data-stu-id="51a74-152">Uploading a file to the web application</span></span>
- <span data-ttu-id="51a74-153">Implementazione della convalida dell'input</span><span class="sxs-lookup"><span data-stu-id="51a74-153">Implementing input validation</span></span>
- <span data-ttu-id="51a74-154">La registrazione delle route per l'applicazione web</span><span class="sxs-lookup"><span data-stu-id="51a74-154">Registering routes for the web application</span></span>
- <span data-ttu-id="51a74-155">Implementazione della gestione degli errori e registrazione degli errori</span><span class="sxs-lookup"><span data-stu-id="51a74-155">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="51a74-156">Panoramica</span><span class="sxs-lookup"><span data-stu-id="51a74-156">Overview</span></span>

<span data-ttu-id="51a74-157">Questa serie di esercitazioni è destinato a chiunque abbia familiarità con i concetti di programmazione, ma per Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="51a74-157">This tutorial series is intended for someone familiar with programming concepts, but new to ASP.NET Web Forms.</span></span> <span data-ttu-id="51a74-158">Se si ha già familiarità con Web Form ASP.NET, questa serie comunque utili sulle nuove funzionalità di ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="51a74-158">If you're already familiar with ASP.NET Web Forms, this series can still help you learn about new ASP.NET 4.5 features.</span></span> <span data-ttu-id="51a74-159">Per i lettori familiarità con la programmazione di concetti e Web Form ASP.NET, vedere le altre esercitazioni di Web Form fornite nel [introduttiva](../../../index.md) sezione sul sito Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="51a74-159">For readers unfamiliar with programming concepts and ASP.NET Web Forms, see the additional Web Forms tutorials provided in the [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="51a74-160">Di ASP.NET 4.5 forniti in questa serie di esercitazioni include le funzionalità seguenti:</span><span class="sxs-lookup"><span data-stu-id="51a74-160">The ASP.NET 4.5 provided in this tutorial series includes the following features:</span></span>

- <span data-ttu-id="51a74-161">Un'interfaccia utente semplice per la creazione di progetti che offre [supporto per molti framework di ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Form, MVC e API Web).</span><span class="sxs-lookup"><span data-stu-id="51a74-161">A simple UI for creating projects that offers [support for many ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="51a74-162">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), un layout, temi e framework progettazione reattiva.</span><span class="sxs-lookup"><span data-stu-id="51a74-162">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout, theming, and responsive design framework.</span></span>
- <span data-ttu-id="51a74-163">[ASP.NET Identity](../../../../identity/index.md), un nuovo sistema di appartenenze ASP.NET che funziona in tutti i framework di ASP.NET e funziona con software diversi da IIS di hosting web.</span><span class="sxs-lookup"><span data-stu-id="51a74-163">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- [<span data-ttu-id="51a74-164">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="51a74-164">Entity Framework 6</span></span>](https://msdn.microsoft.com/data/ef.aspx)

  <span data-ttu-id="51a74-165">Un aggiornamento a Entity Framework che consente di:</span><span class="sxs-lookup"><span data-stu-id="51a74-165">An update to the Entity Framework enabling you to:</span></span>
  - <span data-ttu-id="51a74-166">Recuperare e modificare i dati come oggetti fortemente tipizzati</span><span class="sxs-lookup"><span data-stu-id="51a74-166">Retrieve and manipulate data as strongly-typed objects</span></span>
  - <span data-ttu-id="51a74-167">Accedere ai dati in modo asincrono</span><span class="sxs-lookup"><span data-stu-id="51a74-167">Access data asynchronously</span></span>
  - <span data-ttu-id="51a74-168">Gestire gli errori di connessione temporanei</span><span class="sxs-lookup"><span data-stu-id="51a74-168">Handle transient connection faults</span></span>
  - <span data-ttu-id="51a74-169">Istruzioni di log SQL</span><span class="sxs-lookup"><span data-stu-id="51a74-169">Log SQL statements</span></span>

<span data-ttu-id="51a74-170">Per un elenco completo delle funzionalità ASP.NET 4.5, vedere [ASP.NET and Web Tools per Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="51a74-170">For a complete ASP.NET 4.5 feature list, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="51a74-171">L'applicazione di esempio Wingtip Toys</span><span class="sxs-lookup"><span data-stu-id="51a74-171">The Wingtip Toys sample application</span></span>

<span data-ttu-id="51a74-172">Gli screenshot seguenti sono compresi tra l'applicazione Web Form ASP.NET creato in questa serie di esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="51a74-172">The following screenshots are from the ASP.NET Web Forms application that you create in this tutorial series.</span></span> <span data-ttu-id="51a74-173">Quando si esegue l'applicazione in Visual Studio, viene visualizzata la Home page di web seguenti.</span><span class="sxs-lookup"><span data-stu-id="51a74-173">When you run the application in Visual Studio, the following web Home page appears.</span></span>

![Wingtip Toys - pagina predefinita](introduction-and-overview/_static/image1.png)

<span data-ttu-id="51a74-175">È possibile registrare come un nuovo utente, o accedere come un utente esistente.</span><span class="sxs-lookup"><span data-stu-id="51a74-175">You can register as a new user, or sign in as an existing user.</span></span> <span data-ttu-id="51a74-176">Barra di spostamento superiore è disponibili collegamenti a categorie di prodotti e dei loro prodotti dal database.</span><span class="sxs-lookup"><span data-stu-id="51a74-176">The top navigation has links to product categories and their products from the database.</span></span>

<span data-ttu-id="51a74-177">Se si seleziona **prodotti**, vengono visualizzati tutti i prodotti disponibili.</span><span class="sxs-lookup"><span data-stu-id="51a74-177">If you select **Products**, all available products are displayed.</span></span> 

![Wingtip Toys - prodotti](introduction-and-overview/_static/image2.png)

<span data-ttu-id="51a74-179">Se si seleziona un prodotto specifico, vengono visualizzati i dettagli del prodotto.</span><span class="sxs-lookup"><span data-stu-id="51a74-179">If you select a specific product, product details are displayed.</span></span>


![Wingtip Toys - dettagli sul prodotto](introduction-and-overview/_static/image3.png)

<span data-ttu-id="51a74-181">Come un utente, è possibile registrare e accedere con la funzionalità predefinita di modello Web Form.</span><span class="sxs-lookup"><span data-stu-id="51a74-181">As a user, you can register and sign in with Web Forms template default functionality.</span></span> <span data-ttu-id="51a74-182">Questa esercitazione illustra anche come eseguire l'accesso con un account Gmail esistente.</span><span class="sxs-lookup"><span data-stu-id="51a74-182">This tutorial also explains how to sign in using an existing Gmail account.</span></span> <span data-ttu-id="51a74-183">Inoltre, è possibile accedere come amministratore di aggiungere e rimuovere i prodotti dal database.</span><span class="sxs-lookup"><span data-stu-id="51a74-183">Additionally, you can sign in as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys - Accedi](introduction-and-overview/_static/image4.png)

<span data-ttu-id="51a74-185">Una volta che si è connessi come utente, è possibile aggiungere prodotti al carrello acquisti e completamento della transazione con PayPal.</span><span class="sxs-lookup"><span data-stu-id="51a74-185">Once you've signed in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="51a74-186">L'applicazione di esempio è progettata per funzionare nell'ambiente sandbox per gli sviluppatori di PayPal.</span><span class="sxs-lookup"><span data-stu-id="51a74-186">The sample application is designed to work in PayPal's developer sandbox.</span></span> <span data-ttu-id="51a74-187">Nessuna transazione denaro effettivamente viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="51a74-187">No actual money transaction takes place.</span></span>

![Wingtip Toys - carrello della spesa](introduction-and-overview/_static/image5.png)

<span data-ttu-id="51a74-189">Conferma le informazioni di account, l'ordine e il pagamento.</span><span class="sxs-lookup"><span data-stu-id="51a74-189">PayPal confirms your account, order, and payment information.</span></span>

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="51a74-191">Dopo la restituzione da PayPal, è possibile esaminare e completare il tuo ordine.</span><span class="sxs-lookup"><span data-stu-id="51a74-191">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys - verifica ordine](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="51a74-193">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="51a74-193">Prerequisites</span></span>

<span data-ttu-id="51a74-194">Prima di iniziare, verificare che il software seguente sia installato nel computer:</span><span class="sxs-lookup"><span data-stu-id="51a74-194">Before you start, make sure the following software is installed on your computer:</span></span>

- <span data-ttu-id="51a74-195">[Microsoft Visual Studio 2017 o Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="51a74-195">[Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).</span></span>

<span data-ttu-id="51a74-196">.NET Framework viene installato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="51a74-196">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="51a74-197">Questa serie di esercitazioni viene utilizzato Microsoft Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="51a74-197">This tutorial series uses Microsoft Visual Studio Community 2017.</span></span> <span data-ttu-id="51a74-198">È possibile usare entrambi che o Microsoft Visual Studio 2017 per il completamento di questa serie di esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="51a74-198">You can use either that or Microsoft Visual Studio 2017 to complete this tutorial series.</span></span>

<span data-ttu-id="51a74-199">Tenere presente quanto segue su Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="51a74-199">Note the following about Visual Studio:</span></span>

* <span data-ttu-id="51a74-200">Microsoft Visual Studio 2017 e Microsoft Visual Studio Community 2017 sono dette *Visual Studio* in tutta questa serie di esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="51a74-200">Microsoft Visual Studio 2017 and Microsoft Visual Studio Community 2017 are referred to as *Visual Studio* throughout this tutorial series.</span></span>

* <span data-ttu-id="51a74-201">Visual Studio 2017 viene installata accanto a tutte le versioni precedenti già installate.</span><span class="sxs-lookup"><span data-stu-id="51a74-201">Visual Studio 2017 is installed next to any older versions already installed.</span></span> <span data-ttu-id="51a74-202">I siti creati nelle versioni precedenti possono essere aperti in Visual Studio 2017 e continuano ad aprire nelle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="51a74-202">Sites created in earlier versions can be opened in Visual Studio 2017 and continue to open in previous versions.</span></span>

* <span data-ttu-id="51a74-203">La prima volta che è stato avviato Visual Studio, si presuppone che è stata selezionata la *lo sviluppo Web* impostazioni.</span><span class="sxs-lookup"><span data-stu-id="51a74-203">The first time you started Visual Studio, it is assumed you selected the *Web Development* settings.</span></span> <span data-ttu-id="51a74-204">Per altre informazioni, vedere [Procedura: Selezionare le impostazioni di ambiente di sviluppo Web](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="51a74-204">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

<span data-ttu-id="51a74-205">Dopo aver installato i prerequisiti, si è pronti per iniziare a creare il progetto Web presentato in questa serie di esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="51a74-205">After installing the prerequisites, you're ready to begin creating the Web project presented in this tutorial series.</span></span>

## <a name="download-the-sample-application"></a><span data-ttu-id="51a74-206">Scaricare l'applicazione di esempio</span><span class="sxs-lookup"><span data-stu-id="51a74-206">Download the sample application</span></span>

 <span data-ttu-id="51a74-207">È possibile scaricare l'esempio completato applicatiion in qualsiasi momento dal sito di esempi MSDN:</span><span class="sxs-lookup"><span data-stu-id="51a74-207">You can download the completed sample applicatiion at anytime from the MSDN Samples site:</span></span>

<span data-ttu-id="51a74-208">[Introduzione a Web Form ASP.NET 4.5 e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)</span><span class="sxs-lookup"><span data-stu-id="51a74-208">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

 <span data-ttu-id="51a74-209">Questo download include gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="51a74-209">This download has the following items:</span></span>

- <span data-ttu-id="51a74-210">L'applicazione di esempio nel *WingtipToys* cartella.</span><span class="sxs-lookup"><span data-stu-id="51a74-210">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="51a74-211">Le risorse usate per creare l'applicazione di esempio nel *WingtipToys-asset* cartella le *WingtipToys* cartella.</span><span class="sxs-lookup"><span data-stu-id="51a74-211">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

<span data-ttu-id="51a74-212">Il download è un *zip* file.</span><span class="sxs-lookup"><span data-stu-id="51a74-212">The download is a *.zip* file.</span></span> <span data-ttu-id="51a74-213">Per visualizzare il progetto completato creato questa serie di esercitazioni, trovare e selezionare il *C#* cartella nel file con estensione zip.</span><span class="sxs-lookup"><span data-stu-id="51a74-213">To see the completed project that this tutorial series creates, find and select the *C#* folder in the .zip file.</span></span> <span data-ttu-id="51a74-214">Salvare il C# cartella per la cartella utilizzata per lavorare con progetti di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="51a74-214">Save the C# folder to the folder you use to work with Visual Studio projects.</span></span> <span data-ttu-id="51a74-215">Per impostazione predefinita, la cartella di progetti di Visual Studio 2017 è:</span><span class="sxs-lookup"><span data-stu-id="51a74-215">By default, the Visual Studio 2017 projects folder is:</span></span>

<span data-ttu-id="51a74-216"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong></span><span class="sxs-lookup"><span data-stu-id="51a74-216"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong></span></span>

<span data-ttu-id="51a74-217">Rinominare il ***c#*** cartella in cui ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="51a74-217">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="51a74-218">Se si dispone già di una cartella denominata *WingtipToys* nella cartella dei progetti, rinominare temporaneamente la cartella esistente prima di rinominare il *c#* cartella in cui *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="51a74-218">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>

<span data-ttu-id="51a74-219">Per eseguire il progetto completato, aprire il *WingtipToys* cartella e fare doppio clic il *WingtipToys.sln* file.</span><span class="sxs-lookup"><span data-stu-id="51a74-219">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="51a74-220">Visual Studio 2017 apre il progetto.</span><span class="sxs-lookup"><span data-stu-id="51a74-220">Visual Studio 2017 opens the project.</span></span> <span data-ttu-id="51a74-221">Successivamente, fare doppio clic il *default. aspx* del file in **Esplora soluzioni** e selezionare **Visualizza nel Browser**.</span><span class="sxs-lookup"><span data-stu-id="51a74-221">Next, right-click the *Default.aspx* file in **Solution Explorer** and select **View In Browser**.</span></span>

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a><span data-ttu-id="51a74-222">Questionario per esaminare il contenuto Web Form ASP.NET</span><span class="sxs-lookup"><span data-stu-id="51a74-222">Take a ASP.NET Web Forms quiz to review content</span></span>

<span data-ttu-id="51a74-223">Dopo aver completato la serie di esercitazioni, questionario per testare le proprie conoscenze e rafforzano i concetti chiave.</span><span class="sxs-lookup"><span data-stu-id="51a74-223">After completing the tutorial series, take a quiz to test your knowledge and reinforce key concepts.</span></span> <span data-ttu-id="51a74-224">Ogni domanda fornisce una spiegazione e collegamenti a indicazioni aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="51a74-224">Each question provides an explanation and links to additional guidance.</span></span>

 * [<span data-ttu-id="51a74-225">Web Form ASP.NET Quiz</span><span class="sxs-lookup"><span data-stu-id="51a74-225">ASP.NET Web Forms Quiz</span></span>](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a><span data-ttu-id="51a74-226">I commenti e supporto dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="51a74-226">Tutorial support and comments</span></span>

<span data-ttu-id="51a74-227">Per domande e commenti, usare la sezione di domande e risposte inclusa nella [Introduzione a Web Form ASP.NET 4.5 e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) pagina di esempio.</span><span class="sxs-lookup"><span data-stu-id="51a74-227">For questions and comments, use the Q and A section included on the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span>

<span data-ttu-id="51a74-228">Sono Benvenuti i commenti su questa serie di esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="51a74-228">Comments on this tutorial series are welcome.</span></span> <span data-ttu-id="51a74-229">Quando questa serie di esercitazioni viene aggiornata, viene compiuto ogni sforzo per prendere in considerazione le correzioni o suggerimenti per possibili miglioramenti.</span><span class="sxs-lookup"><span data-stu-id="51a74-229">When this tutorial series is updated, every effort is made to consider corrections or suggestions for improvements.</span></span>

<span data-ttu-id="51a74-230">Se si verifica un errore, messaggi di errore corrispondente potrebbero generare confusione, con nessuna spiegazioni su come risolverlo.</span><span class="sxs-lookup"><span data-stu-id="51a74-230">If an error occurs, the corresponding error messages could be confusing, with no good explanation on how to fix it.</span></span> <span data-ttu-id="51a74-231">Per altre informazioni, è possibile controllare la [forum ASP.NET](https://forums.asp.net/).</span><span class="sxs-lookup"><span data-stu-id="51a74-231">For help, you can check the [ASP.NET forums](https://forums.asp.net/).</span></span> <span data-ttu-id="51a74-232">Un'altra valida fonte è la sezione di domande e risposte nel [Introduzione a Web Form ASP.NET 4.5 e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) pagina di esempio.</span><span class="sxs-lookup"><span data-stu-id="51a74-232">Another good source is the Q and A section in the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="51a74-233">avanti</span><span class="sxs-lookup"><span data-stu-id="51a74-233">Next</span></span>](create-the-project.md)
