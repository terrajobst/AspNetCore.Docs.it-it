---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Guida introduttiva a OWIN e Katana | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/27/2013
ms.topic: article
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: ac0302ef1a786f6b1eef8119b3134a965f01c533
ms.sourcegitcommit: 5ab5c5f4bfdb0150f42ba84c2770eadf540cae48
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
<a name="getting-started-with-owin-and-katana"></a><span data-ttu-id="277fa-102">Guida introduttiva a OWIN e Katana</span><span class="sxs-lookup"><span data-stu-id="277fa-102">Getting Started with OWIN and Katana</span></span>
====================
<span data-ttu-id="277fa-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="277fa-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="277fa-104">[Aprire l'interfaccia Web per .NET (OWIN)](http://owin.org/) definisce un'astrazione tra i server web .NET e le applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="277fa-104">[Open Web Interface for .NET (OWIN)](http://owin.org/) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="277fa-105">Separando il server web dall'applicazione, OWIN rende più semplice creare il middleware per lo sviluppo web .NET.</span><span class="sxs-lookup"><span data-stu-id="277fa-105">By decoupling the web server from the application, OWIN makes it easier to create middleware for .NET web development.</span></span> <span data-ttu-id="277fa-106">Inoltre, OWIN semplifica porting di applicazioni web in altri host&#8212;, ad esempio, self-hosting in un servizio Windows o un altro processo.</span><span class="sxs-lookup"><span data-stu-id="277fa-106">Also, OWIN makes it easier to port web applications to other hosts&#8212;for example, self-hosting in a Windows service or other process.</span></span>

<span data-ttu-id="277fa-107">OWIN è una specifica community di proprietà, non è un'implementazione.</span><span class="sxs-lookup"><span data-stu-id="277fa-107">OWIN is a community-owned specification, not an implementation.</span></span> <span data-ttu-id="277fa-108">Il progetto Katana è un set di componenti OWIN open-source sviluppato da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="277fa-108">The Katana project is a set of open-source OWIN components developed by Microsoft.</span></span> <span data-ttu-id="277fa-109">Per una panoramica generale di OWIN e Katana, vedere [una panoramica del progetto Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="277fa-109">For a general overview of both OWIN and Katana, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="277fa-110">In questo articolo, verrà passare direttamente al codice per iniziare.</span><span class="sxs-lookup"><span data-stu-id="277fa-110">In this article, I will jump right into code to get started.</span></span>

<span data-ttu-id="277fa-111">Questa esercitazione viene utilizzato [Visual Studio 2013 versione finale candidata](https://go.microsoft.com/fwlink/?LinkId=306566), ma è anche possibile usare Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="277fa-111">This tutorial uses [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), but you can also use Visual Studio 2012.</span></span> <span data-ttu-id="277fa-112">Alcuni dei passaggi sono diversi in Visual Studio 2012, nota di seguito.</span><span class="sxs-lookup"><span data-stu-id="277fa-112">A few of the steps are different in Visual Studio 2012, which I note below.</span></span>

## <a name="host-owin-in-iis"></a><span data-ttu-id="277fa-113">Host OWIN in IIS</span><span class="sxs-lookup"><span data-stu-id="277fa-113">Host OWIN in IIS</span></span>

<span data-ttu-id="277fa-114">In questa sezione, ospitare OWIN in IIS.</span><span class="sxs-lookup"><span data-stu-id="277fa-114">In this section, we'll host OWIN in IIS.</span></span> <span data-ttu-id="277fa-115">Questa opzione offre la flessibilità e la possibilità di composizione di una pipeline OWIN con il set di funzionalità avanzata di IIS.</span><span class="sxs-lookup"><span data-stu-id="277fa-115">This option gives you the flexibility and composability of an OWIN pipeline together with the mature feature set of IIS.</span></span> <span data-ttu-id="277fa-116">Con questa opzione, l'applicazione OWIN viene eseguito nella pipeline delle richieste ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="277fa-116">Using this option, the OWIN application runs in the ASP.NET request pipeline.</span></span>

<span data-ttu-id="277fa-117">Innanzitutto, creare un nuovo progetto applicazione Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="277fa-117">First, create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="277fa-118">(In Visual Studio 2012, utilizzare il tipo di progetto applicazione Web ASP.NET vuota).</span><span class="sxs-lookup"><span data-stu-id="277fa-118">(In Visual Studio 2012, use the ASP.NET Empty Web Application project type.)</span></span>

![](getting-started-with-owin-and-katana/_static/image1.png)

<span data-ttu-id="277fa-119">Nel **nuovo progetto ASP.NET** finestra di dialogo Seleziona il **vuoto** modello.</span><span class="sxs-lookup"><span data-stu-id="277fa-119">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span>

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a><span data-ttu-id="277fa-120">Aggiungere pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="277fa-120">Add NuGet Packages</span></span>

<span data-ttu-id="277fa-121">Successivamente, aggiungere i pacchetti NuGet richiesti.</span><span class="sxs-lookup"><span data-stu-id="277fa-121">Next, add the required NuGet packages.</span></span> <span data-ttu-id="277fa-122">Dal **strumenti** dal menu **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="277fa-122">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="277fa-123">Nella finestra della Console di gestione pacchetti, digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="277fa-123">In the Package Manager Console window, type the following command:</span></span>

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a><span data-ttu-id="277fa-124">Aggiungere una classe di avvio</span><span class="sxs-lookup"><span data-stu-id="277fa-124">Add a Startup Class</span></span>

<span data-ttu-id="277fa-125">Successivamente, aggiungere una classe di avvio OWIN.</span><span class="sxs-lookup"><span data-stu-id="277fa-125">Next, add an OWIN startup class.</span></span> <span data-ttu-id="277fa-126">In Esplora soluzioni, fare clic sul progetto e selezionare **Aggiungi**, quindi selezionare **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="277fa-126">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span> <span data-ttu-id="277fa-127">Nel **Aggiungi nuovo elemento** finestra di dialogo Seleziona **classe di avvio di Owin**.</span><span class="sxs-lookup"><span data-stu-id="277fa-127">In the **Add New Item** dialog, select **Owin Startup class**.</span></span> <span data-ttu-id="277fa-128">Per ulteriori informazioni su come configurare la classe di avvio, vedere [OWIN avvio classe rilevamento](owin-startup-class-detection.md).</span><span class="sxs-lookup"><span data-stu-id="277fa-128">For more info on configuring the startup class, see [OWIN Startup Class Detection](owin-startup-class-detection.md).</span></span>

![](getting-started-with-owin-and-katana/_static/image4.png)

<span data-ttu-id="277fa-129">Aggiungere al metodo `Startup1.Configuration` il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="277fa-129">Add the following code to the `Startup1.Configuration` method:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

<span data-ttu-id="277fa-130">Questo codice aggiunge un semplice componente middleware alla pipeline OWIN, implementata come una funzione che riceve un **Microsoft.Owin.IOwinContext** istanza.</span><span class="sxs-lookup"><span data-stu-id="277fa-130">This code adds a simple piece of middleware to the OWIN pipeline, implemented as a function that receives a **Microsoft.Owin.IOwinContext** instance.</span></span> <span data-ttu-id="277fa-131">Quando il server riceve una richiesta HTTP, la pipeline OWIN richiama il middleware.</span><span class="sxs-lookup"><span data-stu-id="277fa-131">When the server receives an HTTP request, the OWIN pipeline invokes the middleware.</span></span> <span data-ttu-id="277fa-132">Il middleware imposta il tipo di contenuto per la risposta e scrive il corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="277fa-132">The middleware sets the content type for the response and writes the response body.</span></span>

> [!NOTE]
> <span data-ttu-id="277fa-133">Il modello di classe di avvio di OWIN è disponibile in Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="277fa-133">The OWIN Startup class template is available in Visual Studio 2013.</span></span> <span data-ttu-id="277fa-134">Se si utilizza Visual Studio 2012, è sufficiente aggiungere una nuova classe vuota denominata `Startup1`e incollare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="277fa-134">If you are using Visual Studio 2012, just add a new empty class named `Startup1`, and paste in the following code:</span></span>


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a><span data-ttu-id="277fa-135">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="277fa-135">Run the Application</span></span>

<span data-ttu-id="277fa-136">Premere F5 per avviare il debug.</span><span class="sxs-lookup"><span data-stu-id="277fa-136">Press F5 to begin debugging.</span></span> <span data-ttu-id="277fa-137">Verrà aperta una finestra del browser per `http://localhost:*port*/`.</span><span class="sxs-lookup"><span data-stu-id="277fa-137">Visual Studio will open a browser window to `http://localhost:*port*/`.</span></span> <span data-ttu-id="277fa-138">La pagina dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="277fa-138">The page should look like the following:</span></span>

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a><span data-ttu-id="277fa-139">OWIN indipendente in un'applicazione Console</span><span class="sxs-lookup"><span data-stu-id="277fa-139">Self-Host OWIN in a Console Application</span></span>

<span data-ttu-id="277fa-140">È facile convertire l'applicazione da IIS che ospita in modalità self-hosting in un processo personalizzato.</span><span class="sxs-lookup"><span data-stu-id="277fa-140">It's easy to convert this application from IIS hosting to self-hosting in a custom process.</span></span> <span data-ttu-id="277fa-141">Con l'hosting IIS, IIS utilizzata sia come server HTTP del processo che ospita il servizio.</span><span class="sxs-lookup"><span data-stu-id="277fa-141">With IIS hosting, IIS acts as both the HTTP server and as the process that hosts the service.</span></span> <span data-ttu-id="277fa-142">Con self-hosting, l'applicazione viene creato il processo e utilizza il **HttpListener** classe come il server HTTP.</span><span class="sxs-lookup"><span data-stu-id="277fa-142">With self-hosting, your application creates the process and uses the **HttpListener** class as the HTTP server.</span></span>

<span data-ttu-id="277fa-143">In Visual Studio, creare una nuova applicazione console.</span><span class="sxs-lookup"><span data-stu-id="277fa-143">In Visual Studio, create a new console application.</span></span> <span data-ttu-id="277fa-144">Nella finestra della Console di gestione pacchetti, digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="277fa-144">In the Package Manager Console window, type the following command:</span></span>

`Install-Package Microsoft.Owin.SelfHost -Pre`

<span data-ttu-id="277fa-145">Aggiungere un `Startup1` classe della parte 1 di questa esercitazione al progetto.</span><span class="sxs-lookup"><span data-stu-id="277fa-145">Add a `Startup1` class from part 1 of this tutorial to the project.</span></span> <span data-ttu-id="277fa-146">Non è necessario modificare questa classe.</span><span class="sxs-lookup"><span data-stu-id="277fa-146">You don't need to modify this class.</span></span>

<span data-ttu-id="277fa-147">Implementare l'applicazione `Main` metodo come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="277fa-147">Implement the application's `Main` method as follows.</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

<span data-ttu-id="277fa-148">Quando si esegue l'applicazione console, il server inizia ad ascoltare `http://localhost:9000`.</span><span class="sxs-lookup"><span data-stu-id="277fa-148">When you run the console application, the server starts listening to `http://localhost:9000`.</span></span> <span data-ttu-id="277fa-149">Se si passa a questo indirizzo in un web browser, si verrà visualizzata la pagina "Hello world".</span><span class="sxs-lookup"><span data-stu-id="277fa-149">If you navigate to this address in a web browser, you will see the "Hello world" page.</span></span>

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a><span data-ttu-id="277fa-150">Aggiungere OWIN Diagnostics</span><span class="sxs-lookup"><span data-stu-id="277fa-150">Add OWIN Diagnostics</span></span>

<span data-ttu-id="277fa-151">Il pacchetto italiano contiene middleware che intercetta le eccezioni non gestite e visualizza una pagina HTML con i dettagli dell'errore.</span><span class="sxs-lookup"><span data-stu-id="277fa-151">The Microsoft.Owin.Diagnostics package contains middleware that catches unhandled exceptions and displays an HTML page with error details.</span></span> <span data-ttu-id="277fa-152">Questo funziona pagina come pagina di errore ASP.NET che talvolta viene denominata il "[schermata giallo](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span><span class="sxs-lookup"><span data-stu-id="277fa-152">This page functions much like the ASP.NET error page that is sometimes called the "[yellow screen of death](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span></span> <span data-ttu-id="277fa-153">Ad esempio YSOD, la pagina di errore Katana è utile durante lo sviluppo, ma è consigliabile disabilitarla in modalità di produzione.</span><span class="sxs-lookup"><span data-stu-id="277fa-153">Like the YSOD, the Katana error page is useful during development, but it's a good practice to disable it in production mode.</span></span>

<span data-ttu-id="277fa-154">Per installare il pacchetto di diagnostica nel progetto, digitare il comando seguente nella finestra della Console di gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="277fa-154">To install the Diagnostics package in your project, type the following command in the Package Manager Console window:</span></span>

`install-package Microsoft.Owin.Diagnostics –Pre`

<span data-ttu-id="277fa-155">Modificare il codice del `Startup1.Configuration` metodo come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="277fa-155">Change the code in your `Startup1.Configuration` method as follows:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

<span data-ttu-id="277fa-156">Utilizzare CTRL + F5 per eseguire l'applicazione senza eseguire il debug, in modo che Visual Studio non si interrompe l'eccezione.</span><span class="sxs-lookup"><span data-stu-id="277fa-156">Now use CTRL+F5 to run the application without debugging, so that Visual Studio will not break on the exception.</span></span> <span data-ttu-id="277fa-157">L'applicazione si comporta esattamente come in precedenza, fino a quando non si passa a `http://localhost/fail`, a quel punto l'applicazione genera l'eccezione.</span><span class="sxs-lookup"><span data-stu-id="277fa-157">The application behaves the same as before, until you navigate to `http://localhost/fail`, at which point the application throws the exception.</span></span> <span data-ttu-id="277fa-158">Il middleware di pagina di errore verrà intercettare l'eccezione e visualizzare una pagina HTML con informazioni sull'errore.</span><span class="sxs-lookup"><span data-stu-id="277fa-158">The error page middleware will catch the exception and display an HTML page with information about the error.</span></span> <span data-ttu-id="277fa-159">È possibile scegliere le schede per visualizzare lo stack, stringa di query, i cookie, intestazione della richiesta e le variabili di ambiente OWIN.</span><span class="sxs-lookup"><span data-stu-id="277fa-159">You can click the tabs to see the stack, query string, cookies, request header, and OWIN environment variables.</span></span>

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a><span data-ttu-id="277fa-160">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="277fa-160">Next Steps</span></span>

- [<span data-ttu-id="277fa-161">Rilevamento della classe di avvio OWIN</span><span class="sxs-lookup"><span data-stu-id="277fa-161">OWIN Startup Class Detection</span></span>](owin-startup-class-detection.md)
- [<span data-ttu-id="277fa-162">Consente di self-hosting ASP.NET Web API OWIN</span><span class="sxs-lookup"><span data-stu-id="277fa-162">Use OWIN to Self-Host ASP.NET Web API</span></span>](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [<span data-ttu-id="277fa-163">Utilizzare OWIN per l'hosting indipendente SignalR</span><span class="sxs-lookup"><span data-stu-id="277fa-163">Use OWIN to Self-Host SignalR</span></span>](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
