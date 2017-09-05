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
# <a name="key-management-extensibility"></a>Estensibilità di gestione delle chiavi

<a name=data-protection-extensibility-key-management></a>

>[!TIP]
> Lettura di [gestione delle chiavi](../implementation/key-management.md#data-protection-implementation-key-management) sezione prima di leggere questa sezione, come vengono illustrati alcuni dei concetti di queste API.

>[!WARNING]
> Tipi che implementano le interfacce seguenti devono essere thread-safe per più i chiamanti.

## <a name="key"></a>Chiave

L'interfaccia IKey è la rappresentazione di base di una chiave nel sistema di crittografia. La chiave di termine viene usata in senso astratta, non in senso letterale "crittografia materiale della chiave". Una chiave ha le proprietà seguenti:

* Date di scadenza, la creazione e attivazione

* Stato di revoca

* Identificatore di chiave (GUID)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

Inoltre, IKey espone un metodo CreateDecryptor che può essere utilizzato per creare un [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) istanza associato a questa chiave.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

Inoltre, IKey espone un metodo CreateEncryptorInstance che può essere utilizzato per creare un [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) istanza associato a questa chiave.

---

> [!NOTE]
> Non è disponibile alcuna API per recuperare il materiale crittografico non elaborato da un'istanza di IKey.

## <a name="ikeymanager"></a>IKeyManager

L'interfaccia IKeyManager rappresenta un oggetto responsabile della chiavi generali di archiviazione, il recupero e modifica. Espone tre operazioni di alto livello:

* Creare una nuova chiave e archiviarlo in un archivio.

* Ottenere tutte le chiavi dall'archiviazione.

* Revocare una o più chiavi e rendere persistenti le informazioni di revoca all'archiviazione.

>[!WARNING]
> Scrittura di un IKeyManager è un'attività molto avanzata e la maggior parte degli sviluppatori non deve tentare. Al contrario, la maggior parte degli sviluppatori devono sfruttare le funzionalità offerte dal [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) classe.

<a name=data-protection-extensibility-key-management-xmlkeymanager></a>

## <a name="xmlkeymanager"></a>XmlKeyManager

Il tipo XmlKeyManager è l'implementazione concreta nella casella di IKeyManager. Fornisce diverse funzionalità utili, inclusi deposito delle chiave e la crittografia delle chiavi inattivi. Le chiavi nel sistema sono rappresentate come elementi XML (in particolare, [XElement](https://msdn.microsoft.com/library/system.xml.linq.xelement(v=vs.110).aspx)).

XmlKeyManager dipende da numerosi altri componenti nel corso di completare le attività:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

* AlgorithmConfiguration, che determina gli algoritmi utilizzati dai nuove chiavi.

* IXmlRepository, che controlla in cui le chiavi vengono rese persistenti nell'archiviazione.

* [Facoltativo] IXmlEncryptor che consente di crittografare le chiavi inattivi.

* [Facoltativo] IKeyEscrowSink che fornisce servizi di deposito delle chiavi.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

* IXmlRepository, che controlla in cui le chiavi vengono rese persistenti nell'archiviazione.

* [Facoltativo] IXmlEncryptor che consente di crittografare le chiavi inattivi.

* [Facoltativo] IKeyEscrowSink che fornisce servizi di deposito delle chiavi.

---

Di seguito sono diagrammi di livello superiore che indicano come questi componenti vengono collegati tra loro in XmlKeyManager.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

   ![Creazione della chiave](key-management/_static/keycreation2.png)

   *Chiave creazione / CreateNewKey*

Nell'implementazione di CreateNewKey, il componente AlgorithmConfiguration viene utilizzato per creare un IAuthenticatedEncryptorDescriptor univoco, che viene quindi serializzato come XML. Se un sink di deposito delle chiavi è presente, il XML non elaborato (non crittografato) viene fornita al sink per l'archiviazione a lungo termine. Il codice XML non crittografato viene eseguito tramite un IXmlEncryptor (se richiesto) per generare il documento XML crittografato. Questo documento crittografato è persistente nell'archiviazione a lungo termine tramite il IXmlRepository. (Se non è configurato alcun IXmlEncryptor, il documento non crittografato è persistente nel IXmlRepository.)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

   ![Creazione della chiave](key-management/_static/keycreation1.png)

   *Chiave creazione / CreateNewKey*

Nell'implementazione di CreateNewKey, il componente IAuthenticatedEncryptorConfiguration viene utilizzato per creare un IAuthenticatedEncryptorDescriptor univoco, che viene quindi serializzato come XML. Se un sink di deposito delle chiavi è presente, il XML non elaborato (non crittografato) viene fornita al sink per l'archiviazione a lungo termine. Il codice XML non crittografato viene eseguito tramite un IXmlEncryptor (se richiesto) per generare il documento XML crittografato. Questo documento crittografato è persistente nell'archiviazione a lungo termine tramite il IXmlRepository. (Se non è configurato alcun IXmlEncryptor, il documento non crittografato è persistente nel IXmlRepository.)

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

   ![Recupero della chiave](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

   ![Recupero della chiave](key-management/_static/keyretrieval1.png)

---

   *Chiave di recupero / GetAllKeys*

Nell'implementazione di GetAllKeys, i documenti XML che rappresenta le chiavi e le decisioni vengono letti dal IXmlRepository sottostante. Se questi documenti sono crittografati, il sistema verrà automaticamente decrittografarli. XmlKeyManager crea le istanze di IAuthenticatedEncryptorDescriptorDeserializer appropriate per deserializzare i documenti in istanze IAuthenticatedEncryptorDescriptor, che vengono quindi eseguito il wrapping in singole istanze IKey. Questa raccolta di istanze IKey viene restituita al chiamante.

Per altre informazioni sugli elementi XML specifici, vedere il [documento di formato di archiviazione chiavi](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).

## <a name="ixmlrepository"></a>IXmlRepository

L'interfaccia IXmlRepository rappresenta un tipo che può rendere persistenti dati XML per recuperare i dati XML da un archivio di backup. Espone due API:

* GetAllElements(): IReadOnlyCollection<XElement>

* StoreElement (elemento XElement, friendlyName stringa)

Le implementazioni di IXmlRepository necessario analizzare il codice XML che li attraversano. Devono considerare i documenti XML come opaco e consentire a livelli più elevati preoccuparsi di generazione e l'analisi dei documenti.

Esistono due tipi concreti predefiniti che implementano IXmlRepository: FileSystemXmlRepository e RegistryXmlRepository. Vedere il [documento i provider di archiviazione chiavi](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) per ulteriori informazioni. Registrazione un IXmlRepository personalizzato potrebbe essere il modo appropriato da utilizzare un altro archivio di backup, ad esempio, archiviazione Blob di Azure. Per modificare il livello di applicazione predefinito repository, registrare un singleton personalizzato IXmlRepository il provider del servizio.

<a name=data-protection-extensibility-key-management-ixmlencryptor></a>

## <a name="ixmlencryptor"></a>IXmlEncryptor

L'interfaccia IXmlEncryptor rappresenta un tipo che è possibile crittografare un elemento XML di testo normale. Espone una singola API:

* Crittografare (plaintextElement XElement): EncryptedXmlInfo

Se un IAuthenticatedEncryptorDescriptor serializzato contiene tutti gli elementi contrassegnati come "richiede la crittografia", quindi XmlKeyManager verranno eseguiti gli elementi tramite il metodo di crittografia del IXmlEncryptor configurato e verrà mantenuti anziché l'elemento crittografato rispetto all'elemento di testo normale per il IXmlRepository. L'output del metodo di crittografia è un oggetto EncryptedXmlInfo. Questo oggetto è un wrapper che contiene il XElement crittografato risultante sia il tipo che rappresenta un IXmlDecryptor che può essere usato per decrittografare l'elemento corrispondente.

Sono disponibili quattro tipi concreti predefiniti che implementano IXmlEncryptor: CertificateXmlEncryptor, DpapiNGXmlEncryptor, DpapiXmlEncryptor e NullXmlEncryptor. Vedere il [crittografia chiave documento rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) per ulteriori informazioni. Per modificare il meccanismo di crittografia chiave al resto predefinito a livello di applicazione, registrare un singleton personalizzato IXmlEncryptor il provider del servizio.

## <a name="ixmldecryptor"></a>IXmlDecryptor

L'interfaccia IXmlDecryptor rappresenta un tipo in grado di decrittografare un XElement che è stato crittografato mediante un IXmlEncryptor. Espone una singola API:

* Decrittografare (encryptedElement XElement): XElement

Il metodo di decrittografia Annulla la crittografia eseguita da IXmlEncryptor.Encrypt. In genere, ogni implementazione concreta di IXmlEncryptor avrà un'implementazione concreta di IXmlDecryptor corrispondente.

I tipi che implementano IXmlDecryptor devono avere uno dei due costruttori pubblici seguenti:

* .ctor(IServiceProvider)

* . ctor)

> [!NOTE]
> L'IServiceProvider passato al costruttore può essere null.

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

L'interfaccia IKeyEscrowSink rappresenta un tipo che può eseguire deposito delle informazioni riservate. Tieni presente che i descrittori serializzati potrebbero contenere informazioni riservate (ad esempio, il materiale di crittografia) e questo è ciò che ha portato all'introduzione del [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) digitare in primo luogo. Tuttavia, il verificarsi di incidenti e keyrings può essere eliminato o danneggiato.

L'interfaccia deposito fornisce un'emergenza stessa, che consente l'accesso al codice XML serializzato non elaborati prima che venga trasformata da qualsiasi configurato [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor). L'interfaccia espone una singola API:

* Archivio (keyId Guid, elemento XElement)

In questo caso, l'implementazione di IKeyEscrowSink per gestire l'elemento specificato in modo sicuro coerenza con i criteri di business. Una possibile implementazione potrebbe essere usato per il sink deposito crittografare l'elemento XML usando un certificato x. 509 azienda noto in cui la chiave privata del certificato è stata depositata; il tipo CertificateXmlEncryptor assiste con questo oggetto. L'implementazione di IKeyEscrowSink è inoltre responsabile per la persistenza in modo appropriato l'elemento specificato.

Per impostazione predefinita non è abilitato alcun meccanismo deposito, anche se gli amministratori di server possono [questa configurazione globale](../configuration/machine-wide-policy.md#data-protection-configuration-machinewidepolicy). Può anche essere configurato a livello di programmazione tramite le *IDataProtectionBuilder.AddKeyEscrowSink* metodo come illustrato nell'esempio riportato di seguito. Il *AddKeyEscrowSink* mirror overload di metodo di *IServiceCollection.AddSingleton* e *IServiceCollection.AddInstance* overload, come IKeyEscrowSink le istanze devono essere singleton. Se sono registrate più istanze di IKeyEscrowSink, ciascuno di essi verrà chiamata durante la generazione di chiavi, in modo possano depositare le chiavi per diversi meccanismi contemporaneamente.

Non è disponibile alcuna API per la lettura di materiale da un'istanza di IKeyEscrowSink. Questo comportamento è coerente con la teoria della progettazione del meccanismo del deposito: è progettato per rendere accessibili a un'autorità attendibile il materiale della chiave e poiché l'applicazione non costituisce un'autorità attendibile, non deve avere accesso al proprio materiale garantite.

Esempio di codice seguente illustra la creazione e la registrazione di un IKeyEscrowSink in cui sono depositare le chiavi in modo che solo i membri di "CONTOSODomain Admins" è possono ripristinare tali componenti.

> [!NOTE]
> Per eseguire questo esempio, è necessario essere in un dominio di Windows 8 / computer con Windows Server 2012 e il controller di dominio deve essere Windows Server 2012 o versione successiva.

[!code-none[Principale](key-management/samples/key-management-extensibility.cs)]
