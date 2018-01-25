---
title: Limitando la durata di payload protetto
author: rick-anderson
description: Questo documento viene illustrato come limitare la durata di un payload protetto utilizzando le API di protezione dati ASP.NET Core.
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 7909f21057f22e78c03b41464a19a18ce0908216
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a>Limitando la durata di payload protetto

Esistono scenari in cui lo sviluppatore dell'applicazione desidera creare un payload protetto che scade dopo un determinato periodo di tempo. Ad esempio, il payload protetto potrebbe rappresentare un token di reimpostazione della password che devono solo essere valido per un'ora. È certamente possibile per gli sviluppatori di creare i propri formato di payload che contiene una data di scadenza incorporati e gli sviluppatori avanzati si consiglia di eseguire questa operazione comunque, ma per la maggior parte degli sviluppatori di gestire queste scadenze può crescere noiosa.

Per semplificare questa operazione per il pubblico di sviluppatore, il pacchetto [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contiene API dell'utilità per la creazione di payload che scadrà automaticamente dopo un determinato periodo di tempo. Queste API sporgere all'esterno del `ITimeLimitedDataProtector` tipo.

## <a name="api-usage"></a>Utilizzo delle API

Il `ITimeLimitedDataProtector` è l'interfaccia di base per la protezione e la rimozione della protezione di payload di tempo limitato / temporanee. Per creare un'istanza di un `ITimeLimitedDataProtector`, è necessario innanzitutto un'istanza di una normale [oggetto IDataProtector](overview.md) costruito con uno scopo specifico. Una volta il `IDataProtector` istanza è disponibile, chiamare il `IDataProtector.ToTimeLimitedDataProtector` metodo di estensione per ottenere una protezione con le funzionalità di scadenza predefinita.

`ITimeLimitedDataProtector`espone i metodi di estensione e di area API seguenti:

* CreateProtector (scopo stringa): ITimeLimitedDataProtector - questa API è simile al `IDataProtectionProvider.CreateProtector` in quanto può essere utilizzato per creare [allo scopo di catene](purpose-strings.md) da una protezione con durata limitata di radice.

* Proteggere (byte [] testo normale, scadenza DateTimeOffset): byte]

* Proteggere (testo normale di byte [], durata intervallo di tempo): byte]

* Proteggere (testo crittografato [byte): byte]

* Proteggere (stringa testo normale, scadenza DateTimeOffset): stringa

* Proteggere (testo normale di stringa, durata intervallo di tempo): stringa

* Proteggere (testo normale stringa): stringa

Oltre alle principali `Protect` metodi che accettano solo il testo non crittografato, sono disponibili nuovi overload che consentono di specificare la data di scadenza del payload. La data di scadenza può essere specificata come una data assoluta (tramite un `DateTimeOffset`) o come un tempo relativi (dal sistema corrente time, tramite un `TimeSpan`). Se viene chiamato un overload che non avranno una scadenza, il payload presuppone scada mai.

* Rimuovere la protezione (protectedData [] byte, out scadenza DateTimeOffset): byte]

* Rimuovere la protezione (byte [] protectedData): byte]

* Rimuovere la protezione (stringa protectedData, out scadenza DateTimeOffset): stringa

* Rimuovere la protezione (stringa protectedData): stringa

Il `Unprotect` i metodi restituiscono i dati originali non protetti. Se il payload non è ancora scaduto, la scadenza assoluta viene restituita come parametro insieme ai dati protetti originali out facoltativa. Se il payload è scaduto, tutti gli overload del metodo Unprotect genererà CryptographicException.

>[!WARNING]
> Ha non è consigliabile usare queste API per proteggere i payload che richiedono la persistenza a lungo termine o indefinita. "È concedere per il payload protetto essere recuperati in modo permanente dopo un mese?" può essere utilizzato come una buona regola pratica; Se la risposta non sviluppatori quindi considerare API alternative.

L'esempio seguente viene utilizzato il [i percorsi del codice non DI](../configuration/non-di-scenarios.md) per creare un'istanza di sistema di protezione dati. Per eseguire questo esempio, verificare che è stato innanzitutto aggiunto un riferimento al pacchetto Microsoft.AspNetCore.DataProtection.Extensions.

[!code-csharp[Main](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
