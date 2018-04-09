# <a name="aspnet-core-authorization-sample"></a>Esempio di autorizzazione ASP.NET Core

In questo esempio illustra l'utilizzo di autorizzazione pagine Razor dalle convenzioni. In questo esempio illustra le funzionalità descritte nel [convenzioni autorizzazione pagine Razor](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization) argomento.

Autorizzazione dell'utente in questo esempio viene utilizzata l'autenticazione di cookie caratteristiche descritte nella [Usa autenticazione di cookie senza ASP.NET Identity Core](https://docs.microsoft.com/aspnet/core/security/authentication/cookie) argomento. Per informazioni sull'utilizzo di ASP.NET Identity Core, vedere gli argomenti in di [autenticazione](https://docs.microsoft.com/aspnet/core/security/authentication/index) sezione della documentazione.

Quando si esegue l'esempio, usare l'indirizzo di posta elettronica **maria.rodriguez@contoso.com** per autenticare l'utente.

## <a name="examples-in-this-sample"></a>Esempi

|                                                                              Funzionalità                                                                               |                                                                                        Descrizione                                                                                         |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          [AuthorizePage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)          |                Aggiunge un [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) alla pagina con il percorso specificato.                |
|        [AuthorizeFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder)        |      Aggiunge un [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) per tutte le pagine in una cartella con il percorso specificato.      |
|   [AllowAnonymousToPage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage)   |            Aggiunge un [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a una pagina con il percorso specificato.            |
| [AllowAnonymousToFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | Aggiunge un [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) per tutte le pagine in una cartella con il percorso specificato. |

