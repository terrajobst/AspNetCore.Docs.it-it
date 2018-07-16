# <a name="custom-webhost-service-sample"></a><span data-ttu-id="919b0-101">Esempio di servizio WebHost personalizzato</span><span class="sxs-lookup"><span data-stu-id="919b0-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="919b0-102">In questo esempio viene illustrato come ospitare un'app ASP.NET Core come servizio Windows senza usare IIS.</span><span class="sxs-lookup"><span data-stu-id="919b0-102">This sample shows how to host an ASP.NET Core app as a Windows Service without using IIS.</span></span> <span data-ttu-id="919b0-103">L'esempio illustra lo scenario descritto in [Ospitare un'app ASP.NET Core in un servizio Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="919b0-103">This sample demonstrates the scenario described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="919b0-104">Istruzioni</span><span class="sxs-lookup"><span data-stu-id="919b0-104">Instructions</span></span>

<span data-ttu-id="919b0-105">L'app di esempio è una app Web Razor Pages modificata in base alle istruzioni indicate in [Ospitare un'app ASP.NET Core in un servizio Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="919b0-105">The sample app is a Razor Pages web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="919b0-106">Per eseguire l'app in un servizio, effettuare i passaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="919b0-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="919b0-107">Creare una cartella in *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="919b0-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="919b0-108">Pubblicare l'app nella cartella con `dotnet publish --configuration Release --output c:\\svc`.</span><span class="sxs-lookup"><span data-stu-id="919b0-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="919b0-109">Il comando sposta nella cartella *svc* gli asset dell'app, tra cui il file `appsettings.json` richiesto e la cartella `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="919b0-109">The command moves the app's assets to the *svc* folder, including the required `appsettings.json` file and the `wwwroot` folder.</span></span>

1. <span data-ttu-id="919b0-110">Aprire un prompt dei comandi come **amministratore**.</span><span class="sxs-lookup"><span data-stu-id="919b0-110">Open an **administrator** command prompt.</span></span>

1. <span data-ttu-id="919b0-111">Eseguire i seguenti comandi:</span><span class="sxs-lookup"><span data-stu-id="919b0-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  <span data-ttu-id="919b0-112">*Lo spazio tra il segno di uguale e l'inizio della stringa del percorso è obbligatorio.*</span><span class="sxs-lookup"><span data-stu-id="919b0-112">*The space between the equal sign and the start of the path string is required.*</span></span>

1. <span data-ttu-id="919b0-113">In un browser passare a `http://localhost:5000` e verificare che il servizio sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="919b0-113">In a browser, navigate to `http://localhost:5000` and verify that the service is running.</span></span> <span data-ttu-id="919b0-114">L'app reindirizza all'endpoint `https://localhost:5001` protetto.</span><span class="sxs-lookup"><span data-stu-id="919b0-114">The app redirects to the secure endpoint `https://localhost:5001`.</span></span>

1. <span data-ttu-id="919b0-115">Per arrestare il servizio, usare il comando:</span><span class="sxs-lookup"><span data-stu-id="919b0-115">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="919b0-116">Se l'app non viene avviata come previsto, è possibile accedere velocemente ai messaggi di errore aggiungendo un provider di registrazione, ad esempio il [provider EventLog di Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="919b0-116">If the app doesn't start as expected, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="919b0-117">Un'altra opzione consiste nel controllare il log eventi dell'applicazione tramite il Visualizzatore eventi nel sistema.</span><span class="sxs-lookup"><span data-stu-id="919b0-117">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="919b0-118">Ad esempio, di seguito è riportata un'eccezione non gestita per un errore FileNotFound nel log eventi dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="919b0-118">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

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
