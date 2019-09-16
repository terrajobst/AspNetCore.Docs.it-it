---
title: Creare un'app ASP.NET Core con i dati utente protetti da autorizzazione
author: rick-anderson
description: Informazioni su come creare un'app Razor Pages con i dati utente protetti da autorizzazione. Include HTTPS, l'autenticazione, sicurezza, ASP.NET Core Identity.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: d95f44394d6ecc3c3896b45c5bebc73fa2d92445
ms.sourcegitcommit: dc5b293e08336dc236de66ed1834f7ef78359531
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/16/2019
ms.locfileid: "71011194"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="76698-104">Creare un'app ASP.NET Core con i dati utente protetti da autorizzazione</span><span class="sxs-lookup"><span data-stu-id="76698-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="76698-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="76698-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="76698-106">Visualizzare [questo PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) per la versione di ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="76698-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="76698-107">La versione di ASP.NET Core 1.1 di questa esercitazione è nel [ciò](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) cartella.</span><span class="sxs-lookup"><span data-stu-id="76698-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="76698-108">1\.1 esempio ASP.NET Core è nel [esempi](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="76698-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="76698-109">Vedere [questo pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="76698-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="76698-110">Questa esercitazione illustra come creare un'app web ASP.NET Core con i dati utente protetti da autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="76698-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="76698-111">Viene visualizzato un elenco di contatti che (registrati) agli utenti autenticati hanno creato.</span><span class="sxs-lookup"><span data-stu-id="76698-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="76698-112">Sono disponibili tre gruppi di sicurezza:</span><span class="sxs-lookup"><span data-stu-id="76698-112">There are three security groups:</span></span>

* <span data-ttu-id="76698-113">**Utenti registrati** può visualizzare tutti i dati approvati e può modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="76698-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="76698-114">**I responsabili** possono approvare o rifiutare i dati di contatto.</span><span class="sxs-lookup"><span data-stu-id="76698-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="76698-115">Solo i contatti approvati sono visibili agli utenti.</span><span class="sxs-lookup"><span data-stu-id="76698-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="76698-116">**Gli amministratori** può approvare o rifiutare e modificare/eliminare tutti i dati.</span><span class="sxs-lookup"><span data-stu-id="76698-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="76698-117">Le immagini in questo documento non corrispondono esattamente ai modelli più recenti.</span><span class="sxs-lookup"><span data-stu-id="76698-117">The images in this document don't exactly match the latest templates.</span></span>

<span data-ttu-id="76698-118">Nell'immagine seguente, l'utente Rick (`rick@example.com`) ha effettuato l'accesso.</span><span class="sxs-lookup"><span data-stu-id="76698-118">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="76698-119">Rick possono visualizzare solo i contatti approvati e **Edit**/**eliminare**/**Crea nuovo** collegamenti per il suo i contatti.</span><span class="sxs-lookup"><span data-stu-id="76698-119">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="76698-120">Solo l'ultimo record, creato da Rick, consente di visualizzare **Edit** e **eliminare** collegamenti.</span><span class="sxs-lookup"><span data-stu-id="76698-120">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="76698-121">Gli altri utenti non vedono l'ultimo record fino a quando un responsabile o l'amministratore assume lo stato "Approvato".</span><span class="sxs-lookup"><span data-stu-id="76698-121">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Screenshot che mostra Rick effettuato l'accesso](secure-data/_static/rick.png)

<span data-ttu-id="76698-123">Nell'immagine seguente, `manager@contoso.com` è stato eseguito l'accesso e nel ruolo del Manager:</span><span class="sxs-lookup"><span data-stu-id="76698-123">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Screenshot che mostra manager@contoso.com effettuato l'accesso](secure-data/_static/manager1.png)

<span data-ttu-id="76698-125">L'immagine seguente mostra i gestori visualizzazione dettagli di un contatto:</span><span class="sxs-lookup"><span data-stu-id="76698-125">The following image shows the managers details view of a contact:</span></span>

![Visualizzazione di gestione di un contatto](secure-data/_static/manager.png)

<span data-ttu-id="76698-127">Il **Approve** e **rifiutare** pulsanti vengono visualizzati solo per i gestori e amministratori.</span><span class="sxs-lookup"><span data-stu-id="76698-127">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="76698-128">Nell'immagine seguente, `admin@contoso.com` è stato eseguito l'accesso e il ruolo dell'amministratore:</span><span class="sxs-lookup"><span data-stu-id="76698-128">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Screenshot che mostra admin@contoso.com effettuato l'accesso](secure-data/_static/admin.png)

<span data-ttu-id="76698-130">L'amministratore ha tutti i privilegi.</span><span class="sxs-lookup"><span data-stu-id="76698-130">The administrator has all privileges.</span></span> <span data-ttu-id="76698-131">Può leggere, modificare ed eliminare un contatto e modificare lo stato dei contatti.</span><span class="sxs-lookup"><span data-stu-id="76698-131">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="76698-132">L'app è stato creato da [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) seguenti `Contact` modello:</span><span class="sxs-lookup"><span data-stu-id="76698-132">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="76698-133">L'esempio contiene i gestori di autorizzazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="76698-133">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="76698-134">`ContactIsOwnerAuthorizationHandler`: Garantisce che un utente possa modificare solo i propri dati.</span><span class="sxs-lookup"><span data-stu-id="76698-134">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="76698-135">`ContactManagerAuthorizationHandler`: Consente ai responsabili di approvare o rifiutare i contatti.</span><span class="sxs-lookup"><span data-stu-id="76698-135">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="76698-136">`ContactAdministratorsAuthorizationHandler`: Consente agli amministratori di approvare o rifiutare i contatti e di modificare/eliminare i contatti.</span><span class="sxs-lookup"><span data-stu-id="76698-136">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="76698-137">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="76698-137">Prerequisites</span></span>

<span data-ttu-id="76698-138">Questa esercitazione viene fatto avanzare.</span><span class="sxs-lookup"><span data-stu-id="76698-138">This tutorial is advanced.</span></span> <span data-ttu-id="76698-139">È necessario avere familiarità con:</span><span class="sxs-lookup"><span data-stu-id="76698-139">You should be familiar with:</span></span>

* [<span data-ttu-id="76698-140">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="76698-140">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="76698-141">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="76698-141">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="76698-142">Conferma account e recupero password</span><span class="sxs-lookup"><span data-stu-id="76698-142">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="76698-143">Autorizzazione</span><span class="sxs-lookup"><span data-stu-id="76698-143">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="76698-144">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="76698-144">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="76698-145">Starter e app completata</span><span class="sxs-lookup"><span data-stu-id="76698-145">The starter and completed app</span></span>

<span data-ttu-id="76698-146">[Scaricare](xref:index#how-to-download-a-sample) il [completato](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span><span class="sxs-lookup"><span data-stu-id="76698-146">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="76698-147">[Test](#test-the-completed-app) dell'app completata in modo da avere acquisito familiarità con le funzionalità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="76698-147">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="76698-148">L'app di base</span><span class="sxs-lookup"><span data-stu-id="76698-148">The starter app</span></span>

<span data-ttu-id="76698-149">[Scaricare](xref:index#how-to-download-a-sample) il [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span><span class="sxs-lookup"><span data-stu-id="76698-149">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="76698-150">Eseguire l'app, toccare il **ContactManager** collegare e verificare è possibile creare, modificare ed eliminare un contatto.</span><span class="sxs-lookup"><span data-stu-id="76698-150">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="76698-151">Proteggere i dati utente</span><span class="sxs-lookup"><span data-stu-id="76698-151">Secure user data</span></span>

<span data-ttu-id="76698-152">Le sezioni seguenti includono tutti i passaggi principali per creare l'app per dati sicura degli utenti.</span><span class="sxs-lookup"><span data-stu-id="76698-152">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="76698-153">Si può risultare utile per fare riferimento al progetto completato.</span><span class="sxs-lookup"><span data-stu-id="76698-153">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="76698-154">Associare i dati di contatto per l'utente</span><span class="sxs-lookup"><span data-stu-id="76698-154">Tie the contact data to the user</span></span>

<span data-ttu-id="76698-155">Usare ASP.NET [identità](xref:security/authentication/identity) ID utente per garantire che gli utenti possono modificare i propri dati, ma non altri dati degli utenti.</span><span class="sxs-lookup"><span data-stu-id="76698-155">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="76698-156">Aggiungere `OwnerID` e `ContactStatus` per il `Contact` modello:</span><span class="sxs-lookup"><span data-stu-id="76698-156">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="76698-157">`OwnerID` è l'ID dell'utente dal `AspNetUser` tabella di [identità](xref:security/authentication/identity) database.</span><span class="sxs-lookup"><span data-stu-id="76698-157">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="76698-158">Il `Status` campo determina se un contatto visibile dagli utenti generali.</span><span class="sxs-lookup"><span data-stu-id="76698-158">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="76698-159">Creare una nuova migrazione e aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="76698-159">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="76698-160">Aggiungere altri servizi di identità</span><span class="sxs-lookup"><span data-stu-id="76698-160">Add Role services to Identity</span></span>

<span data-ttu-id="76698-161">Accodare [Aggiungi ruoli](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) per aggiungere servizi ruolo:</span><span class="sxs-lookup"><span data-stu-id="76698-161">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a><span data-ttu-id="76698-162">Richiedere agli utenti autenticati</span><span class="sxs-lookup"><span data-stu-id="76698-162">Require authenticated users</span></span>

<span data-ttu-id="76698-163">Impostare i criteri di autenticazione predefinito per richiedere agli utenti di essere autenticato:</span><span class="sxs-lookup"><span data-stu-id="76698-163">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 <span data-ttu-id="76698-164">È possibile rifiutare esplicitamente l'autenticazione a livello di metodo di pagina Razor, controller o azione con il `[AllowAnonymous]` attributo.</span><span class="sxs-lookup"><span data-stu-id="76698-164">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="76698-165">L'impostazione di criteri di autenticazione predefinito per richiedere agli utenti di essere autenticati protegge appena aggiunto Razor Pages e controller.</span><span class="sxs-lookup"><span data-stu-id="76698-165">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="76698-166">La presenza di autenticazione richiesto per impostazione predefinita è più sicuro rispetto al semplice uso nuovi controller e le pagine Razor per includere il `[Authorize]` attributo.</span><span class="sxs-lookup"><span data-stu-id="76698-166">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="76698-167">Aggiungere [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) alle pagine indici e privacy in modo che gli utenti anonimi possano ottenere informazioni sul sito prima della registrazione.</span><span class="sxs-lookup"><span data-stu-id="76698-167">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index and Privacy pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a><span data-ttu-id="76698-168">Configurare l'account di test</span><span class="sxs-lookup"><span data-stu-id="76698-168">Configure the test account</span></span>

<span data-ttu-id="76698-169">Il `SeedData` classe crea due account: amministratore e manager.</span><span class="sxs-lookup"><span data-stu-id="76698-169">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="76698-170">Usare la [strumento Secret Manager](xref:security/app-secrets) per impostare una password per questi account.</span><span class="sxs-lookup"><span data-stu-id="76698-170">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="76698-171">Impostare la password dalla directory del progetto (la directory contenente *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="76698-171">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="76698-172">Se non è specificata una password complessa, viene generata un'eccezione quando `SeedData.Initialize` viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="76698-172">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="76698-173">Aggiornamento `Main` di usare la password di test:</span><span class="sxs-lookup"><span data-stu-id="76698-173">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="76698-174">Creare gli account di test e aggiornare i contatti</span><span class="sxs-lookup"><span data-stu-id="76698-174">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="76698-175">Aggiorna il `Initialize` metodo nel `SeedData` classe per creare gli account di test:</span><span class="sxs-lookup"><span data-stu-id="76698-175">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="76698-176">Aggiungere l'ID di utente amministratore e `ContactStatus` ai contatti.</span><span class="sxs-lookup"><span data-stu-id="76698-176">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="76698-177">Rendere uno dei contatti relativi al "Submitted" e "Rejected" uno.</span><span class="sxs-lookup"><span data-stu-id="76698-177">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="76698-178">Aggiungere l'ID utente e lo stato per tutti i contatti.</span><span class="sxs-lookup"><span data-stu-id="76698-178">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="76698-179">Viene visualizzato un solo contatto:</span><span class="sxs-lookup"><span data-stu-id="76698-179">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="76698-180">Creare proprietario, manager e i gestori di autorizzazione di amministratore</span><span class="sxs-lookup"><span data-stu-id="76698-180">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="76698-181">Creare un `ContactIsOwnerAuthorizationHandler` classe la *autorizzazione* cartella.</span><span class="sxs-lookup"><span data-stu-id="76698-181">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="76698-182">Il `ContactIsOwnerAuthorizationHandler` verifica che l'utente che agisce su una risorsa proprietario della risorsa.</span><span class="sxs-lookup"><span data-stu-id="76698-182">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="76698-183">Il `ContactIsOwnerAuthorizationHandler` chiamate [contesto. Esito positivo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) se l'utente autenticato corrente è il proprietario del contatto.</span><span class="sxs-lookup"><span data-stu-id="76698-183">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="76698-184">I gestori di autorizzazione a livello generale:</span><span class="sxs-lookup"><span data-stu-id="76698-184">Authorization handlers generally:</span></span>

* <span data-ttu-id="76698-185">Restituire `context.Succeed` quando vengono soddisfatti i requisiti.</span><span class="sxs-lookup"><span data-stu-id="76698-185">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="76698-186">Restituire `Task.CompletedTask` quando non vengono soddisfatti i requisiti.</span><span class="sxs-lookup"><span data-stu-id="76698-186">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="76698-187">`Task.CompletedTask`non ha esito positivo&mdash;o negativo consente l'esecuzione di altri gestori di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="76698-187">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="76698-188">Se è necessario eseguire in modo esplicito, restituire [contesto. Esito negativo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="76698-188">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="76698-189">L'app consente ai proprietari di contatto di modifica/eliminazione/creare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="76698-189">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="76698-190">`ContactIsOwnerAuthorizationHandler` non è necessario verificare l'operazione passato nel parametro del requisito.</span><span class="sxs-lookup"><span data-stu-id="76698-190">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="76698-191">Creare un gestore di autorizzazione di gestione</span><span class="sxs-lookup"><span data-stu-id="76698-191">Create a manager authorization handler</span></span>

<span data-ttu-id="76698-192">Creare un `ContactManagerAuthorizationHandler` classe la *autorizzazione* cartella.</span><span class="sxs-lookup"><span data-stu-id="76698-192">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="76698-193">Il `ContactManagerAuthorizationHandler` verifica se l'utente che agiscono sulla risorsa è un manager.</span><span class="sxs-lookup"><span data-stu-id="76698-193">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="76698-194">Solo i manager possono approvare o rifiutare le modifiche di contenuto (nuove o modificate).</span><span class="sxs-lookup"><span data-stu-id="76698-194">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="76698-195">Creare un gestore di autorizzazione di amministratore</span><span class="sxs-lookup"><span data-stu-id="76698-195">Create an administrator authorization handler</span></span>

<span data-ttu-id="76698-196">Creare un `ContactAdministratorsAuthorizationHandler` classe la *autorizzazione* cartella.</span><span class="sxs-lookup"><span data-stu-id="76698-196">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="76698-197">Il `ContactAdministratorsAuthorizationHandler` verifica se l'utente che agiscono sulla risorsa è un amministratore.</span><span class="sxs-lookup"><span data-stu-id="76698-197">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="76698-198">Amministratore può eseguire tutte le operazioni.</span><span class="sxs-lookup"><span data-stu-id="76698-198">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="76698-199">Registrare i gestori di eventi di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="76698-199">Register the authorization handlers</span></span>

<span data-ttu-id="76698-200">Servizi usando Entity Framework Core devono essere registrati per [inserimento delle dipendenze](xref:fundamentals/dependency-injection) utilizzando [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="76698-200">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="76698-201">Il `ContactIsOwnerAuthorizationHandler` Usa ASP.NET Core [identità](xref:security/authentication/identity), che si basa su Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="76698-201">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="76698-202">Registrare i gestori con la raccolta di servizio in modo che siano disponibili per il `ContactsController` attraverso [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="76698-202">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="76698-203">Aggiungere il codice seguente alla fine della `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="76698-203">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

<span data-ttu-id="76698-204">`ContactAdministratorsAuthorizationHandler` e `ContactManagerAuthorizationHandler` vengono aggiunti come singleton.</span><span class="sxs-lookup"><span data-stu-id="76698-204">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="76698-205">Sono singleton con stato perché non usano Entity Framework e tutte le informazioni necessarie nel `Context` parametro del `HandleRequirementAsync` (metodo).</span><span class="sxs-lookup"><span data-stu-id="76698-205">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="76698-206">Supporto di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="76698-206">Support authorization</span></span>

<span data-ttu-id="76698-207">In questa sezione aggiornare le pagine Razor e aggiungere una classe di requisiti di operazioni.</span><span class="sxs-lookup"><span data-stu-id="76698-207">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="76698-208">Esaminare la classe di requisiti di operazioni contatto</span><span class="sxs-lookup"><span data-stu-id="76698-208">Review the contact operations requirements class</span></span>

<span data-ttu-id="76698-209">Esaminare il `ContactOperations` classe.</span><span class="sxs-lookup"><span data-stu-id="76698-209">Review the `ContactOperations` class.</span></span> <span data-ttu-id="76698-210">Questa classe contiene i requisiti supportato dall'app:</span><span class="sxs-lookup"><span data-stu-id="76698-210">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="76698-211">Creare una classe di base per le pagine Razor di contatti</span><span class="sxs-lookup"><span data-stu-id="76698-211">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="76698-212">Creare una classe base che contiene i servizi usati in Razor Pages i contatti.</span><span class="sxs-lookup"><span data-stu-id="76698-212">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="76698-213">La classe di base inserisce il codice di inizializzazione in un'unica posizione:</span><span class="sxs-lookup"><span data-stu-id="76698-213">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="76698-214">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="76698-214">The preceding code:</span></span>

* <span data-ttu-id="76698-215">Aggiunge il `IAuthorizationService` servizio per accedere ai gestori di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="76698-215">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="76698-216">Aggiunge le identità `UserManager` servizio.</span><span class="sxs-lookup"><span data-stu-id="76698-216">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="76698-217">Aggiungere l'oggetto `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="76698-217">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="76698-218">Aggiornare il CreateModel</span><span class="sxs-lookup"><span data-stu-id="76698-218">Update the CreateModel</span></span>

<span data-ttu-id="76698-219">Aggiornare il costruttore di modello della pagina create per usare il `DI_BasePageModel` classe di base:</span><span class="sxs-lookup"><span data-stu-id="76698-219">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="76698-220">Aggiornamento di `CreateModel.OnPostAsync` metodo:</span><span class="sxs-lookup"><span data-stu-id="76698-220">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="76698-221">Aggiungere l'ID utente per il `Contact` modello.</span><span class="sxs-lookup"><span data-stu-id="76698-221">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="76698-222">Chiamare il gestore di autorizzazione per verificare che l'utente dispone dell'autorizzazione per creare contatti.</span><span class="sxs-lookup"><span data-stu-id="76698-222">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="76698-223">Aggiornare il IndexModel</span><span class="sxs-lookup"><span data-stu-id="76698-223">Update the IndexModel</span></span>

<span data-ttu-id="76698-224">Aggiornamento di `OnGetAsync` metodo in modo che solo i contatti approvati vengono visualizzati agli utenti generici:</span><span class="sxs-lookup"><span data-stu-id="76698-224">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="76698-225">Aggiornare il EditModel</span><span class="sxs-lookup"><span data-stu-id="76698-225">Update the EditModel</span></span>

<span data-ttu-id="76698-226">Aggiungere un gestore di autorizzazione per verificare che l'utente è proprietario del contatto.</span><span class="sxs-lookup"><span data-stu-id="76698-226">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="76698-227">Poiché viene convalidata l'autorizzazione alle risorse, il `[Authorize]` attributo non è sufficiente.</span><span class="sxs-lookup"><span data-stu-id="76698-227">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="76698-228">L'app non ha accesso alla risorsa quando vengono valutati gli attributi.</span><span class="sxs-lookup"><span data-stu-id="76698-228">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="76698-229">Autorizzazione basata sulle risorse deve essere imperativa.</span><span class="sxs-lookup"><span data-stu-id="76698-229">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="76698-230">Controlli devono essere eseguiti dopo che l'app potrà accedere alla risorsa, è possibile caricarli nel modello di pagina oppure caricarlo all'interno del gestore stesso.</span><span class="sxs-lookup"><span data-stu-id="76698-230">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="76698-231">Si accede di frequente della risorsa passando la chiave di risorsa.</span><span class="sxs-lookup"><span data-stu-id="76698-231">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="76698-232">Aggiornare il DeleteModel</span><span class="sxs-lookup"><span data-stu-id="76698-232">Update the DeleteModel</span></span>

<span data-ttu-id="76698-233">Aggiornare il modello di pagina delete per utilizzare il gestore dell'autorizzazione per verificare che l'utente disponga dell'autorizzazione di eliminazione per il contatto.</span><span class="sxs-lookup"><span data-stu-id="76698-233">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="76698-234">Inserire il servizio di autorizzazione in visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="76698-234">Inject the authorization service into the views</span></span>

<span data-ttu-id="76698-235">Attualmente, l'interfaccia utente Mostra modifica ed elimina i collegamenti per i contatti che l'utente non è possibile modificare.</span><span class="sxs-lookup"><span data-stu-id="76698-235">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="76698-236">Inserire il servizio di autorizzazione nel file *pages/_ViewImports. cshtml* in modo che sia disponibile per tutte le visualizzazioni:</span><span class="sxs-lookup"><span data-stu-id="76698-236">Inject the authorization service in the *Pages/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="76698-237">Il markup precedente aggiunge diverse `using` istruzioni.</span><span class="sxs-lookup"><span data-stu-id="76698-237">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="76698-238">Aggiorna il **modifica** e **eliminare** collega in *Pages/Contacts/Index.cshtml* in modo che vengono sottoposti a rendering solo per gli utenti che dispongono delle autorizzazioni appropriate:</span><span class="sxs-lookup"><span data-stu-id="76698-238">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="76698-239">Nascondere i collegamenti da utenti che non dispone dell'autorizzazione per modificare i dati non proteggere le app.</span><span class="sxs-lookup"><span data-stu-id="76698-239">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="76698-240">Nascondere i collegamenti rende più semplici da usare l'app mediante la visualizzazione di collegamenti solo validi.</span><span class="sxs-lookup"><span data-stu-id="76698-240">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="76698-241">Gli utenti possono hack gli URL generati per richiamare modifica ed eliminare le operazioni sui dati che non sono proprietari.</span><span class="sxs-lookup"><span data-stu-id="76698-241">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="76698-242">La pagina Razor o il controller deve applicare controlli di accesso per proteggere i dati.</span><span class="sxs-lookup"><span data-stu-id="76698-242">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="76698-243">Aggiorna i dettagli</span><span class="sxs-lookup"><span data-stu-id="76698-243">Update Details</span></span>

<span data-ttu-id="76698-244">Aggiornare la visualizzazione dei dettagli in modo che i responsabili possono approvare o rifiutare i contatti:</span><span class="sxs-lookup"><span data-stu-id="76698-244">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="76698-245">Aggiornare il modello di pagina dei dettagli:</span><span class="sxs-lookup"><span data-stu-id="76698-245">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="76698-246">Aggiungere o rimuovere un utente a un ruolo</span><span class="sxs-lookup"><span data-stu-id="76698-246">Add or remove a user to a role</span></span>

<span data-ttu-id="76698-247">Visualizzare [questo problema](https://github.com/aspnet/AspNetCore.Docs/issues/8502) per informazioni su:</span><span class="sxs-lookup"><span data-stu-id="76698-247">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="76698-248">Rimozione dei privilegi da un utente.</span><span class="sxs-lookup"><span data-stu-id="76698-248">Removing privileges from a user.</span></span> <span data-ttu-id="76698-249">Ad esempio, disattivare un utente in un'app di chat.</span><span class="sxs-lookup"><span data-stu-id="76698-249">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="76698-250">Aggiunta di privilegi a un utente.</span><span class="sxs-lookup"><span data-stu-id="76698-250">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="76698-251">Testare l'app completata</span><span class="sxs-lookup"><span data-stu-id="76698-251">Test the completed app</span></span>

<span data-ttu-id="76698-252">Se è già stata impostata una password per gli account utente di seeding, usare il [strumento Secret Manager](xref:security/app-secrets#secret-manager) per impostare una password:</span><span class="sxs-lookup"><span data-stu-id="76698-252">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="76698-253">Scegliere una password complessa: Usare otto o più caratteri e almeno un carattere maiuscolo, un numero e un simbolo.</span><span class="sxs-lookup"><span data-stu-id="76698-253">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="76698-254">Ad esempio, `Passw0rd!` soddisfi i requisiti di password complesse.</span><span class="sxs-lookup"><span data-stu-id="76698-254">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="76698-255">Eseguire il comando seguente dalla cartella del progetto, in cui `<PW>` è la password:</span><span class="sxs-lookup"><span data-stu-id="76698-255">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

<span data-ttu-id="76698-256">Se l'app ha contatti:</span><span class="sxs-lookup"><span data-stu-id="76698-256">If the app has contacts:</span></span>

* <span data-ttu-id="76698-257">Eliminare tutti i record nel `Contact` tabella.</span><span class="sxs-lookup"><span data-stu-id="76698-257">Delete all of the records in the `Contact` table.</span></span>
* <span data-ttu-id="76698-258">Riavviare l'app per l'inizializzazione del database.</span><span class="sxs-lookup"><span data-stu-id="76698-258">Restart the app to seed the database.</span></span>

<span data-ttu-id="76698-259">Un modo semplice per testare l'app completata consiste nell'avviare tre diversi browser (o in incognito o InPrivate sessioni).</span><span class="sxs-lookup"><span data-stu-id="76698-259">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="76698-260">In un browser, registrare un nuovo utente (ad esempio, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="76698-260">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="76698-261">Accedere a ogni browser con un altro utente.</span><span class="sxs-lookup"><span data-stu-id="76698-261">Sign in to each browser with a different user.</span></span> <span data-ttu-id="76698-262">Verificare che le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="76698-262">Verify the following operations:</span></span>

* <span data-ttu-id="76698-263">Gli utenti registrati possono visualizzare tutti i dati di contatto approvati.</span><span class="sxs-lookup"><span data-stu-id="76698-263">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="76698-264">Gli utenti registrati possono modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="76698-264">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="76698-265">I responsabili possono approvare o rifiutare i dati di contatto.</span><span class="sxs-lookup"><span data-stu-id="76698-265">Managers can approve/reject contact data.</span></span> <span data-ttu-id="76698-266">Il `Details` viene mostrata **Approva** e **rifiutare** pulsanti.</span><span class="sxs-lookup"><span data-stu-id="76698-266">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="76698-267">Gli amministratori possono approvare o rifiutare e modificare/eliminare tutti i dati.</span><span class="sxs-lookup"><span data-stu-id="76698-267">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="76698-268">Utente</span><span class="sxs-lookup"><span data-stu-id="76698-268">User</span></span>                | <span data-ttu-id="76698-269">Esegue il seeding dell'app</span><span class="sxs-lookup"><span data-stu-id="76698-269">Seeded by the app</span></span> | <span data-ttu-id="76698-270">Opzioni</span><span class="sxs-lookup"><span data-stu-id="76698-270">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="76698-271">No</span><span class="sxs-lookup"><span data-stu-id="76698-271">No</span></span>                | <span data-ttu-id="76698-272">Modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="76698-272">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="76698-273">Yes</span><span class="sxs-lookup"><span data-stu-id="76698-273">Yes</span></span>               | <span data-ttu-id="76698-274">Approvare o rifiutare e modificare/eliminare i dati personalizzati.</span><span class="sxs-lookup"><span data-stu-id="76698-274">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="76698-275">Yes</span><span class="sxs-lookup"><span data-stu-id="76698-275">Yes</span></span>               | <span data-ttu-id="76698-276">Approvare o rifiutare e modificare/eliminare tutti i dati.</span><span class="sxs-lookup"><span data-stu-id="76698-276">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="76698-277">Creare un contatto nel browser dell'amministratore.</span><span class="sxs-lookup"><span data-stu-id="76698-277">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="76698-278">Copiare l'URL per l'eliminazione e modifica di contattare l'amministratore.</span><span class="sxs-lookup"><span data-stu-id="76698-278">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="76698-279">Incollare questi collegamenti nel browser dell'utente test per verificare che l'utente di test non è possibile eseguire queste operazioni.</span><span class="sxs-lookup"><span data-stu-id="76698-279">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="76698-280">Creare l'app di base</span><span class="sxs-lookup"><span data-stu-id="76698-280">Create the starter app</span></span>

* <span data-ttu-id="76698-281">Creare un'app Razor Pages denominata "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="76698-281">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="76698-282">Creare l'app con **singoli account utente**.</span><span class="sxs-lookup"><span data-stu-id="76698-282">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="76698-283">Il nome "ContactManager" in modo che lo spazio dei nomi corrispondente dello spazio dei nomi utilizzato nell'esempio.</span><span class="sxs-lookup"><span data-stu-id="76698-283">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="76698-284">`-uld` Specifica di Local DB invece di SQLite</span><span class="sxs-lookup"><span data-stu-id="76698-284">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="76698-285">Aggiungi *modelli/Contact. cs*:</span><span class="sxs-lookup"><span data-stu-id="76698-285">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="76698-286">Scaffolding di `Contact` modello.</span><span class="sxs-lookup"><span data-stu-id="76698-286">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="76698-287">Creare la migrazione iniziale e l'aggiornamento del database:</span><span class="sxs-lookup"><span data-stu-id="76698-287">Create initial migration and update the database:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
  ```

<span data-ttu-id="76698-288">Se si verifica un bug con il `dotnet aspnet-codegenerator razorpage` comando, vedere [questo problema di GitHub](https://github.com/aspnet/Scaffolding/issues/984).</span><span class="sxs-lookup"><span data-stu-id="76698-288">If you experience a bug with the `dotnet aspnet-codegenerator razorpage` command, see [this GitHub issue](https://github.com/aspnet/Scaffolding/issues/984).</span></span>

* <span data-ttu-id="76698-289">Aggiornare l'ancoraggio **ContactManager** nel file *pages/Shared/file. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="76698-289">Update the **ContactManager** anchor in the *Pages/Shared/_Layout.cshtml* file:</span></span>

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* <span data-ttu-id="76698-290">Testare l'app per la creazione, modifica ed eliminazione di un contatto</span><span class="sxs-lookup"><span data-stu-id="76698-290">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="76698-291">Specificare il valore di inizializzazione del database</span><span class="sxs-lookup"><span data-stu-id="76698-291">Seed the database</span></span>

<span data-ttu-id="76698-292">Aggiungere la classe [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) alla cartella *Data* :</span><span class="sxs-lookup"><span data-stu-id="76698-292">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) class to the *Data* folder:</span></span>

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

<span data-ttu-id="76698-293">Chiamare `SeedData.Initialize` da `Main`:</span><span class="sxs-lookup"><span data-stu-id="76698-293">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

<span data-ttu-id="76698-294">Verificare che l'app effettuato il seeding del database.</span><span class="sxs-lookup"><span data-stu-id="76698-294">Test that the app seeded the database.</span></span> <span data-ttu-id="76698-295">Se sono presenti tutte le righe nel DB del contatto, il metodo di inizializzazione non viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="76698-295">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

<span data-ttu-id="76698-296">Questa esercitazione illustra come creare un'app web ASP.NET Core con i dati utente protetti da autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="76698-296">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="76698-297">Viene visualizzato un elenco di contatti che (registrati) agli utenti autenticati hanno creato.</span><span class="sxs-lookup"><span data-stu-id="76698-297">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="76698-298">Sono disponibili tre gruppi di sicurezza:</span><span class="sxs-lookup"><span data-stu-id="76698-298">There are three security groups:</span></span>

* <span data-ttu-id="76698-299">**Utenti registrati** può visualizzare tutti i dati approvati e può modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="76698-299">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="76698-300">**I responsabili** possono approvare o rifiutare i dati di contatto.</span><span class="sxs-lookup"><span data-stu-id="76698-300">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="76698-301">Solo i contatti approvati sono visibili agli utenti.</span><span class="sxs-lookup"><span data-stu-id="76698-301">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="76698-302">**Gli amministratori** può approvare o rifiutare e modificare/eliminare tutti i dati.</span><span class="sxs-lookup"><span data-stu-id="76698-302">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="76698-303">Nell'immagine seguente, l'utente Rick (`rick@example.com`) ha effettuato l'accesso.</span><span class="sxs-lookup"><span data-stu-id="76698-303">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="76698-304">Rick possono visualizzare solo i contatti approvati e **Edit**/**eliminare**/**Crea nuovo** collegamenti per il suo i contatti.</span><span class="sxs-lookup"><span data-stu-id="76698-304">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="76698-305">Solo l'ultimo record, creato da Rick, consente di visualizzare **Edit** e **eliminare** collegamenti.</span><span class="sxs-lookup"><span data-stu-id="76698-305">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="76698-306">Gli altri utenti non vedono l'ultimo record fino a quando un responsabile o l'amministratore assume lo stato "Approvato".</span><span class="sxs-lookup"><span data-stu-id="76698-306">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Screenshot che mostra Rick effettuato l'accesso](secure-data/_static/rick.png)

<span data-ttu-id="76698-308">Nell'immagine seguente, `manager@contoso.com` è stato eseguito l'accesso e nel ruolo del Manager:</span><span class="sxs-lookup"><span data-stu-id="76698-308">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Screenshot che mostra manager@contoso.com effettuato l'accesso](secure-data/_static/manager1.png)

<span data-ttu-id="76698-310">L'immagine seguente mostra i gestori visualizzazione dettagli di un contatto:</span><span class="sxs-lookup"><span data-stu-id="76698-310">The following image shows the managers details view of a contact:</span></span>

![Visualizzazione di gestione di un contatto](secure-data/_static/manager.png)

<span data-ttu-id="76698-312">Il **Approve** e **rifiutare** pulsanti vengono visualizzati solo per i gestori e amministratori.</span><span class="sxs-lookup"><span data-stu-id="76698-312">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="76698-313">Nell'immagine seguente, `admin@contoso.com` è stato eseguito l'accesso e il ruolo dell'amministratore:</span><span class="sxs-lookup"><span data-stu-id="76698-313">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Screenshot che mostra admin@contoso.com effettuato l'accesso](secure-data/_static/admin.png)

<span data-ttu-id="76698-315">L'amministratore ha tutti i privilegi.</span><span class="sxs-lookup"><span data-stu-id="76698-315">The administrator has all privileges.</span></span> <span data-ttu-id="76698-316">Può leggere, modificare ed eliminare un contatto e modificare lo stato dei contatti.</span><span class="sxs-lookup"><span data-stu-id="76698-316">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="76698-317">L'app è stato creato da [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) seguenti `Contact` modello:</span><span class="sxs-lookup"><span data-stu-id="76698-317">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="76698-318">L'esempio contiene i gestori di autorizzazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="76698-318">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="76698-319">`ContactIsOwnerAuthorizationHandler`: Garantisce che un utente possa modificare solo i propri dati.</span><span class="sxs-lookup"><span data-stu-id="76698-319">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="76698-320">`ContactManagerAuthorizationHandler`: Consente ai responsabili di approvare o rifiutare i contatti.</span><span class="sxs-lookup"><span data-stu-id="76698-320">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="76698-321">`ContactAdministratorsAuthorizationHandler`: Consente agli amministratori di approvare o rifiutare i contatti e di modificare/eliminare i contatti.</span><span class="sxs-lookup"><span data-stu-id="76698-321">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="76698-322">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="76698-322">Prerequisites</span></span>

<span data-ttu-id="76698-323">Questa esercitazione viene fatto avanzare.</span><span class="sxs-lookup"><span data-stu-id="76698-323">This tutorial is advanced.</span></span> <span data-ttu-id="76698-324">È necessario avere familiarità con:</span><span class="sxs-lookup"><span data-stu-id="76698-324">You should be familiar with:</span></span>

* [<span data-ttu-id="76698-325">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="76698-325">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="76698-326">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="76698-326">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="76698-327">Conferma account e recupero password</span><span class="sxs-lookup"><span data-stu-id="76698-327">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="76698-328">Autorizzazione</span><span class="sxs-lookup"><span data-stu-id="76698-328">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="76698-329">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="76698-329">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="76698-330">Starter e app completata</span><span class="sxs-lookup"><span data-stu-id="76698-330">The starter and completed app</span></span>

<span data-ttu-id="76698-331">[Scaricare](xref:index#how-to-download-a-sample) il [completato](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span><span class="sxs-lookup"><span data-stu-id="76698-331">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="76698-332">[Test](#test-the-completed-app) dell'app completata in modo da avere acquisito familiarità con le funzionalità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="76698-332">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="76698-333">L'app di base</span><span class="sxs-lookup"><span data-stu-id="76698-333">The starter app</span></span>

<span data-ttu-id="76698-334">[Scaricare](xref:index#how-to-download-a-sample) il [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span><span class="sxs-lookup"><span data-stu-id="76698-334">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="76698-335">Eseguire l'app, toccare il **ContactManager** collegare e verificare è possibile creare, modificare ed eliminare un contatto.</span><span class="sxs-lookup"><span data-stu-id="76698-335">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="76698-336">Proteggere i dati utente</span><span class="sxs-lookup"><span data-stu-id="76698-336">Secure user data</span></span>

<span data-ttu-id="76698-337">Le sezioni seguenti includono tutti i passaggi principali per creare l'app per dati sicura degli utenti.</span><span class="sxs-lookup"><span data-stu-id="76698-337">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="76698-338">Si può risultare utile per fare riferimento al progetto completato.</span><span class="sxs-lookup"><span data-stu-id="76698-338">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="76698-339">Associare i dati di contatto per l'utente</span><span class="sxs-lookup"><span data-stu-id="76698-339">Tie the contact data to the user</span></span>

<span data-ttu-id="76698-340">Usare ASP.NET [identità](xref:security/authentication/identity) ID utente per garantire che gli utenti possono modificare i propri dati, ma non altri dati degli utenti.</span><span class="sxs-lookup"><span data-stu-id="76698-340">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="76698-341">Aggiungere `OwnerID` e `ContactStatus` per il `Contact` modello:</span><span class="sxs-lookup"><span data-stu-id="76698-341">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="76698-342">`OwnerID` è l'ID dell'utente dal `AspNetUser` tabella di [identità](xref:security/authentication/identity) database.</span><span class="sxs-lookup"><span data-stu-id="76698-342">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="76698-343">Il `Status` campo determina se un contatto visibile dagli utenti generali.</span><span class="sxs-lookup"><span data-stu-id="76698-343">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="76698-344">Creare una nuova migrazione e aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="76698-344">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="76698-345">Aggiungere altri servizi di identità</span><span class="sxs-lookup"><span data-stu-id="76698-345">Add Role services to Identity</span></span>

<span data-ttu-id="76698-346">Accodare [Aggiungi ruoli](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) per aggiungere servizi ruolo:</span><span class="sxs-lookup"><span data-stu-id="76698-346">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="76698-347">Richiedere agli utenti autenticati</span><span class="sxs-lookup"><span data-stu-id="76698-347">Require authenticated users</span></span>

<span data-ttu-id="76698-348">Impostare i criteri di autenticazione predefinito per richiedere agli utenti di essere autenticato:</span><span class="sxs-lookup"><span data-stu-id="76698-348">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="76698-349">È possibile rifiutare esplicitamente l'autenticazione a livello di metodo di pagina Razor, controller o azione con il `[AllowAnonymous]` attributo.</span><span class="sxs-lookup"><span data-stu-id="76698-349">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="76698-350">L'impostazione di criteri di autenticazione predefinito per richiedere agli utenti di essere autenticati protegge appena aggiunto Razor Pages e controller.</span><span class="sxs-lookup"><span data-stu-id="76698-350">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="76698-351">La presenza di autenticazione richiesto per impostazione predefinita è più sicuro rispetto al semplice uso nuovi controller e le pagine Razor per includere il `[Authorize]` attributo.</span><span class="sxs-lookup"><span data-stu-id="76698-351">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="76698-352">Aggiungere [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) all'indice, le pagine di informazioni e contatti in modo che gli utenti anonimi possono ottenere informazioni sul sito per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="76698-352">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="76698-353">Configurare l'account di test</span><span class="sxs-lookup"><span data-stu-id="76698-353">Configure the test account</span></span>

<span data-ttu-id="76698-354">Il `SeedData` classe crea due account: amministratore e manager.</span><span class="sxs-lookup"><span data-stu-id="76698-354">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="76698-355">Usare la [strumento Secret Manager](xref:security/app-secrets) per impostare una password per questi account.</span><span class="sxs-lookup"><span data-stu-id="76698-355">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="76698-356">Impostare la password dalla directory del progetto (la directory contenente *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="76698-356">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="76698-357">Se non è specificata una password complessa, viene generata un'eccezione quando `SeedData.Initialize` viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="76698-357">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="76698-358">Aggiornamento `Main` di usare la password di test:</span><span class="sxs-lookup"><span data-stu-id="76698-358">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="76698-359">Creare gli account di test e aggiornare i contatti</span><span class="sxs-lookup"><span data-stu-id="76698-359">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="76698-360">Aggiorna il `Initialize` metodo nel `SeedData` classe per creare gli account di test:</span><span class="sxs-lookup"><span data-stu-id="76698-360">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="76698-361">Aggiungere l'ID di utente amministratore e `ContactStatus` ai contatti.</span><span class="sxs-lookup"><span data-stu-id="76698-361">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="76698-362">Rendere uno dei contatti relativi al "Submitted" e "Rejected" uno.</span><span class="sxs-lookup"><span data-stu-id="76698-362">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="76698-363">Aggiungere l'ID utente e lo stato per tutti i contatti.</span><span class="sxs-lookup"><span data-stu-id="76698-363">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="76698-364">Viene visualizzato un solo contatto:</span><span class="sxs-lookup"><span data-stu-id="76698-364">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="76698-365">Creare proprietario, manager e i gestori di autorizzazione di amministratore</span><span class="sxs-lookup"><span data-stu-id="76698-365">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="76698-366">Creare una cartella di *autorizzazione* e crearvi una `ContactIsOwnerAuthorizationHandler` classe.</span><span class="sxs-lookup"><span data-stu-id="76698-366">Create an *Authorization* folder and create a `ContactIsOwnerAuthorizationHandler` class in it.</span></span> <span data-ttu-id="76698-367">Il `ContactIsOwnerAuthorizationHandler` verifica che l'utente che agisce su una risorsa proprietario della risorsa.</span><span class="sxs-lookup"><span data-stu-id="76698-367">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="76698-368">Il `ContactIsOwnerAuthorizationHandler` chiamate [contesto. Esito positivo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) se l'utente autenticato corrente è il proprietario del contatto.</span><span class="sxs-lookup"><span data-stu-id="76698-368">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="76698-369">I gestori di autorizzazione a livello generale:</span><span class="sxs-lookup"><span data-stu-id="76698-369">Authorization handlers generally:</span></span>

* <span data-ttu-id="76698-370">Restituire `context.Succeed` quando vengono soddisfatti i requisiti.</span><span class="sxs-lookup"><span data-stu-id="76698-370">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="76698-371">Restituire `Task.CompletedTask` quando non vengono soddisfatti i requisiti.</span><span class="sxs-lookup"><span data-stu-id="76698-371">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="76698-372">`Task.CompletedTask`non ha esito positivo&mdash;o negativo consente l'esecuzione di altri gestori di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="76698-372">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="76698-373">Se è necessario eseguire in modo esplicito, restituire [contesto. Esito negativo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="76698-373">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="76698-374">L'app consente ai proprietari di contatto di modifica/eliminazione/creare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="76698-374">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="76698-375">`ContactIsOwnerAuthorizationHandler` non è necessario verificare l'operazione passato nel parametro del requisito.</span><span class="sxs-lookup"><span data-stu-id="76698-375">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="76698-376">Creare un gestore di autorizzazione di gestione</span><span class="sxs-lookup"><span data-stu-id="76698-376">Create a manager authorization handler</span></span>

<span data-ttu-id="76698-377">Creare un `ContactManagerAuthorizationHandler` classe la *autorizzazione* cartella.</span><span class="sxs-lookup"><span data-stu-id="76698-377">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="76698-378">Il `ContactManagerAuthorizationHandler` verifica se l'utente che agiscono sulla risorsa è un manager.</span><span class="sxs-lookup"><span data-stu-id="76698-378">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="76698-379">Solo i manager possono approvare o rifiutare le modifiche di contenuto (nuove o modificate).</span><span class="sxs-lookup"><span data-stu-id="76698-379">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="76698-380">Creare un gestore di autorizzazione di amministratore</span><span class="sxs-lookup"><span data-stu-id="76698-380">Create an administrator authorization handler</span></span>

<span data-ttu-id="76698-381">Creare un `ContactAdministratorsAuthorizationHandler` classe la *autorizzazione* cartella.</span><span class="sxs-lookup"><span data-stu-id="76698-381">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="76698-382">Il `ContactAdministratorsAuthorizationHandler` verifica se l'utente che agiscono sulla risorsa è un amministratore.</span><span class="sxs-lookup"><span data-stu-id="76698-382">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="76698-383">Amministratore può eseguire tutte le operazioni.</span><span class="sxs-lookup"><span data-stu-id="76698-383">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="76698-384">Registrare i gestori di eventi di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="76698-384">Register the authorization handlers</span></span>

<span data-ttu-id="76698-385">Servizi usando Entity Framework Core devono essere registrati per [inserimento delle dipendenze](xref:fundamentals/dependency-injection) utilizzando [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="76698-385">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="76698-386">Il `ContactIsOwnerAuthorizationHandler` Usa ASP.NET Core [identità](xref:security/authentication/identity), che si basa su Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="76698-386">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="76698-387">Registrare i gestori con la raccolta di servizio in modo che siano disponibili per il `ContactsController` attraverso [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="76698-387">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="76698-388">Aggiungere il codice seguente alla fine della `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="76698-388">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="76698-389">`ContactAdministratorsAuthorizationHandler` e `ContactManagerAuthorizationHandler` vengono aggiunti come singleton.</span><span class="sxs-lookup"><span data-stu-id="76698-389">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="76698-390">Sono singleton con stato perché non usano Entity Framework e tutte le informazioni necessarie nel `Context` parametro del `HandleRequirementAsync` (metodo).</span><span class="sxs-lookup"><span data-stu-id="76698-390">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="76698-391">Supporto di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="76698-391">Support authorization</span></span>

<span data-ttu-id="76698-392">In questa sezione aggiornare le pagine Razor e aggiungere una classe di requisiti di operazioni.</span><span class="sxs-lookup"><span data-stu-id="76698-392">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="76698-393">Esaminare la classe di requisiti di operazioni contatto</span><span class="sxs-lookup"><span data-stu-id="76698-393">Review the contact operations requirements class</span></span>

<span data-ttu-id="76698-394">Esaminare il `ContactOperations` classe.</span><span class="sxs-lookup"><span data-stu-id="76698-394">Review the `ContactOperations` class.</span></span> <span data-ttu-id="76698-395">Questa classe contiene i requisiti supportato dall'app:</span><span class="sxs-lookup"><span data-stu-id="76698-395">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="76698-396">Creare una classe di base per le pagine Razor di contatti</span><span class="sxs-lookup"><span data-stu-id="76698-396">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="76698-397">Creare una classe base che contiene i servizi usati in Razor Pages i contatti.</span><span class="sxs-lookup"><span data-stu-id="76698-397">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="76698-398">La classe di base inserisce il codice di inizializzazione in un'unica posizione:</span><span class="sxs-lookup"><span data-stu-id="76698-398">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="76698-399">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="76698-399">The preceding code:</span></span>

* <span data-ttu-id="76698-400">Aggiunge il `IAuthorizationService` servizio per accedere ai gestori di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="76698-400">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="76698-401">Aggiunge le identità `UserManager` servizio.</span><span class="sxs-lookup"><span data-stu-id="76698-401">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="76698-402">Aggiungere l'oggetto `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="76698-402">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="76698-403">Aggiornare il CreateModel</span><span class="sxs-lookup"><span data-stu-id="76698-403">Update the CreateModel</span></span>

<span data-ttu-id="76698-404">Aggiornare il costruttore di modello della pagina create per usare il `DI_BasePageModel` classe di base:</span><span class="sxs-lookup"><span data-stu-id="76698-404">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="76698-405">Aggiornamento di `CreateModel.OnPostAsync` metodo:</span><span class="sxs-lookup"><span data-stu-id="76698-405">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="76698-406">Aggiungere l'ID utente per il `Contact` modello.</span><span class="sxs-lookup"><span data-stu-id="76698-406">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="76698-407">Chiamare il gestore di autorizzazione per verificare che l'utente dispone dell'autorizzazione per creare contatti.</span><span class="sxs-lookup"><span data-stu-id="76698-407">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="76698-408">Aggiornare il IndexModel</span><span class="sxs-lookup"><span data-stu-id="76698-408">Update the IndexModel</span></span>

<span data-ttu-id="76698-409">Aggiornamento di `OnGetAsync` metodo in modo che solo i contatti approvati vengono visualizzati agli utenti generici:</span><span class="sxs-lookup"><span data-stu-id="76698-409">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="76698-410">Aggiornare il EditModel</span><span class="sxs-lookup"><span data-stu-id="76698-410">Update the EditModel</span></span>

<span data-ttu-id="76698-411">Aggiungere un gestore di autorizzazione per verificare che l'utente è proprietario del contatto.</span><span class="sxs-lookup"><span data-stu-id="76698-411">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="76698-412">Poiché viene convalidata l'autorizzazione alle risorse, il `[Authorize]` attributo non è sufficiente.</span><span class="sxs-lookup"><span data-stu-id="76698-412">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="76698-413">L'app non ha accesso alla risorsa quando vengono valutati gli attributi.</span><span class="sxs-lookup"><span data-stu-id="76698-413">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="76698-414">Autorizzazione basata sulle risorse deve essere imperativa.</span><span class="sxs-lookup"><span data-stu-id="76698-414">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="76698-415">Controlli devono essere eseguiti dopo che l'app potrà accedere alla risorsa, è possibile caricarli nel modello di pagina oppure caricarlo all'interno del gestore stesso.</span><span class="sxs-lookup"><span data-stu-id="76698-415">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="76698-416">Si accede di frequente della risorsa passando la chiave di risorsa.</span><span class="sxs-lookup"><span data-stu-id="76698-416">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="76698-417">Aggiornare il DeleteModel</span><span class="sxs-lookup"><span data-stu-id="76698-417">Update the DeleteModel</span></span>

<span data-ttu-id="76698-418">Aggiornare il modello di pagina delete per utilizzare il gestore dell'autorizzazione per verificare che l'utente disponga dell'autorizzazione di eliminazione per il contatto.</span><span class="sxs-lookup"><span data-stu-id="76698-418">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="76698-419">Inserire il servizio di autorizzazione in visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="76698-419">Inject the authorization service into the views</span></span>

<span data-ttu-id="76698-420">Attualmente, l'interfaccia utente Mostra modifica ed elimina i collegamenti per i contatti che l'utente non è possibile modificare.</span><span class="sxs-lookup"><span data-stu-id="76698-420">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="76698-421">Inserire il servizio di autorizzazione nel *viewimports* file in modo che sia disponibile per tutte le viste:</span><span class="sxs-lookup"><span data-stu-id="76698-421">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="76698-422">Il markup precedente aggiunge diverse `using` istruzioni.</span><span class="sxs-lookup"><span data-stu-id="76698-422">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="76698-423">Aggiorna il **modifica** e **eliminare** collega in *Pages/Contacts/Index.cshtml* in modo che vengono sottoposti a rendering solo per gli utenti che dispongono delle autorizzazioni appropriate:</span><span class="sxs-lookup"><span data-stu-id="76698-423">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="76698-424">Nascondere i collegamenti da utenti che non dispone dell'autorizzazione per modificare i dati non proteggere le app.</span><span class="sxs-lookup"><span data-stu-id="76698-424">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="76698-425">Nascondere i collegamenti rende più semplici da usare l'app mediante la visualizzazione di collegamenti solo validi.</span><span class="sxs-lookup"><span data-stu-id="76698-425">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="76698-426">Gli utenti possono hack gli URL generati per richiamare modifica ed eliminare le operazioni sui dati che non sono proprietari.</span><span class="sxs-lookup"><span data-stu-id="76698-426">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="76698-427">La pagina Razor o il controller deve applicare controlli di accesso per proteggere i dati.</span><span class="sxs-lookup"><span data-stu-id="76698-427">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="76698-428">Aggiorna i dettagli</span><span class="sxs-lookup"><span data-stu-id="76698-428">Update Details</span></span>

<span data-ttu-id="76698-429">Aggiornare la visualizzazione dei dettagli in modo che i responsabili possono approvare o rifiutare i contatti:</span><span class="sxs-lookup"><span data-stu-id="76698-429">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="76698-430">Aggiornare il modello di pagina dei dettagli:</span><span class="sxs-lookup"><span data-stu-id="76698-430">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="76698-431">Aggiungere o rimuovere un utente a un ruolo</span><span class="sxs-lookup"><span data-stu-id="76698-431">Add or remove a user to a role</span></span>

<span data-ttu-id="76698-432">Visualizzare [questo problema](https://github.com/aspnet/AspNetCore.Docs/issues/8502) per informazioni su:</span><span class="sxs-lookup"><span data-stu-id="76698-432">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="76698-433">Rimozione dei privilegi da un utente.</span><span class="sxs-lookup"><span data-stu-id="76698-433">Removing privileges from a user.</span></span> <span data-ttu-id="76698-434">Ad esempio, disattivare un utente in un'app di chat.</span><span class="sxs-lookup"><span data-stu-id="76698-434">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="76698-435">Aggiunta di privilegi a un utente.</span><span class="sxs-lookup"><span data-stu-id="76698-435">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="76698-436">Testare l'app completata</span><span class="sxs-lookup"><span data-stu-id="76698-436">Test the completed app</span></span>

<span data-ttu-id="76698-437">Se è già stata impostata una password per gli account utente di seeding, usare il [strumento Secret Manager](xref:security/app-secrets#secret-manager) per impostare una password:</span><span class="sxs-lookup"><span data-stu-id="76698-437">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="76698-438">Scegliere una password complessa: Usare otto o più caratteri e almeno un carattere maiuscolo, un numero e un simbolo.</span><span class="sxs-lookup"><span data-stu-id="76698-438">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="76698-439">Ad esempio, `Passw0rd!` soddisfi i requisiti di password complesse.</span><span class="sxs-lookup"><span data-stu-id="76698-439">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="76698-440">Eseguire il comando seguente dalla cartella del progetto, in cui `<PW>` è la password:</span><span class="sxs-lookup"><span data-stu-id="76698-440">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

* <span data-ttu-id="76698-441">Eliminare e aggiornare il database</span><span class="sxs-lookup"><span data-stu-id="76698-441">Drop and update the Database</span></span>

    ```console
     dotnet ef database drop -f
     dotnet ef database update  
     ```

* <span data-ttu-id="76698-442">Riavviare l'app per l'inizializzazione del database.</span><span class="sxs-lookup"><span data-stu-id="76698-442">Restart the app to seed the database.</span></span>

<span data-ttu-id="76698-443">Un modo semplice per testare l'app completata consiste nell'avviare tre diversi browser (o in incognito o InPrivate sessioni).</span><span class="sxs-lookup"><span data-stu-id="76698-443">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="76698-444">In un browser, registrare un nuovo utente (ad esempio, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="76698-444">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="76698-445">Accedere a ogni browser con un altro utente.</span><span class="sxs-lookup"><span data-stu-id="76698-445">Sign in to each browser with a different user.</span></span> <span data-ttu-id="76698-446">Verificare che le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="76698-446">Verify the following operations:</span></span>

* <span data-ttu-id="76698-447">Gli utenti registrati possono visualizzare tutti i dati di contatto approvati.</span><span class="sxs-lookup"><span data-stu-id="76698-447">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="76698-448">Gli utenti registrati possono modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="76698-448">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="76698-449">I responsabili possono approvare o rifiutare i dati di contatto.</span><span class="sxs-lookup"><span data-stu-id="76698-449">Managers can approve/reject contact data.</span></span> <span data-ttu-id="76698-450">Il `Details` viene mostrata **Approva** e **rifiutare** pulsanti.</span><span class="sxs-lookup"><span data-stu-id="76698-450">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="76698-451">Gli amministratori possono approvare o rifiutare e modificare/eliminare tutti i dati.</span><span class="sxs-lookup"><span data-stu-id="76698-451">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="76698-452">Utente</span><span class="sxs-lookup"><span data-stu-id="76698-452">User</span></span>                | <span data-ttu-id="76698-453">Esegue il seeding dell'app</span><span class="sxs-lookup"><span data-stu-id="76698-453">Seeded by the app</span></span> | <span data-ttu-id="76698-454">Opzioni</span><span class="sxs-lookup"><span data-stu-id="76698-454">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="76698-455">No</span><span class="sxs-lookup"><span data-stu-id="76698-455">No</span></span>                | <span data-ttu-id="76698-456">Modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="76698-456">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="76698-457">Yes</span><span class="sxs-lookup"><span data-stu-id="76698-457">Yes</span></span>               | <span data-ttu-id="76698-458">Approvare o rifiutare e modificare/eliminare i dati personalizzati.</span><span class="sxs-lookup"><span data-stu-id="76698-458">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="76698-459">Yes</span><span class="sxs-lookup"><span data-stu-id="76698-459">Yes</span></span>               | <span data-ttu-id="76698-460">Approvare o rifiutare e modificare/eliminare tutti i dati.</span><span class="sxs-lookup"><span data-stu-id="76698-460">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="76698-461">Creare un contatto nel browser dell'amministratore.</span><span class="sxs-lookup"><span data-stu-id="76698-461">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="76698-462">Copiare l'URL per l'eliminazione e modifica di contattare l'amministratore.</span><span class="sxs-lookup"><span data-stu-id="76698-462">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="76698-463">Incollare questi collegamenti nel browser dell'utente test per verificare che l'utente di test non è possibile eseguire queste operazioni.</span><span class="sxs-lookup"><span data-stu-id="76698-463">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="76698-464">Creare l'app di base</span><span class="sxs-lookup"><span data-stu-id="76698-464">Create the starter app</span></span>

* <span data-ttu-id="76698-465">Creare un'app Razor Pages denominata "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="76698-465">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="76698-466">Creare l'app con **singoli account utente**.</span><span class="sxs-lookup"><span data-stu-id="76698-466">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="76698-467">Il nome "ContactManager" in modo che lo spazio dei nomi corrispondente dello spazio dei nomi utilizzato nell'esempio.</span><span class="sxs-lookup"><span data-stu-id="76698-467">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="76698-468">`-uld` Specifica di Local DB invece di SQLite</span><span class="sxs-lookup"><span data-stu-id="76698-468">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="76698-469">Aggiungi *modelli/Contact. cs*:</span><span class="sxs-lookup"><span data-stu-id="76698-469">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="76698-470">Scaffolding di `Contact` modello.</span><span class="sxs-lookup"><span data-stu-id="76698-470">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="76698-471">Creare la migrazione iniziale e l'aggiornamento del database:</span><span class="sxs-lookup"><span data-stu-id="76698-471">Create initial migration and update the database:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* <span data-ttu-id="76698-472">Aggiorna il **ContactManager** ancoraggio nel *cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="76698-472">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* <span data-ttu-id="76698-473">Testare l'app per la creazione, modifica ed eliminazione di un contatto</span><span class="sxs-lookup"><span data-stu-id="76698-473">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="76698-474">Specificare il valore di inizializzazione del database</span><span class="sxs-lookup"><span data-stu-id="76698-474">Seed the database</span></span>

<span data-ttu-id="76698-475">Aggiungere il [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) classe per il *dati* cartella.</span><span class="sxs-lookup"><span data-stu-id="76698-475">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="76698-476">Chiamare `SeedData.Initialize` da `Main`:</span><span class="sxs-lookup"><span data-stu-id="76698-476">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="76698-477">Verificare che l'app effettuato il seeding del database.</span><span class="sxs-lookup"><span data-stu-id="76698-477">Test that the app seeded the database.</span></span> <span data-ttu-id="76698-478">Se sono presenti tutte le righe nel DB del contatto, il metodo di inizializzazione non viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="76698-478">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="76698-479">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="76698-479">Additional resources</span></span>

* [<span data-ttu-id="76698-480">Creare un'app web .NET Core e Database SQL di Azure App Service</span><span class="sxs-lookup"><span data-stu-id="76698-480">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="76698-481">[ASP.NET Core autorizzazione Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="76698-481">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="76698-482">Questa esercitazione descrive in dettaglio più le funzionalità di sicurezza introdotte in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="76698-482">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="76698-483">Autorizzazione personalizzata basata su criteri</span><span class="sxs-lookup"><span data-stu-id="76698-483">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
