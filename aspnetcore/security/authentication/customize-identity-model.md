---
title: Personalizzazione del modello in ASP.NET Core Identity
author: ajcvickers
description: Questo articolo descrive come personalizzare il modello di dati Entity Framework Core sottostante per ASP.NET Core Identity.
ms.author: avickers
ms.date: 04/24/2019
uid: security/authentication/customize_identity_model
ms.openlocfilehash: ae5f4567a8921ce277cd6153f37a5558bcf4e261
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897858"
---
# <a name="identity-model-customization-in-aspnet-core"></a>Personalizzazione del modello in ASP.NET Core Identity

Da [Arthur Vickers](https://github.com/ajcvickers)

ASP.NET Core Identity offre un framework per la gestione e l'archiviazione di account utente nelle App ASP.NET Core. Identità viene aggiunto al progetto quando **singoli account utente di** è selezionato come meccanismo di autenticazione. Per impostazione predefinita, identità rende l'uso di un Entity Framework (EF) modello di dati principale. Questo articolo descrive come personalizzare il modello di identità.

## <a name="identity-and-ef-core-migrations"></a>Identità e le migrazioni di EF Core

Prima di esaminare il modello, è utile comprendere il funzionamento con Identity [migrazioni EF Core](/ef/core/managing-schemas/migrations/) per creare e aggiornare un database. Al livello superiore, il processo è:

1. Definire o aggiornare una [modello di dati nel codice](/ef/core/modeling/).
1. Aggiungere una migrazione per questo modello si traducono in modifiche che possono essere applicate al database.
1. Verificare che la migrazione rappresenta in modo corretto le proprie intenzioni.
1. Applicare la migrazione per aggiornare il database siano sincronizzate con il modello.
1. Ripetere i passaggi da 1 a 4 per un'ulteriore ridefinire il modello e mantenere sincronizzato il database.

Per aggiungere e applicare le migrazioni, usare uno degli approcci seguenti:

* Il **Console di gestione pacchetti** finestra se si usa Visual Studio. Per altre informazioni, vedere [strumenti di Entity Framework Core PMC](/ef/core/miscellaneous/cli/powershell).
* La CLI di .NET Core se si usa la riga di comando. Per altre informazioni, vedere [strumenti da riga di comando .NET Core EF](/ef/core/miscellaneous/cli/dotnet).
* Facendo clic sui **applicare le migrazioni** pulsante nella pagina di errore quando si esegue l'app.

ASP.NET Core include un gestore di pagina di errore in fase di sviluppo. Il gestore di è possibile applicare le migrazioni quando l'app viene eseguita. Le app di produzione in genere generano gli script SQL dalle migrazioni e distribuiscono le modifiche del database come parte di un'app controllata e la distribuzione del database.

Quando viene creata una nuova app con identità, hanno già completati i passaggi 1 e 2 sopra. Vale a dire, il modello di dati iniziale esiste già e la migrazione iniziale è stato aggiunto al progetto. La migrazione iniziale deve comunque essere applicati al database. La migrazione iniziale può essere applicata tramite uno degli approcci seguenti:

* Eseguire `Update-Database` nella console di gestione pacchetti.
* Eseguire `dotnet ef database update` in una shell dei comandi.
* Scegliere il **applicare le migrazioni** pulsante nella pagina di errore quando si esegue l'app.

Ripetere i passaggi precedenti quando vengono apportate modifiche al modello.

## <a name="the-identity-model"></a>Il modello di identità

### <a name="entity-types"></a>Tipi di entità

Il modello di identità è costituita da tipi di entità seguente.

|Tipo di entità|Descrizione                                                  |
|-----------|-------------------------------------------------------------|
|`User`     |Rappresenta l'utente.                                         |
|`Role`     |Rappresenta un ruolo.                                           |
|`UserClaim`|Rappresenta un'attestazione che dispone di un utente.                    |
|`UserToken`|Rappresenta un token di autenticazione per un utente.               |
|`UserLogin`|Associa un utente con un account di accesso.                              |
|`RoleClaim`|Rappresenta un'attestazione che viene concesso a tutti gli utenti all'interno di un ruolo.|
|`UserRole` |Un'entità di join che associano gli utenti e ruoli.               |

### <a name="entity-type-relationships"></a>Relazioni di tipo di entità

Il [tipi di entità](#entity-types) sono correlati tra loro nei modi seguenti:

* Ciascuna `User` può avere numerosi `UserClaims`.
* Ciascuna `User` può avere numerosi `UserLogins`.
* Ciascuna `User` può avere numerosi `UserTokens`.
* Ciascuna `Role` può avere numerosi associati `RoleClaims`.
* Ciascuna `User` può avere numerosi associata `Roles`e ogni `Role` può essere associato a molti `Users`. Questa è una relazione molti-a-molti che richiede una tabella di join del database. La tabella di join è rappresentata dal `UserRole` entità.

### <a name="default-model-configuration"></a>Configurazione del modello predefinito

Identità definisce molti *le classi del contesto* che eredita dalla <xref:Microsoft.EntityFrameworkCore.DbContext> per configurare e usare il modello. Questa configurazione viene eseguita usando il [API Fluent primo codice di Entity Framework Core](/ef/core/modeling/) nel <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating*> metodo della classe del contesto. La configurazione predefinita è:

```csharp
builder.Entity<TUser>(b =>
{
    // Primary key
    b.HasKey(u => u.Id);

    // Indexes for "normalized" username and email, to allow efficient lookups
    b.HasIndex(u => u.NormalizedUserName).HasName("UserNameIndex").IsUnique();
    b.HasIndex(u => u.NormalizedEmail).HasName("EmailIndex");

    // Maps to the AspNetUsers table
    b.ToTable("AspNetUsers");

    // A concurrency token for use with the optimistic concurrency checking
    b.Property(u => u.ConcurrencyStamp).IsConcurrencyToken();

    // Limit the size of columns to use efficient database types
    b.Property(u => u.UserName).HasMaxLength(256);
    b.Property(u => u.NormalizedUserName).HasMaxLength(256);
    b.Property(u => u.Email).HasMaxLength(256);
    b.Property(u => u.NormalizedEmail).HasMaxLength(256);

    // The relationships between User and other entity types
    // Note that these relationships are configured with no navigation properties

    // Each User can have many UserClaims
    b.HasMany<TUserClaim>().WithOne().HasForeignKey(uc => uc.UserId).IsRequired();

    // Each User can have many UserLogins
    b.HasMany<TUserLogin>().WithOne().HasForeignKey(ul => ul.UserId).IsRequired();

    // Each User can have many UserTokens
    b.HasMany<TUserToken>().WithOne().HasForeignKey(ut => ut.UserId).IsRequired();

    // Each User can have many entries in the UserRole join table
    b.HasMany<TUserRole>().WithOne().HasForeignKey(ur => ur.UserId).IsRequired();
});

builder.Entity<TUserClaim>(b =>
{
    // Primary key
    b.HasKey(uc => uc.Id);

    // Maps to the AspNetUserClaims table
    b.ToTable("AspNetUserClaims");
});

builder.Entity<TUserLogin>(b =>
{
    // Composite primary key consisting of the LoginProvider and the key to use
    // with that provider
    b.HasKey(l => new { l.LoginProvider, l.ProviderKey });

    // Limit the size of the composite key columns due to common DB restrictions
    b.Property(l => l.LoginProvider).HasMaxLength(128);
    b.Property(l => l.ProviderKey).HasMaxLength(128);

    // Maps to the AspNetUserLogins table
    b.ToTable("AspNetUserLogins");
});

builder.Entity<TUserToken>(b =>
{
    // Composite primary key consisting of the UserId, LoginProvider and Name
    b.HasKey(t => new { t.UserId, t.LoginProvider, t.Name });

    // Limit the size of the composite key columns due to common DB restrictions
    b.Property(t => t.LoginProvider).HasMaxLength(maxKeyLength);
    b.Property(t => t.Name).HasMaxLength(maxKeyLength);

    // Maps to the AspNetUserTokens table
    b.ToTable("AspNetUserTokens");
});

builder.Entity<TRole>(b =>
{
    // Primary key
    b.HasKey(r => r.Id);

    // Index for "normalized" role name to allow efficient lookups
    b.HasIndex(r => r.NormalizedName).HasName("RoleNameIndex").IsUnique();

    // Maps to the AspNetRoles table
    b.ToTable("AspNetRoles");

    // A concurrency token for use with the optimistic concurrency checking
    b.Property(r => r.ConcurrencyStamp).IsConcurrencyToken();

    // Limit the size of columns to use efficient database types
    b.Property(u => u.Name).HasMaxLength(256);
    b.Property(u => u.NormalizedName).HasMaxLength(256);

    // The relationships between Role and other entity types
    // Note that these relationships are configured with no navigation properties

    // Each Role can have many entries in the UserRole join table
    b.HasMany<TUserRole>().WithOne().HasForeignKey(ur => ur.RoleId).IsRequired();

    // Each Role can have many associated RoleClaims
    b.HasMany<TRoleClaim>().WithOne().HasForeignKey(rc => rc.RoleId).IsRequired();
});

builder.Entity<TRoleClaim>(b =>
{
    // Primary key
    b.HasKey(rc => rc.Id);

    // Maps to the AspNetRoleClaims table
    b.ToTable("AspNetRoleClaims");
});

builder.Entity<TUserRole>(b =>
{
    // Primary key
    b.HasKey(r => new { r.UserId, r.RoleId });

    // Maps to the AspNetUserRoles table
    b.ToTable("AspNetUserRoles");
});
```

### <a name="model-generic-types"></a>Tipi generici di modello

Identità definisce predefinito [Common Language Runtime](/dotnet/standard/glossary#clr) tipi (CLR) per ognuno dei tipi di entità elencati in precedenza. Questi tipi sono precedute tutte dal prefisso *identità*:

* `IdentityUser`
* `IdentityRole`
* `IdentityUserClaim`
* `IdentityUserToken`
* `IdentityUserLogin`
* `IdentityRoleClaim`
* `IdentityUserRole`

Invece di usare direttamente questi tipi, i tipi sono utilizzabile come classi di base per i tipi dell'app. Il `DbContext` classi definite dall'identità sono generiche, tale che i tipi CLR diversi possono essere utilizzati per una o più dei tipi di entità nel modello. Questi tipi generici consentono anche di `User` (PK) dei dati tipo di chiave primaria da modificare.

Quando si usa l'identità con il supporto per i ruoli, un <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext> classe deve essere usata. Ad esempio:

```csharp
// Uses all the built-in Identity types
// Uses `string` as the key type
public class IdentityDbContext
    : IdentityDbContext<IdentityUser, IdentityRole, string>
{
}

// Uses the built-in Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityDbContext<TUser>
    : IdentityDbContext<TUser, IdentityRole, string>
        where TUser : IdentityUser
{
}

// Uses the built-in Identity types except with custom User and Role types
// The key type is defined by TKey
public class IdentityDbContext<TUser, TRole, TKey> : IdentityDbContext<
    TUser, TRole, TKey, IdentityUserClaim<TKey>, IdentityUserRole<TKey>,
    IdentityUserLogin<TKey>, IdentityRoleClaim<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TRole : IdentityRole<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments
// The key type is defined by TKey
public abstract class IdentityDbContext<
    TUser, TRole, TKey, TUserClaim, TUserRole, TUserLogin, TRoleClaim, TUserToken>
    : IdentityUserContext<TUser, TKey, TUserClaim, TUserLogin, TUserToken>
         where TUser : IdentityUser<TKey>
         where TRole : IdentityRole<TKey>
         where TKey : IEquatable<TKey>
         where TUserClaim : IdentityUserClaim<TKey>
         where TUserRole : IdentityUserRole<TKey>
         where TUserLogin : IdentityUserLogin<TKey>
         where TRoleClaim : IdentityRoleClaim<TKey>
         where TUserToken : IdentityUserToken<TKey>
```

È anche possibile usare l'identità senza ruoli (solo attestazioni), nel qual caso un <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext%601> classe deve essere utilizzata:

```csharp
// Uses the built-in non-role Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityUserContext<TUser>
    : IdentityUserContext<TUser, string>
        where TUser : IdentityUser
{
}

// Uses the built-in non-role Identity types except with a custom User type
// The key type is defined by TKey
public class IdentityUserContext<TUser, TKey> : IdentityUserContext<
    TUser, TKey, IdentityUserClaim<TKey>, IdentityUserLogin<TKey>,
    IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments, with no roles
// The key type is defined by TKey
public abstract class IdentityUserContext<
    TUser, TKey, TUserClaim, TUserLogin, TUserToken> : DbContext
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
        where TUserClaim : IdentityUserClaim<TKey>
        where TUserLogin : IdentityUserLogin<TKey>
        where TUserToken : IdentityUserToken<TKey>
{
}
```

## <a name="customize-the-model"></a>Personalizzare il modello

Il punto di partenza per la personalizzazione del modello consiste nel derivare dal tipo di contesto appropriate. Vedere le [modellare tipi generici](#model-generic-types) sezione. Questo tipo di contesto viene abitualmente chiamato `ApplicationDbContext` e viene creato tramite i modelli ASP.NET Core.

Il contesto viene utilizzato per configurare il modello in due modi:

* Fornendo entità e tipi di chiave per i parametri di tipo generico.
* Si esegue l'override `OnModelCreating` per modificare il mapping di questi tipi.

Quando si esegue l'override `OnModelCreating`, `base.OnModelCreating` deve essere chiamato per primo; la configurazione di override deve essere chiamata successivamente. EF Core ha in genere un criterio last-quello-wins per la configurazione. Ad esempio, se il `ToTable` per un tipo di entità viene chiamato prima di tutto con il nome di una tabella e quindi nuovamente in seguito con un nome di tabella diversa, il nome della tabella nella seconda chiamata verrà usato.

### <a name="custom-user-data"></a>Dati utente personalizzati

<!--
set projNam=WebApp1
dotnet new webapp -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design 
dotnet aspnet-codegenerator identity  -dc ApplicationDbContext --useDefaultUI 
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
 -->

[Dati utente personalizzati](xref:security/authentication/add-user-data) è supportata tramite l'eredità dalla `IdentityUser`. Solitamente si per assegnare un nome di questo tipo `ApplicationUser`:

```csharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

Usare il `ApplicationUser` tipo come argomento generico per il contesto:

```csharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder builder)
    {
        base.OnModelCreating(builder);
    }
}
```

Non è necessario eseguire l'override `OnModelCreating` nella `ApplicationDbContext` classe. Esegue il mapping di Entity Framework Core le `CustomTag` proprietà in base alla convenzione. Tuttavia, il database deve essere aggiornato per creare un nuovo `CustomTag` colonna. Per creare la colonna, aggiungere una migrazione e quindi aggiornare il database come descritto in [identità e le migrazioni di EF Core](#identity-and-ef-core-migrations).

Update *Pages/Shared/_LoginPartial.cshtml* e sostituire `IdentityUser` con `ApplicationUser`:

```
@using Microsoft.AspNetCore.Identity
@using WebApp1.Areas.Identity.Data
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager
```

Update *Areas/Identity/IdentityHostingStartup.cs* oppure `Startup.ConfigureServices` e sostituire `IdentityUser` con `ApplicationUser`.

```csharp
services.AddDefaultIdentity<ApplicationUser>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultUI();
```

In ASP.NET Core 2.1 o versioni successive, l'identità viene fornita come una libreria di classi Razor. Per altre informazioni, vedere <xref:security/authentication/scaffold-identity>. Di conseguenza, il codice precedente richiede una chiamata a <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Se l'utilità di scaffolding di identità è stata usata per aggiungere i file di identità per il progetto, rimuovere la chiamata a `AddDefaultUI`. Per altre informazioni, vedere:

* [Scaffolding dell'identità](xref:security/authentication/scaffold-identity)
* [Aggiungere, scaricare ed eliminare i dati utente personalizzati per Identity](xref:security/authentication/add-user-data)

### <a name="change-the-primary-key-type"></a>Modificare il tipo di chiave primaria

Una modifica al tipo di dati della colonna chiave primaria dopo aver creato il database rappresenta un problema in molti sistemi di database. La modifica la chiave primaria in genere implica eliminare e ricreare la tabella. Di conseguenza, i tipi chiave devono essere specificati per la migrazione iniziale quando viene creato il database.

Seguire questi passaggi per modificare il tipo di chiave primaria:

1. Se il database è stato creato prima della modifica della chiave primaria, eseguire `Drop-Database` (console di gestione pacchetti) o `dotnet ef database drop` (CLI di .NET Core) per eliminarlo.
2. Dopo la conferma dell'eliminazione del database, rimuovere la migrazione iniziale con `Remove-Migration` (console di gestione pacchetti) o `dotnet ef migrations remove` (CLI di .NET Core).
3. Aggiorna il `ApplicationDbContext` classe derivandola da <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext%603>. Specificare il nuovo tipo di chiave per `TKey`. Ad esempio, per usare un `Guid` tipo di chiave:

    ```csharp
    public class ApplicationDbContext
        : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
    ```

    ::: moniker range=">= aspnetcore-2.0"

    Nel codice precedente, le classi generiche <xref:Microsoft.AspNetCore.Identity.IdentityUser%601> e <xref:Microsoft.AspNetCore.Identity.IdentityRole%601> devono essere specificate per usare il nuovo tipo di chiave.

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    Nel codice precedente, le classi generiche <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser%601> e <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole%601> devono essere specificate per usare il nuovo tipo di chiave.

    ::: moniker-end

    `Startup.ConfigureServices` deve essere aggiornato per usare l'utente generico:

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<IdentityUser<Guid>>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<IdentityUser<Guid>, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<IdentityUser<Guid>, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    ::: moniker-end

4. Se una classe personalizzata `ApplicationUser` classe viene utilizzata, aggiornare la classe da cui ereditare `IdentityUser`. Ad esempio:

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Models/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    ::: moniker range=">= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    Update `ApplicationDbContext` a cui fare riferimento l'oggetto personalizzato `ApplicationUser` classe:

    ```csharp
    public class ApplicationDbContext
        : IdentityDbContext<ApplicationUser, IdentityRole<Guid>, Guid>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
    ```

    Registrare la classe del contesto del database personalizzate quando si aggiunge il servizio di identità in `Startup.ConfigureServices`:

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<ApplicationUser>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultUI()
            .AddDefaultTokenProviders();
    ```

    Tipo di dati della chiave primaria viene dedotto analizzando il <xref:Microsoft.EntityFrameworkCore.DbContext> oggetto.

    In ASP.NET Core 2.1 o versioni successive, l'identità viene fornita come una libreria di classi Razor. Per altre informazioni, vedere <xref:security/authentication/scaffold-identity>. Di conseguenza, il codice precedente richiede una chiamata a <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Se l'utilità di scaffolding di identità è stata usata per aggiungere i file di identità per il progetto, rimuovere la chiamata a `AddDefaultUI`.

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    Tipo di dati della chiave primaria viene dedotto analizzando il <xref:Microsoft.EntityFrameworkCore.DbContext> oggetto.

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    Il <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> metodo accetta un `TKey` tipo che indica il tipo di dati della chiave primaria.

    ::: moniker-end

5. Se una classe personalizzata `ApplicationRole` classe viene utilizzata, aggiornare la classe da cui ereditare `IdentityRole<TKey>`. Ad esempio:

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationRole.cs?name=snippet_ApplicationRole&highlight=4)]

    Update `ApplicationDbContext` a cui fare riferimento l'oggetto personalizzato `ApplicationRole` classe. Ad esempio, la classe seguente fa riferimento a una classe personalizzata `ApplicationUser` e un oggetto personalizzato `ApplicationRole`:

    ::: moniker range=">= aspnetcore-2.1"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Registrare la classe del contesto del database personalizzate quando si aggiunge il servizio di identità in `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=13-16)]

    Tipo di dati della chiave primaria viene dedotto analizzando il <xref:Microsoft.EntityFrameworkCore.DbContext> oggetto.

    In ASP.NET Core 2.1 o versioni successive, l'identità viene fornita come una libreria di classi Razor. Per altre informazioni, vedere <xref:security/authentication/scaffold-identity>. Di conseguenza, il codice precedente richiede una chiamata a <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Se l'utilità di scaffolding di identità è stata usata per aggiungere i file di identità per il progetto, rimuovere la chiamata a `AddDefaultUI`.

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Registrare la classe del contesto del database personalizzate quando si aggiunge il servizio di identità in `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    Tipo di dati della chiave primaria viene dedotto analizzando il <xref:Microsoft.EntityFrameworkCore.DbContext> oggetto.

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Registrare la classe del contesto del database personalizzate quando si aggiunge il servizio di identità in `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    Il <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> metodo accetta un `TKey` tipo che indica il tipo di dati della chiave primaria.

    ::: moniker-end

### <a name="add-navigation-properties"></a>Aggiungere le proprietà di navigazione

Modifica della configurazione del modello per le relazioni possa essere più difficile apportare altre modifiche. Prestare attenzione a sostituire le relazioni esistenti anziché crearne uno nuovo, relazioni aggiuntive. In particolare, la relazione modificata deve specificare stessa proprietà della chiave esterna (FK) come la relazione esistente. Ad esempio, la relazione tra `Users` e `UserClaims` è, per impostazione predefinita, specificata come indicato di seguito:

```csharp
builder.Entity<TUser>(b =>
{
    // Each User can have many UserClaims
    b.HasMany<TUserClaim>()
     .WithOne()
     .HasForeignKey(uc => uc.UserId)
     .IsRequired();
});
```

La chiave esterna per questa relazione viene specificata come il `UserClaim.UserId` proprietà. `HasMany` e `WithOne` vengono chiamati senza argomenti per creare la relazione senza proprietà di navigazione.

Aggiungere una proprietà di navigazione `ApplicationUser` che consente l'associazione `UserClaims` per farvi riferimento da parte dell'utente:

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

Il `TKey` per `IdentityUserClaim<TKey>` è del tipo specificato per la chiave primaria degli utenti. In questo caso `TKey` è `string` perché vengono utilizzati i valori predefiniti. Dispone **non** il tipo di chiave primaria per la `UserClaim` tipo di entità.

Ora che la proprietà di navigazione è presente, deve essere configurato `OnModelCreating`:

```csharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();
        });
    }
}
```

Si noti che è configurata relazione esattamente com'era prima, solo con una proprietà di navigazione specificata nella chiamata a `HasMany`.

Le proprietà di navigazione esistono solo nel modello di Entity Framework, non il database. Poiché la chiave esterna per la relazione non è stato modificato, questo tipo di modifica del modello non richiede il database da aggiornare. Ciò può essere controllato mediante l'aggiunta di una migrazione dopo aver apportato la modifica. Il `Up` e `Down` metodi sono vuoti.

### <a name="add-all-user-navigation-properties"></a>Aggiungere tutte le proprietà di navigazione di utente

Come prassi consigliata, usando la sezione precedente, l'esempio seguente configura le proprietà di navigazione unidirezionale per tutte le relazioni di utente:

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<IdentityUserRole<string>> UserRoles { get; set; }
}
```

```csharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne()
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne()
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne()
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });
    }
}
```

### <a name="add-user-and-role-navigation-properties"></a>Aggiungere le proprietà di navigazione utenti e ruoli

Come prassi consigliata, usando la sezione precedente, l'esempio seguente configura le proprietà di navigazione per tutte le relazioni per l'utente e ruolo:

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationRole : IdentityRole
{
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationUserRole : IdentityUserRole<string>
{
    public virtual ApplicationUser User { get; set; }
    public virtual ApplicationRole Role { get; set; }
}
```

```csharp
public class ApplicationDbContext
    : IdentityDbContext<
        ApplicationUser, ApplicationRole, string,
        IdentityUserClaim<string>, ApplicationUserRole, IdentityUserLogin<string>,
        IdentityRoleClaim<string>, IdentityUserToken<string>>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne()
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne()
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.User)
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });

        modelBuilder.Entity<ApplicationRole>(b =>
        {
            // Each Role can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.Role)
                .HasForeignKey(ur => ur.RoleId)
                .IsRequired();
        });

    }
}
```

Note:

* Questo esempio include anche il `UserRole` aggiungere entità, che è necessario per esplorare la relazione molti-a-molti utenti ai ruoli.
* Ricordarsi di modificare i tipi delle proprietà di navigazione ciò sia rispecchiato `ApplicationXxx` vengono ora utilizzati tipi anziché `IdentityXxx` tipi.
* Ricordarsi di usare la `ApplicationXxx` nel ma generic `ApplicationContext` definizione.

### <a name="add-all-navigation-properties"></a>Aggiungere tutte le proprietà di navigazione

Come prassi consigliata, usando la sezione precedente, l'esempio seguente configura le proprietà di navigazione per tutte le relazioni in tutti i tipi di entità:

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<ApplicationUserClaim> Claims { get; set; }
    public virtual ICollection<ApplicationUserLogin> Logins { get; set; }
    public virtual ICollection<ApplicationUserToken> Tokens { get; set; }
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationRole : IdentityRole
{
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
    public virtual ICollection<ApplicationRoleClaim> RoleClaims { get; set; }
}

public class ApplicationUserRole : IdentityUserRole<string>
{
    public virtual ApplicationUser User { get; set; }
    public virtual ApplicationRole Role { get; set; }
}

public class ApplicationUserClaim : IdentityUserClaim<string>
{
    public virtual ApplicationUser User { get; set; }
}

public class ApplicationUserLogin : IdentityUserLogin<string>
{
    public virtual ApplicationUser User { get; set; }
}

public class ApplicationRoleClaim : IdentityRoleClaim<string>
{
    public virtual ApplicationRole Role { get; set; }
}

public class ApplicationUserToken : IdentityUserToken<string>
{
    public virtual ApplicationUser User { get; set; }
}
```

```csharp
public class ApplicationDbContext
    : IdentityDbContext<
        ApplicationUser, ApplicationRole, string,
        ApplicationUserClaim, ApplicationUserRole, ApplicationUserLogin,
        ApplicationRoleClaim, ApplicationUserToken>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne(e => e.User)
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne(e => e.User)
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne(e => e.User)
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.User)
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });

        modelBuilder.Entity<ApplicationRole>(b =>
        {
            // Each Role can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.Role)
                .HasForeignKey(ur => ur.RoleId)
                .IsRequired();

            // Each Role can have many associated RoleClaims
            b.HasMany(e => e.RoleClaims)
                .WithOne(e => e.Role)
                .HasForeignKey(rc => rc.RoleId)
                .IsRequired();
        });
    }
}
```

### <a name="use-composite-keys"></a>Utilizzare chiavi composte

Nelle sezioni precedenti è illustrata la modifica del tipo della chiave utilizzata nel modello di identità. La modifica del modello di chiave di identità per usare una chiave composta non è supportata o consigliata. Uso di una chiave composta con identità comporta la modifica come il codice di Identity manager interagisce con il modello. Questa personalizzazione esula dall'ambito di questo documento.

### <a name="change-tablecolumn-names-and-facets"></a>Modificare i nomi di tabella/colonna e i facet

Per modificare i nomi delle tabelle e colonne, chiamare `base.OnModelCreating`. Aggiungere quindi della configurazione per eseguire l'override dei valori predefiniti. Ad esempio, per modificare il nome di tutte le tabelle di identità:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.ToTable("MyUsers");
    });

    modelBuilder.Entity<IdentityUserClaim<string>>(b =>
    {
        b.ToTable("MyUserClaims");
    });

    modelBuilder.Entity<IdentityUserLogin<string>>(b =>
    {
        b.ToTable("MyUserLogins");
    });

    modelBuilder.Entity<IdentityUserToken<string>>(b =>
    {
        b.ToTable("MyUserTokens");
    });

    modelBuilder.Entity<IdentityRole>(b =>
    {
        b.ToTable("MyRoles");
    });

    modelBuilder.Entity<IdentityRoleClaim<string>>(b =>
    {
        b.ToTable("MyRoleClaims");
    });

    modelBuilder.Entity<IdentityUserRole<string>>(b =>
    {
        b.ToTable("MyUserRoles");
    });
}
```

Questi esempi utilizzano i tipi di identità predefinito. Se si usa un tipo di app, ad esempio `ApplicationUser`, configurare quel tipo anziché al tipo predefinito.

L'esempio seguente modifica alcuni nomi di colonna:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.Property(e => e.Email).HasColumnName("EMail");
    });

    modelBuilder.Entity<IdentityUserClaim<string>>(b =>
    {
        b.Property(e => e.ClaimType).HasColumnName("CType");
        b.Property(e => e.ClaimValue).HasColumnName("CValue");
    });
}
```

Alcuni tipi di colonne del database possono essere configurati con determinate *facet* (ad esempio, il valore massimo `string` lunghezza consentita). L'esempio seguente imposta lunghezza massima delle colonne per diversi `string` proprietà nel modello:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.Property(u => u.UserName).HasMaxLength(128);
        b.Property(u => u.NormalizedUserName).HasMaxLength(128);
        b.Property(u => u.Email).HasMaxLength(128);
        b.Property(u => u.NormalizedEmail).HasMaxLength(128);
    });

    modelBuilder.Entity<IdentityUserToken<string>>(b =>
    {
        b.Property(t => t.LoginProvider).HasMaxLength(128);
        b.Property(t => t.Name).HasMaxLength(128);
    });
}
```

### <a name="map-to-a-different-schema"></a>Eseguire il mapping a uno schema diverso

Gli schemi possono avere un comportamento diverso tra i provider di database. Per SQL Server, il valore predefinito consiste nel creare tutte le tabelle di *dbo* dello schema. È possibile creare le tabelle in uno schema diverso. Ad esempio:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

::: moniker range=">= aspnetcore-2.1"

### <a name="lazy-loading"></a>Caricamento lazy

In questa sezione viene aggiunto il supporto per i proxy di caricamento lazy nel modello di identità. Il caricamento lazy è utile perché consente le proprietà di navigazione da utilizzare senza prima verificare che sta caricati.

I tipi di entità possono essere reso adatti per caricamento lazy in diversi modi, come descritto nel [documentazione di Entity Framework Core](/ef/core/querying/related-data#lazy-loading). Per semplicità, usare un proxy di caricamento lazy, che richiede:

* Installazione del [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) pacchetto.
* Una chiamata a <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*> all'interno di <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*>.
* Tipi di entità pubbliche con `public virtual` le proprietà di navigazione.

Nell'esempio seguente viene illustrato come chiamare `UseLazyLoadingProxies` in `Startup.ConfigureServices`:

```csharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
              .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Vedere gli esempi precedenti, per indicazioni su come aggiungere le proprietà di navigazione per i tipi di entità.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:security/authentication/scaffold-identity>

::: moniker-end
