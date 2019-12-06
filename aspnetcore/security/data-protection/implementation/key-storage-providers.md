---
title: Provider di archiviazione chiavi in ASP.NET Core
author: rick-anderson
description: Informazioni sui provider di archiviazione chiavi in ASP.NET Core e su come configurare i percorsi di archiviazione delle chiavi.
ms.author: riande
ms.date: 12/05/2019
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 76f224fb7d322722c3875b734127da4da294a646
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881212"
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

## <a name="azure-storage"></a>Archiviazione di Azure

Il pacchetto [Microsoft. AspNetCore. dataprotection. AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) consente di archiviare le chiavi di protezione dei dati nell'archivio BLOB di Azure. Le chiavi possono essere condivisi tra più istanze di un'app web. Le app possono condividere i cookie di autenticazione o CSRF protection tra più server.

Per configurare il provider di archiviazione BLOB di Azure, chiamare uno degli overload [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) .

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

Se l'app Web è in esecuzione come servizio di Azure, i token di autenticazione possono essere creati automaticamente usando [Microsoft. Azure. Services. AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/).

```csharp
var tokenProvider = new AzureServiceTokenProvider();
var token = await tokenProvider.GetAccessTokenAsync("https://storage.azure.com/");
var credentials = new StorageCredentials(new TokenCredential(token));
var storageAccount = new CloudStorageAccount(credentials, "mystorageaccount", "core.windows.net", useHttps: true);
var client = storageAccount.CreateCloudBlobClient();
var container = client.GetContainerReference("my-key-container");

// optional - provision the container automatically
await container.CreateIfNotExistsAsync();

services.AddDataProtection()
    .PersistKeysToAzureBlobStorage(container, "keys.xml");
```

Per [ulteriori informazioni sulla configurazione dell'autenticazione da servizio a servizio](/azure/key-vault/service-to-service-authentication) , vedere.

## <a name="redis"></a>Redis

::: moniker range=">= aspnetcore-2.2"

Il pacchetto [Microsoft. AspNetCore. dataprotection. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) consente di archiviare le chiavi di protezione dei dati in una cache Redis. Le chiavi possono essere condivisi tra più istanze di un'app web. Le app possono condividere i cookie di autenticazione o CSRF protection tra più server.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Il pacchetto [Microsoft. AspNetCore. dataprotection. Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) consente di archiviare le chiavi di protezione dei dati in una cache Redis. Le chiavi possono essere condivisi tra più istanze di un'app web. Le app possono condividere i cookie di autenticazione o CSRF protection tra più server.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Per configurare in Redis, chiamare uno dei [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) Overload:

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

Per configurare in Redis, chiamare uno dei [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) Overload:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

Per altre informazioni, vedere i seguenti argomenti:

* [ConnectionMultiplexer stackexchange. Redis](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [Cache Redis di Azure](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [esempi di ASPNET/DataProtection](https://github.com/aspnet/AspNetCore/tree/2.2.0/src/DataProtection/samples)

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

Per configurare il provider di EF Core, chiamare il metodo [PersistKeysToDbContext\<TContext >](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) :

[!code-csharp[Main](key-storage-providers/sample/Startup.cs?name=snippet&highlight=13-20)]

Il parametro generico, `TContext`, deve ereditare da [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) e implementare [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):

[!code-csharp[Main](key-storage-providers/sample/MyKeysContext.cs)]

Creare la tabella `DataProtectionKeys`.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Eseguire i comandi seguenti nella finestra **console di gestione pacchetti** (PMC):

```PowerShell
Add-Migration AddDataProtectionKeys -Context MyKeysContext
Update-Database -Context MyKeysContext
```

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Eseguire i comandi seguenti in una shell dei comandi:

```dotnetcli
dotnet ef migrations add AddDataProtectionKeys --context MyKeysContext
dotnet ef database update --context MyKeysContext
```

---

`MyKeysContext` è l'`DbContext` definito nell'esempio di codice precedente. Se si usa un `DbContext` con un nome diverso, sostituire il nome del `DbContext` per `MyKeysContext`.

Il `DataProtectionKeys` classe/entità adotta la struttura illustrata nella tabella seguente.

| Proprietà/campo | Tipo CLR | Tipo SQL              |
| -------------- | -------- | --------------------- |
| `Id`           | `int`    | `int`, PK, not null   |
| `FriendlyName` | `string` | `nvarchar(MAX)`, null |
| `Xml`          | `string` | `nvarchar(MAX)`, null |

::: moniker-end

## <a name="custom-key-repository"></a>Archivio chiave personalizzato

Se i meccanismi in arrivo non sono appropriati, lo sviluppatore può specificare il proprio meccanismo di persistenza chiave, fornendo un oggetto personalizzato [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).
