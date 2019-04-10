---
ms.openlocfilehash: 41021e30ae85dd0ae42cbe6f1606727e21bd7707
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/02/2019
ms.locfileid: "58809384"
---
# <a name="aspnet-core-middleware-extensibility-sample"></a>Esempio di estendibilità del middleware di ASP.NET Core

Questo esempio illustra gli scenari descritti in [Attivazione del middleware basata su factory in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/middleware-extensibility).

L'app di esempio illustra il middleware attivato da:

* Convenzione. Per altre informazioni sull'attivazione del middleware basata su convenzione, vedere l'argomento [Middleware](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/).
* Un'implementazione [IMiddleware](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddleware). La classe [IMiddlewareFactory](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) predefinita attiva il middleware.

Le implementazioni del middleware funzionano in modo identico e registrano il valore specificato da un parametro di stringa di query (`key`). I middleware usano un contesto di database inserito, ovvero un servizio con ambito, per registrare il valore di stringa di query in un database in memoria.
