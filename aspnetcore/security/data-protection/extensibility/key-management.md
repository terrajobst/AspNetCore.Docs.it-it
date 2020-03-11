---
title: Estendibilità della gestione delle chiavi in ASP.NET Core
author: rick-anderson
description: Informazioni sulle estendibilità di gestione delle chiavi di protezione dei dati di ASP.NET Core.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 28932cbef1cc797338980f3e0de8b09caee324c0
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665879"
---
# <a name="key-management-extensibility-in-aspnet-core"></a><span data-ttu-id="ffaa6-103">Estendibilità della gestione delle chiavi in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ffaa6-103">Key management extensibility in ASP.NET Core</span></span>

> [!TIP]
> <span data-ttu-id="ffaa6-104">Leggere la sezione relativa alla [gestione delle chiavi](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) prima di leggere questa sezione, in quanto illustra alcuni dei concetti fondamentali alla base di queste API.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-104">Read the [key management](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

> [!WARNING]
> <span data-ttu-id="ffaa6-105">I tipi che implementano una delle interfacce seguenti devono essere thread-safe per i chiamanti di più.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="ffaa6-106">Chiave</span><span class="sxs-lookup"><span data-stu-id="ffaa6-106">Key</span></span>

<span data-ttu-id="ffaa6-107">L'interfaccia `IKey` è la rappresentazione di base di una chiave in cryptosystem.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-107">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="ffaa6-108">La chiave di termine viene usata in senso astratta, non nel senso letterale di "materiale della chiave crittografica".</span><span class="sxs-lookup"><span data-stu-id="ffaa6-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="ffaa6-109">Una chiave presenta le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="ffaa6-109">A key has the following properties:</span></span>

* <span data-ttu-id="ffaa6-110">Date di attivazione, la creazione e la scadenza</span><span class="sxs-lookup"><span data-stu-id="ffaa6-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="ffaa6-111">Stato di revoca</span><span class="sxs-lookup"><span data-stu-id="ffaa6-111">Revocation status</span></span>

* <span data-ttu-id="ffaa6-112">Identificatore di chiave (GUID)</span><span class="sxs-lookup"><span data-stu-id="ffaa6-112">Key identifier (a GUID)</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ffaa6-113">Inoltre, `IKey` espone un metodo `CreateEncryptor` che può essere usato per creare un'istanza di [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) associata a questa chiave.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-113">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ffaa6-114">Inoltre, `IKey` espone un metodo `CreateEncryptorInstance` che può essere usato per creare un'istanza di [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) associata a questa chiave.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-114">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="ffaa6-115">Non è disponibile alcuna API per recuperare il materiale di crittografia non elaborato da un'istanza di `IKey`.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-115">There's no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="ffaa6-116">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="ffaa6-116">IKeyManager</span></span>

<span data-ttu-id="ffaa6-117">L'interfaccia `IKeyManager` rappresenta un oggetto responsabile dell'archiviazione della chiave generale, del recupero e della manipolazione.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-117">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="ffaa6-118">Che espone tre operazioni generali:</span><span class="sxs-lookup"><span data-stu-id="ffaa6-118">It exposes three high-level operations:</span></span>

* <span data-ttu-id="ffaa6-119">Creare una nuova chiave e renderlo persistente nell'archiviazione.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-119">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="ffaa6-120">Ottiene tutte le chiavi di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-120">Get all keys from storage.</span></span>

* <span data-ttu-id="ffaa6-121">Revoca le chiavi di uno o più e rendere persistenti le informazioni di revoca all'archiviazione.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-121">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="ffaa6-122">La scrittura di un `IKeyManager` è un'attività molto avanzata e la maggior parte degli sviluppatori non dovrebbe tentarla.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-122">Writing an `IKeyManager` is a very advanced task, and the majority of developers shouldn't attempt it.</span></span> <span data-ttu-id="ffaa6-123">Al contrario, la maggior parte degli sviluppatori dovrebbe sfruttare le funzionalità offerte dalla classe [XmlKeyManager](#xmlkeymanager) .</span><span class="sxs-lookup"><span data-stu-id="ffaa6-123">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](#xmlkeymanager) class.</span></span>

## <a name="xmlkeymanager"></a><span data-ttu-id="ffaa6-124">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="ffaa6-124">XmlKeyManager</span></span>

<span data-ttu-id="ffaa6-125">Il tipo di `XmlKeyManager` è l'implementazione concreta di `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-125">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="ffaa6-126">Fornisce diverse funzionalità utili, inclusa deposito delle chiave e la crittografia delle chiavi a riposo.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-126">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="ffaa6-127">Le chiavi in questo sistema sono rappresentate come elementi XML (in particolare, [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span><span class="sxs-lookup"><span data-stu-id="ffaa6-127">Keys in this system are represented as XML elements (specifically, [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="ffaa6-128">`XmlKeyManager` dipende da diversi altri componenti durante il completamento delle attività:</span><span class="sxs-lookup"><span data-stu-id="ffaa6-128">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="ffaa6-129">`AlgorithmConfiguration`, che determina gli algoritmi usati dalle nuove chiavi.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-129">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="ffaa6-130">`IXmlRepository`, che controlla il punto in cui le chiavi vengono salvate in modo permanente nell'archiviazione.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-130">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="ffaa6-131">`IXmlEncryptor` [optional], che consente di crittografare le chiavi inattive.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-131">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="ffaa6-132">`IKeyEscrowSink` [facoltativo], che fornisce i servizi di deposito chiavi.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-132">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="ffaa6-133">`IXmlRepository`, che controlla il punto in cui le chiavi vengono salvate in modo permanente nell'archiviazione.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-133">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="ffaa6-134">`IXmlEncryptor` [optional], che consente di crittografare le chiavi inattive.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-134">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="ffaa6-135">`IKeyEscrowSink` [facoltativo], che fornisce i servizi di deposito chiavi.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-135">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

<span data-ttu-id="ffaa6-136">Di seguito sono riportati i diagrammi di alto livello che indicano il modo in cui questi componenti sono collegati tra `XmlKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-136">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

::: moniker range=">= aspnetcore-2.0"

![Creazione della chiave](key-management/_static/keycreation2.png)

<span data-ttu-id="ffaa6-138">*Creazione della chiave/CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="ffaa6-138">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="ffaa6-139">Nell'implementazione di `CreateNewKey`, il componente `AlgorithmConfiguration` viene utilizzato per creare una `IAuthenticatedEncryptorDescriptor`univoca, che viene quindi serializzata come XML.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-139">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="ffaa6-140">Se è presente un sink di deposito delle chiavi, il XML non elaborato (non crittografato) viene fornito per il sink per l'archiviazione a lungo termine.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-140">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="ffaa6-141">Il codice XML non crittografato viene quindi eseguito tramite un `IXmlEncryptor` (se necessario) per generare il documento XML crittografato.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-141">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="ffaa6-142">Questo documento crittografato viene salvato in modo permanente nell'archiviazione a lungo termine tramite il `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-142">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="ffaa6-143">Se non è stato configurato alcun `IXmlEncryptor`, il documento non crittografato viene salvato in modo permanente nel `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-143">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![Recupero della chiave](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![Creazione della chiave](key-management/_static/keycreation1.png)

<span data-ttu-id="ffaa6-146">*Creazione della chiave/CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="ffaa6-146">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="ffaa6-147">Nell'implementazione di `CreateNewKey`, il componente `IAuthenticatedEncryptorConfiguration` viene utilizzato per creare una `IAuthenticatedEncryptorDescriptor`univoca, che viene quindi serializzata come XML.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-147">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="ffaa6-148">Se è presente un sink di deposito delle chiavi, il XML non elaborato (non crittografato) viene fornito per il sink per l'archiviazione a lungo termine.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-148">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="ffaa6-149">Il codice XML non crittografato viene quindi eseguito tramite un `IXmlEncryptor` (se necessario) per generare il documento XML crittografato.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-149">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="ffaa6-150">Questo documento crittografato viene salvato in modo permanente nell'archiviazione a lungo termine tramite il `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-150">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="ffaa6-151">Se non è stato configurato alcun `IXmlEncryptor`, il documento non crittografato viene salvato in modo permanente nel `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-151">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![Recupero della chiave](key-management/_static/keyretrieval1.png)

::: moniker-end

<span data-ttu-id="ffaa6-153">*Recupero chiavi/GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="ffaa6-153">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="ffaa6-154">Nell'implementazione di `GetAllKeys`, i documenti XML che rappresentano chiavi e revoche vengono letti dall'`IXmlRepository`sottostante.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-154">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="ffaa6-155">Se questi documenti sono crittografati, il sistema verrà automaticamente decrittografarle.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-155">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="ffaa6-156">`XmlKeyManager` crea le istanze di `IAuthenticatedEncryptorDescriptorDeserializer` appropriate per deserializzare i documenti in `IAuthenticatedEncryptorDescriptor` istanze, che vengono quindi sottoposte a incapsulamento in singole istanze di `IKey`.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-156">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="ffaa6-157">Questa raccolta di istanze di `IKey` viene restituita al chiamante.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-157">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="ffaa6-158">Altre informazioni sui particolari elementi XML sono reperibili nel [documento formato archiviazione chiavi](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="ffaa6-158">Further information on the particular XML elements can be found in the [key storage format document](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="ffaa6-159">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="ffaa6-159">IXmlRepository</span></span>

<span data-ttu-id="ffaa6-160">L'interfaccia `IXmlRepository` rappresenta un tipo che può salvare in modo permanente XML e recuperare XML da un archivio di backup.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-160">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="ffaa6-161">Che espone due API:</span><span class="sxs-lookup"><span data-stu-id="ffaa6-161">It exposes two APIs:</span></span>

* <span data-ttu-id="ffaa6-162">`GetAllElements`:`IReadOnlyCollection<XElement>`</span><span class="sxs-lookup"><span data-stu-id="ffaa6-162">`GetAllElements` :`IReadOnlyCollection<XElement>`</span></span>

* `StoreElement(XElement element, string friendlyName)`

<span data-ttu-id="ffaa6-163">Non è necessario che le implementazioni di `IXmlRepository` analizzino il codice XML passato.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-163">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="ffaa6-164">Devono considerare i documenti XML come opaco e consentire ai livelli superiori di preoccuparsi di generazione e l'analisi dei documenti.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-164">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="ffaa6-165">Sono disponibili quattro tipi concreti incorporati che implementano `IXmlRepository`:</span><span class="sxs-lookup"><span data-stu-id="ffaa6-165">There are four built-in concrete types which implement `IXmlRepository`:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="ffaa6-166">FileSystemXmlRepository</span><span class="sxs-lookup"><span data-stu-id="ffaa6-166">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="ffaa6-167">RegistryXmlRepository</span><span class="sxs-lookup"><span data-stu-id="ffaa6-167">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="ffaa6-168">AzureStorage. AzureBlobXmlRepository</span><span class="sxs-lookup"><span data-stu-id="ffaa6-168">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="ffaa6-169">RedisXmlRepository</span><span class="sxs-lookup"><span data-stu-id="ffaa6-169">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="ffaa6-170">FileSystemXmlRepository</span><span class="sxs-lookup"><span data-stu-id="ffaa6-170">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="ffaa6-171">RegistryXmlRepository</span><span class="sxs-lookup"><span data-stu-id="ffaa6-171">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="ffaa6-172">AzureStorage. AzureBlobXmlRepository</span><span class="sxs-lookup"><span data-stu-id="ffaa6-172">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="ffaa6-173">RedisXmlRepository</span><span class="sxs-lookup"><span data-stu-id="ffaa6-173">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

<span data-ttu-id="ffaa6-174">Per ulteriori informazioni, vedere il documento relativo ai [provider di archiviazione chiavi](xref:security/data-protection/implementation/key-storage-providers) .</span><span class="sxs-lookup"><span data-stu-id="ffaa6-174">See the [key storage providers document](xref:security/data-protection/implementation/key-storage-providers) for more information.</span></span>

<span data-ttu-id="ffaa6-175">La registrazione di un `IXmlRepository` personalizzato è appropriata quando si usa un archivio di backup diverso (ad esempio, archiviazione tabelle di Azure).</span><span class="sxs-lookup"><span data-stu-id="ffaa6-175">Registering a custom `IXmlRepository` is appropriate when using a different backing store (for example, Azure Table Storage).</span></span>

<span data-ttu-id="ffaa6-176">Per modificare il repository predefinito a livello di applicazione, registrare un'istanza di `IXmlRepository` personalizzata:</span><span class="sxs-lookup"><span data-stu-id="ffaa6-176">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
```

::: moniker-end

## <a name="ixmlencryptor"></a><span data-ttu-id="ffaa6-177">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="ffaa6-177">IXmlEncryptor</span></span>

<span data-ttu-id="ffaa6-178">L'interfaccia `IXmlEncryptor` rappresenta un tipo in grado di crittografare un elemento XML non crittografato.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-178">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="ffaa6-179">Che espone una singola API:</span><span class="sxs-lookup"><span data-stu-id="ffaa6-179">It exposes a single API:</span></span>

* <span data-ttu-id="ffaa6-180">Crittografare (XElement plaintextElement): EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="ffaa6-180">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="ffaa6-181">Se un `IAuthenticatedEncryptorDescriptor` serializzato contiene elementi contrassegnati come "requires Encryption", `XmlKeyManager` eseguiranno tali elementi tramite il metodo `Encrypt` del `IXmlEncryptor`configurato e manterrà l'elemento crittografato anziché l'elemento del testo non crittografato nell'`IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-181">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="ffaa6-182">L'output del metodo `Encrypt` è un oggetto `EncryptedXmlInfo`.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-182">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="ffaa6-183">Questo oggetto è un wrapper che contiene sia la `XElement` crittografata risultante sia il tipo che rappresenta una `IXmlDecryptor` che può essere utilizzata per decrittografare l'elemento corrispondente.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-183">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="ffaa6-184">Sono disponibili quattro tipi concreti incorporati che implementano `IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="ffaa6-184">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>

* [<span data-ttu-id="ffaa6-185">CertificateXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="ffaa6-185">CertificateXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [<span data-ttu-id="ffaa6-186">DpapiNGXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="ffaa6-186">DpapiNGXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [<span data-ttu-id="ffaa6-187">DpapiXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="ffaa6-187">DpapiXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [<span data-ttu-id="ffaa6-188">NullXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="ffaa6-188">NullXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

<span data-ttu-id="ffaa6-189">Per ulteriori informazioni, vedere il [documento chiave di crittografia](xref:security/data-protection/implementation/key-encryption-at-rest) dei dati inattivi.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-189">See the [key encryption at rest document](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="ffaa6-190">Per modificare il meccanismo predefinito a livello di applicazione per la crittografia delle chiavi, registrare un'istanza di `IXmlEncryptor` personalizzata:</span><span class="sxs-lookup"><span data-stu-id="ffaa6-190">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
```

::: moniker-end

## <a name="ixmldecryptor"></a><span data-ttu-id="ffaa6-191">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="ffaa6-191">IXmlDecryptor</span></span>

<span data-ttu-id="ffaa6-192">L'interfaccia `IXmlDecryptor` rappresenta un tipo che sa come decrittografare una `XElement` che è stata crittografata tramite un `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-192">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="ffaa6-193">Che espone una singola API:</span><span class="sxs-lookup"><span data-stu-id="ffaa6-193">It exposes a single API:</span></span>

* <span data-ttu-id="ffaa6-194">Decrittografare (XElement encryptedElement): XElement</span><span class="sxs-lookup"><span data-stu-id="ffaa6-194">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="ffaa6-195">Il metodo `Decrypt` Annulla la crittografia eseguita da `IXmlEncryptor.Encrypt`.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-195">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="ffaa6-196">In genere, ogni implementazione di `IXmlEncryptor` concreta avrà un'implementazione `IXmlDecryptor` concreta corrispondente.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-196">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="ffaa6-197">I tipi che implementano `IXmlDecryptor` devono avere uno dei due costruttori pubblici seguenti:</span><span class="sxs-lookup"><span data-stu-id="ffaa6-197">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="ffaa6-198">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="ffaa6-198">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="ffaa6-199">ctor)</span><span class="sxs-lookup"><span data-stu-id="ffaa6-199">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="ffaa6-200">Il `IServiceProvider` passato al costruttore può essere null.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-200">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="ffaa6-201">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="ffaa6-201">IKeyEscrowSink</span></span>

<span data-ttu-id="ffaa6-202">L'interfaccia `IKeyEscrowSink` rappresenta un tipo in grado di eseguire il deposito di informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-202">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="ffaa6-203">Si ricordi che i descrittori serializzati potrebbero contenere informazioni riservate, ad esempio il materiale crittografico, ed è ciò che ha portato all'introduzione del tipo [IXmlEncryptor](#ixmlencryptor) .</span><span class="sxs-lookup"><span data-stu-id="ffaa6-203">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](#ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="ffaa6-204">Tuttavia, succedere e chiave di anelli può essere eliminati o danneggiati.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-204">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="ffaa6-205">L'interfaccia Escrow fornisce un tratteggio di escape di emergenza, che consente l'accesso al codice XML serializzato non elaborato prima che venga trasformato da qualsiasi [IXmlEncryptor](#ixmlencryptor)configurato.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-205">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it's transformed by any configured [IXmlEncryptor](#ixmlencryptor).</span></span> <span data-ttu-id="ffaa6-206">L'interfaccia espone una singola API:</span><span class="sxs-lookup"><span data-stu-id="ffaa6-206">The interface exposes a single API:</span></span>

* <span data-ttu-id="ffaa6-207">Store (keyId Guid, elemento XElement)</span><span class="sxs-lookup"><span data-stu-id="ffaa6-207">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="ffaa6-208">L'implementazione di `IKeyEscrowSink` per gestire l'elemento fornito in modo sicuro è coerente con i criteri di business.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-208">It's up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="ffaa6-209">Una possibile implementazione può essere per il sink del deposito a garanzia per crittografare l'elemento XML usando un certificato X. 509 aziendale noto in cui è stata depositata la chiave privata del certificato. il tipo di `CertificateXmlEncryptor` può risultare utile.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-209">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="ffaa6-210">L'implementazione del `IKeyEscrowSink` è anche responsabile del salvataggio permanente dell'elemento fornito in modo appropriato.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-210">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="ffaa6-211">Per impostazione predefinita, non è abilitato alcun meccanismo di deposito, sebbene gli amministratori del server possano [configurarlo a livello globale](xref:security/data-protection/configuration/machine-wide-policy).</span><span class="sxs-lookup"><span data-stu-id="ffaa6-211">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="ffaa6-212">Può anche essere configurato a livello di programmazione tramite il metodo `IDataProtectionBuilder.AddKeyEscrowSink` come illustrato nell'esempio riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-212">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="ffaa6-213">Gli overload del metodo `AddKeyEscrowSink` rispecchiano gli overload `IServiceCollection.AddSingleton` e `IServiceCollection.AddInstance`, perché le istanze di `IKeyEscrowSink` sono destinate a essere singleton.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-213">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="ffaa6-214">Se vengono registrate più istanze di `IKeyEscrowSink`, ciascuna di esse verrà chiamata durante la generazione della chiave, quindi le chiavi possono essere estratta simultaneamente a più meccanismi.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-214">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="ffaa6-215">Non è disponibile alcuna API per leggere il materiale da un'istanza di `IKeyEscrowSink`.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-215">There's no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="ffaa6-216">Questo comportamento è coerente con la teoria della progettazione del meccanismo di deposito delle chiavi: è destinato a rendere accessibili a un'autorità attendibile il materiale della chiave, e poiché l'applicazione non costituisce un'autorità attendibile, non deve avere accesso al proprio materiale garantite.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-216">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="ffaa6-217">Il codice di esempio seguente illustra la creazione e la registrazione di un `IKeyEscrowSink` in cui vengono depositate le chiavi in modo che solo i membri di "CONTOSODomain Admins" possano ripristinarli.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-217">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="ffaa6-218">Per eseguire questo esempio, è necessario essere in un dominio Windows 8 / computer con Windows Server 2012 e il controller di dominio deve essere Windows Server 2012 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="ffaa6-218">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
