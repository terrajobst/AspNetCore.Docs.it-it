# <a name="aspnet-core-web-api-controller-sample"></a>Esempio di controller API Web ASP.NET Core

L'app di esempio è costituita dai progetti seguenti:

- **WebApiSample.Api**: un progetto ASP.NET Core 2.1 destinato a .NET Core 2.1.
- **WebApiSample.Api.Pre21**: un progetto ASP.NET Core 2.0 destinato a .NET Core 2.0.
- **WebApiSample.DataAccess**: una libreria di classi .NET Standard 2.0 che funge da livello di accesso ai dati per i 2 progetti API Web.

Questo esempio illustra le varie possibilità di creazione del controller API Web:

- [Derivare una classe da ControllerBase](https://docs.microsoft.com/en-us/aspnet/core/web-api/define-controller#derive-class-from-controllerbase)
- [Annotare una classe con l'attributo ApiController](https://docs.microsoft.com/en-us/aspnet/core/web-api/define-controller#annotate-class-with-apicontrollerattribute)