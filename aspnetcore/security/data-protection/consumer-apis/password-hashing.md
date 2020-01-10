---
title: Password hash in ASP.NET Core
author: rick-anderson
description: Informazioni su come eseguire l'hashing delle password usando le API di protezione dei dati ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 52532f9f4d7f9037609c8273deb8b6964d81c714
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828568"
---
# <a name="hash-passwords-in-aspnet-core"></a>Password hash in ASP.NET Core

La base di codice di protezione dei dati include un pacchetto *Microsoft. AspNetCore. Cryptography. Key Derivation* che contiene funzioni di derivazione della chiave crittografica. Questo pacchetto è un componente autonomo e non ha dipendenze dal resto del sistema di protezione dei dati. Può essere usato completamente in modo indipendente. L'origine esiste insieme alla base del codice di protezione dei dati per praticità.

Il pacchetto offre attualmente un metodo `KeyDerivation.Pbkdf2` che consente l'hashing di una password usando l' [algoritmo PBKDF2](https://tools.ietf.org/html/rfc2898#section-5.2). Questa API è molto simile al [tipo Rfc2898DeriveBytes](/dotnet/api/system.security.cryptography.rfc2898derivebytes)esistente del .NET Framework, ma sono disponibili tre distinzioni importanti:

1. Il metodo `KeyDerivation.Pbkdf2` supporta l'utilizzo di più PRFs (attualmente `HMACSHA1`, `HMACSHA256`e `HMACSHA512`), mentre il tipo di `Rfc2898DeriveBytes` supporta solo `HMACSHA1`.

2. Il metodo `KeyDerivation.Pbkdf2` rileva il sistema operativo corrente e tenta di scegliere l'implementazione più ottimizzata della routine, garantendo in alcuni casi prestazioni molto migliori. In Windows 8 offre circa 10 volte la velocità effettiva del `Rfc2898DeriveBytes`.

3. Il metodo `KeyDerivation.Pbkdf2` richiede che il chiamante specifichi tutti i parametri (Salt, PRF e count di iterazione). Il tipo di `Rfc2898DeriveBytes` fornisce i valori predefiniti per questi.

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

Per un caso d'uso reale, vedere il [codice sorgente](https://github.com/dotnet/AspNetCore/blob/master/src/Identity/Extensions.Core/src/PasswordHasher.cs) per ASP.NET Core tipo di `PasswordHasher` dell'identità.
