---
title: Pubblicare un'app ASP.NET Core in Azure con gli strumenti della riga di comando
author: camsoper
description: Informazioni su come pubblicare un'app ASP.NET Core in Servizio app di Azure con il client da riga di comando Git.
manager: wpickett
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
ms.devlang: dotnet
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 0418a2695d3afb6dc2c55b8f694a97d62239835f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="deploy-an-aspnet-core-application-to-azure-app-service-from-the-command-line"></a>Distribuire un'applicazione ASP.NET Core in Servizio app di Azure dalla riga di comando

Di [Cam Soper](https://twitter.com/camsoper)

Questa esercitazione illustra come compilare e distribuire un'applicazione ASP.NET Core in Servizio app di Microsoft Azure usando gli strumenti della riga di comando.  Al termine, sarà disponibile un'applicazione Web compilata in ASP.NET MVC Core ospitata come app Web di Servizio app di Azure.  Questa esercitazione è scritta tramite gli strumenti della riga di comando di Windows, ma può essere applicata anche agli ambienti macOS e Linux.  

In questa esercitazione si imparerà a:

> [!div class="checklist"]
> * Creare un sito Web di Servizio app di Azure con l'interfaccia della riga di comando di Azure
> * Distribuire un'applicazione ASP.NET Core in Servizio app di Azure con lo strumento della riga di comando

## <a name="prerequisites"></a>Prerequisiti

Per completare questa esercitazione, è necessario disporre di:

* Una [sottoscrizione di Microsoft Azure](https://azure.microsoft.com/free/)
* [.NET Core](https://www.microsoft.com/net/download/core)
* Client della riga di comando [Git](https://www.git-scm.com/)

## <a name="create-a-web-application"></a>Creare un'applicazione Web

Creare una nuova directory per l'applicazione Web, creare una nuova applicazione ASP.NET Core MVC e quindi eseguire il sito Web in locale.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[Altro](#tab/other)
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

Testare l'applicazione all'indirizzo http://localhost:5000.

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

Per distribuire l'applicazione tramite Git, è necessario un URL di distribuzione.  Recuperare l'URL come illustrato di seguito.

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
Prendere nota dell'URL visualizzato che termina con `.git`, che verrà usato nel passaggio successivo.

## <a name="deploy-the-application-using-git"></a>Distribuire l'applicazione tramite Git

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

Git richiede le credenziali di distribuzione impostate in precedenza. Dopo l'autenticazione, verrà eseguito il push dell'applicazione nel percorso remoto e l'applicazione sarà compilata e distribuita.

![Output della distribuzione di Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a>Testare l'applicazione

Testare l'applicazione all'indirizzo `https://<web app name>.azurewebsites.net`.  Per visualizzare l'indirizzo in Cloud Shell (o nell'interfaccia della riga di comando di Azure), usare il comando seguente:

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
> * Distribuire un'applicazione ASP.NET Core in Servizio app di Azure con lo strumento della riga di comando

Nel passaggio successivo si imparerà a usare la riga di comando per distribuire un'app Web esistente che usa CosmosDB.

> [!div class="nextstepaction"]
> [Distribuire in Azure dalla riga di comando con .NET Core](/dotnet/azure/dotnet-quickstart-xplat)
