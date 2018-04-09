---
title: Configurare la protezione dei dati di ASP.NET Core
author: rick-anderson
description: Informazioni su come configurare la protezione dei dati in ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 07/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 3a19cec2ce4387ca44ca120f031a072269b93454
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="configure-aspnet-core-data-protection"></a>Configurare la protezione dei dati di ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Quando viene inizializzato il sistema di protezione dei dati, si applica [impostazioni predefinite](xref:security/data-protection/configuration/default-settings) in base all'ambiente operativo. Queste impostazioni sono in genere adatte per le applicazioni in esecuzione in un singolo computer. Vi sono casi in cui uno sviluppatore potrebbe desidera modificare le impostazioni predefinite, probabilmente perché l'app viene distribuito tra più computer o per motivi di conformità. Per questi scenari, il sistema di protezione dei dati offre un'API di configurazione avanzate.

È un metodo di estensione [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) che restituisce un [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder). `IDataProtectionBuilder` espone i metodi di estensione che è possibile concatenare per configurare la protezione dati opzioni.

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
> Se si modifica il percorso della chiave di persistenza, il sistema di Crittografa non viene più chiavi inattivi, poiché non è chiaro se DPAPI è un meccanismo di crittografia appropriati.

## <a name="protectkeyswith"></a>ProtectKeysWith\*

È possibile configurare il sistema per proteggere le chiavi inattivi chiamando uno del [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) le API di configurazione. Si consideri l'esempio riportato di seguito, in cui le chiavi vengono archiviate in una condivisione UNC e consente di crittografare le chiavi inattivi con un certificato x. 509 specifico:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

Vedere [chiave di crittografia](xref:security/data-protection/implementation/key-encryption-at-rest) per ulteriori esempi e una discussione sui meccanismi di crittografia con chiave incorporata.

## <a name="setdefaultkeylifetime"></a>SetDefaultKeyLifetime

Per configurare il sistema per l'utilizzo di una durata di 14 giorni anziché il valore predefinito di 90 giorni, utilizzare [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a>SetApplicationName

Per impostazione predefinita, il sistema di protezione dei dati consente di isolare le applicazioni da un altro, anche se essi condividono lo stesso repository chiave fisico. Ciò impedisce che le app la comprensione di altro payload protetto. Per condividere un payload protetto tra due applicazioni, utilizzare [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) con lo stesso valore per ogni app:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a>DisableAutomaticKeyGeneration

È possibile uno scenario in cui si desidera distribuire automaticamente le chiavi (creazione di nuove chiavi) come l'approccio della scadenza di un'app. Un esempio potrebbe essere le app configurate in una relazione primario o secondario, in cui solo l'app principale è responsabile per motivi di gestione delle chiavi e App secondari hanno semplicemente una visualizzazione di sola lettura dell'anello chiave. Le app secondarie possono essere configurate per considerare l'anello chiave come sola lettura tramite la configurazione di sistema con [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a>Isolamento per ogni applicazione

Quando il sistema di protezione dei dati viene fornito da un host ASP.NET Core, automaticamente consente di isolare le app da un altro, anche se tali applicazioni sono in esecuzione con lo stesso account di processo di lavoro e utilizzano il materiale della chiave master stesso. Ciò è simile al modificatore IsolateApps da System. Web  **\<machineKey >** elemento.

Il meccanismo di isolamento funziona considerando ogni app nel computer locale come tenant univoco, pertanto il [oggetto IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted per qualsiasi applicazione specificata include automaticamente l'ID dell'app come discriminatore. ID univoco dell'applicazione proviene da una delle due posizioni:

1. Se l'applicazione è ospitata in IIS, l'identificatore univoco è il percorso di configurazione dell'applicazione. Se un'applicazione viene distribuita in un ambiente web farm, questo valore deve essere stabile, supponendo che gli ambienti di IIS vengono configurati in modo analogo in tutti i computer nella web farm.

2. Se l'applicazione non è ospitata in IIS, l'identificatore univoco è il percorso fisico dell'app.

L'identificatore univoco è progettato per sopravvivere Reimposta &mdash; di singole app e della macchina virtuale.

Questo meccanismo di isolamento si presuppone che le app non siano dannose. Un'app dannoso sempre può influire su qualsiasi altra app in esecuzione con lo stesso account di processo di lavoro. In un ambiente di hosting condiviso in cui le app sono reciprocamente attendibili, il provider di hosting deve eseguire i passaggi per garantire l'isolamento a livello del sistema operativo tra App, tra cui la separazione delle App sottostante repository chiave.

Se il sistema di protezione dei dati non è specificato da un host ASP.NET Core (ad esempio, se si crea un'istanza di tramite il `DataProtectionProvider` tipo concreto) è disabilitato l'isolamento di app per impostazione predefinita. Quando viene disabilitato l'isolamento di app, tutte le applicazioni supportate dal materiale della chiave stesso possono condividere i payload come forniscono appropriata [scopi](xref:security/data-protection/consumer-apis/purpose-strings). Per garantire l'isolamento di app in questo ambiente, chiamare il [SetApplicationName](#setapplicationname) metodo sulla configurazione dell'oggetto e specificare un nome univoco per ogni app.

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a>Algoritmi di modifica con UseCryptographicAlgorithms

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

Il valore predefinito EncryptionAlgorithm è AES-256-CBC e il valore predefinito ValidationAlgorithm è HMACSHA256. Il criterio predefinito può essere impostato dall'amministratore di sistema tramite un [criteri a livello di computer](xref:security/data-protection/configuration/machine-wide-policy), ma una chiamata esplicita a `UseCryptographicAlgorithms` sostituisce il criterio predefinito.

La chiamata `UseCryptographicAlgorithms` consente di specificare l'algoritmo desiderato da un elenco incorporato predefinito. Non è necessario preoccuparsi di implementazione dell'algoritmo. Nello scenario precedente, il sistema di protezione dei dati tenta di utilizzare l'implementazione di CNG di AES, se in esecuzione su Windows. In caso contrario, viene utilizzata la cartella gestito [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) classe.

È possibile specificare manualmente un'implementazione tramite una chiamata a [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).

> [!TIP]
> Gli algoritmi di modifica non influisce sulla chiavi esistenti dell'anello di chiave. Riguarda solo le chiavi appena generato.

### <a name="specifying-custom-managed-algorithms"></a>Specifica gli algoritmi gestiti personalizzati

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

In genere il \*le proprietà del tipo devono puntare a concreto, istanziabili (tramite un costruttore senza parametri pubblico) implementazioni di [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) e [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), anche se il speciale di sistema-case alcuni valori come `typeof(Aes)` per motivi di praticità.

> [!NOTE]
> Il SymmetricAlgorithm deve avere una lunghezza della chiave di ≤ 128 bit e una dimensione del blocco di ≥ 64 bit e deve supportare la crittografia in modalità CBC con riempimento PKCS #7. Il KeyedHashAlgorithm deve avere una dimensione di digest di > = 128 bit, e deve supportare le chiavi di lunghezza uguale alla lunghezza di digest dell'algoritmo hash. Il KeyedHashAlgorithm non è strettamente necessaria per essere HMAC.

### <a name="specifying-custom-windows-cng-algorithms"></a>Specifica gli algoritmi CNG di Windows personalizzati

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Per specificare un algoritmo CNG di Windows personalizzato utilizzando la crittografia in modalità CBC con convalida HMAC, creare un [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) istanza che contiene le informazioni algoritmiche:

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

Per specificare un algoritmo CNG di Windows personalizzato utilizzando la crittografia in modalità CBC con convalida HMAC, creare un [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) istanza che contiene le informazioni algoritmiche:

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
> L'algoritmo di crittografia simmetrica blocco deve avere una lunghezza della chiave di > = 128 bit, una dimensione del blocco di > = 64 bit, e deve supportare la crittografia in modalità CBC con riempimento PKCS #7. L'algoritmo hash deve avere una dimensione di digest di > = 128 bit e deve supportare viene aperto con il BCRYPT\_ALG\_gestire\_HMAC\_FLAG flag. Il \*le proprietà del Provider possono essere impostate su null per utilizzare il provider predefinito per l'algoritmo specificato. Vedere il [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentazione per ulteriori informazioni.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Per specificare un algoritmo CNG di Windows personalizzato utilizzando la crittografia o dei contatori Galois modalità con la convalida, creare un [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) istanza che contiene le informazioni algoritmiche:

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

Per specificare un algoritmo CNG di Windows personalizzato utilizzando la crittografia o dei contatori Galois modalità con la convalida, creare un [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) istanza che contiene le informazioni algoritmiche:

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
> L'algoritmo di crittografia simmetrica blocco deve avere una lunghezza della chiave di > = 128 bit, una dimensione del blocco di esattamente a 128 bit, e deve supportare la crittografia GCM. È possibile impostare il [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) proprietà null per utilizzare il provider predefinito per l'algoritmo specificato. Vedere il [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentazione per ulteriori informazioni.

### <a name="specifying-other-custom-algorithms"></a>Specifica altri algoritmi personalizzati

Se non è esposta come un'API di prima classe, il sistema di protezione dei dati è estendibile per specificare qualsiasi tipo di algoritmo. Ad esempio, è possibile mantenere tutte le chiavi contenute all'interno di un modulo di protezione Hardware (HSM) e per fornire un'implementazione personalizzata di base di routine di crittografia e decrittografia. Vedere [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [dell'estendibilità della crittografia di base](xref:security/data-protection/extensibility/core-crypto) per ulteriori informazioni.

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a>Mantenimento delle chiavi durante l'hosting in un contenitore Docker

Durante l'hosting in un [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) contenitore chiavi devono essere gestite in uno:

* Una cartella che è un volume di Docker che viene mantenuto oltre la durata del contenitore, ad esempio un volume condiviso o un volume montato host.
* Un provider esterno, ad esempio [insieme credenziali chiavi Azure](https://azure.microsoft.com/services/key-vault/) o [Redis](https://redis.io/).

## <a name="see-also"></a>Vedere anche

* [Scenari non compatibili con DI](xref:security/data-protection/configuration/non-di-scenarios)
* [Criteri a livello di computer](xref:security/data-protection/configuration/machine-wide-policy)
