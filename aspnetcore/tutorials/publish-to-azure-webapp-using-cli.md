---
title: Pubblicare un'app ASP.NET Core in Azure con gli strumenti della riga di comando
author: camsoper
description: Informazioni su come pubblicare un'app ASP.NET Core in Servizio app di Azure con il client da riga di comando Git.
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 526ceef469d473706f39cdc3ee645280e99315b1
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38126960"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a><span data-ttu-id="e2f5f-103">Pubblicare un'app ASP.NET Core in Azure con gli strumenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="e2f5f-103">Publish an ASP.NET Core app to Azure with command line tools</span></span>

<span data-ttu-id="e2f5f-104">Di [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="e2f5f-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="e2f5f-105">Questa esercitazione illustra come compilare e distribuire un'app ASP.NET Core nel Servizio app di Microsoft Azure usando gli strumenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="e2f5f-105">This tutorial will show you how to build and deploy an ASP.NET Core app to Microsoft Azure App Service using command line tools.</span></span> <span data-ttu-id="e2f5f-106">Al termine, sarà disponibile un'app Web Razor Pages compilata in ASP.NET Core ospitata come app Web del Servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="e2f5f-106">When finished, you'll have a Razor Pages web app built in ASP.NET Core hosted as an Azure App Service Web App.</span></span> <span data-ttu-id="e2f5f-107">Questa esercitazione è scritta tramite gli strumenti della riga di comando di Windows, ma può essere applicata anche agli ambienti macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="e2f5f-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>

<span data-ttu-id="e2f5f-108">In questa esercitazione si imparerà a:</span><span class="sxs-lookup"><span data-stu-id="e2f5f-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e2f5f-109">Creare un sito Web di Servizio app di Azure con l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="e2f5f-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="e2f5f-110">Distribuire un'appASP.NET Core nel Servizio app di Azure con lo strumento della riga di comando Git</span><span class="sxs-lookup"><span data-stu-id="e2f5f-110">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e2f5f-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="e2f5f-111">Prerequisites</span></span>

<span data-ttu-id="e2f5f-112">Per completare questa esercitazione, è necessario disporre di:</span><span class="sxs-lookup"><span data-stu-id="e2f5f-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="e2f5f-113">Una [sottoscrizione di Microsoft Azure](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="e2f5f-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="e2f5f-114">Client della riga di comando [Git](https://www.git-scm.com/)</span><span class="sxs-lookup"><span data-stu-id="e2f5f-114">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="e2f5f-115">Creare un'app Web</span><span class="sxs-lookup"><span data-stu-id="e2f5f-115">Create a web app</span></span>

<span data-ttu-id="e2f5f-116">Creare una nuova directory per l'app Web, creare una nuova app Razor Pages ASP.NET Core e quindi eseguire il sito Web in locale.</span><span class="sxs-lookup"><span data-stu-id="e2f5f-116">Create a new directory for the web app, create a new ASP.NET Core Razor Pages app, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="e2f5f-117">Windows</span><span class="sxs-lookup"><span data-stu-id="e2f5f-117">Windows</span></span>](#tab/windows)

::: moniker range=">= aspnetcore-2.1"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

::: moniker-end

# <a name="othertabother"></a>[<span data-ttu-id="e2f5f-118">Altro</span><span class="sxs-lookup"><span data-stu-id="e2f5f-118">Other</span></span>](#tab/other)

::: moniker range=">= aspnetcore-2.1"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

::: moniker-end

---

![Output della riga di comando](publish-to-azure-webapp-using-cli/_static/new_prj.png)

<span data-ttu-id="e2f5f-120">Testare l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e2f5f-120">Test the app by browsing to `http://localhost:5000`.</span></span>

![Sito Web in esecuzione in locale](publish-to-azure-webapp-using-cli/_static/app_test.png)

## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="e2f5f-122">Creare l'istanza di Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="e2f5f-122">Create the Azure App Service instance</span></span>

<span data-ttu-id="e2f5f-123">Usando [Azure Cloud Shell](/azure/cloud-shell/quickstart), creare un gruppo di risorse, il piano di servizio app e un'app Web del servizio app.</span><span class="sxs-lookup"><span data-stu-id="e2f5f-123">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

```azurecli-interactive
# Generate a unique Web App name
let randomNum=$RANDOM*$RANDOM
webappname=tutorialApp$randomNum

# Create the DotNetAzureTutorial resource group
az group create --name DotNetAzureTutorial --location EastUS

# Create an App Service plan.
az appservice plan create --name $webappname --resource-group DotNetAzureTutorial --sku FREE

# Create the Web App
az webapp create --name $webappname --resource-group DotNetAzureTutorial --plan $webappname
```

<span data-ttu-id="e2f5f-124">Prima della distribuzione, impostare le credenziali di distribuzione a livello di account usando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e2f5f-124">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="e2f5f-125">Per distribuire l'app tramite Git, è necessario un URL di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="e2f5f-125">A deployment URL is needed to deploy the app using Git.</span></span> <span data-ttu-id="e2f5f-126">Recuperare l'URL come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="e2f5f-126">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```

<span data-ttu-id="e2f5f-127">Prendere nota dell'URL visualizzato che termina con `.git`,</span><span class="sxs-lookup"><span data-stu-id="e2f5f-127">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="e2f5f-128">che verrà usato nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="e2f5f-128">It's used in the next step.</span></span>

## <a name="deploy-the-app-using-git"></a><span data-ttu-id="e2f5f-129">Distribuire l'app tramite Git</span><span class="sxs-lookup"><span data-stu-id="e2f5f-129">Deploy the app using Git</span></span>

<span data-ttu-id="e2f5f-130">A questo punto è possibile procedere alla distribuzione dal computer locale tramite Git.</span><span class="sxs-lookup"><span data-stu-id="e2f5f-130">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="e2f5f-131">Gli avvisi di Git sulle terminazioni riga possono essere ignorati.</span><span class="sxs-lookup"><span data-stu-id="e2f5f-131">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="e2f5f-132">Windows</span><span class="sxs-lookup"><span data-stu-id="e2f5f-132">Windows</span></span>](#tab/windows)

```cmd
REM Initialize the local Git repository
git init

REM Add the contents of the working directory to the repo
git add --all

REM Commit the changes to the local repo
git commit -a -m "Initial commit"

REM Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

REM Push the local repository to the remote
git push azure master
```

# <a name="othertabother"></a>[<span data-ttu-id="e2f5f-133">Altro</span><span class="sxs-lookup"><span data-stu-id="e2f5f-133">Other</span></span>](#tab/other)

```bash
# Initialize the local Git repository
git init

# Add the contents of the working directory to the repo
git add --all

# Commit the changes to the local repo
git commit -a -m "Initial commit"

# Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

# Push the local repository to the remote
git push azure master
```

---

<span data-ttu-id="e2f5f-134">Git richiede le credenziali di distribuzione impostate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="e2f5f-134">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="e2f5f-135">Dopo l'autenticazione, verrà eseguito il push dell'app nel percorso remoto e l'applicazione sarà compilata e distribuita.</span><span class="sxs-lookup"><span data-stu-id="e2f5f-135">After authenticating, the app will be pushed to the remote location, built, and deployed.</span></span>

![Output della distribuzione di Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-app"></a><span data-ttu-id="e2f5f-137">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="e2f5f-137">Test the app</span></span>

<span data-ttu-id="e2f5f-138">Testare l'app all'indirizzo `https://<web app name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="e2f5f-138">Test the app by browsing to `https://<web app name>.azurewebsites.net`.</span></span> <span data-ttu-id="e2f5f-139">Per visualizzare l'indirizzo in Cloud Shell (o nell'interfaccia della riga di comando di Azure), usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e2f5f-139">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![App in esecuzione in Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="e2f5f-141">Eseguire la pulizia</span><span class="sxs-lookup"><span data-stu-id="e2f5f-141">Clean up</span></span>

<span data-ttu-id="e2f5f-142">Al termine del test dell'app e dell'analisi del codice e delle risorse, eliminare l'app Web e il piano eliminando il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="e2f5f-142">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="e2f5f-143">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="e2f5f-143">Next steps</span></span>

<span data-ttu-id="e2f5f-144">In questa esercitazione si è appreso come:</span><span class="sxs-lookup"><span data-stu-id="e2f5f-144">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e2f5f-145">Creare un sito Web di Servizio app di Azure con l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="e2f5f-145">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="e2f5f-146">Distribuire un'appASP.NET Core nel Servizio app di Azure con lo strumento della riga di comando Git</span><span class="sxs-lookup"><span data-stu-id="e2f5f-146">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="e2f5f-147">Nel passaggio successivo si imparerà a usare la riga di comando per distribuire un'app Web esistente che usa CosmosDB.</span><span class="sxs-lookup"><span data-stu-id="e2f5f-147">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e2f5f-148">Distribuire in Azure dalla riga di comando con .NET Core</span><span class="sxs-lookup"><span data-stu-id="e2f5f-148">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
