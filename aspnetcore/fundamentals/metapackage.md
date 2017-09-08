---
title: Metapackage Microsoft.AspNetCore.All per ASP.NET Core 2. x e versioni successive
author: Rick-Anderson
description: Microsoft.AspNetCore.All metapackage include pacchetti tutte supportati.
keywords: ASP.NET Core, NuGet, creare un pacchetto, Microsoft.AspNetCore.All, metapackage
ms.author: riande
manager: wpickett
ms.date: 07/16/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: 255438a4ce36ce4978f8c8ee298388a25ac00d17
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="4bbf8-104">Metapackage Microsoft.AspNetCore.All per ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="4bbf8-104">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="4bbf8-105">Questa funzionalità richiede ASP.NET Core 2. x.</span><span class="sxs-lookup"><span data-stu-id="4bbf8-105">This feature requires ASP.NET Core 2.x.</span></span>

<span data-ttu-id="4bbf8-106">Il [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage per ASP.NET Core include:</span><span class="sxs-lookup"><span data-stu-id="4bbf8-106">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="4bbf8-107">Pacchetti tutte supportate dal team di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4bbf8-107">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="4bbf8-108">Tutte supportate dei pacchetti di base di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4bbf8-108">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="4bbf8-109">Interne e 3rd party dipendenze utilizzate da ASP.NET Core e componenti di base di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4bbf8-109">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="4bbf8-110">Tutte le funzionalità di ASP.NET Core 2. x e Entity Framework Core 2. x sono incluse nel `Microsoft.AspNetCore.All` pacchetto.</span><span class="sxs-lookup"><span data-stu-id="4bbf8-110">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="4bbf8-111">I modelli di progetto predefiniti utilizzano questo pacchetto.</span><span class="sxs-lookup"><span data-stu-id="4bbf8-111">The default project templates use this package.</span></span>

<span data-ttu-id="4bbf8-112">Il numero di versione di `Microsoft.AspNetCore.All` metapackage rappresenta la versione di ASP.NET Core e la versione di Entity Framework Core (allineato con la versione di .NET Core).</span><span class="sxs-lookup"><span data-stu-id="4bbf8-112">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="4bbf8-113">Le applicazioni che utilizzano il `Microsoft.AspNetCore.All` metapackage usufruire automaticamente di archivio di Runtime di .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4bbf8-113">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the .NET Core Runtime Store.</span></span> <span data-ttu-id="4bbf8-114">L'archivio di Runtime contiene tutte le risorse di runtime necessari per eseguire applicazioni a 2. x di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4bbf8-114">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="4bbf8-115">Quando si utilizza il `Microsoft.AspNetCore.All` metapackage, **non** asset dai pacchetti NuGet Core ASP.NET riferimento vengono distribuiti con l'applicazione &mdash; l'archivio di Runtime .NET Core contiene queste risorse.</span><span class="sxs-lookup"><span data-stu-id="4bbf8-115">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="4bbf8-116"><!-- todo add link to Runtime store -->Le risorse nell'archivio di Runtime sono precompilate per migliorare i tempi di avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4bbf8-116"><!-- todo add link to Runtime store --> The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="4bbf8-117">Per rimuovere i pacchetti che non si utilizzano, è possibile utilizzare il processo di rimozione del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="4bbf8-117">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="4bbf8-118">Pacchetti tagliati vengono esclusi nell'output applicazione pubblicata.</span><span class="sxs-lookup"><span data-stu-id="4bbf8-118">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="4bbf8-119">Nell'esempio *csproj* i riferimenti di file di `Microsoft.AspNetCore.All` metapackage per ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="4bbf8-119">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

<span data-ttu-id="4bbf8-120">[!code-xml[Principale](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="4bbf8-120">[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]</span></span>
