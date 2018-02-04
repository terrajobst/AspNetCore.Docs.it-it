---
title: Crea un'applicazione ASP.NET di base con i dati dell'utente protetti dall'autorizzazione
author: rick-anderson
description: Informazioni su come creare un'app di pagine Razor con dati dell'utente protetti dall'autorizzazione. Include SSL, l'autenticazione, protezione, ASP.NET Identity Core.
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/secure-data
ms.openlocfilehash: 6333082a2b2b4f6d3f1ce2afc600b4203a0f5dca
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Crea un'applicazione ASP.NET di base con i dati dell'utente protetti dall'autorizzazione

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)

In questa esercitazione viene illustrato come creare un'app web ASP.NET di base con i dati utente protetti dall'autorizzazione. Viene visualizzato un elenco di contatti che autenticati (registrato) creato. Sono disponibili tre gruppi di sicurezza:

* **Gli utenti registrati** può visualizzare tutti i dati approvati e possono modificare/eliminare i propri dati.
* **Gestori** possono approvare o rifiutare i dati di contatto. Solo i contatti approvati sono visibili agli utenti.
* **Gli amministratori** può approvare/rifiutare e tutti i dati di modifica/eliminazione.

Nella figura seguente, l'utente Rick (`rick@example.com`) ha effettuato l'accesso. Rick possono visualizzare solo i contatti approvati e **modifica**/**eliminare**/**Crea nuovo** collegamenti per il suo contatti. Solo l'ultimo record, creato da Rick, Visualizza **modifica** e **eliminare** collegamenti. Altri utenti vedere dell'ultimo record sarà responsabile o un amministratore modifica lo stato su "Approvata".

![immagine descritta precedente](secure-data/_static/rick.png)

Nella figura seguente, `manager@contoso.com` l'accesso e il ruolo di Manager:

![immagine descritta precedente](secure-data/_static/manager1.png)

L'immagine seguente mostra i gestori di visualizzazione dei dettagli di un contatto:

![immagine descritta precedente](secure-data/_static/manager.png)

Il **Approva** e **rifiutare** pulsanti vengono visualizzati solo per i gestori e amministratori.

Nella figura seguente, `admin@contoso.com` l'accesso e al ruolo administrators:

![immagine descritta precedente](secure-data/_static/admin.png)

L'amministratore ha tutti i privilegi. È possibile lettura/modifica/eliminazione di un contatto e modificare lo stato dei contatti.

L'app è stato creato da [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) seguenti `Contact` modello:

[!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

L'esempio include i gestori di autorizzazione seguente:

* `ContactIsOwnerAuthorizationHandler`: Assicura che un utente può modificare solo i dati.
* `ContactManagerAuthorizationHandler`: Consente ai responsabili di approvare o rifiutare i contatti.
* `ContactAdministratorsAuthorizationHandler`: Consente agli amministratori di approvare o rifiutare i contatti e modifica/eliminazione di contatti.

## <a name="prerequisites"></a>Prerequisiti

In questa esercitazione viene anticipata. È necessario avere familiarità con:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Autenticazione](xref:security/authentication/index)
* [Conferma account e recupero password](xref:security/authentication/accconfirm)
* [Autorizzazione](xref:security/authorization/index)
* [Entity Framework Core](xref:data/ef-mvc/intro)

Vedere [questo file PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) per la versione di ASP.NET MVC di base. La versione di ASP.NET 1.1 componenti di base di questa esercitazione è nel [questo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) cartella. 1.1 esempio ASP.NET Core è il [esempi](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).

## <a name="the-starter-and-completed-app"></a>L'avvio e l'app completata

[Scaricare](xref:tutorials/index#how-to-download-a-sample) il [completato](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app. [Test](#test-the-completed-app) app completata in modo da acquisire familiarità con le funzionalità di sicurezza.

### <a name="the-starter-app"></a>L'applicazione di avvio

[Scaricare](xref:tutorials/index#how-to-download-a-sample) il [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.

Eseguire l'app, toccare il **ContactManager** collegare e verificare è possibile creare, modificare ed eliminare un contatto.

## <a name="secure-user-data"></a>Proteggere i dati utente

Nelle sezioni seguenti sono tutti i passaggi principali per creare l'app di dati utente. Può risultare utile per fare riferimento al progetto completato.

### <a name="tie-the-contact-data-to-the-user"></a>Associare i dati di contatto per l'utente

Utilizzare ASP.NET [identità](xref:security/authentication/identity) ID utente per accertarsi che gli utenti possono modificare i propri dati, ma non altri dati di utenti. Aggiungere `OwnerID` e `ContactStatus` per il `Contact` modello:

[!code-csharp[Main](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID`è l'ID dell'utente dal `AspNetUser` tabella il [identità](xref:security/authentication/identity) database. Il `Status` campo determina se un contatto visibile dagli utenti generale.

Crea una nuova migrazione e aggiornare il database:

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-ssl-and-authenticated-users"></a>Richiedere SSL e gli utenti autenticati

Aggiungere [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) a `Startup`:

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_env)]

Nel `ConfigureServices` metodo il *Startup.cs* file, aggiungere il [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtro di autorizzazione:

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=19-999)]

Se si utilizza Visual Studio, abilitare SSL.

Per reindirizzare le richieste HTTP a HTTPS, vedere [Middleware di riscrittura URL](xref:fundamentals/url-rewriting). Se si sta utilizzando Visual Studio Code o test in una piattaforma locale che non include un certificato di prova per SSL:

  Impostare `"LocalTest:skipSSL": true` nel *appsettings. Developement.JSON* file.

### <a name="require-authenticated-users"></a>Richiedere agli utenti autenticati

Impostare i criteri di autenticazione predefinito per richiedere agli utenti di essere autenticato. È possibile rifiutare esplicitamente l'autenticazione a livello di metodo di azione, controller o pagina Razor con il `[AllowAnonymous]` attributo. L'impostazione del criterio di autenticazione predefinito per richiedere agli utenti di essere autenticato protegge appena aggiunto pagine Razor e controller. La presenza di autenticazione richiesta per impostazione predefinita è più sicuro di basarsi su nuovi controller e pagine Razor per includere il `[Authorize]` attributo. Il comando seguente per aggiungere il `ConfigureServices` metodo il *Startup.cs* file:

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=31-999)]

Aggiungere [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) all'indice, contattare e sulle pagine in modo gli utenti anonimi possono ottenere informazioni sul sito per la registrazione. 

[!code-csharp[Main](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

Aggiungere `[AllowAnonymous]` per il [LoginModel e RegisterModel](https://github.com/aspnet/templating/issues/238).

### <a name="configure-the-test-account"></a>Configurare l'account di test

La `SeedData` classe crea due account: amministratore e gestione. Utilizzare il [lo strumento Gestione segreto](xref:security/app-secrets) per impostare una password per questi account. Impostare la password della directory del progetto (la directory contenente *Program.cs*):

```console
dotnet user-secrets set SeedUserPW <PW>
```

Aggiornamento `Main` per utilizzare la password di test:

[!code-csharp[Main](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Creare gli account di test e aggiornare i contatti

Aggiornamento di `Initialize` metodo la `SeedData` classe per creare gli account di test:

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

Aggiungere l'ID utente di amministratore e `ContactStatus` ai contatti. Effettuare uno dei contatti "Inviato" e "Rejected" uno. Aggiungere l'ID utente e lo stato di tutti i contatti. Viene visualizzato un solo contatto:

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Creare i gestori di autorizzazione di amministratore, manager e proprietario

Creare un `ContactIsOwnerAuthorizationHandler` classe il *autorizzazione* cartella. Il `ContactIsOwnerAuthorizationHandler` verifica che l'utente che agisce su una risorsa proprietario della risorsa.

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

Il `ContactIsOwnerAuthorizationHandler` chiamate [contesto. Esito positivo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) se l'utente autenticato corrente è il proprietario del contatto. I gestori di autorizzazione in genere:

* Restituire `context.Succeed` quando vengono soddisfatti i requisiti.
* Restituire `Task.CompletedTask` quando non sono soddisfatti i requisiti. `Task.CompletedTask`è né esito positivo o negativo&mdash;consente altri gestori di autorizzazione per l'esecuzione.

Se è necessario eseguire in modo esplicito, restituire [contesto. Esito negativo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

L'app consente ai proprietari di contatto di modifica, eliminare o creare i propri dati. `ContactIsOwnerAuthorizationHandler`non è necessario verificare l'operazione passato nel parametro requisito.

### <a name="create-a-manager-authorization-handler"></a>Creare un gestore di autorizzazione di gestione

Creare un `ContactManagerAuthorizationHandler` classe il *autorizzazione* cartella. Il `ContactManagerAuthorizationHandler` verifica l'utente che opera la risorsa è un responsabile. Solo i manager possono approvare o rifiutare le modifiche di contenuto (nuove o modificate).

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Creare un gestore autorizzazioni di amministratore

Creare un `ContactAdministratorsAuthorizationHandler` classe il *autorizzazione* cartella. Il `ContactAdministratorsAuthorizationHandler` verifica l'utente che opera la risorsa è un amministratore. Amministratore può eseguire tutte le operazioni.

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Registrare i gestori di eventi di autorizzazione

Deve essere registrata con Entity Framework Core Services [inserimento di dipendenze](xref:fundamentals/dependency-injection) utilizzando [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). Il `ContactIsOwnerAuthorizationHandler` utilizza ASP.NET Core [identità](xref:security/authentication/identity), che si basa su Entity Framework Core. Registrare i gestori con la raccolta di servizio in modo che siano disponibili per il `ContactsController` tramite [inserimento di dipendenze](xref:fundamentals/dependency-injection). Aggiungere il codice seguente alla fine di `ConfigureServices`:

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

`ContactAdministratorsAuthorizationHandler`e `ContactManagerAuthorizationHandler` vengono aggiunti come singleton. Perché non usano Entity Framework e tutte le informazioni necessarie, ma sono singleton di `Context` parametro del `HandleRequirementAsync` (metodo).

## <a name="support-authorization"></a>Autorizzazione di supporto

In questa sezione aggiornare le pagine Razor e aggiungere una classe di requisiti di operazioni.

### <a name="review-the-contact-operations-requirements-class"></a>Esaminare la classe di requisiti di operazioni di contatto

Esaminare la `ContactOperations` classe. Questa classe contiene i requisiti di supporta l'app:

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a>Creare una classe base per le pagine Razor

Creare una classe di base che contiene i servizi usati nei contatti pagine Razor. La classe di base inserisce il codice di inizializzazione in un'unica posizione:

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

Il codice precedente:

* Aggiunge il `IAuthorizationService` accesso ai gestori di autorizzazione del servizio.
* Aggiunge l'identità `UserManager` servizio.
* Aggiungere l'oggetto `ApplicationDbContext`.

### <a name="update-the-createmodel"></a>Aggiornare il CreateModel

Aggiornare il costruttore di modello della pagina Crea per utilizzare il `DI_BasePageModel` classe di base:

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

Aggiornamento di `CreateModel.OnPostAsync` metodo:

* Aggiungere l'ID utente di `Contact` modello.
* Chiamare il gestore dell'autorizzazione per verificare che l'utente disponga delle autorizzazioni per creare i contatti.

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Aggiornare il IndexModel

Aggiornamento di `OnGetAsync` metodo pertanto solo i contatti approvati vengono visualizzati per gli utenti:

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Aggiornare il EditModel

Aggiungere un gestore di autorizzazione per verificare che l'utente proprietario del contatto. Poiché l'autorizzazione per le risorse in fase di convalida, il `[Authorize]` attributo non è sufficiente. L'app non dispone dell'accesso alla risorsa quando vengono valutati gli attributi. Autorizzazione basata sulle risorse deve essere imperativo. Controlli devono essere eseguiti una volta che l'app ha accesso alla risorsa, è possibile caricarlo nel modello di pagina oppure caricarlo all'interno del gestore stesso. Si accede di frequente la risorsa passando la chiave della risorsa.

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Aggiornare il DeleteModel

Aggiornare il modello di pagina di eliminazione per utilizzare il gestore dell'autorizzazione per verificare che l'utente disponga dell'autorizzazione di eliminazione per il contatto.

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Inserire il servizio di autorizzazione nelle viste

Attualmente, illustrato nell'interfaccia utente modifica ed elimina i collegamenti per l'utente non è possibile modificare i dati. L'interfaccia utente è stato risolto applicando il gestore dell'autorizzazione per le viste.

Inserire il servizio di autorizzazione nel *Views/_ViewImports.cshtml* file in modo che sia disponibile per tutte le viste:

[!code-cshtml[Main](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

Il codice precedente aggiunge diverse `using` istruzioni.

Aggiornamento di **modifica** e **eliminare** di collegamenti in *Pages/Contacts/Index.cshtml* in modo vengono sottoposti a rendering solo per gli utenti con le autorizzazioni appropriate:

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> Nascondere i collegamenti da utenti che non dispone dell'autorizzazione per modificare i dati non proteggere l'applicazione. Nascondere i collegamenti rende più semplici l'app visualizzando i collegamenti sono validi. Gli utenti possono hack gli URL generati per richiamare modifica ed eliminazione di operazioni sui dati che non possiedono. Pagina Razor o del controller deve applicare controlli di accesso per proteggere i dati.

### <a name="update-details"></a>L'aggiornamento dei dettagli

Aggiornare la visualizzazione dei dettagli in modo che i responsabili possano approvare o rifiutare contatti:

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

Aggiornare il modello di pagina dei dettagli:

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a>Testare l'app completata

Se si sta utilizzando Visual Studio Code o test in una piattaforma locale che non include un certificato di prova per SSL:

* Impostare `"LocalTest:skipSSL": true` nel *appsettings. Developement.JSON* file da ignorare il requisito SSL. SSL Skip solo su un computer di sviluppo.

Se l'applicazione dispone di contatti:

* Eliminare tutti i record di `Contact` tabella.
* Riavviare l'app per l'inizializzazione del database.

Registrazione di un utente per esplorare i contatti.

Un modo semplice per testare app completata consiste nell'avviare tre diversi browser (o incognito/InPrivate versioni). In un browser, registrare un nuovo utente (ad esempio, `test@example.com`). Accedere a ogni browser con un altro utente. Verificare che le operazioni seguenti:

* Gli utenti registrati possono visualizzare tutti i dati di contatto approvati.
* Gli utenti registrati possono modificare/eliminare i propri dati.
* Responsabili possano approvare o rifiutare i dati di contatto. Il `Details` vista mostra **Approva** e **rifiutare** pulsanti.
* Gli amministratori possono approvare/rifiutare e tutti i dati di modifica/eliminazione.

| Utente| Opzioni |
| ------------ | ---------|
| test@example.com | Può modificare/eliminare dati |
| manager@contoso.com | Può approvare/rifiutare e modifica/eliminazione disporre di dati |
| admin@contoso.com | Possibile modifica/eliminazione e approvare/rifiutare tutti i dati|

Creare un contatto nel browser dell'amministratore. Copiare l'URL per l'eliminazione e modificare il contatto dell'amministratore. Incollare questi collegamenti nel browser dell'utente test per verificare che l'utente test non è possibile eseguire queste operazioni.

## <a name="create-the-starter-app"></a>Creare l'app starter

* Creare un'applicazione di pagine Razor denominata "ContactManager"

  * Creare l'app con **singoli account utente**.
  * Il nome "ContactManager" in modo lo spazio dei nomi utilizzato nell'esempio corrisponde a uno spazio dei nomi.

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

  * `-uld`Specifica del database locale anziché SQLite

* Aggiungere il seguente `Contact` modello:

  [!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* Lo scaffolding di `Contact` modello:

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* Aggiornamento di **ContactManager** di ancoraggio nel *Pages/_Layout.cshtml* file:

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* Lo scaffolding della migrazione iniziale e aggiornare il database:

```console
dotnet ef migrations add initial
dotnet ef database update
```

* Testare l'app per la creazione, modifica ed eliminazione di un contatto

### <a name="seed-the-database"></a>Specificare il valore di inizializzazione del database

Aggiungere il `SeedData` classe per il *dati* cartella. Se è stato scaricato l'esempio, è possibile copiare il *SeedData.cs* file per il *dati* cartella del progetto di avvio.

Chiamare `SeedData.Initialize` da `Main`:

[!code-csharp[Main](secure-data/samples/starter2/Program.cs?name=snippet)]

Verificare che l'app effettuato il seeding del database. Se sono presenti righe nel database di contatto, il metodo di inizializzazione non viene eseguito.

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>Risorse aggiuntive

* [Lab autorizzazione ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop). Questa esercitazione vengono descritti in dettaglio le funzionalità di sicurezza introdotte in questa esercitazione.
* [Autorizzazione in ASP.NET Core: semplice, ruolo, basata sulle attestazioni e personalizzato](xref:security/authorization/index)
* [Autorizzazione personalizzata basata su criteri](xref:security/authorization/policies)
