---
title: Provider di archiviazione chiavi in ASP.NET Core
author: rick-anderson
description: Informazioni sui provider di archiviazione chiavi in ASP.NET Core e su come configurare i percorsi di archiviazione delle chiavi.
ms.author: riande
ms.date: 12/06/2018
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: e10271d5979b503a8a842f8866a0e2a3fa040656
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121453"
---
# <a name="key-storage-providers-in-aspnet-core"></a><span data-ttu-id="ddc7c-103">Provider di archiviazione chiavi in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ddc7c-103">Key storage providers in ASP.NET Core</span></span>

<span data-ttu-id="ddc7c-104">Il sistema di protezione dati [Usa un meccanismo di individuazione per impostazione predefinita](xref:security/data-protection/configuration/default-settings) per determinare dove le chiavi di crittografia devono essere persistente.</span><span class="sxs-lookup"><span data-stu-id="ddc7c-104">The data protection system [employs a discovery mechanism by default](xref:security/data-protection/configuration/default-settings) to determine where cryptographic keys should be persisted.</span></span> <span data-ttu-id="ddc7c-105">Lo sviluppatore può ignorare il meccanismo di individuazione predefinito e specificare manualmente il percorso.</span><span class="sxs-lookup"><span data-stu-id="ddc7c-105">The developer can override the default discovery mechanism and manually specify the location.</span></span>

> [!WARNING]
> <span data-ttu-id="ddc7c-106">Se si specifica un percorso di persistenza chiave esplicite, il sistema di protezione dati Annulla la registrazione di crittografia della chiave predefinita al meccanismo di rest, in modo che le chiavi non vengono più crittografate a riposo.</span><span class="sxs-lookup"><span data-stu-id="ddc7c-106">If you specify an explicit key persistence location, the data protection system deregisters the default key encryption at rest mechanism, so keys are no longer encrypted at rest.</span></span> <span data-ttu-id="ddc7c-107">È consigliabile che è inoltre [specifica un meccanismo di crittografia della chiave esplicite](xref:security/data-protection/implementation/key-encryption-at-rest) per distribuzioni di produzione.</span><span class="sxs-lookup"><span data-stu-id="ddc7c-107">It's recommended that you additionally [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest) for production deployments.</span></span>

## <a name="file-system"></a><span data-ttu-id="ddc7c-108">File system</span><span class="sxs-lookup"><span data-stu-id="ddc7c-108">File system</span></span>

<span data-ttu-id="ddc7c-109">Per configurare un repository chiave basata sul sistema di file, chiamare il [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) routine di configurazione come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="ddc7c-109">To configure a file system-based key repository, call the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) configuration routine as shown below.</span></span> <span data-ttu-id="ddc7c-110">Fornire una [DirectoryInfo](/dotnet/api/system.io.directoryinfo) che punta al repository in cui le chiavi devono essere archiviate:</span><span class="sxs-lookup"><span data-stu-id="ddc7c-110">Provide a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) pointing to the repository where keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

<span data-ttu-id="ddc7c-111">Il `DirectoryInfo` può puntare a una directory nel computer locale oppure può puntare a una cartella in una condivisione di rete.</span><span class="sxs-lookup"><span data-stu-id="ddc7c-111">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="ddc7c-112">Se punta a una directory nel computer locale e lo scenario è che solo le app nel computer locale richiedono l'accesso per usare questo repository, è consigliabile usare [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (on Windows) per crittografare le chiavi a riposo.</span><span class="sxs-lookup"><span data-stu-id="ddc7c-112">If pointing to a directory on the local machine (and the scenario is that only apps on the local machine require access to use this repository), consider using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (on Windows) to encrypt the keys at rest.</span></span> <span data-ttu-id="ddc7c-113">In caso contrario, è consigliabile usare un [certificato X.509](xref:security/data-protection/implementation/key-encryption-at-rest) per crittografare le chiavi a riposo.</span><span class="sxs-lookup"><span data-stu-id="ddc7c-113">Otherwise, consider using an [X.509 certificate](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-and-redis"></a><span data-ttu-id="ddc7c-114">Azure e Redis</span><span class="sxs-lookup"><span data-stu-id="ddc7c-114">Azure and Redis</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ddc7c-115">Il [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) e [Microsoft.AspNetCore.DataProtection.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) questi pacchetti consentono l'archiviazione delle chiavi di protezione dati in archiviazione di Azure o un Redis servizio cache.</span><span class="sxs-lookup"><span data-stu-id="ddc7c-115">The [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) and [Microsoft.AspNetCore.DataProtection.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) packages allow storing data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="ddc7c-116">Le chiavi possono essere condivisi tra più istanze di un'app web.</span><span class="sxs-lookup"><span data-stu-id="ddc7c-116">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="ddc7c-117">Le app possono condividere i cookie di autenticazione o CSRF protection tra più server.</span><span class="sxs-lookup"><span data-stu-id="ddc7c-117">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ddc7c-118">Il [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) e [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) questi pacchetti consentono l'archiviazione delle chiavi di protezione dati in archiviazione di Azure o una cache Redis.</span><span class="sxs-lookup"><span data-stu-id="ddc7c-118">The [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) and [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) packages allow storing data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="ddc7c-119">Le chiavi possono essere condivisi tra più istanze di un'app web.</span><span class="sxs-lookup"><span data-stu-id="ddc7c-119">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="ddc7c-120">Le app possono condividere i cookie di autenticazione o CSRF protection tra più server.</span><span class="sxs-lookup"><span data-stu-id="ddc7c-120">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span>

::: moniker-end

<span data-ttu-id="ddc7c-121">Per configurare il provider di archiviazione Blob di Azure, chiamare uno dei [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) Overload:</span><span class="sxs-lookup"><span data-stu-id="ddc7c-121">To configure the Azure Blob Storage provider, call one of the [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ddc7c-122">Per configurare in Redis, chiamare uno dei [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) Overload:</span><span class="sxs-lookup"><span data-stu-id="ddc7c-122">To configure on Redis, call one of the [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToStackExchangeRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ddc7c-123">Per configurare in Redis, chiamare uno dei [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) Overload:</span><span class="sxs-lookup"><span data-stu-id="ddc7c-123">To configure on Redis, call one of the [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

<span data-ttu-id="ddc7c-124">Per altre informazioni, vedere i seguenti argomenti:</span><span class="sxs-lookup"><span data-stu-id="ddc7c-124">For more information, see the following topics:</span></span>

* [<span data-ttu-id="ddc7c-125">ConnectionMultiplexer stackexchange. Redis</span><span class="sxs-lookup"><span data-stu-id="ddc7c-125">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [<span data-ttu-id="ddc7c-126">Cache Redis di Azure</span><span class="sxs-lookup"><span data-stu-id="ddc7c-126">Azure Redis Cache</span></span>](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [<span data-ttu-id="ddc7c-127">esempi di ASPNET/DataProtection</span><span class="sxs-lookup"><span data-stu-id="ddc7c-127">aspnet/DataProtection samples</span></span>](https://github.com/aspnet/AspNetCore/tree/2.2.0/src/DataProtection/samples)

## <a name="registry"></a><span data-ttu-id="ddc7c-128">Registro di sistema</span><span class="sxs-lookup"><span data-stu-id="ddc7c-128">Registry</span></span>

<span data-ttu-id="ddc7c-129">**Si applica solo alle distribuzioni di Windows.**</span><span class="sxs-lookup"><span data-stu-id="ddc7c-129">**Only applies to Windows deployments.**</span></span>

<span data-ttu-id="ddc7c-130">In alcuni casi l'app potrebbe non avere accesso in scrittura nel file System.</span><span class="sxs-lookup"><span data-stu-id="ddc7c-130">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="ddc7c-131">Si consideri uno scenario in cui un'app è in esecuzione come account del servizio virtuale (ad esempio *w3wp.exe*dell'identità del pool di app).</span><span class="sxs-lookup"><span data-stu-id="ddc7c-131">Consider a scenario where an app is running as a virtual service account (such as *w3wp.exe*'s app pool identity).</span></span> <span data-ttu-id="ddc7c-132">In questi casi, l'amministratore può effettuare il provisioning di una chiave del Registro di sistema che è accessibile dall'identità account del servizio.</span><span class="sxs-lookup"><span data-stu-id="ddc7c-132">In these cases, the administrator can provision a registry key that's accessible by the service account identity.</span></span> <span data-ttu-id="ddc7c-133">Chiamare il [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) metodo di estensione come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="ddc7c-133">Call the [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) extension method as shown below.</span></span> <span data-ttu-id="ddc7c-134">Fornire una [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) che punta alla posizione in cui le chiavi di crittografia devono essere archiviate:</span><span class="sxs-lookup"><span data-stu-id="ddc7c-134">Provide a [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) pointing to the location where cryptographic keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> <span data-ttu-id="ddc7c-135">È consigliabile usare [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) per crittografare le chiavi a riposo.</span><span class="sxs-lookup"><span data-stu-id="ddc7c-135">We recommend using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt the keys at rest.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="entity-framework-core"></a><span data-ttu-id="ddc7c-136">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="ddc7c-136">Entity Framework Core</span></span>

<span data-ttu-id="ddc7c-137">Il [Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) pacchetto fornisce un meccanismo per l'archiviazione delle chiavi di protezione dati a un database tramite Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ddc7c-137">The [Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) package provides a mechanism for storing data protection keys to a database using Entity Framework Core.</span></span> <span data-ttu-id="ddc7c-138">Il `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` pacchetto NuGet deve essere aggiunto al file di progetto, non è in parte il [Microsoft.AspNetCore.App metapacchetto](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="ddc7c-138">The `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` NuGet package must be added to the project file, it's not part of the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="ddc7c-139">Con questo pacchetto, le chiavi possono essere condivisi tra più istanze di un'app web.</span><span class="sxs-lookup"><span data-stu-id="ddc7c-139">With this package, keys can be shared across multiple instances of a web app.</span></span>

<span data-ttu-id="ddc7c-140">Per configurare il provider di Entity Framework Core, chiamare il [ `PersistKeysToDbContext<TContext>` ](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) metodo:</span><span class="sxs-lookup"><span data-stu-id="ddc7c-140">To configure the EF Core provider, call the [`PersistKeysToDbContext<TContext>`](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) method:</span></span>

[!code-csharp[Main](key-storage-providers/sample/Startup.cs?name=snippet&highlight=13-15)]

<span data-ttu-id="ddc7c-141">Il parametro generico `TContext`, è necessario ereditare [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) e [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):</span><span class="sxs-lookup"><span data-stu-id="ddc7c-141">The generic parameter, `TContext`, must inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) and [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):</span></span>

[!code-csharp[Main](key-storage-providers/sample/MyKeysContext.cs)]

::: moniker-end

## <a name="custom-key-repository"></a><span data-ttu-id="ddc7c-142">Archivio chiave personalizzato</span><span class="sxs-lookup"><span data-stu-id="ddc7c-142">Custom key repository</span></span>

<span data-ttu-id="ddc7c-143">Se i meccanismi in arrivo non sono appropriati, lo sviluppatore può specificare il proprio meccanismo di persistenza chiave, fornendo un oggetto personalizzato [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span><span class="sxs-lookup"><span data-stu-id="ddc7c-143">If the in-box mechanisms aren't appropriate, the developer can specify their own key persistence mechanism by providing a custom [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span></span>
