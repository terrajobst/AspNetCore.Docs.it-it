---
title: Creare un'app ASP.NET Core con i dati utente protetti da autorizzazione
author: rick-anderson
description: Informazioni su come creare un'app Razor Pages con i dati utente protetti da autorizzazione. Include HTTPS, l'autenticazione, sicurezza, ASP.NET Core Identity.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: 225d0e3aa51745253d03e614b1c8568b3a6ba731
ms.sourcegitcommit: 983b31449fe398e6e922eb13e9eb6f4287ec91e8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/24/2019
ms.locfileid: "70017491"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="ff110-104">Creare un'app ASP.NET Core con i dati utente protetti da autorizzazione</span><span class="sxs-lookup"><span data-stu-id="ff110-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="ff110-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="ff110-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="ff110-106">Visualizzare [questo PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) per la versione di ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="ff110-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="ff110-107">La versione di ASP.NET Core 1.1 di questa esercitazione è nel [ciò](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) cartella.</span><span class="sxs-lookup"><span data-stu-id="ff110-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="ff110-108">1\.1 esempio ASP.NET Core è nel [esempi](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="ff110-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="ff110-109">Vedere [questo pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="ff110-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ff110-110">Questa esercitazione illustra come creare un'app web ASP.NET Core con i dati utente protetti da autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="ff110-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="ff110-111">Viene visualizzato un elenco di contatti che (registrati) agli utenti autenticati hanno creato.</span><span class="sxs-lookup"><span data-stu-id="ff110-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="ff110-112">Sono disponibili tre gruppi di sicurezza:</span><span class="sxs-lookup"><span data-stu-id="ff110-112">There are three security groups:</span></span>

* <span data-ttu-id="ff110-113">**Utenti registrati** può visualizzare tutti i dati approvati e può modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="ff110-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="ff110-114">**I responsabili** possono approvare o rifiutare i dati di contatto.</span><span class="sxs-lookup"><span data-stu-id="ff110-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="ff110-115">Solo i contatti approvati sono visibili agli utenti.</span><span class="sxs-lookup"><span data-stu-id="ff110-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="ff110-116">**Gli amministratori** può approvare o rifiutare e modificare/eliminare tutti i dati.</span><span class="sxs-lookup"><span data-stu-id="ff110-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="ff110-117">Le immagini in questo documento non corrispondono esattamente ai modelli più recenti.</span><span class="sxs-lookup"><span data-stu-id="ff110-117">The images in this document don't exactly match the latest templates.</span></span>

<span data-ttu-id="ff110-118">Nell'immagine seguente, l'utente Rick (`rick@example.com`) ha effettuato l'accesso.</span><span class="sxs-lookup"><span data-stu-id="ff110-118">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="ff110-119">Rick possono visualizzare solo i contatti approvati e **Edit**/**eliminare**/**Crea nuovo** collegamenti per il suo i contatti.</span><span class="sxs-lookup"><span data-stu-id="ff110-119">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="ff110-120">Solo l'ultimo record, creato da Rick, consente di visualizzare **Edit** e **eliminare** collegamenti.</span><span class="sxs-lookup"><span data-stu-id="ff110-120">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="ff110-121">Gli altri utenti non vedono l'ultimo record fino a quando un responsabile o l'amministratore assume lo stato "Approvato".</span><span class="sxs-lookup"><span data-stu-id="ff110-121">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Screenshot che mostra Rick effettuato l'accesso](secure-data/_static/rick.png)

<span data-ttu-id="ff110-123">Nell'immagine seguente, `manager@contoso.com` è stato eseguito l'accesso e nel ruolo del Manager:</span><span class="sxs-lookup"><span data-stu-id="ff110-123">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Screenshot che mostra manager@contoso.com effettuato l'accesso](secure-data/_static/manager1.png)

<span data-ttu-id="ff110-125">L'immagine seguente mostra i gestori visualizzazione dettagli di un contatto:</span><span class="sxs-lookup"><span data-stu-id="ff110-125">The following image shows the managers details view of a contact:</span></span>

![Visualizzazione di gestione di un contatto](secure-data/_static/manager.png)

<span data-ttu-id="ff110-127">Il **Approve** e **rifiutare** pulsanti vengono visualizzati solo per i gestori e amministratori.</span><span class="sxs-lookup"><span data-stu-id="ff110-127">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="ff110-128">Nell'immagine seguente, `admin@contoso.com` è stato eseguito l'accesso e il ruolo dell'amministratore:</span><span class="sxs-lookup"><span data-stu-id="ff110-128">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Screenshot che mostra admin@contoso.com effettuato l'accesso](secure-data/_static/admin.png)

<span data-ttu-id="ff110-130">L'amministratore ha tutti i privilegi.</span><span class="sxs-lookup"><span data-stu-id="ff110-130">The administrator has all privileges.</span></span> <span data-ttu-id="ff110-131">Può leggere, modificare ed eliminare un contatto e modificare lo stato dei contatti.</span><span class="sxs-lookup"><span data-stu-id="ff110-131">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="ff110-132">L'app è stato creato da [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) seguenti `Contact` modello:</span><span class="sxs-lookup"><span data-stu-id="ff110-132">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="ff110-133">L'esempio contiene i gestori di autorizzazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="ff110-133">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="ff110-134">`ContactIsOwnerAuthorizationHandler`: Garantisce che un utente possa modificare solo i propri dati.</span><span class="sxs-lookup"><span data-stu-id="ff110-134">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="ff110-135">`ContactManagerAuthorizationHandler`: Consente ai responsabili di approvare o rifiutare i contatti.</span><span class="sxs-lookup"><span data-stu-id="ff110-135">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="ff110-136">`ContactAdministratorsAuthorizationHandler`: Consente agli amministratori di approvare o rifiutare i contatti e di modificare/eliminare i contatti.</span><span class="sxs-lookup"><span data-stu-id="ff110-136">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ff110-137">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="ff110-137">Prerequisites</span></span>

<span data-ttu-id="ff110-138">Questa esercitazione viene fatto avanzare.</span><span class="sxs-lookup"><span data-stu-id="ff110-138">This tutorial is advanced.</span></span> <span data-ttu-id="ff110-139">È necessario avere familiarità con:</span><span class="sxs-lookup"><span data-stu-id="ff110-139">You should be familiar with:</span></span>

* [<span data-ttu-id="ff110-140">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ff110-140">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="ff110-141">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="ff110-141">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="ff110-142">Conferma account e recupero password</span><span class="sxs-lookup"><span data-stu-id="ff110-142">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="ff110-143">Autorizzazione</span><span class="sxs-lookup"><span data-stu-id="ff110-143">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="ff110-144">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="ff110-144">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="ff110-145">Starter e app completata</span><span class="sxs-lookup"><span data-stu-id="ff110-145">The starter and completed app</span></span>

<span data-ttu-id="ff110-146">[Scaricare](xref:index#how-to-download-a-sample) il [completato](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span><span class="sxs-lookup"><span data-stu-id="ff110-146">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="ff110-147">[Test](#test-the-completed-app) dell'app completata in modo da avere acquisito familiarità con le funzionalità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="ff110-147">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="ff110-148">L'app di base</span><span class="sxs-lookup"><span data-stu-id="ff110-148">The starter app</span></span>

<span data-ttu-id="ff110-149">[Scaricare](xref:index#how-to-download-a-sample) il [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span><span class="sxs-lookup"><span data-stu-id="ff110-149">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="ff110-150">Eseguire l'app, toccare il **ContactManager** collegare e verificare è possibile creare, modificare ed eliminare un contatto.</span><span class="sxs-lookup"><span data-stu-id="ff110-150">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="ff110-151">Proteggere i dati utente</span><span class="sxs-lookup"><span data-stu-id="ff110-151">Secure user data</span></span>

<span data-ttu-id="ff110-152">Le sezioni seguenti includono tutti i passaggi principali per creare l'app per dati sicura degli utenti.</span><span class="sxs-lookup"><span data-stu-id="ff110-152">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="ff110-153">Si può risultare utile per fare riferimento al progetto completato.</span><span class="sxs-lookup"><span data-stu-id="ff110-153">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="ff110-154">Associare i dati di contatto per l'utente</span><span class="sxs-lookup"><span data-stu-id="ff110-154">Tie the contact data to the user</span></span>

<span data-ttu-id="ff110-155">Usare ASP.NET [identità](xref:security/authentication/identity) ID utente per garantire che gli utenti possono modificare i propri dati, ma non altri dati degli utenti.</span><span class="sxs-lookup"><span data-stu-id="ff110-155">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="ff110-156">Aggiungere `OwnerID` e `ContactStatus` per il `Contact` modello:</span><span class="sxs-lookup"><span data-stu-id="ff110-156">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="ff110-157">`OwnerID` è l'ID dell'utente dal `AspNetUser` tabella di [identità](xref:security/authentication/identity) database.</span><span class="sxs-lookup"><span data-stu-id="ff110-157">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="ff110-158">Il `Status` campo determina se un contatto visibile dagli utenti generali.</span><span class="sxs-lookup"><span data-stu-id="ff110-158">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="ff110-159">Creare una nuova migrazione e aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="ff110-159">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="ff110-160">Aggiungere altri servizi di identità</span><span class="sxs-lookup"><span data-stu-id="ff110-160">Add Role services to Identity</span></span>

<span data-ttu-id="ff110-161">Accodare [Aggiungi ruoli](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) per aggiungere servizi ruolo:</span><span class="sxs-lookup"><span data-stu-id="ff110-161">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a><span data-ttu-id="ff110-162">Richiedere agli utenti autenticati</span><span class="sxs-lookup"><span data-stu-id="ff110-162">Require authenticated users</span></span>

<span data-ttu-id="ff110-163">Impostare i criteri di autenticazione predefinito per richiedere agli utenti di essere autenticato:</span><span class="sxs-lookup"><span data-stu-id="ff110-163">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 <span data-ttu-id="ff110-164">È possibile rifiutare esplicitamente l'autenticazione a livello di metodo di pagina Razor, controller o azione con il `[AllowAnonymous]` attributo.</span><span class="sxs-lookup"><span data-stu-id="ff110-164">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="ff110-165">L'impostazione di criteri di autenticazione predefinito per richiedere agli utenti di essere autenticati protegge appena aggiunto Razor Pages e controller.</span><span class="sxs-lookup"><span data-stu-id="ff110-165">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="ff110-166">La presenza di autenticazione richiesto per impostazione predefinita è più sicuro rispetto al semplice uso nuovi controller e le pagine Razor per includere il `[Authorize]` attributo.</span><span class="sxs-lookup"><span data-stu-id="ff110-166">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="ff110-167">Aggiungere [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) alle pagine indici e privacy in modo che gli utenti anonimi possano ottenere informazioni sul sito prima della registrazione.</span><span class="sxs-lookup"><span data-stu-id="ff110-167">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index and Privacy pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a><span data-ttu-id="ff110-168">Configurare l'account di test</span><span class="sxs-lookup"><span data-stu-id="ff110-168">Configure the test account</span></span>

<span data-ttu-id="ff110-169">Il `SeedData` classe crea due account: amministratore e manager.</span><span class="sxs-lookup"><span data-stu-id="ff110-169">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="ff110-170">Usare la [strumento Secret Manager](xref:security/app-secrets) per impostare una password per questi account.</span><span class="sxs-lookup"><span data-stu-id="ff110-170">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="ff110-171">Impostare la password dalla directory del progetto (la directory contenente *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="ff110-171">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="ff110-172">Se non è specificata una password complessa, viene generata un'eccezione quando `SeedData.Initialize` viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="ff110-172">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="ff110-173">Aggiornamento `Main` di usare la password di test:</span><span class="sxs-lookup"><span data-stu-id="ff110-173">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="ff110-174">Creare gli account di test e aggiornare i contatti</span><span class="sxs-lookup"><span data-stu-id="ff110-174">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="ff110-175">Aggiorna il `Initialize` metodo nel `SeedData` classe per creare gli account di test:</span><span class="sxs-lookup"><span data-stu-id="ff110-175">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="ff110-176">Aggiungere l'ID di utente amministratore e `ContactStatus` ai contatti.</span><span class="sxs-lookup"><span data-stu-id="ff110-176">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="ff110-177">Rendere uno dei contatti relativi al "Submitted" e "Rejected" uno.</span><span class="sxs-lookup"><span data-stu-id="ff110-177">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="ff110-178">Aggiungere l'ID utente e lo stato per tutti i contatti.</span><span class="sxs-lookup"><span data-stu-id="ff110-178">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="ff110-179">Viene visualizzato un solo contatto:</span><span class="sxs-lookup"><span data-stu-id="ff110-179">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="ff110-180">Creare proprietario, manager e i gestori di autorizzazione di amministratore</span><span class="sxs-lookup"><span data-stu-id="ff110-180">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="ff110-181">Creare un `ContactIsOwnerAuthorizationHandler` classe la *autorizzazione* cartella.</span><span class="sxs-lookup"><span data-stu-id="ff110-181">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="ff110-182">Il `ContactIsOwnerAuthorizationHandler` verifica che l'utente che agisce su una risorsa proprietario della risorsa.</span><span class="sxs-lookup"><span data-stu-id="ff110-182">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="ff110-183">Il `ContactIsOwnerAuthorizationHandler` chiamate [contesto. Esito positivo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) se l'utente autenticato corrente è il proprietario del contatto.</span><span class="sxs-lookup"><span data-stu-id="ff110-183">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="ff110-184">I gestori di autorizzazione a livello generale:</span><span class="sxs-lookup"><span data-stu-id="ff110-184">Authorization handlers generally:</span></span>

* <span data-ttu-id="ff110-185">Restituire `context.Succeed` quando vengono soddisfatti i requisiti.</span><span class="sxs-lookup"><span data-stu-id="ff110-185">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="ff110-186">Restituire `Task.CompletedTask` quando non vengono soddisfatti i requisiti.</span><span class="sxs-lookup"><span data-stu-id="ff110-186">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="ff110-187">`Task.CompletedTask`non ha esito positivo&mdash;o negativo consente l'esecuzione di altri gestori di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="ff110-187">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="ff110-188">Se è necessario eseguire in modo esplicito, restituire [contesto. Esito negativo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="ff110-188">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="ff110-189">L'app consente ai proprietari di contatto di modifica/eliminazione/creare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="ff110-189">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="ff110-190">`ContactIsOwnerAuthorizationHandler` non è necessario verificare l'operazione passato nel parametro del requisito.</span><span class="sxs-lookup"><span data-stu-id="ff110-190">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="ff110-191">Creare un gestore di autorizzazione di gestione</span><span class="sxs-lookup"><span data-stu-id="ff110-191">Create a manager authorization handler</span></span>

<span data-ttu-id="ff110-192">Creare un `ContactManagerAuthorizationHandler` classe la *autorizzazione* cartella.</span><span class="sxs-lookup"><span data-stu-id="ff110-192">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="ff110-193">Il `ContactManagerAuthorizationHandler` verifica se l'utente che agiscono sulla risorsa è un manager.</span><span class="sxs-lookup"><span data-stu-id="ff110-193">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="ff110-194">Solo i manager possono approvare o rifiutare le modifiche di contenuto (nuove o modificate).</span><span class="sxs-lookup"><span data-stu-id="ff110-194">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="ff110-195">Creare un gestore di autorizzazione di amministratore</span><span class="sxs-lookup"><span data-stu-id="ff110-195">Create an administrator authorization handler</span></span>

<span data-ttu-id="ff110-196">Creare un `ContactAdministratorsAuthorizationHandler` classe la *autorizzazione* cartella.</span><span class="sxs-lookup"><span data-stu-id="ff110-196">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="ff110-197">Il `ContactAdministratorsAuthorizationHandler` verifica se l'utente che agiscono sulla risorsa è un amministratore.</span><span class="sxs-lookup"><span data-stu-id="ff110-197">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="ff110-198">Amministratore può eseguire tutte le operazioni.</span><span class="sxs-lookup"><span data-stu-id="ff110-198">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="ff110-199">Registrare i gestori di eventi di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="ff110-199">Register the authorization handlers</span></span>

<span data-ttu-id="ff110-200">Servizi usando Entity Framework Core devono essere registrati per [inserimento delle dipendenze](xref:fundamentals/dependency-injection) utilizzando [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="ff110-200">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="ff110-201">Il `ContactIsOwnerAuthorizationHandler` Usa ASP.NET Core [identità](xref:security/authentication/identity), che si basa su Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ff110-201">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="ff110-202">Registrare i gestori con la raccolta di servizio in modo che siano disponibili per il `ContactsController` attraverso [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ff110-202">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="ff110-203">Aggiungere il codice seguente alla fine della `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ff110-203">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

<span data-ttu-id="ff110-204">`ContactAdministratorsAuthorizationHandler` e `ContactManagerAuthorizationHandler` vengono aggiunti come singleton.</span><span class="sxs-lookup"><span data-stu-id="ff110-204">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="ff110-205">Sono singleton con stato perché non usano Entity Framework e tutte le informazioni necessarie nel `Context` parametro del `HandleRequirementAsync` (metodo).</span><span class="sxs-lookup"><span data-stu-id="ff110-205">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="ff110-206">Supporto di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="ff110-206">Support authorization</span></span>

<span data-ttu-id="ff110-207">In questa sezione aggiornare le pagine Razor e aggiungere una classe di requisiti di operazioni.</span><span class="sxs-lookup"><span data-stu-id="ff110-207">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="ff110-208">Esaminare la classe di requisiti di operazioni contatto</span><span class="sxs-lookup"><span data-stu-id="ff110-208">Review the contact operations requirements class</span></span>

<span data-ttu-id="ff110-209">Esaminare il `ContactOperations` classe.</span><span class="sxs-lookup"><span data-stu-id="ff110-209">Review the `ContactOperations` class.</span></span> <span data-ttu-id="ff110-210">Questa classe contiene i requisiti supportato dall'app:</span><span class="sxs-lookup"><span data-stu-id="ff110-210">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="ff110-211">Creare una classe di base per le pagine Razor di contatti</span><span class="sxs-lookup"><span data-stu-id="ff110-211">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="ff110-212">Creare una classe base che contiene i servizi usati in Razor Pages i contatti.</span><span class="sxs-lookup"><span data-stu-id="ff110-212">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="ff110-213">La classe di base inserisce il codice di inizializzazione in un'unica posizione:</span><span class="sxs-lookup"><span data-stu-id="ff110-213">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="ff110-214">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="ff110-214">The preceding code:</span></span>

* <span data-ttu-id="ff110-215">Aggiunge il `IAuthorizationService` servizio per accedere ai gestori di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="ff110-215">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="ff110-216">Aggiunge le identità `UserManager` servizio.</span><span class="sxs-lookup"><span data-stu-id="ff110-216">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="ff110-217">Aggiungere l'oggetto `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="ff110-217">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="ff110-218">Aggiornare il CreateModel</span><span class="sxs-lookup"><span data-stu-id="ff110-218">Update the CreateModel</span></span>

<span data-ttu-id="ff110-219">Aggiornare il costruttore di modello della pagina create per usare il `DI_BasePageModel` classe di base:</span><span class="sxs-lookup"><span data-stu-id="ff110-219">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="ff110-220">Aggiornamento di `CreateModel.OnPostAsync` metodo:</span><span class="sxs-lookup"><span data-stu-id="ff110-220">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="ff110-221">Aggiungere l'ID utente per il `Contact` modello.</span><span class="sxs-lookup"><span data-stu-id="ff110-221">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="ff110-222">Chiamare il gestore di autorizzazione per verificare che l'utente dispone dell'autorizzazione per creare contatti.</span><span class="sxs-lookup"><span data-stu-id="ff110-222">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="ff110-223">Aggiornare il IndexModel</span><span class="sxs-lookup"><span data-stu-id="ff110-223">Update the IndexModel</span></span>

<span data-ttu-id="ff110-224">Aggiornamento di `OnGetAsync` metodo in modo che solo i contatti approvati vengono visualizzati agli utenti generici:</span><span class="sxs-lookup"><span data-stu-id="ff110-224">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="ff110-225">Aggiornare il EditModel</span><span class="sxs-lookup"><span data-stu-id="ff110-225">Update the EditModel</span></span>

<span data-ttu-id="ff110-226">Aggiungere un gestore di autorizzazione per verificare che l'utente è proprietario del contatto.</span><span class="sxs-lookup"><span data-stu-id="ff110-226">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="ff110-227">Poiché viene convalidata l'autorizzazione alle risorse, il `[Authorize]` attributo non è sufficiente.</span><span class="sxs-lookup"><span data-stu-id="ff110-227">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="ff110-228">L'app non ha accesso alla risorsa quando vengono valutati gli attributi.</span><span class="sxs-lookup"><span data-stu-id="ff110-228">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="ff110-229">Autorizzazione basata sulle risorse deve essere imperativa.</span><span class="sxs-lookup"><span data-stu-id="ff110-229">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="ff110-230">Controlli devono essere eseguiti dopo che l'app potrà accedere alla risorsa, è possibile caricarli nel modello di pagina oppure caricarlo all'interno del gestore stesso.</span><span class="sxs-lookup"><span data-stu-id="ff110-230">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="ff110-231">Si accede di frequente della risorsa passando la chiave di risorsa.</span><span class="sxs-lookup"><span data-stu-id="ff110-231">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="ff110-232">Aggiornare il DeleteModel</span><span class="sxs-lookup"><span data-stu-id="ff110-232">Update the DeleteModel</span></span>

<span data-ttu-id="ff110-233">Aggiornare il modello di pagina delete per utilizzare il gestore dell'autorizzazione per verificare che l'utente disponga dell'autorizzazione di eliminazione per il contatto.</span><span class="sxs-lookup"><span data-stu-id="ff110-233">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="ff110-234">Inserire il servizio di autorizzazione in visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="ff110-234">Inject the authorization service into the views</span></span>

<span data-ttu-id="ff110-235">Attualmente, l'interfaccia utente Mostra modifica ed elimina i collegamenti per i contatti che l'utente non è possibile modificare.</span><span class="sxs-lookup"><span data-stu-id="ff110-235">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="ff110-236">Inserire il servizio di autorizzazione nel file *pages/_ViewImports. cshtml* in modo che sia disponibile per tutte le visualizzazioni:</span><span class="sxs-lookup"><span data-stu-id="ff110-236">Inject the authorization service in the *Pages/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="ff110-237">Il markup precedente aggiunge diverse `using` istruzioni.</span><span class="sxs-lookup"><span data-stu-id="ff110-237">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="ff110-238">Aggiorna il **modifica** e **eliminare** collega in *Pages/Contacts/Index.cshtml* in modo che vengono sottoposti a rendering solo per gli utenti che dispongono delle autorizzazioni appropriate:</span><span class="sxs-lookup"><span data-stu-id="ff110-238">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="ff110-239">Nascondere i collegamenti da utenti che non dispone dell'autorizzazione per modificare i dati non proteggere le app.</span><span class="sxs-lookup"><span data-stu-id="ff110-239">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="ff110-240">Nascondere i collegamenti rende più semplici da usare l'app mediante la visualizzazione di collegamenti solo validi.</span><span class="sxs-lookup"><span data-stu-id="ff110-240">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="ff110-241">Gli utenti possono hack gli URL generati per richiamare modifica ed eliminare le operazioni sui dati che non sono proprietari.</span><span class="sxs-lookup"><span data-stu-id="ff110-241">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="ff110-242">La pagina Razor o il controller deve applicare controlli di accesso per proteggere i dati.</span><span class="sxs-lookup"><span data-stu-id="ff110-242">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="ff110-243">Aggiorna i dettagli</span><span class="sxs-lookup"><span data-stu-id="ff110-243">Update Details</span></span>

<span data-ttu-id="ff110-244">Aggiornare la visualizzazione dei dettagli in modo che i responsabili possono approvare o rifiutare i contatti:</span><span class="sxs-lookup"><span data-stu-id="ff110-244">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="ff110-245">Aggiornare il modello di pagina dei dettagli:</span><span class="sxs-lookup"><span data-stu-id="ff110-245">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="ff110-246">Aggiungere o rimuovere un utente a un ruolo</span><span class="sxs-lookup"><span data-stu-id="ff110-246">Add or remove a user to a role</span></span>

<span data-ttu-id="ff110-247">Visualizzare [questo problema](https://github.com/aspnet/AspNetCore.Docs/issues/8502) per informazioni su:</span><span class="sxs-lookup"><span data-stu-id="ff110-247">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="ff110-248">Rimozione dei privilegi da un utente.</span><span class="sxs-lookup"><span data-stu-id="ff110-248">Removing privileges from a user.</span></span> <span data-ttu-id="ff110-249">Ad esempio, disattivare un utente in un'app di chat.</span><span class="sxs-lookup"><span data-stu-id="ff110-249">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="ff110-250">Aggiunta di privilegi a un utente.</span><span class="sxs-lookup"><span data-stu-id="ff110-250">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="ff110-251">Testare l'app completata</span><span class="sxs-lookup"><span data-stu-id="ff110-251">Test the completed app</span></span>

<span data-ttu-id="ff110-252">Se è già stata impostata una password per gli account utente di seeding, usare il [strumento Secret Manager](xref:security/app-secrets#secret-manager) per impostare una password:</span><span class="sxs-lookup"><span data-stu-id="ff110-252">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="ff110-253">Scegliere una password complessa: Usare otto o più caratteri e almeno un carattere maiuscolo, un numero e un simbolo.</span><span class="sxs-lookup"><span data-stu-id="ff110-253">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="ff110-254">Ad esempio, `Passw0rd!` soddisfi i requisiti di password complesse.</span><span class="sxs-lookup"><span data-stu-id="ff110-254">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="ff110-255">Eseguire il comando seguente dalla cartella del progetto, in cui `<PW>` è la password:</span><span class="sxs-lookup"><span data-stu-id="ff110-255">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

<span data-ttu-id="ff110-256">Se l'app ha contatti:</span><span class="sxs-lookup"><span data-stu-id="ff110-256">If the app has contacts:</span></span>

* <span data-ttu-id="ff110-257">Eliminare tutti i record nel `Contact` tabella.</span><span class="sxs-lookup"><span data-stu-id="ff110-257">Delete all of the records in the `Contact` table.</span></span>
* <span data-ttu-id="ff110-258">Riavviare l'app per l'inizializzazione del database.</span><span class="sxs-lookup"><span data-stu-id="ff110-258">Restart the app to seed the database.</span></span>

<span data-ttu-id="ff110-259">Un modo semplice per testare l'app completata consiste nell'avviare tre diversi browser (o in incognito o InPrivate sessioni).</span><span class="sxs-lookup"><span data-stu-id="ff110-259">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="ff110-260">In un browser, registrare un nuovo utente (ad esempio, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="ff110-260">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="ff110-261">Accedere a ogni browser con un altro utente.</span><span class="sxs-lookup"><span data-stu-id="ff110-261">Sign in to each browser with a different user.</span></span> <span data-ttu-id="ff110-262">Verificare che le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="ff110-262">Verify the following operations:</span></span>

* <span data-ttu-id="ff110-263">Gli utenti registrati possono visualizzare tutti i dati di contatto approvati.</span><span class="sxs-lookup"><span data-stu-id="ff110-263">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="ff110-264">Gli utenti registrati possono modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="ff110-264">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="ff110-265">I responsabili possono approvare o rifiutare i dati di contatto.</span><span class="sxs-lookup"><span data-stu-id="ff110-265">Managers can approve/reject contact data.</span></span> <span data-ttu-id="ff110-266">Il `Details` viene mostrata **Approva** e **rifiutare** pulsanti.</span><span class="sxs-lookup"><span data-stu-id="ff110-266">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="ff110-267">Gli amministratori possono approvare o rifiutare e modificare/eliminare tutti i dati.</span><span class="sxs-lookup"><span data-stu-id="ff110-267">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="ff110-268">Utente</span><span class="sxs-lookup"><span data-stu-id="ff110-268">User</span></span>                | <span data-ttu-id="ff110-269">Esegue il seeding dell'app</span><span class="sxs-lookup"><span data-stu-id="ff110-269">Seeded by the app</span></span> | <span data-ttu-id="ff110-270">Opzioni</span><span class="sxs-lookup"><span data-stu-id="ff110-270">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="ff110-271">No</span><span class="sxs-lookup"><span data-stu-id="ff110-271">No</span></span>                | <span data-ttu-id="ff110-272">Modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="ff110-272">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="ff110-273">Yes</span><span class="sxs-lookup"><span data-stu-id="ff110-273">Yes</span></span>               | <span data-ttu-id="ff110-274">Approvare o rifiutare e modificare/eliminare i dati personalizzati.</span><span class="sxs-lookup"><span data-stu-id="ff110-274">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="ff110-275">Yes</span><span class="sxs-lookup"><span data-stu-id="ff110-275">Yes</span></span>               | <span data-ttu-id="ff110-276">Approvare o rifiutare e modificare/eliminare tutti i dati.</span><span class="sxs-lookup"><span data-stu-id="ff110-276">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="ff110-277">Creare un contatto nel browser dell'amministratore.</span><span class="sxs-lookup"><span data-stu-id="ff110-277">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="ff110-278">Copiare l'URL per l'eliminazione e modifica di contattare l'amministratore.</span><span class="sxs-lookup"><span data-stu-id="ff110-278">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="ff110-279">Incollare questi collegamenti nel browser dell'utente test per verificare che l'utente di test non è possibile eseguire queste operazioni.</span><span class="sxs-lookup"><span data-stu-id="ff110-279">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="ff110-280">Creare l'app di base</span><span class="sxs-lookup"><span data-stu-id="ff110-280">Create the starter app</span></span>

* <span data-ttu-id="ff110-281">Creare un'app Razor Pages denominata "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="ff110-281">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="ff110-282">Creare l'app con **singoli account utente**.</span><span class="sxs-lookup"><span data-stu-id="ff110-282">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="ff110-283">Il nome "ContactManager" in modo che lo spazio dei nomi corrispondente dello spazio dei nomi utilizzato nell'esempio.</span><span class="sxs-lookup"><span data-stu-id="ff110-283">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="ff110-284">`-uld` Specifica di Local DB invece di SQLite</span><span class="sxs-lookup"><span data-stu-id="ff110-284">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="ff110-285">Aggiungi *modelli/Contact. cs*:</span><span class="sxs-lookup"><span data-stu-id="ff110-285">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="ff110-286">Scaffolding di `Contact` modello.</span><span class="sxs-lookup"><span data-stu-id="ff110-286">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="ff110-287">Creare la migrazione iniziale e l'aggiornamento del database:</span><span class="sxs-lookup"><span data-stu-id="ff110-287">Create initial migration and update the database:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
  ```

<span data-ttu-id="ff110-288">Se si verifica un bug con il `dotnet aspnet-codegenerator razorpage` comando, vedere [questo problema di GitHub](https://github.com/aspnet/Scaffolding/issues/984).</span><span class="sxs-lookup"><span data-stu-id="ff110-288">If you experience a bug with the `dotnet aspnet-codegenerator razorpage` command, see [this GitHub issue](https://github.com/aspnet/Scaffolding/issues/984).</span></span>

* <span data-ttu-id="ff110-289">Aggiornare l'ancoraggio **ContactManager** nel file *pages/Shared/file. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="ff110-289">Update the **ContactManager** anchor in the *Pages/Shared/_Layout.cshtml* file:</span></span>

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* <span data-ttu-id="ff110-290">Testare l'app per la creazione, modifica ed eliminazione di un contatto</span><span class="sxs-lookup"><span data-stu-id="ff110-290">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="ff110-291">Specificare il valore di inizializzazione del database</span><span class="sxs-lookup"><span data-stu-id="ff110-291">Seed the database</span></span>

<span data-ttu-id="ff110-292">Aggiungere la classe [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) alla cartella *Data* :</span><span class="sxs-lookup"><span data-stu-id="ff110-292">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) class to the *Data* folder:</span></span>

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

<span data-ttu-id="ff110-293">Chiamare `SeedData.Initialize` da `Main`:</span><span class="sxs-lookup"><span data-stu-id="ff110-293">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

<span data-ttu-id="ff110-294">Verificare che l'app effettuato il seeding del database.</span><span class="sxs-lookup"><span data-stu-id="ff110-294">Test that the app seeded the database.</span></span> <span data-ttu-id="ff110-295">Se sono presenti tutte le righe nel DB del contatto, il metodo di inizializzazione non viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="ff110-295">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

<span data-ttu-id="ff110-296">Questa esercitazione illustra come creare un'app web ASP.NET Core con i dati utente protetti da autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="ff110-296">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="ff110-297">Viene visualizzato un elenco di contatti che (registrati) agli utenti autenticati hanno creato.</span><span class="sxs-lookup"><span data-stu-id="ff110-297">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="ff110-298">Sono disponibili tre gruppi di sicurezza:</span><span class="sxs-lookup"><span data-stu-id="ff110-298">There are three security groups:</span></span>

* <span data-ttu-id="ff110-299">**Utenti registrati** può visualizzare tutti i dati approvati e può modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="ff110-299">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="ff110-300">**I responsabili** possono approvare o rifiutare i dati di contatto.</span><span class="sxs-lookup"><span data-stu-id="ff110-300">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="ff110-301">Solo i contatti approvati sono visibili agli utenti.</span><span class="sxs-lookup"><span data-stu-id="ff110-301">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="ff110-302">**Gli amministratori** può approvare o rifiutare e modificare/eliminare tutti i dati.</span><span class="sxs-lookup"><span data-stu-id="ff110-302">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="ff110-303">Nell'immagine seguente, l'utente Rick (`rick@example.com`) ha effettuato l'accesso.</span><span class="sxs-lookup"><span data-stu-id="ff110-303">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="ff110-304">Rick possono visualizzare solo i contatti approvati e **Edit**/**eliminare**/**Crea nuovo** collegamenti per il suo i contatti.</span><span class="sxs-lookup"><span data-stu-id="ff110-304">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="ff110-305">Solo l'ultimo record, creato da Rick, consente di visualizzare **Edit** e **eliminare** collegamenti.</span><span class="sxs-lookup"><span data-stu-id="ff110-305">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="ff110-306">Gli altri utenti non vedono l'ultimo record fino a quando un responsabile o l'amministratore assume lo stato "Approvato".</span><span class="sxs-lookup"><span data-stu-id="ff110-306">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Screenshot che mostra Rick effettuato l'accesso](secure-data/_static/rick.png)

<span data-ttu-id="ff110-308">Nell'immagine seguente, `manager@contoso.com` è stato eseguito l'accesso e nel ruolo del Manager:</span><span class="sxs-lookup"><span data-stu-id="ff110-308">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Screenshot che mostra manager@contoso.com effettuato l'accesso](secure-data/_static/manager1.png)

<span data-ttu-id="ff110-310">L'immagine seguente mostra i gestori visualizzazione dettagli di un contatto:</span><span class="sxs-lookup"><span data-stu-id="ff110-310">The following image shows the managers details view of a contact:</span></span>

![Visualizzazione di gestione di un contatto](secure-data/_static/manager.png)

<span data-ttu-id="ff110-312">Il **Approve** e **rifiutare** pulsanti vengono visualizzati solo per i gestori e amministratori.</span><span class="sxs-lookup"><span data-stu-id="ff110-312">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="ff110-313">Nell'immagine seguente, `admin@contoso.com` è stato eseguito l'accesso e il ruolo dell'amministratore:</span><span class="sxs-lookup"><span data-stu-id="ff110-313">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Screenshot che mostra admin@contoso.com effettuato l'accesso](secure-data/_static/admin.png)

<span data-ttu-id="ff110-315">L'amministratore ha tutti i privilegi.</span><span class="sxs-lookup"><span data-stu-id="ff110-315">The administrator has all privileges.</span></span> <span data-ttu-id="ff110-316">Può leggere, modificare ed eliminare un contatto e modificare lo stato dei contatti.</span><span class="sxs-lookup"><span data-stu-id="ff110-316">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="ff110-317">L'app è stato creato da [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) seguenti `Contact` modello:</span><span class="sxs-lookup"><span data-stu-id="ff110-317">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="ff110-318">L'esempio contiene i gestori di autorizzazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="ff110-318">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="ff110-319">`ContactIsOwnerAuthorizationHandler`: Garantisce che un utente possa modificare solo i propri dati.</span><span class="sxs-lookup"><span data-stu-id="ff110-319">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="ff110-320">`ContactManagerAuthorizationHandler`: Consente ai responsabili di approvare o rifiutare i contatti.</span><span class="sxs-lookup"><span data-stu-id="ff110-320">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="ff110-321">`ContactAdministratorsAuthorizationHandler`: Consente agli amministratori di approvare o rifiutare i contatti e di modificare/eliminare i contatti.</span><span class="sxs-lookup"><span data-stu-id="ff110-321">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ff110-322">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="ff110-322">Prerequisites</span></span>

<span data-ttu-id="ff110-323">Questa esercitazione viene fatto avanzare.</span><span class="sxs-lookup"><span data-stu-id="ff110-323">This tutorial is advanced.</span></span> <span data-ttu-id="ff110-324">È necessario avere familiarità con:</span><span class="sxs-lookup"><span data-stu-id="ff110-324">You should be familiar with:</span></span>

* [<span data-ttu-id="ff110-325">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ff110-325">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="ff110-326">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="ff110-326">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="ff110-327">Conferma account e recupero password</span><span class="sxs-lookup"><span data-stu-id="ff110-327">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="ff110-328">Autorizzazione</span><span class="sxs-lookup"><span data-stu-id="ff110-328">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="ff110-329">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="ff110-329">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="ff110-330">Starter e app completata</span><span class="sxs-lookup"><span data-stu-id="ff110-330">The starter and completed app</span></span>

<span data-ttu-id="ff110-331">[Scaricare](xref:index#how-to-download-a-sample) il [completato](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span><span class="sxs-lookup"><span data-stu-id="ff110-331">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="ff110-332">[Test](#test-the-completed-app) dell'app completata in modo da avere acquisito familiarità con le funzionalità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="ff110-332">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="ff110-333">L'app di base</span><span class="sxs-lookup"><span data-stu-id="ff110-333">The starter app</span></span>

<span data-ttu-id="ff110-334">[Scaricare](xref:index#how-to-download-a-sample) il [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span><span class="sxs-lookup"><span data-stu-id="ff110-334">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="ff110-335">Eseguire l'app, toccare il **ContactManager** collegare e verificare è possibile creare, modificare ed eliminare un contatto.</span><span class="sxs-lookup"><span data-stu-id="ff110-335">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="ff110-336">Proteggere i dati utente</span><span class="sxs-lookup"><span data-stu-id="ff110-336">Secure user data</span></span>

<span data-ttu-id="ff110-337">Le sezioni seguenti includono tutti i passaggi principali per creare l'app per dati sicura degli utenti.</span><span class="sxs-lookup"><span data-stu-id="ff110-337">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="ff110-338">Si può risultare utile per fare riferimento al progetto completato.</span><span class="sxs-lookup"><span data-stu-id="ff110-338">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="ff110-339">Associare i dati di contatto per l'utente</span><span class="sxs-lookup"><span data-stu-id="ff110-339">Tie the contact data to the user</span></span>

<span data-ttu-id="ff110-340">Usare ASP.NET [identità](xref:security/authentication/identity) ID utente per garantire che gli utenti possono modificare i propri dati, ma non altri dati degli utenti.</span><span class="sxs-lookup"><span data-stu-id="ff110-340">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="ff110-341">Aggiungere `OwnerID` e `ContactStatus` per il `Contact` modello:</span><span class="sxs-lookup"><span data-stu-id="ff110-341">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="ff110-342">`OwnerID` è l'ID dell'utente dal `AspNetUser` tabella di [identità](xref:security/authentication/identity) database.</span><span class="sxs-lookup"><span data-stu-id="ff110-342">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="ff110-343">Il `Status` campo determina se un contatto visibile dagli utenti generali.</span><span class="sxs-lookup"><span data-stu-id="ff110-343">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="ff110-344">Creare una nuova migrazione e aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="ff110-344">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="ff110-345">Aggiungere altri servizi di identità</span><span class="sxs-lookup"><span data-stu-id="ff110-345">Add Role services to Identity</span></span>

<span data-ttu-id="ff110-346">Accodare [Aggiungi ruoli](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) per aggiungere servizi ruolo:</span><span class="sxs-lookup"><span data-stu-id="ff110-346">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="ff110-347">Richiedere agli utenti autenticati</span><span class="sxs-lookup"><span data-stu-id="ff110-347">Require authenticated users</span></span>

<span data-ttu-id="ff110-348">Impostare i criteri di autenticazione predefinito per richiedere agli utenti di essere autenticato:</span><span class="sxs-lookup"><span data-stu-id="ff110-348">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="ff110-349">È possibile rifiutare esplicitamente l'autenticazione a livello di metodo di pagina Razor, controller o azione con il `[AllowAnonymous]` attributo.</span><span class="sxs-lookup"><span data-stu-id="ff110-349">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="ff110-350">L'impostazione di criteri di autenticazione predefinito per richiedere agli utenti di essere autenticati protegge appena aggiunto Razor Pages e controller.</span><span class="sxs-lookup"><span data-stu-id="ff110-350">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="ff110-351">La presenza di autenticazione richiesto per impostazione predefinita è più sicuro rispetto al semplice uso nuovi controller e le pagine Razor per includere il `[Authorize]` attributo.</span><span class="sxs-lookup"><span data-stu-id="ff110-351">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="ff110-352">Aggiungere [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) all'indice, le pagine di informazioni e contatti in modo che gli utenti anonimi possono ottenere informazioni sul sito per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="ff110-352">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="ff110-353">Configurare l'account di test</span><span class="sxs-lookup"><span data-stu-id="ff110-353">Configure the test account</span></span>

<span data-ttu-id="ff110-354">Il `SeedData` classe crea due account: amministratore e manager.</span><span class="sxs-lookup"><span data-stu-id="ff110-354">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="ff110-355">Usare la [strumento Secret Manager](xref:security/app-secrets) per impostare una password per questi account.</span><span class="sxs-lookup"><span data-stu-id="ff110-355">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="ff110-356">Impostare la password dalla directory del progetto (la directory contenente *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="ff110-356">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="ff110-357">Se non è specificata una password complessa, viene generata un'eccezione quando `SeedData.Initialize` viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="ff110-357">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="ff110-358">Aggiornamento `Main` di usare la password di test:</span><span class="sxs-lookup"><span data-stu-id="ff110-358">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="ff110-359">Creare gli account di test e aggiornare i contatti</span><span class="sxs-lookup"><span data-stu-id="ff110-359">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="ff110-360">Aggiorna il `Initialize` metodo nel `SeedData` classe per creare gli account di test:</span><span class="sxs-lookup"><span data-stu-id="ff110-360">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="ff110-361">Aggiungere l'ID di utente amministratore e `ContactStatus` ai contatti.</span><span class="sxs-lookup"><span data-stu-id="ff110-361">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="ff110-362">Rendere uno dei contatti relativi al "Submitted" e "Rejected" uno.</span><span class="sxs-lookup"><span data-stu-id="ff110-362">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="ff110-363">Aggiungere l'ID utente e lo stato per tutti i contatti.</span><span class="sxs-lookup"><span data-stu-id="ff110-363">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="ff110-364">Viene visualizzato un solo contatto:</span><span class="sxs-lookup"><span data-stu-id="ff110-364">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="ff110-365">Creare proprietario, manager e i gestori di autorizzazione di amministratore</span><span class="sxs-lookup"><span data-stu-id="ff110-365">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="ff110-366">Creare un `ContactIsOwnerAuthorizationHandler` classe la *autorizzazione* cartella.</span><span class="sxs-lookup"><span data-stu-id="ff110-366">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="ff110-367">Il `ContactIsOwnerAuthorizationHandler` verifica che l'utente che agisce su una risorsa proprietario della risorsa.</span><span class="sxs-lookup"><span data-stu-id="ff110-367">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="ff110-368">Il `ContactIsOwnerAuthorizationHandler` chiamate [contesto. Esito positivo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) se l'utente autenticato corrente è il proprietario del contatto.</span><span class="sxs-lookup"><span data-stu-id="ff110-368">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="ff110-369">I gestori di autorizzazione a livello generale:</span><span class="sxs-lookup"><span data-stu-id="ff110-369">Authorization handlers generally:</span></span>

* <span data-ttu-id="ff110-370">Restituire `context.Succeed` quando vengono soddisfatti i requisiti.</span><span class="sxs-lookup"><span data-stu-id="ff110-370">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="ff110-371">Restituire `Task.CompletedTask` quando non vengono soddisfatti i requisiti.</span><span class="sxs-lookup"><span data-stu-id="ff110-371">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="ff110-372">`Task.CompletedTask`non ha esito positivo&mdash;o negativo consente l'esecuzione di altri gestori di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="ff110-372">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="ff110-373">Se è necessario eseguire in modo esplicito, restituire [contesto. Esito negativo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="ff110-373">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="ff110-374">L'app consente ai proprietari di contatto di modifica/eliminazione/creare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="ff110-374">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="ff110-375">`ContactIsOwnerAuthorizationHandler` non è necessario verificare l'operazione passato nel parametro del requisito.</span><span class="sxs-lookup"><span data-stu-id="ff110-375">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="ff110-376">Creare un gestore di autorizzazione di gestione</span><span class="sxs-lookup"><span data-stu-id="ff110-376">Create a manager authorization handler</span></span>

<span data-ttu-id="ff110-377">Creare un `ContactManagerAuthorizationHandler` classe la *autorizzazione* cartella.</span><span class="sxs-lookup"><span data-stu-id="ff110-377">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="ff110-378">Il `ContactManagerAuthorizationHandler` verifica se l'utente che agiscono sulla risorsa è un manager.</span><span class="sxs-lookup"><span data-stu-id="ff110-378">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="ff110-379">Solo i manager possono approvare o rifiutare le modifiche di contenuto (nuove o modificate).</span><span class="sxs-lookup"><span data-stu-id="ff110-379">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="ff110-380">Creare un gestore di autorizzazione di amministratore</span><span class="sxs-lookup"><span data-stu-id="ff110-380">Create an administrator authorization handler</span></span>

<span data-ttu-id="ff110-381">Creare un `ContactAdministratorsAuthorizationHandler` classe la *autorizzazione* cartella.</span><span class="sxs-lookup"><span data-stu-id="ff110-381">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="ff110-382">Il `ContactAdministratorsAuthorizationHandler` verifica se l'utente che agiscono sulla risorsa è un amministratore.</span><span class="sxs-lookup"><span data-stu-id="ff110-382">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="ff110-383">Amministratore può eseguire tutte le operazioni.</span><span class="sxs-lookup"><span data-stu-id="ff110-383">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="ff110-384">Registrare i gestori di eventi di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="ff110-384">Register the authorization handlers</span></span>

<span data-ttu-id="ff110-385">Servizi usando Entity Framework Core devono essere registrati per [inserimento delle dipendenze](xref:fundamentals/dependency-injection) utilizzando [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="ff110-385">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="ff110-386">Il `ContactIsOwnerAuthorizationHandler` Usa ASP.NET Core [identità](xref:security/authentication/identity), che si basa su Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ff110-386">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="ff110-387">Registrare i gestori con la raccolta di servizio in modo che siano disponibili per il `ContactsController` attraverso [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ff110-387">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="ff110-388">Aggiungere il codice seguente alla fine della `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ff110-388">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="ff110-389">`ContactAdministratorsAuthorizationHandler` e `ContactManagerAuthorizationHandler` vengono aggiunti come singleton.</span><span class="sxs-lookup"><span data-stu-id="ff110-389">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="ff110-390">Sono singleton con stato perché non usano Entity Framework e tutte le informazioni necessarie nel `Context` parametro del `HandleRequirementAsync` (metodo).</span><span class="sxs-lookup"><span data-stu-id="ff110-390">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="ff110-391">Supporto di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="ff110-391">Support authorization</span></span>

<span data-ttu-id="ff110-392">In questa sezione aggiornare le pagine Razor e aggiungere una classe di requisiti di operazioni.</span><span class="sxs-lookup"><span data-stu-id="ff110-392">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="ff110-393">Esaminare la classe di requisiti di operazioni contatto</span><span class="sxs-lookup"><span data-stu-id="ff110-393">Review the contact operations requirements class</span></span>

<span data-ttu-id="ff110-394">Esaminare il `ContactOperations` classe.</span><span class="sxs-lookup"><span data-stu-id="ff110-394">Review the `ContactOperations` class.</span></span> <span data-ttu-id="ff110-395">Questa classe contiene i requisiti supportato dall'app:</span><span class="sxs-lookup"><span data-stu-id="ff110-395">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="ff110-396">Creare una classe di base per le pagine Razor di contatti</span><span class="sxs-lookup"><span data-stu-id="ff110-396">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="ff110-397">Creare una classe base che contiene i servizi usati in Razor Pages i contatti.</span><span class="sxs-lookup"><span data-stu-id="ff110-397">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="ff110-398">La classe di base inserisce il codice di inizializzazione in un'unica posizione:</span><span class="sxs-lookup"><span data-stu-id="ff110-398">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="ff110-399">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="ff110-399">The preceding code:</span></span>

* <span data-ttu-id="ff110-400">Aggiunge il `IAuthorizationService` servizio per accedere ai gestori di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="ff110-400">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="ff110-401">Aggiunge le identità `UserManager` servizio.</span><span class="sxs-lookup"><span data-stu-id="ff110-401">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="ff110-402">Aggiungere l'oggetto `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="ff110-402">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="ff110-403">Aggiornare il CreateModel</span><span class="sxs-lookup"><span data-stu-id="ff110-403">Update the CreateModel</span></span>

<span data-ttu-id="ff110-404">Aggiornare il costruttore di modello della pagina create per usare il `DI_BasePageModel` classe di base:</span><span class="sxs-lookup"><span data-stu-id="ff110-404">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="ff110-405">Aggiornamento di `CreateModel.OnPostAsync` metodo:</span><span class="sxs-lookup"><span data-stu-id="ff110-405">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="ff110-406">Aggiungere l'ID utente per il `Contact` modello.</span><span class="sxs-lookup"><span data-stu-id="ff110-406">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="ff110-407">Chiamare il gestore di autorizzazione per verificare che l'utente dispone dell'autorizzazione per creare contatti.</span><span class="sxs-lookup"><span data-stu-id="ff110-407">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="ff110-408">Aggiornare il IndexModel</span><span class="sxs-lookup"><span data-stu-id="ff110-408">Update the IndexModel</span></span>

<span data-ttu-id="ff110-409">Aggiornamento di `OnGetAsync` metodo in modo che solo i contatti approvati vengono visualizzati agli utenti generici:</span><span class="sxs-lookup"><span data-stu-id="ff110-409">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="ff110-410">Aggiornare il EditModel</span><span class="sxs-lookup"><span data-stu-id="ff110-410">Update the EditModel</span></span>

<span data-ttu-id="ff110-411">Aggiungere un gestore di autorizzazione per verificare che l'utente è proprietario del contatto.</span><span class="sxs-lookup"><span data-stu-id="ff110-411">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="ff110-412">Poiché viene convalidata l'autorizzazione alle risorse, il `[Authorize]` attributo non è sufficiente.</span><span class="sxs-lookup"><span data-stu-id="ff110-412">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="ff110-413">L'app non ha accesso alla risorsa quando vengono valutati gli attributi.</span><span class="sxs-lookup"><span data-stu-id="ff110-413">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="ff110-414">Autorizzazione basata sulle risorse deve essere imperativa.</span><span class="sxs-lookup"><span data-stu-id="ff110-414">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="ff110-415">Controlli devono essere eseguiti dopo che l'app potrà accedere alla risorsa, è possibile caricarli nel modello di pagina oppure caricarlo all'interno del gestore stesso.</span><span class="sxs-lookup"><span data-stu-id="ff110-415">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="ff110-416">Si accede di frequente della risorsa passando la chiave di risorsa.</span><span class="sxs-lookup"><span data-stu-id="ff110-416">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="ff110-417">Aggiornare il DeleteModel</span><span class="sxs-lookup"><span data-stu-id="ff110-417">Update the DeleteModel</span></span>

<span data-ttu-id="ff110-418">Aggiornare il modello di pagina delete per utilizzare il gestore dell'autorizzazione per verificare che l'utente disponga dell'autorizzazione di eliminazione per il contatto.</span><span class="sxs-lookup"><span data-stu-id="ff110-418">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="ff110-419">Inserire il servizio di autorizzazione in visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="ff110-419">Inject the authorization service into the views</span></span>

<span data-ttu-id="ff110-420">Attualmente, l'interfaccia utente Mostra modifica ed elimina i collegamenti per i contatti che l'utente non è possibile modificare.</span><span class="sxs-lookup"><span data-stu-id="ff110-420">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="ff110-421">Inserire il servizio di autorizzazione nel *viewimports* file in modo che sia disponibile per tutte le viste:</span><span class="sxs-lookup"><span data-stu-id="ff110-421">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="ff110-422">Il markup precedente aggiunge diverse `using` istruzioni.</span><span class="sxs-lookup"><span data-stu-id="ff110-422">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="ff110-423">Aggiorna il **modifica** e **eliminare** collega in *Pages/Contacts/Index.cshtml* in modo che vengono sottoposti a rendering solo per gli utenti che dispongono delle autorizzazioni appropriate:</span><span class="sxs-lookup"><span data-stu-id="ff110-423">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="ff110-424">Nascondere i collegamenti da utenti che non dispone dell'autorizzazione per modificare i dati non proteggere le app.</span><span class="sxs-lookup"><span data-stu-id="ff110-424">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="ff110-425">Nascondere i collegamenti rende più semplici da usare l'app mediante la visualizzazione di collegamenti solo validi.</span><span class="sxs-lookup"><span data-stu-id="ff110-425">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="ff110-426">Gli utenti possono hack gli URL generati per richiamare modifica ed eliminare le operazioni sui dati che non sono proprietari.</span><span class="sxs-lookup"><span data-stu-id="ff110-426">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="ff110-427">La pagina Razor o il controller deve applicare controlli di accesso per proteggere i dati.</span><span class="sxs-lookup"><span data-stu-id="ff110-427">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="ff110-428">Aggiorna i dettagli</span><span class="sxs-lookup"><span data-stu-id="ff110-428">Update Details</span></span>

<span data-ttu-id="ff110-429">Aggiornare la visualizzazione dei dettagli in modo che i responsabili possono approvare o rifiutare i contatti:</span><span class="sxs-lookup"><span data-stu-id="ff110-429">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="ff110-430">Aggiornare il modello di pagina dei dettagli:</span><span class="sxs-lookup"><span data-stu-id="ff110-430">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="ff110-431">Aggiungere o rimuovere un utente a un ruolo</span><span class="sxs-lookup"><span data-stu-id="ff110-431">Add or remove a user to a role</span></span>

<span data-ttu-id="ff110-432">Visualizzare [questo problema](https://github.com/aspnet/AspNetCore.Docs/issues/8502) per informazioni su:</span><span class="sxs-lookup"><span data-stu-id="ff110-432">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="ff110-433">Rimozione dei privilegi da un utente.</span><span class="sxs-lookup"><span data-stu-id="ff110-433">Removing privileges from a user.</span></span> <span data-ttu-id="ff110-434">Ad esempio, disattivare un utente in un'app di chat.</span><span class="sxs-lookup"><span data-stu-id="ff110-434">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="ff110-435">Aggiunta di privilegi a un utente.</span><span class="sxs-lookup"><span data-stu-id="ff110-435">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="ff110-436">Testare l'app completata</span><span class="sxs-lookup"><span data-stu-id="ff110-436">Test the completed app</span></span>

<span data-ttu-id="ff110-437">Se è già stata impostata una password per gli account utente di seeding, usare il [strumento Secret Manager](xref:security/app-secrets#secret-manager) per impostare una password:</span><span class="sxs-lookup"><span data-stu-id="ff110-437">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="ff110-438">Scegliere una password complessa: Usare otto o più caratteri e almeno un carattere maiuscolo, un numero e un simbolo.</span><span class="sxs-lookup"><span data-stu-id="ff110-438">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="ff110-439">Ad esempio, `Passw0rd!` soddisfi i requisiti di password complesse.</span><span class="sxs-lookup"><span data-stu-id="ff110-439">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="ff110-440">Eseguire il comando seguente dalla cartella del progetto, in cui `<PW>` è la password:</span><span class="sxs-lookup"><span data-stu-id="ff110-440">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

* <span data-ttu-id="ff110-441">Eliminare e aggiornare il database</span><span class="sxs-lookup"><span data-stu-id="ff110-441">Drop and update the Database</span></span>

    ```console
     dotnet ef database drop -f
     dotnet ef database update  
     ```

* <span data-ttu-id="ff110-442">Riavviare l'app per l'inizializzazione del database.</span><span class="sxs-lookup"><span data-stu-id="ff110-442">Restart the app to seed the database.</span></span>

<span data-ttu-id="ff110-443">Un modo semplice per testare l'app completata consiste nell'avviare tre diversi browser (o in incognito o InPrivate sessioni).</span><span class="sxs-lookup"><span data-stu-id="ff110-443">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="ff110-444">In un browser, registrare un nuovo utente (ad esempio, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="ff110-444">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="ff110-445">Accedere a ogni browser con un altro utente.</span><span class="sxs-lookup"><span data-stu-id="ff110-445">Sign in to each browser with a different user.</span></span> <span data-ttu-id="ff110-446">Verificare che le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="ff110-446">Verify the following operations:</span></span>

* <span data-ttu-id="ff110-447">Gli utenti registrati possono visualizzare tutti i dati di contatto approvati.</span><span class="sxs-lookup"><span data-stu-id="ff110-447">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="ff110-448">Gli utenti registrati possono modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="ff110-448">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="ff110-449">I responsabili possono approvare o rifiutare i dati di contatto.</span><span class="sxs-lookup"><span data-stu-id="ff110-449">Managers can approve/reject contact data.</span></span> <span data-ttu-id="ff110-450">Il `Details` viene mostrata **Approva** e **rifiutare** pulsanti.</span><span class="sxs-lookup"><span data-stu-id="ff110-450">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="ff110-451">Gli amministratori possono approvare o rifiutare e modificare/eliminare tutti i dati.</span><span class="sxs-lookup"><span data-stu-id="ff110-451">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="ff110-452">Utente</span><span class="sxs-lookup"><span data-stu-id="ff110-452">User</span></span>                | <span data-ttu-id="ff110-453">Esegue il seeding dell'app</span><span class="sxs-lookup"><span data-stu-id="ff110-453">Seeded by the app</span></span> | <span data-ttu-id="ff110-454">Opzioni</span><span class="sxs-lookup"><span data-stu-id="ff110-454">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="ff110-455">No</span><span class="sxs-lookup"><span data-stu-id="ff110-455">No</span></span>                | <span data-ttu-id="ff110-456">Modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="ff110-456">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="ff110-457">Yes</span><span class="sxs-lookup"><span data-stu-id="ff110-457">Yes</span></span>               | <span data-ttu-id="ff110-458">Approvare o rifiutare e modificare/eliminare i dati personalizzati.</span><span class="sxs-lookup"><span data-stu-id="ff110-458">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="ff110-459">Yes</span><span class="sxs-lookup"><span data-stu-id="ff110-459">Yes</span></span>               | <span data-ttu-id="ff110-460">Approvare o rifiutare e modificare/eliminare tutti i dati.</span><span class="sxs-lookup"><span data-stu-id="ff110-460">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="ff110-461">Creare un contatto nel browser dell'amministratore.</span><span class="sxs-lookup"><span data-stu-id="ff110-461">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="ff110-462">Copiare l'URL per l'eliminazione e modifica di contattare l'amministratore.</span><span class="sxs-lookup"><span data-stu-id="ff110-462">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="ff110-463">Incollare questi collegamenti nel browser dell'utente test per verificare che l'utente di test non è possibile eseguire queste operazioni.</span><span class="sxs-lookup"><span data-stu-id="ff110-463">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="ff110-464">Creare l'app di base</span><span class="sxs-lookup"><span data-stu-id="ff110-464">Create the starter app</span></span>

* <span data-ttu-id="ff110-465">Creare un'app Razor Pages denominata "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="ff110-465">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="ff110-466">Creare l'app con **singoli account utente**.</span><span class="sxs-lookup"><span data-stu-id="ff110-466">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="ff110-467">Il nome "ContactManager" in modo che lo spazio dei nomi corrispondente dello spazio dei nomi utilizzato nell'esempio.</span><span class="sxs-lookup"><span data-stu-id="ff110-467">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="ff110-468">`-uld` Specifica di Local DB invece di SQLite</span><span class="sxs-lookup"><span data-stu-id="ff110-468">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="ff110-469">Aggiungi *modelli/Contact. cs*:</span><span class="sxs-lookup"><span data-stu-id="ff110-469">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="ff110-470">Scaffolding di `Contact` modello.</span><span class="sxs-lookup"><span data-stu-id="ff110-470">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="ff110-471">Creare la migrazione iniziale e l'aggiornamento del database:</span><span class="sxs-lookup"><span data-stu-id="ff110-471">Create initial migration and update the database:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* <span data-ttu-id="ff110-472">Aggiorna il **ContactManager** ancoraggio nel *cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="ff110-472">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* <span data-ttu-id="ff110-473">Testare l'app per la creazione, modifica ed eliminazione di un contatto</span><span class="sxs-lookup"><span data-stu-id="ff110-473">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="ff110-474">Specificare il valore di inizializzazione del database</span><span class="sxs-lookup"><span data-stu-id="ff110-474">Seed the database</span></span>

<span data-ttu-id="ff110-475">Aggiungere il [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) classe per il *dati* cartella.</span><span class="sxs-lookup"><span data-stu-id="ff110-475">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="ff110-476">Chiamare `SeedData.Initialize` da `Main`:</span><span class="sxs-lookup"><span data-stu-id="ff110-476">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="ff110-477">Verificare che l'app effettuato il seeding del database.</span><span class="sxs-lookup"><span data-stu-id="ff110-477">Test that the app seeded the database.</span></span> <span data-ttu-id="ff110-478">Se sono presenti tutte le righe nel DB del contatto, il metodo di inizializzazione non viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="ff110-478">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="ff110-479">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="ff110-479">Additional resources</span></span>

* [<span data-ttu-id="ff110-480">Creare un'app web .NET Core e Database SQL di Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ff110-480">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="ff110-481">[ASP.NET Core autorizzazione Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="ff110-481">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="ff110-482">Questa esercitazione descrive in dettaglio più le funzionalità di sicurezza introdotte in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="ff110-482">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="ff110-483">Autorizzazione personalizzata basata su criteri</span><span class="sxs-lookup"><span data-stu-id="ff110-483">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
