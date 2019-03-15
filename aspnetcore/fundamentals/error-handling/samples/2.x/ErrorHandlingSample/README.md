---
ms.openlocfilehash: b1f7c306533e70f84e73a020c74756b91cae7ea7
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665751"
---
# <a name="error-handling-sample-application"></a>Applicazione di esempio di gestione degli errori

Questa app di esempio illustra gli scenari descritti in [Gestire gli errori in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling).

## <a name="configure-a-custom-exception-handling-page"></a>Configurare una pagina di gestione delle eccezioni personalizzata

Quando l'app non è in esecuzione nell'ambiente di sviluppo, il middleware di gestione delle eccezioni:

* Intercetta le eccezioni.
* Registra le eccezioni.
* Esegue nuovamente la richiesta in una pipeline alternativa nel percorso specificato.

## <a name="configure-custom-exception-handling-code"></a>Configurare codice di gestione delle eccezioni personalizzato

Un'alternativa al fornire un endpoint per gli errori con una pagina di gestione delle eccezioni personalizzata consiste nel fornire una funzione lambda a `UseExceptionHandler`. L'uso di una funzione lambda con `UseExceptionHandler` consente l'accesso all'errore prima di restituire la risposta.

L'app di esempio illustra il codice di gestione delle eccezioni personalizzato in `Startup.Configure`. Seguire le istruzioni riportate all'inizio del file *Startup.cs* (`LambdaErrorHandler`). Dopo l'avvio dell'app, generare un'eccezione tramite il collegamento **Throw Exception** (Genera eccezione) nella pagina Index.
