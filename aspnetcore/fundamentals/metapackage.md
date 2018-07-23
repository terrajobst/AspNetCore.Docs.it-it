---
title: Metapacchetto Microsoft.AspNetCore.All per ASP.NET Core 2.0
author: Rick-Anderson
description: Il metapacchetto Microsoft.AspNetCore.All include tutti i pacchetti ASP.NET Core e Entity Framework Core supportati con le relative dipendenze.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage
ms.openlocfilehash: fbc0f5465dc37a612b81c293f1a58b53ea4b2238
ms.sourcegitcommit: cb0c27fa0184f954fce591d417e6ab2a51d8bb22
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39123827"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a><span data-ttu-id="73c27-103">Metapacchetto Microsoft.AspNetCore.All per ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="73c27-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0</span></span>

> [!NOTE]
> <span data-ttu-id="73c27-104">È consigliabile che le applicazioni destinate a ASP.NET Core 2.1 e versioni successive usino [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) invece di questo pacchetto.</span><span class="sxs-lookup"><span data-stu-id="73c27-104">We recommend applications targeting ASP.NET Core 2.1 and later use the [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) rather than this package.</span></span> <span data-ttu-id="73c27-105">Vedere [Migrazione da Microsoft.AspNetCore.All a Microsoft.AspNetCore.App](#migrate) in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="73c27-105">See [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](#migrate) in this article.</span></span>

<span data-ttu-id="73c27-106">Questa funzionalità richiede ASP.NET Core 2.x con destinazione .NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="73c27-106">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="73c27-107">Il metapacchetto [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) per ASP.NET include:</span><span class="sxs-lookup"><span data-stu-id="73c27-107">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="73c27-108">Tutti i pacchetti supportati dal team ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="73c27-108">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="73c27-109">Tutti i pacchetti supportati da Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="73c27-109">All supported packages by the Entity Framework Core.</span></span>
* <span data-ttu-id="73c27-110">Le dipendenze interne e di terze parti usate da ASP.NET Core e da Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="73c27-110">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="73c27-111">Tutte le funzionalità di ASP.NET Core 2.x e Entity Framework Core 2.x sono incluse nel pacchetto `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="73c27-111">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="73c27-112">I modelli di progetto predefiniti destinati ad ASP.NET Core 2.0 usano questo pacchetto.</span><span class="sxs-lookup"><span data-stu-id="73c27-112">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="73c27-113">Il numero di versione del metapacchetto `Microsoft.AspNetCore.All` rappresenta la versione di ASP.NET Core e la versione di Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="73c27-113">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="73c27-114">Le applicazioni che usano il metapacchetto `Microsoft.AspNetCore.All` sfruttano automaticamente l'[archivio di runtime di .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="73c27-114">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="73c27-115">L'archivio di runtime contiene tutti gli asset di runtime necessari per eseguire le applicazioni ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="73c27-115">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="73c27-116">Quando si usa il metapacchetto `Microsoft.AspNetCore.All`, con l'applicazione **non** vengono distribuiti asset dai pacchetti NuGet di riferimento di ASP.NET Core. Questi asset sono infatti inclusi nell'archivio di runtime di .NET Core.</span><span class="sxs-lookup"><span data-stu-id="73c27-116">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="73c27-117">Gli asset contenuti nell'archivio di runtime sono precompilati per migliorare i tempi di avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="73c27-117">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="73c27-118">Per rimuovere i pacchetti che non vengono usati è possibile usare il processo di trimming dei pacchetti.</span><span class="sxs-lookup"><span data-stu-id="73c27-118">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="73c27-119">I pacchetti di cui è stato eseguito il trimming sono esclusi dall'output dell'applicazione pubblicata.</span><span class="sxs-lookup"><span data-stu-id="73c27-119">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="73c27-120">Il file con estensione *csproj* seguente fa riferimento al metapacchetto `Microsoft.AspNetCore.All` per ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="73c27-120">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

<a name="migrate"></a>
## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a><span data-ttu-id="73c27-121">Migrazione da Microsoft.AspNetCore.All a Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="73c27-121">Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App</span></span>

<span data-ttu-id="73c27-122">I pacchetti seguenti sono inclusi in `Microsoft.AspNetCore.All` ma non il pacchetto `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="73c27-122">The following packages are included in `Microsoft.AspNetCore.All` but not the `Microsoft.AspNetCore.App` package.</span></span> 

* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServices.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServicesIntegration`
* `Microsoft.AspNetCore.DataProtection.AzureKeyVault`
* `Microsoft.AspNetCore.DataProtection.AzureStorage`
* `Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv`
* `Microsoft.AspNetCore.SignalR.Redis`
* `Microsoft.Data.Sqlite`
* `Microsoft.Data.Sqlite.Core`
* `Microsoft.EntityFrameworkCore.Sqlite`
* `Microsoft.EntityFrameworkCore.Sqlite.Core`
* `Microsoft.Extensions.Caching.Redis`
* `Microsoft.Extensions.Configuration.AzureKeyVault`
* `Microsoft.Extensions.Logging.AzureAppServices`
* `Microsoft.VisualStudio.Web.BrowserLink`

<span data-ttu-id="73c27-123">Per passare da `Microsoft.AspNetCore.All` a `Microsoft.AspNetCore.App`, se l'app usa qualsiasi API dai pacchetti sopra o pacchetti inseriti da tali pacchetti, aggiungere i riferimenti a tali pacchetti nel progetto.</span><span class="sxs-lookup"><span data-stu-id="73c27-123">To move from `Microsoft.AspNetCore.All` to `Microsoft.AspNetCore.App`, if your app uses any APIs from the above packages, or packages brought in by those packages, add references to those packages in your project.</span></span>

<span data-ttu-id="73c27-124">Tutte le dipendenze dei pacchetti precedenti che non sono in altro modo dipendenze di `Microsoft.AspNetCore.App` non sono incluse in modo implicito.</span><span class="sxs-lookup"><span data-stu-id="73c27-124">Any dependencies of the preceding packages that otherwise aren't dependencies of `Microsoft.AspNetCore.App` are not included implicitly.</span></span> <span data-ttu-id="73c27-125">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="73c27-125">For example:</span></span>

* <span data-ttu-id="73c27-126">`StackExchange.Redis` come dipendenza di `Microsoft.Extensions.Caching.Redis`</span><span class="sxs-lookup"><span data-stu-id="73c27-126">`StackExchange.Redis` as a dependency of `Microsoft.Extensions.Caching.Redis`</span></span>
* <span data-ttu-id="73c27-127">`Microsoft.ApplicationInsights` come dipendenza di `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span><span class="sxs-lookup"><span data-stu-id="73c27-127">`Microsoft.ApplicationInsights` as a dependency of `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span></span>
