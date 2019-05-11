---
title: Personalizzazione del modello in ASP.NET Core Identity
author: ajcvickers
description: Questo articolo descrive come personalizzare il modello di dati Entity Framework Core sottostante per ASP.NET Core Identity.
ms.author: avickers
ms.date: 04/24/2019
uid: security/authentication/customize_identity_model
ms.openlocfilehash: 53ce77e20722f3ba3282ff4455a0b70d30e635b0
ms.sourcegitcommit: ffe3ed7921ec6c7c70abaac1d10703ec9a43374c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/11/2019
ms.locfileid: "65536015"
---
# <a name="identity-model-customization-in-aspnet-core"></a><span data-ttu-id="58880-103">Personalizzazione del modello in ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="58880-103">Identity model customization in ASP.NET Core</span></span>

<span data-ttu-id="58880-104">Da [Arthur Vickers](https://github.com/ajcvickers)</span><span class="sxs-lookup"><span data-stu-id="58880-104">By [Arthur Vickers](https://github.com/ajcvickers)</span></span>

<span data-ttu-id="58880-105">ASP.NET Core Identity offre un framework per la gestione e l'archiviazione di account utente nelle App ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="58880-105">ASP.NET Core Identity provides a framework for managing and storing user accounts in ASP.NET Core apps.</span></span> <span data-ttu-id="58880-106">Identità viene aggiunto al progetto quando **singoli account utente di** è selezionato come meccanismo di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="58880-106">Identity is added to your project when **Individual User Accounts** is selected as the authentication mechanism.</span></span> <span data-ttu-id="58880-107">Per impostazione predefinita, identità rende l'uso di un Entity Framework (EF) modello di dati principale.</span><span class="sxs-lookup"><span data-stu-id="58880-107">By default, Identity makes use of an Entity Framework (EF) Core data model.</span></span> <span data-ttu-id="58880-108">Questo articolo descrive come personalizzare il modello di identità.</span><span class="sxs-lookup"><span data-stu-id="58880-108">This article describes how to customize the Identity model.</span></span>

## <a name="identity-and-ef-core-migrations"></a><span data-ttu-id="58880-109">Identità e le migrazioni di EF Core</span><span class="sxs-lookup"><span data-stu-id="58880-109">Identity and EF Core Migrations</span></span>

<span data-ttu-id="58880-110">Prima di esaminare il modello, è utile comprendere il funzionamento con Identity [migrazioni EF Core](/ef/core/managing-schemas/migrations/) per creare e aggiornare un database.</span><span class="sxs-lookup"><span data-stu-id="58880-110">Before examining the model, it's useful to understand how Identity works with [EF Core Migrations](/ef/core/managing-schemas/migrations/) to create and update a database.</span></span> <span data-ttu-id="58880-111">Al livello superiore, il processo è:</span><span class="sxs-lookup"><span data-stu-id="58880-111">At the top level, the process is:</span></span>

1. <span data-ttu-id="58880-112">Definire o aggiornare una [modello di dati nel codice](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="58880-112">Define or update a [data model in code](/ef/core/modeling/).</span></span>
1. <span data-ttu-id="58880-113">Aggiungere una migrazione per questo modello si traducono in modifiche che possono essere applicate al database.</span><span class="sxs-lookup"><span data-stu-id="58880-113">Add a Migration to translate this model into changes that can be applied to the database.</span></span>
1. <span data-ttu-id="58880-114">Verificare che la migrazione rappresenta in modo corretto le proprie intenzioni.</span><span class="sxs-lookup"><span data-stu-id="58880-114">Check that the Migration correctly represents your intentions.</span></span>
1. <span data-ttu-id="58880-115">Applicare la migrazione per aggiornare il database siano sincronizzate con il modello.</span><span class="sxs-lookup"><span data-stu-id="58880-115">Apply the Migration to update the database to be in sync with the model.</span></span>
1. <span data-ttu-id="58880-116">Ripetere i passaggi da 1 a 4 per un'ulteriore ridefinire il modello e mantenere sincronizzato il database.</span><span class="sxs-lookup"><span data-stu-id="58880-116">Repeat steps 1 through 4 to further refine the model and keep the database in sync.</span></span>

<span data-ttu-id="58880-117">Per aggiungere e applicare le migrazioni, usare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="58880-117">Use one of the following approaches to add and apply Migrations:</span></span>

* <span data-ttu-id="58880-118">Il **Console di gestione pacchetti** finestra se si usa Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="58880-118">The **Package Manager Console** (PMC) window if using Visual Studio.</span></span> <span data-ttu-id="58880-119">Per altre informazioni, vedere [strumenti di Entity Framework Core PMC](/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="58880-119">For more information, see [EF Core PMC tools](/ef/core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="58880-120">La CLI di .NET Core se si usa la riga di comando.</span><span class="sxs-lookup"><span data-stu-id="58880-120">The .NET Core CLI if using the command line.</span></span> <span data-ttu-id="58880-121">Per altre informazioni, vedere [strumenti da riga di comando .NET Core EF](/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="58880-121">For more information, see [EF Core .NET command line tools](/ef/core/miscellaneous/cli/dotnet).</span></span>
* <span data-ttu-id="58880-122">Facendo clic sui **applicare le migrazioni** pulsante nella pagina di errore quando si esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="58880-122">Clicking the **Apply Migrations** button on the error page when the app is run.</span></span>

<span data-ttu-id="58880-123">ASP.NET Core include un gestore di pagina di errore in fase di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="58880-123">ASP.NET Core has a development-time error page handler.</span></span> <span data-ttu-id="58880-124">Il gestore di è possibile applicare le migrazioni quando l'app viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="58880-124">The handler can apply migrations when the app is run.</span></span> <span data-ttu-id="58880-125">Le app di produzione in genere generano gli script SQL dalle migrazioni e distribuiscono le modifiche del database come parte di un'app controllata e la distribuzione del database.</span><span class="sxs-lookup"><span data-stu-id="58880-125">Production apps typically generate SQL scripts from the migrations and deploy database changes as part of a controlled app and database deployment.</span></span>

<span data-ttu-id="58880-126">Quando viene creata una nuova app con identità, hanno già completati i passaggi 1 e 2 sopra.</span><span class="sxs-lookup"><span data-stu-id="58880-126">When a new app using Identity is created, steps 1 and 2 above have already been completed.</span></span> <span data-ttu-id="58880-127">Vale a dire, il modello di dati iniziale esiste già e la migrazione iniziale è stato aggiunto al progetto.</span><span class="sxs-lookup"><span data-stu-id="58880-127">That is, the initial data model already exists, and the initial migration has been added to the project.</span></span> <span data-ttu-id="58880-128">La migrazione iniziale deve comunque essere applicati al database.</span><span class="sxs-lookup"><span data-stu-id="58880-128">The initial migration still needs to be applied to the database.</span></span> <span data-ttu-id="58880-129">La migrazione iniziale può essere applicata tramite uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="58880-129">The initial migration can be applied via one of the following approaches:</span></span>

* <span data-ttu-id="58880-130">Eseguire `Update-Database` nella console di gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="58880-130">Run `Update-Database` in PMC.</span></span>
* <span data-ttu-id="58880-131">Eseguire `dotnet ef database update` in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="58880-131">Run `dotnet ef database update` in a command shell.</span></span>
* <span data-ttu-id="58880-132">Scegliere il **applicare le migrazioni** pulsante nella pagina di errore quando si esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="58880-132">Click the **Apply Migrations** button on the error page when the app is run.</span></span>

<span data-ttu-id="58880-133">Ripetere i passaggi precedenti quando vengono apportate modifiche al modello.</span><span class="sxs-lookup"><span data-stu-id="58880-133">Repeat the preceding steps as changes are made to the model.</span></span>

## <a name="the-identity-model"></a><span data-ttu-id="58880-134">Il modello di identità</span><span class="sxs-lookup"><span data-stu-id="58880-134">The Identity model</span></span>

### <a name="entity-types"></a><span data-ttu-id="58880-135">Tipi di entità</span><span class="sxs-lookup"><span data-stu-id="58880-135">Entity types</span></span>

<span data-ttu-id="58880-136">Il modello di identità è costituita da tipi di entità seguente.</span><span class="sxs-lookup"><span data-stu-id="58880-136">The Identity model consists of the following entity types.</span></span>

|<span data-ttu-id="58880-137">Tipo di entità</span><span class="sxs-lookup"><span data-stu-id="58880-137">Entity type</span></span>|<span data-ttu-id="58880-138">Descrizione</span><span class="sxs-lookup"><span data-stu-id="58880-138">Description</span></span>                                                  |
|-----------|-------------------------------------------------------------|
|`User`     |<span data-ttu-id="58880-139">Rappresenta l'utente.</span><span class="sxs-lookup"><span data-stu-id="58880-139">Represents the user.</span></span>                                         |
|`Role`     |<span data-ttu-id="58880-140">Rappresenta un ruolo.</span><span class="sxs-lookup"><span data-stu-id="58880-140">Represents a role.</span></span>                                           |
|`UserClaim`|<span data-ttu-id="58880-141">Rappresenta un'attestazione che dispone di un utente.</span><span class="sxs-lookup"><span data-stu-id="58880-141">Represents a claim that a user possesses.</span></span>                    |
|`UserToken`|<span data-ttu-id="58880-142">Rappresenta un token di autenticazione per un utente.</span><span class="sxs-lookup"><span data-stu-id="58880-142">Represents an authentication token for a user.</span></span>               |
|`UserLogin`|<span data-ttu-id="58880-143">Associa un utente con un account di accesso.</span><span class="sxs-lookup"><span data-stu-id="58880-143">Associates a user with a login.</span></span>                              |
|`RoleClaim`|<span data-ttu-id="58880-144">Rappresenta un'attestazione che viene concesso a tutti gli utenti all'interno di un ruolo.</span><span class="sxs-lookup"><span data-stu-id="58880-144">Represents a claim that's granted to all users within a role.</span></span>|
|`UserRole` |<span data-ttu-id="58880-145">Un'entità di join che associano gli utenti e ruoli.</span><span class="sxs-lookup"><span data-stu-id="58880-145">A join entity that associates users and roles.</span></span>               |

### <a name="entity-type-relationships"></a><span data-ttu-id="58880-146">Relazioni di tipo di entità</span><span class="sxs-lookup"><span data-stu-id="58880-146">Entity type relationships</span></span>

<span data-ttu-id="58880-147">Il [tipi di entità](#entity-types) sono correlati tra loro nei modi seguenti:</span><span class="sxs-lookup"><span data-stu-id="58880-147">The [entity types](#entity-types) are related to each other in the following ways:</span></span>

* <span data-ttu-id="58880-148">Ciascuna `User` può avere numerosi `UserClaims`.</span><span class="sxs-lookup"><span data-stu-id="58880-148">Each `User` can have many `UserClaims`.</span></span>
* <span data-ttu-id="58880-149">Ciascuna `User` può avere numerosi `UserLogins`.</span><span class="sxs-lookup"><span data-stu-id="58880-149">Each `User` can have many `UserLogins`.</span></span>
* <span data-ttu-id="58880-150">Ciascuna `User` può avere numerosi `UserTokens`.</span><span class="sxs-lookup"><span data-stu-id="58880-150">Each `User` can have many `UserTokens`.</span></span>
* <span data-ttu-id="58880-151">Ciascuna `Role` può avere numerosi associati `RoleClaims`.</span><span class="sxs-lookup"><span data-stu-id="58880-151">Each `Role` can have many associated `RoleClaims`.</span></span>
* <span data-ttu-id="58880-152">Ciascuna `User` può avere numerosi associata `Roles`e ogni `Role` può essere associato a molti `Users`.</span><span class="sxs-lookup"><span data-stu-id="58880-152">Each `User` can have many associated `Roles`, and each `Role` can be associated with many `Users`.</span></span> <span data-ttu-id="58880-153">Questa è una relazione molti-a-molti che richiede una tabella di join del database.</span><span class="sxs-lookup"><span data-stu-id="58880-153">This is a many-to-many relationship that requires a join table in the database.</span></span> <span data-ttu-id="58880-154">La tabella di join è rappresentata dal `UserRole` entità.</span><span class="sxs-lookup"><span data-stu-id="58880-154">The join table is represented by the `UserRole` entity.</span></span>

### <a name="default-model-configuration"></a><span data-ttu-id="58880-155">Configurazione del modello predefinito</span><span class="sxs-lookup"><span data-stu-id="58880-155">Default model configuration</span></span>

<span data-ttu-id="58880-156">Identità definisce molti *le classi del contesto* che eredita dalla <xref:Microsoft.EntityFrameworkCore.DbContext> per configurare e usare il modello.</span><span class="sxs-lookup"><span data-stu-id="58880-156">Identity defines many *context classes* that inherit from <xref:Microsoft.EntityFrameworkCore.DbContext> to configure and use the model.</span></span> <span data-ttu-id="58880-157">Questa configurazione viene eseguita usando il [API Fluent primo codice di Entity Framework Core](/ef/core/modeling/) nel <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating*> metodo della classe del contesto.</span><span class="sxs-lookup"><span data-stu-id="58880-157">This configuration is done using the [EF Core Code First Fluent API](/ef/core/modeling/) in the <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating*> method of the context class.</span></span> <span data-ttu-id="58880-158">La configurazione predefinita è:</span><span class="sxs-lookup"><span data-stu-id="58880-158">The default configuration is:</span></span>

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

### <a name="model-generic-types"></a><span data-ttu-id="58880-159">Tipi generici di modello</span><span class="sxs-lookup"><span data-stu-id="58880-159">Model generic types</span></span>

<span data-ttu-id="58880-160">Identità definisce predefinito [Common Language Runtime](/dotnet/standard/glossary#clr) tipi (CLR) per ognuno dei tipi di entità elencati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="58880-160">Identity defines default [Common Language Runtime](/dotnet/standard/glossary#clr) (CLR) types for each of the entity types listed above.</span></span> <span data-ttu-id="58880-161">Questi tipi sono precedute tutte dal prefisso *identità*:</span><span class="sxs-lookup"><span data-stu-id="58880-161">These types are all prefixed with *Identity*:</span></span>

* `IdentityUser`
* `IdentityRole`
* `IdentityUserClaim`
* `IdentityUserToken`
* `IdentityUserLogin`
* `IdentityRoleClaim`
* `IdentityUserRole`

<span data-ttu-id="58880-162">Invece di usare direttamente questi tipi, i tipi sono utilizzabile come classi di base per i tipi dell'app.</span><span class="sxs-lookup"><span data-stu-id="58880-162">Rather than using these types directly, the types can be used as base classes for the app's own types.</span></span> <span data-ttu-id="58880-163">Il `DbContext` classi definite dall'identità sono generiche, tale che i tipi CLR diversi possono essere utilizzati per una o più dei tipi di entità nel modello.</span><span class="sxs-lookup"><span data-stu-id="58880-163">The `DbContext` classes defined by Identity are generic, such that different CLR types can be used for one or more of the entity types in the model.</span></span> <span data-ttu-id="58880-164">Questi tipi generici consentono anche di `User` (PK) dei dati tipo di chiave primaria da modificare.</span><span class="sxs-lookup"><span data-stu-id="58880-164">These generic types also allow the `User` primary key (PK) data type to be changed.</span></span>

<span data-ttu-id="58880-165">Quando si usa l'identità con il supporto per i ruoli, un <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext> classe deve essere usata.</span><span class="sxs-lookup"><span data-stu-id="58880-165">When using Identity with support for roles, an <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext> class should be used.</span></span> <span data-ttu-id="58880-166">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="58880-166">For example:</span></span>

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

<span data-ttu-id="58880-167">È anche possibile usare l'identità senza ruoli (solo attestazioni), nel qual caso un <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext%601> classe deve essere utilizzata:</span><span class="sxs-lookup"><span data-stu-id="58880-167">It's also possible to use Identity without roles (only claims), in which case an <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext%601> class should be used:</span></span>

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

## <a name="customize-the-model"></a><span data-ttu-id="58880-168">Personalizzare il modello</span><span class="sxs-lookup"><span data-stu-id="58880-168">Customize the model</span></span>

<span data-ttu-id="58880-169">Il punto di partenza per la personalizzazione del modello consiste nel derivare dal tipo di contesto appropriate.</span><span class="sxs-lookup"><span data-stu-id="58880-169">The starting point for model customization is to derive from the appropriate context type.</span></span> <span data-ttu-id="58880-170">Vedere le [modellare tipi generici](#model-generic-types) sezione.</span><span class="sxs-lookup"><span data-stu-id="58880-170">See the [Model generic types](#model-generic-types) section.</span></span> <span data-ttu-id="58880-171">Questo tipo di contesto viene abitualmente chiamato `ApplicationDbContext` e viene creato tramite i modelli ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="58880-171">This context type is customarily called `ApplicationDbContext` and is created by the ASP.NET Core templates.</span></span>

<span data-ttu-id="58880-172">Il contesto viene utilizzato per configurare il modello in due modi:</span><span class="sxs-lookup"><span data-stu-id="58880-172">The context is used to configure the model in two ways:</span></span>

* <span data-ttu-id="58880-173">Fornendo entità e tipi di chiave per i parametri di tipo generico.</span><span class="sxs-lookup"><span data-stu-id="58880-173">Supplying entity and key types for the generic type parameters.</span></span>
* <span data-ttu-id="58880-174">Si esegue l'override `OnModelCreating` per modificare il mapping di questi tipi.</span><span class="sxs-lookup"><span data-stu-id="58880-174">Overriding `OnModelCreating` to modify the mapping of these types.</span></span>

<span data-ttu-id="58880-175">Quando si esegue l'override `OnModelCreating`, `base.OnModelCreating` deve essere chiamato per primo; la configurazione di override deve essere chiamata successivamente.</span><span class="sxs-lookup"><span data-stu-id="58880-175">When overriding `OnModelCreating`, `base.OnModelCreating` should be called first; the overriding configuration should be called next.</span></span> <span data-ttu-id="58880-176">EF Core ha in genere un criterio last-quello-wins per la configurazione.</span><span class="sxs-lookup"><span data-stu-id="58880-176">EF Core generally has a last-one-wins policy for configuration.</span></span> <span data-ttu-id="58880-177">Ad esempio, se il `ToTable` per un tipo di entità viene chiamato prima di tutto con il nome di una tabella e quindi nuovamente in seguito con un nome di tabella diversa, il nome della tabella nella seconda chiamata verrà usato.</span><span class="sxs-lookup"><span data-stu-id="58880-177">For example, if the `ToTable` method for an entity type is called first with one table name and then again later with a different table name, the table name in the second call is used.</span></span>

### <a name="custom-user-data"></a><span data-ttu-id="58880-178">Dati utente personalizzati</span><span class="sxs-lookup"><span data-stu-id="58880-178">Custom user data</span></span>

<!--
set projNam=WebApp1
dotnet new webapp -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design 
dotnet aspnet-codegenerator identity  -dc ApplicationDbContext --useDefaultUI 
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
 -->

<span data-ttu-id="58880-179">[Dati utente personalizzati](xref:security/authentication/add-user-data) è supportata tramite l'eredità dalla `IdentityUser`.</span><span class="sxs-lookup"><span data-stu-id="58880-179">[Custom user data](xref:security/authentication/add-user-data) is supported by inheriting from `IdentityUser`.</span></span> <span data-ttu-id="58880-180">Solitamente si per assegnare un nome di questo tipo `ApplicationUser`:</span><span class="sxs-lookup"><span data-stu-id="58880-180">It's customary to name this type `ApplicationUser`:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

<span data-ttu-id="58880-181">Usare il `ApplicationUser` tipo come argomento generico per il contesto:</span><span class="sxs-lookup"><span data-stu-id="58880-181">Use the `ApplicationUser` type as a generic argument for the context:</span></span>

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

<span data-ttu-id="58880-182">Non è necessario eseguire l'override `OnModelCreating` nella `ApplicationDbContext` classe.</span><span class="sxs-lookup"><span data-stu-id="58880-182">There's no need to override `OnModelCreating` in the `ApplicationDbContext` class.</span></span> <span data-ttu-id="58880-183">Esegue il mapping di Entity Framework Core le `CustomTag` proprietà in base alla convenzione.</span><span class="sxs-lookup"><span data-stu-id="58880-183">EF Core maps the `CustomTag` property by convention.</span></span> <span data-ttu-id="58880-184">Tuttavia, il database deve essere aggiornato per creare un nuovo `CustomTag` colonna.</span><span class="sxs-lookup"><span data-stu-id="58880-184">However, the database needs to be updated to create a new `CustomTag` column.</span></span> <span data-ttu-id="58880-185">Per creare la colonna, aggiungere una migrazione e quindi aggiornare il database come descritto in [identità e le migrazioni di EF Core](#identity-and-ef-core-migrations).</span><span class="sxs-lookup"><span data-stu-id="58880-185">To create the column, add a migration, and then update the database as described in [Identity and EF Core Migrations](#identity-and-ef-core-migrations).</span></span>

<span data-ttu-id="58880-186">Update *Pages/Shared/_LoginPartial.cshtml* e sostituire `IdentityUser` con `ApplicationUser`:</span><span class="sxs-lookup"><span data-stu-id="58880-186">Update *Pages/Shared/_LoginPartial.cshtml* and replace `IdentityUser` with `ApplicationUser`:</span></span>

```cshtml
@using Microsoft.AspNetCore.Identity
@using WebApp1.Areas.Identity.Data
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager
```

<span data-ttu-id="58880-187">Update *Areas/Identity/IdentityHostingStartup.cs* oppure `Startup.ConfigureServices` e sostituire `IdentityUser` con `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="58880-187">Update *Areas/Identity/IdentityHostingStartup.cs*  or `Startup.ConfigureServices` and replace `IdentityUser` with `ApplicationUser`.</span></span>

```csharp
services.AddDefaultIdentity<ApplicationUser>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultUI();
```

<span data-ttu-id="58880-188">In ASP.NET Core 2.1 o versioni successive, l'identità viene fornita come una libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="58880-188">In ASP.NET Core 2.1 or later, Identity is provided as a Razor Class Library.</span></span> <span data-ttu-id="58880-189">Per altre informazioni, vedere <xref:security/authentication/scaffold-identity>.</span><span class="sxs-lookup"><span data-stu-id="58880-189">For more information, see <xref:security/authentication/scaffold-identity>.</span></span> <span data-ttu-id="58880-190">Di conseguenza, il codice precedente richiede una chiamata a <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span><span class="sxs-lookup"><span data-stu-id="58880-190">Consequently, the preceding code requires a call to <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span></span> <span data-ttu-id="58880-191">Se l'utilità di scaffolding di identità è stata usata per aggiungere i file di identità per il progetto, rimuovere la chiamata a `AddDefaultUI`.</span><span class="sxs-lookup"><span data-stu-id="58880-191">If the Identity scaffolder was used to add Identity files to the project, remove the call to `AddDefaultUI`.</span></span> <span data-ttu-id="58880-192">Per altre informazioni, vedere:</span><span class="sxs-lookup"><span data-stu-id="58880-192">For more information, see:</span></span>

* [<span data-ttu-id="58880-193">Scaffolding dell'identità</span><span class="sxs-lookup"><span data-stu-id="58880-193">Scaffold Identity</span></span>](xref:security/authentication/scaffold-identity)
* [<span data-ttu-id="58880-194">Aggiungere, scaricare ed eliminare i dati utente personalizzati per Identity</span><span class="sxs-lookup"><span data-stu-id="58880-194">Add, download, and delete custom user data to Identity</span></span>](xref:security/authentication/add-user-data)

### <a name="change-the-primary-key-type"></a><span data-ttu-id="58880-195">Modificare il tipo di chiave primaria</span><span class="sxs-lookup"><span data-stu-id="58880-195">Change the primary key type</span></span>

<span data-ttu-id="58880-196">Una modifica al tipo di dati della colonna chiave primaria dopo aver creato il database rappresenta un problema in molti sistemi di database.</span><span class="sxs-lookup"><span data-stu-id="58880-196">A change to the PK column's data type after the database has been created is problematic on many database systems.</span></span> <span data-ttu-id="58880-197">La modifica la chiave primaria in genere implica eliminare e ricreare la tabella.</span><span class="sxs-lookup"><span data-stu-id="58880-197">Changing the PK typically involves dropping and re-creating the table.</span></span> <span data-ttu-id="58880-198">Di conseguenza, i tipi chiave devono essere specificati per la migrazione iniziale quando viene creato il database.</span><span class="sxs-lookup"><span data-stu-id="58880-198">Therefore, key types should be specified in the initial migration when the database is created.</span></span>

<span data-ttu-id="58880-199">Seguire questi passaggi per modificare il tipo di chiave primaria:</span><span class="sxs-lookup"><span data-stu-id="58880-199">Follow these steps to change the PK type:</span></span>

1. <span data-ttu-id="58880-200">Se il database è stato creato prima della modifica della chiave primaria, eseguire `Drop-Database` (console di gestione pacchetti) o `dotnet ef database drop` (CLI di .NET Core) per eliminarlo.</span><span class="sxs-lookup"><span data-stu-id="58880-200">If the database was created before the PK change, run `Drop-Database` (PMC) or `dotnet ef database drop` (.NET Core CLI) to delete it.</span></span>
2. <span data-ttu-id="58880-201">Dopo la conferma dell'eliminazione del database, rimuovere la migrazione iniziale con `Remove-Migration` (console di gestione pacchetti) o `dotnet ef migrations remove` (CLI di .NET Core).</span><span class="sxs-lookup"><span data-stu-id="58880-201">After confirming deletion of the database, remove the initial migration with `Remove-Migration` (PMC) or `dotnet ef migrations remove` (.NET Core CLI).</span></span>
3. <span data-ttu-id="58880-202">Aggiorna il `ApplicationDbContext` classe derivandola da <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext%603>.</span><span class="sxs-lookup"><span data-stu-id="58880-202">Update the `ApplicationDbContext` class to derive from <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext%603>.</span></span> <span data-ttu-id="58880-203">Specificare il nuovo tipo di chiave per `TKey`.</span><span class="sxs-lookup"><span data-stu-id="58880-203">Specify the new key type for `TKey`.</span></span> <span data-ttu-id="58880-204">Ad esempio, per usare un `Guid` tipo di chiave:</span><span class="sxs-lookup"><span data-stu-id="58880-204">For example, to use a `Guid` key type:</span></span>

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

    <span data-ttu-id="58880-205">Nel codice precedente, le classi generiche <xref:Microsoft.AspNetCore.Identity.IdentityUser%601> e <xref:Microsoft.AspNetCore.Identity.IdentityRole%601> devono essere specificate per usare il nuovo tipo di chiave.</span><span class="sxs-lookup"><span data-stu-id="58880-205">In the preceding code, the generic classes <xref:Microsoft.AspNetCore.Identity.IdentityUser%601> and <xref:Microsoft.AspNetCore.Identity.IdentityRole%601> must be specified to use the new key type.</span></span>

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    <span data-ttu-id="58880-206">Nel codice precedente, le classi generiche <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser%601> e <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole%601> devono essere specificate per usare il nuovo tipo di chiave.</span><span class="sxs-lookup"><span data-stu-id="58880-206">In the preceding code, the generic classes <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser%601> and <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole%601> must be specified to use the new key type.</span></span>

    ::: moniker-end

    <span data-ttu-id="58880-207">`Startup.ConfigureServices` deve essere aggiornato per usare l'utente generico:</span><span class="sxs-lookup"><span data-stu-id="58880-207">`Startup.ConfigureServices` must be updated to use the generic user:</span></span>

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

4. <span data-ttu-id="58880-208">Se una classe personalizzata `ApplicationUser` classe viene utilizzata, aggiornare la classe da cui ereditare `IdentityUser`.</span><span class="sxs-lookup"><span data-stu-id="58880-208">If a custom `ApplicationUser` class is being used, update the class to inherit from `IdentityUser`.</span></span> <span data-ttu-id="58880-209">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="58880-209">For example:</span></span>

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Models/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    ::: moniker range=">= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    <span data-ttu-id="58880-210">Update `ApplicationDbContext` a cui fare riferimento l'oggetto personalizzato `ApplicationUser` classe:</span><span class="sxs-lookup"><span data-stu-id="58880-210">Update `ApplicationDbContext` to reference the custom `ApplicationUser` class:</span></span>

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

    <span data-ttu-id="58880-211">Registrare la classe del contesto del database personalizzate quando si aggiunge il servizio di identità in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="58880-211">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<ApplicationUser>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultUI()
            .AddDefaultTokenProviders();
    ```

    <span data-ttu-id="58880-212">Tipo di dati della chiave primaria viene dedotto analizzando il <xref:Microsoft.EntityFrameworkCore.DbContext> oggetto.</span><span class="sxs-lookup"><span data-stu-id="58880-212">The primary key's data type is inferred by analyzing the <xref:Microsoft.EntityFrameworkCore.DbContext> object.</span></span>

    <span data-ttu-id="58880-213">In ASP.NET Core 2.1 o versioni successive, l'identità viene fornita come una libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="58880-213">In ASP.NET Core 2.1 or later, Identity is provided as a Razor Class Library.</span></span> <span data-ttu-id="58880-214">Per altre informazioni, vedere <xref:security/authentication/scaffold-identity>.</span><span class="sxs-lookup"><span data-stu-id="58880-214">For more information, see <xref:security/authentication/scaffold-identity>.</span></span> <span data-ttu-id="58880-215">Di conseguenza, il codice precedente richiede una chiamata a <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span><span class="sxs-lookup"><span data-stu-id="58880-215">Consequently, the preceding code requires a call to <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span></span> <span data-ttu-id="58880-216">Se l'utilità di scaffolding di identità è stata usata per aggiungere i file di identità per il progetto, rimuovere la chiamata a `AddDefaultUI`.</span><span class="sxs-lookup"><span data-stu-id="58880-216">If the Identity scaffolder was used to add Identity files to the project, remove the call to `AddDefaultUI`.</span></span>

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    <span data-ttu-id="58880-217">Tipo di dati della chiave primaria viene dedotto analizzando il <xref:Microsoft.EntityFrameworkCore.DbContext> oggetto.</span><span class="sxs-lookup"><span data-stu-id="58880-217">The primary key's data type is inferred by analyzing the <xref:Microsoft.EntityFrameworkCore.DbContext> object.</span></span>

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    <span data-ttu-id="58880-218">Il <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> metodo accetta un `TKey` tipo che indica il tipo di dati della chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="58880-218">The <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> method accepts a `TKey` type indicating the primary key's data type.</span></span>

    ::: moniker-end

5. <span data-ttu-id="58880-219">Se una classe personalizzata `ApplicationRole` classe viene utilizzata, aggiornare la classe da cui ereditare `IdentityRole<TKey>`.</span><span class="sxs-lookup"><span data-stu-id="58880-219">If a custom `ApplicationRole` class is being used, update the class to inherit from `IdentityRole<TKey>`.</span></span> <span data-ttu-id="58880-220">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="58880-220">For example:</span></span>

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationRole.cs?name=snippet_ApplicationRole&highlight=4)]

    <span data-ttu-id="58880-221">Update `ApplicationDbContext` a cui fare riferimento l'oggetto personalizzato `ApplicationRole` classe.</span><span class="sxs-lookup"><span data-stu-id="58880-221">Update `ApplicationDbContext` to reference the custom `ApplicationRole` class.</span></span> <span data-ttu-id="58880-222">Ad esempio, la classe seguente fa riferimento a una classe personalizzata `ApplicationUser` e un oggetto personalizzato `ApplicationRole`:</span><span class="sxs-lookup"><span data-stu-id="58880-222">For example, the following class references a custom `ApplicationUser` and a custom `ApplicationRole`:</span></span>

    ::: moniker range=">= aspnetcore-2.1"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    <span data-ttu-id="58880-223">Registrare la classe del contesto del database personalizzate quando si aggiunge il servizio di identità in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="58880-223">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=13-16)]

    <span data-ttu-id="58880-224">Tipo di dati della chiave primaria viene dedotto analizzando il <xref:Microsoft.EntityFrameworkCore.DbContext> oggetto.</span><span class="sxs-lookup"><span data-stu-id="58880-224">The primary key's data type is inferred by analyzing the <xref:Microsoft.EntityFrameworkCore.DbContext> object.</span></span>

    <span data-ttu-id="58880-225">In ASP.NET Core 2.1 o versioni successive, l'identità viene fornita come una libreria di classi Razor.</span><span class="sxs-lookup"><span data-stu-id="58880-225">In ASP.NET Core 2.1 or later, Identity is provided as a Razor Class Library.</span></span> <span data-ttu-id="58880-226">Per altre informazioni, vedere <xref:security/authentication/scaffold-identity>.</span><span class="sxs-lookup"><span data-stu-id="58880-226">For more information, see <xref:security/authentication/scaffold-identity>.</span></span> <span data-ttu-id="58880-227">Di conseguenza, il codice precedente richiede una chiamata a <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span><span class="sxs-lookup"><span data-stu-id="58880-227">Consequently, the preceding code requires a call to <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span></span> <span data-ttu-id="58880-228">Se l'utilità di scaffolding di identità è stata usata per aggiungere i file di identità per il progetto, rimuovere la chiamata a `AddDefaultUI`.</span><span class="sxs-lookup"><span data-stu-id="58880-228">If the Identity scaffolder was used to add Identity files to the project, remove the call to `AddDefaultUI`.</span></span>

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    <span data-ttu-id="58880-229">Registrare la classe del contesto del database personalizzate quando si aggiunge il servizio di identità in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="58880-229">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    <span data-ttu-id="58880-230">Tipo di dati della chiave primaria viene dedotto analizzando il <xref:Microsoft.EntityFrameworkCore.DbContext> oggetto.</span><span class="sxs-lookup"><span data-stu-id="58880-230">The primary key's data type is inferred by analyzing the <xref:Microsoft.EntityFrameworkCore.DbContext> object.</span></span>

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    <span data-ttu-id="58880-231">Registrare la classe del contesto del database personalizzate quando si aggiunge il servizio di identità in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="58880-231">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    <span data-ttu-id="58880-232">Il <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> metodo accetta un `TKey` tipo che indica il tipo di dati della chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="58880-232">The <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> method accepts a `TKey` type indicating the primary key's data type.</span></span>

    ::: moniker-end

### <a name="add-navigation-properties"></a><span data-ttu-id="58880-233">Aggiungere le proprietà di navigazione</span><span class="sxs-lookup"><span data-stu-id="58880-233">Add navigation properties</span></span>

<span data-ttu-id="58880-234">Modifica della configurazione del modello per le relazioni possa essere più difficile apportare altre modifiche.</span><span class="sxs-lookup"><span data-stu-id="58880-234">Changing the model configuration for relationships can be more difficult than making other changes.</span></span> <span data-ttu-id="58880-235">Prestare attenzione a sostituire le relazioni esistenti anziché crearne uno nuovo, relazioni aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="58880-235">Care must be taken to replace the existing relationships rather than create new, additional relationships.</span></span> <span data-ttu-id="58880-236">In particolare, la relazione modificata deve specificare stessa proprietà della chiave esterna (FK) come la relazione esistente.</span><span class="sxs-lookup"><span data-stu-id="58880-236">In particular, the changed relationship must specify the same foreign key (FK) property as the existing relationship.</span></span> <span data-ttu-id="58880-237">Ad esempio, la relazione tra `Users` e `UserClaims` è, per impostazione predefinita, specificata come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="58880-237">For example, the relationship between `Users` and `UserClaims` is, by default, specified as follows:</span></span>

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

<span data-ttu-id="58880-238">La chiave esterna per questa relazione viene specificata come il `UserClaim.UserId` proprietà.</span><span class="sxs-lookup"><span data-stu-id="58880-238">The FK for this relationship is specified as the `UserClaim.UserId` property.</span></span> <span data-ttu-id="58880-239">`HasMany` e `WithOne` vengono chiamati senza argomenti per creare la relazione senza proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="58880-239">`HasMany` and `WithOne` are called without arguments to create the relationship without navigation properties.</span></span>

<span data-ttu-id="58880-240">Aggiungere una proprietà di navigazione `ApplicationUser` che consente l'associazione `UserClaims` per farvi riferimento da parte dell'utente:</span><span class="sxs-lookup"><span data-stu-id="58880-240">Add a navigation property to `ApplicationUser` that allows associated `UserClaims` to be referenced from the user:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

<span data-ttu-id="58880-241">Il `TKey` per `IdentityUserClaim<TKey>` è del tipo specificato per la chiave primaria degli utenti.</span><span class="sxs-lookup"><span data-stu-id="58880-241">The `TKey` for `IdentityUserClaim<TKey>` is the type specified for the PK of users.</span></span> <span data-ttu-id="58880-242">In questo caso `TKey` è `string` perché vengono utilizzati i valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="58880-242">In this case, `TKey` is `string` because the defaults are being used.</span></span> <span data-ttu-id="58880-243">Dispone **non** il tipo di chiave primaria per la `UserClaim` tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="58880-243">It's **not** the PK type for the `UserClaim` entity type.</span></span>

<span data-ttu-id="58880-244">Ora che la proprietà di navigazione è presente, deve essere configurato `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="58880-244">Now that the navigation property exists, it must be configured in `OnModelCreating`:</span></span>

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

<span data-ttu-id="58880-245">Si noti che è configurata relazione esattamente com'era prima, solo con una proprietà di navigazione specificata nella chiamata a `HasMany`.</span><span class="sxs-lookup"><span data-stu-id="58880-245">Notice that relationship is configured exactly as it was before, only with a navigation property specified in the call to `HasMany`.</span></span>

<span data-ttu-id="58880-246">Le proprietà di navigazione esistono solo nel modello di Entity Framework, non il database.</span><span class="sxs-lookup"><span data-stu-id="58880-246">The navigation properties only exist in the EF model, not the database.</span></span> <span data-ttu-id="58880-247">Poiché la chiave esterna per la relazione non è stato modificato, questo tipo di modifica del modello non richiede il database da aggiornare.</span><span class="sxs-lookup"><span data-stu-id="58880-247">Because the FK for the relationship hasn't changed, this kind of model change doesn't require the database to be updated.</span></span> <span data-ttu-id="58880-248">Ciò può essere controllato mediante l'aggiunta di una migrazione dopo aver apportato la modifica.</span><span class="sxs-lookup"><span data-stu-id="58880-248">This can be checked by adding a migration after making the change.</span></span> <span data-ttu-id="58880-249">Il `Up` e `Down` metodi sono vuoti.</span><span class="sxs-lookup"><span data-stu-id="58880-249">The `Up` and `Down` methods are empty.</span></span>

### <a name="add-all-user-navigation-properties"></a><span data-ttu-id="58880-250">Aggiungere tutte le proprietà di navigazione di utente</span><span class="sxs-lookup"><span data-stu-id="58880-250">Add all User navigation properties</span></span>

<span data-ttu-id="58880-251">Come prassi consigliata, usando la sezione precedente, l'esempio seguente configura le proprietà di navigazione unidirezionale per tutte le relazioni di utente:</span><span class="sxs-lookup"><span data-stu-id="58880-251">Using the section above as guidance, the following example configures unidirectional navigation properties for all relationships on User:</span></span>

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

### <a name="add-user-and-role-navigation-properties"></a><span data-ttu-id="58880-252">Aggiungere le proprietà di navigazione utenti e ruoli</span><span class="sxs-lookup"><span data-stu-id="58880-252">Add User and Role navigation properties</span></span>

<span data-ttu-id="58880-253">Come prassi consigliata, usando la sezione precedente, l'esempio seguente configura le proprietà di navigazione per tutte le relazioni per l'utente e ruolo:</span><span class="sxs-lookup"><span data-stu-id="58880-253">Using the section above as guidance, the following example configures navigation properties for all relationships on User and Role:</span></span>

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

<span data-ttu-id="58880-254">Note:</span><span class="sxs-lookup"><span data-stu-id="58880-254">Notes:</span></span>

* <span data-ttu-id="58880-255">Questo esempio include anche il `UserRole` aggiungere entità, che è necessario per esplorare la relazione molti-a-molti utenti ai ruoli.</span><span class="sxs-lookup"><span data-stu-id="58880-255">This example also includes the `UserRole` join entity, which is needed to navigate the many-to-many relationship from Users to Roles.</span></span>
* <span data-ttu-id="58880-256">Ricordarsi di modificare i tipi delle proprietà di navigazione ciò sia rispecchiato `ApplicationXxx` vengono ora utilizzati tipi anziché `IdentityXxx` tipi.</span><span class="sxs-lookup"><span data-stu-id="58880-256">Remember to change the types of the navigation properties to reflect that `ApplicationXxx` types are now being used instead of `IdentityXxx` types.</span></span>
* <span data-ttu-id="58880-257">Ricordarsi di usare la `ApplicationXxx` nel ma generic `ApplicationContext` definizione.</span><span class="sxs-lookup"><span data-stu-id="58880-257">Remember to use the `ApplicationXxx` in the generic `ApplicationContext` definition.</span></span>

### <a name="add-all-navigation-properties"></a><span data-ttu-id="58880-258">Aggiungere tutte le proprietà di navigazione</span><span class="sxs-lookup"><span data-stu-id="58880-258">Add all navigation properties</span></span>

<span data-ttu-id="58880-259">Come prassi consigliata, usando la sezione precedente, l'esempio seguente configura le proprietà di navigazione per tutte le relazioni in tutti i tipi di entità:</span><span class="sxs-lookup"><span data-stu-id="58880-259">Using the section above as guidance, the following example configures navigation properties for all relationships on all entity types:</span></span>

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

### <a name="use-composite-keys"></a><span data-ttu-id="58880-260">Utilizzare chiavi composte</span><span class="sxs-lookup"><span data-stu-id="58880-260">Use composite keys</span></span>

<span data-ttu-id="58880-261">Nelle sezioni precedenti è illustrata la modifica del tipo della chiave utilizzata nel modello di identità.</span><span class="sxs-lookup"><span data-stu-id="58880-261">The preceding sections demonstrated changing the type of key used in the Identity model.</span></span> <span data-ttu-id="58880-262">La modifica del modello di chiave di identità per usare una chiave composta non è supportata o consigliata.</span><span class="sxs-lookup"><span data-stu-id="58880-262">Changing the Identity key model to use composite keys isn't supported or recommended.</span></span> <span data-ttu-id="58880-263">Uso di una chiave composta con identità comporta la modifica come il codice di Identity manager interagisce con il modello.</span><span class="sxs-lookup"><span data-stu-id="58880-263">Using a composite key with Identity involves changing how the Identity manager code interacts with the model.</span></span> <span data-ttu-id="58880-264">Questa personalizzazione esula dall'ambito di questo documento.</span><span class="sxs-lookup"><span data-stu-id="58880-264">This customization is beyond the scope of this document.</span></span>

### <a name="change-tablecolumn-names-and-facets"></a><span data-ttu-id="58880-265">Modificare i nomi di tabella/colonna e i facet</span><span class="sxs-lookup"><span data-stu-id="58880-265">Change table/column names and facets</span></span>

<span data-ttu-id="58880-266">Per modificare i nomi delle tabelle e colonne, chiamare `base.OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="58880-266">To change the names of tables and columns, call `base.OnModelCreating`.</span></span> <span data-ttu-id="58880-267">Aggiungere quindi della configurazione per eseguire l'override dei valori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="58880-267">Then, add configuration to override any of the defaults.</span></span> <span data-ttu-id="58880-268">Ad esempio, per modificare il nome di tutte le tabelle di identità:</span><span class="sxs-lookup"><span data-stu-id="58880-268">For example, to change the name of all the Identity tables:</span></span>

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

<span data-ttu-id="58880-269">Questi esempi utilizzano i tipi di identità predefinito.</span><span class="sxs-lookup"><span data-stu-id="58880-269">These examples use the default Identity types.</span></span> <span data-ttu-id="58880-270">Se si usa un tipo di app, ad esempio `ApplicationUser`, configurare quel tipo anziché al tipo predefinito.</span><span class="sxs-lookup"><span data-stu-id="58880-270">If using an app type such as `ApplicationUser`, configure that type instead of the default type.</span></span>

<span data-ttu-id="58880-271">L'esempio seguente modifica alcuni nomi di colonna:</span><span class="sxs-lookup"><span data-stu-id="58880-271">The following example changes some column names:</span></span>

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

<span data-ttu-id="58880-272">Alcuni tipi di colonne del database possono essere configurati con determinate *facet* (ad esempio, il valore massimo `string` lunghezza consentita).</span><span class="sxs-lookup"><span data-stu-id="58880-272">Some types of database columns can be configured with certain *facets* (for example, the maximum `string` length allowed).</span></span> <span data-ttu-id="58880-273">L'esempio seguente imposta lunghezza massima delle colonne per diversi `string` proprietà nel modello:</span><span class="sxs-lookup"><span data-stu-id="58880-273">The following example sets column maximum lengths for several `string` properties in the model:</span></span>

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

### <a name="map-to-a-different-schema"></a><span data-ttu-id="58880-274">Eseguire il mapping a uno schema diverso</span><span class="sxs-lookup"><span data-stu-id="58880-274">Map to a different schema</span></span>

<span data-ttu-id="58880-275">Gli schemi possono avere un comportamento diverso tra i provider di database.</span><span class="sxs-lookup"><span data-stu-id="58880-275">Schemas can behave differently across database providers.</span></span> <span data-ttu-id="58880-276">Per SQL Server, il valore predefinito consiste nel creare tutte le tabelle di *dbo* dello schema.</span><span class="sxs-lookup"><span data-stu-id="58880-276">For SQL Server, the default is to create all tables in the *dbo* schema.</span></span> <span data-ttu-id="58880-277">È possibile creare le tabelle in uno schema diverso.</span><span class="sxs-lookup"><span data-stu-id="58880-277">The tables can be created in a different schema.</span></span> <span data-ttu-id="58880-278">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="58880-278">For example:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

::: moniker range=">= aspnetcore-2.1"

### <a name="lazy-loading"></a><span data-ttu-id="58880-279">Caricamento lazy</span><span class="sxs-lookup"><span data-stu-id="58880-279">Lazy loading</span></span>

<span data-ttu-id="58880-280">In questa sezione viene aggiunto il supporto per i proxy di caricamento lazy nel modello di identità.</span><span class="sxs-lookup"><span data-stu-id="58880-280">In this section, support for lazy-loading proxies in the Identity model is added.</span></span> <span data-ttu-id="58880-281">Il caricamento lazy è utile perché consente le proprietà di navigazione da utilizzare senza prima verificare che sta caricati.</span><span class="sxs-lookup"><span data-stu-id="58880-281">Lazy-loading is useful since it allows navigation properties to be used without first ensuring they're loaded.</span></span>

<span data-ttu-id="58880-282">I tipi di entità possono essere reso adatti per caricamento lazy in diversi modi, come descritto nel [documentazione di Entity Framework Core](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="58880-282">Entity types can be made suitable for lazy-loading in several ways, as described in the [EF Core documentation](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="58880-283">Per semplicità, usare un proxy di caricamento lazy, che richiede:</span><span class="sxs-lookup"><span data-stu-id="58880-283">For simplicity, use lazy-loading proxies, which requires:</span></span>

* <span data-ttu-id="58880-284">Installazione del [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="58880-284">Installation of the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package.</span></span>
* <span data-ttu-id="58880-285">Una chiamata a <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*> all'interno di <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*>.</span><span class="sxs-lookup"><span data-stu-id="58880-285">A call to <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*> inside <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*>.</span></span>
* <span data-ttu-id="58880-286">Tipi di entità pubbliche con `public virtual` le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="58880-286">Public entity types with `public virtual` navigation properties.</span></span>

<span data-ttu-id="58880-287">Nell'esempio seguente viene illustrato come chiamare `UseLazyLoadingProxies` in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="58880-287">The following example demonstrates calling `UseLazyLoadingProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
              .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="58880-288">Vedere gli esempi precedenti, per indicazioni su come aggiungere le proprietà di navigazione per i tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="58880-288">Refer to the preceding examples for guidance on adding navigation properties to the entity types.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="58880-289">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="58880-289">Additional resources</span></span>

* <xref:security/authentication/scaffold-identity>

::: moniker-end
