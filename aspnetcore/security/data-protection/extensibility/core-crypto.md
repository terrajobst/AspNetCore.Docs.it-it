---
title: Estendibilità della crittografia core in ASP.NET Core
author: rick-anderson
description: Informazioni su IAuthenticatedEncryptor, IAuthenticatedEncryptorDescriptor, IAuthenticatedEncryptorDescriptorDeserializer e la factory di primo livello.
ms.author: riande
ms.date: 8/11/2017
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: cf4a142992efe5b00a75285ef9ad9735fe7be411
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209070"
---
# <a name="core-cryptography-extensibility-in-aspnet-core"></a>Estendibilità della crittografia core in ASP.NET Core

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> I tipi che implementano una delle interfacce seguenti devono essere thread-safe per i chiamanti di più.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a>IAuthenticatedEncryptor

Il **IAuthenticatedEncryptor** interfaccia è il blocco predefinito di base del sottosistema di crittografia. È in genere uno IAuthenticatedEncryptor per ogni chiave e l'istanza IAuthenticatedEncryptor esegue il wrapping di tutte le chiavi crittografiche e algoritmiche informazioni necessarie per eseguire operazioni di crittografia.

Come suggerisce il nome, il tipo è responsabile di fornire servizi di crittografia e decrittografia autenticato. Espone le seguenti due API.

* `Decrypt(ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData) : byte[]`

* `Encrypt(ArraySegment<byte> plaintext, ArraySegment<byte> additionalAuthenticatedData) : byte[]`

Il metodo Encrypt restituisce un blob che include il testo non crittografato cifrato e un tag di autenticazione. Il tag di autenticazione deve includere i dati di autenticazione aggiuntivi (AAD), anche se non è necessario ripristinabili dal payload finale di AAD. Il metodo Decrypt convalida il tag di autenticazione e restituisce il payload deciphered. Tutti gli errori (ad eccezione ArgumentNullException e simili) devono essere omogeneizzati a CryptographicException.

> [!NOTE]
> La stessa istanza IAuthenticatedEncryptor non è effettivamente necessario contenere il materiale della chiave. Ad esempio, l'implementazione potesse delegare a un modulo HSM per tutte le operazioni.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a>Come creare un IAuthenticatedEncryptor

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Il **IAuthenticatedEncryptorFactory** interfaccia rappresenta un tipo in grado di creare un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) istanza. L'API è come indicato di seguito.

* CreateEncryptorInstance (chiave di strumentazione): IAuthenticatedEncryptor

Per qualsiasi istanza IKey, qualsiasi autenticazione rifiutati creati dal relativo metodo CreateEncryptorInstance devono essere considerati equivalente, come nell'esempio di codice seguente.

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

* CreateEncryptorInstance() : IAuthenticatedEncryptor

* ExportToXml(): XmlSerializedDescriptorInfo

Ad esempio IAuthenticatedEncryptor, un'istanza di IAuthenticatedEncryptorDescriptor viene considerato come eseguire il wrapping di una chiave specifica. Ciò significa che per qualsiasi istanza IAuthenticatedEncryptorDescriptor, qualsiasi autenticazione rifiutati creati dal relativo metodo CreateEncryptorInstance devono essere considerati equivalenti, come nell'esempio di codice seguente.

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

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a>IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x solo)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Il **IAuthenticatedEncryptorDescriptor** interfaccia rappresenta un tipo in grado di esportare se stessa in XML. L'API è come indicato di seguito.

* ExportToXml(): XmlSerializedDescriptorInfo

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a>Serializzazione XML

La differenza principale tra IAuthenticatedEncryptor e IAuthenticatedEncryptorDescriptor è che il descrittore di grado di creare il componente di crittografia e fornirlo con gli argomenti validi. Prendere in considerazione un IAuthenticatedEncryptor la cui implementazione si basa su SymmetricAlgorithm e KeyedHashAlgorithm. Processo del componente di crittografia consiste nell'utilizzare questi tipi, ma non necessariamente provenienza questi tipi, in modo davvero non è possibile scrivere una descrizione appropriata della procedura ricreare stesso se l'applicazione viene riavviata. Il descrittore agisce come un livello più elevato oltre a questa. Poiché il descrittore di grado di creare l'istanza del componente di crittografia (ad esempio, sapere come creare gli algoritmi necessari), può serializzare tali informazioni in formato XML in modo che l'istanza del componente di crittografia può essere ricreata dopo che un'applicazione di reimpostazione.

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

Il descrittore può essere serializzato tramite la routine ExportToXml. Questa routine restituisce un XmlSerializedDescriptorInfo che contiene due proprietà: la rappresentazione di XElement del descrittore e il tipo che rappresenta un' [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) che può essere utilizzato per riprendere il descrittore specificato di XElement corrispondente.

Il descrittore serializzato può contenere informazioni riservate, ad esempio chiavi crittografiche. Il sistema di protezione dati include il supporto incorporato per crittografare le informazioni prima che questo ha reso persistente in un archivio. Per sfruttare i vantaggi di questo oggetto, il descrittore deve contrassegnare l'elemento che contiene informazioni riservate con il nome di attributo "requiresEncryption" (xmlns "<http://schemas.asp.net/2015/03/dataProtection>"), valore "true".

>[!TIP]
> È presente un'API di supporto per l'impostazione di questo attributo. Chiamare il metodo di estensione XElement.MarkAsRequiresEncryption() che si trova nello spazio dei nomi Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.

Possono anche esserci casi in cui il descrittore serializzato non contiene informazioni riservate. Si consideri di nuovo nel caso di una chiave di crittografia archiviata in un modulo HSM. Il descrittore non è possibile scrivere il materiale della chiave durante la serializzazione stesso, poiché il modulo di protezione hardware non esporre il materiale in formato testo normale. Al contrario, il descrittore può scrivere la versione della chiave (se il modulo di protezione hardware permette di esportare in questo modo) o l'identificatore univoco del modulo per la chiave con wrapping chiave.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a>IAuthenticatedEncryptorDescriptorDeserializer

Il **IAuthenticatedEncryptorDescriptorDeserializer** interfaccia rappresenta un tipo in grado di deserializzare un'istanza di IAuthenticatedEncryptorDescriptor da un XElement. Che espone un solo metodo:

* ImportFromXml (elemento XElement): IAuthenticatedEncryptorDescriptor

Il metodo ImportFromXml accetta XElement che è stato restituito da [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) e crea l'equivalente di IAuthenticatedEncryptorDescriptor l'originale.

I tipi che implementano IAuthenticatedEncryptorDescriptorDeserializer devono avere uno dei due costruttori pubblici seguenti:

* .ctor(IServiceProvider)

* ctor)

> [!NOTE]
> IServiceProvider passato al costruttore può essere null.

## <a name="the-top-level-factory"></a>La factory di primo livello

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Il **AlgorithmConfiguration** classe rappresenta un tipo che sia in grado di creare [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) istanze. Espone una singola API.

* CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor

Pensare AlgorithmConfiguration perché la factory di primo livello. La configurazione viene usato come modello. Esegue il wrapping informazioni algoritmiche (ad esempio, questa configurazione genera descrittori con una chiave master di AES-128-GCM), ma non ancora associato con una chiave specifica.

Quando viene chiamato CreateNewDescriptor, esclusivamente per questa chiamata viene creato nuovo materiale della chiave, e viene generato un nuovo IAuthenticatedEncryptorDescriptor che esegue il wrapping di questo materiale della chiave e le informazioni algoritmiche per utilizzare il materiale necessaria. Il materiale della chiave potessero essere creati nel software (e mantenute in memoria), potessero essere creato e mantenuto all'interno di un modulo di protezione hardware e così via. Il punto essenziale è che tutte le due chiamate a CreateNewDescriptor mai devono creare istanze IAuthenticatedEncryptorDescriptor equivalente.

Il tipo AlgorithmConfiguration serve come punto di ingresso per le routine di creazione della chiave, ad esempio [rollover della chiave automatico](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling). Per modificare l'implementazione per tutte le chiavi future, impostare la proprietà AuthenticatedEncryptorConfiguration in KeyManagementOptions.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Il **IAuthenticatedEncryptorConfiguration** interfaccia rappresenta un tipo che sia in grado di creare [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) istanze. Espone una singola API.

* CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor

Pensare IAuthenticatedEncryptorConfiguration perché la factory di primo livello. La configurazione viene usato come modello. Esegue il wrapping informazioni algoritmiche (ad esempio, questa configurazione genera descrittori con una chiave master di AES-128-GCM), ma non ancora associato con una chiave specifica.

Quando viene chiamato CreateNewDescriptor, esclusivamente per questa chiamata viene creato nuovo materiale della chiave, e viene generato un nuovo IAuthenticatedEncryptorDescriptor che esegue il wrapping di questo materiale della chiave e le informazioni algoritmiche per utilizzare il materiale necessaria. Il materiale della chiave potessero essere creati nel software (e mantenute in memoria), potessero essere creato e mantenuto all'interno di un modulo di protezione hardware e così via. Il punto essenziale è che tutte le due chiamate a CreateNewDescriptor mai devono creare istanze IAuthenticatedEncryptorDescriptor equivalente.

Il tipo IAuthenticatedEncryptorConfiguration serve come punto di ingresso per le routine di creazione della chiave, ad esempio [rollover della chiave automatico](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling). Per modificare l'implementazione per tutte le chiavi futuro, registrare un singleton IAuthenticatedEncryptorConfiguration nel contenitore dei servizi.

---
