---
title: Introduzione ad ASP.NET Core
author: rick-anderson
description: Un'esercitazione rapida per creare ed eseguire una semplice app Hello World usando ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2019
uid: getting-started
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="06ffc-103">Esercitazione: Introduzione ad ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="06ffc-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="06ffc-104">Questa esercitazione illustra come usare l'interfaccia della riga di comando di .NET Core per creare un'app Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="06ffc-104">This tutorial shows how to use the .NET Core command-line interface to create an ASP.NET Core web app.</span></span>

<span data-ttu-id="06ffc-105">Si imparerà a eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="06ffc-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="06ffc-106">Creare un progetto di app Web.</span><span class="sxs-lookup"><span data-stu-id="06ffc-106">Create a web app project.</span></span>
> * <span data-ttu-id="06ffc-107">Abilitare HTTPS locale.</span><span class="sxs-lookup"><span data-stu-id="06ffc-107">Enable local HTTPS.</span></span>
> * <span data-ttu-id="06ffc-108">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="06ffc-108">Run the app.</span></span>
> * <span data-ttu-id="06ffc-109">Modificare una pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="06ffc-109">Edit a Razor page.</span></span>

<span data-ttu-id="06ffc-110">Al termine, si avrà un'app Web funzionante che viene eseguita nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="06ffc-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Home page dell'app Web](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="06ffc-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="06ffc-112">Prerequisites</span></span>

* [<span data-ttu-id="06ffc-113">.NET Core 2.2 SDK</span><span class="sxs-lookup"><span data-stu-id="06ffc-113">.NET Core 2.2 SDK</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a><span data-ttu-id="06ffc-114">Creare un progetto di app Web</span><span class="sxs-lookup"><span data-stu-id="06ffc-114">Create a web app project</span></span>

<span data-ttu-id="06ffc-115">Aprire una shell dei comandi e immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="06ffc-115">Open a command shell, and enter the following command:</span></span>

```console
dotnet new webapp -o aspnetcoreapp
```

## <a name="enable-local-https"></a><span data-ttu-id="06ffc-116">Abilitare HTTPS locale</span><span class="sxs-lookup"><span data-stu-id="06ffc-116">Enable local HTTPS</span></span>

<span data-ttu-id="06ffc-117">Considerare attendibile il certificato di sviluppo HTTPS:</span><span class="sxs-lookup"><span data-stu-id="06ffc-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="06ffc-118">Windows</span><span class="sxs-lookup"><span data-stu-id="06ffc-118">Windows</span></span>](#tab/windows)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="06ffc-119">Il comando precedente consente di visualizzare la finestra di dialogo seguente:</span><span class="sxs-lookup"><span data-stu-id="06ffc-119">The preceding command displays the following dialog:</span></span>

![Finestra di dialogo Avviso di sicurezza](~/getting-started/_static/cert.png)

<span data-ttu-id="06ffc-121">Selezionare **Sì** se si accetta di considerare attendibile il certificato di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="06ffc-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="06ffc-122">macOS</span><span class="sxs-lookup"><span data-stu-id="06ffc-122">macOS</span></span>](#tab/macos)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="06ffc-123">Il comando precedente consente di visualizzare il messaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="06ffc-123">The preceding command displays the following message:</span></span>

<span data-ttu-id="06ffc-124">*Trusting the HTTPS development certificate was requested (È stato richiesto di considerare attendibile il certificato di sviluppo HTTPS). Se il certificato non è già stato considerato attendibile, il comando seguente non viene eseguito:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="06ffc-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>
 
<span data-ttu-id="06ffc-125">Questo comando potrebbe richiedere la password per installare il certificato nella keychain di sistema.</span><span class="sxs-lookup"><span data-stu-id="06ffc-125">This command command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="06ffc-126">Inserire la password se si accetta di considerare attendibile il certificato di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="06ffc-126">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="06ffc-127">Linux</span><span class="sxs-lookup"><span data-stu-id="06ffc-127">Linux</span></span>](#tab/linux)

<span data-ttu-id="06ffc-128">Vedere la documentazione della distribuzione di Linux su come rendere attendibile il certificato di sviluppo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="06ffc-128">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="06ffc-129">Per altre informazioni, vedere [Considerare attendibile il certificato di sviluppo di ASP.NET Core HTTPS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span><span class="sxs-lookup"><span data-stu-id="06ffc-129">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="06ffc-130">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="06ffc-130">Run the app</span></span>

<span data-ttu-id="06ffc-131">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="06ffc-131">Run the following commands:</span></span>

```console
cd aspnetcoreapp
dotnet run
```

<span data-ttu-id="06ffc-132">Dopo che la shell dei comandi indica che l'app è stata avviata, passare a [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="06ffc-132">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="06ffc-133">Fare clic su **Accept** (Accetto) per accettare l'informativa sulla privacy e la policy per i cookie.</span><span class="sxs-lookup"><span data-stu-id="06ffc-133">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="06ffc-134">Questa app non memorizza informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="06ffc-134">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="06ffc-135">Modificare una pagina Razor</span><span class="sxs-lookup"><span data-stu-id="06ffc-135">Edit a Razor page</span></span>

<span data-ttu-id="06ffc-136">Aprire *Pages/Index.cshtml* e modificare la pagina con il markup evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="06ffc-136">Open *Pages/Index.cshtml* and modify the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="06ffc-137">Passare a [https://localhost:5001](https://localhost:5001) e verificare che le modifiche siano visualizzate.</span><span class="sxs-lookup"><span data-stu-id="06ffc-137">Browse to [https://localhost:5001](https://localhost:5001), and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="06ffc-138">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="06ffc-138">Next steps</span></span>

<span data-ttu-id="06ffc-139">In questa esercitazione si è appreso come:</span><span class="sxs-lookup"><span data-stu-id="06ffc-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="06ffc-140">Creare un progetto di app Web.</span><span class="sxs-lookup"><span data-stu-id="06ffc-140">Create a web app project.</span></span>
> * <span data-ttu-id="06ffc-141">Abilitare HTTPS locale.</span><span class="sxs-lookup"><span data-stu-id="06ffc-141">Enable local HTTPS.</span></span>
> * <span data-ttu-id="06ffc-142">Eseguire il progetto.</span><span class="sxs-lookup"><span data-stu-id="06ffc-142">Run the project.</span></span>
> * <span data-ttu-id="06ffc-143">Apportare una modifica.</span><span class="sxs-lookup"><span data-stu-id="06ffc-143">Make a change.</span></span>

<span data-ttu-id="06ffc-144">Per altre informazioni su ASP.NET Core, vedere il percorso di apprendimento consigliato nell'introduzione:</span><span class="sxs-lookup"><span data-stu-id="06ffc-144">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
