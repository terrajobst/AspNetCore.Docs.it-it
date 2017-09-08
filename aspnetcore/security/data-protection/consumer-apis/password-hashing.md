---
title: Hashing della password
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 982a1eb2-1e6f-4909-896f-82784364472d
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 262eed1f310ff3216c893fdc8a738f2ba6ec6729
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="password-hashing"></a>Hashing della password

Base di codice di protezione dati include un pacchetto *Microsoft.AspNetCore.Cryptography.KeyDerivation* che contiene funzioni di derivazione della chiave di crittografia. Questo pacchetto è un componente autonomo e non presenta dipendenze sul resto del sistema di protezione dati. E può essere utilizzato in modo completamente indipendente. L'origine esista insieme il codice di protezione dati base per motivi di praticità.

Il pacchetto attualmente offre un metodo `KeyDerivation.Pbkdf2` che consente l'hashing di una password utilizzando il [PBKDF2 algoritmo](https://tools.ietf.org/html/rfc2898#section-5.2). Questa API è molto simile a quello esistente di .NET Framework [Rfc2898DeriveBytes tipo](https://msdn.microsoft.com/library/System.Security.Cryptography.Rfc2898DeriveBytes(v=vs.110).aspx), ma esistono tre differenze importanti:

1. Il `KeyDerivation.Pbkdf2` metodo supporta l'utilizzo di più PRFs (attualmente `HMACSHA1`, `HMACSHA256`, e `HMACSHA512`), mentre il `Rfc2898DeriveBytes` digitare supporta solo `HMACSHA1`.

2. Il `KeyDerivation.Pbkdf2` metodo rileva il sistema operativo corrente e tenta di scegliere l'implementazione più ottimizzato della routine, che fornisce prestazioni migliori in alcuni casi. (In Windows 8, offre circa 10 volte la velocità effettiva di `Rfc2898DeriveBytes`.)

3. Il `KeyDerivation.Pbkdf2` metodo richiede che il chiamante specificare tutti i parametri (salinità, PRF e conteggio delle iterazioni). Il `Rfc2898DeriveBytes` tipo fornisce i valori predefiniti per questi.

[!code-csharp[Principale](password-hashing/samples/passwordhasher.cs)]

Vedere il codice sorgente per l'identità di ASP.NET Core `PasswordHasher` caso d'uso di tipo per un mondo reale.
