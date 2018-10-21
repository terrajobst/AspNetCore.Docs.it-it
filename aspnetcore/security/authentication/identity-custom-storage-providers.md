---
title: Provider di archiviazione personalizzati per ASP.NET Core Identity
author: ardalis
description: Informazioni su come configurare il provider di archiviazione personalizzati per ASP.NET Core Identity.
ms.author: riande
ms.date: 09/17/2018
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: e206cf584d92a17d61676d71abc6fb577ae63453
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477618"
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a>Provider di archiviazione personalizzati per ASP.NET Core Identity

Di [Steve Smith](https://ardalis.com/)

ASP.NET Core Identity è un sistema estendibile che consente di creare un provider di archiviazione personalizzati e connetterla all'app. Questo argomento descrive come creare un provider di archiviazione personalizzati per ASP.NET Core Identity. Sono inclusi i concetti importanti per la creazione di un proprio provider di archiviazione, ma non è una procedura dettagliata.

[Visualizzare o scaricare un esempio da GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample).

## <a name="introduction"></a>Introduzione

Per impostazione predefinita, il sistema di ASP.NET Core Identity archivia informazioni utente in un database di SQL Server usando Entity Framework Core. Per molte App, questo approccio funziona bene. Tuttavia, è preferibile usare un meccanismo di persistenza diversa o schema di dati. Ad esempio:

* Si utilizza [archiviazione tabelle di Azure](https://docs.microsoft.com/azure/storage/) o un altro archivio dati.
* Le tabelle di database hanno una struttura diversa. 
* È possibile usare un approccio di accesso di dati diversi, ad esempio [Dapper](https://github.com/StackExchange/Dapper). 

In ognuno di questi casi, è possibile scrivere un provider personalizzato per il meccanismo di archiviazione e inserire tale provider nell'app.

ASP.NET Core Identity è incluso nei modelli di progetto in Visual Studio con l'opzione "Account utente individuali".

Quando si usa .NET Core CLI, aggiungere `-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a>L'architettura di ASP.NET Core Identity

ASP.NET Core Identity è costituito da classi responsabili e gli archivi. *I responsabili* sono classi ad alto livello che gli sviluppatori di app impiega per eseguire operazioni, ad esempio la creazione di un utente di identità. *Gli archivi* sono classi di basso livello che specificano come entità, ad esempio utenti e ruoli, vengono resi persistenti. Gli archivi seguono il modello di repository e sono strettamente associati con il meccanismo di persistenza. I responsabili sono separati dagli archivi, che significa che è possibile sostituire il meccanismo di persistenza senza modificare il codice dell'applicazione (ad eccezione di configurazione).

Il diagramma seguente mostra come un'app web interagisce con i gestori, mentre gli archivi interagiscono con il livello di accesso ai dati.

![Le app ASP.NET Core funzionano con i gestori (ad esempio, 'UserManager', 'RoleManager'). I responsabili usano archivi (ad esempio, ' oggetto UserStore') che comunicano con un'origine dati usando una libreria, ad esempio Entity Framework Core.](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

Per creare un provider di archiviazione personalizzati, creare l'origine dati, il livello di accesso ai dati e le classi di archiviazione che interagiscono con questo livello di accesso ai dati (caselle grigie e verde nel diagramma precedente). Non è necessario personalizzare i responsabili o il codice dell'app che interagisce con essi (le caselle blu sopra).

Quando si crea una nuova istanza della `UserManager` o `RoleManager` è specificare il tipo della classe utente e passare un'istanza della classe store come argomento. Questo approccio consente di inserire le classi personalizzate in ASP.NET Core. 

[Riconfigurare l'appper utilizzare il nuovo provider di archiviazione](#reconfigure-app-to-use-a-new-storage-provider) viene illustrato come creare un'istanza `UserManager` e `RoleManager` con un archivio personalizzato.

## <a name="aspnet-core-identity-stores-data-types"></a>ASP.NET Core Identity archivia i tipi di dati

[ASP.NET Core Identity](https://github.com/aspnet/identity) i tipi di dati sono descritti in dettaglio nelle sezioni seguenti:

### <a name="users"></a>Utenti

Utenti registrati del sito web. Il [IdentityUser](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuser) tipo può essere estesa o utilizzato come esempio per il tipo personalizzato. Non è necessario ereditare da un determinato tipo per implementare la propria soluzione di archiviazione identità personalizzata.

### <a name="user-claims"></a>Attestazioni utente

Un set di istruzioni (o [attestazioni](/dotnet/api/system.security.claims.claim)) sull'utente che rappresentano l'identità dell'utente. Possibile abilitare l'espressione più grande dell'identità dell'utente rispetto a quello ottenibile tramite i ruoli.

### <a name="user-logins"></a>Account di accesso utente

Informazioni sul provider di autenticazione esterni (ad esempio Facebook o un account Microsoft) da usare durante la registrazione di un utente. [Esempio](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a>Ruoli

Gruppi di autorizzazione per il sito. Include il nome di Id e il ruolo del ruolo (ad esempio, "Admin" o "Employee"). [Esempio](/dotnet/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a>Il livello di accesso ai dati

In questo argomento si presuppone che si ha familiarità con il meccanismo di persistenza che si intende usare e come creare le entità per questo meccanismo. In questo argomento non offre informazioni dettagliate su come creare il repository o classi di accesso ai dati; vengono forniti alcuni suggerimenti sulle decisioni di progettazione quando si lavora con ASP.NET Core Identity.

Si dispone di molta libertà quando si progetta il livello di accesso ai dati per un provider di archivio personalizzato. È sufficiente creare meccanismi di persistenza per le funzionalità che si intendono usare nell'app. Ad esempio, se non si usa i ruoli nell'app, non devi creare un archivio per i ruoli o le associazioni di ruolo utente. L'infrastruttura esistente e la tecnologia può richiedere una struttura che è molto diversa da un'implementazione predefinita di ASP.NET Core Identity. Nel livello di accesso ai dati, è fornire la logica da utilizzare con la struttura dell'implementazione di archiviazione.

Il livello di accesso ai dati fornisce la logica per salvare i dati di ASP.NET Core Identity a un'origine dati. Il livello di accesso ai dati per il provider di archiviazione personalizzato potrebbe includere le classi seguenti per archiviare le informazioni utente e il ruolo.

### <a name="context-class"></a>Context (classe)

Incapsula le informazioni per connettersi al meccanismo di persistenza ed eseguire query. Diverse classi di dati richiedono un'istanza di questa classe, in genere fornita tramite inserimento delle dipendenze. [Esempio](/dotnet/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1).

### <a name="user-storage"></a>Archiviazione utente

Archivia e recupera le informazioni utente (ad esempio hash della password e nome utente). [Esempio](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a>Archiviazione di ruolo

Archivia e recupera le informazioni sui ruoli (ad esempio, il nome del ruolo). [Esempio](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a>Archiviazione UserClaims

Archivia e recupera informazioni sulle attestazioni di utente (ad esempio il tipo di attestazione e il valore). [Esempio](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a>Accessi utente archiviazione

Archivia e recupera le informazioni di accesso utente (ad esempio un provider di autenticazione esterni). [Esempio](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a>Archiviazione UserRole

Archivia e recupera i ruoli assegnati a cui gli utenti. [Esempio](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

**Suggerimento:** implementare solo le classi si intende usare nell'app.

Nelle classi di accesso dati, fornire il codice per eseguire operazioni sui dati per il meccanismo di persistenza. Ad esempio, all'interno di un provider personalizzato, è possibile il codice seguente per creare un nuovo utente nel *archiviare* classe:

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

La logica di implementazione per la creazione dell'utente è nel `_usersTable.CreateAsync` metodo, illustrato di seguito.

## <a name="customize-the-user-class"></a>Personalizzare la classe utente

Quando si implementa un provider di archiviazione, creare una classe di utenti che corrispondono al [IdentityUser classe](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuser).

Come minimo, è necessario includere la classe utente un' `Id` e un `UserName` proprietà.

Il `IdentityUser` classe definisce le proprietà che la `UserManager` chiamate durante l'esecuzione di operazioni richieste. Il tipo predefinito del `Id` proprietà è una stringa, ma è possibile ereditare da `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` e specificare un tipo diverso. Il framework prevede l'implementazione di archiviazione per gestire le conversioni di tipo di dati.

## <a name="customize-the-user-store"></a>Personalizzare l'archivio dell'utente

Creare un `UserStore` classe che fornisce i metodi per tutte le operazioni di dati per l'utente. Questa classe è equivalente al [nello UserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1) classe. Nel `UserStore` classe, implementare `IUserStore<TUser>` e le interfacce facoltative necessarie. Si seleziona quali interfacce facoltative per implementare la funzionalità fornita nell'app in base.

### <a name="optional-interfaces"></a>Interfacce facoltative

* [IUserRoleStore](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1)
* [IUserClaimStore](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1)
* [IUserPasswordStore](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1)
* [IUserSecurityStampStore](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1)
* [IUserEmailStore](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1)
* [IPhoneNumberStore](/dotnet/api/microsoft.aspnetcore.identity.iphonenumberstore-1)
* [IQueryableUserStore](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1)
* [IUserLoginStore](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1)
* [IUserTwoFactorStore](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1)
* [IUserLockoutStore](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1)

Le interfacce facoltative ereditano `IUserStore<TUser>`. È possibile visualizzare archiviare in un utente di esempio implementata parzialmente il [app di esempio](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs).

All'interno di `UserStore` (classe), è utilizzare le classi di accesso di dati creato per eseguire operazioni. Questi vengono passati tramite inserimento delle dipendenze. Ad esempio, in SQL Server con Dapper implementazione, il `UserStore` classe ha la `CreateAsync` metodo che utilizza un'istanza di `DapperUsersTable` per inserire un nuovo record:

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Interfacce da implementare quando si personalizza l'archivio dell'utente

- **IUserStore**  
 Il [IUserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserstore-1) è l'unica interfaccia è necessario implementare nell'archivio dell'utente. Definisce i metodi per la creazione, aggiornamento, eliminazione e recupero degli utenti.
- **IUserClaimStore**  
 Il [IUserClaimStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1) interfaccia definisce i metodi implementati per abilitare attestazioni utente. Contiene i metodi per l'aggiunta, rimozione e recupera le attestazioni utente.
- **IUserLoginStore**  
 Il [IUserLoginStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1) definisce i metodi implementati per consentire ai provider di autenticazione esterni. Contiene i metodi per l'aggiunta, rimozione e il recupero di account di accesso utente e un metodo per il recupero di un utente in base a informazioni di accesso.
- **IUserRoleStore**  
 Il [IUserRoleStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1) interfaccia definisce i metodi implementati per eseguire il mapping di un utente a un ruolo. Contiene i metodi per aggiungere, rimuovere e recuperare i ruoli dell'utente e un metodo per verificare se un utente è assegnato a un ruolo.
- **IUserPasswordStore**  
 Il [IUserPasswordStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1) interfaccia definisce i metodi implementati per rendere persistenti le password con hash. Contiene i metodi per ottenere e impostare la password con hash e un metodo che indica se l'utente ha impostato una password.
- **IUserSecurityStampStore**  
 Il [IUserSecurityStampStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1) interfaccia definisce i metodi implementati per utilizzare un indicatore di sicurezza per indicare se le informazioni dell'utente account sono stato modificato. Questo indicatore viene aggiornato quando un utente modifica la password, o aggiunge o rimuove gli account di accesso. Contiene i metodi per ottenere e impostare l'indicatore di sicurezza.
- **IUserTwoFactorStore**  
 Il [IUserTwoFactorStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1) interfaccia definisce i metodi implementati per supportare l'autenticazione a due fattori. Contiene i metodi per ottenere e impostare se per un utente è abilitata l'autenticazione a due fattori.
- **IUserPhoneNumberStore**  
 Il [IUserPhoneNumberStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1) interfaccia definisce i metodi implementati per archiviare i numeri di telefono degli utenti. Contiene i metodi per ottenere e impostare il numero di telefono e indica se il numero di telefono è confermato.
- **IUserEmailStore**  
 Il [IUserEmailStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1) interfaccia definisce i metodi implementati per archiviare gli indirizzi di posta elettronica utente. Contiene i metodi per ottenere e impostare l'indirizzo di posta elettronica e indica se il messaggio di posta elettronica è confermato.
- **IUserLockoutStore**  
 Il [IUserLockoutStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1) interfaccia definisce i metodi implementati per archiviare le informazioni sul blocco di un account. Contiene i metodi per tenere traccia dei blocchi e i tentativi di accesso non riuscito.
- **IQueryableUserStore**  
 Il [IQueryableUserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1) interfaccia definisce i membri implementare per fornire un archivio utente disponibile per query.

È implementare solo le interfacce che sono necessari nell'app. Ad esempio:

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

Il `Microsoft.AspNet.Identity.EntityFramework` dello spazio dei nomi contiene le implementazioni del [IdentityUserClaim](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1), [IdentityUserLogin](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuserlogin), e [IdentityUserRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1) classi. Se si usano queste funzionalità, è possibile creare le versioni personalizzate di queste classi e definire le proprietà per l'app. Tuttavia, a volte è più efficiente carica tali entità in memoria durante l'esecuzione di operazioni di base (ad esempio aggiungendo o rimuovendo l'attestazione dell'utente). Al contrario, le classi di archiviazione back-end possono eseguire queste operazioni direttamente sull'origine dati. Ad esempio, il `UserStore.GetClaimsAsync` metodo può chiamare il `userClaimTable.FindByUserId(user.Id)` metodo per eseguire una query su tabella direttamente e restituiscono un elenco di attestazioni.

## <a name="customize-the-role-class"></a>Personalizzare la classe di ruoli

Quando si implementa un provider di archiviazione di ruolo, è possibile creare un tipo di ruolo personalizzate. Non è necessario implementare un'interfaccia specifica, ma deve essere un' `Id` e in genere avrà un `Name` proprietà.

Di seguito è riportata una classe ruolo di esempio:

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a>Personalizzare l'archivio di ruolo

È possibile creare un `RoleStore` classe che fornisce i metodi per tutte le operazioni di dati sui ruoli. Questa classe è equivalente al [oggetto RoleStore&lt;TRole&gt; ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1) classe. Nel `RoleStore` (classe), si implementa il `IRoleStore<TRole>` e facoltativamente il `IQueryableRoleStore<TRole>` interfaccia.

- **IRoleStore&lt;TRole&gt;**  
 Il [IRoleStore&lt;TRole&gt; ](/dotnet/api/microsoft.aspnetcore.identity.irolestore-1) interfaccia definisce i metodi da implementare nella classe archivio ruoli. Contiene i metodi per la creazione, aggiornamento, eliminazione e recupero dei ruoli.
- **RoleStore&lt;TRole&gt;**  
 Per personalizzare `RoleStore`, creare una classe che implementa il `IRoleStore<TRole>` interfaccia. 

## <a name="reconfigure-app-to-use-a-new-storage-provider"></a>Riconfigurare l'app per l'uso di un nuovo provider di archiviazione

Dopo aver implementato un provider di archiviazione, configurare l'app per usarla. Se l'app Usa il provider predefinito, sostituirlo con il provider personalizzato.

1. Rimuovere il `Microsoft.AspNetCore.EntityFramework.Identity` pacchetto NuGet.
1. Se il provider di archiviazione si trova in un progetto separato o un pacchetto, aggiungere un riferimento a esso.
1. Sostituire tutti i riferimenti a `Microsoft.AspNetCore.EntityFramework.Identity` con un'istruzione per lo spazio dei nomi del provider di archiviazione.
1. Nel `ConfigureServices` metodo, cambia il `AddIdentity` metodo da usare tipi personalizzati. È possibile creare i propri metodi di estensione per questo scopo. Visualizzare [IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) per un esempio.
1. Se si usano i ruoli, aggiornare il `RoleManager` usare il `RoleStore` classe.
1. Aggiornare la stringa di connessione e le credenziali per la configurazione dell'app.

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

- [Provider di archiviazione personalizzati per ASP.NET 4.x Identity](/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
- [ASP.NET Core Identity](https://github.com/aspnet/identity) -questo repository sono inclusi collegamenti alla community mantenuto i provider dell'archivio.
