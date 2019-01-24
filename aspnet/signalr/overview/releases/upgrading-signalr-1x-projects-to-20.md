---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: Aggiornamento dei progetti di SignalR 1.x alla versione 2 | Microsoft Docs
author: bradygaster
description: In questo argomento descrive come aggiornare un progetto 1.x SignalR esistente a SignalR 2.x e come risolvere i problemi che potrebbero verificarsi durante il processo di aggiornamento...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: a1cb4903f3cdeef70ffd0f624a3a2170f641a395
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837338"
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a><span data-ttu-id="00a69-103">L'aggiornamento di progetti SignalR 1.x alla versione 2</span><span class="sxs-lookup"><span data-stu-id="00a69-103">Upgrading SignalR 1.x Projects to version 2</span></span>
====================
<span data-ttu-id="00a69-104">da [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="00a69-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="00a69-105">In questo argomento descrive come aggiornare un progetto 1.x SignalR esistente a SignalR 2.x e come risolvere i problemi che potrebbero verificarsi durante il processo di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="00a69-105">This topic describes how to upgrade an existing SignalR 1.x project to SignalR 2.x, and how to troubleshoot issues that may arise during the upgrade process.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="00a69-106">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="00a69-106">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="00a69-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="00a69-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="00a69-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="00a69-108">.NET 4.5</span></span>
> - <span data-ttu-id="00a69-109">Versioni di SignalR 1 e 2</span><span class="sxs-lookup"><span data-stu-id="00a69-109">SignalR versions 1 and 2</span></span>
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="00a69-110">Uso di Visual Studio 2012 con questa esercitazione</span><span class="sxs-lookup"><span data-stu-id="00a69-110">Using Visual Studio 2012 with this tutorial</span></span>
>
>
> <span data-ttu-id="00a69-111">Per usare Visual Studio 2012 con questa esercitazione, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="00a69-111">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
>
> - <span data-ttu-id="00a69-112">Aggiornamento di [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) alla versione più recente.</span><span class="sxs-lookup"><span data-stu-id="00a69-112">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="00a69-113">Installare il [installazione guidata piattaforma Web](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="00a69-113">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="00a69-114">L'installazione guidata piattaforma Web, cercare e installare **ASP.NET e Web Tools 2013.1 per Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="00a69-114">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="00a69-115">Verrà installato, ad esempio modelli di Visual Studio per le classi di SignalR **Hub**.</span><span class="sxs-lookup"><span data-stu-id="00a69-115">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="00a69-116">Alcuni modelli (ad esempio **classe di avvio OWIN**) non è disponibile; per questo motivo, usare invece un file di classe.</span><span class="sxs-lookup"><span data-stu-id="00a69-116">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="00a69-117">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="00a69-117">Questions and comments</span></span>
>
> <span data-ttu-id="00a69-118">Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="00a69-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="00a69-119">Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="00a69-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="00a69-120">SignalR 2 offre un'esperienza di sviluppo coerente nelle diverse piattaforme server usando [OWIN](http://owin.org).</span><span class="sxs-lookup"><span data-stu-id="00a69-120">SignalR 2 offers a consistent development experience across server platforms using [OWIN](http://owin.org).</span></span> <span data-ttu-id="00a69-121">Questo articolo descrive i passaggi necessari per aggiornare un'applicazione di SignalR 1.x alla versione 2.</span><span class="sxs-lookup"><span data-stu-id="00a69-121">This article describes the few steps that are needed to update a SignalR 1.x application to version 2.</span></span>

<span data-ttu-id="00a69-122">Anche se è consigliabile aggiornare le applicazioni a SignalR 2, SignalR 1.x verrà comunque essere supportato.</span><span class="sxs-lookup"><span data-stu-id="00a69-122">While it is encouraged to upgrade applications to SignalR 2, SignalR 1.x will still be supported.</span></span>

<span data-ttu-id="00a69-123">Questa esercitazione descrive come aggiornare un'applicazione ospitata sul web a SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="00a69-123">This tutorial describes how to upgrade a web-hosted application to SignalR 2.</span></span> <span data-ttu-id="00a69-124">Le applicazioni self-hosted (quelli che ospitano un server in un'applicazione console, il servizio di Windows o altri processi) sono ora supportate in SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="00a69-124">Self-hosted applications (those that host a server in a console application, Windows service, or other process) are now supported under SignalR 2.</span></span> <span data-ttu-id="00a69-125">Per informazioni su come iniziare a creare un'applicazione indipendente con SignalR 2, vedere [esercitazione: Hosting indipendente di SignalR](../deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="00a69-125">For information on how to get started creating a self-hosted application with SignalR 2, see [Tutorial: SignalR Self-Host](../deployment/tutorial-signalr-self-host.md).</span></span>

## <a name="contents"></a><span data-ttu-id="00a69-126">Sommario</span><span class="sxs-lookup"><span data-stu-id="00a69-126">Contents</span></span>

<span data-ttu-id="00a69-127">Le sezioni seguenti descrivono le attività coinvolte relativi all'aggiornamento di progetti SignalR e come risolvere i problemi che potrebbero verificarsi.</span><span class="sxs-lookup"><span data-stu-id="00a69-127">The following sections describe tasks involved with upgrading SignalR projects, and how to troubleshoot issues that may arise.</span></span>

- [<span data-ttu-id="00a69-128">Esempio: L'aggiornamento dell'esercitazione Introduzione a SignalR 2</span><span class="sxs-lookup"><span data-stu-id="00a69-128">Example: Upgrading the Getting Started tutorial to SignalR 2</span></span>](#example)
- [<span data-ttu-id="00a69-129">Risoluzione degli errori rilevati durante l'aggiornamento</span><span class="sxs-lookup"><span data-stu-id="00a69-129">Troubleshooting errors encountered during upgrading</span></span>](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a><span data-ttu-id="00a69-130">Esempio: L'aggiornamento dell'applicazione di esercitazione Introduzione a SignalR 2</span><span class="sxs-lookup"><span data-stu-id="00a69-130">Example: Upgrading the Getting Started tutorial application to SignalR 2</span></span>

<span data-ttu-id="00a69-131">In questa sezione, si aggiornerà l'applicazione creata nel [versione di SignalR 1.x dell'esercitazione introduttiva](../older-versions/index.md) usare SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="00a69-131">In this section, you'll update the application created in the [SignalR 1.x version of the Getting Started Tutorial](../older-versions/index.md) to use SignalR 2.</span></span>

1. <span data-ttu-id="00a69-132">Dopo aver completato l'esercitazione introduttiva, pulsante destro del mouse sul progetto e selezionare **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="00a69-132">Once you've finished the Getting Started tutorial, right-click on the project, and select **Properties**.</span></span> <span data-ttu-id="00a69-133">Verificare che il **framework di destinazione** è impostata su **.NET Framework 4.5.**</span><span class="sxs-lookup"><span data-stu-id="00a69-133">Verify that the **Target framework** is set to **.NET Framework 4.5.**</span></span>
2. <span data-ttu-id="00a69-134">Aprire la Console di gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="00a69-134">Open the Package Manager Console.</span></span> <span data-ttu-id="00a69-135">Rimuovere SignalR 1.x dal progetto usando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="00a69-135">Remove SignalR 1.x from the project using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. <span data-ttu-id="00a69-136">Installare SignalR 2 usando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="00a69-136">Install SignalR 2 using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. <span data-ttu-id="00a69-137">Nella pagina HTML, aggiornare il riferimento allo script per SignalR in base alla versione dello script ora inclusa nel progetto.</span><span class="sxs-lookup"><span data-stu-id="00a69-137">In the HTML page, update the script reference for SignalR to match the version of the script now included in the project.</span></span>

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. <span data-ttu-id="00a69-138">Nella classe di applicazione globale, rimuovere la chiamata a MapHubs.</span><span class="sxs-lookup"><span data-stu-id="00a69-138">In the global application class, remove the call to MapHubs.</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. <span data-ttu-id="00a69-139">La soluzione e scegliere **Add**, **nuovo elemento...** . Nella finestra di dialogo, selezionare **classe di avvio Owin**.</span><span class="sxs-lookup"><span data-stu-id="00a69-139">Right-click the solution, and select **Add**, **New Item...**. In the dialog, select **Owin Startup Class**.</span></span> <span data-ttu-id="00a69-140">Denominare la nuova classe **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="00a69-140">Name the new class **Startup.cs**.</span></span>

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. <span data-ttu-id="00a69-141">Sostituire il contenuto di Startup.cs con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="00a69-141">Replace the contents of Startup.cs with the following code:</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    <span data-ttu-id="00a69-142">L'attributo assembly aggiunge la classe al processo di avvio di Owin, che viene eseguito il `Configuration` metodo all'avvio di Owin.</span><span class="sxs-lookup"><span data-stu-id="00a69-142">The assembly attribute adds the class to Owin's startup process, which executes the `Configuration` method when Owin starts up.</span></span> <span data-ttu-id="00a69-143">Ciò a sua volta chiama il `MapSignalR` metodo, che consente di creare le route per tutti gli hub SignalR nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="00a69-143">This in turn calls the `MapSignalR` method, which creates routes for all SignalR hubs in the application.</span></span>
8. <span data-ttu-id="00a69-144">Eseguire il progetto e copiare l'URL della pagina principale in un altro browser o nel riquadro del browser, come in precedenza.</span><span class="sxs-lookup"><span data-stu-id="00a69-144">Run the project, and copy the URL of the main page into another browser or browser pane, as before.</span></span> <span data-ttu-id="00a69-145">Ogni pagina chiederà un nome utente e i messaggi inviati da ogni pagina devono essere visibili in entrambi i riquadri del browser.</span><span class="sxs-lookup"><span data-stu-id="00a69-145">Each page will ask for a username, and messages sent from each page should be visible in both browser panes.</span></span>

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a><span data-ttu-id="00a69-146">Risoluzione degli errori rilevati durante l'aggiornamento</span><span class="sxs-lookup"><span data-stu-id="00a69-146">Troubleshooting errors encountered during upgrading</span></span>

<span data-ttu-id="00a69-147">In questa sezione vengono descritti i problemi che potrebbero verificarsi durante l'aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="00a69-147">This section describes issues that may arise during upgrading.</span></span> <span data-ttu-id="00a69-148">Per un elenco più completo di errori e problemi che possono verificarsi con un'applicazione di SignalR, vedere [risoluzione dei problemi di SignalR](../testing-and-debugging/troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="00a69-148">For a more comprehensive list of errors and issues that may occur with a SignalR application, see [SignalR Troubleshooting](../testing-and-debugging/troubleshooting.md).</span></span>

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a><span data-ttu-id="00a69-149">'È ambigua tra i seguenti metodi o proprietà chiamata'</span><span class="sxs-lookup"><span data-stu-id="00a69-149">'The call is ambiguous between the following methods or properties'</span></span>

<span data-ttu-id="00a69-150">Questo errore si verifica se un riferimento a `Microsoft.AspNet.SignalR.Owin` non viene rimosso.</span><span class="sxs-lookup"><span data-stu-id="00a69-150">This error will occur if a reference to `Microsoft.AspNet.SignalR.Owin` is not removed.</span></span> <span data-ttu-id="00a69-151">Questo pacchetto è deprecato. il riferimento deve essere rimossa e la versione 1.x del pacchetto SelfHost deve essere disinstallata.</span><span class="sxs-lookup"><span data-stu-id="00a69-151">This package is deprecated; the reference must be removed and the 1.x version of the SelfHost package must be uninstalled.</span></span>

### <a name="hub-methods-fail-silently"></a><span data-ttu-id="00a69-152">I metodi dell'hub interrompersi senza avvisi</span><span class="sxs-lookup"><span data-stu-id="00a69-152">Hub methods fail silently</span></span>

<span data-ttu-id="00a69-153">Verificare che i riferimenti a script nel client vengono aggiornate e che il `OwinStartup` attributo per la classe Startup ha la classe corretto e nomi di assembly per il progetto.</span><span class="sxs-lookup"><span data-stu-id="00a69-153">Verify that the script references in your client are up to date, and that the `OwinStartup` attribute for your Startup class has the correct class and assembly names for your project.</span></span> <span data-ttu-id="00a69-154">Inoltre, provare ad aprire l'indirizzo di hub (hub signalr /) nel browser; qualsiasi errore che compare offrono altre informazioni su cosa sta succedendo errato.</span><span class="sxs-lookup"><span data-stu-id="00a69-154">Also, try opening up the hubs address (/signalr/hubs) in your browser; any error that appears will offer more information about what's going wrong.</span></span>
