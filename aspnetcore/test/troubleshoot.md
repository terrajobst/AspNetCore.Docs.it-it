---
title: Risolvere i problemi di progetti ASP.NET Core
author: Rick-Anderson
description: Riconoscere e risolvere i problemi di avvisi ed errori con i progetti ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2019
uid: test/troubleshoot
ms.openlocfilehash: bcec8a55a5111e1f3acf53ae2f57b45e6e609d25
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313680"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="027fd-103">Risolvere i problemi di progetti ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="027fd-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="027fd-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="027fd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="027fd-105">I collegamenti seguenti forniscono indicazioni sulla risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="027fd-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="027fd-106">NDC Conference (Londra, 2018): Diagnosi dei problemi nelle applicazioni ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="027fd-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="027fd-107">Blog su ASP.NET: Risoluzione dei problemi di prestazioni di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="027fd-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="027fd-108">Avvisi di .NET core SDK</span><span class="sxs-lookup"><span data-stu-id="027fd-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="027fd-109">Entrambe le versioni a 32 e 64 bit di .NET Core SDK vengono installate</span><span class="sxs-lookup"><span data-stu-id="027fd-109">Both the 32-bit and 64-bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="027fd-110">Nel **nuovo progetto** finestra di dialogo per ASP.NET Core, si può vedere l'avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="027fd-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="027fd-111">Le versioni a 32 bit sia 64 bit di .NET Core SDK vengono installate.</span><span class="sxs-lookup"><span data-stu-id="027fd-111">Both 32-bit and 64-bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="027fd-112">Solo i modelli rispetto alle versioni a 64 bit installate in ' c:\\Program Files\\dotnet\\sdk\\' vengono visualizzati.</span><span class="sxs-lookup"><span data-stu-id="027fd-112">Only templates from the 64-bit versions installed at 'C:\\Program Files\\dotnet\\sdk\\' are displayed.</span></span>

<span data-ttu-id="027fd-113">Questo avviso viene visualizzato quando (x86) 32 bit sia versioni a 64 bit (x64) del [.NET Core SDK](https://www.microsoft.com/net/download/all) siano installati.</span><span class="sxs-lookup"><span data-stu-id="027fd-113">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="027fd-114">Cause più comuni che è possibile installare entrambe le versioni includono:</span><span class="sxs-lookup"><span data-stu-id="027fd-114">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="027fd-115">Si originariamente scaricato il programma di installazione di .NET Core SDK Usa un computer a 32 bit, ma quindi copiata quest'ultimo e viene installato in un computer a 64 bit.</span><span class="sxs-lookup"><span data-stu-id="027fd-115">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="027fd-116">32 bit .NET Core SDK è stato installato da un'altra applicazione.</span><span class="sxs-lookup"><span data-stu-id="027fd-116">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="027fd-117">La versione errata è stata scaricata e installata.</span><span class="sxs-lookup"><span data-stu-id="027fd-117">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="027fd-118">Disinstallare il SDK di .NET Core a 32 bit per evitare questo avviso.</span><span class="sxs-lookup"><span data-stu-id="027fd-118">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="027fd-119">Disinstallare dal **Pannello di controllo** > **programmi e funzionalità** > **Disinstalla o modifica programma**.</span><span class="sxs-lookup"><span data-stu-id="027fd-119">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="027fd-120">Se si conosce il motivo per cui l'avviso viene generato e le relative implicazioni, è possibile ignorare l'avviso.</span><span class="sxs-lookup"><span data-stu-id="027fd-120">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="027fd-121">.NET Core SDK è installato in più posizioni</span><span class="sxs-lookup"><span data-stu-id="027fd-121">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="027fd-122">Nel **nuovo progetto** finestra di dialogo per ASP.NET Core, si può vedere l'avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="027fd-122">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="027fd-123">.NET Core SDK è installato in più posizioni.</span><span class="sxs-lookup"><span data-stu-id="027fd-123">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="027fd-124">Solo i modelli dagli SDK installati in ' c:\\Program Files\\dotnet\\sdk\\' vengono visualizzati.</span><span class="sxs-lookup"><span data-stu-id="027fd-124">Only templates from the SDKs installed at 'C:\\Program Files\\dotnet\\sdk\\' are displayed.</span></span>

<span data-ttu-id="027fd-125">Viene visualizzato questo messaggio quando si dispone di almeno un'installazione di .NET Core SDK in una directory fuori *c:\\Program Files\\dotnet\\sdk\\* .</span><span class="sxs-lookup"><span data-stu-id="027fd-125">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="027fd-126">In genere ciò si verifica quando .NET Core SDK è stato distribuito in un computer tramite Copia/Incolla anziché il programma di installazione MSI.</span><span class="sxs-lookup"><span data-stu-id="027fd-126">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="027fd-127">Disinstallare tutte le 32-bit .NET Core SDK e runtime per evitare questo avviso.</span><span class="sxs-lookup"><span data-stu-id="027fd-127">Uninstall all 32-bit .NET Core SDKs and runtimes to prevent this warning.</span></span> <span data-ttu-id="027fd-128">Disinstallare dal **Pannello di controllo** > **programmi e funzionalità** > **Disinstalla o modifica programma**.</span><span class="sxs-lookup"><span data-stu-id="027fd-128">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="027fd-129">Se si conosce il motivo per cui l'avviso viene generato e le relative implicazioni, è possibile ignorare l'avviso.</span><span class="sxs-lookup"><span data-stu-id="027fd-129">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="027fd-130">Nessun SDK per .NET Core sono stati rilevati</span><span class="sxs-lookup"><span data-stu-id="027fd-130">No .NET Core SDKs were detected</span></span>

* <span data-ttu-id="027fd-131">In Visual Studio **nuovo progetto** finestra di dialogo per ASP.NET Core, si può vedere l'avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="027fd-131">In the Visual Studio **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

  > <span data-ttu-id="027fd-132">Nessun SDK per .NET Core sono stati rilevati, assicurarsi che siano inclusi nella variabile di ambiente `PATH`.</span><span class="sxs-lookup"><span data-stu-id="027fd-132">No .NET Core SDKs were detected, ensure they are included in the environment variable `PATH`.</span></span>

* <span data-ttu-id="027fd-133">Quando si esegue un `dotnet` comando, viene visualizzato l'avviso come:</span><span class="sxs-lookup"><span data-stu-id="027fd-133">When executing a `dotnet` command, the warning appears as:</span></span>

  > <span data-ttu-id="027fd-134">Non è stato possibile trovare qualsiasi installato dotnet SDK.</span><span class="sxs-lookup"><span data-stu-id="027fd-134">It was not possible to find any installed dotnet SDKs.</span></span>

<span data-ttu-id="027fd-135">Questi avvisi vengono visualizzati quando la variabile di ambiente `PATH` non punta a qualsiasi SDK per .NET Core nel computer.</span><span class="sxs-lookup"><span data-stu-id="027fd-135">These warnings appear when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="027fd-136">Per risolvere questo problema:</span><span class="sxs-lookup"><span data-stu-id="027fd-136">To resolve this problem:</span></span>

* <span data-ttu-id="027fd-137">Installare .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="027fd-137">Install the .NET Core SDK.</span></span> <span data-ttu-id="027fd-138">Ottenere il programma di installazione più recente dal [.NET Downloads](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="027fd-138">Obtain the latest installer from [.NET Downloads](https://dotnet.microsoft.com/download).</span></span>
* <span data-ttu-id="027fd-139">Verificare che il `PATH` variabile di ambiente punta alla posizione in cui è installato il SDK (`C:\Program Files\dotnet\` 64-bit o x64 o `C:\Program Files (x86)\dotnet\` per 32-bit/x86).</span><span class="sxs-lookup"><span data-stu-id="027fd-139">Verify that the `PATH` environment variable points to the location where the SDK is installed (`C:\Program Files\dotnet\` for 64-bit/x64 or `C:\Program Files (x86)\dotnet\` for 32-bit/x86).</span></span> <span data-ttu-id="027fd-140">Il programma di installazione SDK imposta in genere il `PATH`.</span><span class="sxs-lookup"><span data-stu-id="027fd-140">The SDK installer normally sets the `PATH`.</span></span> <span data-ttu-id="027fd-141">Installare sempre lo stesso numero di bit del SDK e Runtime nello stesso computer.</span><span class="sxs-lookup"><span data-stu-id="027fd-141">Always install the same bitness SDKs and runtimes on the same machine.</span></span>

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a><span data-ttu-id="027fd-142">SDK mancante dopo aver installato il Bundle di Hosting di .NET Core</span><span class="sxs-lookup"><span data-stu-id="027fd-142">Missing SDK after installing the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="027fd-143">Installare il [Bundle di Hosting di .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) modifica le `PATH` quando installa il runtime di .NET Core in modo che punti alla versione a 32 bit (x86) di .NET Core (`C:\Program Files (x86)\dotnet\`).</span><span class="sxs-lookup"><span data-stu-id="027fd-143">Installing the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) modifies the `PATH` when it installs the .NET Core runtime to point to the 32-bit (x86) version of .NET Core (`C:\Program Files (x86)\dotnet\`).</span></span> <span data-ttu-id="027fd-144">Ciò può comportare mancante SDK quando i 32 bit (x86) di .NET Core `dotnet` comando viene usato ([SDK per .NET Core non sono state rilevate](#no-net-core-sdks-were-detected)).</span><span class="sxs-lookup"><span data-stu-id="027fd-144">This can result in missing SDKs when the 32-bit (x86) .NET Core `dotnet` command is used ([No .NET Core SDKs were detected](#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="027fd-145">Per risolvere questo problema, spostare `C:\Program Files\dotnet\` in una posizione precedente `C:\Program Files (x86)\dotnet\` nel `PATH`.</span><span class="sxs-lookup"><span data-stu-id="027fd-145">To resolve this problem, move `C:\Program Files\dotnet\` to a position before `C:\Program Files (x86)\dotnet\` on the `PATH`.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="027fd-146">Ottenere dati da un'app</span><span class="sxs-lookup"><span data-stu-id="027fd-146">Obtain data from an app</span></span>

<span data-ttu-id="027fd-147">Se un'app è in grado di rispondere alle richieste, è possibile ottenere i dati seguenti dell'App Usa il middleware:</span><span class="sxs-lookup"><span data-stu-id="027fd-147">If an app is capable of responding to requests, you can obtain the following data from the app using middleware:</span></span>

* <span data-ttu-id="027fd-148">Richiedere &ndash; stringa, le intestazioni di metodo, lo schema, host, pathbase, percorso, query</span><span class="sxs-lookup"><span data-stu-id="027fd-148">Request &ndash; Method, scheme, host, pathbase, path, query string, headers</span></span>
* <span data-ttu-id="027fd-149">Connessione &ndash; indirizzo IP remoto, porta remota, indirizzo IP locale, porta locale, il certificato client</span><span class="sxs-lookup"><span data-stu-id="027fd-149">Connection &ndash; Remote IP address, remote port, local IP address, local port, client certificate</span></span>
* <span data-ttu-id="027fd-150">Identità &ndash; Name, nome visualizzato</span><span class="sxs-lookup"><span data-stu-id="027fd-150">Identity &ndash; Name, display name</span></span>
* <span data-ttu-id="027fd-151">Impostazioni di configurazione</span><span class="sxs-lookup"><span data-stu-id="027fd-151">Configuration settings</span></span>
* <span data-ttu-id="027fd-152">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="027fd-152">Environment variables</span></span>

<span data-ttu-id="027fd-153">Inserire quanto segue [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) all'inizio del codice di `Startup.Configure` pipeline di elaborazione di richiesta del metodo.</span><span class="sxs-lookup"><span data-stu-id="027fd-153">Place the following [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code at the beginning of the `Startup.Configure` method's request processing pipeline.</span></span> <span data-ttu-id="027fd-154">L'ambiente viene verificata prima che il middleware viene eseguito per garantire che il codice venga eseguito solo nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="027fd-154">The environment is checked before the middleware is run to ensure that the code is only executed in the Development environment.</span></span>

<span data-ttu-id="027fd-155">Per ottenere l'ambiente, utilizzare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="027fd-155">To obtain the environment, use either of the following approaches:</span></span>

* <span data-ttu-id="027fd-156">Inserire il `IHostingEnvironment` nella `Startup.Configure` (metodo) e controllare l'ambiente con la variabile locale.</span><span class="sxs-lookup"><span data-stu-id="027fd-156">Inject the `IHostingEnvironment` into the `Startup.Configure` method and check the environment with the local variable.</span></span> <span data-ttu-id="027fd-157">Esempio di codice seguente illustra questo approccio.</span><span class="sxs-lookup"><span data-stu-id="027fd-157">The following sample code demonstrates this approach.</span></span>

* <span data-ttu-id="027fd-158">Assegnare l'ambiente a una proprietà nel `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="027fd-158">Assign the environment to a property in the `Startup` class.</span></span> <span data-ttu-id="027fd-159">Controllare l'ambiente utilizzando la proprietà (ad esempio, `if (Environment.IsDevelopment())`).</span><span class="sxs-lookup"><span data-stu-id="027fd-159">Check the environment using the property (for example, `if (Environment.IsDevelopment())`).</span></span>

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
