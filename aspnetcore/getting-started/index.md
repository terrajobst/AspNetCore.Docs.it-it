---
title: Introduzione ad ASP.NET Core
author: rick-anderson
description: Un'esercitazione rapida per creare ed eseguire una semplice app Hello World usando ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 22e9c982921cc03d89506e18ff99bf481027dda6
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077660"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="acffa-103">Introduzione ad ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="acffa-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="acffa-104">Installare [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="acffa-104">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="acffa-105">Creare un progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="acffa-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="acffa-106">Aprire una shell dei comandi e immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="acffa-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    <span data-ttu-id="acffa-107">[!INCLUDE [](~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]</span><span class="sxs-lookup"><span data-stu-id="acffa-107">[!INCLUDE [](~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]</span></span>

3. <span data-ttu-id="acffa-108">Considerare attendibile il certificato di sviluppo HTTPS:</span><span class="sxs-lookup"><span data-stu-id="acffa-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="acffa-109">Windows</span><span class="sxs-lookup"><span data-stu-id="acffa-109">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[<span data-ttu-id="acffa-110">macOS</span><span class="sxs-lookup"><span data-stu-id="acffa-110">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[<span data-ttu-id="acffa-111">Linux</span><span class="sxs-lookup"><span data-stu-id="acffa-111">Linux</span></span>](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. <span data-ttu-id="acffa-112">Eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="acffa-112">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="acffa-113">Passare a [http://localhost:5001](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="acffa-113">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="acffa-114">Fare clic su **Accept** (Accetto) per accettare l'informativa sulla privacy e la policy per i cookie.</span><span class="sxs-lookup"><span data-stu-id="acffa-114">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="acffa-115">Questa app non memorizza informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="acffa-115">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="acffa-116">Aprire *Pages/About.cshtml* e modificare la pagina con il markup evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="acffa-116">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="acffa-117">Passare a [http://localhost:5001/About](http://localhost:5001/About) e verificare che le modifiche siano visualizzate.</span><span class="sxs-lookup"><span data-stu-id="acffa-117">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="acffa-118">Installare [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="acffa-118">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="acffa-119">Creare un nuovo progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="acffa-119">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="acffa-120">Aprire una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="acffa-120">Open a command shell.</span></span> <span data-ttu-id="acffa-121">Immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="acffa-121">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="acffa-122">Eseguire l'app con i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="acffa-122">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="acffa-123">Passare a [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="acffa-123">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="acffa-124">Aprire *Pages/About.cshtml* e modificare la pagina in modo che visualizzi il messaggio "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="acffa-124">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="acffa-125">The time on the server is@DateTime.Now" (Buongiorno mondo! L'ora nel server è):</span><span class="sxs-lookup"><span data-stu-id="acffa-125">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="acffa-126">Passare a [http://localhost:5000/About](http://localhost:5000/About) e verificare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="acffa-126">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="acffa-127">Installare il **programma di installazione di SDK** per SDK 1.0.4 dalla [pagina di tutti i download di .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="acffa-127">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="acffa-128">Creare una cartella per un nuovo progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="acffa-128">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="acffa-129">Aprire una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="acffa-129">Open a command shell.</span></span> <span data-ttu-id="acffa-130">Immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="acffa-130">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="acffa-131">Se è installata una versione SDK successiva nel computer in uso, creare un file *global.json* per selezionare l'SDK 1.0.4.</span><span class="sxs-lookup"><span data-stu-id="acffa-131">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="acffa-132">Creare un nuovo progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="acffa-132">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="acffa-133">Ripristinare i pacchetti.</span><span class="sxs-lookup"><span data-stu-id="acffa-133">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="acffa-134">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="acffa-134">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="acffa-135">Il comando [dotnet run](/dotnet/core/tools/dotnet-run) compila prima l'app se necessario.</span><span class="sxs-lookup"><span data-stu-id="acffa-135">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="acffa-136">Passare a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="acffa-136">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
