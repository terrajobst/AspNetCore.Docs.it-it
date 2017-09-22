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
ms.openlocfilehash: 54a737f140a8434035e5fc5abfefa458fdb69321
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2017
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Crea un'applicazione ASP.NET di base con i dati dell'utente protetti dall'autorizzazione

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)

In questa esercitazione viene illustrato come creare un'app web con i dati utente protetti dall'autorizzazione. Viene visualizzato un elenco di contatti che autenticati (registrato) creato. Sono disponibili tre gruppi di sicurezza:

* Gli utenti registrati possono visualizzare tutti i dati di contatto approvati.
* Gli utenti registrati possono modificare/eliminare i propri dati. 
* Responsabili possano approvare o rifiutare i dati di contatto. Solo i contatti approvati sono visibili agli utenti.
* Gli amministratori possono approvare/rifiutare e tutti i dati di modifica/eliminazione.

Nella figura seguente, l'utente Rick (`rick@example.com`) ha effettuato l'accesso. Utente Rick può solo visualizzazione approvato contatti e il suo contatti modifica/eliminazione. Solo l'ultimo record, creato da Rick, Visualizza modifica ed eliminare i collegamenti

![immagine descritto in precedenza](secure-data/_static/rick.png)

Nella figura seguente, `manager@contoso.com` l'accesso e il ruolo di Manager. 

![immagine descritto in precedenza](secure-data/_static/manager1.png)

L'immagine seguente mostra i gestori di visualizzazione dei dettagli di un contatto.

![immagine descritto in precedenza](secure-data/_static/manager.png)

Solo i responsabili e gli amministratori hanno l'approvazione e rifiutare i pulsanti.

Nella figura seguente, `admin@contoso.com` è firmato in e il ruolo dell'amministratore. 

![immagine descritto in precedenza](secure-data/_static/admin.png)

L'amministratore ha tutti i privilegi. È possibile lettura/modifica/eliminazione di un contatto e modificare lo stato dei contatti.

L'app è stato creato da [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) seguenti `Contact` modello:

[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

Oggetto `ContactIsOwnerAuthorizationHandler` gestore autorizzazione assicura che un utente può modificare solo i dati. Oggetto `ContactManagerAuthorizationHandler` gestore autorizzazioni consente ai responsabili di approvare o rifiutare i contatti.  Oggetto `ContactAdministratorsAuthorizationHandler` gestore di autorizzazione consente agli amministratori di approvare o rifiutare i contatti e modifica/eliminazione di contatti. 

## <a name="prerequisites"></a>Prerequisiti

Non si tratta di un'esercitazione di inizio. È necessario avere familiarità con:

* [Componenti di base di ASP.NET MVC](xref:tutorials/first-mvc-app/start-mvc)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>L'avvio e l'app completata

[Scaricare](xref:tutorials/index#how-to-download-a-sample) il [completato](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) app. [Test](#test-the-completed-app) app completata in modo da acquisire familiarità con le funzionalità di sicurezza. 

### <a name="the-starter-app"></a>L'applicazione di avvio

È utile confrontare il codice con il codice di esempio completata.

[Scaricare](xref:tutorials/index#how-to-download-a-sample) il [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) app. 

Vedere [creare l'app starter](#create-the-starter-app) se si desidera crearne uno da zero.

Aggiornare il database:

```none
   dotnet ef database update
```

Eseguire l'app, toccare il **ContactManager** collegare e verificare è possibile creare, modificare ed eliminare un contatto.

Questa esercitazione sono tutti i passaggi principali per creare l'app di dati utente. Può risultare utile per fare riferimento al progetto completato.

## <a name="modify-the-app-to-secure-user-data"></a>Modificare l'app per proteggere i dati utente

Nelle sezioni seguenti sono tutti i passaggi principali per creare l'app di dati utente. Può risultare utile per fare riferimento al progetto completato.

### <a name="tie-the-contact-data-to-the-user"></a>Associare i dati di contatto per l'utente

Utilizzare ASP.NET [identità](xref:security/authentication/identity) ID utente per accertarsi che gli utenti possono modificare i propri dati, ma non altri dati di utenti. Aggiungere `OwnerID` per il `Contact` modello:

[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

`OwnerID`è l'ID dell'utente dal `AspNetUser` tabella il [identità](xref:security/authentication/identity) database. Il `Status` campo determina se un contatto visibile dagli utenti generale. 

Lo scaffolding di una nuova migrazione e aggiornare il database:

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a>Richiedere SSL e gli utenti autenticati

Nel `ConfigureServices` metodo il *Startup.cs* file, aggiungere il [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtro di autorizzazione:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]

Se si utilizza Visual Studio, vedere [configurare IIS Express per SSL e HTTPS](xref:security/enforcing-ssl#set-up-iis-express-for-sslhttps). Per reindirizzare le richieste HTTP a HTTPS, vedere [Middleware di riscrittura URL](xref:fundamentals/url-rewriting). Se si utilizza Visual Studio Code o test in locale della piattaforma che non include un certificato di prova per SSL:

- Impostare `"LocalTest:skipSSL": true` nel *appSettings. JSON* file.

### <a name="require-authenticated-users"></a>Richiedere agli utenti autenticati

Impostare i criteri di autenticazione predefinito per richiedere agli utenti di essere autenticato. È possibile rifiutare esplicitamente l'autenticazione in corrispondenza del metodo di azione o controller con la `[AllowAnonymous]` attributo. Con questo approccio, qualsiasi controller di nuovo aggiunto automaticamente richiederà l'autenticazione, che è più sicuro di basarsi sui nuovi controller per includere il `[Authorize]` attributo. Il comando seguente per aggiungere il `ConfigureServices` metodo il *Startup.cs* file:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]

Aggiungere `[AllowAnonymous]` al controller home in modo gli utenti anonimi possono ottenere informazioni sul sito per la registrazione.

[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]

### <a name="configure-the-test-account"></a>Configurare l'account di test

La `SeedData` classe crea due account, amministratore e gestione. Utilizzare il [lo strumento Gestione segreto](xref:security/app-secrets) per impostare una password per questi account. Eseguire questa operazione dalla directory del progetto (la directory contenente *Program.cs*).

```console
dotnet user-secrets set SeedUserPW <PW>
```

Aggiornamento `Configure` per utilizzare la password di test:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]

Aggiungere l'ID utente di amministratore e `Status = ContactStatus.Approved` ai contatti. Viene visualizzato un solo contatto, aggiungere l'ID utente per tutti i contatti:

[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Creare i gestori di autorizzazione di amministratore, manager e proprietario

Creare un `ContactIsOwnerAuthorizationHandler` classe il *autorizzazione* cartella. Il `ContactIsOwnerAuthorizationHandler` verificherà che opera la risorsa utente proprietario della risorsa.

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

Il `ContactIsOwnerAuthorizationHandler` chiamate `context.Succeed` se l'utente autenticato corrente è il proprietario del contatto. I gestori di autorizzazione restituiscono in genere `context.Succeed` quando vengono soddisfatti i requisiti. Restituiscono `Task.FromResult(0)` quando non vengono soddisfatti i requisiti. `Task.FromResult(0)`è né esito positivo o negativo, consente un altro gestore eventi di autorizzazione per l'esecuzione. Se è necessario eseguire in modo esplicito, restituire `context.Fail()`.

È consentire ai proprietari di contatto di modifica o Elimina i propri dati, in modo che non è necessario verificare l'operazione passato nel parametro requisito.

### <a name="create-a-manager-authorization-handler"></a>Creare un gestore di autorizzazione di gestione

Creare un `ContactManagerAuthorizationHandler` classe il *autorizzazione* cartella. Il `ContactManagerAuthorizationHandler` verificherà l'utente che opera la risorsa è un responsabile. Solo i manager possono approvare o rifiutare le modifiche di contenuto (nuove o modificate).

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Creare un gestore autorizzazioni di amministratore

Creare un `ContactAdministratorsAuthorizationHandler` classe il *autorizzazione* cartella. Il `ContactAdministratorsAuthorizationHandler` verificherà l'utente che opera la risorsa è un amministratore. Amministratore può eseguire tutte le operazioni.

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Registrare i gestori di eventi di autorizzazione

Deve essere registrata con Entity Framework Core Services [inserimento di dipendenze](xref:fundamentals/dependency-injection) utilizzando [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). Il `ContactIsOwnerAuthorizationHandler` utilizza ASP.NET Core [identità](xref:security/authentication/identity), che si basa su Entity Framework Core. Registrare i gestori con la raccolta di servizio in modo saranno disponibili per il `ContactsController` tramite [inserimento di dipendenze](xref:fundamentals/dependency-injection). Aggiungere il codice seguente alla fine di `ConfigureServices`:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]

`ContactAdministratorsAuthorizationHandler`e `ContactManagerAuthorizationHandler` vengono aggiunti come singleton. Perché non usano Entity Framework e tutte le informazioni necessarie sono singleton di `Context` parametro del `HandleRequirementAsync` (metodo).

L'intero `ConfigureServices`:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]

## <a name="update-the-code-to-support-authorization"></a>Aggiornare il codice per supportare l'autorizzazione

In questa sezione aggiornare il controller e visualizzazioni e aggiungere una classe di requisiti di operazioni.

### <a name="update-the-contacts-controller"></a>Aggiornare il controller di contatti

Aggiornamento di `ContactsController` costruttore:

* Aggiungere il `IAuthorizationService` accesso ai gestori di autorizzazione del servizio. 
* Aggiungere il `Identity` `UserManager` servizio:

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]

### <a name="add-a-contact-operations-requirements-class"></a>Aggiungere una classe di requisiti di operazioni di contatto

Aggiungere il `ContactOperationsRequirements` classe per il *autorizzazione* cartella. I requisiti di contenere questa classe supporta l'app:

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

### <a name="update-create"></a>Creazione di aggiornamento

Aggiornamento di `HTTP POST Create` metodo:

* Aggiungere l'ID utente di `Contact` modello.
* Chiamare il gestore dell'autorizzazione per verificare che l'utente proprietario del contatto.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]

### <a name="update-edit"></a>Modifica di aggiornamento

Aggiornare i valori `Edit` metodi da utilizzare il gestore dell'autorizzazione per verificare che l'utente appartiene il contatto. Poiché stiamo eseguendo l'autorizzazione per le risorse non è possibile usare il `[Authorize]` attributo. Quando vengono valutati gli attributi non sono l'accesso alla risorsa. Autorizzazione per le risorse in base deve essere imperativo. Controlli devono essere eseguiti una volta ottenuto l'accesso alla risorsa, caricandolo in questo controller o caricandola all'interno del gestore stesso. Spesso si accede alla risorsa passando la chiave della risorsa.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]

### <a name="update-the-delete-method"></a>Aggiornare il metodo Delete

Aggiornare i valori `Delete` metodi da utilizzare il gestore dell'autorizzazione per verificare che l'utente appartiene il contatto.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]

## <a name="inject-the-authorization-service-into-the-views"></a>Inserire il servizio di autorizzazione nelle viste

Attualmente la Mostra interfaccia utente modifica ed elimina i collegamenti per l'utente non è possibile modificare i dati. Che verrà risolto applicando il gestore dell'autorizzazione per le viste.

Inserire il servizio di autorizzazione nel *Views/_ViewImports.cshtml* file in modo che sia disponibile per tutte le viste:

[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]

Aggiornamento di *Views/Contacts/Index.cshtml* visualizzazione Razor solo visualizzare la modifica ed eliminare i collegamenti per gli utenti possono modificare/eliminare il contatto.

Aggiungere`@using ContactManager.Authorization;`

Aggiornamento di `Edit` e `Delete` collega in modo da essere sottoposti a rendering solo per gli utenti con autorizzazione modificare ed eliminare il contatto.

[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]

Avviso: Nascondere i collegamenti da utenti che non dispone dell'autorizzazione per modificare o eliminare i dati non è possibile proteggere l'applicazione. Nascondere i collegamenti rende l'app utente più descrittivo visualizzando i collegamenti sono validi. Gli utenti possono hack gli URL generati per richiamare modifica ed eliminazione di operazioni sui dati che non possiedono.  Il controller è necessario ripetere che controlla l'accesso per essere protetti.

### <a name="update-the-details-view"></a>Aggiornare la visualizzazione dei dettagli

Aggiornare la visualizzazione dei dettagli in modo che i responsabili possano approvare o rifiutare contatti:

[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]

## <a name="test-the-completed-app"></a>Testare l'app completata

Se si utilizza Visual Studio Code o test in locale della piattaforma che non include un certificato di prova per SSL:

- Impostare `"LocalTest:skipSSL": true` nel *appSettings. JSON* file.

Se si hanno eseguito l'app e contatti, eliminare tutti i record di `Contact` tabella e riavviare l'applicazione per l'inizializzazione del database. Se si utilizza Visual Studio, è necessario chiudere e riavviare IIS Express al valore di inizializzazione del database.

Registrazione di un utente per cercare i contatti.

Un modo semplice per testare app completata consiste nell'avviare tre diversi browser (o incognito/InPrivate versioni). In un browser, registrare un nuovo utente, ad esempio, `test@example.com`. Accedere a ogni browser con un altro utente. Verificare quanto segue:

* Gli utenti registrati possono visualizzare tutti i dati di contatto approvati.
* Gli utenti registrati possono modificare/eliminare i propri dati. 
* Responsabili possano approvare o rifiutare i dati di contatto. Il `Details` vista mostra **Approva** e **rifiutare** pulsanti. 
* Gli amministratori possono approvare/rifiutare e tutti i dati di modifica/eliminazione.

| Utente| Opzioni |
| ------------ | ---------|
| test@example.com | Può modificare/eliminare dati |
| manager@contoso.com | Può approvare/rifiutare e modifica/eliminazione disporre di dati  |
| admin@contoso.com | Possibile modifica/eliminazione e approvare/rifiutare tutti i dati|

Creare un contatto nel browser gli amministratori. Copiare l'URL per l'eliminazione e modificare il contatto dell'amministratore. Incollare questi collegamenti nel browser dell'utente test per verificare che l'utente test non è possibile eseguire queste operazioni.

## <a name="create-the-starter-app"></a>Creare l'app starter

Seguire queste istruzioni per creare l'applicazione di avvio.

* Creare un **applicazione Web di ASP.NET Core** utilizzando [Visual Studio 2017](https://www.visualstudio.com/) denominato "ContactManager"

  * Creare l'app con **singoli account utente**.
  * Il nome "ContactManager" in modo l'utilizzo dello spazio dei nomi nell'esempio corrisponderà a uno spazio dei nomi.

* Aggiungere il seguente `Contact` modello:

  [!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

* Lo scaffolding di `Contact` del modello tramite Entity Framework Core e `ApplicationDbContext` contesto dati. Accettare tutte le impostazioni predefinite di scaffolding. Utilizzando `ApplicationDbContext` per il contesto dati classe inserisce nella tabella contatto nel [identità](xref:security/authentication/identity) database. Vedere [aggiunta di un modello](xref:tutorials/first-mvc-app/adding-model) per ulteriori informazioni.

* Aggiornamento di **ContactManager** di ancoraggio nel *Views/Shared/_Layout.cshtml* file dalla `asp-controller="Home"` a `asp-controller="Contacts"` quindi toccando il **ContactManager** collegamento richiama il controller di contatti. Il tag originale:

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

Il markup aggiornato:

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* Lo scaffolding della migrazione iniziale e aggiornare il database

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* Testare l'app per la creazione, modifica ed eliminazione di un contatto

### <a name="seed-the-database"></a>Specificare il valore di inizializzazione del database

Aggiungere il `SeedData` classe per il *dati* cartella. Se è stato scaricato l'esempio, è possibile copiare il *SeedData.cs* file per il *dati* cartella del progetto di avvio.

[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]

Aggiungere il codice evidenziato alla fine del `Configure` metodo il *Startup.cs* file:

[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]

Verificare che l'app effettuato il seeding del database. Il metodo di inizializzazione non viene eseguito se sono presenti righe nel database di contatto.

### <a name="create-a-class-used-in-the-tutorial"></a>Creare una classe utilizzata nell'esercitazione

* Creare una cartella denominata *autorizzazione*.
* Copia il *Authorization\ContactOperations.cs* file da scaricare il progetto completato o copiare il codice seguente:

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

<a name=secure-data-add-resources-label></a>

### <a name="additional-resources"></a>Risorse aggiuntive

* [Lab autorizzazione ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop). Questa esercitazione vengono descritti in dettaglio le funzionalità di sicurezza introdotte in questa esercitazione.
* [Autorizzazione in ASP.NET Core: semplice, ruolo, basata sulle attestazioni e personalizzato](index.md)
* [Autorizzazione personalizzata basata su criteri](policies.md)
