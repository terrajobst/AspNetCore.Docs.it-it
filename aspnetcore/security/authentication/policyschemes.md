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
# <a name="policy-schemes-in-aspnet-core"></a><span data-ttu-id="da870-103">Schemi di criteri in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="da870-103">Policy schemes in ASP.NET Core</span></span>

<span data-ttu-id="da870-104">Gli schemi dei criteri di autenticazione rendono più semplice avere uno schema di autenticazione logico singolo potenzialmente usare più approcci.</span><span class="sxs-lookup"><span data-stu-id="da870-104">Authentication policy schemes make it easier to have a single logical authentication scheme potentially use multiple approaches.</span></span> <span data-ttu-id="da870-105">Ad esempio, una combinazione di criteri potrebbe utilizzare l'autenticazione di Google per problematiche connesse e autenticazione dei cookie per tutto il resto.</span><span class="sxs-lookup"><span data-stu-id="da870-105">For example, a policy scheme might use Google authentication for challenges, and cookie authentication for everything else.</span></span> <span data-ttu-id="da870-106">Rendono gli schemi dei criteri di autenticazione:</span><span class="sxs-lookup"><span data-stu-id="da870-106">Authentication policy schemes make it:</span></span>

* <span data-ttu-id="da870-107">Consente di inoltrare alcuna azione di autenticazione in un altro schema.</span><span class="sxs-lookup"><span data-stu-id="da870-107">Easy to forward any authentication action to another scheme.</span></span>
* <span data-ttu-id="da870-108">Forward-only in modo dinamico in base alla richiesta.</span><span class="sxs-lookup"><span data-stu-id="da870-108">Forward dynamically based on the request.</span></span>

<span data-ttu-id="da870-109">Tutti gli schemi di autenticazione che usa derivato <xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions> associate [ `AuthenticationHandler<TOptions>` ](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1):</span><span class="sxs-lookup"><span data-stu-id="da870-109">All authentication schemes that use derived <xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions> and the associated [`AuthenticationHandler<TOptions>`](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1):</span></span>

* <span data-ttu-id="da870-110">Vengono automaticamente gli schemi di criteri in ASP.NET Core 2.1 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="da870-110">Are automatically policy schemes in ASP.NET Core 2.1 and later.</span></span>
* <span data-ttu-id="da870-111">Può essere abilitata tramite la configurazione delle opzioni di schema.</span><span class="sxs-lookup"><span data-stu-id="da870-111">Can be enabled via configuring the scheme's options.</span></span>

[!code-csharp[sample](policyschemes/samples/AuthenticationSchemeOptions.cs?name=snippet)]

## <a name="examples"></a><span data-ttu-id="da870-112">Esempi</span><span class="sxs-lookup"><span data-stu-id="da870-112">Examples</span></span>

<span data-ttu-id="da870-113">Nell'esempio seguente viene illustrato uno schema di livello superiore che combina gli schemi di livello inferiore.</span><span class="sxs-lookup"><span data-stu-id="da870-113">The following example shows a higher level scheme that combines lower level schemes.</span></span> <span data-ttu-id="da870-114">Viene utilizzata l'autenticazione di Google per problematiche connesse e l'autenticazione tramite cookie viene utilizzato per tutti gli altri:</span><span class="sxs-lookup"><span data-stu-id="da870-114">Google authentication is used for challenges, and cookie authentication is used for everything else:</span></span>

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet1)]

<span data-ttu-id="da870-115">L'esempio seguente abilita la selezione dinamica di schemi in una singola richiesta.</span><span class="sxs-lookup"><span data-stu-id="da870-115">The following example enables dynamic selection of schemes on a per request basis.</span></span> <span data-ttu-id="da870-116">Vale a dire come combinare l'autenticazione di API e i cookie:</span><span class="sxs-lookup"><span data-stu-id="da870-116">That is, how to mix cookies and API authentication:</span></span>

 <!-- REVIEW, missing If set in public Func<HttpContext, string> ForwardDefaultSelector -->

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet2)]
