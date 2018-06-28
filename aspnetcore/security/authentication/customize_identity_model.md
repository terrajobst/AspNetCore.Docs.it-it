---
title: Personalizzazione del modello di identità
author: ajcvickers
description: In questo articolo viene descritto come personalizzare il modello di dati Entity Framework Core sottostante per ASP.NET Identity Core.
ms.author: avickers
ms.date: 04/12/2018
ms.prod: asp.net-core
uid: security/authentication/customize_identity_model
ms.openlocfilehash: b44b4fd0f24d245b969588a7226ea6aacbe2a722
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036894"
---
# <a name="identity-model-customization"></a>Personalizzazione del modello di identità

Da [Arthur Vickers](https://github.com/ajcvickers)

Identità di ASP.NET Core fornisce un framework per gestire e archiviare gli account utente nelle applicazioni ASP.NET Core. Identità viene aggiunto al progetto quando "Singoli account utente di" sia selezionato come il meccanismo di autenticazione. Per impostazione predefinita, identità, utilizzerà di un Entity Framework (EF) modello di dati principale. In questo articolo viene descritto come personalizzare il modello di identità.

<a name="identity-migrations"></a>

## <a name="identity-and-ef-core-migrations"></a>Identità e le migrazioni dei componenti di base di Entity Framework

Prima di esaminare il modello, è utile comprendere il funzionamento con identità [EF Core migrazioni](/ef/core/managing-schemas/migrations/) per creare e aggiornare un database. Al primo livello, il processo è:

1. Definire o aggiornare una [modello di dati nel codice](/ef/core/modeling/).
1. Aggiungere una migrazione per tradurre le modifiche che possono essere applicate al database di questo modello.
1. Verificare che la migrazione rappresenta correttamente intenzioni.
1. Applicare la migrazione per aggiornare il database per essere sincronizzato con il modello.
1. Ripetere i passaggi da 1 a 4 per ulteriormente ridefinire il modello e mantenere sincronizzati il database.

Le migrazioni vengono aggiunti e applicata utilizzando:

* Il [Console di gestione pacchetti](/ef/core/miscellaneous/cli/powershell) (PMC) se si utilizza Visual Studio.
* Il [dotnet comandi](/ef/core/miscellaneous/cli/dotnet) nella riga di comando.
* Quando si seleziona il collegamento di migrazioni nella pagina di errore quando l'applicazione viene eseguita.

ASP.NET Core dispone di un gestore di pagina di errore in fase di sviluppo che può essere usato per applicare le migrazioni quando l'applicazione viene eseguita. Per le applicazioni di produzione, è spesso più appropriato per generare gli script SQL dalle migrazioni e distribuirle nel database come parte di una distribuzione di applicazioni e database controllata.

Quando viene creata una nuova applicazione utilizzando l'identità, i passaggi 1 e 2 sopra sono già stati completati. Vale a dire, il modello di dati iniziale esiste già, e la migrazione iniziale è stato aggiunto al progetto. La migrazione iniziale deve ancora essere applicato al database. La migrazione iniziale può essere eseguita tramite l'esecuzione di Update-Database (PMC), il comando update (.NET Core CLI) database ef dotnet, o utilizzando la pagina di errore quando l'applicazione viene eseguita.

I passaggi precedenti devono essere ripetuti quando vengono apportate modifiche al modello.

<a name="identity-model"></a>

## <a name="the-identity-model"></a>Il modello di identità

### <a name="entity-types"></a>Tipi di entità

Il modello di identità è costituita da sette tipi di entità:

* `User` -rappresenta l'utente
* `Role` -rappresenta un ruolo
* `UserClaim` -rappresenta un'attestazione che deve disporre di un utente
* `UserToken` -rappresenta un token di autenticazione per un utente
* `UserLogin` -Associa un utente con un account di accesso
* `RoleClaim` -rappresenta un'attestazione che viene concesso a tutti gli utenti all'interno di un ruolo
* `UserRole` -aggiungere entità che associa gli utenti e ruoli

### <a name="entity-type-relationships"></a>Relazioni di tipo di entità

Questi tipi di entità sono correlati tra loro nei modi seguenti:

* Ogni `User` può avere numerosi `UserClaims`
* Ogni `User` può avere numerosi `UserLogins`
* Ogni `User` può avere numerosi `UserTokens`
* Ogni `Role` può avere numerosi associata `RoleClaims`
* Ogni `User` può avere numerosi associata `Roles`e ogni `Role` può essere associato a molti utenti
  * Si tratta di una relazione molti-a-molti, che richiede una tabella di join del database. La tabella di join è rappresentata dal `UserRole` entità.

### <a name="default-model-configuration"></a>Configurazione del modello predefinito

Identità definisce una varietà di "classi di contesto" che ereditano da [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) per configurare e utilizzare il modello. Questa configurazione viene eseguita utilizzando il [EF Core codice prima Fluent API](/ef/core/modeling/) nel [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) metodo della classe del contesto. La configurazione predefinita è:

```CSharp
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

    // Limit the size of the composite key columns due to common database restrictions
    b.Property(l => l.LoginProvider).HasMaxLength(128);
    b.Property(l => l.ProviderKey).HasMaxLength(128);

    // Maps to the AspNetUserLogins table
    b.ToTable("AspNetUserLogins");
});

builder.Entity<TUserToken>(b =>
{
    // Composite primary key consisting of the UserId, LoginProvider and Name
    b.HasKey(t => new { t.UserId, t.LoginProvider, t.Name });

    // Limit the size of the composite key columns due to common database restrictions
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

Identità definisce i tipi CLR predefinito per ognuno dei tipi di entità elencati in precedenza. Questi tipi sono precedute tutte dal prefisso "Identity": `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, e `IdentityUserRole`.

Anziché utilizzare direttamente questi tipi, i tipi è utilizzabile come classi di base per i tipi dell'applicazione. Il `DbContext` classi definite dall'identità sono generiche in modo che i tipi CLR diversi possono essere utilizzati per uno o più dei tipi di entità nel modello. Questi tipi generici consentono anche per il tipo della chiave primaria utente da modificare.

Quando si utilizza l'identità con supporto per i ruoli, un [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) consigliabile utilizzare la classe:

```CSharp
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
public class IdentityDbContext<TUser, TRole, TKey>
    : IdentityDbContext<TUser, TRole, TKey, IdentityUserClaim<TKey>, IdentityUserRole<TKey>, IdentityUserLogin<TKey>, IdentityRoleClaim<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TRole : IdentityRole<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments
// The key type is defined by TKey
public abstract class IdentityDbContext<TUser, TRole, TKey, TUserClaim, TUserRole, TUserLogin, TRoleClaim, TUserToken>
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

È inoltre possibile utilizzare identità senza ruoli (solo attestazioni), nel qual caso un `IdentityUserContext` consigliabile utilizzare la classe:


```CSharp
// Uses the built-in non-role Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityUserContext<TUser>
    : IdentityUserContext<TUser, string>
        where TUser : IdentityUser
{
}

// Uses the built-in non-role Identity types except with a custom User type
// The key type is defined by TKey
public class IdentityUserContext<TUser, TKey>
    : IdentityUserContext<TUser, TKey, IdentityUserClaim<TKey>, IdentityUserLogin<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments, with no roles
// The key type is defined by TKey
public abstract class IdentityUserContext<TUser, TKey, TUserClaim, TUserLogin, TUserToken>
    : DbContext
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
        where TUserClaim : IdentityUserClaim<TKey>
        where TUserLogin : IdentityUserLogin<TKey>
        where TUserToken : IdentityUserToken<TKey>
{
}
```

## <a name="customizing-the-model"></a>Personalizzazione del modello

Il punto di partenza per la personalizzazione del modello consiste nel derivare dal tipo di contesto appropriate; vedere la sezione precedente. Questo tipo di contesto viene abitualmente chiamato `ApplicationDbContext` e viene creato tramite i modelli di ASP.NET Core.

Il contesto viene utilizzato per configurare il modello in due modi:
* Fornendo i tipi di entità e la chiave per i parametri di tipo generico.
* Eseguendo l'override `OnModelCreating` per modificare il mapping di questi tipi.

Quando si esegue l'override `OnModelCreating`, `base.OnModelCreating` deve essere chiamato per primo, l'override di configurazione deve essere chiamata successivamente. In genere EF dei componenti di base ha un criterio last-un-wins per la configurazione. Ad esempio, se il `ToTable` per un'entità tipo viene chiamato prima con il nome di una tabella e quindi nuovamente versioni successive con un nome di tabella diverso, quindi il nome della tabella nella seconda chiamata è quella che viene utilizzato.

### <a name="using-a-custom-user-type"></a>Utilizzando un tipo di utente personalizzato

Per utilizzare un tipo di utente personalizzato, creare il tipo che eredita da `IdentityUser`. È facoltativa per assegnare un nome di questo tipo `ApplicationUser`. Questo tipo in genere hanno proprietà aggiuntive non nel tipo di base, in caso contrario, non vi sarà alcun valore in crearlo. Ad esempio:

```CSharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

Successivamente è possibile utilizzare questo tipo come argomento generico per il contesto:

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

Aggiornamento `ConfigureServices` usare il nuovo `ApplicationUser` classe:

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Non è necessario eseguire l'override `OnModelCreating` qui perché verrà eseguito il mapping dei componenti di base di EF il `CustomTag` proprietà per convenzione. Tuttavia, il database dovrà essere aggiornata per ottenere un nuovo `CustomTag` colonna. A tale scopo, aggiungere una migrazione e quindi aggiornare il database come descritto in [identità e le migrazioni Core EF](#identity-migrations).

### <a name="changing-the-key-type"></a>Modifica il tipo di chiave

Modifica del tipo di colonne chiave primaria (PK) dopo aver creato il database risulta problematica in molti sistemi di database. Modifica la chiave pubblica in genere implica eliminare e ricreare la tabella. Pertanto, è consigliabile specificare i tipi di chiave della migrazione iniziale in modo che i tipi di chiave di destinazione vengono creati quando viene creato il database.

Se il database è stato creato, quindi `Drop-Database` (PMC) o `dotnet ef database drop` (.NET Core CLI) può essere usato per eliminarlo.

Dopo la conferma che il database non esiste, rimuovere la migrazione iniziale con `Remove-Migration` (PMC) o `dotnet ef migrations remove` (.NET Core CLI).

Aggiornamento di `ApplicationDbContext` per utilizzare un'altra classe di base, che specifica il nuovo tipo di chiave per `TKey`. Ad esempio, per usare un `Guid` chiave:

```CSharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

Si noti che le classi generiche `IdentityUser<TKey>` e `IdentityRole<TKey>` necessario specificare anche usare il nuovo tipo di chiave. `ConfigureServices` deve essere aggiornato per usare l'utente generico:

```CSharp
services.AddDefaultIdentity<IdentityUser<Guid>>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Se un oggetto personalizzato `ApplicationUser` è in uso, aggiornarlo da cui ereditare `IdentityUser<TKey>`. Ad esempio:

```CSharp
public class ApplicationUser : IdentityUser<Guid>
{
    public string CustomTag { get; set; }
}

public class ApplicationDbContext : IdentityDbContext<ApplicationUser, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

### <a name="adding-navigation-properties"></a>Aggiunta di proprietà di navigazione

Modifica della configurazione del modello per le relazioni può essere un più difficile apportare altre modifiche. Per sostituire le relazioni esistenti anziché creare un nuovo relazioni aggiuntive, è necessario prestare attenzione. In particolare, la relazione modificata deve specificare la stessa proprietà di chiave esterna come la relazione esistente. Ad esempio, la relazione tra `Users` e `UserClaims` è per impostazione predefinita specificata come:

```CSharp
builder.Entity<TUser>(b =>
{
    // Each User can have many UserClaims
    b.HasMany<TUserClaim>()
     .WithOne()
     .HasForeignKey(uc => uc.UserId)
     .IsRequired();
});
```

La chiave esterna per la relazione viene specificata come il `UserClaim.UserId` proprietà. `HasMany` e `WithOne` chiamato senza argomenti per creare la relazione senza proprietà di navigazione.

Aggiungere una proprietà di navigazione verso `ApplicationUser` che consentirà associata `UserClaims` per farvi riferimento da parte dell'utente:

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

Si noti che il `TKey` per `IdentityUserClaim<TKey>` è di tipo specificato per la chiave primaria della users, in questo caso `string` che si sta utilizzando le impostazioni predefinite. Si tratta **non** tipo di chiave primario per la `UserClaim` tipo di entità.

Ora che la proprietà di navigazione deve essere configurata `OnModelCreating`:

```CSharp
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

Si noti che sia configurata relazione esattamente com'era prima, solo con una proprietà di navigazione specificata nella chiamata a `HasMany`.

Le proprietà di navigazione esistono solo nel modello di Entity Framework, non il database. Perché la chiave esterna per la relazione non è cambiato, questo tipo di modifica del modello non richiede il database da aggiornare. Tale valore può essere controllato mediante l'aggiunta di una migrazione dopo la modifica: il `Up` e `Down` metodi sono vuoti.

### <a name="adding-all-user-navigation-properties"></a>Aggiunta di tutte le proprietà di navigazione di utente

Utilizzando la sezione come guida, ecco un esempio che configura le proprietà di navigazione unidirezionale per tutte le relazioni sull'utente:

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<IdentityUserRole<string>> UserRoles { get; set; }
}
```

```CSharp
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

### <a name="adding-user-and-role-navigation-properties"></a>Proprietà di navigazione di aggiunta dell'utente e il ruolo

Utilizzando la sezione come guida, ecco un esempio che configura le proprietà di navigazione per tutte le relazioni per l'utente e ruolo:

```CSharp
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

```CSharp
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

* Questo esempio è inclusa anche la `UserRole` aggiungere entità, è necessario per esplorare la relazione molti-a-molti utenti ai ruoli.
* Ricordarsi di modificare i tipi di proprietà di navigazione in modo da riflettere che `ApplicationXxx` sono ora utilizzati tipi anziché `IdentityXxx` tipi.
* Ricordarsi di utilizzare il `ApplicationXxx` nell'interfaccia generica `ApplicationContext` definizione.

### <a name="adding-all-navigation-properties"></a>Aggiunta di tutte le proprietà di navigazione

Utilizzando la sezione come guida, di seguito è riportato un esempio che configura le proprietà di navigazione per tutte le relazioni in tutti i tipi di entità:

```CSharp
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

```CSharp
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

### <a name="using-composite-keys"></a>Utilizzando le chiavi composte

Le sezioni precedenti hanno illustrato la modifica del tipo di chiave utilizzata nel modello di identità. La modifica del modello di chiave di identità per l'utilizzo chiavi composte non è supportata né consigliata. Utilizzo di una chiave composta con identità comporta la modifica come il codice di gestione di identità interagisce con il modello, ovvero una personalizzazione non supportata e rientra nell'ambito di questo documento.

### <a name="changing-tablecolumn-names-and-facets"></a>Modifica i nomi di tabella/colonna e i facet

Per modificare i nomi di tabelle e colonne, chiamare `base.OnModelCreating`e quindi aggiungere la configurazione per eseguire l'override di uno qualsiasi dei valori predefiniti. Ad esempio, per modificare il nome di tutte le tabelle di identità:

```CSharp
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

Si noti che questi esempi i tipi di identità predefinito. Se si utilizza un tipo di applicazione, ad esempio `ApplicationUser` quindi configurare tale tipo anziché il tipo predefinito.

Di seguito è riportato un esempio che modifica alcuni nomi di colonna:

```CSharp
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

Alcuni tipi di colonne del database possono essere configurati con determinati _facet_ , ad esempio la lunghezza della stringa massima consentita. Di seguito è riportato un esempio che consente di impostare max lunghezze delle colonne di diverse proprietà di stringa del modello:

```CSharp
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

### <a name="mapping-to-a-different-schema"></a>Il mapping a uno schema diverso

Gli schemi possono comportarsi in modo diverso nei provider di database diverso, ma per SQL Server, il valore predefinito consiste nel creare tutte le tabelle nello schema "dbo". Ciò può essere modificato per invece creare le tabelle in uno schema diverso. Ad esempio:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

### <a name="lazy-loading"></a>Caricamento lazy

In questa sezione viene aggiunto il supporto per il caricamento differito proxy nel modello di identità. Il caricamento lazy è utile perché consente a proprietà di navigazione da utilizzare senza prima verificare che vengono caricati.

Tipi di entità è possibile eseguire adatti per caricamento lazy in diversi modi, come descritto nel [documentazione principale di EF](/ef/core/querying/related-data#lazy-loading). Per semplicità, si utilizzerà il caricamento differito proxy, che richiede:

* Installazione del [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) pacchetto.
* Una chiamata a `.UseLazyLoadingProxies()` all'interno di `AddDbContext`.
* Tipi di entità pubbliche con le proprietà di navigazione virtuale pubblico.

Di seguito è riportato un esempio della chiamata al metodo `.UseLazyLoadingProxies()`:

```CSharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
            .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Si riferiscono nuovamente gli esempi precedenti per l'aggiunta di proprietà di navigazione per i tipi di entità.
