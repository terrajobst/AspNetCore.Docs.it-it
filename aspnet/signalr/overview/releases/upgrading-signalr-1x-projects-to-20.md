---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: L'aggiornamento di progetti di 1. x SignalR versione 2 | Documenti Microsoft
author: pfletcher
description: In questo argomento viene descritto come aggiornare un progetto esistente di 1. x SignalR per SignalR 2. x e come risolvere i problemi che possono verificarsi durante il processo di aggiornamento...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: e372275ae5dd4bbf354db2d02e4407f8c513b7a3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a><span data-ttu-id="c1fc6-103">L'aggiornamento di progetti SignalR 1. x alla versione 2</span><span class="sxs-lookup"><span data-stu-id="c1fc6-103">Upgrading SignalR 1.x Projects to version 2</span></span>
====================
<span data-ttu-id="c1fc6-104">da [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="c1fc6-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="c1fc6-105">In questo argomento viene descritto come aggiornare un progetto esistente di 1. x SignalR per SignalR 2. x e come risolvere i problemi che possono verificarsi durante il processo di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-105">This topic describes how to upgrade an existing SignalR 1.x project to SignalR 2.x, and how to troubleshoot issues that may arise during the upgrade process.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c1fc6-106">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="c1fc6-106">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="c1fc6-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="c1fc6-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="c1fc6-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="c1fc6-108">.NET 4.5</span></span>
> - <span data-ttu-id="c1fc6-109">SignalR versioni 1 e 2</span><span class="sxs-lookup"><span data-stu-id="c1fc6-109">SignalR versions 1 and 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="c1fc6-110">Utilizzo di Visual Studio 2012 con questa esercitazione</span><span class="sxs-lookup"><span data-stu-id="c1fc6-110">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="c1fc6-111">Per utilizzare Visual Studio 2012 con questa esercitazione, effettuare le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="c1fc6-111">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="c1fc6-112">Aggiornamento del [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) alla versione più recente.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-112">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="c1fc6-113">Installare il [installazione guidata piattaforma Web](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="c1fc6-113">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="c1fc6-114">L'installazione guidata piattaforma Web, cercare e installare **ASP.NET e 2013.1 strumenti Web per Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-114">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="c1fc6-115">Verrà installato come modelli di Visual Studio per le classi di SignalR **Hub**.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-115">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="c1fc6-116">Alcuni modelli (ad esempio **classe di avvio di OWIN**) non saranno disponibili per tali file, usare un file di classe.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-116">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="c1fc6-117">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="c1fc6-117">Questions and comments</span></span>
> 
> <span data-ttu-id="c1fc6-118">Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="c1fc6-119">In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="c1fc6-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="c1fc6-120">SignalR 2 offre un'esperienza di sviluppo coerente tra le piattaforme server utilizzando [OWIN](http://owin.org).</span><span class="sxs-lookup"><span data-stu-id="c1fc6-120">SignalR 2 offers a consistent development experience across server platforms using [OWIN](http://owin.org).</span></span> <span data-ttu-id="c1fc6-121">Questo articolo descrive alcuni passaggi necessari per aggiornare un'applicazione di SignalR 1. x alla versione 2.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-121">This article describes the few steps that are needed to update a SignalR 1.x application to version 2.</span></span>

<span data-ttu-id="c1fc6-122">È consigliabile aggiornare le applicazioni a 2 SignalR, SignalR 1. x verrà comunque essere supportato.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-122">While it is encouraged to upgrade applications to SignalR 2, SignalR 1.x will still be supported.</span></span>

<span data-ttu-id="c1fc6-123">In questa esercitazione viene descritto come aggiornare un'applicazione ospitata sul web a 2 SignalR.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-123">This tutorial describes how to upgrade a web-hosted application to SignalR 2.</span></span> <span data-ttu-id="c1fc6-124">Le applicazioni indipendenti (quelli che ospitano un server in un'applicazione console, il servizio di Windows o altri processi) sono ora supportate in 2 SignalR.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-124">Self-hosted applications (those that host a server in a console application, Windows service, or other process) are now supported under SignalR 2.</span></span> <span data-ttu-id="c1fc6-125">Per informazioni su come iniziare a creare un'applicazione indipendente con 2 SignalR, vedere [esercitazione: SignalR indipendente](../deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="c1fc6-125">For information on how to get started creating a self-hosted application with SignalR 2, see [Tutorial: SignalR Self-Host](../deployment/tutorial-signalr-self-host.md).</span></span>

## <a name="contents"></a><span data-ttu-id="c1fc6-126">Contenuto</span><span class="sxs-lookup"><span data-stu-id="c1fc6-126">Contents</span></span>

<span data-ttu-id="c1fc6-127">Nelle sezioni seguenti vengono descrivono le attività necessarie per l'aggiornamento di progetti SignalR e come risolvere i problemi che possono verificarsi.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-127">The following sections describe tasks involved with upgrading SignalR projects, and how to troubleshoot issues that may arise.</span></span>

- [<span data-ttu-id="c1fc6-128">Esempio: Aggiornamento di esercitazione introduttiva su SignalR 2</span><span class="sxs-lookup"><span data-stu-id="c1fc6-128">Example: Upgrading the Getting Started tutorial to SignalR 2</span></span>](#example)
- [<span data-ttu-id="c1fc6-129">Risoluzione dei problemi rilevati errori durante l'aggiornamento</span><span class="sxs-lookup"><span data-stu-id="c1fc6-129">Troubleshooting errors encountered during upgrading</span></span>](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a><span data-ttu-id="c1fc6-130">Esempio: L'aggiornamento dell'applicazione di esercitazione Introduzione a SignalR 2</span><span class="sxs-lookup"><span data-stu-id="c1fc6-130">Example: Upgrading the Getting Started tutorial application to SignalR 2</span></span>

<span data-ttu-id="c1fc6-131">In questa sezione si aggiornerà l'applicazione creata nel [versione 1. x SignalR dell'esercitazione introduttiva](../older-versions/index.md) per usare 2 SignalR.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-131">In this section, you'll update the application created in the [SignalR 1.x version of the Getting Started Tutorial](../older-versions/index.md) to use SignalR 2.</span></span>

1. <span data-ttu-id="c1fc6-132">Dopo aver completato l'esercitazione introduttiva, pulsante destro del mouse sul progetto e selezionare **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-132">Once you've finished the Getting Started tutorial, right-click on the project, and select **Properties**.</span></span> <span data-ttu-id="c1fc6-133">Verificare che il **framework di destinazione** è impostato su **.NET Framework 4.5.**</span><span class="sxs-lookup"><span data-stu-id="c1fc6-133">Verify that the **Target framework** is set to **.NET Framework 4.5.**</span></span>
2. <span data-ttu-id="c1fc6-134">Aprire la Console di gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-134">Open the Package Manager Console.</span></span> <span data-ttu-id="c1fc6-135">Rimuovere SignalR 1. x dal progetto utilizzando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c1fc6-135">Remove SignalR 1.x from the project using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. <span data-ttu-id="c1fc6-136">Installare SignalR 2 utilizzando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c1fc6-136">Install SignalR 2 using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. <span data-ttu-id="c1fc6-137">Nella pagina HTML, aggiornare il riferimento allo script per SignalR in base alla versione dello script ora inclusa nel progetto.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-137">In the HTML page, update the script reference for SignalR to match the version of the script now included in the project.</span></span>

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. <span data-ttu-id="c1fc6-138">Nella classe di applicazione globale, rimuovere la chiamata a MapHubs.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-138">In the global application class, remove the call to MapHubs.</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. <span data-ttu-id="c1fc6-139">La soluzione e scegliere **Aggiungi**, **nuovo elemento...** . Nella finestra di dialogo selezionare **classe di avvio di Owin**.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-139">Right-click the solution, and select **Add**, **New Item...**. In the dialog, select **Owin Startup Class**.</span></span> <span data-ttu-id="c1fc6-140">Denominare la nuova classe **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-140">Name the new class **Startup.cs**.</span></span>

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. <span data-ttu-id="c1fc6-141">Sostituire il contenuto di Startup.cs con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c1fc6-141">Replace the contents of Startup.cs with the following code:</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    <span data-ttu-id="c1fc6-142">L'attributo di assembly consente di aggiungere la classe al processo di avvio di Owin, che esegue il `Configuration` metodo all'avvio di Owin.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-142">The assembly attribute adds the class to Owin's startup process, which executes the `Configuration` method when Owin starts up.</span></span> <span data-ttu-id="c1fc6-143">A sua volta chiama il `MapSignalR` metodo, che crea le route per tutti gli hub SignalR nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-143">This in turn calls the `MapSignalR` method, which creates routes for all SignalR hubs in the application.</span></span>
8. <span data-ttu-id="c1fc6-144">Eseguire il progetto e copiare l'URL della pagina principale in un altro browser o il riquadro del browser, come prima.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-144">Run the project, and copy the URL of the main page into another browser or browser pane, as before.</span></span> <span data-ttu-id="c1fc6-145">Ogni pagina richiederà l'immissione di un nome utente e i messaggi inviati da ogni pagina devono essere visibili in entrambi i riquadri del browser.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-145">Each page will ask for a username, and messages sent from each page should be visible in both browser panes.</span></span>

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a><span data-ttu-id="c1fc6-146">Risoluzione dei problemi rilevati errori durante l'aggiornamento</span><span class="sxs-lookup"><span data-stu-id="c1fc6-146">Troubleshooting errors encountered during upgrading</span></span>

<span data-ttu-id="c1fc6-147">In questa sezione vengono descritti i problemi che possono verificarsi durante l'aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-147">This section describes issues that may arise during upgrading.</span></span> <span data-ttu-id="c1fc6-148">Per un elenco più completo di errori e problemi che possono verificarsi con un'applicazione di SignalR, vedere [la risoluzione dei problemi di SignalR](../testing-and-debugging/troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="c1fc6-148">For a more comprehensive list of errors and issues that may occur with a SignalR application, see [SignalR Troubleshooting](../testing-and-debugging/troubleshooting.md).</span></span>

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a><span data-ttu-id="c1fc6-149">'La chiamata è ambigua tra i seguenti metodi o proprietà'</span><span class="sxs-lookup"><span data-stu-id="c1fc6-149">'The call is ambiguous between the following methods or properties'</span></span>

<span data-ttu-id="c1fc6-150">Questo errore si verifica se un riferimento a `Microsoft.AspNet.SignalR.Owin` non viene rimosso.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-150">This error will occur if a reference to `Microsoft.AspNet.SignalR.Owin` is not removed.</span></span> <span data-ttu-id="c1fc6-151">Questo pacchetto è stato deprecato. il riferimento deve essere rimossa e la versione 1. x del pacchetto host deve essere disinstallata.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-151">This package is deprecated; the reference must be removed and the 1.x version of the SelfHost package must be uninstalled.</span></span>

### <a name="hub-methods-fail-silently"></a><span data-ttu-id="c1fc6-152">Metodi dell'hub non venga segnalato</span><span class="sxs-lookup"><span data-stu-id="c1fc6-152">Hub methods fail silently</span></span>

<span data-ttu-id="c1fc6-153">Verificare che i riferimenti a script nel client e che il `OwinStartup` attributo per la classe di avvio è la classe corretta e nomi di assembly per il progetto.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-153">Verify that the script references in your client are up to date, and that the `OwinStartup` attribute for your Startup class has the correct class and assembly names for your project.</span></span> <span data-ttu-id="c1fc6-154">Inoltre, provare ad aprire l'indirizzo di hub (signalr/hub) nel browser; qualsiasi errore che compare offrono ulteriori informazioni su qual è il problema.</span><span class="sxs-lookup"><span data-stu-id="c1fc6-154">Also, try opening up the hubs address (/signalr/hubs) in your browser; any error that appears will offer more information about what's going wrong.</span></span>
