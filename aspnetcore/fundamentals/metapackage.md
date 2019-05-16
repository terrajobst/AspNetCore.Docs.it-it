---
title: Metapacchetto Microsoft.AspNetCore.All per ASP.NET Core 2.0
author: Rick-Anderson
description: Il metapacchetto Microsoft.AspNetCore.All non è consigliato per ASP.NET Core 2.1 e versioni successive.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/metapackage
ms.openlocfilehash: 5d49213e6d694f121d8301c94ba71782b2dc45cf
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086942"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a><span data-ttu-id="0af17-103">Metapacchetto Microsoft.AspNetCore.All per ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="0af17-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0</span></span>

> [!NOTE]
> <span data-ttu-id="0af17-104">È consigliabile che le applicazioni destinate a ASP.NET Core 2.1 e versioni successive usino il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) invece di questo pacchetto.</span><span class="sxs-lookup"><span data-stu-id="0af17-104">We recommend applications targeting ASP.NET Core 2.1 and later use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) rather than this package.</span></span> <span data-ttu-id="0af17-105">Vedere [Migrazione da Microsoft.AspNetCore.All a Microsoft.AspNetCore.App](#migrate) in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="0af17-105">See [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](#migrate) in this article.</span></span>

<span data-ttu-id="0af17-106">Questa funzionalità richiede ASP.NET Core 2.x con destinazione .NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="0af17-106">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="0af17-107">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) è un metapacchetto che fa riferimento a un framework condiviso.</span><span class="sxs-lookup"><span data-stu-id="0af17-107">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) is a metapackage that refers to a shared framework.</span></span> <span data-ttu-id="0af17-108">Un *framework condiviso* è un set di assembly (file *DLL*) che non sono presenti nelle cartelle dell'app.</span><span class="sxs-lookup"><span data-stu-id="0af17-108">A *shared framework* is a set of assemblies (*.dll* files) that are not in the app's folders.</span></span> <span data-ttu-id="0af17-109">Il framework condiviso deve essere installato nel computer per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="0af17-109">The shared framework must be installed on the machine to run the app.</span></span> <span data-ttu-id="0af17-110">Per altre informazioni, vedere [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) (Il framework condiviso).</span><span class="sxs-lookup"><span data-stu-id="0af17-110">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="0af17-111">Il framework condiviso cui fa riferimento `Microsoft.AspNetCore.All` include:</span><span class="sxs-lookup"><span data-stu-id="0af17-111">The shared framework that `Microsoft.AspNetCore.All` refers to includes:</span></span>

* <span data-ttu-id="0af17-112">Tutti i pacchetti supportati dal team ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0af17-112">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="0af17-113">Tutti i pacchetti supportati da Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0af17-113">All supported packages by the Entity Framework Core.</span></span>
* <span data-ttu-id="0af17-114">Le dipendenze interne e di terze parti usate da ASP.NET Core e da Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0af17-114">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="0af17-115">Tutte le funzionalità di ASP.NET Core 2.x e Entity Framework Core 2.x sono incluse nel pacchetto `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="0af17-115">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="0af17-116">I modelli di progetto predefiniti destinati ad ASP.NET Core 2.0 usano questo pacchetto.</span><span class="sxs-lookup"><span data-stu-id="0af17-116">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="0af17-117">Il numero di versione del metapacchetto `Microsoft.AspNetCore.All` rappresenta le versioni minime di ASP.NET Core e di Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0af17-117">The version number of the `Microsoft.AspNetCore.All` metapackage represents the minimum ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="0af17-118">Il file con estensione *csproj* seguente fa riferimento al metapacchetto `Microsoft.AspNetCore.All` per ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="0af17-118">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=8)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implicit-versioning"></a><span data-ttu-id="0af17-119">Controllo delle versioni implicito</span><span class="sxs-lookup"><span data-stu-id="0af17-119">Implicit versioning</span></span>

<span data-ttu-id="0af17-120">In ASP.NET Core 2.1 o versioni successive è possibile specificare il riferimento al pacchetto `Microsoft.AspNetCore.All` senza la versione.</span><span class="sxs-lookup"><span data-stu-id="0af17-120">In ASP.NET Core 2.1 or later, you can specify the `Microsoft.AspNetCore.All` package reference without a version.</span></span> <span data-ttu-id="0af17-121">Quando la versione non è specificata, il SDK specifica una versione implicita (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="0af17-121">When the version isn't specified, an implicit version is specified by the SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="0af17-122">È consigliabile basarsi sulla versione implicita specificata dall'SDK e non impostando in modo esplicito il numero di versione sul riferimento al pacchetto.</span><span class="sxs-lookup"><span data-stu-id="0af17-122">We recommend relying on the implicit version specified by the SDK and not explicitly setting the version number on the package reference.</span></span> <span data-ttu-id="0af17-123">In caso di domande su questo approccio, lasciare un commento GitHub nella pagina della [discussione per la versione implicita Microsoft.AspNetCore.App](https://github.com/aspnet/AspNetCore.Docs/issues/6430).</span><span class="sxs-lookup"><span data-stu-id="0af17-123">If you have questions about this approach, leave a GitHub comment at the [Discussion for the Microsoft.AspNetCore.App implicit version](https://github.com/aspnet/AspNetCore.Docs/issues/6430).</span></span>

<span data-ttu-id="0af17-124">La versione implicita è impostata su `major.minor.0` per le app portabili.</span><span class="sxs-lookup"><span data-stu-id="0af17-124">The implicit version is set to `major.minor.0` for portable apps.</span></span> <span data-ttu-id="0af17-125">Il meccanismo di roll forward del framework condiviso esegue l'app sulla versione compatibile più recente tra i framework condivisi installati.</span><span class="sxs-lookup"><span data-stu-id="0af17-125">The shared framework roll-forward mechanism runs the app on the latest compatible version among the installed shared frameworks.</span></span> <span data-ttu-id="0af17-126">Per garantire che venga usata la stessa versione in fase di sviluppo, test e produzione, verificare che in tutti gli ambienti sia installata la stessa versione del framework condiviso.</span><span class="sxs-lookup"><span data-stu-id="0af17-126">To guarantee the same version is used in development, test, and production, ensure the same version of the shared framework is installed in all environments.</span></span> <span data-ttu-id="0af17-127">Per le app autonome, il numero di versione implicita è impostato sul valore `major.minor.patch` del framework condiviso nel bundle nell'SDK installato.</span><span class="sxs-lookup"><span data-stu-id="0af17-127">For self-contained apps, the implicit version number is set to the `major.minor.patch` of the shared framework bundled in the installed SDK.</span></span>

<span data-ttu-id="0af17-128">Il fatto di specificare un numero di versione nel riferimento del pacchetto `Microsoft.AspNetCore.All` **non** garantisce che verrà scelta la versione corrispondente del framework condiviso.</span><span class="sxs-lookup"><span data-stu-id="0af17-128">Specifying a version number on the `Microsoft.AspNetCore.All` package reference does **not** guarantee that version of the shared framework is chosen.</span></span> <span data-ttu-id="0af17-129">Si supponga ad esempio che venga specificata la versione "2.1.1" ma che sia installata la versione "2.1.3".</span><span class="sxs-lookup"><span data-stu-id="0af17-129">For example, suppose version "2.1.1" is specified, but "2.1.3" is installed.</span></span> <span data-ttu-id="0af17-130">In tal caso, l'app userà "2.1.3".</span><span class="sxs-lookup"><span data-stu-id="0af17-130">In that case, the app will use "2.1.3".</span></span> <span data-ttu-id="0af17-131">Benché non sia consigliabile, è possibile disabilitare il roll forward (patch e/o versioni secondarie).</span><span class="sxs-lookup"><span data-stu-id="0af17-131">Although not recommended, you can disable roll forward (patch and/or minor).</span></span> <span data-ttu-id="0af17-132">Per altre informazioni su come eseguire il roll forward dell'host dotnet e configurarne il comportamento, vedere [dotnet host rollforward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md) (Roll forward dell'host dotnet).</span><span class="sxs-lookup"><span data-stu-id="0af17-132">For more information regarding dotnet host roll-forward and how to configure its behavior, see [dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).</span></span>

<span data-ttu-id="0af17-133">Il SDK del progetto deve essere impostato su `Microsoft.NET.Sdk.Web` nel file di progetto perché venga usata la versione implicita di `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="0af17-133">The project's SDK must be set to `Microsoft.NET.Sdk.Web` in the project file to use the implicit version of `Microsoft.AspNetCore.All`.</span></span> <span data-ttu-id="0af17-134">Se è specificato il SDK `Microsoft.NET.Sdk` (`<Project Sdk="Microsoft.NET.Sdk">` nella parte superiore del file di progetto), viene generato l'avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="0af17-134">When the `Microsoft.NET.Sdk` SDK is specified (`<Project Sdk="Microsoft.NET.Sdk">` at the top of the project file), the following warning is generated:</span></span>

<span data-ttu-id="0af17-135">*Avviso NU1604: La dipendenza Microsoft.AspNetCore.All del progetto non contiene un limite inferiore inclusivo. Includere un limite inferiore nella versione della dipendenza per garantire risultati di ripristino coerenti.*</span><span class="sxs-lookup"><span data-stu-id="0af17-135">*Warning NU1604: Project dependency Microsoft.AspNetCore.All does not contain an inclusive lower bound. Include a lower bound in the dependency version to ensure consistent restore results.*</span></span>

<span data-ttu-id="0af17-136">Si tratta di un problema noto con .NET Core 2.1 SDK, che verrà risolto in .NET Core 2.2 SDK.</span><span class="sxs-lookup"><span data-stu-id="0af17-136">This is a known issue with the .NET Core 2.1 SDK and will be fixed in the .NET Core 2.2 SDK.</span></span>

::: moniker-end

<a name="migrate"></a>

## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a><span data-ttu-id="0af17-137">Migrazione da Microsoft.AspNetCore.All a Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="0af17-137">Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App</span></span>

<span data-ttu-id="0af17-138">I pacchetti seguenti sono inclusi in `Microsoft.AspNetCore.All` ma non il pacchetto `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="0af17-138">The following packages are included in `Microsoft.AspNetCore.All` but not the `Microsoft.AspNetCore.App` package.</span></span>

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

<span data-ttu-id="0af17-139">Per passare da `Microsoft.AspNetCore.All` a `Microsoft.AspNetCore.App`, se l'app usa qualsiasi API dai pacchetti sopra o pacchetti inseriti da tali pacchetti, aggiungere i riferimenti a tali pacchetti nel progetto.</span><span class="sxs-lookup"><span data-stu-id="0af17-139">To move from `Microsoft.AspNetCore.All` to `Microsoft.AspNetCore.App`, if your app uses any APIs from the above packages, or packages brought in by those packages, add references to those packages in your project.</span></span>

<span data-ttu-id="0af17-140">Tutte le dipendenze dei pacchetti precedenti che non sono in altro modo dipendenze di `Microsoft.AspNetCore.App` non sono incluse in modo implicito.</span><span class="sxs-lookup"><span data-stu-id="0af17-140">Any dependencies of the preceding packages that otherwise aren't dependencies of `Microsoft.AspNetCore.App` are not included implicitly.</span></span> <span data-ttu-id="0af17-141">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="0af17-141">For example:</span></span>

* <span data-ttu-id="0af17-142">`StackExchange.Redis` come dipendenza di `Microsoft.Extensions.Caching.Redis`</span><span class="sxs-lookup"><span data-stu-id="0af17-142">`StackExchange.Redis` as a dependency of `Microsoft.Extensions.Caching.Redis`</span></span>
* <span data-ttu-id="0af17-143">`Microsoft.ApplicationInsights` come dipendenza di `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span><span class="sxs-lookup"><span data-stu-id="0af17-143">`Microsoft.ApplicationInsights` as a dependency of `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span></span>

## <a name="update-aspnet-core-21"></a><span data-ttu-id="0af17-144">Aggiornare ASP.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="0af17-144">Update ASP.NET Core 2.1</span></span>

<span data-ttu-id="0af17-145">È consigliabile eseguire la migrazione al metapacchetto `Microsoft.AspNetCore.App` per la versione 2.1 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="0af17-145">We recommend migrating to the `Microsoft.AspNetCore.App` metapackage for 2.1 and later.</span></span> <span data-ttu-id="0af17-146">Per continuare a usare il metapacchetto `Microsoft.AspNetCore.All` e assicurarsi che venga distribuita la versione della patch più recente:</span><span class="sxs-lookup"><span data-stu-id="0af17-146">To keep using the `Microsoft.AspNetCore.All` metapackage and ensure the latest patch version is deployed:</span></span>

* <span data-ttu-id="0af17-147">Nei computer di sviluppo e nei server di compilazione: installare la versione più recente di [.NET Core SDK](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="0af17-147">On development machines and build servers: Install the latest [.NET Core SDK](https://www.microsoft.com/net/download).</span></span>
* <span data-ttu-id="0af17-148">Nei server di distribuzione: installare la versione più recente del [runtime .NET Core](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="0af17-148">On deployment servers: Install the latest [.NET Core runtime](https://www.microsoft.com/net/download).</span></span>
 <span data-ttu-id="0af17-149">L'app eseguirà il roll forward all'ultima versione installata al riavvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="0af17-149">Your app will roll forward to the latest installed version on an application restart.</span></span>
