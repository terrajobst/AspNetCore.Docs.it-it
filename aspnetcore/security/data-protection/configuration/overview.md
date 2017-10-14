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
ms.openlocfilehash: d35e0e806999ffd2e0f8f82e0adfc940ea2b503d
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2017
---
# <a name="configuring-data-protection"></a><span data-ttu-id="996a5-103">Configurazione della protezione dati</span><span class="sxs-lookup"><span data-stu-id="996a5-103">Configuring data protection</span></span>

<a name="data-protection-configuring"></a>

<span data-ttu-id="996a5-104">Quando viene inizializzato il sistema di protezione dati applica alcuni [impostazioni predefinite](default-settings.md#data-protection-default-settings) in base all'ambiente operativo.</span><span class="sxs-lookup"><span data-stu-id="996a5-104">When the data protection system is initialized it applies some [default settings](default-settings.md#data-protection-default-settings) based on the operational environment.</span></span> <span data-ttu-id="996a5-105">Queste impostazioni sono in genere utile per le applicazioni in esecuzione in un singolo computer.</span><span class="sxs-lookup"><span data-stu-id="996a5-105">These settings are generally good for applications running on a single machine.</span></span> <span data-ttu-id="996a5-106">Vi sono casi in cui uno sviluppatore potrebbe essere necessario modificare questi (probabilmente perché l'applicazione viene distribuito tra più computer o per motivi di conformità), e per questi scenari, il sistema di protezione dati offre un'API di configurazione avanzate.</span><span class="sxs-lookup"><span data-stu-id="996a5-106">There are some cases where a developer may want to change these (perhaps because their application is spread across multiple machines or for compliance reasons), and for these scenarios the data protection system offers a rich configuration API.</span></span>

<a name="data-protection-configuration-callback"></a>

<span data-ttu-id="996a5-107">È un metodo di estensione AddDataProtection che restituisce un IDataProtectionBuilder che a sua volta espone i metodi di estensione che è possibile concatenare per configurare la protezione dei dati di varie opzioni.</span><span class="sxs-lookup"><span data-stu-id="996a5-107">There is an extension method AddDataProtection which returns an IDataProtectionBuilder which itself exposes extension methods that you can chain together to configure various data protection options.</span></span> <span data-ttu-id="996a5-108">Ad esempio, per archiviare le chiavi in una condivisione UNC anziché % LOCALAPPDATA % (impostazione predefinita), configurare il sistema come segue:</span><span class="sxs-lookup"><span data-stu-id="996a5-108">For instance, to store keys at a UNC share instead of %LOCALAPPDATA% (the default), configure the system as follows:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

>[!WARNING]
> <span data-ttu-id="996a5-109">Se si modifica il percorso della chiave di persistenza, il sistema non viene più crittografare chiavi inattivi, poiché non è chiaro se DPAPI è un meccanismo di crittografia appropriati.</span><span class="sxs-lookup"><span data-stu-id="996a5-109">If you change the key persistence location, the system will no longer automatically encrypt keys at rest since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

<a name="configuring-x509-certificate"></a>

<span data-ttu-id="996a5-110">È possibile configurare il sistema per proteggere le chiavi inattivi chiamando uno del ProtectKeysWith\* le API di configurazione.</span><span class="sxs-lookup"><span data-stu-id="996a5-110">You can configure the system to protect keys at rest by calling any of the ProtectKeysWith\* configuration APIs.</span></span> <span data-ttu-id="996a5-111">Si consideri l'esempio seguente, che archivia le chiavi in una condivisione UNC e consente di crittografare le chiavi inattivi con un certificato x. 509 specifico.</span><span class="sxs-lookup"><span data-stu-id="996a5-111">Consider the example below, which stores keys at a UNC share and encrypts those keys at rest with a specific X.509 certificate.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

<span data-ttu-id="996a5-112">Vedere [crittografia chiave](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) per altri esempi e per informazioni sui meccanismi di crittografia con chiave incorporata.</span><span class="sxs-lookup"><span data-stu-id="996a5-112">See [key encryption at rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more examples and for discussion on the built-in key encryption mechanisms.</span></span>

<span data-ttu-id="996a5-113">Per configurare il sistema per l'utilizzo predefinito di una durata 14 giorni anziché 90 giorni, tenere presente quanto segue:</span><span class="sxs-lookup"><span data-stu-id="996a5-113">To configure the system to use a default key lifetime of 14 days instead of 90 days, consider the following example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

<span data-ttu-id="996a5-114">Per impostazione predefinita il sistema di protezione dati consente di isolare le applicazioni da un altro, anche se essi condividono lo stesso repository chiave fisico.</span><span class="sxs-lookup"><span data-stu-id="996a5-114">By default the data protection system isolates applications from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="996a5-115">In questo modo le applicazioni dalla comprensione di altro payload protetto.</span><span class="sxs-lookup"><span data-stu-id="996a5-115">This prevents the applications from understanding each other's protected payloads.</span></span> <span data-ttu-id="996a5-116">Per condividere un payload protetto tra due diverse applicazioni, configurare il sistema passando il nome dell'applicazione stessa sia per le applicazioni come nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="996a5-116">To share protected payloads between two different applications, configure the system passing in the same application name for both applications as in the below example:</span></span>

<a name="data-protection-code-sample-application-name"></a>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("my application");
}
```

<a name="data-protection-configuring-disable-automatic-key-generation"></a>

<span data-ttu-id="996a5-117">Infine, è possibile uno scenario in cui non si desidera un'applicazione per distribuire automaticamente le chiavi come l'approccio della scadenza.</span><span class="sxs-lookup"><span data-stu-id="996a5-117">Finally, you may have a scenario where you do not want an application to automatically roll keys as they approach expiration.</span></span> <span data-ttu-id="996a5-118">Un esempio potrebbe essere impostate in una relazione primaria / secondaria, in cui solo l'applicazione principale è responsabile per motivi di gestione delle chiavi e tutte le applicazioni secondarie sono semplicemente una visualizzazione di sola lettura dell'anello chiave applicazioni.</span><span class="sxs-lookup"><span data-stu-id="996a5-118">One example of this might be applications set up in a primary / secondary relationship, where only the primary application is responsible for key management concerns, and all secondary applications simply have a read-only view of the key ring.</span></span> <span data-ttu-id="996a5-119">Le applicazioni secondarie possono essere configurate per considerare la gestione delle chiavi in sola lettura per la configurazione del sistema come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="996a5-119">The secondary applications can be configured to treat the key ring as read-only by configuring the system as below:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

<a name="data-protection-configuration-per-app-isolation"></a>

## <a name="per-application-isolation"></a><span data-ttu-id="996a5-120">Isolamento per ogni applicazione</span><span class="sxs-lookup"><span data-stu-id="996a5-120">Per-application isolation</span></span>

<span data-ttu-id="996a5-121">Quando il sistema di protezione dati viene fornito da un host ASP.NET Core, automaticamente verrà isolare le applicazioni da un altro, anche se tali applicazioni sono in esecuzione con lo stesso account di processo di lavoro e utilizzano il materiale della chiave master stesso.</span><span class="sxs-lookup"><span data-stu-id="996a5-121">When the data protection system is provided by an ASP.NET Core host, it will automatically isolate applications from one another, even if those applications are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="996a5-122">Ciò è simile al modificatore IsolateApps da System. Web <machineKey> elemento.</span><span class="sxs-lookup"><span data-stu-id="996a5-122">This is somewhat similar to the IsolateApps modifier from System.Web's <machineKey> element.</span></span>

<span data-ttu-id="996a5-123">Funzionamento del meccanismo di isolamento considerando ogni applicazione nel computer locale come tenant univoco, pertanto l'oggetto IDataProtector rooted automaticamente per qualsiasi applicazione include l'ID dell'applicazione come discriminatore.</span><span class="sxs-lookup"><span data-stu-id="996a5-123">The isolation mechanism works by considering each application on the local machine as a unique tenant, thus the IDataProtector rooted for any given application automatically includes the application ID as a discriminator.</span></span> <span data-ttu-id="996a5-124">ID univoco dell'applicazione proviene da una delle due posizioni.</span><span class="sxs-lookup"><span data-stu-id="996a5-124">The application's unique ID comes from one of two places.</span></span>

1. <span data-ttu-id="996a5-125">Se l'applicazione è ospitata in IIS, l'identificatore univoco è il percorso di configurazione dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="996a5-125">If the application is hosted in IIS, the unique identifier is the application's configuration path.</span></span> <span data-ttu-id="996a5-126">Se un'applicazione viene distribuita in un ambiente di farm, questo valore deve essere stabile, supponendo che gli ambienti di IIS vengono configurati in modo analogo in tutti i computer nella farm.</span><span class="sxs-lookup"><span data-stu-id="996a5-126">If an application is deployed in a farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the farm.</span></span>

2. <span data-ttu-id="996a5-127">Se l'applicazione non è ospitato in IIS, l'identificatore univoco è il percorso fisico dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="996a5-127">If the application is not hosted in IIS, the unique identifier is the physical path of the application.</span></span>

<span data-ttu-id="996a5-128">L'identificatore univoco è progettato per superare Reimposta - della singola applicazione e della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="996a5-128">The unique identifier is designed to survive resets - both of the individual application and of the machine itself.</span></span>

<span data-ttu-id="996a5-129">Questo meccanismo di isolamento si presuppone che le applicazioni non siano dannose.</span><span class="sxs-lookup"><span data-stu-id="996a5-129">This isolation mechanism assumes that the applications are not malicious.</span></span> <span data-ttu-id="996a5-130">Un'applicazione dannosa sempre può influire su qualsiasi altra applicazione in esecuzione con lo stesso account di processo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="996a5-130">A malicious application can always impact any other application running under the same worker process account.</span></span> <span data-ttu-id="996a5-131">In un ambiente di hosting condiviso in cui le applicazioni sono reciprocamente attendibili, il provider di hosting deve eseguire i passaggi per garantire l'isolamento a livello del sistema operativo tra le applicazioni, tra cui la separazione delle applicazioni sottostante repository chiave.</span><span class="sxs-lookup"><span data-stu-id="996a5-131">In a shared hosting environment where applications are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between applications, including separating the applications' underlying key repositories.</span></span>

<span data-ttu-id="996a5-132">Se il sistema di protezione dati non viene fornito da un host ASP.NET Core (ad esempio, se lo sviluppatore ne crea un'istanza se stesso tramite il tipo concreto DataProtectionProvider), l'isolamento delle applicazioni è disabilitato per impostazione predefinita e tutte le applicazioni supportate da reimpostazione della chiave stessa materiale può condividere i payload come forniscono gli scopi appropriati.</span><span class="sxs-lookup"><span data-stu-id="996a5-132">If the data protection system is not provided by an ASP.NET Core host (e.g., if the developer instantiates it himself via the DataProtectionProvider concrete type), application isolation is disabled by default, and all applications backed by the same keying material can share payloads as long as they provide the appropriate purposes.</span></span> <span data-ttu-id="996a5-133">Per garantire l'isolamento delle applicazioni in questo ambiente, chiamare il metodo SetApplicationName sull'oggetto di configurazione, vedere il [nell'esempio di codice](#data-protection-code-sample-application-name) sopra.</span><span class="sxs-lookup"><span data-stu-id="996a5-133">To provide application isolation in this environment, call the SetApplicationName method on the configuration object, see the [code sample](#data-protection-code-sample-application-name) above.</span></span>

<a name="data-protection-changing-algorithms"></a>

## <a name="changing-algorithms"></a><span data-ttu-id="996a5-134">Algoritmi di modifica</span><span class="sxs-lookup"><span data-stu-id="996a5-134">Changing algorithms</span></span>

<span data-ttu-id="996a5-135">Lo stack di protezione dati consente di modificare l'algoritmo predefinito usato dalle chiavi appena generato.</span><span class="sxs-lookup"><span data-stu-id="996a5-135">The data protection stack allows changing the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="996a5-136">Il modo più semplice per eseguire questa operazione consiste nel chiamare UseCryptographicAlgorithms dal callback di configurazione, ad esempio l'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="996a5-136">The simplest way to do this is to call UseCryptographicAlgorithms from the configuration callback, as in the below example.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="996a5-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="996a5-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="996a5-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="996a5-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

<span data-ttu-id="996a5-139">Predefiniti EncryptionAlgorithm e ValidationAlgorithm sono AES-256-CBC e HMACSHA256, rispettivamente.</span><span class="sxs-lookup"><span data-stu-id="996a5-139">The default EncryptionAlgorithm and ValidationAlgorithm are AES-256-CBC and HMACSHA256, respectively.</span></span> <span data-ttu-id="996a5-140">Il criterio predefinito può essere impostato dall'amministratore di sistema tramite [criterio per l'intero computer](machine-wide-policy.md), ma una chiamata esplicita a UseCryptographicAlgorithms sostituirà i criteri predefiniti.</span><span class="sxs-lookup"><span data-stu-id="996a5-140">The default policy can be set by a system administrator via [Machine Wide Policy](machine-wide-policy.md), but an explicit call to UseCryptographicAlgorithms will override the default policy.</span></span>

<span data-ttu-id="996a5-141">La chiamata UseCryptographicAlgorithms consente allo sviluppatore di specificare l'algoritmo desiderato (da un elenco incorporato predefinito) e lo sviluppatore non è necessario preoccuparsi di implementazione dell'algoritmo.</span><span class="sxs-lookup"><span data-stu-id="996a5-141">Calling UseCryptographicAlgorithms will allow the developer to specify the desired algorithm (from a predefined built-in list), and the developer does not need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="996a5-142">Ad esempio, nello scenario sopra il sistema di protezione dati tenterà di utilizzare l'implementazione di CNG di AES se in esecuzione su Windows, in caso contrario eseguirà il fallback alla classe System.Security.Cryptography.Aes gestita.</span><span class="sxs-lookup"><span data-stu-id="996a5-142">For instance, in the scenario above the data protection system will attempt to use the CNG implementation of AES if running on Windows, otherwise it will fall back to the managed System.Security.Cryptography.Aes class.</span></span>

<span data-ttu-id="996a5-143">Lo sviluppatore può specificare un'implementazione manualmente se si desidera tramite una chiamata a UseCustomCryptographicAlgorithms, come illustrato di seguito alcuni esempi.</span><span class="sxs-lookup"><span data-stu-id="996a5-143">The developer can manually specify an implementation if desired via a call to UseCustomCryptographicAlgorithms, as show in the below examples.</span></span>

>[!TIP]
> <span data-ttu-id="996a5-144">La modifica di algoritmi non influisce sulle chiavi esistenti dell'anello di chiave.</span><span class="sxs-lookup"><span data-stu-id="996a5-144">Changing algorithms does not affect existing keys in the key ring.</span></span> <span data-ttu-id="996a5-145">Riguarda solo le chiavi appena generato.</span><span class="sxs-lookup"><span data-stu-id="996a5-145">It only affects newly-generated keys.</span></span>

<a name="data-protection-changing-algorithms-custom-managed"></a>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="996a5-146">Specifica gli algoritmi gestiti personalizzati</span><span class="sxs-lookup"><span data-stu-id="996a5-146">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="996a5-147">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="996a5-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="996a5-148">Per specificare gli algoritmi gestiti personalizzati, creare un'istanza di ManagedAuthenticatedEncryptorConfiguration che punta ai tipi di implementazione.</span><span class="sxs-lookup"><span data-stu-id="996a5-148">To specify custom managed algorithms, create a ManagedAuthenticatedEncryptorConfiguration instance that points to the implementation types.</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="996a5-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="996a5-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="996a5-150">Per specificare gli algoritmi gestiti personalizzati, creare un'istanza di ManagedAuthenticatedEncryptionSettings che punta ai tipi di implementazione.</span><span class="sxs-lookup"><span data-stu-id="996a5-150">To specify custom managed algorithms, create a ManagedAuthenticatedEncryptionSettings instance that points to the implementation types.</span></span>

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

<span data-ttu-id="996a5-151">In genere il \*le proprietà del tipo devono puntare a concreto, istanziabili (tramite un costruttore senza parametri pubblico) implementazioni SymmetricAlgorithm e KeyedHashAlgorithm, anche se alcuni valori di casi speciali di sistema-come typeof(Aes) per praticità .</span><span class="sxs-lookup"><span data-stu-id="996a5-151">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of SymmetricAlgorithm and KeyedHashAlgorithm, though the system special-cases some values like typeof(Aes) for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="996a5-152">Il SymmetricAlgorithm deve avere una lunghezza della chiave di ≤ 128 bit e una dimensione del blocco di ≥ 64 bit e deve supportare la crittografia in modalità CBC con riempimento PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="996a5-152">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="996a5-153">Il KeyedHashAlgorithm deve avere una dimensione di digest di > = 128 bit, e deve supportare le chiavi di lunghezza uguale alla lunghezza di digest dell'algoritmo hash.</span><span class="sxs-lookup"><span data-stu-id="996a5-153">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="996a5-154">Il KeyedHashAlgorithm non è strettamente necessaria per essere HMAC.</span><span class="sxs-lookup"><span data-stu-id="996a5-154">The KeyedHashAlgorithm is not strictly required to be HMAC.</span></span>

<a name="data-protection-changing-algorithms-cng"></a>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="996a5-155">Specifica gli algoritmi CNG di Windows personalizzati</span><span class="sxs-lookup"><span data-stu-id="996a5-155">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="996a5-156">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="996a5-156">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="996a5-157">Per specificare un algoritmo CNG di Windows personalizzato utilizzando la crittografia in modalità CBC + convalida HMAC, creare un'istanza di CngCbcAuthenticatedEncryptorConfiguration contenente le informazioni algoritmiche.</span><span class="sxs-lookup"><span data-stu-id="996a5-157">To specify a custom Windows CNG algorithm using CBC-mode encryption + HMAC validation, create a CngCbcAuthenticatedEncryptorConfiguration instance that contains the algorithmic information.</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="996a5-158">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="996a5-158">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="996a5-159">Per specificare un algoritmo CNG di Windows personalizzato utilizzando la crittografia in modalità CBC + convalida HMAC, creare un'istanza di CngCbcAuthenticatedEncryptionSettings contenente le informazioni algoritmiche.</span><span class="sxs-lookup"><span data-stu-id="996a5-159">To specify a custom Windows CNG algorithm using CBC-mode encryption + HMAC validation, create a CngCbcAuthenticatedEncryptionSettings instance that contains the algorithmic information.</span></span>

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
> <span data-ttu-id="996a5-160">L'algoritmo di crittografia simmetrica blocco deve avere una lunghezza della chiave di ≤ 128 bit e una dimensione del blocco di ≥ 64 bit e deve supportare la crittografia in modalità CBC con riempimento PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="996a5-160">The symmetric block cipher algorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="996a5-161">L'algoritmo hash deve avere una dimensione di digest di > = 128 bit e deve supportare viene aperto con il flag BCRYPT_ALG_HANDLE_HMAC_FLAG.</span><span class="sxs-lookup"><span data-stu-id="996a5-161">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT_ALG_HANDLE_HMAC_FLAG flag.</span></span> <span data-ttu-id="996a5-162">Il \*le proprietà del Provider possono essere impostate su null per utilizzare il provider predefinito per l'algoritmo specificato.</span><span class="sxs-lookup"><span data-stu-id="996a5-162">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="996a5-163">Vedere il [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentazione per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="996a5-163">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="996a5-164">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="996a5-164">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="996a5-165">Per specificare un algoritmo CNG di Windows personalizzato utilizzando la crittografia della modalità Galois/contatore + convalida, creare un'istanza di CngGcmAuthenticatedEncryptorConfiguration contenente le informazioni algoritmiche.</span><span class="sxs-lookup"><span data-stu-id="996a5-165">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption + validation, create a CngGcmAuthenticatedEncryptorConfiguration instance that contains the algorithmic information.</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="996a5-166">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="996a5-166">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="996a5-167">Per specificare un algoritmo CNG di Windows personalizzato utilizzando la crittografia della modalità Galois/contatore + convalida, creare un'istanza di CngGcmAuthenticatedEncryptionSettings contenente le informazioni algoritmiche.</span><span class="sxs-lookup"><span data-stu-id="996a5-167">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption + validation, create a CngGcmAuthenticatedEncryptionSettings instance that contains the algorithmic information.</span></span>

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
> <span data-ttu-id="996a5-168">L'algoritmo di crittografia simmetrica blocco deve avere una lunghezza della chiave di ≤ 128 bit e una dimensione del blocco di esattamente a 128 bit e deve supportare la crittografia GCM.</span><span class="sxs-lookup"><span data-stu-id="996a5-168">The symmetric block cipher algorithm must have a key length of ≥ 128 bits and a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="996a5-169">La proprietà EncryptionAlgorithmProvider può essere impostata su null da utilizzare il provider predefinito per l'algoritmo specificato.</span><span class="sxs-lookup"><span data-stu-id="996a5-169">The EncryptionAlgorithmProvider property can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="996a5-170">Vedere il [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentazione per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="996a5-170">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="996a5-171">Specifica altri algoritmi personalizzati</span><span class="sxs-lookup"><span data-stu-id="996a5-171">Specifying other custom algorithms</span></span>

<span data-ttu-id="996a5-172">Se non è esposta come un'API di prima classe, il sistema di protezione dati è estendibile per specificare qualsiasi tipo di algoritmo.</span><span class="sxs-lookup"><span data-stu-id="996a5-172">Though not exposed as a first-class API, the data protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="996a5-173">Ad esempio, è possibile mantenere tutte le chiavi contenute all'interno di un modulo HSM e per fornire un'implementazione personalizzata di base di routine di crittografia e decrittografia.</span><span class="sxs-lookup"><span data-stu-id="996a5-173">For example, it is possible to keep all keys contained within an HSM and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="996a5-174">IAuthenticatedEncryptorConfiguration nella sezione di estendibilità principali crittografia per ulteriori informazioni, vedere.</span><span class="sxs-lookup"><span data-stu-id="996a5-174">See IAuthenticatedEncryptorConfiguration in the core cryptography extensibility section for more information.</span></span>

### <a name="see-also"></a><span data-ttu-id="996a5-175">Vedere anche</span><span class="sxs-lookup"><span data-stu-id="996a5-175">See also</span></span>

* [<span data-ttu-id="996a5-176">Scenari non compatibili con DI</span><span class="sxs-lookup"><span data-stu-id="996a5-176">Non DI Aware Scenarios</span></span>](non-di-scenarios.md)
* [<span data-ttu-id="996a5-177">Criteri a livello di computer</span><span class="sxs-lookup"><span data-stu-id="996a5-177">Machine Wide Policy</span></span>](machine-wide-policy.md)
