---
title: Hashing della password
author: rick-anderson
description: Questo documento illustra come eseguire l'hashing delle password mediante le API di protezione dati ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 8e2796108be14ef382f46e6deb3d584517120d27
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/02/2018
---
# <a name="password-hashing"></a><span data-ttu-id="0b3a5-103">Hashing della password</span><span class="sxs-lookup"><span data-stu-id="0b3a5-103">Password Hashing</span></span>

<span data-ttu-id="0b3a5-104">Base di codice di protezione dati include un pacchetto *Microsoft.AspNetCore.Cryptography.KeyDerivation* che contiene funzioni di derivazione della chiave di crittografia.</span><span class="sxs-lookup"><span data-stu-id="0b3a5-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="0b3a5-105">Questo pacchetto è un componente autonomo e non presenta dipendenze sul resto del sistema di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="0b3a5-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="0b3a5-106">E può essere utilizzato in modo completamente indipendente.</span><span class="sxs-lookup"><span data-stu-id="0b3a5-106">It can be used completely independently.</span></span> <span data-ttu-id="0b3a5-107">L'origine esista insieme il codice di protezione dati base per motivi di praticità.</span><span class="sxs-lookup"><span data-stu-id="0b3a5-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="0b3a5-108">Il pacchetto attualmente offre un metodo `KeyDerivation.Pbkdf2` che consente l'hashing di una password utilizzando il [PBKDF2 algoritmo](https://tools.ietf.org/html/rfc2898#section-5.2).</span><span class="sxs-lookup"><span data-stu-id="0b3a5-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="0b3a5-109">Questa API è molto simile a quello esistente di .NET Framework [Rfc2898DeriveBytes tipo](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), ma esistono tre differenze importanti:</span><span class="sxs-lookup"><span data-stu-id="0b3a5-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="0b3a5-110">Il `KeyDerivation.Pbkdf2` metodo supporta l'utilizzo di più PRFs (attualmente `HMACSHA1`, `HMACSHA256`, e `HMACSHA512`), mentre il `Rfc2898DeriveBytes` digitare supporta solo `HMACSHA1`.</span><span class="sxs-lookup"><span data-stu-id="0b3a5-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="0b3a5-111">Il `KeyDerivation.Pbkdf2` metodo rileva il sistema operativo corrente e tenta di scegliere l'implementazione più ottimizzato della routine, che fornisce prestazioni migliori in alcuni casi.</span><span class="sxs-lookup"><span data-stu-id="0b3a5-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="0b3a5-112">(In Windows 8, offre circa 10 volte la velocità effettiva di `Rfc2898DeriveBytes`.)</span><span class="sxs-lookup"><span data-stu-id="0b3a5-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="0b3a5-113">Il `KeyDerivation.Pbkdf2` metodo richiede che il chiamante specificare tutti i parametri (salinità, PRF e conteggio delle iterazioni).</span><span class="sxs-lookup"><span data-stu-id="0b3a5-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="0b3a5-114">Il `Rfc2898DeriveBytes` tipo fornisce i valori predefiniti per questi.</span><span class="sxs-lookup"><span data-stu-id="0b3a5-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="0b3a5-115">Vedere il codice sorgente per l'identità di ASP.NET Core `PasswordHasher` caso d'uso di tipo per un mondo reale.</span><span class="sxs-lookup"><span data-stu-id="0b3a5-115">See the source code for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
