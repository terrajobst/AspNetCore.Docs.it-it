---
title: Indirizzi IP attendibili del client per ASP.NET Core
author: damienbod
description: Informazioni su come scrivere filtri middleware o azione per convalidare gli indirizzi IP remoti rispetto a un elenco di indirizzi IP approvati.
ms.author: riande
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: ca5b0f8088773027f7403120247cbeca8900bcf5
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034339"
---
# <a name="client-ip-safelist-for-aspnet-core"></a>Indirizzi IP attendibili del client per ASP.NET Core

Di [Damien Bowden](https://twitter.com/damien_bod) e [Tom Dykstra](https://github.com/tdykstra)
 
Questo articolo illustra tre modi per implementare un elenco di indirizzi IP attendibili (noto anche come whitelist) in un'app ASP.NET Core. È possibile usare:

* Middleware per controllare l'indirizzo IP remoto di ogni richiesta.
* Filtri azione per controllare l'indirizzo IP remoto delle richieste per controller o metodi di azione specifici.
* Razor Pages filtri per controllare l'indirizzo IP remoto delle richieste per Razor Pages.

In ogni caso, una stringa contenente gli indirizzi IP client approvati viene archiviata in un'impostazione dell'app. Il middleware o il filtro analizza la stringa in un elenco e controlla se l'IP remoto è nell'elenco. In caso contrario, viene restituito un codice di stato HTTP 403 Forbidden.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="the-safelist"></a>Elenchi di sicurezza

L'elenco è configurato nel file *appSettings. JSON* . Si tratta di un elenco delimitato da punti e virgola e può contenere indirizzi IPv4 e IPv6.

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a>Middleware

Il metodo `Configure` aggiunge il middleware e vi passa la stringa di attendibilità in un parametro del costruttore.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=10)]

Il middleware analizza la stringa in una matrice e cerca l'indirizzo IP remoto nella matrice. Se l'indirizzo IP remoto non viene trovato, il middleware restituisce HTTP 401 non consentito. Questo processo di convalida viene ignorato per le richieste HTTP Get.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a>Filtro azioni

Se si desidera un tipo di attendibilità solo per controller o metodi di azione specifici, utilizzare un filtro azioni. Di seguito è riportato un esempio: 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIpCheckFilter.cs)]

Il filtro azioni viene aggiunto al contenitore dei servizi.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Il filtro può quindi essere utilizzato su un controller o un metodo di azione.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

Nell'app di esempio, il filtro viene applicato al metodo `Get`. Quindi, quando si esegue il test dell'app inviando una richiesta API `Get`, l'attributo convalida l'indirizzo IP del client. Quando si esegue il test chiamando l'API con qualsiasi altro metodo HTTP, il middleware convalida l'IP del client.

## <a name="razor-pages-filter"></a>Filtro Razor Pages 

Per un'app Razor Pages, usare un filtro Razor Pages. Di seguito è riportato un esempio: 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIpCheckPageFilter.cs)]

Questo filtro viene abilitato aggiungendolo alla raccolta di filtri MVC.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

Quando si esegue l'app e si richiede una pagina Razor, il filtro Razor Pages convalida l'IP del client.

## <a name="next-steps"></a>Passaggi successivi

[Altre informazioni su ASP.NET Core middleware](xref:fundamentals/middleware/index).
