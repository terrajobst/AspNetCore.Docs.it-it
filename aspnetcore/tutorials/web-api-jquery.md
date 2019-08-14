---
title: "Esercitazione: Chiamare un'app API Web con jQuery usando ASP.NET Core"
author: rick-anderson
description: Informazioni su come chiamare un'app API Web ASP.NET Core con jQuery.
ms.author: riande
ms.custom: mvc
ms.date: 07/20/2019
uid: tutorials/web-api-jquery
ms.openlocfilehash: eb8b2453fd037170a49f531fea4c3ef1c056292d
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602570"
---
# <a name="tutorial-call-an-aspnet-core-web-api-with-jquery"></a><span data-ttu-id="efab6-103">Esercitazione: Chiamare un'app API Web ASP.NET Core con jQuery</span><span class="sxs-lookup"><span data-stu-id="efab6-103">Tutorial: Call an ASP.NET Core web API with jQuery</span></span>

<span data-ttu-id="efab6-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="efab6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="efab6-105">Questa esercitazione illustra come chiamare un'app API Web ASP.NET Core con jQuery</span><span class="sxs-lookup"><span data-stu-id="efab6-105">This tutorial shows how to call an ASP.NET Core web API with jQuery</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="efab6-106">Per ASP.NET Core 2.2, vedere la versione 2.2 di [Chiamare l'API Web con jQuery](xref:tutorials/first-web-api#call-the-api-with-jquery).</span><span class="sxs-lookup"><span data-stu-id="efab6-106">For ASP.NET Core 2.2, see the 2.2 version of [Call the Web API with jQuery](xref:tutorials/first-web-api#call-the-api-with-jquery).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="efab6-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="efab6-107">Prerequisites</span></span>

<span data-ttu-id="efab6-108">Completare [Esercitazione: Creare un'API Web](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="efab6-108">Complete [Tutorial: Create a web API](xref:tutorials/first-web-api)</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="efab6-109">Chiamare l'API con jQuery</span><span class="sxs-lookup"><span data-stu-id="efab6-109">Call the API with jQuery</span></span>

<span data-ttu-id="efab6-110">In questa sezione viene aggiunta una pagina HTML che usa jQuery per chiamare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="efab6-110">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="efab6-111">jQuery avvia la richiesta e aggiorna la pagina con i dettagli ottenuti dalla risposta dell'API.</span><span class="sxs-lookup"><span data-stu-id="efab6-111">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="efab6-112">Configurare l'app per [servire file statici](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [abilitare il mapping del file predefinito](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) aggiornando *Startup.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="efab6-112">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/StartupJquery.cs?highlight=8-9&name=snippet_configure)]

<span data-ttu-id="efab6-113">Creare una cartella *wwwroot* nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="efab6-113">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="efab6-114">Aggiungere un file HTML denominato *index.html* alla directory *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="efab6-114">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="efab6-115">Sostituire il contenuto con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="efab6-115">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/3.0/TodoApi/wwwroot/index.html)]

<span data-ttu-id="efab6-116">Aggiungere un file JavaScript con nome *site.js* alla directory *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="efab6-116">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="efab6-117">Sostituire il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="efab6-117">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="efab6-118">Può essere necessario modificare le impostazioni di avvio del progetto ASP.NET Core per il test della pagina HTML in locale:</span><span class="sxs-lookup"><span data-stu-id="efab6-118">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="efab6-119">Aprire *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="efab6-119">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="efab6-120">Rimuovere la proprietà `launchUrl` per forzare l'app ad aprire *index.html*, il file predefinito del progetto.</span><span class="sxs-lookup"><span data-stu-id="efab6-120">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="efab6-121">È possibile ottenere jQuery in vari modi.</span><span class="sxs-lookup"><span data-stu-id="efab6-121">There are several ways to get jQuery.</span></span> <span data-ttu-id="efab6-122">Nel frammento precedente la libreria viene caricata da una rete CDN.</span><span class="sxs-lookup"><span data-stu-id="efab6-122">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="efab6-123">In questo esempio vengono chiamati tutti i metodi CRUD dell'API.</span><span class="sxs-lookup"><span data-stu-id="efab6-123">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="efab6-124">Di seguito sono incluse le spiegazioni delle chiamate all'API.</span><span class="sxs-lookup"><span data-stu-id="efab6-124">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="efab6-125">Ottenere un elenco di elementi attività</span><span class="sxs-lookup"><span data-stu-id="efab6-125">Get a list of to-do items</span></span>

<span data-ttu-id="efab6-126">La funzione [ajax](https://api.jquery.com/jquery.ajax/) di jQuery invia una richiesta `GET` all'API, la quale restituisce codice JSON che rappresenta una matrice di elementi attività.</span><span class="sxs-lookup"><span data-stu-id="efab6-126">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="efab6-127">La funzione di callback `success` viene chiamata se la richiesta ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="efab6-127">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="efab6-128">Nel callback il modello DOM viene aggiornato con le informazioni dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="efab6-128">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="efab6-129">Aggiungere un elemento attività</span><span class="sxs-lookup"><span data-stu-id="efab6-129">Add a to-do item</span></span>

<span data-ttu-id="efab6-130">La funzione [ajax](https://api.jquery.com/jquery.ajax/) invia una richiesta `POST` con l'elemento attività nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="efab6-130">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="efab6-131">Le opzioni `accepts` e `contentType` sono impostate su `application/json` per specificare il tipo di supporto ricevuto e inviato.</span><span class="sxs-lookup"><span data-stu-id="efab6-131">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="efab6-132">L'elemento attività viene convertito in JSON usando [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="efab6-132">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="efab6-133">Quando l'API restituisce un codice di stato di esecuzione riuscita, viene chiamata la funzione `getData` per aggiornare la tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="efab6-133">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="efab6-134">Aggiornare un elemento attività</span><span class="sxs-lookup"><span data-stu-id="efab6-134">Update a to-do item</span></span>

<span data-ttu-id="efab6-135">L'aggiornamento di un elemento attività è simile all'aggiunta di un elemento di questo tipo.</span><span class="sxs-lookup"><span data-stu-id="efab6-135">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="efab6-136">L'`url` cambia per includere l'identificatore univoco dell'elemento e `type` è `PUT`.</span><span class="sxs-lookup"><span data-stu-id="efab6-136">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="efab6-137">Eliminare un elemento attività</span><span class="sxs-lookup"><span data-stu-id="efab6-137">Delete a to-do item</span></span>

<span data-ttu-id="efab6-138">È possibile eliminare un elemento attività impostando `type` su `DELETE` nella chiamata AJAX e specificando l'identificatore univoco dell'elemento nell'URL.</span><span class="sxs-lookup"><span data-stu-id="efab6-138">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

<span data-ttu-id="efab6-139">Passare all'esercitazione successiva per apprendere come generare pagine della Guida dell'API:</span><span class="sxs-lookup"><span data-stu-id="efab6-139">Advance to the next tutorial to learn how to generate API help pages:</span></span>

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
::: moniker-end