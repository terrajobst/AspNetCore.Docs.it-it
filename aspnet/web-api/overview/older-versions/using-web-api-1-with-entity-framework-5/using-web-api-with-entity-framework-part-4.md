---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Parte 4: Aggiunta di una visualizzazione di amministrazione | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/04/2012
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 8d9d14e5228606a81482eb08086a114ad5145e44
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828812"
---
<a name="part-4-adding-an-admin-view"></a><span data-ttu-id="3cf1e-102">Parte 4: Aggiunta di una visualizzazione di amministrazione</span><span class="sxs-lookup"><span data-stu-id="3cf1e-102">Part 4: Adding an Admin View</span></span>
====================
<span data-ttu-id="3cf1e-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3cf1e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="3cf1e-104">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="3cf1e-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a><span data-ttu-id="3cf1e-105">Aggiungere una visualizzazione di amministrazione</span><span class="sxs-lookup"><span data-stu-id="3cf1e-105">Add an Admin View</span></span>

<span data-ttu-id="3cf1e-106">A questo punto si sarà attiva sul lato client e aggiungere una pagina che può usare dati dal controller di amministrazione.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-106">Now we'll turn to the client side, and add a page that can consume data from the Admin controller.</span></span> <span data-ttu-id="3cf1e-107">La pagina consentirà agli utenti di creare, modificare o eliminare i prodotti, inviando le richieste AJAX al controller.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-107">The page will allow users to create, edit, or delete products, by sending AJAX requests to the controller.</span></span>

<span data-ttu-id="3cf1e-108">In Esplora soluzioni espandere la cartella Controllers e aprire il file denominato HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-108">In Solution Explorer, expand the Controllers folder and open the file named HomeController.cs.</span></span> <span data-ttu-id="3cf1e-109">Questo file contiene un controller MVC.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-109">This file contains an MVC controller.</span></span> <span data-ttu-id="3cf1e-110">Aggiungere un metodo denominato `Admin`:</span><span class="sxs-lookup"><span data-stu-id="3cf1e-110">Add a method named `Admin`:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

<span data-ttu-id="3cf1e-111">Il **HttpRouteUrl** metodo consente di creare l'URI per l'API web e viene memorizzato nel contenitore di visualizzazione per un uso successivo.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-111">The **HttpRouteUrl** method creates the URI to the web API, and we store this in the view bag for later.</span></span>

<span data-ttu-id="3cf1e-112">Successivamente, posizionare il cursore del testo all'interno di `Admin` metodo di azione, quindi fare clic e scegliere **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-112">Next, position the text cursor within the `Admin` action method, then right-click and select **Add View**.</span></span> <span data-ttu-id="3cf1e-113">Verrà visualizzata la **Aggiungi visualizzazione** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-113">This will bring up the **Add View** dialog.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

<span data-ttu-id="3cf1e-114">Nel **Aggiungi visualizzazione** finestra di dialogo Nome visualizzazione "Admin".</span><span class="sxs-lookup"><span data-stu-id="3cf1e-114">In the **Add View** dialog, name the view "Admin".</span></span> <span data-ttu-id="3cf1e-115">Selezionare la casella di controllo etichettata **creare una visualizzazione fortemente tipizzata**.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-115">Select the check box labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="3cf1e-116">Sotto **classe di modello**, selezionare "Prodotto (ProductStore.Models)".</span><span class="sxs-lookup"><span data-stu-id="3cf1e-116">Under **Model Class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="3cf1e-117">Lasciare tutte le altre opzioni sui valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-117">Leave all the other options as their default values.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

<span data-ttu-id="3cf1e-118">Facendo clic **Aggiungi** aggiunge un file denominato Admin.cshtml sotto le visualizzazioni/Home.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-118">Clicking **Add** adds a file named Admin.cshtml under Views/Home.</span></span> <span data-ttu-id="3cf1e-119">Aprire il file e aggiungere il codice HTML seguente.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-119">Open this file and add the following HTML.</span></span> <span data-ttu-id="3cf1e-120">Questo codice HTML definisce la struttura della pagina, ma non è collegata alcuna funzionalità ancora.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-120">This HTML defines the structure of the page, but no functionality is wired up yet.</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a><span data-ttu-id="3cf1e-121">Creare un collegamento alla pagina di amministrazione</span><span class="sxs-lookup"><span data-stu-id="3cf1e-121">Create a Link to the Admin Page</span></span>

<span data-ttu-id="3cf1e-122">In Esplora soluzioni espandere la cartella visualizzazioni e quindi espandere la cartella condivisa.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-122">In Solution Explorer, expand the Views folder and then expand the Shared folder.</span></span> <span data-ttu-id="3cf1e-123">Aprire il file denominato \_layout. cshtml.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-123">Open the file named \_Layout.cshtml.</span></span> <span data-ttu-id="3cf1e-124">Individuare il **ul** elemento con id = "menu" e un collegamento all'azione per la visualizzazione di amministrazione:</span><span class="sxs-lookup"><span data-stu-id="3cf1e-124">Locate the **ul** element with id = "menu", and an action link for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> <span data-ttu-id="3cf1e-125">Nel progetto di esempio, ho apportato alcune altre modifiche descrittivo, ad esempio sostituendo la stringa "Inserire qui il logo".</span><span class="sxs-lookup"><span data-stu-id="3cf1e-125">In the sample project, I made a few other cosmetic changes, such as replacing the string "Your logo here".</span></span> <span data-ttu-id="3cf1e-126">Queste non influiscono sulle funzionalità dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-126">These don't affect the functionality of the application.</span></span> <span data-ttu-id="3cf1e-127">È possibile scaricare il progetto e confrontare i file.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-127">You can download the project and compare the files.</span></span>


<span data-ttu-id="3cf1e-128">Eseguire l'applicazione e fare clic sul collegamento "Admin" che viene visualizzato nella parte superiore della home page.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-128">Run the application and click the "Admin" link that appears at the top of the home page.</span></span> <span data-ttu-id="3cf1e-129">Pagina di amministrazione dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="3cf1e-129">The Admin page should look like the following:</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

<span data-ttu-id="3cf1e-130">Diritto a questo punto, la pagina non esegue alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-130">Right now, the page doesn't do anything.</span></span> <span data-ttu-id="3cf1e-131">Nella sezione successiva, si userà Knockout. js per creare un'interfaccia utente dinamica.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-131">In the next section, we'll use Knockout.js to create a dynamic UI.</span></span>

## <a name="add-authorization"></a><span data-ttu-id="3cf1e-132">Aggiungere l'autorizzazione</span><span class="sxs-lookup"><span data-stu-id="3cf1e-132">Add Authorization</span></span>

<span data-ttu-id="3cf1e-133">Pagina di amministrazione è attualmente accessibile a tutti gli utenti che visitano il sito.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-133">The Admin page is currently accessible to anyone visiting the site.</span></span> <span data-ttu-id="3cf1e-134">È possibile modificare questa impostazione per limitare le autorizzazioni per gli amministratori.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-134">Let's change this to restrict permission to administrators.</span></span>

<span data-ttu-id="3cf1e-135">Iniziare aggiungendo un ruolo di "Amministratore" e un utente amministratore.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-135">Start by adding an "Administrator" role and an administrator user.</span></span> <span data-ttu-id="3cf1e-136">In Esplora soluzioni espandere la cartella di filtri e aprire il file denominato InitializeSimpleMembershipAttribute.cs.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-136">In Solution Explorer, expand the Filters folder and open the file named InitializeSimpleMembershipAttribute.cs.</span></span> <span data-ttu-id="3cf1e-137">Individuare il `SimpleMembershipInitializer` costruttore.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-137">Locate the `SimpleMembershipInitializer` constructor.</span></span> <span data-ttu-id="3cf1e-138">Dopo la chiamata a **WebSecurity.InitializeDatabaseConnection**, aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="3cf1e-138">After the call to **WebSecurity.InitializeDatabaseConnection**, add the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

<span data-ttu-id="3cf1e-139">Questo è un modo rapido per aggiungere il ruolo "Amministratore" e creare un utente per il ruolo.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-139">This is a quick-and-dirty way to add the "Administrator" role and create a user for the role.</span></span>

<span data-ttu-id="3cf1e-140">In Esplora soluzioni espandere la cartella Controllers e aprire il file HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-140">In Solution Explorer, expand the Controllers folder and open the HomeController.cs file.</span></span> <span data-ttu-id="3cf1e-141">Aggiungere il **Authorize** dell'attributo di `Admin` (metodo).</span><span class="sxs-lookup"><span data-stu-id="3cf1e-141">Add the **Authorize** attribute to the `Admin` method.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

<span data-ttu-id="3cf1e-142">Aprire il file AdminController.cs e aggiungere il **Authorize** dell'attributo all'intero `AdminController` classe.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-142">Open the AdminController.cs file and add the **Authorize** attribute to the entire `AdminController` class.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="3cf1e-143">MVC e API Web definiscono entrambi **Authorize** attributi, in diversi spazi dei nomi.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-143">MVC and Web API both define **Authorize** attributes, in different namespaces.</span></span> <span data-ttu-id="3cf1e-144">Usa MVC **System.Web.Mvc.AuthorizeAttribute**, mentre l'API Web Usa **System.Web.Http.AuthorizeAttribute**.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-144">MVC uses **System.Web.Mvc.AuthorizeAttribute**, while Web API uses **System.Web.Http.AuthorizeAttribute**.</span></span>


<span data-ttu-id="3cf1e-145">Solo gli amministratori possono ora visualizzare la pagina di amministrazione.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-145">Now only administrators can view the Admin page.</span></span> <span data-ttu-id="3cf1e-146">Inoltre, se si invia una richiesta HTTP al controller di amministrazione, la richiesta deve contenere un cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-146">Also, if you send an HTTP request to the Admin controller, the request must contain an authentication cookie.</span></span> <span data-ttu-id="3cf1e-147">In caso contrario, il server invia una risposta HTTP 401 (non autorizzato).</span><span class="sxs-lookup"><span data-stu-id="3cf1e-147">If not, the server sends an HTTP 401 (Unauthorized) response.</span></span> <span data-ttu-id="3cf1e-148">È possibile verificarlo in Fiddler inviando una richiesta GET a `http://localhost:*port*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="3cf1e-148">You can see this in Fiddler by sending a GET request to `http://localhost:*port*/api/admin`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3cf1e-149">[Precedente](using-web-api-with-entity-framework-part-3.md)
> [Successivo](using-web-api-with-entity-framework-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="3cf1e-149">[Previous](using-web-api-with-entity-framework-part-3.md)
[Next](using-web-api-with-entity-framework-part-5.md)</span></span>
