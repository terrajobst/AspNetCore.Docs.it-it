---
title: Metapackage Microsoft.AspNetCore.All per ASP.NET Core 2. x e versioni successive
author: Rick-Anderson
description: Il metapackage Microsoft.AspNetCore.All include tutte supportate dei pacchetti di ASP.NET Core e di Entity Framework Core, con le relative dipendenze.
keywords: Core,NuGet,package,Microsoft.AspNetCore.All,metapackage ASP.NET
ms.author: riande
manager: wpickett
ms.date: 09/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: 23a07867874eb534c75c4e7b3be00c4a376f8a8b
ms.sourcegitcommit: 4e45fd4e3f1374cd51cc931cee93c9d72631d9fc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2017
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="05e41-104">Metapackage Microsoft.AspNetCore.All per ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="05e41-104">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="05e41-105">Questa funzionalità richiede ASP.NET Core 2. x.</span><span class="sxs-lookup"><span data-stu-id="05e41-105">This feature requires ASP.NET Core 2.x.</span></span>

<span data-ttu-id="05e41-106">Il [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage per ASP.NET Core include:</span><span class="sxs-lookup"><span data-stu-id="05e41-106">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="05e41-107">Pacchetti tutte supportate dal team di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="05e41-107">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="05e41-108">Tutte supportate dei pacchetti di base di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="05e41-108">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="05e41-109">Interne e 3rd party dipendenze utilizzate da ASP.NET Core e componenti di base di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="05e41-109">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="05e41-110">Tutte le funzionalità di ASP.NET Core 2. x e Entity Framework Core 2. x sono incluse nel `Microsoft.AspNetCore.All` pacchetto.</span><span class="sxs-lookup"><span data-stu-id="05e41-110">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="05e41-111">I modelli di progetto predefiniti utilizzano questo pacchetto.</span><span class="sxs-lookup"><span data-stu-id="05e41-111">The default project templates use this package.</span></span>

<span data-ttu-id="05e41-112">Il numero di versione di `Microsoft.AspNetCore.All` metapackage rappresenta la versione di ASP.NET Core e la versione di Entity Framework Core (allineato con la versione di .NET Core).</span><span class="sxs-lookup"><span data-stu-id="05e41-112">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="05e41-113">Le applicazioni che utilizzano il `Microsoft.AspNetCore.All` metapackage automaticamente sfruttare il [archivio di Runtime di .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="05e41-113">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="05e41-114">L'archivio di Runtime contiene tutte le risorse di runtime necessari per eseguire applicazioni a 2. x di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="05e41-114">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="05e41-115">Quando si utilizza il `Microsoft.AspNetCore.All` metapackage, **non** asset dai pacchetti NuGet Core ASP.NET riferimento vengono distribuiti con l'applicazione &mdash; l'archivio di Runtime .NET Core contiene queste risorse.</span><span class="sxs-lookup"><span data-stu-id="05e41-115">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="05e41-116">Le risorse nell'archivio di Runtime sono precompilate per migliorare i tempi di avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="05e41-116">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="05e41-117">Per rimuovere i pacchetti che non si utilizzano, è possibile utilizzare il processo di rimozione del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="05e41-117">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="05e41-118">Pacchetti tagliati vengono esclusi nell'output applicazione pubblicata.</span><span class="sxs-lookup"><span data-stu-id="05e41-118">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="05e41-119">Nell'esempio *csproj* i riferimenti di file di `Microsoft.AspNetCore.All` metapackage per ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="05e41-119">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
