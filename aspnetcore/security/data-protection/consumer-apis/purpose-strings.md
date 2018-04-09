---
title: Stringhe scopo in ASP.NET Core
author: rick-anderson
description: Informazioni sull'utilizzo delle stringhe scopo nelle API di protezione dei dati di ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: 8fe0020256d3a105b1968db693b0c667244957ec
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="purpose-strings-in-aspnet-core"></a>Stringhe scopo in ASP.NET Core

<a name="data-protection-consumer-apis-purposes"></a>

I componenti che utilizzano `IDataProtectionProvider` deve passare un univoco *scopi* parametro per il `CreateProtector` (metodo). Gli scopi *parametro* è implicito per la sicurezza del sistema di protezione dati, in quanto forniscono isolamento tra il servizio di crittografia consumer, anche se le chiavi di crittografia radice sono uguali.

Quando un consumer specifica una funzione, la stringa di scopo viene utilizzata con le chiavi di crittografia principale per derivare il sottochiavi crittografia univoche per tale utente. Ciò consente di isolare il consumer da tutti gli altri consumer di crittografia nell'applicazione: nessun altro componente può leggere il payload, ed è possibile leggere payload di qualsiasi altro componente. Questo isolamento esegue il rendering anche computazionale intere categorie di attacco del componente.

![Esempio di diagramma scopo](purpose-strings/_static/purposes.png)

Nel diagramma precedente, `IDataProtector` istanze A e B **Impossibile** reciproci payload, solo di leggere i propri.

La stringa scopo non deve necessariamente essere segreto. Semplicemente deve essere univoco nel senso che nessun altro componente ben progettato fornirà mai la stessa stringa scopo.

>[!TIP]
> Utilizzando il nome dello spazio dei nomi e il tipo del componente utilizza le API di protezione dati è una buona regola pratica, come in pratica, che queste informazioni non saranno mai in conflitto.
>
>Un componente creato Contoso che è responsabile per minting i token di connessione può usare Contoso.Security.BearerToken come stringa il relativo scopo. O, ancora meglio, utilizzare la Contoso.Security.BearerToken.v1 sotto forma di stringa relativo scopo. Aggiungendo il numero di versione consente a una versione futura di utilizzare Contoso.Security.BearerToken.v2 come scopo e le diverse versioni sarebbe completamente isolate una da altra come payload go.

Poiché il parametro scopi `CreateProtector` è una matrice di stringhe, il precedente potrebbe invece specificate come `[ "Contoso.Security.BearerToken", "v1" ]`. In questo modo la definizione di una gerarchia di scopi e offre la possibilità di scenari multi-tenancy con il sistema di protezione dati.

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> Componenti di consentire l'input utente non attendibile per essere l'unica fonte di input per la catena di scopi.
>
>Si consideri ad esempio un componente Contoso.Messaging.SecureMessage che è responsabile per l'archiviazione di messaggi protetti. Se il componente di messaggistica protetto deve chiamare `CreateProtector([ username ])`, un utente malintenzionato potrebbe creare un account con il nome utente "Contoso.Security.BearerToken" nel tentativo di ottenere il componente di chiamare `CreateProtector([ "Contoso.Security.BearerToken" ])`, causando inavvertitamente la messaggistica protetta sistema di payload menta che potrebbe essere interpretati come i token di autenticazione.
>
>Una catena di scopi migliorata per il componente di messaggistica sarebbe `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`, che fornisce isolamento appropriato.

L'isolamento fornito dai e i comportamenti delle `IDataProtectionProvider`, `IDataProtector`, e a scopo è le seguenti:

* Per un determinato `IDataProtectionProvider` oggetto, il `CreateProtector` metodo creerà un `IDataProtector` oggetto associato in modo univoco a entrambi il `IDataProtectionProvider` oggetto creato e il parametro scopi che è stato passato al metodo.

* Il parametro scopo non debba essere null. (Se scopi viene specificato sotto forma di matrice, ciò significa che la matrice non deve essere di lunghezza zero e tutti gli elementi della matrice devono essere non null.) Scopo una stringa vuota è tecnicamente consentito, ma è sconsigliato.

* Gli argomenti di due funzioni sono equivalenti se e solo se contengono le stesse stringhe (tramite un confronto ordinale) nello stesso ordine. Un argomento unico scopo è equivalente alla matrice a elemento singolo scopi corrispondente.

* Due `IDataProtector` oggetti sono equivalenti se e solo se vengono creati dalla equivalente `IDataProtectionProvider` oggetti con parametri scopi equivalente.

* Per un determinato `IDataProtector` oggetto, una chiamata a `Unprotect(protectedData)` restituirà originale `unprotectedData` se e solo se `protectedData := Protect(unprotectedData)` per un equivalente `IDataProtector` oggetto.

> [!NOTE]
> Non si sta esaminando il caso in cui alcuni componenti sceglie intenzionalmente una stringa scopo che è noto in conflitto con un altro componente. Tale componente essenzialmente ritenuto dannoso e il sistema non è lo scopo di fornire le garanzie di sicurezza nel caso in cui il codice dannoso è già in esecuzione all'interno del processo di lavoro.
