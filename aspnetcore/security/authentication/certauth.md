---
title: Configurare l'autenticazione del certificato in ASP.NET Core
author: blowdart
description: Informazioni su come configurare l'autenticazione del certificato in ASP.NET Core per HTTP. sys e IIS.
monikerRange: '>= aspnetcore-3.0'
ms.author: bdorrans
ms.date: 06/11/2019
uid: security/authentication/certauth
ms.openlocfilehash: 8609c58265340da1d618135795915d6c49e750a3
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538716"
---
# <a name="overview"></a><span data-ttu-id="cc015-103">Panoramica</span><span class="sxs-lookup"><span data-stu-id="cc015-103">Overview</span></span>

<span data-ttu-id="cc015-104">`Microsoft.AspNetCore.Authentication.Certificate` contiene un'implementazione simile a [l'autenticazione del certificato](https://tools.ietf.org/html/rfc5246#section-7.4.4) per ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cc015-104">`Microsoft.AspNetCore.Authentication.Certificate` contains an implementation similar to [Certificate Authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) for ASP.NET Core.</span></span> <span data-ttu-id="cc015-105">L'autenticazione del certificato si verifica a livello TLS, lungo prima che diventi mai ad ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cc015-105">Certificate authentication happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="cc015-106">Più precisamente, questo è un gestore di autenticazione che convalida il certificato e quindi ti offre un evento in cui è possibile risolvere tale certificato per un `ClaimsPrincipal`.</span><span class="sxs-lookup"><span data-stu-id="cc015-106">More accurately, this is an authentication handler that validates the certificate and then gives you an event where you can resolve that certificate to a `ClaimsPrincipal`.</span></span> 

<span data-ttu-id="cc015-107">[Configurare l'host](#configure-your-host-to-require-certificates) per l'autenticazione del certificato, sia esso IIS, Kestrel, App Web di Azure o qualsiasi altro componente in uso.</span><span class="sxs-lookup"><span data-stu-id="cc015-107">[Configure your host](#configure-your-host-to-require-certificates) for certificate authentication, be it IIS, Kestrel, Azure Web Apps, or whatever else you're using.</span></span>

## <a name="get-started"></a><span data-ttu-id="cc015-108">Introduzione</span><span class="sxs-lookup"><span data-stu-id="cc015-108">Get started</span></span>

<span data-ttu-id="cc015-109">Acquisire un certificato HTTPS, metterle in pratica, e [configurare l'host](#configure-your-host-to-require-certificates) per richiedere i certificati.</span><span class="sxs-lookup"><span data-stu-id="cc015-109">Acquire an HTTPS certificate, apply it, and [configure your host](#configure-your-host-to-require-certificates) to require certificates.</span></span>

<span data-ttu-id="cc015-110">Nell'app web, aggiungere un riferimento al `Microsoft.AspNetCore.Authentication.Certificate` pacchetto.</span><span class="sxs-lookup"><span data-stu-id="cc015-110">In your web app, add a reference to the `Microsoft.AspNetCore.Authentication.Certificate` package.</span></span> <span data-ttu-id="cc015-111">Quindi nel `Startup.Configure` metodo, chiamare `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` le opzioni, che fornisce un delegato per `OnCertificateValidated` eseguire alcuna convalida del certificato client inviato con le richieste supplementare.</span><span class="sxs-lookup"><span data-stu-id="cc015-111">Then in the `Startup.Configure` method, call `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` with your options, providing a delegate for `OnCertificateValidated` to do any supplementary validation on the client certificate sent with requests.</span></span> <span data-ttu-id="cc015-112">Attivare tali informazioni in un `ClaimsPrincipal` e impostarlo sul `context.Principal` proprietà.</span><span class="sxs-lookup"><span data-stu-id="cc015-112">Turn that information into a `ClaimsPrincipal` and set it on the `context.Principal` property.</span></span>

<span data-ttu-id="cc015-113">Se l'autenticazione ha esito negativo, il gestore restituisce un `403 (Forbidden)` risposta anziché una `401 (Unauthorized)`, come è prevedibile.</span><span class="sxs-lookup"><span data-stu-id="cc015-113">If authentication fails, this handler returns a `403 (Forbidden)` response rather a `401 (Unauthorized)`, as you might expect.</span></span> <span data-ttu-id="cc015-114">Il concetto di base è che l'autenticazione dovrebbe verificarsi durante la connessione TLS iniziale.</span><span class="sxs-lookup"><span data-stu-id="cc015-114">The reasoning is that the authentication should happen during the initial TLS connection.</span></span> <span data-ttu-id="cc015-115">Per il tempo di che raggiunge il gestore è troppo tardi.</span><span class="sxs-lookup"><span data-stu-id="cc015-115">By the time it reaches the handler, it's too late.</span></span> <span data-ttu-id="cc015-116">Non è possibile eseguire l'aggiornamento della connessione da una connessione anonima a uno con un certificato.</span><span class="sxs-lookup"><span data-stu-id="cc015-116">There's no way to upgrade the connection from an anonymous connection to one with a certificate.</span></span>

<span data-ttu-id="cc015-117">Aggiungere inoltre `app.UseAuthentication();` nella `Startup.Configure` (metodo).</span><span class="sxs-lookup"><span data-stu-id="cc015-117">Also add `app.UseAuthentication();` in the `Startup.Configure` method.</span></span> <span data-ttu-id="cc015-118">In caso contrario, HttpContext non essere impostato su `ClaimsPrincipal` creato dal certificato.</span><span class="sxs-lookup"><span data-stu-id="cc015-118">Otherwise, the HttpContext.User will not be set to `ClaimsPrincipal` created from the certificate.</span></span> <span data-ttu-id="cc015-119">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cc015-119">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(
        CertificateAuthenticationDefaults.AuthenticationScheme)
            .AddCertificate();
    // All the other service configuration.
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseAuthentication();

    // All the other app configuration.
}
```

<span data-ttu-id="cc015-120">L'esempio precedente dimostra la modalità predefinita per aggiungere l'autenticazione del certificato.</span><span class="sxs-lookup"><span data-stu-id="cc015-120">The preceding example demonstrates the default way to add certificate authentication.</span></span> <span data-ttu-id="cc015-121">Il gestore costruisce un'entità utente usando le proprietà comuni del certificato.</span><span class="sxs-lookup"><span data-stu-id="cc015-121">The handler constructs a user principal using the common certificate properties.</span></span>

## <a name="configure-certificate-validation"></a><span data-ttu-id="cc015-122">Configurazione della convalida del certificato</span><span class="sxs-lookup"><span data-stu-id="cc015-122">Configure certificate validation</span></span>

<span data-ttu-id="cc015-123">Il `CertificateAuthenticationOptions` gestore ha alcune convalide incorporate che fanno le convalide minime è consigliabile eseguire in un certificato.</span><span class="sxs-lookup"><span data-stu-id="cc015-123">The `CertificateAuthenticationOptions` handler has some built-in validations that are the minimum validations you should perform on a certificate.</span></span> <span data-ttu-id="cc015-124">Ognuna di queste impostazioni è abilitata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="cc015-124">Each of these settings is enabled by default.</span></span>

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a><span data-ttu-id="cc015-125">AllowedCertificateTypes = concatenate, SelfSigned oppure tutti (concatenate | SelfSigned)</span><span class="sxs-lookup"><span data-stu-id="cc015-125">AllowedCertificateTypes = Chained, SelfSigned, or All (Chained | SelfSigned)</span></span>

<span data-ttu-id="cc015-126">Questo controllo verifica che sia consentito solo il tipo di certificato appropriato.</span><span class="sxs-lookup"><span data-stu-id="cc015-126">This check validates that only the appropriate certificate type is allowed.</span></span>

### <a name="validatecertificateuse"></a><span data-ttu-id="cc015-127">ValidateCertificateUse</span><span class="sxs-lookup"><span data-stu-id="cc015-127">ValidateCertificateUse</span></span>

<span data-ttu-id="cc015-128">Questo controllo verifica che il certificato presentato dal client con l'autenticazione Client esteso per l'uso delle chiavi (EKU), o nessun EKU affatto.</span><span class="sxs-lookup"><span data-stu-id="cc015-128">This check validates that the certificate presented by the client has the Client Authentication extended key use (EKU), or no EKUs at all.</span></span> <span data-ttu-id="cc015-129">Come dice le specifiche, se non viene specificato alcun utilizzo chiavi avanzato, tutti gli EKU vengono ritenuti validi.</span><span class="sxs-lookup"><span data-stu-id="cc015-129">As the specifications say, if no EKU is specified, then all EKUs are deemed valid.</span></span>

### <a name="validatevalidityperiod"></a><span data-ttu-id="cc015-130">ValidateValidityPeriod</span><span class="sxs-lookup"><span data-stu-id="cc015-130">ValidateValidityPeriod</span></span>

<span data-ttu-id="cc015-131">Questo controllo verifica che il certificato sia entro il relativo periodo di validità.</span><span class="sxs-lookup"><span data-stu-id="cc015-131">This check validates that the certificate is within its validity period.</span></span> <span data-ttu-id="cc015-132">Per ogni richiesta, il gestore assicura che non sia scaduto un certificato che era valido quando viene presentato durante la sessione corrente.</span><span class="sxs-lookup"><span data-stu-id="cc015-132">On each request, the handler ensures that a certificate that was valid when it was presented hasn't expired during its current session.</span></span>

### <a name="revocationflag"></a><span data-ttu-id="cc015-133">RevocationFlag</span><span class="sxs-lookup"><span data-stu-id="cc015-133">RevocationFlag</span></span>

<span data-ttu-id="cc015-134">Un flag che specifica quali certificati nella catena è controllato per la revoca.</span><span class="sxs-lookup"><span data-stu-id="cc015-134">A flag that specifies which certificates in the chain are checked for revocation.</span></span>

<span data-ttu-id="cc015-135">Controlli di revoche di certificati vengono eseguiti solo quando il certificato sia concatenato a un certificato radice.</span><span class="sxs-lookup"><span data-stu-id="cc015-135">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="revocationmode"></a><span data-ttu-id="cc015-136">RevocationMode</span><span class="sxs-lookup"><span data-stu-id="cc015-136">RevocationMode</span></span>

<span data-ttu-id="cc015-137">Flag che specifica come vengono eseguiti i controlli di revoche di certificati.</span><span class="sxs-lookup"><span data-stu-id="cc015-137">A flag that specifies how revocation checks are performed.</span></span>

<span data-ttu-id="cc015-138">Specifica di un controllo in linea può comportare un ritardo prolungato mentre viene contattato l'autorità di certificazione.</span><span class="sxs-lookup"><span data-stu-id="cc015-138">Specifying an online check can result in a long delay while the certificate authority is contacted.</span></span>

<span data-ttu-id="cc015-139">Controlli di revoche di certificati vengono eseguiti solo quando il certificato sia concatenato a un certificato radice.</span><span class="sxs-lookup"><span data-stu-id="cc015-139">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a><span data-ttu-id="cc015-140">È possibile configurare l'app per richiedere un certificato solo in determinati percorsi?</span><span class="sxs-lookup"><span data-stu-id="cc015-140">Can I configure my app to require a certificate only on certain paths?</span></span>

<span data-ttu-id="cc015-141">Questo non è possibile.</span><span class="sxs-lookup"><span data-stu-id="cc015-141">This isn't possible.</span></span> <span data-ttu-id="cc015-142">Tenere presente che lo scambio del certificato viene eseguito l'avvio della conversazione HTTPS, il termine dal server prima che venga ricevuta la prima richiesta su tale connessione in modo che non è possibile all'ambito basato su tutti i campi di richiesta.</span><span class="sxs-lookup"><span data-stu-id="cc015-142">Remember the certificate exchange is done that the start of the HTTPS conversation, it's done by the server before the first request is received on that connection so it's not possible to scope based on any request fields.</span></span>

## <a name="handler-events"></a><span data-ttu-id="cc015-143">Eventi del gestore</span><span class="sxs-lookup"><span data-stu-id="cc015-143">Handler events</span></span>

<span data-ttu-id="cc015-144">Il gestore ha due eventi:</span><span class="sxs-lookup"><span data-stu-id="cc015-144">The handler has two events:</span></span>

* <span data-ttu-id="cc015-145">`OnAuthenticationFailed` &ndash; Chiamata eseguita se un'eccezione si verifica durante l'autenticazione e consente di rispondere.</span><span class="sxs-lookup"><span data-stu-id="cc015-145">`OnAuthenticationFailed` &ndash; Called if an exception happens during authentication and allows you to react.</span></span>
* <span data-ttu-id="cc015-146">`OnCertificateValidated` &ndash; Chiamato dopo che il certificato è stato convalidato, ha superato la convalida e sia stata creata un'entità di sicurezza predefinita.</span><span class="sxs-lookup"><span data-stu-id="cc015-146">`OnCertificateValidated` &ndash; Called after the certificate has been validated, passed validation and a default principal has been created.</span></span> <span data-ttu-id="cc015-147">Questo evento consente di eseguire la propria convalida e migliorare o sostituire l'entità.</span><span class="sxs-lookup"><span data-stu-id="cc015-147">This event allows you to perform your own validation and augment or replace the principal.</span></span> <span data-ttu-id="cc015-148">Per alcuni esempi includono:</span><span class="sxs-lookup"><span data-stu-id="cc015-148">For examples include:</span></span>
  * <span data-ttu-id="cc015-149">Determinare se il certificato è noto ai tuoi servizi.</span><span class="sxs-lookup"><span data-stu-id="cc015-149">Determining if the certificate is known to your services.</span></span>
  * <span data-ttu-id="cc015-150">Costruzione di propria entità.</span><span class="sxs-lookup"><span data-stu-id="cc015-150">Constructing your own principal.</span></span> <span data-ttu-id="cc015-151">Si consideri l'esempio seguente in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="cc015-151">Consider the following example in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(
    CertificateAuthenticationDefaults.AuthenticationScheme)
    .AddCertificate(options =>
    {
        options.Events = new CertificateAuthenticationEvents
        {
            OnCertificateValidated = context =>
            {
                var claims = new[]
                {
                    new Claim(
                        ClaimTypes.NameIdentifier, 
                        context.ClientCertificate.Subject,
                        ClaimValueTypes.String, 
                        context.Options.ClaimsIssuer),
                    new Claim(ClaimTypes.Name,
                        context.ClientCertificate.Subject,
                        ClaimValueTypes.String, 
                        context.Options.ClaimsIssuer)
                };

                context.Principal = new ClaimsPrincipal(
                    new ClaimsIdentity(claims, context.Scheme.Name));
                context.Success();

                return Task.CompletedTask;
            }
        };
    });
```

<span data-ttu-id="cc015-152">Se si trova il certificato in ingresso non soddisfa la convalida aggiuntiva, chiamare `context.Fail("failure reason")` con un motivo dell'errore.</span><span class="sxs-lookup"><span data-stu-id="cc015-152">If you find the inbound certificate doesn't meet your extra validation, call `context.Fail("failure reason")` with a failure reason.</span></span>

<span data-ttu-id="cc015-153">La funzionalità per il real, si sarà probabilmente necessario chiamare un servizio registrato nell'inserimento delle dipendenze che si connette a un database o di altro tipo di archivio dell'utente.</span><span class="sxs-lookup"><span data-stu-id="cc015-153">For real functionality, you'll probably want to call a service registered in dependency injection that connects to a database or other type of user store.</span></span> <span data-ttu-id="cc015-154">Accedere al servizio utilizzando il contesto passato al delegato.</span><span class="sxs-lookup"><span data-stu-id="cc015-154">Access your service by using the context passed into your delegate.</span></span> <span data-ttu-id="cc015-155">Si consideri l'esempio seguente in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="cc015-155">Consider the following example in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(
    CertificateAuthenticationDefaults.AuthenticationScheme)
    .AddCertificate(options =>
    {
        options.Events = new CertificateAuthenticationEvents
        {
            OnCertificateValidated = context =>
            {
                var validationService =
                    context.HttpContext.RequestServices
                        .GetService<ICertificateValidationService>();
                
                if (validationService.ValidateCertificate(
                    context.ClientCertificate))
                {
                    var claims = new[]
                    {
                        new Claim(
                            ClaimTypes.NameIdentifier, 
                            context.ClientCertificate.Subject, 
                            ClaimValueTypes.String, 
                            context.Options.ClaimsIssuer),
                        new Claim(
                            ClaimTypes.Name, 
                            context.ClientCertificate.Subject, 
                            ClaimValueTypes.String, 
                            context.Options.ClaimsIssuer)
                    };

                    context.Principal = new ClaimsPrincipal(
                        new ClaimsIdentity(claims, context.Scheme.Name));
                    context.Success();
                }                     

                return Task.CompletedTask;
            }
        };
    });
```

<span data-ttu-id="cc015-156">Concettualmente, la convalida del certificato è un problema di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="cc015-156">Conceptually, the validation of the certificate is an authorization concern.</span></span> <span data-ttu-id="cc015-157">L'aggiunta di un controllo, ad esempio, un'autorità di certificazione o identificazione personale in un criterio di autorizzazione, anziché all'interno `OnCertificateValidated`, è perfettamente accettabile.</span><span class="sxs-lookup"><span data-stu-id="cc015-157">Adding a check on, for example, an issuer or thumbprint in an authorization policy, rather than inside `OnCertificateValidated`, is perfectly acceptable.</span></span>

## <a name="configure-your-host-to-require-certificates"></a><span data-ttu-id="cc015-158">Configurare l'host per richiedere i certificati</span><span class="sxs-lookup"><span data-stu-id="cc015-158">Configure your host to require certificates</span></span>

### <a name="kestrel"></a><span data-ttu-id="cc015-159">Kestrel</span><span class="sxs-lookup"><span data-stu-id="cc015-159">Kestrel</span></span>

<span data-ttu-id="cc015-160">Nelle *Program.cs*, configurare Kestrel come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="cc015-160">In *Program.cs*, configure Kestrel as follows:</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel(options =>
        {
            options.ConfigureHttpsDefaults(opt => 
                opt.ClientCertificateMode = 
                    ClientCertificateMode.RequireCertificate);
        })
        .Build();
```

### <a name="iis"></a><span data-ttu-id="cc015-161">IIS</span><span class="sxs-lookup"><span data-stu-id="cc015-161">IIS</span></span>

<span data-ttu-id="cc015-162">Completare i passaggi seguenti In Gestione IIS:</span><span class="sxs-lookup"><span data-stu-id="cc015-162">Complete the following steps In IIS Manager:</span></span>

1. <span data-ttu-id="cc015-163">Selezionare il sito dal **connessioni** scheda.</span><span class="sxs-lookup"><span data-stu-id="cc015-163">Select your site from the **Connections** tab.</span></span>
1. <span data-ttu-id="cc015-164">Fare doppio clic il **impostazioni SSL** opzione il **visualizzazione funzionalità** finestra.</span><span class="sxs-lookup"><span data-stu-id="cc015-164">Double-click the **SSL Settings** option in the **Features View** window.</span></span>
1. <span data-ttu-id="cc015-165">Controllare la **Richiedi SSL** casella di controllo e selezionare il **richiedono** pulsante di opzione nel **certificati Client** sezione.</span><span class="sxs-lookup"><span data-stu-id="cc015-165">Check the **Require SSL** checkbox, and select the **Require** radio button in the **Client certificates** section.</span></span>

![Impostazioni del certificato client in IIS](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a><span data-ttu-id="cc015-167">Azure e i proxy web personalizzata</span><span class="sxs-lookup"><span data-stu-id="cc015-167">Azure and custom web proxies</span></span>

<span data-ttu-id="cc015-168">Vedere le [ospitare e distribuire la documentazione](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) per informazioni su come configurare il certificato del middleware di inoltro.</span><span class="sxs-lookup"><span data-stu-id="cc015-168">See the [host and deploy documentation](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) for how to configure the certificate forwarding middleware.</span></span>
