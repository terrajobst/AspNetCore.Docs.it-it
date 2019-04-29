---
title: Creare un'app ASP.NET Core con i dati utente protetti da autorizzazione
author: rick-anderson
description: Informazioni su come creare un'app Razor Pages con i dati utente protetti da autorizzazione. Include HTTPS, l'autenticazione, sicurezza, ASP.NET Core Identity.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: 9b01aca0cb2b33f5561193ffdce4aa4950485fce
ms.sourcegitcommit: 8a84ce880b4c40d6694ba6423038f18fc2eb5746
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "60165238"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="c3f95-104">Creare un'app ASP.NET Core con i dati utente protetti da autorizzazione</span><span class="sxs-lookup"><span data-stu-id="c3f95-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="c3f95-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="c3f95-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="c3f95-106">Visualizzare [questo PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) per la versione di ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="c3f95-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="c3f95-107">La versione di ASP.NET Core 1.1 di questa esercitazione è nel [ciò](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) cartella.</span><span class="sxs-lookup"><span data-stu-id="c3f95-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="c3f95-108">1.1 esempio ASP.NET Core è nel [esempi](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="c3f95-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c3f95-109">Vedere [questo pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="c3f95-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c3f95-110">Questa esercitazione illustra come creare un'app web ASP.NET Core con i dati utente protetti da autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="c3f95-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="c3f95-111">Viene visualizzato un elenco di contatti che (registrati) agli utenti autenticati hanno creato.</span><span class="sxs-lookup"><span data-stu-id="c3f95-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="c3f95-112">Sono disponibili tre gruppi di sicurezza:</span><span class="sxs-lookup"><span data-stu-id="c3f95-112">There are three security groups:</span></span>

* <span data-ttu-id="c3f95-113">**Utenti registrati** può visualizzare tutti i dati approvati e può modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="c3f95-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="c3f95-114">**I responsabili** possono approvare o rifiutare i dati di contatto.</span><span class="sxs-lookup"><span data-stu-id="c3f95-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="c3f95-115">Solo i contatti approvati sono visibili agli utenti.</span><span class="sxs-lookup"><span data-stu-id="c3f95-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="c3f95-116">**Gli amministratori** può approvare o rifiutare e modificare/eliminare tutti i dati.</span><span class="sxs-lookup"><span data-stu-id="c3f95-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="c3f95-117">Nell'immagine seguente, l'utente Rick (`rick@example.com`) ha effettuato l'accesso.</span><span class="sxs-lookup"><span data-stu-id="c3f95-117">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="c3f95-118">Rick possono visualizzare solo i contatti approvati e **Edit**/**eliminare**/**Crea nuovo** collegamenti per il suo i contatti.</span><span class="sxs-lookup"><span data-stu-id="c3f95-118">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="c3f95-119">Solo l'ultimo record, creato da Rick, consente di visualizzare **Edit** e **eliminare** collegamenti.</span><span class="sxs-lookup"><span data-stu-id="c3f95-119">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="c3f95-120">Gli altri utenti non vedono l'ultimo record fino a quando un responsabile o l'amministratore assume lo stato "Approvato".</span><span class="sxs-lookup"><span data-stu-id="c3f95-120">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Screenshot che mostra Rick effettuato l'accesso](secure-data/_static/rick.png)

<span data-ttu-id="c3f95-122">Nell'immagine seguente, `manager@contoso.com` viene effettuato l'accesso e nel ruolo managers:</span><span class="sxs-lookup"><span data-stu-id="c3f95-122">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![Screenshot che mostra manager@contoso.com effettuato l'accesso](secure-data/_static/manager1.png)

<span data-ttu-id="c3f95-124">L'immagine seguente mostra i gestori visualizzazione dettagli di un contatto:</span><span class="sxs-lookup"><span data-stu-id="c3f95-124">The following image shows the managers details view of a contact:</span></span>

![Visualizzazione di gestione di un contatto](secure-data/_static/manager.png)

<span data-ttu-id="c3f95-126">Il **Approve** e **rifiutare** pulsanti vengono visualizzati solo per i gestori e amministratori.</span><span class="sxs-lookup"><span data-stu-id="c3f95-126">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="c3f95-127">Nell'immagine seguente, `admin@contoso.com` viene effettuato l'accesso e al ruolo administrators:</span><span class="sxs-lookup"><span data-stu-id="c3f95-127">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![Screenshot che mostra admin@contoso.com effettuato l'accesso](secure-data/_static/admin.png)

<span data-ttu-id="c3f95-129">L'amministratore ha tutti i privilegi.</span><span class="sxs-lookup"><span data-stu-id="c3f95-129">The administrator has all privileges.</span></span> <span data-ttu-id="c3f95-130">Può leggere, modificare ed eliminare un contatto e modificare lo stato dei contatti.</span><span class="sxs-lookup"><span data-stu-id="c3f95-130">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="c3f95-131">L'app è stato creato da [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) seguenti `Contact` modello:</span><span class="sxs-lookup"><span data-stu-id="c3f95-131">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="c3f95-132">L'esempio contiene i gestori di autorizzazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="c3f95-132">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="c3f95-133">`ContactIsOwnerAuthorizationHandler`: Assicura che un utente può modificare solo i propri dati.</span><span class="sxs-lookup"><span data-stu-id="c3f95-133">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="c3f95-134">`ContactManagerAuthorizationHandler`: Consente ai responsabili di approvare o rifiutare i contatti.</span><span class="sxs-lookup"><span data-stu-id="c3f95-134">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="c3f95-135">`ContactAdministratorsAuthorizationHandler`: Consente agli amministratori di approvare o rifiutare i contatti e di modificare/eliminare i contatti.</span><span class="sxs-lookup"><span data-stu-id="c3f95-135">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c3f95-136">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="c3f95-136">Prerequisites</span></span>

<span data-ttu-id="c3f95-137">Questa esercitazione viene fatto avanzare.</span><span class="sxs-lookup"><span data-stu-id="c3f95-137">This tutorial is advanced.</span></span> <span data-ttu-id="c3f95-138">È necessario avere familiarità con:</span><span class="sxs-lookup"><span data-stu-id="c3f95-138">You should be familiar with:</span></span>

* [<span data-ttu-id="c3f95-139">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c3f95-139">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="c3f95-140">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="c3f95-140">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="c3f95-141">Conferma account e recupero password</span><span class="sxs-lookup"><span data-stu-id="c3f95-141">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="c3f95-142">Autorizzazione</span><span class="sxs-lookup"><span data-stu-id="c3f95-142">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="c3f95-143">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c3f95-143">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="c3f95-144">In ASP.NET Core 2.1 `User.IsInRole` ha esito negativo quando si usa `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="c3f95-144">In ASP.NET Core 2.1, `User.IsInRole` fails when using `AddDefaultIdentity`.</span></span> <span data-ttu-id="c3f95-145">Questa esercitazione Usa `AddDefaultIdentity` e pertanto richiede ASP.NET Core 2.2 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="c3f95-145">This tutorial uses `AddDefaultIdentity` and therefore requires ASP.NET Core 2.2 or later.</span></span> <span data-ttu-id="c3f95-146">Visualizzare [questo problema su GitHub](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) per una soluzione alternativa.</span><span class="sxs-lookup"><span data-stu-id="c3f95-146">See [this GitHub issue](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) for a work-around.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="c3f95-147">Starter e app completata</span><span class="sxs-lookup"><span data-stu-id="c3f95-147">The starter and completed app</span></span>

<span data-ttu-id="c3f95-148">[Scaricare](xref:index#how-to-download-a-sample) il [completato](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span><span class="sxs-lookup"><span data-stu-id="c3f95-148">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="c3f95-149">[Test](#test-the-completed-app) dell'app completata in modo da avere acquisito familiarità con le funzionalità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="c3f95-149">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="c3f95-150">L'app di base</span><span class="sxs-lookup"><span data-stu-id="c3f95-150">The starter app</span></span>

<span data-ttu-id="c3f95-151">[Scaricare](xref:index#how-to-download-a-sample) il [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span><span class="sxs-lookup"><span data-stu-id="c3f95-151">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="c3f95-152">Eseguire l'app, toccare il **ContactManager** collegare e verificare è possibile creare, modificare ed eliminare un contatto.</span><span class="sxs-lookup"><span data-stu-id="c3f95-152">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="c3f95-153">Proteggere i dati utente</span><span class="sxs-lookup"><span data-stu-id="c3f95-153">Secure user data</span></span>

<span data-ttu-id="c3f95-154">Le sezioni seguenti includono tutti i passaggi principali per creare l'app per dati sicura degli utenti.</span><span class="sxs-lookup"><span data-stu-id="c3f95-154">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="c3f95-155">Si può risultare utile per fare riferimento al progetto completato.</span><span class="sxs-lookup"><span data-stu-id="c3f95-155">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="c3f95-156">Associare i dati di contatto per l'utente</span><span class="sxs-lookup"><span data-stu-id="c3f95-156">Tie the contact data to the user</span></span>

<span data-ttu-id="c3f95-157">Usare ASP.NET [identità](xref:security/authentication/identity) ID utente per garantire che gli utenti possono modificare i propri dati, ma non altri dati degli utenti.</span><span class="sxs-lookup"><span data-stu-id="c3f95-157">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="c3f95-158">Aggiungere `OwnerID` e `ContactStatus` per il `Contact` modello:</span><span class="sxs-lookup"><span data-stu-id="c3f95-158">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="c3f95-159">`OwnerID` è l'ID dell'utente dal `AspNetUser` tabella di [identità](xref:security/authentication/identity) database.</span><span class="sxs-lookup"><span data-stu-id="c3f95-159">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="c3f95-160">Il `Status` campo determina se un contatto visibile dagli utenti generali.</span><span class="sxs-lookup"><span data-stu-id="c3f95-160">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="c3f95-161">Creare una nuova migrazione e aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="c3f95-161">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="c3f95-162">Aggiungere altri servizi di identità</span><span class="sxs-lookup"><span data-stu-id="c3f95-162">Add Role services to Identity</span></span>

<span data-ttu-id="c3f95-163">Accodare [Aggiungi ruoli](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) per aggiungere servizi ruolo:</span><span class="sxs-lookup"><span data-stu-id="c3f95-163">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="c3f95-164">Richiedere agli utenti autenticati</span><span class="sxs-lookup"><span data-stu-id="c3f95-164">Require authenticated users</span></span>

<span data-ttu-id="c3f95-165">Impostare i criteri di autenticazione predefinito per richiedere agli utenti di essere autenticato:</span><span class="sxs-lookup"><span data-stu-id="c3f95-165">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="c3f95-166">È possibile rifiutare esplicitamente l'autenticazione a livello di metodo di pagina Razor, controller o azione con il `[AllowAnonymous]` attributo.</span><span class="sxs-lookup"><span data-stu-id="c3f95-166">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="c3f95-167">L'impostazione di criteri di autenticazione predefinito per richiedere agli utenti di essere autenticati protegge appena aggiunto Razor Pages e controller.</span><span class="sxs-lookup"><span data-stu-id="c3f95-167">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="c3f95-168">La presenza di autenticazione richiesto per impostazione predefinita è più sicuro rispetto al semplice uso nuovi controller e le pagine Razor per includere il `[Authorize]` attributo.</span><span class="sxs-lookup"><span data-stu-id="c3f95-168">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="c3f95-169">Aggiungere [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) all'indice, le pagine di informazioni e contatti in modo che gli utenti anonimi possono ottenere informazioni sul sito per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="c3f95-169">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="c3f95-170">Configurare l'account di test</span><span class="sxs-lookup"><span data-stu-id="c3f95-170">Configure the test account</span></span>

<span data-ttu-id="c3f95-171">Il `SeedData` classe crea due account: amministratore e manager.</span><span class="sxs-lookup"><span data-stu-id="c3f95-171">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="c3f95-172">Usare la [strumento Secret Manager](xref:security/app-secrets) per impostare una password per questi account.</span><span class="sxs-lookup"><span data-stu-id="c3f95-172">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="c3f95-173">Impostare la password dalla directory del progetto (la directory contenente *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="c3f95-173">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="c3f95-174">Se non è specificata una password complessa, viene generata un'eccezione quando `SeedData.Initialize` viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="c3f95-174">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="c3f95-175">Aggiornamento `Main` di usare la password di test:</span><span class="sxs-lookup"><span data-stu-id="c3f95-175">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="c3f95-176">Creare gli account di test e aggiornare i contatti</span><span class="sxs-lookup"><span data-stu-id="c3f95-176">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="c3f95-177">Aggiorna il `Initialize` metodo nel `SeedData` classe per creare gli account di test:</span><span class="sxs-lookup"><span data-stu-id="c3f95-177">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="c3f95-178">Aggiungere l'ID di utente amministratore e `ContactStatus` ai contatti.</span><span class="sxs-lookup"><span data-stu-id="c3f95-178">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="c3f95-179">Rendere uno dei contatti relativi al "Submitted" e "Rejected" uno.</span><span class="sxs-lookup"><span data-stu-id="c3f95-179">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="c3f95-180">Aggiungere l'ID utente e lo stato per tutti i contatti.</span><span class="sxs-lookup"><span data-stu-id="c3f95-180">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="c3f95-181">Viene visualizzato un solo contatto:</span><span class="sxs-lookup"><span data-stu-id="c3f95-181">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="c3f95-182">Creare proprietario, manager e i gestori di autorizzazione di amministratore</span><span class="sxs-lookup"><span data-stu-id="c3f95-182">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="c3f95-183">Creare un `ContactIsOwnerAuthorizationHandler` classe la *autorizzazione* cartella.</span><span class="sxs-lookup"><span data-stu-id="c3f95-183">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c3f95-184">Il `ContactIsOwnerAuthorizationHandler` verifica che l'utente che agisce su una risorsa proprietario della risorsa.</span><span class="sxs-lookup"><span data-stu-id="c3f95-184">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="c3f95-185">Il `ContactIsOwnerAuthorizationHandler` chiamate [contesto. Esito positivo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) se l'utente autenticato corrente è il proprietario del contatto.</span><span class="sxs-lookup"><span data-stu-id="c3f95-185">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="c3f95-186">I gestori di autorizzazione a livello generale:</span><span class="sxs-lookup"><span data-stu-id="c3f95-186">Authorization handlers generally:</span></span>

* <span data-ttu-id="c3f95-187">Restituire `context.Succeed` quando vengono soddisfatti i requisiti.</span><span class="sxs-lookup"><span data-stu-id="c3f95-187">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="c3f95-188">Restituire `Task.CompletedTask` quando non vengono soddisfatti i requisiti.</span><span class="sxs-lookup"><span data-stu-id="c3f95-188">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="c3f95-189">`Task.CompletedTask` né esito positivo o negativo&mdash;consente altri gestori di autorizzazione per l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="c3f95-189">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="c3f95-190">Se è necessario eseguire in modo esplicito, restituire [contesto. Esito negativo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="c3f95-190">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="c3f95-191">L'app consente ai proprietari di contatto di modifica/eliminazione/creare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="c3f95-191">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="c3f95-192">`ContactIsOwnerAuthorizationHandler` non è necessario verificare l'operazione passato nel parametro del requisito.</span><span class="sxs-lookup"><span data-stu-id="c3f95-192">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="c3f95-193">Creare un gestore di autorizzazione di gestione</span><span class="sxs-lookup"><span data-stu-id="c3f95-193">Create a manager authorization handler</span></span>

<span data-ttu-id="c3f95-194">Creare un `ContactManagerAuthorizationHandler` classe la *autorizzazione* cartella.</span><span class="sxs-lookup"><span data-stu-id="c3f95-194">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c3f95-195">Il `ContactManagerAuthorizationHandler` verifica se l'utente che agiscono sulla risorsa è un manager.</span><span class="sxs-lookup"><span data-stu-id="c3f95-195">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="c3f95-196">Solo i manager possono approvare o rifiutare le modifiche di contenuto (nuove o modificate).</span><span class="sxs-lookup"><span data-stu-id="c3f95-196">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="c3f95-197">Creare un gestore di autorizzazione di amministratore</span><span class="sxs-lookup"><span data-stu-id="c3f95-197">Create an administrator authorization handler</span></span>

<span data-ttu-id="c3f95-198">Creare un `ContactAdministratorsAuthorizationHandler` classe la *autorizzazione* cartella.</span><span class="sxs-lookup"><span data-stu-id="c3f95-198">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="c3f95-199">Il `ContactAdministratorsAuthorizationHandler` verifica se l'utente che agiscono sulla risorsa è un amministratore.</span><span class="sxs-lookup"><span data-stu-id="c3f95-199">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="c3f95-200">Amministratore può eseguire tutte le operazioni.</span><span class="sxs-lookup"><span data-stu-id="c3f95-200">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="c3f95-201">Registrare i gestori di eventi di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="c3f95-201">Register the authorization handlers</span></span>

<span data-ttu-id="c3f95-202">Servizi usando Entity Framework Core devono essere registrati per [inserimento delle dipendenze](xref:fundamentals/dependency-injection) utilizzando [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="c3f95-202">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="c3f95-203">Il `ContactIsOwnerAuthorizationHandler` Usa ASP.NET Core [identità](xref:security/authentication/identity), che si basa su Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c3f95-203">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="c3f95-204">Registrare i gestori con la raccolta di servizio in modo che siano disponibili per il `ContactsController` attraverso [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c3f95-204">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c3f95-205">Aggiungere il codice seguente alla fine della `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c3f95-205">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="c3f95-206">`ContactAdministratorsAuthorizationHandler` e `ContactManagerAuthorizationHandler` vengono aggiunti come singleton.</span><span class="sxs-lookup"><span data-stu-id="c3f95-206">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="c3f95-207">Sono singleton con stato perché non usano Entity Framework e tutte le informazioni necessarie nel `Context` parametro del `HandleRequirementAsync` (metodo).</span><span class="sxs-lookup"><span data-stu-id="c3f95-207">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="c3f95-208">Supporto di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="c3f95-208">Support authorization</span></span>

<span data-ttu-id="c3f95-209">In questa sezione aggiornare le pagine Razor e aggiungere una classe di requisiti di operazioni.</span><span class="sxs-lookup"><span data-stu-id="c3f95-209">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="c3f95-210">Esaminare la classe di requisiti di operazioni contatto</span><span class="sxs-lookup"><span data-stu-id="c3f95-210">Review the contact operations requirements class</span></span>

<span data-ttu-id="c3f95-211">Esaminare il `ContactOperations` classe.</span><span class="sxs-lookup"><span data-stu-id="c3f95-211">Review the `ContactOperations` class.</span></span> <span data-ttu-id="c3f95-212">Questa classe contiene i requisiti supportato dall'app:</span><span class="sxs-lookup"><span data-stu-id="c3f95-212">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="c3f95-213">Creare una classe di base per le pagine Razor di contatti</span><span class="sxs-lookup"><span data-stu-id="c3f95-213">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="c3f95-214">Creare una classe base che contiene i servizi usati in Razor Pages i contatti.</span><span class="sxs-lookup"><span data-stu-id="c3f95-214">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="c3f95-215">La classe di base inserisce il codice di inizializzazione in un'unica posizione:</span><span class="sxs-lookup"><span data-stu-id="c3f95-215">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="c3f95-216">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="c3f95-216">The preceding code:</span></span>

* <span data-ttu-id="c3f95-217">Aggiunge il `IAuthorizationService` servizio per accedere ai gestori di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="c3f95-217">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="c3f95-218">Aggiunge le identità `UserManager` servizio.</span><span class="sxs-lookup"><span data-stu-id="c3f95-218">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="c3f95-219">Aggiungere l'oggetto `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="c3f95-219">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="c3f95-220">Aggiornare il CreateModel</span><span class="sxs-lookup"><span data-stu-id="c3f95-220">Update the CreateModel</span></span>

<span data-ttu-id="c3f95-221">Aggiornare il costruttore di modello della pagina create per usare il `DI_BasePageModel` classe di base:</span><span class="sxs-lookup"><span data-stu-id="c3f95-221">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="c3f95-222">Aggiornamento di `CreateModel.OnPostAsync` metodo:</span><span class="sxs-lookup"><span data-stu-id="c3f95-222">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="c3f95-223">Aggiungere l'ID utente per il `Contact` modello.</span><span class="sxs-lookup"><span data-stu-id="c3f95-223">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="c3f95-224">Chiamare il gestore di autorizzazione per verificare che l'utente dispone dell'autorizzazione per creare contatti.</span><span class="sxs-lookup"><span data-stu-id="c3f95-224">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="c3f95-225">Aggiornare il IndexModel</span><span class="sxs-lookup"><span data-stu-id="c3f95-225">Update the IndexModel</span></span>

<span data-ttu-id="c3f95-226">Aggiornamento di `OnGetAsync` metodo in modo che solo i contatti approvati vengono visualizzati agli utenti generici:</span><span class="sxs-lookup"><span data-stu-id="c3f95-226">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="c3f95-227">Aggiornare il EditModel</span><span class="sxs-lookup"><span data-stu-id="c3f95-227">Update the EditModel</span></span>

<span data-ttu-id="c3f95-228">Aggiungere un gestore di autorizzazione per verificare che l'utente è proprietario del contatto.</span><span class="sxs-lookup"><span data-stu-id="c3f95-228">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="c3f95-229">Poiché viene convalidata l'autorizzazione alle risorse, il `[Authorize]` attributo non è sufficiente.</span><span class="sxs-lookup"><span data-stu-id="c3f95-229">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="c3f95-230">L'app non ha accesso alla risorsa quando vengono valutati gli attributi.</span><span class="sxs-lookup"><span data-stu-id="c3f95-230">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="c3f95-231">Autorizzazione basata sulle risorse deve essere imperativa.</span><span class="sxs-lookup"><span data-stu-id="c3f95-231">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="c3f95-232">Controlli devono essere eseguiti dopo che l'app potrà accedere alla risorsa, è possibile caricarli nel modello di pagina oppure caricarlo all'interno del gestore stesso.</span><span class="sxs-lookup"><span data-stu-id="c3f95-232">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="c3f95-233">Si accede di frequente della risorsa passando la chiave di risorsa.</span><span class="sxs-lookup"><span data-stu-id="c3f95-233">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="c3f95-234">Aggiornare il DeleteModel</span><span class="sxs-lookup"><span data-stu-id="c3f95-234">Update the DeleteModel</span></span>

<span data-ttu-id="c3f95-235">Aggiornare il modello di pagina delete per utilizzare il gestore dell'autorizzazione per verificare che l'utente disponga dell'autorizzazione di eliminazione per il contatto.</span><span class="sxs-lookup"><span data-stu-id="c3f95-235">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="c3f95-236">Inserire il servizio di autorizzazione in visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="c3f95-236">Inject the authorization service into the views</span></span>

<span data-ttu-id="c3f95-237">Attualmente, l'interfaccia utente Mostra modifica ed elimina i collegamenti per i contatti che l'utente non è possibile modificare.</span><span class="sxs-lookup"><span data-stu-id="c3f95-237">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="c3f95-238">Inserire il servizio di autorizzazione nel *viewimports* file in modo che sia disponibile per tutte le viste:</span><span class="sxs-lookup"><span data-stu-id="c3f95-238">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="c3f95-239">Il markup precedente aggiunge diverse `using` istruzioni.</span><span class="sxs-lookup"><span data-stu-id="c3f95-239">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="c3f95-240">Aggiorna il **modifica** e **eliminare** collega in *Pages/Contacts/Index.cshtml* in modo che vengono sottoposti a rendering solo per gli utenti che dispongono delle autorizzazioni appropriate:</span><span class="sxs-lookup"><span data-stu-id="c3f95-240">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="c3f95-241">Nascondere i collegamenti da utenti che non dispone dell'autorizzazione per modificare i dati non proteggere le app.</span><span class="sxs-lookup"><span data-stu-id="c3f95-241">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="c3f95-242">Nascondere i collegamenti rende più semplici da usare l'app mediante la visualizzazione di collegamenti solo validi.</span><span class="sxs-lookup"><span data-stu-id="c3f95-242">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="c3f95-243">Gli utenti possono hack gli URL generati per richiamare modifica ed eliminare le operazioni sui dati che non sono proprietari.</span><span class="sxs-lookup"><span data-stu-id="c3f95-243">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="c3f95-244">La pagina Razor o il controller deve applicare controlli di accesso per proteggere i dati.</span><span class="sxs-lookup"><span data-stu-id="c3f95-244">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="c3f95-245">Aggiorna i dettagli</span><span class="sxs-lookup"><span data-stu-id="c3f95-245">Update Details</span></span>

<span data-ttu-id="c3f95-246">Aggiornare la visualizzazione dei dettagli in modo che i responsabili possono approvare o rifiutare i contatti:</span><span class="sxs-lookup"><span data-stu-id="c3f95-246">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="c3f95-247">Aggiornare il modello di pagina dei dettagli:</span><span class="sxs-lookup"><span data-stu-id="c3f95-247">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="c3f95-248">Aggiungere o rimuovere un utente a un ruolo</span><span class="sxs-lookup"><span data-stu-id="c3f95-248">Add or remove a user to a role</span></span>

<span data-ttu-id="c3f95-249">Visualizzare [questo problema](https://github.com/aspnet/Docs/issues/8502) per informazioni su:</span><span class="sxs-lookup"><span data-stu-id="c3f95-249">See [this issue](https://github.com/aspnet/Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="c3f95-250">Rimozione dei privilegi da un utente.</span><span class="sxs-lookup"><span data-stu-id="c3f95-250">Removing privileges from a user.</span></span> <span data-ttu-id="c3f95-251">La disattivazione dell'audio, ad esempio un utente in un'app di chat.</span><span class="sxs-lookup"><span data-stu-id="c3f95-251">For example muting a user in a chat app.</span></span>
* <span data-ttu-id="c3f95-252">Aggiunta di privilegi a un utente.</span><span class="sxs-lookup"><span data-stu-id="c3f95-252">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="c3f95-253">Testare l'app completata</span><span class="sxs-lookup"><span data-stu-id="c3f95-253">Test the completed app</span></span>

<span data-ttu-id="c3f95-254">Se è già stata impostata una password per gli account utente di seeding, usare il [strumento Secret Manager](xref:security/app-secrets#secret-manager) per impostare una password:</span><span class="sxs-lookup"><span data-stu-id="c3f95-254">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="c3f95-255">Scegliere una password complessa: Usare otto o più caratteri e contenere almeno un carattere maiuscolo, numeri e simboli.</span><span class="sxs-lookup"><span data-stu-id="c3f95-255">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="c3f95-256">Ad esempio, `Passw0rd!` soddisfi i requisiti di password complesse.</span><span class="sxs-lookup"><span data-stu-id="c3f95-256">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="c3f95-257">Eseguire il comando seguente dalla cartella del progetto, in cui `<PW>` è la password:</span><span class="sxs-lookup"><span data-stu-id="c3f95-257">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

<span data-ttu-id="c3f95-258">Se l'app ha contatti:</span><span class="sxs-lookup"><span data-stu-id="c3f95-258">If the app has contacts:</span></span>

* <span data-ttu-id="c3f95-259">Eliminare tutti i record nel `Contact` tabella.</span><span class="sxs-lookup"><span data-stu-id="c3f95-259">Delete all of the records in the `Contact` table.</span></span>
* <span data-ttu-id="c3f95-260">Riavviare l'app per l'inizializzazione del database.</span><span class="sxs-lookup"><span data-stu-id="c3f95-260">Restart the app to seed the database.</span></span>

<span data-ttu-id="c3f95-261">Un modo semplice per testare l'app completata consiste nell'avviare tre diversi browser (o in incognito o InPrivate sessioni).</span><span class="sxs-lookup"><span data-stu-id="c3f95-261">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="c3f95-262">In un browser, registrare un nuovo utente (ad esempio, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="c3f95-262">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="c3f95-263">Accedere a ogni browser con un altro utente.</span><span class="sxs-lookup"><span data-stu-id="c3f95-263">Sign in to each browser with a different user.</span></span> <span data-ttu-id="c3f95-264">Verificare che le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="c3f95-264">Verify the following operations:</span></span>

* <span data-ttu-id="c3f95-265">Gli utenti registrati possono visualizzare tutti i dati di contatto approvati.</span><span class="sxs-lookup"><span data-stu-id="c3f95-265">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="c3f95-266">Gli utenti registrati possono modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="c3f95-266">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="c3f95-267">I responsabili possono approvare o rifiutare i dati di contatto.</span><span class="sxs-lookup"><span data-stu-id="c3f95-267">Managers can approve/reject contact data.</span></span> <span data-ttu-id="c3f95-268">Il `Details` viene mostrata **Approva** e **rifiutare** pulsanti.</span><span class="sxs-lookup"><span data-stu-id="c3f95-268">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="c3f95-269">Gli amministratori possono approvare o rifiutare e modificare/eliminare tutti i dati.</span><span class="sxs-lookup"><span data-stu-id="c3f95-269">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="c3f95-270">Utente</span><span class="sxs-lookup"><span data-stu-id="c3f95-270">User</span></span>                | <span data-ttu-id="c3f95-271">Esegue il seeding dell'app</span><span class="sxs-lookup"><span data-stu-id="c3f95-271">Seeded by the app</span></span> | <span data-ttu-id="c3f95-272">Opzioni</span><span class="sxs-lookup"><span data-stu-id="c3f95-272">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="c3f95-273">No</span><span class="sxs-lookup"><span data-stu-id="c3f95-273">No</span></span>                | <span data-ttu-id="c3f95-274">Modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="c3f95-274">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="c3f95-275">Yes</span><span class="sxs-lookup"><span data-stu-id="c3f95-275">Yes</span></span>               | <span data-ttu-id="c3f95-276">Approvare o rifiutare e modificare/eliminare i dati personalizzati.</span><span class="sxs-lookup"><span data-stu-id="c3f95-276">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="c3f95-277">Yes</span><span class="sxs-lookup"><span data-stu-id="c3f95-277">Yes</span></span>               | <span data-ttu-id="c3f95-278">Approvare o rifiutare e modificare/eliminare tutti i dati.</span><span class="sxs-lookup"><span data-stu-id="c3f95-278">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="c3f95-279">Creare un contatto nel browser dell'amministratore.</span><span class="sxs-lookup"><span data-stu-id="c3f95-279">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="c3f95-280">Copiare l'URL per l'eliminazione e modifica di contattare l'amministratore.</span><span class="sxs-lookup"><span data-stu-id="c3f95-280">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="c3f95-281">Incollare questi collegamenti nel browser dell'utente test per verificare che l'utente di test non è possibile eseguire queste operazioni.</span><span class="sxs-lookup"><span data-stu-id="c3f95-281">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="c3f95-282">Creare l'app di base</span><span class="sxs-lookup"><span data-stu-id="c3f95-282">Create the starter app</span></span>

* <span data-ttu-id="c3f95-283">Creare un'app Razor Pages denominata "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="c3f95-283">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="c3f95-284">Creare l'app con **singoli account utente**.</span><span class="sxs-lookup"><span data-stu-id="c3f95-284">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="c3f95-285">Il nome "ContactManager" in modo che lo spazio dei nomi corrispondente dello spazio dei nomi utilizzato nell'esempio.</span><span class="sxs-lookup"><span data-stu-id="c3f95-285">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="c3f95-286">`-uld` Specifica di Local DB invece di SQLite</span><span class="sxs-lookup"><span data-stu-id="c3f95-286">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="c3f95-287">Aggiungere *Models/Contact.cs*:</span><span class="sxs-lookup"><span data-stu-id="c3f95-287">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="c3f95-288">Scaffolding di `Contact` modello.</span><span class="sxs-lookup"><span data-stu-id="c3f95-288">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="c3f95-289">Creare la migrazione iniziale e l'aggiornamento del database:</span><span class="sxs-lookup"><span data-stu-id="c3f95-289">Create initial migration and update the database:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* <span data-ttu-id="c3f95-290">Aggiorna il **ContactManager** ancoraggio nel *cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="c3f95-290">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* <span data-ttu-id="c3f95-291">Testare l'app per la creazione, modifica ed eliminazione di un contatto</span><span class="sxs-lookup"><span data-stu-id="c3f95-291">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="c3f95-292">Specificare il valore di inizializzazione del database</span><span class="sxs-lookup"><span data-stu-id="c3f95-292">Seed the database</span></span>

<span data-ttu-id="c3f95-293">Aggiungere il [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) classe per il *dati* cartella.</span><span class="sxs-lookup"><span data-stu-id="c3f95-293">Add the [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="c3f95-294">Chiamare `SeedData.Initialize` da `Main`:</span><span class="sxs-lookup"><span data-stu-id="c3f95-294">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="c3f95-295">Verificare che l'app effettuato il seeding del database.</span><span class="sxs-lookup"><span data-stu-id="c3f95-295">Test that the app seeded the database.</span></span> <span data-ttu-id="c3f95-296">Se sono presenti tutte le righe nel DB del contatto, il metodo di inizializzazione non viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="c3f95-296">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="c3f95-297">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c3f95-297">Additional resources</span></span>

* [<span data-ttu-id="c3f95-298">Creare un'app web .NET Core e Database SQL di Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c3f95-298">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="c3f95-299">[ASP.NET Core autorizzazione Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="c3f95-299">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="c3f95-300">Questa esercitazione descrive in dettaglio più le funzionalità di sicurezza introdotte in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="c3f95-300">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="c3f95-301">Autorizzazione personalizzata basata su criteri</span><span class="sxs-lookup"><span data-stu-id="c3f95-301">Custom policy-based authorization</span></span>](xref:security/authorization/policies)

::: moniker-end
