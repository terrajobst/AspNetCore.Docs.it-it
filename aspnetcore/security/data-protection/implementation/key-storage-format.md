---
title: Formato di archiviazione chiavi in ASP.NET Core
author: rick-anderson
description: Scopri i dettagli di implementazione del formato di archiviazione delle chiavi di protezione dei dati di ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 81df124f3dd0cadf8fd895ab55f66eec6415705f
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897478"
---
# <a name="key-storage-format-in-aspnet-core"></a>Formato di archiviazione chiavi in ASP.NET Core

<a name="data-protection-implementation-key-storage-format"></a>

Gli oggetti sono archiviati inattivi nella rappresentazione XML. La directory predefinita per l'archiviazione delle chiavi è % LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.

## <a name="the-key-element"></a>Il \<chiave > elemento

Le chiavi esistono come oggetti di primo livello nel repository di chiave. Per convenzione, le chiavi hanno il nome del file **key dal {guid}. XML**, dove {guid} è l'id della chiave. Ciascuno di questi file contiene una singola chiave. Il formato del file è come indicato di seguito.

```xml
<?xml version="1.0" encoding="utf-8"?>
<key id="80732141-ec8f-4b80-af9c-c4d2d1ff8901" version="1">
  <creationDate>2015-03-19T23:32:02.3949887Z</creationDate>
  <activationDate>2015-03-19T23:32:02.3839429Z</activationDate>
  <expirationDate>2015-06-17T23:32:02.3839429Z</expirationDate>
  <descriptor deserializerType="{deserializerType}">
    <descriptor>
      <encryption algorithm="AES_256_CBC" />
      <validation algorithm="HMACSHA256" />
      <enc:encryptedSecret decryptorType="{decryptorType}" xmlns:enc="...">
        <encryptedKey>
          <!-- This key is encrypted with Windows DPAPI. -->
          <value>AQAAANCM...8/zeP8lcwAg==</value>
        </encryptedKey>
      </enc:encryptedSecret>
    </descriptor>
  </descriptor>
</key>
```

Il \<chiave > elemento contiene elementi figlio e gli attributi seguenti:

* Id della chiave. Questo valore viene trattato come autorevoli; il nome del file è semplicemente un nicety leggibile.

* La versione del \<chiave > elemento, attualmente è fissato a 1.

* Date di creazione, l'attivazione e la scadenza della chiave.

* Oggetto \<descrittore > elemento che contiene le informazioni in base all'implementazione di crittografia autenticata contenuta all'interno di questa chiave.

Nell'esempio precedente, l'id della chiave è {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, è stato creato e attivato 19 marzo 2015, e ha una durata di 90 giorni. (In alcuni casi la data di attivazione può essere leggermente prima della data di creazione come in questo esempio. Ciò è dovuto un nit in modalità di lavoro e non provoca problemi nella pratica.)

## <a name="the-descriptor-element"></a>Il \<descrittore > elemento

L'outer \<descrittore > elemento contiene un deserializerType attributo, ovvero il nome qualificato dall'assembly di un tipo che implementa IAuthenticatedEncryptorDescriptorDeserializer. Questo tipo è responsabile della lettura interna \<descrittore > elemento e analizzare le informazioni contenute all'interno.

Il formato particolare del \<descrittore > elemento dipende dall'implementazione del componente di crittografia autenticata incapsulato dalla chiave e ogni tipo deserializzatore prevede un formato leggermente diverso per questo oggetto. In generale, tuttavia, questo elemento conterrà informazioni algoritmiche (i nomi di tipi, OID, o simile) e il materiale della chiave privato. Nell'esempio precedente, il descrittore specifica che questa chiave esegue il wrapping di crittografia AES-256-CBC + convalida HMACSHA256.

## <a name="the-encryptedsecret-element"></a>Il \<encryptedSecret > elemento

Un' **&lt;encryptedSecret&gt;** elemento che contiene la forma crittografata del materiale della chiave privata possono essere presente se [è abilitata la crittografia dei segreti a riposo](xref:security/data-protection/implementation/key-encryption-at-rest). L'attributo `decryptorType` è il nome qualificato dall'assembly di un tipo che implementa [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor). Questo tipo è responsabile della lettura interna **&lt;encryptedKey&gt;** elemento e ne esegue la decrittografia per recuperare il testo non crittografato originale.

Come per gli `<descriptor>`, il formato particolare del `<encryptedSecret>` elemento dipende dal meccanismo di crittografia dei dati inattivi in uso. Nell'esempio precedente, la chiave master viene crittografata usando DPAPI di Windows per ogni commento.

## <a name="the-revocation-element"></a>Il \<revoca > elemento

Revoca esista come oggetti di primo livello nel repository di chiave. Per convenzione revoche hanno il nome del file **revoca-{timestamp}. XML** (per la revoca tutte le chiavi entro una determinata data) o **revoca-{guid}. XML** (per la revoca di una chiave specifica). Ogni file contiene un singolo \<revoca > elemento.

Per la revoca delle chiavi singole, il contenuto del file saranno come indicato di seguito.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

In questo caso, viene revocata solo la chiave specificata. Se l'id chiave è "*", tuttavia, come nell'esempio seguente vengono revocate tutte le chiavi la cui data di creazione viene prima della data di revoche di certificati specificato.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

Il \<motivo > elemento non viene mai letto dal sistema. È semplicemente un modo pratico per archiviare un motivo leggibile dall'utente per la revoca.
