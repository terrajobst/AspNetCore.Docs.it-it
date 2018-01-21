---
title: Provider di protezione di dati temporanei
author: rick-anderson
description: Questo documento illustra i dettagli di implementazione dei provider di protezione dati temporanei di ASP.NET Core.
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: 9c1d03373c9d7fb6dffb3583c58aa593fd3875f4
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="ephemeral-data-protection-providers"></a>Provider di protezione di dati temporanei

<a name="data-protection-implementation-key-storage-ephemeral"></a>

Esistono scenari in cui un'applicazione richiede un buttare via `IDataProtectionProvider`. Ad esempio, lo sviluppatore può solo in fase di sperimentazione in un'applicazione console One-Off o l'applicazione stessa è temporaneo (viene inserito nello script o un progetto unit test). Per supportare questi scenari di [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) pacchetto include un tipo `EphemeralDataProtectionProvider`. Questo tipo fornisce un'implementazione di base `IDataProtectionProvider` repository la cui chiave viene mantenuto solo in memoria e non è stato scritto nell'archivio di backup.

Ogni istanza di `EphemeralDataProtectionProvider` utilizza la propria chiave master univoca. Pertanto, se un `IDataProtector` radice un `EphemeralDataProtectionProvider` genera un payload protetto, quel payload può solo essere non protetto da un equivalente `IDataProtector` (quello specificato è uguale [scopo](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes) catena) rooted contemporaneamente `EphemeralDataProtectionProvider` istanza.

L'esempio seguente viene illustrato come creare un'istanza di un `EphemeralDataProtectionProvider` da utilizzare per proteggere e annullare la protezione dati.

```csharp
using System;
using Microsoft.AspNetCore.DataProtection;

public class Program
{
    public static void Main(string[] args)
    {
        const string purpose = "Ephemeral.App.v1";

        // create an ephemeral provider and demonstrate that it can round-trip a payload
        var provider = new EphemeralDataProtectionProvider();
        var protector = provider.CreateProtector(purpose);
        Console.Write("Enter input: ");
        string input = Console.ReadLine();

        // protect the payload
        string protectedPayload = protector.Protect(input);
        Console.WriteLine($"Protect returned: {protectedPayload}");

        // unprotect the payload
        string unprotectedPayload = protector.Unprotect(protectedPayload);
        Console.WriteLine($"Unprotect returned: {unprotectedPayload}");

        // if I create a new ephemeral provider, it won't be able to unprotect existing
        // payloads, even if I specify the same purpose
        provider = new EphemeralDataProtectionProvider();
        protector = provider.CreateProtector(purpose);
        unprotectedPayload = protector.Unprotect(protectedPayload); // THROWS
    }
}

/*
* SAMPLE OUTPUT
*
* Enter input: Hello!
* Protect returned: CfDJ8AAAAAAAAAAAAAAAAAAAAA...uGoxWLjGKtm1SkNACQ
* Unprotect returned: Hello!
* << throws CryptographicException >>
*/
```
