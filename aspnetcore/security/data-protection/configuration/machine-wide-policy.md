---
title: Supporto di criteri a livello di computer di protezione dati in ASP.NET Core
author: rick-anderson
description: Informazioni sul supporto per l'impostazione di criteri a livello di computer predefiniti per tutte le applicazioni che utilizzano la protezione dei dati di ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: c2d5760cd18f4e3ecaf0261f36414c9298e3f4c5
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
ms.locfileid: "30076962"
---
# <a name="data-protection-machine-wide-policy-support-in-aspnet-core"></a>Supporto di criteri a livello di computer di protezione dati in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Durante l'esecuzione in Windows, il sistema di protezione dei dati offre supporto limitato per l'impostazione di criteri a livello di computer predefiniti per tutte le applicazioni che utilizzano la protezione dei dati di ASP.NET Core. L'idea generale è che un amministratore potrebbe essere opportuno modificare un'impostazione predefinita, ad esempio gli algoritmi utilizzati o durata della chiave, senza la necessità di aggiornare manualmente ogni app nel computer.

> [!WARNING]
> L'amministratore di sistema può impostare i criteri predefiniti, ma non è possibile applicarla. Lo sviluppatore dell'app sempre possibile eseguire l'override di qualsiasi valore con una di propria scelta. Il criterio predefinito influisce solo sulle App in cui lo sviluppatore non è stato specificato un valore esplicito per un'impostazione.

## <a name="setting-default-policy"></a>Impostazione dei criteri predefiniti

Per impostare i criteri predefiniti, un amministratore può impostare i valori noti nel Registro di sistema nella chiave del Registro di sistema seguente:

**HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection**

Se si desidera influire sul funzionamento di applicazioni a 32 bit in un sistema operativo a 64 bit, ricordarsi di configurare l'equivalente di Wow6432Node della chiave sopra riportata.

I valori supportati sono illustrati di seguito.

| Valore              | Tipo   | Descrizione |
| ------------------ | :----: | ----------- |
| EncryptionType     | stringa | Specifica gli algoritmi da utilizzare per la protezione dati. Il valore deve essere CNG CBC, GCM di CNG o gestito e viene descritto in dettaglio più avanti. |
| DefaultKeyLifetime | DWORD  | Specifica la durata per le chiavi appena generato. Il valore è specificato in giorni e deve essere > = 7. |
| KeyEscrowSinks     | stringa | Specifica i tipi utilizzati per deposito delle chiave. Il valore è un elenco delimitato da punto e virgola di sink di deposito delle chiavi, in cui ogni elemento nell'elenco è il nome completo dell'assembly di un tipo che implementa [IKeyEscrowSink](/dotnet/api/microsoft.aspnetcore.dataprotection.keymanagement.ikeyescrowsink). |

## <a name="encryption-types"></a>Tipi di crittografia

Se EncryptionType CNG CBC, il sistema è configurato per utilizzare una crittografia simmetrica blocco in modalità CBC la riservatezza e HMAC autenticità con servizi forniti da CNG di Windows (vedere [che specifica gli algoritmi CNG di Windows personalizzati](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) per ulteriori dettagli). Sono supportati i valori aggiuntivi seguenti, ognuno dei quali corrisponde a una proprietà sul tipo CngCbcAuthenticatedEncryptionSettings.

| Valore                       | Tipo   | Descrizione |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | stringa | Il nome di un algoritmo di crittografia simmetrica blocco compreso CNG. Questo algoritmo viene aperto in modalità CBC. |
| EncryptionAlgorithmProvider | stringa | Il nome dell'implementazione del provider CNG in grado di produrre l'algoritmo EncryptionAlgorithm. |
| EncryptionAlgorithmKeySize  | DWORD  | La lunghezza della chiave per la derivazione per l'algoritmo di crittografia simmetrica blocchi (in bit). |
| HashAlgorithm               | stringa | Il nome di un algoritmo di hash compreso CNG. Questo algoritmo viene aperto in modalità HMAC. |
| HashAlgorithmProvider       | stringa | Il nome dell'implementazione del provider CNG in grado di produrre l'algoritmo HashAlgorithm. |

Se EncryptionType CNG GCM, il sistema è configurato per utilizzare una crittografia simmetrica blocco modalità Galois o dei contatori per la riservatezza e l'autenticità con servizi forniti da CNG di Windows (vedere [che specifica gli algoritmi CNG di Windows personalizzati](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) Per ulteriori dettagli). Sono supportati i valori aggiuntivi seguenti, ognuno dei quali corrisponde a una proprietà sul tipo CngGcmAuthenticatedEncryptionSettings.

| Valore                       | Tipo   | Descrizione |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | stringa | Il nome di un algoritmo di crittografia simmetrica blocco compreso CNG. Questo algoritmo viene aperto in modalità di Galois o dei contatori. |
| EncryptionAlgorithmProvider | stringa | Il nome dell'implementazione del provider CNG in grado di produrre l'algoritmo EncryptionAlgorithm. |
| EncryptionAlgorithmKeySize  | DWORD  | La lunghezza della chiave per la derivazione per l'algoritmo di crittografia simmetrica blocchi (in bit). |

Se EncryptionType è gestita, il sistema è configurato per utilizzare un SymmetricAlgorithm gestito per la riservatezza e KeyedHashAlgorithm autenticità (vedere [personalizzata specifica gestiti algoritmi](xref:security/data-protection/configuration/overview#specifying-custom-managed-algorithms) per altri dettagli). Sono supportati i valori aggiuntivi seguenti, ognuno dei quali corrisponde a una proprietà sul tipo ManagedAuthenticatedEncryptionSettings.

| Valore                      | Tipo   | Descrizione |
| -------------------------- | :----: | ----------- |
| EncryptionAlgorithmType    | stringa | Il nome completo dell'assembly di un tipo che implementa SymmetricAlgorithm. |
| EncryptionAlgorithmKeySize | DWORD  | La lunghezza (in bit) della chiave da derivare l'algoritmo di crittografia simmetrica. |
| ValidationAlgorithmType    | stringa | Il nome completo dell'assembly di un tipo che implementa KeyedHashAlgorithm. |

Se EncryptionType è qualsiasi altro valore diverso da null o vuoto, il sistema di protezione dei dati genera un'eccezione all'avvio.

> [!WARNING]
> Quando si configura un'impostazione di criteri predefinito che include i nomi dei tipi (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), i tipi devono essere disponibili per l'app. Ciò significa che per le App in esecuzione in CLR Desktop, gli assembly contenenti questi tipi devono essere presenti nella Global Assembly Cache (GAC). Per le app ASP.NET Core in esecuzione su .NET Core, i pacchetti che contengono questi tipi devono essere installati.
