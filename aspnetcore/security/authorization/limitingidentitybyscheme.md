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
# <a name="limiting-identity-by-scheme"></a>Limitazione dell'identità dallo schema

<a name=security-authorization-limiting-by-scheme></a>

In alcuni scenari, ad esempio applicazioni a pagina singola è possibile finire con più metodi di autenticazione. Ad esempio, l'applicazione potrà usare l'autenticazione basata su cookie di accesso e l'autenticazione della connessione per le richieste di JavaScript. In alcuni casi potrebbe essere più istanze di un middleware di autenticazione. Ad esempio, due cookie middlewares in cui una contiene un'identità di base e ne viene creata quando è attivata un'autenticazione a più fattori perché l'utente ha richiesto un'operazione che richiede una maggiore sicurezza.

Gli schemi di autenticazione sono denominati quando middleware di autenticazione viene configurato durante l'autenticazione, ad esempio

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

In questa configurazione due middlewares di autenticazione sono stati aggiunti, uno per i cookie e uno per la connessione.

>[!NOTE]
>Quando si aggiungono più middleware di autenticazione, che è necessario assicurarsi che nessun middleware sia configurato per l'esecuzione automatica. A questo scopo, impostare il `AutomaticAuthenticate` opzioni proprietà su false. Se non è possibile eseguire il filtraggio dallo schema non funzionerà.

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>Selezionare lo schema con l'attributo Authorize

Il middleware di autenticazione non è configurato per eseguire automaticamente e creare un'identità, che è necessario, nel punto di autorizzazione selezionare quale middleware verrà utilizzato. È il modo più semplice per selezionare il middleware desiderato per l'autorizzazione a utilizzare il `ActiveAuthenticationSchemes` proprietà. Questa proprietà accetta un elenco delimitato da virgole degli schemi di autenticazione da utilizzare. Ad esempio;

```csharp
[Authorize(ActiveAuthenticationSchemes = "Cookie,Bearer")]
public class MixedController : Controller
```

Nell'esempio precedente il cookie e di connessione middlewares verrà eseguito e la possibilità di creare e aggiungere un'identità per l'utente corrente. Specificando un unico schema verrà eseguita solo il middleware specificato.

```csharp
[Authorize(ActiveAuthenticationSchemes = "Bearer")]
```

In questo caso è necessario eseguire solo il middleware con lo schema della connessione e le identità basata su cookie verrebbero ignorate.

## <a name="selecting-the-scheme-with-policies"></a>Lo schema con i criteri di selezione

Se si preferisce specificare gli schemi desiderati in [criteri](policies.md#security-authorization-policies-based) è possibile impostare il `AuthenticationSchemes` raccolta quando si aggiunge il criterio.

```csharp
options.AddPolicy("Over18", policy =>
{
    policy.AuthenticationSchemes.Add("Bearer");
    policy.RequireAuthenticatedUser();
    policy.Requirements.Add(new Over18Requirement());
});
```

In questo esempio il Over18 criteri verranno eseguiti solo in base all'identità creati il `Bearer` middleware.
