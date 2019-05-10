---
title: Estendibilità della gestione delle chiavi in ASP.NET Core
author: rick-anderson
description: Informazioni sulle estendibilità di gestione delle chiavi di protezione dei dati di ASP.NET Core.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 28932cbef1cc797338980f3e0de8b09caee324c0
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896908"
---
# <a name="key-management-extensibility-in-aspnet-core"></a><span data-ttu-id="15c92-103">Estendibilità della gestione delle chiavi in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="15c92-103">Key management extensibility in ASP.NET Core</span></span>

> [!TIP]
> <span data-ttu-id="15c92-104">Leggere il [gestione delle chiavi](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) sezione prima di leggere questa sezione, perché vengono descritti alcuni dei concetti fondamentali alla base di queste API.</span><span class="sxs-lookup"><span data-stu-id="15c92-104">Read the [key management](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

> [!WARNING]
> <span data-ttu-id="15c92-105">I tipi che implementano una delle interfacce seguenti devono essere thread-safe per i chiamanti di più.</span><span class="sxs-lookup"><span data-stu-id="15c92-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="15c92-106">Chiave</span><span class="sxs-lookup"><span data-stu-id="15c92-106">Key</span></span>

<span data-ttu-id="15c92-107">Il `IKey` interfaccia è la rappresentazione di base di una chiave nel sistema di crittografia.</span><span class="sxs-lookup"><span data-stu-id="15c92-107">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="15c92-108">La chiave di termine viene usata in senso astratta, non nel senso letterale di "materiale della chiave crittografica".</span><span class="sxs-lookup"><span data-stu-id="15c92-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="15c92-109">Una chiave presenta le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="15c92-109">A key has the following properties:</span></span>

* <span data-ttu-id="15c92-110">Date di attivazione, la creazione e la scadenza</span><span class="sxs-lookup"><span data-stu-id="15c92-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="15c92-111">Stato di revoca</span><span class="sxs-lookup"><span data-stu-id="15c92-111">Revocation status</span></span>

* <span data-ttu-id="15c92-112">Identificatore di chiave (GUID)</span><span class="sxs-lookup"><span data-stu-id="15c92-112">Key identifier (a GUID)</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="15c92-113">È inoltre `IKey` espone una `CreateEncryptor` metodo che può essere usato per creare un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) istanza associate a questa chiave.</span><span class="sxs-lookup"><span data-stu-id="15c92-113">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="15c92-114">È inoltre `IKey` espone una `CreateEncryptorInstance` metodo che può essere usato per creare un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) istanza associate a questa chiave.</span><span class="sxs-lookup"><span data-stu-id="15c92-114">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="15c92-115">Non è disponibile alcuna API per recuperare il materiale di crittografia non elaborato da un `IKey` istanza.</span><span class="sxs-lookup"><span data-stu-id="15c92-115">There's no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="15c92-116">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="15c92-116">IKeyManager</span></span>

<span data-ttu-id="15c92-117">Il `IKeyManager` interfaccia rappresenta un oggetto responsabile dell'archiviazione delle chiavi generale, il recupero e la modifica.</span><span class="sxs-lookup"><span data-stu-id="15c92-117">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="15c92-118">Che espone tre operazioni generali:</span><span class="sxs-lookup"><span data-stu-id="15c92-118">It exposes three high-level operations:</span></span>

* <span data-ttu-id="15c92-119">Creare una nuova chiave e renderlo persistente nell'archiviazione.</span><span class="sxs-lookup"><span data-stu-id="15c92-119">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="15c92-120">Ottiene tutte le chiavi di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="15c92-120">Get all keys from storage.</span></span>

* <span data-ttu-id="15c92-121">Revoca le chiavi di uno o più e rendere persistenti le informazioni di revoca all'archiviazione.</span><span class="sxs-lookup"><span data-stu-id="15c92-121">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="15c92-122">La scrittura di un `IKeyManager` è un'attività molto avanzata e la maggior parte degli sviluppatori non deve provare lo.</span><span class="sxs-lookup"><span data-stu-id="15c92-122">Writing an `IKeyManager` is a very advanced task, and the majority of developers shouldn't attempt it.</span></span> <span data-ttu-id="15c92-123">Al contrario, la maggior parte degli sviluppatori consigliabile sfruttare le funzionalità offerte dal [XmlKeyManager](#xmlkeymanager) classe.</span><span class="sxs-lookup"><span data-stu-id="15c92-123">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](#xmlkeymanager) class.</span></span>

## <a name="xmlkeymanager"></a><span data-ttu-id="15c92-124">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="15c92-124">XmlKeyManager</span></span>

<span data-ttu-id="15c92-125">Il `XmlKeyManager` il tipo è l'implementazione concreta in arrivo del `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="15c92-125">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="15c92-126">Fornisce diverse funzionalità utili, inclusa deposito delle chiave e la crittografia delle chiavi a riposo.</span><span class="sxs-lookup"><span data-stu-id="15c92-126">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="15c92-127">Le chiavi nel sistema sono rappresentate come elementi XML (in particolare [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span><span class="sxs-lookup"><span data-stu-id="15c92-127">Keys in this system are represented as XML elements (specifically, [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="15c92-128">`XmlKeyManager` dipende da diversi altri componenti nel corso di evadere la suddetta le relative attività:</span><span class="sxs-lookup"><span data-stu-id="15c92-128">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="15c92-129">`AlgorithmConfiguration`, che determinano gli algoritmi utilizzati dai nuovi codici.</span><span class="sxs-lookup"><span data-stu-id="15c92-129">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="15c92-130">`IXmlRepository`, che controlla in cui le chiavi vengono rese persistenti nell'archiviazione.</span><span class="sxs-lookup"><span data-stu-id="15c92-130">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="15c92-131">`IXmlEncryptor` [optional], che consente di crittografare le chiavi a riposo.</span><span class="sxs-lookup"><span data-stu-id="15c92-131">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="15c92-132">`IKeyEscrowSink` [optional], che fornisce servizi di deposito delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="15c92-132">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="15c92-133">`IXmlRepository`, che controlla in cui le chiavi vengono rese persistenti nell'archiviazione.</span><span class="sxs-lookup"><span data-stu-id="15c92-133">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="15c92-134">`IXmlEncryptor` [optional], che consente di crittografare le chiavi a riposo.</span><span class="sxs-lookup"><span data-stu-id="15c92-134">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="15c92-135">`IKeyEscrowSink` [optional], che fornisce servizi di deposito delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="15c92-135">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

<span data-ttu-id="15c92-136">Di seguito sono diagrammi di livello superiore che indicano come questi componenti vengono collegati tra loro all'interno di `XmlKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="15c92-136">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

::: moniker range=">= aspnetcore-2.0"

![Creazione della chiave](key-management/_static/keycreation2.png)

<span data-ttu-id="15c92-138">*La creazione della chiave / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="15c92-138">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="15c92-139">Nell'implementazione di `CreateNewKey`, il `AlgorithmConfiguration` componente viene usato per creare un valore univoco `IAuthenticatedEncryptorDescriptor`, che vengono quindi serializzati come XML.</span><span class="sxs-lookup"><span data-stu-id="15c92-139">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="15c92-140">Se è presente un sink di deposito delle chiavi, il XML non elaborato (non crittografato) viene fornito per il sink per l'archiviazione a lungo termine.</span><span class="sxs-lookup"><span data-stu-id="15c92-140">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="15c92-141">Il codice XML non crittografato viene quindi eseguito un `IXmlEncryptor` (se richiesto) per generare il documento XML crittografato.</span><span class="sxs-lookup"><span data-stu-id="15c92-141">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="15c92-142">Questo documento è persistente in un archivio a lungo termine tramite il `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="15c92-142">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="15c92-143">(Se no `IXmlEncryptor` è configurato, il documento non crittografato viene mantenuto nel `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="15c92-143">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![Recupero della chiave](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![Creazione della chiave](key-management/_static/keycreation1.png)

<span data-ttu-id="15c92-146">*La creazione della chiave / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="15c92-146">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="15c92-147">Nell'implementazione di `CreateNewKey`, il `IAuthenticatedEncryptorConfiguration` componente viene usato per creare un valore univoco `IAuthenticatedEncryptorDescriptor`, che vengono quindi serializzati come XML.</span><span class="sxs-lookup"><span data-stu-id="15c92-147">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="15c92-148">Se è presente un sink di deposito delle chiavi, il XML non elaborato (non crittografato) viene fornito per il sink per l'archiviazione a lungo termine.</span><span class="sxs-lookup"><span data-stu-id="15c92-148">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="15c92-149">Il codice XML non crittografato viene quindi eseguito un `IXmlEncryptor` (se richiesto) per generare il documento XML crittografato.</span><span class="sxs-lookup"><span data-stu-id="15c92-149">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="15c92-150">Questo documento è persistente in un archivio a lungo termine tramite il `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="15c92-150">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="15c92-151">(Se no `IXmlEncryptor` è configurato, il documento non crittografato viene mantenuto nel `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="15c92-151">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![Recupero della chiave](key-management/_static/keyretrieval1.png)

::: moniker-end

<span data-ttu-id="15c92-153">*Il recupero della chiave / GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="15c92-153">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="15c92-154">Nell'implementazione di `GetAllKeys`, che rappresentano le chiavi di documenti XML e revoca viene letti da sottostante `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="15c92-154">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="15c92-155">Se questi documenti sono crittografati, il sistema verrà automaticamente decrittografarle.</span><span class="sxs-lookup"><span data-stu-id="15c92-155">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="15c92-156">`XmlKeyManager` Crea l'appropriato `IAuthenticatedEncryptorDescriptorDeserializer` allo stato le istanze per deserializzare i documenti `IAuthenticatedEncryptorDescriptor` istanze, che vengono quindi eseguito il wrapping in singoli `IKey` istanze.</span><span class="sxs-lookup"><span data-stu-id="15c92-156">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="15c92-157">Questa raccolta di `IKey` istanze viene restituito al chiamante.</span><span class="sxs-lookup"><span data-stu-id="15c92-157">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="15c92-158">Altre informazioni sugli elementi XML specifici sono reperibile nel [documento di formato di archiviazione chiavi](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="15c92-158">Further information on the particular XML elements can be found in the [key storage format document](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="15c92-159">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="15c92-159">IXmlRepository</span></span>

<span data-ttu-id="15c92-160">Il `IXmlRepository` interfaccia rappresenta un tipo che può rendere persistenti dati XML da e recuperare dati XML da un archivio di backup.</span><span class="sxs-lookup"><span data-stu-id="15c92-160">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="15c92-161">Che espone due API:</span><span class="sxs-lookup"><span data-stu-id="15c92-161">It exposes two APIs:</span></span>

* <span data-ttu-id="15c92-162">`GetAllElements` :`IReadOnlyCollection<XElement>`</span><span class="sxs-lookup"><span data-stu-id="15c92-162">`GetAllElements` :`IReadOnlyCollection<XElement>`</span></span>

* `StoreElement(XElement element, string friendlyName)`

<span data-ttu-id="15c92-163">Le implementazioni di `IXmlRepository` non necessario analizzare il codice XML che li attraversano.</span><span class="sxs-lookup"><span data-stu-id="15c92-163">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="15c92-164">Devono considerare i documenti XML come opaco e consentire ai livelli superiori di preoccuparsi di generazione e l'analisi dei documenti.</span><span class="sxs-lookup"><span data-stu-id="15c92-164">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="15c92-165">Esistono quattro tipi concreti predefiniti che implementano `IXmlRepository`:</span><span class="sxs-lookup"><span data-stu-id="15c92-165">There are four built-in concrete types which implement `IXmlRepository`:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="15c92-166">FileSystemXmlRepository</span><span class="sxs-lookup"><span data-stu-id="15c92-166">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="15c92-167">RegistryXmlRepository</span><span class="sxs-lookup"><span data-stu-id="15c92-167">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="15c92-168">AzureStorage.AzureBlobXmlRepository</span><span class="sxs-lookup"><span data-stu-id="15c92-168">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="15c92-169">RedisXmlRepository</span><span class="sxs-lookup"><span data-stu-id="15c92-169">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="15c92-170">FileSystemXmlRepository</span><span class="sxs-lookup"><span data-stu-id="15c92-170">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="15c92-171">RegistryXmlRepository</span><span class="sxs-lookup"><span data-stu-id="15c92-171">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="15c92-172">AzureStorage.AzureBlobXmlRepository</span><span class="sxs-lookup"><span data-stu-id="15c92-172">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="15c92-173">RedisXmlRepository</span><span class="sxs-lookup"><span data-stu-id="15c92-173">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

<span data-ttu-id="15c92-174">Vedere le [documento provider archiviazione chiavi](xref:security/data-protection/implementation/key-storage-providers) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="15c92-174">See the [key storage providers document](xref:security/data-protection/implementation/key-storage-providers) for more information.</span></span>

<span data-ttu-id="15c92-175">La registrazione di un oggetto personalizzato `IXmlRepository` è appropriato quando si usa un archivio di backup (ad esempio, archiviazione tabelle di Azure).</span><span class="sxs-lookup"><span data-stu-id="15c92-175">Registering a custom `IXmlRepository` is appropriate when using a different backing store (for example, Azure Table Storage).</span></span>

<span data-ttu-id="15c92-176">Per modificare il repository predefinito a livello di applicazione, registrare un oggetto personalizzato `IXmlRepository` istanza:</span><span class="sxs-lookup"><span data-stu-id="15c92-176">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

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

## <a name="ixmlencryptor"></a><span data-ttu-id="15c92-177">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="15c92-177">IXmlEncryptor</span></span>

<span data-ttu-id="15c92-178">Il `IXmlEncryptor` interfaccia rappresenta un tipo che può crittografare un elemento XML di testo non crittografato.</span><span class="sxs-lookup"><span data-stu-id="15c92-178">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="15c92-179">Che espone una singola API:</span><span class="sxs-lookup"><span data-stu-id="15c92-179">It exposes a single API:</span></span>

* <span data-ttu-id="15c92-180">Encrypt(XElement plaintextElement): EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="15c92-180">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="15c92-181">Se serializzato `IAuthenticatedEncryptorDescriptor` contiene tutti gli elementi contrassegnati come "richiede la crittografia", quindi `XmlKeyManager` verranno esaminate solo gli elementi dell'applicazione configurata `IXmlEncryptor`del `Encrypt` (metodo) che verranno mantenuti l'elemento cifrato anziché quella di elemento di testo non crittografato per la `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="15c92-181">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="15c92-182">L'output del `Encrypt` metodo è un `EncryptedXmlInfo` oggetto.</span><span class="sxs-lookup"><span data-stu-id="15c92-182">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="15c92-183">Questo oggetto è un wrapper che contiene entrambi i risultanti cifrati `XElement` e il tipo che rappresenta un `IXmlDecryptor` che può essere usato per decrittografare l'elemento corrispondente.</span><span class="sxs-lookup"><span data-stu-id="15c92-183">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="15c92-184">Esistono quattro tipi concreti predefiniti che implementano `IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="15c92-184">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>

* [<span data-ttu-id="15c92-185">CertificateXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="15c92-185">CertificateXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [<span data-ttu-id="15c92-186">DpapiNGXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="15c92-186">DpapiNGXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [<span data-ttu-id="15c92-187">DpapiXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="15c92-187">DpapiXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [<span data-ttu-id="15c92-188">NullXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="15c92-188">NullXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

<span data-ttu-id="15c92-189">Vedere le [chiave di crittografia al documento rest](xref:security/data-protection/implementation/key-encryption-at-rest) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="15c92-189">See the [key encryption at rest document](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="15c92-190">Per modificare il meccanismo di key---crittografia a livello di applicazione predefinito, registrare un oggetto personalizzato `IXmlEncryptor` istanza:</span><span class="sxs-lookup"><span data-stu-id="15c92-190">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

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

## <a name="ixmldecryptor"></a><span data-ttu-id="15c92-191">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="15c92-191">IXmlDecryptor</span></span>

<span data-ttu-id="15c92-192">Il `IXmlDecryptor` interfaccia rappresenta un tipo in grado di decrittografare un' `XElement` che è stata cifrati tramite un `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="15c92-192">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="15c92-193">Che espone una singola API:</span><span class="sxs-lookup"><span data-stu-id="15c92-193">It exposes a single API:</span></span>

* <span data-ttu-id="15c92-194">Decrittografare (XElement encryptedElement): XElement</span><span class="sxs-lookup"><span data-stu-id="15c92-194">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="15c92-195">Il `Decrypt` metodo annulla la crittografia eseguita da `IXmlEncryptor.Encrypt`.</span><span class="sxs-lookup"><span data-stu-id="15c92-195">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="15c92-196">In generale, ogni concreto `IXmlEncryptor` implementazione avrà un concreto corrispondente `IXmlDecryptor` implementazione.</span><span class="sxs-lookup"><span data-stu-id="15c92-196">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="15c92-197">I tipi che implementano `IXmlDecryptor` deve avere uno dei due costruttori pubblici seguenti:</span><span class="sxs-lookup"><span data-stu-id="15c92-197">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="15c92-198">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="15c92-198">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="15c92-199">ctor)</span><span class="sxs-lookup"><span data-stu-id="15c92-199">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="15c92-200">Il `IServiceProvider` passato al costruttore può essere null.</span><span class="sxs-lookup"><span data-stu-id="15c92-200">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="15c92-201">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="15c92-201">IKeyEscrowSink</span></span>

<span data-ttu-id="15c92-202">Il `IKeyEscrowSink` interfaccia rappresenta un tipo che può eseguire il deposito di informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="15c92-202">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="15c92-203">È importante ricordare che i descrittori serializzati potrebbero contenere informazioni riservate (ad esempio, il materiale di crittografia) e questo è ciò che ha portato all'introduzione del [IXmlEncryptor](#ixmlencryptor) digitare in primo luogo.</span><span class="sxs-lookup"><span data-stu-id="15c92-203">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](#ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="15c92-204">Tuttavia, succedere e chiave di anelli può essere eliminati o danneggiati.</span><span class="sxs-lookup"><span data-stu-id="15c92-204">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="15c92-205">L'interfaccia di deposito delle chiavi fornisce un'emergenza di emergenza, che consente l'accesso al codice XML serializzato non elaborati prima che venga trasformata da qualsiasi configurata [IXmlEncryptor](#ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="15c92-205">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it's transformed by any configured [IXmlEncryptor](#ixmlencryptor).</span></span> <span data-ttu-id="15c92-206">L'interfaccia espone una singola API:</span><span class="sxs-lookup"><span data-stu-id="15c92-206">The interface exposes a single API:</span></span>

* <span data-ttu-id="15c92-207">Store (keyId Guid, elemento XElement)</span><span class="sxs-lookup"><span data-stu-id="15c92-207">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="15c92-208">Fino a è il `IKeyEscrowSink` implementazione per gestire l'elemento specificato in modo sicuro coerente con i criteri di business.</span><span class="sxs-lookup"><span data-stu-id="15c92-208">It's up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="15c92-209">Una possibile implementazione potrebbe essere usato per il sink di deposito delle chiavi crittografare l'elemento XML usando un certificato X.509 azienda noto in cui la chiave privata del certificato è stata depositata; il `CertificateXmlEncryptor` tipo può essere usato per questo.</span><span class="sxs-lookup"><span data-stu-id="15c92-209">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="15c92-210">Il `IKeyEscrowSink` implementazione è anche responsabile della persistenza l'elemento specificato in modo appropriato.</span><span class="sxs-lookup"><span data-stu-id="15c92-210">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="15c92-211">Per impostazione predefinita non è abilitato alcun meccanismo di deposito delle chiavi, anche se gli amministratori server possono [questa configurazione a livello globale](xref:security/data-protection/configuration/machine-wide-policy).</span><span class="sxs-lookup"><span data-stu-id="15c92-211">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="15c92-212">Può anche essere configurato a livello di programmazione tramite le `IDataProtectionBuilder.AddKeyEscrowSink` metodo come illustrato nell'esempio riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="15c92-212">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="15c92-213">Il `AddKeyEscrowSink` mirror gli overload di metodo di `IServiceCollection.AddSingleton` e `IServiceCollection.AddInstance` overload, come `IKeyEscrowSink` istanze sono progettate per essere singleton con stato.</span><span class="sxs-lookup"><span data-stu-id="15c92-213">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="15c92-214">Se più `IKeyEscrowSink` registrate le istanze, ciascuna di esse verrà chiamato durante la generazione di chiavi, in modo che le chiavi possono potrà essere depositate ai meccanismi più contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="15c92-214">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="15c92-215">Non è disponibile alcuna API per leggere i materiali da un `IKeyEscrowSink` istanza.</span><span class="sxs-lookup"><span data-stu-id="15c92-215">There's no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="15c92-216">Questo comportamento è coerente con la teoria della progettazione del meccanismo di deposito delle chiavi: è destinato a rendere accessibili a un'autorità attendibile il materiale della chiave, e poiché l'applicazione non costituisce un'autorità attendibile, non deve avere accesso al proprio materiale garantite.</span><span class="sxs-lookup"><span data-stu-id="15c92-216">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="15c92-217">Esempio di codice seguente illustra la creazione e la registrazione di un `IKeyEscrowSink` in cui le chiavi sono depositate in modo che solo i membri di "CONTOSODomain Admins" possono ripristinare tali componenti.</span><span class="sxs-lookup"><span data-stu-id="15c92-217">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="15c92-218">Per eseguire questo esempio, è necessario essere in un dominio Windows 8 / computer con Windows Server 2012 e il controller di dominio deve essere Windows Server 2012 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="15c92-218">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
