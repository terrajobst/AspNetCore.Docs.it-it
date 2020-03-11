---
title: Ospitare e distribuire ASP.NET Core Blazor
author: guardrex
description: Scopri come ospitare e distribuire app Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 238e7fc8f8d64c7847dc8847fb66e22442a3c8e0
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667153"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a><span data-ttu-id="9282f-103">Hosting e distribuzione di ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="9282f-103">Host and deploy ASP.NET Core Blazor</span></span>

<span data-ttu-id="9282f-104">Di [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="9282f-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="publish-the-app"></a><span data-ttu-id="9282f-105">Pubblicare l'app</span><span class="sxs-lookup"><span data-stu-id="9282f-105">Publish the app</span></span>

<span data-ttu-id="9282f-106">Le app vengono pubblicate per la distribuzione nella configurazione per il rilascio.</span><span class="sxs-lookup"><span data-stu-id="9282f-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="9282f-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9282f-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="9282f-108">Selezionare **Compila** > **Pubblica {APPLICAZIONE}** sulla barra di spostamento.</span><span class="sxs-lookup"><span data-stu-id="9282f-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="9282f-109">Selezionare la *destinazione di pubblicazione*.</span><span class="sxs-lookup"><span data-stu-id="9282f-109">Select the *publish target*.</span></span> <span data-ttu-id="9282f-110">Per pubblicare in locale, selezionare **Cartella**.</span><span class="sxs-lookup"><span data-stu-id="9282f-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="9282f-111">Accettare il percorso predefinito nel campo **Scegliere una cartella** oppure specificarne uno diverso.</span><span class="sxs-lookup"><span data-stu-id="9282f-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="9282f-112">Fare clic sul pulsante **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="9282f-112">Select the **Publish** button.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="9282f-113">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="9282f-113">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="9282f-114">Usare il comando [dotnet publish](/dotnet/core/tools/dotnet-publish) per pubblicare l'app con una configurazione per il rilascio:</span><span class="sxs-lookup"><span data-stu-id="9282f-114">Use the [dotnet publish](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```dotnetcli
dotnet publish -c Release
```

---

<span data-ttu-id="9282f-115">La pubblicazione dell'app attiva un [ripristino](/dotnet/core/tools/dotnet-restore) delle dipendenze del progetto e [compila](/dotnet/core/tools/dotnet-build) il progetto prima di creare gli asset per la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="9282f-115">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="9282f-116">Come parte del processo di compilazione, vengono rimossi gli assembly e i metodi non usati per ridurre le dimensioni del download e i tempi di caricamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="9282f-116">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="9282f-117">Una Blazor app webassembly viene pubblicata nella cartella */bin/Release/{Target Framework}/Publish/{assembly nome}/dist* .</span><span class="sxs-lookup"><span data-stu-id="9282f-117">A Blazor WebAssembly app is published to the */bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span> <span data-ttu-id="9282f-118">Una Blazor app Server viene pubblicata nella cartella */bin/Release/{Target Framework}/Publish*</span><span class="sxs-lookup"><span data-stu-id="9282f-118">A Blazor Server app is published to the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="9282f-119">Gli asset nella cartella vengono distribuiti nel server Web.</span><span class="sxs-lookup"><span data-stu-id="9282f-119">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="9282f-120">La distribuzione potrebbe essere un processo manuale o automatizzato a seconda degli strumenti di sviluppo in uso.</span><span class="sxs-lookup"><span data-stu-id="9282f-120">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="app-base-path"></a><span data-ttu-id="9282f-121">Percorso di base dell'app</span><span class="sxs-lookup"><span data-stu-id="9282f-121">App base path</span></span>

<span data-ttu-id="9282f-122">Il *percorso di base dell'app* è il percorso dell'URL radice dell'app.</span><span class="sxs-lookup"><span data-stu-id="9282f-122">The *app base path* is the app's root URL path.</span></span> <span data-ttu-id="9282f-123">Prendere in considerazione le seguenti app ASP.NET Core e Blazor Sub-App:</span><span class="sxs-lookup"><span data-stu-id="9282f-123">Consider the following ASP.NET Core app and Blazor sub-app:</span></span>

* <span data-ttu-id="9282f-124">L'app ASP.NET Core è denominata `MyApp`:</span><span class="sxs-lookup"><span data-stu-id="9282f-124">The ASP.NET Core app is named `MyApp`:</span></span>
  * <span data-ttu-id="9282f-125">L'app risiede fisicamente in *d:/MyApp*.</span><span class="sxs-lookup"><span data-stu-id="9282f-125">The app physically resides at *d:/MyApp*.</span></span>
  * <span data-ttu-id="9282f-126">Le richieste vengono ricevute in `https://www.contoso.com/{MYAPP RESOURCE}`.</span><span class="sxs-lookup"><span data-stu-id="9282f-126">Requests are received at `https://www.contoso.com/{MYAPP RESOURCE}`.</span></span>
* <span data-ttu-id="9282f-127">Un'app Blazor denominata `CoolApp` è una sottoapp di `MyApp`:</span><span class="sxs-lookup"><span data-stu-id="9282f-127">A Blazor app named `CoolApp` is a sub-app of `MyApp`:</span></span>
  * <span data-ttu-id="9282f-128">La Sub-App risiede fisicamente in *d:/MyApp/CoolApp*.</span><span class="sxs-lookup"><span data-stu-id="9282f-128">The sub-app physically resides at *d:/MyApp/CoolApp*.</span></span>
  * <span data-ttu-id="9282f-129">Le richieste vengono ricevute in `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.</span><span class="sxs-lookup"><span data-stu-id="9282f-129">Requests are received at `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.</span></span>

<span data-ttu-id="9282f-130">Se non si specifica alcuna configurazione aggiuntiva per `CoolApp`, la sottoapp in questo scenario non è in alcun luogo in cui si trova nel server.</span><span class="sxs-lookup"><span data-stu-id="9282f-130">Without specifying additional configuration for `CoolApp`, the sub-app in this scenario has no knowledge of where it resides on the server.</span></span> <span data-ttu-id="9282f-131">Ad esempio, l'app non può costruire URL relativi corretti per le risorse senza sapere che risiede nel percorso URL relativo `/CoolApp/`.</span><span class="sxs-lookup"><span data-stu-id="9282f-131">For example, the app can't construct correct relative URLs to its resources without knowing that it resides at the relative URL path `/CoolApp/`.</span></span>

<span data-ttu-id="9282f-132">Per specificare la configurazione per il percorso di base dell'app Blazor di `https://www.contoso.com/CoolApp/`, l'attributo `href` del tag `<base>` è impostato sul percorso radice relativo nel file *pages/_Host. cshtml* (Blazor Server) o *wwwroot/index.html* (Blazor webassembly):</span><span class="sxs-lookup"><span data-stu-id="9282f-132">To provide configuration for the Blazor app's base path of `https://www.contoso.com/CoolApp/`, the `<base>` tag's `href` attribute is set to the relative root path in the *Pages/_Host.cshtml* file (Blazor Server) or *wwwroot/index.html* file (Blazor WebAssembly):</span></span>

```html
<base href="/CoolApp/">
```

Blazor<span data-ttu-id="9282f-133"> app Server imposta anche il percorso di base lato server chiamando <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*> nella pipeline delle richieste dell'app di `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="9282f-133"> Server apps additionally set the server-side base path by calling <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*> in the app's request pipeline of `Startup.Configure`:</span></span>

```csharp
app.UsePathBase("/CoolApp");
```

<span data-ttu-id="9282f-134">Fornendo il percorso URL relativo, un componente che non si trova nella directory radice può costruire URL relativi al percorso radice dell'app.</span><span class="sxs-lookup"><span data-stu-id="9282f-134">By providing the relative URL path, a component that isn't in the root directory can construct URLs relative to the app's root path.</span></span> <span data-ttu-id="9282f-135">I componenti a livelli diversi della struttura di directory possono creare collegamenti ad altre risorse in posizioni all'interno dell'app.</span><span class="sxs-lookup"><span data-stu-id="9282f-135">Components at different levels of the directory structure can build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="9282f-136">Il percorso di base dell'app viene usato anche per intercettare i collegamenti ipertestuali selezionati in cui la destinazione `href` del collegamento si trova nello spazio URI del percorso di base dell'app.</span><span class="sxs-lookup"><span data-stu-id="9282f-136">The app base path is also used to intercept selected hyperlinks where the `href` target of the link is within the app base path URI space.</span></span> <span data-ttu-id="9282f-137">Il router di Blazor gestisce la navigazione interna.</span><span class="sxs-lookup"><span data-stu-id="9282f-137">The Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="9282f-138">In molti scenari di hosting il percorso URL relativo dell'app è la radice dell'app.</span><span class="sxs-lookup"><span data-stu-id="9282f-138">In many hosting scenarios, the relative URL path to the app is the root of the app.</span></span> <span data-ttu-id="9282f-139">In questi casi, il percorso di base dell'URL relativo dell'app è una barra (`<base href="/" />`), che è la configurazione predefinita per un'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="9282f-139">In these cases, the app's relative URL base path is a forward slash (`<base href="/" />`), which is the default configuration for a Blazor app.</span></span> <span data-ttu-id="9282f-140">In altri scenari di hosting, ad esempio pagine di GitHub e sottoapp di IIS, il percorso di base dell'app deve essere impostato sul percorso URL relativo del server dell'app.</span><span class="sxs-lookup"><span data-stu-id="9282f-140">In other hosting scenarios, such as GitHub Pages and IIS sub-apps, the app base path must be set to the server's relative URL path of the app.</span></span>

<span data-ttu-id="9282f-141">Per impostare il percorso di base dell'app, aggiornare il tag `<base>` all'interno degli elementi tag `<head>` del file *pages/_Host. cshtml* (Blazor Server) o *wwwroot/index.html* (Blazor webassembly).</span><span class="sxs-lookup"><span data-stu-id="9282f-141">To set the app's base path, update the `<base>` tag within the `<head>` tag elements of the *Pages/_Host.cshtml* file (Blazor Server) or *wwwroot/index.html* file (Blazor WebAssembly).</span></span> <span data-ttu-id="9282f-142">Impostare il valore dell'attributo `href` su `/{RELATIVE URL PATH}/` (la barra finale è obbligatoria), dove `{RELATIVE URL PATH}` è il percorso URL relativo completo dell'app.</span><span class="sxs-lookup"><span data-stu-id="9282f-142">Set the `href` attribute value to `/{RELATIVE URL PATH}/` (the trailing slash is required), where `{RELATIVE URL PATH}` is the app's full relative URL path.</span></span>

<span data-ttu-id="9282f-143">Per un'app webassembly Blazor con un percorso URL relativo non radice, ad esempio `<base href="/CoolApp/">`, l'app non riesce a trovare le proprie risorse *quando viene eseguita localmente*.</span><span class="sxs-lookup"><span data-stu-id="9282f-143">For an Blazor WebAssembly app with a non-root relative URL path (for example, `<base href="/CoolApp/">`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="9282f-144">Per risolvere questo problema durante sviluppo e test locali, è possibile specificare un argomento *base del percorso* corrispondente al valore `href` del tag `<base>` in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="9282f-144">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span> <span data-ttu-id="9282f-145">Non includere una barra finale.</span><span class="sxs-lookup"><span data-stu-id="9282f-145">Don't include a trailing slash.</span></span> <span data-ttu-id="9282f-146">Per passare l'argomento di base del percorso quando si esegue l'app in locale, eseguire il comando `dotnet run` dalla directory dell'app con l'opzione `--pathbase`:</span><span class="sxs-lookup"><span data-stu-id="9282f-146">To pass the path base argument when running the app locally, execute the `dotnet run` command from the app's directory with the `--pathbase` option:</span></span>

```dotnetcli
dotnet run --pathbase=/{RELATIVE URL PATH (no trailing slash)}
```

<span data-ttu-id="9282f-147">Per un'app webassembly Blazor con un percorso URL relativo `/CoolApp/` (`<base href="/CoolApp/">`), il comando è:</span><span class="sxs-lookup"><span data-stu-id="9282f-147">For a Blazor WebAssembly app with a relative URL path of `/CoolApp/` (`<base href="/CoolApp/">`), the command is:</span></span>

```dotnetcli
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="9282f-148">L'app webassembly Blazor risponde localmente al `http://localhost:port/CoolApp`.</span><span class="sxs-lookup"><span data-stu-id="9282f-148">The Blazor WebAssembly app responds locally at `http://localhost:port/CoolApp`.</span></span>

## <a name="deployment"></a><span data-ttu-id="9282f-149">Distribuzione</span><span class="sxs-lookup"><span data-stu-id="9282f-149">Deployment</span></span>

<span data-ttu-id="9282f-150">Per indicazioni sulla distribuzione, vedere gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="9282f-150">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/webassembly>
* <xref:host-and-deploy/blazor/server>
