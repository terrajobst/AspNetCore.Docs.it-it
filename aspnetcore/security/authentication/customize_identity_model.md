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
# <a name="identity-model-customization"></a><span data-ttu-id="3c7f3-103">Personalizzazione del modello di identità</span><span class="sxs-lookup"><span data-stu-id="3c7f3-103">Identity model customization</span></span>

<span data-ttu-id="3c7f3-104">Da [Arthur Vickers](https://github.com/ajcvickers)</span><span class="sxs-lookup"><span data-stu-id="3c7f3-104">By [Arthur Vickers](https://github.com/ajcvickers)</span></span>

<span data-ttu-id="3c7f3-105">Identità di ASP.NET Core fornisce un framework per gestire e archiviare gli account utente nelle applicazioni ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-105">ASP.NET Core Identity provides a framework for managing and storing user accounts in ASP.NET Core applications.</span></span> <span data-ttu-id="3c7f3-106">Identità viene aggiunto al progetto quando "Singoli account utente di" sia selezionato come il meccanismo di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-106">Identity is added to your project when "Individual User Accounts" is selected as the authentication mechanism.</span></span> <span data-ttu-id="3c7f3-107">Per impostazione predefinita, identità, utilizzerà di un Entity Framework (EF) modello di dati principale.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-107">By default, Identity makes use of an Entity Framework (EF) Core data model.</span></span> <span data-ttu-id="3c7f3-108">In questo articolo viene descritto come personalizzare il modello di identità.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-108">This article describes how to customize the Identity model.</span></span>

<a name="identity-migrations"></a>

## <a name="identity-and-ef-core-migrations"></a><span data-ttu-id="3c7f3-109">Identità e le migrazioni dei componenti di base di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="3c7f3-109">Identity and EF Core Migrations</span></span>

<span data-ttu-id="3c7f3-110">Prima di esaminare il modello, è utile comprendere il funzionamento con identità [EF Core migrazioni](/ef/core/managing-schemas/migrations/) per creare e aggiornare un database.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-110">Before examining the model, it's useful to understand how Identity works with [EF Core Migrations](/ef/core/managing-schemas/migrations/) to create and update a database.</span></span> <span data-ttu-id="3c7f3-111">Al primo livello, il processo è:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-111">At the top level, the process is:</span></span>

1. <span data-ttu-id="3c7f3-112">Definire o aggiornare una [modello di dati nel codice](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="3c7f3-112">Define or update a [data model in code](/ef/core/modeling/).</span></span>
1. <span data-ttu-id="3c7f3-113">Aggiungere una migrazione per tradurre le modifiche che possono essere applicate al database di questo modello.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-113">Add a migration to translate this model into changes that can be applied to the database.</span></span>
1. <span data-ttu-id="3c7f3-114">Verificare che la migrazione rappresenta correttamente intenzioni.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-114">Check that the migration correctly represents your intentions.</span></span>
1. <span data-ttu-id="3c7f3-115">Applicare la migrazione per aggiornare il database per essere sincronizzato con il modello.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-115">Apply the migration to update the database to be in sync with the model.</span></span>
1. <span data-ttu-id="3c7f3-116">Ripetere i passaggi da 1 a 4 per ulteriormente ridefinire il modello e mantenere sincronizzati il database.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-116">Repeat steps 1-4 to further refine the model and keep the database in sync.</span></span>

<span data-ttu-id="3c7f3-117">Le migrazioni vengono aggiunti e applicata utilizzando:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-117">Migrations are added and applied using:</span></span>

* <span data-ttu-id="3c7f3-118">Il [Console di gestione pacchetti](/ef/core/miscellaneous/cli/powershell) (PMC) se si utilizza Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-118">The [Package Manager Console](/ef/core/miscellaneous/cli/powershell) (PMC) if you are using Visual Studio.</span></span>
* <span data-ttu-id="3c7f3-119">Il [dotnet comandi](/ef/core/miscellaneous/cli/dotnet) nella riga di comando.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-119">The [dotnet commands](/ef/core/miscellaneous/cli/dotnet) on the command line.</span></span>
* <span data-ttu-id="3c7f3-120">Quando si seleziona il collegamento di migrazioni nella pagina di errore quando l'applicazione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-120">Selecting the migrations link on the error page when the application is run.</span></span>

<span data-ttu-id="3c7f3-121">ASP.NET Core dispone di un gestore di pagina di errore in fase di sviluppo che può essere usato per applicare le migrazioni quando l'applicazione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-121">ASP.NET Core has a development-time error page handler that can be used to apply migrations when the application is run.</span></span> <span data-ttu-id="3c7f3-122">Per le applicazioni di produzione, è spesso più appropriato per generare gli script SQL dalle migrazioni e distribuirle nel database come parte di una distribuzione di applicazioni e database controllata.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-122">For production applications, it is often more appropriate to generate SQL scripts from the migrations and deploy these to the database as part of a controlled application and database deployment.</span></span>

<span data-ttu-id="3c7f3-123">Quando viene creata una nuova applicazione utilizzando l'identità, i passaggi 1 e 2 sopra sono già stati completati.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-123">When a new application using Identity is created, steps 1 and 2 above have already been completed.</span></span> <span data-ttu-id="3c7f3-124">Vale a dire, il modello di dati iniziale esiste già, e la migrazione iniziale è stato aggiunto al progetto.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-124">That is, the initial data model already exists, and the initial migration has been added to the project.</span></span> <span data-ttu-id="3c7f3-125">La migrazione iniziale deve ancora essere applicato al database.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-125">The initial migration still needs to be applied to the database.</span></span> <span data-ttu-id="3c7f3-126">La migrazione iniziale può essere eseguita tramite l'esecuzione di Update-Database (PMC), il comando update (.NET Core CLI) database ef dotnet, o utilizzando la pagina di errore quando l'applicazione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-126">The initial migration can be done by running the Update-Database (PMC), the dotnet ef database update (.NET Core CLI) command, or by using the error page when the application is run.</span></span>

<span data-ttu-id="3c7f3-127">I passaggi precedenti devono essere ripetuti quando vengono apportate modifiche al modello.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-127">The preceding steps need to be repeated as changes are made to the model.</span></span>

<a name="identity-model"></a>

## <a name="the-identity-model"></a><span data-ttu-id="3c7f3-128">Il modello di identità</span><span class="sxs-lookup"><span data-stu-id="3c7f3-128">The Identity model</span></span>

### <a name="entity-types"></a><span data-ttu-id="3c7f3-129">Tipi di entità</span><span class="sxs-lookup"><span data-stu-id="3c7f3-129">Entity types</span></span>

<span data-ttu-id="3c7f3-130">Il modello di identità è costituita da sette tipi di entità:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-130">The Identity model consists of seven entity types:</span></span>

* <span data-ttu-id="3c7f3-131">`User` -rappresenta l'utente</span><span class="sxs-lookup"><span data-stu-id="3c7f3-131">`User` - represents the user</span></span>
* <span data-ttu-id="3c7f3-132">`Role` -rappresenta un ruolo</span><span class="sxs-lookup"><span data-stu-id="3c7f3-132">`Role` - represents a role</span></span>
* <span data-ttu-id="3c7f3-133">`UserClaim` -rappresenta un'attestazione che deve disporre di un utente</span><span class="sxs-lookup"><span data-stu-id="3c7f3-133">`UserClaim` - represents a claim that a user possess</span></span>
* <span data-ttu-id="3c7f3-134">`UserToken` -rappresenta un token di autenticazione per un utente</span><span class="sxs-lookup"><span data-stu-id="3c7f3-134">`UserToken` - represents an authentication token for a user</span></span>
* <span data-ttu-id="3c7f3-135">`UserLogin` -Associa un utente con un account di accesso</span><span class="sxs-lookup"><span data-stu-id="3c7f3-135">`UserLogin` - associates a user with a login</span></span>
* <span data-ttu-id="3c7f3-136">`RoleClaim` -rappresenta un'attestazione che viene concesso a tutti gli utenti all'interno di un ruolo</span><span class="sxs-lookup"><span data-stu-id="3c7f3-136">`RoleClaim` - represents a claim that is granted to all users within a role</span></span>
* <span data-ttu-id="3c7f3-137">`UserRole` -aggiungere entità che associa gli utenti e ruoli</span><span class="sxs-lookup"><span data-stu-id="3c7f3-137">`UserRole` - join entity that associates users and roles</span></span>

### <a name="entity-type-relationships"></a><span data-ttu-id="3c7f3-138">Relazioni di tipo di entità</span><span class="sxs-lookup"><span data-stu-id="3c7f3-138">Entity type relationships</span></span>

<span data-ttu-id="3c7f3-139">Questi tipi di entità sono correlati tra loro nei modi seguenti:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-139">These entity types are related to each other in the following ways:</span></span>

* <span data-ttu-id="3c7f3-140">Ogni `User` può avere numerosi `UserClaims`</span><span class="sxs-lookup"><span data-stu-id="3c7f3-140">Each `User` can have many `UserClaims`</span></span>
* <span data-ttu-id="3c7f3-141">Ogni `User` può avere numerosi `UserLogins`</span><span class="sxs-lookup"><span data-stu-id="3c7f3-141">Each `User` can have many `UserLogins`</span></span>
* <span data-ttu-id="3c7f3-142">Ogni `User` può avere numerosi `UserTokens`</span><span class="sxs-lookup"><span data-stu-id="3c7f3-142">Each `User` can have many `UserTokens`</span></span>
* <span data-ttu-id="3c7f3-143">Ogni `Role` può avere numerosi associata `RoleClaims`</span><span class="sxs-lookup"><span data-stu-id="3c7f3-143">Each `Role` can have many associated `RoleClaims`</span></span>
* <span data-ttu-id="3c7f3-144">Ogni `User` può avere numerosi associata `Roles`e ogni `Role` può essere associato a molti utenti</span><span class="sxs-lookup"><span data-stu-id="3c7f3-144">Each `User` can have many associated `Roles`, and each `Role` can be associated with many Users</span></span>
  * <span data-ttu-id="3c7f3-145">Si tratta di una relazione molti-a-molti, che richiede una tabella di join del database.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-145">This is a many-to-many relationship, which requires a join table in the database.</span></span> <span data-ttu-id="3c7f3-146">La tabella di join è rappresentata dal `UserRole` entità.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-146">The join table is represented by the `UserRole` entity.</span></span>

### <a name="default-model-configuration"></a><span data-ttu-id="3c7f3-147">Configurazione del modello predefinito</span><span class="sxs-lookup"><span data-stu-id="3c7f3-147">Default model configuration</span></span>

<span data-ttu-id="3c7f3-148">Identità definisce una varietà di "classi di contesto" che ereditano da [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) per configurare e utilizzare il modello.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-148">Identity defines a variety of "context classes" which inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) to configure and use the model.</span></span> <span data-ttu-id="3c7f3-149">Questa configurazione viene eseguita utilizzando il [EF Core codice prima Fluent API](/ef/core/modeling/) nel [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) metodo della classe del contesto.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-149">This configuration is done using the [EF Core Code First Fluent API](/ef/core/modeling/) in the [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) method of the context class.</span></span> <span data-ttu-id="3c7f3-150">La configurazione predefinita è:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-150">The default configuration is:</span></span>

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

### <a name="model-generic-types"></a><span data-ttu-id="3c7f3-151">Tipi generici di modello</span><span class="sxs-lookup"><span data-stu-id="3c7f3-151">Model generic types</span></span>

<span data-ttu-id="3c7f3-152">Identità definisce i tipi CLR predefinito per ognuno dei tipi di entità elencati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-152">Identity defines default CLR types for each of the entity types listed above.</span></span> <span data-ttu-id="3c7f3-153">Questi tipi sono precedute tutte dal prefisso "Identity": `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, e `IdentityUserRole`.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-153">These types are all prefixed with "Identity": `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, and `IdentityUserRole`.</span></span>

<span data-ttu-id="3c7f3-154">Anziché utilizzare direttamente questi tipi, i tipi è utilizzabile come classi di base per i tipi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-154">Rather than using these types directly, the types can be used as base classes for the application's own types.</span></span> <span data-ttu-id="3c7f3-155">Il `DbContext` classi definite dall'identità sono generiche in modo che i tipi CLR diversi possono essere utilizzati per uno o più dei tipi di entità nel modello.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-155">The `DbContext` classes defined by Identity are generic such that different CLR types can be used for one or more of the entity types in the model.</span></span> <span data-ttu-id="3c7f3-156">Questi tipi generici consentono anche per il tipo della chiave primaria utente da modificare.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-156">These generic types also allow for the type of the User primary key to be changed.</span></span>

<span data-ttu-id="3c7f3-157">Quando si utilizza l'identità con supporto per i ruoli, un [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) consigliabile utilizzare la classe:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-157">When using Identity with support for roles, an [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) class should be used:</span></span>

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

<span data-ttu-id="3c7f3-158">È inoltre possibile utilizzare identità senza ruoli (solo attestazioni), nel qual caso un `IdentityUserContext` consigliabile utilizzare la classe:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-158">It is also possible to use Identity without roles (only claims), in which case an `IdentityUserContext` class should be used:</span></span>


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

## <a name="customizing-the-model"></a><span data-ttu-id="3c7f3-159">Personalizzazione del modello</span><span class="sxs-lookup"><span data-stu-id="3c7f3-159">Customizing the model</span></span>

<span data-ttu-id="3c7f3-160">Il punto di partenza per la personalizzazione del modello consiste nel derivare dal tipo di contesto appropriate; vedere la sezione precedente.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-160">The starting point for customizing the model is to derive from the appropriate context type; see the preceding section.</span></span> <span data-ttu-id="3c7f3-161">Questo tipo di contesto viene abitualmente chiamato `ApplicationDbContext` e viene creato tramite i modelli di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-161">This context type is customarily called `ApplicationDbContext` and is created by the ASP.NET Core templates.</span></span>

<span data-ttu-id="3c7f3-162">Il contesto viene utilizzato per configurare il modello in due modi:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-162">The context is used to configure the model in two ways:</span></span>
* <span data-ttu-id="3c7f3-163">Fornendo i tipi di entità e la chiave per i parametri di tipo generico.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-163">By supplying entity and key types for the generic type parameters.</span></span>
* <span data-ttu-id="3c7f3-164">Eseguendo l'override `OnModelCreating` per modificare il mapping di questi tipi.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-164">By overriding `OnModelCreating` to modify the mapping of these types.</span></span>

<span data-ttu-id="3c7f3-165">Quando si esegue l'override `OnModelCreating`, `base.OnModelCreating` deve essere chiamato per primo, l'override di configurazione deve essere chiamata successivamente.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-165">When overriding `OnModelCreating`, `base.OnModelCreating` should be called first, the overriding configuration should be called next.</span></span> <span data-ttu-id="3c7f3-166">In genere EF dei componenti di base ha un criterio last-un-wins per la configurazione.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-166">EF Core generally has a last-one-wins policy for configuration.</span></span> <span data-ttu-id="3c7f3-167">Ad esempio, se il `ToTable` per un'entità tipo viene chiamato prima con il nome di una tabella e quindi nuovamente versioni successive con un nome di tabella diverso, quindi il nome della tabella nella seconda chiamata è quella che viene utilizzato.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-167">For example, if the `ToTable` for an entity type is called first with one table name and then again later with a different table name, then the table name in the second call is the one that is used.</span></span>

### <a name="using-a-custom-user-type"></a><span data-ttu-id="3c7f3-168">Utilizzando un tipo di utente personalizzato</span><span class="sxs-lookup"><span data-stu-id="3c7f3-168">Using a custom User type</span></span>

<span data-ttu-id="3c7f3-169">Per utilizzare un tipo di utente personalizzato, creare il tipo che eredita da `IdentityUser`.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-169">To use a custom User type, create the type and have it inherit from `IdentityUser`.</span></span> <span data-ttu-id="3c7f3-170">È facoltativa per assegnare un nome di questo tipo `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-170">It is customary to name this type `ApplicationUser`.</span></span> <span data-ttu-id="3c7f3-171">Questo tipo in genere hanno proprietà aggiuntive non nel tipo di base, in caso contrario, non vi sarà alcun valore in crearlo.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-171">This type will typically have additional properties not in the base type, otherwise there would be no value in creating it.</span></span> <span data-ttu-id="3c7f3-172">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-172">For example:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

<span data-ttu-id="3c7f3-173">Successivamente è possibile utilizzare questo tipo come argomento generico per il contesto:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-173">Next use this type as a generic argument for the context:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="3c7f3-174">Aggiornamento `ConfigureServices` usare il nuovo `ApplicationUser` classe:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-174">Update `ConfigureServices` to use the new `ApplicationUser` class:</span></span>

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="3c7f3-175">Non è necessario eseguire l'override `OnModelCreating` qui perché verrà eseguito il mapping dei componenti di base di EF il `CustomTag` proprietà per convenzione.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-175">There is no need to override `OnModelCreating` here because EF Core will map the `CustomTag` property by convention.</span></span> <span data-ttu-id="3c7f3-176">Tuttavia, il database dovrà essere aggiornata per ottenere un nuovo `CustomTag` colonna.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-176">However, the database will need to be updated to get a new `CustomTag` column.</span></span> <span data-ttu-id="3c7f3-177">A tale scopo, aggiungere una migrazione e quindi aggiornare il database come descritto in [identità e le migrazioni Core EF](#identity-migrations).</span><span class="sxs-lookup"><span data-stu-id="3c7f3-177">To do this, add a migration, and then update the database as described in  [Identity and EF Core Migrations](#identity-migrations).</span></span>

### <a name="changing-the-key-type"></a><span data-ttu-id="3c7f3-178">Modifica il tipo di chiave</span><span class="sxs-lookup"><span data-stu-id="3c7f3-178">Changing the key type</span></span>

<span data-ttu-id="3c7f3-179">Modifica del tipo di colonne chiave primaria (PK) dopo aver creato il database risulta problematica in molti sistemi di database.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-179">Changing the type of a primary key (PK) column after the database has been created is problematic on many database systems.</span></span> <span data-ttu-id="3c7f3-180">Modifica la chiave pubblica in genere implica eliminare e ricreare la tabella.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-180">Changing the PK typically involves dropping and re-creating the table.</span></span> <span data-ttu-id="3c7f3-181">Pertanto, è consigliabile specificare i tipi di chiave della migrazione iniziale in modo che i tipi di chiave di destinazione vengono creati quando viene creato il database.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-181">Therefore, it is recommended that key types be specified in the initial migration such that the target key types are created when the database is created.</span></span>

<span data-ttu-id="3c7f3-182">Se il database è stato creato, quindi `Drop-Database` (PMC) o `dotnet ef database drop` (.NET Core CLI) può essere usato per eliminarlo.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-182">If the database has been created, then `Drop-Database` (PMC) or `dotnet ef database drop` (.NET Core CLI) can be used to delete it.</span></span>

<span data-ttu-id="3c7f3-183">Dopo la conferma che il database non esiste, rimuovere la migrazione iniziale con `Remove-Migration` (PMC) o `dotnet ef migrations remove` (.NET Core CLI).</span><span class="sxs-lookup"><span data-stu-id="3c7f3-183">Once it is confirmed that the database doesn't exist,  remove the initial migration with `Remove-Migration` (PMC) or `dotnet ef migrations remove` (.NET Core CLI).</span></span>

<span data-ttu-id="3c7f3-184">Aggiornamento di `ApplicationDbContext` per utilizzare un'altra classe di base, che specifica il nuovo tipo di chiave per `TKey`.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-184">Update the `ApplicationDbContext` to use a different base class, specifying the new key type for `TKey`.</span></span> <span data-ttu-id="3c7f3-185">Ad esempio, per usare un `Guid` chiave:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-185">For example, to use a `Guid` key:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="3c7f3-186">Si noti che le classi generiche `IdentityUser<TKey>` e `IdentityRole<TKey>` necessario specificare anche usare il nuovo tipo di chiave.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-186">Notice that the generic classes `IdentityUser<TKey>` and `IdentityRole<TKey>` must also be specified to use the new key type.</span></span> <span data-ttu-id="3c7f3-187">`ConfigureServices` deve essere aggiornato per usare l'utente generico:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-187">`ConfigureServices` must be updated to use the generic user:</span></span>

```CSharp
services.AddDefaultIdentity<IdentityUser<Guid>>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="3c7f3-188">Se un oggetto personalizzato `ApplicationUser` è in uso, aggiornarlo da cui ereditare `IdentityUser<TKey>`.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-188">If a custom `ApplicationUser` is being used, update it to inherit from `IdentityUser<TKey>`.</span></span> <span data-ttu-id="3c7f3-189">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-189">For example:</span></span>

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

### <a name="adding-navigation-properties"></a><span data-ttu-id="3c7f3-190">Aggiunta di proprietà di navigazione</span><span class="sxs-lookup"><span data-stu-id="3c7f3-190">Adding navigation properties</span></span>

<span data-ttu-id="3c7f3-191">Modifica della configurazione del modello per le relazioni può essere un più difficile apportare altre modifiche.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-191">Changing the model configuration for relationships can be a more difficult than making other changes.</span></span> <span data-ttu-id="3c7f3-192">Per sostituire le relazioni esistenti anziché creare un nuovo relazioni aggiuntive, è necessario prestare attenzione.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-192">Care must be taken to replace the existing relationships rather than create a new additional relationships.</span></span> <span data-ttu-id="3c7f3-193">In particolare, la relazione modificata deve specificare la stessa proprietà di chiave esterna come la relazione esistente.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-193">In particular, the changed relationship must specify the same foreign key property as the existing relationship.</span></span> <span data-ttu-id="3c7f3-194">Ad esempio, la relazione tra `Users` e `UserClaims` è per impostazione predefinita specificata come:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-194">For example, the relationship between `Users` and `UserClaims` is by default specified as:</span></span>

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

<span data-ttu-id="3c7f3-195">La chiave esterna per la relazione viene specificata come il `UserClaim.UserId` proprietà.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-195">The foreign key for this relationship is specified as the `UserClaim.UserId` property.</span></span> <span data-ttu-id="3c7f3-196">`HasMany` e `WithOne` chiamato senza argomenti per creare la relazione senza proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-196">`HasMany` and `WithOne` are called without arguments to create the relationship without navigation properties.</span></span>

<span data-ttu-id="3c7f3-197">Aggiungere una proprietà di navigazione verso `ApplicationUser` che consentirà associata `UserClaims` per farvi riferimento da parte dell'utente:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-197">Add a navigation property to `ApplicationUser` that will allow associated `UserClaims` to be referenced from the user:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

<span data-ttu-id="3c7f3-198">Si noti che il `TKey` per `IdentityUserClaim<TKey>` è di tipo specificato per la chiave primaria della users, in questo caso `string` che si sta utilizzando le impostazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-198">Note that the `TKey` for `IdentityUserClaim<TKey>` is type specified for the primary key of users--in this case `string` since we're using the defaults.</span></span> <span data-ttu-id="3c7f3-199">Si tratta **non** tipo di chiave primario per la `UserClaim` tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-199">It is **not** the primary key type for the `UserClaim` entity type.</span></span>

<span data-ttu-id="3c7f3-200">Ora che la proprietà di navigazione deve essere configurata `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-200">Now that the navigation property exists it must be configured in `OnModelCreating`:</span></span>

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

<span data-ttu-id="3c7f3-201">Si noti che sia configurata relazione esattamente com'era prima, solo con una proprietà di navigazione specificata nella chiamata a `HasMany`.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-201">Notice that relationship is configured exactly as it was before, only with a navigation property specified in the call to `HasMany`.</span></span>

<span data-ttu-id="3c7f3-202">Le proprietà di navigazione esistono solo nel modello di Entity Framework, non il database.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-202">The navigation properties only exist in the EF model, not the database.</span></span> <span data-ttu-id="3c7f3-203">Perché la chiave esterna per la relazione non è cambiato, questo tipo di modifica del modello non richiede il database da aggiornare.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-203">Because the foreign key for the relationship has not changed, this kind of model change does not require the database to be updated.</span></span> <span data-ttu-id="3c7f3-204">Tale valore può essere controllato mediante l'aggiunta di una migrazione dopo la modifica: il `Up` e `Down` metodi sono vuoti.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-204">This can be checked by adding a migration after making the change: the `Up` and `Down` methods are empty.</span></span>

### <a name="adding-all-user-navigation-properties"></a><span data-ttu-id="3c7f3-205">Aggiunta di tutte le proprietà di navigazione di utente</span><span class="sxs-lookup"><span data-stu-id="3c7f3-205">Adding all User navigation properties</span></span>

<span data-ttu-id="3c7f3-206">Utilizzando la sezione come guida, ecco un esempio che configura le proprietà di navigazione unidirezionale per tutte le relazioni sull'utente:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-206">Using the section above as guidance, here is an example that configures unidirectional navigation properties for all relationships on User:</span></span>

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

### <a name="adding-user-and-role-navigation-properties"></a><span data-ttu-id="3c7f3-207">Proprietà di navigazione di aggiunta dell'utente e il ruolo</span><span class="sxs-lookup"><span data-stu-id="3c7f3-207">Adding User and Role navigation properties</span></span>

<span data-ttu-id="3c7f3-208">Utilizzando la sezione come guida, ecco un esempio che configura le proprietà di navigazione per tutte le relazioni per l'utente e ruolo:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-208">Using the section above as guidance, here is an example that configures navigation properties for all relationships on User and Role:</span></span>

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

<span data-ttu-id="3c7f3-209">Note:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-209">Notes:</span></span>

* <span data-ttu-id="3c7f3-210">Questo esempio è inclusa anche la `UserRole` aggiungere entità, è necessario per esplorare la relazione molti-a-molti utenti ai ruoli.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-210">This example also includes the `UserRole` join entity, which is needed to navigate the many-to-many relationship from Users to Roles.</span></span>
* <span data-ttu-id="3c7f3-211">Ricordarsi di modificare i tipi di proprietà di navigazione in modo da riflettere che `ApplicationXxx` sono ora utilizzati tipi anziché `IdentityXxx` tipi.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-211">Remember to change the types of the navigation properties to reflect that `ApplicationXxx` types are now being used instead of `IdentityXxx` types.</span></span>
* <span data-ttu-id="3c7f3-212">Ricordarsi di utilizzare il `ApplicationXxx` nell'interfaccia generica `ApplicationContext` definizione.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-212">Remember to use the `ApplicationXxx` in the generic `ApplicationContext` definition.</span></span>

### <a name="adding-all-navigation-properties"></a><span data-ttu-id="3c7f3-213">Aggiunta di tutte le proprietà di navigazione</span><span class="sxs-lookup"><span data-stu-id="3c7f3-213">Adding all navigation properties</span></span>

<span data-ttu-id="3c7f3-214">Utilizzando la sezione come guida, di seguito è riportato un esempio che configura le proprietà di navigazione per tutte le relazioni in tutti i tipi di entità:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-214">Using the section above as guidance, here is an example that configures navigation properties for all relationships on all entity types:</span></span>

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

### <a name="using-composite-keys"></a><span data-ttu-id="3c7f3-215">Utilizzando le chiavi composte</span><span class="sxs-lookup"><span data-stu-id="3c7f3-215">Using composite keys</span></span>

<span data-ttu-id="3c7f3-216">Le sezioni precedenti hanno illustrato la modifica del tipo di chiave utilizzata nel modello di identità.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-216">The preceding sections demonstrated changing the type of key used in the Identity model.</span></span> <span data-ttu-id="3c7f3-217">La modifica del modello di chiave di identità per l'utilizzo chiavi composte non è supportata né consigliata.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-217">Changing the Identity key model to use composite keys is not supported or recommended.</span></span> <span data-ttu-id="3c7f3-218">Utilizzo di una chiave composta con identità comporta la modifica come il codice di gestione di identità interagisce con il modello, ovvero una personalizzazione non supportata e rientra nell'ambito di questo documento.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-218">Using a composite key with Identity would involve changing how the Identity manager code interacts with the model, which is an unsupported customization and beyond the scope of this document.</span></span>

### <a name="changing-tablecolumn-names-and-facets"></a><span data-ttu-id="3c7f3-219">Modifica i nomi di tabella/colonna e i facet</span><span class="sxs-lookup"><span data-stu-id="3c7f3-219">Changing table/column names and facets</span></span>

<span data-ttu-id="3c7f3-220">Per modificare i nomi di tabelle e colonne, chiamare `base.OnModelCreating`e quindi aggiungere la configurazione per eseguire l'override di uno qualsiasi dei valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-220">To change the names of tables and columns, call `base.OnModelCreating`, and then add configuration to override any of the defaults.</span></span> <span data-ttu-id="3c7f3-221">Ad esempio, per modificare il nome di tutte le tabelle di identità:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-221">For example, to change the name of all the identity tables:</span></span>

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

<span data-ttu-id="3c7f3-222">Si noti che questi esempi i tipi di identità predefinito.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-222">Note that these examples use the default Identity types.</span></span> <span data-ttu-id="3c7f3-223">Se si utilizza un tipo di applicazione, ad esempio `ApplicationUser` quindi configurare tale tipo anziché il tipo predefinito.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-223">If you are using an application type, such as `ApplicationUser` then configure that type instead of the default type.</span></span>

<span data-ttu-id="3c7f3-224">Di seguito è riportato un esempio che modifica alcuni nomi di colonna:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-224">Here is an example that changes some column names:</span></span>

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

<span data-ttu-id="3c7f3-225">Alcuni tipi di colonne del database possono essere configurati con determinati _facet_ , ad esempio la lunghezza della stringa massima consentita.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-225">Some types of database columns can be configured with certain _facets_ such as the maximum string length allowed.</span></span> <span data-ttu-id="3c7f3-226">Di seguito è riportato un esempio che consente di impostare max lunghezze delle colonne di diverse proprietà di stringa del modello:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-226">Here is an example that sets column max lengths for several string properties in the model:</span></span>

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

### <a name="mapping-to-a-different-schema"></a><span data-ttu-id="3c7f3-227">Il mapping a uno schema diverso</span><span class="sxs-lookup"><span data-stu-id="3c7f3-227">Mapping to a different schema</span></span>

<span data-ttu-id="3c7f3-228">Gli schemi possono comportarsi in modo diverso nei provider di database diverso, ma per SQL Server, il valore predefinito consiste nel creare tutte le tabelle nello schema "dbo".</span><span class="sxs-lookup"><span data-stu-id="3c7f3-228">Schemas can behave differently in different database providers, but for SQL Server, the default is to create all tables in the "dbo" schema.</span></span> <span data-ttu-id="3c7f3-229">Ciò può essere modificato per invece creare le tabelle in uno schema diverso.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-229">This can be changed to instead create the tables in a different schema.</span></span> <span data-ttu-id="3c7f3-230">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-230">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

### <a name="lazy-loading"></a><span data-ttu-id="3c7f3-231">Caricamento lazy</span><span class="sxs-lookup"><span data-stu-id="3c7f3-231">Lazy loading</span></span>

<span data-ttu-id="3c7f3-232">In questa sezione viene aggiunto il supporto per il caricamento differito proxy nel modello di identità.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-232">In this section support for lazy-loading proxies in the Identity model is added.</span></span> <span data-ttu-id="3c7f3-233">Il caricamento lazy è utile perché consente a proprietà di navigazione da utilizzare senza prima verificare che vengono caricati.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-233">Lazy-loading is useful since it allows navigation properties to be used without first ensuring they are loaded.</span></span>

<span data-ttu-id="3c7f3-234">Tipi di entità è possibile eseguire adatti per caricamento lazy in diversi modi, come descritto nel [documentazione principale di EF](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="3c7f3-234">Entity types can be made suitable for lazy-loading in several ways, as described in the [EF Core documentation](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="3c7f3-235">Per semplicità, si utilizzerà il caricamento differito proxy, che richiede:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-235">For simplicity, we will use lazy-loading proxies, which requires:</span></span>

* <span data-ttu-id="3c7f3-236">Installazione del [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-236">Installation of the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package.</span></span>
* <span data-ttu-id="3c7f3-237">Una chiamata a `.UseLazyLoadingProxies()` all'interno di `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-237">A call to `.UseLazyLoadingProxies()` inside `AddDbContext`.</span></span>
* <span data-ttu-id="3c7f3-238">Tipi di entità pubbliche con le proprietà di navigazione virtuale pubblico.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-238">Public entity types with public virtual navigation properties.</span></span>

<span data-ttu-id="3c7f3-239">Di seguito è riportato un esempio della chiamata al metodo `.UseLazyLoadingProxies()`:</span><span class="sxs-lookup"><span data-stu-id="3c7f3-239">Here is an example of calling `.UseLazyLoadingProxies()`:</span></span>

```CSharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
            .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="3c7f3-240">Si riferiscono nuovamente gli esempi precedenti per l'aggiunta di proprietà di navigazione per i tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="3c7f3-240">Refer back to the examples above for adding navigation properties to the entity types.</span></span>
