---
title: Provider di protezione di dati temporanei
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: af6ea1d0-0d9d-41df-a870-5dda24978e2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: ee8dccac3ba990b110758042192779426b01fc53
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2017
---
# <a name="ephemeral-data-protection-providers"></a>Provider di protezione di dati temporanei

<a name="data-protection-implementation-key-storage-ephemeral"></a>

Esistono scenari in cui un'applicazione richiede un IDataProtectionProvider buttare via. Ad esempio, lo sviluppatore può solo in fase di sperimentazione in un'applicazione console One-Off o l'applicazione stessa è temporaneo (viene inserito nello script o un progetto unit test). Per supportare questi scenari Microsoft.AspNetCore.DataProtection il pacchetto include un tipo EphemeralDataProtectionProvider. Questo tipo fornisce un'implementazione di base di IDataProtectionProvider repository la cui chiave viene mantenuto solo in memoria e non è stato scritto nell'archivio di backup.

Ogni istanza di EphemeralDataProtectionProvider utilizza la propria chiave master univoca. Pertanto, se un oggetto IDataProtector con radice in un EphemeralDataProtectionProvider genera un payload protetto, tale payload può solo essere non protetto da un oggetto equivalente IDataProtector (quello specificato è uguale [scopo](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes) catena) rooted contemporaneamente Istanza EphemeralDataProtectionProvider.

L'esempio seguente viene illustrata un'istanza di un EphemeralDataProtectionProvider e usarlo per proteggere e annullare la protezione dati.

```none
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
