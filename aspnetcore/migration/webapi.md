---
title: Eseguire la migrazione da API Web ASP.NET ad ASP.NET Core
author: ardalis
description: Informazioni su come eseguire la migrazione di un'implementazione di API web dall'API Web ASP.NET 4.x ad ASP.NET Core MVC.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/10/2018
uid: migration/webapi
ms.openlocfilehash: 9806c502f8f5244740f9f9614657a40cfaa03314
ms.sourcegitcommit: 1872d2e6f299093c78a6795a486929ffb0bbffff
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/11/2018
ms.locfileid: "53216833"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="cbf07-103">Eseguire la migrazione da API Web ASP.NET ad ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cbf07-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="cbf07-104">Dal [Scott Addie](https://twitter.com/scott_addie) e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="cbf07-104">By [Scott Addie](https://twitter.com/scott_addie) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="cbf07-105">Un'API Web ASP.NET 4.x è un servizio HTTP che raggiunge un'ampia gamma di client, inclusi browser e dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="cbf07-105">An ASP.NET 4.x Web API is an HTTP service that reaches a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="cbf07-106">Unifica di ASP.NET Core MVC di ASP.NET del 4.x e i modelli di app per le API Web in un modello di programmazione più semplice, noto come ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="cbf07-106">ASP.NET Core unifies ASP.NET 4.x's MVC and Web API app models into a simpler programming model known as ASP.NET Core MVC.</span></span> <span data-ttu-id="cbf07-107">Questo articolo illustra i passaggi necessari per eseguire la migrazione dall'API Web ASP.NET 4.x ad ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="cbf07-107">This article demonstrates the steps required to migrate from ASP.NET 4.x Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="cbf07-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cbf07-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cbf07-109">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="cbf07-109">Prerequisites</span></span>

[!INCLUDE [net-core-prereqs-vs-2.2](../includes/net-core-prereqs-vs-2.2.md)]

## <a name="review-aspnet-4x-web-api-project"></a><span data-ttu-id="cbf07-110">Esaminare progetto API Web ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="cbf07-110">Review ASP.NET 4.x Web API project</span></span>

<span data-ttu-id="cbf07-111">Come punto di partenza, questo articolo usa il *ProductsApp* al progetto creato in [Introduzione a ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span><span class="sxs-lookup"><span data-stu-id="cbf07-111">As a starting point, this article uses the *ProductsApp* project created in [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span></span> <span data-ttu-id="cbf07-112">Nel progetto, un semplice progetto di API Web ASP.NET 4.x è configurato come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="cbf07-112">In that project, a simple ASP.NET 4.x Web API project is configured as follows.</span></span>

<span data-ttu-id="cbf07-113">Nelle *Global.asax.cs*, viene effettuata una chiamata a `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="cbf07-113">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="cbf07-114">Il `WebApiConfig` classe è reperibile nella *App_Start* cartella e ha un valore statico `Register` metodo:</span><span class="sxs-lookup"><span data-stu-id="cbf07-114">The `WebApiConfig` class is found in the *App_Start* folder and has a static `Register` method:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs)]

<span data-ttu-id="cbf07-115">Questa classe consente di configurare [routing con attributi](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), anche se non è effettivamente utilizzato nel progetto.</span><span class="sxs-lookup"><span data-stu-id="cbf07-115">This class configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="cbf07-116">Configura inoltre la tabella di routing, che viene usata da ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="cbf07-116">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="cbf07-117">In questo caso, API Web ASP.NET 4.x prevede che gli URL in base al formato `/api/{controller}/{id}`, con `{id}` sia facoltativo.</span><span class="sxs-lookup"><span data-stu-id="cbf07-117">In this case, ASP.NET 4.x Web API expects URLs to match the format `/api/{controller}/{id}`, with `{id}` being optional.</span></span>

<span data-ttu-id="cbf07-118">Il *ProductsApp* progetto include un controller.</span><span class="sxs-lookup"><span data-stu-id="cbf07-118">The *ProductsApp* project includes one controller.</span></span> <span data-ttu-id="cbf07-119">Il controller eredita da `ApiController` e contiene due azioni:</span><span class="sxs-lookup"><span data-stu-id="cbf07-119">The controller inherits from `ApiController` and contains two actions:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=28,33)]

<span data-ttu-id="cbf07-120">Il `Product` modello utilizzato da `ProductsController` è una semplice classe:</span><span class="sxs-lookup"><span data-stu-id="cbf07-120">The `Product` model used by `ProductsController` is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="cbf07-121">Le sezioni seguenti illustrano la migrazione del progetto API Web ad ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="cbf07-121">The following sections demonstrate migration of the Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-destination-project"></a><span data-ttu-id="cbf07-122">Creare il progetto di destinazione</span><span class="sxs-lookup"><span data-stu-id="cbf07-122">Create destination project</span></span>

<span data-ttu-id="cbf07-123">Completare i passaggi seguenti in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="cbf07-123">Complete the following steps in Visual Studio:</span></span>

* <span data-ttu-id="cbf07-124">Passare a **File** > **nuova** > **progetto** > **altri tipi di progetto**  >  **Soluzioni di visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="cbf07-124">Go to **File** > **New** > **Project** > **Other Project Types** > **Visual Studio Solutions**.</span></span> <span data-ttu-id="cbf07-125">Selezionare **soluzione vuota**e denominare la soluzione *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="cbf07-125">Select **Blank Solution**, and name the solution *WebAPIMigration*.</span></span> <span data-ttu-id="cbf07-126">Scegliere il **OK** pulsante.</span><span class="sxs-lookup"><span data-stu-id="cbf07-126">Click the **OK** button.</span></span>
* <span data-ttu-id="cbf07-127">Aggiungi esistenti *ProductsApp* progetto alla soluzione.</span><span class="sxs-lookup"><span data-stu-id="cbf07-127">Add the existing *ProductsApp* project to the solution.</span></span>
* <span data-ttu-id="cbf07-128">Aggiungere un nuovo **applicazione Web ASP.NET Core** progetto alla soluzione.</span><span class="sxs-lookup"><span data-stu-id="cbf07-128">Add a new **ASP.NET Core Web Application** project to the solution.</span></span> <span data-ttu-id="cbf07-129">Selezionare il **.NET Core** destinati a framework dall'elenco a discesa e selezionare il **API** modello di progetto.</span><span class="sxs-lookup"><span data-stu-id="cbf07-129">Select the **.NET Core** target framework from the drop-down, and select the **API** project template.</span></span> <span data-ttu-id="cbf07-130">Denominare il progetto *ProductsCore*, fare clic sui **OK** pulsante.</span><span class="sxs-lookup"><span data-stu-id="cbf07-130">Name the project *ProductsCore*, and click the **OK** button.</span></span>

<span data-ttu-id="cbf07-131">A questo punto, la soluzione contiene due progetti.</span><span class="sxs-lookup"><span data-stu-id="cbf07-131">The solution now contains two projects.</span></span> <span data-ttu-id="cbf07-132">Le sezioni seguenti illustrano la migrazione di *ProductsApp* contenuto del progetto per il *ProductsCore* progetto.</span><span class="sxs-lookup"><span data-stu-id="cbf07-132">The following sections explain migrating the *ProductsApp* project's contents to the *ProductsCore* project.</span></span>

## <a name="migrate-configuration"></a><span data-ttu-id="cbf07-133">La migrazione della configurazione</span><span class="sxs-lookup"><span data-stu-id="cbf07-133">Migrate configuration</span></span>

<span data-ttu-id="cbf07-134">ASP.NET Core non usa la *App_Start* cartella o il *Global. asax* file e il *Web. config* file viene aggiunto al momento della pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="cbf07-134">ASP.NET Core doesn't use the *App_Start* folder or the *Global.asax* file, and the *web.config* file is added at publish time.</span></span> <span data-ttu-id="cbf07-135">*Startup.cs* sostituisce la *Global. asax* e si trova nella radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="cbf07-135">*Startup.cs* is the replacement for *Global.asax* and is located in the project root.</span></span> <span data-ttu-id="cbf07-136">Il `Startup` classe gestisce tutte le attività di avvio di app.</span><span class="sxs-lookup"><span data-stu-id="cbf07-136">The `Startup` class handles all app startup tasks.</span></span> <span data-ttu-id="cbf07-137">Per altre informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="cbf07-137">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="cbf07-138">In ASP.NET Core MVC, il routing con attributi è incluso per impostazione predefinita quando <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> viene chiamato `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="cbf07-138">In ASP.NET Core MVC, attribute routing is included by default when <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> is called in `Startup.Configure`.</span></span> <span data-ttu-id="cbf07-139">Quanto segue `UseMvc` chiamare sostituisce il *ProductsApp* del progetto *App_Start/WebApiConfig.cs* file:</span><span class="sxs-lookup"><span data-stu-id="cbf07-139">The following `UseMvc` call replaces the *ProductsApp* project's *App_Start/WebApiConfig.cs* file:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="cbf07-140">Eseguire la migrazione di modelli e controller</span><span class="sxs-lookup"><span data-stu-id="cbf07-140">Migrate models and controllers</span></span>

<span data-ttu-id="cbf07-141">Copiare il *ProductApp* controller del progetto e il modello utilizzato.</span><span class="sxs-lookup"><span data-stu-id="cbf07-141">Copy over the *ProductApp* project's controller and the model it uses.</span></span> <span data-ttu-id="cbf07-142">Attenersi ai passaggi riportati di seguito.</span><span class="sxs-lookup"><span data-stu-id="cbf07-142">Follow these steps:</span></span>

1. <span data-ttu-id="cbf07-143">Copia *Controllers/ProductsController.cs* dal progetto originale a quello nuovo.</span><span class="sxs-lookup"><span data-stu-id="cbf07-143">Copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span>
1. <span data-ttu-id="cbf07-144">Copiare l'intera *modelli* cartella dal progetto originale a quello nuovo.</span><span class="sxs-lookup"><span data-stu-id="cbf07-144">Copy the entire *Models* folder from the original project to the new one.</span></span>
1. <span data-ttu-id="cbf07-145">Modificare gli spazi dei nomi i file copiati in modo che corrisponda il nuovo nome del progetto (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="cbf07-145">Change the copied files' namespaces to match the new project name (*ProductsCore*).</span></span> <span data-ttu-id="cbf07-146">Modificare il `using ProductsApp.Models;` istruzione nel *ProductsController.cs* troppo.</span><span class="sxs-lookup"><span data-stu-id="cbf07-146">Adjust the `using ProductsApp.Models;` statement in *ProductsController.cs* too.</span></span>

<span data-ttu-id="cbf07-147">Compilazione a questo punto, i risultati per le app in un numero di errori di compilazione.</span><span class="sxs-lookup"><span data-stu-id="cbf07-147">At this point, building the app results in a number of compilation errors.</span></span> <span data-ttu-id="cbf07-148">Gli errori si verificano perché i componenti seguenti non esistono in ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="cbf07-148">The errors occur because the following components don't exist in ASP.NET Core:</span></span>

* <span data-ttu-id="cbf07-149">Classe `ApiController`</span><span class="sxs-lookup"><span data-stu-id="cbf07-149">`ApiController` class</span></span>
* <span data-ttu-id="cbf07-150">Spazio dei nomi `System.Web.Http`</span><span class="sxs-lookup"><span data-stu-id="cbf07-150">`System.Web.Http` namespace</span></span>
* <span data-ttu-id="cbf07-151">`IHttpActionResult` Interfaccia</span><span class="sxs-lookup"><span data-stu-id="cbf07-151">`IHttpActionResult` interface</span></span>

<span data-ttu-id="cbf07-152">Correggere gli errori come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="cbf07-152">Fix the errors as follows:</span></span>

1. <span data-ttu-id="cbf07-153">Change `ApiController` a <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="cbf07-153">Change `ApiController` to <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="cbf07-154">Aggiungere `using Microsoft.AspNetCore.Mvc;` per risolvere il `ControllerBase` riferimento.</span><span class="sxs-lookup"><span data-stu-id="cbf07-154">Add `using Microsoft.AspNetCore.Mvc;` to resolve the `ControllerBase` reference.</span></span>
1. <span data-ttu-id="cbf07-155">Eliminare `using System.Web.Http;`.</span><span class="sxs-lookup"><span data-stu-id="cbf07-155">Delete `using System.Web.Http;`.</span></span>
1. <span data-ttu-id="cbf07-156">Modifica il `GetProduct` tipo restituito dell'azione dal `IHttpActionResult` a `ActionResult<Product>`.</span><span class="sxs-lookup"><span data-stu-id="cbf07-156">Change the `GetProduct` action's return type from `IHttpActionResult` to `ActionResult<Product>`.</span></span>

<span data-ttu-id="cbf07-157">Semplificare la `GetProduct` dell'azione `return` come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="cbf07-157">Simplify the `GetProduct` action's `return` statement to the following:</span></span>

```csharp
return product;
```

## <a name="configure-routing"></a><span data-ttu-id="cbf07-158">Configurare il routing</span><span class="sxs-lookup"><span data-stu-id="cbf07-158">Configure routing</span></span>

<span data-ttu-id="cbf07-159">Configurare il routing come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="cbf07-159">Configure routing as follows:</span></span>

1. <span data-ttu-id="cbf07-160">Decorare il `ProductsController` classe con gli attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="cbf07-160">Decorate the `ProductsController` class with the following attributes:</span></span>

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    <span data-ttu-id="cbf07-161">Precedente [[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) attributo Configura modello di routing di attributi del controller.</span><span class="sxs-lookup"><span data-stu-id="cbf07-161">The preceding [[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) attribute configures the controller's attribute routing pattern.</span></span> <span data-ttu-id="cbf07-162">Il [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attributo rende il routing di un requisito per tutte le azioni in questo controller di attributo.</span><span class="sxs-lookup"><span data-stu-id="cbf07-162">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute makes attribute routing a requirement for all actions in this controller.</span></span>

    <span data-ttu-id="cbf07-163">Routing con attributi supporta i token, ad esempio `[controller]` e `[action]`.</span><span class="sxs-lookup"><span data-stu-id="cbf07-163">Attribute routing supports tokens, such as `[controller]` and `[action]`.</span></span> <span data-ttu-id="cbf07-164">In fase di esecuzione, ogni token viene sostituito con il nome del controller o azione, rispettivamente, a cui è stato applicato l'attributo.</span><span class="sxs-lookup"><span data-stu-id="cbf07-164">At runtime, each token is replaced with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="cbf07-165">I token di riducono il numero di "stringhe magiche" nel progetto.</span><span class="sxs-lookup"><span data-stu-id="cbf07-165">The tokens reduce the number of magic strings in the project.</span></span> <span data-ttu-id="cbf07-166">I token assicurarsi anche le route rimangano sincronizzate con i corrispondenti controller e azioni quando refactoring di ridenominazione automatica da applicare.</span><span class="sxs-lookup"><span data-stu-id="cbf07-166">The tokens also ensure routes remain synchronized with the corresponding controllers and actions when automatic rename refactorings are applied.</span></span>
1. <span data-ttu-id="cbf07-167">Impostare la modalità di compatibilità del progetto in ASP.NET Core 2.2:</span><span class="sxs-lookup"><span data-stu-id="cbf07-167">Set the project's compatibility mode to ASP.NET Core 2.2:</span></span>

    [!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    <span data-ttu-id="cbf07-168">La modifica precedente:</span><span class="sxs-lookup"><span data-stu-id="cbf07-168">The preceding change:</span></span>

    * <span data-ttu-id="cbf07-169">È necessario per usare il `[ApiController]` attributo a livello di controller.</span><span class="sxs-lookup"><span data-stu-id="cbf07-169">Is required to use the `[ApiController]` attribute at the controller level.</span></span>
    * <span data-ttu-id="cbf07-170">Consente di partecipare a potenzialmente di rilievo introdotti in ASP.NET Core 2.2 comportamenti.</span><span class="sxs-lookup"><span data-stu-id="cbf07-170">Opts in to potentially breaking behaviors introduced in ASP.NET Core 2.2.</span></span>
1. <span data-ttu-id="cbf07-171">Abilitare le richieste HTTP Get per il `ProductController` azioni:</span><span class="sxs-lookup"><span data-stu-id="cbf07-171">Enable HTTP Get requests to the `ProductController` actions:</span></span>
    * <span data-ttu-id="cbf07-172">Si applicano i [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) dell'attributo di `GetAllProducts` azione.</span><span class="sxs-lookup"><span data-stu-id="cbf07-172">Apply the [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute to the `GetAllProducts` action.</span></span>
    * <span data-ttu-id="cbf07-173">Si applicano i `[HttpGet("{id}")]` dell'attributo di `GetProduct` azione.</span><span class="sxs-lookup"><span data-stu-id="cbf07-173">Apply the `[HttpGet("{id}")]` attribute to the `GetProduct` action.</span></span>

<span data-ttu-id="cbf07-174">Dopo le modifiche precedenti e la rimozione di inutilizzati `using` istruzioni *ProductsController.cs* file avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="cbf07-174">After the preceding changes and the removal of unused `using` statements, *ProductsController.cs* file looks like this:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

<span data-ttu-id="cbf07-175">Eseguire il progetto migrato e passare a `/api/products`.</span><span class="sxs-lookup"><span data-stu-id="cbf07-175">Run the migrated project, and browse to `/api/products`.</span></span> <span data-ttu-id="cbf07-176">Viene visualizzato un elenco completo delle tre prodotti.</span><span class="sxs-lookup"><span data-stu-id="cbf07-176">A full list of three products appears.</span></span> <span data-ttu-id="cbf07-177">Passare a `/api/products/1`.</span><span class="sxs-lookup"><span data-stu-id="cbf07-177">Browse to `/api/products/1`.</span></span> <span data-ttu-id="cbf07-178">Viene visualizzato il primo prodotto.</span><span class="sxs-lookup"><span data-stu-id="cbf07-178">The first product appears.</span></span>

## <a name="compatibility-shim"></a><span data-ttu-id="cbf07-179">Shim di compatibilità</span><span class="sxs-lookup"><span data-stu-id="cbf07-179">Compatibility shim</span></span>

<span data-ttu-id="cbf07-180">Il [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) libreria fornisce uno shim di compatibilità per spostare i progetti API Web ASP.NET 4.x ad ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cbf07-180">The [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library provides a compatibility shim to move ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="cbf07-181">Lo shim di compatibilità si estende a ASP.NET Core per supportare un numero di convenzioni da ASP.NET 4.x Web API 2.</span><span class="sxs-lookup"><span data-stu-id="cbf07-181">The compatibility shim extends ASP.NET Core to support a number of conventions from ASP.NET 4.x Web API 2.</span></span> <span data-ttu-id="cbf07-182">L'esempio trasferita in precedenza in questo documento è abbastanza semplice che non lo shim di compatibilità era necessario.</span><span class="sxs-lookup"><span data-stu-id="cbf07-182">The sample ported previously in this document is basic enough that the compatibility shim was unnecessary.</span></span> <span data-ttu-id="cbf07-183">Per progetti di grandi dimensioni usano lo shim di compatibilità può essere utile per temporaneamente colmare la lacuna API tra ASP.NET Core e ASP.NET 4.x Web API 2.</span><span class="sxs-lookup"><span data-stu-id="cbf07-183">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET 4.x Web API 2.</span></span>

<span data-ttu-id="cbf07-184">Lo shim di compatibilità delle API Web deve essere utilizzata come misura temporanea per supportare la migrazione grandi progetti ASP.NET 4.x API Web ad ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cbf07-184">The Web API compatibility shim is meant to be used as a temporary measure to support migrating large ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="cbf07-185">Nel corso del tempo, i progetti devono essere aggiornati per usare i modelli ASP.NET Core anziché basarsi sullo shim di compatibilità.</span><span class="sxs-lookup"><span data-stu-id="cbf07-185">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span>

<span data-ttu-id="cbf07-186">Le funzionalità di compatibilità inclusi in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` includono:</span><span class="sxs-lookup"><span data-stu-id="cbf07-186">Compatibility features included in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` include:</span></span>

* <span data-ttu-id="cbf07-187">Aggiunge un `ApiController` tipo in modo che i tipi di base di controller non devono essere aggiornati.</span><span class="sxs-lookup"><span data-stu-id="cbf07-187">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="cbf07-188">Consente l'associazione del modello basato su Web API.</span><span class="sxs-lookup"><span data-stu-id="cbf07-188">Enables Web API-style model binding.</span></span> <span data-ttu-id="cbf07-189">ASP.NET Core MVC modellare le funzioni di associazione in modo analogo a quello di ASP.NET 4.x MVC 5, per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="cbf07-189">ASP.NET Core MVC model binding functions similarly to that of ASP.NET 4.x MVC 5, by default.</span></span> <span data-ttu-id="cbf07-190">Le modifiche di shim di compatibilità del modello dell'associazione per essere più simile alle convenzioni dell'associazione del modello ASP.NET 4.x Web API 2.</span><span class="sxs-lookup"><span data-stu-id="cbf07-190">The compatibility shim changes model binding to be more similar to ASP.NET 4.x Web API 2 model binding conventions.</span></span> <span data-ttu-id="cbf07-191">Ad esempio, i tipi complessi vengono associati automaticamente dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="cbf07-191">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="cbf07-192">Estende l'associazione di modelli in modo che le azioni del controller possono accettare parametri di tipo `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="cbf07-192">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="cbf07-193">Aggiunge i formattatori dei messaggi che consente le azioni per restituire i risultati di tipo `HttpResponseMessage`.</span><span class="sxs-lookup"><span data-stu-id="cbf07-193">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="cbf07-194">Aggiunge i metodi di risposta aggiuntivi che le azioni di API Web 2 potrebbero essere utilizzati per gestire le risposte:</span><span class="sxs-lookup"><span data-stu-id="cbf07-194">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
  * <span data-ttu-id="cbf07-195">`HttpResponseMessage` generatori di:</span><span class="sxs-lookup"><span data-stu-id="cbf07-195">`HttpResponseMessage` generators:</span></span>
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * <span data-ttu-id="cbf07-196">Metodi di azione risultati:</span><span class="sxs-lookup"><span data-stu-id="cbf07-196">Action result methods:</span></span>
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* <span data-ttu-id="cbf07-197">Aggiunge un'istanza di `IContentNegotiator` all'app del contenitore di inserimento delle dipendenze e rende disponibili i tipi correlati alla negoziazione del contenuto da [ASPNET](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span><span class="sxs-lookup"><span data-stu-id="cbf07-197">Adds an instance of `IContentNegotiator` to the app's dependency injection (DI) container and makes available the content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span></span> <span data-ttu-id="cbf07-198">Esempi di tali tipi `DefaultContentNegotiator` e `MediaTypeFormatter`.</span><span class="sxs-lookup"><span data-stu-id="cbf07-198">Examples of such types include `DefaultContentNegotiator` and `MediaTypeFormatter`.</span></span>

<span data-ttu-id="cbf07-199">Per usare lo shim di compatibilità:</span><span class="sxs-lookup"><span data-stu-id="cbf07-199">To use the compatibility shim:</span></span>

1. <span data-ttu-id="cbf07-200">Installare il [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="cbf07-200">Install the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
1. <span data-ttu-id="cbf07-201">Registrazione servizi dello shim di compatibilità con contenitore di inserimento delle dipendenze dell'app tramite la chiamata `services.AddMvc().AddWebApiConventions()` in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="cbf07-201">Register the compatibility shim's services with the app's DI container by calling `services.AddMvc().AddWebApiConventions()` in `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="cbf07-202">Definire web specifici dell'API instrada usando `MapWebApiRoute` nella `IRouteBuilder` dell'app `IApplicationBuilder.UseMvc` chiamare.</span><span class="sxs-lookup"><span data-stu-id="cbf07-202">Define web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the app's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cbf07-203">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="cbf07-203">Additional resources</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
* <xref:mvc/compatibility-version>
