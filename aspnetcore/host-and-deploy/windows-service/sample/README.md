# <a name="custom-webhost-service-sample"></a>Esempio di servizio personalizzati WebHost

Questo esempio viene illustrato il modo consigliato per ospitare un'applicazione ASP.NET Core in Windows senza utilizzare IIS come servizio Windows. In questo esempio illustra le funzionalità descritte in [ospitare un'applicazione ASP.NET Core in un servizio Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

## <a name="instructions"></a>Istruzioni

L'applicazione di esempio è una semplice applicazione web MVC modificata seguendo le istruzioni di [ospitare un'applicazione ASP.NET Core in un servizio Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

Per eseguire l'app in un servizio, eseguire la procedura seguente:

1. Creare una cartella a *c:\svc*.

1. Pubblicare l'app per la cartella con `dotnet publish --configuration Release --output c:\\svc`. Il comando sposterà asset dell'app nella cartella, tra cui la `appsettings.json` file e `wwwroot` cartella con il relativo contenuto.

1. Aprire un **amministratore** shell dei comandi.

1. Eseguire i seguenti comandi:

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

1. In un browser, passare a `http://localhost:5000` per verificare che il servizio sia in esecuzione.

1. Per arrestare il servizio, utilizzare il comando:

   ```console
   sc stop MyService
   ```

Se l'app non viene avviato fino come previsto durante l'esecuzione in un servizio, un modo rapido per rendere accessibili i messaggi di errore consiste nell'aggiungere un provider di log, ad esempio il [provider registro eventi di Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog). Un'altra opzione consiste nel controllare il registro eventi dell'applicazione utilizzando il Visualizzatore eventi nel sistema. Di seguito è ad esempio, un'eccezione non gestita per un errore di file non trovato nel registro eventi dell'applicazione:

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
