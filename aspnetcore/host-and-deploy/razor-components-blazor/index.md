---
title: Ospitare e distribuire Razor Components e Blazor
author: guardrex
description: Informazioni su come ospitare e distribuire Razor Components e app Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: host-and-deploy/razor-components-blazor/index
ms.openlocfilehash: eeaa27bef584523b77d8b47000b310fe0cac7341
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2019
ms.locfileid: "59070308"
---
# <a name="host-and-deploy-razor-components-and-blazor"></a><span data-ttu-id="9e31d-103">Ospitare e distribuire Razor Components e Blazor</span><span class="sxs-lookup"><span data-stu-id="9e31d-103">Host and deploy Razor Components and Blazor</span></span>

<span data-ttu-id="9e31d-104">Di [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="9e31d-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="9e31d-105">Pubblicare l'app</span><span class="sxs-lookup"><span data-stu-id="9e31d-105">Publish the app</span></span>

<span data-ttu-id="9e31d-106">Le app vengono pubblicate per la distribuzione nella configurazione di rilascio con il comando [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="9e31d-106">Apps are published for deployment in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="9e31d-107">Un ambiente di sviluppo integrato (IDE, Integrated Development Environment) può gestire l'esecuzione del comando `dotnet publish` automaticamente usando le funzionalità di pubblicazione predefinite, quindi potrebbe non essere necessario eseguire manualmente il comando da un prompt dei comandi a seconda degli strumenti di sviluppo in uso.</span><span class="sxs-lookup"><span data-stu-id="9e31d-107">An integrated development environment (IDE) may handle executing the `dotnet publish` command automatically using its built-in publishing features, so it might not be necessary to manually execute the command from a command prompt depending on the development tools in use.</span></span>

```console
dotnet publish -c Release
```

`dotnet publish` <span data-ttu-id="9e31d-108">attiva un [ripristino](/dotnet/core/tools/dotnet-restore) delle dipendenze del progetto e [compila](/dotnet/core/tools/dotnet-build) il progetto prima di creare gli asset per la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="9e31d-108">triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="9e31d-109">Come parte del processo di compilazione, vengono rimossi gli assembly e i metodi non usati per ridurre le dimensioni del download e i tempi di caricamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="9e31d-109">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span> <span data-ttu-id="9e31d-110">La distribuzione viene creata nella cartella */bin/Release/{FRAMEWORK DESTINAZIONE}/publish*.</span><span class="sxs-lookup"><span data-stu-id="9e31d-110">The deployment is created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="9e31d-111">Gli asset nella cartella *publish* vengono distribuiti nel server Web.</span><span class="sxs-lookup"><span data-stu-id="9e31d-111">The assets in the *publish* folder are deployed to the web server.</span></span> <span data-ttu-id="9e31d-112">La distribuzione potrebbe essere un processo manuale o automatizzato a seconda degli strumenti di sviluppo in uso.</span><span class="sxs-lookup"><span data-stu-id="9e31d-112">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="9e31d-113">Distribuzione</span><span class="sxs-lookup"><span data-stu-id="9e31d-113">Deployment</span></span>

<span data-ttu-id="9e31d-114">Per indicazioni sulla distribuzione, vedere gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="9e31d-114">For deployment guidance, see the following topics:</span></span>

* [<span data-ttu-id="9e31d-115">Blazor sul lato client</span><span class="sxs-lookup"><span data-stu-id="9e31d-115">Client-side Blazor</span></span>](xref:host-and-deploy/razor-components-blazor/blazor)
* [<span data-ttu-id="9e31d-116">Razor Components ASP.NET Core sul lato server</span><span class="sxs-lookup"><span data-stu-id="9e31d-116">Server-side ASP.NET Core Razor Components</span></span>](xref:host-and-deploy/razor-components-blazor/razor-components)
