---
title: Applicazione di SSL in un'applicazione ASP.NET di base
author: rick-anderson
description: Viene illustrato come richiedere SSL in un ASP.NET Core app web
manager: wpickett
ms.author: riande
ms.date: 07/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 3b72cddb7a240ad6d6e1427796e9bb4f7003a3f7
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a>Applicazione di SSL in un'applicazione ASP.NET di base

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questo documento illustra come:

- Richiedere SSL per tutte le richieste (solo richieste HTTPS).
- Reindirizzare tutte le richieste HTTP a HTTPS.

## <a name="require-ssl"></a>Richiedere SSL

Il [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) viene utilizzato per richiedere SSL. È possibile decorare controller o i metodi con questo attributo o è possibile applicare a livello globale, come illustrato di seguito:

Aggiungere il seguente codice al `ConfigureServices` in `Startup`:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Il codice evidenziato sopra è necessario utilizzano tutte le richieste `HTTPS`, pertanto vengono ignorate le richieste HTTP. Il seguente codice evidenziato reindirizza tutte le richieste HTTP a HTTPS:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Vedere [Middleware di riscrittura URL](xref:fundamentals/url-rewriting) per ulteriori informazioni.

Che richiedono HTTPS a livello globale (`options.Filters.Add(new RequireHttpsAttribute());`) è una procedura consigliata. L'applicazione di `[RequireHttps]` attributo per tutti i controller non è considerata sicura come che richiedono HTTPS a livello globale. Non è possibile garantire nuovi controller aggiunto all'app verranno memorizzate applicare il `[RequireHttps]` attributo.
