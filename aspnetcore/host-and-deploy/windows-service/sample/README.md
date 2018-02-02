# <a name="custom-webhost-service-sample"></a><span data-ttu-id="64dcd-101">Esempio di servizio personalizzati WebHost</span><span class="sxs-lookup"><span data-stu-id="64dcd-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="64dcd-102">Questo esempio viene illustrato il modo consigliato per ospitare un'applicazione ASP.NET Core in Windows senza utilizzare IIS come servizio Windows.</span><span class="sxs-lookup"><span data-stu-id="64dcd-102">This sample shows the recommended way to host an ASP.NET Core app on Windows without using IIS as a Windows Service.</span></span> <span data-ttu-id="64dcd-103">In questo esempio illustra le funzionalità descritte in [ospitare un'applicazione ASP.NET Core in un servizio Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="64dcd-103">This sample demonstrates the features described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="64dcd-104">Istruzioni</span><span class="sxs-lookup"><span data-stu-id="64dcd-104">Instructions</span></span>

<span data-ttu-id="64dcd-105">L'applicazione di esempio è una semplice applicazione web MVC modificata seguendo le istruzioni di [ospitare un'applicazione ASP.NET Core in un servizio Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="64dcd-105">The sample app is a simple MVC web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="64dcd-106">Per eseguire l'app in un servizio, eseguire la procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="64dcd-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="64dcd-107">Creare una cartella a *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="64dcd-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="64dcd-108">Pubblicare l'app per la cartella con `dotnet publish --configuration Release --output c:\\svc`.</span><span class="sxs-lookup"><span data-stu-id="64dcd-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="64dcd-109">Il comando sposterà asset dell'app nella cartella, tra cui la `appsettings.json` file e `wwwroot` cartella con il relativo contenuto.</span><span class="sxs-lookup"><span data-stu-id="64dcd-109">The command will move the app's assets to the folder, including the required `appsettings.json` file and the `wwwroot` folder with its contents.</span></span>

1. <span data-ttu-id="64dcd-110">Aprire un **amministratore** shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="64dcd-110">Open an **administrator** command shell.</span></span>

1. <span data-ttu-id="64dcd-111">Eseguire i seguenti comandi:</span><span class="sxs-lookup"><span data-stu-id="64dcd-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

1. <span data-ttu-id="64dcd-112">In un browser, passare a `http://localhost:5000` per verificare che il servizio sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="64dcd-112">In a browser, go to `http://localhost:5000` to verify that the service is running.</span></span>

1. <span data-ttu-id="64dcd-113">Per arrestare il servizio, utilizzare il comando:</span><span class="sxs-lookup"><span data-stu-id="64dcd-113">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="64dcd-114">Se l'app non viene avviato fino come previsto durante l'esecuzione in un servizio, un modo rapido per rendere accessibili i messaggi di errore consiste nell'aggiungere un provider di log, ad esempio il [provider registro eventi di Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="64dcd-114">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="64dcd-115">Un'altra opzione consiste nel controllare il registro eventi dell'applicazione utilizzando il Visualizzatore eventi nel sistema.</span><span class="sxs-lookup"><span data-stu-id="64dcd-115">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="64dcd-116">Di seguito è ad esempio, un'eccezione non gestita per un errore di file non trovato nel registro eventi dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="64dcd-116">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

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
