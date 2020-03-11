---
title: Creare un'app ASP.NET Core con i dati utente protetti da autorizzazione
author: rick-anderson
description: Informazioni su come creare un'app Razor Pages con i dati utente protetti da autorizzazione. Include HTTPS, l'autenticazione, sicurezza, ASP.NET Core Identity.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: 7710a8965771db02e601dafb7da752906bcd43e5
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659579"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="43a35-104">Creare un'app ASP.NET Core con i dati utente protetti da autorizzazione</span><span class="sxs-lookup"><span data-stu-id="43a35-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="43a35-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="43a35-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="43a35-106">Vedere [questo file PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) per la versione ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="43a35-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="43a35-107">La versione ASP.NET Core 1,1 di questa esercitazione si trova in [questa](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) cartella.</span><span class="sxs-lookup"><span data-stu-id="43a35-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="43a35-108">L'esempio 1,1 ASP.NET Core è presente negli [esempi](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="43a35-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="43a35-109">Vedi [questo PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="43a35-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="43a35-110">Questa esercitazione illustra come creare un'app web ASP.NET Core con i dati utente protetti da autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="43a35-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="43a35-111">Viene visualizzato un elenco di contatti che (registrati) agli utenti autenticati hanno creato.</span><span class="sxs-lookup"><span data-stu-id="43a35-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="43a35-112">Sono disponibili tre gruppi di sicurezza:</span><span class="sxs-lookup"><span data-stu-id="43a35-112">There are three security groups:</span></span>

* <span data-ttu-id="43a35-113">**Gli utenti registrati** possono visualizzare tutti i dati approvati e possono modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="43a35-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="43a35-114">I **responsabili** possono approvare o rifiutare i dati di contatto.</span><span class="sxs-lookup"><span data-stu-id="43a35-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="43a35-115">Solo i contatti approvati sono visibili agli utenti.</span><span class="sxs-lookup"><span data-stu-id="43a35-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="43a35-116">Gli **amministratori** possono approvare/rifiutare e modificare/eliminare i dati.</span><span class="sxs-lookup"><span data-stu-id="43a35-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="43a35-117">Le immagini in questo documento non corrispondono esattamente ai modelli più recenti.</span><span class="sxs-lookup"><span data-stu-id="43a35-117">The images in this document don't exactly match the latest templates.</span></span>

<span data-ttu-id="43a35-118">Nell'immagine seguente l'utente Rick (`rick@example.com`) ha eseguito l'accesso.</span><span class="sxs-lookup"><span data-stu-id="43a35-118">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="43a35-119">Rick può visualizzare solo i contatti approvati e **modificare**/**eliminare**/**creare nuovi** collegamenti per i propri contatti.</span><span class="sxs-lookup"><span data-stu-id="43a35-119">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="43a35-120">Solo l'ultimo record, creato da Rick, Visualizza i collegamenti **modifica** ed **Elimina** .</span><span class="sxs-lookup"><span data-stu-id="43a35-120">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="43a35-121">Gli altri utenti non vedono l'ultimo record fino a quando un responsabile o l'amministratore assume lo stato "Approvato".</span><span class="sxs-lookup"><span data-stu-id="43a35-121">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Screenshot che mostra Rick effettuato l'accesso](secure-data/_static/rick.png)

<span data-ttu-id="43a35-123">Nella figura seguente `manager@contoso.com` è stato eseguito l'accesso e nel ruolo del Manager:</span><span class="sxs-lookup"><span data-stu-id="43a35-123">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Screenshot che mostra l'accesso manager@contoso.com](secure-data/_static/manager1.png)

<span data-ttu-id="43a35-125">L'immagine seguente mostra i gestori visualizzazione dettagli di un contatto:</span><span class="sxs-lookup"><span data-stu-id="43a35-125">The following image shows the managers details view of a contact:</span></span>

![Visualizzazione di gestione di un contatto](secure-data/_static/manager.png)

<span data-ttu-id="43a35-127">I pulsanti **approva** e **rifiuta** vengono visualizzati solo per Manager e amministratori.</span><span class="sxs-lookup"><span data-stu-id="43a35-127">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="43a35-128">Nella figura seguente `admin@contoso.com` è stato eseguito l'accesso e il ruolo dell'amministratore:</span><span class="sxs-lookup"><span data-stu-id="43a35-128">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Screenshot che mostra l'accesso admin@contoso.com](secure-data/_static/admin.png)

<span data-ttu-id="43a35-130">L'amministratore ha tutti i privilegi.</span><span class="sxs-lookup"><span data-stu-id="43a35-130">The administrator has all privileges.</span></span> <span data-ttu-id="43a35-131">Può leggere, modificare ed eliminare un contatto e modificare lo stato dei contatti.</span><span class="sxs-lookup"><span data-stu-id="43a35-131">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="43a35-132">L'app è stata creata mediante l' [impalcatura](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) del modello di `Contact` seguente:</span><span class="sxs-lookup"><span data-stu-id="43a35-132">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="43a35-133">L'esempio contiene i gestori di autorizzazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="43a35-133">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="43a35-134">`ContactIsOwnerAuthorizationHandler`: assicura che un utente possa modificare solo i propri dati.</span><span class="sxs-lookup"><span data-stu-id="43a35-134">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="43a35-135">`ContactManagerAuthorizationHandler`: consente ai responsabili di approvare o rifiutare i contatti.</span><span class="sxs-lookup"><span data-stu-id="43a35-135">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="43a35-136">`ContactAdministratorsAuthorizationHandler`: consente agli amministratori di approvare o rifiutare i contatti e di modificare/eliminare contatti.</span><span class="sxs-lookup"><span data-stu-id="43a35-136">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="43a35-137">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="43a35-137">Prerequisites</span></span>

<span data-ttu-id="43a35-138">Questa esercitazione viene fatto avanzare.</span><span class="sxs-lookup"><span data-stu-id="43a35-138">This tutorial is advanced.</span></span> <span data-ttu-id="43a35-139">È necessario avere familiarità con:</span><span class="sxs-lookup"><span data-stu-id="43a35-139">You should be familiar with:</span></span>

* [<span data-ttu-id="43a35-140">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="43a35-140">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="43a35-141">autenticazione</span><span class="sxs-lookup"><span data-stu-id="43a35-141">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="43a35-142">Conferma dell'account e recupero della password</span><span class="sxs-lookup"><span data-stu-id="43a35-142">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="43a35-143">autorizzazioni</span><span class="sxs-lookup"><span data-stu-id="43a35-143">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="43a35-144">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="43a35-144">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="43a35-145">Starter e app completata</span><span class="sxs-lookup"><span data-stu-id="43a35-145">The starter and completed app</span></span>

<span data-ttu-id="43a35-146">[Scaricare](xref:index#how-to-download-a-sample) l'app [completata](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) .</span><span class="sxs-lookup"><span data-stu-id="43a35-146">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="43a35-147">[Testare](#test-the-completed-app) l'app completata in modo da acquisire familiarità con le funzionalità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="43a35-147">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="43a35-148">L'app di base</span><span class="sxs-lookup"><span data-stu-id="43a35-148">The starter app</span></span>

<span data-ttu-id="43a35-149">[Scaricare](xref:index#how-to-download-a-sample) l'app [Starter](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) .</span><span class="sxs-lookup"><span data-stu-id="43a35-149">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="43a35-150">Eseguire l'app, toccare il collegamento **ContactManager** e verificare che sia possibile creare, modificare ed eliminare un contatto.</span><span class="sxs-lookup"><span data-stu-id="43a35-150">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="43a35-151">Proteggere i dati utente</span><span class="sxs-lookup"><span data-stu-id="43a35-151">Secure user data</span></span>

<span data-ttu-id="43a35-152">Le sezioni seguenti includono tutti i passaggi principali per creare l'app per dati sicura degli utenti.</span><span class="sxs-lookup"><span data-stu-id="43a35-152">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="43a35-153">Si può risultare utile per fare riferimento al progetto completato.</span><span class="sxs-lookup"><span data-stu-id="43a35-153">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="43a35-154">Associare i dati di contatto per l'utente</span><span class="sxs-lookup"><span data-stu-id="43a35-154">Tie the contact data to the user</span></span>

<span data-ttu-id="43a35-155">Usare l'ID utente dell' [identità](xref:security/authentication/identity) ASP.NET per assicurarsi che gli utenti possano modificare i dati, ma non i dati di altri utenti.</span><span class="sxs-lookup"><span data-stu-id="43a35-155">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="43a35-156">Aggiungere `OwnerID` e `ContactStatus` al modello di `Contact`:</span><span class="sxs-lookup"><span data-stu-id="43a35-156">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="43a35-157">`OwnerID` è l'ID dell'utente della tabella `AspNetUser` nel database di [identità](xref:security/authentication/identity) .</span><span class="sxs-lookup"><span data-stu-id="43a35-157">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="43a35-158">Il campo `Status` determina se un contatto è visualizzabile dagli utenti generali.</span><span class="sxs-lookup"><span data-stu-id="43a35-158">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="43a35-159">Creare una nuova migrazione e aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="43a35-159">Create a new migration and update the database:</span></span>

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="43a35-160">Aggiungere altri servizi di identità</span><span class="sxs-lookup"><span data-stu-id="43a35-160">Add Role services to Identity</span></span>

<span data-ttu-id="43a35-161">Aggiungere [Aggiungi ruoli](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) per aggiungere servizi ruolo:</span><span class="sxs-lookup"><span data-stu-id="43a35-161">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a><span data-ttu-id="43a35-162">Richiedere agli utenti autenticati</span><span class="sxs-lookup"><span data-stu-id="43a35-162">Require authenticated users</span></span>

<span data-ttu-id="43a35-163">Impostare i criteri di autenticazione predefinito per richiedere agli utenti di essere autenticato:</span><span class="sxs-lookup"><span data-stu-id="43a35-163">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 <span data-ttu-id="43a35-164">È possibile rifiutare esplicitamente l'autenticazione a livello di pagina Razor, controller o metodo di azione con l'attributo `[AllowAnonymous]`.</span><span class="sxs-lookup"><span data-stu-id="43a35-164">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="43a35-165">L'impostazione di criteri di autenticazione predefinito per richiedere agli utenti di essere autenticati protegge appena aggiunto Razor Pages e controller.</span><span class="sxs-lookup"><span data-stu-id="43a35-165">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="43a35-166">La necessità di eseguire l'autenticazione per impostazione predefinita è più sicura rispetto all'uso di nuovi controller e Razor Pages per includere l'attributo `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="43a35-166">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="43a35-167">Aggiungere [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) alle pagine indici e privacy in modo che gli utenti anonimi possano ottenere informazioni sul sito prima della registrazione.</span><span class="sxs-lookup"><span data-stu-id="43a35-167">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index and Privacy pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a><span data-ttu-id="43a35-168">Configurare l'account di test</span><span class="sxs-lookup"><span data-stu-id="43a35-168">Configure the test account</span></span>

<span data-ttu-id="43a35-169">La classe `SeedData` crea due account: Administrator e Manager.</span><span class="sxs-lookup"><span data-stu-id="43a35-169">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="43a35-170">Utilizzare lo [strumento Gestione segreta](xref:security/app-secrets) per impostare una password per questi account.</span><span class="sxs-lookup"><span data-stu-id="43a35-170">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="43a35-171">Impostare la password dalla directory del progetto (la directory contenente *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="43a35-171">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="43a35-172">Se non si specifica una password complessa, quando viene chiamato `SeedData.Initialize` viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="43a35-172">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="43a35-173">Aggiornare `Main` per usare la password di prova:</span><span class="sxs-lookup"><span data-stu-id="43a35-173">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="43a35-174">Creare gli account di test e aggiornare i contatti</span><span class="sxs-lookup"><span data-stu-id="43a35-174">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="43a35-175">Aggiornare il metodo `Initialize` nella classe `SeedData` per creare gli account di test:</span><span class="sxs-lookup"><span data-stu-id="43a35-175">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="43a35-176">Aggiungere l'ID utente amministratore e `ContactStatus` ai contatti.</span><span class="sxs-lookup"><span data-stu-id="43a35-176">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="43a35-177">Rendere uno dei contatti relativi al "Submitted" e "Rejected" uno.</span><span class="sxs-lookup"><span data-stu-id="43a35-177">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="43a35-178">Aggiungere l'ID utente e lo stato per tutti i contatti.</span><span class="sxs-lookup"><span data-stu-id="43a35-178">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="43a35-179">Viene visualizzato un solo contatto:</span><span class="sxs-lookup"><span data-stu-id="43a35-179">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="43a35-180">Creare proprietario, manager e i gestori di autorizzazione di amministratore</span><span class="sxs-lookup"><span data-stu-id="43a35-180">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="43a35-181">Creare una classe `ContactIsOwnerAuthorizationHandler` nella cartella *authorization* .</span><span class="sxs-lookup"><span data-stu-id="43a35-181">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="43a35-182">Il `ContactIsOwnerAuthorizationHandler` verifica che l'utente che agisce su una risorsa appartenga alla risorsa.</span><span class="sxs-lookup"><span data-stu-id="43a35-182">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="43a35-183">Il `ContactIsOwnerAuthorizationHandler` chiama il [contesto. Ha esito positivo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) se l'utente autenticato corrente è il proprietario del contatto.</span><span class="sxs-lookup"><span data-stu-id="43a35-183">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="43a35-184">I gestori di autorizzazione a livello generale:</span><span class="sxs-lookup"><span data-stu-id="43a35-184">Authorization handlers generally:</span></span>

* <span data-ttu-id="43a35-185">Restituisce `context.Succeed` quando vengono soddisfatti i requisiti.</span><span class="sxs-lookup"><span data-stu-id="43a35-185">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="43a35-186">Restituisce `Task.CompletedTask` quando i requisiti non vengono soddisfatti.</span><span class="sxs-lookup"><span data-stu-id="43a35-186">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="43a35-187">`Task.CompletedTask` non ha esito positivo o negativo&mdash;consente l'esecuzione di altri gestori di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="43a35-187">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="43a35-188">Se è necessario eseguire in modo esplicito l'errore, restituire [context. Esito negativo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="43a35-188">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="43a35-189">L'app consente ai proprietari di contatto di modifica/eliminazione/creare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="43a35-189">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="43a35-190">`ContactIsOwnerAuthorizationHandler` non è necessario controllare l'operazione passata nel parametro requirement.</span><span class="sxs-lookup"><span data-stu-id="43a35-190">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="43a35-191">Creare un gestore di autorizzazione di gestione</span><span class="sxs-lookup"><span data-stu-id="43a35-191">Create a manager authorization handler</span></span>

<span data-ttu-id="43a35-192">Creare una classe `ContactManagerAuthorizationHandler` nella cartella *authorization* .</span><span class="sxs-lookup"><span data-stu-id="43a35-192">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="43a35-193">Il `ContactManagerAuthorizationHandler` verifica che l'utente che agisce sulla risorsa sia un responsabile.</span><span class="sxs-lookup"><span data-stu-id="43a35-193">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="43a35-194">Solo i manager possono approvare o rifiutare le modifiche di contenuto (nuove o modificate).</span><span class="sxs-lookup"><span data-stu-id="43a35-194">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="43a35-195">Creare un gestore di autorizzazione di amministratore</span><span class="sxs-lookup"><span data-stu-id="43a35-195">Create an administrator authorization handler</span></span>

<span data-ttu-id="43a35-196">Creare una classe `ContactAdministratorsAuthorizationHandler` nella cartella *authorization* .</span><span class="sxs-lookup"><span data-stu-id="43a35-196">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="43a35-197">Il `ContactAdministratorsAuthorizationHandler` verifica che l'utente che agisce sulla risorsa sia un amministratore.</span><span class="sxs-lookup"><span data-stu-id="43a35-197">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="43a35-198">Amministratore può eseguire tutte le operazioni.</span><span class="sxs-lookup"><span data-stu-id="43a35-198">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="43a35-199">Registrare i gestori di eventi di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="43a35-199">Register the authorization handlers</span></span>

<span data-ttu-id="43a35-200">I servizi che usano Entity Framework Core devono essere registrati per l' [inserimento di dipendenze](xref:fundamentals/dependency-injection) usando [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="43a35-200">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="43a35-201">Il `ContactIsOwnerAuthorizationHandler` utilizza ASP.NET Core [identità](xref:security/authentication/identity), che è compilata in Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="43a35-201">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="43a35-202">Registrare i gestori con la raccolta di servizi in modo che siano disponibili per il `ContactsController` tramite l' [inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="43a35-202">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="43a35-203">Aggiungere il codice seguente alla fine del `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="43a35-203">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

<span data-ttu-id="43a35-204">`ContactAdministratorsAuthorizationHandler` e `ContactManagerAuthorizationHandler` vengono aggiunti come singleton.</span><span class="sxs-lookup"><span data-stu-id="43a35-204">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="43a35-205">Si tratta di singleton perché non usano EF e tutte le informazioni necessarie si trovano nel parametro `Context` del metodo `HandleRequirementAsync`.</span><span class="sxs-lookup"><span data-stu-id="43a35-205">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="43a35-206">Supporto di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="43a35-206">Support authorization</span></span>

<span data-ttu-id="43a35-207">In questa sezione aggiornare le pagine Razor e aggiungere una classe di requisiti di operazioni.</span><span class="sxs-lookup"><span data-stu-id="43a35-207">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="43a35-208">Esaminare la classe di requisiti di operazioni contatto</span><span class="sxs-lookup"><span data-stu-id="43a35-208">Review the contact operations requirements class</span></span>

<span data-ttu-id="43a35-209">Esaminare la classe `ContactOperations`.</span><span class="sxs-lookup"><span data-stu-id="43a35-209">Review the `ContactOperations` class.</span></span> <span data-ttu-id="43a35-210">Questa classe contiene i requisiti supportato dall'app:</span><span class="sxs-lookup"><span data-stu-id="43a35-210">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="43a35-211">Creare una classe di base per le pagine Razor di contatti</span><span class="sxs-lookup"><span data-stu-id="43a35-211">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="43a35-212">Creare una classe base che contiene i servizi usati in Razor Pages i contatti.</span><span class="sxs-lookup"><span data-stu-id="43a35-212">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="43a35-213">La classe di base inserisce il codice di inizializzazione in un'unica posizione:</span><span class="sxs-lookup"><span data-stu-id="43a35-213">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="43a35-214">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="43a35-214">The preceding code:</span></span>

* <span data-ttu-id="43a35-215">Aggiunge il servizio `IAuthorizationService` per accedere ai gestori di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="43a35-215">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="43a35-216">Aggiunge il servizio Identity `UserManager`.</span><span class="sxs-lookup"><span data-stu-id="43a35-216">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="43a35-217">Aggiungere l'oggetto `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="43a35-217">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="43a35-218">Aggiornare il CreateModel</span><span class="sxs-lookup"><span data-stu-id="43a35-218">Update the CreateModel</span></span>

<span data-ttu-id="43a35-219">Aggiornare il costruttore di modello di pagina create per usare la classe di base `DI_BasePageModel`:</span><span class="sxs-lookup"><span data-stu-id="43a35-219">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="43a35-220">Aggiornare il metodo `CreateModel.OnPostAsync` a:</span><span class="sxs-lookup"><span data-stu-id="43a35-220">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="43a35-221">Aggiungere l'ID utente al modello di `Contact`.</span><span class="sxs-lookup"><span data-stu-id="43a35-221">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="43a35-222">Chiamare il gestore di autorizzazione per verificare che l'utente dispone dell'autorizzazione per creare contatti.</span><span class="sxs-lookup"><span data-stu-id="43a35-222">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="43a35-223">Aggiornare il IndexModel</span><span class="sxs-lookup"><span data-stu-id="43a35-223">Update the IndexModel</span></span>

<span data-ttu-id="43a35-224">Aggiornare il metodo `OnGetAsync` in modo da visualizzare solo i contatti approvati per gli utenti generali:</span><span class="sxs-lookup"><span data-stu-id="43a35-224">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="43a35-225">Aggiornare il EditModel</span><span class="sxs-lookup"><span data-stu-id="43a35-225">Update the EditModel</span></span>

<span data-ttu-id="43a35-226">Aggiungere un gestore di autorizzazione per verificare che l'utente è proprietario del contatto.</span><span class="sxs-lookup"><span data-stu-id="43a35-226">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="43a35-227">Poiché l'autorizzazione della risorsa viene convalidata, l'attributo `[Authorize]` non è sufficiente.</span><span class="sxs-lookup"><span data-stu-id="43a35-227">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="43a35-228">L'app non ha accesso alla risorsa quando vengono valutati gli attributi.</span><span class="sxs-lookup"><span data-stu-id="43a35-228">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="43a35-229">Autorizzazione basata sulle risorse deve essere imperativa.</span><span class="sxs-lookup"><span data-stu-id="43a35-229">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="43a35-230">Controlli devono essere eseguiti dopo che l'app potrà accedere alla risorsa, è possibile caricarli nel modello di pagina oppure caricarlo all'interno del gestore stesso.</span><span class="sxs-lookup"><span data-stu-id="43a35-230">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="43a35-231">Si accede di frequente della risorsa passando la chiave di risorsa.</span><span class="sxs-lookup"><span data-stu-id="43a35-231">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="43a35-232">Aggiornare il DeleteModel</span><span class="sxs-lookup"><span data-stu-id="43a35-232">Update the DeleteModel</span></span>

<span data-ttu-id="43a35-233">Aggiornare il modello di pagina delete per utilizzare il gestore dell'autorizzazione per verificare che l'utente disponga dell'autorizzazione di eliminazione per il contatto.</span><span class="sxs-lookup"><span data-stu-id="43a35-233">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="43a35-234">Inserire il servizio di autorizzazione in visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="43a35-234">Inject the authorization service into the views</span></span>

<span data-ttu-id="43a35-235">Attualmente, l'interfaccia utente Mostra modifica ed elimina i collegamenti per i contatti che l'utente non è possibile modificare.</span><span class="sxs-lookup"><span data-stu-id="43a35-235">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="43a35-236">Inserire il servizio di autorizzazione nel file *pages/_ViewImports. cshtml* in modo che sia disponibile per tutte le visualizzazioni:</span><span class="sxs-lookup"><span data-stu-id="43a35-236">Inject the authorization service in the *Pages/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="43a35-237">Il markup precedente aggiunge diverse istruzioni `using`.</span><span class="sxs-lookup"><span data-stu-id="43a35-237">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="43a35-238">Aggiornare i collegamenti **Edit** ed **Delete** in *pages/Contacts/index. cshtml* in modo che vengano sottoposti a rendering solo per gli utenti con le autorizzazioni appropriate:</span><span class="sxs-lookup"><span data-stu-id="43a35-238">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="43a35-239">Nascondere i collegamenti da utenti che non dispone dell'autorizzazione per modificare i dati non proteggere le app.</span><span class="sxs-lookup"><span data-stu-id="43a35-239">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="43a35-240">Nascondere i collegamenti rende più semplici da usare l'app mediante la visualizzazione di collegamenti solo validi.</span><span class="sxs-lookup"><span data-stu-id="43a35-240">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="43a35-241">Gli utenti possono hack gli URL generati per richiamare modifica ed eliminare le operazioni sui dati che non sono proprietari.</span><span class="sxs-lookup"><span data-stu-id="43a35-241">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="43a35-242">La pagina Razor o il controller deve applicare controlli di accesso per proteggere i dati.</span><span class="sxs-lookup"><span data-stu-id="43a35-242">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="43a35-243">Aggiorna i dettagli</span><span class="sxs-lookup"><span data-stu-id="43a35-243">Update Details</span></span>

<span data-ttu-id="43a35-244">Aggiornare la visualizzazione dei dettagli in modo che i responsabili possono approvare o rifiutare i contatti:</span><span class="sxs-lookup"><span data-stu-id="43a35-244">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="43a35-245">Aggiornare il modello di pagina dei dettagli:</span><span class="sxs-lookup"><span data-stu-id="43a35-245">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="43a35-246">Aggiungere o rimuovere un utente a un ruolo</span><span class="sxs-lookup"><span data-stu-id="43a35-246">Add or remove a user to a role</span></span>

<span data-ttu-id="43a35-247">Per informazioni su, vedere [questo problema](https://github.com/dotnet/AspNetCore.Docs/issues/8502) :</span><span class="sxs-lookup"><span data-stu-id="43a35-247">See [this issue](https://github.com/dotnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="43a35-248">Rimozione dei privilegi da un utente.</span><span class="sxs-lookup"><span data-stu-id="43a35-248">Removing privileges from a user.</span></span> <span data-ttu-id="43a35-249">Ad esempio, disattivare un utente in un'app di chat.</span><span class="sxs-lookup"><span data-stu-id="43a35-249">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="43a35-250">Aggiunta di privilegi a un utente.</span><span class="sxs-lookup"><span data-stu-id="43a35-250">Adding privileges to a user.</span></span>

<a name="challenge"></a>

## <a name="differences-between-challenge-and-forbid"></a><span data-ttu-id="43a35-251">Differenze tra la sfida e la proibizione</span><span class="sxs-lookup"><span data-stu-id="43a35-251">Differences between Challenge and Forbid</span></span>

<span data-ttu-id="43a35-252">Questa app imposta i criteri predefiniti per [richiedere l'autenticazione degli utenti](#require-authenticated-users).</span><span class="sxs-lookup"><span data-stu-id="43a35-252">This app sets the default policy to [require authenticated users](#require-authenticated-users).</span></span> <span data-ttu-id="43a35-253">Il codice seguente consente agli utenti anonimi.</span><span class="sxs-lookup"><span data-stu-id="43a35-253">The following code allows anonymous users.</span></span> <span data-ttu-id="43a35-254">Gli utenti anonimi possono mostrare le differenze tra la richiesta di confronto e la proibizione.</span><span class="sxs-lookup"><span data-stu-id="43a35-254">Anonymous users are allowed to show the differences between Challenge vs Forbid.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details2.cshtml.cs?name=snippet)]

<span data-ttu-id="43a35-255">Nel codice precedente:</span><span class="sxs-lookup"><span data-stu-id="43a35-255">In the preceding code:</span></span>

* <span data-ttu-id="43a35-256">Quando l'utente **non** è autenticato, viene restituito un `ChallengeResult`.</span><span class="sxs-lookup"><span data-stu-id="43a35-256">When the user is **not** authenticated, a `ChallengeResult` is returned.</span></span> <span data-ttu-id="43a35-257">Quando viene restituito un `ChallengeResult`, l'utente viene reindirizzato alla pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="43a35-257">When a `ChallengeResult` is returned, the user is redirected to the sign-in page.</span></span>
* <span data-ttu-id="43a35-258">Quando l'utente viene autenticato, ma non autorizzato, viene restituito un `ForbidResult`.</span><span class="sxs-lookup"><span data-stu-id="43a35-258">When the user is authenticated, but not authorized, a `ForbidResult` is returned.</span></span> <span data-ttu-id="43a35-259">Quando viene restituito un `ForbidResult`, l'utente viene reindirizzato alla pagina di accesso negato.</span><span class="sxs-lookup"><span data-stu-id="43a35-259">When a `ForbidResult` is returned, the user is redirected to the access denied page.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="43a35-260">Testare l'app completata</span><span class="sxs-lookup"><span data-stu-id="43a35-260">Test the completed app</span></span>

<span data-ttu-id="43a35-261">Se non è già stata impostata una password per gli account utente di cui è stato eseguito il seeding, usare lo [strumento Gestione segreta](xref:security/app-secrets#secret-manager) per impostare una password:</span><span class="sxs-lookup"><span data-stu-id="43a35-261">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="43a35-262">Scegliere una password complessa: usare otto o più caratteri e contenere almeno un carattere maiuscolo, numero e simboli.</span><span class="sxs-lookup"><span data-stu-id="43a35-262">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="43a35-263">Ad esempio, `Passw0rd!` soddisfa i requisiti per le password complesse.</span><span class="sxs-lookup"><span data-stu-id="43a35-263">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="43a35-264">Eseguire il comando seguente dalla cartella del progetto, dove `<PW>` è la password:</span><span class="sxs-lookup"><span data-stu-id="43a35-264">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

<span data-ttu-id="43a35-265">Se l'app ha contatti:</span><span class="sxs-lookup"><span data-stu-id="43a35-265">If the app has contacts:</span></span>

* <span data-ttu-id="43a35-266">Eliminare tutti i record nella tabella `Contact`.</span><span class="sxs-lookup"><span data-stu-id="43a35-266">Delete all of the records in the `Contact` table.</span></span>
* <span data-ttu-id="43a35-267">Riavviare l'app per l'inizializzazione del database.</span><span class="sxs-lookup"><span data-stu-id="43a35-267">Restart the app to seed the database.</span></span>

<span data-ttu-id="43a35-268">Un modo semplice per testare l'app completata consiste nell'avviare tre diversi browser (o in incognito o InPrivate sessioni).</span><span class="sxs-lookup"><span data-stu-id="43a35-268">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="43a35-269">In un browser registrare un nuovo utente, ad esempio `test@example.com`.</span><span class="sxs-lookup"><span data-stu-id="43a35-269">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="43a35-270">Accedere a ogni browser con un altro utente.</span><span class="sxs-lookup"><span data-stu-id="43a35-270">Sign in to each browser with a different user.</span></span> <span data-ttu-id="43a35-271">Verificare che le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="43a35-271">Verify the following operations:</span></span>

* <span data-ttu-id="43a35-272">Gli utenti registrati possono visualizzare tutti i dati di contatto approvati.</span><span class="sxs-lookup"><span data-stu-id="43a35-272">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="43a35-273">Gli utenti registrati possono modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="43a35-273">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="43a35-274">I responsabili possono approvare o rifiutare i dati di contatto.</span><span class="sxs-lookup"><span data-stu-id="43a35-274">Managers can approve/reject contact data.</span></span> <span data-ttu-id="43a35-275">La visualizzazione `Details` Mostra i pulsanti **approva** e **rifiuta** .</span><span class="sxs-lookup"><span data-stu-id="43a35-275">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="43a35-276">Gli amministratori possono approvare o rifiutare e modificare/eliminare tutti i dati.</span><span class="sxs-lookup"><span data-stu-id="43a35-276">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="43a35-277">Utente</span><span class="sxs-lookup"><span data-stu-id="43a35-277">User</span></span>                | <span data-ttu-id="43a35-278">Esegue il seeding dell'app</span><span class="sxs-lookup"><span data-stu-id="43a35-278">Seeded by the app</span></span> | <span data-ttu-id="43a35-279">Opzioni</span><span class="sxs-lookup"><span data-stu-id="43a35-279">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="43a35-280">No</span><span class="sxs-lookup"><span data-stu-id="43a35-280">No</span></span>                | <span data-ttu-id="43a35-281">Modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="43a35-281">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="43a35-282">Sì</span><span class="sxs-lookup"><span data-stu-id="43a35-282">Yes</span></span>               | <span data-ttu-id="43a35-283">Approvare o rifiutare e modificare/eliminare i dati personalizzati.</span><span class="sxs-lookup"><span data-stu-id="43a35-283">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="43a35-284">Sì</span><span class="sxs-lookup"><span data-stu-id="43a35-284">Yes</span></span>               | <span data-ttu-id="43a35-285">Approvare o rifiutare e modificare/eliminare tutti i dati.</span><span class="sxs-lookup"><span data-stu-id="43a35-285">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="43a35-286">Creare un contatto nel browser dell'amministratore.</span><span class="sxs-lookup"><span data-stu-id="43a35-286">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="43a35-287">Copiare l'URL per l'eliminazione e modifica di contattare l'amministratore.</span><span class="sxs-lookup"><span data-stu-id="43a35-287">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="43a35-288">Incollare questi collegamenti nel browser dell'utente test per verificare che l'utente di test non è possibile eseguire queste operazioni.</span><span class="sxs-lookup"><span data-stu-id="43a35-288">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="43a35-289">Creare l'app di base</span><span class="sxs-lookup"><span data-stu-id="43a35-289">Create the starter app</span></span>

* <span data-ttu-id="43a35-290">Creare un'app Razor Pages denominata "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="43a35-290">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="43a35-291">Creare l'app con **singoli account utente**.</span><span class="sxs-lookup"><span data-stu-id="43a35-291">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="43a35-292">Il nome "ContactManager" in modo che lo spazio dei nomi corrispondente dello spazio dei nomi utilizzato nell'esempio.</span><span class="sxs-lookup"><span data-stu-id="43a35-292">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="43a35-293">`-uld` specifica il database locale anziché SQLite</span><span class="sxs-lookup"><span data-stu-id="43a35-293">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="43a35-294">Aggiungi *modelli/Contact. cs*:</span><span class="sxs-lookup"><span data-stu-id="43a35-294">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="43a35-295">Impalcatura del modello di `Contact`.</span><span class="sxs-lookup"><span data-stu-id="43a35-295">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="43a35-296">Creare la migrazione iniziale e l'aggiornamento del database:</span><span class="sxs-lookup"><span data-stu-id="43a35-296">Create initial migration and update the database:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

<span data-ttu-id="43a35-297">Se si verifica un bug con il comando `dotnet aspnet-codegenerator razorpage`, vedere [questo problema di GitHub](https://github.com/aspnet/Scaffolding/issues/984).</span><span class="sxs-lookup"><span data-stu-id="43a35-297">If you experience a bug with the `dotnet aspnet-codegenerator razorpage` command, see [this GitHub issue](https://github.com/aspnet/Scaffolding/issues/984).</span></span>

* <span data-ttu-id="43a35-298">Aggiornare l'ancoraggio **ContactManager** nel file *pages/Shared/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="43a35-298">Update the **ContactManager** anchor in the *Pages/Shared/_Layout.cshtml* file:</span></span>

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* <span data-ttu-id="43a35-299">Testare l'app per la creazione, modifica ed eliminazione di un contatto</span><span class="sxs-lookup"><span data-stu-id="43a35-299">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="43a35-300">Specificare il valore di inizializzazione del database</span><span class="sxs-lookup"><span data-stu-id="43a35-300">Seed the database</span></span>

<span data-ttu-id="43a35-301">Aggiungere la classe [SeedData](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) alla cartella *Data* :</span><span class="sxs-lookup"><span data-stu-id="43a35-301">Add the [SeedData](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) class to the *Data* folder:</span></span>

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

<span data-ttu-id="43a35-302">Chiama `SeedData.Initialize` da `Main`:</span><span class="sxs-lookup"><span data-stu-id="43a35-302">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

<span data-ttu-id="43a35-303">Verificare che l'app effettuato il seeding del database.</span><span class="sxs-lookup"><span data-stu-id="43a35-303">Test that the app seeded the database.</span></span> <span data-ttu-id="43a35-304">Se sono presenti tutte le righe nel DB del contatto, il metodo di inizializzazione non viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="43a35-304">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

<span data-ttu-id="43a35-305">Questa esercitazione illustra come creare un'app web ASP.NET Core con i dati utente protetti da autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="43a35-305">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="43a35-306">Viene visualizzato un elenco di contatti che (registrati) agli utenti autenticati hanno creato.</span><span class="sxs-lookup"><span data-stu-id="43a35-306">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="43a35-307">Sono disponibili tre gruppi di sicurezza:</span><span class="sxs-lookup"><span data-stu-id="43a35-307">There are three security groups:</span></span>

* <span data-ttu-id="43a35-308">**Gli utenti registrati** possono visualizzare tutti i dati approvati e possono modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="43a35-308">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="43a35-309">I **responsabili** possono approvare o rifiutare i dati di contatto.</span><span class="sxs-lookup"><span data-stu-id="43a35-309">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="43a35-310">Solo i contatti approvati sono visibili agli utenti.</span><span class="sxs-lookup"><span data-stu-id="43a35-310">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="43a35-311">Gli **amministratori** possono approvare/rifiutare e modificare/eliminare i dati.</span><span class="sxs-lookup"><span data-stu-id="43a35-311">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="43a35-312">Nell'immagine seguente l'utente Rick (`rick@example.com`) ha eseguito l'accesso.</span><span class="sxs-lookup"><span data-stu-id="43a35-312">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="43a35-313">Rick può visualizzare solo i contatti approvati e **modificare**/**eliminare**/**creare nuovi** collegamenti per i propri contatti.</span><span class="sxs-lookup"><span data-stu-id="43a35-313">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="43a35-314">Solo l'ultimo record, creato da Rick, Visualizza i collegamenti **modifica** ed **Elimina** .</span><span class="sxs-lookup"><span data-stu-id="43a35-314">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="43a35-315">Gli altri utenti non vedono l'ultimo record fino a quando un responsabile o l'amministratore assume lo stato "Approvato".</span><span class="sxs-lookup"><span data-stu-id="43a35-315">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Screenshot che mostra Rick effettuato l'accesso](secure-data/_static/rick.png)

<span data-ttu-id="43a35-317">Nella figura seguente `manager@contoso.com` è stato eseguito l'accesso e nel ruolo del Manager:</span><span class="sxs-lookup"><span data-stu-id="43a35-317">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Screenshot che mostra l'accesso manager@contoso.com](secure-data/_static/manager1.png)

<span data-ttu-id="43a35-319">L'immagine seguente mostra i gestori visualizzazione dettagli di un contatto:</span><span class="sxs-lookup"><span data-stu-id="43a35-319">The following image shows the managers details view of a contact:</span></span>

![Visualizzazione di gestione di un contatto](secure-data/_static/manager.png)

<span data-ttu-id="43a35-321">I pulsanti **approva** e **rifiuta** vengono visualizzati solo per Manager e amministratori.</span><span class="sxs-lookup"><span data-stu-id="43a35-321">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="43a35-322">Nella figura seguente `admin@contoso.com` è stato eseguito l'accesso e il ruolo dell'amministratore:</span><span class="sxs-lookup"><span data-stu-id="43a35-322">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Screenshot che mostra l'accesso admin@contoso.com](secure-data/_static/admin.png)

<span data-ttu-id="43a35-324">L'amministratore ha tutti i privilegi.</span><span class="sxs-lookup"><span data-stu-id="43a35-324">The administrator has all privileges.</span></span> <span data-ttu-id="43a35-325">Può leggere, modificare ed eliminare un contatto e modificare lo stato dei contatti.</span><span class="sxs-lookup"><span data-stu-id="43a35-325">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="43a35-326">L'app è stata creata mediante l' [impalcatura](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) del modello di `Contact` seguente:</span><span class="sxs-lookup"><span data-stu-id="43a35-326">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="43a35-327">L'esempio contiene i gestori di autorizzazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="43a35-327">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="43a35-328">`ContactIsOwnerAuthorizationHandler`: assicura che un utente possa modificare solo i propri dati.</span><span class="sxs-lookup"><span data-stu-id="43a35-328">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="43a35-329">`ContactManagerAuthorizationHandler`: consente ai responsabili di approvare o rifiutare i contatti.</span><span class="sxs-lookup"><span data-stu-id="43a35-329">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="43a35-330">`ContactAdministratorsAuthorizationHandler`: consente agli amministratori di approvare o rifiutare i contatti e di modificare/eliminare contatti.</span><span class="sxs-lookup"><span data-stu-id="43a35-330">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="43a35-331">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="43a35-331">Prerequisites</span></span>

<span data-ttu-id="43a35-332">Questa esercitazione viene fatto avanzare.</span><span class="sxs-lookup"><span data-stu-id="43a35-332">This tutorial is advanced.</span></span> <span data-ttu-id="43a35-333">È necessario avere familiarità con:</span><span class="sxs-lookup"><span data-stu-id="43a35-333">You should be familiar with:</span></span>

* [<span data-ttu-id="43a35-334">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="43a35-334">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="43a35-335">autenticazione</span><span class="sxs-lookup"><span data-stu-id="43a35-335">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="43a35-336">Conferma dell'account e recupero della password</span><span class="sxs-lookup"><span data-stu-id="43a35-336">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="43a35-337">autorizzazioni</span><span class="sxs-lookup"><span data-stu-id="43a35-337">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="43a35-338">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="43a35-338">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="43a35-339">Starter e app completata</span><span class="sxs-lookup"><span data-stu-id="43a35-339">The starter and completed app</span></span>

<span data-ttu-id="43a35-340">[Scaricare](xref:index#how-to-download-a-sample) l'app [completata](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) .</span><span class="sxs-lookup"><span data-stu-id="43a35-340">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="43a35-341">[Testare](#test-the-completed-app) l'app completata in modo da acquisire familiarità con le funzionalità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="43a35-341">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="43a35-342">L'app di base</span><span class="sxs-lookup"><span data-stu-id="43a35-342">The starter app</span></span>

<span data-ttu-id="43a35-343">[Scaricare](xref:index#how-to-download-a-sample) l'app [Starter](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) .</span><span class="sxs-lookup"><span data-stu-id="43a35-343">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="43a35-344">Eseguire l'app, toccare il collegamento **ContactManager** e verificare che sia possibile creare, modificare ed eliminare un contatto.</span><span class="sxs-lookup"><span data-stu-id="43a35-344">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="43a35-345">Proteggere i dati utente</span><span class="sxs-lookup"><span data-stu-id="43a35-345">Secure user data</span></span>

<span data-ttu-id="43a35-346">Le sezioni seguenti includono tutti i passaggi principali per creare l'app per dati sicura degli utenti.</span><span class="sxs-lookup"><span data-stu-id="43a35-346">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="43a35-347">Si può risultare utile per fare riferimento al progetto completato.</span><span class="sxs-lookup"><span data-stu-id="43a35-347">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="43a35-348">Associare i dati di contatto per l'utente</span><span class="sxs-lookup"><span data-stu-id="43a35-348">Tie the contact data to the user</span></span>

<span data-ttu-id="43a35-349">Usare l'ID utente dell' [identità](xref:security/authentication/identity) ASP.NET per assicurarsi che gli utenti possano modificare i dati, ma non i dati di altri utenti.</span><span class="sxs-lookup"><span data-stu-id="43a35-349">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="43a35-350">Aggiungere `OwnerID` e `ContactStatus` al modello di `Contact`:</span><span class="sxs-lookup"><span data-stu-id="43a35-350">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="43a35-351">`OwnerID` è l'ID dell'utente della tabella `AspNetUser` nel database di [identità](xref:security/authentication/identity) .</span><span class="sxs-lookup"><span data-stu-id="43a35-351">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="43a35-352">Il campo `Status` determina se un contatto è visualizzabile dagli utenti generali.</span><span class="sxs-lookup"><span data-stu-id="43a35-352">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="43a35-353">Creare una nuova migrazione e aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="43a35-353">Create a new migration and update the database:</span></span>

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="43a35-354">Aggiungere altri servizi di identità</span><span class="sxs-lookup"><span data-stu-id="43a35-354">Add Role services to Identity</span></span>

<span data-ttu-id="43a35-355">Aggiungere [Aggiungi ruoli](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) per aggiungere servizi ruolo:</span><span class="sxs-lookup"><span data-stu-id="43a35-355">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="43a35-356">Richiedere agli utenti autenticati</span><span class="sxs-lookup"><span data-stu-id="43a35-356">Require authenticated users</span></span>

<span data-ttu-id="43a35-357">Impostare i criteri di autenticazione predefinito per richiedere agli utenti di essere autenticato:</span><span class="sxs-lookup"><span data-stu-id="43a35-357">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="43a35-358">È possibile rifiutare esplicitamente l'autenticazione a livello di pagina Razor, controller o metodo di azione con l'attributo `[AllowAnonymous]`.</span><span class="sxs-lookup"><span data-stu-id="43a35-358">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="43a35-359">L'impostazione di criteri di autenticazione predefinito per richiedere agli utenti di essere autenticati protegge appena aggiunto Razor Pages e controller.</span><span class="sxs-lookup"><span data-stu-id="43a35-359">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="43a35-360">La necessità di eseguire l'autenticazione per impostazione predefinita è più sicura rispetto all'uso di nuovi controller e Razor Pages per includere l'attributo `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="43a35-360">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="43a35-361">Aggiungere [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) alle pagine di indice, informazioni e contatti in modo che gli utenti anonimi possano ottenere informazioni sul sito prima della registrazione.</span><span class="sxs-lookup"><span data-stu-id="43a35-361">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="43a35-362">Configurare l'account di test</span><span class="sxs-lookup"><span data-stu-id="43a35-362">Configure the test account</span></span>

<span data-ttu-id="43a35-363">La classe `SeedData` crea due account: Administrator e Manager.</span><span class="sxs-lookup"><span data-stu-id="43a35-363">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="43a35-364">Utilizzare lo [strumento Gestione segreta](xref:security/app-secrets) per impostare una password per questi account.</span><span class="sxs-lookup"><span data-stu-id="43a35-364">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="43a35-365">Impostare la password dalla directory del progetto (la directory contenente *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="43a35-365">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="43a35-366">Se non si specifica una password complessa, quando viene chiamato `SeedData.Initialize` viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="43a35-366">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="43a35-367">Aggiornare `Main` per usare la password di prova:</span><span class="sxs-lookup"><span data-stu-id="43a35-367">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="43a35-368">Creare gli account di test e aggiornare i contatti</span><span class="sxs-lookup"><span data-stu-id="43a35-368">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="43a35-369">Aggiornare il metodo `Initialize` nella classe `SeedData` per creare gli account di test:</span><span class="sxs-lookup"><span data-stu-id="43a35-369">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="43a35-370">Aggiungere l'ID utente amministratore e `ContactStatus` ai contatti.</span><span class="sxs-lookup"><span data-stu-id="43a35-370">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="43a35-371">Rendere uno dei contatti relativi al "Submitted" e "Rejected" uno.</span><span class="sxs-lookup"><span data-stu-id="43a35-371">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="43a35-372">Aggiungere l'ID utente e lo stato per tutti i contatti.</span><span class="sxs-lookup"><span data-stu-id="43a35-372">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="43a35-373">Viene visualizzato un solo contatto:</span><span class="sxs-lookup"><span data-stu-id="43a35-373">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="43a35-374">Creare proprietario, manager e i gestori di autorizzazione di amministratore</span><span class="sxs-lookup"><span data-stu-id="43a35-374">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="43a35-375">Creare una cartella di *autorizzazione* e crearvi una classe `ContactIsOwnerAuthorizationHandler`.</span><span class="sxs-lookup"><span data-stu-id="43a35-375">Create an *Authorization* folder and create a `ContactIsOwnerAuthorizationHandler` class in it.</span></span> <span data-ttu-id="43a35-376">Il `ContactIsOwnerAuthorizationHandler` verifica che l'utente che agisce su una risorsa appartenga alla risorsa.</span><span class="sxs-lookup"><span data-stu-id="43a35-376">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="43a35-377">Il `ContactIsOwnerAuthorizationHandler` chiama il [contesto. Ha esito positivo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) se l'utente autenticato corrente è il proprietario del contatto.</span><span class="sxs-lookup"><span data-stu-id="43a35-377">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="43a35-378">I gestori di autorizzazione a livello generale:</span><span class="sxs-lookup"><span data-stu-id="43a35-378">Authorization handlers generally:</span></span>

* <span data-ttu-id="43a35-379">Restituisce `context.Succeed` quando vengono soddisfatti i requisiti.</span><span class="sxs-lookup"><span data-stu-id="43a35-379">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="43a35-380">Restituisce `Task.CompletedTask` quando i requisiti non vengono soddisfatti.</span><span class="sxs-lookup"><span data-stu-id="43a35-380">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="43a35-381">`Task.CompletedTask` non ha esito positivo o negativo&mdash;consente l'esecuzione di altri gestori di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="43a35-381">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="43a35-382">Se è necessario eseguire in modo esplicito l'errore, restituire [context. Esito negativo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="43a35-382">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="43a35-383">L'app consente ai proprietari di contatto di modifica/eliminazione/creare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="43a35-383">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="43a35-384">`ContactIsOwnerAuthorizationHandler` non è necessario controllare l'operazione passata nel parametro requirement.</span><span class="sxs-lookup"><span data-stu-id="43a35-384">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="43a35-385">Creare un gestore di autorizzazione di gestione</span><span class="sxs-lookup"><span data-stu-id="43a35-385">Create a manager authorization handler</span></span>

<span data-ttu-id="43a35-386">Creare una classe `ContactManagerAuthorizationHandler` nella cartella *authorization* .</span><span class="sxs-lookup"><span data-stu-id="43a35-386">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="43a35-387">Il `ContactManagerAuthorizationHandler` verifica che l'utente che agisce sulla risorsa sia un responsabile.</span><span class="sxs-lookup"><span data-stu-id="43a35-387">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="43a35-388">Solo i manager possono approvare o rifiutare le modifiche di contenuto (nuove o modificate).</span><span class="sxs-lookup"><span data-stu-id="43a35-388">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="43a35-389">Creare un gestore di autorizzazione di amministratore</span><span class="sxs-lookup"><span data-stu-id="43a35-389">Create an administrator authorization handler</span></span>

<span data-ttu-id="43a35-390">Creare una classe `ContactAdministratorsAuthorizationHandler` nella cartella *authorization* .</span><span class="sxs-lookup"><span data-stu-id="43a35-390">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="43a35-391">Il `ContactAdministratorsAuthorizationHandler` verifica che l'utente che agisce sulla risorsa sia un amministratore.</span><span class="sxs-lookup"><span data-stu-id="43a35-391">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="43a35-392">Amministratore può eseguire tutte le operazioni.</span><span class="sxs-lookup"><span data-stu-id="43a35-392">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="43a35-393">Registrare i gestori di eventi di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="43a35-393">Register the authorization handlers</span></span>

<span data-ttu-id="43a35-394">I servizi che usano Entity Framework Core devono essere registrati per l' [inserimento di dipendenze](xref:fundamentals/dependency-injection) usando [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="43a35-394">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="43a35-395">Il `ContactIsOwnerAuthorizationHandler` utilizza ASP.NET Core [identità](xref:security/authentication/identity), che è compilata in Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="43a35-395">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="43a35-396">Registrare i gestori con la raccolta di servizi in modo che siano disponibili per il `ContactsController` tramite l' [inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="43a35-396">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="43a35-397">Aggiungere il codice seguente alla fine del `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="43a35-397">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="43a35-398">`ContactAdministratorsAuthorizationHandler` e `ContactManagerAuthorizationHandler` vengono aggiunti come singleton.</span><span class="sxs-lookup"><span data-stu-id="43a35-398">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="43a35-399">Si tratta di singleton perché non usano EF e tutte le informazioni necessarie si trovano nel parametro `Context` del metodo `HandleRequirementAsync`.</span><span class="sxs-lookup"><span data-stu-id="43a35-399">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="43a35-400">Supporto di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="43a35-400">Support authorization</span></span>

<span data-ttu-id="43a35-401">In questa sezione aggiornare le pagine Razor e aggiungere una classe di requisiti di operazioni.</span><span class="sxs-lookup"><span data-stu-id="43a35-401">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="43a35-402">Esaminare la classe di requisiti di operazioni contatto</span><span class="sxs-lookup"><span data-stu-id="43a35-402">Review the contact operations requirements class</span></span>

<span data-ttu-id="43a35-403">Esaminare la classe `ContactOperations`.</span><span class="sxs-lookup"><span data-stu-id="43a35-403">Review the `ContactOperations` class.</span></span> <span data-ttu-id="43a35-404">Questa classe contiene i requisiti supportato dall'app:</span><span class="sxs-lookup"><span data-stu-id="43a35-404">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="43a35-405">Creare una classe di base per le pagine Razor di contatti</span><span class="sxs-lookup"><span data-stu-id="43a35-405">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="43a35-406">Creare una classe base che contiene i servizi usati in Razor Pages i contatti.</span><span class="sxs-lookup"><span data-stu-id="43a35-406">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="43a35-407">La classe di base inserisce il codice di inizializzazione in un'unica posizione:</span><span class="sxs-lookup"><span data-stu-id="43a35-407">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="43a35-408">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="43a35-408">The preceding code:</span></span>

* <span data-ttu-id="43a35-409">Aggiunge il servizio `IAuthorizationService` per accedere ai gestori di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="43a35-409">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="43a35-410">Aggiunge il servizio Identity `UserManager`.</span><span class="sxs-lookup"><span data-stu-id="43a35-410">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="43a35-411">Aggiungere l'oggetto `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="43a35-411">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="43a35-412">Aggiornare il CreateModel</span><span class="sxs-lookup"><span data-stu-id="43a35-412">Update the CreateModel</span></span>

<span data-ttu-id="43a35-413">Aggiornare il costruttore di modello di pagina create per usare la classe di base `DI_BasePageModel`:</span><span class="sxs-lookup"><span data-stu-id="43a35-413">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="43a35-414">Aggiornare il metodo `CreateModel.OnPostAsync` a:</span><span class="sxs-lookup"><span data-stu-id="43a35-414">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="43a35-415">Aggiungere l'ID utente al modello di `Contact`.</span><span class="sxs-lookup"><span data-stu-id="43a35-415">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="43a35-416">Chiamare il gestore di autorizzazione per verificare che l'utente dispone dell'autorizzazione per creare contatti.</span><span class="sxs-lookup"><span data-stu-id="43a35-416">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="43a35-417">Aggiornare il IndexModel</span><span class="sxs-lookup"><span data-stu-id="43a35-417">Update the IndexModel</span></span>

<span data-ttu-id="43a35-418">Aggiornare il metodo `OnGetAsync` in modo da visualizzare solo i contatti approvati per gli utenti generali:</span><span class="sxs-lookup"><span data-stu-id="43a35-418">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="43a35-419">Aggiornare il EditModel</span><span class="sxs-lookup"><span data-stu-id="43a35-419">Update the EditModel</span></span>

<span data-ttu-id="43a35-420">Aggiungere un gestore di autorizzazione per verificare che l'utente è proprietario del contatto.</span><span class="sxs-lookup"><span data-stu-id="43a35-420">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="43a35-421">Poiché l'autorizzazione della risorsa viene convalidata, l'attributo `[Authorize]` non è sufficiente.</span><span class="sxs-lookup"><span data-stu-id="43a35-421">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="43a35-422">L'app non ha accesso alla risorsa quando vengono valutati gli attributi.</span><span class="sxs-lookup"><span data-stu-id="43a35-422">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="43a35-423">Autorizzazione basata sulle risorse deve essere imperativa.</span><span class="sxs-lookup"><span data-stu-id="43a35-423">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="43a35-424">Controlli devono essere eseguiti dopo che l'app potrà accedere alla risorsa, è possibile caricarli nel modello di pagina oppure caricarlo all'interno del gestore stesso.</span><span class="sxs-lookup"><span data-stu-id="43a35-424">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="43a35-425">Si accede di frequente della risorsa passando la chiave di risorsa.</span><span class="sxs-lookup"><span data-stu-id="43a35-425">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="43a35-426">Aggiornare il DeleteModel</span><span class="sxs-lookup"><span data-stu-id="43a35-426">Update the DeleteModel</span></span>

<span data-ttu-id="43a35-427">Aggiornare il modello di pagina delete per utilizzare il gestore dell'autorizzazione per verificare che l'utente disponga dell'autorizzazione di eliminazione per il contatto.</span><span class="sxs-lookup"><span data-stu-id="43a35-427">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="43a35-428">Inserire il servizio di autorizzazione in visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="43a35-428">Inject the authorization service into the views</span></span>

<span data-ttu-id="43a35-429">Attualmente, l'interfaccia utente Mostra modifica ed elimina i collegamenti per i contatti che l'utente non è possibile modificare.</span><span class="sxs-lookup"><span data-stu-id="43a35-429">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="43a35-430">Inserire il servizio di autorizzazione nel file *views/_ViewImports. cshtml* in modo che sia disponibile per tutte le visualizzazioni:</span><span class="sxs-lookup"><span data-stu-id="43a35-430">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="43a35-431">Il markup precedente aggiunge diverse istruzioni `using`.</span><span class="sxs-lookup"><span data-stu-id="43a35-431">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="43a35-432">Aggiornare i collegamenti **Edit** ed **Delete** in *pages/Contacts/index. cshtml* in modo che vengano sottoposti a rendering solo per gli utenti con le autorizzazioni appropriate:</span><span class="sxs-lookup"><span data-stu-id="43a35-432">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="43a35-433">Nascondere i collegamenti da utenti che non dispone dell'autorizzazione per modificare i dati non proteggere le app.</span><span class="sxs-lookup"><span data-stu-id="43a35-433">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="43a35-434">Nascondere i collegamenti rende più semplici da usare l'app mediante la visualizzazione di collegamenti solo validi.</span><span class="sxs-lookup"><span data-stu-id="43a35-434">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="43a35-435">Gli utenti possono hack gli URL generati per richiamare modifica ed eliminare le operazioni sui dati che non sono proprietari.</span><span class="sxs-lookup"><span data-stu-id="43a35-435">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="43a35-436">La pagina Razor o il controller deve applicare controlli di accesso per proteggere i dati.</span><span class="sxs-lookup"><span data-stu-id="43a35-436">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="43a35-437">Aggiorna i dettagli</span><span class="sxs-lookup"><span data-stu-id="43a35-437">Update Details</span></span>

<span data-ttu-id="43a35-438">Aggiornare la visualizzazione dei dettagli in modo che i responsabili possono approvare o rifiutare i contatti:</span><span class="sxs-lookup"><span data-stu-id="43a35-438">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="43a35-439">Aggiornare il modello di pagina dei dettagli:</span><span class="sxs-lookup"><span data-stu-id="43a35-439">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="43a35-440">Aggiungere o rimuovere un utente a un ruolo</span><span class="sxs-lookup"><span data-stu-id="43a35-440">Add or remove a user to a role</span></span>

<span data-ttu-id="43a35-441">Per informazioni su, vedere [questo problema](https://github.com/dotnet/AspNetCore.Docs/issues/8502) :</span><span class="sxs-lookup"><span data-stu-id="43a35-441">See [this issue](https://github.com/dotnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="43a35-442">Rimozione dei privilegi da un utente.</span><span class="sxs-lookup"><span data-stu-id="43a35-442">Removing privileges from a user.</span></span> <span data-ttu-id="43a35-443">Ad esempio, disattivare un utente in un'app di chat.</span><span class="sxs-lookup"><span data-stu-id="43a35-443">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="43a35-444">Aggiunta di privilegi a un utente.</span><span class="sxs-lookup"><span data-stu-id="43a35-444">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="43a35-445">Testare l'app completata</span><span class="sxs-lookup"><span data-stu-id="43a35-445">Test the completed app</span></span>

<span data-ttu-id="43a35-446">Se non è già stata impostata una password per gli account utente di cui è stato eseguito il seeding, usare lo [strumento Gestione segreta](xref:security/app-secrets#secret-manager) per impostare una password:</span><span class="sxs-lookup"><span data-stu-id="43a35-446">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="43a35-447">Scegliere una password complessa: usare otto o più caratteri e contenere almeno un carattere maiuscolo, numero e simboli.</span><span class="sxs-lookup"><span data-stu-id="43a35-447">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="43a35-448">Ad esempio, `Passw0rd!` soddisfa i requisiti per le password complesse.</span><span class="sxs-lookup"><span data-stu-id="43a35-448">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="43a35-449">Eseguire il comando seguente dalla cartella del progetto, dove `<PW>` è la password:</span><span class="sxs-lookup"><span data-stu-id="43a35-449">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

* <span data-ttu-id="43a35-450">Eliminare e aggiornare il database</span><span class="sxs-lookup"><span data-stu-id="43a35-450">Drop and update the Database</span></span>

  ```dotnetcli
  dotnet ef database drop -f
  dotnet ef database update  
  ```

* <span data-ttu-id="43a35-451">Riavviare l'app per l'inizializzazione del database.</span><span class="sxs-lookup"><span data-stu-id="43a35-451">Restart the app to seed the database.</span></span>

<span data-ttu-id="43a35-452">Un modo semplice per testare l'app completata consiste nell'avviare tre diversi browser (o in incognito o InPrivate sessioni).</span><span class="sxs-lookup"><span data-stu-id="43a35-452">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="43a35-453">In un browser registrare un nuovo utente, ad esempio `test@example.com`.</span><span class="sxs-lookup"><span data-stu-id="43a35-453">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="43a35-454">Accedere a ogni browser con un altro utente.</span><span class="sxs-lookup"><span data-stu-id="43a35-454">Sign in to each browser with a different user.</span></span> <span data-ttu-id="43a35-455">Verificare che le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="43a35-455">Verify the following operations:</span></span>

* <span data-ttu-id="43a35-456">Gli utenti registrati possono visualizzare tutti i dati di contatto approvati.</span><span class="sxs-lookup"><span data-stu-id="43a35-456">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="43a35-457">Gli utenti registrati possono modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="43a35-457">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="43a35-458">I responsabili possono approvare o rifiutare i dati di contatto.</span><span class="sxs-lookup"><span data-stu-id="43a35-458">Managers can approve/reject contact data.</span></span> <span data-ttu-id="43a35-459">La visualizzazione `Details` Mostra i pulsanti **approva** e **rifiuta** .</span><span class="sxs-lookup"><span data-stu-id="43a35-459">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="43a35-460">Gli amministratori possono approvare o rifiutare e modificare/eliminare tutti i dati.</span><span class="sxs-lookup"><span data-stu-id="43a35-460">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="43a35-461">Utente</span><span class="sxs-lookup"><span data-stu-id="43a35-461">User</span></span>                | <span data-ttu-id="43a35-462">Esegue il seeding dell'app</span><span class="sxs-lookup"><span data-stu-id="43a35-462">Seeded by the app</span></span> | <span data-ttu-id="43a35-463">Opzioni</span><span class="sxs-lookup"><span data-stu-id="43a35-463">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="43a35-464">No</span><span class="sxs-lookup"><span data-stu-id="43a35-464">No</span></span>                | <span data-ttu-id="43a35-465">Modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="43a35-465">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="43a35-466">Sì</span><span class="sxs-lookup"><span data-stu-id="43a35-466">Yes</span></span>               | <span data-ttu-id="43a35-467">Approvare o rifiutare e modificare/eliminare i dati personalizzati.</span><span class="sxs-lookup"><span data-stu-id="43a35-467">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="43a35-468">Sì</span><span class="sxs-lookup"><span data-stu-id="43a35-468">Yes</span></span>               | <span data-ttu-id="43a35-469">Approvare o rifiutare e modificare/eliminare tutti i dati.</span><span class="sxs-lookup"><span data-stu-id="43a35-469">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="43a35-470">Creare un contatto nel browser dell'amministratore.</span><span class="sxs-lookup"><span data-stu-id="43a35-470">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="43a35-471">Copiare l'URL per l'eliminazione e modifica di contattare l'amministratore.</span><span class="sxs-lookup"><span data-stu-id="43a35-471">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="43a35-472">Incollare questi collegamenti nel browser dell'utente test per verificare che l'utente di test non è possibile eseguire queste operazioni.</span><span class="sxs-lookup"><span data-stu-id="43a35-472">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="43a35-473">Creare l'app di base</span><span class="sxs-lookup"><span data-stu-id="43a35-473">Create the starter app</span></span>

* <span data-ttu-id="43a35-474">Creare un'app Razor Pages denominata "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="43a35-474">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="43a35-475">Creare l'app con **singoli account utente**.</span><span class="sxs-lookup"><span data-stu-id="43a35-475">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="43a35-476">Il nome "ContactManager" in modo che lo spazio dei nomi corrispondente dello spazio dei nomi utilizzato nell'esempio.</span><span class="sxs-lookup"><span data-stu-id="43a35-476">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="43a35-477">`-uld` specifica il database locale anziché SQLite</span><span class="sxs-lookup"><span data-stu-id="43a35-477">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="43a35-478">Aggiungi *modelli/Contact. cs*:</span><span class="sxs-lookup"><span data-stu-id="43a35-478">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="43a35-479">Impalcatura del modello di `Contact`.</span><span class="sxs-lookup"><span data-stu-id="43a35-479">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="43a35-480">Creare la migrazione iniziale e l'aggiornamento del database:</span><span class="sxs-lookup"><span data-stu-id="43a35-480">Create initial migration and update the database:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* <span data-ttu-id="43a35-481">Aggiornare l'ancoraggio **ContactManager** nel file *pages/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="43a35-481">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* <span data-ttu-id="43a35-482">Testare l'app per la creazione, modifica ed eliminazione di un contatto</span><span class="sxs-lookup"><span data-stu-id="43a35-482">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="43a35-483">Specificare il valore di inizializzazione del database</span><span class="sxs-lookup"><span data-stu-id="43a35-483">Seed the database</span></span>

<span data-ttu-id="43a35-484">Aggiungere la classe [SeedData](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) alla cartella *Data* .</span><span class="sxs-lookup"><span data-stu-id="43a35-484">Add the [SeedData](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="43a35-485">Chiama `SeedData.Initialize` da `Main`:</span><span class="sxs-lookup"><span data-stu-id="43a35-485">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="43a35-486">Verificare che l'app effettuato il seeding del database.</span><span class="sxs-lookup"><span data-stu-id="43a35-486">Test that the app seeded the database.</span></span> <span data-ttu-id="43a35-487">Se sono presenti tutte le righe nel DB del contatto, il metodo di inizializzazione non viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="43a35-487">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="43a35-488">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="43a35-488">Additional resources</span></span>

* [<span data-ttu-id="43a35-489">Creare un'app Web .NET Core e database SQL nel servizio app Azure</span><span class="sxs-lookup"><span data-stu-id="43a35-489">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="43a35-490">[ASP.NET Core Lab di autorizzazione](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="43a35-490">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="43a35-491">Questa esercitazione descrive in dettaglio più le funzionalità di sicurezza introdotte in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="43a35-491">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="43a35-492">Autorizzazione personalizzata basata su criteri</span><span class="sxs-lookup"><span data-stu-id="43a35-492">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
