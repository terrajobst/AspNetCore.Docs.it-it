---
title: Formato di archiviazione chiavi in ASP.NET Core
author: rick-anderson
description: Informazioni su dettagli di implementazione del formato di archiviazione delle chiavi di protezione dei dati di ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 1a5912f246708355e6677c60034d982d053c3938
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2018
---
# <a name="key-storage-format-in-aspnet-core"></a>Formato di archiviazione chiavi in ASP.NET Core

<a name="data-protection-implementation-key-storage-format"></a>

Gli oggetti vengono archiviati nel resto nella rappresentazione XML. La directory predefinita per l'archiviazione delle chiavi è % LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.

## <a name="the-key-element"></a>Il \<chiave > elemento

Chiavi esistono come oggetti di livello superiore nel repository di chiave. Per convenzione, le chiavi hanno il nome del file **chiave a {guid}. XML**, dove {guid} è l'id della chiave. Ognuno di questi file contiene una chiave singola. Il formato del file è come indicato di seguito.

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

Il \<chiave > elemento contiene i seguenti attributi e gli elementi figlio:

* Id della chiave. Questo valore viene trattato come autorevoli; il nome del file è semplicemente un nicety leggibile.

* La versione di \<chiave > elemento, attualmente è fissato a 1.

* Data di creazione, attivazione e la scadenza della chiave.

* Oggetto \<descrittore > elemento che contiene informazioni sull'implementazione di crittografia autenticata contenuto all'interno di questa chiave.

Nell'esempio precedente, l'id della chiave è {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, è stato creato e attivato 19 marzo 2015, e ha una durata di 90 giorni. (In alcuni casi la data di attivazione potrebbe essere leggermente prima della data di creazione come nel seguente esempio. Ciò è dovuto a un nit nel funzionamento le API e non provoca problemi in pratica.)

## <a name="the-descriptor-element"></a>Il \<descrittore > elemento

Esterna \<descrittore > elemento contiene un deserializerType di attributo, ovvero il nome completo dell'assembly di un tipo che implementa IAuthenticatedEncryptorDescriptorDeserializer. Questo tipo è responsabile della lettura interna \<descrittore > elemento e per l'analisi delle informazioni contenute all'interno.

Il formato specifico del \<descrittore > elemento dipende dall'implementazione del componente di crittografia autenticata incapsulata dalla chiave e ogni tipo deserializzatore prevede un formato leggermente diverso per questo oggetto. In generale, tuttavia, questo elemento conterrà informazioni algoritmiche (i nomi di tipi, gli identificatori di oggetto, o simile) e il materiale della chiave privato. Nell'esempio precedente, il descrittore specifica che questa chiave esegue il wrapping di crittografia AES-256-CBC + convalida HMACSHA256.

## <a name="the-encryptedsecret-element"></a>Il \<encryptedSecret > elemento

Un <encryptedSecret> elemento che contiene la forma crittografata del materiale della chiave privata può essere presente se [è abilitata la crittografia dei segreti inattivi](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest). L'attributo decryptorType sarà il nome completo dell'assembly di un tipo che implementa IXmlDecryptor. Questo tipo è responsabile della lettura interna <encryptedKey> elemento e la decrittografia per recuperare il testo non crittografato originale.

Come con \<descrittore >, il formato specifico del <encryptedSecret> elemento dipende dal meccanismo di crittografia a riposo in uso. Nell'esempio precedente, la chiave master viene crittografata utilizzando DPAPI di Windows per il commento.

## <a name="the-revocation-element"></a>Il \<revoca > elemento

Revoca esiste come oggetti di livello superiore nel repository di chiave. Per convenzione revoca presentano il nome del file **revoca-{timestamp}. XML** (per la revoca di tutte le chiavi prima di una data specifica) o **revoca-{guid}. XML** (per la revoca di una chiave specifica). Ogni file contiene un singolo \<revoca > elemento.

Per le decisioni di singole chiavi, il contenuto del file saranno come indicato di seguito.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

In questo caso, viene revocata solo la chiave specificata. Se l'id chiave è "*", tuttavia, come nell'esempio seguente vengono revocate tutte le chiavi la cui data di creazione è precedente alla data di revoche di certificati specificato.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

Il \<motivo > elemento non viene mai letto dal sistema. È semplicemente una posizione comoda in cui archiviare un motivo della revoca leggibile.
