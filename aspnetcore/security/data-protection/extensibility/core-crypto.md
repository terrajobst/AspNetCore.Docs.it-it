---
title: Estendibilità della crittografia core in ASP.NET Core
author: rick-anderson
description: Informazioni su IAuthenticatedEncryptor, IAuthenticatedEncryptorDescriptor, IAuthenticatedEncryptorDescriptorDeserializer e la factory di primo livello.
ms.author: riande
ms.date: 08/11/2017
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: a5f651e3313cc579b995b45905826a5bffcc241c
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814700"
---
# <a name="core-cryptography-extensibility-in-aspnet-core"></a><span data-ttu-id="aed2f-103">Estendibilità della crittografia core in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aed2f-103">Core cryptography extensibility in ASP.NET Core</span></span>

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> <span data-ttu-id="aed2f-104">I tipi che implementano una delle interfacce seguenti devono essere thread-safe per i chiamanti di più.</span><span class="sxs-lookup"><span data-stu-id="aed2f-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a><span data-ttu-id="aed2f-105">IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="aed2f-105">IAuthenticatedEncryptor</span></span>

<span data-ttu-id="aed2f-106">Il **IAuthenticatedEncryptor** interfaccia è il blocco predefinito di base del sottosistema di crittografia.</span><span class="sxs-lookup"><span data-stu-id="aed2f-106">The **IAuthenticatedEncryptor** interface is the basic building block of the cryptographic subsystem.</span></span> <span data-ttu-id="aed2f-107">È in genere uno IAuthenticatedEncryptor per ogni chiave e l'istanza IAuthenticatedEncryptor esegue il wrapping di tutte le chiavi crittografiche e algoritmiche informazioni necessarie per eseguire operazioni di crittografia.</span><span class="sxs-lookup"><span data-stu-id="aed2f-107">There's generally one IAuthenticatedEncryptor per key, and the IAuthenticatedEncryptor instance wraps all cryptographic key material and algorithmic information necessary to perform cryptographic operations.</span></span>

<span data-ttu-id="aed2f-108">Come suggerisce il nome, il tipo è responsabile di fornire servizi di crittografia e decrittografia autenticato.</span><span class="sxs-lookup"><span data-stu-id="aed2f-108">As its name suggests, the type is responsible for providing authenticated encryption and decryption services.</span></span> <span data-ttu-id="aed2f-109">Espone le seguenti due API.</span><span class="sxs-lookup"><span data-stu-id="aed2f-109">It exposes the following two APIs.</span></span>

* `Decrypt(ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData) : byte[]`

* `Encrypt(ArraySegment<byte> plaintext, ArraySegment<byte> additionalAuthenticatedData) : byte[]`

<span data-ttu-id="aed2f-110">Il metodo Encrypt restituisce un blob che include il testo non crittografato cifrato e un tag di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="aed2f-110">The Encrypt method returns a blob that includes the enciphered plaintext and an authentication tag.</span></span> <span data-ttu-id="aed2f-111">Il tag di autenticazione deve includere i dati di autenticazione aggiuntivi (AAD), anche se non è necessario ripristinabili dal payload finale di AAD.</span><span class="sxs-lookup"><span data-stu-id="aed2f-111">The authentication tag must encompass the additional authenticated data (AAD), though the AAD itself need not be recoverable from the final payload.</span></span> <span data-ttu-id="aed2f-112">Il metodo Decrypt convalida il tag di autenticazione e restituisce il payload deciphered.</span><span class="sxs-lookup"><span data-stu-id="aed2f-112">The Decrypt method validates the authentication tag and returns the deciphered payload.</span></span> <span data-ttu-id="aed2f-113">Tutti gli errori (ad eccezione ArgumentNullException e simili) devono essere omogeneizzati a CryptographicException.</span><span class="sxs-lookup"><span data-stu-id="aed2f-113">All failures (except ArgumentNullException and similar) should be homogenized to CryptographicException.</span></span>

> [!NOTE]
> <span data-ttu-id="aed2f-114">La stessa istanza IAuthenticatedEncryptor non è effettivamente necessario contenere il materiale della chiave.</span><span class="sxs-lookup"><span data-stu-id="aed2f-114">The IAuthenticatedEncryptor instance itself doesn't actually need to contain the key material.</span></span> <span data-ttu-id="aed2f-115">Ad esempio, l'implementazione potesse delegare a un modulo HSM per tutte le operazioni.</span><span class="sxs-lookup"><span data-stu-id="aed2f-115">For example, the implementation could delegate to an HSM for all operations.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a><span data-ttu-id="aed2f-116">Come creare un IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="aed2f-116">How to create an IAuthenticatedEncryptor</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="aed2f-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="aed2f-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="aed2f-118">Il **IAuthenticatedEncryptorFactory** interfaccia rappresenta un tipo in grado di creare un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) istanza.</span><span class="sxs-lookup"><span data-stu-id="aed2f-118">The **IAuthenticatedEncryptorFactory** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="aed2f-119">L'API è come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="aed2f-119">Its API is as follows.</span></span>

* <span data-ttu-id="aed2f-120">CreateEncryptorInstance (chiave di strumentazione): IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="aed2f-120">CreateEncryptorInstance(IKey key) : IAuthenticatedEncryptor</span></span>

<span data-ttu-id="aed2f-121">Per qualsiasi istanza IKey, qualsiasi autenticazione rifiutati creati dal relativo metodo CreateEncryptorInstance devono essere considerati equivalente, come nell'esempio di codice seguente.</span><span class="sxs-lookup"><span data-stu-id="aed2f-121">For any given IKey instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

```csharp
// we have an IAuthenticatedEncryptorFactory instance and an IKey instance
IAuthenticatedEncryptorFactory factory = ...;
IKey key = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = factory.CreateEncryptorInstance(key);
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = factory.CreateEncryptorInstance(key);
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="aed2f-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="aed2f-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="aed2f-123">Il **IAuthenticatedEncryptorDescriptor** interfaccia rappresenta un tipo in grado di creare un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) istanza.</span><span class="sxs-lookup"><span data-stu-id="aed2f-123">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="aed2f-124">L'API è come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="aed2f-124">Its API is as follows.</span></span>

* <span data-ttu-id="aed2f-125">CreateEncryptorInstance() : IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="aed2f-125">CreateEncryptorInstance() : IAuthenticatedEncryptor</span></span>

* <span data-ttu-id="aed2f-126">ExportToXml(): XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="aed2f-126">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

<span data-ttu-id="aed2f-127">Ad esempio IAuthenticatedEncryptor, un'istanza di IAuthenticatedEncryptorDescriptor viene considerato come eseguire il wrapping di una chiave specifica.</span><span class="sxs-lookup"><span data-stu-id="aed2f-127">Like IAuthenticatedEncryptor, an instance of IAuthenticatedEncryptorDescriptor is assumed to wrap one specific key.</span></span> <span data-ttu-id="aed2f-128">Ciò significa che per qualsiasi istanza IAuthenticatedEncryptorDescriptor, qualsiasi autenticazione rifiutati creati dal relativo metodo CreateEncryptorInstance devono essere considerati equivalenti, come nell'esempio di codice seguente.</span><span class="sxs-lookup"><span data-stu-id="aed2f-128">This means that for any given IAuthenticatedEncryptorDescriptor instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

```csharp
// we have an IAuthenticatedEncryptorDescriptor instance
IAuthenticatedEncryptorDescriptor descriptor = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = descriptor.CreateEncryptorInstance();
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = descriptor.CreateEncryptorInstance();
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

---

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a><span data-ttu-id="aed2f-129">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x solo)</span><span class="sxs-lookup"><span data-stu-id="aed2f-129">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x only)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="aed2f-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="aed2f-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="aed2f-131">Il **IAuthenticatedEncryptorDescriptor** interfaccia rappresenta un tipo in grado di esportare se stessa in XML.</span><span class="sxs-lookup"><span data-stu-id="aed2f-131">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to export itself to XML.</span></span> <span data-ttu-id="aed2f-132">L'API è come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="aed2f-132">Its API is as follows.</span></span>

* <span data-ttu-id="aed2f-133">ExportToXml(): XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="aed2f-133">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="aed2f-134">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="aed2f-134">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a><span data-ttu-id="aed2f-135">Serializzazione XML</span><span class="sxs-lookup"><span data-stu-id="aed2f-135">XML Serialization</span></span>

<span data-ttu-id="aed2f-136">La differenza principale tra IAuthenticatedEncryptor e IAuthenticatedEncryptorDescriptor è che il descrittore di grado di creare il componente di crittografia e fornirlo con gli argomenti validi.</span><span class="sxs-lookup"><span data-stu-id="aed2f-136">The primary difference between IAuthenticatedEncryptor and IAuthenticatedEncryptorDescriptor is that the descriptor knows how to create the encryptor and supply it with valid arguments.</span></span> <span data-ttu-id="aed2f-137">Prendere in considerazione un IAuthenticatedEncryptor la cui implementazione si basa su SymmetricAlgorithm e KeyedHashAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="aed2f-137">Consider an IAuthenticatedEncryptor whose implementation relies on SymmetricAlgorithm and KeyedHashAlgorithm.</span></span> <span data-ttu-id="aed2f-138">Processo del componente di crittografia consiste nell'utilizzare questi tipi, ma non necessariamente provenienza questi tipi, in modo davvero non è possibile scrivere una descrizione appropriata della procedura ricreare stesso se l'applicazione viene riavviata.</span><span class="sxs-lookup"><span data-stu-id="aed2f-138">The encryptor's job is to consume these types, but it doesn't necessarily know where these types came from, so it can't really write out a proper description of how to recreate itself if the application restarts.</span></span> <span data-ttu-id="aed2f-139">Il descrittore agisce come un livello più elevato oltre a questa.</span><span class="sxs-lookup"><span data-stu-id="aed2f-139">The descriptor acts as a higher level on top of this.</span></span> <span data-ttu-id="aed2f-140">Poiché il descrittore di grado di creare l'istanza del componente di crittografia (ad esempio, sapere come creare gli algoritmi necessari), può serializzare tali informazioni in formato XML in modo che l'istanza del componente di crittografia può essere ricreata dopo che un'applicazione di reimpostazione.</span><span class="sxs-lookup"><span data-stu-id="aed2f-140">Since the descriptor knows how to create the encryptor instance (e.g., it knows how to create the required algorithms), it can serialize that knowledge in XML form so that the encryptor instance can be recreated after an application reset.</span></span>

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

<span data-ttu-id="aed2f-141">Il descrittore può essere serializzato tramite la routine ExportToXml.</span><span class="sxs-lookup"><span data-stu-id="aed2f-141">The descriptor can be serialized via its ExportToXml routine.</span></span> <span data-ttu-id="aed2f-142">Questa routine restituisce un XmlSerializedDescriptorInfo che contiene due proprietà: la rappresentazione di XElement del descrittore e il tipo che rappresenta un' [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) che può essere utilizzato per riprendere il descrittore specificato di XElement corrispondente.</span><span class="sxs-lookup"><span data-stu-id="aed2f-142">This routine returns an XmlSerializedDescriptorInfo which contains two properties: the XElement representation of the descriptor and the Type which represents an [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) which can be used to resurrect this descriptor given the corresponding XElement.</span></span>

<span data-ttu-id="aed2f-143">Il descrittore serializzato può contenere informazioni riservate, ad esempio chiavi crittografiche.</span><span class="sxs-lookup"><span data-stu-id="aed2f-143">The serialized descriptor may contain sensitive information such as cryptographic key material.</span></span> <span data-ttu-id="aed2f-144">Il sistema di protezione dati include il supporto incorporato per crittografare le informazioni prima che questo ha reso persistente in un archivio.</span><span class="sxs-lookup"><span data-stu-id="aed2f-144">The data protection system has built-in support for encrypting information before it's persisted to storage.</span></span> <span data-ttu-id="aed2f-145">Per sfruttare i vantaggi di questo oggetto, il descrittore deve contrassegnare l'elemento che contiene informazioni riservate con il nome di attributo "requiresEncryption" (xmlns "<http://schemas.asp.net/2015/03/dataProtection>"), valore "true".</span><span class="sxs-lookup"><span data-stu-id="aed2f-145">To take advantage of this, the descriptor should mark the element which contains sensitive information with the attribute name "requiresEncryption" (xmlns "<http://schemas.asp.net/2015/03/dataProtection>"), value "true".</span></span>

>[!TIP]
> <span data-ttu-id="aed2f-146">È presente un'API di supporto per l'impostazione di questo attributo.</span><span class="sxs-lookup"><span data-stu-id="aed2f-146">There's a helper API for setting this attribute.</span></span> <span data-ttu-id="aed2f-147">Chiamare il metodo di estensione XElement.MarkAsRequiresEncryption() che si trova nello spazio dei nomi Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span><span class="sxs-lookup"><span data-stu-id="aed2f-147">Call the extension method XElement.MarkAsRequiresEncryption() located in namespace Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span></span>

<span data-ttu-id="aed2f-148">Possono anche esserci casi in cui il descrittore serializzato non contiene informazioni riservate.</span><span class="sxs-lookup"><span data-stu-id="aed2f-148">There can also be cases where the serialized descriptor doesn't contain sensitive information.</span></span> <span data-ttu-id="aed2f-149">Si consideri di nuovo nel caso di una chiave di crittografia archiviata in un modulo HSM.</span><span class="sxs-lookup"><span data-stu-id="aed2f-149">Consider again the case of a cryptographic key stored in an HSM.</span></span> <span data-ttu-id="aed2f-150">Il descrittore non è possibile scrivere il materiale della chiave durante la serializzazione stesso, poiché il modulo di protezione hardware non esporre il materiale in formato testo normale.</span><span class="sxs-lookup"><span data-stu-id="aed2f-150">The descriptor cannot write out the key material when serializing itself since the HSM won't expose the material in plaintext form.</span></span> <span data-ttu-id="aed2f-151">Al contrario, il descrittore può scrivere la versione della chiave (se il modulo di protezione hardware permette di esportare in questo modo) o l'identificatore univoco del modulo per la chiave con wrapping chiave.</span><span class="sxs-lookup"><span data-stu-id="aed2f-151">Instead, the descriptor might write out the key-wrapped version of the key (if the HSM allows export in this fashion) or the HSM's own unique identifier for the key.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a><span data-ttu-id="aed2f-152">IAuthenticatedEncryptorDescriptorDeserializer</span><span class="sxs-lookup"><span data-stu-id="aed2f-152">IAuthenticatedEncryptorDescriptorDeserializer</span></span>

<span data-ttu-id="aed2f-153">Il **IAuthenticatedEncryptorDescriptorDeserializer** interfaccia rappresenta un tipo in grado di deserializzare un'istanza di IAuthenticatedEncryptorDescriptor da un XElement.</span><span class="sxs-lookup"><span data-stu-id="aed2f-153">The **IAuthenticatedEncryptorDescriptorDeserializer** interface represents a type that knows how to deserialize an IAuthenticatedEncryptorDescriptor instance from an XElement.</span></span> <span data-ttu-id="aed2f-154">Che espone un solo metodo:</span><span class="sxs-lookup"><span data-stu-id="aed2f-154">It exposes a single method:</span></span>

* <span data-ttu-id="aed2f-155">ImportFromXml (elemento XElement): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="aed2f-155">ImportFromXml(XElement element) : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="aed2f-156">Il metodo ImportFromXml accetta XElement che è stato restituito da [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) e crea l'equivalente di IAuthenticatedEncryptorDescriptor l'originale.</span><span class="sxs-lookup"><span data-stu-id="aed2f-156">The ImportFromXml method takes the XElement that was returned by [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) and creates an equivalent of the original IAuthenticatedEncryptorDescriptor.</span></span>

<span data-ttu-id="aed2f-157">I tipi che implementano IAuthenticatedEncryptorDescriptorDeserializer devono avere uno dei due costruttori pubblici seguenti:</span><span class="sxs-lookup"><span data-stu-id="aed2f-157">Types which implement IAuthenticatedEncryptorDescriptorDeserializer should have one of the following two public constructors:</span></span>

* <span data-ttu-id="aed2f-158">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="aed2f-158">.ctor(IServiceProvider)</span></span>

* <span data-ttu-id="aed2f-159">ctor)</span><span class="sxs-lookup"><span data-stu-id="aed2f-159">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="aed2f-160">IServiceProvider passato al costruttore può essere null.</span><span class="sxs-lookup"><span data-stu-id="aed2f-160">The IServiceProvider passed to the constructor may be null.</span></span>

## <a name="the-top-level-factory"></a><span data-ttu-id="aed2f-161">La factory di primo livello</span><span class="sxs-lookup"><span data-stu-id="aed2f-161">The top-level factory</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="aed2f-162">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="aed2f-162">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="aed2f-163">Il **AlgorithmConfiguration** classe rappresenta un tipo che sia in grado di creare [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) istanze.</span><span class="sxs-lookup"><span data-stu-id="aed2f-163">The **AlgorithmConfiguration** class represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="aed2f-164">Espone una singola API.</span><span class="sxs-lookup"><span data-stu-id="aed2f-164">It exposes a single API.</span></span>

* <span data-ttu-id="aed2f-165">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="aed2f-165">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="aed2f-166">Pensare AlgorithmConfiguration perché la factory di primo livello.</span><span class="sxs-lookup"><span data-stu-id="aed2f-166">Think of AlgorithmConfiguration as the top-level factory.</span></span> <span data-ttu-id="aed2f-167">La configurazione viene usato come modello.</span><span class="sxs-lookup"><span data-stu-id="aed2f-167">The configuration serves as a template.</span></span> <span data-ttu-id="aed2f-168">Esegue il wrapping informazioni algoritmiche (ad esempio, questa configurazione genera descrittori con una chiave master di AES-128-GCM), ma non ancora associato con una chiave specifica.</span><span class="sxs-lookup"><span data-stu-id="aed2f-168">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it's not yet associated with a specific key.</span></span>

<span data-ttu-id="aed2f-169">Quando viene chiamato CreateNewDescriptor, esclusivamente per questa chiamata viene creato nuovo materiale della chiave, e viene generato un nuovo IAuthenticatedEncryptorDescriptor che esegue il wrapping di questo materiale della chiave e le informazioni algoritmiche per utilizzare il materiale necessaria.</span><span class="sxs-lookup"><span data-stu-id="aed2f-169">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="aed2f-170">Il materiale della chiave potessero essere creati nel software (e mantenute in memoria), potessero essere creato e mantenuto all'interno di un modulo di protezione hardware e così via.</span><span class="sxs-lookup"><span data-stu-id="aed2f-170">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="aed2f-171">Il punto essenziale è che tutte le due chiamate a CreateNewDescriptor mai devono creare istanze IAuthenticatedEncryptorDescriptor equivalente.</span><span class="sxs-lookup"><span data-stu-id="aed2f-171">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="aed2f-172">Il tipo AlgorithmConfiguration serve come punto di ingresso per le routine di creazione della chiave, ad esempio [rollover della chiave automatico](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="aed2f-172">The AlgorithmConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span></span> <span data-ttu-id="aed2f-173">Per modificare l'implementazione per tutte le chiavi future, impostare la proprietà AuthenticatedEncryptorConfiguration in KeyManagementOptions.</span><span class="sxs-lookup"><span data-stu-id="aed2f-173">To change the implementation for all future keys, set the AuthenticatedEncryptorConfiguration property in KeyManagementOptions.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="aed2f-174">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="aed2f-174">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="aed2f-175">Il **IAuthenticatedEncryptorConfiguration** interfaccia rappresenta un tipo che sia in grado di creare [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) istanze.</span><span class="sxs-lookup"><span data-stu-id="aed2f-175">The **IAuthenticatedEncryptorConfiguration** interface represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="aed2f-176">Espone una singola API.</span><span class="sxs-lookup"><span data-stu-id="aed2f-176">It exposes a single API.</span></span>

* <span data-ttu-id="aed2f-177">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="aed2f-177">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="aed2f-178">Pensare IAuthenticatedEncryptorConfiguration perché la factory di primo livello.</span><span class="sxs-lookup"><span data-stu-id="aed2f-178">Think of IAuthenticatedEncryptorConfiguration as the top-level factory.</span></span> <span data-ttu-id="aed2f-179">La configurazione viene usato come modello.</span><span class="sxs-lookup"><span data-stu-id="aed2f-179">The configuration serves as a template.</span></span> <span data-ttu-id="aed2f-180">Esegue il wrapping informazioni algoritmiche (ad esempio, questa configurazione genera descrittori con una chiave master di AES-128-GCM), ma non ancora associato con una chiave specifica.</span><span class="sxs-lookup"><span data-stu-id="aed2f-180">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it's not yet associated with a specific key.</span></span>

<span data-ttu-id="aed2f-181">Quando viene chiamato CreateNewDescriptor, esclusivamente per questa chiamata viene creato nuovo materiale della chiave, e viene generato un nuovo IAuthenticatedEncryptorDescriptor che esegue il wrapping di questo materiale della chiave e le informazioni algoritmiche per utilizzare il materiale necessaria.</span><span class="sxs-lookup"><span data-stu-id="aed2f-181">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="aed2f-182">Il materiale della chiave potessero essere creati nel software (e mantenute in memoria), potessero essere creato e mantenuto all'interno di un modulo di protezione hardware e così via.</span><span class="sxs-lookup"><span data-stu-id="aed2f-182">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="aed2f-183">Il punto essenziale è che tutte le due chiamate a CreateNewDescriptor mai devono creare istanze IAuthenticatedEncryptorDescriptor equivalente.</span><span class="sxs-lookup"><span data-stu-id="aed2f-183">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="aed2f-184">Il tipo IAuthenticatedEncryptorConfiguration serve come punto di ingresso per le routine di creazione della chiave, ad esempio [rollover della chiave automatico](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="aed2f-184">The IAuthenticatedEncryptorConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span></span> <span data-ttu-id="aed2f-185">Per modificare l'implementazione per tutte le chiavi futuro, registrare un singleton IAuthenticatedEncryptorConfiguration nel contenitore dei servizi.</span><span class="sxs-lookup"><span data-stu-id="aed2f-185">To change the implementation for all future keys, register a singleton IAuthenticatedEncryptorConfiguration in the service container.</span></span>

---
