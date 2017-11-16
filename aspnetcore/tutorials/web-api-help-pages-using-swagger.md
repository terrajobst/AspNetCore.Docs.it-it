---
title: Pagine della Guida dell'API Web ASP.NET Core con Swagger
author: spboyer
description: In questa esercitazione viene descritta una procedura dettagliata per aggiungere Swagger per generare la documentazione e le pagine della Guida di un'applicazione API Web.
keywords: ASP.NET Core,Swagger,Swashbuckle,pagine Guida,API Web
ms.author: spboyer
manager: wpickett
ms.date: 09/01/2017
ms.topic: article
ms.assetid: 54bb961d-29d9-4dee-8e2c-a93fc33c16f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 8a87972a7394246ece2af3485d93739975ba5383
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-core-web-api-help-pages-using-swagger"></a><span data-ttu-id="7b06e-104">Pagine della Guida dell'API Web ASP.NET Core con Swagger</span><span class="sxs-lookup"><span data-stu-id="7b06e-104">ASP.NET Core Web API Help Pages using Swagger</span></span>

<a name="web-api-help-pages-using-swagger"></a>

<span data-ttu-id="7b06e-105">Di [Shayne Boyer](https://twitter.com/spboyer) e [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="7b06e-105">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="7b06e-106">Capire i diversi metodi di un'API può risultare difficile per gli sviluppatori che compilano applicazioni a consumo elevato.</span><span class="sxs-lookup"><span data-stu-id="7b06e-106">Understanding the various methods of an API can be a challenge for a developer when building a consuming application.</span></span>

<span data-ttu-id="7b06e-107">La generazione di pagine di Guida e di una documentazione valide per l'API Web tramite [Swagger](https://swagger.io/) con l'implementazione di .NET Core [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), è facile come aggiungere un paio di pacchetti NuGet e modificare *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="7b06e-107">Generating good documentation and help pages for your Web API, using [Swagger](https://swagger.io/) with the .NET Core implementation [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), is as easy as adding a couple of NuGet packages and modifying the *Startup.cs*.</span></span>

* <span data-ttu-id="7b06e-108">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) è un progetto open source per la generazione di documenti Swagger per le API Web di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7b06e-108">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="7b06e-109">[Swagger](https://swagger.io/) è una rappresentazione leggibile al computer di un'API RESTful che consente il supporto per la documentazione interattiva, la generazione di client SDK e l'esposizione al rilevamento.</span><span class="sxs-lookup"><span data-stu-id="7b06e-109">[Swagger](https://swagger.io/) is a machine-readable representation of a RESTful API that enables support for interactive documentation, client SDK generation, and discoverability.</span></span>

<span data-ttu-id="7b06e-110">Questa esercitazione si basa sull'esempio su [Building Your First Web API with ASP.NET Core MVC and Visual Studio](xref:tutorials/first-web-api) (Compilazione di un'API Web con ASP.NET Core MVC e Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="7b06e-110">This tutorial builds on the sample on [Building Your First Web API with ASP.NET Core MVC and Visual Studio](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="7b06e-111">Se si vuole proseguire, scaricare l'esempio all'indirizzo [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span><span class="sxs-lookup"><span data-stu-id="7b06e-111">If you'd like to follow along, download the sample at [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span></span>

## <a name="getting-started"></a><span data-ttu-id="7b06e-112">Introduzione</span><span class="sxs-lookup"><span data-stu-id="7b06e-112">Getting Started</span></span>

<span data-ttu-id="7b06e-113">Esistono tre componenti principali di Swashbuckle:</span><span class="sxs-lookup"><span data-stu-id="7b06e-113">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="7b06e-114">`Swashbuckle.AspNetCore.Swagger`: un modello a oggetti Swagger e middleware per esporre oggetti `SwaggerDocument` come endpoint JSON.</span><span class="sxs-lookup"><span data-stu-id="7b06e-114">`Swashbuckle.AspNetCore.Swagger`: a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="7b06e-115">`Swashbuckle.AspNetCore.SwaggerGen`: un generatore Swagger che compila oggetti `SwaggerDocument` direttamente da route, controller e modelli.</span><span class="sxs-lookup"><span data-stu-id="7b06e-115">`Swashbuckle.AspNetCore.SwaggerGen`: a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="7b06e-116">È in genere combinato con il middleware di endpoint Swagger per esporre automaticamente il JSON Swagger.</span><span class="sxs-lookup"><span data-stu-id="7b06e-116">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="7b06e-117">`Swashbuckle.AspNetCore.SwaggerUI`: una versione incorporata dello strumento interfaccia utente di Swagger che interpreta il JSON Swagger per descrivere in modo completo e personalizzabile la funzionalità dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="7b06e-117">`Swashbuckle.AspNetCore.SwaggerUI`: an embedded version of the Swagger UI tool which interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="7b06e-118">Include test harness predefiniti per i metodi pubblici.</span><span class="sxs-lookup"><span data-stu-id="7b06e-118">It includes built-in test harnesses for the public methods.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="7b06e-119">Pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="7b06e-119">NuGet Packages</span></span>

<span data-ttu-id="7b06e-120">È possibile aggiungere Swashbuckle con gli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="7b06e-120">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7b06e-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7b06e-121">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7b06e-122">Dalla finestra **Console di Gestione pacchetti**:</span><span class="sxs-lookup"><span data-stu-id="7b06e-122">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="7b06e-123">Dalla finestra di dialogo **Gestisci pacchetti NuGet**:</span><span class="sxs-lookup"><span data-stu-id="7b06e-123">From the **Manage NuGet Packages** dialog:</span></span>

     * <span data-ttu-id="7b06e-124">Fare clic con il pulsante destro del mouse su **Esplora soluzioni** > **Gestisci pacchetti NuGet**</span><span class="sxs-lookup"><span data-stu-id="7b06e-124">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
     * <span data-ttu-id="7b06e-125">Impostare **Origine pacchetto** su "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="7b06e-125">Set the **Package source** to "nuget.org"</span></span>
     * <span data-ttu-id="7b06e-126">Immettere "Swashbuckle.AspNetCore" nella casella di ricerca</span><span class="sxs-lookup"><span data-stu-id="7b06e-126">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
     * <span data-ttu-id="7b06e-127">Selezionare il pacchetto "Swashbuckle.AspNetCore" dalla scheda **Sfoglia** e fare clic su **Installa**</span><span class="sxs-lookup"><span data-stu-id="7b06e-127">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7b06e-128">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="7b06e-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="7b06e-129">Fare clic su con il pulsante destro del mouse sulla cartella *Pacchetti* in **Solution Pad** (Riquadro della soluzione) > **Aggiungi pacchetti**</span><span class="sxs-lookup"><span data-stu-id="7b06e-129">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="7b06e-130">Impostare l'elenco a discesa **Aggiungi pacchetti** della finestra **Origine** su "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="7b06e-130">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="7b06e-131">Immettere Swashbuckle.AspNetCore nella casella di ricerca</span><span class="sxs-lookup"><span data-stu-id="7b06e-131">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="7b06e-132">Selezionare il pacchetto Swashbuckle.AspNetCore dal riquadro dei risultati e fare clic su **Aggiungi pacchetto**</span><span class="sxs-lookup"><span data-stu-id="7b06e-132">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7b06e-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7b06e-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7b06e-134">Eseguire il comando seguente da **Terminale integrato**:</span><span class="sxs-lookup"><span data-stu-id="7b06e-134">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7b06e-135">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="7b06e-135">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="7b06e-136">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="7b06e-136">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a><span data-ttu-id="7b06e-137">Aggiungere e configurare Swagger per il middleware</span><span class="sxs-lookup"><span data-stu-id="7b06e-137">Add and configure Swagger to the middleware</span></span>

<span data-ttu-id="7b06e-138">Aggiungere il generatore di Swagger alla raccolta di servizi nel metodo `ConfigureServices` di *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7b06e-138">Add the Swagger generator to the services collection in the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

<span data-ttu-id="7b06e-139">Aggiungere l'istruzione using seguente per la classe `Info`:</span><span class="sxs-lookup"><span data-stu-id="7b06e-139">Add the following using statement for the `Info` class:</span></span>

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

<span data-ttu-id="7b06e-140">Nel metodo `Configure` di *Startup.cs* abilitare il middleware per la gestione del documento JSON generato e l'interfaccia utente di Swagger:</span><span class="sxs-lookup"><span data-stu-id="7b06e-140">In the `Configure` method of *Startup.cs*, enable the middleware for serving the generated JSON document and the SwaggerUI:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="7b06e-141">Avviare l'app e passare a `http://localhost:<random_port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="7b06e-141">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="7b06e-142">Viene visualizzato il documento generato che descrive gli endpoint.</span><span class="sxs-lookup"><span data-stu-id="7b06e-142">The generated document describing the endpoints appears.</span></span>

<span data-ttu-id="7b06e-143">**Nota:** Microsoft Edge, Google Chrome e Firefox visualizzano i documenti JSON in modo nativo.</span><span class="sxs-lookup"><span data-stu-id="7b06e-143">**Note:** Microsoft Edge, Google Chrome, and Firefox display JSON documents natively.</span></span> <span data-ttu-id="7b06e-144">Sono disponibili estensioni per Chrome che formattano il documento per agevolare la lettura.</span><span class="sxs-lookup"><span data-stu-id="7b06e-144">There are extensions for Chrome that format the document for easier reading.</span></span> <span data-ttu-id="7b06e-145">*L'esempio seguente viene ridotto per brevità.*</span><span class="sxs-lookup"><span data-stu-id="7b06e-145">*The following example is reduced for brevity.*</span></span>

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
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
   "securityDefinitions": {}
}
```

<span data-ttu-id="7b06e-146">Il documento attiva l'interfaccia utente di Swagger che è possibile visualizzare accedendo a `http://localhost:<random_port>/swagger`:</span><span class="sxs-lookup"><span data-stu-id="7b06e-146">This document drives the Swagger UI, which can be viewed by navigating to `http://localhost:<random_port>/swagger`:</span></span>

![Interfaccia utente di Swagger](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="7b06e-148">Ogni metodo di azione pubblico in `TodoController` può essere testato dall'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="7b06e-148">Each public action method in `TodoController` can be tested from the UI.</span></span> <span data-ttu-id="7b06e-149">Fare clic su un nome di metodo per espandere la sezione.</span><span class="sxs-lookup"><span data-stu-id="7b06e-149">Click a method name to expand the section.</span></span> <span data-ttu-id="7b06e-150">Aggiungere i parametri necessari e fare clic su "Try it out!" (Provalo!).</span><span class="sxs-lookup"><span data-stu-id="7b06e-150">Add any necessary parameters, and click "Try it out!".</span></span>

![Esempio test GET di Swagger](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customization--extensibility"></a><span data-ttu-id="7b06e-152">Personalizzazione ed estendibilità</span><span class="sxs-lookup"><span data-stu-id="7b06e-152">Customization & Extensibility</span></span>

<span data-ttu-id="7b06e-153">Swagger offre opzioni per documentare il modello a oggetti e personalizzare l'interfaccia utente in modo che corrisponda al tema.</span><span class="sxs-lookup"><span data-stu-id="7b06e-153">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="7b06e-154">Informazioni e descrizione API</span><span class="sxs-lookup"><span data-stu-id="7b06e-154">API Info and Description</span></span>

<span data-ttu-id="7b06e-155">L'azione di configurazione passata al metodo `AddSwaggerGen` può essere usata per aggiungere informazioni come ad esempio autore, licenza e descrizione:</span><span class="sxs-lookup"><span data-stu-id="7b06e-155">The configuration action passed to the `AddSwaggerGen` method can be used to add information such as the author, license, and description:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]

<span data-ttu-id="7b06e-156">Nella figura seguente viene illustrata l'interfaccia utente di Swagger che visualizza le informazioni sulla versione:</span><span class="sxs-lookup"><span data-stu-id="7b06e-156">The following image depicts the Swagger UI displaying the version information:</span></span>

![Interfaccia utente di Swagger con informazioni sulla versione: descrizione, autore e altri collegamenti](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="7b06e-158">Commenti XML</span><span class="sxs-lookup"><span data-stu-id="7b06e-158">XML Comments</span></span>

<span data-ttu-id="7b06e-159">I commenti XML possono essere abilitati con gli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="7b06e-159">XML comments can be enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7b06e-160">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7b06e-160">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7b06e-161">Fare clic con il pulsante destro del mouse in **Esplora soluzioni** e selezionare **Proprietà**</span><span class="sxs-lookup"><span data-stu-id="7b06e-161">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="7b06e-162">Selezionare la casella di controllo **File di documentazione XML** nella sezione **Output** della scheda **Compilazione**:</span><span class="sxs-lookup"><span data-stu-id="7b06e-162">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Scheda Compilazione delle proprietà del progetto](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7b06e-164">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="7b06e-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="7b06e-165">Aprire la finestra di dialogo **Opzioni progetto** > **Compila** > **Compilatore**</span><span class="sxs-lookup"><span data-stu-id="7b06e-165">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="7b06e-166">Selezionare la casella di controllo **Genera la documentazione XML** nella sezione **Opzioni generali**:</span><span class="sxs-lookup"><span data-stu-id="7b06e-166">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Sezione Opzioni generali delle opzioni del progetto](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7b06e-168">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7b06e-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7b06e-169">Aggiungere manualmente il frammento di codice seguente al file *csproj*:</span><span class="sxs-lookup"><span data-stu-id="7b06e-169">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]

---

<span data-ttu-id="7b06e-170">Configurare Swagger in modo che usi il file XML generato.</span><span class="sxs-lookup"><span data-stu-id="7b06e-170">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="7b06e-171">Per Linux o sistemi operativi diversi da Windows, i percorsi e i nomi di file possono fare distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="7b06e-171">For Linux or non-Windows operating systems, file names and paths can be case sensitive.</span></span> <span data-ttu-id="7b06e-172">Ad esempio, un file *ToDoApi.XML* potrebbe trovarsi in Windows ma non in CentOS.</span><span class="sxs-lookup"><span data-stu-id="7b06e-172">For example, a *ToDoApi.XML* file would be found on Windows but not CentOS.</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

<span data-ttu-id="7b06e-173">Nel codice precedente `ApplicationBasePath` ottiene il percorso base dell'app.</span><span class="sxs-lookup"><span data-stu-id="7b06e-173">In the preceding code, `ApplicationBasePath` gets the base path of the app.</span></span> <span data-ttu-id="7b06e-174">Il percorso base viene usato per individuare il file di commenti XML.</span><span class="sxs-lookup"><span data-stu-id="7b06e-174">The base path is used to locate the XML comments file.</span></span> <span data-ttu-id="7b06e-175">*TodoApi.xml* funziona solo per questo esempio, poiché il nome del file di commenti XML generato è basato sul nome dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7b06e-175">*TodoApi.xml* only works for this example, since the name of the generated XML comments file is based on the application name.</span></span>

<span data-ttu-id="7b06e-176">L'aggiunta al metodo di commenti a barra tripla migliora l'interfaccia utente di Swagger poiché viene aggiunta la descrizione all'intestazione della sezione:</span><span class="sxs-lookup"><span data-stu-id="7b06e-176">Adding the triple-slash comments to the method enhances the Swagger UI by adding the description to the section header:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![Interfaccia utente di Swagger che visualizza il commento XML "Deletes a specific TodoItem."](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="7b06e-179">L'interfaccia utente viene attivata dal file JSON generato, che contiene anche i commenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="7b06e-179">The UI is driven by the generated JSON file, which also contains these comments:</span></span>

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

<span data-ttu-id="7b06e-180">Aggiungere un tag [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) alla documentazione del metodo di azione `Create`.</span><span class="sxs-lookup"><span data-stu-id="7b06e-180">Add a [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) tag to the `Create` action method documentation.</span></span> <span data-ttu-id="7b06e-181">In questo modo vengono integrate le informazioni specificate nel tag `<summary>` e l'interfaccia utente di Swagger risulta più affidabile.</span><span class="sxs-lookup"><span data-stu-id="7b06e-181">It supplements information specified in the `<summary>` tag and provides a more robust Swagger UI.</span></span> <span data-ttu-id="7b06e-182">Il contenuto del tag `<remarks>` può essere costituito da testo, JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="7b06e-182">The `<remarks>` tag content can consist of text, JSON, or XML.</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

<span data-ttu-id="7b06e-183">Si notino i miglioramenti dell'interfaccia utente con questi commenti aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="7b06e-183">Notice the UI enhancements with these additional comments.</span></span>

![Interfaccia utente di Swagger con commenti aggiuntivi visualizzati](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="7b06e-185">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="7b06e-185">Data Annotations</span></span>

<span data-ttu-id="7b06e-186">Complementare il modello con gli attributi, disponibili in `System.ComponentModel.DataAnnotations`, per attivare i componenti dell'interfaccia utente di Swagger.</span><span class="sxs-lookup"><span data-stu-id="7b06e-186">Decorate the model with attributes, found in `System.ComponentModel.DataAnnotations`, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="7b06e-187">Aggiungere l'attributo `[Required]` alla proprietà `Name` della classe `TodoItem`:</span><span class="sxs-lookup"><span data-stu-id="7b06e-187">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="7b06e-188">La presenza di questo attributo modifica il comportamento dell'interfaccia utente e lo schema JSON sottostante:</span><span class="sxs-lookup"><span data-stu-id="7b06e-188">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="7b06e-189">Aggiungere l'attributo `[Produces("application/json")]` al controller API.</span><span class="sxs-lookup"><span data-stu-id="7b06e-189">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="7b06e-190">Il suo scopo consiste nel dichiarare che le azioni del controller supportano un tipo di contenuto restituito *application/json*:</span><span class="sxs-lookup"><span data-stu-id="7b06e-190">Its purpose is to declare that the controller's actions support a return a content type of *application/json*:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

<span data-ttu-id="7b06e-191">L'elenco a discesa **Response Content Type** (Tipo di contenuto risposta) include questo tipo di contenuto come selezione predefinita per le azioni GET del controller:</span><span class="sxs-lookup"><span data-stu-id="7b06e-191">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Interfaccia utente di Swagger con tipo di contenuto di risposta predefinito](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="7b06e-193">Con l'aumento dell'uso delle annotazioni dei dati nell'API Web, le pagine della Guida dell'API e dell'interfaccia utente diventano più descrittive e utili.</span><span class="sxs-lookup"><span data-stu-id="7b06e-193">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describing-response-types"></a><span data-ttu-id="7b06e-194">Descrizione dei tipi di risposta</span><span class="sxs-lookup"><span data-stu-id="7b06e-194">Describing Response Types</span></span>

<span data-ttu-id="7b06e-195">Per gli sviluppatori delle applicazioni a consumo elevato è più importante ciò che viene restituito, in particolare i tipi di risposta e i codici di errore, se diversi da quelli standard.</span><span class="sxs-lookup"><span data-stu-id="7b06e-195">Consuming developers are most concerned with what is returned &mdash; specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="7b06e-196">Questi vengono gestiti nei commenti XML e nelle annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="7b06e-196">These are handled in the XML comments and data annotations.</span></span>

<span data-ttu-id="7b06e-197">L'azione `Create` restituisce `201 Created` in caso di esito positivo o `400 Bad Request` quando il corpo della richiesta inviata è Null.</span><span class="sxs-lookup"><span data-stu-id="7b06e-197">The `Create` action returns `201 Created` on success or `400 Bad Request` when the posted request body is null.</span></span> <span data-ttu-id="7b06e-198">Senza la documentazione appropriata nell'interfaccia utente di Swagger, il consumer non riconosce tali risultati previsti.</span><span class="sxs-lookup"><span data-stu-id="7b06e-198">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="7b06e-199">Il problema si risolve aggiungendo le righe evidenziate nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="7b06e-199">That problem is fixed by adding the highlighted lines in the following example:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

<span data-ttu-id="7b06e-200">L'interfaccia utente di Swagger ora documenta chiaramente i codici di risposta HTTP previsti:</span><span class="sxs-lookup"><span data-stu-id="7b06e-200">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Interfaccia utente di Swagger che visualizza la descrizione della classe risposta POST "Returns the newly created Todo item" e "400 - If the item is null" per il codice di stato e il motivo nei messaggi di risposta](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customizing-the-ui"></a><span data-ttu-id="7b06e-202">Personalizzazione dell'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="7b06e-202">Customizing the UI</span></span>

<span data-ttu-id="7b06e-203">L'interfaccia utente è funzionale e presentabile ma, quando si compilano pagine della documentazione per l'API, è necessario che rappresenti il proprio marchio o tema.</span><span class="sxs-lookup"><span data-stu-id="7b06e-203">The stock UI is both functional and presentable; however, when building documentation pages for your API, you want it to represent your brand or theme.</span></span> <span data-ttu-id="7b06e-204">Per eseguire questa attività con i componenti Swashbuckle è necessario aggiungere le risorse per gestire i file statici e quindi compilare la struttura di cartelle per ospitare i file.</span><span class="sxs-lookup"><span data-stu-id="7b06e-204">Accomplishing that task with the Swashbuckle components requires adding the resources to serve static files and then building the folder structure to host those files.</span></span>

<span data-ttu-id="7b06e-205">Se la destinazione è .NET Framework, aggiungere il pacchetto NuGet `Microsoft.AspNetCore.StaticFiles` al progetto:</span><span class="sxs-lookup"><span data-stu-id="7b06e-205">If targeting .NET Framework, add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="7b06e-206">Abilitare il middleware dei file statici:</span><span class="sxs-lookup"><span data-stu-id="7b06e-206">Enable the static files middleware:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="7b06e-207">Acquisire il contenuto della cartella *dist* dall'[archivio dell'interfaccia utente di Swagger di GitHub](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span><span class="sxs-lookup"><span data-stu-id="7b06e-207">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span></span> <span data-ttu-id="7b06e-208">La cartella contiene le risorse necessarie per la pagina dell'interfaccia utente di Swagger.</span><span class="sxs-lookup"><span data-stu-id="7b06e-208">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="7b06e-209">Creare una cartella *wwwroot/swagger/ui* e copiarvi il contenuto della cartella *dist*.</span><span class="sxs-lookup"><span data-stu-id="7b06e-209">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="7b06e-210">Creare un file *wwwroot/swagger/ui/css/custom.css* con il CSS riportato di seguito per personalizzare l'intestazione della pagina:</span><span class="sxs-lookup"><span data-stu-id="7b06e-210">Create a *wwwroot/swagger/ui/css/custom.css* file with the following CSS to customize the page header:</span></span>

[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]

<span data-ttu-id="7b06e-211">Creare il riferimento a *custom.css* nel file *index.html*:</span><span class="sxs-lookup"><span data-stu-id="7b06e-211">Reference *custom.css* in the *index.html* file:</span></span>

[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]

<span data-ttu-id="7b06e-212">Passare alla pagina *index.html* all'indirizzo `http://localhost:<random_port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="7b06e-212">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="7b06e-213">Immettere `http://localhost:<random_port>/swagger/v1/swagger.json` nella casella di testo dell'intestazione e scegliere il pulsante **Explore** (Esplora).</span><span class="sxs-lookup"><span data-stu-id="7b06e-213">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="7b06e-214">La pagina risultante è simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="7b06e-214">The resulting page looks as follows:</span></span>

![Interfaccia utente di Swagger con il titolo dell'intestazione personalizzato](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="7b06e-216">Si può fare molto altro con la pagina.</span><span class="sxs-lookup"><span data-stu-id="7b06e-216">There is much more you can do with the page.</span></span> <span data-ttu-id="7b06e-217">Per vedere le funzionalità complete per le risorse dell'interfaccia utente, accedere all'[archivio dell'interfaccia utente di Swagger di GitHub](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="7b06e-217">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
