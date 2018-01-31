---
title: Pubblicare un'app ASP.NET Core in Azure con gli strumenti della riga di comando | Microsoft Docs
description: Informazioni su come pubblicare un'app ASP.NET Core in Servizio app di Azure con il client da riga di comando Git.
services: multiple
author: camsoper
ms.author: casoper
manager: wpickett
ms.date: 11/03/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
ms.custom: mvc
ms.devlang: dotnet
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: de05c1688d7de6126434395042103d803ee3064e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="deploy-an-aspnet-core-application-to-azure-app-service-from-the-command-line"></a><span data-ttu-id="12cd4-103">Distribuire un'applicazione ASP.NET Core in Servizio app di Azure dalla riga di comando</span><span class="sxs-lookup"><span data-stu-id="12cd4-103">Deploy an ASP.NET Core application to Azure App Service from the command line</span></span>

<span data-ttu-id="12cd4-104">Di [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="12cd4-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="12cd4-105">Questa esercitazione illustra come compilare e distribuire un'applicazione ASP.NET Core in Servizio app di Microsoft Azure usando gli strumenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="12cd4-105">This tutorial will show you how to build and deploy an ASP.NET Core application to Microsoft Azure App Service using command line tools.</span></span>  <span data-ttu-id="12cd4-106">Al termine, sarà disponibile un'applicazione Web compilata in ASP.NET MVC Core ospitata come app Web di Servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="12cd4-106">When finished, you'll have a web application built in ASP.NET MVC Core hosted as an Azure App Service Web App.</span></span>  <span data-ttu-id="12cd4-107">Questa esercitazione è scritta tramite gli strumenti della riga di comando di Windows, ma può essere applicata anche agli ambienti macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="12cd4-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>  

<span data-ttu-id="12cd4-108">In questa esercitazione si imparerà a:</span><span class="sxs-lookup"><span data-stu-id="12cd4-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="12cd4-109">Creare un sito Web di Servizio app di Azure con l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="12cd4-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="12cd4-110">Distribuire un'applicazione ASP.NET Core in Servizio app di Azure con lo strumento della riga di comando</span><span class="sxs-lookup"><span data-stu-id="12cd4-110">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="12cd4-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="12cd4-111">Prerequisites</span></span>

<span data-ttu-id="12cd4-112">Per completare questa esercitazione, è necessario disporre di:</span><span class="sxs-lookup"><span data-stu-id="12cd4-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="12cd4-113">Una [sottoscrizione di Microsoft Azure](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="12cd4-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [<span data-ttu-id="12cd4-114">.NET Core</span><span class="sxs-lookup"><span data-stu-id="12cd4-114">.NET Core</span></span>](https://www.microsoft.com/net/download/core)
* <span data-ttu-id="12cd4-115">Client della riga di comando [Git](https://www.git-scm.com/)</span><span class="sxs-lookup"><span data-stu-id="12cd4-115">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-application"></a><span data-ttu-id="12cd4-116">Creare un'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="12cd4-116">Create a web application</span></span>

<span data-ttu-id="12cd4-117">Creare una nuova directory per l'applicazione Web, creare una nuova applicazione ASP.NET Core MVC e quindi eseguire il sito Web in locale.</span><span class="sxs-lookup"><span data-stu-id="12cd4-117">Create a new directory for the web application, create a new ASP.NET Core MVC application, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="12cd4-118">Windows</span><span class="sxs-lookup"><span data-stu-id="12cd4-118">Windows</span></span>](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[<span data-ttu-id="12cd4-119">Altro</span><span class="sxs-lookup"><span data-stu-id="12cd4-119">Other</span></span>](#tab/other)
```bash
# Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the application
dotnet run
```
---

![Output della riga di comando](publish-to-azure-webapp-using-cli/_static/new_prj.png)

<span data-ttu-id="12cd4-121">Testare l'applicazione all'indirizzo http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="12cd4-121">Test the application by browsing to http://localhost:5000.</span></span>

![Sito Web in esecuzione in locale](publish-to-azure-webapp-using-cli/_static/app_test.png)


## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="12cd4-123">Creare l'istanza di Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="12cd4-123">Create the Azure App Service instance</span></span>

<span data-ttu-id="12cd4-124">Usando [Azure Cloud Shell](/azure/cloud-shell/quickstart), creare un gruppo di risorse, il piano di servizio app e un'app Web del servizio app.</span><span class="sxs-lookup"><span data-stu-id="12cd4-124">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

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

<span data-ttu-id="12cd4-125">Prima della distribuzione, impostare le credenziali di distribuzione a livello di account usando il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="12cd4-125">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="12cd4-126">Per distribuire l'applicazione tramite Git, è necessario un URL di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="12cd4-126">A deployment URL is needed to deploy the application using Git.</span></span>  <span data-ttu-id="12cd4-127">Recuperare l'URL come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="12cd4-127">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
<span data-ttu-id="12cd4-128">Prendere nota dell'URL visualizzato che termina con `.git`,</span><span class="sxs-lookup"><span data-stu-id="12cd4-128">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="12cd4-129">che verrà usato nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="12cd4-129">It's used in the next step.</span></span>

## <a name="deploy-the-application-using-git"></a><span data-ttu-id="12cd4-130">Distribuire l'applicazione tramite Git</span><span class="sxs-lookup"><span data-stu-id="12cd4-130">Deploy the application using Git</span></span>

<span data-ttu-id="12cd4-131">A questo punto è possibile procedere alla distribuzione dal computer locale tramite Git.</span><span class="sxs-lookup"><span data-stu-id="12cd4-131">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="12cd4-132">Gli avvisi di Git sulle terminazioni riga possono essere ignorati.</span><span class="sxs-lookup"><span data-stu-id="12cd4-132">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="12cd4-133">Windows</span><span class="sxs-lookup"><span data-stu-id="12cd4-133">Windows</span></span>](#tab/windows)
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

# <a name="othertabother"></a>[<span data-ttu-id="12cd4-134">Altro</span><span class="sxs-lookup"><span data-stu-id="12cd4-134">Other</span></span>](#tab/other)
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

<span data-ttu-id="12cd4-135">Git richiede le credenziali di distribuzione impostate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="12cd4-135">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="12cd4-136">Dopo l'autenticazione, verrà eseguito il push dell'applicazione nel percorso remoto e l'applicazione sarà compilata e distribuita.</span><span class="sxs-lookup"><span data-stu-id="12cd4-136">After authenticating, the application will be pushed to the remote location, built, and deployed.</span></span>

![Output della distribuzione di Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a><span data-ttu-id="12cd4-138">Testare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="12cd4-138">Test the application</span></span>

<span data-ttu-id="12cd4-139">Testare l'applicazione all'indirizzo `https://<web app name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="12cd4-139">Test the application by browsing to `https://<web app name>.azurewebsites.net`.</span></span>  <span data-ttu-id="12cd4-140">Per visualizzare l'indirizzo in Cloud Shell (o nell'interfaccia della riga di comando di Azure), usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="12cd4-140">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![App in esecuzione in Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="12cd4-142">Eseguire la pulizia</span><span class="sxs-lookup"><span data-stu-id="12cd4-142">Clean up</span></span>

<span data-ttu-id="12cd4-143">Al termine del test dell'app e dell'analisi del codice e delle risorse, eliminare l'app Web e il piano eliminando il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="12cd4-143">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="12cd4-144">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="12cd4-144">Next steps</span></span>

<span data-ttu-id="12cd4-145">In questa esercitazione si è appreso come:</span><span class="sxs-lookup"><span data-stu-id="12cd4-145">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="12cd4-146">Creare un sito Web di Servizio app di Azure con l'interfaccia della riga di comando di Azure</span><span class="sxs-lookup"><span data-stu-id="12cd4-146">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="12cd4-147">Distribuire un'applicazione ASP.NET Core in Servizio app di Azure con lo strumento della riga di comando</span><span class="sxs-lookup"><span data-stu-id="12cd4-147">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="12cd4-148">Nel passaggio successivo si imparerà a usare la riga di comando per distribuire un'app Web esistente che usa CosmosDB.</span><span class="sxs-lookup"><span data-stu-id="12cd4-148">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="12cd4-149">Distribuire in Azure dalla riga di comando con .NET Core</span><span class="sxs-lookup"><span data-stu-id="12cd4-149">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
