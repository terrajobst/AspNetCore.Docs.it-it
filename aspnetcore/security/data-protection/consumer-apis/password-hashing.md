---
title: Password hash in ASP.NET Core
author: rick-anderson
description: Informazioni su come eseguire l'hashing delle password usando le API di protezione dei dati ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: bd4b8fcf6a5a16a86ada97bbd3519f872d1417b7
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602449"
---
# <a name="hash-passwords-in-aspnet-core"></a><span data-ttu-id="0b0da-103">Password hash in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b0da-103">Hash passwords in ASP.NET Core</span></span>

<span data-ttu-id="0b0da-104">La base di codice di protezione dei dati include un pacchetto *Microsoft. AspNetCore. Cryptography.* Key Derivation che contiene funzioni di derivazione della chiave crittografica.</span><span class="sxs-lookup"><span data-stu-id="0b0da-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="0b0da-105">Questo pacchetto è un componente autonomo e non ha dipendenze dal resto del sistema di protezione dei dati.</span><span class="sxs-lookup"><span data-stu-id="0b0da-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="0b0da-106">Può essere usato completamente in modo indipendente.</span><span class="sxs-lookup"><span data-stu-id="0b0da-106">It can be used completely independently.</span></span> <span data-ttu-id="0b0da-107">L'origine esiste insieme alla base del codice di protezione dei dati per praticità.</span><span class="sxs-lookup"><span data-stu-id="0b0da-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="0b0da-108">Il pacchetto offre attualmente un metodo `KeyDerivation.Pbkdf2` che consente l'hashing di una password usando l' [algoritmo PBKDF2](https://tools.ietf.org/html/rfc2898#section-5.2).</span><span class="sxs-lookup"><span data-stu-id="0b0da-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="0b0da-109">Questa API è molto simile al [tipo Rfc2898DeriveBytes](/dotnet/api/system.security.cryptography.rfc2898derivebytes)esistente del .NET Framework, ma sono disponibili tre distinzioni importanti:</span><span class="sxs-lookup"><span data-stu-id="0b0da-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="0b0da-110">Il `KeyDerivation.Pbkdf2` metodo supporta l'utilizzo di più PRFs ( `HMACSHA1`attualmente `HMACSHA256`, e `HMACSHA512`), mentre il `Rfc2898DeriveBytes` tipo supporta `HMACSHA1`solo.</span><span class="sxs-lookup"><span data-stu-id="0b0da-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="0b0da-111">Il `KeyDerivation.Pbkdf2` metodo rileva il sistema operativo corrente e tenta di scegliere l'implementazione più ottimizzata della routine, garantendo in alcuni casi prestazioni molto migliori.</span><span class="sxs-lookup"><span data-stu-id="0b0da-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="0b0da-112">In Windows 8 offre circa 10 volte la velocità effettiva di `Rfc2898DeriveBytes`.</span><span class="sxs-lookup"><span data-stu-id="0b0da-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="0b0da-113">Il `KeyDerivation.Pbkdf2` metodo richiede che il chiamante specifichi tutti i parametri (Salt, PRF e count di iterazione).</span><span class="sxs-lookup"><span data-stu-id="0b0da-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="0b0da-114">Il `Rfc2898DeriveBytes` tipo fornisce i valori predefiniti per questi.</span><span class="sxs-lookup"><span data-stu-id="0b0da-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="0b0da-115">Per un caso d'uso reale, vedere `PasswordHasher` il [codice sorgente](https://github.com/aspnet/AspNetCore/blob/master/src/Identity/Extensions.Core/src/PasswordHasher.cs) per ASP.NET Core tipo di identità.</span><span class="sxs-lookup"><span data-stu-id="0b0da-115">See the [source code](https://github.com/aspnet/AspNetCore/blob/master/src/Identity/Extensions.Core/src/PasswordHasher.cs) for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
