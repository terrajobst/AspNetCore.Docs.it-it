---
title: Provider di protezione dati temporanee in ASP.NET Core
author: rick-anderson
description: Scopri i dettagli di implementazione dei provider di protezione dati temporanei di ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: e4b0014ab3bdbf90b91383e8a33102f94faa8153
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895178"
---
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a>Provider di protezione dati temporanee in ASP.NET Core

<a name="data-protection-implementation-key-storage-ephemeral"></a>

Esistono scenari in cui un'applicazione richiede un buttare via `IDataProtectionProvider`. Ad esempio, lo sviluppatore può solo in fase di sperimentazione in un'applicazione console non standard, o l'applicazione stessa è temporaneo (viene inserito nello script o un progetto unit test). Per supportare questi scenari la [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) pacchetto include un tipo `EphemeralDataProtectionProvider`. Questo tipo fornisce un'implementazione di base di `IDataProtectionProvider` la cui chiave repository viene mantenuto attivo esclusivamente in memoria e non viene scritto nell'archivio di backup.

Ogni istanza di `EphemeralDataProtectionProvider` Usa la propria chiave master univoca. Pertanto, se un' `IDataProtector` nodo radice è un `EphemeralDataProtectionProvider` genera un payload protetto, quel payload possono solo essere non protetto da un equivalente `IDataProtector` (quello specificato è uguale [scopo](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) catena) rooted allo stesso `EphemeralDataProtectionProvider` istanza.

L'esempio seguente viene illustrato come creare un'istanza di un `EphemeralDataProtectionProvider` e usandolo per proteggere e rimuovere la protezione dei dati.

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
