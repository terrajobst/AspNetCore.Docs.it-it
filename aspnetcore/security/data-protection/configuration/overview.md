---
title: Configurazione della protezione dati
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0e4881a3-a94d-4e35-9c1c-f025d65dcff0
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 39fab796c24456d61a6a103c4a3f7a8722b4718c
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/25/2017
---
# <a name="configuring-data-protection"></a>Configurazione della protezione dati

<a name=data-protection-configuring></a>

Quando viene inizializzato il sistema di protezione dati applica alcuni [impostazioni predefinite](default-settings.md#data-protection-default-settings) in base all'ambiente operativo. Queste impostazioni sono in genere utile per le applicazioni in esecuzione in un singolo computer. Vi sono casi in cui uno sviluppatore potrebbe essere necessario modificare questi (probabilmente perché l'applicazione viene distribuito tra più computer o per motivi di conformità), e per questi scenari, il sistema di protezione dati offre un'API di configurazione avanzate.

<a name=data-protection-configuration-callback></a>

È un metodo di estensione AddDataProtection che restituisce un IDataProtectionBuilder che a sua volta espone i metodi di estensione che è possibile concatenare per configurare la protezione dei dati di varie opzioni. Ad esempio, per archiviare le chiavi in una condivisione UNC anziché % LOCALAPPDATA % (impostazione predefinita), configurare il sistema come segue:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

>[!WARNING]
> Se si modifica il percorso della chiave di persistenza, il sistema non viene più crittografare chiavi inattivi, poiché non è chiaro se DPAPI è un meccanismo di crittografia appropriati.

<a name=configuring-x509-certificate></a>

È possibile configurare il sistema per proteggere le chiavi inattivi chiamando uno del ProtectKeysWith\* le API di configurazione. Si consideri l'esempio seguente, che archivia le chiavi in una condivisione UNC e consente di crittografare le chiavi inattivi con un certificato x. 509 specifico.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

Vedere [crittografia chiave](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) per altri esempi e per informazioni sui meccanismi di crittografia con chiave incorporata.

Per configurare il sistema per l'utilizzo predefinito di una durata 14 giorni anziché 90 giorni, tenere presente quanto segue:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

Per impostazione predefinita il sistema di protezione dati consente di isolare le applicazioni da un altro, anche se essi condividono lo stesso repository chiave fisico. In questo modo le applicazioni dalla comprensione di altro payload protetto. Per condividere un payload protetto tra due diverse applicazioni, configurare il sistema passando il nome dell'applicazione stessa sia per le applicazioni come nell'esempio seguente:

<a name=data-protection-code-sample-application-name></a>

<!-- literal_block {"ids": ["data-protection-code-sample-application-name"], "linenos": false, "names": ["data-protection-code-sample-application-name"], "xml:space": "preserve", "language": "csharp"} -->

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("my application");
}
```

<a name=data-protection-configuring-disable-automatic-key-generation></a>

Infine, è possibile uno scenario in cui non si desidera un'applicazione per distribuire automaticamente le chiavi come l'approccio della scadenza. Un esempio potrebbe essere impostate in una relazione primaria / secondaria, in cui solo l'applicazione principale è responsabile per motivi di gestione delle chiavi e tutte le applicazioni secondarie sono semplicemente una visualizzazione di sola lettura dell'anello chiave applicazioni. Le applicazioni secondarie possono essere configurate per considerare la gestione delle chiavi in sola lettura per la configurazione del sistema come indicato di seguito:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

<a name=data-protection-configuration-per-app-isolation></a>

## <a name="per-application-isolation"></a>Isolamento per ogni applicazione

Quando il sistema di protezione dati viene fornito da un host ASP.NET Core, automaticamente verrà isolare le applicazioni da un altro, anche se tali applicazioni sono in esecuzione con lo stesso account di processo di lavoro e utilizzano il materiale della chiave master stesso. Ciò è simile al modificatore IsolateApps da System. Web <machineKey> elemento.

Funzionamento del meccanismo di isolamento considerando ogni applicazione nel computer locale come tenant univoco, pertanto l'oggetto IDataProtector rooted automaticamente per qualsiasi applicazione include l'ID dell'applicazione come discriminatore. ID univoco dell'applicazione proviene da una delle due posizioni.

1. Se l'applicazione è ospitata in IIS, l'identificatore univoco è il percorso di configurazione dell'applicazione. Se un'applicazione viene distribuita in un ambiente di farm, questo valore deve essere stabile, supponendo che gli ambienti di IIS vengono configurati in modo analogo in tutti i computer nella farm.

2. Se l'applicazione non è ospitato in IIS, l'identificatore univoco è il percorso fisico dell'applicazione.

L'identificatore univoco è progettato per superare Reimposta - della singola applicazione e della macchina virtuale.

Questo meccanismo di isolamento si presuppone che le applicazioni non siano dannose. Un'applicazione dannosa sempre può influire su qualsiasi altra applicazione in esecuzione con lo stesso account di processo di lavoro. In un ambiente di hosting condiviso in cui le applicazioni sono reciprocamente attendibili, il provider di hosting deve eseguire i passaggi per garantire l'isolamento a livello del sistema operativo tra le applicazioni, tra cui la separazione delle applicazioni sottostante repository chiave.

Se il sistema di protezione dati non viene fornito da un host ASP.NET Core (ad esempio, se lo sviluppatore ne crea un'istanza se stesso tramite il tipo concreto DataProtectionProvider), l'isolamento delle applicazioni è disabilitato per impostazione predefinita e tutte le applicazioni supportate da reimpostazione della chiave stessa materiale può condividere i payload come forniscono gli scopi appropriati. Per garantire l'isolamento delle applicazioni in questo ambiente, chiamare il metodo SetApplicationName sull'oggetto di configurazione, vedere il [nell'esempio di codice](#data-protection-code-sample-application-name) sopra.

<a name=data-protection-changing-algorithms></a>

## <a name="changing-algorithms"></a>Algoritmi di modifica

Lo stack di protezione dati consente di modificare l'algoritmo predefinito usato dalle chiavi appena generato. Il modo più semplice per eseguire questa operazione consiste nel chiamare UseCryptographicAlgorithms dal callback di configurazione, ad esempio l'esempio seguente.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

Predefiniti EncryptionAlgorithm e ValidationAlgorithm sono AES-256-CBC e HMACSHA256, rispettivamente. Il criterio predefinito può essere impostato dall'amministratore di sistema tramite [criterio per l'intero computer](machine-wide-policy.md), ma una chiamata esplicita a UseCryptographicAlgorithms sostituirà i criteri predefiniti.

La chiamata UseCryptographicAlgorithms consente allo sviluppatore di specificare l'algoritmo desiderato (da un elenco incorporato predefinito) e lo sviluppatore non è necessario preoccuparsi di implementazione dell'algoritmo. Ad esempio, nello scenario sopra il sistema di protezione dati tenterà di utilizzare l'implementazione di CNG di AES se in esecuzione su Windows, in caso contrario eseguirà il fallback alla classe System.Security.Cryptography.Aes gestita.

Lo sviluppatore può specificare un'implementazione manualmente se si desidera tramite una chiamata a UseCustomCryptographicAlgorithms, come illustrato di seguito alcuni esempi.

>[!TIP]
> La modifica di algoritmi non influisce sulle chiavi esistenti dell'anello di chiave. Riguarda solo le chiavi appena generato.

<a name=data-protection-changing-algorithms-custom-managed></a>

### <a name="specifying-custom-managed-algorithms"></a>Specifica gli algoritmi gestiti personalizzati

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

Per specificare gli algoritmi gestiti personalizzati, creare un'istanza di ManagedAuthenticatedEncryptorConfiguration che punta ai tipi di implementazione.

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new ManagedAuthenticatedEncryptorConfiguration()
    {
        // a type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // a type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

Per specificare gli algoritmi gestiti personalizzati, creare un'istanza di ManagedAuthenticatedEncryptionSettings che punta ai tipi di implementazione.

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new ManagedAuthenticatedEncryptionSettings()
    {
        // a type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // a type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

---

In genere il \*le proprietà del tipo devono puntare a concreto, istanziabili (tramite un costruttore senza parametri pubblico) implementazioni SymmetricAlgorithm e KeyedHashAlgorithm, anche se alcuni valori di casi speciali di sistema-come typeof(Aes) per praticità .

> [!NOTE]
> Il SymmetricAlgorithm deve avere una lunghezza della chiave di ≤ 128 bit e una dimensione del blocco di ≥ 64 bit e deve supportare la crittografia in modalità CBC con riempimento PKCS #7. Il KeyedHashAlgorithm deve avere una dimensione di digest di > = 128 bit, e deve supportare le chiavi di lunghezza uguale alla lunghezza di digest dell'algoritmo hash. Il KeyedHashAlgorithm non è strettamente necessaria per essere HMAC.

<a name=data-protection-changing-algorithms-cng></a>

### <a name="specifying-custom-windows-cng-algorithms"></a>Specifica gli algoritmi CNG di Windows personalizzati

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

Per specificare un algoritmo CNG di Windows personalizzato utilizzando la crittografia in modalità CBC + convalida HMAC, creare un'istanza di CngCbcAuthenticatedEncryptorConfiguration contenente le informazioni algoritmiche.

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

Per specificare un algoritmo CNG di Windows personalizzato utilizzando la crittografia in modalità CBC + convalida HMAC, creare un'istanza di CngCbcAuthenticatedEncryptionSettings contenente le informazioni algoritmiche.

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngCbcAuthenticatedEncryptionSettings()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

---

> [!NOTE]
> L'algoritmo di crittografia simmetrica blocco deve avere una lunghezza della chiave di ≤ 128 bit e una dimensione del blocco di ≥ 64 bit e deve supportare la crittografia in modalità CBC con riempimento PKCS #7. L'algoritmo hash deve avere una dimensione di digest di > = 128 bit e deve supportare viene aperto con il flag BCRYPT_ALG_HANDLE_HMAC_FLAG. Il \*le proprietà del Provider possono essere impostate su null per utilizzare il provider predefinito per l'algoritmo specificato. Vedere il [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentazione per ulteriori informazioni.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

Per specificare un algoritmo CNG di Windows personalizzato utilizzando la crittografia della modalità Galois/contatore + convalida, creare un'istanza di CngGcmAuthenticatedEncryptorConfiguration contenente le informazioni algoritmiche.

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

Per specificare un algoritmo CNG di Windows personalizzato utilizzando la crittografia della modalità Galois/contatore + convalida, creare un'istanza di CngGcmAuthenticatedEncryptionSettings contenente le informazioni algoritmiche.

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngGcmAuthenticatedEncryptionSettings()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

---

> [!NOTE]
> L'algoritmo di crittografia simmetrica blocco deve avere una lunghezza della chiave di ≤ 128 bit e una dimensione del blocco di esattamente a 128 bit e deve supportare la crittografia GCM. La proprietà EncryptionAlgorithmProvider può essere impostata su null da utilizzare il provider predefinito per l'algoritmo specificato. Vedere il [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentazione per ulteriori informazioni.

### <a name="specifying-other-custom-algorithms"></a>Specifica altri algoritmi personalizzati

Se non è esposta come un'API di prima classe, il sistema di protezione dati è estendibile per specificare qualsiasi tipo di algoritmo. Ad esempio, è possibile mantenere tutte le chiavi contenute all'interno di un modulo HSM e per fornire un'implementazione personalizzata di base di routine di crittografia e decrittografia. IAuthenticatedEncryptorConfiguration nella sezione di estendibilità principali crittografia per ulteriori informazioni, vedere.

### <a name="see-also"></a>Vedere anche

* [Scenari di supporto DI non](non-di-scenarios.md)
* [Criterio per l'intero computer](machine-wide-policy.md)
