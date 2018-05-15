---
title: Introduzione a NSwag e ad ASP.NET Core
author: zuckerthoben
description: Informazioni su come usare NSwag per generare le pagine della documentazione e della Guida per un'app per le API di ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 80e6a9e1702d8f68d139d2ff9c3a01a27c40cecb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="3ad37-103">Introduzione a NSwag e ad ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3ad37-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="3ad37-104">Di [Christoph Nienaber](https://twitter.com/zuckerthoben) e [Rico Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="3ad37-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

<span data-ttu-id="3ad37-105">Per usare [NSwag](https://github.com/RSuter/NSwag) con il middleware di ASP.NET Core è necessario il pacchetto NuGet[NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/).</span><span class="sxs-lookup"><span data-stu-id="3ad37-105">Using [NSwag](https://github.com/RSuter/NSwag) with ASP.NET Core middleware requires the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="3ad37-106">Il pacchetto contiene un generatore di Swagger, l'interfaccia utente di Swagger (versione 2 e 3) e [l'interfaccia utente di ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="3ad37-106">The package consists of a Swagger generator, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="3ad37-107">È consigliabile usare le funzionalità di generazione del codice di NSwag.</span><span class="sxs-lookup"><span data-stu-id="3ad37-107">It's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="3ad37-108">Scegliere una delle opzioni seguenti per generare il codice:</span><span class="sxs-lookup"><span data-stu-id="3ad37-108">Choose one of the following options for code generation:</span></span>

* <span data-ttu-id="3ad37-109">Usare [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), un'applicazione desktop di Windows per generare il codice client in C# e TypeScript per l'API</span><span class="sxs-lookup"><span data-stu-id="3ad37-109">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API</span></span>
* <span data-ttu-id="3ad37-110">Usare i pacchetti NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) o [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) per generare il codice all'interno del progetto</span><span class="sxs-lookup"><span data-stu-id="3ad37-110">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project</span></span>
* <span data-ttu-id="3ad37-111">Usare NSwag dalla [riga di comando](https://github.com/NSwag/NSwag/wiki/CommandLine)</span><span class="sxs-lookup"><span data-stu-id="3ad37-111">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine)</span></span>
* <span data-ttu-id="3ad37-112">Usare il pacchetto NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild)</span><span class="sxs-lookup"><span data-stu-id="3ad37-112">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package</span></span>

## <a name="features"></a><span data-ttu-id="3ad37-113">Funzionalità</span><span class="sxs-lookup"><span data-stu-id="3ad37-113">Features</span></span>

<span data-ttu-id="3ad37-114">È consigliabile usare NSwag soprattutto perché consente non solo di introdurre l'interfaccia utente e il generatore di Swagger, ma di usare le funzionalità flessibili di generazione del codice.</span><span class="sxs-lookup"><span data-stu-id="3ad37-114">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="3ad37-115">Non occorre un'API esistente. È possibile usare le API di terze parti che includono Swagger e consentire a NSwag di generare un'implementazione client.</span><span class="sxs-lookup"><span data-stu-id="3ad37-115">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="3ad37-116">Il ciclo di sviluppo risulta in ogni caso velocizzato ed è facile adattarsi alle modifiche dell'API.</span><span class="sxs-lookup"><span data-stu-id="3ad37-116">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="3ad37-117">Installazione del pacchetto</span><span class="sxs-lookup"><span data-stu-id="3ad37-117">Package installation</span></span>

<span data-ttu-id="3ad37-118">È possibile aggiungere il pacchetto NuGet NSwag con gli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="3ad37-118">The NSwag NuGet package can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3ad37-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3ad37-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3ad37-120">Dalla finestra **Console di Gestione pacchetti**:</span><span class="sxs-lookup"><span data-stu-id="3ad37-120">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="3ad37-121">Dalla finestra di dialogo **Gestisci pacchetti NuGet**:</span><span class="sxs-lookup"><span data-stu-id="3ad37-121">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="3ad37-122">Fare clic con il pulsante destro del mouse su **Esplora soluzioni** > **Gestisci pacchetti NuGet**</span><span class="sxs-lookup"><span data-stu-id="3ad37-122">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="3ad37-123">Impostare **Origine pacchetto** su "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="3ad37-123">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="3ad37-124">Immettere "NSwag.AspNetCore" nella casella di ricerca</span><span class="sxs-lookup"><span data-stu-id="3ad37-124">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="3ad37-125">Selezionare il pacchetto "NSwag.AspNetCore" dalla scheda **Sfoglia** e fare clic su **Installa**</span><span class="sxs-lookup"><span data-stu-id="3ad37-125">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3ad37-126">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3ad37-126">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="3ad37-127">Fare clic su con il pulsante destro del mouse sulla cartella *Pacchetti* in **Solution Pad** (Riquadro della soluzione) > **Aggiungi pacchetti**</span><span class="sxs-lookup"><span data-stu-id="3ad37-127">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="3ad37-128">Impostare l'elenco a discesa **Aggiungi pacchetti** della finestra **Origine** su "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="3ad37-128">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="3ad37-129">Immettere NSwag.AspNetCore nella casella di ricerca</span><span class="sxs-lookup"><span data-stu-id="3ad37-129">Enter NSwag.AspNetCore in the search box</span></span>
* <span data-ttu-id="3ad37-130">Selezionare il pacchetto NSwag.AspNetCore dal riquadro dei risultati e fare clic su **Aggiungi pacchetto**</span><span class="sxs-lookup"><span data-stu-id="3ad37-130">Select the NSwag.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3ad37-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3ad37-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="3ad37-132">Eseguire il comando seguente da **Terminale integrato**:</span><span class="sxs-lookup"><span data-stu-id="3ad37-132">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3ad37-133">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="3ad37-133">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="3ad37-134">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="3ad37-134">Run the following command:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="3ad37-135">Aggiungere e configurare il middleware di Swagger</span><span class="sxs-lookup"><span data-stu-id="3ad37-135">Add and configure Swagger middleware</span></span>

<span data-ttu-id="3ad37-136">Importare gli spazi dei nomi seguenti nella classe `Info`:</span><span class="sxs-lookup"><span data-stu-id="3ad37-136">Import the following namespaces in the `Info` class:</span></span>

```csharp
using NSwag.AspNetCore;
using System.Reflection;
using NJsonSchema;
```

<span data-ttu-id="3ad37-137">Nel metodo `Startup.Configure` abilitare il middleware per la gestione della specifica generata e dell'interfaccia utente di Swagger:</span><span class="sxs-lookup"><span data-stu-id="3ad37-137">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-cs[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="3ad37-138">Avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="3ad37-138">Launch the app.</span></span> <span data-ttu-id="3ad37-139">Passare a `/swagger` per visualizzare l'interfaccia utente di Swagger.</span><span class="sxs-lookup"><span data-stu-id="3ad37-139">Navigate to `/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="3ad37-140">Passare a `/swagger/v1/swagger.json` per visualizzare la specifica di Swagger.</span><span class="sxs-lookup"><span data-stu-id="3ad37-140">Navigate to `/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="3ad37-141">Generazione del codice</span><span class="sxs-lookup"><span data-stu-id="3ad37-141">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="3ad37-142">Tramite NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="3ad37-142">Via NSwagStudio</span></span>

* <span data-ttu-id="3ad37-143">Installare `NSwagStudio` dal [repository GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio) ufficiale.</span><span class="sxs-lookup"><span data-stu-id="3ad37-143">Install `NSwagStudio` from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>

* <span data-ttu-id="3ad37-144">Tramite NSwagStudio.</span><span class="sxs-lookup"><span data-stu-id="3ad37-144">Launch NSwagStudio.</span></span> <span data-ttu-id="3ad37-145">Immettere il percorso del file *swagger.json* oppure copiarlo direttamente:</span><span class="sxs-lookup"><span data-stu-id="3ad37-145">Enter the location of your *swagger.json* or copy it directly:</span></span>

![NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio.png)

* <span data-ttu-id="3ad37-147">Indicare il tipo di output del client desiderato.</span><span class="sxs-lookup"><span data-stu-id="3ad37-147">Indicate the desired client output type.</span></span> <span data-ttu-id="3ad37-148">Le opzioni includono **Client TypeScript**, **Client CSharp** o **Controller API Web CSharp**.</span><span class="sxs-lookup"><span data-stu-id="3ad37-148">Options include **TypeScript Client**, **CSharp Client**, or **CSharp Web API Controller**.</span></span> <span data-ttu-id="3ad37-149">L'uso di un controller API Web è sostanzialmente una generazione inversa.</span><span class="sxs-lookup"><span data-stu-id="3ad37-149">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="3ad37-150">Viene usata una specifica di un servizio per rigenerare il servizio.</span><span class="sxs-lookup"><span data-stu-id="3ad37-150">It uses a specification of a service to rebuild the service.</span></span>

* <span data-ttu-id="3ad37-151">Fare clic su **Generate Outputs** (Genera output).</span><span class="sxs-lookup"><span data-stu-id="3ad37-151">Click **Generate Outputs**.</span></span>

* <span data-ttu-id="3ad37-152">Di seguito è possibile vedere un'implementazione client completa dell'esempio *TodoApi.NSwag* in C#:</span><span class="sxs-lookup"><span data-stu-id="3ad37-152">Here you see a complete client implementation of the *TodoApi.NSwag* sample in C#:</span></span>

![Output di NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio-Output.png)

* <span data-ttu-id="3ad37-154">Inserire il file in un progetto client, ad esempio un'app [Xamarin.Forms](/xamarin/xamarin-forms/).</span><span class="sxs-lookup"><span data-stu-id="3ad37-154">Put the file into a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span> <span data-ttu-id="3ad37-155">Iniziare a usare l'API:</span><span class="sxs-lookup"><span data-stu-id="3ad37-155">Start consuming the API:</span></span>

```csharp
var todoClient = new TodoClient();

// Gets all Todos from the Api
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem and save it in the Api
var createdTodo = await todoClient.CreateAsync(new TodoItem);

// Get a single Todo by Id
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> <span data-ttu-id="3ad37-156">È possibile inserire un URL di base e/o un client HTTP nel client dell'API.</span><span class="sxs-lookup"><span data-stu-id="3ad37-156">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="3ad37-157">È sempre consigliabile [riusare HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span><span class="sxs-lookup"><span data-stu-id="3ad37-157">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

<span data-ttu-id="3ad37-158">È ora possibile avviare facilmente l'implementazione dell'API nei progetti client.</span><span class="sxs-lookup"><span data-stu-id="3ad37-158">You can now start implementing your API into client projects easily.</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="3ad37-159">Altri modi per generare codice client</span><span class="sxs-lookup"><span data-stu-id="3ad37-159">Other ways to generate client code</span></span>

<span data-ttu-id="3ad37-160">È possibile generare il codice in altri modi, più adatti al flusso di lavoro:</span><span class="sxs-lookup"><span data-stu-id="3ad37-160">You can generate the code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="3ad37-161">MSBuild</span><span class="sxs-lookup"><span data-stu-id="3ad37-161">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="3ad37-162">Nel codice</span><span class="sxs-lookup"><span data-stu-id="3ad37-162">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="3ad37-163">Modelli T4</span><span class="sxs-lookup"><span data-stu-id="3ad37-163">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customization"></a><span data-ttu-id="3ad37-164">Personalizzazione</span><span class="sxs-lookup"><span data-stu-id="3ad37-164">Customization</span></span>

### <a name="xml-comments"></a><span data-ttu-id="3ad37-165">XML (commenti)</span><span class="sxs-lookup"><span data-stu-id="3ad37-165">XML comments</span></span>

<span data-ttu-id="3ad37-166">I commenti XML vengono abilitati con gli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="3ad37-166">XML comments are enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="3ad37-167">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3ad37-167">Visual Studio</span></span>](#tab/visual-studio-xml/)
* <span data-ttu-id="3ad37-168">Fare clic con il pulsante destro del mouse in **Esplora soluzioni** e selezionare **Proprietà**</span><span class="sxs-lookup"><span data-stu-id="3ad37-168">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="3ad37-169">Selezionare la casella di controllo **File di documentazione XML** nella sezione **Output** della scheda **Compilazione**:</span><span class="sxs-lookup"><span data-stu-id="3ad37-169">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Scheda Compilazione delle proprietà del progetto](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="3ad37-171">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3ad37-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)
* <span data-ttu-id="3ad37-172">Aprire la finestra di dialogo **Opzioni progetto** > **Compila** > **Compilatore**</span><span class="sxs-lookup"><span data-stu-id="3ad37-172">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="3ad37-173">Selezionare la casella di controllo **Genera la documentazione XML** nella sezione **Opzioni generali**:</span><span class="sxs-lookup"><span data-stu-id="3ad37-173">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Sezione Opzioni generali delle opzioni del progetto](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="3ad37-175">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3ad37-175">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)
<span data-ttu-id="3ad37-176">Aggiungere manualmente il frammento di codice seguente al file *csproj*:</span><span class="sxs-lookup"><span data-stu-id="3ad37-176">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/TodoApiNSwag.csproj?range=7-9)]

* * *
## <a name="data-annotations"></a><span data-ttu-id="3ad37-177">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="3ad37-177">Data annotations</span></span>

<span data-ttu-id="3ad37-178">NSwag usa la [reflection](/dotnet/csharp/programming-guide/concepts/reflection)Per le azioni API Web la procedura consigliata è la restituzione di [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="3ad37-178">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the best practice for Web API actions is to return [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="3ad37-179">Di conseguenza, non è possibile per NSwag dedurre l'azione e il valore restituito.</span><span class="sxs-lookup"><span data-stu-id="3ad37-179">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="3ad37-180">Si consideri l'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="3ad37-180">Consider the following example:</span></span>

```csharp
public IActionResult Create([FromBody] TodoItem item)
{
    if (item == null)
    {
        return BadRequest();
    }

    _context.TodoItems.Add(item);
    _context.SaveChanges();

     return CreatedAtRoute("GetTodo", new { id = item.Id }, item);
}
```

<span data-ttu-id="3ad37-181">L'azione precedente restituisce `IActionResult`, ma all'interno dell'azione viene restituito [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) o [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span><span class="sxs-lookup"><span data-stu-id="3ad37-181">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="3ad37-182">Le annotazioni dei dati vengono usate per indicare ai client la risposta HTTP restituita da questa azione.</span><span class="sxs-lookup"><span data-stu-id="3ad37-182">Data annotations are used to tell clients which HTTP response this action is returning.</span></span> <span data-ttu-id="3ad37-183">Decorare l'azione con gli attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="3ad37-183">Decorate the action with the following attributes:</span></span>

```csharp
[HttpPost]
[ProducesResponseType(typeof(TodoItem), 201)] // Created
[ProducesResponseType(typeof(TodoItem), 400)] // BadRequest
public IActionResult Create([FromBody] TodoItem item)
```

<span data-ttu-id="3ad37-184">Il generatore di Swagger ora può descrivere accuratamente questa azione e i client generati conoscono la risposta che riceveranno quando si chiamerà l'endpoint.</span><span class="sxs-lookup"><span data-stu-id="3ad37-184">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="3ad37-185">È consigliabile la decorazione di tutte le azioni con questi attributi.</span><span class="sxs-lookup"><span data-stu-id="3ad37-185">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="3ad37-186">Per le linee guida sulle risposte HTTP restituite dalle azioni API, vedere la [specifica RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="3ad37-186">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
