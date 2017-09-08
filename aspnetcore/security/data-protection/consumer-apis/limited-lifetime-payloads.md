---
title: Limitando la durata di payload protetto
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 000175e2-10fc-43dd-bfc2-51e004b97b44
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 4ff13803b328c1e9dd2934c38c88b43f5798de03
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a>Limitando la durata di payload protetto

Esistono scenari in cui lo sviluppatore dell'applicazione desidera creare un payload protetto che scade dopo un determinato periodo di tempo. Ad esempio, il payload protetto potrebbe rappresentare un token di reimpostazione della password che devono solo essere valido per un'ora. È certamente possibile per gli sviluppatori di creare i propri formato di payload che contiene una data di scadenza incorporati e gli sviluppatori avanzati si consiglia di eseguire questa operazione comunque, ma per la maggior parte degli sviluppatori di gestire queste scadenze può crescere noiosa.

Per semplificare questa operazione per il gruppo di destinatari di sviluppatore, il pacchetto Microsoft.AspNetCore.DataProtection.Extensions contiene API dell'utilità per la creazione di payload che scadrà automaticamente dopo un determinato periodo di tempo. Queste API di blocco di fuori del tipo ITimeLimitedDataProtector.

## <a name="api-usage"></a>Utilizzo delle API

L'interfaccia ITimeLimitedDataProtector è l'interfaccia principale per la protezione e la rimozione della protezione di tempo limitato / self-scade il payload. Per creare un'istanza di un ITimeLimitedDataProtector, è necessario innanzitutto un'istanza di una normale [oggetto IDataProtector](overview.md) costruito con uno scopo specifico. Una volta l'istanza di oggetto IDataProtector è disponibile, chiamare il metodo di estensione IDataProtector.ToTimeLimitedDataProtector per ottenere una protezione con le funzionalità di scadenza predefinita.

ITimeLimitedDataProtector espone i metodi di estensione e di area API seguenti:

* CreateProtector (scopo stringa): ITimeLimitedDataProtector questa API è simile per la IDataProtectionProvider.CreateProtector esistente può essere utilizzato per creare [allo scopo di catene](purpose-strings.md) da una protezione con durata limitata di radice.

* Proteggere (byte [] testo normale, scadenza DateTimeOffset): byte]

* Proteggere (testo normale di byte [], durata intervallo di tempo): byte]

* Proteggere (testo crittografato [byte): byte]

* Proteggere (stringa testo normale, scadenza DateTimeOffset): stringa

* Proteggere (testo normale di stringa, durata intervallo di tempo): stringa

* Proteggere (testo normale stringa): stringa

Oltre ai metodi di protezione di core che accettano solo il testo non crittografato, sono disponibili nuovi overload che consentono di specificare la data di scadenza del payload. La data di scadenza può essere specificata come una data assoluta (tramite un oggetto DateTimeOffset) o come relativo data (ora di sistema corrente, tramite un intervallo di tempo). Se viene chiamato un overload che non avranno una scadenza, il payload presuppone scada mai.

* Rimuovere la protezione (protectedData [] byte, out scadenza DateTimeOffset): byte]

* Rimuovere la protezione (byte [] protectedData): byte]

* Rimuovere la protezione (stringa protectedData, out scadenza DateTimeOffset): stringa

* Rimuovere la protezione (stringa protectedData): stringa

I metodi Unprotect restituiscono i dati originali non protetti. Se il payload non è ancora scaduto, la scadenza assoluta viene restituita come parametro insieme ai dati protetti originali out facoltativa. Se il payload è scaduto, tutti gli overload del metodo Unprotect genererà CryptographicException.

>[!WARNING]
> È consigliabile non usare queste API per proteggere i payload che richiedono la persistenza a lungo termine o indefinita. "È concedere per il payload protetto essere recuperati in modo permanente dopo un mese?" può essere utilizzato come una buona regola pratica; Se la risposta non sviluppatori quindi considerare API alternative.

L'esempio seguente viene utilizzato il [i percorsi del codice non DI](../configuration/non-di-scenarios.md) per creare un'istanza di sistema di protezione dati. Per eseguire questo esempio, verificare che è stato innanzitutto aggiunto un riferimento al pacchetto Microsoft.AspNetCore.DataProtection.Extensions.

[!code-none[Principale](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
