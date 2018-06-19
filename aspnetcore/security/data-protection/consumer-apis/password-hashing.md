---
title: Hash delle password in ASP.NET Core
author: rick-anderson
description: Informazioni su come eseguire l'hashing delle password tramite le API di protezione dati di ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: f44e66789bf348ef6d99f6d862fb34c2d943a0b2
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2018
ms.locfileid: "32740101"
---
# <a name="hash-passwords-in-aspnet-core"></a>Hash delle password in ASP.NET Core

Base di codice di protezione dati include un pacchetto *Microsoft.AspNetCore.Cryptography.KeyDerivation* che contiene funzioni di derivazione della chiave di crittografia. Questo pacchetto è un componente autonomo e non presenta dipendenze sul resto del sistema di protezione dati. E può essere utilizzato in modo completamente indipendente. L'origine esista insieme il codice di protezione dati base per motivi di praticità.

Il pacchetto attualmente offre un metodo `KeyDerivation.Pbkdf2` che consente l'hashing di una password utilizzando il [PBKDF2 algoritmo](https://tools.ietf.org/html/rfc2898#section-5.2). Questa API è molto simile a quello esistente di .NET Framework [Rfc2898DeriveBytes tipo](/dotnet/api/system.security.cryptography.rfc2898derivebytes), ma esistono tre differenze importanti:

1. Il `KeyDerivation.Pbkdf2` metodo supporta l'utilizzo di più PRFs (attualmente `HMACSHA1`, `HMACSHA256`, e `HMACSHA512`), mentre il `Rfc2898DeriveBytes` digitare supporta solo `HMACSHA1`.

2. Il `KeyDerivation.Pbkdf2` metodo rileva il sistema operativo corrente e tenta di scegliere l'implementazione più ottimizzato della routine, che fornisce prestazioni migliori in alcuni casi. (In Windows 8, offre circa 10 volte la velocità effettiva di `Rfc2898DeriveBytes`.)

3. Il `KeyDerivation.Pbkdf2` metodo richiede che il chiamante specificare tutti i parametri (salinità, PRF e conteggio delle iterazioni). Il `Rfc2898DeriveBytes` tipo fornisce i valori predefiniti per questi.

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

Vedere il codice sorgente per l'identità di ASP.NET Core `PasswordHasher` caso d'uso di tipo per un mondo reale.
