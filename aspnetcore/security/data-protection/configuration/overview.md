---
title: Configurare la protezione dei dati di ASP.NET Core
author: rick-anderson
description: Informazioni su come configurare la protezione dei dati in ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2017
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 0377fe9fbe5a1eeddb384443370751baa3c0ee43
ms.sourcegitcommit: 8bf4dff3069e62972c1b0839a93fb444e502afe7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2018
ms.locfileid: "46482996"
---
# <a name="configure-aspnet-core-data-protection"></a>Configurare la protezione dei dati di ASP.NET Core

Quando il sistema di protezione dei dati viene inizializzato, viene applicato [le impostazioni predefinite](xref:security/data-protection/configuration/default-settings) in base all'ambiente operativo. Queste impostazioni sono in genere adatte per le App in esecuzione in un singolo computer. Vi sono casi in cui uno sviluppatore potrebbe voler modificare le impostazioni predefinite:

* L'app viene distribuita tra più computer.
* Per motivi di conformità.

Per questi scenari, il sistema di protezione dei dati offre un'API di configurazione avanzate.

> [!WARNING]
> Analogamente ai file di configurazione, il gruppo di chiavi di protezione dati devono essere protetti usando le autorizzazioni appropriate. È possibile scegliere di crittografare le chiavi a riposo, ma ciò non impedisce agli utenti malintenzionati di creazione di nuove chiavi. Di conseguenza, la sicurezza dell'app è interessata. Il percorso di archiviazione configurato con la protezione dei dati deve avere l'accesso limitato all'app stessa, simile al modo in cui che si proteggono i file di configurazione. Ad esempio, se si sceglie di archiviare il gruppo di chiavi su disco, usare le autorizzazioni del file. Assicurarsi che solo l'identità con cui eseguire l'app web ha leggere, scrivere e creare l'accesso a tale directory. Se si Usa archiviazione tabelle di Azure, solo l'app web deve avere la possibilità di leggere, scrivere o creare nuove voci nell'archivio tabelle di e così via.
>
> Il metodo di estensione [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) restituisce un [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder). `IDataProtectionBuilder` espone i metodi di estensione che è possibile concatenare per configurare la protezione dati opzioni.

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a>ProtectKeysWithAzureKeyVault

Per archiviare le chiavi nel [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configurare il sistema con [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) nel `Startup` classe:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

Impostare il percorso di archiviazione chiavi (ad esempio, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)). Il percorso deve essere impostato perché la chiamata `ProtectKeysWithAzureKeyVault` implementa un' [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) che disattiva le impostazioni di protezione automatica dei dati, tra cui la posizione di archiviazione del gruppo di chiavi. L'esempio precedente Usa l'archiviazione Blob di Azure per rendere persistente il gruppo di chiavi. Per altre informazioni, vedere [provider di archiviazione chiavi: Azure e Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis). È anche possibile salvare il gruppo di chiavi in locale con [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).

Il `keyIdentifier` è l'identificatore di chiave di insieme di credenziali delle chiavi usato per la chiave di crittografia (ad esempio, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).

`ProtectKeysWithAzureKeyVault` Overload:

* [ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) consente l'uso di un [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) per abilitare il sistema di protezione dati usare l'insieme di credenziali delle chiavi.
* [ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) consente l'uso di un `ClientId` e [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) per abilitare il sistema di protezione dati usare l'insieme di credenziali delle chiavi.
* [ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) consente l'uso di un `ClientId` e `ClientSecret` per abilitare il sistema di protezione dati usare l'insieme di credenziali delle chiavi.

::: moniker-end

## <a name="persistkeystofilesystem"></a>PersistKeysToFileSystem

Per archiviare le chiavi in una condivisione UNC invece che nel *% LOCALAPPDATA %* percorso predefinito, configurare il sistema con [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> Se si modifica il percorso di persistenza chiave, il sistema di Crittografa non sono più automaticamente le chiavi a riposo, perché non conoscere se DPAPI è un meccanismo di crittografia appropriato.

## <a name="protectkeyswith"></a>ProtectKeysWith\*

È possibile configurare il sistema per proteggere le chiavi a riposo mediante la chiamata a uno qualsiasi dei [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) API di configurazione. Si consideri l'esempio seguente, che archivia le chiavi in una condivisione UNC e consente di crittografare tali chiavi a riposo con un certificato X.509 specifico:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

::: moniker range=">= aspnetcore-2.1"

In ASP.NET Core 2.1 o versioni successive, è possibile fornire un' [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) al [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate), ad esempio un certificato caricato da un file:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate(
            new X509Certificate2("certificate.pfx", "password"));
}
```

::: moniker-end

Visualizzare [chiave di crittografia dei dati](xref:security/data-protection/implementation/key-encryption-at-rest) per altri esempi e una discussione sui meccanismi di crittografia della chiave incorporato.

::: moniker range=">= aspnetcore-2.1"

## <a name="unprotectkeyswithanycertificate"></a>UnprotectKeysWithAnyCertificate

In ASP.NET Core 2.1 o versioni successive, è possibile ruotare i certificati e la decrittografia di chiavi a riposo usando la matrice di [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) certificati con [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate(
            new X509Certificate2("certificate.pfx", "password"));
        .UnprotectKeysWithAnyCertificate(
            new X509Certificate2("certificate_old_1.pfx", "password_1"),
            new X509Certificate2("certificate_old_2.pfx", "password_2"));
}
```

::: moniker-end

## <a name="setdefaultkeylifetime"></a>SetDefaultKeyLifetime

Per configurare il sistema per l'uso di durata di una chiave di 14 giorni anziché il valore predefinito di 90 giorni, usare [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a>SetApplicationName

Per impostazione predefinita, il sistema di protezione dati consente di isolare le app da uno a altro, anche se si condivide lo stesso repository chiave fisico. Ciò impedisce che le app la comprensione di payload protetti di altro. Per condividere payload protetti tra due App, usare [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) con lo stesso valore per ogni app:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a>DisableAutomaticKeyGeneration

È possibile uno scenario in cui non si desidera distribuire automaticamente le chiavi (creazione di nuove chiavi) come approccio alla scadenza di un'app. Un esempio di questo può essere impostate in una relazione primario/secondario, solo l'app principale è responsabile di competenze di gestione delle chiavi e App secondaria hanno semplicemente una vista di sola lettura del gruppo di chiavi in cui le app. Le app secondarie possono essere configurate per considerare il gruppo di chiavi di sola lettura tramite la configurazione di sistema con [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a>Isolamento per ogni applicazione

Quando il sistema di protezione dei dati viene fornito da un host ASP.NET Core, automaticamente consente di isolare le app da uno a altro, anche se queste App sono in esecuzione con lo stesso account di processo di lavoro e utilizzano il materiale della chiave master stesso. Questo è un po' come il modificatore IsolateApps da System. Web  **\<machineKey >** elemento.

Il meccanismo di isolamento funziona da prendere in considerazione ogni app nel computer locale come tenant univoco, pertanto il [oggetto IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted per un'app include automaticamente l'ID dell'app come discriminatore. ID univoco dell'app deriva da una delle due posizioni:

1. Se l'app è ospitata in IIS, l'identificatore univoco è il percorso di configurazione dell'app. Se un'app viene distribuita in un ambiente web farm, questo valore deve essere stabile, presupponendo che gli ambienti di IIS vengono configurati in modo analogo tra tutte le macchine nella web farm.

2. Se l'app non è ospitato in IIS, l'identificatore univoco è il percorso fisico dell'app.

L'identificatore univoco è progettato per sopravvivere alle reimpostazioni &mdash; dell'app per le singole e della macchina stessa.

Questo meccanismo di isolamento si presuppone che l'App non siano dannose. Un'applicazione dannosa sempre può influire su qualsiasi altra app in esecuzione con lo stesso account di processo di lavoro. In un ambiente di hosting condiviso in cui le app sono reciprocamente attendibili, provider di hosting deve provvedere a garantire isolamento a livello di sistema operativo tra le app, tra cui la separazione delle App sottostante repository della chiave.

Se il sistema di protezione dei dati non viene fornito da un host ASP.NET Core (ad esempio, se si crea un'istanza di quest'ultima con il `DataProtectionProvider` tipo concreto) isolamento delle app è disabilitata per impostazione predefinita. Quando l'isolamento delle app è disabilitata, tutte le app supportate dal materiale della chiave stesso possono condividere i payload, purché forniscono appropriato [scopi](xref:security/data-protection/consumer-apis/purpose-strings). Per fornire isolamento delle app in questo ambiente, chiamare il [SetApplicationName](#setapplicationname) metodo sulla configurazione dell'oggetto e specificare un nome univoco per ogni app.

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a>Modifica gli algoritmi con UseCryptographicAlgorithms

Lo stack di protezione dei dati consente di modificare l'algoritmo predefinito usato dalle chiavi appena generato. Il modo più semplice per eseguire questa operazione consiste nel chiamare [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) dal callback di configurazione:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

Il valore predefinito EncryptionAlgorithm è AES-256-CBC e il valore predefinito ValidationAlgorithm è HMACSHA256. Il criterio predefinito può essere impostato dall'amministratore di sistema tramite un [dei criteri a livello di computer](xref:security/data-protection/configuration/machine-wide-policy), ma una chiamata esplicita a `UseCryptographicAlgorithms` sovrascrivono i criteri predefiniti.

La chiamata `UseCryptographicAlgorithms` consente di specificare l'algoritmo desiderato da un elenco incorporato predefinito. Non è necessario preoccuparsi di implementazione dell'algoritmo. Nello scenario precedente, il sistema di protezione dei dati tenta di usare l'implementazione di CNG di AES se in esecuzione su Windows. In caso contrario, esegue il fallback a managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) classe.

È possibile specificare manualmente un'implementazione tramite una chiamata verso [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).

> [!TIP]
> Gli algoritmi di modifica non influenza le chiavi esistenti nel gruppo di chiavi. Riguarda solo le chiavi appena generato.

### <a name="specifying-custom-managed-algorithms"></a>Specifica di algoritmi personalizzati gestiti

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Per specificare gli algoritmi gestiti personalizzati, creare un [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) istanza che fa riferimento ai tipi di implementazione:

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptorConfiguration()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Per specificare gli algoritmi gestiti personalizzati, creare un [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) istanza che fa riferimento ai tipi di implementazione:

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptionSettings()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

---

In genere il \*proprietà del tipo devono puntare a concreto, implementazioni istanziabile (tramite un costruttore senza parametri pubblico) di [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) e [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), anche se la casi di speciali del sistema, ad esempio alcuni valori `typeof(Aes)` per motivi di praticità.

> [!NOTE]
> Il SymmetricAlgorithm deve avere una lunghezza della chiave di ≤ 128 bit e una dimensione del blocco di ≥ 64 bit e deve supportare la crittografia in modalità CBC con riempimento PKCS #7. Il KeyedHashAlgorithm deve avere una dimensione del digest di > = 128 bit, e deve supportare le chiavi di lunghezza uguale alla lunghezza di digest dell'algoritmo hash. Il KeyedHashAlgorithm non è strettamente necessaria essere HMAC.

### <a name="specifying-custom-windows-cng-algorithms"></a>Specifica gli algoritmi CNG di Windows personalizzati

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Per specificare un algoritmo CNG di Windows personalizzato usando la crittografia in modalità CBC con convalida HMAC, creare un [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) istanza che contiene le informazioni su algoritmi:

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Per specificare un algoritmo CNG di Windows personalizzato usando la crittografia in modalità CBC con convalida HMAC, creare un [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) istanza che contiene le informazioni su algoritmi:

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

---

> [!NOTE]
> L'algoritmo di crittografia a blocchi simmetriche debba avere una lunghezza della chiave di > = 128 bit, una dimensione del blocco di > = 64 bit, e deve supportare la crittografia in modalità CBC con riempimento PKCS #7. L'algoritmo di hash deve avere una dimensione del digest di > = 128 bit e devono supportare viene aperto con la BCRYPT\_ALG\_gestire\_HMAC\_FLAG flag. Il \*proprietà Provider possono essere impostate su null per usare il provider predefinito per l'algoritmo specificato. Vedere le [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) per altre informazioni.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Per specificare un algoritmo CNG di Windows personalizzato usando la crittografia/contatore Galois modalità con la convalida, creare un [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) istanza che contiene le informazioni su algoritmi:

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Per specificare un algoritmo CNG di Windows personalizzato usando la crittografia/contatore Galois modalità con la convalida, creare un [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) istanza che contiene le informazioni su algoritmi:

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

---

> [!NOTE]
> L'algoritmo di crittografia a blocchi simmetriche debba avere una lunghezza della chiave di > = 128 bit, una dimensione del blocco di esattamente a 128 bit, e deve supportare la crittografia di GCM. È possibile impostare il [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) proprietà su null per usare il provider predefinito per l'algoritmo specificato. Vedere le [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) per altre informazioni.

### <a name="specifying-other-custom-algorithms"></a>Specifica altri algoritmi personalizzati

Anche se non è esposta come un'API di alto livello, il sistema di protezione dei dati è sufficientemente estensibile per consentire la specifica di qualsiasi tipo di algoritmo. Ad esempio, è possibile mantenere tutte le chiavi contenute all'interno di un modulo di protezione Hardware (HSM) e per fornire un'implementazione personalizzata di una core routine di crittografia e decrittografia. Visualizzare [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) nelle [estendibilità della crittografia di base](xref:security/data-protection/extensibility/core-crypto) per altre informazioni.

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a>Mantenimento delle chiavi per l'hosting in un contenitore Docker

Durante l'hosting di un [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) contenitore, le chiavi devono essere mantenute in uno:

* Una cartella che è un volume di Docker che persiste oltre la durata del contenitore, ad esempio un volume condiviso o un volume montato host.
* Un provider esterno, ad esempio [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) oppure [Redis](https://redis.io/).

## <a name="see-also"></a>Vedere anche

* <xref:security/data-protection/configuration/non-di-scenarios>
* <xref:security/data-protection/configuration/machine-wide-policy>
* <xref:host-and-deploy/web-farm>
