---
title: La condivisione dei cookie tra applicazioni
author: rick-anderson
description: Questo documento viene illustrato come lo stack di protezione dati supporta la condivisione dei cookie di autenticazione tra ASP.NET 4. x e le applicazioni ASP.NET Core.
keywords: ASP.NET Core,ASP.NET,cookies,Interop,cookie condivisione
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 9a7aae45-8e21-4c54-950c-3c29df6c1082
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: e92cc81f9362787b7b4bfb44ba26b82182826a59
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="sharing-cookies-between-applications"></a>La condivisione dei cookie tra applicazioni

Siti Web in genere costituiti da numerose singole applicazioni web funzionante tutti insieme harmoniously. Se uno sviluppatore di applicazioni deve fornire un'esperienza ottimale single sign-on, sarà spesso necessario tutte le applicazioni web diverse all'interno del sito per condividere i ticket di autenticazione reciproca.

Per supportare questo scenario, lo stack di protezione dati consente la condivisione di autenticazione dei cookie Katana e il ticket di autenticazione ASP.NET Core cookie.

## <a name="sharing-authentication-cookies-between-applications"></a>Condividere i cookie di autenticazione tra applicazioni

Per condividere i cookie di autenticazione tra due diverse applicazioni ASP.NET Core, configurare ciascuna applicazione che deve condividere i cookie, come indicato di seguito.

Nel configurare (metodo), utilizzare il CookieAuthenticationOptions per configurare il servizio di protezione dati per i cookie e di AuthenticationScheme in modo che corrisponda ASP.NET 4. x.

Se si utilizza l'identità:

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = "ApplicationCookie";
    var protectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"));
    options.Cookies.ApplicationCookie.DataProtectionProvider = protectionProvider;
    options.Cookies.ApplicationCookie.TicketDataFormat = new TicketDataFormat(protectionProvider.CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", "Cookies", "v2"));
});
```

Se si utilizza direttamente i cookie:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
});
```
   
Il `DataProtectionProvider` richiede il `Microsoft.AspNetCore.DataProtection.Extensions` pacchetto NuGet.

Quando utilizzato in questo modo, DirectoryInfo deve puntare a un percorso di archiviazione chiavi specificamente riservare per i cookie di autenticazione. Il middleware di autenticazione cookie userà l'implementazione fornita in modo esplicito di DataProtectionProvider, che ora è isolato dal sistema di protezione di dati utilizzato da altre parti dell'applicazione. Il nome dell'applicazione viene ignorato (intenzionalmente in questo caso, poiché si sta tentando di ottenere più applicazioni possono condividere payload).

>[!WARNING]
>È consigliabile configurare il DataProtectionProvider in modo che le chiavi vengono crittografate a riposo, come nell'esempio seguente.
>
>
>  ```csharp
>  app.UseCookieAuthentication(new CookieAuthenticationOptions
>  {
>      DataProtectionProvider = DataProtectionProvider.Create(
>          new DirectoryInfo(@"c:\shared-auth-ticket-keys\"),
>          configure =>
>          {
>              configure.ProtectKeysWithCertificate("thumbprint");
>          })
>  });
>  ```

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-applications"></a>Condividere i cookie di autenticazione tra ASP.NET 4. x e le applicazioni ASP.NET Core

Applicazioni di 4. x ASP.NET che utilizzano Katana cookie middleware di autenticazione possono essere configurate per generare i cookie di autenticazione che sono compatibili con il middleware di autenticazione ASP.NET Core cookie. In questo modo l'aggiornamento a fasi di singole applicazioni del sito di grandi dimensioni pur fornendo un unico segno smooth sull'esperienza nell'intero sito.

>[!TIP]
> È possibile indicare se l'applicazione esistente Usa middleware di autenticazione cookie Katana dall'esistenza di una chiamata a UseCookieAuthentication in Startup.Auth.cs del progetto. Progetti applicazione web ASP.NET 4. x creati con Visual Studio 2013 e versioni successive, utilizzare il middleware di autenticazione del cookie Katana per impostazione predefinita.

> [!NOTE]
> Applicazione ASP.NET 4. x deve avere come destinazione .NET Framework 4.5.1 o versioni successive, in caso contrario i pacchetti NuGet necessari installazione avrà esito negativo.

Per condividere i cookie di autenticazione tra applicazioni ASP.NET 4. x e le applicazioni ASP.NET Core, configurare l'applicazione ASP.NET di base, come descritto in precedenza, quindi configurare le applicazioni di ASP.NET 4. x seguendo la procedura seguente.

1.  Installare il pacchetto Microsoft.Owin.Security.Interop in ognuna delle applicazioni ASP.NET 4. x.

2.   In Startup.Auth.cs, individuare la chiamata a UseCookieAuthentication, che in genere avrà un aspetto simile al seguente.

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        // ...
    });
    ```
    
3.  Modificare la chiamata a UseCookieAuthentication come indicato di seguito, modificando il CookieName per corrispondere al nome utilizzato per il middleware di autenticazione ASP.NET Core cookie, fornendo un'istanza di un DataProtectionProvider che è stata inizializzata in un percorso di archiviazione delle chiavi, e impostata CookieManager ChunkingCookieManager interoperabilità il formato di suddivisione in blocchi è compatibile.

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
        CookieName = ".AspNetCore.Cookies",
        // CookieName = ".AspNetCore.ApplicationCookie", (if you're using identity)
        // CookiePath = "...", (if necessary)
        // ...
        TicketDataFormat = new AspNetTicketDataFormat(
            new DataProtectorShim(
                DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
                .CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                "Cookies", "v2"))),
        CookieManager = new ChunkingCookieManager()
    });
    ```
    DirectoryInfo contiene puntare alla stessa posizione di archiviazione a cui fa riferimento l'applicazione ASP.NET di base e deve essere configurato usando le stesse impostazioni.

In ASP.NET 4. x e le applicazioni ASP.NET Core siano configurate per condividere i cookie di autenticazione.

> [!NOTE]
> È necessario assicurarsi che il sistema di identità per ogni applicazione fa riferimento allo stesso database utente. In caso contrario il sistema di identità genererà errori in fase di esecuzione quando tenta di far corrispondere le informazioni nel cookie di autenticazione e le informazioni sul proprio database.
