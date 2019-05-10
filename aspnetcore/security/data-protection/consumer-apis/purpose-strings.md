---
title: Stringhe di scopi in ASP.NET Core
author: rick-anderson
description: Informazioni sull'utilizzo delle stringhe di scopi in ASP.NET Core Data Protection API.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: 4c85423f8de7e4b784ae1bb304a884541df251b6
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087542"
---
# <a name="purpose-strings-in-aspnet-core"></a>Stringhe di scopi in ASP.NET Core

<a name="data-protection-consumer-apis-purposes"></a>

I componenti che usano `IDataProtectionProvider` deve passare un valore univoco *scopi* parametro per il `CreateProtector` (metodo). Gli scopi *parametro* sono inerenti alla sicurezza del sistema di protezione dati, in quanto forniscono isolamento tra i consumer di crittografia, anche se le chiavi di crittografia radice sono uguali.

Quando un consumer specifica uno scopo, la stringa scopo viene utilizzata con le chiavi di crittografia principale per derivare le sottochiavi di crittografia univoche per tale utente. Ciò consente di isolare consumer da tutte le altri consumer di crittografia nell'applicazione: nessun altro componente può leggere il payload e non può leggere payload di qualsiasi altro componente. Questo isolamento viene eseguito il rendering anche computazionale, categorie di intere di attacco contro il componente.

![Esempio di diagramma utilizzo generico](purpose-strings/_static/purposes.png)

Nel diagramma precedente, `IDataProtector` istanze A e B **Impossibile** leggere reciproci payload, solo i propri.

La stringa scopo non deve essere privata. È sufficiente deve essere univoco nel senso che nessun altro componente ben progettato fornirà mai la stessa stringa scopo.

>[!TIP]
> Usando il nome dello spazio dei nomi e il tipo del componente utilizzando l'API di protezione dati è una buona norma, come in pratica che queste informazioni non saranno mai in conflitto.
>
>Un componente creato Contoso che è responsabile minting i token di connessione potrebbe usare Contoso.Security.BearerToken sotto forma di stringa relativo scopo. O, ancora meglio, è possibile che usi Contoso.Security.BearerToken.v1 sotto forma di stringa relativo scopo. Aggiungendo il numero di versione consente a una versione futura di utilizzare Contoso.Security.BearerToken.v2 del relativo scopo e le diverse versioni sarebbe completamente isolate l'una da altra per quanto riguarda i payload di go.

Poiché il parametro scopi `CreateProtector` è una matrice di stringhe, quello precedente è stato invece specificate come `[ "Contoso.Security.BearerToken", "v1" ]`. Questo permette di stabilire una gerarchia di scopi e offre la possibilità di scenari multi-tenancy con il sistema di protezione dati.

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> Componenti non devono consentire l'input utente non attendibile per essere l'unica fonte di input per la catena di scopi.
>
>Si consideri, ad esempio, un componente Contoso.Messaging.SecureMessage che è responsabile per l'archiviazione di messaggi protetti. Se il componente di messaggistica sicuro chiamare `CreateProtector([ username ])`, quindi un utente malintenzionato potrebbe creare un account con il nome utente "Contoso.Security.BearerToken" nel tentativo di ottenere il componente chiamare `CreateProtector([ "Contoso.Security.BearerToken" ])`, con conseguente inavvertitamente la messaggistica protetta sistema mint payload che può essere percepita come i token di autenticazione.
>
>Una catena di motivi migliorata per il componente di messaggistica sarebbe `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`, che offre isolamento appropriato.

L'isolamento fornito dai e i comportamenti delle `IDataProtectionProvider`, `IDataProtector`, e sono i seguenti scopi:

* Per un determinato `IDataProtectionProvider` oggetto, il `CreateProtector` metodo creerà una `IDataProtector` oggetto associato in modo univoco a entrambi il `IDataProtectionProvider` oggetto che ha creato e il parametro scopi che è stato passato al metodo.

* Il parametro scopo non deve essere null. (Se scopi è specificato come matrice, ciò significa che la matrice non deve essere di lunghezza zero e tutti gli elementi della matrice devono essere non null.) Uno scopo di una stringa vuota è tecnicamente consentito, ma è sconsigliato.

* A scopo di due argomenti è equivalenti se e solo se contengono le stesse stringhe (tramite un confronto ordinale) nello stesso ordine. Un argomento unico scopo è equivalente alla matrice a elemento singolo scopo corrispondente.

* Due `IDataProtector` gli oggetti sono equivalenti se e solo se vengono create da equivalente `IDataProtectionProvider` oggetti con i parametri scopi equivalente.

* Per un determinato `IDataProtector` object, una chiamata a `Unprotect(protectedData)` restituirà originale `unprotectedData` solo se `protectedData := Protect(unprotectedData)` per un equivalente `IDataProtector` oggetto.

> [!NOTE]
> Microsoft non stiamo prendere in considerazione il caso in cui alcuni componenti sceglie intenzionalmente una stringa di utilizzo generico che è noto che sono in conflitto con un altro componente. Tale componente viene essenzialmente considerato dannoso, e questo sistema non è destinato a fornire garanzie di sicurezza in caso di codice dannoso è già in esecuzione all'interno del processo di lavoro.
