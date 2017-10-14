---
title: Scopo stringhe
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: c96ed361-c382-4980-8933-800e740cfc38
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: 799c3dc2768e264307783efafee626a346a9362c
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2017
---
# <a name="purpose-strings"></a>Scopo stringhe

<a name="data-protection-consumer-apis-purposes"></a>

I componenti che utilizzano IDataProtectionProvider devono passare un univoco *scopi* parametro al metodo CreateProtector. Gli scopi *parametro* è implicito per la sicurezza del sistema di protezione dati, in quanto forniscono isolamento tra il servizio di crittografia consumer, anche se le chiavi di crittografia radice sono uguali.

Quando un consumer specifica una funzione, la stringa di scopo viene utilizzata con le chiavi di crittografia principale per derivare il sottochiavi crittografia univoche per tale utente. Ciò consente di isolare il consumer da tutti gli altri consumer di crittografia nell'applicazione: nessun altro componente può leggere il payload, ed è possibile leggere payload di qualsiasi altro componente. Questo isolamento esegue il rendering anche computazionale intere categorie di attacco del componente.

![Esempio di diagramma scopo](purpose-strings/_static/purposes.png)

Nel diagramma precedente le istanze di oggetto IDataProtector A e B **Impossibile** reciproci payload, solo di leggere i propri.

La stringa scopo non deve necessariamente essere segreto. Semplicemente deve essere univoco nel senso che nessun altro componente ben progettato fornirà mai la stessa stringa scopo.

>[!TIP]
> Utilizzando il nome dello spazio dei nomi e il tipo del componente utilizza le API di protezione dati è una buona regola pratica, come in pratica, che queste informazioni non saranno mai in conflitto.
>
>Un componente creato Contoso che è responsabile per minting i token di connessione può usare Contoso.Security.BearerToken come stringa il relativo scopo. O, ancora meglio, utilizzare la Contoso.Security.BearerToken.v1 sotto forma di stringa relativo scopo. Aggiungendo il numero di versione consente a una versione futura di utilizzare Contoso.Security.BearerToken.v2 come scopo e le diverse versioni sarebbe completamente isolate una da altra come payload go.

Poiché il parametro scopi CreateProtector è una matrice di stringhe, il precedente stato sono stati invece specificati come ["Contoso.Security.BearerToken", "v1"]. In questo modo la definizione di una gerarchia di scopi e offre la possibilità di scenari multi-tenancy con il sistema di protezione dati.

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> I componenti non devono consentire input utente non attendibile per essere l'unica fonte di input per la catena di scopi.
>
>Si consideri ad esempio un componente Contoso.Messaging.SecureMessage che è responsabile per l'archiviazione di messaggi protetti. Se il componente di messaggistica protetto sono stati chiamare CreateProtector ([username]), quindi un utente malintenzionato potrebbe creare un account con il nome utente "Contoso.Security.BearerToken" nel tentativo di ottenere il componente di chiamare CreateProtector([" Contoso.Security.BearerToken"]), pertanto al sistema di messaggistica sicuro di payload menta inavvertitamente causando che potrebbe essere interpretato come i token di autenticazione.
>
>Una catena di scopi migliorata per il componente di messaggistica sarebbe CreateProtector (["Contoso.Messaging.SecureMessage", "utente: nome utente"]), che fornisce isolamento appropriato.

L'isolamento fornito dai e i comportamenti di IDataProtectionProvider, l'oggetto IDataProtector e scopi sono come segue:

* Per un determinato oggetto IDataProtectionProvider, il metodo CreateProtector creerà un oggetto oggetto IDataProtector collegato in modo univoco l'oggetto IDataProtectionProvider cui è stato creato sia il parametro scopi che è stato passato al metodo.

* Il parametro scopo non debba essere null. (Se scopi viene specificato sotto forma di matrice, ciò significa che la matrice non deve essere di lunghezza zero e tutti gli elementi della matrice devono essere non null.) Scopo una stringa vuota è tecnicamente consentito, ma è sconsigliato.

* Gli argomenti di due funzioni sono equivalenti se e solo se contengono le stesse stringhe (tramite un confronto ordinale) nello stesso ordine. Un argomento unico scopo è equivalente alla matrice a elemento singolo scopi corrispondente.

* Due oggetti oggetto IDataProtector sono equivalenti se e solo se vengono creati da oggetti IDataProtectionProvider equivalenti con parametri scopi equivalente.

* Per un determinato oggetto oggetto IDataProtector, una chiamata a Unprotect(protectedData) restituirà il unprotectedData originale se e solo se protectedData: = Protect(unprotectedData) per un oggetto oggetto IDataProtector equivalente.

> [!NOTE]
> Non si sta esaminando il caso in cui alcuni componenti sceglie intenzionalmente una stringa scopo che è noto in conflitto con un altro componente. Tale componente essenzialmente ritenuto dannoso e il sistema non è previsto per fornire le garanzie di sicurezza nel caso in cui il codice dannoso è già in esecuzione all'interno del processo di lavoro.
