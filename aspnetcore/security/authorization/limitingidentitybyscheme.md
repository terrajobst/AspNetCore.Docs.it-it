---
title: "Limitazione dell'identità dallo schema"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d3d6ca1b-b4b5-4bf7-898e-dcd90ec1bf8c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 2483c441da317a5c29b611b3a4910eae3c01fd7a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="limiting-identity-by-scheme"></a><span data-ttu-id="1ee12-103">Limitazione dell'identità dallo schema</span><span class="sxs-lookup"><span data-stu-id="1ee12-103">Limiting identity by scheme</span></span>

<a name=security-authorization-limiting-by-scheme></a>

<span data-ttu-id="1ee12-104">In alcuni scenari, ad esempio applicazioni a pagina singola è possibile finire con più metodi di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="1ee12-104">In some scenarios, such as Single Page Applications it is possible to end up with multiple authentication methods.</span></span> <span data-ttu-id="1ee12-105">Ad esempio, l'applicazione potrà usare l'autenticazione basata su cookie di accesso e l'autenticazione della connessione per le richieste di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1ee12-105">For example, your application may use cookie-based authentication to log in and bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="1ee12-106">In alcuni casi potrebbe essere più istanze di un middleware di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="1ee12-106">In some cases you may have multiple instances of an authentication middleware.</span></span> <span data-ttu-id="1ee12-107">Ad esempio, due cookie middlewares in cui una contiene un'identità di base e ne viene creata quando è attivata un'autenticazione a più fattori perché l'utente ha richiesto un'operazione che richiede una maggiore sicurezza.</span><span class="sxs-lookup"><span data-stu-id="1ee12-107">For example, two cookie middlewares where one contains a basic identity and one is created when a multi-factor authentication has triggered because the user requested an operation that requires extra security.</span></span>

<span data-ttu-id="1ee12-108">Gli schemi di autenticazione sono denominati quando middleware di autenticazione viene configurato durante l'autenticazione, ad esempio</span><span class="sxs-lookup"><span data-stu-id="1ee12-108">Authentication schemes are named when authentication middleware is configured during authentication, for example</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AuthenticationScheme = "Cookie",
    LoginPath = new PathString("/Account/Unauthorized/"),
    AccessDeniedPath = new PathString("/Account/Forbidden/"),
    AutomaticAuthenticate = false
});

app.UseBearerAuthentication(options =>
{
    options.AuthenticationScheme = "Bearer";
    options.AutomaticAuthenticate = false;
});
```

<span data-ttu-id="1ee12-109">In questa configurazione due middlewares di autenticazione sono stati aggiunti, uno per i cookie e uno per la connessione.</span><span class="sxs-lookup"><span data-stu-id="1ee12-109">In this configuration two authentication middlewares have been added, one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="1ee12-110">Quando si aggiungono più middleware di autenticazione, che è necessario assicurarsi che nessun middleware sia configurato per l'esecuzione automatica.</span><span class="sxs-lookup"><span data-stu-id="1ee12-110">When adding multiple authentication middleware you should ensure that no middleware is configured to run automatically.</span></span> <span data-ttu-id="1ee12-111">A questo scopo, impostare il `AutomaticAuthenticate` opzioni proprietà su false.</span><span class="sxs-lookup"><span data-stu-id="1ee12-111">You do this by setting the `AutomaticAuthenticate` options property to false.</span></span> <span data-ttu-id="1ee12-112">Se non è possibile eseguire il filtraggio dallo schema non funzionerà.</span><span class="sxs-lookup"><span data-stu-id="1ee12-112">If you fail to do this filtering by scheme will not work.</span></span>

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="1ee12-113">Selezionare lo schema con l'attributo Authorize</span><span class="sxs-lookup"><span data-stu-id="1ee12-113">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="1ee12-114">Il middleware di autenticazione non è configurato per eseguire automaticamente e creare un'identità, che è necessario, nel punto di autorizzazione selezionare quale middleware verrà utilizzato.</span><span class="sxs-lookup"><span data-stu-id="1ee12-114">As no authentication middleware is configured to automatically run and create an identity you must, at the point of authorization choose which middleware will be used.</span></span> <span data-ttu-id="1ee12-115">È il modo più semplice per selezionare il middleware desiderato per l'autorizzazione a utilizzare il `ActiveAuthenticationSchemes` proprietà.</span><span class="sxs-lookup"><span data-stu-id="1ee12-115">The simplest way to select the middleware you wish to authorize with is to use the `ActiveAuthenticationSchemes` property.</span></span> <span data-ttu-id="1ee12-116">Questa proprietà accetta un elenco delimitato da virgole degli schemi di autenticazione da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="1ee12-116">This property accepts a comma delimited list of Authentication Schemes to use.</span></span> <span data-ttu-id="1ee12-117">Ad esempio;</span><span class="sxs-lookup"><span data-stu-id="1ee12-117">For example;</span></span>

```csharp
[Authorize(ActiveAuthenticationSchemes = "Cookie,Bearer")]
public class MixedController : Controller
```

<span data-ttu-id="1ee12-118">Nell'esempio precedente il cookie e di connessione middlewares verrà eseguito e la possibilità di creare e aggiungere un'identità per l'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="1ee12-118">In the example above both the cookie and bearer middlewares will run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="1ee12-119">Specificando un unico schema verrà eseguita solo il middleware specificato.</span><span class="sxs-lookup"><span data-stu-id="1ee12-119">By specifying a single scheme only the specified middleware will run;</span></span>

```csharp
[Authorize(ActiveAuthenticationSchemes = "Bearer")]
```

<span data-ttu-id="1ee12-120">In questo caso è necessario eseguire solo il middleware con lo schema della connessione e le identità basata su cookie verrebbero ignorate.</span><span class="sxs-lookup"><span data-stu-id="1ee12-120">In this case only the middleware with the Bearer scheme would run, and any cookie based identities would be ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="1ee12-121">Lo schema con i criteri di selezione</span><span class="sxs-lookup"><span data-stu-id="1ee12-121">Selecting the scheme with policies</span></span>

<span data-ttu-id="1ee12-122">Se si preferisce specificare gli schemi desiderati in [criteri](policies.md#security-authorization-policies-based) è possibile impostare il `AuthenticationSchemes` raccolta quando si aggiunge il criterio.</span><span class="sxs-lookup"><span data-stu-id="1ee12-122">If you prefer to specify the desired schemes in [policy](policies.md#security-authorization-policies-based) you can set the `AuthenticationSchemes` collection when adding your policy.</span></span>

```csharp
options.AddPolicy("Over18", policy =>
{
    policy.AuthenticationSchemes.Add("Bearer");
    policy.RequireAuthenticatedUser();
    policy.Requirements.Add(new Over18Requirement());
});
```

<span data-ttu-id="1ee12-123">In questo esempio il Over18 criteri verranno eseguiti solo in base all'identità creati il `Bearer` middleware.</span><span class="sxs-lookup"><span data-stu-id="1ee12-123">In this example the Over18 policy will only run against the identity created by the `Bearer` middleware.</span></span>
