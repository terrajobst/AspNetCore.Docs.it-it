# <a name="cookie-sharing-sample-app"></a>App di esempio di condivisione di cookie

L'esempio illustra cookie condivisione tra le tre App che usano l'autenticazione tramite cookie:

| Progetto                             | Descrizione |
| ----------------------------------- | ----------- |
| CookieAuth.Core                     | App ASP.NET Core Razor Pages senza l'utilizzo di ASP.NET Core Identity |
| CookieAuthWithIdentity.Core         | App ASP.NET Core MVC con ASP.NET Core Identity |
| CookieAuthWithIdentity.NETFramework | App del Framework ASP.NET MVC con ASP.NET Identity |

Istruzioni:

1. Eseguire l'app CookieAuth.Core. Registrazione di un utente. L'app autentica l'utente quando l'utente è registrato. Disconnettere l'utente.
1. Nella stessa sessione del browser, eseguire l'app CookieAuthWithIdentity.Core. Registrare lo stesso utente come quelle utilizzate con l'app di base. L'app autentica l'utente quando l'utente è registrato. Disconnettere l'utente.
1. Nella stessa sessione del browser, eseguire l'app CookieAuthWithIdentity.NETFramework. Registrare lo stesso account utente usato con le altre app. L'app autentica l'utente quando l'utente è registrato. Disconnettere l'utente.
1. Accesso dell'utente a uno qualsiasi dei tre app. Il cookie di autenticazione verrà condivisi tra le app. Si noti che l'utente viene connesso automaticamente in altri due app.
1. Disconnessione dell'utente da una qualsiasi delle app. Si noti che l'utente viene automaticamente connesso all'esterno di altri due app.

In questo esempio illustra le funzionalità descritte nel [condividere cookie tra le app](https://docs.microsoft.com/aspnet/core/security/cookie-sharing) argomento.
