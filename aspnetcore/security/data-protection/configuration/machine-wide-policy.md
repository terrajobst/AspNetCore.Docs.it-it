---
title: Supporto di criteri a livello di computer di protezione dei dati in ASP.NET Core
author: rick-anderson
description: Informazioni sul supporto per l'impostazione di criteri a livello di computer predefiniti per tutte le app che usano la protezione dei dati di ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: 70aaca7afcd3df22cebb4466fbd9845a2277688c
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897268"
---
# <a name="data-protection-machine-wide-policy-support-in-aspnet-core"></a>Supporto di criteri a livello di computer di protezione dei dati in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Quando si esegue Windows, il sistema di protezione dei dati ha supporto limitato per impostazione di criteri a livello di computer predefiniti per tutte le app che usano la protezione dei dati di ASP.NET Core. L'idea generale è che un amministratore potrebbe essere opportuno modificare un'impostazione predefinita, ad esempio gli algoritmi utilizzati o la durata delle chiavi, senza la necessità di aggiornare manualmente ogni app nel computer.

> [!WARNING]
> L'amministratore di sistema può impostare criteri predefiniti, ma non potranno applicarla. Lo sviluppatore dell'app può sempre eseguire l'override di qualsiasi valore con uno di propria scelta. Il criterio predefinito influisce solo sulle App in cui lo sviluppatore non ha specificato un valore esplicito per un'impostazione.

## <a name="setting-default-policy"></a>Impostazione dei criteri predefiniti

Per impostare i criteri predefiniti, un amministratore può impostare i valori noti nel Registro di sistema nella chiave del Registro di sistema seguente:

**HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection**

Se si ha in un sistema operativo a 64 bit e si vuole modificare il comportamento delle App a 32 bit, ricordarsi di configurare l'equivalente Wow6432Node della chiave precedente.

I valori supportati sono illustrati di seguito.

| Value              | Tipo   | Descrizione |
| ------------------ | :----: | ----------- |
| EncryptionType     | string | Specifica gli algoritmi devono essere utilizzati per la protezione dati. Il valore deve essere CNG-CBC, CNG-GCM o gestito e viene descritto in dettaglio più avanti. |
| DefaultKeyLifetime | DWORD  | Specifica la durata per le chiavi appena generato. Il valore è specificato in giorni e deve essere > = 7. |
| KeyEscrowSinks     | string | Specifica i tipi usati per deposito delle chiave. Il valore è un elenco delimitato da punto e virgola di sink di deposito delle chiavi, dove ogni elemento nell'elenco è il nome qualificato dall'assembly di un tipo che implementa [IKeyEscrowSink](/dotnet/api/microsoft.aspnetcore.dataprotection.keymanagement.ikeyescrowsink). |

## <a name="encryption-types"></a>Tipi di crittografia

Se EncryptionType è CNG-CBC, il sistema è configurato per usare una crittografia a blocchi simmetriche modalità CBC per la riservatezza e HMAC per autenticità con servizi forniti da CNG di Windows (vedere [che specifica gli algoritmi CNG di Windows personalizzati](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) per altri dettagli). Sono supportati i seguenti valori aggiuntivi, ognuno dei quali corrisponde a una proprietà sul tipo CngCbcAuthenticatedEncryptionSettings.

| Value                       | Tipo   | Descrizione |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | string | Il nome di un algoritmo di crittografia a blocchi simmetriche riconosciuto da CNG. Questo algoritmo viene aperto in modalità CBC. |
| EncryptionAlgorithmProvider | string | Il nome dell'implementazione del provider CNG in grado di produrre l'algoritmo EncryptionAlgorithm. |
| EncryptionAlgorithmKeySize  | DWORD  | La lunghezza (in bit) della chiave da derivare per l'algoritmo di crittografia a blocchi simmetriche. |
| HashAlgorithm               | string | Il nome di un algoritmo di hash riconosciuto da CNG. Questo algoritmo viene aperto in modalità HMAC. |
| HashAlgorithmProvider       | string | Il nome dell'implementazione del provider CNG in grado di produrre l'algoritmo HashAlgorithm. |

Se EncryptionType è CNG-GCM, il sistema è configurato per usare una crittografia a blocchi simmetriche/contatore Galois modalità per la riservatezza e l'autenticità con i servizi forniti da CNG di Windows (vedere [che specifica gli algoritmi CNG di Windows personalizzati](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) per altri dettagli). Sono supportati i seguenti valori aggiuntivi, ognuno dei quali corrisponde a una proprietà sul tipo CngGcmAuthenticatedEncryptionSettings.

| Value                       | Tipo   | Descrizione |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | string | Il nome di un algoritmo di crittografia a blocchi simmetriche riconosciuto da CNG. Questo algoritmo viene aperto in modalità/contatore Galois. |
| EncryptionAlgorithmProvider | string | Il nome dell'implementazione del provider CNG in grado di produrre l'algoritmo EncryptionAlgorithm. |
| EncryptionAlgorithmKeySize  | DWORD  | La lunghezza (in bit) della chiave da derivare per l'algoritmo di crittografia a blocchi simmetriche. |

Se EncryptionType è gestito, il sistema è configurato per usare un SymmetricAlgorithm gestito per la riservatezza e KeyedHashAlgorithm per autenticità (vedere [se si specifica personalizzata gestita algoritmi](xref:security/data-protection/configuration/overview#specifying-custom-managed-algorithms) per altri dettagli). Sono supportati i seguenti valori aggiuntivi, ognuno dei quali corrisponde a una proprietà sul tipo ManagedAuthenticatedEncryptionSettings.

| Value                      | Tipo   | Descrizione |
| -------------------------- | :----: | ----------- |
| EncryptionAlgorithmType    | string | Il nome qualificato dall'assembly di un tipo che implementa SymmetricAlgorithm. |
| EncryptionAlgorithmKeySize | DWORD  | La lunghezza (in bit) della chiave da derivare l'algoritmo di crittografia simmetrica. |
| ValidationAlgorithmType    | string | Il nome qualificato dall'assembly di un tipo che implementa KeyedHashAlgorithm. |

Se EncryptionType dispone di qualsiasi altro valore diverso da null o vuoto, il sistema di protezione dei dati genera un'eccezione all'avvio.

> [!WARNING]
> Quando si configura un'impostazione di criteri predefinita che prevede i nomi dei tipi (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), i tipi devono essere disponibili per l'app. Ciò significa che per le App in esecuzione in CLR Desktop, gli assembly che contengono tali tipi devono essere presenti nella Global Assembly Cache (GAC). Per le app ASP.NET Core in esecuzione in .NET Core, i pacchetti che contengono questi tipi devono essere installati.
