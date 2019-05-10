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
# <a name="hash-passwords-in-aspnet-core"></a>Codifica hash delle password in ASP.NET Core

Base di codice di protezione dati include un pacchetto *Microsoft.AspNetCore.Cryptography.KeyDerivation* che contiene funzioni di derivazione della chiave di crittografia. Questo pacchetto è un componente autonomo e non ha dipendenze sul resto del sistema di protezione dati. Può essere utilizzato in modo completamente indipendente. L'origine esista insieme al codice di protezione di dati base per maggiore praticità.

Il pacchetto offre attualmente un metodo `KeyDerivation.Pbkdf2` che consente di hash password usando il [algoritmo PBKDF2](https://tools.ietf.org/html/rfc2898#section-5.2). Questa API è molto simile a quello di .NET Framework esistente [Rfc2898DeriveBytes tipo](/dotnet/api/system.security.cryptography.rfc2898derivebytes), ma sono presenti tre importanti differenze:

1. Il `KeyDerivation.Pbkdf2` metodo supporta il consumo di più PRFs (attualmente `HMACSHA1`, `HMACSHA256`, e `HMACSHA512`), mentre il `Rfc2898DeriveBytes` digitare supporta solo `HMACSHA1`.

2. Il `KeyDerivation.Pbkdf2` metodo rileva il sistema operativo corrente e tenta di scegliere l'implementazione della routine, offrendo prestazioni superiori in determinati casi più ottimizzata. (In Windows 8, offre circa 10 volte la velocità effettiva di `Rfc2898DeriveBytes`.)

3. Il `KeyDerivation.Pbkdf2` metodo richiede che il chiamante specificare tutti i parametri (valori salt, PRF e il numero di iterazione). Il `Rfc2898DeriveBytes` tipo fornisce i valori predefiniti per questi.

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

Vedere le [codice sorgente](https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs) per ASP.NET Core Identity `PasswordHasher` caso d'uso di tipo per una reale.
