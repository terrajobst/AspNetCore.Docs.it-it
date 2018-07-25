---
title: Provider di archiviazione chiavi in ASP.NET Core
author: rick-anderson
description: Informazioni sui provider di archiviazione chiavi in ASP.NET Core e su come configurare i percorsi di archiviazione delle chiavi.
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 74d62e88b40cfcefb81d699a5aba2665c56ac51a
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219264"
---
# <a name="key-storage-providers-in-aspnet-core"></a><span data-ttu-id="e8a1d-103">Provider di archiviazione chiavi in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e8a1d-103">Key storage providers in ASP.NET Core</span></span>

<span data-ttu-id="e8a1d-104">Il sistema di protezione dati [Usa un meccanismo di individuazione per impostazione predefinita](xref:security/data-protection/configuration/default-settings) per determinare dove le chiavi di crittografia devono essere persistente.</span><span class="sxs-lookup"><span data-stu-id="e8a1d-104">The data protection system [employs a discovery mechanism by default](xref:security/data-protection/configuration/default-settings) to determine where cryptographic keys should be persisted.</span></span> <span data-ttu-id="e8a1d-105">Lo sviluppatore può ignorare il meccanismo di individuazione predefinito e specificare manualmente il percorso.</span><span class="sxs-lookup"><span data-stu-id="e8a1d-105">The developer can override the default discovery mechanism and manually specify the location.</span></span>

> [!WARNING]
> <span data-ttu-id="e8a1d-106">Se si specifica un percorso di persistenza chiave esplicite, il sistema di protezione dati Annulla la registrazione di crittografia della chiave predefinita al meccanismo di rest, in modo che le chiavi non vengono più crittografate a riposo.</span><span class="sxs-lookup"><span data-stu-id="e8a1d-106">If you specify an explicit key persistence location, the data protection system deregisters the default key encryption at rest mechanism, so keys are no longer encrypted at rest.</span></span> <span data-ttu-id="e8a1d-107">È consigliabile che è inoltre [specifica un meccanismo di crittografia della chiave esplicite](xref:security/data-protection/implementation/key-encryption-at-rest) per distribuzioni di produzione.</span><span class="sxs-lookup"><span data-stu-id="e8a1d-107">It's recommended that you additionally [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest) for production deployments.</span></span>

## <a name="file-system"></a><span data-ttu-id="e8a1d-108">File system</span><span class="sxs-lookup"><span data-stu-id="e8a1d-108">File system</span></span>

<span data-ttu-id="e8a1d-109">Per configurare un repository chiave basata sul sistema di file, chiamare il [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) routine di configurazione come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="e8a1d-109">To configure a file system-based key repository, call the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) configuration routine as shown below.</span></span> <span data-ttu-id="e8a1d-110">Fornire una [DirectoryInfo](/dotnet/api/system.io.directoryinfo) che punta al repository in cui le chiavi devono essere archiviate:</span><span class="sxs-lookup"><span data-stu-id="e8a1d-110">Provide a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) pointing to the repository where keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

<span data-ttu-id="e8a1d-111">Il `DirectoryInfo` può puntare a una directory nel computer locale oppure può puntare a una cartella in una condivisione di rete.</span><span class="sxs-lookup"><span data-stu-id="e8a1d-111">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="e8a1d-112">Se punta a una directory nel computer locale e lo scenario è che solo le app nel computer locale richiedono l'accesso per usare questo repository, è consigliabile usare [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (on Windows) per crittografare le chiavi a riposo.</span><span class="sxs-lookup"><span data-stu-id="e8a1d-112">If pointing to a directory on the local machine (and the scenario is that only apps on the local machine require access to use this repository), consider using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (on Windows) to encrypt the keys at rest.</span></span> <span data-ttu-id="e8a1d-113">In caso contrario, è consigliabile usare un [certificato X.509](xref:security/data-protection/implementation/key-encryption-at-rest) per crittografare le chiavi a riposo.</span><span class="sxs-lookup"><span data-stu-id="e8a1d-113">Otherwise, consider using an [X.509 certificate](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-and-redis"></a><span data-ttu-id="e8a1d-114">Azure e Redis</span><span class="sxs-lookup"><span data-stu-id="e8a1d-114">Azure and Redis</span></span>

<span data-ttu-id="e8a1d-115">Il [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) e [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) questi pacchetti consentono l'archiviazione delle chiavi di protezione dati in archiviazione di Azure o una cache Redis.</span><span class="sxs-lookup"><span data-stu-id="e8a1d-115">The [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) and [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) packages allow storing data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="e8a1d-116">Le chiavi possono essere condivisi tra più istanze di un'app web.</span><span class="sxs-lookup"><span data-stu-id="e8a1d-116">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="e8a1d-117">Le app possono condividere i cookie di autenticazione o CSRF protection tra più server.</span><span class="sxs-lookup"><span data-stu-id="e8a1d-117">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span> <span data-ttu-id="e8a1d-118">Per configurare il provider di archiviazione Blob di Azure, chiamare uno dei [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) Overload:</span><span class="sxs-lookup"><span data-stu-id="e8a1d-118">To configure the Azure Blob Storage provider, call one of the [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

<span data-ttu-id="e8a1d-119">Per configurare in Redis, chiamare uno dei [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) Overload:</span><span class="sxs-lookup"><span data-stu-id="e8a1d-119">To configure on Redis, call one of the [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

<span data-ttu-id="e8a1d-120">Per altre informazioni, vedere i seguenti argomenti:</span><span class="sxs-lookup"><span data-stu-id="e8a1d-120">For more information, see the following topics:</span></span>

* [<span data-ttu-id="e8a1d-121">ConnectionMultiplexer stackexchange. Redis</span><span class="sxs-lookup"><span data-stu-id="e8a1d-121">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [<span data-ttu-id="e8a1d-122">Cache Redis di Azure</span><span class="sxs-lookup"><span data-stu-id="e8a1d-122">Azure Redis Cache</span></span>](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [<span data-ttu-id="e8a1d-123">esempi di ASPNET/DataProtection</span><span class="sxs-lookup"><span data-stu-id="e8a1d-123">aspnet/DataProtection samples</span></span>](https://github.com/aspnet/DataProtection/samples)

## <a name="registry"></a><span data-ttu-id="e8a1d-124">Registro di sistema</span><span class="sxs-lookup"><span data-stu-id="e8a1d-124">Registry</span></span>

<span data-ttu-id="e8a1d-125">**Si applica solo alle distribuzioni di Windows.**</span><span class="sxs-lookup"><span data-stu-id="e8a1d-125">**Only applies to Windows deployments.**</span></span>

<span data-ttu-id="e8a1d-126">In alcuni casi l'app potrebbe non avere accesso in scrittura nel file System.</span><span class="sxs-lookup"><span data-stu-id="e8a1d-126">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="e8a1d-127">Si consideri uno scenario in cui un'app è in esecuzione come account del servizio virtuale (ad esempio *w3wp.exe*dell'identità del pool di app).</span><span class="sxs-lookup"><span data-stu-id="e8a1d-127">Consider a scenario where an app is running as a virtual service account (such as *w3wp.exe*'s app pool identity).</span></span> <span data-ttu-id="e8a1d-128">In questi casi, l'amministratore può effettuare il provisioning di una chiave del Registro di sistema che è accessibile dall'identità account del servizio.</span><span class="sxs-lookup"><span data-stu-id="e8a1d-128">In these cases, the administrator can provision a registry key that's accessible by the service account identity.</span></span> <span data-ttu-id="e8a1d-129">Chiamare il [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) metodo di estensione come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="e8a1d-129">Call the [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) extension method as shown below.</span></span> <span data-ttu-id="e8a1d-130">Fornire una [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) che punta alla posizione in cui le chiavi di crittografia devono essere archiviate:</span><span class="sxs-lookup"><span data-stu-id="e8a1d-130">Provide a [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) pointing to the location where cryptographic keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> <span data-ttu-id="e8a1d-131">È consigliabile usare [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) per crittografare le chiavi a riposo.</span><span class="sxs-lookup"><span data-stu-id="e8a1d-131">We recommend using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt the keys at rest.</span></span>

## <a name="custom-key-repository"></a><span data-ttu-id="e8a1d-132">Archivio chiave personalizzato</span><span class="sxs-lookup"><span data-stu-id="e8a1d-132">Custom key repository</span></span>

<span data-ttu-id="e8a1d-133">Se i meccanismi in arrivo non sono appropriati, lo sviluppatore può specificare il proprio meccanismo di persistenza chiave, fornendo un oggetto personalizzato [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span><span class="sxs-lookup"><span data-stu-id="e8a1d-133">If the in-box mechanisms aren't appropriate, the developer can specify their own key persistence mechanism by providing a custom [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span></span>
