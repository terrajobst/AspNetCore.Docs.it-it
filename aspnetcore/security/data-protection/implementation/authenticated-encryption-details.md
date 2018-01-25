---
title: Dettagli di crittografia autenticata
author: rick-anderson
description: Questo strutture documento i dettagli di implementazione della protezione dei dati di ASP.NET Core autenticato di crittografia.
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: 2a59a7531c946927b4b266d5dfa9af46afdae411
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="authenticated-encryption-details"></a>Dettagli di crittografia autenticata

<a name="data-protection-implementation-authenticated-encryption-details"></a>

Le chiamate a IDataProtector.Protect sono le operazioni di crittografia autenticata. Il metodo Protect offre riservatezza e autenticità e quest'ultima è associata alla catena scopo utilizzato per derivare questa particolare istanza di oggetto IDataProtector IDataProtectionProvider radice.

IDataProtector.Protect accetta un parametro di testo normale byte [] e produce un byte [] protetto del payload, il cui formato è descritto di seguito. (È inoltre disponibile un overload del metodo di estensione che accetta un parametro di stringa in testo normale e restituisce un payload di stringa protetta. Se questa API viene usata il formato di payload protetto avranno comunque il seguito struttura, ma sarà [con codifica base64url](https://tools.ietf.org/html/rfc4648#section-5).)

## <a name="protected-payload-format"></a>Formato di payload protetto

Il formato di payload protetto è costituita da tre componenti principali:

* Intestazione magic a 32 bit che identifica la versione del sistema di protezione dati.

* Id chiave a 128 bit che identifica la chiave utilizzata per proteggere il payload specifico.

* Il resto del payload protetto è [specifiche per il componente di crittografia incapsulata da questa chiave](subkeyderivation.md#data-protection-implementation-subkey-derivation). Nell'esempio seguente la chiave rappresenta un AES-256-CBC + HMACSHA256 simmetrica e il payload è ulteriormente suddivisi come segue: * il modificatore di chiave A 128 bit. * Un vettore di inizializzazione di 128 bit. * 48 byte di output AES-256-CBC. * Un tag di autenticazione HMACSHA256.

Un payload protetto di esempio è illustrato di seguito.

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

Il formato di payload sopra i primi 32 bit o 4 byte sono l'intestazione di chiave che identifica la versione (09 F0 C9 F0)

La successiva 128 bit o 16 byte è l'identificatore di chiave (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)

Il resto contiene il payload e specifico il formato utilizzato.

>[!WARNING]
> Tutti i payload protetti per una determinata chiave inizia con la stessa intestazione a 20 byte (valore chiave, chiave id). Gli amministratori possono utilizzare questo fatto per motivi di diagnostica per simulare quando è stato generato un payload. Ad esempio, il payload precedente corrisponde alla chiave {0c819c80-6619-4019-9536-53f8aaffee57}. Se dopo aver controllato l'archivio chiave che la data di attivazione della chiave specifico è stato 2015-01-01 e la data di scadenza è stata 2015-03-01, quindi è ragionevole presupporre che il payload (se non manomessi) è stato generato all'interno di tale finestra, fornire o richiedere una piccola per indurre su entrambi i lati.
