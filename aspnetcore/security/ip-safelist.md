---
title: Client IP safelist per ASP.NET Core
author: damienbod
description: Informazioni su come scrivere i filtri Middleware o azione per convalidare gli indirizzi IP remoti rispetto a un elenco di indirizzi IP approvati.
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: 286f199c0d9164fa70d511aba523210c85c2fdfd
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207732"
---
# <a name="client-ip-safelist-for-aspnet-core"></a>Client IP safelist per ASP.NET Core

Dal [Damien Bowden](https://twitter.com/damien_bod) e [Tom Dykstra](https://github.com/tdykstra)
 
Questo articolo illustra tre modi per implementare un safelist IP (noto anche come un elenco elementi consentiti) in un'app ASP.NET Core. È possibile usare:

* Middleware per controllare l'indirizzo IP remoto di ogni richiesta.
* Filtri dell'azione per controllare l'indirizzo IP remoto di richieste di controller specifici o i metodi di azione.
* Razor Pages i filtri per controllare l'indirizzo IP remoto di richieste di pagine Razor.

L'app di esempio illustra entrambi gli approcci. In ogni caso, una stringa che contiene gli indirizzi IP client riconosciuto viene archiviata in un'impostazione dell'app. Il middleware o il filtro analizza la stringa in un elenco e verifica se l'indirizzo IP remoto è nell'elenco. In caso contrario, viene restituito un codice di stato HTTP 403-accesso negato.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="the-safelist"></a>Il safelist

L'elenco viene configurato nel *appSettings. JSON* file. È un elenco delimitato da punto e virgola e può contenere indirizzi IPv4 e IPv6.

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a>Middleware

Il `Configure` metodo aggiunge il middleware e passa la stringa dell'elenco indirizzi attendibili in un parametro di costruttore.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=7)]

Il middleware analizza la stringa in una matrice e Cerca l'indirizzo IP remoto nella matrice. Se non viene trovato l'indirizzo IP remoto, il middleware restituisce HTTP 401 accesso negato. Questo processo di convalida viene ignorato per le richieste HTTP Get.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a>Filtro azioni

Se si desidera un safelist solo per i metodi di azione o controller specifici, usare un filtro azione. Di seguito è riportato un esempio: 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

Il filtro azioni viene aggiunto al contenitore dei servizi.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Il filtro è quindi utilizzabile in un controller o metodo di azione.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

Nell'app di esempio, viene applicato il filtro per il `Get` (metodo). In questo caso quando si testa l'app mediante l'invio di un `Get` API richiesta, l'attributo è la convalida di indirizzo IP del client. Quando si testa chiamando l'API con qualsiasi altro metodo HTTP, il middleware convalida l'indirizzo IP del client.

## <a name="razor-pages-filter"></a>Filtrare le pagine Razor 

Se si desidera un safelist per un'app Razor Pages, usare un filtro di Razor Pages. Di seguito è riportato un esempio: 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

Questo filtro è abilitato, aggiungerlo alla raccolta di filtri MVC.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

Quando si esegue l'app e richiedere una pagina Razor, il filtro di Razor Pages è convalida l'indirizzo IP del client.

## <a name="next-steps"></a>Passaggi successivi

[Altre informazioni sul Middleware di ASP.NET Core](xref:fundamentals/middleware/index).
