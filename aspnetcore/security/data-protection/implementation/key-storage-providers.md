---
title: Provider di archiviazione chiavi
author: rick-anderson
description: Provider di archiviazione chiavi
manager: wpickett
ms.author: riande
ms.date: 01/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 4f0e29a94593cd1cbb9890d7ee8bd09cddb4f69c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="key-storage-providers"></a>Provider di archiviazione chiavi

<a name="data-protection-implementation-key-storage-providers"></a>

Per impostazione predefinita il sistema di protezione dati [utilizza un approccio euristico](xref:security/data-protection/configuration/default-settings) per determinare dove deve essere mantenuti chiavi crittografiche. Lo sviluppatore può eseguire l'override l'euristica e specificare manualmente il percorso.

> [!NOTE]
> Se si specifica un percorso esplicito persistenza chiave, il sistema di protezione dati verrà annullare la registrazione alla crittografia della chiave predefinita al meccanismo di rest che ha fornito l'approccio euristico, pertanto non è più chiavi verranno crittografate a riposo. Si consiglia di che è inoltre [specifica un meccanismo di crittografia della chiave esplicita](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers) per applicazioni di produzione.

Il sistema di protezione dati viene fornito con diversi provider di archiviazione della chiave nella casella.

## <a name="file-system"></a>File system

Si prevede che molte App utilizzerà un repository chiave basata sul sistema di file. A questo scopo, chiamare il [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) routine di configurazione come illustrato di seguito. Fornire un `DirectoryInfo` che punta al repository in cui le chiavi devono essere archiviate.

```csharp
sc.AddDataProtection()
       // persist keys to a specific directory
       .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
   ```

Il `DirectoryInfo` può puntare a una directory sul computer locale oppure può puntare a una cartella in una condivisione di rete. Se si fa riferimento a una directory sul computer locale e lo scenario è che solo le applicazioni nel computer locale saranno necessario usare questo repository, è consigliabile utilizzare [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) per crittografare le chiavi inattivi. In caso contrario è consigliabile utilizzare un [certificato x. 509](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) per crittografare le chiavi inattivi.

## <a name="azure-and-redis"></a>Azure e Redis

Il `Microsoft.AspNetCore.DataProtection.AzureStorage` e `Microsoft.AspNetCore.DataProtection.Redis` pacchetti consentono l'archiviazione delle chiavi di protezione dati nell'archiviazione di Azure o di una cache Redis. Le chiavi possono essere condivise tra più istanze di un'app web. L'app ASP.NET Core è possibile condividere i cookie di autenticazione o protezione CSRF tra più server. Per configurare in Azure, chiamare uno del [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) overload come illustrato di seguito.

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));

    services.AddMvc();
}
```

Vedere anche il [codice di test Azure](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).

Per configurare in Redis, chiamare uno del [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) overload come illustrato di seguito.

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

Per altre informazioni, vedere i seguenti argomenti:

- [ConnectionMultiplexer stackexchange. Redis](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
- [Cache Redis di Azure](https://docs.microsoft.com/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
- [Codice di test di redis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).

## <a name="registry"></a>Registro di sistema

In alcuni casi l'app potrebbe non avere accesso in scrittura nel file System. Si consideri uno scenario in cui un'applicazione viene eseguita come account del servizio virtuale (ad esempio, l'identità del pool di app di w3wp.exe). In questi casi, l'amministratore potrebbe avere il provisioning una chiave del Registro di sistema che appropriato per l'identità dell'account del servizio. Chiamare il [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) routine di configurazione come illustrato di seguito. Fornire un `RegistryKey` che punta al percorso in cui devono essere archiviati e valori di chiavi di crittografia.

```csharp
   sc.AddDataProtection()
       // persist keys to a specific location in the system registry
       .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
   ```

Se si utilizza il Registro di sistema come un meccanismo di persistenza, è consigliabile utilizzare [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) per crittografare le chiavi inattivi.

## <a name="custom-key-repository"></a>Archivio chiave personalizzato

Se i meccanismi di casella in non sono appropriati, lo sviluppatore può specificare il proprio meccanismo di persistenza chiave, fornendo un oggetto personalizzato `IXmlRepository`.
