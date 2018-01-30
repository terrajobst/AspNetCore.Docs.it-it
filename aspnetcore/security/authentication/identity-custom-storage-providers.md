---
title: Provider di archiviazione personalizzato per ASP.NET Identity Core
author: ardalis
description: Informazioni su come configurare i provider di archiviazione personalizzato per ASP.NET Identity Core.
manager: wpickett
ms.author: riande
ms.date: 05/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: 8541fe47207c0af232ca81ae45da6af201d94799
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a>Provider di archiviazione personalizzato per ASP.NET Identity Core

Da [Steve Smith](https://ardalis.com/)

Identità di ASP.NET Core è un sistema estendibile che consente di creare un provider di archiviazione personalizzato e collegarlo all'app. In questo argomento viene descritto come creare un provider di archiviazione personalizzato per ASP.NET Identity Core. Vengono illustrati i concetti importanti per la creazione di un provider di archiviazione, ma non è una procedura dettagliata.

[Consente di visualizzare o scaricare l'esempio da GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample).

## <a name="introduction"></a>Introduzione

Per impostazione predefinita, il sistema di ASP.NET Identity Core memorizza informazioni utente in un database di SQL Server usando Entity Framework Core. Per molte applicazioni, questo approccio funziona bene. Tuttavia, è preferibile utilizzare un meccanismo di persistenza diverso o schema di dati. Ad esempio:

* Utilizzare [archiviazione tabelle di Azure](https://docs.microsoft.com/azure/storage/) o un altro archivio dati.
* Tabelle di database hanno una struttura differente. 
* Si consiglia di utilizzare un approccio di accesso ai dati diversi, ad esempio [Dapper](https://github.com/StackExchange/Dapper). 

In ognuno di questi casi, è possibile scrivere un provider personalizzato per il meccanismo di archiviazione e integrare tale provider nell'app.

Identità di ASP.NET Core è incluso nei modelli di progetto in Visual Studio con l'opzione "Singoli account utente".

Quando si utilizza l'interfaccia CLI Core .NET, aggiungere `-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a>L'architettura di ASP.NET Identity Core

ASP.NET Identity Core è costituito da classi, denominato responsabili e archivi. *Gestori* sono classi di alto livello che usa da uno sviluppatore di app per eseguire operazioni, ad esempio la creazione di un utente di identità. *Archivi* che specificare le modalità di persistenza delle entità, ad esempio utenti e ruoli, le classi di livello inferiore. Seguono gli archivi di [modello di repository](http://deviq.com/repository-pattern/) e sono strettamente collegate con il meccanismo di persistenza. Gestori vengono disaccoppiati da archivi, ovvero che è possibile sostituire il meccanismo di persistenza senza modificare il codice dell'applicazione (ad eccezione di configurazione).

Il diagramma seguente mostra come un'app web interagisce con le Gestioni, mentre gli archivi di interagire con il livello di accesso ai dati.

![Le app ASP.NET Core funzioneranno con i gestori (ad esempio, 'UserManager', 'RoleManager'). Gestioni funzionano con archivi (ad esempio, ' oggetto UserStore') che comunicano con un'origine dati tramite una libreria come Entity Framework Core.](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

Per creare un provider di archiviazione personalizzata, creare l'origine dati, il livello di accesso ai dati e le classi di archivio che interagiscono con il livello di accesso ai dati (caselle grigie e verde nel diagramma precedente). Non è necessario personalizzare i gestori o codice dell'app che interagisce con essi (le caselle blu sopra).

Quando si crea una nuova istanza della `UserManager` o `RoleManager` è fornire il tipo della classe utente e passare un'istanza della classe di archiviazione come argomento. Questo approccio consente di inserire le classi personalizzate in ASP.NET Core. 

[Riconfigurare l'applicazione per utilizzare il nuovo provider di archiviazione](#reconfigure-app-to-use-new-storage-provider) viene illustrato come creare un'istanza di `UserManager` e `RoleManager` con un archivio personalizzato.

## <a name="aspnet-core-identity-stores-data-types"></a>Identità di ASP.NET Core vengono archiviati tipi di dati

[ASP.NET Identity Core](https://github.com/aspnet/identity) tipi di dati vengono descritti in dettaglio nelle sezioni seguenti:

### <a name="users"></a>Utenti

Utenti registrati del sito web. Il [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser) tipo può essere estesa o utilizzato come esempio per un tipo personalizzato. Non è necessario ereditare da un determinato tipo per implementare la propria soluzione di archiviazione di identità personalizzata.

### <a name="user-claims"></a>Attestazioni utente

Un set di istruzioni (o [attestazioni](https://docs.microsoft.com//dotnet/api/system.security.claims.claim)) sull'utente che rappresentano l'identità dell'utente. Consente l'espressione più grande dell'identità dell'utente rispetto a quello ottenibile tramite i ruoli.

### <a name="user-logins"></a>Account di accesso utente

Informazioni sul provider di autenticazione esterno (ad esempio Facebook o un account Microsoft) da utilizzare durante l'accesso a un utente. [Esempio](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a>Ruoli

Gruppi di autorizzazioni per il sito. Include il nome di ruolo e l'Id del ruolo (ad esempio, "Admin" o "Employee"). [Esempio](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a>Il livello di accesso ai dati

In questo argomento si presuppone che si ha familiarità con il meccanismo di persistenza che si desidera utilizzare e come creare le entità per tale meccanismo. In questo argomento non fornisce informazioni dettagliate su come creare il repository o classi di accesso ai dati. vengono forniti alcuni suggerimenti sulle decisioni di progettazione quando si lavora con ASP.NET Identity Core.

Sono presenti molti di libertà quando si progetta il livello di accesso ai dati per un provider di archivio personalizzato. È sufficiente creare meccanismi di persistenza per le funzionalità che si intendono utilizzare nell'applicazione. Ad esempio, se non si utilizzano ruoli nell'applicazione, non occorre creare spazio di archiviazione per i ruoli o le associazioni di ruolo utente. La tecnologia e l'infrastruttura esistente potrebbe richiedere una struttura che è molto diversa dall'implementazione predefinita di ASP.NET Identity Core. Nel livello di accesso ai dati, fornire la logica per il funzionamento con la struttura dell'implementazione di archiviazione.

Il livello di accesso ai dati fornisce la logica per salvare i dati di ASP.NET Identity Core a un'origine dati. Il livello di accesso ai dati per il provider di archiviazione personalizzato potrebbe includere le seguenti classi per archiviare le informazioni utente e il ruolo.

### <a name="context-class"></a>Context (classe)

Incapsula le informazioni per connettersi al meccanismo di persistenza ed eseguire query. Diverse classi di dati richiedono un'istanza di questa classe, in genere viene fornita tramite l'inserimento di dipendenze. [Esempio](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1).

### <a name="user-storage"></a>Archiviazione utente

Archivia e recupera le informazioni utente (ad esempio, l'hash di nome e una password utente). [Esempio](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a>Archiviazione di ruolo

Archivia e recupera le informazioni sui ruoli (ad esempio il nome del ruolo). [Esempio](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a>Archiviazione di attestazioni utente

Archivia e recupera le informazioni sulle attestazioni utente (ad esempio il tipo di attestazione e il valore). [Esempio](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a>Archiviazione di accessi utente

Archivia e recupera informazioni sull'accesso utente (ad esempio un provider di autenticazione esterno). [Esempio](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a>Archiviazione UserRole

Archivia e recupera i ruoli assegnati a cui gli utenti. [Esempio](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

**Suggerimento:** implementare solo le classi che si intende utilizzare nell'applicazione.

Nelle classi di accesso ai dati, fornire il codice per eseguire operazioni sui dati per il meccanismo di persistenza. Ad esempio, all'interno di un provider personalizzato, è necessario il codice seguente per creare un nuovo utente nel *archiviare* classe:

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

La logica di implementazione per la creazione dell'utente è il ``_usersTable.CreateAsync`` metodo illustrato di seguito.

## <a name="customize-the-user-class"></a>Personalizzare la classe utente

Quando si implementa un provider di archiviazione, creare una classe utente, che equivale al [ `IdentityUser` classe](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser).

Come minimo, è necessario includere la classe utente un `Id` e `UserName` proprietà.

Il `IdentityUser` classe definisce le proprietà che la ``UserManager`` chiamate quando si eseguono le operazioni richieste. Il tipo predefinito del `Id` proprietà è una stringa, ma è possibile ereditare da `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` e specificare un tipo diverso. Il framework prevede l'implementazione di archiviazione per gestire le conversioni di tipo di dati.

## <a name="customize-the-user-store"></a>Personalizzare l'archivio dell'utente

Creare un `UserStore` classe che fornisce i metodi per tutte le operazioni di dati per l'utente. Questa classe è equivalente al [nello UserStore<TUser> ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1) classe. Nel `UserStore` classe, implementare `IUserStore<TUser>` e le interfacce facoltative richieste. Selezionare facoltative interfacce da implementare in base alle funzionalità disponibili in app.

### <a name="optional-interfaces"></a>Interfacce facoltative

- Https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1 IUserRoleStore
- Https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1 IUserClaimStore
- Https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1 IUserPasswordStore
- IUserSecurityStampStore<!-- make these all links and remove / -->
- IUserEmailStore
- IPhoneNumberStore
- IQueryableUserStore
- IUserLoginStore
- IUserTwoFactorStore
- IUserLockoutStore

Il parametro facoltativo che ereditano da `IUserStore`. È possibile visualizzare un utente di esempio parzialmente implementato archiviare [qui](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs).

All'interno di `UserStore` (classe), utilizzare le classi di accesso ai dati creati per eseguire operazioni. Questi vengono passati mediante l'inserimento di dipendenza. Ad esempio, in SQL Server con implementazione Dapper, il `UserStore` classe dispone di `CreateAsync` metodo che utilizza un'istanza di `DapperUsersTable` per inserire un nuovo record:

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Interfacce da implementare per la personalizzazione archivio utente

- **IUserStore**  
 Il [IUserStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserstore-1) è l'unica interfaccia è necessario implementare nell'archivio dell'utente. Definisce metodi per la creazione, aggiornamento, eliminazione e recupero degli utenti.
- **IUserClaimStore**  
 Il [IUserClaimStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1) interfaccia definisce i metodi implementati per abilitare le attestazioni utente. Contiene metodi per l'aggiunta, rimozione e recupera le attestazioni utente.
- **IUserLoginStore**  
 Il [IUserLoginStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserloginstore-1) definisce i metodi implementati per consentire ai provider di autenticazione esterno. Contiene metodi per l'aggiunta, rimozione e il recupero degli account di accesso utente e un metodo per il recupero di un utente in base alle informazioni di accesso.
- **IUserRoleStore**  
 Il [IUserRoleStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1) interfaccia definisce i metodi implementati per eseguire il mapping di un utente a un ruolo. Contiene metodi per aggiungere, rimuovere e recuperare i ruoli dell'utente e un metodo per verificare se un utente è assegnato a un ruolo.
- **IUserPasswordStore**  
 Il [IUserPasswordStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1) interfaccia definisce i metodi implementati per mantenere le password con hash. Contiene metodi per ottenere e impostare la password con hash e un metodo che indica se l'utente ha impostato una password.
- **IUserSecurityStampStore**  
 Il [IUserSecurityStampStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1) interfaccia definisce i metodi implementati per l'utilizzo di un indicatore di sicurezza per indicare se le informazioni sull'account è stato modificato. Questo indicatore viene aggiornato quando un utente modifica la password, o aggiunge o rimuove gli account di accesso. Contiene metodi per ottenere e impostare l'indicatore di sicurezza.
- **IUserTwoFactorStore**  
 Il [IUserTwoFactorStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iusertwofactorstore-1) interfaccia definisce i metodi implementati per supportare l'autenticazione a due fattori. Contiene metodi per ottenere e impostare se per un utente è abilitata l'autenticazione a due fattori.
- **IUserPhoneNumberStore**  
 Il [IUserPhoneNumberStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1) interfaccia definisce i metodi implementati per archiviare i numeri di telefono utente. Contiene metodi per ottenere e impostare il numero di telefono e indica se il numero di telefono è confermato.
- **IUserEmailStore**  
 Il [IUserEmailStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuseremailstore-1) interfaccia definisce i metodi implementati per archiviare gli indirizzi di posta elettronica utente. Contiene metodi per ottenere e impostare l'indirizzo di posta elettronica e se il messaggio di posta elettronica è confermato.
- **IUserLockoutStore**  
 Il [IUserLockoutStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserlockoutstore-1) interfaccia definisce i metodi implementati per archiviare le informazioni sul blocco di un account. Contiene metodi per il rilevamento di tentativi di accesso non riusciti e blocchi.
- **IQueryableUserStore**  
 Il [IQueryableUserStore&lt;TUser&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iqueryableuserstore-1) interfaccia definisce implementare i membri per fornire un archivio utente queryable.

Implementare solo le interfacce che sono necessari nell'app. Ad esempio:

```csharp
public class UserStore : IUserStore<IdentityUser>,
                         IUserClaimStore<IdentityUser>,
                         IUserLoginStore<IdentityUser>,
                         IUserRoleStore<IdentityUser>,
                         IUserPasswordStore<IdentityUser>,
                         IUserSecurityStampStore<IdentityUser>
{
    // interface implementations not shown
}
```

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim IdentityUserLogin e IdentityUserRole

Il ``Microsoft.AspNet.Identity.EntityFramework`` dello spazio dei nomi contiene le implementazioni del [IdentityUserClaim](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1), [IdentityUserLogin](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin), e [IdentityUserRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1) classi. Se si utilizzano queste funzionalità, si desidera creare la propria versioni di queste classi e definire le proprietà per l'app. Tuttavia, a volte risulta più efficiente per non caricare queste entità in memoria durante l'esecuzione di operazioni di base (ad esempio aggiunta o rimozione di attestazione dell'utente). Al contrario, le classi di archiviazione back-end possono eseguire queste operazioni direttamente sull'origine dati. Ad esempio, il ``UserStore.GetClaimsAsync`` metodo può chiamare il ``userClaimTable.FindByUserId(user.Id)`` metodo per eseguire una query nella tabella direttamente e restituire un elenco di attestazioni.

## <a name="customize-the-role-class"></a>Personalizzare la classe di ruoli

Quando si implementa un provider di archiviazione di ruolo, è possibile creare un tipo di ruolo personalizzata. Non è necessario implementare un'interfaccia specifica, ma deve essere un `Id` e in genere avrà un `Name` proprietà.

Di seguito è una classe ruolo di esempio:

[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a>Personalizzare l'archivio di ruolo

È possibile creare un ``RoleStore`` classe che fornisce i metodi per tutte le operazioni di dati sui ruoli. Questa classe è equivalente al [oggetto RoleStore<TRole> ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1) classe. Nel `RoleStore` (classe), implementare il ``IRoleStore<TRole>`` e facoltativamente la ``IQueryableRoleStore<TRole>`` interfaccia.

- **IRoleStore&lt;TRole&gt;**  
 Il [IRoleStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.irolestore-1) interfaccia definisce i metodi da implementare nella classe dell'archivio di ruolo. Contiene metodi per la creazione, aggiornamento, eliminazione e il recupero dei ruoli.
- **RoleStore&lt;TRole&gt;**  
 Per personalizzare `RoleStore`, creare una classe che implementa il `IRoleStore` interfaccia. 

## <a name="reconfigure-app-to-use-new-storage-provider"></a>Riconfigurare l'applicazione per utilizzare il nuovo provider di archiviazione

Dopo aver implementato un provider di archiviazione, configurare l'app per utilizzarlo. Se l'app utilizza il provider predefinito, sostituirlo con il provider personalizzato.

1. Rimuovere il `Microsoft.AspNetCore.EntityFramework.Identity` pacchetto NuGet.
1. Se il provider di archiviazione si trova in un progetto separato o un pacchetto, aggiungere un riferimento a esso.
1. Sostituire tutti i riferimenti a `Microsoft.AspNetCore.EntityFramework.Identity` con un'istruzione per lo spazio dei nomi del provider di archiviazione.
1. Nel ``ConfigureServices`` (metodo), modifica il `AddIdentity` metodo da utilizzare tipi personalizzati. È possibile creare i propri metodi di estensione per questo scopo. Vedere [IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) per un esempio.
1. Se si utilizzano ruoli, aggiornare il `RoleManager` per utilizzare il `RoleStore` classe.
1. Aggiornare la stringa di connessione e le credenziali per la configurazione dell'applicazione.

Esempio:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add identity types
    services.AddIdentity<ApplicationUser, ApplicationRole>()
        .AddDefaultTokenProviders();

    // Identity Services
    services.AddTransient<IUserStore<ApplicationUser>, CustomUserStore>();
    services.AddTransient<IRoleStore<ApplicationRole>, CustomRoleStore>();
    string connectionString = Configuration.GetConnectionString("DefaultConnection");
    services.AddTransient<SqlConnection>(e => new SqlConnection(connectionString));
    services.AddTransient<DapperUsersTable>();

    // additional configuration
}
```

## <a name="references"></a>Riferimenti

- [Provider di archiviazione personalizzato per l'identità ASP.NET](https://docs.microsoft.com/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
- [ASP.NET Identity Core](https://github.com/aspnet/identity) -questo repository include collegamenti alla Comunità gestiti provider dell'archivio.
