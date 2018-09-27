---
title: Provider di archiviazione chiavi in ASP.NET Core
author: rick-anderson
description: Informazioni sui provider di archiviazione chiavi in ASP.NET Core e su come configurare i percorsi di archiviazione delle chiavi.
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 0e64a65ab1d65efa9f2e4d36a23663b607f206d7
ms.sourcegitcommit: 9bdba90b2c97a4016188434657194b2d7027d6e3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47402068"
---
# <a name="key-storage-providers-in-aspnet-core"></a><span data-ttu-id="0dba7-103">Provider di archiviazione chiavi in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0dba7-103">Key storage providers in ASP.NET Core</span></span>

<span data-ttu-id="0dba7-104">Il sistema di protezione dati [Usa un meccanismo di individuazione per impostazione predefinita](xref:security/data-protection/configuration/default-settings) per determinare dove le chiavi di crittografia devono essere persistente.</span><span class="sxs-lookup"><span data-stu-id="0dba7-104">The data protection system [employs a discovery mechanism by default](xref:security/data-protection/configuration/default-settings) to determine where cryptographic keys should be persisted.</span></span> <span data-ttu-id="0dba7-105">Lo sviluppatore può ignorare il meccanismo di individuazione predefinito e specificare manualmente il percorso.</span><span class="sxs-lookup"><span data-stu-id="0dba7-105">The developer can override the default discovery mechanism and manually specify the location.</span></span>

> [!WARNING]
> <span data-ttu-id="0dba7-106">Se si specifica un percorso di persistenza chiave esplicite, il sistema di protezione dati Annulla la registrazione di crittografia della chiave predefinita al meccanismo di rest, in modo che le chiavi non vengono più crittografate a riposo.</span><span class="sxs-lookup"><span data-stu-id="0dba7-106">If you specify an explicit key persistence location, the data protection system deregisters the default key encryption at rest mechanism, so keys are no longer encrypted at rest.</span></span> <span data-ttu-id="0dba7-107">È consigliabile che è inoltre [specifica un meccanismo di crittografia della chiave esplicite](xref:security/data-protection/implementation/key-encryption-at-rest) per distribuzioni di produzione.</span><span class="sxs-lookup"><span data-stu-id="0dba7-107">It's recommended that you additionally [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest) for production deployments.</span></span>

## <a name="file-system"></a><span data-ttu-id="0dba7-108">File system</span><span class="sxs-lookup"><span data-stu-id="0dba7-108">File system</span></span>

<span data-ttu-id="0dba7-109">Per configurare un repository chiave basata sul sistema di file, chiamare il [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) routine di configurazione come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="0dba7-109">To configure a file system-based key repository, call the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) configuration routine as shown below.</span></span> <span data-ttu-id="0dba7-110">Fornire una [DirectoryInfo](/dotnet/api/system.io.directoryinfo) che punta al repository in cui le chiavi devono essere archiviate:</span><span class="sxs-lookup"><span data-stu-id="0dba7-110">Provide a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) pointing to the repository where keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

<span data-ttu-id="0dba7-111">Il `DirectoryInfo` può puntare a una directory nel computer locale oppure può puntare a una cartella in una condivisione di rete.</span><span class="sxs-lookup"><span data-stu-id="0dba7-111">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="0dba7-112">Se punta a una directory nel computer locale e lo scenario è che solo le app nel computer locale richiedono l'accesso per usare questo repository, è consigliabile usare [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (on Windows) per crittografare le chiavi a riposo.</span><span class="sxs-lookup"><span data-stu-id="0dba7-112">If pointing to a directory on the local machine (and the scenario is that only apps on the local machine require access to use this repository), consider using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (on Windows) to encrypt the keys at rest.</span></span> <span data-ttu-id="0dba7-113">In caso contrario, è consigliabile usare un [certificato X.509](xref:security/data-protection/implementation/key-encryption-at-rest) per crittografare le chiavi a riposo.</span><span class="sxs-lookup"><span data-stu-id="0dba7-113">Otherwise, consider using an [X.509 certificate](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-and-redis"></a><span data-ttu-id="0dba7-114">Azure e Redis</span><span class="sxs-lookup"><span data-stu-id="0dba7-114">Azure and Redis</span></span>

<span data-ttu-id="0dba7-115">Il [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) e [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) questi pacchetti consentono l'archiviazione delle chiavi di protezione dati in archiviazione di Azure o una cache Redis.</span><span class="sxs-lookup"><span data-stu-id="0dba7-115">The [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) and [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) packages allow storing data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="0dba7-116">Le chiavi possono essere condivisi tra più istanze di un'app web.</span><span class="sxs-lookup"><span data-stu-id="0dba7-116">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="0dba7-117">Le app possono condividere i cookie di autenticazione o CSRF protection tra più server.</span><span class="sxs-lookup"><span data-stu-id="0dba7-117">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span> <span data-ttu-id="0dba7-118">Per configurare il provider di archiviazione Blob di Azure, chiamare uno dei [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) Overload:</span><span class="sxs-lookup"><span data-stu-id="0dba7-118">To configure the Azure Blob Storage provider, call one of the [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

<span data-ttu-id="0dba7-119">Per configurare in Redis, chiamare uno dei [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) Overload:</span><span class="sxs-lookup"><span data-stu-id="0dba7-119">To configure on Redis, call one of the [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

<span data-ttu-id="0dba7-120">Per altre informazioni, vedere i seguenti argomenti:</span><span class="sxs-lookup"><span data-stu-id="0dba7-120">For more information, see the following topics:</span></span>

* [<span data-ttu-id="0dba7-121">ConnectionMultiplexer stackexchange. Redis</span><span class="sxs-lookup"><span data-stu-id="0dba7-121">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [<span data-ttu-id="0dba7-122">Cache Redis di Azure</span><span class="sxs-lookup"><span data-stu-id="0dba7-122">Azure Redis Cache</span></span>](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [<span data-ttu-id="0dba7-123">esempi di ASPNET/DataProtection</span><span class="sxs-lookup"><span data-stu-id="0dba7-123">aspnet/DataProtection samples</span></span>](https://github.com/aspnet/DataProtection/tree/master/samples)

## <a name="registry"></a><span data-ttu-id="0dba7-124">Registro di sistema</span><span class="sxs-lookup"><span data-stu-id="0dba7-124">Registry</span></span>

<span data-ttu-id="0dba7-125">**Si applica solo alle distribuzioni di Windows.**</span><span class="sxs-lookup"><span data-stu-id="0dba7-125">**Only applies to Windows deployments.**</span></span>

<span data-ttu-id="0dba7-126">In alcuni casi l'app potrebbe non avere accesso in scrittura nel file System.</span><span class="sxs-lookup"><span data-stu-id="0dba7-126">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="0dba7-127">Si consideri uno scenario in cui un'app è in esecuzione come account del servizio virtuale (ad esempio *w3wp.exe*dell'identità del pool di app).</span><span class="sxs-lookup"><span data-stu-id="0dba7-127">Consider a scenario where an app is running as a virtual service account (such as *w3wp.exe*'s app pool identity).</span></span> <span data-ttu-id="0dba7-128">In questi casi, l'amministratore può effettuare il provisioning di una chiave del Registro di sistema che è accessibile dall'identità account del servizio.</span><span class="sxs-lookup"><span data-stu-id="0dba7-128">In these cases, the administrator can provision a registry key that's accessible by the service account identity.</span></span> <span data-ttu-id="0dba7-129">Chiamare il [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) metodo di estensione come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="0dba7-129">Call the [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) extension method as shown below.</span></span> <span data-ttu-id="0dba7-130">Fornire una [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) che punta alla posizione in cui le chiavi di crittografia devono essere archiviate:</span><span class="sxs-lookup"><span data-stu-id="0dba7-130">Provide a [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) pointing to the location where cryptographic keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> <span data-ttu-id="0dba7-131">È consigliabile usare [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) per crittografare le chiavi a riposo.</span><span class="sxs-lookup"><span data-stu-id="0dba7-131">We recommend using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt the keys at rest.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="entity-framework-core"></a><span data-ttu-id="0dba7-132">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="0dba7-132">Entity Framework Core</span></span>

<span data-ttu-id="0dba7-133">Il [Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) pacchetto fornisce un meccanismo per l'archiviazione delle chiavi di protezione dati a un database tramite Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0dba7-133">The [Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) package provides a mechanism for storing data protection keys to a database using Entity Framework Core.</span></span> <span data-ttu-id="0dba7-134">Il `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` pacchetto NuGet deve essere aggiunto al file di progetto, non è in parte il [Microsoft.AspNetCore.App metapacchetto](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="0dba7-134">The `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` NuGet package must be added to the project file, it's not part of the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="0dba7-135">Con questo pacchetto, le chiavi possono essere condivisi tra più istanze di un'app web.</span><span class="sxs-lookup"><span data-stu-id="0dba7-135">With this package, keys can be shared across multiple instances of a web app.</span></span>

<span data-ttu-id="0dba7-136">Per configurare il provider di Entity Framework Core, chiamare il [ `PersistKeysToDbContext<TContext>` ](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) metodo:</span><span class="sxs-lookup"><span data-stu-id="0dba7-136">To configure the EF Core provider, call the [`PersistKeysToDbContext<TContext>`](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) method:</span></span>

[!code-csharp[Main](key-storage-providers/sample/Startup.cs?name=snippet&highlight=13-15)]

<span data-ttu-id="0dba7-137">Il parametro generico `TContext`, è necessario ereditare [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) e [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):</span><span class="sxs-lookup"><span data-stu-id="0dba7-137">The generic parameter, `TContext`, must inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) and [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):</span></span>

[!code-csharp[Main](key-storage-providers/sample/MyKeysContext.cs)]

::: moniker-end

## <a name="custom-key-repository"></a><span data-ttu-id="0dba7-138">Archivio chiave personalizzato</span><span class="sxs-lookup"><span data-stu-id="0dba7-138">Custom key repository</span></span>

<span data-ttu-id="0dba7-139">Se i meccanismi in arrivo non sono appropriati, lo sviluppatore può specificare il proprio meccanismo di persistenza chiave, fornendo un oggetto personalizzato [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span><span class="sxs-lookup"><span data-stu-id="0dba7-139">If the in-box mechanisms aren't appropriate, the developer can specify their own key persistence mechanism by providing a custom [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span></span>
