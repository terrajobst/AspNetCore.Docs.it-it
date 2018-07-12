---
title: Hosting di ASP.NET Core in Linux con Apache
description: Informazioni su come configurare Apache come server proxy inverso in CentOS per reindirizzare il traffico HTTP a un'app Web ASP.NET Core in esecuzione su Kestrel.
author: spboyer
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
uid: host-and-deploy/linux-apache
ms.openlocfilehash: d02fbd82be37e6d67214a9a0bf5851662b577cb9
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433974"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="841ca-103">Hosting di ASP.NET Core in Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="841ca-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="841ca-104">Di [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="841ca-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="841ca-105">Questa guida fornisce informazioni su come configurare [Apache](https://httpd.apache.org/) come server proxy inverso in [CentOS 7](https://www.centos.org/) per reindirizzare il traffico HTTP a un'app Web ASP.NET Core in esecuzione su [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="841ca-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="841ca-106">L'[estensione mod_proxy](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) e i moduli correlati creano il proxy inverso del server.</span><span class="sxs-lookup"><span data-stu-id="841ca-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="841ca-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="841ca-107">Prerequisites</span></span>

1. <span data-ttu-id="841ca-108">Server che esegue CentOS 7 con un account utente standard con privilegio sudo.</span><span class="sxs-lookup"><span data-stu-id="841ca-108">Server running CentOS 7 with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="841ca-109">Installare il runtime .NET Core nel server.</span><span class="sxs-lookup"><span data-stu-id="841ca-109">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="841ca-110">Visitare la [pagina di tutti i download per .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="841ca-110">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="841ca-111">Selezionare il runtime non di anteprima più recente nell'elenco **Runtime**.</span><span class="sxs-lookup"><span data-stu-id="841ca-111">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="841ca-112">Selezionare e seguire le istruzioni per CentOS/Oracle.</span><span class="sxs-lookup"><span data-stu-id="841ca-112">Select and follow the instructions for CentOS/Oracle.</span></span>
1. <span data-ttu-id="841ca-113">Un'app ASP.NET Core esistente.</span><span class="sxs-lookup"><span data-stu-id="841ca-113">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="841ca-114">Pubblicare e copiare l'app</span><span class="sxs-lookup"><span data-stu-id="841ca-114">Publish and copy over the app</span></span>

<span data-ttu-id="841ca-115">Configurare l'app per una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="841ca-115">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="841ca-116">Eseguire [dotnet publish](/dotnet/core/tools/dotnet-publish) dall'ambiente di sviluppo per creare il pacchetto di un'app in una directory (ad esempio, *bin/Release/&lt;target_framework_moniker&gt;/publish*) che può essere eseguita nel server:</span><span class="sxs-lookup"><span data-stu-id="841ca-116">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="841ca-117">È anche possibile pubblicare l'app come [distribuzione completa](/dotnet/core/deploying/#self-contained-deployments-scd) se si preferisce non mantenere il runtime .NET Core nel server.</span><span class="sxs-lookup"><span data-stu-id="841ca-117">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="841ca-118">Copiare l'app ASP.NET Core sul server usando uno strumento integrato nel flusso di lavoro dell'organizzazione, ad esempio SCP o FTP.</span><span class="sxs-lookup"><span data-stu-id="841ca-118">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="841ca-119">Le app Web si trovano in genere nella directory *var*, ad esempio *var/aspnetcore/hellomvc*.</span><span class="sxs-lookup"><span data-stu-id="841ca-119">It's common to locate web apps under the *var* directory (for example, *var/aspnetcore/hellomvc*).</span></span>

> [!NOTE]
> <span data-ttu-id="841ca-120">In uno scenario di distribuzione di produzione un flusso di lavoro di integrazione continua esegue le operazioni di pubblicazione dell'app e di copia degli asset nel server.</span><span class="sxs-lookup"><span data-stu-id="841ca-120">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

## <a name="configure-a-proxy-server"></a><span data-ttu-id="841ca-121">Configurazione di un server proxy</span><span class="sxs-lookup"><span data-stu-id="841ca-121">Configure a proxy server</span></span>

<span data-ttu-id="841ca-122">Un proxy inverso è una configurazione comune per la gestione delle app Web dinamiche.</span><span class="sxs-lookup"><span data-stu-id="841ca-122">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="841ca-123">Il proxy inverso termina la richiesta HTTP e la inoltra all'app ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="841ca-123">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="841ca-124">Un server proxy inoltra le richieste del client a un altro server anziché evaderle da solo.</span><span class="sxs-lookup"><span data-stu-id="841ca-124">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="841ca-125">Un proxy inverso inoltra a una destinazione fissa, in genere per conto di client non autorizzati.</span><span class="sxs-lookup"><span data-stu-id="841ca-125">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="841ca-126">In questa guida Apache viene configurato come proxy inverso in esecuzione nello stesso server usato da Kestrel per gestire l'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="841ca-126">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="841ca-127">Poiché le richieste vengono inoltrate dal proxy inverso, usare il [middleware delle intestazioni inoltrate](xref:host-and-deploy/proxy-load-balancer) dal pacchetto [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/).</span><span class="sxs-lookup"><span data-stu-id="841ca-127">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="841ca-128">Il middleware aggiorna `Request.Scheme` usando l'intestazione `X-Forwarded-Proto`, in modo che gli URI di reindirizzamento e altri criteri di sicurezza funzionino correttamente.</span><span class="sxs-lookup"><span data-stu-id="841ca-128">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="841ca-129">Qualsiasi componente che dipende dallo schema, ad esempio l'autenticazione, la generazione di collegamenti, i reindirizzamenti e la georilevazione, deve essere inserito dopo aver richiamato il middleware delle intestazioni inoltrate.</span><span class="sxs-lookup"><span data-stu-id="841ca-129">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="841ca-130">Come regola generale,il middleware delle intestazioni inoltrate deve essere eseguito prima degli altri middleware, ad eccezione del middleware di diagnostica e gestione degli errori.</span><span class="sxs-lookup"><span data-stu-id="841ca-130">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="841ca-131">Questo ordine garantisce che il middleware basato sulle intestazioni inoltrate possa usare i valori di intestazione per l'elaborazione.</span><span class="sxs-lookup"><span data-stu-id="841ca-131">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

::: moniker range=">= aspnetcore-2.0"
> [!NOTE]
> <span data-ttu-id="841ca-132">Entrambe le configurazioni (con o senza un server proxy inverso) sono configurazioni di hosting valide e supportate per le app ASP.NET Core 2.0 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="841ca-132">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="841ca-133">Per altre informazioni, vedere [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Quando usare Kestrel con un proxy inverso).</span><span class="sxs-lookup"><span data-stu-id="841ca-133">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>
::: moniker-end

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="841ca-134">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="841ca-134">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="841ca-135">Richiamare il metodo [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) in `Startup.Configure` prima di chiamare [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) o un middleware con schema di autenticazione simile.</span><span class="sxs-lookup"><span data-stu-id="841ca-135">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="841ca-136">Configurare il middleware per l'inoltro delle intestazioni `X-Forwarded-For` e `X-Forwarded-Proto`:</span><span class="sxs-lookup"><span data-stu-id="841ca-136">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="841ca-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="841ca-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="841ca-138">Richiamare il metodo [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) in `Startup.Configure` prima di chiamare [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) e [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) o un middleware con schema di autenticazione simile.</span><span class="sxs-lookup"><span data-stu-id="841ca-138">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="841ca-139">Configurare il middleware per l'inoltro delle intestazioni `X-Forwarded-For` e `X-Forwarded-Proto`:</span><span class="sxs-lookup"><span data-stu-id="841ca-139">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="841ca-140">Se non sono specificate opzioni [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) per il middleware, le intestazioni predefinite per l'inoltro sono `None`.</span><span class="sxs-lookup"><span data-stu-id="841ca-140">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="841ca-141">Potrebbero essere necessari interventi di configurazione aggiuntivi per le app ospitate dietro a server proxy e a servizi di bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="841ca-141">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="841ca-142">Per altre informazioni, vedere [Configurare ASP.NET Core per l'utilizzo di server proxy e servizi di bilanciamento del carico](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="841ca-142">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-apache"></a><span data-ttu-id="841ca-143">Installare Apache</span><span class="sxs-lookup"><span data-stu-id="841ca-143">Install Apache</span></span>

<span data-ttu-id="841ca-144">Aggiornare i pacchetti CentOS alle versioni stabili più recenti:</span><span class="sxs-lookup"><span data-stu-id="841ca-144">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="841ca-145">Installare il server Web Apache in CentOS con un singolo comando `yum`:</span><span class="sxs-lookup"><span data-stu-id="841ca-145">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="841ca-146">Esempio di output dopo l'esecuzione del comando:</span><span class="sxs-lookup"><span data-stu-id="841ca-146">Sample output after running the command:</span></span>

```bash
Downloading packages:
httpd-2.4.6-40.el7.centos.4.x86_64.rpm               | 2.7 MB  00:00:01
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
Installing : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 
Verifying  : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 

Installed:
httpd.x86_64 0:2.4.6-40.el7.centos.4

Complete!
```

> [!NOTE]
> <span data-ttu-id="841ca-147">In questo esempio, l'output rispecchia httpd.86_64 poiché la versione CentOS 7 è a 64 bit.</span><span class="sxs-lookup"><span data-stu-id="841ca-147">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="841ca-148">Per verificare dove è installato Apache, eseguire `whereis httpd` da un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="841ca-148">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache"></a><span data-ttu-id="841ca-149">Configurare Apache</span><span class="sxs-lookup"><span data-stu-id="841ca-149">Configure Apache</span></span>

<span data-ttu-id="841ca-150">I file di configurazione per Apache si trovano all'interno della directory `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="841ca-150">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="841ca-151">Qualsiasi file con l'estensione *.conf* viene elaborato in ordine alfabetico, in aggiunta ai file di configurazione dei moduli in `/etc/httpd/conf.modules.d/`, che contiene tutti i file di configurazione necessari per caricare i moduli.</span><span class="sxs-lookup"><span data-stu-id="841ca-151">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="841ca-152">Creare un file di configurazione denominato *hellomvc.conf* per l'app:</span><span class="sxs-lookup"><span data-stu-id="841ca-152">Create a configuration file, named *hellomvc.conf*, for the app:</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ServerName www.example.com
    ServerAlias *.example.com
    ErrorLog ${APACHE_LOG_DIR}hellomvc-error.log
    CustomLog ${APACHE_LOG_DIR}hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="841ca-153">Il blocco `VirtualHost` può comparire più volte, in uno o più file su un server.</span><span class="sxs-lookup"><span data-stu-id="841ca-153">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="841ca-154">Nel file di configurazione precedente Apache accetta il traffico pubblico sulla porta 80.</span><span class="sxs-lookup"><span data-stu-id="841ca-154">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="841ca-155">Il dominio `www.example.com` viene gestito e l'alias `*.example.com` viene risolto nello stesso sito.</span><span class="sxs-lookup"><span data-stu-id="841ca-155">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="841ca-156">Per altre informazioni, vedere [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) (Supporto degli host virtuali basati sul nome).</span><span class="sxs-lookup"><span data-stu-id="841ca-156">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="841ca-157">Le richieste vengono trasmesse tramite proxy alla radice sulla porta 5000 del server all'indirizzo 127.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="841ca-157">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="841ca-158">Per la comunicazione bidirezionale, sono necessari `ProxyPass` e `ProxyPassReverse`.</span><span class="sxs-lookup"><span data-stu-id="841ca-158">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span> <span data-ttu-id="841ca-159">Per cambiare porta/IP di Kestrel, vedere [Kestrel: Configurazione dell'endpoint](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="841ca-159">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="841ca-160">Se non si specifica una [direttiva ServerName](https://httpd.apache.org/docs/current/mod/core.html#servername) corretta nel blocco **VirtualHost**, l'app può risultare esposta a vulnerabilità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="841ca-160">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="841ca-161">L'associazione con caratteri jolly del sottodominio (ad esempio, `*.example.com`) non costituisce un rischio per la sicurezza se viene controllato l'intero dominio padre (a differenza di `*.com`, che è vulnerabile).</span><span class="sxs-lookup"><span data-stu-id="841ca-161">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="841ca-162">Vedere la [sezione 5.4 di RFC7230](https://tools.ietf.org/html/rfc7230#section-5.4) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="841ca-162">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="841ca-163">La registrazione può essere configurata per ogni `VirtualHost` usando le direttive `ErrorLog` e `CustomLog`.</span><span class="sxs-lookup"><span data-stu-id="841ca-163">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="841ca-164">`ErrorLog` è la posizione in cui il server registra gli errori e `CustomLog` imposta il nome del file e il formato del file di log.</span><span class="sxs-lookup"><span data-stu-id="841ca-164">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="841ca-165">In questo caso, si tratta della posizione in cui vengono registrate le informazioni sulla richiesta.</span><span class="sxs-lookup"><span data-stu-id="841ca-165">In this case, this is where request information is logged.</span></span> <span data-ttu-id="841ca-166">È presente una riga per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="841ca-166">There's one line for each request.</span></span>

<span data-ttu-id="841ca-167">Salvare il file e testare la configurazione.</span><span class="sxs-lookup"><span data-stu-id="841ca-167">Save the file and test the configuration.</span></span> <span data-ttu-id="841ca-168">Se tutto ciò viene superato, la risposta deve essere `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="841ca-168">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="841ca-169">Riavviare Apache:</span><span class="sxs-lookup"><span data-stu-id="841ca-169">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="841ca-170">Monitoraggio dell'app</span><span class="sxs-lookup"><span data-stu-id="841ca-170">Monitoring the app</span></span>

<span data-ttu-id="841ca-171">Apache è ora configurato in modo da inoltrare le richieste eseguite a `http://localhost:80` all'app ASP.NET Core eseguita in Kestrel all'indirizzo `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="841ca-171">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="841ca-172">Tuttavia, Apache non è configurato per la gestione del processo Kestrel.</span><span class="sxs-lookup"><span data-stu-id="841ca-172">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="841ca-173">Usare *systemd* e creare un file del servizio per avviare e monitorare l'app Web sottostante.</span><span class="sxs-lookup"><span data-stu-id="841ca-173">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="841ca-174">*systemd* è un sistema di inizializzazione che offre molte funzionalità potenti per l'avvio, l'arresto e la gestione dei processi.</span><span class="sxs-lookup"><span data-stu-id="841ca-174">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="841ca-175">Creare il file del servizio</span><span class="sxs-lookup"><span data-stu-id="841ca-175">Create the service file</span></span>

<span data-ttu-id="841ca-176">Creare il file di definizione del servizio:</span><span class="sxs-lookup"><span data-stu-id="841ca-176">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="841ca-177">Un esempio di file del servizio per l'app:</span><span class="sxs-lookup"><span data-stu-id="841ca-177">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

> [!NOTE]
> <span data-ttu-id="841ca-178">**Utente**: se l'utente *apache* non viene usato dalla configurazione, è necessario creare prima l'utente e assegnargli correttamente la proprietà dei file.</span><span class="sxs-lookup"><span data-stu-id="841ca-178">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

> [!NOTE]
> <span data-ttu-id="841ca-179">Per alcuni valori (ad esempio, le stringhe di connessione SQL) è necessario usare caratteri di escape, in modo da consentire ai provider di configurazione di leggere le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="841ca-179">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="841ca-180">Usare il comando seguente per generare un valore con caratteri di escape corretti per l'uso nel file di configurazione:</span><span class="sxs-lookup"><span data-stu-id="841ca-180">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="841ca-181">Salvare il file e abilitare il servizio:</span><span class="sxs-lookup"><span data-stu-id="841ca-181">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="841ca-182">Avviare il servizio e verificare che sia in esecuzione:</span><span class="sxs-lookup"><span data-stu-id="841ca-182">Start the service and verify that it's running:</span></span>

```bash
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="841ca-183">Con il proxy inverso configurato e Kestrel gestito tramite *systemd*, l'app Web è completamente configurata ed è possibile accedervi da un browser nel computer locale all'indirizzo `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="841ca-183">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="841ca-184">Se si esaminano le intestazioni di risposta, l'intestazione **Server** indica che l'app ASP.NET Core è gestita da Kestrel:</span><span class="sxs-lookup"><span data-stu-id="841ca-184">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="841ca-185">Vista dei log</span><span class="sxs-lookup"><span data-stu-id="841ca-185">Viewing logs</span></span>

<span data-ttu-id="841ca-186">Poiché l'app Web che usa Kestrel viene gestita tramite *systemd*, gli eventi e i processi vengono registrati in un giornale di registrazione centralizzato.</span><span class="sxs-lookup"><span data-stu-id="841ca-186">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="841ca-187">Tuttavia, questo giornale include le voci per tutti i servizi e i processi gestiti da *systemd*.</span><span class="sxs-lookup"><span data-stu-id="841ca-187">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="841ca-188">Per visualizzare le voci specifiche di `kestrel-hellomvc.service`, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="841ca-188">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="841ca-189">Per il filtro di data e ora, specificare le opzioni di tempo con il comando.</span><span class="sxs-lookup"><span data-stu-id="841ca-189">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="841ca-190">Ad esempio, usare `--since today` per filtrare in base al giorno corrente o `--until 1 hour ago` per visualizzare le voci dell'ora precedente.</span><span class="sxs-lookup"><span data-stu-id="841ca-190">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="841ca-191">Per altre informazioni, vedere la [pagina di manuale per journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="841ca-191">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="841ca-192">Protezione dell'app</span><span class="sxs-lookup"><span data-stu-id="841ca-192">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="841ca-193">Configurare firewall</span><span class="sxs-lookup"><span data-stu-id="841ca-193">Configure firewall</span></span>

<span data-ttu-id="841ca-194">*Firewalld* è un daemon dinamico per gestire il firewall con il supporto per le aree di rete.</span><span class="sxs-lookup"><span data-stu-id="841ca-194">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="841ca-195">È comunque possibile usare iptables per gestire le porte e il filtro dei pacchetti.</span><span class="sxs-lookup"><span data-stu-id="841ca-195">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="841ca-196">*Firewalld* dovrebbe essere installato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="841ca-196">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="841ca-197">È possibile usare `yum` per installare il pacchetto o verificare che sia installato.</span><span class="sxs-lookup"><span data-stu-id="841ca-197">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="841ca-198">Usare `firewalld` per aprire solo le porte necessarie per l'app.</span><span class="sxs-lookup"><span data-stu-id="841ca-198">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="841ca-199">In questo caso vengono usate le porte 80 e 443.</span><span class="sxs-lookup"><span data-stu-id="841ca-199">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="841ca-200">I comandi seguenti impostano in modo permanente le porte 80 e 443 per l'apertura:</span><span class="sxs-lookup"><span data-stu-id="841ca-200">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="841ca-201">Ricaricare le impostazioni del firewall.</span><span class="sxs-lookup"><span data-stu-id="841ca-201">Reload the firewall settings.</span></span> <span data-ttu-id="841ca-202">Verificare i servizi e le porte disponibili nell'area predefinita.</span><span class="sxs-lookup"><span data-stu-id="841ca-202">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="841ca-203">Le opzioni sono disponibili controllando `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="841ca-203">Options are available by inspecting `firewall-cmd -h`.</span></span>

```bash
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

```bash
public (default, active)
interfaces: eth0
sources: 
services: dhcpv6-client
ports: 443/tcp 80/tcp
masquerade: no
forward-ports: 
icmp-blocks: 
rich rules: 
```

### <a name="ssl-configuration"></a><span data-ttu-id="841ca-204">Configurazione SSL</span><span class="sxs-lookup"><span data-stu-id="841ca-204">SSL configuration</span></span>

<span data-ttu-id="841ca-205">Per configurare Apache per SSL, viene usato il modulo *mod_ssl*.</span><span class="sxs-lookup"><span data-stu-id="841ca-205">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="841ca-206">Durante l'installazione del modulo *httpd*, è stato installato anche il modulo *mod_ssl*.</span><span class="sxs-lookup"><span data-stu-id="841ca-206">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="841ca-207">Se non è stato installato, usare `yum` per aggiungerlo alla configurazione.</span><span class="sxs-lookup"><span data-stu-id="841ca-207">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```

<span data-ttu-id="841ca-208">Per imporre SSL, installare il modulo `mod_rewrite` per abilitare la riscrittura degli URL:</span><span class="sxs-lookup"><span data-stu-id="841ca-208">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="841ca-209">Modificare il file *hellomvc.conf* per abilitare la riscrittura degli URL e proteggere le comunicazioni sulla porta 443:</span><span class="sxs-lookup"><span data-stu-id="841ca-209">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

> [!NOTE]
> <span data-ttu-id="841ca-210">Questo esempio usa un certificato generato localmente.</span><span class="sxs-lookup"><span data-stu-id="841ca-210">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="841ca-211">**SSLCertificateFile** deve essere il file del certificato primario per il nome di dominio.</span><span class="sxs-lookup"><span data-stu-id="841ca-211">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="841ca-212">**SSLCertificateKeyFile** deve essere il file di chiave generato quando viene creato il CSR.</span><span class="sxs-lookup"><span data-stu-id="841ca-212">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="841ca-213">**SSLCertificateChainFile** deve essere il file di certificato intermedio (se presente) che è stato fornito dall'autorità di certificazione.</span><span class="sxs-lookup"><span data-stu-id="841ca-213">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="841ca-214">Salvare il file e testare la configurazione:</span><span class="sxs-lookup"><span data-stu-id="841ca-214">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="841ca-215">Riavviare Apache:</span><span class="sxs-lookup"><span data-stu-id="841ca-215">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="841ca-216">Altri suggerimenti di Apache</span><span class="sxs-lookup"><span data-stu-id="841ca-216">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="841ca-217">Intestazioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="841ca-217">Additional headers</span></span>

<span data-ttu-id="841ca-218">Al fine di garantire la protezione dagli attacchi dannosi, vi sono alcune intestazioni che devono essere modificate o aggiunte.</span><span class="sxs-lookup"><span data-stu-id="841ca-218">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="841ca-219">Verificare che il modulo `mod_headers` sia installato:</span><span class="sxs-lookup"><span data-stu-id="841ca-219">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="841ca-220">Proteggere Apache dagli attacchi di clickjacking</span><span class="sxs-lookup"><span data-stu-id="841ca-220">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="841ca-221">Il [clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), anche noto come *attacco UI Redress*, è un attacco dannoso in cui un visitatore di un sito Web viene indotto a fare clic su un collegamento o un pulsante in una pagina diversa da quella che sta attualmente visualizzando.</span><span class="sxs-lookup"><span data-stu-id="841ca-221">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="841ca-222">Usare `X-FRAME-OPTIONS` per proteggere il sito.</span><span class="sxs-lookup"><span data-stu-id="841ca-222">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="841ca-223">Modificare il file *httpd.conf*:</span><span class="sxs-lookup"><span data-stu-id="841ca-223">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="841ca-224">Aggiungere la riga `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="841ca-224">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="841ca-225">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="841ca-225">Save the file.</span></span> <span data-ttu-id="841ca-226">Riavviare Apache.</span><span class="sxs-lookup"><span data-stu-id="841ca-226">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="841ca-227">Analisi del tipo MIME</span><span class="sxs-lookup"><span data-stu-id="841ca-227">MIME-type sniffing</span></span>

<span data-ttu-id="841ca-228">L'intestazione `X-Content-Type-Options` impedisce a Internet Explorer di eseguire l'*analisi del tipo MIME*, ovvero di determinare il `Content-Type` di un file dal contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="841ca-228">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="841ca-229">Se il server imposta l'intestazione `Content-Type` su `text/html` con l'opzione `nosniff` impostata, Internet Explorer esegue il rendering del contenuto come `text/html`, indipendentemente dal contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="841ca-229">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="841ca-230">Modificare il file *httpd.conf*:</span><span class="sxs-lookup"><span data-stu-id="841ca-230">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="841ca-231">Aggiungere la riga `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="841ca-231">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="841ca-232">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="841ca-232">Save the file.</span></span> <span data-ttu-id="841ca-233">Riavviare Apache.</span><span class="sxs-lookup"><span data-stu-id="841ca-233">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="841ca-234">Bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="841ca-234">Load Balancing</span></span>

<span data-ttu-id="841ca-235">Questo esempio illustra come impostare e configurare Apache CentOS 7 e Kestrel nello stesso computer dell'istanza.</span><span class="sxs-lookup"><span data-stu-id="841ca-235">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="841ca-236">Per evitare di avere un singolo punto di errore, l'uso di *mod_proxy_balancer* e la modifica di **VirtualHost** consentono la gestione di più istanze delle app Web dietro il server proxy di Apache.</span><span class="sxs-lookup"><span data-stu-id="841ca-236">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="841ca-237">Nel file di configurazione illustrato di seguito, un'istanza aggiuntiva dell'app `hellomvc` viene configurata per l'esecuzione sulla porta 5001.</span><span class="sxs-lookup"><span data-stu-id="841ca-237">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="841ca-238">La sezione *Proxy* è impostata con una configurazione del servizio di bilanciamento del carico, con due membri per il bilanciamento del carico di *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="841ca-238">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPass / balancer://mycluster/ 

    ProxyPassReverse / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5001/

    <Proxy balancer://mycluster>
        BalancerMember http://127.0.0.1:5000
        BalancerMember http://127.0.0.1:5001 
        ProxySet lbmethod=byrequests
    </Proxy>

    <Location />
        SetHandler balancer
    </Location>
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a><span data-ttu-id="841ca-239">Limiti di velocità</span><span class="sxs-lookup"><span data-stu-id="841ca-239">Rate Limits</span></span>

<span data-ttu-id="841ca-240">Usando *mod_ratelimit*, incluso nel modulo *httpd*, è possibile limitare la larghezza di banda del client:</span><span class="sxs-lookup"><span data-stu-id="841ca-240">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="841ca-241">Il file di esempio limita la larghezza di banda a 600 KB al secondo nel percorso radice:</span><span class="sxs-lookup"><span data-stu-id="841ca-241">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

## <a name="additional-resources"></a><span data-ttu-id="841ca-242">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="841ca-242">Additional resources</span></span>

* [<span data-ttu-id="841ca-243">Configurare ASP.NET Core per l'utilizzo di server proxy e servizi di bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="841ca-243">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
