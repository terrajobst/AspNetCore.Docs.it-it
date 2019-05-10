---
title: Impedire attacchi di reindirizzamento aperti in ASP.NET Core
author: ardalis
description: Viene illustrato come evitare attacchi di reindirizzamento aperti di un'app ASP.NET Core
ms.author: riande
ms.date: 07/07/2017
uid: security/preventing-open-redirects
ms.openlocfilehash: 0896189d2caaccb19647eb7c6d57f29dfc0290dd
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64891618"
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a>Impedire attacchi di reindirizzamento aperti in ASP.NET Core

Un'app web che reindirizza a un URL che viene specificato tramite la richiesta, ad esempio i dati della stringa di query o form potenzialmente può essere manomessi per reindirizzare gli utenti a un URL esterno, dannoso. Questo manomissioni, si tratta di un attacco di reindirizzamento aperti.

Ogni volta che la logica dell'applicazione reindirizza a un URL specificato, è necessario verificare che l'URL di reindirizzamento non è stato manomesso. ASP.NET Core ha funzionalità incorporate per proteggere le app da attacchi di reindirizzamento aperti (noto anche come aprire il reindirizzamento).

## <a name="what-is-an-open-redirect-attack"></a>Che cos'è un attacco di reindirizzamento aperti?

Le applicazioni Web spesso reindirizzare gli utenti a una pagina di accesso quando accedono alle risorse che richiedono l'autenticazione. Il reindirizzamento in genere include un `returnUrl` parametro querystring in modo che l'utente può essere restituito all'URL richiesto originariamente dopo che hanno eseguito l'accesso. Dopo che l'utente viene autenticato, si verrà reindirizzati all'URL richiesto originariamente.

Poiché l'URL di destinazione è specificato nella stringa di query della richiesta, un utente malintenzionato può manomettere la stringa di query. Una stringa di query alterato potrebbe consentire il sito reindirizzare l'utente a un sito esterno, dannoso. Questa tecnica viene chiamata un attacco di reindirizzamento (o il reindirizzamento) aperto.

### <a name="an-example-attack"></a>Un attacco di esempio

Un utente malintenzionato può sviluppare un attacco lo scopo di consentire all'utente malintenzionato l'accesso a credenziali o le informazioni riservate dell'utente. Per avviare l'attacco, l'utente malintenzionato induce l'utente di fare clic su un collegamento alla pagina di accesso del sito con un `returnUrl` valore querystring aggiunto all'URL. Si consideri ad esempio un'app all'indirizzo `contoso.com` che include una pagina di accesso a `http://contoso.com/Account/LogOn?returnUrl=/Home/About`. L'attacco segue questi passaggi:

1. L'utente seleziona il collegamento dannoso `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` (il secondo URL è "contoso**1**. com", non "contoso.com").
2. L'utente accede correttamente.
3. L'utente viene reindirizzato (dal sito) a `http://contoso1.com/Account/LogOn` (un sito dannoso che assomiglia esattamente a un sito vero e).
4. L'utente accede nuovamente (offrendo dannoso le proprie credenziali del sito) e viene reindirizzato al sito effettivo.

Probabilmente l'utente ritiene che il primo tentativo di accesso non è riuscito e che il secondo tentativo ha esito positivo. L'utente rimane molto probabilmente riconosce che le credenziali vengano compromesse.

![Processo di attacchi di reindirizzamento aperti](preventing-open-redirects/_static/open-redirection-attack-process.png)

Oltre alle pagine di accesso, alcuni siti forniscono pagine di reindirizzamento o gli endpoint. Si supponga l'app ha una pagina con un reindirizzamento aperto, `/Home/Redirect`. Un utente malintenzionato è stato possibile creare, ad esempio, un collegamento tramite posta elettronica che rimanda al `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`. Un utente tipico cercherà l'URL e vedere che inizia con il nome del sito. Concessione dell'attendibilità che, essi verranno fare clic sul collegamento. Il reindirizzamento aperto invierebbe quindi l'utente al sito di phishing, il cui aspetto è identico a quella dell'utente, e l'utente potrebbe accedere a ciò che si ritiene che è il sito.

## <a name="protecting-against-open-redirect-attacks"></a>La protezione da attacchi di reindirizzamento aperti

Durante lo sviluppo di applicazioni web, considera tutti i dati forniti dall'utente come non attendibili. Se l'applicazione include funzionalità che reindirizza l'utente in base al contenuto dell'URL, verificare che questi reindirizzamenti vengono eseguiti solo in locale all'interno dell'app (o a un URL noto, non qualsiasi URL che può essere fornito nella stringa di query).

### <a name="localredirect"></a>LocalRedirect

Usare la `LocalRedirect` metodo di supporto dalla base `Controller` classe:

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

`LocalRedirect` verrà generata un'eccezione se viene specificato un URL non locali. In caso contrario, si comporta esattamente come il `Redirect` (metodo).

### <a name="islocalurl"></a>IsLocalUrl

Usare la [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) metodo per testare gli URL prima di reindirizzamento:

Nell'esempio seguente viene illustrato come controllare se un URL è locale prima del reindirizzamento.

```csharp
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

Il `IsLocalUrl` metodo protegge gli utenti non venga inavvertitamente reindirizzato a un sito dannoso. È possibile registrare i dettagli dell'URL che è stato fornito quando viene fornito un URL non locali in una situazione in cui previsto un URL locale. La registrazione di reindirizzamento URL può essere utile per diagnosticare gli attacchi di reindirizzamento.
