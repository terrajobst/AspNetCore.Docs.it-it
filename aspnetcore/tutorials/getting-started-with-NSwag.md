---
title: Introduzione a NSwag e ad ASP.NET Core
author: zuckerthoben
description: Informazioni su come usare NSwag per generare le pagine della documentazione e della Guida per un'API Web di ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 09/20/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: b9266e2df75563be6bad1a1f464cef788c333d4c
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028168"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="c91df-103">Introduzione a NSwag e ad ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c91df-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="c91df-104">Di [Christoph Nienaber](https://twitter.com/zuckerthoben) e [Rico Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="c91df-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c91df-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c91df-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="c91df-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c91df-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="c91df-107">Registrare i middleware NSwag per:</span><span class="sxs-lookup"><span data-stu-id="c91df-107">Register the NSwag middlewares to:</span></span>

* <span data-ttu-id="c91df-108">Generare la specifica Swagger per l'API Web implementata.</span><span class="sxs-lookup"><span data-stu-id="c91df-108">Generate the Swagger specification for the implemented web API.</span></span>
* <span data-ttu-id="c91df-109">Usare l'interfaccia utente di Swagger per esplorare e testare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="c91df-109">Serve the Swagger UI to browse and test the web API.</span></span>

<span data-ttu-id="c91df-110">Per usare i middleware ASP.NET Core di [NSwag](https://github.com/RSuter/NSwag), installare il pacchetto NuGet [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/).</span><span class="sxs-lookup"><span data-stu-id="c91df-110">To use the [NSwag](https://github.com/RSuter/NSwag) ASP.NET Core middlewares, install the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="c91df-111">Questo pacchetto contiene i middleware necessari per generare e usare la specifica Swagger, l'interfaccia utente di Swagger (v2 e v3) e l'[interfaccia utente di ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="c91df-111">This package contains the middlewares to generate and serve the Swagger specification, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="c91df-112">È inoltre consigliabile usare le funzionalità di generazione del codice di NSwag.</span><span class="sxs-lookup"><span data-stu-id="c91df-112">Additionally, it's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="c91df-113">Scegliere una delle opzioni seguenti per usare le funzionalità di generazione del codice:</span><span class="sxs-lookup"><span data-stu-id="c91df-113">Choose one of the following options to use the code generation capabilities:</span></span>

* <span data-ttu-id="c91df-114">Usare [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), un'applicazione desktop di Windows per generare il codice client in C# e TypeScript per l'API.</span><span class="sxs-lookup"><span data-stu-id="c91df-114">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API.</span></span>
* <span data-ttu-id="c91df-115">Usare i pacchetti NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) o [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) per generare il codice all'interno del progetto.</span><span class="sxs-lookup"><span data-stu-id="c91df-115">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project.</span></span>
* <span data-ttu-id="c91df-116">Usare NSwag dalla [riga di comando](https://github.com/NSwag/NSwag/wiki/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="c91df-116">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="c91df-117">Usare il pacchetto NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild).</span><span class="sxs-lookup"><span data-stu-id="c91df-117">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package.</span></span>

## <a name="features"></a><span data-ttu-id="c91df-118">Funzionalità</span><span class="sxs-lookup"><span data-stu-id="c91df-118">Features</span></span>

<span data-ttu-id="c91df-119">È consigliabile usare NSwag soprattutto perché consente non solo di introdurre l'interfaccia utente e il generatore di Swagger, ma anche di usare le funzionalità flessibili di generazione del codice.</span><span class="sxs-lookup"><span data-stu-id="c91df-119">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to also make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="c91df-120">Non occorre un'API esistente. È possibile usare le API di terze parti che includono Swagger e consentire a NSwag di generare un'implementazione client.</span><span class="sxs-lookup"><span data-stu-id="c91df-120">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="c91df-121">Il ciclo di sviluppo risulta in ogni caso velocizzato ed è facile adattarsi alle modifiche dell'API.</span><span class="sxs-lookup"><span data-stu-id="c91df-121">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="c91df-122">Installazione del pacchetto</span><span class="sxs-lookup"><span data-stu-id="c91df-122">Package installation</span></span>

<span data-ttu-id="c91df-123">È possibile aggiungere il pacchetto NuGet NSwag con gli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="c91df-123">The NSwag NuGet package can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c91df-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c91df-124">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c91df-125">Dalla finestra **Console di Gestione pacchetti**:</span><span class="sxs-lookup"><span data-stu-id="c91df-125">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="c91df-126">Passare a **Vista** > **Altre finestre** > **Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="c91df-126">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="c91df-127">Passare alla directory che contiene il file *TodoApi.csproj*</span><span class="sxs-lookup"><span data-stu-id="c91df-127">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="c91df-128">Eseguire il seguente comando:</span><span class="sxs-lookup"><span data-stu-id="c91df-128">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="c91df-129">Dalla finestra di dialogo **Gestisci pacchetti NuGet**:</span><span class="sxs-lookup"><span data-stu-id="c91df-129">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="c91df-130">Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** > **Gestisci pacchetti NuGet**</span><span class="sxs-lookup"><span data-stu-id="c91df-130">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="c91df-131">Impostare **Origine pacchetto** su "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="c91df-131">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="c91df-132">Immettere "NSwag.AspNetCore" nella casella di ricerca</span><span class="sxs-lookup"><span data-stu-id="c91df-132">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="c91df-133">Selezionare il pacchetto "NSwag.AspNetCore" dalla scheda **Sfoglia** e fare clic su **Installa**</span><span class="sxs-lookup"><span data-stu-id="c91df-133">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c91df-134">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="c91df-134">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c91df-135">Fare clic su con il pulsante destro del mouse sulla cartella *Pacchetti* in **Solution Pad** (Riquadro della soluzione) > **Aggiungi pacchetti**</span><span class="sxs-lookup"><span data-stu-id="c91df-135">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="c91df-136">Impostare l'elenco a discesa **Aggiungi pacchetti** della finestra **Origine** su "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="c91df-136">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="c91df-137">Immettere "NSwag.AspNetCore" nella casella di ricerca</span><span class="sxs-lookup"><span data-stu-id="c91df-137">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="c91df-138">Selezionare il pacchetto "NSwag.AspNetCore" dal riquadro dei risultati e fare clic su **Aggiungi pacchetto**</span><span class="sxs-lookup"><span data-stu-id="c91df-138">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c91df-139">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c91df-139">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c91df-140">Eseguire il comando seguente da **Terminale integrato**:</span><span class="sxs-lookup"><span data-stu-id="c91df-140">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c91df-141">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="c91df-141">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c91df-142">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c91df-142">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="c91df-143">Aggiungere e configurare il middleware di Swagger</span><span class="sxs-lookup"><span data-stu-id="c91df-143">Add and configure Swagger middleware</span></span>

<span data-ttu-id="c91df-144">Importare gli spazi dei nomi seguenti nella classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="c91df-144">Import the following namespaces in the `Startup` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

<span data-ttu-id="c91df-145">Nel metodo `Startup.ConfigureServices` registrare i servizi Swagger necessari:</span><span class="sxs-lookup"><span data-stu-id="c91df-145">In the `Startup.ConfigureServices` method, register the required Swagger services:</span></span> 

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

<span data-ttu-id="c91df-146">Nel metodo `Startup.Configure` abilitare il middleware per la gestione della specifica Swagger generata e dell'interfaccia utente di Swagger v3:</span><span class="sxs-lookup"><span data-stu-id="c91df-146">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI v3:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

<span data-ttu-id="c91df-147">Avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="c91df-147">Launch the app.</span></span> <span data-ttu-id="c91df-148">Passare a `http://localhost:<port>/swagger` per visualizzare l'interfaccia utente di Swagger.</span><span class="sxs-lookup"><span data-stu-id="c91df-148">Navigate to `http://localhost:<port>/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="c91df-149">Passare a `http://localhost:<port>/swagger/v1/swagger.json` per visualizzare la specifica di Swagger.</span><span class="sxs-lookup"><span data-stu-id="c91df-149">Navigate to `http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="c91df-150">Generazione del codice</span><span class="sxs-lookup"><span data-stu-id="c91df-150">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="c91df-151">Tramite NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="c91df-151">Via NSwagStudio</span></span>

* <span data-ttu-id="c91df-152">Installare NSwagStudio dal [repository GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio) ufficiale.</span><span class="sxs-lookup"><span data-stu-id="c91df-152">Install NSwagStudio from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="c91df-153">Tramite NSwagStudio.</span><span class="sxs-lookup"><span data-stu-id="c91df-153">Launch NSwagStudio.</span></span> <span data-ttu-id="c91df-154">Immettere l'URL del file *swagger.json* nella casella di testo **Swagger Specification URL** (URL di specifica Swagger) e fare clic sul pulsante **Create local Copy** (Crea copia locale).</span><span class="sxs-lookup"><span data-stu-id="c91df-154">Enter the *swagger.json* file URL in the **Swagger Specification URL** textbox, and click the **Create local Copy** button.</span></span>
* <span data-ttu-id="c91df-155">Selezionare il tipo di output del client **CSharp Client** (Client CSharp).</span><span class="sxs-lookup"><span data-stu-id="c91df-155">Select the **CSharp Client** client output type.</span></span> <span data-ttu-id="c91df-156">Le opzioni includono **TypeScript** (Client TypeScript) e **CSharp Web API Controller** (Controller API Web CSharp).</span><span class="sxs-lookup"><span data-stu-id="c91df-156">Other options include **TypeScript Client** and **CSharp Web API Controller**.</span></span> <span data-ttu-id="c91df-157">L'uso di un controller API Web è sostanzialmente una generazione inversa.</span><span class="sxs-lookup"><span data-stu-id="c91df-157">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="c91df-158">Viene usata una specifica di un servizio per rigenerare il servizio.</span><span class="sxs-lookup"><span data-stu-id="c91df-158">It uses a specification of a service to rebuild the service.</span></span>
* <span data-ttu-id="c91df-159">Fare clic sul pulsante **Generate Outputs** (Genera output).</span><span class="sxs-lookup"><span data-stu-id="c91df-159">Click the **Generate Outputs** button.</span></span> <span data-ttu-id="c91df-160">Verrà generata un'implementazione client completa C# del progetto *TodoApi.NSwag*.</span><span class="sxs-lookup"><span data-stu-id="c91df-160">A complete C# client implementation of the *TodoApi.NSwag* project is produced.</span></span> <span data-ttu-id="c91df-161">Fare clic sulla scheda **CSharp Client** (Client CSharp) dalla sezione **Outputs** (Output) per visualizzare il codice client generato:</span><span class="sxs-lookup"><span data-stu-id="c91df-161">Click the **CSharp Client** tab of the **Outputs** section to see the generated client code:</span></span>

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable // Disable all warnings

    [System.CodeDom.Compiler.GeneratedCode("NSwag",
        "11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "http://localhost:50499";
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient()
        {
            _settings = new System.Lazy
                <Newtonsoft.Json.JsonSerializerSettings>(() =>
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
> <span data-ttu-id="c91df-162">Il codice client C# viene generato in base alle impostazioni definite nella scheda **Settings** (Impostazioni) della scheda **CSharp Client** (Client CSharp). Modificare le impostazioni per eseguire attività come la ridenominazione dello spazio dei nomi predefinito e la generazione di metodi sincrona.</span><span class="sxs-lookup"><span data-stu-id="c91df-162">The C# client code is generated based on settings defined in the **Settings** tab of the **CSharp Client** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="c91df-163">Copiare il codice C# generato in un file di un progetto client, ad esempio un'app [Xamarin.Forms](/xamarin/xamarin-forms/).</span><span class="sxs-lookup"><span data-stu-id="c91df-163">Copy the generated C# code into a file in a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span>
* <span data-ttu-id="c91df-164">Iniziare a usare l'API Web:</span><span class="sxs-lookup"><span data-stu-id="c91df-164">Start consuming the web API:</span></span>

```csharp
var todoClient = new TodoClient();

// Gets all to-dos from the API
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem, and save it in the API
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> <span data-ttu-id="c91df-165">È possibile inserire un URL di base e/o un client HTTP nel client dell'API.</span><span class="sxs-lookup"><span data-stu-id="c91df-165">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="c91df-166">È sempre consigliabile [riusare HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span><span class="sxs-lookup"><span data-stu-id="c91df-166">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="c91df-167">Altri modi per generare codice client</span><span class="sxs-lookup"><span data-stu-id="c91df-167">Other ways to generate client code</span></span>

<span data-ttu-id="c91df-168">È possibile generare il codice client in altri modi, più adatti al flusso di lavoro:</span><span class="sxs-lookup"><span data-stu-id="c91df-168">You can generate the client code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="c91df-169">MSBuild</span><span class="sxs-lookup"><span data-stu-id="c91df-169">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="c91df-170">Nel codice</span><span class="sxs-lookup"><span data-stu-id="c91df-170">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="c91df-171">Modelli T4</span><span class="sxs-lookup"><span data-stu-id="c91df-171">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a><span data-ttu-id="c91df-172">Personalizza</span><span class="sxs-lookup"><span data-stu-id="c91df-172">Customize</span></span>

<span data-ttu-id="c91df-173">Swagger offre opzioni per documentare il modello a oggetti e semplificare l'uso dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="c91df-173">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="c91df-174">Informazioni e descrizione API</span><span class="sxs-lookup"><span data-stu-id="c91df-174">API info and description</span></span>

<span data-ttu-id="c91df-175">Nel metodo `UseSwagger` l'azione di configurazione passata al metodo `Startup.Configure` aggiunge informazioni come autore, licenza e descrizione:</span><span class="sxs-lookup"><span data-stu-id="c91df-175">In the `Startup.Configure` method, a configuration action passed to the `UseSwagger` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

<span data-ttu-id="c91df-176">L'interfaccia utente di Swagger visualizza le informazioni sulla versione:</span><span class="sxs-lookup"><span data-stu-id="c91df-176">The Swagger UI displays the version's information:</span></span>

![Interfaccia utente Swagger con informazioni sulla versione](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="c91df-178">XML (commenti)</span><span class="sxs-lookup"><span data-stu-id="c91df-178">XML comments</span></span>

<span data-ttu-id="c91df-179">I commenti XML vengono abilitati con gli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="c91df-179">XML comments are enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="c91df-180">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c91df-180">Visual Studio</span></span>](#tab/visual-studio-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="c91df-181">Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Modifica <nome_progetto>.csproj**.</span><span class="sxs-lookup"><span data-stu-id="c91df-181">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="c91df-182">Aggiungere manualmente le righe evidenziate al file con estensione *csproj*:</span><span class="sxs-lookup"><span data-stu-id="c91df-182">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="c91df-183">Fare clic con il pulsante destro del mouse in **Esplora soluzioni** e selezionare **Proprietà**</span><span class="sxs-lookup"><span data-stu-id="c91df-183">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="c91df-184">Selezionare la casella di controllo **File di documentazione XML** nella sezione **Output** della scheda **Compilazione**</span><span class="sxs-lookup"><span data-stu-id="c91df-184">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="c91df-185">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="c91df-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="c91df-186">Dal *riquadro della soluzione* premere **controllo** e fare clic sul nome del progetto.</span><span class="sxs-lookup"><span data-stu-id="c91df-186">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="c91df-187">Passare a **Strumenti** > **Modifica file**.</span><span class="sxs-lookup"><span data-stu-id="c91df-187">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="c91df-188">Aggiungere manualmente le righe evidenziate al file con estensione *csproj*:</span><span class="sxs-lookup"><span data-stu-id="c91df-188">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="c91df-189">Aprire la finestra di dialogo **Opzioni progetto** > **Compila** > **Compilatore**.</span><span class="sxs-lookup"><span data-stu-id="c91df-189">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="c91df-190">Selezionare la casella di controllo **Genera la documentazione XML** nella sezione **Opzioni generali**</span><span class="sxs-lookup"><span data-stu-id="c91df-190">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="c91df-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c91df-191">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="c91df-192">Aggiungere manualmente le righe evidenziate al file con estensione *csproj*:</span><span class="sxs-lookup"><span data-stu-id="c91df-192">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a><span data-ttu-id="c91df-193">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="c91df-193">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="c91df-194">NSwag usa la [reflection](/dotnet/csharp/programming-guide/concepts/reflection). Il tipo restituito consigliato per le azioni dell'API Web è [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="c91df-194">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="c91df-195">Di conseguenza, non è possibile per NSwag dedurre l'azione e il valore restituito.</span><span class="sxs-lookup"><span data-stu-id="c91df-195">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="c91df-196">Si consideri l'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="c91df-196">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="c91df-197">L'azione precedente restituisce `IActionResult`, ma all'interno dell'azione viene restituito [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) o [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="c91df-197">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="c91df-198">Le annotazioni dei dati vengono usate per indicare ai client i codici di stato HTTP restituiti da questa azione.</span><span class="sxs-lookup"><span data-stu-id="c91df-198">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="c91df-199">Decorare l'azione con gli attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c91df-199">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c91df-200">NSwag usa la [reflection](/dotnet/csharp/programming-guide/concepts/reflection). Il tipo restituito consigliato per le azioni dell'API Web è [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span><span class="sxs-lookup"><span data-stu-id="c91df-200">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span></span> <span data-ttu-id="c91df-201">Di conseguenza, NSwag può solo dedurre il tipo restituito definito da `T`.</span><span class="sxs-lookup"><span data-stu-id="c91df-201">Consequently, NSwag can only infer the return type defined by `T`.</span></span> <span data-ttu-id="c91df-202">Non è possibile dedurre altri tipi restituiti possibili dell'azione.</span><span class="sxs-lookup"><span data-stu-id="c91df-202">Other possible return types in the action cannot be inferred.</span></span> <span data-ttu-id="c91df-203">Si consideri l'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="c91df-203">Consider the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

<span data-ttu-id="c91df-204">L'azione precedente restituisce `ActionResult<T>`, ma all'interno dell'azione viene restituito [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span><span class="sxs-lookup"><span data-stu-id="c91df-204">The preceding action returns `ActionResult<T>`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span></span> <span data-ttu-id="c91df-205">Poiché il controller è decorato con l'attributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute), è anche possibile una risposta [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="c91df-205">Since the controller is decorated with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute, a [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) response is possible too.</span></span> <span data-ttu-id="c91df-206">Vedere [Risposte HTTP 400 automatiche](xref:web-api/index#automatic-http-400-responses) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="c91df-206">See [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses) for more info.</span></span> <span data-ttu-id="c91df-207">Le annotazioni dei dati vengono usate per indicare ai client i codici di stato HTTP restituiti da questa azione.</span><span class="sxs-lookup"><span data-stu-id="c91df-207">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="c91df-208">Decorare l'azione con gli attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c91df-208">Decorate the action with the following attributes:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

<span data-ttu-id="c91df-209">Il generatore di Swagger ora può descrivere accuratamente questa azione e i client generati conoscono la risposta che riceveranno quando si chiamerà l'endpoint.</span><span class="sxs-lookup"><span data-stu-id="c91df-209">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="c91df-210">È consigliabile la decorazione di tutte le azioni con questi attributi.</span><span class="sxs-lookup"><span data-stu-id="c91df-210">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="c91df-211">Per le linee guida sulle risposte HTTP restituite dalle azioni API, vedere la [specifica RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="c91df-211">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
