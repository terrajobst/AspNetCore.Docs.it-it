---
title: Metapacchetto Microsoft.AspNetCore.All per ASP.NET Core 2.x e versioni successive
author: Rick-Anderson
description: Il metapacchetto Microsoft.AspNetCore.All include tutti i pacchetti ASP.NET Core e Entity Framework Core supportati con le relative dipendenze.
manager: wpickett
monikerRange: = aspnetcore-2.0
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: 4c11f15e659565325bfe8b8d91188b62177b251d
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/18/2018
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="16474-103">Metapacchetto Microsoft.AspNetCore.All per ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="16474-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="16474-104">Questa funzionalità richiede ASP.NET Core 2.x con destinazione .NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="16474-104">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="16474-105">Il metapacchetto [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) per ASP.NET include:</span><span class="sxs-lookup"><span data-stu-id="16474-105">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="16474-106">Tutti i pacchetti supportati dal team ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="16474-106">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="16474-107">Tutti i pacchetti supportati da Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="16474-107">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="16474-108">Le dipendenze interne e di terze parti usate da ASP.NET Core e da Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="16474-108">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="16474-109">Tutte le funzionalità di ASP.NET Core 2.x e Entity Framework Core 2.x sono incluse nel pacchetto `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="16474-109">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="16474-110">I modelli di progetto predefiniti destinati ad ASP.NET Core 2.0 usano questo pacchetto.</span><span class="sxs-lookup"><span data-stu-id="16474-110">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="16474-111">Il numero di versione del metapacchetto `Microsoft.AspNetCore.All` rappresenta la versione di ASP.NET Core e la versione di Entity Framework Core (allineata alla versione di .NET Core).</span><span class="sxs-lookup"><span data-stu-id="16474-111">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="16474-112">Le applicazioni che usano il metapacchetto `Microsoft.AspNetCore.All` sfruttano automaticamente l'[archivio di runtime di .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="16474-112">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="16474-113">L'archivio di runtime contiene tutti gli asset di runtime necessari per eseguire le applicazioni ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="16474-113">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="16474-114">Quando si usa il metapacchetto `Microsoft.AspNetCore.All`, con l'applicazione **non** vengono distribuiti asset dai pacchetti NuGet di riferimento di ASP.NET Core. Questi asset sono infatti inclusi nell'archivio di runtime di .NET Core.</span><span class="sxs-lookup"><span data-stu-id="16474-114">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="16474-115">Gli asset contenuti nell'archivio di runtime sono precompilati per migliorare i tempi di avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="16474-115">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="16474-116">Per rimuovere i pacchetti che non vengono usati è possibile usare il processo di trimming dei pacchetti.</span><span class="sxs-lookup"><span data-stu-id="16474-116">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="16474-117">I pacchetti di cui è stato eseguito il trimming sono esclusi dall'output dell'applicazione pubblicata.</span><span class="sxs-lookup"><span data-stu-id="16474-117">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="16474-118">Il file con estensione *csproj* seguente fa riferimento al metapacchetto `Microsoft.AspNetCore.All` per ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="16474-118">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](../mvc/views/view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=9)]
