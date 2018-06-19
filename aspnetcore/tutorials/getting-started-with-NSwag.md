---
title: Introduzione a NSwag e ad ASP.NET Core
author: zuckerthoben
description: Informazioni su come usare NSwag per generare le pagine della documentazione e della Guida per un'API Web di ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 0f0aa0eeaa174ef40f03aab2500a8b3ce37e9448
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/12/2018
ms.locfileid: "34094892"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="c73be-103">Introduzione a NSwag e ad ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c73be-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="c73be-104">Di [Christoph Nienaber](https://twitter.com/zuckerthoben) e [Rico Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="c73be-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="c73be-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c73be-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="c73be-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c73be-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end

<span data-ttu-id="c73be-107">Per usare [NSwag](https://github.com/RSuter/NSwag) con il middleware di ASP.NET Core è necessario il pacchetto NuGet[NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/).</span><span class="sxs-lookup"><span data-stu-id="c73be-107">Using [NSwag](https://github.com/RSuter/NSwag) with ASP.NET Core middleware requires the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="c73be-108">Il pacchetto contiene un generatore di Swagger, l'interfaccia utente di Swagger (versione 2 e 3) e [l'interfaccia utente di ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="c73be-108">The package consists of a Swagger generator, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="c73be-109">È consigliabile usare le funzionalità di generazione del codice di NSwag.</span><span class="sxs-lookup"><span data-stu-id="c73be-109">It's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="c73be-110">Scegliere una delle opzioni seguenti per generare il codice:</span><span class="sxs-lookup"><span data-stu-id="c73be-110">Choose one of the following options for code generation:</span></span>

* <span data-ttu-id="c73be-111">Usare [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), un'applicazione desktop di Windows per generare il codice client in C# e TypeScript per l'API.</span><span class="sxs-lookup"><span data-stu-id="c73be-111">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API.</span></span>
* <span data-ttu-id="c73be-112">Usare i pacchetti NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) o [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) per generare il codice all'interno del progetto.</span><span class="sxs-lookup"><span data-stu-id="c73be-112">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project.</span></span>
* <span data-ttu-id="c73be-113">Usare NSwag dalla [riga di comando](https://github.com/NSwag/NSwag/wiki/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="c73be-113">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="c73be-114">Usare il pacchetto NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild).</span><span class="sxs-lookup"><span data-stu-id="c73be-114">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package.</span></span>

## <a name="features"></a><span data-ttu-id="c73be-115">Funzionalità</span><span class="sxs-lookup"><span data-stu-id="c73be-115">Features</span></span>

<span data-ttu-id="c73be-116">È consigliabile usare NSwag soprattutto perché consente non solo di introdurre l'interfaccia utente e il generatore di Swagger, ma di usare le funzionalità flessibili di generazione del codice.</span><span class="sxs-lookup"><span data-stu-id="c73be-116">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="c73be-117">Non occorre un'API esistente. È possibile usare le API di terze parti che includono Swagger e consentire a NSwag di generare un'implementazione client.</span><span class="sxs-lookup"><span data-stu-id="c73be-117">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="c73be-118">Il ciclo di sviluppo risulta in ogni caso velocizzato ed è facile adattarsi alle modifiche dell'API.</span><span class="sxs-lookup"><span data-stu-id="c73be-118">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="c73be-119">Installazione del pacchetto</span><span class="sxs-lookup"><span data-stu-id="c73be-119">Package installation</span></span>

<span data-ttu-id="c73be-120">È possibile aggiungere il pacchetto NuGet NSwag con gli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="c73be-120">The NSwag NuGet package can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c73be-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c73be-121">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c73be-122">Dalla finestra **Console di Gestione pacchetti**:</span><span class="sxs-lookup"><span data-stu-id="c73be-122">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="c73be-123">Passare a **Vista** > **Altre finestre** > **Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="c73be-123">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="c73be-124">Passare alla directory che contiene il file *TodoApi.csproj*</span><span class="sxs-lookup"><span data-stu-id="c73be-124">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="c73be-125">Eseguire il seguente comando:</span><span class="sxs-lookup"><span data-stu-id="c73be-125">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="c73be-126">Dalla finestra di dialogo **Gestisci pacchetti NuGet**:</span><span class="sxs-lookup"><span data-stu-id="c73be-126">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="c73be-127">Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** > **Gestisci pacchetti NuGet**</span><span class="sxs-lookup"><span data-stu-id="c73be-127">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="c73be-128">Impostare **Origine pacchetto** su "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="c73be-128">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="c73be-129">Immettere "NSwag.AspNetCore" nella casella di ricerca</span><span class="sxs-lookup"><span data-stu-id="c73be-129">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="c73be-130">Selezionare il pacchetto "NSwag.AspNetCore" dalla scheda **Sfoglia** e fare clic su **Installa**</span><span class="sxs-lookup"><span data-stu-id="c73be-130">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c73be-131">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="c73be-131">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c73be-132">Fare clic su con il pulsante destro del mouse sulla cartella *Pacchetti* in **Solution Pad** (Riquadro della soluzione) > **Aggiungi pacchetti**</span><span class="sxs-lookup"><span data-stu-id="c73be-132">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="c73be-133">Impostare l'elenco a discesa **Aggiungi pacchetti** della finestra **Origine** su "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="c73be-133">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="c73be-134">Immettere "NSwag.AspNetCore" nella casella di ricerca</span><span class="sxs-lookup"><span data-stu-id="c73be-134">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="c73be-135">Selezionare il pacchetto "NSwag.AspNetCore" dal riquadro dei risultati e fare clic su **Aggiungi pacchetto**</span><span class="sxs-lookup"><span data-stu-id="c73be-135">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c73be-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c73be-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c73be-137">Eseguire il comando seguente da **Terminale integrato**:</span><span class="sxs-lookup"><span data-stu-id="c73be-137">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c73be-138">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="c73be-138">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c73be-139">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c73be-139">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="c73be-140">Aggiungere e configurare il middleware di Swagger</span><span class="sxs-lookup"><span data-stu-id="c73be-140">Add and configure Swagger middleware</span></span>

<span data-ttu-id="c73be-141">Importare gli spazi dei nomi seguenti nella classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="c73be-141">Import the following namespaces in the `Startup` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

<span data-ttu-id="c73be-142">Nel metodo `Startup.Configure` abilitare il middleware per la gestione della specifica generata e dell'interfaccia utente di Swagger:</span><span class="sxs-lookup"><span data-stu-id="c73be-142">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

<span data-ttu-id="c73be-143">Avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="c73be-143">Launch the app.</span></span> <span data-ttu-id="c73be-144">Passare a `http://localhost:<port>/swagger` per visualizzare l'interfaccia utente di Swagger.</span><span class="sxs-lookup"><span data-stu-id="c73be-144">Navigate to `http://localhost:<port>/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="c73be-145">Passare a `http://localhost:<port>/swagger/v1/swagger.json` per visualizzare la specifica di Swagger.</span><span class="sxs-lookup"><span data-stu-id="c73be-145">Navigate to `http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="c73be-146">Generazione del codice</span><span class="sxs-lookup"><span data-stu-id="c73be-146">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="c73be-147">Tramite NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="c73be-147">Via NSwagStudio</span></span>

* <span data-ttu-id="c73be-148">Installare NSwagStudio dal [repository GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio) ufficiale.</span><span class="sxs-lookup"><span data-stu-id="c73be-148">Install NSwagStudio from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="c73be-149">Tramite NSwagStudio.</span><span class="sxs-lookup"><span data-stu-id="c73be-149">Launch NSwagStudio.</span></span> <span data-ttu-id="c73be-150">Immettere l'URL del file *swagger.json* nella casella di testo **Swagger Specification URL** (URL di specifica Swagger) e fare clic sul pulsante **Create local Copy** (Crea copia locale).</span><span class="sxs-lookup"><span data-stu-id="c73be-150">Enter the *swagger.json* file URL in the **Swagger Specification URL** textbox, and click the **Create local Copy** button.</span></span>
* <span data-ttu-id="c73be-151">Selezionare il tipo di output del client **CSharp Client** (Client CSharp).</span><span class="sxs-lookup"><span data-stu-id="c73be-151">Select the **CSharp Client** client output type.</span></span> <span data-ttu-id="c73be-152">Le opzioni includono **TypeScript** (Client TypeScript) e **CSharp Web API Controller** (Controller API Web CSharp).</span><span class="sxs-lookup"><span data-stu-id="c73be-152">Other options include **TypeScript Client** and **CSharp Web API Controller**.</span></span> <span data-ttu-id="c73be-153">L'uso di un controller API Web è sostanzialmente una generazione inversa.</span><span class="sxs-lookup"><span data-stu-id="c73be-153">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="c73be-154">Viene usata una specifica di un servizio per rigenerare il servizio.</span><span class="sxs-lookup"><span data-stu-id="c73be-154">It uses a specification of a service to rebuild the service.</span></span>
* <span data-ttu-id="c73be-155">Fare clic sul pulsante **Generate Outputs** (Genera output).</span><span class="sxs-lookup"><span data-stu-id="c73be-155">Click the **Generate Outputs** button.</span></span> <span data-ttu-id="c73be-156">Verrà generata un'implementazione client completa C# del progetto *TodoApi.NSwag*.</span><span class="sxs-lookup"><span data-stu-id="c73be-156">A complete C# client implementation of the *TodoApi.NSwag* project is produced.</span></span> <span data-ttu-id="c73be-157">Fare clic sulla scheda **CSharp Client** (Client CSharp) dalla sezione **Outputs** (Output) per visualizzare il codice client generato:</span><span class="sxs-lookup"><span data-stu-id="c73be-157">Click the **CSharp Client** tab of the **Outputs** section to see the generated client code:</span></span>

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
> <span data-ttu-id="c73be-158">Il codice client C# viene generato in base alle impostazioni definite nella scheda **Settings** (Impostazioni) della scheda **CSharp Client** (Client CSharp). Modificare le impostazioni per eseguire attività come la ridenominazione dello spazio dei nomi predefinito e la generazione di metodi sincrona.</span><span class="sxs-lookup"><span data-stu-id="c73be-158">The C# client code is generated based on settings defined in the **Settings** tab of the **CSharp Client** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="c73be-159">Copiare il codice C# generato in un file di un progetto client, ad esempio un'app [Xamarin.Forms](/xamarin/xamarin-forms/).</span><span class="sxs-lookup"><span data-stu-id="c73be-159">Copy the generated C# code into a file in a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span>
* <span data-ttu-id="c73be-160">Iniziare a usare l'API Web:</span><span class="sxs-lookup"><span data-stu-id="c73be-160">Start consuming the web API:</span></span>

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
> <span data-ttu-id="c73be-161">È possibile inserire un URL di base e/o un client HTTP nel client dell'API.</span><span class="sxs-lookup"><span data-stu-id="c73be-161">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="c73be-162">È sempre consigliabile [riusare HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span><span class="sxs-lookup"><span data-stu-id="c73be-162">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="c73be-163">Altri modi per generare codice client</span><span class="sxs-lookup"><span data-stu-id="c73be-163">Other ways to generate client code</span></span>

<span data-ttu-id="c73be-164">È possibile generare il codice client in altri modi, più adatti al flusso di lavoro:</span><span class="sxs-lookup"><span data-stu-id="c73be-164">You can generate the client code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="c73be-165">MSBuild</span><span class="sxs-lookup"><span data-stu-id="c73be-165">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="c73be-166">Nel codice</span><span class="sxs-lookup"><span data-stu-id="c73be-166">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="c73be-167">Modelli T4</span><span class="sxs-lookup"><span data-stu-id="c73be-167">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a><span data-ttu-id="c73be-168">Personalizza</span><span class="sxs-lookup"><span data-stu-id="c73be-168">Customize</span></span>

<span data-ttu-id="c73be-169">Swagger offre opzioni per documentare il modello a oggetti e semplificare l'uso dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="c73be-169">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="c73be-170">Informazioni e descrizione API</span><span class="sxs-lookup"><span data-stu-id="c73be-170">API info and description</span></span>

<span data-ttu-id="c73be-171">Nel metodo `UseSwagger` l'azione di configurazione passata al metodo `Startup.Configure` aggiunge informazioni come autore, licenza e descrizione:</span><span class="sxs-lookup"><span data-stu-id="c73be-171">In the `Startup.Configure` method, a configuration action passed to the `UseSwagger` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

<span data-ttu-id="c73be-172">L'interfaccia utente di Swagger visualizza le informazioni sulla versione:</span><span class="sxs-lookup"><span data-stu-id="c73be-172">The Swagger UI displays the version's information:</span></span>

![Interfaccia utente Swagger con informazioni sulla versione](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="c73be-174">XML (commenti)</span><span class="sxs-lookup"><span data-stu-id="c73be-174">XML comments</span></span>

<span data-ttu-id="c73be-175">I commenti XML vengono abilitati con gli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="c73be-175">XML comments are enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="c73be-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c73be-176">Visual Studio</span></span>](#tab/visual-studio-xml/)

* <span data-ttu-id="c73be-177">Fare clic con il pulsante destro del mouse in **Esplora soluzioni** e selezionare **Proprietà**</span><span class="sxs-lookup"><span data-stu-id="c73be-177">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="c73be-178">Selezionare la casella di controllo **File di documentazione XML** nella sezione **Output** della scheda **Compilazione**</span><span class="sxs-lookup"><span data-stu-id="c73be-178">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="c73be-179">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="c73be-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

* <span data-ttu-id="c73be-180">Aprire la finestra di dialogo **Opzioni progetto** > **Compila** > **Compilatore**</span><span class="sxs-lookup"><span data-stu-id="c73be-180">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="c73be-181">Selezionare la casella di controllo **Genera la documentazione XML** nella sezione **Opzioni generali**</span><span class="sxs-lookup"><span data-stu-id="c73be-181">Check the **Generate xml documentation** box under the **General Options** section</span></span>

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="c73be-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c73be-182">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="c73be-183">Aggiungere manualmente il frammento di codice seguente al file *csproj*:</span><span class="sxs-lookup"><span data-stu-id="c73be-183">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement)]

---

### <a name="data-annotations"></a><span data-ttu-id="c73be-184">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="c73be-184">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="c73be-185">NSwag usa la [reflection](/dotnet/csharp/programming-guide/concepts/reflection). Il tipo restituito consigliato per le azioni dell'API Web è [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="c73be-185">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="c73be-186">Di conseguenza, non è possibile per NSwag dedurre l'azione e il valore restituito.</span><span class="sxs-lookup"><span data-stu-id="c73be-186">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="c73be-187">Si consideri l'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="c73be-187">Consider the following example:</span></span>

<span data-ttu-id="c73be-188">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span><span class="sxs-lookup"><span data-stu-id="c73be-188">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span></span>

<span data-ttu-id="c73be-189">L'azione precedente restituisce `IActionResult`, ma all'interno dell'azione viene restituito [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) o [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="c73be-189">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="c73be-190">Le annotazioni dei dati vengono usate per indicare ai client i codici di stato HTTP restituiti da questa azione.</span><span class="sxs-lookup"><span data-stu-id="c73be-190">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="c73be-191">Decorare l'azione con gli attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c73be-191">Decorate the action with the following attributes:</span></span>

<span data-ttu-id="c73be-192">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span><span class="sxs-lookup"><span data-stu-id="c73be-192">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="c73be-193">NSwag usa la [reflection](/dotnet/csharp/programming-guide/concepts/reflection). Il tipo restituito consigliato per le azioni dell'API Web è [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span><span class="sxs-lookup"><span data-stu-id="c73be-193">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span></span> <span data-ttu-id="c73be-194">Di conseguenza, NSwag può solo dedurre il tipo restituito definito da `T`.</span><span class="sxs-lookup"><span data-stu-id="c73be-194">Consequently, NSwag can only infer the return type defined by `T`.</span></span> <span data-ttu-id="c73be-195">Non è possibile dedurre altri tipi restituiti possibili dell'azione.</span><span class="sxs-lookup"><span data-stu-id="c73be-195">Other possible return types in the action cannot be inferred.</span></span> <span data-ttu-id="c73be-196">Si consideri l'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="c73be-196">Consider the following example:</span></span>

<span data-ttu-id="c73be-197">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span><span class="sxs-lookup"><span data-stu-id="c73be-197">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span></span>

<span data-ttu-id="c73be-198">L'azione precedente restituisce `ActionResult<T>`, ma all'interno dell'azione viene restituito [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span><span class="sxs-lookup"><span data-stu-id="c73be-198">The preceding action returns `ActionResult<T>`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span></span> <span data-ttu-id="c73be-199">Poiché il controller è decorato con l'attributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute), è anche possibile una risposta [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="c73be-199">Since the controller is decorated with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute, a [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) response is possible too.</span></span> <span data-ttu-id="c73be-200">Vedere [Risposte HTTP 400 automatiche](xref:web-api/index#automatic-http-400-responses) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="c73be-200">See [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses) for more info.</span></span> <span data-ttu-id="c73be-201">Le annotazioni dei dati vengono usate per indicare ai client i codici di stato HTTP restituiti da questa azione.</span><span class="sxs-lookup"><span data-stu-id="c73be-201">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="c73be-202">Decorare l'azione con gli attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c73be-202">Decorate the action with the following attributes:</span></span>

<span data-ttu-id="c73be-203">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span><span class="sxs-lookup"><span data-stu-id="c73be-203">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span></span>
::: moniker-end

<span data-ttu-id="c73be-204">Il generatore di Swagger ora può descrivere accuratamente questa azione e i client generati conoscono la risposta che riceveranno quando si chiamerà l'endpoint.</span><span class="sxs-lookup"><span data-stu-id="c73be-204">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="c73be-205">È consigliabile la decorazione di tutte le azioni con questi attributi.</span><span class="sxs-lookup"><span data-stu-id="c73be-205">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="c73be-206">Per le linee guida sulle risposte HTTP restituite dalle azioni API, vedere la [specifica RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="c73be-206">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
