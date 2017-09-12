---
title: Crea un'applicazione ASP.NET di base con i dati dell'utente protetti dall'autorizzazione
author: rick-anderson
keywords: ASP.NET Core, MVC, autorizzazione, ruoli, sicurezza, amministratore
ms.author: riande
manager: wpickett
ms.date: 05/22/2017
ms.topic: article
ms.assetid: abeb2f8e-dfbf-4398-a04c-338a613a65bc
ms.technology: aspnet
ms.prod: aspnet-core
uid: security/authorization/secure-data
ms.openlocfilehash: db05ffb585022c3d9512d32da28c54788f97129c
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="4e4aa-103">Crea un'applicazione ASP.NET di base con i dati dell'utente protetti dall'autorizzazione</span><span class="sxs-lookup"><span data-stu-id="4e4aa-103">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="4e4aa-104">Da [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="4e4aa-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="4e4aa-105">In questa esercitazione viene illustrato come creare un'app web con i dati utente protetti dall'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-105">This tutorial shows how to create a web app with user data protected by authorization.</span></span> <span data-ttu-id="4e4aa-106">Viene visualizzato un elenco di contatti che autenticati (registrato) creato.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-106">It  displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="4e4aa-107">Sono disponibili tre gruppi di sicurezza:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-107">There are three security groups:</span></span>

* <span data-ttu-id="4e4aa-108">Gli utenti registrati possono visualizzare tutti i dati di contatto approvati.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-108">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="4e4aa-109">Gli utenti registrati possono modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-109">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="4e4aa-110">Responsabili possano approvare o rifiutare i dati di contatto.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-110">Managers can approve or reject contact data.</span></span> <span data-ttu-id="4e4aa-111">Solo i contatti approvati sono visibili agli utenti.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="4e4aa-112">Gli amministratori possono approvare/rifiutare e tutti i dati di modifica/eliminazione.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-112">Administrators can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="4e4aa-113">Nella figura seguente, l'utente Rick (`rick@example.com`) ha effettuato l'accesso.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="4e4aa-114">Utente Rick può solo visualizzazione approvato contatti e il suo contatti modifica/eliminazione.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-114">User Rick can only view approved contacts and edit/delete his contacts.</span></span> <span data-ttu-id="4e4aa-115">Solo l'ultimo record, creato da Rick, Visualizza modifica ed eliminare i collegamenti</span><span class="sxs-lookup"><span data-stu-id="4e4aa-115">Only the last record, created by Rick, displays edit and delete links</span></span>

![immagine descritto in precedenza](secure-data/_static/rick.png)

<span data-ttu-id="4e4aa-117">Nella figura seguente, `manager@contoso.com` l'accesso e il ruolo di Manager.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-117">In the following image, `manager@contoso.com` is signed in and in the managers role.</span></span> 

![immagine descritto in precedenza](secure-data/_static/manager1.png)

<span data-ttu-id="4e4aa-119">L'immagine seguente mostra i gestori di visualizzazione dei dettagli di un contatto.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-119">The following image shows the  managers details view of a contact.</span></span>

![immagine descritto in precedenza](secure-data/_static/manager.png)

<span data-ttu-id="4e4aa-121">Solo i responsabili e gli amministratori hanno l'approvazione e rifiutare i pulsanti.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-121">Only managers and administrators have the approve and reject buttons.</span></span>

<span data-ttu-id="4e4aa-122">Nella figura seguente, `admin@contoso.com` è firmato in e il ruolo dell'amministratore.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-122">In the following image, `admin@contoso.com` is signed in and in the administrator’s role.</span></span> 

![immagine descritto in precedenza](secure-data/_static/admin.png)

<span data-ttu-id="4e4aa-124">L'amministratore ha tutti i privilegi.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-124">The administrator has all privileges.</span></span> <span data-ttu-id="4e4aa-125">È possibile lettura/modifica/eliminazione di un contatto e modificare lo stato dei contatti.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-125">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="4e4aa-126">L'app è stato creato da [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) seguenti `Contact` modello:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-126">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)  the following `Contact` model:</span></span>

<span data-ttu-id="4e4aa-127">[!code-csharp[Principale](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-127">[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span></span>

<span data-ttu-id="4e4aa-128">Oggetto `ContactIsOwnerAuthorizationHandler` gestore autorizzazione assicura che un utente può modificare solo i dati.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-128">A `ContactIsOwnerAuthorizationHandler` authorization handler ensures that a user can only edit their data.</span></span> <span data-ttu-id="4e4aa-129">Oggetto `ContactManagerAuthorizationHandler` gestore autorizzazioni consente ai responsabili di approvare o rifiutare i contatti.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-129">A `ContactManagerAuthorizationHandler` authorization handler allows managers to approve or reject contacts.</span></span>  <span data-ttu-id="4e4aa-130">Oggetto `ContactAdministratorsAuthorizationHandler` gestore di autorizzazione consente agli amministratori di approvare o rifiutare i contatti e modifica/eliminazione di contatti.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-130">A `ContactAdministratorsAuthorizationHandler` authorization handler allows administrators to approve or reject contacts and to edit/delete contacts.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="4e4aa-131">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="4e4aa-131">Prerequisites</span></span>

<span data-ttu-id="4e4aa-132">Non si tratta di un'esercitazione di inizio.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-132">This is not a beginning tutorial.</span></span> <span data-ttu-id="4e4aa-133">È necessario avere familiarità con:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-133">You should be familiar with:</span></span>

* [<span data-ttu-id="4e4aa-134">Componenti di base di ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="4e4aa-134">ASP.NET Core MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="4e4aa-135">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="4e4aa-135">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="4e4aa-136">L'avvio e l'app completata</span><span class="sxs-lookup"><span data-stu-id="4e4aa-136">The starter and completed app</span></span>

<span data-ttu-id="4e4aa-137">[Scaricare](xref:tutorials/index#how-to-download-a-sample) il [completato](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) app.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-137">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) app.</span></span> <span data-ttu-id="4e4aa-138">[Test](#test-the-completed-app) app completata in modo da acquisire familiarità con le funzionalità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-138">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span> 

### <a name="the-starter-app"></a><span data-ttu-id="4e4aa-139">L'applicazione di avvio</span><span class="sxs-lookup"><span data-stu-id="4e4aa-139">The starter app</span></span>

<span data-ttu-id="4e4aa-140">È utile confrontare il codice con il codice di esempio completata.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-140">It's helpful to compare your code with the completed sample.</span></span>

<span data-ttu-id="4e4aa-141">[Scaricare](xref:tutorials/index#how-to-download-a-sample) il [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) app.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-141">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) app.</span></span> 

<span data-ttu-id="4e4aa-142">Vedere [creare l'app starter](#create-the-starter-app) se si desidera crearne uno da zero.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-142">See [Create the starter app](#create-the-starter-app) if you'd like to create it from scratch.</span></span>

<span data-ttu-id="4e4aa-143">Aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-143">Update the database:</span></span>

```none
   dotnet ef database update
```

<span data-ttu-id="4e4aa-144">Eseguire l'app, toccare il **ContactManager** collegare e verificare è possibile creare, modificare ed eliminare un contatto.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-144">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

<span data-ttu-id="4e4aa-145">Questa esercitazione sono tutti i passaggi principali per creare l'app di dati utente.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-145">This tutorial has all the major steps to create the secure user data app.</span></span> <span data-ttu-id="4e4aa-146">Può risultare utile per fare riferimento al progetto completato.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-146">You may find it helpful to refer to the completed project.</span></span>

## <a name="modify-the-app-to-secure-user-data"></a><span data-ttu-id="4e4aa-147">Modificare l'app per proteggere i dati utente</span><span class="sxs-lookup"><span data-stu-id="4e4aa-147">Modify the app to secure user data</span></span>

<span data-ttu-id="4e4aa-148">Nelle sezioni seguenti sono tutti i passaggi principali per creare l'app di dati utente.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-148">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="4e4aa-149">Può risultare utile per fare riferimento al progetto completato.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-149">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="4e4aa-150">Associare i dati di contatto per l'utente</span><span class="sxs-lookup"><span data-stu-id="4e4aa-150">Tie the contact data to the user</span></span>

<span data-ttu-id="4e4aa-151">Utilizzare ASP.NET [identità](xref:security/authentication/identity) ID utente per accertarsi che gli utenti possono modificare i propri dati, ma non altri dati di utenti.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-151">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="4e4aa-152">Aggiungere `OwnerID` per il `Contact` modello:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-152">Add `OwnerID` to the `Contact` model:</span></span>

<span data-ttu-id="4e4aa-153">[!code-csharp[Principale](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-153">[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]</span></span>

<span data-ttu-id="4e4aa-154">`OwnerID`è l'ID dell'utente dal `AspNetUser` tabella il [identità](xref:security/authentication/identity) database.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-154">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="4e4aa-155">Il `Status` campo determina se un contatto visibile dagli utenti generale.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-155">The `Status` field determines if a contact is viewable by general users.</span></span> 

<span data-ttu-id="4e4aa-156">Lo scaffolding di una nuova migrazione e aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-156">Scaffold a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="4e4aa-157">Richiedere SSL e gli utenti autenticati</span><span class="sxs-lookup"><span data-stu-id="4e4aa-157">Require SSL and authenticated users</span></span>

<span data-ttu-id="4e4aa-158">Nel `ConfigureServices` metodo il *Startup.cs* file, aggiungere il [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api) filtro di autorizzazione:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-158">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api) authorization filter:</span></span>

<span data-ttu-id="4e4aa-159">[!code-csharp[Principale](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-159">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]</span></span>

<span data-ttu-id="4e4aa-160">Se si utilizza Visual Studio, vedere [configurare IIS Express per SSL e HTTPS](xref:security/enforcing-ssl#set-up-iis-express-for-sslhttps).</span><span class="sxs-lookup"><span data-stu-id="4e4aa-160">If you're using Visual Studio, see [Set up IIS Express for SSL/HTTPS](xref:security/enforcing-ssl#set-up-iis-express-for-sslhttps).</span></span> <span data-ttu-id="4e4aa-161">Per reindirizzare le richieste HTTP a HTTPS, vedere [Middleware di riscrittura URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="4e4aa-161">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="4e4aa-162">Se si utilizza Visual Studio Code o test in locale della piattaforma che non include un certificato di prova per SSL:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-162">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="4e4aa-163">Impostare `"LocalTest:skipSSL": true` nel *appSettings. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-163">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="4e4aa-164">Richiedere agli utenti autenticati</span><span class="sxs-lookup"><span data-stu-id="4e4aa-164">Require authenticated users</span></span>

<span data-ttu-id="4e4aa-165">Impostare i criteri di autenticazione predefinito per richiedere agli utenti di essere autenticato.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-165">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="4e4aa-166">È possibile rifiutare esplicitamente l'autenticazione in corrispondenza del metodo di azione o controller con la `[AllowAnonymous]` attributo.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-166">You can opt out of authentication at the controller or action method with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="4e4aa-167">Con questo approccio, qualsiasi controller di nuovo aggiunto automaticamente richiederà l'autenticazione, che è più sicuro di basarsi sui nuovi controller per includere il `[Authorize]` attributo.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-167">With this approach, any new controllers added will automatically require authentication, which is safer than relying on new controllers to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="4e4aa-168">Il comando seguente per aggiungere il `ConfigureServices` metodo il *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-168">Add the following to  the `ConfigureServices` method of the *Startup.cs* file:</span></span>

<span data-ttu-id="4e4aa-169">[!code-csharp[Principale](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-169">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]</span></span>

<span data-ttu-id="4e4aa-170">Aggiungere `[AllowAnonymous]` al controller home in modo gli utenti anonimi possono ottenere informazioni sul sito per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-170">Add `[AllowAnonymous]` to the home controller so anonymous users can get information about the site before they register.</span></span>

<span data-ttu-id="4e4aa-171">[!code-csharp[Principale](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-171">[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="4e4aa-172">Configurare l'account di test</span><span class="sxs-lookup"><span data-stu-id="4e4aa-172">Configure the test account</span></span>

<span data-ttu-id="4e4aa-173">La `SeedData` classe crea due account, amministratore e gestione.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-173">The `SeedData` class creates two accounts,  administrator and manager.</span></span> <span data-ttu-id="4e4aa-174">Utilizzare il [lo strumento Gestione segreto](xref:security/app-secrets) per impostare una password per questi account.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-174">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="4e4aa-175">Eseguire questa operazione dalla directory del progetto (la directory contenente *Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="4e4aa-175">Do this from the project directory (the directory containing *Program.cs*).</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="4e4aa-176">Aggiornamento `Configure` per utilizzare la password di test:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-176">Update `Configure` to use the test password:</span></span>

<span data-ttu-id="4e4aa-177">[!code-csharp[Principale](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-177">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]</span></span>

<span data-ttu-id="4e4aa-178">Aggiungere l'ID utente di amministratore e `Status = ContactStatus.Approved` ai contatti.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-178">Add the administrator user ID and `Status = ContactStatus.Approved` to the contacts.</span></span> <span data-ttu-id="4e4aa-179">Viene visualizzato un solo contatto, aggiungere l'ID utente per tutti i contatti:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-179">Only one contact is shown, add the user ID to all contacts:</span></span>

<span data-ttu-id="4e4aa-180">[!code-csharp[Principale](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-180">[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]</span></span>

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="4e4aa-181">Creare i gestori di autorizzazione di amministratore, manager e proprietario</span><span class="sxs-lookup"><span data-stu-id="4e4aa-181">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="4e4aa-182">Creare un `ContactIsOwnerAuthorizationHandler` classe il *autorizzazione* cartella.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-182">Create a `ContactIsOwnerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="4e4aa-183">Il `ContactIsOwnerAuthorizationHandler` verificherà che opera la risorsa utente proprietario della risorsa.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-183">The `ContactIsOwnerAuthorizationHandler` will verify the user acting on the resource owns the resource.</span></span>

<span data-ttu-id="4e4aa-184">[!code-csharp[Principale](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-184">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]</span></span>

<span data-ttu-id="4e4aa-185">Il `ContactIsOwnerAuthorizationHandler` chiamate `context.Succeed` se l'utente autenticato corrente è il proprietario del contatto.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-185">The `ContactIsOwnerAuthorizationHandler` calls `context.Succeed` if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="4e4aa-186">I gestori di autorizzazione restituiscono in genere `context.Succeed` quando vengono soddisfatti i requisiti.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-186">Authorization handlers generally return `context.Succeed` when the requirements are met.</span></span> <span data-ttu-id="4e4aa-187">Restituiscono `Task.FromResult(0)` quando non vengono soddisfatti i requisiti.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-187">They return `Task.FromResult(0)` when requirements are not met.</span></span> <span data-ttu-id="4e4aa-188">`Task.FromResult(0)`è né esito positivo o negativo, consente un altro gestore eventi di autorizzazione per l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-188">`Task.FromResult(0)` is neither success or failure, it allows other authorization handler to run.</span></span> <span data-ttu-id="4e4aa-189">Se è necessario eseguire in modo esplicito, restituire `context.Fail()`.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-189">If you need to explicitly fail, return `context.Fail()`.</span></span>

<span data-ttu-id="4e4aa-190">È consentire ai proprietari di contatto di modifica o Elimina i propri dati, in modo che non è necessario verificare l'operazione passato nel parametro requisito.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-190">We allow contact owners to edit/delete their own data, so we don't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="4e4aa-191">Creare un gestore di autorizzazione di gestione</span><span class="sxs-lookup"><span data-stu-id="4e4aa-191">Create a manager authorization handler</span></span>

<span data-ttu-id="4e4aa-192">Creare un `ContactManagerAuthorizationHandler` classe il *autorizzazione* cartella.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-192">Create a `ContactManagerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="4e4aa-193">Il `ContactManagerAuthorizationHandler` verificherà l'utente che opera la risorsa è un responsabile.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-193">The `ContactManagerAuthorizationHandler` will verify the user acting on the resource is a manager.</span></span> <span data-ttu-id="4e4aa-194">Solo i manager possono approvare o rifiutare le modifiche di contenuto (nuove o modificate).</span><span class="sxs-lookup"><span data-stu-id="4e4aa-194">Only managers can approve or reject content changes (new or changed).</span></span>

<span data-ttu-id="4e4aa-195">[!code-csharp[Principale](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-195">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]</span></span>

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="4e4aa-196">Creare un gestore autorizzazioni di amministratore</span><span class="sxs-lookup"><span data-stu-id="4e4aa-196">Create an administrator authorization handler</span></span>

<span data-ttu-id="4e4aa-197">Creare un `ContactAdministratorsAuthorizationHandler` classe il *autorizzazione* cartella.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-197">Create a `ContactAdministratorsAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="4e4aa-198">Il `ContactAdministratorsAuthorizationHandler` verificherà l'utente che opera la risorsa è un amministratore.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-198">The `ContactAdministratorsAuthorizationHandler` will verify the user acting on the resource is a administrator.</span></span> <span data-ttu-id="4e4aa-199">Amministratore può eseguire tutte le operazioni.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-199">Administrator can do all operations.</span></span>

<span data-ttu-id="4e4aa-200">[!code-csharp[Principale](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-200">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]</span></span>

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="4e4aa-201">Registrare i gestori di eventi di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="4e4aa-201">Register the authorization handlers</span></span>

<span data-ttu-id="4e4aa-202">Deve essere registrata con Entity Framework Core Services [inserimento di dipendenze](xref:fundamentals/dependency-injection) utilizzando [AddScoped](https://docs.microsoft.com/aspnet/core/api).</span><span class="sxs-lookup"><span data-stu-id="4e4aa-202">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](https://docs.microsoft.com/aspnet/core/api).</span></span> <span data-ttu-id="4e4aa-203">Il `ContactIsOwnerAuthorizationHandler` utilizza ASP.NET Core [identità](xref:security/authentication/identity), che si basa su Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-203">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="4e4aa-204">Registrare i gestori con la raccolta di servizio in modo saranno disponibili per il `ContactsController` tramite [inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="4e4aa-204">Register the handlers with the service collection so they will be available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="4e4aa-205">Aggiungere il codice seguente alla fine di `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-205">Add the following code to the end of `ConfigureServices`:</span></span>

<span data-ttu-id="4e4aa-206">[!code-csharp[Principale](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-206">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]</span></span>

<span data-ttu-id="4e4aa-207">`ContactAdministratorsAuthorizationHandler`e `ContactManagerAuthorizationHandler` vengono aggiunti come singleton.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-207">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="4e4aa-208">Perché non usano Entity Framework e tutte le informazioni necessarie sono singleton di `Context` parametro del `HandleRequirementAsync` (metodo).</span><span class="sxs-lookup"><span data-stu-id="4e4aa-208">They are singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

<span data-ttu-id="4e4aa-209">L'intero `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-209">The complete `ConfigureServices`:</span></span>

<span data-ttu-id="4e4aa-210">[!code-csharp[Principale](secure-data/samples/final/Startup.cs?name=ConfigureServices)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-210">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]</span></span>

## <a name="update-the-code-to-support-authorization"></a><span data-ttu-id="4e4aa-211">Aggiornare il codice per supportare l'autorizzazione</span><span class="sxs-lookup"><span data-stu-id="4e4aa-211">Update the code to support authorization</span></span>

<span data-ttu-id="4e4aa-212">In questa sezione aggiornare il controller e visualizzazioni e aggiungere una classe di requisiti di operazioni.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-212">In this section, you update the controller and views and add an operations requirements class.</span></span>

### <a name="update-the-contacts-controller"></a><span data-ttu-id="4e4aa-213">Aggiornare il controller di contatti</span><span class="sxs-lookup"><span data-stu-id="4e4aa-213">Update the Contacts controller</span></span>

<span data-ttu-id="4e4aa-214">Aggiornamento di `ContactsController` costruttore:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-214">Update the `ContactsController` constructor:</span></span>

* <span data-ttu-id="4e4aa-215">Aggiungere il `IAuthorizationService` accesso ai gestori di autorizzazione del servizio.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-215">Add the `IAuthorizationService` service to  access to the authorization handlers.</span></span> 
* <span data-ttu-id="4e4aa-216">Aggiungere il `Identity` `UserManager` servizio:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-216">Add the `Identity` `UserManager` service:</span></span>

<span data-ttu-id="4e4aa-217">[!code-csharp[Principale](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-217">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]</span></span>

### <a name="add-a-contact-operations-requirements-class"></a><span data-ttu-id="4e4aa-218">Aggiungere una classe di requisiti di operazioni di contatto</span><span class="sxs-lookup"><span data-stu-id="4e4aa-218">Add a contact operations requirements class</span></span>

<span data-ttu-id="4e4aa-219">Aggiungere il `ContactOperationsRequirements` classe per il *autorizzazione* cartella.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-219">Add the `ContactOperationsRequirements` class to the *Authorization* folder.</span></span> <span data-ttu-id="4e4aa-220">I requisiti di contenere questa classe supporta l'app:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-220">This class  contain the requirements our app supports:</span></span>

<span data-ttu-id="4e4aa-221">[!code-csharp[Principale](secure-data/samples/final/Authorization/ContactOperations.cs)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-221">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]</span></span>

### <a name="update-create"></a><span data-ttu-id="4e4aa-222">Creazione di aggiornamento</span><span class="sxs-lookup"><span data-stu-id="4e4aa-222">Update Create</span></span>

<span data-ttu-id="4e4aa-223">Aggiornamento di `HTTP POST Create` metodo:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-223">Update the `HTTP POST Create` method to:</span></span>

* <span data-ttu-id="4e4aa-224">Aggiungere l'ID utente di `Contact` modello.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-224">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="4e4aa-225">Chiamare il gestore dell'autorizzazione per verificare che l'utente proprietario del contatto.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-225">Call the authorization handler to verify the user owns the contact.</span></span>

<span data-ttu-id="4e4aa-226">[!code-csharp[Principale](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-226">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]</span></span>

### <a name="update-edit"></a><span data-ttu-id="4e4aa-227">Modifica di aggiornamento</span><span class="sxs-lookup"><span data-stu-id="4e4aa-227">Update Edit</span></span>

<span data-ttu-id="4e4aa-228">Aggiornare i valori `Edit` metodi da utilizzare il gestore dell'autorizzazione per verificare che l'utente appartiene il contatto.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-228">Update both `Edit` methods to use the authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="4e4aa-229">Poiché stiamo eseguendo l'autorizzazione per le risorse non è possibile usare il `[Authorize]` attributo.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-229">Because we are performing resource authorization we cannot use the `[Authorize]` attribute.</span></span> <span data-ttu-id="4e4aa-230">Quando vengono valutati gli attributi non sono l'accesso alla risorsa.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-230">We don't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="4e4aa-231">Autorizzazione per le risorse in base deve essere imperativo.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-231">Resource based authorization must be imperative.</span></span> <span data-ttu-id="4e4aa-232">Controlli devono essere eseguiti una volta ottenuto l'accesso alla risorsa, caricandolo in questo controller o caricandola all'interno del gestore stesso.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-232">Checks must be performed once we have access to the resource, either by loading it in our controller, or by loading it within the handler itself.</span></span> <span data-ttu-id="4e4aa-233">Spesso si accede alla risorsa passando la chiave della risorsa.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-233">Frequently you will access the resource by passing in the resource key.</span></span>

<span data-ttu-id="4e4aa-234">[!code-csharp[Principale](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-234">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]</span></span>

### <a name="update-the-delete-method"></a><span data-ttu-id="4e4aa-235">Aggiornare il metodo Delete</span><span class="sxs-lookup"><span data-stu-id="4e4aa-235">Update the Delete method</span></span>

<span data-ttu-id="4e4aa-236">Aggiornare i valori `Delete` metodi da utilizzare il gestore dell'autorizzazione per verificare che l'utente appartiene il contatto.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-236">Update both `Delete` methods to use the authorization handler to verify the user owns the contact.</span></span>

<span data-ttu-id="4e4aa-237">[!code-csharp[Principale](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-237">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]</span></span>

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="4e4aa-238">Inserire il servizio di autorizzazione nelle viste</span><span class="sxs-lookup"><span data-stu-id="4e4aa-238">Inject the authorization service into the views</span></span>

<span data-ttu-id="4e4aa-239">Attualmente la Mostra interfaccia utente modifica ed elimina i collegamenti per l'utente non è possibile modificare i dati.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-239">Currently the UI shows edit and delete links for data the user cannot modify.</span></span> <span data-ttu-id="4e4aa-240">Che verrà risolto applicando il gestore dell'autorizzazione per le viste.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-240">We'll fix that by applying the authorization handler to the views.</span></span>

<span data-ttu-id="4e4aa-241">Inserire il servizio di autorizzazione nel *Views/_ViewImports.cshtml* file in modo che sia disponibile per tutte le viste:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-241">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it will be available to all views:</span></span>

<span data-ttu-id="4e4aa-242">[!code-html[Principale](secure-data/samples/final/Views/_ViewImports.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-242">[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]</span></span>

<span data-ttu-id="4e4aa-243">Aggiornamento di *Views/Contacts/Index.cshtml* visualizzazione Razor solo visualizzare la modifica ed eliminare i collegamenti per gli utenti possono modificare/eliminare il contatto.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-243">Update the *Views/Contacts/Index.cshtml* Razor view to only display the edit and delete links for users who can edit/delete the contact.</span></span>

<span data-ttu-id="4e4aa-244">Aggiungere`@using ContactManager.Authorization;`</span><span class="sxs-lookup"><span data-stu-id="4e4aa-244">Add `@using ContactManager.Authorization;`</span></span>

<span data-ttu-id="4e4aa-245">Aggiornamento di `Edit` e `Delete` collega in modo da essere sottoposti a rendering solo per gli utenti con autorizzazione modificare ed eliminare il contatto.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-245">Update the `Edit` and `Delete` links so they are only rendered for users with permission to edit and delete the contact.</span></span>

<span data-ttu-id="4e4aa-246">[!code-html[Principale](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-246">[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]</span></span>

<span data-ttu-id="4e4aa-247">Avviso: Nascondere i collegamenti da utenti che non dispone dell'autorizzazione per modificare o eliminare i dati non è possibile proteggere l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-247">Warning: Hiding links from users that do not have permission to edit or delete data does not secure the app.</span></span> <span data-ttu-id="4e4aa-248">Nascondere i collegamenti rende l'app utente più descrittivo visualizzando i collegamenti sono validi.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-248">Hiding links makes the app more user friendly by displaying only valid links.</span></span> <span data-ttu-id="4e4aa-249">Gli utenti possono hack gli URL generati per richiamare modifica ed eliminazione di operazioni sui dati che non possiedono.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-249">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span>  <span data-ttu-id="4e4aa-250">Il controller è necessario ripetere che controlla l'accesso per essere protetti.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-250">The controller must repeat the access checks to be secure.</span></span>

### <a name="update-the-details-view"></a><span data-ttu-id="4e4aa-251">Aggiornare la visualizzazione dei dettagli</span><span class="sxs-lookup"><span data-stu-id="4e4aa-251">Update the Details view</span></span>

<span data-ttu-id="4e4aa-252">Aggiornare la visualizzazione dei dettagli in modo che i responsabili possano approvare o rifiutare contatti:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-252">Update the details view so managers can approve or reject contacts:</span></span>

<span data-ttu-id="4e4aa-253">[!code-html[Principale](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-253">[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="4e4aa-254">Testare l'app completata</span><span class="sxs-lookup"><span data-stu-id="4e4aa-254">Test the completed app</span></span>

<span data-ttu-id="4e4aa-255">Se si utilizza Visual Studio Code o test in locale della piattaforma che non include un certificato di prova per SSL:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-255">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="4e4aa-256">Impostare `"LocalTest:skipSSL": true` nel *appSettings. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-256">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

<span data-ttu-id="4e4aa-257">Se si hanno eseguito l'app e contatti, eliminare tutti i record di `Contact` tabella e riavviare l'applicazione per l'inizializzazione del database.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-257">If you have run the app and have contacts, delete all the records in the `Contact` table and restart the app to seed the database.</span></span> <span data-ttu-id="4e4aa-258">Se si utilizza Visual Studio, è necessario chiudere e riavviare IIS Express al valore di inizializzazione del database.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-258">If you are using Visual Studio, you need to exit and restart IIS Express to seed the database.</span></span>

<span data-ttu-id="4e4aa-259">Registrazione di un utente per cercare i contatti.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-259">Register a user to browse the contacts.</span></span>

<span data-ttu-id="4e4aa-260">Un modo semplice per testare app completata consiste nell'avviare tre diversi browser (o incognito/InPrivate versioni).</span><span class="sxs-lookup"><span data-stu-id="4e4aa-260">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="4e4aa-261">In un browser, registrare un nuovo utente, ad esempio, `test@example.com`.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-261">In one browser, register a new user, for example, `test@example.com`.</span></span> <span data-ttu-id="4e4aa-262">Accedere a ogni browser con un altro utente.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-262">Sign in to each browser with a different user.</span></span> <span data-ttu-id="4e4aa-263">Verificare quanto segue:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-263">Verify the following:</span></span>

* <span data-ttu-id="4e4aa-264">Gli utenti registrati possono visualizzare tutti i dati di contatto approvati.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-264">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="4e4aa-265">Gli utenti registrati possono modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-265">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="4e4aa-266">Responsabili possano approvare o rifiutare i dati di contatto.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-266">Managers can approve or reject contact data.</span></span> <span data-ttu-id="4e4aa-267">Il `Details` vista mostra **Approva** e **rifiutare** pulsanti.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-267">The `Details` view shows **Approve** and **Reject** buttons.</span></span> 
* <span data-ttu-id="4e4aa-268">Gli amministratori possono approvare/rifiutare e tutti i dati di modifica/eliminazione.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-268">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="4e4aa-269">Utente</span><span class="sxs-lookup"><span data-stu-id="4e4aa-269">User</span></span>| <span data-ttu-id="4e4aa-270">Opzioni</span><span class="sxs-lookup"><span data-stu-id="4e4aa-270">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="4e4aa-271">Può modificare/eliminare dati</span><span class="sxs-lookup"><span data-stu-id="4e4aa-271">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="4e4aa-272">Può approvare/rifiutare e modifica/eliminazione disporre di dati</span><span class="sxs-lookup"><span data-stu-id="4e4aa-272">Can approve/reject and edit/delete own data</span></span>  |
| admin@contoso.com | <span data-ttu-id="4e4aa-273">Possibile modifica/eliminazione e approvare/rifiutare tutti i dati</span><span class="sxs-lookup"><span data-stu-id="4e4aa-273">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="4e4aa-274">Creare un contatto nel browser gli amministratori.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-274">Create a contact in the administrators browser.</span></span> <span data-ttu-id="4e4aa-275">Copiare l'URL per l'eliminazione e modificare il contatto dell'amministratore.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-275">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="4e4aa-276">Incollare questi collegamenti nel browser dell'utente test per verificare che l'utente test non è possibile eseguire queste operazioni.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-276">Paste these links into the test user's browser to verify the test user cannot perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="4e4aa-277">Creare l'app starter</span><span class="sxs-lookup"><span data-stu-id="4e4aa-277">Create the starter app</span></span>

<span data-ttu-id="4e4aa-278">Seguire queste istruzioni per creare l'applicazione di avvio.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-278">Follow these instructions to create the starter app.</span></span>

* <span data-ttu-id="4e4aa-279">Creare un **applicazione Web di ASP.NET Core** utilizzando [Visual Studio 2017](https://www.visualstudio.com/) denominato "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="4e4aa-279">Create an **ASP.NET Core Web Application** using [Visual Studio 2017](https://www.visualstudio.com/) named "ContactManager"</span></span>

  * <span data-ttu-id="4e4aa-280">Creare l'app con **singoli account utente**.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-280">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="4e4aa-281">Il nome "ContactManager" in modo l'utilizzo dello spazio dei nomi nell'esempio corrisponderà a uno spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-281">Name it "ContactManager" so your namespace will match the namespace use in the sample.</span></span>

* <span data-ttu-id="4e4aa-282">Aggiungere il seguente `Contact` modello:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-282">Add the following `Contact` model:</span></span>

  <span data-ttu-id="4e4aa-283">[!code-csharp[Principale](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-283">[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span></span>

* <span data-ttu-id="4e4aa-284">Lo scaffolding di `Contact` del modello tramite Entity Framework Core e `ApplicationDbContext` contesto dati.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-284">Scaffold the `Contact` model using Entity Framework Core and the `ApplicationDbContext` data context.</span></span> <span data-ttu-id="4e4aa-285">Accettare tutte le impostazioni predefinite di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-285">Accept all the scaffolding defaults.</span></span> <span data-ttu-id="4e4aa-286">Utilizzando `ApplicationDbContext` per il contesto dati classe inserisce nella tabella contatto nel [identità](xref:security/authentication/identity) database.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-286">Using `ApplicationDbContext` for the data context class  puts the contact table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="4e4aa-287">Vedere [aggiunta di un modello](xref:tutorials/first-mvc-app/adding-model) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-287">See [Adding a model](xref:tutorials/first-mvc-app/adding-model) for more information.</span></span>

* <span data-ttu-id="4e4aa-288">Aggiornamento di **ContactManager** di ancoraggio nel *Views/Shared/_Layout.cshtml* file dalla `asp-controller="Home"` a `asp-controller="Contacts"` quindi toccando il **ContactManager** collegamento richiama il controller di contatti.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-288">Update the **ContactManager** anchor in the *Views/Shared/_Layout.cshtml* file from `asp-controller="Home"` to `asp-controller="Contacts"` so tapping the **ContactManager** link will invoke the Contacts controller.</span></span> <span data-ttu-id="4e4aa-289">Il tag originale:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-289">The original markup:</span></span>

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

<span data-ttu-id="4e4aa-290">Il markup aggiornato:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-290">The updated markup:</span></span>

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* <span data-ttu-id="4e4aa-291">Lo scaffolding della migrazione iniziale e aggiornare il database</span><span class="sxs-lookup"><span data-stu-id="4e4aa-291">Scaffold the initial migration and update the database</span></span>

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* <span data-ttu-id="4e4aa-292">Testare l'app per la creazione, modifica ed eliminazione di un contatto</span><span class="sxs-lookup"><span data-stu-id="4e4aa-292">Test the app by creating, editing and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="4e4aa-293">Valore di inizializzazione del database</span><span class="sxs-lookup"><span data-stu-id="4e4aa-293">Seed the database</span></span>

<span data-ttu-id="4e4aa-294">Aggiungere il `SeedData` classe per il *dati* cartella.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-294">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="4e4aa-295">Se è stato scaricato l'esempio, è possibile copiare il *SeedData.cs* file per il *dati* cartella del progetto di avvio.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-295">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="4e4aa-296">[!code-csharp[Principale](secure-data/samples/starter/Data/SeedData.cs)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-296">[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]</span></span>

<span data-ttu-id="4e4aa-297">Aggiungere il codice evidenziato alla fine del `Configure` metodo il *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-297">Add the highlighted code to the end of the `Configure` method in the *Startup.cs* file:</span></span>

<span data-ttu-id="4e4aa-298">[!code-csharp[Principale](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-298">[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]</span></span>

<span data-ttu-id="4e4aa-299">Verificare che l'app effettuato il seeding del database.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-299">Test that the app seeded the database.</span></span> <span data-ttu-id="4e4aa-300">Il metodo di inizializzazione non viene eseguito se sono presenti righe nel database di contatto.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-300">The seed method does not run if there are any rows in the contact DB.</span></span>

### <a name="create-a-class-used-in-the-tutorial"></a><span data-ttu-id="4e4aa-301">Creare una classe utilizzata nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="4e4aa-301">Create a class used in the tutorial</span></span>

* <span data-ttu-id="4e4aa-302">Creare una cartella denominata *autorizzazione*.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-302">Create a folder named *Authorization*.</span></span>
* <span data-ttu-id="4e4aa-303">Copia il *Authorization\ContactOperations.cs* file da scaricare il progetto completato o copiare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4e4aa-303">Copy the *Authorization\ContactOperations.cs* file from the completed project download, or copy the following code:</span></span>

<span data-ttu-id="4e4aa-304">[!code-csharp[Principale](secure-data/samples/final/Authorization/ContactOperations.cs)]</span><span class="sxs-lookup"><span data-stu-id="4e4aa-304">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]</span></span>

<a name=secure-data-add-resources-label></a>

### <a name="additional-resources"></a><span data-ttu-id="4e4aa-305">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4e4aa-305">Additional resources</span></span>

* <span data-ttu-id="4e4aa-306">[Lab autorizzazione ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="4e4aa-306">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="4e4aa-307">Questa esercitazione vengono descritti in dettaglio le funzionalità di sicurezza introdotte in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="4e4aa-307">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="4e4aa-308">Autorizzazione in ASP.NET Core: semplice, ruolo, basata sulle attestazioni e personalizzato</span><span class="sxs-lookup"><span data-stu-id="4e4aa-308">Authorization in ASP.NET Core : Simple, role, claims-based and custom</span></span>](index.md)
* [<span data-ttu-id="4e4aa-309">Autorizzazione personalizzata basata su criteri</span><span class="sxs-lookup"><span data-stu-id="4e4aa-309">Custom Policy-Based Authorization</span></span>](policies.md)
