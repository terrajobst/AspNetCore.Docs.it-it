---
title: Risolvere i problemi di progetti ASP.NET Core
author: Rick-Anderson
description: Riconoscere e risolvere i problemi di avvisi ed errori con i progetti ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: test/troubleshoot
ms.openlocfilehash: 1f53375a6aa504c989920a0bff83b400b7bd83ec
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208843"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="3aa7b-103">Risolvere i problemi di progetti ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3aa7b-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="3aa7b-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3aa7b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3aa7b-105">I collegamenti seguenti forniscono indicazioni sulla risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="3aa7b-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="3aa7b-106">NDC Conference (Londra, 2018): Diagnosi dei problemi nelle applicazioni ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3aa7b-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="3aa7b-107">Blog su ASP.NET: Risoluzione dei problemi di prestazioni di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3aa7b-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="3aa7b-108">Avvisi di .NET core SDK</span><span class="sxs-lookup"><span data-stu-id="3aa7b-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="3aa7b-109">Entrambe le versioni a 64 bit di .NET Core SDK e a 32 bit installate</span><span class="sxs-lookup"><span data-stu-id="3aa7b-109">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="3aa7b-110">Nel **nuovo progetto** finestra di dialogo per ASP.NET Core, si può vedere l'avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="3aa7b-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="3aa7b-111">Vengono installate sia 32 e 64 versioni di bit di .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-111">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="3aa7b-112">Solo i modelli dalle versioni a 64 bit installate in ' c:\\Program Files\\dotnet\\sdk\\' verranno visualizzati.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-112">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Screenshot della finestra di dialogo OneASP.NET viene visualizzato il messaggio di avviso](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="3aa7b-114">Questo avviso viene visualizzato quando (x86) 32 bit sia versioni a 64 bit (x64) del [.NET Core SDK](https://www.microsoft.com/net/download/all) siano installati.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="3aa7b-115">Cause più comuni che è possibile installare entrambe le versioni includono:</span><span class="sxs-lookup"><span data-stu-id="3aa7b-115">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="3aa7b-116">Si originariamente scaricato il programma di installazione di .NET Core SDK Usa un computer a 32 bit, ma quindi copiata quest'ultimo e viene installato in un computer a 64 bit.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="3aa7b-117">32 bit .NET Core SDK è stato installato da un'altra applicazione.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="3aa7b-118">La versione errata è stata scaricata e installata.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="3aa7b-119">Disinstallare il SDK di .NET Core a 32 bit per evitare questo avviso.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="3aa7b-120">Disinstallare dal **Pannello di controllo** > **programmi e funzionalità** > **Disinstalla o modifica programma**.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="3aa7b-121">Se si conosce il motivo per cui l'avviso viene generato e le relative implicazioni, è possibile ignorare l'avviso.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="3aa7b-122">.NET Core SDK è installato in più posizioni</span><span class="sxs-lookup"><span data-stu-id="3aa7b-122">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="3aa7b-123">Nel **nuovo progetto** finestra di dialogo per ASP.NET Core, si può vedere l'avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="3aa7b-123">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="3aa7b-124">.NET Core SDK è installato in più posizioni.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="3aa7b-125">Solo i modelli dagli SDK installati in ' c:\\Program Files\\dotnet\\sdk\\' verranno visualizzati.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-125">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Screenshot della finestra di dialogo OneASP.NET viene visualizzato il messaggio di avviso](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="3aa7b-127">Viene visualizzato questo messaggio quando si dispone di almeno un'installazione di .NET Core SDK in una directory fuori *c:\\Program Files\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="3aa7b-128">In genere ciò si verifica quando .NET Core SDK è stato distribuito in un computer tramite Copia/Incolla anziché il programma di installazione MSI.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-128">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="3aa7b-129">Disinstallare tutte le 32-bit .NET Core SDK e runtime per evitare questo avviso.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-129">Uninstall all 32-bit .NET Core SDKs and runtimes to prevent this warning.</span></span> <span data-ttu-id="3aa7b-130">Disinstallare dal **Pannello di controllo** > **programmi e funzionalità** > **Disinstalla o modifica programma**.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="3aa7b-131">Se si conosce il motivo per cui l'avviso viene generato e le relative implicazioni, è possibile ignorare l'avviso.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="3aa7b-132">Nessun SDK per .NET Core sono stati rilevati</span><span class="sxs-lookup"><span data-stu-id="3aa7b-132">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="3aa7b-133">Nel **nuovo progetto** finestra di dialogo per ASP.NET Core, si può vedere l'avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="3aa7b-133">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="3aa7b-134">Nessun SDK per .NET Core sono stati rilevati, assicurarsi che siano inclusi nella variabile di ambiente 'PATH'.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-134">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Screenshot della finestra di dialogo OneASP.NET viene visualizzato il messaggio di avviso](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="3aa7b-136">Questo avviso viene visualizzato quando la variabile di ambiente `PATH` non fa riferimento a qualsiasi SDK per .NET Core nel computer (ad esempio `C:\Program Files\dotnet\` e `C:\Program Files (x86)\dotnet\`).</span><span class="sxs-lookup"><span data-stu-id="3aa7b-136">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine (for example, `C:\Program Files\dotnet\` and `C:\Program Files (x86)\dotnet\`).</span></span> <span data-ttu-id="3aa7b-137">Per risolvere questo problema:</span><span class="sxs-lookup"><span data-stu-id="3aa7b-137">To resolve this problem:</span></span>

* <span data-ttu-id="3aa7b-138">Installare o verificare che sia installato .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-138">Install or verify the .NET Core SDK is installed.</span></span> <span data-ttu-id="3aa7b-139">Ottenere il programma di installazione più recente dal [.NET Downloads](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="3aa7b-139">Obtain the latest installer from [.NET Downloads](https://dotnet.microsoft.com/download).</span></span> 
* <span data-ttu-id="3aa7b-140">Verificare che il `PATH` variabile di ambiente punta alla posizione in cui è installato il SDK.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-140">Verify that the `PATH` environment variable points to the location where the SDK is installed.</span></span> <span data-ttu-id="3aa7b-141">Il programma di installazione è imposta in genere il `PATH`.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-141">The installer normally sets the `PATH`.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="3aa7b-142">Ottenere dati da un'app</span><span class="sxs-lookup"><span data-stu-id="3aa7b-142">Obtain data from an app</span></span>

<span data-ttu-id="3aa7b-143">Se un'app è in grado di rispondere alle richieste, è possibile ottenere i dati seguenti dell'App Usa il middleware:</span><span class="sxs-lookup"><span data-stu-id="3aa7b-143">If an app is capable of responding to requests, you can obtain the following data from the app using middleware:</span></span>

* <span data-ttu-id="3aa7b-144">Richiedere &ndash; stringa, le intestazioni di metodo, lo schema, host, pathbase, percorso, query</span><span class="sxs-lookup"><span data-stu-id="3aa7b-144">Request &ndash; Method, scheme, host, pathbase, path, query string, headers</span></span>
* <span data-ttu-id="3aa7b-145">Connessione &ndash; indirizzo IP remoto, porta remota, indirizzo IP locale, porta locale, il certificato client</span><span class="sxs-lookup"><span data-stu-id="3aa7b-145">Connection &ndash; Remote IP address, remote port, local IP address, local port, client certificate</span></span>
* <span data-ttu-id="3aa7b-146">Identità &ndash; Name, nome visualizzato</span><span class="sxs-lookup"><span data-stu-id="3aa7b-146">Identity &ndash; Name, display name</span></span>
* <span data-ttu-id="3aa7b-147">Impostazioni di configurazione</span><span class="sxs-lookup"><span data-stu-id="3aa7b-147">Configuration settings</span></span>
* <span data-ttu-id="3aa7b-148">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="3aa7b-148">Environment variables</span></span>

<span data-ttu-id="3aa7b-149">Inserire quanto segue [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) all'inizio del codice di `Startup.Configure` pipeline di elaborazione di richiesta del metodo.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-149">Place the following [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code at the beginning of the `Startup.Configure` method's request processing pipeline.</span></span> <span data-ttu-id="3aa7b-150">L'ambiente viene verificata prima che il middleware viene eseguito per garantire che il codice venga eseguito solo nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-150">The environment is checked before the middleware is run to ensure that the code is only executed in the Development environment.</span></span>

<span data-ttu-id="3aa7b-151">Per ottenere l'ambiente, utilizzare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="3aa7b-151">To obtain the environment, use either of the following approaches:</span></span>

* <span data-ttu-id="3aa7b-152">Inserire il `IHostingEnvironment` nella `Startup.Configure` (metodo) e controllare l'ambiente con la variabile locale.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-152">Inject the `IHostingEnvironment` into the `Startup.Configure` method and check the environment with the local variable.</span></span> <span data-ttu-id="3aa7b-153">Esempio di codice seguente illustra questo approccio.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-153">The following sample code demonstrates this approach.</span></span>

* <span data-ttu-id="3aa7b-154">Assegnare l'ambiente a una proprietà nel `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="3aa7b-154">Assign the environment to a property in the `Startup` class.</span></span> <span data-ttu-id="3aa7b-155">Controllare l'ambiente utilizzando la proprietà (ad esempio, `if (Environment.IsDevelopment())`).</span><span class="sxs-lookup"><span data-stu-id="3aa7b-155">Check the environment using the property (for example, `if (Environment.IsDevelopment())`).</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, 
    IConfiguration config)
{
    if (env.IsDevelopment())
    {
        app.Run(async (context) =>
        {
            var sb = new StringBuilder();
            var nl = System.Environment.NewLine;
            var rule = string.Concat(nl, new string('-', 40), nl);
            var authSchemeProvider = app.ApplicationServices
                .GetRequiredService<IAuthenticationSchemeProvider>();

            sb.Append($"Request{rule}");
            sb.Append($"{DateTimeOffset.Now}{nl}");
            sb.Append($"{context.Request.Method} {context.Request.Path}{nl}");
            sb.Append($"Scheme: {context.Request.Scheme}{nl}");
            sb.Append($"Host: {context.Request.Headers["Host"]}{nl}");
            sb.Append($"PathBase: {context.Request.PathBase.Value}{nl}");
            sb.Append($"Path: {context.Request.Path.Value}{nl}");
            sb.Append($"Query: {context.Request.QueryString.Value}{nl}{nl}");

            sb.Append($"Connection{rule}");
            sb.Append($"RemoteIp: {context.Connection.RemoteIpAddress}{nl}");
            sb.Append($"RemotePort: {context.Connection.RemotePort}{nl}");
            sb.Append($"LocalIp: {context.Connection.LocalIpAddress}{nl}");
            sb.Append($"LocalPort: {context.Connection.LocalPort}{nl}");
            sb.Append($"ClientCert: {context.Connection.ClientCertificate}{nl}{nl}");

            sb.Append($"Identity{rule}");
            sb.Append($"User: {context.User.Identity.Name}{nl}");
            var scheme = await authSchemeProvider
                .GetSchemeAsync(IISDefaults.AuthenticationScheme);
            sb.Append($"DisplayName: {scheme?.DisplayName}{nl}{nl}");

            sb.Append($"Headers{rule}");
            foreach (var header in context.Request.Headers)
            {
                sb.Append($"{header.Key}: {header.Value}{nl}");
            }
            sb.Append(nl);

            sb.Append($"Websockets{rule}");
            if (context.Features.Get<IHttpUpgradeFeature>() != null)
            {
                sb.Append($"Status: Enabled{nl}{nl}");
            }
            else
            {
                sb.Append($"Status: Disabled{nl}{nl}");
            }

            sb.Append($"Configuration{rule}");
            foreach (var pair in config.AsEnumerable())
            {
                sb.Append($"{pair.Path}: {pair.Value}{nl}");
            }
            sb.Append(nl);

            sb.Append($"Environment Variables{rule}");
            var vars = System.Environment.GetEnvironmentVariables();
            foreach (var key in vars.Keys.Cast<string>().OrderBy(key => key, 
                StringComparer.OrdinalIgnoreCase))
            {
                var value = vars[key];
                sb.Append($"{key}: {value}{nl}");
            }

            context.Response.ContentType = "text/plain";
            await context.Response.WriteAsync(sb.ToString());
        });
    }
}
```
