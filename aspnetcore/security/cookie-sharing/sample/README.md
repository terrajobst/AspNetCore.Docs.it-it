# <a name="cookie-sharing-sample-app"></a>App di esempio la condivisione di cookie

L'esempio illustra la condivisione tra tre applicazioni che utilizzano l'autenticazione dei cookie cookie:

| Progetto                             | Descrizione |
| ----------------------------------- | ----------- |
| CookieAuth.Core                     | App ASP.NET Core 2.0 Razor pagine senza l'utilizzo di ASP.NET Identity Core |
| CookieAuthWithIdentity.Core         | Applicazione MVC ASP.NET Core 2.0 con identità di ASP.NET Core |
| CookieAuthWithIdentity.NETFramework | Applicazione MVC ASP.NET Framework 4.6.1 con identità di ASP.NET |

Istruzioni:

1. Eseguire l'app CookieAuth.Core. Registrazione di un utente. L'applicazione autentica l'utente quando l'utente è registrato. Disconnettere l'utente.
1. Nella stessa sessione del browser, eseguire l'app CookieAuthWithIdentity.Core. Registrare lo stesso utente utilizzata con l'app di base. L'applicazione autentica l'utente quando l'utente è registrato. Disconnettere l'utente.
1. Nella stessa sessione del browser, eseguire l'app CookieAuthWithIdentity.NETFramework. Se utilizzato con le altre App, registrare l'utente stesso. L'applicazione autentica l'utente quando l'utente è registrato. Disconnettere l'utente.
1. Accesso dell'utente a uno dei tre app. Il cookie di autenticazione è condivisa tra le app. Si noti che l'utente è connesso automaticamente in altri due app.
1. Disconnettersi da una qualsiasi delle App per utente. Si noti che l'utente viene automaticamente disconnesso le altre due applicazioni.

In questo esempio illustra le funzionalità descritte nel [cookie tra le app di condivisione](https://docs.microsoft.com/aspnet/core/security/cookie-sharing) argomento.
