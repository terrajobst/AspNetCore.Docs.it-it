---
title: Limitare la durata di payload protetto in ASP.NET Core
author: rick-anderson
description: Informazioni su come limitare la durata di un payload protetto utilizzando le API di protezione dei dati di ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 324887b3d29de989ad855c4e78fd5a235fdb560e
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072023"
---
# <a name="limit-the-lifetime-of-protected-payloads-in-aspnet-core"></a>Limitare la durata di payload protetto in ASP.NET Core

Esistono scenari in cui lo sviluppatore dell'applicazione desidera creare un payload protetto che scade dopo un determinato periodo di tempo. Ad esempio, il payload protetto potrebbe rappresentare un token di reimpostazione della password che devono solo essere valido per un'ora. È certamente possibile per gli sviluppatori di creare i propri formato di payload che contiene una data di scadenza incorporati e gli sviluppatori avanzati si consiglia di eseguire questa operazione comunque, ma per la maggior parte degli sviluppatori di gestire queste scadenze può crescere noiosa.

Per semplificare questa operazione per il pubblico di sviluppatore, il pacchetto [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contiene API dell'utilità per la creazione di payload che scadrà automaticamente dopo un determinato periodo di tempo. Queste API sporgere all'esterno del `ITimeLimitedDataProtector` tipo.

## <a name="api-usage"></a>Utilizzo delle API

Il `ITimeLimitedDataProtector` è l'interfaccia di base per la protezione e la rimozione della protezione di payload di tempo limitato / temporanee. Per creare un'istanza di un `ITimeLimitedDataProtector`, è necessario innanzitutto un'istanza di una normale [oggetto IDataProtector](xref:security/data-protection/consumer-apis/overview) costruito con uno scopo specifico. Una volta il `IDataProtector` istanza è disponibile, chiamare il `IDataProtector.ToTimeLimitedDataProtector` metodo di estensione per ottenere una protezione con le funzionalità di scadenza predefinita.

`ITimeLimitedDataProtector` espone i metodi di estensione e di area API seguenti:

* CreateProtector (scopo stringa): ITimeLimitedDataProtector - questa API è simile al `IDataProtectionProvider.CreateProtector` in quanto può essere utilizzato per creare [allo scopo di catene](xref:security/data-protection/consumer-apis/purpose-strings) da una protezione con durata limitata di radice.

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

L'esempio seguente viene utilizzato il [i percorsi del codice non DI](xref:security/data-protection/configuration/non-di-scenarios) per creare un'istanza di sistema di protezione dati. Per eseguire questo esempio, verificare che è stato innanzitutto aggiunto un riferimento al pacchetto Microsoft.AspNetCore.DataProtection.Extensions.

[!code-csharp[](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
