---
title: Ospitare e distribuire Blazor
author: guardrex
description: Informazioni su come ospitare e distribuire app Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/18/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 774fbb6fdaab14a015db4fb39de2e1ea73a1837b
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982640"
---
# <a name="host-and-deploy-blazor"></a><span data-ttu-id="f93d1-103">Ospitare e distribuire Blazor</span><span class="sxs-lookup"><span data-stu-id="f93d1-103">Host and deploy Blazor</span></span>

<span data-ttu-id="f93d1-104">Di [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="f93d1-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="f93d1-105">Pubblicare l'app</span><span class="sxs-lookup"><span data-stu-id="f93d1-105">Publish the app</span></span>

<span data-ttu-id="f93d1-106">Le app vengono pubblicate per la distribuzione nella configurazione di rilascio con il comando [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="f93d1-106">Apps are published for deployment in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="f93d1-107">Un ambiente di sviluppo integrato (IDE, Integrated Development Environment) può gestire l'esecuzione del comando `dotnet publish` automaticamente usando le funzionalità di pubblicazione predefinite, quindi potrebbe non essere necessario eseguire manualmente il comando da un prompt dei comandi a seconda degli strumenti di sviluppo in uso.</span><span class="sxs-lookup"><span data-stu-id="f93d1-107">An integrated development environment (IDE) may handle executing the `dotnet publish` command automatically using its built-in publishing features, so it might not be necessary to manually execute the command from a command prompt depending on the development tools in use.</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="f93d1-108">`dotnet publish` attiva un [ripristino](/dotnet/core/tools/dotnet-restore) delle dipendenze del progetto e [compila](/dotnet/core/tools/dotnet-build) il progetto prima di creare gli asset per la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="f93d1-108">`dotnet publish` triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="f93d1-109">Come parte del processo di compilazione, vengono rimossi gli assembly e i metodi non usati per ridurre le dimensioni del download e i tempi di caricamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="f93d1-109">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span> <span data-ttu-id="f93d1-110">La distribuzione viene creata nella cartella */bin/Release/{FRAMEWORK DESTINAZIONE}/publish*.</span><span class="sxs-lookup"><span data-stu-id="f93d1-110">The deployment is created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="f93d1-111">Gli asset nella cartella *publish* vengono distribuiti nel server Web.</span><span class="sxs-lookup"><span data-stu-id="f93d1-111">The assets in the *publish* folder are deployed to the web server.</span></span> <span data-ttu-id="f93d1-112">La distribuzione potrebbe essere un processo manuale o automatizzato a seconda degli strumenti di sviluppo in uso.</span><span class="sxs-lookup"><span data-stu-id="f93d1-112">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="f93d1-113">Distribuzione</span><span class="sxs-lookup"><span data-stu-id="f93d1-113">Deployment</span></span>

<span data-ttu-id="f93d1-114">Per indicazioni sulla distribuzione, vedere gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="f93d1-114">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>
