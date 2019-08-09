---
title: Hosting e distribuzione di ASP.NET Core Blazor
author: guardrex
description: Informazioni su come ospitare e distribuire app Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: d18abbf33c71dca5130bfc6b503b46c1d5bce537
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2019
ms.locfileid: "68913930"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a><span data-ttu-id="90e57-103">Hosting e distribuzione di ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="90e57-103">Host and deploy ASP.NET Core Blazor</span></span>

<span data-ttu-id="90e57-104">Di [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="90e57-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="90e57-105">Pubblicare l'app</span><span class="sxs-lookup"><span data-stu-id="90e57-105">Publish the app</span></span>

<span data-ttu-id="90e57-106">Le app vengono pubblicate per la distribuzione nella configurazione per il rilascio.</span><span class="sxs-lookup"><span data-stu-id="90e57-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="90e57-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="90e57-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="90e57-108">Selezionare **Compila** > **Pubblica {APPLICAZIONE}** sulla barra di spostamento.</span><span class="sxs-lookup"><span data-stu-id="90e57-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="90e57-109">Selezionare la *destinazione di pubblicazione*.</span><span class="sxs-lookup"><span data-stu-id="90e57-109">Select the *publish target*.</span></span> <span data-ttu-id="90e57-110">Per pubblicare in locale, selezionare **Cartella**.</span><span class="sxs-lookup"><span data-stu-id="90e57-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="90e57-111">Accettare il percorso predefinito nel campo **Scegliere una cartella** oppure specificarne uno diverso.</span><span class="sxs-lookup"><span data-stu-id="90e57-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="90e57-112">Selezionare il pulsante **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="90e57-112">Select the **Publish** button.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="90e57-113">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="90e57-113">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="90e57-114">Usare il comando [dotnet publish](/dotnet/core/tools/dotnet-publish) per pubblicare l'app con una configurazione per il rilascio:</span><span class="sxs-lookup"><span data-stu-id="90e57-114">Use the [dotnet publish](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```console
dotnet publish -c Release
```

---

<span data-ttu-id="90e57-115">La pubblicazione dell'app attiva un [ripristino](/dotnet/core/tools/dotnet-restore) delle dipendenze del progetto e [compila](/dotnet/core/tools/dotnet-build) il progetto prima di creare gli asset per la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="90e57-115">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="90e57-116">Come parte del processo di compilazione, vengono rimossi gli assembly e i metodi non usati per ridurre le dimensioni del download e i tempi di caricamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="90e57-116">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="90e57-117">Un'app lato client Blazor viene pubblicata nella cartella */bin/Release/{FRAMEWORK DI DESTINAZIONE}/publish/{NOME ASSEMBLY}/dist*.</span><span class="sxs-lookup"><span data-stu-id="90e57-117">A Blazor client-side app is published to the */bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span> <span data-ttu-id="90e57-118">Un'app lato server Blazor viene pubblicata nella cartella */bin/Release/{FRAMEWORK DI DESTINAZIONE}/publish*.</span><span class="sxs-lookup"><span data-stu-id="90e57-118">A Blazor server-side app is published to the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="90e57-119">Gli asset nella cartella vengono distribuiti nel server Web.</span><span class="sxs-lookup"><span data-stu-id="90e57-119">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="90e57-120">La distribuzione potrebbe essere un processo manuale o automatizzato a seconda degli strumenti di sviluppo in uso.</span><span class="sxs-lookup"><span data-stu-id="90e57-120">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="90e57-121">Distribuzione</span><span class="sxs-lookup"><span data-stu-id="90e57-121">Deployment</span></span>

<span data-ttu-id="90e57-122">Per indicazioni sulla distribuzione, vedere gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="90e57-122">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>

## <a name="blazor-serverless-hosting-with-azure-storage"></a><span data-ttu-id="90e57-123">Hosting serverless in Blazor con Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="90e57-123">Blazor serverless hosting with Azure Storage</span></span>

<span data-ttu-id="90e57-124">Le app Blazor sul lato client possono essere rese disponibili da [Archiviazione di Azure](https://azure.microsoft.com/services/storage/) come contenuto statico direttamente da un contenitore di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="90e57-124">Blazor client-side apps can be served from [Azure Storage](https://azure.microsoft.com/services/storage/) as static content directly from a storage container.</span></span>

<span data-ttu-id="90e57-125">Per altre informazioni, vedere [Ospitare e distribuire ASP.NET Core Blazor sul lato client (distribuzione autonoma): Archiviazione di Azure](xref:host-and-deploy/blazor/client-side#azure-storage).</span><span class="sxs-lookup"><span data-stu-id="90e57-125">For more information, see [Host and deploy ASP.NET Core Blazor client-side (Standalone deployment): Azure Storage](xref:host-and-deploy/blazor/client-side#azure-storage).</span></span>
