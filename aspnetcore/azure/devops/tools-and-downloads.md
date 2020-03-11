---
title: Strumenti e download - DevOps con ASP.NET Core e Azure
author: CamSoper
description: Strumenti e download necessari per DevOps con ASP.NET Core e Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: 25ce311373b0aaddfa3bc2728c39e503acbca69d
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659488"
---
# <a name="tools-and-downloads"></a><span data-ttu-id="17146-103">Strumenti e download</span><span class="sxs-lookup"><span data-stu-id="17146-103">Tools and downloads</span></span>

<span data-ttu-id="17146-104">Azure offre diverse interfacce per il provisioning e la gestione delle risorse, ad esempio [portale di Azure](https://portal.azure.com), l'interfaccia della riga di comando di [Azure](/cli/azure/), [Azure PowerShell](/powershell/azure/overview), [Azure cloud Shell](https://shell.azure.com/bash)e Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="17146-104">Azure has several interfaces for provisioning and managing resources, such as the [Azure portal](https://portal.azure.com), [Azure CLI](/cli/azure/), [Azure PowerShell](/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash), and Visual Studio.</span></span> <span data-ttu-id="17146-105">Questa Guida adotta un approccio minimalista e Usa Azure Cloud Shell ogni volta che è possibile ridurre i passaggi necessari.</span><span class="sxs-lookup"><span data-stu-id="17146-105">This guide takes a minimalist approach and uses the Azure Cloud Shell whenever possible to reduce the steps required.</span></span> <span data-ttu-id="17146-106">Tuttavia, il portale di Azure deve essere usato per alcune parti.</span><span class="sxs-lookup"><span data-stu-id="17146-106">However, the Azure portal must be used for some portions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="17146-107">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="17146-107">Prerequisites</span></span>

<span data-ttu-id="17146-108">Le sottoscrizioni seguenti sono necessari:</span><span class="sxs-lookup"><span data-stu-id="17146-108">The following subscriptions are required:</span></span>

* <span data-ttu-id="17146-109">Azure &mdash; se non si ha un account, [ottenere una versione di valutazione gratuita](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="17146-109">Azure &mdash; If you don't have an account, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="17146-110">Azure DevOps Services &mdash; la sottoscrizione e l'organizzazione di Azure DevOps vengono create nel capitolo 4.</span><span class="sxs-lookup"><span data-stu-id="17146-110">Azure DevOps Services &mdash; your Azure DevOps subscription and organization is created in Chapter 4.</span></span>
* <span data-ttu-id="17146-111">GitHub &mdash; se non si ha un account, [iscriversi](https://github.com/join)gratuitamente.</span><span class="sxs-lookup"><span data-stu-id="17146-111">GitHub &mdash; If you don't have an account, [sign up for free](https://github.com/join).</span></span>

<span data-ttu-id="17146-112">Gli strumenti seguenti sono necessari:</span><span class="sxs-lookup"><span data-stu-id="17146-112">The following tools are required:</span></span>

* <span data-ttu-id="17146-113">[Git](https://git-scm.com/downloads) &mdash; una conoscenza di base di Git è consigliata per questa guida.</span><span class="sxs-lookup"><span data-stu-id="17146-113">[Git](https://git-scm.com/downloads) &mdash; A fundamental understanding of Git is recommended for this guide.</span></span> <span data-ttu-id="17146-114">Esaminare la [documentazione di git](https://git-scm.com/doc), in particolare [git Remote](https://git-scm.com/docs/git-remote) e [git push](https://git-scm.com/docs/git-push).</span><span class="sxs-lookup"><span data-stu-id="17146-114">Review the [Git documentation](https://git-scm.com/doc), specifically [git remote](https://git-scm.com/docs/git-remote) and [git push](https://git-scm.com/docs/git-push).</span></span>
* <span data-ttu-id="17146-115">Per compilare ed eseguire l'app di esempio, è necessario [.NET Core SDK](https://www.microsoft.com/net/download/) &mdash; versione 2.1.300 o successiva.</span><span class="sxs-lookup"><span data-stu-id="17146-115">[.NET Core SDK](https://www.microsoft.com/net/download/) &mdash; Version 2.1.300 or later is required to build and run the sample app.</span></span> <span data-ttu-id="17146-116">Se Visual Studio viene installato con il carico di lavoro **sviluppo multipiattaforma .NET Core** , il .NET Core SDK è già installato.</span><span class="sxs-lookup"><span data-stu-id="17146-116">If Visual Studio is installed with the **.NET Core cross-platform development** workload, the .NET Core SDK is already installed.</span></span>

    <span data-ttu-id="17146-117">Verificare l'installazione di .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="17146-117">Verify your .NET Core SDK installation.</span></span> <span data-ttu-id="17146-118">Aprire una shell dei comandi ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="17146-118">Open a command shell, and run the following command:</span></span>

    ```dotnetcli
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a><span data-ttu-id="17146-119">Strumenti consigliati (solo Windows)</span><span class="sxs-lookup"><span data-stu-id="17146-119">Recommended tools (Windows only)</span></span>

* <span data-ttu-id="17146-120">I robusti strumenti di Azure di [Visual Studio](https://visualstudio.microsoft.com)forniscono un'interfaccia utente grafica per la maggior parte delle funzionalità descritte in questa guida.</span><span class="sxs-lookup"><span data-stu-id="17146-120">[Visual Studio](https://visualstudio.microsoft.com)'s robust Azure tools provide a GUI for most of the functionality described in this guide.</span></span> <span data-ttu-id="17146-121">Qualsiasi edizione di Visual Studio funzionerà, tra cui l'edizione gratuita di Visual Studio Community.</span><span class="sxs-lookup"><span data-stu-id="17146-121">Any edition of Visual Studio will work, including the free Visual Studio Community Edition.</span></span> <span data-ttu-id="17146-122">Le esercitazioni vengono scritti per illustrare lo sviluppo, distribuzione e DevOps con e senza Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="17146-122">The tutorials are written to demonstrate development, deployment, and DevOps both with and without Visual Studio.</span></span>

  <span data-ttu-id="17146-123">Verificare che in Visual Studio siano installati i [carichi di lavoro](/visualstudio/install/modify-visual-studio) seguenti:</span><span class="sxs-lookup"><span data-stu-id="17146-123">Confirm that Visual Studio has the following [workloads](/visualstudio/install/modify-visual-studio) installed:</span></span>

  * <span data-ttu-id="17146-124">Sviluppo Web e ASP.NET</span><span class="sxs-lookup"><span data-stu-id="17146-124">ASP.NET and web development</span></span>
  * <span data-ttu-id="17146-125">Sviluppo di Azure</span><span class="sxs-lookup"><span data-stu-id="17146-125">Azure development</span></span>
  * <span data-ttu-id="17146-126">Sviluppo multipiattaforma .NET Core</span><span class="sxs-lookup"><span data-stu-id="17146-126">.NET Core cross-platform development</span></span>
