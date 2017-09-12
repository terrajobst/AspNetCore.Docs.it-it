---
title: Criterio per l'intero computer
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 285ae47d-e0bf-4b03-b0a8-2b1fb18bc3a1
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: 7ada940acfbb7fb0887fd7c0cd722bf62f211248
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="machine-wide-policy"></a>Criterio per l'intero computer

<a name=data-protection-configuration-machinewidepolicy></a>

Durante l'esecuzione in Windows, il sistema di protezione dati offre supporto limitato per l'impostazione di criteri a livello di computer predefiniti per tutte le applicazioni che utilizzano la protezione dei dati. L'idea generale è che un amministratore potrebbe essere opportuno modificare alcune impostazioni predefinite (ad esempio gli algoritmi utilizzati o chiave la durata) senza la necessità di aggiornare manualmente ogni applicazione nel computer.

>[!WARNING]
> L'amministratore di sistema può impostare i criteri predefiniti, ma non è possibile applicarla. Lo sviluppatore di applicazioni è sempre possibile sostituire qualsiasi valore con una di propria scelta. Il criterio predefinito influisce solo sulle applicazioni in cui lo sviluppatore non è specificato un valore esplicito per alcune impostazioni specifiche.

## <a name="setting-default-policy"></a>Impostazione dei criteri predefiniti

Per impostare i criteri predefiniti, un amministratore può impostare i valori noti nel Registro di sistema nella chiave seguente.

La chiave del registro:`HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection`

Se si sta in un sistema operativo a 64 bit e si desidera influire sul funzionamento di applicazioni a 32 bit, ricordare inoltre di configurare l'equivalente di Wow6432Node della chiave sopra riportata.

I valori supportati sono:

* [Stringa] - EncryptionType specifica gli algoritmi da utilizzare per la protezione dati. Questo valore deve essere "CNG CBC", "CNG GCM" o "Gestito" e viene descritta in dettaglio [sotto](#data-protection-encryption-types).

* DefaultKeyLifetime [DWORD] - Specifica la durata per le chiavi appena generato. Questo valore è specificato in giorni e deve essere ≥ 7.

* [Stringa] - KeyEscrowSinks specifica i tipi che verranno utilizzati per deposito delle chiave. Questo valore è un elenco delimitato da punto e virgola di sink di deposito delle chiavi, in cui ogni elemento nell'elenco è il nome completo dell'assembly di un tipo che implementa IKeyEscrowSink.

<a name=data-protection-encryption-types></a>

### <a name="encryption-types"></a>Tipi di crittografia

Se EncryptionType è "CNG CBC", il sistema verrà configurato per utilizzare una crittografia simmetrica blocco in modalità CBC la riservatezza e HMAC autenticità con servizi forniti da CNG di Windows (vedere [che specifica gli algoritmi CNG di Windows personalizzati](overview.md#data-protection-changing-algorithms-cng)per altri dettagli). Sono supportati i valori aggiuntivi seguenti, ognuno dei quali corrisponde a una proprietà sul tipo CngCbcAuthenticatedEncryptionSettings:

* EncryptionAlgorithm [stringa] - il nome di un algoritmo di crittografia simmetrica blocco compreso CNG. Verrà aperta in modalità CBC questo algoritmo.

* EncryptionAlgorithmProvider [stringa] - il nome dell'implementazione del provider CNG con conseguenti EncryptionAlgorithm l'algoritmo.

* EncryptionAlgorithmKeySize [DWORD] - la lunghezza (in bit) della chiave per la derivazione per l'algoritmo di crittografia simmetrico.

* HashAlgorithm [stringa] - il nome di un algoritmo di hash compreso CNG. Questo algoritmo verrà aperta in modalità HMAC.

* HashAlgorithmProvider [stringa] - il nome dell'implementazione del provider CNG con conseguenti HashAlgorithm l'algoritmo.

Se EncryptionType è "CNG GCM", il sistema verrà configurato per utilizzare una crittografia simmetrica blocco modalità Galois o dei contatori per la riservatezza e l'autenticità con servizi forniti da CNG di Windows (vedere [che specifica gli algoritmi CNG di Windows personalizzati](overview.md#data-protection-changing-algorithms-cng)per altri dettagli). Sono supportati i valori aggiuntivi seguenti, ognuno dei quali corrisponde a una proprietà sul tipo CngGcmAuthenticatedEncryptionSettings:

* EncryptionAlgorithm [stringa] - il nome di un algoritmo di crittografia simmetrica blocco compreso CNG. Questo algoritmo verrà aperta in modalità Galois o dei contatori.

* EncryptionAlgorithmProvider [stringa] - il nome dell'implementazione del provider CNG con conseguenti EncryptionAlgorithm l'algoritmo.

* EncryptionAlgorithmKeySize [DWORD] - la lunghezza (in bit) della chiave per la derivazione per l'algoritmo di crittografia simmetrico.

Se EncryptionType è "gestito", il sistema verrà configurato per utilizzare un SymmetricAlgorithm gestito per la riservatezza e KeyedHashAlgorithm autenticità (vedere [personalizzata specifica gestiti algoritmi](overview.md#data-protection-changing-algorithms-custom-managed) per altri dettagli). Sono supportati i valori aggiuntivi seguenti, ognuno dei quali corrisponde a una proprietà sul tipo ManagedAuthenticatedEncryptionSettings:

* EncryptionAlgorithmType [stringa] - il nome completo dell'assembly di un tipo che implementa SymmetricAlgorithm.

* EncryptionAlgorithmKeySize [DWORD] - la lunghezza (in bit) della chiave da derivare l'algoritmo di crittografia simmetrica.

* ValidationAlgorithmType [stringa] - il nome completo dell'assembly di un tipo che implementa KeyedHashAlgorithm.

Se EncryptionType è qualsiasi altro valore (diverso da null / vuoto), il sistema di protezione di dati genererà un'eccezione all'avvio.

>[!WARNING]
> Quando si configura un'impostazione di criteri predefinito che include i nomi dei tipi (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), i tipi devono essere disponibili per l'applicazione. In pratica, ciò significa che per le applicazioni in esecuzione in CLR Desktop, gli assembly contenenti questi tipi devono essere GAC. Per le applicazioni ASP.NET Core in [.NET Core](https://www.microsoft.com/net/core), i pacchetti che contengono questi tipi devono essere installati.
