---
title: "Estensibilità di gestione delle chiavi"
author: rick-anderson
description: Questo documento descrive extensibility gestione delle chiavi di protezione dei dati di ASP.NET Core.
keywords: ASP.NET Core, protezione dei dati, gestione delle chiavi
ms.author: riande
manager: wpickett
ms.date: 11/22/2017
ms.topic: article
ms.assetid: 3606b251-8324-4485-8d52-582a2cd5cffb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 0702e13163c0208e9d2863e711b02ffb257f6260
ms.sourcegitcommit: e641c5794525f983485621860926d8ab4e7360c8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/23/2017
---
# <a name="key-management-extensibility"></a><span data-ttu-id="11719-104">Estensibilità di gestione delle chiavi</span><span class="sxs-lookup"><span data-stu-id="11719-104">Key management extensibility</span></span>

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> <span data-ttu-id="11719-105">Lettura di [gestione delle chiavi](../implementation/key-management.md#data-protection-implementation-key-management) sezione prima di leggere questa sezione, come vengono illustrati alcuni dei concetti di queste API.</span><span class="sxs-lookup"><span data-stu-id="11719-105">Read the [key management](../implementation/key-management.md#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

>[!WARNING]
> <span data-ttu-id="11719-106">Tipi che implementano le interfacce seguenti devono essere thread-safe per più i chiamanti.</span><span class="sxs-lookup"><span data-stu-id="11719-106">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="11719-107">Chiave</span><span class="sxs-lookup"><span data-stu-id="11719-107">Key</span></span>

<span data-ttu-id="11719-108">Il `IKey` interfaccia è la rappresentazione di base di una chiave nel sistema di crittografia.</span><span class="sxs-lookup"><span data-stu-id="11719-108">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="11719-109">La chiave di termine viene usata in senso astratta, non in senso letterale "crittografia materiale della chiave".</span><span class="sxs-lookup"><span data-stu-id="11719-109">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="11719-110">Una chiave ha le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="11719-110">A key has the following properties:</span></span>

* <span data-ttu-id="11719-111">Date di scadenza, la creazione e attivazione</span><span class="sxs-lookup"><span data-stu-id="11719-111">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="11719-112">Stato di revoca</span><span class="sxs-lookup"><span data-stu-id="11719-112">Revocation status</span></span>

* <span data-ttu-id="11719-113">Identificatore di chiave (GUID)</span><span class="sxs-lookup"><span data-stu-id="11719-113">Key identifier (a GUID)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="11719-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="11719-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="11719-115">Inoltre, `IKey` espone un `CreateEncryptor` metodo che può essere utilizzato per creare un [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) istanza associato a questa chiave.</span><span class="sxs-lookup"><span data-stu-id="11719-115">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="11719-116">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="11719-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="11719-117">Inoltre, `IKey` espone un `CreateEncryptorInstance` metodo che può essere utilizzato per creare un [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) istanza associato a questa chiave.</span><span class="sxs-lookup"><span data-stu-id="11719-117">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

---

> [!NOTE]
> <span data-ttu-id="11719-118">Non è disponibile alcuna API per recuperare il materiale crittografico non elaborato da un `IKey` istanza.</span><span class="sxs-lookup"><span data-stu-id="11719-118">There is no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="11719-119">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="11719-119">IKeyManager</span></span>

<span data-ttu-id="11719-120">Il `IKeyManager` interfaccia rappresenta un oggetto responsabile dell'archiviazione delle chiavi generale, il recupero e la modifica.</span><span class="sxs-lookup"><span data-stu-id="11719-120">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="11719-121">Espone tre operazioni di alto livello:</span><span class="sxs-lookup"><span data-stu-id="11719-121">It exposes three high-level operations:</span></span>

* <span data-ttu-id="11719-122">Creare una nuova chiave e archiviarlo in un archivio.</span><span class="sxs-lookup"><span data-stu-id="11719-122">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="11719-123">Ottenere tutte le chiavi dall'archiviazione.</span><span class="sxs-lookup"><span data-stu-id="11719-123">Get all keys from storage.</span></span>

* <span data-ttu-id="11719-124">Revocare una o più chiavi e rendere persistenti le informazioni di revoca all'archiviazione.</span><span class="sxs-lookup"><span data-stu-id="11719-124">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="11719-125">Scrittura di un `IKeyManager` è un'attività molto avanzata e la maggior parte degli sviluppatori non deve tentare.</span><span class="sxs-lookup"><span data-stu-id="11719-125">Writing an `IKeyManager` is a very advanced task, and the majority of developers should not attempt it.</span></span> <span data-ttu-id="11719-126">Al contrario, la maggior parte degli sviluppatori devono sfruttare le funzionalità offerte dal [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) classe.</span><span class="sxs-lookup"><span data-stu-id="11719-126">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) class.</span></span>

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a><span data-ttu-id="11719-127">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="11719-127">XmlKeyManager</span></span>

<span data-ttu-id="11719-128">Il `XmlKeyManager` tipo è un'implementazione concreta nella casella della `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="11719-128">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="11719-129">Fornisce diverse funzionalità utili, inclusi deposito delle chiave e la crittografia delle chiavi inattivi.</span><span class="sxs-lookup"><span data-stu-id="11719-129">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="11719-130">Le chiavi nel sistema sono rappresentate come elementi XML (in particolare, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span><span class="sxs-lookup"><span data-stu-id="11719-130">Keys in this system are represented as XML elements (specifically, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="11719-131">`XmlKeyManager`dipende da numerosi altri componenti nel corso di completare le attività:</span><span class="sxs-lookup"><span data-stu-id="11719-131">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="11719-132">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="11719-132">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="11719-133">`AlgorithmConfiguration`, che indicano gli algoritmi utilizzati dai nuove chiavi.</span><span class="sxs-lookup"><span data-stu-id="11719-133">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="11719-134">`IXmlRepository`, che controlla in cui le chiavi vengono rese persistenti nell'archiviazione.</span><span class="sxs-lookup"><span data-stu-id="11719-134">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="11719-135">`IXmlEncryptor`[facoltativo], che consente di crittografare le chiavi inattivi.</span><span class="sxs-lookup"><span data-stu-id="11719-135">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="11719-136">`IKeyEscrowSink`[facoltativo], che fornisce servizi di deposito delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="11719-136">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="11719-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="11719-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="11719-138">`IXmlRepository`, che controlla in cui le chiavi vengono rese persistenti nell'archiviazione.</span><span class="sxs-lookup"><span data-stu-id="11719-138">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="11719-139">`IXmlEncryptor`[facoltativo], che consente di crittografare le chiavi inattivi.</span><span class="sxs-lookup"><span data-stu-id="11719-139">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="11719-140">`IKeyEscrowSink`[facoltativo], che fornisce servizi di deposito delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="11719-140">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

---

<span data-ttu-id="11719-141">Di seguito sono diagrammi di livello superiore che indicano come questi componenti vengono collegati tra loro in `XmlKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="11719-141">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="11719-142">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="11719-142">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Creazione della chiave](key-management/_static/keycreation2.png)

   <span data-ttu-id="11719-144">*Chiave creazione / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="11719-144">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="11719-145">Nell'implementazione di `CreateNewKey`, `AlgorithmConfiguration` componente viene utilizzato per creare un nome `IAuthenticatedEncryptorDescriptor`, che viene quindi serializzato come XML.</span><span class="sxs-lookup"><span data-stu-id="11719-145">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="11719-146">Se un sink di deposito delle chiavi è presente, il XML non elaborato (non crittografato) viene fornita al sink per l'archiviazione a lungo termine.</span><span class="sxs-lookup"><span data-stu-id="11719-146">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="11719-147">Il codice XML non crittografato viene quindi eseguito tramite un `IXmlEncryptor` (se richiesto) per generare il documento XML crittografato.</span><span class="sxs-lookup"><span data-stu-id="11719-147">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="11719-148">Questo documento crittografato viene reso persistente per archiviazione a lungo termine tramite il `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="11719-148">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="11719-149">(Se non `IXmlEncryptor` è configurato, il documento non crittografato è persistente nel `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="11719-149">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="11719-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="11719-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Creazione della chiave](key-management/_static/keycreation1.png)

   <span data-ttu-id="11719-152">*Chiave creazione / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="11719-152">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="11719-153">Nell'implementazione di `CreateNewKey`, `IAuthenticatedEncryptorConfiguration` componente viene utilizzato per creare un nome `IAuthenticatedEncryptorDescriptor`, che viene quindi serializzato come XML.</span><span class="sxs-lookup"><span data-stu-id="11719-153">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="11719-154">Se un sink di deposito delle chiavi è presente, il XML non elaborato (non crittografato) viene fornita al sink per l'archiviazione a lungo termine.</span><span class="sxs-lookup"><span data-stu-id="11719-154">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="11719-155">Il codice XML non crittografato viene quindi eseguito tramite un `IXmlEncryptor` (se richiesto) per generare il documento XML crittografato.</span><span class="sxs-lookup"><span data-stu-id="11719-155">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="11719-156">Questo documento crittografato viene reso persistente per archiviazione a lungo termine tramite il `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="11719-156">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="11719-157">(Se non `IXmlEncryptor` è configurato, il documento non crittografato è persistente nel `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="11719-157">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="11719-158">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="11719-158">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Recupero della chiave](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="11719-160">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="11719-160">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Recupero della chiave](key-management/_static/keyretrieval1.png)

---

   <span data-ttu-id="11719-162">*Chiave di recupero / GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="11719-162">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="11719-163">Nell'implementazione di `GetAllKeys`, le chiavi che rappresenta i documenti XML e revoca è leggervi sottostante `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="11719-163">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="11719-164">Se questi documenti sono crittografati, il sistema verrà automaticamente decrittografarli.</span><span class="sxs-lookup"><span data-stu-id="11719-164">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="11719-165">`XmlKeyManager`Crea l'oggetto appropriato `IAuthenticatedEncryptorDescriptorDeserializer` nuovamente le istanze per deserializzare i documenti in `IAuthenticatedEncryptorDescriptor` istanze, che vengono quindi eseguito il wrapping in singoli `IKey` istanze.</span><span class="sxs-lookup"><span data-stu-id="11719-165">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="11719-166">Questa raccolta di `IKey` istanze viene restituito al chiamante.</span><span class="sxs-lookup"><span data-stu-id="11719-166">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="11719-167">Per altre informazioni sugli elementi XML specifici, vedere il [documento di formato di archiviazione chiavi](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="11719-167">Further information on the particular XML elements can be found in the [key storage format document](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="11719-168">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="11719-168">IXmlRepository</span></span>

<span data-ttu-id="11719-169">Il `IXmlRepository` interfaccia rappresenta un tipo che può rendere persistenti dati XML per recuperare i dati XML da un archivio di backup.</span><span class="sxs-lookup"><span data-stu-id="11719-169">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="11719-170">Espone due API:</span><span class="sxs-lookup"><span data-stu-id="11719-170">It exposes two APIs:</span></span>

* <span data-ttu-id="11719-171">GetAllElements(): IReadOnlyCollection<XElement></span><span class="sxs-lookup"><span data-stu-id="11719-171">GetAllElements() : IReadOnlyCollection<XElement></span></span>

* <span data-ttu-id="11719-172">StoreElement (elemento XElement, friendlyName stringa)</span><span class="sxs-lookup"><span data-stu-id="11719-172">StoreElement(XElement element, string friendlyName)</span></span>

<span data-ttu-id="11719-173">Le implementazioni di `IXmlRepository` non è necessario analizzare il codice XML che li attraversano.</span><span class="sxs-lookup"><span data-stu-id="11719-173">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="11719-174">Devono considerare i documenti XML come opaco e consentire a livelli più elevati preoccuparsi di generazione e l'analisi dei documenti.</span><span class="sxs-lookup"><span data-stu-id="11719-174">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="11719-175">Esistono due tipi concreti predefiniti che implementano `IXmlRepository`: `FileSystemXmlRepository` e `RegistryXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="11719-175">There are two built-in concrete types which implement `IXmlRepository`: `FileSystemXmlRepository` and `RegistryXmlRepository`.</span></span> <span data-ttu-id="11719-176">Vedere il [documento i provider di archiviazione chiavi](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="11719-176">See the [key storage providers document](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) for more information.</span></span> <span data-ttu-id="11719-177">Registrazione di un oggetto personalizzato `IXmlRepository` sarebbe il modo appropriato da utilizzare un archivio di backup diversi, ad esempio, archiviazione Blob di Azure.</span><span class="sxs-lookup"><span data-stu-id="11719-177">Registering a custom `IXmlRepository` would be the appropriate manner to use a different backing store, e.g., Azure Blob Storage.</span></span>

<span data-ttu-id="11719-178">Per modificare il repository predefinito a livello di applicazione, registrare un oggetto personalizzato `IXmlRepository` istanza:</span><span class="sxs-lookup"><span data-stu-id="11719-178">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="11719-179">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="11719-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="11719-180">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="11719-180">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a><span data-ttu-id="11719-181">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="11719-181">IXmlEncryptor</span></span>

<span data-ttu-id="11719-182">Il `IXmlEncryptor` interfaccia rappresenta un tipo che è possibile crittografare un elemento XML di testo normale.</span><span class="sxs-lookup"><span data-stu-id="11719-182">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="11719-183">Espone una singola API:</span><span class="sxs-lookup"><span data-stu-id="11719-183">It exposes a single API:</span></span>

* <span data-ttu-id="11719-184">Crittografare (plaintextElement XElement): EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="11719-184">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="11719-185">Se serializzata `IAuthenticatedEncryptorDescriptor` conterrà tutti gli elementi contrassegnati come "richiede la crittografia", quindi `XmlKeyManager` eseguirà tali elementi tramite l'applicazione configurata `IXmlEncryptor`del `Encrypt` metodo e manterrà l'elemento crittografato invece di elemento di testo normale per il `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="11719-185">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="11719-186">L'output del `Encrypt` metodo è un `EncryptedXmlInfo` oggetto.</span><span class="sxs-lookup"><span data-stu-id="11719-186">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="11719-187">Questo oggetto è un wrapper che contiene entrambi i risultanti crittografato `XElement` e il tipo che rappresenta un `IXmlDecryptor` che può essere usato per decrittografare l'elemento corrispondente.</span><span class="sxs-lookup"><span data-stu-id="11719-187">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="11719-188">Sono disponibili quattro tipi concreti predefiniti che implementano `IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="11719-188">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

<span data-ttu-id="11719-189">Vedere il [crittografia chiave documento rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="11719-189">See the [key encryption at rest document](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="11719-190">Per modificare il meccanismo di crittografia chiave al resto predefinito a livello di applicazione, registrare un oggetto personalizzato `IXmlEncryptor` istanza:</span><span class="sxs-lookup"><span data-stu-id="11719-190">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="11719-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="11719-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="11719-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="11719-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

## <a name="ixmldecryptor"></a><span data-ttu-id="11719-193">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="11719-193">IXmlDecryptor</span></span>

<span data-ttu-id="11719-194">Il `IXmlDecryptor` interfaccia rappresenta un tipo in grado di decrittografare un `XElement` che è stato crittografato mediante un `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="11719-194">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="11719-195">Espone una singola API:</span><span class="sxs-lookup"><span data-stu-id="11719-195">It exposes a single API:</span></span>

* <span data-ttu-id="11719-196">Decrittografare (encryptedElement XElement): XElement</span><span class="sxs-lookup"><span data-stu-id="11719-196">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="11719-197">Il `Decrypt` metodo annulla la crittografia eseguita da `IXmlEncryptor.Encrypt`.</span><span class="sxs-lookup"><span data-stu-id="11719-197">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="11719-198">In genere, ogni concreto `IXmlEncryptor` implementazione avrà una corrispondente concreta `IXmlDecryptor` implementazione.</span><span class="sxs-lookup"><span data-stu-id="11719-198">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="11719-199">I tipi che implementano `IXmlDecryptor` deve avere uno dei due costruttori pubblici seguenti:</span><span class="sxs-lookup"><span data-stu-id="11719-199">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="11719-200">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="11719-200">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="11719-201">. ctor)</span><span class="sxs-lookup"><span data-stu-id="11719-201">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="11719-202">Il `IServiceProvider` passato al costruttore può essere null.</span><span class="sxs-lookup"><span data-stu-id="11719-202">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="11719-203">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="11719-203">IKeyEscrowSink</span></span>

<span data-ttu-id="11719-204">Il `IKeyEscrowSink` interfaccia rappresenta un tipo che può eseguire deposito delle informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="11719-204">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="11719-205">Tieni presente che i descrittori serializzati potrebbero contenere informazioni riservate (ad esempio, il materiale di crittografia) e questo è ciò che ha portato all'introduzione del [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) digitare in primo luogo.</span><span class="sxs-lookup"><span data-stu-id="11719-205">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="11719-206">Tuttavia, che degli incidenti verificarsi e anelli chiave può essere eliminati o danneggiati.</span><span class="sxs-lookup"><span data-stu-id="11719-206">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="11719-207">L'interfaccia deposito fornisce un'emergenza stessa, che consente l'accesso al codice XML serializzato non elaborati prima che venga trasformata da qualsiasi configurato [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="11719-207">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it is transformed by any configured [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span></span> <span data-ttu-id="11719-208">L'interfaccia espone una singola API:</span><span class="sxs-lookup"><span data-stu-id="11719-208">The interface exposes a single API:</span></span>

* <span data-ttu-id="11719-209">Archivio (keyId Guid, elemento XElement)</span><span class="sxs-lookup"><span data-stu-id="11719-209">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="11719-210">È il `IKeyEscrowSink` implementazione per gestire l'elemento specificato in modo sicuro coerenza con i criteri di business.</span><span class="sxs-lookup"><span data-stu-id="11719-210">It is up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="11719-211">Una possibile implementazione potrebbe essere usato per il sink deposito crittografare l'elemento XML usando un certificato x. 509 azienda noto in cui la chiave privata del certificato è stata depositata; il `CertificateXmlEncryptor` possono essere utili in questo tipo.</span><span class="sxs-lookup"><span data-stu-id="11719-211">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="11719-212">Il `IKeyEscrowSink` implementazione è inoltre responsabile per la persistenza in modo appropriato l'elemento specificato.</span><span class="sxs-lookup"><span data-stu-id="11719-212">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="11719-213">Per impostazione predefinita non è abilitato alcun meccanismo deposito, anche se gli amministratori di server possono [questa configurazione globale](xref:security/data-protection/configuration/machine-wide-policy).</span><span class="sxs-lookup"><span data-stu-id="11719-213">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="11719-214">Può anche essere configurato a livello di programmazione tramite le `IDataProtectionBuilder.AddKeyEscrowSink` metodo come illustrato nell'esempio riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="11719-214">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="11719-215">Il `AddKeyEscrowSink` mirror overload di metodo di `IServiceCollection.AddSingleton` e `IServiceCollection.AddInstance` overload, come `IKeyEscrowSink` le istanze devono essere singleton.</span><span class="sxs-lookup"><span data-stu-id="11719-215">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="11719-216">Se più `IKeyEscrowSink` registrate le istanze, verrà chiamato durante la generazione di chiavi, ognuna in modo possano depositare le chiavi per diversi meccanismi contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="11719-216">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="11719-217">Non è disponibile alcuna API per leggere materiale proveniente da un `IKeyEscrowSink` istanza.</span><span class="sxs-lookup"><span data-stu-id="11719-217">There is no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="11719-218">Questo comportamento è coerente con la teoria della progettazione del meccanismo del deposito: è progettato per rendere accessibili a un'autorità attendibile il materiale della chiave e poiché l'applicazione non costituisce un'autorità attendibile, non deve avere accesso al proprio materiale garantite.</span><span class="sxs-lookup"><span data-stu-id="11719-218">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="11719-219">Esempio di codice seguente illustra la creazione e la registrazione di un `IKeyEscrowSink` in cui sono depositare le chiavi in modo che solo i membri di "CONTOSODomain Admins" è possono ripristinare tali componenti.</span><span class="sxs-lookup"><span data-stu-id="11719-219">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="11719-220">Per eseguire questo esempio, è necessario essere in un dominio di Windows 8 / computer con Windows Server 2012 e il controller di dominio deve essere Windows Server 2012 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="11719-220">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[Main](key-management/samples/key-management-extensibility.cs)]
