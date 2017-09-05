---
title: Provider di archiviazione chiavi
author: rick-anderson
description: Provider di archiviazione chiavi
keywords: crittografia, ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 1/14/2017
ms.topic: article
ms.assetid: 423e0a79-2f34-44c4-aaf3-146a53c39251
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 0a1798e60b52cbb16b6cb6d41c36200ed629d06d
ms.sourcegitcommit: 1db592cd8f6ea4061b31689f7109356de8f59440
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/29/2017
---
# <a name="key-storage-providers"></a><span data-ttu-id="b0ce8-104">Provider di archiviazione chiavi</span><span class="sxs-lookup"><span data-stu-id="b0ce8-104">Key storage providers</span></span>

<a name=data-protection-implementation-key-storage-providers></a>

<span data-ttu-id="b0ce8-105">Per impostazione predefinita il sistema di protezione dati [utilizza un approccio euristico](../configuration/default-settings.md#data-protection-default-settings) per determinare dove deve essere mantenuti chiavi crittografiche.</span><span class="sxs-lookup"><span data-stu-id="b0ce8-105">By default the data protection system [employs a heuristic](../configuration/default-settings.md#data-protection-default-settings) to determine where cryptographic key material should be persisted.</span></span> <span data-ttu-id="b0ce8-106">Lo sviluppatore può eseguire l'override l'euristica e specificare manualmente il percorso.</span><span class="sxs-lookup"><span data-stu-id="b0ce8-106">The developer can override the heuristic and manually specify the location.</span></span>

> [!NOTE]
> <span data-ttu-id="b0ce8-107">Se si specifica un percorso esplicito persistenza chiave, il sistema di protezione dati verrà annullare la registrazione alla crittografia della chiave predefinita al meccanismo di rest che ha fornito l'approccio euristico, pertanto non è più chiavi verranno crittografate a riposo.</span><span class="sxs-lookup"><span data-stu-id="b0ce8-107">If you specify an explicit key persistence location, the data protection system will deregister the default key encryption at rest mechanism that the heuristic provided, so keys will no longer be encrypted at rest.</span></span> <span data-ttu-id="b0ce8-108">È consigliabile che si inoltre [specifica un meccanismo di crittografia della chiave esplicita](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers) per applicazioni di produzione.</span><span class="sxs-lookup"><span data-stu-id="b0ce8-108">It is recommended that you additionally [specify an explicit key encryption mechanism](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers) for production applications.</span></span>

<span data-ttu-id="b0ce8-109">Il sistema di protezione dati viene fornito con diversi provider di archiviazione della chiave nella casella.</span><span class="sxs-lookup"><span data-stu-id="b0ce8-109">The data protection system ships with several in-box key storage providers.</span></span>

## <a name="file-system"></a><span data-ttu-id="b0ce8-110">File system</span><span class="sxs-lookup"><span data-stu-id="b0ce8-110">File system</span></span>

<span data-ttu-id="b0ce8-111">Si prevede che molte App utilizzerà un repository chiave basata sul sistema di file.</span><span class="sxs-lookup"><span data-stu-id="b0ce8-111">We anticipate that many apps will use a file system-based key repository.</span></span> <span data-ttu-id="b0ce8-112">A questo scopo, chiamare il [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) routine di configurazione come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="b0ce8-112">To configure this, call the [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="b0ce8-113">Fornire un `DirectoryInfo` che punta al repository in cui le chiavi devono essere archiviate.</span><span class="sxs-lookup"><span data-stu-id="b0ce8-113">Provide a `DirectoryInfo` pointing to the repository where keys should be stored.</span></span>

```csharp
sc.AddDataProtection()
       // persist keys to a specific directory
       .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
   ```

<span data-ttu-id="b0ce8-114">Il `DirectoryInfo` può puntare a una directory sul computer locale oppure può puntare a una cartella in una condivisione di rete.</span><span class="sxs-lookup"><span data-stu-id="b0ce8-114">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="b0ce8-115">Se si fa riferimento a una directory sul computer locale e lo scenario è che solo le applicazioni nel computer locale saranno necessario usare questo repository, è consigliabile utilizzare [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) per crittografare le chiavi inattivi.</span><span class="sxs-lookup"><span data-stu-id="b0ce8-115">If pointing to a directory on the local machine (and the scenario is that only applications on the local machine will need to use this repository), consider using [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span> <span data-ttu-id="b0ce8-116">In caso contrario è consigliabile utilizzare un [certificato x. 509](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) per crittografare le chiavi inattivi.</span><span class="sxs-lookup"><span data-stu-id="b0ce8-116">Otherwise consider using an [X.509 certificate](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-and-redis"></a><span data-ttu-id="b0ce8-117">Azure e Redis</span><span class="sxs-lookup"><span data-stu-id="b0ce8-117">Azure and Redis</span></span>

<span data-ttu-id="b0ce8-118">Il `Microsoft.AspNetCore.DataProtection.AzureStorage` e `Microsoft.AspNetCore.DataProtection.Redis` pacchetti consentono l'archiviazione delle chiavi di protezione dati nell'archiviazione di Azure o di una cache Redis.</span><span class="sxs-lookup"><span data-stu-id="b0ce8-118">The `Microsoft.AspNetCore.DataProtection.AzureStorage` and `Microsoft.AspNetCore.DataProtection.Redis` packages allow storing your data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="b0ce8-119">Le chiavi possono essere condivise tra più istanze di un'app web.</span><span class="sxs-lookup"><span data-stu-id="b0ce8-119">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="b0ce8-120">L'app ASP.NET Core è possibile condividere i cookie di autenticazione o protezione CSRF tra più server.</span><span class="sxs-lookup"><span data-stu-id="b0ce8-120">Your ASP.NET Core app can share authentication cookies or CSRF protection across multiple servers.</span></span> <span data-ttu-id="b0ce8-121">Per configurare in Azure, chiamare uno del [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) overload come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="b0ce8-121">To configure on Azure, call one of the [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));

    services.AddMvc();
}
```

<span data-ttu-id="b0ce8-122">Vedere anche il [codice di test Azure](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="b0ce8-122">See also the [Azure test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span></span>

<span data-ttu-id="b0ce8-123">Per configurare in Redis, chiamare uno del [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) overload come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="b0ce8-123">To configure on Redis, call one of the [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    // Connect to Redis database.
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");

    services.AddMvc();
}
```

<span data-ttu-id="b0ce8-124">Per altre informazioni, vedere i seguenti argomenti:</span><span class="sxs-lookup"><span data-stu-id="b0ce8-124">See the following for more information:</span></span>

- [<span data-ttu-id="b0ce8-125">ConnectionMultiplexer stackexchange. Redis</span><span class="sxs-lookup"><span data-stu-id="b0ce8-125">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
- [<span data-ttu-id="b0ce8-126">Cache Redis di Azure</span><span class="sxs-lookup"><span data-stu-id="b0ce8-126">Azure Redis Cache</span></span>](https://docs.microsoft.com/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
- <span data-ttu-id="b0ce8-127">[Codice di test di redis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="b0ce8-127">[Redis test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span></span>

## <a name="registry"></a><span data-ttu-id="b0ce8-128">Registro di sistema</span><span class="sxs-lookup"><span data-stu-id="b0ce8-128">Registry</span></span>

<span data-ttu-id="b0ce8-129">In alcuni casi l'app potrebbe non avere accesso in scrittura nel file System.</span><span class="sxs-lookup"><span data-stu-id="b0ce8-129">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="b0ce8-130">Si consideri uno scenario in cui un'applicazione viene eseguita come account del servizio virtuale (ad esempio, l'identità del pool di app di w3wp.exe).</span><span class="sxs-lookup"><span data-stu-id="b0ce8-130">Consider a scenario where an app is running as a virtual service account (such as w3wp.exe's app pool identity).</span></span> <span data-ttu-id="b0ce8-131">In questi casi, l'amministratore potrebbe avere il provisioning una chiave del Registro di sistema che appropriato per l'identità dell'account del servizio.</span><span class="sxs-lookup"><span data-stu-id="b0ce8-131">In these cases, the administrator may have provisioned a registry key that is appropriate ACLed for the service account identity.</span></span> <span data-ttu-id="b0ce8-132">Chiamare il [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) routine di configurazione come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="b0ce8-132">Call the [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="b0ce8-133">Fornire un `RegistryKey` che punta al percorso in cui devono essere archiviati e valori di chiavi di crittografia.</span><span class="sxs-lookup"><span data-stu-id="b0ce8-133">Provide a `RegistryKey` pointing to the location where cryptographic keys/values should be stored.</span></span>

```csharp
   sc.AddDataProtection()
       // persist keys to a specific location in the system registry
       .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
   ```

<span data-ttu-id="b0ce8-134">Se si utilizza il Registro di sistema come un meccanismo di persistenza, è consigliabile utilizzare [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) per crittografare le chiavi inattivi.</span><span class="sxs-lookup"><span data-stu-id="b0ce8-134">If you use the system registry as a persistence mechanism, consider using [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span>

## <a name="custom-key-repository"></a><span data-ttu-id="b0ce8-135">Archivio chiave personalizzato</span><span class="sxs-lookup"><span data-stu-id="b0ce8-135">Custom key repository</span></span>

<span data-ttu-id="b0ce8-136">Se i meccanismi di casella in non sono appropriati, lo sviluppatore può specificare il proprio meccanismo di persistenza chiave, fornendo un oggetto personalizzato `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="b0ce8-136">If the in-box mechanisms are not appropriate, the developer can specify their own key persistence mechanism by providing a custom `IXmlRepository`.</span></span>
