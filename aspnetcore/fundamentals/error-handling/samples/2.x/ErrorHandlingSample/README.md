---
ms.openlocfilehash: b1f7c306533e70f84e73a020c74756b91cae7ea7
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665751"
---
# <a name="error-handling-sample-application"></a><span data-ttu-id="67151-101">Applicazione di esempio di gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="67151-101">Error Handling Sample Application</span></span>

<span data-ttu-id="67151-102">Questa app di esempio illustra gli scenari descritti in [Gestire gli errori in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="67151-102">This sample app demonstrates the scenarios described in the [Handle errors in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling) topic.</span></span>

## <a name="configure-a-custom-exception-handling-page"></a><span data-ttu-id="67151-103">Configurare una pagina di gestione delle eccezioni personalizzata</span><span class="sxs-lookup"><span data-stu-id="67151-103">Configure a custom exception handling page</span></span>

<span data-ttu-id="67151-104">Quando l'app non è in esecuzione nell'ambiente di sviluppo, il middleware di gestione delle eccezioni:</span><span class="sxs-lookup"><span data-stu-id="67151-104">When the app isn't running in the Development environment, Exception Handling Middleware:</span></span>

* <span data-ttu-id="67151-105">Intercetta le eccezioni.</span><span class="sxs-lookup"><span data-stu-id="67151-105">Catches exceptions.</span></span>
* <span data-ttu-id="67151-106">Registra le eccezioni.</span><span class="sxs-lookup"><span data-stu-id="67151-106">Logs exceptions.</span></span>
* <span data-ttu-id="67151-107">Esegue nuovamente la richiesta in una pipeline alternativa nel percorso specificato.</span><span class="sxs-lookup"><span data-stu-id="67151-107">Re-executes the request in an alternate pipeline at the path provided.</span></span>

## <a name="configure-custom-exception-handling-code"></a><span data-ttu-id="67151-108">Configurare codice di gestione delle eccezioni personalizzato</span><span class="sxs-lookup"><span data-stu-id="67151-108">Configure custom exception handling code</span></span>

<span data-ttu-id="67151-109">Un'alternativa al fornire un endpoint per gli errori con una pagina di gestione delle eccezioni personalizzata consiste nel fornire una funzione lambda a `UseExceptionHandler`.</span><span class="sxs-lookup"><span data-stu-id="67151-109">An alternative to serving an endpoint for errors with a custom exception handling page is to provide a lambda to `UseExceptionHandler`.</span></span> <span data-ttu-id="67151-110">L'uso di una funzione lambda con `UseExceptionHandler` consente l'accesso all'errore prima di restituire la risposta.</span><span class="sxs-lookup"><span data-stu-id="67151-110">Using a lambda with `UseExceptionHandler` allows access to the error before returning the response.</span></span>

<span data-ttu-id="67151-111">L'app di esempio illustra il codice di gestione delle eccezioni personalizzato in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="67151-111">The sample app demonstrates custom exception handling code in `Startup.Configure`.</span></span> <span data-ttu-id="67151-112">Seguire le istruzioni riportate all'inizio del file *Startup.cs* (`LambdaErrorHandler`).</span><span class="sxs-lookup"><span data-stu-id="67151-112">Follow the instructions at the top of the *Startup.cs* file (`LambdaErrorHandler`).</span></span> <span data-ttu-id="67151-113">Dopo l'avvio dell'app, generare un'eccezione tramite il collegamento **Throw Exception** (Genera eccezione) nella pagina Index.</span><span class="sxs-lookup"><span data-stu-id="67151-113">After the app starts, trigger an exception with the **Throw Exception** link on the Index page.</span></span>
