---
title: Limitare la durata di payload protetti in ASP.NET Core
author: rick-anderson
description: Informazioni su come limitare la durata di un payload protetto usando le API di protezione dati di ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 8dc3b856ec67477ec8ae777749c9bf3107eb4eda
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897118"
---
# <a name="limit-the-lifetime-of-protected-payloads-in-aspnet-core"></a>Limitare la durata di payload protetti in ASP.NET Core

Esistono scenari in cui lo sviluppatore dell'applicazione deve creare un payload protetto che scade dopo un determinato periodo di tempo. Ad esempio, il payload protetto potrebbe rappresentare un token di reimpostazione della password che debba essere valido solo per un'ora. È certamente possibile per gli sviluppatori creare i propri formato di payload contenente una data di scadenza incorporata e sviluppatori avanzati potrebbero voler eseguire questa operazione comunque, ma per la maggior parte degli sviluppatori di gestione di queste scadenze può aumentare noiosa.

Per semplificare questa operazione per il pubblico di sviluppatore, il pacchetto [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contiene API dell'utilità per la creazione di payload che scadono automaticamente dopo un periodo di tempo. Queste API di blocco all'esterno del `ITimeLimitedDataProtector` tipo.

## <a name="api-usage"></a>Utilizzo dell'API

Il `ITimeLimitedDataProtector` è l'interfaccia di base per la protezione e rimozione della protezione i payload di durata limitata o temporanee. Per creare un'istanza di un `ITimeLimitedDataProtector`, è necessario innanzitutto un'istanza di un normale [oggetto IDataProtector](xref:security/data-protection/consumer-apis/overview) costruito con uno scopo specifico. Una volta il `IDataProtector` istanza è disponibile, chiamare il `IDataProtector.ToTimeLimitedDataProtector` metodo di estensione per ottenere una protezione con le funzionalità di scadenza incorporata.

`ITimeLimitedDataProtector` espone i metodi di superficie e l'estensione API seguenti:

* CreateProtector (utilizzo generico di stringa): ITimeLimitedDataProtector - questa API è simile all'attuale `IDataProtectionProvider.CreateProtector` in quanto può essere utilizzato per creare [allo scopo di catene](xref:security/data-protection/consumer-apis/purpose-strings) da una protezione con durata limitata di radice.

* Proteggere (byte [] testo normale, scadenza DateTimeOffset): byte]

* Protect(byte[] plaintext, TimeSpan lifetime) : byte[]

* Proteggere (testo normale [] byte): byte]

* Protect(string plaintext, DateTimeOffset expiration) : string

* Protect(string plaintext, TimeSpan lifetime) : string

* Proteggere (testo normale stringa): stringa

Oltre alle principali `Protect` metodi che accettano solo il testo non crittografato, sono presenti nuovi overload che consentono di specificare la data di scadenza del payload. La data di scadenza può essere specificata come una data assoluta (tramite un `DateTimeOffset`) o come un'ora relativa (dal sistema corrente tempo, tramite un `TimeSpan`). Se viene chiamato un overload che non accetta una data di scadenza, il payload si presuppone che non scadono.

* Rimuovere la protezione (protectedData [] byte, di scadenza DateTimeOffset): byte]

* Unprotect(byte[] protectedData) : byte[]

* Unprotect(string protectedData, out DateTimeOffset expiration) : string

* Unprotect(string protectedData) : string

Il `Unprotect` metodi restituiscono i dati protetti originali. Se il payload non è ancora scaduto, la scadenza assoluta viene restituita come parametro insieme ai dati protetti originali out facoltativo. Se il payload è scaduto, tutti gli overload del metodo Unprotect genererà CryptographicException.

>[!WARNING]
> È consigliabile non usare queste API per proteggere i payload che richiedono la persistenza a lungo termine o indeterminata. "Posso permettersi per il payload protetti essere recuperati in modo permanente dopo un mese?" può essere usato come una buona norma; Se la risposta è Nessuno sviluppatore quindi necessario considerare le API alternative.

L'esempio seguente usa il [percorsi di codice non è l'inserimento delle dipendenze](xref:security/data-protection/configuration/non-di-scenarios) per creare un'istanza nel sistema di protezione dei dati. Per eseguire questo esempio, assicurarsi che prima di tutto è stato aggiunto un riferimento al pacchetto Microsoft.AspNetCore.DataProtection.Extensions.

[!code-csharp[](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
