---
title: Limitare la durata dei payload protetti in ASP.NET Core
author: rick-anderson
description: Informazioni su come limitare la durata di un payload protetto usando le API di protezione dei dati ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 8dc3b856ec67477ec8ae777749c9bf3107eb4eda
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656058"
---
# <a name="limit-the-lifetime-of-protected-payloads-in-aspnet-core"></a>Limitare la durata dei payload protetti in ASP.NET Core

Esistono scenari in cui lo sviluppatore di applicazioni desidera creare un payload protetto che scada dopo un determinato periodo di tempo. Ad esempio, il payload protetto potrebbe rappresentare un token di reimpostazione della password che dovrebbe essere valido solo per un'ora. È certamente possibile che lo sviluppatore crei un formato di payload che contenga una data di scadenza incorporata e che gli sviluppatori avanzati vogliano comunque eseguire questa operazione, ma per la maggior parte degli sviluppatori che gestiscono queste scadenze può diventare noioso.

Per semplificare questa operazione per i destinatari degli sviluppatori, il pacchetto [Microsoft. AspNetCore. dataprotection. Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contiene API di utilità per la creazione di payload che scadono automaticamente dopo un determinato periodo di tempo. Queste API si bloccano dal tipo di `ITimeLimitedDataProtector`.

## <a name="api-usage"></a>Utilizzo API

L'interfaccia `ITimeLimitedDataProtector` è l'interfaccia di base per la protezione e la rimozione della protezione dei payload a tempo limitato o a scadenza automatica. Per creare un'istanza di una `ITimeLimitedDataProtector`, è necessario innanzitutto un'istanza di un [IDataProtector](xref:security/data-protection/consumer-apis/overview) normale costruito con uno scopo specifico. Quando l'istanza di `IDataProtector` è disponibile, chiamare il metodo di estensione `IDataProtector.ToTimeLimitedDataProtector` per ottenere una protezione con funzionalità di scadenza predefinite.

`ITimeLimitedDataProtector` espone i seguenti metodi di estensione e superficie API:

* CreateProtector (scopo della stringa): ITimeLimitedDataProtector: questa API è simile alla `IDataProtectionProvider.CreateProtector` esistente perché può essere usata per creare catene di [scopi](xref:security/data-protection/consumer-apis/purpose-strings) da un programma di protezione con limitazioni temporali radice.

* Protect (byte [] testo normale, scadenza DateTimeOffset): byte []

* Protect (byte [] testo non crittografato, durata TimeSpan): byte []

* Protect (byte [] testo normale): byte []

* Protect (testo non crittografato, scadenza DateTimeOffset): stringa

* Protect(string plaintext, TimeSpan lifetime) : string

* Protect (testo non crittografato): stringa

Oltre ai metodi di base `Protect` che accettano solo il testo non crittografato, sono disponibili nuovi overload che consentono di specificare la data di scadenza del payload. La data di scadenza può essere specificata come una data assoluta (tramite un `DateTimeOffset`) o come tempo relativo (dall'ora di sistema corrente, tramite una `TimeSpan`). Se viene chiamato un overload che non accetta una scadenza, il payload viene considerato mai scaduto.

* Unprotect (byte [] protectedData, out DateTimeOffset scadenza): byte []

* Unprotect(byte[] protectedData) : byte[]

* Unprotect (String protectedData, out DateTimeOffset scadenza): stringa

* Unprotect (String protectedData): stringa

I `Unprotect` metodi restituiscono i dati originali non protetti. Se il payload non è ancora scaduto, la scadenza assoluta viene restituita come parametro out facoltativo insieme ai dati non protetti originali. Se il payload è scaduto, tutti gli overload del metodo Unprotect genereranno CryptographicException.

>[!WARNING]
> Non è consigliabile usare queste API per proteggere i payload che richiedono persistenza a lungo termine o indefinito. "È possibile garantire che i payload protetti siano irreversibili in modo permanente dopo un mese?" può fungere da regola generale. Se la risposta è No, gli sviluppatori devono prendere in considerazione API alternative.

Nell'esempio seguente vengono usati i [percorsi di codice non di](xref:security/data-protection/configuration/non-di-scenarios) per la creazione di un'istanza del sistema di protezione dei dati. Per eseguire questo esempio, verificare di aver prima aggiunto un riferimento al pacchetto Microsoft. AspNetCore. dataprotection. Extensions.

[!code-csharp[](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
