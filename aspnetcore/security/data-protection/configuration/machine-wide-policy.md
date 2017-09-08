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
ms.openlocfilehash: 513726e209401d158ac98d5874942765751ac07d
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="machine-wide-policy"></a><span data-ttu-id="0b9cc-103">Criterio per l'intero computer</span><span class="sxs-lookup"><span data-stu-id="0b9cc-103">Machine Wide Policy</span></span>

<a name=data-protection-configuration-machinewidepolicy></a>

<span data-ttu-id="0b9cc-104">Durante l'esecuzione in Windows, il sistema di protezione dati offre supporto limitato per l'impostazione di criteri a livello di computer predefiniti per tutte le applicazioni che utilizzano la protezione dei dati.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-104">When running on Windows, the data protection system has limited support for setting default machine-wide policy for all applications which consume data protection.</span></span> <span data-ttu-id="0b9cc-105">L'idea generale è che un amministratore potrebbe essere opportuno modificare alcune impostazioni predefinite (ad esempio gli algoritmi utilizzati o chiave la durata) senza la necessità di aggiornare manualmente ogni applicazione nel computer.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-105">The general idea is that an administrator might wish to change some default setting (such as algorithms used or key lifetime) without needing to manually update every application on the machine.</span></span>

>[!WARNING]
> <span data-ttu-id="0b9cc-106">L'amministratore di sistema può impostare i criteri predefiniti, ma non è possibile applicarla.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-106">The system administrator can set default policy, but they cannot enforce it.</span></span> <span data-ttu-id="0b9cc-107">Lo sviluppatore di applicazioni è sempre possibile sostituire qualsiasi valore con una di propria scelta.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-107">The application developer can always override any value with one of their own choosing.</span></span> <span data-ttu-id="0b9cc-108">Il criterio predefinito influisce solo sulle applicazioni in cui lo sviluppatore non è specificato un valore esplicito per alcune impostazioni specifiche.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-108">The default policy only affects applications where the developer has not specified an explicit value for some particular setting.</span></span>

## <a name="setting-default-policy"></a><span data-ttu-id="0b9cc-109">Impostazione dei criteri predefiniti</span><span class="sxs-lookup"><span data-stu-id="0b9cc-109">Setting default policy</span></span>

<span data-ttu-id="0b9cc-110">Per impostare i criteri predefiniti, un amministratore può impostare i valori noti nel Registro di sistema nella chiave seguente.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-110">To set default policy, an administrator can set known values in the system registry under the following key.</span></span>

<span data-ttu-id="0b9cc-111">La chiave del registro:`HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection`</span><span class="sxs-lookup"><span data-stu-id="0b9cc-111">Reg key: `HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection`</span></span>

<span data-ttu-id="0b9cc-112">Se si sta in un sistema operativo a 64 bit e si desidera influire sul funzionamento di applicazioni a 32 bit, ricordare inoltre di configurare l'equivalente di Wow6432Node della chiave sopra riportata.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-112">If you're on a 64-bit operating system and want to affect the behavior of 32-bit applications, remember also to configure the Wow6432Node equivalent of the above key.</span></span>

<span data-ttu-id="0b9cc-113">I valori supportati sono:</span><span class="sxs-lookup"><span data-stu-id="0b9cc-113">The supported values are:</span></span>

* <span data-ttu-id="0b9cc-114">[Stringa] - EncryptionType specifica gli algoritmi da utilizzare per la protezione dati.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-114">EncryptionType [string] - specifies which algorithms should be used for data protection.</span></span> <span data-ttu-id="0b9cc-115">Questo valore deve essere "CNG CBC", "CNG GCM" o "Gestito" e viene descritta in dettaglio [sotto](#data-protection-encryption-types).</span><span class="sxs-lookup"><span data-stu-id="0b9cc-115">This value must be "CNG-CBC", "CNG-GCM", or "Managed" and is described in more detail [below](#data-protection-encryption-types).</span></span>

* <span data-ttu-id="0b9cc-116">DefaultKeyLifetime [DWORD] - Specifica la durata per le chiavi appena generato.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-116">DefaultKeyLifetime [DWORD] - specifies the lifetime for newly-generated keys.</span></span> <span data-ttu-id="0b9cc-117">Questo valore è specificato in giorni e deve essere ≥ 7.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-117">This value is specified in days and must be ≥ 7.</span></span>

* <span data-ttu-id="0b9cc-118">[Stringa] - KeyEscrowSinks specifica i tipi che verranno utilizzati per deposito delle chiave.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-118">KeyEscrowSinks [string] - specifies the types which will be used for key escrow.</span></span> <span data-ttu-id="0b9cc-119">Questo valore è un elenco delimitato da punto e virgola di sink di deposito delle chiavi, in cui ogni elemento nell'elenco è il nome completo dell'assembly di un tipo che implementa IKeyEscrowSink.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-119">This value is a semicolon-delimited list of key escrow sinks, where each element in the list is the assembly qualified name of a type which implements IKeyEscrowSink.</span></span>

<a name=data-protection-encryption-types></a>

### <a name="encryption-types"></a><span data-ttu-id="0b9cc-120">Tipi di crittografia</span><span class="sxs-lookup"><span data-stu-id="0b9cc-120">Encryption types</span></span>

<span data-ttu-id="0b9cc-121">Se EncryptionType è "CNG CBC", il sistema verrà configurato per utilizzare una crittografia simmetrica blocco in modalità CBC la riservatezza e HMAC autenticità con servizi forniti da CNG di Windows (vedere [che specifica gli algoritmi CNG di Windows personalizzati](overview.md#data-protection-changing-algorithms-cng)per altri dettagli).</span><span class="sxs-lookup"><span data-stu-id="0b9cc-121">If EncryptionType is "CNG-CBC", the system will be configured to use a CBC-mode symmetric block cipher for confidentiality and HMAC for authenticity with services provided by Windows CNG (see [Specifying custom Windows CNG algorithms](overview.md#data-protection-changing-algorithms-cng) for more details).</span></span> <span data-ttu-id="0b9cc-122">Sono supportati i valori aggiuntivi seguenti, ognuno dei quali corrisponde a una proprietà sul tipo CngCbcAuthenticatedEncryptionSettings:</span><span class="sxs-lookup"><span data-stu-id="0b9cc-122">The following additional values are supported, each of which corresponds to a property on the CngCbcAuthenticatedEncryptionSettings type:</span></span>

* <span data-ttu-id="0b9cc-123">EncryptionAlgorithm [stringa] - il nome di un algoritmo di crittografia simmetrica blocco compreso CNG.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-123">EncryptionAlgorithm [string] - the name of a symmetric block cipher algorithm understood by CNG.</span></span> <span data-ttu-id="0b9cc-124">Verrà aperta in modalità CBC questo algoritmo.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-124">This algorithm will be opened in CBC mode.</span></span>

* <span data-ttu-id="0b9cc-125">EncryptionAlgorithmProvider [stringa] - il nome dell'implementazione del provider CNG con conseguenti EncryptionAlgorithm l'algoritmo.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-125">EncryptionAlgorithmProvider [string] - the name of the CNG provider implementation which can produce the algorithm EncryptionAlgorithm.</span></span>

* <span data-ttu-id="0b9cc-126">EncryptionAlgorithmKeySize [DWORD] - la lunghezza (in bit) della chiave per la derivazione per l'algoritmo di crittografia simmetrico.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-126">EncryptionAlgorithmKeySize [DWORD] - the length (in bits) of the key to derive for the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="0b9cc-127">HashAlgorithm [stringa] - il nome di un algoritmo di hash compreso CNG.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-127">HashAlgorithm [string] - the name of a hash algorithm understood by CNG.</span></span> <span data-ttu-id="0b9cc-128">Questo algoritmo verrà aperta in modalità HMAC.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-128">This algorithm will be opened in HMAC mode.</span></span>

* <span data-ttu-id="0b9cc-129">HashAlgorithmProvider [stringa] - il nome dell'implementazione del provider CNG con conseguenti HashAlgorithm l'algoritmo.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-129">HashAlgorithmProvider [string] - the name of the CNG provider implementation which can produce the algorithm HashAlgorithm.</span></span>

<span data-ttu-id="0b9cc-130">Se EncryptionType è "CNG GCM", il sistema verrà configurato per utilizzare una crittografia simmetrica blocco modalità Galois o dei contatori per la riservatezza e l'autenticità con servizi forniti da CNG di Windows (vedere [che specifica gli algoritmi CNG di Windows personalizzati](overview.md#data-protection-changing-algorithms-cng)per altri dettagli).</span><span class="sxs-lookup"><span data-stu-id="0b9cc-130">If EncryptionType is "CNG-GCM", the system will be configured to use a Galois/Counter Mode symmetric block cipher for confidentiality and authenticity with services provided by Windows CNG (see [Specifying custom Windows CNG algorithms](overview.md#data-protection-changing-algorithms-cng) for more details).</span></span> <span data-ttu-id="0b9cc-131">Sono supportati i valori aggiuntivi seguenti, ognuno dei quali corrisponde a una proprietà sul tipo CngGcmAuthenticatedEncryptionSettings:</span><span class="sxs-lookup"><span data-stu-id="0b9cc-131">The following additional values are supported, each of which corresponds to a property on the CngGcmAuthenticatedEncryptionSettings type:</span></span>

* <span data-ttu-id="0b9cc-132">EncryptionAlgorithm [stringa] - il nome di un algoritmo di crittografia simmetrica blocco compreso CNG.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-132">EncryptionAlgorithm [string] - the name of a symmetric block cipher algorithm understood by CNG.</span></span> <span data-ttu-id="0b9cc-133">Questo algoritmo verrà aperta in modalità Galois o dei contatori.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-133">This algorithm will be opened in Galois/Counter Mode.</span></span>

* <span data-ttu-id="0b9cc-134">EncryptionAlgorithmProvider [stringa] - il nome dell'implementazione del provider CNG con conseguenti EncryptionAlgorithm l'algoritmo.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-134">EncryptionAlgorithmProvider [string] - the name of the CNG provider implementation which can produce the algorithm EncryptionAlgorithm.</span></span>

* <span data-ttu-id="0b9cc-135">EncryptionAlgorithmKeySize [DWORD] - la lunghezza (in bit) della chiave per la derivazione per l'algoritmo di crittografia simmetrico.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-135">EncryptionAlgorithmKeySize [DWORD] - the length (in bits) of the key to derive for the symmetric block cipher algorithm.</span></span>

<span data-ttu-id="0b9cc-136">Se EncryptionType è "gestito", il sistema verrà configurato per utilizzare un SymmetricAlgorithm gestito per la riservatezza e KeyedHashAlgorithm autenticità (vedere [personalizzata specifica gestiti algoritmi](overview.md#data-protection-changing-algorithms-custom-managed) per altri dettagli).</span><span class="sxs-lookup"><span data-stu-id="0b9cc-136">If EncryptionType is "Managed", the system will be configured to use a managed SymmetricAlgorithm for confidentiality and KeyedHashAlgorithm for authenticity (see [Specifying custom managed algorithms](overview.md#data-protection-changing-algorithms-custom-managed) for more details).</span></span> <span data-ttu-id="0b9cc-137">Sono supportati i valori aggiuntivi seguenti, ognuno dei quali corrisponde a una proprietà sul tipo ManagedAuthenticatedEncryptionSettings:</span><span class="sxs-lookup"><span data-stu-id="0b9cc-137">The following additional values are supported, each of which corresponds to a property on the ManagedAuthenticatedEncryptionSettings type:</span></span>

* <span data-ttu-id="0b9cc-138">EncryptionAlgorithmType [stringa] - il nome completo dell'assembly di un tipo che implementa SymmetricAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-138">EncryptionAlgorithmType [string] - the assembly-qualified name of a type which implements SymmetricAlgorithm.</span></span>

* <span data-ttu-id="0b9cc-139">EncryptionAlgorithmKeySize [DWORD] - la lunghezza (in bit) della chiave da derivare l'algoritmo di crittografia simmetrica.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-139">EncryptionAlgorithmKeySize [DWORD] - the length (in bits) of the key to derive for the symmetric encryption algorithm.</span></span>

* <span data-ttu-id="0b9cc-140">ValidationAlgorithmType [stringa] - il nome completo dell'assembly di un tipo che implementa KeyedHashAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-140">ValidationAlgorithmType [string] - the assembly-qualified name of a type which implements KeyedHashAlgorithm.</span></span>

<span data-ttu-id="0b9cc-141">Se EncryptionType è qualsiasi altro valore (diverso da null / vuoto), il sistema di protezione di dati genererà un'eccezione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-141">If EncryptionType has any other value (other than null / empty), the data protection system will throw an exception at startup.</span></span>

>[!WARNING]
> <span data-ttu-id="0b9cc-142">Quando si configura un'impostazione di criteri predefinito che include i nomi dei tipi (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), i tipi devono essere disponibili per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-142">When configuring a default policy setting that involves type names (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), the types must be available to the application.</span></span> <span data-ttu-id="0b9cc-143">In pratica, ciò significa che per le applicazioni in esecuzione in CLR Desktop, gli assembly contenenti questi tipi devono essere GAC.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-143">In practice, this means that for applications running on Desktop CLR, the assemblies which contain these types should be GACed.</span></span> <span data-ttu-id="0b9cc-144">Per le applicazioni ASP.NET Core in [.NET Core](https://microsoft.com/net/core), i pacchetti che contengono questi tipi devono essere installati.</span><span class="sxs-lookup"><span data-stu-id="0b9cc-144">For ASP.NET Core applications running on [.NET Core](https://microsoft.com/net/core), the packages which contain these types should be installed.</span></span>
