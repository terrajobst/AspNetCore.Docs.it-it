---
title: "Estendibilità della crittografia core"
author: rick-anderson
description: Spiega IAuthenticatedEncryptor, IAuthenticatedEncryptorDescriptor, IAuthenticatedEncryptorDescriptorDeserializer e la factory di primo livello.
manager: wpickett
ms.author: riande
ms.date: 8/11/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: ead4012236244d88cff0b0520d000d89f93f3355
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="core-cryptography-extensibility"></a>Estendibilità della crittografia core

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> Tipi che implementano le interfacce seguenti devono essere thread-safe per più i chiamanti.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a>IAuthenticatedEncryptor

Il **IAuthenticatedEncryptor** interfaccia è il componente di base del sottosistema di crittografia. È in genere uno IAuthenticatedEncryptor per ogni chiave e l'istanza IAuthenticatedEncryptor esegue il wrapping di tutte le chiavi crittografiche e algoritmiche informazioni necessarie per eseguire le operazioni di crittografia.

Come suggerisce il nome, il tipo è responsabile di fornire servizi di crittografia e decrittografia autenticati. Espone le seguenti due API.

* Decrittografare (ArraySegment<byte> testo crittografato, ArraySegment<byte> additionalAuthenticatedData): byte]

* Crittografare (ArraySegment<byte> in testo normale, ArraySegment<byte> additionalAuthenticatedData): byte]

Il metodo di crittografia restituisce un blob che include il testo non crittografato crittografato e un tag di autenticazione. Il tag di autenticazione deve includere i dati di autenticazione aggiuntivi (ad), anche se non devono essere ripristinabili dal payload finale di AAD. Il metodo Decrypt convalida il tag di autenticazione e restituisce il payload deciphered. Tutti gli errori (ad eccezione di ArgumentNullException e così via) devono essere omogeneizzati per CryptographicException.

> [!NOTE]
> La stessa istanza di IAuthenticatedEncryptor non è effettivamente necessario contenere il materiale della chiave. Ad esempio, l'implementazione può delegare a un modulo HSM per tutte le operazioni.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a>Come creare un IAuthenticatedEncryptor

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Il **IAuthenticatedEncryptorFactory** interfaccia rappresenta un tipo in grado di creare un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) istanza. L'API è come indicato di seguito.

* CreateEncryptorInstance (chiave IKey): IAuthenticatedEncryptor

Per un'istanza specificata di IKey, qualsiasi autenticazione rifiutati creati dal metodo CreateEncryptorInstance devono essere considerati equivalente, come nell'esempio di codice riportato di seguito.

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Il **IAuthenticatedEncryptorDescriptor** interfaccia rappresenta un tipo in grado di creare un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) istanza. L'API è come indicato di seguito.

* CreateEncryptorInstance(): IAuthenticatedEncryptor

* ExportToXml() : XmlSerializedDescriptorInfo

Ad esempio IAuthenticatedEncryptor, un'istanza di IAuthenticatedEncryptorDescriptor equivale a eseguire il wrapping di una chiave specifica. Ciò significa che per un'istanza di IAuthenticatedEncryptorDescriptor specificata, qualsiasi rifiutati autenticati creati dal metodo CreateEncryptorInstance devono essere considerati equivalenti, come l'esempio di codice riportato di seguito.

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

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a>IAuthenticatedEncryptorDescriptor (ASP.NET Core solo 2. x)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Il **IAuthenticatedEncryptorDescriptor** interfaccia rappresenta un tipo in grado di esportare se stessa in formato XML. L'API è come indicato di seguito.

* ExportToXml() : XmlSerializedDescriptorInfo

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a>Serializzazione XML

La differenza principale tra IAuthenticatedEncryptor e IAuthenticatedEncryptorDescriptor è che il descrittore di grado di creare il componente di crittografia e fornirlo con gli argomenti validi. Si consideri un IAuthenticatedEncryptor la cui implementazione si basa su SymmetricAlgorithm e KeyedHashAlgorithm. Processo del componente di crittografia consiste nell'utilizzare questi tipi, ma non necessariamente conoscere provenienza questi tipi, in modo non è realmente scrivere una descrizione appropriata della procedura ricreare stesso se l'applicazione viene riavviata. Il descrittore agisce come un livello superiore nella parte superiore di questa. Poiché il descrittore sia in grado di creare l'istanza del componente di crittografia (ad esempio, da sapere come creare gli algoritmi necessari), in modo che l'istanza del componente di crittografia può essere ricreato dopo la reimpostazione di un'applicazione può serializzare tali informazioni in formato XML.

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

Il descrittore di può essere serializzato tramite la routine ExportToXml. Questa routine restituisce un XmlSerializedDescriptorInfo che contiene due proprietà: la rappresentazione di XElement del descrittore e il tipo che rappresenta un [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) che può essere utilizzato per ripristinare il descrittore di base di XElement corrispondente.

Il descrittore serializzato può contenere informazioni riservate, ad esempio chiavi crittografiche. Il sistema di protezione dati include il supporto incorporato per crittografare le informazioni prima di è resa persistente nell'archivio. Per sfruttare i vantaggi di questo, è necessario il descrittore di contrassegnare l'elemento che contiene informazioni riservate con il nome dell'attributo "requiresEncryption" (xmlns "http://schemas.asp.net/2015/03/dataProtection"), valore "true".

>[!TIP]
> È un'API di supporto per l'impostazione di questo attributo. Chiamare il metodo di estensione che XElement.markasrequiresencryption() si trova nello spazio dei nomi Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.

Può anche essere casi in cui il descrittore serializzato non contiene informazioni riservate. Si consideri di nuovo nel caso di una chiave di crittografia archiviata in un HSM. Il descrittore non è possibile scrivere il materiale della chiave durante la serializzazione di se stesso dall'hardware di non esporre il materiale di riferimento in formato non crittografato. Al contrario, il descrittore di può scrivere la versione di chiave con wrapping della chiave (se il modulo di protezione hardware consente l'esportazione in questo modo) o l'identificatore univoco dell'hardware per la chiave.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a>IAuthenticatedEncryptorDescriptorDeserializer

Il **IAuthenticatedEncryptorDescriptorDeserializer** interfaccia rappresenta un tipo in grado di deserializzare un'istanza di IAuthenticatedEncryptorDescriptor da un XElement. Espone un singolo metodo:

* ImportFromXml (elemento XElement): IAuthenticatedEncryptorDescriptor

Il metodo ImportFromXml accetta XElement che è stato restituito da [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) e crea un equivalente di IAuthenticatedEncryptorDescriptor l'originale.

I tipi che implementano IAuthenticatedEncryptorDescriptorDeserializer devono avere uno dei due costruttori pubblici seguenti:

* .ctor(IServiceProvider)

* .ctor()

> [!NOTE]
> L'IServiceProvider passato al costruttore può essere null.

## <a name="the-top-level-factory"></a>La factory di primo livello

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Il **AlgorithmConfiguration** classe rappresenta un tipo che sia in grado di creare [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) istanze. Espone una singola API.

* CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor

AlgorithmConfiguration può essere paragonato alla factory di primo livello. La configurazione viene utilizzato come modello. Esegue il wrapping informazioni algoritmiche (ad esempio, questa configurazione genera descrittori con una chiave master di AES-128-GCM), ma non ancora è associata a una chiave specifica.

Quando viene chiamato CreateNewDescriptor, nuovo materiale della chiave viene creato esclusivamente per la chiamata e viene generato un nuovo IAuthenticatedEncryptorDescriptor che include il materiale della chiave e le informazioni algoritmiche necessarie per utilizzare il materiale di riferimento. Il materiale della chiave potrebbe creato in software (e in memoria), viene creato e potrebbe essere contenuta all'interno di un modulo di protezione hardware e così via. Il punto essenziale è che le due chiamate a CreateNewDescriptor mai devono essere create istanze IAuthenticatedEncryptorDescriptor equivalente.

Il tipo AlgorithmConfiguration funge da punto di ingresso per le routine di creazione della chiave, ad esempio [rollover automatico della chiave](../implementation/key-management.md#key-expiration-and-rolling). Per modificare l'implementazione per tutte le chiavi future, impostare la proprietà AuthenticatedEncryptorConfiguration in KeyManagementOptions.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Il **IAuthenticatedEncryptorConfiguration** interfaccia rappresenta un tipo che sia in grado di creare [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) istanze. Espone una singola API.

* CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor

IAuthenticatedEncryptorConfiguration può essere paragonato alla factory di primo livello. La configurazione viene utilizzato come modello. Esegue il wrapping informazioni algoritmiche (ad esempio, questa configurazione genera descrittori con una chiave master di AES-128-GCM), ma non ancora è associata a una chiave specifica.

Quando viene chiamato CreateNewDescriptor, nuovo materiale della chiave viene creato esclusivamente per la chiamata e viene generato un nuovo IAuthenticatedEncryptorDescriptor che include il materiale della chiave e le informazioni algoritmiche necessarie per utilizzare il materiale di riferimento. Il materiale della chiave potrebbe creato in software (e in memoria), viene creato e potrebbe essere contenuta all'interno di un modulo di protezione hardware e così via. Il punto essenziale è che le due chiamate a CreateNewDescriptor mai devono essere create istanze IAuthenticatedEncryptorDescriptor equivalente.

Il tipo IAuthenticatedEncryptorConfiguration funge da punto di ingresso per le routine di creazione della chiave, ad esempio [rollover automatico della chiave](../implementation/key-management.md#key-expiration-and-rolling). Per modificare l'implementazione per tutte le chiavi future, registrare un singleton IAuthenticatedEncryptorConfiguration nel contenitore dei servizi.

---
