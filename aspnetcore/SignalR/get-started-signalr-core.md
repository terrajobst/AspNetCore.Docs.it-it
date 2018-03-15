---
title: Introduzione a SignalR su ASP.NET Core
author: rachelappel
description: Le informazioni di base della creazione di un'applicazione in tempo reale mediante SignalR per ASP.NET Core.
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/06/2018
ms.prod: aspnet-core
ms.technology: dotnet-signalr
ms.topic: tutorial
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 79af59fc8c2ada71d764ada95a431e10f4f00f27
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a><span data-ttu-id="7b2d7-103">Esercitazione: Introduzione a SignalR per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7b2d7-103">Tutorial: Get started with SignalR for ASP.NET Core</span></span>

<span data-ttu-id="7b2d7-104">[Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="7b2d7-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="7b2d7-105">In questa esercitazione illustra le nozioni di base della creazione di un'applicazione in tempo reale mediante SignalR per ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Soluzione](get-started-signalr-core/_static/signalr-get-started-finished.png)

<span data-ttu-id="7b2d7-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7b2d7-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7b2d7-108">Questa esercitazione illustra le attività di sviluppo SignalR seguenti:</span><span class="sxs-lookup"><span data-stu-id="7b2d7-108">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7b2d7-109">Creare un'app web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-109">Create an ASP.NET Core web app.</span></span>
> * <span data-ttu-id="7b2d7-110">Creare un hub di SignalR per eseguire il push del contenuto ai client.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-110">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="7b2d7-111">Utilizzare la libreria SignalR JavaScript per l'invio di messaggi e visualizza gli aggiornamenti dall'hub.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-111">Use the SignalR JavaScript library to send messages and display updates from the hub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7b2d7-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="7b2d7-112">Prerequisites</span></span>

<span data-ttu-id="7b2d7-113">Installare il software seguente:</span><span class="sxs-lookup"><span data-stu-id="7b2d7-113">Install the following software:</span></span>

* <span data-ttu-id="7b2d7-114">[.NET core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) o versione successiva</span><span class="sxs-lookup"><span data-stu-id="7b2d7-114">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* <span data-ttu-id="7b2d7-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15,6 o versione successiva con il carico di lavoro di sviluppo ASP.NET e web</span><span class="sxs-lookup"><span data-stu-id="7b2d7-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.6 or later with the ASP.NET and web development workload</span></span>
* [<span data-ttu-id="7b2d7-116">npm</span><span class="sxs-lookup"><span data-stu-id="7b2d7-116">npm</span></span>](https://www.npmjs.com/get-npm)

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="7b2d7-117">Creare un progetto ASP.NET di base che ospita i server e client SignalR</span><span class="sxs-lookup"><span data-stu-id="7b2d7-117">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

1. <span data-ttu-id="7b2d7-118">Utilizzare il **File** > **nuovo progetto** menu opzione **applicazione Web di ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-118">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="7b2d7-119">Denominare il progetto `SignalRChat`.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-119">Name the project `SignalRChat`.</span></span>

  ![Finestra di dialogo Nuovo progetto in Visual Studio](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="7b2d7-121">Selezionare **applicazione Web** per creare un progetto utilizzando le pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-121">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="7b2d7-122">Selezionare quindi **Ok**.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-122">Then select **Ok**.</span></span> <span data-ttu-id="7b2d7-123">Assicurarsi che **ASP.NET Core 2.1** è selezionata dal selettore del framework, se SignalR viene eseguito nelle versioni precedenti di .NET.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-123">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

  ![Finestra di dialogo Nuovo progetto in Visual Studio](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

  <span data-ttu-id="7b2d7-125">Le librerie che ospitano il codice lato server di SignalR sono inclusi nel modello di progetto.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-125">The libraries that host SignalR server-side code are included in the project template.</span></span> <span data-ttu-id="7b2d7-126">Installare il codice JavaScript sul lato client separatamente con [npm](https://www.npmjs.com/).</span><span class="sxs-lookup"><span data-stu-id="7b2d7-126">Install the client-side JavaScript separately with [npm](https://www.npmjs.com/).</span></span>

  ```console
   npm install @aspnet/signalr
  ```

3. <span data-ttu-id="7b2d7-127">Copia il *signalr.js* da *node_modules\\ @aspnet\signalr\dist\browser*  per il *wwwroot\lib* cartella nel progetto.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-127">Copy the *signalr.js* from *node_modules\\@aspnet\signalr\dist\browser* to the *wwwroot\lib* folder in your project.</span></span>

## <a name="create-the-signalr-hub"></a><span data-ttu-id="7b2d7-128">Creazione dell'Hub di SignalR</span><span class="sxs-lookup"><span data-stu-id="7b2d7-128">Create the SignalR Hub</span></span>

<span data-ttu-id="7b2d7-129">Un hub è una classe che funge da una pipeline di alto livello che consente al client e server chiamare i metodi tra loro.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-129">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

1. <span data-ttu-id="7b2d7-130">Aggiungere una classe al progetto scegliendo **File** > **New** > **File** e selezionando **classe Visual c#**.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-130">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> 

1. <span data-ttu-id="7b2d7-131">Ereditare da `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-131">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="7b2d7-132">La `Hub` classe contiene proprietà ed eventi per la gestione delle connessioni e gruppi, nonché inviare e ricevere dati.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-132">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

1. <span data-ttu-id="7b2d7-133">Creare il `Send` metodo che invia un messaggio a tutti i client connessi chat.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-133">Create the `Send` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="7b2d7-134">Si noti restituisce un `Task`perché SignalR è asincrono.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-134">Notice it returns a `Task`, because SignalR is asynchronous.</span></span> <span data-ttu-id="7b2d7-135">Codice asincrono è scalabile.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-135">Asynchronous code scales better.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="7b2d7-136">Configurare il progetto per l'utilizzo di SignalR</span><span class="sxs-lookup"><span data-stu-id="7b2d7-136">Configure the project to use SignalR</span></span>

<span data-ttu-id="7b2d7-137">Il server di SignalR deve essere configurato in modo da informarlo che può per passare richieste a SignalR.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-137">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="7b2d7-138">Per configurare un progetto SignalR, modificare il `ConfigureServices` metodo dell'applicazione `Startup` classe mediante l'inserimento di una chiamata a `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-138">To configure a SignalR project, modify the `ConfigureServices` method of the application's `Startup` class by inserting a call to `services.AddSignalR`.</span></span>

  <span data-ttu-id="7b2d7-139">`services.AddSignalR` Aggiunge SignalR come parte di [middleware ASP.NET Core](xref:fundamentals/middleware/index) pipeline.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-139">`services.AddSignalR` adds SignalR as part of the [ASP.NET Core middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

1. <span data-ttu-id="7b2d7-140">Configurare le route per l'hub utilizzando `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-140">Configure routes to your hubs using `UseSignalR`.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="7b2d7-141">Creare il codice client SignalR</span><span class="sxs-lookup"><span data-stu-id="7b2d7-141">Create the SignalR client code</span></span>

1. <span data-ttu-id="7b2d7-142">Sostituire il contenuto di *Pages\Index.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7b2d7-142">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  <span data-ttu-id="7b2d7-143">HTML precedente Visualizza nome e i campi dei messaggi e un pulsante di invio.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-143">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="7b2d7-144">Notare i riferimenti a script nella parte inferiore: un riferimento a SignalR e *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-144">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

1. <span data-ttu-id="7b2d7-145">Aggiungere un file JavaScript al *wwwroot\js* cartella denominata *chat.js* e aggiungervi il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7b2d7-145">Add a JavaScript file to the *wwwroot\js* folder named *chat.js* and add the following code to it:</span></span>

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="7b2d7-146">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="7b2d7-146">Run the app</span></span>

1. <span data-ttu-id="7b2d7-147">Selezionare **Debug** > **Avvia senza eseguire debug** per avviare un browser e caricare il sito Web in locale.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-147">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="7b2d7-148">Copiare l'URL dalla barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-148">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="7b2d7-149">Aprire un'altra istanza del browser (qualsiasi browser) e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-149">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="7b2d7-150">Scegliere un browser e immettere un nome e un messaggio, fare clic su di **inviare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-150">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="7b2d7-151">Il nome e il messaggio vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="7b2d7-151">The name and message are displayed on both pages instantly.</span></span>

  ![Soluzione](get-started-signalr-core/_static/signalr-get-started-finished.png)
