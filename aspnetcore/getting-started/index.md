---
title: Introduzione ad ASP.NET Core
author: rick-anderson
description: Un'esercitazione rapida per creare ed eseguire una semplice app Hello World usando ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 7ab9f303d74786c4ac76f002d0f2c66371e78cb8
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228582"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="60cdd-103">Introduzione ad ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="60cdd-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="60cdd-104">Installare [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="60cdd-104">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="60cdd-105">Creare un progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="60cdd-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="60cdd-106">Aprire una shell dei comandi e immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="60cdd-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE [](~/includes/webapp-alias-notice.md)]

3. <span data-ttu-id="60cdd-107">Considerare attendibile il certificato di sviluppo HTTPS:</span><span class="sxs-lookup"><span data-stu-id="60cdd-107">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="60cdd-108">Windows</span><span class="sxs-lookup"><span data-stu-id="60cdd-108">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="60cdd-109">Il comando precedente consente di visualizzare la finestra di dialogo seguente:</span><span class="sxs-lookup"><span data-stu-id="60cdd-109">The preceding command displays the following dialog:</span></span>

   ![Finestra di dialogo Avviso di sicurezza](_static/cert.png)

   <span data-ttu-id="60cdd-111">Selezionare **Sì** se si accetta di considerare attendibile il certificato di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="60cdd-111">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="60cdd-112">macOS</span><span class="sxs-lookup"><span data-stu-id="60cdd-112">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="60cdd-113">Il comando precedente consente di visualizzare il messaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="60cdd-113">The preceding command displays the following message:</span></span>

   <span data-ttu-id="60cdd-114">*Trusting the HTTPS development certificate was requested (È stato richiesto di considerare attendibile il certificato di sviluppo HTTPS). Se il certificato non è già attendibile viene eseguito il comando seguente:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'` *Questo comando potrebbe richiedere la password per installare il certificato nel keychain del sistema.    Password:*</span><span class="sxs-lookup"><span data-stu-id="60cdd-114">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'` *This command might prompt you for your password to install the certificate on the system keychain.    Password:*</span></span>

   <span data-ttu-id="60cdd-115">Inserire la password se si accetta di considerare attendibile il certificato di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="60cdd-115">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="60cdd-116">Linux</span><span class="sxs-lookup"><span data-stu-id="60cdd-116">Linux</span></span>](#tab/linux)

   <a name="see-the-documentation-for-your-linux-distribution-on-how-to-trust-the-https-development-certificate"></a><span data-ttu-id="60cdd-117">Vedere la documentazione della distribuzione di Linux su come rendere attendibile il certificato di sviluppo HTTPS</span><span class="sxs-lookup"><span data-stu-id="60cdd-117">See the documentation for your Linux distribution on how to trust the HTTPS development certificate</span></span>
---

4. <span data-ttu-id="60cdd-118">Eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="60cdd-118">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="60cdd-119">Passare a [http://localhost:5001](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="60cdd-119">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="60cdd-120">Fare clic su **Accept** (Accetto) per accettare l'informativa sulla privacy e la policy per i cookie.</span><span class="sxs-lookup"><span data-stu-id="60cdd-120">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="60cdd-121">Questa app non memorizza informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="60cdd-121">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="60cdd-122">Aprire *Pages/About.cshtml* e modificare la pagina con il markup evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="60cdd-122">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="60cdd-123">Passare a [http://localhost:5001/About](http://localhost:5001/About) e verificare che le modifiche siano visualizzate.</span><span class="sxs-lookup"><span data-stu-id="60cdd-123">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="60cdd-124">Installare [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="60cdd-124">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="60cdd-125">Creare un nuovo progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="60cdd-125">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="60cdd-126">Aprire una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="60cdd-126">Open a command shell.</span></span> <span data-ttu-id="60cdd-127">Immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="60cdd-127">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="60cdd-128">Eseguire l'app con i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="60cdd-128">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="60cdd-129">Passare a [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="60cdd-129">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="60cdd-130">Aprire *Pages/About.cshtml* e modificare la pagina in modo che visualizzi il messaggio "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="60cdd-130">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="60cdd-131">The time on the server is@DateTime.Now" (Buongiorno mondo! L'ora nel server è):</span><span class="sxs-lookup"><span data-stu-id="60cdd-131">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="60cdd-132">Passare a [http://localhost:5000/About](http://localhost:5000/About) e verificare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="60cdd-132">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="60cdd-133">Installare il **programma di installazione di SDK** per SDK 1.0.4 dalla [pagina di tutti i download di .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="60cdd-133">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="60cdd-134">Creare una cartella per un nuovo progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="60cdd-134">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="60cdd-135">Aprire una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="60cdd-135">Open a command shell.</span></span> <span data-ttu-id="60cdd-136">Immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="60cdd-136">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="60cdd-137">Se è installata una versione SDK successiva nel computer in uso, creare un file *global.json* per selezionare l'SDK 1.0.4.</span><span class="sxs-lookup"><span data-stu-id="60cdd-137">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="60cdd-138">Creare un nuovo progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="60cdd-138">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="60cdd-139">Ripristinare i pacchetti.</span><span class="sxs-lookup"><span data-stu-id="60cdd-139">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="60cdd-140">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="60cdd-140">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="60cdd-141">Il comando [dotnet run](/dotnet/core/tools/dotnet-run) compila prima l'app se necessario.</span><span class="sxs-lookup"><span data-stu-id="60cdd-141">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="60cdd-142">Passare a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="60cdd-142">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
