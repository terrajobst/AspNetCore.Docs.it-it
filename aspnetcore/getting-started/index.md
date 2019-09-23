---
title: Introduzione ad ASP.NET Core
author: rick-anderson
description: Un'esercitazione rapida per creare ed eseguire una semplice app Hello World usando ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/22/2019
uid: getting-started
ms.openlocfilehash: 0f05ab120d64832a4bc2fd70921efc7238ee9eac
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187066"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="b5284-103">Esercitazione: Introduzione ad ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5284-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="b5284-104">Questa esercitazione illustra come usare l'interfaccia della riga di comando di .NET Core per creare ed eseguire un'app Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b5284-104">This tutorial shows how to use the .NET Core command-line interface to create and run an ASP.NET Core web app.</span></span>

<span data-ttu-id="b5284-105">Si imparerà a eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="b5284-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b5284-106">Creare un progetto di app Web.</span><span class="sxs-lookup"><span data-stu-id="b5284-106">Create a web app project.</span></span>
> * <span data-ttu-id="b5284-107">Considerare attendibile il certificato di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="b5284-107">Trust the development certificate.</span></span>
> * <span data-ttu-id="b5284-108">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="b5284-108">Run the app.</span></span>
> * <span data-ttu-id="b5284-109">Modificare una pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="b5284-109">Edit a Razor page.</span></span>

<span data-ttu-id="b5284-110">Al termine, si avrà un'app Web funzionante che viene eseguita nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="b5284-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Home page dell'app Web](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="b5284-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="b5284-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/3.0-SDK.md)]

## <a name="create-a-web-app-project"></a><span data-ttu-id="b5284-113">Creare un progetto di app Web</span><span class="sxs-lookup"><span data-stu-id="b5284-113">Create a web app project</span></span>

<span data-ttu-id="b5284-114">Aprire una shell dei comandi e immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b5284-114">Open a command shell, and enter the following command:</span></span>

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

<span data-ttu-id="b5284-115">Il comando precedente:</span><span class="sxs-lookup"><span data-stu-id="b5284-115">The preceding command:</span></span>

* <span data-ttu-id="b5284-116">Crea una nuova app Web.</span><span class="sxs-lookup"><span data-stu-id="b5284-116">Creates a new web app.</span></span>  
* <span data-ttu-id="b5284-117">Il `-o` parametro crea una directory denominata *aspnetcoreapp* con i file di origine per l'app.</span><span class="sxs-lookup"><span data-stu-id="b5284-117">The `-o` parameter creates a directory named *aspnetcoreapp* with the source files for the app.</span></span>

### <a name="trust-the-development-certificate"></a><span data-ttu-id="b5284-118">Considerare attendibile il certificato di sviluppo</span><span class="sxs-lookup"><span data-stu-id="b5284-118">Trust the development certificate</span></span>

<span data-ttu-id="b5284-119">Considerare attendibile il certificato di sviluppo HTTPS:</span><span class="sxs-lookup"><span data-stu-id="b5284-119">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b5284-120">Windows</span><span class="sxs-lookup"><span data-stu-id="b5284-120">Windows</span></span>](#tab/windows)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="b5284-121">Il comando precedente consente di visualizzare la finestra di dialogo seguente:</span><span class="sxs-lookup"><span data-stu-id="b5284-121">The preceding command displays the following dialog:</span></span>

![Finestra di dialogo Avviso di sicurezza](~/getting-started/_static/cert.png)

<span data-ttu-id="b5284-123">Selezionare **Sì** se si accetta di considerare attendibile il certificato di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="b5284-123">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="b5284-124">macOS</span><span class="sxs-lookup"><span data-stu-id="b5284-124">macOS</span></span>](#tab/macos)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="b5284-125">Il comando precedente consente di visualizzare il messaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="b5284-125">The preceding command displays the following message:</span></span>

<span data-ttu-id="b5284-126">*Trusting the HTTPS development certificate was requested (È stato richiesto di considerare attendibile il certificato di sviluppo HTTPS). Se il certificato non è già attendibile, verrà eseguito il comando seguente:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span><span class="sxs-lookup"><span data-stu-id="b5284-126">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>

<span data-ttu-id="b5284-127">Questo comando potrebbe richiedere la password per installare il certificato nel keychain di sistema.</span><span class="sxs-lookup"><span data-stu-id="b5284-127">This command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="b5284-128">Inserire la password se si accetta di considerare attendibile il certificato di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="b5284-128">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="b5284-129">Linux</span><span class="sxs-lookup"><span data-stu-id="b5284-129">Linux</span></span>](#tab/linux)

<span data-ttu-id="b5284-130">Per il sottosistema Windows per Linux, vedere [Considerare attendibile il certificato HTTPS dal sottosistema Windows per Linux](xref:security/enforcing-ssl#wsl).</span><span class="sxs-lookup"><span data-stu-id="b5284-130">For Windows Subsystem for Linux, see [Trust HTTPS certificate from Windows Subsystem for Linux](xref:security/enforcing-ssl#wsl).</span></span>

<span data-ttu-id="b5284-131">Vedere la documentazione della distribuzione di Linux su come rendere attendibile il certificato di sviluppo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b5284-131">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="b5284-132">Per altre informazioni, vedere [Considerare attendibile il certificato di sviluppo di ASP.NET Core HTTPS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span><span class="sxs-lookup"><span data-stu-id="b5284-132">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="b5284-133">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="b5284-133">Run the app</span></span>

<span data-ttu-id="b5284-134">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="b5284-134">Run the following commands:</span></span>

```dotnetcli
cd aspnetcoreapp
dotnet run
```

<span data-ttu-id="b5284-135">Dopo che la shell dei comandi indica che l'app è stata avviata, passare a [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="b5284-135">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="b5284-136">Fare clic su **Accept** (Accetto) per accettare l'informativa sulla privacy e la policy per i cookie.</span><span class="sxs-lookup"><span data-stu-id="b5284-136">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="b5284-137">Questa app non memorizza informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="b5284-137">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="b5284-138">Modificare una pagina Razor</span><span class="sxs-lookup"><span data-stu-id="b5284-138">Edit a Razor page</span></span>

<span data-ttu-id="b5284-139">Aprire *Pages/Index.cshtml* e modificare la pagina con il markup evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="b5284-139">Open *Pages/Index.cshtml* and modify the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="b5284-140">Passare a [https://localhost:5001](https://localhost:5001) e verificare che le modifiche siano visualizzate.</span><span class="sxs-lookup"><span data-stu-id="b5284-140">Browse to [https://localhost:5001](https://localhost:5001), and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b5284-141">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="b5284-141">Next steps</span></span>

<span data-ttu-id="b5284-142">In questa esercitazione si è appreso come:</span><span class="sxs-lookup"><span data-stu-id="b5284-142">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b5284-143">Creare un progetto di app Web.</span><span class="sxs-lookup"><span data-stu-id="b5284-143">Create a web app project.</span></span>
> * <span data-ttu-id="b5284-144">Considerare attendibile il certificato di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="b5284-144">Trust the development certificate.</span></span>
> * <span data-ttu-id="b5284-145">Eseguire il progetto.</span><span class="sxs-lookup"><span data-stu-id="b5284-145">Run the project.</span></span>
> * <span data-ttu-id="b5284-146">Apportare una modifica.</span><span class="sxs-lookup"><span data-stu-id="b5284-146">Make a change.</span></span>

<span data-ttu-id="b5284-147">Per altre informazioni su ASP.NET Core, vedere il percorso di apprendimento consigliato nell'introduzione:</span><span class="sxs-lookup"><span data-stu-id="b5284-147">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
