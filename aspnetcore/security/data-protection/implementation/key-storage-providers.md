---
title: Provider di archiviazione chiavi in ASP.NET Core
author: rick-anderson
description: Informazioni sui provider di archiviazione chiavi in ASP.NET Core e su come configurare i percorsi di archiviazione chiavi.
ms.author: riande
ms.date: 01/14/2017
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 432c2690f216325470bbea9b974ea772bcdc39ed
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273769"
---
# <a name="key-storage-providers-in-aspnet-core"></a><span data-ttu-id="b9ede-103">Provider di archiviazione chiavi in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b9ede-103">Key storage providers in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-providers"></a>

<span data-ttu-id="b9ede-104">Per impostazione predefinita il sistema di protezione dati [utilizza un approccio euristico](xref:security/data-protection/configuration/default-settings) per determinare dove deve essere mantenuti chiavi crittografiche.</span><span class="sxs-lookup"><span data-stu-id="b9ede-104">By default the data protection system [employs a heuristic](xref:security/data-protection/configuration/default-settings) to determine where cryptographic key material should be persisted.</span></span> <span data-ttu-id="b9ede-105">Lo sviluppatore può eseguire l'override l'euristica e specificare manualmente il percorso.</span><span class="sxs-lookup"><span data-stu-id="b9ede-105">The developer can override the heuristic and manually specify the location.</span></span>

> [!NOTE]
> <span data-ttu-id="b9ede-106">Se si specifica un percorso esplicito persistenza chiave, il sistema di protezione dati verrà annullare la registrazione alla crittografia della chiave predefinita al meccanismo di rest che ha fornito l'approccio euristico, pertanto non è più chiavi verranno crittografate a riposo.</span><span class="sxs-lookup"><span data-stu-id="b9ede-106">If you specify an explicit key persistence location, the data protection system will deregister the default key encryption at rest mechanism that the heuristic provided, so keys will no longer be encrypted at rest.</span></span> <span data-ttu-id="b9ede-107">Si consiglia di che è inoltre [specifica un meccanismo di crittografia della chiave esplicita](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest-providers) per applicazioni di produzione.</span><span class="sxs-lookup"><span data-stu-id="b9ede-107">It's recommended that you additionally [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest-providers) for production applications.</span></span>

<span data-ttu-id="b9ede-108">Il sistema di protezione dati viene fornito con diversi provider di archiviazione della chiave nella casella.</span><span class="sxs-lookup"><span data-stu-id="b9ede-108">The data protection system ships with several in-box key storage providers.</span></span>

## <a name="file-system"></a><span data-ttu-id="b9ede-109">File system</span><span class="sxs-lookup"><span data-stu-id="b9ede-109">File system</span></span>

<span data-ttu-id="b9ede-110">Si prevede che molte App utilizzerà un repository chiave basata sul sistema di file.</span><span class="sxs-lookup"><span data-stu-id="b9ede-110">We anticipate that many apps will use a file system-based key repository.</span></span> <span data-ttu-id="b9ede-111">A questo scopo, chiamare il [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) routine di configurazione come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="b9ede-111">To configure this, call the [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="b9ede-112">Fornire un `DirectoryInfo` che punta al repository in cui le chiavi devono essere archiviate.</span><span class="sxs-lookup"><span data-stu-id="b9ede-112">Provide a `DirectoryInfo` pointing to the repository where keys should be stored.</span></span>

```csharp
sc.AddDataProtection()
       // persist keys to a specific directory
       .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
   ```

<span data-ttu-id="b9ede-113">Il `DirectoryInfo` può puntare a una directory sul computer locale oppure può puntare a una cartella in una condivisione di rete.</span><span class="sxs-lookup"><span data-stu-id="b9ede-113">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="b9ede-114">Se si fa riferimento a una directory sul computer locale e lo scenario è che solo le applicazioni nel computer locale saranno necessario usare questo repository, è consigliabile utilizzare [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) per crittografare le chiavi inattivi.</span><span class="sxs-lookup"><span data-stu-id="b9ede-114">If pointing to a directory on the local machine (and the scenario is that only applications on the local machine will need to use this repository), consider using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span> <span data-ttu-id="b9ede-115">In caso contrario è consigliabile utilizzare un [certificato x. 509](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) per crittografare le chiavi inattivi.</span><span class="sxs-lookup"><span data-stu-id="b9ede-115">Otherwise consider using an [X.509 certificate](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-and-redis"></a><span data-ttu-id="b9ede-116">Azure e Redis</span><span class="sxs-lookup"><span data-stu-id="b9ede-116">Azure and Redis</span></span>

<span data-ttu-id="b9ede-117">Il `Microsoft.AspNetCore.DataProtection.AzureStorage` e `Microsoft.AspNetCore.DataProtection.Redis` pacchetti consentono l'archiviazione delle chiavi di protezione dati nell'archiviazione di Azure o di una cache Redis.</span><span class="sxs-lookup"><span data-stu-id="b9ede-117">The `Microsoft.AspNetCore.DataProtection.AzureStorage` and `Microsoft.AspNetCore.DataProtection.Redis` packages allow storing your data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="b9ede-118">Le chiavi possono essere condivise tra più istanze di un'app web.</span><span class="sxs-lookup"><span data-stu-id="b9ede-118">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="b9ede-119">L'app ASP.NET Core è possibile condividere i cookie di autenticazione o protezione CSRF tra più server.</span><span class="sxs-lookup"><span data-stu-id="b9ede-119">Your ASP.NET Core app can share authentication cookies or CSRF protection across multiple servers.</span></span> <span data-ttu-id="b9ede-120">Per configurare in Azure, chiamare uno del [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) overload come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="b9ede-120">To configure on Azure, call one of the [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));

    services.AddMvc();
}
```

<span data-ttu-id="b9ede-121">Vedere anche il [codice di test Azure](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="b9ede-121">See also the [Azure test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span></span>

<span data-ttu-id="b9ede-122">Per configurare in Redis, chiamare uno del [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) overload come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="b9ede-122">To configure on Redis, call one of the [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

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

<span data-ttu-id="b9ede-123">Per altre informazioni, vedere i seguenti argomenti:</span><span class="sxs-lookup"><span data-stu-id="b9ede-123">See the following for more information:</span></span>

- [<span data-ttu-id="b9ede-124">ConnectionMultiplexer stackexchange. Redis</span><span class="sxs-lookup"><span data-stu-id="b9ede-124">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
- [<span data-ttu-id="b9ede-125">Cache Redis di Azure</span><span class="sxs-lookup"><span data-stu-id="b9ede-125">Azure Redis Cache</span></span>](https://docs.microsoft.com/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
- <span data-ttu-id="b9ede-126">[Codice di test di redis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="b9ede-126">[Redis test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span></span>

## <a name="registry"></a><span data-ttu-id="b9ede-127">Registro di sistema</span><span class="sxs-lookup"><span data-stu-id="b9ede-127">Registry</span></span>

<span data-ttu-id="b9ede-128">In alcuni casi l'app potrebbe non avere accesso in scrittura nel file System.</span><span class="sxs-lookup"><span data-stu-id="b9ede-128">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="b9ede-129">Si consideri uno scenario in cui un'applicazione viene eseguita come account del servizio virtuale (ad esempio, l'identità del pool di app di w3wp.exe).</span><span class="sxs-lookup"><span data-stu-id="b9ede-129">Consider a scenario where an app is running as a virtual service account (such as w3wp.exe's app pool identity).</span></span> <span data-ttu-id="b9ede-130">In questi casi, l'amministratore potrebbe avere il provisioning una chiave del Registro di sistema che appropriato per l'identità dell'account del servizio.</span><span class="sxs-lookup"><span data-stu-id="b9ede-130">In these cases, the administrator may have provisioned a registry key that's appropriate ACLed for the service account identity.</span></span> <span data-ttu-id="b9ede-131">Chiamare il [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) routine di configurazione come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="b9ede-131">Call the [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="b9ede-132">Fornire un `RegistryKey` che punta al percorso in cui devono essere archiviati e valori di chiavi di crittografia.</span><span class="sxs-lookup"><span data-stu-id="b9ede-132">Provide a `RegistryKey` pointing to the location where cryptographic keys/values should be stored.</span></span>

```csharp
   sc.AddDataProtection()
       // persist keys to a specific location in the system registry
       .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
   ```

<span data-ttu-id="b9ede-133">Se si utilizza il Registro di sistema come un meccanismo di persistenza, è consigliabile utilizzare [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) per crittografare le chiavi inattivi.</span><span class="sxs-lookup"><span data-stu-id="b9ede-133">If you use the system registry as a persistence mechanism, consider using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span>

## <a name="custom-key-repository"></a><span data-ttu-id="b9ede-134">Archivio chiave personalizzato</span><span class="sxs-lookup"><span data-stu-id="b9ede-134">Custom key repository</span></span>

<span data-ttu-id="b9ede-135">Se i meccanismi di casella in non sono appropriati, lo sviluppatore può specificare il proprio meccanismo di persistenza chiave, fornendo un oggetto personalizzato `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="b9ede-135">If the in-box mechanisms are not appropriate, the developer can specify their own key persistence mechanism by providing a custom `IXmlRepository`.</span></span>
