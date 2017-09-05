---
title: "Estensibilità di gestione delle chiavi"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 3606b251-8324-4485-8d52-582a2cd5cffb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: fb74905660015b9a83503e1f74b25c66ae9df9e3
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/25/2017
---
# <a name="key-management-extensibility"></a><span data-ttu-id="ab5a8-103">Estensibilità di gestione delle chiavi</span><span class="sxs-lookup"><span data-stu-id="ab5a8-103">Key management extensibility</span></span>

<a name=data-protection-extensibility-key-management></a>

>[!TIP]
> <span data-ttu-id="ab5a8-104">Lettura di [gestione delle chiavi](../implementation/key-management.md#data-protection-implementation-key-management) sezione prima di leggere questa sezione, come vengono illustrati alcuni dei concetti di queste API.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-104">Read the [key management](../implementation/key-management.md#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

>[!WARNING]
> <span data-ttu-id="ab5a8-105">Tipi che implementano le interfacce seguenti devono essere thread-safe per più i chiamanti.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="ab5a8-106">Chiave</span><span class="sxs-lookup"><span data-stu-id="ab5a8-106">Key</span></span>

<span data-ttu-id="ab5a8-107">L'interfaccia IKey è la rappresentazione di base di una chiave nel sistema di crittografia.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-107">The IKey interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="ab5a8-108">La chiave di termine viene usata in senso astratta, non in senso letterale "crittografia materiale della chiave".</span><span class="sxs-lookup"><span data-stu-id="ab5a8-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="ab5a8-109">Una chiave ha le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="ab5a8-109">A key has the following properties:</span></span>

* <span data-ttu-id="ab5a8-110">Date di scadenza, la creazione e attivazione</span><span class="sxs-lookup"><span data-stu-id="ab5a8-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="ab5a8-111">Stato di revoca</span><span class="sxs-lookup"><span data-stu-id="ab5a8-111">Revocation status</span></span>

* <span data-ttu-id="ab5a8-112">Identificatore di chiave (GUID)</span><span class="sxs-lookup"><span data-stu-id="ab5a8-112">Key identifier (a GUID)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ab5a8-113">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="ab5a8-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ab5a8-114">Inoltre, IKey espone un metodo CreateDecryptor che può essere utilizzato per creare un [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) istanza associato a questa chiave.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-114">Additionally, IKey exposes a CreateEncryptor method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ab5a8-115">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="ab5a8-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ab5a8-116">Inoltre, IKey espone un metodo CreateEncryptorInstance che può essere utilizzato per creare un [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) istanza associato a questa chiave.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-116">Additionally, IKey exposes a CreateEncryptorInstance method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

---

> [!NOTE]
> <span data-ttu-id="ab5a8-117">Non è disponibile alcuna API per recuperare il materiale crittografico non elaborato da un'istanza di IKey.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-117">There is no API to retrieve the raw cryptographic material from an IKey instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="ab5a8-118">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="ab5a8-118">IKeyManager</span></span>

<span data-ttu-id="ab5a8-119">L'interfaccia IKeyManager rappresenta un oggetto responsabile della chiavi generali di archiviazione, il recupero e modifica.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-119">The IKeyManager interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="ab5a8-120">Espone tre operazioni di alto livello:</span><span class="sxs-lookup"><span data-stu-id="ab5a8-120">It exposes three high-level operations:</span></span>

* <span data-ttu-id="ab5a8-121">Creare una nuova chiave e archiviarlo in un archivio.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-121">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="ab5a8-122">Ottenere tutte le chiavi dall'archiviazione.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-122">Get all keys from storage.</span></span>

* <span data-ttu-id="ab5a8-123">Revocare una o più chiavi e rendere persistenti le informazioni di revoca all'archiviazione.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-123">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="ab5a8-124">Scrittura di un IKeyManager è un'attività molto avanzata e la maggior parte degli sviluppatori non deve tentare.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-124">Writing an IKeyManager is a very advanced task, and the majority of developers should not attempt it.</span></span> <span data-ttu-id="ab5a8-125">Al contrario, la maggior parte degli sviluppatori devono sfruttare le funzionalità offerte dal [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) classe.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-125">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) class.</span></span>

<a name=data-protection-extensibility-key-management-xmlkeymanager></a>

## <a name="xmlkeymanager"></a><span data-ttu-id="ab5a8-126">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="ab5a8-126">XmlKeyManager</span></span>

<span data-ttu-id="ab5a8-127">Il tipo XmlKeyManager è l'implementazione concreta nella casella di IKeyManager.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-127">The XmlKeyManager type is the in-box concrete implementation of IKeyManager.</span></span> <span data-ttu-id="ab5a8-128">Fornisce diverse funzionalità utili, inclusi deposito delle chiave e la crittografia delle chiavi inattivi.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-128">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="ab5a8-129">Le chiavi nel sistema sono rappresentate come elementi XML (in particolare, [XElement](https://msdn.microsoft.com/library/system.xml.linq.xelement(v=vs.110).aspx)).</span><span class="sxs-lookup"><span data-stu-id="ab5a8-129">Keys in this system are represented as XML elements (specifically, [XElement](https://msdn.microsoft.com/library/system.xml.linq.xelement(v=vs.110).aspx)).</span></span>

<span data-ttu-id="ab5a8-130">XmlKeyManager dipende da numerosi altri componenti nel corso di completare le attività:</span><span class="sxs-lookup"><span data-stu-id="ab5a8-130">XmlKeyManager depends on several other components in the course of fulfilling its tasks:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ab5a8-131">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="ab5a8-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="ab5a8-132">AlgorithmConfiguration, che determina gli algoritmi utilizzati dai nuove chiavi.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-132">AlgorithmConfiguration, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="ab5a8-133">IXmlRepository, che controlla in cui le chiavi vengono rese persistenti nell'archiviazione.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-133">IXmlRepository, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="ab5a8-134">[Facoltativo] IXmlEncryptor che consente di crittografare le chiavi inattivi.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-134">IXmlEncryptor [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="ab5a8-135">[Facoltativo] IKeyEscrowSink che fornisce servizi di deposito delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-135">IKeyEscrowSink [optional], which provides key escrow services.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ab5a8-136">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="ab5a8-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="ab5a8-137">IXmlRepository, che controlla in cui le chiavi vengono rese persistenti nell'archiviazione.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-137">IXmlRepository, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="ab5a8-138">[Facoltativo] IXmlEncryptor che consente di crittografare le chiavi inattivi.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-138">IXmlEncryptor [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="ab5a8-139">[Facoltativo] IKeyEscrowSink che fornisce servizi di deposito delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-139">IKeyEscrowSink [optional], which provides key escrow services.</span></span>

---

<span data-ttu-id="ab5a8-140">Di seguito sono diagrammi di livello superiore che indicano come questi componenti vengono collegati tra loro in XmlKeyManager.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-140">Below are high-level diagrams which indicate how these components are wired together within XmlKeyManager.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ab5a8-141">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="ab5a8-141">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Creazione della chiave](key-management/_static/keycreation2.png)

   <span data-ttu-id="ab5a8-143">*Chiave creazione / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="ab5a8-143">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="ab5a8-144">Nell'implementazione di CreateNewKey, il componente AlgorithmConfiguration viene utilizzato per creare un IAuthenticatedEncryptorDescriptor univoco, che viene quindi serializzato come XML.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-144">In the implementation of CreateNewKey, the AlgorithmConfiguration component is used to create a unique IAuthenticatedEncryptorDescriptor, which is then serialized as XML.</span></span> <span data-ttu-id="ab5a8-145">Se un sink di deposito delle chiavi è presente, il XML non elaborato (non crittografato) viene fornita al sink per l'archiviazione a lungo termine.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-145">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="ab5a8-146">Il codice XML non crittografato viene eseguito tramite un IXmlEncryptor (se richiesto) per generare il documento XML crittografato.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-146">The unencrypted XML is then run through an IXmlEncryptor (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="ab5a8-147">Questo documento crittografato è persistente nell'archiviazione a lungo termine tramite il IXmlRepository.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-147">This encrypted document is persisted to long-term storage via the IXmlRepository.</span></span> <span data-ttu-id="ab5a8-148">(Se non è configurato alcun IXmlEncryptor, il documento non crittografato è persistente nel IXmlRepository.)</span><span class="sxs-lookup"><span data-stu-id="ab5a8-148">(If no IXmlEncryptor is configured, the unencrypted document is persisted in the IXmlRepository.)</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ab5a8-149">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="ab5a8-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Creazione della chiave](key-management/_static/keycreation1.png)

   <span data-ttu-id="ab5a8-151">*Chiave creazione / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="ab5a8-151">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="ab5a8-152">Nell'implementazione di CreateNewKey, il componente IAuthenticatedEncryptorConfiguration viene utilizzato per creare un IAuthenticatedEncryptorDescriptor univoco, che viene quindi serializzato come XML.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-152">In the implementation of CreateNewKey, the IAuthenticatedEncryptorConfiguration component is used to create a unique IAuthenticatedEncryptorDescriptor, which is then serialized as XML.</span></span> <span data-ttu-id="ab5a8-153">Se un sink di deposito delle chiavi è presente, il XML non elaborato (non crittografato) viene fornita al sink per l'archiviazione a lungo termine.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-153">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="ab5a8-154">Il codice XML non crittografato viene eseguito tramite un IXmlEncryptor (se richiesto) per generare il documento XML crittografato.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-154">The unencrypted XML is then run through an IXmlEncryptor (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="ab5a8-155">Questo documento crittografato è persistente nell'archiviazione a lungo termine tramite il IXmlRepository.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-155">This encrypted document is persisted to long-term storage via the IXmlRepository.</span></span> <span data-ttu-id="ab5a8-156">(Se non è configurato alcun IXmlEncryptor, il documento non crittografato è persistente nel IXmlRepository.)</span><span class="sxs-lookup"><span data-stu-id="ab5a8-156">(If no IXmlEncryptor is configured, the unencrypted document is persisted in the IXmlRepository.)</span></span>

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ab5a8-157">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="ab5a8-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Recupero della chiave](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ab5a8-159">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="ab5a8-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Recupero della chiave](key-management/_static/keyretrieval1.png)

---

   <span data-ttu-id="ab5a8-161">*Chiave di recupero / GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="ab5a8-161">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="ab5a8-162">Nell'implementazione di GetAllKeys, i documenti XML che rappresenta le chiavi e le decisioni vengono letti dal IXmlRepository sottostante.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-162">In the implementation of GetAllKeys, the XML documents representing keys and revocations are read from the underlying IXmlRepository.</span></span> <span data-ttu-id="ab5a8-163">Se questi documenti sono crittografati, il sistema verrà automaticamente decrittografarli.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-163">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="ab5a8-164">XmlKeyManager crea le istanze di IAuthenticatedEncryptorDescriptorDeserializer appropriate per deserializzare i documenti in istanze IAuthenticatedEncryptorDescriptor, che vengono quindi eseguito il wrapping in singole istanze IKey.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-164">XmlKeyManager creates the appropriate IAuthenticatedEncryptorDescriptorDeserializer instances to deserialize the documents back into IAuthenticatedEncryptorDescriptor instances, which are then wrapped in individual IKey instances.</span></span> <span data-ttu-id="ab5a8-165">Questa raccolta di istanze IKey viene restituita al chiamante.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-165">This collection of IKey instances is returned to the caller.</span></span>

<span data-ttu-id="ab5a8-166">Per altre informazioni sugli elementi XML specifici, vedere il [documento di formato di archiviazione chiavi](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="ab5a8-166">Further information on the particular XML elements can be found in the [key storage format document](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="ab5a8-167">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="ab5a8-167">IXmlRepository</span></span>

<span data-ttu-id="ab5a8-168">L'interfaccia IXmlRepository rappresenta un tipo che può rendere persistenti dati XML per recuperare i dati XML da un archivio di backup.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-168">The IXmlRepository interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="ab5a8-169">Espone due API:</span><span class="sxs-lookup"><span data-stu-id="ab5a8-169">It exposes two APIs:</span></span>

* <span data-ttu-id="ab5a8-170">GetAllElements(): IReadOnlyCollection<XElement></span><span class="sxs-lookup"><span data-stu-id="ab5a8-170">GetAllElements() : IReadOnlyCollection<XElement></span></span>

* <span data-ttu-id="ab5a8-171">StoreElement (elemento XElement, friendlyName stringa)</span><span class="sxs-lookup"><span data-stu-id="ab5a8-171">StoreElement(XElement element, string friendlyName)</span></span>

<span data-ttu-id="ab5a8-172">Le implementazioni di IXmlRepository necessario analizzare il codice XML che li attraversano.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-172">Implementations of IXmlRepository don't need to parse the XML passing through them.</span></span> <span data-ttu-id="ab5a8-173">Devono considerare i documenti XML come opaco e consentire a livelli più elevati preoccuparsi di generazione e l'analisi dei documenti.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-173">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="ab5a8-174">Esistono due tipi concreti predefiniti che implementano IXmlRepository: FileSystemXmlRepository e RegistryXmlRepository.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-174">There are two built-in concrete types which implement IXmlRepository: FileSystemXmlRepository and RegistryXmlRepository.</span></span> <span data-ttu-id="ab5a8-175">Vedere il [documento i provider di archiviazione chiavi](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-175">See the [key storage providers document](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) for more information.</span></span> <span data-ttu-id="ab5a8-176">Registrazione un IXmlRepository personalizzato potrebbe essere il modo appropriato da utilizzare un altro archivio di backup, ad esempio, archiviazione Blob di Azure.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-176">Registering a custom IXmlRepository would be the appropriate manner to use a different backing store, e.g., Azure Blob Storage.</span></span> <span data-ttu-id="ab5a8-177">Per modificare il livello di applicazione predefinito repository, registrare un singleton personalizzato IXmlRepository il provider del servizio.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-177">To change the default repository application-wide, register a custom singleton IXmlRepository in the service provider.</span></span>

<a name=data-protection-extensibility-key-management-ixmlencryptor></a>

## <a name="ixmlencryptor"></a><span data-ttu-id="ab5a8-178">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="ab5a8-178">IXmlEncryptor</span></span>

<span data-ttu-id="ab5a8-179">L'interfaccia IXmlEncryptor rappresenta un tipo che è possibile crittografare un elemento XML di testo normale.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-179">The IXmlEncryptor interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="ab5a8-180">Espone una singola API:</span><span class="sxs-lookup"><span data-stu-id="ab5a8-180">It exposes a single API:</span></span>

* <span data-ttu-id="ab5a8-181">Crittografare (plaintextElement XElement): EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="ab5a8-181">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="ab5a8-182">Se un IAuthenticatedEncryptorDescriptor serializzato contiene tutti gli elementi contrassegnati come "richiede la crittografia", quindi XmlKeyManager verranno eseguiti gli elementi tramite il metodo di crittografia del IXmlEncryptor configurato e verrà mantenuti anziché l'elemento crittografato rispetto all'elemento di testo normale per il IXmlRepository.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-182">If a serialized IAuthenticatedEncryptorDescriptor contains any elements marked as "requires encryption", then XmlKeyManager will run those elements through the configured IXmlEncryptor's Encrypt method, and it will persist the enciphered element rather than the plaintext element to the IXmlRepository.</span></span> <span data-ttu-id="ab5a8-183">L'output del metodo di crittografia è un oggetto EncryptedXmlInfo.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-183">The output of the Encrypt method is an EncryptedXmlInfo object.</span></span> <span data-ttu-id="ab5a8-184">Questo oggetto è un wrapper che contiene il XElement crittografato risultante sia il tipo che rappresenta un IXmlDecryptor che può essere usato per decrittografare l'elemento corrispondente.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-184">This object is a wrapper which contains both the resultant enciphered XElement and the Type which represents an IXmlDecryptor which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="ab5a8-185">Sono disponibili quattro tipi concreti predefiniti che implementano IXmlEncryptor: CertificateXmlEncryptor, DpapiNGXmlEncryptor, DpapiXmlEncryptor e NullXmlEncryptor.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-185">There are four built-in concrete types which implement IXmlEncryptor: CertificateXmlEncryptor, DpapiNGXmlEncryptor, DpapiXmlEncryptor, and NullXmlEncryptor.</span></span> <span data-ttu-id="ab5a8-186">Vedere il [crittografia chiave documento rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-186">See the [key encryption at rest document](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more information.</span></span> <span data-ttu-id="ab5a8-187">Per modificare il meccanismo di crittografia chiave al resto predefinito a livello di applicazione, registrare un singleton personalizzato IXmlEncryptor il provider del servizio.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-187">To change the default key-encryption-at-rest mechanism application-wide, register a custom singleton IXmlEncryptor in the service provider.</span></span>

## <a name="ixmldecryptor"></a><span data-ttu-id="ab5a8-188">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="ab5a8-188">IXmlDecryptor</span></span>

<span data-ttu-id="ab5a8-189">L'interfaccia IXmlDecryptor rappresenta un tipo in grado di decrittografare un XElement che è stato crittografato mediante un IXmlEncryptor.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-189">The IXmlDecryptor interface represents a type that knows how to decrypt an XElement that was enciphered via an IXmlEncryptor.</span></span> <span data-ttu-id="ab5a8-190">Espone una singola API:</span><span class="sxs-lookup"><span data-stu-id="ab5a8-190">It exposes a single API:</span></span>

* <span data-ttu-id="ab5a8-191">Decrittografare (encryptedElement XElement): XElement</span><span class="sxs-lookup"><span data-stu-id="ab5a8-191">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="ab5a8-192">Il metodo di decrittografia Annulla la crittografia eseguita da IXmlEncryptor.Encrypt.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-192">The Decrypt method undoes the encryption performed by IXmlEncryptor.Encrypt.</span></span> <span data-ttu-id="ab5a8-193">In genere, ogni implementazione concreta di IXmlEncryptor avrà un'implementazione concreta di IXmlDecryptor corrispondente.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-193">Generally each concrete IXmlEncryptor implementation will have a corresponding concrete IXmlDecryptor implementation.</span></span>

<span data-ttu-id="ab5a8-194">I tipi che implementano IXmlDecryptor devono avere uno dei due costruttori pubblici seguenti:</span><span class="sxs-lookup"><span data-stu-id="ab5a8-194">Types which implement IXmlDecryptor should have one of the following two public constructors:</span></span>

* <span data-ttu-id="ab5a8-195">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="ab5a8-195">.ctor(IServiceProvider)</span></span>

* <span data-ttu-id="ab5a8-196">. ctor)</span><span class="sxs-lookup"><span data-stu-id="ab5a8-196">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="ab5a8-197">L'IServiceProvider passato al costruttore può essere null.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-197">The IServiceProvider passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="ab5a8-198">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="ab5a8-198">IKeyEscrowSink</span></span>

<span data-ttu-id="ab5a8-199">L'interfaccia IKeyEscrowSink rappresenta un tipo che può eseguire deposito delle informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-199">The IKeyEscrowSink interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="ab5a8-200">Tieni presente che i descrittori serializzati potrebbero contenere informazioni riservate (ad esempio, il materiale di crittografia) e questo è ciò che ha portato all'introduzione del [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) digitare in primo luogo.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-200">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="ab5a8-201">Tuttavia, il verificarsi di incidenti e keyrings può essere eliminato o danneggiato.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-201">However, accidents happen, and keyrings can be deleted or become corrupted.</span></span>

<span data-ttu-id="ab5a8-202">L'interfaccia deposito fornisce un'emergenza stessa, che consente l'accesso al codice XML serializzato non elaborati prima che venga trasformata da qualsiasi configurato [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="ab5a8-202">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it is transformed by any configured [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span></span> <span data-ttu-id="ab5a8-203">L'interfaccia espone una singola API:</span><span class="sxs-lookup"><span data-stu-id="ab5a8-203">The interface exposes a single API:</span></span>

* <span data-ttu-id="ab5a8-204">Archivio (keyId Guid, elemento XElement)</span><span class="sxs-lookup"><span data-stu-id="ab5a8-204">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="ab5a8-205">In questo caso, l'implementazione di IKeyEscrowSink per gestire l'elemento specificato in modo sicuro coerenza con i criteri di business.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-205">It is up to the IKeyEscrowSink implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="ab5a8-206">Una possibile implementazione potrebbe essere usato per il sink deposito crittografare l'elemento XML usando un certificato x. 509 azienda noto in cui la chiave privata del certificato è stata depositata; il tipo CertificateXmlEncryptor assiste con questo oggetto.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-206">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the CertificateXmlEncryptor type can assist with this.</span></span> <span data-ttu-id="ab5a8-207">L'implementazione di IKeyEscrowSink è inoltre responsabile per la persistenza in modo appropriato l'elemento specificato.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-207">The IKeyEscrowSink implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="ab5a8-208">Per impostazione predefinita non è abilitato alcun meccanismo deposito, anche se gli amministratori di server possono [questa configurazione globale](../configuration/machine-wide-policy.md#data-protection-configuration-machinewidepolicy).</span><span class="sxs-lookup"><span data-stu-id="ab5a8-208">By default no escrow mechanism is enabled, though server administrators can [configure this globally](../configuration/machine-wide-policy.md#data-protection-configuration-machinewidepolicy).</span></span> <span data-ttu-id="ab5a8-209">Può anche essere configurato a livello di programmazione tramite le *IDataProtectionBuilder.AddKeyEscrowSink* metodo come illustrato nell'esempio riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-209">It can also be configured programmatically via the *IDataProtectionBuilder.AddKeyEscrowSink* method as shown in the sample below.</span></span> <span data-ttu-id="ab5a8-210">Il *AddKeyEscrowSink* mirror overload di metodo di *IServiceCollection.AddSingleton* e *IServiceCollection.AddInstance* overload, come IKeyEscrowSink le istanze devono essere singleton.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-210">The *AddKeyEscrowSink* method overloads mirror the *IServiceCollection.AddSingleton* and *IServiceCollection.AddInstance* overloads, as IKeyEscrowSink instances are intended to be singletons.</span></span> <span data-ttu-id="ab5a8-211">Se sono registrate più istanze di IKeyEscrowSink, ciascuno di essi verrà chiamata durante la generazione di chiavi, in modo possano depositare le chiavi per diversi meccanismi contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-211">If multiple IKeyEscrowSink instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="ab5a8-212">Non è disponibile alcuna API per la lettura di materiale da un'istanza di IKeyEscrowSink.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-212">There is no API to read material from an IKeyEscrowSink instance.</span></span> <span data-ttu-id="ab5a8-213">Questo comportamento è coerente con la teoria della progettazione del meccanismo del deposito: è progettato per rendere accessibili a un'autorità attendibile il materiale della chiave e poiché l'applicazione non costituisce un'autorità attendibile, non deve avere accesso al proprio materiale garantite.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-213">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="ab5a8-214">Esempio di codice seguente illustra la creazione e la registrazione di un IKeyEscrowSink in cui sono depositare le chiavi in modo che solo i membri di "CONTOSODomain Admins" è possono ripristinare tali componenti.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-214">The following sample code demonstrates creating and registering an IKeyEscrowSink where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="ab5a8-215">Per eseguire questo esempio, è necessario essere in un dominio di Windows 8 / computer con Windows Server 2012 e il controller di dominio deve essere Windows Server 2012 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="ab5a8-215">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

<span data-ttu-id="ab5a8-216">[!code-none[Principale](key-management/samples/key-management-extensibility.cs)]</span><span class="sxs-lookup"><span data-stu-id="ab5a8-216">[!code-none[Main](key-management/samples/key-management-extensibility.cs)]</span></span>
