---
title: Estendibilità di gestione delle chiavi in ASP.NET Core
author: rick-anderson
description: Informazioni sulle estendibilità di gestione delle chiavi di protezione dei dati di ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 11/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: e3042b371cf7be8fa0218c1906042d2810b180e3
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="key-management-extensibility-in-aspnet-core"></a>Estendibilità di gestione delle chiavi in ASP.NET Core

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> Lettura di [gestione delle chiavi](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) sezione prima di leggere questa sezione, come vengono illustrati alcuni dei concetti di queste API.

>[!WARNING]
> Tipi che implementano le interfacce seguenti devono essere thread-safe per più i chiamanti.

## <a name="key"></a>Chiave

Il `IKey` interfaccia è la rappresentazione di base di una chiave nel sistema di crittografia. La chiave di termine viene usata in senso astratta, non in senso letterale "crittografia materiale della chiave". Una chiave ha le proprietà seguenti:

* Date di scadenza, la creazione e attivazione

* Stato di revoca

* Identificatore di chiave (GUID)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Inoltre, `IKey` espone un `CreateEncryptor` metodo che può essere utilizzato per creare un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) istanza associato a questa chiave.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Inoltre, `IKey` espone un `CreateEncryptorInstance` metodo che può essere utilizzato per creare un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) istanza associato a questa chiave.

---

> [!NOTE]
> Non è disponibile alcuna API per recuperare il materiale crittografico non elaborato da un `IKey` istanza.

## <a name="ikeymanager"></a>IKeyManager

Il `IKeyManager` interfaccia rappresenta un oggetto responsabile dell'archiviazione delle chiavi generale, il recupero e la modifica. Espone tre operazioni di alto livello:

* Creare una nuova chiave e archiviarlo in un archivio.

* Ottenere tutte le chiavi dall'archiviazione.

* Revocare una o più chiavi e rendere persistenti le informazioni di revoca all'archiviazione.

>[!WARNING]
> Scrittura di un `IKeyManager` un'attività molto avanzata e la maggior parte degli sviluppatori non deve tentare. Al contrario, la maggior parte degli sviluppatori devono sfruttare le funzionalità offerte dal [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) classe.

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a>XmlKeyManager

Il `XmlKeyManager` tipo è un'implementazione concreta nella casella della `IKeyManager`. Fornisce diverse funzionalità utili, inclusi deposito delle chiave e la crittografia delle chiavi inattivi. Le chiavi nel sistema sono rappresentate come elementi XML (in particolare, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).

`XmlKeyManager` dipende da numerosi altri componenti nel corso di completare le attività:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* `AlgorithmConfiguration`, che indicano gli algoritmi utilizzati dai nuove chiavi.

* `IXmlRepository`, che controlla in cui le chiavi vengono rese persistenti nell'archiviazione.

* `IXmlEncryptor` [facoltativo], che consente di crittografare le chiavi inattivi.

* `IKeyEscrowSink` [facoltativo], che fornisce servizi di deposito delle chiavi.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* `IXmlRepository`, che controlla in cui le chiavi vengono rese persistenti nell'archiviazione.

* `IXmlEncryptor` [facoltativo], che consente di crittografare le chiavi inattivi.

* `IKeyEscrowSink` [facoltativo], che fornisce servizi di deposito delle chiavi.

---

Di seguito sono diagrammi di livello superiore che indicano come questi componenti vengono collegati tra loro in `XmlKeyManager`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![Creazione della chiave](key-management/_static/keycreation2.png)

   *Chiave creazione / CreateNewKey*

Nell'implementazione di `CreateNewKey`, `AlgorithmConfiguration` componente viene utilizzato per creare un nome `IAuthenticatedEncryptorDescriptor`, che viene quindi serializzato come XML. Se un sink di deposito delle chiavi è presente, il XML non elaborato (non crittografato) viene fornita al sink per l'archiviazione a lungo termine. Il codice XML non crittografato viene quindi eseguito tramite un `IXmlEncryptor` (se richiesto) per generare il documento XML crittografato. Questo documento crittografato viene reso persistente per archiviazione a lungo termine tramite il `IXmlRepository`. (Se non `IXmlEncryptor` è configurato, il documento non crittografato è persistente nel `IXmlRepository`.)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![Creazione della chiave](key-management/_static/keycreation1.png)

   *Chiave creazione / CreateNewKey*

Nell'implementazione di `CreateNewKey`, `IAuthenticatedEncryptorConfiguration` componente viene utilizzato per creare un nome `IAuthenticatedEncryptorDescriptor`, che viene quindi serializzato come XML. Se un sink di deposito delle chiavi è presente, il XML non elaborato (non crittografato) viene fornita al sink per l'archiviazione a lungo termine. Il codice XML non crittografato viene quindi eseguito tramite un `IXmlEncryptor` (se richiesto) per generare il documento XML crittografato. Questo documento crittografato viene reso persistente per archiviazione a lungo termine tramite il `IXmlRepository`. (Se non `IXmlEncryptor` è configurato, il documento non crittografato è persistente nel `IXmlRepository`.)

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![Recupero della chiave](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![Recupero della chiave](key-management/_static/keyretrieval1.png)

---

   *Chiave di recupero / GetAllKeys*

Nell'implementazione di `GetAllKeys`, le chiavi che rappresenta i documenti XML e revoca è leggervi sottostante `IXmlRepository`. Se questi documenti sono crittografati, il sistema verrà automaticamente decrittografarli. `XmlKeyManager` Crea l'oggetto appropriato `IAuthenticatedEncryptorDescriptorDeserializer` nuovamente le istanze per deserializzare i documenti in `IAuthenticatedEncryptorDescriptor` istanze, che vengono quindi eseguito il wrapping in singoli `IKey` istanze. Questa raccolta di `IKey` istanze viene restituito al chiamante.

Per altre informazioni sugli elementi XML specifici, vedere il [documento di formato di archiviazione chiavi](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).

## <a name="ixmlrepository"></a>IXmlRepository

Il `IXmlRepository` interfaccia rappresenta un tipo che può rendere persistenti dati XML per recuperare i dati XML da un archivio di backup. Espone due API:

* GetAllElements(): IReadOnlyCollection<XElement>

* StoreElement (elemento XElement, friendlyName stringa)

Le implementazioni di `IXmlRepository` non è necessario analizzare il codice XML che li attraversano. Devono considerare i documenti XML come opaco e consentire a livelli più elevati preoccuparsi di generazione e l'analisi dei documenti.

Esistono due tipi concreti predefiniti che implementano `IXmlRepository`: `FileSystemXmlRepository` e `RegistryXmlRepository`. Vedere il [documento i provider di archiviazione chiavi](xref:security/data-protection/implementation/key-storage-providers#data-protection-implementation-key-storage-providers) per ulteriori informazioni. Registrazione di un oggetto personalizzato `IXmlRepository` sarebbe il modo appropriato da utilizzare un archivio di backup diversi, ad esempio, archiviazione Blob di Azure.

Per modificare il repository predefinito a livello di applicazione, registrare un oggetto personalizzato `IXmlRepository` istanza:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a>IXmlEncryptor

Il `IXmlEncryptor` interfaccia rappresenta un tipo che è possibile crittografare un elemento XML di testo normale. Espone una singola API:

* Crittografare (plaintextElement XElement): EncryptedXmlInfo

Se serializzata `IAuthenticatedEncryptorDescriptor` conterrà tutti gli elementi contrassegnati come "richiede la crittografia", quindi `XmlKeyManager` eseguirà tali elementi tramite l'applicazione configurata `IXmlEncryptor`del `Encrypt` metodo e manterrà l'elemento crittografato invece di elemento di testo normale per il `IXmlRepository`. L'output del `Encrypt` metodo è un `EncryptedXmlInfo` oggetto. Questo oggetto è un wrapper che contiene entrambi i risultanti crittografato `XElement` e il tipo che rappresenta un `IXmlDecryptor` che può essere usato per decrittografare l'elemento corrispondente.

Sono disponibili quattro tipi concreti predefiniti che implementano `IXmlEncryptor`:
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

Vedere il [crittografia chiave documento rest](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) per ulteriori informazioni.

Per modificare il meccanismo di crittografia chiave al resto predefinito a livello di applicazione, registrare un oggetto personalizzato `IXmlEncryptor` istanza:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

## <a name="ixmldecryptor"></a>IXmlDecryptor

Il `IXmlDecryptor` interfaccia rappresenta un tipo in grado di decrittografare un `XElement` che è stato crittografato mediante un `IXmlEncryptor`. Espone una singola API:

* Decrittografare (encryptedElement XElement): XElement

Il `Decrypt` metodo annulla la crittografia eseguita da `IXmlEncryptor.Encrypt`. In genere, ogni concreto `IXmlEncryptor` implementazione avrà una corrispondente concreta `IXmlDecryptor` implementazione.

I tipi che implementano `IXmlDecryptor` deve avere uno dei due costruttori pubblici seguenti:

* .ctor(IServiceProvider)
* .ctor()

> [!NOTE]
> Il `IServiceProvider` passato al costruttore può essere null.

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

Il `IKeyEscrowSink` interfaccia rappresenta un tipo che può eseguire deposito delle informazioni riservate. Tieni presente che i descrittori serializzati potrebbero contenere informazioni riservate (ad esempio, il materiale di crittografia) e questo è ciò che ha portato all'introduzione del [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) digitare in primo luogo. Tuttavia, che degli incidenti verificarsi e anelli chiave può essere eliminati o danneggiati.

L'interfaccia deposito fornisce un'emergenza stessa, che consente l'accesso al codice XML serializzato non elaborati prima che venga trasformata da qualsiasi configurato [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor). L'interfaccia espone una singola API:

* Archivio (keyId Guid, elemento XElement)

È il `IKeyEscrowSink` implementazione per gestire l'elemento specificato in modo sicuro coerenza con i criteri di business. Una possibile implementazione potrebbe essere usato per il sink deposito crittografare l'elemento XML usando un certificato x. 509 azienda noto in cui la chiave privata del certificato è stata depositata; il `CertificateXmlEncryptor` possono essere utili in questo tipo. Il `IKeyEscrowSink` implementazione è inoltre responsabile per la persistenza in modo appropriato l'elemento specificato.

Per impostazione predefinita non è abilitato alcun meccanismo deposito, anche se gli amministratori di server possono [questa configurazione globale](xref:security/data-protection/configuration/machine-wide-policy). Può anche essere configurato a livello di programmazione tramite le `IDataProtectionBuilder.AddKeyEscrowSink` metodo come illustrato nell'esempio riportato di seguito. Il `AddKeyEscrowSink` mirror overload di metodo di `IServiceCollection.AddSingleton` e `IServiceCollection.AddInstance` overload, come `IKeyEscrowSink` le istanze devono essere singleton. Se più `IKeyEscrowSink` registrate le istanze, verrà chiamato durante la generazione di chiavi, ognuna in modo possano depositare le chiavi per diversi meccanismi contemporaneamente.

Non è disponibile alcuna API per leggere materiale proveniente da un `IKeyEscrowSink` istanza. Questo comportamento è coerente con la teoria della progettazione del meccanismo del deposito: è progettato per rendere accessibili a un'autorità attendibile il materiale della chiave e poiché l'applicazione non costituisce un'autorità attendibile, non deve avere accesso al proprio materiale garantite.

Esempio di codice seguente illustra la creazione e la registrazione di un `IKeyEscrowSink` in cui sono depositare le chiavi in modo che solo i membri di "CONTOSODomain Admins" è possono ripristinare tali componenti.

> [!NOTE]
> Per eseguire questo esempio, è necessario essere in un dominio di Windows 8 / computer con Windows Server 2012 e il controller di dominio deve essere Windows Server 2012 o versione successiva.

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
