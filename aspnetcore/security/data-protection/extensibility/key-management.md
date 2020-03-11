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
# <a name="key-management-extensibility-in-aspnet-core"></a>Estendibilità della gestione delle chiavi in ASP.NET Core

> [!TIP]
> Leggere la sezione relativa alla [gestione delle chiavi](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) prima di leggere questa sezione, in quanto illustra alcuni dei concetti fondamentali alla base di queste API.

> [!WARNING]
> I tipi che implementano una delle interfacce seguenti devono essere thread-safe per i chiamanti di più.

## <a name="key"></a>Chiave

L'interfaccia `IKey` è la rappresentazione di base di una chiave in cryptosystem. La chiave di termine viene usata in senso astratta, non nel senso letterale di "materiale della chiave crittografica". Una chiave presenta le proprietà seguenti:

* Date di attivazione, la creazione e la scadenza

* Stato di revoca

* Identificatore di chiave (GUID)

::: moniker range=">= aspnetcore-2.0"

Inoltre, `IKey` espone un metodo `CreateEncryptor` che può essere usato per creare un'istanza di [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) associata a questa chiave.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Inoltre, `IKey` espone un metodo `CreateEncryptorInstance` che può essere usato per creare un'istanza di [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) associata a questa chiave.

::: moniker-end

> [!NOTE]
> Non è disponibile alcuna API per recuperare il materiale di crittografia non elaborato da un'istanza di `IKey`.

## <a name="ikeymanager"></a>IKeyManager

L'interfaccia `IKeyManager` rappresenta un oggetto responsabile dell'archiviazione della chiave generale, del recupero e della manipolazione. Che espone tre operazioni generali:

* Creare una nuova chiave e renderlo persistente nell'archiviazione.

* Ottiene tutte le chiavi di archiviazione.

* Revoca le chiavi di uno o più e rendere persistenti le informazioni di revoca all'archiviazione.

>[!WARNING]
> La scrittura di un `IKeyManager` è un'attività molto avanzata e la maggior parte degli sviluppatori non dovrebbe tentarla. Al contrario, la maggior parte degli sviluppatori dovrebbe sfruttare le funzionalità offerte dalla classe [XmlKeyManager](#xmlkeymanager) .

## <a name="xmlkeymanager"></a>XmlKeyManager

Il tipo di `XmlKeyManager` è l'implementazione concreta di `IKeyManager`. Fornisce diverse funzionalità utili, inclusa deposito delle chiave e la crittografia delle chiavi a riposo. Le chiavi in questo sistema sono rappresentate come elementi XML (in particolare, [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).

`XmlKeyManager` dipende da diversi altri componenti durante il completamento delle attività:

::: moniker range=">= aspnetcore-2.0"

* `AlgorithmConfiguration`, che determina gli algoritmi usati dalle nuove chiavi.

* `IXmlRepository`, che controlla il punto in cui le chiavi vengono salvate in modo permanente nell'archiviazione.

* `IXmlEncryptor` [optional], che consente di crittografare le chiavi inattive.

* `IKeyEscrowSink` [facoltativo], che fornisce i servizi di deposito chiavi.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* `IXmlRepository`, che controlla il punto in cui le chiavi vengono salvate in modo permanente nell'archiviazione.

* `IXmlEncryptor` [optional], che consente di crittografare le chiavi inattive.

* `IKeyEscrowSink` [facoltativo], che fornisce i servizi di deposito chiavi.

::: moniker-end

Di seguito sono riportati i diagrammi di alto livello che indicano il modo in cui questi componenti sono collegati tra `XmlKeyManager`.

::: moniker range=">= aspnetcore-2.0"

![Creazione della chiave](key-management/_static/keycreation2.png)

*Creazione della chiave/CreateNewKey*

Nell'implementazione di `CreateNewKey`, il componente `AlgorithmConfiguration` viene utilizzato per creare una `IAuthenticatedEncryptorDescriptor`univoca, che viene quindi serializzata come XML. Se è presente un sink di deposito delle chiavi, il XML non elaborato (non crittografato) viene fornito per il sink per l'archiviazione a lungo termine. Il codice XML non crittografato viene quindi eseguito tramite un `IXmlEncryptor` (se necessario) per generare il documento XML crittografato. Questo documento crittografato viene salvato in modo permanente nell'archiviazione a lungo termine tramite il `IXmlRepository`. Se non è stato configurato alcun `IXmlEncryptor`, il documento non crittografato viene salvato in modo permanente nel `IXmlRepository`.

![Recupero della chiave](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![Creazione della chiave](key-management/_static/keycreation1.png)

*Creazione della chiave/CreateNewKey*

Nell'implementazione di `CreateNewKey`, il componente `IAuthenticatedEncryptorConfiguration` viene utilizzato per creare una `IAuthenticatedEncryptorDescriptor`univoca, che viene quindi serializzata come XML. Se è presente un sink di deposito delle chiavi, il XML non elaborato (non crittografato) viene fornito per il sink per l'archiviazione a lungo termine. Il codice XML non crittografato viene quindi eseguito tramite un `IXmlEncryptor` (se necessario) per generare il documento XML crittografato. Questo documento crittografato viene salvato in modo permanente nell'archiviazione a lungo termine tramite il `IXmlRepository`. Se non è stato configurato alcun `IXmlEncryptor`, il documento non crittografato viene salvato in modo permanente nel `IXmlRepository`.

![Recupero della chiave](key-management/_static/keyretrieval1.png)

::: moniker-end

*Recupero chiavi/GetAllKeys*

Nell'implementazione di `GetAllKeys`, i documenti XML che rappresentano chiavi e revoche vengono letti dall'`IXmlRepository`sottostante. Se questi documenti sono crittografati, il sistema verrà automaticamente decrittografarle. `XmlKeyManager` crea le istanze di `IAuthenticatedEncryptorDescriptorDeserializer` appropriate per deserializzare i documenti in `IAuthenticatedEncryptorDescriptor` istanze, che vengono quindi sottoposte a incapsulamento in singole istanze di `IKey`. Questa raccolta di istanze di `IKey` viene restituita al chiamante.

Altre informazioni sui particolari elementi XML sono reperibili nel [documento formato archiviazione chiavi](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).

## <a name="ixmlrepository"></a>IXmlRepository

L'interfaccia `IXmlRepository` rappresenta un tipo che può salvare in modo permanente XML e recuperare XML da un archivio di backup. Che espone due API:

* `GetAllElements`:`IReadOnlyCollection<XElement>`

* `StoreElement(XElement element, string friendlyName)`

Non è necessario che le implementazioni di `IXmlRepository` analizzino il codice XML passato. Devono considerare i documenti XML come opaco e consentire ai livelli superiori di preoccuparsi di generazione e l'analisi dei documenti.

Sono disponibili quattro tipi concreti incorporati che implementano `IXmlRepository`:

::: moniker range=">= aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage. AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage. AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

Per ulteriori informazioni, vedere il documento relativo ai [provider di archiviazione chiavi](xref:security/data-protection/implementation/key-storage-providers) .

La registrazione di un `IXmlRepository` personalizzato è appropriata quando si usa un archivio di backup diverso (ad esempio, archiviazione tabelle di Azure).

Per modificare il repository predefinito a livello di applicazione, registrare un'istanza di `IXmlRepository` personalizzata:

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

## <a name="ixmlencryptor"></a>IXmlEncryptor

L'interfaccia `IXmlEncryptor` rappresenta un tipo in grado di crittografare un elemento XML non crittografato. Che espone una singola API:

* Crittografare (XElement plaintextElement): EncryptedXmlInfo

Se un `IAuthenticatedEncryptorDescriptor` serializzato contiene elementi contrassegnati come "requires Encryption", `XmlKeyManager` eseguiranno tali elementi tramite il metodo `Encrypt` del `IXmlEncryptor`configurato e manterrà l'elemento crittografato anziché l'elemento del testo non crittografato nell'`IXmlRepository`. L'output del metodo `Encrypt` è un oggetto `EncryptedXmlInfo`. Questo oggetto è un wrapper che contiene sia la `XElement` crittografata risultante sia il tipo che rappresenta una `IXmlDecryptor` che può essere utilizzata per decrittografare l'elemento corrispondente.

Sono disponibili quattro tipi concreti incorporati che implementano `IXmlEncryptor`:

* [CertificateXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [DpapiNGXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [DpapiXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [NullXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

Per ulteriori informazioni, vedere il [documento chiave di crittografia](xref:security/data-protection/implementation/key-encryption-at-rest) dei dati inattivi.

Per modificare il meccanismo predefinito a livello di applicazione per la crittografia delle chiavi, registrare un'istanza di `IXmlEncryptor` personalizzata:

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

## <a name="ixmldecryptor"></a>IXmlDecryptor

L'interfaccia `IXmlDecryptor` rappresenta un tipo che sa come decrittografare una `XElement` che è stata crittografata tramite un `IXmlEncryptor`. Che espone una singola API:

* Decrittografare (XElement encryptedElement): XElement

Il metodo `Decrypt` Annulla la crittografia eseguita da `IXmlEncryptor.Encrypt`. In genere, ogni implementazione di `IXmlEncryptor` concreta avrà un'implementazione `IXmlDecryptor` concreta corrispondente.

I tipi che implementano `IXmlDecryptor` devono avere uno dei due costruttori pubblici seguenti:

* .ctor(IServiceProvider)
* ctor)

> [!NOTE]
> Il `IServiceProvider` passato al costruttore può essere null.

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

L'interfaccia `IKeyEscrowSink` rappresenta un tipo in grado di eseguire il deposito di informazioni riservate. Si ricordi che i descrittori serializzati potrebbero contenere informazioni riservate, ad esempio il materiale crittografico, ed è ciò che ha portato all'introduzione del tipo [IXmlEncryptor](#ixmlencryptor) . Tuttavia, succedere e chiave di anelli può essere eliminati o danneggiati.

L'interfaccia Escrow fornisce un tratteggio di escape di emergenza, che consente l'accesso al codice XML serializzato non elaborato prima che venga trasformato da qualsiasi [IXmlEncryptor](#ixmlencryptor)configurato. L'interfaccia espone una singola API:

* Store (keyId Guid, elemento XElement)

L'implementazione di `IKeyEscrowSink` per gestire l'elemento fornito in modo sicuro è coerente con i criteri di business. Una possibile implementazione può essere per il sink del deposito a garanzia per crittografare l'elemento XML usando un certificato X. 509 aziendale noto in cui è stata depositata la chiave privata del certificato. il tipo di `CertificateXmlEncryptor` può risultare utile. L'implementazione del `IKeyEscrowSink` è anche responsabile del salvataggio permanente dell'elemento fornito in modo appropriato.

Per impostazione predefinita, non è abilitato alcun meccanismo di deposito, sebbene gli amministratori del server possano [configurarlo a livello globale](xref:security/data-protection/configuration/machine-wide-policy). Può anche essere configurato a livello di programmazione tramite il metodo `IDataProtectionBuilder.AddKeyEscrowSink` come illustrato nell'esempio riportato di seguito. Gli overload del metodo `AddKeyEscrowSink` rispecchiano gli overload `IServiceCollection.AddSingleton` e `IServiceCollection.AddInstance`, perché le istanze di `IKeyEscrowSink` sono destinate a essere singleton. Se vengono registrate più istanze di `IKeyEscrowSink`, ciascuna di esse verrà chiamata durante la generazione della chiave, quindi le chiavi possono essere estratta simultaneamente a più meccanismi.

Non è disponibile alcuna API per leggere il materiale da un'istanza di `IKeyEscrowSink`. Questo comportamento è coerente con la teoria della progettazione del meccanismo di deposito delle chiavi: è destinato a rendere accessibili a un'autorità attendibile il materiale della chiave, e poiché l'applicazione non costituisce un'autorità attendibile, non deve avere accesso al proprio materiale garantite.

Il codice di esempio seguente illustra la creazione e la registrazione di un `IKeyEscrowSink` in cui vengono depositate le chiavi in modo che solo i membri di "CONTOSODomain Admins" possano ripristinarli.

> [!NOTE]
> Per eseguire questo esempio, è necessario essere in un dominio Windows 8 / computer con Windows Server 2012 e il controller di dominio deve essere Windows Server 2012 o versione successiva.

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
