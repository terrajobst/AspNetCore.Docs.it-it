---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Parte 4: Aggiunta di una vista di amministrazione | Documenti Microsoft'
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 2960eee37201655a9e4632bf0196ba18a0e2e82a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="part-4-adding-an-admin-view"></a><span data-ttu-id="de512-102">Parte 4: Aggiunta di una vista di amministrazione</span><span class="sxs-lookup"><span data-stu-id="de512-102">Part 4: Adding an Admin View</span></span>
====================
<span data-ttu-id="de512-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="de512-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="de512-104">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="de512-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a><span data-ttu-id="de512-105">Aggiungere una visualizzazione di amministrazione</span><span class="sxs-lookup"><span data-stu-id="de512-105">Add an Admin View</span></span>

<span data-ttu-id="de512-106">A questo punto verrà attiva al lato client e aggiungere una pagina che è possibile utilizzare i dati dal controller di amministrazione.</span><span class="sxs-lookup"><span data-stu-id="de512-106">Now we'll turn to the client side, and add a page that can consume data from the Admin controller.</span></span> <span data-ttu-id="de512-107">La pagina consentirà agli utenti di creare, modificare o eliminare i prodotti, inviando le richieste AJAX al controller.</span><span class="sxs-lookup"><span data-stu-id="de512-107">The page will allow users to create, edit, or delete products, by sending AJAX requests to the controller.</span></span>

<span data-ttu-id="de512-108">In Esplora soluzioni, espandere la cartella Controllers e aprire il file denominato HomeController. cs.</span><span class="sxs-lookup"><span data-stu-id="de512-108">In Solution Explorer, expand the Controllers folder and open the file named HomeController.cs.</span></span> <span data-ttu-id="de512-109">Questo file contiene un controller MVC.</span><span class="sxs-lookup"><span data-stu-id="de512-109">This file contains an MVC controller.</span></span> <span data-ttu-id="de512-110">Aggiungere un metodo denominato `Admin`:</span><span class="sxs-lookup"><span data-stu-id="de512-110">Add a method named `Admin`:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

<span data-ttu-id="de512-111">Il **HttpRouteUrl** metodo crea l'URI per l'API web e si archiviarlo nel contenitore delle visualizzazioni per un momento successivo.</span><span class="sxs-lookup"><span data-stu-id="de512-111">The **HttpRouteUrl** method creates the URI to the web API, and we store this in the view bag for later.</span></span>

<span data-ttu-id="de512-112">Successivamente, posizionare il cursore di testo all'interno di `Admin` metodo di azione, quindi fare clic e scegliere **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="de512-112">Next, position the text cursor within the `Admin` action method, then right-click and select **Add View**.</span></span> <span data-ttu-id="de512-113">Verrà visualizzata la **Aggiungi visualizzazione** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="de512-113">This will bring up the **Add View** dialog.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

<span data-ttu-id="de512-114">Nel **Aggiungi visualizzazione** finestra di dialogo, nome visualizzazione "Admin".</span><span class="sxs-lookup"><span data-stu-id="de512-114">In the **Add View** dialog, name the view "Admin".</span></span> <span data-ttu-id="de512-115">Selezionare la casella di controllo **creare una visualizzazione fortemente tipizzata**.</span><span class="sxs-lookup"><span data-stu-id="de512-115">Select the check box labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="de512-116">In **classe modello**, selezionare "Prodotto (ProductStore.Models)".</span><span class="sxs-lookup"><span data-stu-id="de512-116">Under **Model Class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="de512-117">Lasciare tutte le altre opzioni come valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="de512-117">Leave all the other options as their default values.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

<span data-ttu-id="de512-118">Fare clic su **Aggiungi** aggiunge un file denominato Admin.cshtml in Views/Home.</span><span class="sxs-lookup"><span data-stu-id="de512-118">Clicking **Add** adds a file named Admin.cshtml under Views/Home.</span></span> <span data-ttu-id="de512-119">Aprire il file e aggiungere il seguente codice HTML.</span><span class="sxs-lookup"><span data-stu-id="de512-119">Open this file and add the following HTML.</span></span> <span data-ttu-id="de512-120">Questo codice HTML definisce la struttura della pagina, ma non è associata alcuna funzionalità ancora.</span><span class="sxs-lookup"><span data-stu-id="de512-120">This HTML defines the structure of the page, but no functionality is wired up yet.</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a><span data-ttu-id="de512-121">Creare un collegamento alla pagina di amministrazione</span><span class="sxs-lookup"><span data-stu-id="de512-121">Create a Link to the Admin Page</span></span>

<span data-ttu-id="de512-122">In Esplora soluzioni, espandere la cartella Views e quindi espandere la cartella condivisa.</span><span class="sxs-lookup"><span data-stu-id="de512-122">In Solution Explorer, expand the Views folder and then expand the Shared folder.</span></span> <span data-ttu-id="de512-123">Aprire il file denominato \_cshtml.</span><span class="sxs-lookup"><span data-stu-id="de512-123">Open the file named \_Layout.cshtml.</span></span> <span data-ttu-id="de512-124">Individuare il **ul** elemento con id = "menu" e un collegamento all'azione per la visualizzazione di amministrazione:</span><span class="sxs-lookup"><span data-stu-id="de512-124">Locate the **ul** element with id = "menu", and an action link for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> <span data-ttu-id="de512-125">Nel progetto di esempio, apportate alcune altre modifiche cosmetici, ad esempio sostituendo la stringa "Inserire qui il logo".</span><span class="sxs-lookup"><span data-stu-id="de512-125">In the sample project, I made a few other cosmetic changes, such as replacing the string "Your logo here".</span></span> <span data-ttu-id="de512-126">Questi non influisce sulla funzionalità dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="de512-126">These don't affect the functionality of the application.</span></span> <span data-ttu-id="de512-127">È possibile scaricare il progetto e confrontare i file.</span><span class="sxs-lookup"><span data-stu-id="de512-127">You can download the project and compare the files.</span></span>


<span data-ttu-id="de512-128">Eseguire l'applicazione e fare clic sul collegamento "Admin" che viene visualizzato nella parte superiore della home page.</span><span class="sxs-lookup"><span data-stu-id="de512-128">Run the application and click the "Admin" link that appears at the top of the home page.</span></span> <span data-ttu-id="de512-129">Pagina di amministrazione di dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="de512-129">The Admin page should look like the following:</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

<span data-ttu-id="de512-130">Diritto a questo punto, la pagina non esegue alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="de512-130">Right now, the page doesn't do anything.</span></span> <span data-ttu-id="de512-131">Nella sezione successiva, useremo Knockout.js per creare un'interfaccia utente dinamica.</span><span class="sxs-lookup"><span data-stu-id="de512-131">In the next section, we'll use Knockout.js to create a dynamic UI.</span></span>

## <a name="add-authorization"></a><span data-ttu-id="de512-132">Aggiungi autorizzazione</span><span class="sxs-lookup"><span data-stu-id="de512-132">Add Authorization</span></span>

<span data-ttu-id="de512-133">La pagina di amministrazione è attualmente accessibile a tutti gli utenti che visitano il sito.</span><span class="sxs-lookup"><span data-stu-id="de512-133">The Admin page is currently accessible to anyone visiting the site.</span></span> <span data-ttu-id="de512-134">È necessario modificare questa opzione per limitare le autorizzazioni per gli amministratori.</span><span class="sxs-lookup"><span data-stu-id="de512-134">Let's change this to restrict permission to administrators.</span></span>

<span data-ttu-id="de512-135">Per iniziare, aggiungere un ruolo "Administrator" e un utente amministratore.</span><span class="sxs-lookup"><span data-stu-id="de512-135">Start by adding an "Administrator" role and an administrator user.</span></span> <span data-ttu-id="de512-136">In Esplora soluzioni, espandere la cartella di filtri e aprire il file denominato InitializeSimpleMembershipAttribute.cs.</span><span class="sxs-lookup"><span data-stu-id="de512-136">In Solution Explorer, expand the Filters folder and open the file named InitializeSimpleMembershipAttribute.cs.</span></span> <span data-ttu-id="de512-137">Individuare il `SimpleMembershipInitializer` costruttore.</span><span class="sxs-lookup"><span data-stu-id="de512-137">Locate the `SimpleMembershipInitializer` constructor.</span></span> <span data-ttu-id="de512-138">Dopo la chiamata a **WebSecurity.InitializeDatabaseConnection**, aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="de512-138">After the call to **WebSecurity.InitializeDatabaseConnection**, add the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

<span data-ttu-id="de512-139">Questo è un modo rapido per aggiungere il ruolo "Administrator" e creare un utente per il ruolo.</span><span class="sxs-lookup"><span data-stu-id="de512-139">This is a quick-and-dirty way to add the "Administrator" role and create a user for the role.</span></span>

<span data-ttu-id="de512-140">In Esplora soluzioni, espandere la cartella Controllers e aprire il file HomeController.</span><span class="sxs-lookup"><span data-stu-id="de512-140">In Solution Explorer, expand the Controllers folder and open the HomeController.cs file.</span></span> <span data-ttu-id="de512-141">Aggiungere il **Authorize** attributo la `Admin` metodo.</span><span class="sxs-lookup"><span data-stu-id="de512-141">Add the **Authorize** attribute to the `Admin` method.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

<span data-ttu-id="de512-142">Aprire il file AdminController.cs e aggiungere il **Authorize** attributo per l'intero `AdminController` classe.</span><span class="sxs-lookup"><span data-stu-id="de512-142">Open the AdminController.cs file and add the **Authorize** attribute to the entire `AdminController` class.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="de512-143">MVC e Web API definiscono entrambi **Authorize** gli attributi, in spazi dei nomi diversi.</span><span class="sxs-lookup"><span data-stu-id="de512-143">MVC and Web API both define **Authorize** attributes, in different namespaces.</span></span> <span data-ttu-id="de512-144">MVC utilizza **System.Web.Mvc.AuthorizeAttribute**, mentre utilizza l'API Web **System.Web.Http.AuthorizeAttribute**.</span><span class="sxs-lookup"><span data-stu-id="de512-144">MVC uses **System.Web.Mvc.AuthorizeAttribute**, while Web API uses **System.Web.Http.AuthorizeAttribute**.</span></span>


<span data-ttu-id="de512-145">Ora solo gli amministratori possono visualizzare la pagina di amministrazione.</span><span class="sxs-lookup"><span data-stu-id="de512-145">Now only administrators can view the Admin page.</span></span> <span data-ttu-id="de512-146">Inoltre, se si invia una richiesta HTTP al controller di amministrazione, la richiesta deve contenere un cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="de512-146">Also, if you send an HTTP request to the Admin controller, the request must contain an authentication cookie.</span></span> <span data-ttu-id="de512-147">In caso contrario, il server invia una risposta HTTP 401 (non autorizzato).</span><span class="sxs-lookup"><span data-stu-id="de512-147">If not, the server sends an HTTP 401 (Unauthorized) response.</span></span> <span data-ttu-id="de512-148">È possibile notarlo in Fiddler inviando una richiesta GET al `http://localhost:*port*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="de512-148">You can see this in Fiddler by sending a GET request to `http://localhost:*port*/api/admin`.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="de512-149">[Precedente](using-web-api-with-entity-framework-part-3.md)
[Successivo](using-web-api-with-entity-framework-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="de512-149">[Previous](using-web-api-with-entity-framework-part-3.md)
[Next](using-web-api-with-entity-framework-part-5.md)</span></span>
