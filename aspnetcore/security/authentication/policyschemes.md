---
title: Schemi di criteri in ASP.NET Core
author: rick-anderson
description: Gli schemi dei criteri di autenticazione rendono più semplice avere uno schema di autenticazione logico singolo
ms.author: riande
ms.date: 2/28/2019
uid: security/authentication/policyschemes
ms.openlocfilehash: 1a2d92e6fa54189b8154fc501b31c8a99d1f9081
ms.sourcegitcommit: 357a7120632b20465801c093e4e5bd4a315496a8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2019
ms.locfileid: "67649184"
---
# <a name="policy-schemes-in-aspnet-core"></a>Schemi di criteri in ASP.NET Core

Gli schemi dei criteri di autenticazione rendono più semplice avere uno schema di autenticazione logico singolo potenzialmente usare più approcci. Ad esempio, una combinazione di criteri potrebbe utilizzare l'autenticazione di Google per problematiche connesse e autenticazione dei cookie per tutto il resto. Rendono gli schemi dei criteri di autenticazione:

* Consente di inoltrare alcuna azione di autenticazione in un altro schema.
* Forward-only in modo dinamico in base alla richiesta.

Tutti gli schemi di autenticazione che usa derivato <xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions> associate [ `AuthenticationHandler<TOptions>` ](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1):

* Vengono automaticamente gli schemi di criteri in ASP.NET Core 2.1 e versioni successive.
* Può essere abilitata tramite la configurazione delle opzioni di schema.

[!code-csharp[sample](policyschemes/samples/AuthenticationSchemeOptions.cs?name=snippet)]

## <a name="examples"></a>Esempi

Nell'esempio seguente viene illustrato uno schema di livello superiore che combina gli schemi di livello inferiore. Viene utilizzata l'autenticazione di Google per problematiche connesse e l'autenticazione tramite cookie viene utilizzato per tutti gli altri:

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet1)]

L'esempio seguente abilita la selezione dinamica di schemi in una singola richiesta. Vale a dire come combinare l'autenticazione di API e i cookie:

 <!-- REVIEW, missing If set in public Func<HttpContext, string> ForwardDefaultSelector -->

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet2)]
