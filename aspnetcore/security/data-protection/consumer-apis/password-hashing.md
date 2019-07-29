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
# <a name="hash-passwords-in-aspnet-core"></a>Password hash in ASP.NET Core

La base di codice di protezione dei dati include un pacchetto *Microsoft. AspNetCore. Cryptography.* Key Derivation che contiene funzioni di derivazione della chiave crittografica. Questo pacchetto è un componente autonomo e non ha dipendenze dal resto del sistema di protezione dei dati. Può essere usato completamente in modo indipendente. L'origine esiste insieme alla base del codice di protezione dei dati per praticità.

Il pacchetto offre attualmente un metodo `KeyDerivation.Pbkdf2` che consente l'hashing di una password usando l' [algoritmo PBKDF2](https://tools.ietf.org/html/rfc2898#section-5.2). Questa API è molto simile al [tipo Rfc2898DeriveBytes](/dotnet/api/system.security.cryptography.rfc2898derivebytes)esistente del .NET Framework, ma sono disponibili tre distinzioni importanti:

1. Il `KeyDerivation.Pbkdf2` metodo supporta l'utilizzo di più PRFs ( `HMACSHA1`attualmente `HMACSHA256`, e `HMACSHA512`), mentre il `Rfc2898DeriveBytes` tipo supporta `HMACSHA1`solo.

2. Il `KeyDerivation.Pbkdf2` metodo rileva il sistema operativo corrente e tenta di scegliere l'implementazione più ottimizzata della routine, garantendo in alcuni casi prestazioni molto migliori. In Windows 8 offre circa 10 volte la velocità effettiva di `Rfc2898DeriveBytes`.

3. Il `KeyDerivation.Pbkdf2` metodo richiede che il chiamante specifichi tutti i parametri (Salt, PRF e count di iterazione). Il `Rfc2898DeriveBytes` tipo fornisce i valori predefiniti per questi.

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

Per un caso d'uso reale, vedere `PasswordHasher` il [codice sorgente](https://github.com/aspnet/AspNetCore/blob/master/src/Identity/Extensions.Core/src/PasswordHasher.cs) per ASP.NET Core tipo di identità.
