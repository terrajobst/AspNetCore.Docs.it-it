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
# <a name="key-storage-providers-in-aspnet-core"></a>Provider di archiviazione chiavi in ASP.NET Core

Il sistema di protezione dati [Usa un meccanismo di individuazione per impostazione predefinita](xref:security/data-protection/configuration/default-settings) per determinare dove le chiavi di crittografia devono essere persistente. Lo sviluppatore può ignorare il meccanismo di individuazione predefinito e specificare manualmente il percorso.

> [!WARNING]
> Se si specifica un percorso di persistenza chiave esplicite, il sistema di protezione dati Annulla la registrazione di crittografia della chiave predefinita al meccanismo di rest, in modo che le chiavi non vengono più crittografate a riposo. È consigliabile che è inoltre [specifica un meccanismo di crittografia della chiave esplicite](xref:security/data-protection/implementation/key-encryption-at-rest) per distribuzioni di produzione.

## <a name="file-system"></a>File system

Per configurare un repository chiave basata sul sistema di file, chiamare il [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) routine di configurazione come illustrato di seguito. Fornire una [DirectoryInfo](/dotnet/api/system.io.directoryinfo) che punta al repository in cui le chiavi devono essere archiviate:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

Il `DirectoryInfo` può puntare a una directory nel computer locale oppure può puntare a una cartella in una condivisione di rete. Se punta a una directory nel computer locale e lo scenario è che solo le app nel computer locale richiedono l'accesso per usare questo repository, è consigliabile usare [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (on Windows) per crittografare le chiavi a riposo. In caso contrario, è consigliabile usare un [certificato X.509](xref:security/data-protection/implementation/key-encryption-at-rest) per crittografare le chiavi a riposo.

## <a name="azure-and-redis"></a>Azure e Redis

Il [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) e [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) questi pacchetti consentono l'archiviazione delle chiavi di protezione dati in archiviazione di Azure o una cache Redis. Le chiavi possono essere condivisi tra più istanze di un'app web. Le app possono condividere i cookie di autenticazione o CSRF protection tra più server. Per configurare il provider di archiviazione Blob di Azure, chiamare uno dei [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) Overload:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

Per configurare in Redis, chiamare uno dei [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) Overload:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

Per altre informazioni, vedere i seguenti argomenti:

* [ConnectionMultiplexer stackexchange. Redis](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [Cache Redis di Azure](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [esempi di ASPNET/DataProtection](https://github.com/aspnet/DataProtection/tree/master/samples)

## <a name="registry"></a>Registro di sistema

**Si applica solo alle distribuzioni di Windows.**

In alcuni casi l'app potrebbe non avere accesso in scrittura nel file System. Si consideri uno scenario in cui un'app è in esecuzione come account del servizio virtuale (ad esempio *w3wp.exe*dell'identità del pool di app). In questi casi, l'amministratore può effettuare il provisioning di una chiave del Registro di sistema che è accessibile dall'identità account del servizio. Chiamare il [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) metodo di estensione come illustrato di seguito. Fornire una [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) che punta alla posizione in cui le chiavi di crittografia devono essere archiviate:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> È consigliabile usare [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) per crittografare le chiavi a riposo.

::: moniker range=">= aspnetcore-2.2"

## <a name="entity-framework-core"></a>Entity Framework Core

Il [Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) pacchetto fornisce un meccanismo per l'archiviazione delle chiavi di protezione dati a un database tramite Entity Framework Core. Il `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` pacchetto NuGet deve essere aggiunto al file di progetto, non è in parte il [Microsoft.AspNetCore.App metapacchetto](xref:fundamentals/metapackage-app).

Con questo pacchetto, le chiavi possono essere condivisi tra più istanze di un'app web.

Per configurare il provider di Entity Framework Core, chiamare il [ `PersistKeysToDbContext<TContext>` ](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) metodo:

[!code-csharp[Main](key-storage-providers/sample/Startup.cs?name=snippet&highlight=13-15)]

Il parametro generico `TContext`, è necessario ereditare [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) e [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):

[!code-csharp[Main](key-storage-providers/sample/MyKeysContext.cs)]

::: moniker-end

## <a name="custom-key-repository"></a>Archivio chiave personalizzato

Se i meccanismi in arrivo non sono appropriati, lo sviluppatore può specificare il proprio meccanismo di persistenza chiave, fornendo un oggetto personalizzato [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).
