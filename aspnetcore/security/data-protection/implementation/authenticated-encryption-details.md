---
title: Dettagli di crittografia autenticata in ASP.NET Core
author: rick-anderson
description: Scopri i dettagli di implementazione della crittografia autenticata la protezione dei dati di ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: 9def03e6b27e19fc34a839e923d6152e086889db
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208580"
---
# <a name="authenticated-encryption-details-in-aspnet-core"></a>Dettagli di crittografia autenticata in ASP.NET Core

<a name="data-protection-implementation-authenticated-encryption-details"></a>

Le chiamate a IDataProtector.Protect sono operazioni di crittografia autenticata. Il metodo Protect offre sia la riservatezza e l'autenticità e quest'ultima è associata alla catena scopo che è stata utilizzata per derivare questa particolare istanza di oggetto IDataProtector dalla radice IDataProtectionProvider.

IDataProtector.Protect accetta un parametro di testo normale byte [] e produce un payload protetto [] byte, il cui formato è descritto di seguito. (È inoltre disponibile un overload del metodo di estensione che accetta un parametro di stringa in testo normale e restituisce un payload protetto di stringa. Se questa API viene usata il formato di payload protetti avranno comunque la seguente struttura, ma sarà [con codifica base64url](https://tools.ietf.org/html/rfc4648#section-5).)

## <a name="protected-payload-format"></a>Formato di payload protetti

Il formato di payload protetti è costituito da tre componenti principali:

* Un'intestazione magic a 32 bit che identifica la versione del sistema di protezione dati.

* Id chiave a 128 bit che identifica la chiave utilizzata per proteggere questo payload specifico.

* È il resto del payload protetto [specifiche per il componente di crittografia incapsulata da questa chiave](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation). Nell'esempio seguente, la chiave rappresenta un AES-256-CBC + encryptor HMACSHA256, e il payload viene ulteriormente suddiviso come indicato di seguito:
  * Modificatore di un chiave a 128 bit.
  * Un vettore di inizializzazione di 128 bit.
  * 48 byte di output di AES-256-CBC.
  * Un tag di autenticazione HMACSHA256.

Di seguito è illustrato un esempio di payload protetti.

```
09 F0 C9 F0 80 9C 81 0C 19 66 19 40 95 36 53 F8
AA FF EE 57 57 2F 40 4C 3F 7F CC 9D CC D9 32 3E
84 17 99 16 EC BA 1F 4A A1 18 45 1F 2D 13 7A 28
79 6B 86 9C F8 B7 84 F9 26 31 FC B1 86 0A F1 56
61 CF 14 58 D3 51 6F CF 36 50 85 82 08 2D 3F 73
5F B0 AD 9E 1A B2 AE 13 57 90 C8 F5 7C 95 4E 6A
8A AA 06 EF 43 CA 19 62 84 7C 11 B2 C8 71 9D AA
52 19 2E 5B 4C 1E 54 F0 55 BE 88 92 12 C1 4B 5E
52 C9 74 A0
```

Dal formato di payload sopra i primi 32 bit o 4 byte rappresentano l'intestazione magic che identifica la versione (09 F0 C9 F0)

I successivi 128 bit o 16 byte è l'identificatore di chiave (80 9 81 C 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)

Il resto contiene il payload e specifico del formato utilizzato.

> [!WARNING]
> Tutti i payload protetti per una determinata chiave inizia con la stessa intestazione di 20 byte (valore magic, id chiave). Gli amministratori possono utilizzare questo fatto per scopi diagnostici per approssimare quando è stato generato un payload. Ad esempio, il payload precedente corrisponde alla chiave {0c819c80-6619-4019-9536-53f8aaffee57}. Se dopo aver controllato l'archivio chiave che la data di attivazione della chiave specifico era 2015-01-01 e relativa data di scadenza cadeva 2015-03-01, quindi a diminuire è ragionevole presupporre che il payload (se non manomessi) è stato generato in tale intervallo, assegnare o eseguire un piccolo fattore di fudge su entrambi i lati.
