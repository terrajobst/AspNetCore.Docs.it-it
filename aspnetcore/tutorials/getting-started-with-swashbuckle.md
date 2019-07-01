---
title: Introduzione a Swashbuckle e ad ASP.NET Core
author: zuckerthoben
description: Informazioni su come aggiungere Swashbuckle al progetto dell'API Web ASP.NET Core per integrare l'interfaccia utente di Swagger.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/21/2019
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: 033f2dda9942f91f23158cf2d986062157a3b69e
ms.sourcegitcommit: 06a455d63ff7d6b571ca832e8117f4ac9d646baf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/21/2019
ms.locfileid: "67316601"
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="e14a3-103">Introduzione a Swashbuckle e ad ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e14a3-103">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="e14a3-104">Di [Shayne Boyer](https://twitter.com/spboyer) e [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="e14a3-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="e14a3-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e14a3-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e14a3-106">Esistono tre componenti principali di Swashbuckle:</span><span class="sxs-lookup"><span data-stu-id="e14a3-106">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="e14a3-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): un modello a oggetti Swagger e il middleware per esporre oggetti `SwaggerDocument` come endpoint JSON.</span><span class="sxs-lookup"><span data-stu-id="e14a3-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="e14a3-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): un generatore Swagger che compila oggetti `SwaggerDocument` direttamente da route, controller e modelli.</span><span class="sxs-lookup"><span data-stu-id="e14a3-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="e14a3-109">È in genere combinato con il middleware di endpoint Swagger per esporre automaticamente il JSON Swagger.</span><span class="sxs-lookup"><span data-stu-id="e14a3-109">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="e14a3-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): una versione incorporata dello strumento interfaccia utente di Swagger,</span><span class="sxs-lookup"><span data-stu-id="e14a3-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool.</span></span> <span data-ttu-id="e14a3-111">che interpreta il JSON di Swagger per descrivere in modo completo e personalizzabile la funzionalità dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="e14a3-111">It interprets Swagger JSON to build a rich, customizable experience for describing the web API functionality.</span></span> <span data-ttu-id="e14a3-112">Include test harness predefiniti per i metodi pubblici.</span><span class="sxs-lookup"><span data-stu-id="e14a3-112">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="e14a3-113">Installazione del pacchetto</span><span class="sxs-lookup"><span data-stu-id="e14a3-113">Package installation</span></span>

<span data-ttu-id="e14a3-114">È possibile aggiungere Swashbuckle con gli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="e14a3-114">Swashbuckle can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e14a3-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e14a3-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e14a3-116">Dalla finestra **Console di Gestione pacchetti**:</span><span class="sxs-lookup"><span data-stu-id="e14a3-116">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="e14a3-117">Passare a **Vista** > **Altre finestre** > **Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="e14a3-117">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="e14a3-118">Passare alla directory che contiene il file *TodoApi.csproj*</span><span class="sxs-lookup"><span data-stu-id="e14a3-118">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="e14a3-119">Eseguire il seguente comando:</span><span class="sxs-lookup"><span data-stu-id="e14a3-119">Execute the following command:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore -Version 5.0.0-rc2
    ```

* <span data-ttu-id="e14a3-120">Dalla finestra di dialogo **Gestisci pacchetti NuGet**:</span><span class="sxs-lookup"><span data-stu-id="e14a3-120">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="e14a3-121">Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** > **Gestisci pacchetti NuGet**</span><span class="sxs-lookup"><span data-stu-id="e14a3-121">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="e14a3-122">Impostare **Origine pacchetto** su "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="e14a3-122">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="e14a3-123">Immettere "Swashbuckle.AspNetCore" nella casella di ricerca</span><span class="sxs-lookup"><span data-stu-id="e14a3-123">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="e14a3-124">Selezionare il pacchetto "Swashbuckle.AspNetCore" dalla scheda **Sfoglia** e fare clic su **Installa**</span><span class="sxs-lookup"><span data-stu-id="e14a3-124">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e14a3-125">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e14a3-125">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e14a3-126">Fare clic su con il pulsante destro del mouse sulla cartella *Pacchetti* in **Solution Pad** (Riquadro della soluzione) > **Aggiungi pacchetti**</span><span class="sxs-lookup"><span data-stu-id="e14a3-126">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="e14a3-127">Impostare l'elenco a discesa **Aggiungi pacchetti** della finestra **Origine** su "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="e14a3-127">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="e14a3-128">Immettere "Swashbuckle.AspNetCore" nella casella di ricerca</span><span class="sxs-lookup"><span data-stu-id="e14a3-128">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
* <span data-ttu-id="e14a3-129">Selezionare il pacchetto "Swashbuckle.AspNetCore" dal riquadro dei risultati e fare clic su **Aggiungi pacchetto**</span><span class="sxs-lookup"><span data-stu-id="e14a3-129">Select the "Swashbuckle.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e14a3-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e14a3-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e14a3-131">Eseguire il comando seguente da **Terminale integrato**:</span><span class="sxs-lookup"><span data-stu-id="e14a3-131">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore -v 5.0.0-rc2
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e14a3-132">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="e14a3-132">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e14a3-133">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e14a3-133">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore -v 5.0.0-rc2
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="e14a3-134">Aggiungere e configurare il middleware di Swagger</span><span class="sxs-lookup"><span data-stu-id="e14a3-134">Add and configure Swagger middleware</span></span>

<span data-ttu-id="e14a3-135">Nella classe `Startup` importare gli spazi dei nomi seguenti per usare la classe `OpenApiInfo`:</span><span class="sxs-lookup"><span data-stu-id="e14a3-135">In the `Startup` class, import the following namespace to use the `OpenApiInfo` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_InfoClassNamespace)]

<span data-ttu-id="e14a3-136">Aggiungere il generatore di Swagger alla raccolta di servizi nel metodo `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e14a3-136">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=9-12)]

::: moniker-end

<span data-ttu-id="e14a3-137">Nel metodo `Startup.Configure` abilitare il middleware per la gestione del documento JSON generato e l'interfaccia utente di Swagger:</span><span class="sxs-lookup"><span data-stu-id="e14a3-137">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,8-11)]

<span data-ttu-id="e14a3-138">La chiamata del metodo `UseSwaggerUI` precedente abilita il [middleware dei file statici](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="e14a3-138">The preceding `UseSwaggerUI` method call enables the [Static File Middleware](xref:fundamentals/static-files).</span></span> <span data-ttu-id="e14a3-139">Se si usa .NET Framework o .NET Core 1.x, aggiungere il pacchetto NuGet [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) al progetto.</span><span class="sxs-lookup"><span data-stu-id="e14a3-139">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) NuGet package to the project.</span></span>

<span data-ttu-id="e14a3-140">Avviare l'app e passare a `http://localhost:<port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="e14a3-140">Launch the app, and navigate to `http://localhost:<port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="e14a3-141">Il documento generato che descrive gli endpoint viene illustrato in [Specifica Swagger (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span><span class="sxs-lookup"><span data-stu-id="e14a3-141">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="e14a3-142">L'interfaccia utente di Swagger è disponibile in `http://localhost:<port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="e14a3-142">The Swagger UI can be found at `http://localhost:<port>/swagger`.</span></span> <span data-ttu-id="e14a3-143">Esplorare l'API tramite l'interfaccia utente di Swagger e incorporarla in altri programmi.</span><span class="sxs-lookup"><span data-stu-id="e14a3-143">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="e14a3-144">Per gestire l'interfaccia utente di Swagger nella radice dell'app (`http://localhost:<port>/`), impostare la proprietà `RoutePrefix` su una stringa vuota:</span><span class="sxs-lookup"><span data-stu-id="e14a3-144">To serve the Swagger UI at the app's root (`http://localhost:<port>/`), set the `RoutePrefix` property to an empty string:</span></span>
>
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

<span data-ttu-id="e14a3-145">Se si usano le directory con ISS o il proxy inverso, impostare l'endpoint Swagger su un percorso relativo tramite il prefisso `./`.</span><span class="sxs-lookup"><span data-stu-id="e14a3-145">If using directories with IIS or a reverse proxy, set the Swagger endpoint to a relative path using the `./` prefix.</span></span> <span data-ttu-id="e14a3-146">Ad esempio `./swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="e14a3-146">For example, `./swagger/v1/swagger.json`.</span></span> <span data-ttu-id="e14a3-147">L'uso di `/swagger/v1/swagger.json` indica all'app di cercare il file JSON nella vera radice dell'URL (con il prefisso della route, se usato).</span><span class="sxs-lookup"><span data-stu-id="e14a3-147">Using `/swagger/v1/swagger.json` instructs the app to look for the JSON file at the true root of the URL (plus the route prefix, if used).</span></span> <span data-ttu-id="e14a3-148">Ad esempio, usare `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` invece di `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="e14a3-148">For example, use `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` instead of `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`.</span></span>

## <a name="customize-and-extend"></a><span data-ttu-id="e14a3-149">Personalizzare ed estendere</span><span class="sxs-lookup"><span data-stu-id="e14a3-149">Customize and extend</span></span>

<span data-ttu-id="e14a3-150">Swagger offre opzioni per documentare il modello a oggetti e personalizzare l'interfaccia utente in modo che corrisponda al tema.</span><span class="sxs-lookup"><span data-stu-id="e14a3-150">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="e14a3-151">Informazioni e descrizione API</span><span class="sxs-lookup"><span data-stu-id="e14a3-151">API info and description</span></span>

<span data-ttu-id="e14a3-152">L'azione di configurazione passata al metodo `AddSwaggerGen` aggiunge informazioni come ad esempio autore, licenza e descrizione:</span><span class="sxs-lookup"><span data-stu-id="e14a3-152">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup4.cs?name=snippet_AddSwaggerGen)]

<span data-ttu-id="e14a3-153">L'interfaccia utente di Swagger visualizza le informazioni sulla versione:</span><span class="sxs-lookup"><span data-stu-id="e14a3-153">The Swagger UI displays the version's information:</span></span>

![Interfaccia utente di Swagger con informazioni sulla versione: descrizione, autore e altri collegamenti](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="e14a3-155">XML (commenti)</span><span class="sxs-lookup"><span data-stu-id="e14a3-155">XML comments</span></span>

<span data-ttu-id="e14a3-156">I commenti XML possono essere abilitati con gli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="e14a3-156">XML comments can be enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e14a3-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e14a3-157">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="e14a3-158">Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Modifica <nome_progetto>.csproj**.</span><span class="sxs-lookup"><span data-stu-id="e14a3-158">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="e14a3-159">Aggiungere manualmente le righe evidenziate al file con estensione *csproj*:</span><span class="sxs-lookup"><span data-stu-id="e14a3-159">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="e14a3-160">Fare clic con il pulsante destro del mouse in **Esplora soluzioni** e scegliere **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="e14a3-160">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
* <span data-ttu-id="e14a3-161">Selezionare la casella di controllo **File di documentazione XML** nella sezione **Output** della scheda **Compilazione**.</span><span class="sxs-lookup"><span data-stu-id="e14a3-161">Check the **XML documentation file** box under the **Output** section of the **Build** tab.</span></span>

::: moniker-end

#### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e14a3-162">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e14a3-162">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="e14a3-163">Dal *riquadro della soluzione* premere **controllo** e fare clic sul nome del progetto.</span><span class="sxs-lookup"><span data-stu-id="e14a3-163">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="e14a3-164">Passare a **Strumenti** > **Modifica file**.</span><span class="sxs-lookup"><span data-stu-id="e14a3-164">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="e14a3-165">Aggiungere manualmente le righe evidenziate al file con estensione *csproj*:</span><span class="sxs-lookup"><span data-stu-id="e14a3-165">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="e14a3-166">Aprire la finestra di dialogo **Opzioni progetto** > **Compila** > **Compilatore**.</span><span class="sxs-lookup"><span data-stu-id="e14a3-166">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="e14a3-167">Selezionare la casella di controllo **Genera la documentazione XML** nella sezione **Opzioni generali**</span><span class="sxs-lookup"><span data-stu-id="e14a3-167">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

#### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e14a3-168">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e14a3-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e14a3-169">Aggiungere manualmente le righe evidenziate al file con estensione *csproj*:</span><span class="sxs-lookup"><span data-stu-id="e14a3-169">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

#### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e14a3-170">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="e14a3-170">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e14a3-171">Aggiungere manualmente le righe evidenziate al file con estensione *csproj*:</span><span class="sxs-lookup"><span data-stu-id="e14a3-171">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

---

<span data-ttu-id="e14a3-172">Abilitando i commenti XML, viene eseguito il debug delle informazioni per membri e tipi pubblici non documentati.</span><span class="sxs-lookup"><span data-stu-id="e14a3-172">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="e14a3-173">I membri e tipi non documentati sono indicati nel messaggio di avviso.</span><span class="sxs-lookup"><span data-stu-id="e14a3-173">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="e14a3-174">Ad esempio, il messaggio seguente indica una violazione del codice di avviso 1591:</span><span class="sxs-lookup"><span data-stu-id="e14a3-174">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="e14a3-175">Per eliminare gli avvisi per l'intero progetto, definire un elenco delimitato da punto e virgola di codici di avviso da ignorare nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="e14a3-175">To suppress warnings project-wide, define a semicolon-delimited list of warning codes to ignore in the project file.</span></span> <span data-ttu-id="e14a3-176">L'aggiunta di codici di avviso a `$(NoWarn);` applica anche i [valori predefiniti di C#](https://github.com/dotnet/sdk/blob/2eb6c546931b5bcb92cd3128b93932a980553ea1/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.Sdk.CSharp.props#L16).</span><span class="sxs-lookup"><span data-stu-id="e14a3-176">Appending the warning codes to `$(NoWarn);` applies the [C# default values](https://github.com/dotnet/sdk/blob/2eb6c546931b5bcb92cd3128b93932a980553ea1/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.Sdk.CSharp.props#L16) too.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

<span data-ttu-id="e14a3-177">Per eliminare gli avvisi solo per membri specifici, racchiudere il codice in direttive del preprocessore [#pragma warning](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning).</span><span class="sxs-lookup"><span data-stu-id="e14a3-177">To suppress warnings only for specific members, enclose the code in [#pragma warning](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning) preprocessor directives.</span></span> <span data-ttu-id="e14a3-178">Questo approccio è utile per il codice che non deve essere esposto tramite la documentazione delle API. Nell'esempio seguente, il codice di avviso CS1591 viene ignorato per l'intera classe `Program`.</span><span class="sxs-lookup"><span data-stu-id="e14a3-178">This approach is useful for code that shouldn't be exposed via the API docs. In the following example, warning code CS1591 is ignored for the entire `Program` class.</span></span> <span data-ttu-id="e14a3-179">L'imposizione del codice di avviso viene ripristinata alla chiusura della definizione della classe.</span><span class="sxs-lookup"><span data-stu-id="e14a3-179">Enforcement of the warning code is restored at the close of the class definition.</span></span> <span data-ttu-id="e14a3-180">Specificare più codici di avviso con un elenco delimitato da virgole.</span><span class="sxs-lookup"><span data-stu-id="e14a3-180">Specify multiple warning codes with a comma-delimited list.</span></span>

```csharp
namespace TodoApi
{
#pragma warning disable CS1591
    public class Program
    {
        public static void Main(string[] args) =>
            BuildWebHost(args).Run();

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
    }
#pragma warning restore CS1591
}
```

<span data-ttu-id="e14a3-181">Configurare Swagger per usare il file XML che viene generato con le istruzioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="e14a3-181">Configure Swagger to use the XML file that's generated with the preceding instructions.</span></span> <span data-ttu-id="e14a3-182">Per Linux o sistemi operativi diversi da Windows, i percorsi e i nomi di file possono fare distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="e14a3-182">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="e14a3-183">Ad esempio, un file *ToDoApi.XML* è valido in Windows, ma non in CentOS.</span><span class="sxs-lookup"><span data-stu-id="e14a3-183">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=31-33)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup1x.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

<span data-ttu-id="e14a3-184">Nel codice precedente la [reflection](/dotnet/csharp/programming-guide/concepts/reflection) consente di compilare un nome file XML che corrisponde a quello del progetto API Web.</span><span class="sxs-lookup"><span data-stu-id="e14a3-184">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the web API project.</span></span> <span data-ttu-id="e14a3-185">La proprietà [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory*) viene usata per costruire un percorso del file XML.</span><span class="sxs-lookup"><span data-stu-id="e14a3-185">The [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory*) property is used to construct a path to the XML file.</span></span> <span data-ttu-id="e14a3-186">Alcune funzionalità di Swagger (ad esempio, schemi di parametri di input o i metodi HTTP e i codici di risposta dai rispettivi attributi) funzionano senza l'uso di un file di documentazione XML.</span><span class="sxs-lookup"><span data-stu-id="e14a3-186">Some Swagger features (for example, schemata of input parameters or HTTP methods and response codes from the respective attributes) work without the use of an XML documentation file.</span></span> <span data-ttu-id="e14a3-187">Per la maggior parte delle funzionalità, vale a dire i riepiloghi dei metodi e le descrizioni dei parametri e dei codici di risposta, l'uso di un file XML è obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="e14a3-187">For most features, namely method summaries and the descriptions of parameters and response codes, the use of an XML file is mandatory.</span></span>

<span data-ttu-id="e14a3-188">L'aggiunta a un'azione di commenti con tripla barra migliora l'interfaccia utente di Swagger poiché viene aggiunta la descrizione all'intestazione della sezione.</span><span class="sxs-lookup"><span data-stu-id="e14a3-188">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="e14a3-189">Aggiungere un elemento [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) sopra l'azione `Delete`:</span><span class="sxs-lookup"><span data-stu-id="e14a3-189">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

<span data-ttu-id="e14a3-190">L'interfaccia utente di Swagger visualizza il testo interno dell'elemento `<summary>` del codice precedente:</span><span class="sxs-lookup"><span data-stu-id="e14a3-190">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![Interfaccia utente di Swagger che visualizza il commento XML "Deletes a specific TodoItem."](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="e14a3-193">L'interfaccia utente è determinata dallo schema JSON generato:</span><span class="sxs-lookup"><span data-stu-id="e14a3-193">The UI is driven by the generated JSON schema:</span></span>

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

<span data-ttu-id="e14a3-194">Aggiungere un elemento [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) alla documentazione del metodo di azione `Create`.</span><span class="sxs-lookup"><span data-stu-id="e14a3-194">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="e14a3-195">In questo modo vengono integrate le informazioni specificate nell'elemento `<summary>` e l'interfaccia utente di Swagger risulta più affidabile.</span><span class="sxs-lookup"><span data-stu-id="e14a3-195">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="e14a3-196">Il contenuto dell'elemento `<remarks>` può essere costituito da testo, JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="e14a3-196">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

<span data-ttu-id="e14a3-197">Si notino i miglioramenti dell'interfaccia utente con questi commenti aggiuntivi:</span><span class="sxs-lookup"><span data-stu-id="e14a3-197">Notice the UI enhancements with these additional comments:</span></span>

![Interfaccia utente di Swagger con commenti aggiuntivi visualizzati](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="e14a3-199">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="e14a3-199">Data annotations</span></span>

<span data-ttu-id="e14a3-200">Decorare il modello con gli attributi inclusi nello spazio dei nomi [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) per gestire i componenti dell'interfaccia utente di Swagger.</span><span class="sxs-lookup"><span data-stu-id="e14a3-200">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="e14a3-201">Aggiungere l'attributo `[Required]` alla proprietà `Name` della classe `TodoItem`:</span><span class="sxs-lookup"><span data-stu-id="e14a3-201">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="e14a3-202">La presenza di questo attributo modifica il comportamento dell'interfaccia utente e lo schema JSON sottostante:</span><span class="sxs-lookup"><span data-stu-id="e14a3-202">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

<span data-ttu-id="e14a3-203">Aggiungere l'attributo `[Produces("application/json")]` al controller API.</span><span class="sxs-lookup"><span data-stu-id="e14a3-203">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="e14a3-204">Il suo scopo consiste nel dichiarare che le azioni del controller supportano un tipo di contenuto della risposta di *application/json*:</span><span class="sxs-lookup"><span data-stu-id="e14a3-204">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

<span data-ttu-id="e14a3-205">L'elenco a discesa **Response Content Type** (Tipo di contenuto risposta) include questo tipo di contenuto come selezione predefinita per le azioni GET del controller:</span><span class="sxs-lookup"><span data-stu-id="e14a3-205">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Interfaccia utente di Swagger con tipo di contenuto di risposta predefinito](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="e14a3-207">Con l'aumento dell'uso delle annotazioni dei dati nell'API Web, le pagine della Guida dell'API e dell'interfaccia utente diventano più descrittive e utili.</span><span class="sxs-lookup"><span data-stu-id="e14a3-207">As the usage of data annotations in the web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="e14a3-208">Descrivere i tipi di risposta</span><span class="sxs-lookup"><span data-stu-id="e14a3-208">Describe response types</span></span>

<span data-ttu-id="e14a3-209">La principale preoccupazione degli sviluppatori che utilizzano un'API Web è ciò che viene restituito, in particolare i tipi di risposta e i codici di errore, se diversi da quelli standard.</span><span class="sxs-lookup"><span data-stu-id="e14a3-209">Developers consuming a web API are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="e14a3-210">I tipi di risposta e i codici di errore vengono indicati nei commenti XML e nelle annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="e14a3-210">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="e14a3-211">L'azione `Create` restituisce un codice di stato HTTP 201 in caso di esito positivo.</span><span class="sxs-lookup"><span data-stu-id="e14a3-211">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="e14a3-212">Quando il corpo della richiesta inviata è Null, viene restituito un codice di stato HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="e14a3-212">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="e14a3-213">Senza la documentazione appropriata nell'interfaccia utente di Swagger, il consumer non riconosce tali risultati previsti.</span><span class="sxs-lookup"><span data-stu-id="e14a3-213">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="e14a3-214">Risolvere il problema aggiungendo le righe evidenziate nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="e14a3-214">Fix that problem by adding the highlighted lines in the following example:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

<span data-ttu-id="e14a3-215">L'interfaccia utente di Swagger ora documenta chiaramente i codici di risposta HTTP previsti:</span><span class="sxs-lookup"><span data-stu-id="e14a3-215">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Interfaccia utente di Swagger che visualizza la descrizione della classe risposta POST "Returns the newly created Todo item" e "400 - If the item is null" per il codice di stato e il motivo nei messaggi di risposta](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="e14a3-217">In ASP.NET Core 2.2 o versioni successive, possono essere usate convenzioni in alternativa alla decorazione esplicita di azioni singole con `[ProducesResponseType]`.</span><span class="sxs-lookup"><span data-stu-id="e14a3-217">In ASP.NET Core 2.2 or later, conventions can be used as an alternative to explicitly decorating individual actions with `[ProducesResponseType]`.</span></span> <span data-ttu-id="e14a3-218">Per altre informazioni, vedere <xref:web-api/advanced/conventions>.</span><span class="sxs-lookup"><span data-stu-id="e14a3-218">For more information, see <xref:web-api/advanced/conventions>.</span></span>

::: moniker-end

### <a name="customize-the-ui"></a><span data-ttu-id="e14a3-219">Personalizzare l'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="e14a3-219">Customize the UI</span></span>

<span data-ttu-id="e14a3-220">L'interfaccia utente è funzionale e presentabile,</span><span class="sxs-lookup"><span data-stu-id="e14a3-220">The stock UI is both functional and presentable.</span></span> <span data-ttu-id="e14a3-221">ma è necessario che le pagine della documentazione API rappresentino il proprio marchio o tema.</span><span class="sxs-lookup"><span data-stu-id="e14a3-221">However, API documentation pages should represent your brand or theme.</span></span> <span data-ttu-id="e14a3-222">Per personalizzare i componenti Swashbuckle è necessario aggiungere le risorse per gestire i file statici e quindi compilare la struttura di cartelle per ospitare i file.</span><span class="sxs-lookup"><span data-stu-id="e14a3-222">Branding the Swashbuckle components requires adding the resources to serve static files and building the folder structure to host those files.</span></span>

<span data-ttu-id="e14a3-223">Se si usa .NET Framework o .NET Core 1.x, aggiungere il pacchetto NuGet [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) al progetto:</span><span class="sxs-lookup"><span data-stu-id="e14a3-223">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="e14a3-224">Il pacchetto NuGet precedente è già installato se si usano .NET Core 2.x e il [metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="e14a3-224">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="e14a3-225">Abilitare il middleware dei file statici:</span><span class="sxs-lookup"><span data-stu-id="e14a3-225">Enable Static File Middleware:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="e14a3-226">Acquisire il contenuto della cartella *dist* dall'[archivio dell'interfaccia utente di Swagger di GitHub](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span><span class="sxs-lookup"><span data-stu-id="e14a3-226">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span></span> <span data-ttu-id="e14a3-227">La cartella contiene le risorse necessarie per la pagina dell'interfaccia utente di Swagger.</span><span class="sxs-lookup"><span data-stu-id="e14a3-227">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="e14a3-228">Creare una cartella *wwwroot/swagger/ui* e copiarvi il contenuto della cartella *dist*.</span><span class="sxs-lookup"><span data-stu-id="e14a3-228">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="e14a3-229">Creare un file *custom.css* in *wwwroot/swagger/ui*, con il CSS riportato di seguito per personalizzare l'intestazione della pagina:</span><span class="sxs-lookup"><span data-stu-id="e14a3-229">Create a *custom.css* file, in *wwwroot/swagger/ui*, with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

<span data-ttu-id="e14a3-230">Creare il riferimento a *custom.css* nel file *index.html* dopo tutti gli altri file CSS:</span><span class="sxs-lookup"><span data-stu-id="e14a3-230">Reference *custom.css* in the *index.html* file, after any other CSS files:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

<span data-ttu-id="e14a3-231">Passare alla pagina *index.html* all'indirizzo `http://localhost:<port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="e14a3-231">Browse to the *index.html* page at `http://localhost:<port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="e14a3-232">Immettere `http://localhost:<port>/swagger/v1/swagger.json` nella casella di testo dell'intestazione e scegliere il pulsante **Explore** (Esplora).</span><span class="sxs-lookup"><span data-stu-id="e14a3-232">Enter `http://localhost:<port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="e14a3-233">La pagina risultante è simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="e14a3-233">The resulting page looks as follows:</span></span>

![Interfaccia utente di Swagger con il titolo dell'intestazione personalizzato](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="e14a3-235">Si può fare molto altro con la pagina.</span><span class="sxs-lookup"><span data-stu-id="e14a3-235">There's much more you can do with the page.</span></span> <span data-ttu-id="e14a3-236">Per vedere le funzionalità complete per le risorse dell'interfaccia utente, accedere all'[archivio dell'interfaccia utente di Swagger di GitHub](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="e14a3-236">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
