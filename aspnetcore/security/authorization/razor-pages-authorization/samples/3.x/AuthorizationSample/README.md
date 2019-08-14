# <a name="aspnet-core-authorization-sample"></a>Esempio di autorizzazione ASP.NET Core

In questo esempio viene illustrato l'utilizzo di Razor Pages autorizzazione per convenzione. In questo esempio vengono illustrate le funzionalità descritte nell'argomento [Razor Pages convenzioni di autorizzazione](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization) .

L'autorizzazione dell'utente in questo esempio usa le funzionalità di autenticazione dei cookie descritte nell'argomento [usare l'autenticazione tramite cookie senza ASP.NET Core Identity](https://docs.microsoft.com/aspnet/core/security/authentication/cookie) . I concetti e gli esempi illustrati in questo argomento si applicano ugualmente alle app che usano ASP.NET Core identità. Per informazioni sull'uso di ASP.NET Core identità, vedere [Introduzione all'identità su ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/authentication/identity).

Usare l'indirizzo **maria.rodriguez@contoso.com** di posta elettronica per autenticare l'utente con qualsiasi password. L'utente viene autenticato nel `AuthenticateUser` metodo nel file pages */account/login. cshtml. cs* . In un esempio reale, l'utente viene autenticato a fronte di un database.

## <a name="examples-in-this-sample"></a>Esempi

| Funzionalità | Descrizione |
| --- | --- |
| [AuthorizePage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) | Aggiunge un [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) alla pagina con il percorso specificato. |
| [AuthorizeFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) | Aggiunge un [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a tutte le pagine di una cartella con il percorso specificato. |
| [AllowAnonymousToPage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) | Aggiunge un [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a una pagina con il percorso specificato. |
| [AllowAnonymousToFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | Aggiunge un [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a tutte le pagine di una cartella con il percorso specificato. |
