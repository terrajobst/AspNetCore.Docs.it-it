---
title: Introduzione a NSwag e ad ASP.NET Core
author: zuckerthoben
description: Informazioni su come usare NSwag per generare le pagine della documentazione e della Guida per un'API Web di ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/30/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 787f51629b044e638d2790b3dda8723c2644c2d8
ms.sourcegitcommit: a04eb20e81243930ec829a9db5dd5de49f669450
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/03/2019
ms.locfileid: "66470434"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="2761e-103">Introduzione a NSwag e ad ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2761e-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="2761e-104">Di [Christoph Nienaber](https://twitter.com/zuckerthoben), [Rico Suter](https://rsuter.com) e [Dave Brock](https://twitter.com/daveabrock)</span><span class="sxs-lookup"><span data-stu-id="2761e-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben), [Rico Suter](https://rsuter.com), and [Dave Brock](https://twitter.com/daveabrock)</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2761e-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2761e-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="2761e-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2761e-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="2761e-107">NSwag offre le funzionalità seguenti:</span><span class="sxs-lookup"><span data-stu-id="2761e-107">NSwag offers the following capabilities:</span></span>

* <span data-ttu-id="2761e-108">La possibilità di usare l'interfaccia utente di Swagger e un generatore Swagger.</span><span class="sxs-lookup"><span data-stu-id="2761e-108">The ability to utilize the Swagger UI and Swagger generator.</span></span>
* <span data-ttu-id="2761e-109">Funzionalità di generazione del codice flessibili.</span><span class="sxs-lookup"><span data-stu-id="2761e-109">Flexible code generation capabilities.</span></span>

<span data-ttu-id="2761e-110">Con NSwag non è necessaria un'API esistente. È possibile usare le API di terze parti che includono Swagger e generare un'implementazione client.</span><span class="sxs-lookup"><span data-stu-id="2761e-110">With NSwag, you don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and generate a client implementation.</span></span> <span data-ttu-id="2761e-111">NSwag consente di velocizzare il ciclo di sviluppo e adattarsi facilmente alle modifiche dell'API.</span><span class="sxs-lookup"><span data-stu-id="2761e-111">NSwag allows you to expedite the development cycle and easily adapt to API changes.</span></span>

## <a name="register-the-nswag-middleware"></a><span data-ttu-id="2761e-112">Registrare il middleware NSwag</span><span class="sxs-lookup"><span data-stu-id="2761e-112">Register the NSwag middleware</span></span>

<span data-ttu-id="2761e-113">Registrare il middleware NSwag per:</span><span class="sxs-lookup"><span data-stu-id="2761e-113">Register the NSwag middleware to:</span></span>

* <span data-ttu-id="2761e-114">Generare la specifica Swagger per l'API Web implementata.</span><span class="sxs-lookup"><span data-stu-id="2761e-114">Generate the Swagger specification for the implemented web API.</span></span>
* <span data-ttu-id="2761e-115">Usare l'interfaccia utente di Swagger per esplorare e testare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="2761e-115">Serve the Swagger UI to browse and test the web API.</span></span>

<span data-ttu-id="2761e-116">Per usare il middleware ASP.NET Core di [NSwag](https://github.com/RSuter/NSwag), installare il pacchetto NuGet [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/).</span><span class="sxs-lookup"><span data-stu-id="2761e-116">To use the [NSwag](https://github.com/RSuter/NSwag) ASP.NET Core middleware, install the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="2761e-117">Questo pacchetto contiene il middleware necessario per generare e usare la specifica Swagger, l'interfaccia utente di Swagger (v2 e v3) e l'[interfaccia utente di ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="2761e-117">This package contains the middleware to generate and serve the Swagger specification, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="2761e-118">Usare uno degli approcci seguenti per installare il pacchetto NuGet NSwag:</span><span class="sxs-lookup"><span data-stu-id="2761e-118">Use one of the following approaches to install the NSwag NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2761e-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2761e-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2761e-120">Dalla finestra **Console di Gestione pacchetti**:</span><span class="sxs-lookup"><span data-stu-id="2761e-120">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="2761e-121">Passare a **Vista** > **Altre finestre** > **Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="2761e-121">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="2761e-122">Passare alla directory che contiene il file *TodoApi.csproj*</span><span class="sxs-lookup"><span data-stu-id="2761e-122">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="2761e-123">Eseguire il seguente comando:</span><span class="sxs-lookup"><span data-stu-id="2761e-123">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="2761e-124">Dalla finestra di dialogo **Gestisci pacchetti NuGet**:</span><span class="sxs-lookup"><span data-stu-id="2761e-124">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="2761e-125">Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** > **Gestisci pacchetti NuGet**</span><span class="sxs-lookup"><span data-stu-id="2761e-125">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="2761e-126">Impostare **Origine pacchetto** su "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="2761e-126">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="2761e-127">Immettere "NSwag.AspNetCore" nella casella di ricerca</span><span class="sxs-lookup"><span data-stu-id="2761e-127">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="2761e-128">Selezionare il pacchetto "NSwag.AspNetCore" dalla scheda **Sfoglia** e fare clic su **Installa**</span><span class="sxs-lookup"><span data-stu-id="2761e-128">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2761e-129">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="2761e-129">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2761e-130">Fare clic su con il pulsante destro del mouse sulla cartella *Pacchetti* in **Solution Pad** (Riquadro della soluzione) > **Aggiungi pacchetti**</span><span class="sxs-lookup"><span data-stu-id="2761e-130">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="2761e-131">Impostare l'elenco a discesa **Aggiungi pacchetti** della finestra **Origine** su "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="2761e-131">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="2761e-132">Immettere "NSwag.AspNetCore" nella casella di ricerca</span><span class="sxs-lookup"><span data-stu-id="2761e-132">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="2761e-133">Selezionare il pacchetto "NSwag.AspNetCore" dal riquadro dei risultati e fare clic su **Aggiungi pacchetto**</span><span class="sxs-lookup"><span data-stu-id="2761e-133">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2761e-134">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2761e-134">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="2761e-135">Eseguire il comando seguente da **Terminale integrato**:</span><span class="sxs-lookup"><span data-stu-id="2761e-135">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2761e-136">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="2761e-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="2761e-137">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="2761e-137">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="2761e-138">Aggiungere e configurare il middleware di Swagger</span><span class="sxs-lookup"><span data-stu-id="2761e-138">Add and configure Swagger middleware</span></span>

 <span data-ttu-id="2761e-139">Aggiungere e configurare Swagger nell'app ASP.NET Core eseguendo i passaggi seguenti nella classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="2761e-139">Add and configure Swagger in your ASP.NET Core app by performing the following steps in the `Startup` class:</span></span>

* <span data-ttu-id="2761e-140">Importare gli spazi dei nomi seguenti:</span><span class="sxs-lookup"><span data-stu-id="2761e-140">Import the following namespaces:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

* <span data-ttu-id="2761e-141">Nel metodo `ConfigureServices` registrare i servizi Swagger necessari:</span><span class="sxs-lookup"><span data-stu-id="2761e-141">In the `ConfigureServices` method, register the required Swagger services:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

* <span data-ttu-id="2761e-142">Nel metodo `Configure` abilitare il middleware per la gestione della specifica generata e dell'interfaccia utente di Swagger:</span><span class="sxs-lookup"><span data-stu-id="2761e-142">In the `Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-7)]

* <span data-ttu-id="2761e-143">Avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="2761e-143">Launch the app.</span></span> <span data-ttu-id="2761e-144">Passare a:</span><span class="sxs-lookup"><span data-stu-id="2761e-144">Navigate to:</span></span>
  * <span data-ttu-id="2761e-145">`http://localhost:<port>/swagger` per visualizzare l'interfaccia utente di Swagger.</span><span class="sxs-lookup"><span data-stu-id="2761e-145">`http://localhost:<port>/swagger` to view the Swagger UI.</span></span>
  * <span data-ttu-id="2761e-146">`http://localhost:<port>/swagger/v1/swagger.json` per visualizzare la specifica di Swagger.</span><span class="sxs-lookup"><span data-stu-id="2761e-146">`http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="2761e-147">Generazione del codice</span><span class="sxs-lookup"><span data-stu-id="2761e-147">Code generation</span></span>

<span data-ttu-id="2761e-148">È possibile sfruttare i vantaggi delle funzionalità di generazione del codice di NSwag scegliendo una delle opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="2761e-148">You can take advantage of NSwag's code generation capabilities by choosing one of the following options:</span></span>

* <span data-ttu-id="2761e-149">[NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio) &ndash; Un'app desktop di Windows per generare il codice client in C# o TypeScript per l'API.</span><span class="sxs-lookup"><span data-stu-id="2761e-149">[NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio) &ndash; a Windows desktop app for generating API client code in C# or TypeScript.</span></span>
* <span data-ttu-id="2761e-150">I pacchetti NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) o [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) per generare il codice all'interno del progetto.</span><span class="sxs-lookup"><span data-stu-id="2761e-150">The [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages for code generation inside your project.</span></span>
* <span data-ttu-id="2761e-151">NSwag dalla [riga di comando](https://github.com/NSwag/NSwag/wiki/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="2761e-151">NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="2761e-152">Il pacchetto NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild).</span><span class="sxs-lookup"><span data-stu-id="2761e-152">The [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package.</span></span>
* <span data-ttu-id="2761e-153">Il [servizio connesso Unchase OpenAPI (Swagger)](https://marketplace.visualstudio.com/items?itemName=Unchase.unchaseopenapiconnectedservice) &ndash; è un servizio connesso di Visual Studio per la generazione di codice client API in C# o TypeScript,</span><span class="sxs-lookup"><span data-stu-id="2761e-153">The [Unchase OpenAPI (Swagger) Connected Service](https://marketplace.visualstudio.com/items?itemName=Unchase.unchaseopenapiconnectedservice) &ndash; a Visual Studio Connected Service for generating API client code in C# or TypeScript.</span></span> <span data-ttu-id="2761e-154">nonché per la generazione di controller C# per i servizi OpenAPI con NSwag.</span><span class="sxs-lookup"><span data-stu-id="2761e-154">Also generates C# controllers for OpenAPI services with NSwag.</span></span>

### <a name="generate-code-with-nswagstudio"></a><span data-ttu-id="2761e-155">Generare il codice con NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="2761e-155">Generate code with NSwagStudio</span></span>

* <span data-ttu-id="2761e-156">Installare NSwagStudio seguendo le istruzioni riportate nel [repository di GitHub NSwagStudio](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span><span class="sxs-lookup"><span data-stu-id="2761e-156">Install NSwagStudio by following the instructions at the [NSwagStudio GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="2761e-157">Avviare NSwagStudio e immettere l'URL del file *swagger.json* nella casella di testo **Swagger Specification URL** (URL di specifica Swagger).</span><span class="sxs-lookup"><span data-stu-id="2761e-157">Launch NSwagStudio and enter the *swagger.json* file URL in the **Swagger Specification URL** text box.</span></span> <span data-ttu-id="2761e-158">Ad esempio: *http://localhost:44354/swagger/v1/swagger.json* .</span><span class="sxs-lookup"><span data-stu-id="2761e-158">For example, *http://localhost:44354/swagger/v1/swagger.json*.</span></span>
* <span data-ttu-id="2761e-159">Fare clic sul pulsante **Create local Copy** (Crea copia locale) per generare una rappresentazione JSON della specifica di Swagger.</span><span class="sxs-lookup"><span data-stu-id="2761e-159">Click the **Create local Copy** button to generate a JSON representation of your Swagger specification.</span></span>

  ![Creare una copia locale della specifica di Swagger](web-api-help-pages-using-swagger/_static/CreateLocalCopy-NSwagStudio.PNG)

* <span data-ttu-id="2761e-161">Nell'area **Outputs** (Output) fare clic sulla casella di controllo **CSharp Client** (Client CSharp).</span><span class="sxs-lookup"><span data-stu-id="2761e-161">In the **Outputs** area, click the **CSharp Client** check box.</span></span> <span data-ttu-id="2761e-162">A seconda del progetto, è anche possibile scegliere **TypeScript Client** (Client TypeScript) o **CSharp Web API Controller** (Controller API Web CSharp).</span><span class="sxs-lookup"><span data-stu-id="2761e-162">Depending on your project, you can also choose **TypeScript Client** or **CSharp Web API Controller**.</span></span> <span data-ttu-id="2761e-163">Se si seleziona **CSharp Web API Controller** (Controller API Web CSharp), una specifica del servizio ricompila il servizio, fungendo da generazione inversa.</span><span class="sxs-lookup"><span data-stu-id="2761e-163">If you select **CSharp Web API Controller**, a service specification rebuilds the service, serving as a reverse generation.</span></span>
* <span data-ttu-id="2761e-164">Fare clic su **Generate Outputs** (Genera output) per generare un'implementazione client completa C# del progetto *TodoApi.NSwag*.</span><span class="sxs-lookup"><span data-stu-id="2761e-164">Click **Generate Outputs** to produce a complete C# client implementation of the *TodoApi.NSwag* project.</span></span> <span data-ttu-id="2761e-165">Per visualizzare il codice client generato, fare clic sulla scheda **CSharp Client** (Client CSharp):</span><span class="sxs-lookup"><span data-stu-id="2761e-165">To see the generated client code, click the **CSharp Client** tab:</span></span>

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v12.0.9.0 (NJsonSchema v9.13.10.0 (Newtonsoft.Json v11.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable

    [System.CodeDom.Compiler.GeneratedCode("NSwag", "12.0.9.0 (NJsonSchema v9.13.10.0 (Newtonsoft.Json v11.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "https://localhost:44354";
        private System.Net.Http.HttpClient _httpClient;
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient(System.Net.Http.HttpClient httpClient)
        {
            _httpClient = httpClient;
            _settings = new System.Lazy<Newtonsoft.Json.JsonSerializerSettings>(() =>
            {
                var settings = new Newtonsoft.Json.JsonSerializerSettings();
                UpdateJsonSerializerSettings(settings);
                return settings;
            });
        }

        public string BaseUrl
        {
            get { return _baseUrl; }
            set { _baseUrl = value; }
        }

        // code omitted for brevity
```

> [!TIP]
> <span data-ttu-id="2761e-166">Il codice client C# viene generato in base alle selezioni nella scheda **Settings** (Impostazioni). Modificare le impostazioni per eseguire attività come la ridenominazione dello spazio dei nomi predefinito e la generazione di metodi sincrona.</span><span class="sxs-lookup"><span data-stu-id="2761e-166">The C# client code is generated based on selections in the **Settings** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="2761e-167">Copiare il codice C# generato in un file nel progetto client che utilizzerà l'API.</span><span class="sxs-lookup"><span data-stu-id="2761e-167">Copy the generated C# code into a file in the client project that will consume the API.</span></span>
* <span data-ttu-id="2761e-168">Iniziare a usare l'API Web:</span><span class="sxs-lookup"><span data-stu-id="2761e-168">Start consuming the web API:</span></span>

```csharp
 var todoClient = new TodoClient();

// Gets all to-dos from the API
 var allTodos = await todoClient.GetAllAsync();

 // Create a new TodoItem, and save it via the API.
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

## <a name="customize-api-documentation"></a><span data-ttu-id="2761e-169">Personalizzare la documentazione dell'API</span><span class="sxs-lookup"><span data-stu-id="2761e-169">Customize API documentation</span></span>

<span data-ttu-id="2761e-170">Swagger offre opzioni per documentare il modello a oggetti e semplificare l'uso dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="2761e-170">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="2761e-171">Informazioni e descrizione API</span><span class="sxs-lookup"><span data-stu-id="2761e-171">API info and description</span></span>

<span data-ttu-id="2761e-172">Nel metodo `AddSwaggerDocument` l'azione di configurazione passata al metodo `Startup.ConfigureServices` aggiunge informazioni come autore, licenza e descrizione:</span><span class="sxs-lookup"><span data-stu-id="2761e-172">In the `Startup.ConfigureServices` method, a configuration action passed to the `AddSwaggerDocument` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_AddSwaggerDocument)]

<span data-ttu-id="2761e-173">L'interfaccia utente di Swagger visualizza le informazioni sulla versione:</span><span class="sxs-lookup"><span data-stu-id="2761e-173">The Swagger UI displays the version's information:</span></span>

![Interfaccia utente Swagger con informazioni sulla versione](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="2761e-175">XML (commenti)</span><span class="sxs-lookup"><span data-stu-id="2761e-175">XML comments</span></span>

<span data-ttu-id="2761e-176">Per abilitare i commenti XML, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="2761e-176">To enable XML comments, perform the following steps:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2761e-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2761e-177">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="2761e-178">Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Modifica <nome_progetto>.csproj**.</span><span class="sxs-lookup"><span data-stu-id="2761e-178">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="2761e-179">Aggiungere manualmente le righe evidenziate al file con estensione *csproj*:</span><span class="sxs-lookup"><span data-stu-id="2761e-179">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="2761e-180">Fare clic con il pulsante destro del mouse in **Esplora soluzioni** e selezionare **Proprietà**</span><span class="sxs-lookup"><span data-stu-id="2761e-180">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="2761e-181">Selezionare la casella di controllo **File di documentazione XML** nella sezione **Output** della scheda **Compilazione**</span><span class="sxs-lookup"><span data-stu-id="2761e-181">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2761e-182">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="2761e-182">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="2761e-183">Dal *riquadro della soluzione* premere **controllo** e fare clic sul nome del progetto.</span><span class="sxs-lookup"><span data-stu-id="2761e-183">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="2761e-184">Passare a **Strumenti** > **Modifica file**.</span><span class="sxs-lookup"><span data-stu-id="2761e-184">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="2761e-185">Aggiungere manualmente le righe evidenziate al file con estensione *csproj*:</span><span class="sxs-lookup"><span data-stu-id="2761e-185">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="2761e-186">Aprire la finestra di dialogo **Opzioni progetto** > **Compila** > **Compilatore**.</span><span class="sxs-lookup"><span data-stu-id="2761e-186">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="2761e-187">Selezionare la casella di controllo **Genera la documentazione XML** nella sezione **Opzioni generali**</span><span class="sxs-lookup"><span data-stu-id="2761e-187">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="2761e-188">Visual Studio Code/Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="2761e-188">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="2761e-189">Aggiungere manualmente le righe evidenziate al file con estensione *csproj*:</span><span class="sxs-lookup"><span data-stu-id="2761e-189">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a><span data-ttu-id="2761e-190">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="2761e-190">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="2761e-191">Dato che NSwag usa la [reflection](/dotnet/csharp/programming-guide/concepts/reflection) e il tipo restituito consigliato per le azioni API Web è [IActionResult](xref:Microsoft.AspNetCore.Mvc.IActionResult), non è possibile dedurre cosa sta facendo l'azione e il risultato.</span><span class="sxs-lookup"><span data-stu-id="2761e-191">Because NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](xref:Microsoft.AspNetCore.Mvc.IActionResult), it can't infer what your action is doing and what it returns.</span></span>

<span data-ttu-id="2761e-192">Si consideri l'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="2761e-192">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="2761e-193">L'azione precedente restituisce `IActionResult`, ma all'interno dell'azione viene restituito [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*) o [BadRequest](xref:System.Web.Http.ApiController.BadRequest*).</span><span class="sxs-lookup"><span data-stu-id="2761e-193">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*) or [BadRequest](xref:System.Web.Http.ApiController.BadRequest*).</span></span> <span data-ttu-id="2761e-194">Usare le annotazioni dei dati per indicare ai client i codici di stato HTTP restituiti da questa azione.</span><span class="sxs-lookup"><span data-stu-id="2761e-194">Use data annotations to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="2761e-195">Decorare l'azione con gli attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="2761e-195">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

 <span data-ttu-id="2761e-196">Dato che NSwag usa la [reflection](/dotnet/csharp/programming-guide/concepts/reflection) e il tipo restituito consigliato per le azioni API Web è [ActionResult\<T>](xref:Microsoft.AspNetCore.Mvc.ActionResult%601), può solo dedurre il tipo restituito definito da `T`.</span><span class="sxs-lookup"><span data-stu-id="2761e-196">Because NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](xref:Microsoft.AspNetCore.Mvc.ActionResult%601), it can only infer the return type defined by `T`.</span></span> <span data-ttu-id="2761e-197">Non è possibile dedurre automaticamente altri tipi restituiti possibili.</span><span class="sxs-lookup"><span data-stu-id="2761e-197">You can't automatically infer other possible return types.</span></span> 

<span data-ttu-id="2761e-198">Si consideri l'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="2761e-198">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="2761e-199">L'azione precedente restituisce `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="2761e-199">The preceding action returns `ActionResult<T>`.</span></span> <span data-ttu-id="2761e-200">All'interno dell'azione, viene restituito [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*).</span><span class="sxs-lookup"><span data-stu-id="2761e-200">Inside the action, it's returning [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*).</span></span> <span data-ttu-id="2761e-201">Poiché il controller è decorato con l'attributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute), è anche possibile una risposta [BadRequest](xref:System.Web.Http.ApiController.BadRequest*).</span><span class="sxs-lookup"><span data-stu-id="2761e-201">Since the controller is decorated with the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute, a [BadRequest](xref:System.Web.Http.ApiController.BadRequest*) response is possible, too.</span></span> <span data-ttu-id="2761e-202">Per altre informazioni, vedere [Risposte HTTP 400 automatiche](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="2761e-202">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span> <span data-ttu-id="2761e-203">Usare le annotazioni dei dati per indicare ai client i codici di stato HTTP restituiti da questa azione.</span><span class="sxs-lookup"><span data-stu-id="2761e-203">Use data annotations to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="2761e-204">Decorare l'azione con gli attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="2761e-204">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

<span data-ttu-id="2761e-205">In ASP.NET Core 2.2 o versioni successive, è possibile usare convenzioni in alternativa alla decorazione esplicita di azioni singole con `[ProducesResponseType]`.</span><span class="sxs-lookup"><span data-stu-id="2761e-205">In ASP.NET Core 2.2 or later, you can use conventions instead of explicitly decorating individual actions with `[ProducesResponseType]`.</span></span> <span data-ttu-id="2761e-206">Per ulteriori informazioni, vedere <xref:web-api/advanced/conventions>.</span><span class="sxs-lookup"><span data-stu-id="2761e-206">For more information, see <xref:web-api/advanced/conventions>.</span></span>

::: moniker-end

<span data-ttu-id="2761e-207">Il generatore di Swagger ora può descrivere accuratamente questa azione e i client generati conoscono la risposta che riceveranno quando si chiamerà l'endpoint.</span><span class="sxs-lookup"><span data-stu-id="2761e-207">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="2761e-208">È consigliabile decorare tutte le azioni con questi attributi.</span><span class="sxs-lookup"><span data-stu-id="2761e-208">As a recommendation, decorate all actions with these attributes.</span></span>

<span data-ttu-id="2761e-209">Per le linee guida sulle risposte HTTP restituite dalle azioni API, vedere la [specifica RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="2761e-209">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
