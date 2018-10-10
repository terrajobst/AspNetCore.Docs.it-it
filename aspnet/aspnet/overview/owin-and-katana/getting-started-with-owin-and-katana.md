---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Introduzione a OWIN e Katana | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 9920861da0e67d9304a944cacfb8ff8685267cd6
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913177"
---
<a name="getting-started-with-owin-and-katana"></a><span data-ttu-id="8e87c-102">Introduzione a OWIN e Katana</span><span class="sxs-lookup"><span data-stu-id="8e87c-102">Getting Started with OWIN and Katana</span></span>
====================
<span data-ttu-id="8e87c-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8e87c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="8e87c-104">[Open Web Interface for .NET (OWIN)](http://owin.org/) definisce un'astrazione tra i server web .NET e applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="8e87c-104">[Open Web Interface for .NET (OWIN)](http://owin.org/) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="8e87c-105">Separando il server web dell'applicazione, OWIN rende più semplice creare middleware per lo sviluppo web .NET.</span><span class="sxs-lookup"><span data-stu-id="8e87c-105">By decoupling the web server from the application, OWIN makes it easier to create middleware for .NET web development.</span></span> <span data-ttu-id="8e87c-106">Inoltre, OWIN rende più semplice per trasportare le applicazioni web in altri host&#8212;, ad esempio, self-hosting in un servizio di Windows o un altro processo.</span><span class="sxs-lookup"><span data-stu-id="8e87c-106">Also, OWIN makes it easier to port web applications to other hosts&#8212;for example, self-hosting in a Windows service or other process.</span></span>

<span data-ttu-id="8e87c-107">OWIN è una specifica proprietà della community, non è un'implementazione.</span><span class="sxs-lookup"><span data-stu-id="8e87c-107">OWIN is a community-owned specification, not an implementation.</span></span> <span data-ttu-id="8e87c-108">Il progetto Katana è un set di componenti OWIN open source sviluppato da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="8e87c-108">The Katana project is a set of open-source OWIN components developed by Microsoft.</span></span> <span data-ttu-id="8e87c-109">Per una panoramica generale di OWIN e Katana, vedere [una panoramica del progetto Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="8e87c-109">For a general overview of both OWIN and Katana, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="8e87c-110">In questo articolo, passerà a destra nel codice per iniziare.</span><span class="sxs-lookup"><span data-stu-id="8e87c-110">In this article, I will jump right into code to get started.</span></span>

<span data-ttu-id="8e87c-111">Questa esercitazione viene usato [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), ma è anche possibile usare Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="8e87c-111">This tutorial uses [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), but you can also use Visual Studio 2012.</span></span> <span data-ttu-id="8e87c-112">Alcuni dei passaggi sono diversi in Visual Studio 2012, nota di seguito.</span><span class="sxs-lookup"><span data-stu-id="8e87c-112">A few of the steps are different in Visual Studio 2012, which I note below.</span></span>

## <a name="host-owin-in-iis"></a><span data-ttu-id="8e87c-113">Hosting di OWIN in IIS</span><span class="sxs-lookup"><span data-stu-id="8e87c-113">Host OWIN in IIS</span></span>

<span data-ttu-id="8e87c-114">In questa sezione verranno ospitati in IIS OWIN.</span><span class="sxs-lookup"><span data-stu-id="8e87c-114">In this section, we'll host OWIN in IIS.</span></span> <span data-ttu-id="8e87c-115">Questa opzione offre la flessibilità e la possibilità di composizione di una pipeline OWIN insieme ai set di funzionalità avanzata di IIS.</span><span class="sxs-lookup"><span data-stu-id="8e87c-115">This option gives you the flexibility and composability of an OWIN pipeline together with the mature feature set of IIS.</span></span> <span data-ttu-id="8e87c-116">Con questa opzione, viene eseguita l'applicazione OWIN nella pipeline delle richieste ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8e87c-116">Using this option, the OWIN application runs in the ASP.NET request pipeline.</span></span>

<span data-ttu-id="8e87c-117">In primo luogo, creare un nuovo progetto di applicazione Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8e87c-117">First, create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="8e87c-118">(In Visual Studio 2012, utilizzare il tipo di progetto applicazione Web ASP.NET vuota).</span><span class="sxs-lookup"><span data-stu-id="8e87c-118">(In Visual Studio 2012, use the ASP.NET Empty Web Application project type.)</span></span>

![](getting-started-with-owin-and-katana/_static/image1.png)

<span data-ttu-id="8e87c-119">Nel **nuovo progetto ASP.NET** finestra di dialogo, seleziona la **vuota** modello.</span><span class="sxs-lookup"><span data-stu-id="8e87c-119">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span>

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a><span data-ttu-id="8e87c-120">Aggiungere i pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="8e87c-120">Add NuGet Packages</span></span>

<span data-ttu-id="8e87c-121">Successivamente, aggiungere i pacchetti NuGet necessari.</span><span class="sxs-lookup"><span data-stu-id="8e87c-121">Next, add the required NuGet packages.</span></span> <span data-ttu-id="8e87c-122">Dal **degli strumenti** dal menu **Gestione pacchetti NuGet**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="8e87c-122">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="8e87c-123">Nella finestra della Console di gestione pacchetti, digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="8e87c-123">In the Package Manager Console window, type the following command:</span></span>

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a><span data-ttu-id="8e87c-124">Aggiungere una classe di avvio</span><span class="sxs-lookup"><span data-stu-id="8e87c-124">Add a Startup Class</span></span>

<span data-ttu-id="8e87c-125">Successivamente, aggiungere una classe di avvio OWIN.</span><span class="sxs-lookup"><span data-stu-id="8e87c-125">Next, add an OWIN startup class.</span></span> <span data-ttu-id="8e87c-126">In Esplora soluzioni fare clic sul progetto e selezionare **Add**, quindi selezionare **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="8e87c-126">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span> <span data-ttu-id="8e87c-127">Nel **Aggiungi nuovo elemento** finestra di dialogo, seleziona **Owin Startup class**.</span><span class="sxs-lookup"><span data-stu-id="8e87c-127">In the **Add New Item** dialog, select **Owin Startup class**.</span></span> <span data-ttu-id="8e87c-128">Per altre informazioni su come configurare la classe startup, vedi [rilevamento della classe di avvio OWIN](owin-startup-class-detection.md).</span><span class="sxs-lookup"><span data-stu-id="8e87c-128">For more info on configuring the startup class, see [OWIN Startup Class Detection](owin-startup-class-detection.md).</span></span>

![](getting-started-with-owin-and-katana/_static/image4.png)

<span data-ttu-id="8e87c-129">Aggiungere al metodo `Startup1.Configuration` il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="8e87c-129">Add the following code to the `Startup1.Configuration` method:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

<span data-ttu-id="8e87c-130">Questo codice aggiunge un semplice componente middleware alla pipeline OWIN, implementata come funzione che riceve un **Microsoft.Owin.IOwinContext** istanza.</span><span class="sxs-lookup"><span data-stu-id="8e87c-130">This code adds a simple piece of middleware to the OWIN pipeline, implemented as a function that receives a **Microsoft.Owin.IOwinContext** instance.</span></span> <span data-ttu-id="8e87c-131">Quando il server riceve una richiesta HTTP, la pipeline OWIN richiama il middleware.</span><span class="sxs-lookup"><span data-stu-id="8e87c-131">When the server receives an HTTP request, the OWIN pipeline invokes the middleware.</span></span> <span data-ttu-id="8e87c-132">Il middleware imposta il tipo di contenuto per la risposta e scrive il corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="8e87c-132">The middleware sets the content type for the response and writes the response body.</span></span>

> [!NOTE]
> <span data-ttu-id="8e87c-133">Il modello di classe OWIN Startup è disponibile in Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="8e87c-133">The OWIN Startup class template is available in Visual Studio 2013.</span></span> <span data-ttu-id="8e87c-134">Se si usa Visual Studio 2012, è sufficiente aggiungere una nuova classe vuota denominata `Startup1`e incollare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="8e87c-134">If you are using Visual Studio 2012, just add a new empty class named `Startup1`, and paste in the following code:</span></span>


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a><span data-ttu-id="8e87c-135">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="8e87c-135">Run the Application</span></span>

<span data-ttu-id="8e87c-136">Premere F5 per avviare il debug.</span><span class="sxs-lookup"><span data-stu-id="8e87c-136">Press F5 to begin debugging.</span></span> <span data-ttu-id="8e87c-137">Visual Studio aprirà una finestra del browser a `http://localhost:*port*/`.</span><span class="sxs-lookup"><span data-stu-id="8e87c-137">Visual Studio will open a browser window to `http://localhost:*port*/`.</span></span> <span data-ttu-id="8e87c-138">La pagina dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="8e87c-138">The page should look like the following:</span></span>

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a><span data-ttu-id="8e87c-139">Self-hosting OWIN in un'applicazione Console</span><span class="sxs-lookup"><span data-stu-id="8e87c-139">Self-Host OWIN in a Console Application</span></span>

<span data-ttu-id="8e87c-140">È facile convertire questa applicazione da IIS che ospita self-hosting in un processo personalizzato.</span><span class="sxs-lookup"><span data-stu-id="8e87c-140">It's easy to convert this application from IIS hosting to self-hosting in a custom process.</span></span> <span data-ttu-id="8e87c-141">Con l'hosting di IIS, IIS agisce come sia il server HTTP e del processo che ospita il servizio.</span><span class="sxs-lookup"><span data-stu-id="8e87c-141">With IIS hosting, IIS acts as both the HTTP server and as the process that hosts the service.</span></span> <span data-ttu-id="8e87c-142">Con self-hosting, l'applicazione crea il processo e Usa il **HttpListener** classe come il server HTTP.</span><span class="sxs-lookup"><span data-stu-id="8e87c-142">With self-hosting, your application creates the process and uses the **HttpListener** class as the HTTP server.</span></span>

<span data-ttu-id="8e87c-143">In Visual Studio, creare una nuova applicazione console.</span><span class="sxs-lookup"><span data-stu-id="8e87c-143">In Visual Studio, create a new console application.</span></span> <span data-ttu-id="8e87c-144">Nella finestra della Console di gestione pacchetti, digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="8e87c-144">In the Package Manager Console window, type the following command:</span></span>

`Install-Package Microsoft.Owin.SelfHost -Pre`

<span data-ttu-id="8e87c-145">Aggiungere un `Startup1` classe della parte 1 di questa esercitazione per il progetto.</span><span class="sxs-lookup"><span data-stu-id="8e87c-145">Add a `Startup1` class from part 1 of this tutorial to the project.</span></span> <span data-ttu-id="8e87c-146">Non devi modificare questa classe.</span><span class="sxs-lookup"><span data-stu-id="8e87c-146">You don't need to modify this class.</span></span>

<span data-ttu-id="8e87c-147">Implementare l'applicazione `Main` metodo come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="8e87c-147">Implement the application's `Main` method as follows.</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

<span data-ttu-id="8e87c-148">Quando si esegue l'applicazione console, il server inizia ad ascoltare `http://localhost:9000`.</span><span class="sxs-lookup"><span data-stu-id="8e87c-148">When you run the console application, the server starts listening to `http://localhost:9000`.</span></span> <span data-ttu-id="8e87c-149">Se si passa a questo indirizzo in un web browser, si verrà visualizzata la pagina "Hello world".</span><span class="sxs-lookup"><span data-stu-id="8e87c-149">If you navigate to this address in a web browser, you will see the "Hello world" page.</span></span>

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a><span data-ttu-id="8e87c-150">Aggiungere diagnostica OWIN</span><span class="sxs-lookup"><span data-stu-id="8e87c-150">Add OWIN Diagnostics</span></span>

<span data-ttu-id="8e87c-151">Il pacchetto Microsoft.Owin.Diagnostics contiene middleware che intercetta le eccezioni non gestite e visualizza una pagina HTML con i dettagli dell'errore.</span><span class="sxs-lookup"><span data-stu-id="8e87c-151">The Microsoft.Owin.Diagnostics package contains middleware that catches unhandled exceptions and displays an HTML page with error details.</span></span> <span data-ttu-id="8e87c-152">Questo funziona pagina come pagina di errore ASP.NET che viene talvolta definita di "[giallo schermata](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span><span class="sxs-lookup"><span data-stu-id="8e87c-152">This page functions much like the ASP.NET error page that is sometimes called the "[yellow screen of death](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span></span> <span data-ttu-id="8e87c-153">Come YSOD, la pagina di errore Katana è utile durante lo sviluppo, ma è consigliabile disabilitarlo in modalità di produzione.</span><span class="sxs-lookup"><span data-stu-id="8e87c-153">Like the YSOD, the Katana error page is useful during development, but it's a good practice to disable it in production mode.</span></span>

<span data-ttu-id="8e87c-154">Per installare il pacchetto di diagnostica nel progetto, digitare il comando seguente nella finestra della Console di gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="8e87c-154">To install the Diagnostics package in your project, type the following command in the Package Manager Console window:</span></span>

`install-package Microsoft.Owin.Diagnostics –Pre`

<span data-ttu-id="8e87c-155">Modificare il codice di `Startup1.Configuration` metodo come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="8e87c-155">Change the code in your `Startup1.Configuration` method as follows:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

<span data-ttu-id="8e87c-156">A questo punto usare CTRL + F5 per eseguire l'applicazione senza debug, in modo che Visual Studio non si interrompe l'eccezione.</span><span class="sxs-lookup"><span data-stu-id="8e87c-156">Now use CTRL+F5 to run the application without debugging, so that Visual Studio will not break on the exception.</span></span> <span data-ttu-id="8e87c-157">L'applicazione si comporta esattamente come in precedenza, fino a quando non si passa a `http://localhost/fail`, a quel punto l'applicazione genera l'eccezione.</span><span class="sxs-lookup"><span data-stu-id="8e87c-157">The application behaves the same as before, until you navigate to `http://localhost/fail`, at which point the application throws the exception.</span></span> <span data-ttu-id="8e87c-158">Il middleware di pagina di errore verrà intercettare l'eccezione e visualizzare una pagina HTML con informazioni sull'errore.</span><span class="sxs-lookup"><span data-stu-id="8e87c-158">The error page middleware will catch the exception and display an HTML page with information about the error.</span></span> <span data-ttu-id="8e87c-159">È possibile fare clic sulle schede per vedere dello stack, stringa di query, cookie, intestazione di richiesta e le variabili di ambiente OWIN.</span><span class="sxs-lookup"><span data-stu-id="8e87c-159">You can click the tabs to see the stack, query string, cookies, request header, and OWIN environment variables.</span></span>

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a><span data-ttu-id="8e87c-160">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="8e87c-160">Next Steps</span></span>

- [<span data-ttu-id="8e87c-161">Rilevamento della classe di avvio OWIN</span><span class="sxs-lookup"><span data-stu-id="8e87c-161">OWIN Startup Class Detection</span></span>](owin-startup-class-detection.md)
- [<span data-ttu-id="8e87c-162">Usare OWIN per l'hosting indipendente di API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8e87c-162">Use OWIN to Self-Host ASP.NET Web API</span></span>](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [<span data-ttu-id="8e87c-163">Usare OWIN per l'hosting indipendente di SignalR</span><span class="sxs-lookup"><span data-stu-id="8e87c-163">Use OWIN to Self-Host SignalR</span></span>](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
