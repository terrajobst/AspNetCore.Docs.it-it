---
title: Estendibilità della gestione delle chiavi in ASP.NET Core
author: rick-anderson
description: Informazioni sulle estendibilità di gestione delle chiavi di protezione dei dati di ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 1cf3fc30f72fb872ff9d7f33fc5ffb12a11a982f
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090615"
---
# <a name="key-management-extensibility-in-aspnet-core"></a>Estendibilità della gestione delle chiavi in ASP.NET Core

> [!TIP]
> Leggere il [gestione delle chiavi](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) sezione prima di leggere questa sezione, perché vengono descritti alcuni dei concetti fondamentali alla base di queste API.

> [!WARNING]
> I tipi che implementano una delle interfacce seguenti devono essere thread-safe per i chiamanti di più.

## <a name="key"></a>Chiave

Il `IKey` interfaccia è la rappresentazione di base di una chiave nel sistema di crittografia. La chiave di termine viene usata in senso astratta, non nel senso letterale di "materiale della chiave crittografica". Una chiave presenta le proprietà seguenti:

* Date di attivazione, la creazione e la scadenza

* Stato di revoca

* Identificatore di chiave (GUID)

::: moniker range=">= aspnetcore-2.0"

È inoltre `IKey` espone una `CreateEncryptor` metodo che può essere usato per creare un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) istanza associate a questa chiave.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

È inoltre `IKey` espone una `CreateEncryptorInstance` metodo che può essere usato per creare un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) istanza associate a questa chiave.

::: moniker-end

> [!NOTE]
> Non è disponibile alcuna API per recuperare il materiale di crittografia non elaborato da un `IKey` istanza.

## <a name="ikeymanager"></a>IKeyManager

Il `IKeyManager` interfaccia rappresenta un oggetto responsabile dell'archiviazione delle chiavi generale, il recupero e la modifica. Che espone tre operazioni generali:

* Creare una nuova chiave e renderlo persistente nell'archiviazione.

* Ottiene tutte le chiavi di archiviazione.

* Revoca le chiavi di uno o più e rendere persistenti le informazioni di revoca all'archiviazione.

>[!WARNING]
> La scrittura di un `IKeyManager` è un'attività molto avanzata e la maggior parte degli sviluppatori non deve provare lo. Al contrario, la maggior parte degli sviluppatori consigliabile sfruttare le funzionalità offerte dal [XmlKeyManager](#xmlkeymanager) classe.

## <a name="xmlkeymanager"></a>XmlKeyManager

Il `XmlKeyManager` il tipo è l'implementazione concreta in arrivo del `IKeyManager`. Fornisce diverse funzionalità utili, inclusa deposito delle chiave e la crittografia delle chiavi a riposo. Le chiavi nel sistema sono rappresentate come elementi XML (in particolare [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).

`XmlKeyManager` dipende da diversi altri componenti nel corso di evadere la suddetta le relative attività:

::: moniker range=">= aspnetcore-2.0"

* `AlgorithmConfiguration`, che determinano gli algoritmi utilizzati dai nuovi codici.

* `IXmlRepository`, che controlla in cui le chiavi vengono rese persistenti nell'archiviazione.

* `IXmlEncryptor` [optional], che consente di crittografare le chiavi a riposo.

* `IKeyEscrowSink` [optional], che fornisce servizi di deposito delle chiavi.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* `IXmlRepository`, che controlla in cui le chiavi vengono rese persistenti nell'archiviazione.

* `IXmlEncryptor` [optional], che consente di crittografare le chiavi a riposo.

* `IKeyEscrowSink` [optional], che fornisce servizi di deposito delle chiavi.

::: moniker-end

Di seguito sono diagrammi di livello superiore che indicano come questi componenti vengono collegati tra loro all'interno di `XmlKeyManager`.

::: moniker range=">= aspnetcore-2.0"

![Creazione della chiave](key-management/_static/keycreation2.png)

*La creazione della chiave / CreateNewKey*

Nell'implementazione di `CreateNewKey`, il `AlgorithmConfiguration` componente viene usato per creare un valore univoco `IAuthenticatedEncryptorDescriptor`, che vengono quindi serializzati come XML. Se è presente un sink di deposito delle chiavi, il XML non elaborato (non crittografato) viene fornito per il sink per l'archiviazione a lungo termine. Il codice XML non crittografato viene quindi eseguito un `IXmlEncryptor` (se richiesto) per generare il documento XML crittografato. Questo documento è persistente in un archivio a lungo termine tramite il `IXmlRepository`. (Se no `IXmlEncryptor` è configurato, il documento non crittografato viene mantenuto nel `IXmlRepository`.)

![Recupero della chiave](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![Creazione della chiave](key-management/_static/keycreation1.png)

*La creazione della chiave / CreateNewKey*

Nell'implementazione di `CreateNewKey`, il `IAuthenticatedEncryptorConfiguration` componente viene usato per creare un valore univoco `IAuthenticatedEncryptorDescriptor`, che vengono quindi serializzati come XML. Se è presente un sink di deposito delle chiavi, il XML non elaborato (non crittografato) viene fornito per il sink per l'archiviazione a lungo termine. Il codice XML non crittografato viene quindi eseguito un `IXmlEncryptor` (se richiesto) per generare il documento XML crittografato. Questo documento è persistente in un archivio a lungo termine tramite il `IXmlRepository`. (Se no `IXmlEncryptor` è configurato, il documento non crittografato viene mantenuto nel `IXmlRepository`.)

![Recupero della chiave](key-management/_static/keyretrieval1.png)

::: moniker-end

*Il recupero della chiave / GetAllKeys*

Nell'implementazione di `GetAllKeys`, che rappresentano le chiavi di documenti XML e revoca viene letti da sottostante `IXmlRepository`. Se questi documenti sono crittografati, il sistema verrà automaticamente decrittografarle. `XmlKeyManager` Crea l'appropriato `IAuthenticatedEncryptorDescriptorDeserializer` allo stato le istanze per deserializzare i documenti `IAuthenticatedEncryptorDescriptor` istanze, che vengono quindi eseguito il wrapping in singoli `IKey` istanze. Questa raccolta di `IKey` istanze viene restituito al chiamante.

Altre informazioni sugli elementi XML specifici sono reperibile nel [documento di formato di archiviazione chiavi](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).

## <a name="ixmlrepository"></a>IXmlRepository

Il `IXmlRepository` interfaccia rappresenta un tipo che può rendere persistenti dati XML da e recuperare dati XML da un archivio di backup. Che espone due API:

* `GetAllElements` :`IReadOnlyCollection<XElement>`

* `StoreElement(XElement element, string friendlyName)`

Le implementazioni di `IXmlRepository` non necessario analizzare il codice XML che li attraversano. Devono considerare i documenti XML come opaco e consentire ai livelli superiori di preoccuparsi di generazione e l'analisi dei documenti.

Esistono quattro tipi concreti predefiniti che implementano `IXmlRepository`:

::: moniker range=">= aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage.AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage.AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

Vedere le [documento provider archiviazione chiavi](xref:security/data-protection/implementation/key-storage-providers) per altre informazioni.

La registrazione di un oggetto personalizzato `IXmlRepository` è appropriato quando si usa un archivio di backup (ad esempio, archiviazione tabelle di Azure).

Per modificare il repository predefinito a livello di applicazione, registrare un oggetto personalizzato `IXmlRepository` istanza:

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

Il `IXmlEncryptor` interfaccia rappresenta un tipo che può crittografare un elemento XML di testo non crittografato. Che espone una singola API:

* Crittografare (XElement plaintextElement): EncryptedXmlInfo

Se serializzato `IAuthenticatedEncryptorDescriptor` contiene tutti gli elementi contrassegnati come "richiede la crittografia", quindi `XmlKeyManager` verranno esaminate solo gli elementi dell'applicazione configurata `IXmlEncryptor`del `Encrypt` (metodo) che verranno mantenuti l'elemento cifrato anziché quella di elemento di testo non crittografato per la `IXmlRepository`. L'output del `Encrypt` metodo è un `EncryptedXmlInfo` oggetto. Questo oggetto è un wrapper che contiene entrambi i risultanti cifrati `XElement` e il tipo che rappresenta un `IXmlDecryptor` che può essere usato per decrittografare l'elemento corrispondente.

Esistono quattro tipi concreti predefiniti che implementano `IXmlEncryptor`:

* [CertificateXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [DpapiNGXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [DpapiXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [NullXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

Vedere le [chiave di crittografia al documento rest](xref:security/data-protection/implementation/key-encryption-at-rest) per altre informazioni.

Per modificare il meccanismo di key---crittografia a livello di applicazione predefinito, registrare un oggetto personalizzato `IXmlEncryptor` istanza:

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

Il `IXmlDecryptor` interfaccia rappresenta un tipo in grado di decrittografare un' `XElement` che è stata cifrati tramite un `IXmlEncryptor`. Che espone una singola API:

* Decrittografare (XElement encryptedElement): XElement

Il `Decrypt` metodo annulla la crittografia eseguita da `IXmlEncryptor.Encrypt`. In generale, ogni concreto `IXmlEncryptor` implementazione avrà un concreto corrispondente `IXmlDecryptor` implementazione.

I tipi che implementano `IXmlDecryptor` deve avere uno dei due costruttori pubblici seguenti:

* .ctor(IServiceProvider)
* ctor)

> [!NOTE]
> Il `IServiceProvider` passato al costruttore può essere null.

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

Il `IKeyEscrowSink` interfaccia rappresenta un tipo che può eseguire il deposito di informazioni riservate. È importante ricordare che i descrittori serializzati potrebbero contenere informazioni riservate (ad esempio, il materiale di crittografia) e questo è ciò che ha portato all'introduzione del [IXmlEncryptor](#ixmlencryptor) digitare in primo luogo. Tuttavia, succedere e chiave di anelli può essere eliminati o danneggiati.

L'interfaccia di deposito delle chiavi fornisce un'emergenza di emergenza, che consente l'accesso al codice XML serializzato non elaborati prima che venga trasformata da qualsiasi configurata [IXmlEncryptor](#ixmlencryptor). L'interfaccia espone una singola API:

* Store (keyId Guid, elemento XElement)

Fino a è il `IKeyEscrowSink` implementazione per gestire l'elemento specificato in modo sicuro coerente con i criteri di business. Una possibile implementazione potrebbe essere usato per il sink di deposito delle chiavi crittografare l'elemento XML usando un certificato X.509 azienda noto in cui la chiave privata del certificato è stata depositata; il `CertificateXmlEncryptor` tipo può essere usato per questo. Il `IKeyEscrowSink` implementazione è anche responsabile della persistenza l'elemento specificato in modo appropriato.

Per impostazione predefinita non è abilitato alcun meccanismo di deposito delle chiavi, anche se gli amministratori server possono [questa configurazione a livello globale](xref:security/data-protection/configuration/machine-wide-policy). Può anche essere configurato a livello di programmazione tramite le `IDataProtectionBuilder.AddKeyEscrowSink` metodo come illustrato nell'esempio riportato di seguito. Il `AddKeyEscrowSink` mirror gli overload di metodo di `IServiceCollection.AddSingleton` e `IServiceCollection.AddInstance` overload, come `IKeyEscrowSink` istanze sono progettate per essere singleton con stato. Se più `IKeyEscrowSink` registrate le istanze, ciascuna di esse verrà chiamato durante la generazione di chiavi, in modo che le chiavi possono potrà essere depositate ai meccanismi più contemporaneamente.

Non è disponibile alcuna API per leggere i materiali da un `IKeyEscrowSink` istanza. Questo comportamento è coerente con la teoria della progettazione del meccanismo di deposito delle chiavi: è destinato a rendere accessibili a un'autorità attendibile il materiale della chiave, e poiché l'applicazione non costituisce un'autorità attendibile, non deve avere accesso al proprio materiale garantite.

Esempio di codice seguente illustra la creazione e la registrazione di un `IKeyEscrowSink` in cui le chiavi sono depositate in modo che solo i membri di "CONTOSODomain Admins" possono ripristinare tali componenti.

> [!NOTE]
> Per eseguire questo esempio, è necessario essere in un dominio Windows 8 / computer con Windows Server 2012 e il controller di dominio deve essere Windows Server 2012 o versione successiva.

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
