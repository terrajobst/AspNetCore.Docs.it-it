# <a name="custom-webhost-service-sample"></a>Esempio di servizio WebHost personalizzato

In questo esempio viene illustrato come ospitare un'app ASP.NET Core come servizio Windows senza usare IIS. L'esempio illustra lo scenario descritto in [Ospitare un'app ASP.NET Core in un servizio Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

## <a name="instructions"></a>Istruzioni

L'app di esempio è una app Web Razor Pages modificata in base alle istruzioni indicate in [Ospitare un'app ASP.NET Core in un servizio Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

Per eseguire l'app in un servizio, effettuare i passaggi seguenti:

1. Creare una cartella in *c:\svc*.

1. Pubblicare l'app nella cartella con `dotnet publish --configuration Release --output c:\\svc`. Il comando sposta nella cartella *svc* gli asset dell'app, tra cui il file `appsettings.json` richiesto e la cartella `wwwroot`.

1. Aprire un prompt dei comandi come **amministratore**.

1. Eseguire i seguenti comandi:

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  *Lo spazio tra il segno di uguale e l'inizio della stringa del percorso è obbligatorio.*

1. In un browser passare a `http://localhost:5000` e verificare che il servizio sia in esecuzione. L'app reindirizza all'endpoint `https://localhost:5001` protetto.

1. Per arrestare il servizio, usare il comando:

   ```console
   sc stop MyService
   ```

Se l'app non viene avviata come previsto, è possibile accedere velocemente ai messaggi di errore aggiungendo un provider di registrazione, ad esempio il [provider EventLog di Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog). Un'altra opzione consiste nel controllare il log eventi dell'applicazione tramite il Visualizzatore eventi nel sistema. Ad esempio, di seguito è riportata un'eccezione non gestita per un errore FileNotFound nel log eventi dell'applicazione:

```console
Application: AspNetCoreService.exe
Framework Version: v4.0.30319
Description: The process was terminated due to an unhandled exception.
Exception Info: System.IO.FileNotFoundException
   at Microsoft.Extensions.Configuration.FileConfigurationProvider.Load(Boolean)
   at Microsoft.Extensions.Configuration.ConfigurationRoot..ctor(System.Collections.Generic.IList`1<Microsoft.Extensions.Configuration.IConfigurationProvider>)
   at Microsoft.Extensions.Configuration.ConfigurationBuilder.Build()
   ...
```
