---
title: Introduzione ad ASP.NET Core
author: rick-anderson
description: Un'esercitazione rapida per creare ed eseguire una semplice app Hello World usando ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: eb049dea2800cf2e12c044b88d1664ee80bb95a5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296768"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="a24a9-103">Introduzione ad ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a24a9-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="a24a9-104">Installare [!INCLUDE[](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="a24a9-104">Install the [!INCLUDE[](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="a24a9-105">Creare un progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a24a9-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="a24a9-106">Aprire una shell dei comandi e immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="a24a9-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

3. <span data-ttu-id="a24a9-108">Considerare attendibile il certificato di sviluppo HTTPS:</span><span class="sxs-lookup"><span data-stu-id="a24a9-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="a24a9-109">Windows</span><span class="sxs-lookup"><span data-stu-id="a24a9-109">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[<span data-ttu-id="a24a9-110">macOS</span><span class="sxs-lookup"><span data-stu-id="a24a9-110">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[<span data-ttu-id="a24a9-111">Linux</span><span class="sxs-lookup"><span data-stu-id="a24a9-111">Linux</span></span>](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. <span data-ttu-id="a24a9-112">Eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="a24a9-112">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="a24a9-113">Passare a [http://localhost:5001](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="a24a9-113">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="a24a9-114">Fare clic su **Accept** (Accetto) per accettare l'informativa sulla privacy e la policy per i cookie.</span><span class="sxs-lookup"><span data-stu-id="a24a9-114">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="a24a9-115">Questa app non memorizza informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="a24a9-115">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="a24a9-116">Aprire *Pages/About.cshtml* e modificare la pagina con il markup evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="a24a9-116">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    <span data-ttu-id="a24a9-117">[!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="a24a9-117">[!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]</span></span>

7. <span data-ttu-id="a24a9-118">Passare a [http://localhost:5001/About](http://localhost:5001/About) e verificare che le modifiche siano visualizzate.</span><span class="sxs-lookup"><span data-stu-id="a24a9-118">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="a24a9-119">Installare [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="a24a9-119">Install the [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="a24a9-120">Creare un nuovo progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a24a9-120">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="a24a9-121">Aprire una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="a24a9-121">Open a command shell.</span></span> <span data-ttu-id="a24a9-122">Immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="a24a9-122">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="a24a9-123">Eseguire l'app con i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="a24a9-123">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="a24a9-124">Passare a [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="a24a9-124">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="a24a9-125">Aprire *Pages/About.cshtml* e modificare la pagina in modo che visualizzi il messaggio "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="a24a9-125">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="a24a9-126">The time on the server is@DateTime.Now" (Buongiorno mondo! L'ora nel server è):</span><span class="sxs-lookup"><span data-stu-id="a24a9-126">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="a24a9-127">[!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="a24a9-127">[!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

6. <span data-ttu-id="a24a9-128">Passare a [http://localhost:5000/About](http://localhost:5000/About) e verificare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="a24a9-128">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="a24a9-129">Installare il **programma di installazione di SDK** per SDK 1.0.4 dalla [pagina di tutti i download di .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="a24a9-129">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="a24a9-130">Creare una cartella per un nuovo progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a24a9-130">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="a24a9-131">Aprire una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="a24a9-131">Open a command shell.</span></span> <span data-ttu-id="a24a9-132">Immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="a24a9-132">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="a24a9-133">Se è installata una versione SDK successiva nel computer in uso, creare un file *global.json* per selezionare l'SDK 1.0.4.</span><span class="sxs-lookup"><span data-stu-id="a24a9-133">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="a24a9-134">Creare un nuovo progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a24a9-134">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="a24a9-135">Ripristinare i pacchetti.</span><span class="sxs-lookup"><span data-stu-id="a24a9-135">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="a24a9-136">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="a24a9-136">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="a24a9-137">Il comando [dotnet run](/dotnet/core/tools/dotnet-run) compila prima l'app se necessario.</span><span class="sxs-lookup"><span data-stu-id="a24a9-137">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="a24a9-138">Passare a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a24a9-138">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
