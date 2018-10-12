---
title: Introduzione ad ASP.NET Core
author: rick-anderson
description: Un'esercitazione rapida per creare ed eseguire una semplice app Hello World usando ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: a6a5023594aec01370143e7d1f35fb45c109122a
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/01/2018
ms.locfileid: "47860940"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="32981-103">Introduzione ad ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="32981-103">Get started with ASP.NET Core</span></span>

<span data-ttu-id="32981-104">Questo documento illustra la procedura per la creazione e l'esecuzione di un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="32981-104">This document provides steps for creating and running an ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="32981-105">Installare [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="32981-105">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="32981-106">Creare un progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="32981-106">Create an ASP.NET Core project.</span></span> <span data-ttu-id="32981-107">Aprire una shell dei comandi e immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="32981-107">Open a command shell and enter the following command:</span></span>

   ```console
   dotnet new webapp -o aspnetcoreapp
   ```

3. <span data-ttu-id="32981-108">Considerare attendibile il certificato di sviluppo HTTPS:</span><span class="sxs-lookup"><span data-stu-id="32981-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="32981-109">Windows</span><span class="sxs-lookup"><span data-stu-id="32981-109">Windows</span></span>](#tab/windows)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="32981-110">Il comando precedente consente di visualizzare la finestra di dialogo seguente:</span><span class="sxs-lookup"><span data-stu-id="32981-110">The preceding command displays the following dialog:</span></span>

  ![Finestra di dialogo Avviso di sicurezza](_static/cert.png)

  <span data-ttu-id="32981-112">Selezionare **Sì** se si accetta di considerare attendibile il certificato di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="32981-112">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="32981-113">macOS</span><span class="sxs-lookup"><span data-stu-id="32981-113">macOS</span></span>](#tab/macos)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="32981-114">Il comando precedente consente di visualizzare il messaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="32981-114">The preceding command displays the following message:</span></span>

  <span data-ttu-id="32981-115">*Trusting the HTTPS development certificate was requested (È stato richiesto di considerare attendibile il certificato di sviluppo HTTPS). Se il certificato non è già stato considerato attendibile, il comando seguente non viene eseguito:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="32981-115">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>  
  <span data-ttu-id="32981-116">\*Questo comando potrebbe richiedere la password per installare il certificato nella keychain di sistema.</span><span class="sxs-lookup"><span data-stu-id="32981-116">\*This command might prompt you for your password to install the certificate on the system keychain.</span></span>
  
  <span data-ttu-id="32981-117">Password:\*</span><span class="sxs-lookup"><span data-stu-id="32981-117">Password:\*</span></span>

  <span data-ttu-id="32981-118">Inserire la password se si accetta di considerare attendibile il certificato di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="32981-118">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="32981-119">Linux</span><span class="sxs-lookup"><span data-stu-id="32981-119">Linux</span></span>](#tab/linux)

  <span data-ttu-id="32981-120">Vedere la documentazione della distribuzione di Linux su come rendere attendibile il certificato di sviluppo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="32981-120">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>
   
---

4. <span data-ttu-id="32981-121">Eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="32981-121">Run the app:</span></span>

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

5. <span data-ttu-id="32981-122">Passare a [http://localhost:5001](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="32981-122">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="32981-123">Fare clic su **Accept** (Accetto) per accettare l'informativa sulla privacy e la policy per i cookie.</span><span class="sxs-lookup"><span data-stu-id="32981-123">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="32981-124">Questa app non memorizza informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="32981-124">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="32981-125">Aprire *Pages/About.cshtml* e modificare la pagina con il markup evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="32981-125">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="32981-126">Passare a [http://localhost:5001/About](http://localhost:5001/About) e verificare che le modifiche siano visualizzate.</span><span class="sxs-lookup"><span data-stu-id="32981-126">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="32981-127">Installare [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="32981-127">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="32981-128">Creare un nuovo progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="32981-128">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="32981-129">Aprire una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="32981-129">Open a command shell.</span></span> <span data-ttu-id="32981-130">Immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="32981-130">Enter the following command:</span></span>

   ```console
   dotnet new razor -o aspnetcoreapp
   ```

3. <span data-ttu-id="32981-131">Eseguire l'app con i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="32981-131">Run the app with the following commands:</span></span>

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

4. <span data-ttu-id="32981-132">Passare a [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="32981-132">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="32981-133">Aprire *Pages/About.cshtml* e modificare la pagina in modo che visualizzi il messaggio "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="32981-133">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="32981-134">The time on the server is@DateTime.Now" (Buongiorno mondo! L'ora nel server è):</span><span class="sxs-lookup"><span data-stu-id="32981-134">The time on the server is @DateTime.Now":</span></span>

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="32981-135">Passare a [http://localhost:5000/About](http://localhost:5000/About) e verificare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="32981-135">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="32981-136">Installare il **programma di installazione di SDK** per SDK 1.0.4 dalla [pagina di tutti i download di .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="32981-136">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="32981-137">Creare una cartella per un nuovo progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="32981-137">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="32981-138">Aprire una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="32981-138">Open a command shell.</span></span> <span data-ttu-id="32981-139">Immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="32981-139">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="32981-140">Se è installata una versione SDK successiva nel computer in uso, creare un file *global.json* per selezionare l'SDK 1.0.4.</span><span class="sxs-lookup"><span data-stu-id="32981-140">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="32981-141">Creare un nuovo progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="32981-141">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="32981-142">Ripristinare i pacchetti.</span><span class="sxs-lookup"><span data-stu-id="32981-142">Restore the packages.</span></span>

   ```console
   dotnet restore
   ```

6. <span data-ttu-id="32981-143">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="32981-143">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="32981-144">Il comando [dotnet run](/dotnet/core/tools/dotnet-run) compila prima l'app se necessario.</span><span class="sxs-lookup"><span data-stu-id="32981-144">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="32981-145">Passare a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="32981-145">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end
