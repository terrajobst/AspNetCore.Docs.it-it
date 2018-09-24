---
title: Pubblicare un'app ASP.NET Core in Azure con gli strumenti della riga di comando
author: camsoper
description: Informazioni su come pubblicare un'app ASP.NET Core in Servizio app di Azure con il client da riga di comando Git.
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 63a313de786b1f89e84c594cbd665d1b230e4ba3
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523246"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a>Pubblicare un'app ASP.NET Core in Azure con gli strumenti della riga di comando

Di [Cam Soper](https://twitter.com/camsoper)

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

Questa esercitazione illustra come compilare e distribuire un'app ASP.NET Core nel Servizio app di Microsoft Azure usando gli strumenti della riga di comando. Al termine, sarà disponibile un'app Web Razor Pages compilata in ASP.NET Core ospitata come app Web del Servizio app di Azure. Questa esercitazione è scritta tramite gli strumenti della riga di comando di Windows, ma può essere applicata anche agli ambienti macOS e Linux.

In questa esercitazione si imparerà a:

> [!div class="checklist"]
> * Creare un sito Web di Servizio app di Azure con l'interfaccia della riga di comando di Azure
> * Distribuire un'appASP.NET Core nel Servizio app di Azure con lo strumento della riga di comando Git

## <a name="prerequisites"></a>Prerequisiti

Per completare questa esercitazione, è necessario disporre di:

* Una [sottoscrizione di Microsoft Azure](https://azure.microsoft.com/free/)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* Client della riga di comando [Git](https://www.git-scm.com/)

## <a name="create-a-web-app"></a>Creare un'app Web

Creare una nuova directory per l'app Web, creare una nuova app Razor Pages ASP.NET Core e quindi eseguire il sito Web in locale.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

::: moniker range=">= aspnetcore-2.1"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

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

# <a name="othertabother"></a>[Altro](#tab/other)

::: moniker range=">= aspnetcore-2.1"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

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

Testare l'app all'indirizzo `http://localhost:5000`.

![Sito Web in esecuzione in locale](publish-to-azure-webapp-using-cli/_static/app_test.png)

## <a name="create-the-azure-app-service-instance"></a>Creare l'istanza di Servizio app di Azure

Usando [Azure Cloud Shell](/azure/cloud-shell/quickstart), creare un gruppo di risorse, il piano di servizio app e un'app Web del servizio app.

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

Prima della distribuzione, impostare le credenziali di distribuzione a livello di account usando il comando seguente:

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

Per distribuire l'app tramite Git, è necessario un URL di distribuzione. Recuperare l'URL come illustrato di seguito.

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```

Prendere nota dell'URL visualizzato che termina con `.git`, che verrà usato nel passaggio successivo.

## <a name="deploy-the-app-using-git"></a>Distribuire l'app tramite Git

A questo punto è possibile procedere alla distribuzione dal computer locale tramite Git.

> [!NOTE]
> Gli avvisi di Git sulle terminazioni riga possono essere ignorati.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

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

# <a name="othertabother"></a>[Altro](#tab/other)

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

Git richiede le credenziali di distribuzione impostate in precedenza. Dopo l'autenticazione, verrà eseguito il push dell'app nel percorso remoto e l'applicazione sarà compilata e distribuita.

![Output della distribuzione di Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-app"></a>Eseguire il test dell'app

Testare l'app all'indirizzo `https://<web app name>.azurewebsites.net`. Per visualizzare l'indirizzo in Cloud Shell (o nell'interfaccia della riga di comando di Azure), usare il comando seguente:

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![App in esecuzione in Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a>Eseguire la pulizia

Al termine del test dell'app e dell'analisi del codice e delle risorse, eliminare l'app Web e il piano eliminando il gruppo di risorse.

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione si è appreso come:

> [!div class="checklist"]
> * Creare un sito Web di Servizio app di Azure con l'interfaccia della riga di comando di Azure
> * Distribuire un'appASP.NET Core nel Servizio app di Azure con lo strumento della riga di comando Git

Nel passaggio successivo si imparerà a usare la riga di comando per distribuire un'app Web esistente che usa CosmosDB.

> [!div class="nextstepaction"]
> [Distribuire in Azure dalla riga di comando con .NET Core](/dotnet/azure/dotnet-quickstart-xplat)
