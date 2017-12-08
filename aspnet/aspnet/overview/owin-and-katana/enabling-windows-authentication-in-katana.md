---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Abilitazione dell'autenticazione di Windows in Katana | Documenti Microsoft
author: MikeWasson
description: 'In questo articolo viene illustrato come abilitare l''autenticazione di Windows in Katana. Illustra due scenari: utilizzo di IIS per ospitare Katana e HttpListener per indipendente Kat...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: cc23a053fb1ba60ea84eca59e99f0e375fefc4cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="enabling-windows-authentication-in-katana"></a><span data-ttu-id="d0718-104">Abilitazione dell'autenticazione di Windows in Katana</span><span class="sxs-lookup"><span data-stu-id="d0718-104">Enabling Windows Authentication in Katana</span></span>
====================
<span data-ttu-id="d0718-105">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d0718-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="d0718-106">In questo articolo viene illustrato come abilitare l'autenticazione di Windows in Katana.</span><span class="sxs-lookup"><span data-stu-id="d0718-106">This article shows how to enable Windows Authentication in Katana.</span></span> <span data-ttu-id="d0718-107">Illustra due scenari: utilizzo di IIS per ospitare Katana e HttpListener per indipendente Katana in un processo personalizzato.</span><span class="sxs-lookup"><span data-stu-id="d0718-107">It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process.</span></span> <span data-ttu-id="d0718-108">Grazie a Chris Ross Barry Dorrans e David Matson per la revisione dell'articolo.</span><span class="sxs-lookup"><span data-stu-id="d0718-108">Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.</span></span>


<span data-ttu-id="d0718-109">Katana è l'implementazione Microsoft di [OWIN](http://owin.org/), l'interfaccia Web aperta per .NET.</span><span class="sxs-lookup"><span data-stu-id="d0718-109">Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET.</span></span> <span data-ttu-id="d0718-110">È possibile leggere un'introduzione ai OWIN e Katana [qui](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="d0718-110">You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="d0718-111">L'architettura OWIN ha diversi livelli:</span><span class="sxs-lookup"><span data-stu-id="d0718-111">The OWIN architecture has several layers:</span></span>

- <span data-ttu-id="d0718-112">Host: Gestisce il processo in cui viene eseguita la pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="d0718-112">Host: Manages the process in which the OWIN pipeline runs.</span></span>
- <span data-ttu-id="d0718-113">Server: Consente di aprire un socket di rete e in ascolto delle richieste.</span><span class="sxs-lookup"><span data-stu-id="d0718-113">Server: Opens a network socket and listens for requests.</span></span>
- <span data-ttu-id="d0718-114">Middleware: Elabora la richiesta e risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="d0718-114">Middleware: Processes the HTTP request and response.</span></span>

<span data-ttu-id="d0718-115">Katana offre attualmente due server, che supportano entrambi l'autenticazione integrata di Windows:</span><span class="sxs-lookup"><span data-stu-id="d0718-115">Katana currently provides two servers, both of which support Windows Integrated Authentication:</span></span>

- <span data-ttu-id="d0718-116">**Systemweb**.</span><span class="sxs-lookup"><span data-stu-id="d0718-116">**Microsoft.Owin.Host.SystemWeb**.</span></span> <span data-ttu-id="d0718-117">Usa IIS con la pipeline ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d0718-117">Uses IIS with the ASP.NET pipeline.</span></span>
- <span data-ttu-id="d0718-118">**HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="d0718-118">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="d0718-119">Usa [System.Net.HttpListener](https://msdn.microsoft.com/en-us/library/system.net.httplistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="d0718-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/en-us/library/system.net.httplistener.aspx).</span></span> <span data-ttu-id="d0718-120">Il server è l'opzione predefinita quando self-hosting Katana.</span><span class="sxs-lookup"><span data-stu-id="d0718-120">This server is currently the default option when self-hosting Katana.</span></span>

> [!NOTE]
> <span data-ttu-id="d0718-121">Katana non attualmente fornisce middleware OWIN per l'autenticazione di Windows, perché questa funzionalità è già disponibile nel server.</span><span class="sxs-lookup"><span data-stu-id="d0718-121">Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.</span></span>


## <a name="windows-authentication-in-iis"></a><span data-ttu-id="d0718-122">Autenticazione di Windows in IIS</span><span class="sxs-lookup"><span data-stu-id="d0718-122">Windows Authentication in IIS</span></span>

<span data-ttu-id="d0718-123">Utilizza systemweb, è semplicemente possibile abilitare l'autenticazione di Windows in IIS.</span><span class="sxs-lookup"><span data-stu-id="d0718-123">Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.</span></span>

<span data-ttu-id="d0718-124">Iniziamo creando una nuova applicazione ASP.NET, utilizzando il modello di progetto "Applicazione Web ASP.NET vuota".</span><span class="sxs-lookup"><span data-stu-id="d0718-124">Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.</span></span>

![](enabling-windows-authentication-in-katana/_static/image1.png)

<span data-ttu-id="d0718-125">Successivamente, aggiungere pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="d0718-125">Next, add NuGet packages.</span></span> <span data-ttu-id="d0718-126">Dal **strumenti** dal menu **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="d0718-126">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="d0718-127">Nella finestra della Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="d0718-127">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

<span data-ttu-id="d0718-128">Ora aggiungere una classe denominata `Startup` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="d0718-128">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

<span data-ttu-id="d0718-129">Questo è tutto che è necessario creare un'applicazione "Hello world" per OWIN, in esecuzione in IIS.</span><span class="sxs-lookup"><span data-stu-id="d0718-129">That's all you need to create a "Hello world" application for OWIN, running on IIS.</span></span> <span data-ttu-id="d0718-130">‎Premere F5 per eseguire il debug dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d0718-130">Press F5 to debug the application.</span></span> <span data-ttu-id="d0718-131">Dovrebbe essere "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="d0718-131">You should see "Hello World!"</span></span> <span data-ttu-id="d0718-132">Nella finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="d0718-132">in the browser window.</span></span>

![](enabling-windows-authentication-in-katana/_static/image2.png)

<span data-ttu-id="d0718-133">Successivamente, si verrà abilitata l'autenticazione di Windows in IIS Express.</span><span class="sxs-lookup"><span data-stu-id="d0718-133">Next, we'll enable Windows Authentication in IIS Express.</span></span> <span data-ttu-id="d0718-134">Dal **vista** dal menu **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="d0718-134">From the **View** menu, select **Properties**.</span></span> <span data-ttu-id="d0718-135">Fare clic sul nome del progetto in Esplora soluzioni per visualizzare le proprietà del progetto.</span><span class="sxs-lookup"><span data-stu-id="d0718-135">Click on the project name in Solution Explorer to view the project properties.</span></span>

<span data-ttu-id="d0718-136">Nel **proprietà** finestra impostare **l'autenticazione anonima** a **disabilitato** e impostare **l'autenticazione di Windows** per  **Abilitato**.</span><span class="sxs-lookup"><span data-stu-id="d0718-136">In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.</span></span>

![](enabling-windows-authentication-in-katana/_static/image3.png)

<span data-ttu-id="d0718-137">Quando si esegue l'applicazione da Visual Studio, IIS Express richiederà le credenziali dell'utente Windows.</span><span class="sxs-lookup"><span data-stu-id="d0718-137">When you run the application from Visual Studio, IIS Express will require the user's Windows credentials.</span></span> <span data-ttu-id="d0718-138">È possibile vedere questo utilizzando [Fiddler](http://fiddler2.com/home) o HTTP di un altro strumento di debug.</span><span class="sxs-lookup"><span data-stu-id="d0718-138">You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool.</span></span> <span data-ttu-id="d0718-139">Di seguito è riportato un esempio di risposta HTTP:</span><span class="sxs-lookup"><span data-stu-id="d0718-139">Here is an example HTTP response:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

<span data-ttu-id="d0718-140">Le intestazioni WWW-Authenticate nella risposta indicano che il server supporta il [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocollo, che utilizza Kerberos o NTLM.</span><span class="sxs-lookup"><span data-stu-id="d0718-140">The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.</span></span>

<span data-ttu-id="d0718-141">In un secondo momento, quando si distribuisce l'applicazione a un server, seguire [procedura](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) per abilitare l'autenticazione di Windows in IIS su tale server.</span><span class="sxs-lookup"><span data-stu-id="d0718-141">Later, when you deploy the application to a server, follow [these steps](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.</span></span>

## <a name="windows-authentication-in-httplistener"></a><span data-ttu-id="d0718-142">Autenticazione di Windows in HttpListener</span><span class="sxs-lookup"><span data-stu-id="d0718-142">Windows Authentication in HttpListener</span></span>

<span data-ttu-id="d0718-143">Se si utilizza HttpListener per l'hosting indipendente Katana, è possibile abilitare l'autenticazione di Windows direttamente nel **HttpListener** istanza.</span><span class="sxs-lookup"><span data-stu-id="d0718-143">If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.</span></span>

<span data-ttu-id="d0718-144">Innanzitutto, creare una nuova applicazione console.</span><span class="sxs-lookup"><span data-stu-id="d0718-144">First, create a new console application.</span></span> <span data-ttu-id="d0718-145">Successivamente, aggiungere pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="d0718-145">Next, add NuGet packages.</span></span> <span data-ttu-id="d0718-146">Dal **strumenti** dal menu **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="d0718-146">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="d0718-147">Nella finestra della Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="d0718-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

<span data-ttu-id="d0718-148">Ora aggiungere una classe denominata `Startup` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="d0718-148">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

<span data-ttu-id="d0718-149">Questa classe implementa lo stesso esempio "Hello world" dalla prima, ma imposta inoltre l'autenticazione di Windows come schema di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="d0718-149">This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.</span></span>

<span data-ttu-id="d0718-150">All'interno di `Main` di funzione, avviare la pipeline OWIN:</span><span class="sxs-lookup"><span data-stu-id="d0718-150">Inside the `Main` function, start the OWIN pipeline:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

<span data-ttu-id="d0718-151">È possibile inviare una richiesta in Fiddler per confermare che l'applicazione utilizza l'autenticazione di Windows:</span><span class="sxs-lookup"><span data-stu-id="d0718-151">You can send a request in Fiddler to confirm that the application is using Windows Authentication:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a><span data-ttu-id="d0718-152">Argomenti correlati</span><span class="sxs-lookup"><span data-stu-id="d0718-152">Related Topics</span></span>

[<span data-ttu-id="d0718-153">Una panoramica del progetto Katana</span><span class="sxs-lookup"><span data-stu-id="d0718-153">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)

[<span data-ttu-id="d0718-154">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="d0718-154">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/en-us/library/system.net.httplistener.aspx)

[<span data-ttu-id="d0718-155">Informazioni sui OWIN autenticazione basata su form in MVC 5</span><span class="sxs-lookup"><span data-stu-id="d0718-155">Understanding OWIN Forms Authentication in MVC 5</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
