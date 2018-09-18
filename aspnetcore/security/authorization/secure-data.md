---
title: Creare un'app ASP.NET Core con i dati utente protetti da autorizzazione
author: rick-anderson
description: Informazioni su come creare un'app Razor Pages con i dati utente protetti da autorizzazione. Include HTTPS, l'autenticazione, sicurezza, ASP.NET Core Identity.
ms.author: riande
ms.date: 7/24/2018
uid: security/authorization/secure-data
ms.openlocfilehash: 2fb13f0772a1f8aa4ed2ff3ece2a2c5d3b7360c9
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46010936"
---
::: moniker range="<= aspnetcore-1.1"

Visualizzare [questo PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) per la versione di ASP.NET Core MVC. La versione di ASP.NET Core 1.1 di questa esercitazione è nel [ciò](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) cartella. 1.1 esempio ASP.NET Core è nel [esempi](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Vedere [questo pdf](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_July16_18.pdf)

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Creare un'app ASP.NET Core con i dati utente protetti da autorizzazione

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)

Questa esercitazione illustra come creare un'app web ASP.NET Core con i dati utente protetti da autorizzazione. Viene visualizzato un elenco di contatti che (registrati) agli utenti autenticati hanno creato. Sono disponibili tre gruppi di sicurezza:

* **Utenti registrati** può visualizzare tutti i dati approvati e può modificare/eliminare i propri dati.
* **I responsabili** possono approvare o rifiutare i dati di contatto. Solo i contatti approvati sono visibili agli utenti.
* **Gli amministratori** può approvare o rifiutare e modificare/eliminare tutti i dati.

Nell'immagine seguente, l'utente Rick (`rick@example.com`) ha effettuato l'accesso. Rick possono visualizzare solo i contatti approvati e **Edit**/**eliminare**/**Crea nuovo** collegamenti per il suo i contatti. Solo l'ultimo record, creato da Rick, consente di visualizzare **Edit** e **eliminare** collegamenti. Gli altri utenti non vedono l'ultimo record fino a quando un responsabile o l'amministratore assume lo stato "Approvato".

![immagine descritto precedente](secure-data/_static/rick.png)

Nell'immagine seguente, `manager@contoso.com` viene effettuato l'accesso e nel ruolo managers:

![immagine descritto precedente](secure-data/_static/manager1.png)

L'immagine seguente mostra i gestori visualizzazione dettagli di un contatto:

![immagine descritto precedente](secure-data/_static/manager.png)

Il **Approve** e **rifiutare** pulsanti vengono visualizzati solo per i gestori e amministratori.

Nell'immagine seguente, `admin@contoso.com` viene effettuato l'accesso e al ruolo administrators:

![immagine descritto precedente](secure-data/_static/admin.png)

L'amministratore ha tutti i privilegi. Può leggere, modificare ed eliminare un contatto e modificare lo stato dei contatti.

L'app è stato creato da [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) seguenti `Contact` modello:

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet)]

L'esempio contiene i gestori di autorizzazione seguenti:

* `ContactIsOwnerAuthorizationHandler`: Assicura che un utente può modificare solo i propri dati.
* `ContactManagerAuthorizationHandler`: Consente ai responsabili di approvare o rifiutare i contatti.
* `ContactAdministratorsAuthorizationHandler`: Consente agli amministratori di approvare o rifiutare i contatti e di modificare/eliminare i contatti.

## <a name="prerequisites"></a>Prerequisiti

Questa esercitazione viene fatto avanzare. È necessario avere familiarità con:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Autenticazione](xref:security/authentication/index)
* [Conferma account e recupero password](xref:security/authentication/accconfirm)
* [Autorizzazione](xref:security/authorization/index)
* [Entity Framework Core](xref:data/ef-mvc/intro)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

In ASP.NET Core 2.1 `User.IsInRole` ha esito negativo quando si usa `AddDefaultIdentity`. Questa esercitazione Usa `AddDefaultIdentity` e pertanto richiede ASP.NET Core 2.2 preview 1 o versione successiva. Visualizzare [questo problema su GitHub](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) per una soluzione alternativa.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="the-starter-and-completed-app"></a>Starter e app completata

[Scaricare](xref:tutorials/index#how-to-download-a-sample) il [completato](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app. [Test](#test-the-completed-app) dell'app completata in modo da avere acquisito familiarità con le funzionalità di sicurezza.

### <a name="the-starter-app"></a>L'app di base

[Scaricare](xref:tutorials/index#how-to-download-a-sample) il [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.

Eseguire l'app, toccare il **ContactManager** collegare e verificare è possibile creare, modificare ed eliminare un contatto.

## <a name="secure-user-data"></a>Proteggere i dati utente

Le sezioni seguenti includono tutti i passaggi principali per creare l'app per dati sicura degli utenti. Si può risultare utile per fare riferimento al progetto completato.

### <a name="tie-the-contact-data-to-the-user"></a>Associare i dati di contatto per l'utente

Usare ASP.NET [identità](xref:security/authentication/identity) ID utente per garantire che gli utenti possono modificare i propri dati, ma non altri dati degli utenti. Aggiungere `OwnerID` e `ContactStatus` per il `Contact` modello:

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` è l'ID dell'utente dal `AspNetUser` tabella di [identità](xref:security/authentication/identity) database. Il `Status` campo determina se un contatto visibile dagli utenti generali.

Creare una nuova migrazione e aggiornare il database:

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a>Aggiungere altri servizi di identità

Accodare [Aggiungi ruoli](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) per aggiungere servizi ruolo:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a>Richiedere agli utenti autenticati

Impostare i criteri di autenticazione predefinito per richiedere agli utenti di essere autenticato:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 È possibile rifiutare esplicitamente l'autenticazione a livello di metodo di pagina Razor, controller o azione con il `[AllowAnonymous]` attributo. L'impostazione di criteri di autenticazione predefinito per richiedere agli utenti di essere autenticati protegge appena aggiunto Razor Pages e controller. La presenza di autenticazione richiesto per impostazione predefinita è più sicuro rispetto al semplice uso nuovi controller e le pagine Razor per includere il `[Authorize]` attributo.

Aggiungere [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) all'indice, le pagine di informazioni e contatti in modo che gli utenti anonimi possono ottenere informazioni sul sito per la registrazione.

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a>Configurare l'account di test

Il `SeedData` classe crea due account: amministratore e manager. Usare la [strumento Secret Manager](xref:security/app-secrets) per impostare una password per questi account. Impostare la password dalla directory del progetto (la directory contenente *Program.cs*):

```console
dotnet user-secrets set SeedUserPW <PW>
```

Se non è specificata una password complessa, viene generata un'eccezione quando `SeedData.Initialize` viene chiamato.

Aggiornamento `Main` di usare la password di test:

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Creare gli account di test e aggiornare i contatti

Aggiorna il `Initialize` metodo nel `SeedData` classe per creare gli account di test:

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

Aggiungere l'ID di utente amministratore e `ContactStatus` ai contatti. Rendere uno dei contatti relativi al "Submitted" e "Rejected" uno. Aggiungere l'ID utente e lo stato per tutti i contatti. Viene visualizzato un solo contatto:

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Creare proprietario, manager e i gestori di autorizzazione di amministratore

Creare un `ContactIsOwnerAuthorizationHandler` classe la *autorizzazione* cartella. Il `ContactIsOwnerAuthorizationHandler` verifica che l'utente che agisce su una risorsa proprietario della risorsa.

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

Il `ContactIsOwnerAuthorizationHandler` chiamate [contesto. Esito positivo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) se l'utente autenticato corrente è il proprietario del contatto. I gestori di autorizzazione a livello generale:

* Restituire `context.Succeed` quando vengono soddisfatti i requisiti.
* Restituire `Task.CompletedTask` quando non vengono soddisfatti i requisiti. `Task.CompletedTask` né esito positivo o negativo&mdash;consente altri gestori di autorizzazione per l'esecuzione.

Se è necessario eseguire in modo esplicito, restituire [contesto. Esito negativo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

L'app consente ai proprietari di contatto di modifica/eliminazione/creare i propri dati. `ContactIsOwnerAuthorizationHandler` non è necessario verificare l'operazione passato nel parametro del requisito.

### <a name="create-a-manager-authorization-handler"></a>Creare un gestore di autorizzazione di gestione

Creare un `ContactManagerAuthorizationHandler` classe la *autorizzazione* cartella. Il `ContactManagerAuthorizationHandler` verifica se l'utente che agiscono sulla risorsa è un manager. Solo i manager possono approvare o rifiutare le modifiche di contenuto (nuove o modificate).

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Creare un gestore di autorizzazione di amministratore

Creare un `ContactAdministratorsAuthorizationHandler` classe la *autorizzazione* cartella. Il `ContactAdministratorsAuthorizationHandler` verifica se l'utente che agiscono sulla risorsa è un amministratore. Amministratore può eseguire tutte le operazioni.

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Registrare i gestori di eventi di autorizzazione

Servizi usando Entity Framework Core devono essere registrati per [inserimento delle dipendenze](xref:fundamentals/dependency-injection) utilizzando [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). Il `ContactIsOwnerAuthorizationHandler` Usa ASP.NET Core [identità](xref:security/authentication/identity), che si basa su Entity Framework Core. Registrare i gestori con la raccolta di servizio in modo che siano disponibili per il `ContactsController` attraverso [inserimento delle dipendenze](xref:fundamentals/dependency-injection). Aggiungere il codice seguente alla fine della `ConfigureServices`:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

`ContactAdministratorsAuthorizationHandler` e `ContactManagerAuthorizationHandler` vengono aggiunti come singleton. Sono singleton con stato perché non usano Entity Framework e tutte le informazioni necessarie nel `Context` parametro del `HandleRequirementAsync` (metodo).

## <a name="support-authorization"></a>Supporto di autorizzazione

In questa sezione aggiornare le pagine Razor e aggiungere una classe di requisiti di operazioni.

### <a name="review-the-contact-operations-requirements-class"></a>Esaminare la classe di requisiti di operazioni contatto

Esaminare il `ContactOperations` classe. Questa classe contiene i requisiti supportato dall'app:

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a>Creare una classe di base per le pagine Razor di contatti

Creare una classe base che contiene i servizi usati in Razor Pages i contatti. La classe di base inserisce il codice di inizializzazione in un'unica posizione:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

Il codice precedente:

* Aggiunge il `IAuthorizationService` servizio per accedere ai gestori di autorizzazione.
* Aggiunge le identità `UserManager` servizio.
* Aggiungere l'oggetto `ApplicationDbContext`.

### <a name="update-the-createmodel"></a>Aggiornare il CreateModel

Aggiornare il costruttore di modello della pagina create per usare il `DI_BasePageModel` classe di base:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

Aggiornamento di `CreateModel.OnPostAsync` metodo:

* Aggiungere l'ID utente per il `Contact` modello.
* Chiamare il gestore di autorizzazione per verificare che l'utente dispone dell'autorizzazione per creare contatti.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Aggiornare il IndexModel

Aggiornamento di `OnGetAsync` metodo in modo che solo i contatti approvati vengono visualizzati agli utenti generici:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Aggiornare il EditModel

Aggiungere un gestore di autorizzazione per verificare che l'utente è proprietario del contatto. Poiché viene convalidata l'autorizzazione alle risorse, il `[Authorize]` attributo non è sufficiente. L'app non ha accesso alla risorsa quando vengono valutati gli attributi. Autorizzazione basata sulle risorse deve essere imperativa. Controlli devono essere eseguiti dopo che l'app potrà accedere alla risorsa, è possibile caricarli nel modello di pagina oppure caricarlo all'interno del gestore stesso. Si accede di frequente della risorsa passando la chiave di risorsa.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Aggiornare il DeleteModel

Aggiornare il modello di pagina delete per utilizzare il gestore dell'autorizzazione per verificare che l'utente disponga dell'autorizzazione di eliminazione per il contatto.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Inserire il servizio di autorizzazione in visualizzazioni

Attualmente, l'interfaccia utente Mostra modifica ed elimina i collegamenti per i contatti che l'utente non è possibile modificare.

Inserire il servizio di autorizzazione nel *viewimports* file in modo che sia disponibile per tutte le viste:

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

Il markup precedente aggiunge diverse `using` istruzioni.

Aggiorna il **modifica** e **eliminare** collega in *Pages/Contacts/Index.cshtml* in modo che vengono sottoposti a rendering solo per gli utenti che dispongono delle autorizzazioni appropriate:

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> Nascondere i collegamenti da utenti che non dispone dell'autorizzazione per modificare i dati non proteggere le app. Nascondere i collegamenti rende più semplici da usare l'app mediante la visualizzazione di collegamenti solo validi. Gli utenti possono hack gli URL generati per richiamare modifica ed eliminare le operazioni sui dati che non sono proprietari. La pagina Razor o il controller deve applicare controlli di accesso per proteggere i dati.

### <a name="update-details"></a>Aggiorna i dettagli

Aggiornare la visualizzazione dei dettagli in modo che i responsabili possono approvare o rifiutare i contatti:

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

Aggiornare il modello di pagina dei dettagli:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a>Aggiungere o rimuovere un utente a un ruolo

Visualizzare [questo problema](https://github.com/aspnet/Docs/issues/8502) per informazioni su:

* Rimozione dei privilegi da un utente. La disattivazione dell'audio, ad esempio un utente in un'app di chat.
* Aggiunta di privilegi a un utente.

## <a name="test-the-completed-app"></a>Testare l'app completata

Se l'app ha contatti:

* Eliminare tutti i record di `Contact` tabella.
* Riavviare l'app per l'inizializzazione del database.

Registrazione di un utente per l'esplorazione di contatti.

Un modo semplice per testare l'app completata consiste nell'avviare tre diversi browser (o in incognito o InPrivate versioni). In un browser, registrare un nuovo utente (ad esempio, `test@example.com`). Accedere a ogni browser con un altro utente. Verificare che le operazioni seguenti:

* Gli utenti registrati possono visualizzare tutti i dati di contatto approvati.
* Gli utenti registrati possono modificare/eliminare i propri dati.
* I responsabili possono approvare o rifiutare i dati di contatto. Il `Details` viene mostrata **Approva** e **rifiutare** pulsanti.
* Gli amministratori possono approvare o rifiutare e modificare/eliminare tutti i dati.

| Utente| Opzioni |
| ------------ | ---------|
| test@example.com | Può modificare/eliminare i dati personalizzati |
| manager@contoso.com | Può approvare o rifiutare e modificare/eliminare proprietario dei dati |
| admin@contoso.com | Può modificare/eliminare e approvare o rifiutare tutti i dati|

Creare un contatto nel browser dell'amministratore. Copiare l'URL per l'eliminazione e modifica di contattare l'amministratore. Incollare questi collegamenti nel browser dell'utente test per verificare che l'utente di test non è possibile eseguire queste operazioni.

## <a name="create-the-starter-app"></a>Creare l'app di base

* Creare un'app Razor Pages denominata "ContactManager"
   * Creare l'app con **singoli account utente**.
   * Il nome "ContactManager" in modo che lo spazio dei nomi corrispondente dello spazio dei nomi utilizzato nell'esempio.
   * `-uld` Specifica di Local DB invece di SQLite

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* Aggiungere *Models\Contact.cs*:

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* Scaffolding di `Contact` modello.
* Creare la migrazione iniziale e l'aggiornamento del database:

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

* Aggiorna il **ContactManager** ancoraggio nel *cshtml* file:

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* Testare l'app per la creazione, modifica ed eliminazione di un contatto

### <a name="seed-the-database"></a>Specificare il valore di inizializzazione del database

Aggiungere il [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) classe per il *dati* cartella.

Chiamare `SeedData.Initialize` da `Main`:

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

Verificare che l'app effettuato il seeding del database. Se sono presenti tutte le righe nel DB del contatto, il metodo di inizializzazione non viene eseguito.

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>Risorse aggiuntive

* [ASP.NET Core autorizzazione Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop). Questa esercitazione descrive in dettaglio più le funzionalità di sicurezza introdotte in questa esercitazione.
* [Autorizzazione in ASP.NET Core: semplice, ruolo, basata sulle attestazioni e personalizzato](xref:security/authorization/index)
* [Autorizzazione personalizzata basata su criteri](xref:security/authorization/policies)

::: moniker-end
