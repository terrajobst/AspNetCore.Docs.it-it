---
title: Introduzione ad ASP.NET Core
author: rick-anderson
description: Un'esercitazione rapida per creare ed eseguire una semplice app Hello World usando ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: getting-started
ms.openlocfilehash: 29a328b610b0a6e1616cd6ebc70a8fa3e515eb92
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861706"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="dfe60-103">Esercitazione: Introduzione ad ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dfe60-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="dfe60-104">Questa esercitazione illustra come usare l'interfaccia della riga di comando di .NET Core per creare un'app Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dfe60-104">This tutorial shows how to use the .NET Core command-line interface to create an ASP.NET Core web app.</span></span>

<span data-ttu-id="dfe60-105">Si imparerà a eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="dfe60-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dfe60-106">Creare un progetto di app Web.</span><span class="sxs-lookup"><span data-stu-id="dfe60-106">Create a web app project.</span></span>
> * <span data-ttu-id="dfe60-107">Abilitare HTTPS locale.</span><span class="sxs-lookup"><span data-stu-id="dfe60-107">Enable local HTTPS.</span></span>
> * <span data-ttu-id="dfe60-108">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="dfe60-108">Run the app.</span></span>
> * <span data-ttu-id="dfe60-109">Modificare una pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="dfe60-109">Edit a Razor page.</span></span>

<span data-ttu-id="dfe60-110">Al termine, si avrà un'app Web funzionante che viene eseguita nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="dfe60-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Home page dell'app Web](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="dfe60-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="dfe60-112">Prerequisites</span></span>

<span data-ttu-id="dfe60-113">Installare [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="dfe60-113">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

## <a name="create-a-web-app-project"></a><span data-ttu-id="dfe60-114">Creare un progetto di app Web</span><span class="sxs-lookup"><span data-stu-id="dfe60-114">Create a web app project</span></span>

<span data-ttu-id="dfe60-115">Aprire una shell dei comandi e immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="dfe60-115">Open a command shell, and enter the following command:</span></span>

```console
dotnet new webapp -o aspnetcoreapp
```

## <a name="enable-local-https"></a><span data-ttu-id="dfe60-116">Abilitare HTTPS locale</span><span class="sxs-lookup"><span data-stu-id="dfe60-116">Enable local HTTPS</span></span>

<span data-ttu-id="dfe60-117">Considerare attendibile il certificato di sviluppo HTTPS:</span><span class="sxs-lookup"><span data-stu-id="dfe60-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="dfe60-118">Windows</span><span class="sxs-lookup"><span data-stu-id="dfe60-118">Windows</span></span>](#tab/windows)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="dfe60-119">Il comando precedente consente di visualizzare la finestra di dialogo seguente:</span><span class="sxs-lookup"><span data-stu-id="dfe60-119">The preceding command displays the following dialog:</span></span>

![Finestra di dialogo Avviso di sicurezza](_static/cert.png)

<span data-ttu-id="dfe60-121">Selezionare **Sì** se si accetta di considerare attendibile il certificato di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="dfe60-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="dfe60-122">macOS</span><span class="sxs-lookup"><span data-stu-id="dfe60-122">macOS</span></span>](#tab/macos)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="dfe60-123">Il comando precedente consente di visualizzare il messaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="dfe60-123">The preceding command displays the following message:</span></span>

<span data-ttu-id="dfe60-124">*Trusting the HTTPS development certificate was requested (È stato richiesto di considerare attendibile il certificato di sviluppo HTTPS). Se il certificato non è già stato considerato attendibile, il comando seguente non viene eseguito:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="dfe60-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>  
<span data-ttu-id="dfe60-125">\*Questo comando potrebbe richiedere la password per installare il certificato nella keychain di sistema.</span><span class="sxs-lookup"><span data-stu-id="dfe60-125">\*This command might prompt you for your password to install the certificate on the system keychain.</span></span>

<span data-ttu-id="dfe60-126">Password:\*</span><span class="sxs-lookup"><span data-stu-id="dfe60-126">Password:\*</span></span>

<span data-ttu-id="dfe60-127">Inserire la password se si accetta di considerare attendibile il certificato di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="dfe60-127">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="dfe60-128">Linux</span><span class="sxs-lookup"><span data-stu-id="dfe60-128">Linux</span></span>](#tab/linux)

<span data-ttu-id="dfe60-129">Vedere la documentazione della distribuzione di Linux su come rendere attendibile il certificato di sviluppo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="dfe60-129">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

## <a name="run-the-app"></a><span data-ttu-id="dfe60-130">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="dfe60-130">Run the app</span></span>

<span data-ttu-id="dfe60-131">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="dfe60-131">Run the following commands:</span></span>

```console
cd aspnetcoreapp
dotnet run
```

<span data-ttu-id="dfe60-132">Passare a [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="dfe60-132">Browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="dfe60-133">Fare clic su **Accept** (Accetto) per accettare l'informativa sulla privacy e la policy per i cookie.</span><span class="sxs-lookup"><span data-stu-id="dfe60-133">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="dfe60-134">Questa app non memorizza informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="dfe60-134">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="dfe60-135">Modificare una pagina Razor</span><span class="sxs-lookup"><span data-stu-id="dfe60-135">Edit a Razor page</span></span>

<span data-ttu-id="dfe60-136">Aprire *Pages/About.cshtml* e modificare la pagina con il markup evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="dfe60-136">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

<span data-ttu-id="dfe60-137">Passare a [https://localhost:5001/About](https://localhost:5001/About) e verificare che le modifiche siano visualizzate.</span><span class="sxs-lookup"><span data-stu-id="dfe60-137">Browse to [https://localhost:5001/About](https://localhost:5001/About) and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dfe60-138">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="dfe60-138">Next steps</span></span>

<span data-ttu-id="dfe60-139">In questa esercitazione si è appreso come:</span><span class="sxs-lookup"><span data-stu-id="dfe60-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dfe60-140">Creare un progetto di app Web.</span><span class="sxs-lookup"><span data-stu-id="dfe60-140">Create a web app project.</span></span>
> * <span data-ttu-id="dfe60-141">Abilitare HTTPS locale.</span><span class="sxs-lookup"><span data-stu-id="dfe60-141">Enable local HTTPS.</span></span>
> * <span data-ttu-id="dfe60-142">Eseguire il progetto.</span><span class="sxs-lookup"><span data-stu-id="dfe60-142">Run the project.</span></span>
> * <span data-ttu-id="dfe60-143">Apportare una modifica.</span><span class="sxs-lookup"><span data-stu-id="dfe60-143">Make a change.</span></span>

<span data-ttu-id="dfe60-144">Per altre informazioni su ASP.NET Core, vedere l'introduzione:</span><span class="sxs-lookup"><span data-stu-id="dfe60-144">To learn more about ASP.NET Core, see the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index>

> [!NOTE]
> <span data-ttu-id="dfe60-145">È in corso un test di usabilità di una nuova proposta di struttura del sommario di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dfe60-145">We're testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span> <span data-ttu-id="dfe60-146">Se si dispone di alcuni minuti per provare a cercare sette argomenti diversi nel sommario corrente o proposto, [fare clic qui per partecipare al test](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span><span class="sxs-lookup"><span data-stu-id="dfe60-146">If you have a few minutes to try an exercise of finding seven different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>
