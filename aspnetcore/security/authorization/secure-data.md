---
title: Creare un'app ASP.NET Core con i dati utente protetti da autorizzazione
author: rick-anderson
description: Informazioni su come creare un'app Razor Pages con i dati utente protetti da autorizzazione. Include HTTPS, l'autenticazione, sicurezza, ASP.NET Core Identity.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: 65c72d4dd457f85451796c5713bedebafec7a7de
ms.sourcegitcommit: 8157e5a351f49aeef3769f7d38b787b4386aad5f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/20/2019
ms.locfileid: "74239828"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Creare un'app ASP.NET Core con i dati utente protetti da autorizzazione

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)

::: moniker range="<= aspnetcore-1.1"

Vedere [questo file PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) per la versione ASP.NET Core MVC. La versione ASP.NET Core 1,1 di questa esercitazione si trova in [questa](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) cartella. L'esempio 1,1 ASP.NET Core è presente negli [esempi](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Vedi [questo PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Questa esercitazione illustra come creare un'app web ASP.NET Core con i dati utente protetti da autorizzazione. Viene visualizzato un elenco di contatti che (registrati) agli utenti autenticati hanno creato. Sono disponibili tre gruppi di sicurezza:

* **Gli utenti registrati** possono visualizzare tutti i dati approvati e possono modificare/eliminare i propri dati.
* I **responsabili** possono approvare o rifiutare i dati di contatto. Solo i contatti approvati sono visibili agli utenti.
* Gli **amministratori** possono approvare/rifiutare e modificare/eliminare i dati.

Le immagini in questo documento non corrispondono esattamente ai modelli più recenti.

Nell'immagine seguente l'utente Rick (`rick@example.com`) ha eseguito l'accesso. Rick può visualizzare solo i contatti approvati e **modificare**/**eliminare**/**creare nuovi** collegamenti per i propri contatti. Solo l'ultimo record, creato da Rick, Visualizza i collegamenti **modifica** ed **Elimina** . Gli altri utenti non vedono l'ultimo record fino a quando un responsabile o l'amministratore assume lo stato "Approvato".

![Screenshot che mostra Rick effettuato l'accesso](secure-data/_static/rick.png)

Nella figura seguente `manager@contoso.com` è stato eseguito l'accesso e nel ruolo del Manager:

![Screenshot che mostra l'accesso manager@contoso.com](secure-data/_static/manager1.png)

L'immagine seguente mostra i gestori visualizzazione dettagli di un contatto:

![Visualizzazione di gestione di un contatto](secure-data/_static/manager.png)

I pulsanti **approva** e **rifiuta** vengono visualizzati solo per Manager e amministratori.

Nella figura seguente `admin@contoso.com` è stato eseguito l'accesso e il ruolo dell'amministratore:

![Screenshot che mostra l'accesso admin@contoso.com](secure-data/_static/admin.png)

L'amministratore ha tutti i privilegi. Può leggere, modificare ed eliminare un contatto e modificare lo stato dei contatti.

L'app è stata creata mediante l' [impalcatura](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) del modello di `Contact` seguente:

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

L'esempio contiene i gestori di autorizzazione seguenti:

* `ContactIsOwnerAuthorizationHandler`: assicura che un utente possa modificare solo i propri dati.
* `ContactManagerAuthorizationHandler`: consente ai responsabili di approvare o rifiutare i contatti.
* `ContactAdministratorsAuthorizationHandler`: consente agli amministratori di approvare o rifiutare i contatti e di modificare/eliminare contatti.

## <a name="prerequisites"></a>Prerequisiti

Questa esercitazione viene fatto avanzare. È necessario avere familiarità con:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Autenticazione](xref:security/authentication/identity)
* [Conferma dell'account e recupero della password](xref:security/authentication/accconfirm)
* [Autorizzazione](xref:security/authorization/introduction)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>Starter e app completata

[Scaricare](xref:index#how-to-download-a-sample) l'app [completata](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) . [Testare](#test-the-completed-app) l'app completata in modo da acquisire familiarità con le funzionalità di sicurezza.

### <a name="the-starter-app"></a>L'app di base

[Scaricare](xref:index#how-to-download-a-sample) l'app [Starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) .

Eseguire l'app, toccare il collegamento **ContactManager** e verificare che sia possibile creare, modificare ed eliminare un contatto.

## <a name="secure-user-data"></a>Proteggere i dati utente

Le sezioni seguenti includono tutti i passaggi principali per creare l'app per dati sicura degli utenti. Si può risultare utile per fare riferimento al progetto completato.

### <a name="tie-the-contact-data-to-the-user"></a>Associare i dati di contatto per l'utente

Usare l'ID utente dell' [identità](xref:security/authentication/identity) ASP.NET per assicurarsi che gli utenti possano modificare i dati, ma non i dati di altri utenti. Aggiungere `OwnerID` e `ContactStatus` al modello di `Contact`:

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` è l'ID dell'utente della tabella `AspNetUser` nel database di [identità](xref:security/authentication/identity) . Il campo `Status` determina se un contatto è visualizzabile dagli utenti generali.

Creare una nuova migrazione e aggiornare il database:

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a>Aggiungere altri servizi di identità

Aggiungere [Aggiungi ruoli](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) per aggiungere servizi ruolo:

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a>Richiedere agli utenti autenticati

Impostare i criteri di autenticazione predefinito per richiedere agli utenti di essere autenticato:

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 È possibile rifiutare esplicitamente l'autenticazione a livello di pagina Razor, controller o metodo di azione con l'attributo `[AllowAnonymous]`. L'impostazione di criteri di autenticazione predefinito per richiedere agli utenti di essere autenticati protegge appena aggiunto Razor Pages e controller. La necessità di eseguire l'autenticazione per impostazione predefinita è più sicura rispetto all'uso di nuovi controller e Razor Pages per includere l'attributo `[Authorize]`.

Aggiungere [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) alle pagine indici e privacy in modo che gli utenti anonimi possano ottenere informazioni sul sito prima della registrazione.

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a>Configurare l'account di test

La classe `SeedData` crea due account: Administrator e Manager. Utilizzare lo [strumento Gestione segreta](xref:security/app-secrets) per impostare una password per questi account. Impostare la password dalla directory del progetto (la directory contenente *Program.cs*):

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

Se non si specifica una password complessa, quando viene chiamato `SeedData.Initialize` viene generata un'eccezione.

Aggiornare `Main` per usare la password di prova:

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Creare gli account di test e aggiornare i contatti

Aggiornare il metodo `Initialize` nella classe `SeedData` per creare gli account di test:

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

Aggiungere l'ID utente amministratore e `ContactStatus` ai contatti. Rendere uno dei contatti relativi al "Submitted" e "Rejected" uno. Aggiungere l'ID utente e lo stato per tutti i contatti. Viene visualizzato un solo contatto:

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Creare proprietario, manager e i gestori di autorizzazione di amministratore

Creare una classe `ContactIsOwnerAuthorizationHandler` nella cartella *authorization* . Il `ContactIsOwnerAuthorizationHandler` verifica che l'utente che agisce su una risorsa appartenga alla risorsa.

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

Il `ContactIsOwnerAuthorizationHandler` chiama il [contesto. Ha esito positivo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) se l'utente autenticato corrente è il proprietario del contatto. I gestori di autorizzazione a livello generale:

* Restituisce `context.Succeed` quando vengono soddisfatti i requisiti.
* Restituisce `Task.CompletedTask` quando i requisiti non vengono soddisfatti. `Task.CompletedTask` non ha esito positivo o negativo&mdash;consente l'esecuzione di altri gestori di autorizzazione.

Se è necessario eseguire in modo esplicito l'errore, restituire [context. Esito negativo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

L'app consente ai proprietari di contatto di modifica/eliminazione/creare i propri dati. `ContactIsOwnerAuthorizationHandler` non è necessario controllare l'operazione passata nel parametro requirement.

### <a name="create-a-manager-authorization-handler"></a>Creare un gestore di autorizzazione di gestione

Creare una classe `ContactManagerAuthorizationHandler` nella cartella *authorization* . Il `ContactManagerAuthorizationHandler` verifica che l'utente che agisce sulla risorsa sia un responsabile. Solo i manager possono approvare o rifiutare le modifiche di contenuto (nuove o modificate).

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Creare un gestore di autorizzazione di amministratore

Creare una classe `ContactAdministratorsAuthorizationHandler` nella cartella *authorization* . Il `ContactAdministratorsAuthorizationHandler` verifica che l'utente che agisce sulla risorsa sia un amministratore. Amministratore può eseguire tutte le operazioni.

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Registrare i gestori di eventi di autorizzazione

I servizi che usano Entity Framework Core devono essere registrati per l' [inserimento di dipendenze](xref:fundamentals/dependency-injection) usando [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). Il `ContactIsOwnerAuthorizationHandler` utilizza ASP.NET Core [identità](xref:security/authentication/identity), che è compilata in Entity Framework Core. Registrare i gestori con la raccolta di servizi in modo che siano disponibili per il `ContactsController` tramite l' [inserimento di dipendenze](xref:fundamentals/dependency-injection). Aggiungere il codice seguente alla fine del `ConfigureServices`:

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

`ContactAdministratorsAuthorizationHandler` e `ContactManagerAuthorizationHandler` vengono aggiunti come singleton. Si tratta di singleton perché non usano EF e tutte le informazioni necessarie si trovano nel parametro `Context` del metodo `HandleRequirementAsync`.

## <a name="support-authorization"></a>Supporto di autorizzazione

In questa sezione aggiornare le pagine Razor e aggiungere una classe di requisiti di operazioni.

### <a name="review-the-contact-operations-requirements-class"></a>Esaminare la classe di requisiti di operazioni contatto

Esaminare la classe `ContactOperations`. Questa classe contiene i requisiti supportato dall'app:

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a>Creare una classe di base per le pagine Razor di contatti

Creare una classe base che contiene i servizi usati in Razor Pages i contatti. La classe di base inserisce il codice di inizializzazione in un'unica posizione:

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

Il codice precedente:

* Aggiunge il servizio `IAuthorizationService` per accedere ai gestori di autorizzazione.
* Aggiunge il servizio Identity `UserManager`.
* Aggiungere l'oggetto `ApplicationDbContext`.

### <a name="update-the-createmodel"></a>Aggiornare il CreateModel

Aggiornare il costruttore di modello di pagina create per usare la classe di base `DI_BasePageModel`:

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

Aggiornare il metodo `CreateModel.OnPostAsync` a:

* Aggiungere l'ID utente al modello di `Contact`.
* Chiamare il gestore di autorizzazione per verificare che l'utente dispone dell'autorizzazione per creare contatti.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Aggiornare il IndexModel

Aggiornare il metodo `OnGetAsync` in modo da visualizzare solo i contatti approvati per gli utenti generali:

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Aggiornare il EditModel

Aggiungere un gestore di autorizzazione per verificare che l'utente è proprietario del contatto. Poiché l'autorizzazione della risorsa viene convalidata, l'attributo `[Authorize]` non è sufficiente. L'app non ha accesso alla risorsa quando vengono valutati gli attributi. Autorizzazione basata sulle risorse deve essere imperativa. Controlli devono essere eseguiti dopo che l'app potrà accedere alla risorsa, è possibile caricarli nel modello di pagina oppure caricarlo all'interno del gestore stesso. Si accede di frequente della risorsa passando la chiave di risorsa.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Aggiornare il DeleteModel

Aggiornare il modello di pagina delete per utilizzare il gestore dell'autorizzazione per verificare che l'utente disponga dell'autorizzazione di eliminazione per il contatto.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Inserire il servizio di autorizzazione in visualizzazioni

Attualmente, l'interfaccia utente Mostra modifica ed elimina i collegamenti per i contatti che l'utente non è possibile modificare.

Inserire il servizio di autorizzazione nel file *pages/_ViewImports. cshtml* in modo che sia disponibile per tutte le visualizzazioni:

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

Il markup precedente aggiunge diverse istruzioni `using`.

Aggiornare i collegamenti **Edit** ed **Delete** in *pages/Contacts/index. cshtml* in modo che vengano sottoposti a rendering solo per gli utenti con le autorizzazioni appropriate:

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> Nascondere i collegamenti da utenti che non dispone dell'autorizzazione per modificare i dati non proteggere le app. Nascondere i collegamenti rende più semplici da usare l'app mediante la visualizzazione di collegamenti solo validi. Gli utenti possono hack gli URL generati per richiamare modifica ed eliminare le operazioni sui dati che non sono proprietari. La pagina Razor o il controller deve applicare controlli di accesso per proteggere i dati.

### <a name="update-details"></a>Aggiorna i dettagli

Aggiornare la visualizzazione dei dettagli in modo che i responsabili possono approvare o rifiutare i contatti:

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Details.cshtml?name=snippet)]

Aggiornare il modello di pagina dei dettagli:

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a>Aggiungere o rimuovere un utente a un ruolo

Per informazioni su, vedere [questo problema](https://github.com/aspnet/AspNetCore.Docs/issues/8502) :

* Rimozione dei privilegi da un utente. Ad esempio, disattivare un utente in un'app di chat.
* Aggiunta di privilegi a un utente.

<a name="challenge"></a>

## <a name="differences-between-challenge-and-forbid"></a>Differenze tra la sfida e la proibizione

Questa app imposta i criteri predefiniti per [richiedere l'autenticazione degli utenti](#require-authenticated-users). Il codice seguente consente agli utenti anonimi. Gli utenti anonimi possono mostrare le differenze tra la richiesta di confronto e la proibizione.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details2.cshtml.cs?name=snippet)]

Nel codice precedente:

* Quando l'utente **non** è autenticato, viene restituito un `ChallengeResult`. Quando viene restituito un `ChallengeResult`, l'utente viene reindirizzato alla pagina di accesso.
* Quando l'utente viene autenticato, ma non autorizzato, viene restituito un `ForbidResult`. Quando viene restituito un `ForbidResult`, l'utente viene reindirizzato alla pagina di accesso negato.

## <a name="test-the-completed-app"></a>Testare l'app completata

Se non è già stata impostata una password per gli account utente di cui è stato eseguito il seeding, usare lo [strumento Gestione segreta](xref:security/app-secrets#secret-manager) per impostare una password:

* Scegliere una password complessa: usare otto o più caratteri e contenere almeno un carattere maiuscolo, numero e simboli. Ad esempio, `Passw0rd!` soddisfa i requisiti per le password complesse.
* Eseguire il comando seguente dalla cartella del progetto, dove `<PW>` è la password:

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

Se l'app ha contatti:

* Eliminare tutti i record nella tabella `Contact`.
* Riavviare l'app per l'inizializzazione del database.

Un modo semplice per testare l'app completata consiste nell'avviare tre diversi browser (o in incognito o InPrivate sessioni). In un browser registrare un nuovo utente, ad esempio `test@example.com`. Accedere a ogni browser con un altro utente. Verificare che le operazioni seguenti:

* Gli utenti registrati possono visualizzare tutti i dati di contatto approvati.
* Gli utenti registrati possono modificare/eliminare i propri dati.
* I responsabili possono approvare o rifiutare i dati di contatto. La visualizzazione `Details` Mostra i pulsanti **approva** e **rifiuta** .
* Gli amministratori possono approvare o rifiutare e modificare/eliminare tutti i dati.

| Utente                | Esegue il seeding dell'app | Opzioni                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | No                | Modificare/eliminare i propri dati.                |
| manager@contoso.com | Sì               | Approvare o rifiutare e modificare/eliminare i dati personalizzati. |
| admin@contoso.com   | Sì               | Approvare o rifiutare e modificare/eliminare tutti i dati. |

Creare un contatto nel browser dell'amministratore. Copiare l'URL per l'eliminazione e modifica di contattare l'amministratore. Incollare questi collegamenti nel browser dell'utente test per verificare che l'utente di test non è possibile eseguire queste operazioni.

## <a name="create-the-starter-app"></a>Creare l'app di base

* Creare un'app Razor Pages denominata "ContactManager"
  * Creare l'app con **singoli account utente**.
  * Il nome "ContactManager" in modo che lo spazio dei nomi corrispondente dello spazio dei nomi utilizzato nell'esempio.
  * `-uld` specifica il database locale anziché SQLite

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* Aggiungi *modelli/Contact. cs*:

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* Impalcatura del modello di `Contact`.
* Creare la migrazione iniziale e l'aggiornamento del database:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

Se si verifica un bug con il comando `dotnet aspnet-codegenerator razorpage`, vedere [questo problema di GitHub](https://github.com/aspnet/Scaffolding/issues/984).

* Aggiornare l'ancoraggio **ContactManager** nel file *pages/Shared/_Layout. cshtml* :

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* Testare l'app per la creazione, modifica ed eliminazione di un contatto

### <a name="seed-the-database"></a>Specificare il valore di inizializzazione del database

Aggiungere la classe [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) alla cartella *Data* :

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

Chiama `SeedData.Initialize` da `Main`:

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

Verificare che l'app effettuato il seeding del database. Se sono presenti tutte le righe nel DB del contatto, il metodo di inizializzazione non viene eseguito.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

Questa esercitazione illustra come creare un'app web ASP.NET Core con i dati utente protetti da autorizzazione. Viene visualizzato un elenco di contatti che (registrati) agli utenti autenticati hanno creato. Sono disponibili tre gruppi di sicurezza:

* **Gli utenti registrati** possono visualizzare tutti i dati approvati e possono modificare/eliminare i propri dati.
* I **responsabili** possono approvare o rifiutare i dati di contatto. Solo i contatti approvati sono visibili agli utenti.
* Gli **amministratori** possono approvare/rifiutare e modificare/eliminare i dati.

Nell'immagine seguente l'utente Rick (`rick@example.com`) ha eseguito l'accesso. Rick può visualizzare solo i contatti approvati e **modificare**/**eliminare**/**creare nuovi** collegamenti per i propri contatti. Solo l'ultimo record, creato da Rick, Visualizza i collegamenti **modifica** ed **Elimina** . Gli altri utenti non vedono l'ultimo record fino a quando un responsabile o l'amministratore assume lo stato "Approvato".

![Screenshot che mostra Rick effettuato l'accesso](secure-data/_static/rick.png)

Nella figura seguente `manager@contoso.com` è stato eseguito l'accesso e nel ruolo del Manager:

![Screenshot che mostra l'accesso manager@contoso.com](secure-data/_static/manager1.png)

L'immagine seguente mostra i gestori visualizzazione dettagli di un contatto:

![Visualizzazione di gestione di un contatto](secure-data/_static/manager.png)

I pulsanti **approva** e **rifiuta** vengono visualizzati solo per Manager e amministratori.

Nella figura seguente `admin@contoso.com` è stato eseguito l'accesso e il ruolo dell'amministratore:

![Screenshot che mostra l'accesso admin@contoso.com](secure-data/_static/admin.png)

L'amministratore ha tutti i privilegi. Può leggere, modificare ed eliminare un contatto e modificare lo stato dei contatti.

L'app è stata creata mediante l' [impalcatura](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) del modello di `Contact` seguente:

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

L'esempio contiene i gestori di autorizzazione seguenti:

* `ContactIsOwnerAuthorizationHandler`: assicura che un utente possa modificare solo i propri dati.
* `ContactManagerAuthorizationHandler`: consente ai responsabili di approvare o rifiutare i contatti.
* `ContactAdministratorsAuthorizationHandler`: consente agli amministratori di approvare o rifiutare i contatti e di modificare/eliminare contatti.

## <a name="prerequisites"></a>Prerequisiti

Questa esercitazione viene fatto avanzare. È necessario avere familiarità con:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Autenticazione](xref:security/authentication/identity)
* [Conferma dell'account e recupero della password](xref:security/authentication/accconfirm)
* [Autorizzazione](xref:security/authorization/introduction)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>Starter e app completata

[Scaricare](xref:index#how-to-download-a-sample) l'app [completata](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) . [Testare](#test-the-completed-app) l'app completata in modo da acquisire familiarità con le funzionalità di sicurezza.

### <a name="the-starter-app"></a>L'app di base

[Scaricare](xref:index#how-to-download-a-sample) l'app [Starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) .

Eseguire l'app, toccare il collegamento **ContactManager** e verificare che sia possibile creare, modificare ed eliminare un contatto.

## <a name="secure-user-data"></a>Proteggere i dati utente

Le sezioni seguenti includono tutti i passaggi principali per creare l'app per dati sicura degli utenti. Si può risultare utile per fare riferimento al progetto completato.

### <a name="tie-the-contact-data-to-the-user"></a>Associare i dati di contatto per l'utente

Usare l'ID utente dell' [identità](xref:security/authentication/identity) ASP.NET per assicurarsi che gli utenti possano modificare i dati, ma non i dati di altri utenti. Aggiungere `OwnerID` e `ContactStatus` al modello di `Contact`:

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` è l'ID dell'utente della tabella `AspNetUser` nel database di [identità](xref:security/authentication/identity) . Il campo `Status` determina se un contatto è visualizzabile dagli utenti generali.

Creare una nuova migrazione e aggiornare il database:

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a>Aggiungere altri servizi di identità

Aggiungere [Aggiungi ruoli](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) per aggiungere servizi ruolo:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a>Richiedere agli utenti autenticati

Impostare i criteri di autenticazione predefinito per richiedere agli utenti di essere autenticato:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 È possibile rifiutare esplicitamente l'autenticazione a livello di pagina Razor, controller o metodo di azione con l'attributo `[AllowAnonymous]`. L'impostazione di criteri di autenticazione predefinito per richiedere agli utenti di essere autenticati protegge appena aggiunto Razor Pages e controller. La necessità di eseguire l'autenticazione per impostazione predefinita è più sicura rispetto all'uso di nuovi controller e Razor Pages per includere l'attributo `[Authorize]`.

Aggiungere [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) alle pagine di indice, informazioni e contatti in modo che gli utenti anonimi possano ottenere informazioni sul sito prima della registrazione.

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a>Configurare l'account di test

La classe `SeedData` crea due account: Administrator e Manager. Utilizzare lo [strumento Gestione segreta](xref:security/app-secrets) per impostare una password per questi account. Impostare la password dalla directory del progetto (la directory contenente *Program.cs*):

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

Se non si specifica una password complessa, quando viene chiamato `SeedData.Initialize` viene generata un'eccezione.

Aggiornare `Main` per usare la password di prova:

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Creare gli account di test e aggiornare i contatti

Aggiornare il metodo `Initialize` nella classe `SeedData` per creare gli account di test:

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

Aggiungere l'ID utente amministratore e `ContactStatus` ai contatti. Rendere uno dei contatti relativi al "Submitted" e "Rejected" uno. Aggiungere l'ID utente e lo stato per tutti i contatti. Viene visualizzato un solo contatto:

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Creare proprietario, manager e i gestori di autorizzazione di amministratore

Creare una cartella di *autorizzazione* e crearvi una classe `ContactIsOwnerAuthorizationHandler`. Il `ContactIsOwnerAuthorizationHandler` verifica che l'utente che agisce su una risorsa appartenga alla risorsa.

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

Il `ContactIsOwnerAuthorizationHandler` chiama il [contesto. Ha esito positivo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) se l'utente autenticato corrente è il proprietario del contatto. I gestori di autorizzazione a livello generale:

* Restituisce `context.Succeed` quando vengono soddisfatti i requisiti.
* Restituisce `Task.CompletedTask` quando i requisiti non vengono soddisfatti. `Task.CompletedTask` non ha esito positivo o negativo&mdash;consente l'esecuzione di altri gestori di autorizzazione.

Se è necessario eseguire in modo esplicito l'errore, restituire [context. Esito negativo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

L'app consente ai proprietari di contatto di modifica/eliminazione/creare i propri dati. `ContactIsOwnerAuthorizationHandler` non è necessario controllare l'operazione passata nel parametro requirement.

### <a name="create-a-manager-authorization-handler"></a>Creare un gestore di autorizzazione di gestione

Creare una classe `ContactManagerAuthorizationHandler` nella cartella *authorization* . Il `ContactManagerAuthorizationHandler` verifica che l'utente che agisce sulla risorsa sia un responsabile. Solo i manager possono approvare o rifiutare le modifiche di contenuto (nuove o modificate).

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Creare un gestore di autorizzazione di amministratore

Creare una classe `ContactAdministratorsAuthorizationHandler` nella cartella *authorization* . Il `ContactAdministratorsAuthorizationHandler` verifica che l'utente che agisce sulla risorsa sia un amministratore. Amministratore può eseguire tutte le operazioni.

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Registrare i gestori di eventi di autorizzazione

I servizi che usano Entity Framework Core devono essere registrati per l' [inserimento di dipendenze](xref:fundamentals/dependency-injection) usando [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). Il `ContactIsOwnerAuthorizationHandler` utilizza ASP.NET Core [identità](xref:security/authentication/identity), che è compilata in Entity Framework Core. Registrare i gestori con la raccolta di servizi in modo che siano disponibili per il `ContactsController` tramite l' [inserimento di dipendenze](xref:fundamentals/dependency-injection). Aggiungere il codice seguente alla fine del `ConfigureServices`:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

`ContactAdministratorsAuthorizationHandler` e `ContactManagerAuthorizationHandler` vengono aggiunti come singleton. Si tratta di singleton perché non usano EF e tutte le informazioni necessarie si trovano nel parametro `Context` del metodo `HandleRequirementAsync`.

## <a name="support-authorization"></a>Supporto di autorizzazione

In questa sezione aggiornare le pagine Razor e aggiungere una classe di requisiti di operazioni.

### <a name="review-the-contact-operations-requirements-class"></a>Esaminare la classe di requisiti di operazioni contatto

Esaminare la classe `ContactOperations`. Questa classe contiene i requisiti supportato dall'app:

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a>Creare una classe di base per le pagine Razor di contatti

Creare una classe base che contiene i servizi usati in Razor Pages i contatti. La classe di base inserisce il codice di inizializzazione in un'unica posizione:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

Il codice precedente:

* Aggiunge il servizio `IAuthorizationService` per accedere ai gestori di autorizzazione.
* Aggiunge il servizio Identity `UserManager`.
* Aggiungere l'oggetto `ApplicationDbContext`.

### <a name="update-the-createmodel"></a>Aggiornare il CreateModel

Aggiornare il costruttore di modello di pagina create per usare la classe di base `DI_BasePageModel`:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

Aggiornare il metodo `CreateModel.OnPostAsync` a:

* Aggiungere l'ID utente al modello di `Contact`.
* Chiamare il gestore di autorizzazione per verificare che l'utente dispone dell'autorizzazione per creare contatti.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Aggiornare il IndexModel

Aggiornare il metodo `OnGetAsync` in modo da visualizzare solo i contatti approvati per gli utenti generali:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Aggiornare il EditModel

Aggiungere un gestore di autorizzazione per verificare che l'utente è proprietario del contatto. Poiché l'autorizzazione della risorsa viene convalidata, l'attributo `[Authorize]` non è sufficiente. L'app non ha accesso alla risorsa quando vengono valutati gli attributi. Autorizzazione basata sulle risorse deve essere imperativa. Controlli devono essere eseguiti dopo che l'app potrà accedere alla risorsa, è possibile caricarli nel modello di pagina oppure caricarlo all'interno del gestore stesso. Si accede di frequente della risorsa passando la chiave di risorsa.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Aggiornare il DeleteModel

Aggiornare il modello di pagina delete per utilizzare il gestore dell'autorizzazione per verificare che l'utente disponga dell'autorizzazione di eliminazione per il contatto.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Inserire il servizio di autorizzazione in visualizzazioni

Attualmente, l'interfaccia utente Mostra modifica ed elimina i collegamenti per i contatti che l'utente non è possibile modificare.

Inserire il servizio di autorizzazione nel file *views/_ViewImports. cshtml* in modo che sia disponibile per tutte le visualizzazioni:

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

Il markup precedente aggiunge diverse istruzioni `using`.

Aggiornare i collegamenti **Edit** ed **Delete** in *pages/Contacts/index. cshtml* in modo che vengano sottoposti a rendering solo per gli utenti con le autorizzazioni appropriate:

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> Nascondere i collegamenti da utenti che non dispone dell'autorizzazione per modificare i dati non proteggere le app. Nascondere i collegamenti rende più semplici da usare l'app mediante la visualizzazione di collegamenti solo validi. Gli utenti possono hack gli URL generati per richiamare modifica ed eliminare le operazioni sui dati che non sono proprietari. La pagina Razor o il controller deve applicare controlli di accesso per proteggere i dati.

### <a name="update-details"></a>Aggiorna i dettagli

Aggiornare la visualizzazione dei dettagli in modo che i responsabili possono approvare o rifiutare i contatti:

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

Aggiornare il modello di pagina dei dettagli:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a>Aggiungere o rimuovere un utente a un ruolo

Per informazioni su, vedere [questo problema](https://github.com/aspnet/AspNetCore.Docs/issues/8502) :

* Rimozione dei privilegi da un utente. Ad esempio, disattivare un utente in un'app di chat.
* Aggiunta di privilegi a un utente.

## <a name="test-the-completed-app"></a>Testare l'app completata

Se non è già stata impostata una password per gli account utente di cui è stato eseguito il seeding, usare lo [strumento Gestione segreta](xref:security/app-secrets#secret-manager) per impostare una password:

* Scegliere una password complessa: usare otto o più caratteri e contenere almeno un carattere maiuscolo, numero e simboli. Ad esempio, `Passw0rd!` soddisfa i requisiti per le password complesse.
* Eseguire il comando seguente dalla cartella del progetto, dove `<PW>` è la password:

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

* Eliminare e aggiornare il database

  ```dotnetcli
  dotnet ef database drop -f
  dotnet ef database update  
  ```

* Riavviare l'app per l'inizializzazione del database.

Un modo semplice per testare l'app completata consiste nell'avviare tre diversi browser (o in incognito o InPrivate sessioni). In un browser registrare un nuovo utente, ad esempio `test@example.com`. Accedere a ogni browser con un altro utente. Verificare che le operazioni seguenti:

* Gli utenti registrati possono visualizzare tutti i dati di contatto approvati.
* Gli utenti registrati possono modificare/eliminare i propri dati.
* I responsabili possono approvare o rifiutare i dati di contatto. La visualizzazione `Details` Mostra i pulsanti **approva** e **rifiuta** .
* Gli amministratori possono approvare o rifiutare e modificare/eliminare tutti i dati.

| Utente                | Esegue il seeding dell'app | Opzioni                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | No                | Modificare/eliminare i propri dati.                |
| manager@contoso.com | Sì               | Approvare o rifiutare e modificare/eliminare i dati personalizzati. |
| admin@contoso.com   | Sì               | Approvare o rifiutare e modificare/eliminare tutti i dati. |

Creare un contatto nel browser dell'amministratore. Copiare l'URL per l'eliminazione e modifica di contattare l'amministratore. Incollare questi collegamenti nel browser dell'utente test per verificare che l'utente di test non è possibile eseguire queste operazioni.

## <a name="create-the-starter-app"></a>Creare l'app di base

* Creare un'app Razor Pages denominata "ContactManager"
  * Creare l'app con **singoli account utente**.
  * Il nome "ContactManager" in modo che lo spazio dei nomi corrispondente dello spazio dei nomi utilizzato nell'esempio.
  * `-uld` specifica il database locale anziché SQLite

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* Aggiungi *modelli/Contact. cs*:

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* Impalcatura del modello di `Contact`.
* Creare la migrazione iniziale e l'aggiornamento del database:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* Aggiornare l'ancoraggio **ContactManager** nel file *pages/_Layout. cshtml* :

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* Testare l'app per la creazione, modifica ed eliminazione di un contatto

### <a name="seed-the-database"></a>Specificare il valore di inizializzazione del database

Aggiungere la classe [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) alla cartella *Data* .

Chiama `SeedData.Initialize` da `Main`:

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

Verificare che l'app effettuato il seeding del database. Se sono presenti tutte le righe nel DB del contatto, il metodo di inizializzazione non viene eseguito.

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>Risorse aggiuntive

* [Creare un'app Web .NET Core e database SQL nel servizio app Azure](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [ASP.NET Core Lab di autorizzazione](https://github.com/blowdart/AspNetAuthorizationWorkshop). Questa esercitazione descrive in dettaglio più le funzionalità di sicurezza introdotte in questa esercitazione.
* <xref:security/authorization/introduction>
* [Autorizzazione personalizzata basata su criteri](xref:security/authorization/policies)
