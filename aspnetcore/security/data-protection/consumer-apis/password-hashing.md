---
title: Codifica hash delle password in ASP.NET Core
author: rick-anderson
description: Informazioni su come eseguire l'hashing delle password usando le API di protezione dati di ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 70301ffffbaaf3c5ff0642b19b80e40be83aa438
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896648"
---
# <a name="hash-passwords-in-aspnet-core"></a><span data-ttu-id="9393b-103">Codifica hash delle password in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9393b-103">Hash passwords in ASP.NET Core</span></span>

<span data-ttu-id="9393b-104">Base di codice di protezione dati include un pacchetto *Microsoft.AspNetCore.Cryptography.KeyDerivation* che contiene funzioni di derivazione della chiave di crittografia.</span><span class="sxs-lookup"><span data-stu-id="9393b-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="9393b-105">Questo pacchetto è un componente autonomo e non ha dipendenze sul resto del sistema di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="9393b-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="9393b-106">Può essere utilizzato in modo completamente indipendente.</span><span class="sxs-lookup"><span data-stu-id="9393b-106">It can be used completely independently.</span></span> <span data-ttu-id="9393b-107">L'origine esista insieme al codice di protezione di dati base per maggiore praticità.</span><span class="sxs-lookup"><span data-stu-id="9393b-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="9393b-108">Il pacchetto offre attualmente un metodo `KeyDerivation.Pbkdf2` che consente di hash password usando il [algoritmo PBKDF2](https://tools.ietf.org/html/rfc2898#section-5.2).</span><span class="sxs-lookup"><span data-stu-id="9393b-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="9393b-109">Questa API è molto simile a quello di .NET Framework esistente [Rfc2898DeriveBytes tipo](/dotnet/api/system.security.cryptography.rfc2898derivebytes), ma sono presenti tre importanti differenze:</span><span class="sxs-lookup"><span data-stu-id="9393b-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="9393b-110">Il `KeyDerivation.Pbkdf2` metodo supporta il consumo di più PRFs (attualmente `HMACSHA1`, `HMACSHA256`, e `HMACSHA512`), mentre il `Rfc2898DeriveBytes` digitare supporta solo `HMACSHA1`.</span><span class="sxs-lookup"><span data-stu-id="9393b-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="9393b-111">Il `KeyDerivation.Pbkdf2` metodo rileva il sistema operativo corrente e tenta di scegliere l'implementazione della routine, offrendo prestazioni superiori in determinati casi più ottimizzata.</span><span class="sxs-lookup"><span data-stu-id="9393b-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="9393b-112">(In Windows 8, offre circa 10 volte la velocità effettiva di `Rfc2898DeriveBytes`.)</span><span class="sxs-lookup"><span data-stu-id="9393b-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="9393b-113">Il `KeyDerivation.Pbkdf2` metodo richiede che il chiamante specificare tutti i parametri (valori salt, PRF e il numero di iterazione).</span><span class="sxs-lookup"><span data-stu-id="9393b-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="9393b-114">Il `Rfc2898DeriveBytes` tipo fornisce i valori predefiniti per questi.</span><span class="sxs-lookup"><span data-stu-id="9393b-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="9393b-115">Vedere le [codice sorgente](https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs) per ASP.NET Core Identity `PasswordHasher` caso d'uso di tipo per una reale.</span><span class="sxs-lookup"><span data-stu-id="9393b-115">See the [source code](https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs) for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
