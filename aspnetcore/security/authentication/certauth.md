---
title: Configurare l'autenticazione del certificato in ASP.NET Core
author: blowdart
description: Informazioni su come configurare l'autenticazione del certificato in ASP.NET Core per IIS e HTTP. sys.
monikerRange: '>= aspnetcore-3.0'
ms.author: bdorrans
ms.date: 08/19/2019
uid: security/authentication/certauth
ms.openlocfilehash: 1e646aabb4e384e6906575e7beaa680e91f968a0
ms.sourcegitcommit: e5d4768aaf85703effb4557a520d681af8284e26
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2019
ms.locfileid: "73616574"
---
# <a name="configure-certificate-authentication-in-aspnet-core"></a><span data-ttu-id="c33c7-103">Configurare l'autenticazione del certificato in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c33c7-103">Configure certificate authentication in ASP.NET Core</span></span>

<span data-ttu-id="c33c7-104">`Microsoft.AspNetCore.Authentication.Certificate` contiene un'implementazione simile all' [autenticazione del certificato](https://tools.ietf.org/html/rfc5246#section-7.4.4) per ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c33c7-104">`Microsoft.AspNetCore.Authentication.Certificate` contains an implementation similar to [Certificate Authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) for ASP.NET Core.</span></span> <span data-ttu-id="c33c7-105">L'autenticazione del certificato viene eseguita a livello di TLS, molto prima che venga mai ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c33c7-105">Certificate authentication happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="c33c7-106">Più precisamente, si tratta di un gestore di autenticazione che convalida il certificato e quindi fornisce un evento in cui è possibile risolvere il certificato in un `ClaimsPrincipal`.</span><span class="sxs-lookup"><span data-stu-id="c33c7-106">More accurately, this is an authentication handler that validates the certificate and then gives you an event where you can resolve that certificate to a `ClaimsPrincipal`.</span></span> 

<span data-ttu-id="c33c7-107">[Configurare l'host](#configure-your-host-to-require-certificates) per l'autenticazione del certificato, essere IIS, gheppio, app Web di Azure o qualsiasi altro elemento in uso.</span><span class="sxs-lookup"><span data-stu-id="c33c7-107">[Configure your host](#configure-your-host-to-require-certificates) for certificate authentication, be it IIS, Kestrel, Azure Web Apps, or whatever else you're using.</span></span>

## <a name="proxy-and-load-balancer-scenarios"></a><span data-ttu-id="c33c7-108">Scenari di proxy e bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="c33c7-108">Proxy and load balancer scenarios</span></span>

<span data-ttu-id="c33c7-109">L'autenticazione del certificato è uno scenario con stato usato principalmente quando un proxy o un servizio di bilanciamento del carico non gestisce il traffico tra client e server.</span><span class="sxs-lookup"><span data-stu-id="c33c7-109">Certificate authentication is a stateful scenario primarily used where a proxy or load balancer doesn't handle traffic between clients and servers.</span></span> <span data-ttu-id="c33c7-110">Se si usa un proxy o un servizio di bilanciamento del carico, l'autenticazione del certificato funziona solo se il proxy o il servizio di bilanciamento del carico:</span><span class="sxs-lookup"><span data-stu-id="c33c7-110">If a proxy or load balancer is used, certificate authentication only works if the proxy or load balancer:</span></span>

* <span data-ttu-id="c33c7-111">Gestisce l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="c33c7-111">Handles the authentication.</span></span>
* <span data-ttu-id="c33c7-112">Passa le informazioni di autenticazione utente all'app, ad esempio in un'intestazione di richiesta, che agisce sulle informazioni di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="c33c7-112">Passes the user authentication information to the app (for example, in a request header), which acts on the authentication information.</span></span>

<span data-ttu-id="c33c7-113">Un'alternativa all'autenticazione del certificato negli ambienti in cui vengono usati proxy e servizi di bilanciamento del carico è Active Directory servizi federati (ADFS) con OpenID Connect (OIDC).</span><span class="sxs-lookup"><span data-stu-id="c33c7-113">An alternative to certificate authentication in environments where proxies and load balancers are used is Active Directory Federated Services (ADFS) with OpenID Connect (OIDC).</span></span>

## <a name="get-started"></a><span data-ttu-id="c33c7-114">Introduzione</span><span class="sxs-lookup"><span data-stu-id="c33c7-114">Get started</span></span>

<span data-ttu-id="c33c7-115">Acquisire un certificato HTTPS, applicarlo e [configurare l'host](#configure-your-host-to-require-certificates) per richiedere i certificati.</span><span class="sxs-lookup"><span data-stu-id="c33c7-115">Acquire an HTTPS certificate, apply it, and [configure your host](#configure-your-host-to-require-certificates) to require certificates.</span></span>

<span data-ttu-id="c33c7-116">Nell'app Web aggiungere un riferimento al pacchetto `Microsoft.AspNetCore.Authentication.Certificate`.</span><span class="sxs-lookup"><span data-stu-id="c33c7-116">In your web app, add a reference to the `Microsoft.AspNetCore.Authentication.Certificate` package.</span></span> <span data-ttu-id="c33c7-117">Quindi, nel metodo `Startup.ConfigureServices`, chiamare `services.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` con le opzioni, fornendo un delegato per `OnCertificateValidated` eseguire qualsiasi convalida supplementare sul certificato client inviato con le richieste.</span><span class="sxs-lookup"><span data-stu-id="c33c7-117">Then in the `Startup.ConfigureServices` method, call `services.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` with your options, providing a delegate for `OnCertificateValidated` to do any supplementary validation on the client certificate sent with requests.</span></span> <span data-ttu-id="c33c7-118">Trasformare le informazioni in un `ClaimsPrincipal` e impostarle sulla proprietà `context.Principal`.</span><span class="sxs-lookup"><span data-stu-id="c33c7-118">Turn that information into a `ClaimsPrincipal` and set it on the `context.Principal` property.</span></span>

<span data-ttu-id="c33c7-119">Se l'autenticazione ha esito negativo, questo gestore restituisce una risposta `403 (Forbidden)` piuttosto una `401 (Unauthorized)`, come si può immaginare.</span><span class="sxs-lookup"><span data-stu-id="c33c7-119">If authentication fails, this handler returns a `403 (Forbidden)` response rather a `401 (Unauthorized)`, as you might expect.</span></span> <span data-ttu-id="c33c7-120">Il motivo è che l'autenticazione deve verificarsi durante la connessione TLS iniziale.</span><span class="sxs-lookup"><span data-stu-id="c33c7-120">The reasoning is that the authentication should happen during the initial TLS connection.</span></span> <span data-ttu-id="c33c7-121">Quando raggiunge il gestore, è troppo tardi.</span><span class="sxs-lookup"><span data-stu-id="c33c7-121">By the time it reaches the handler, it's too late.</span></span> <span data-ttu-id="c33c7-122">Non è possibile aggiornare la connessione da una connessione anonima a un'altra con un certificato.</span><span class="sxs-lookup"><span data-stu-id="c33c7-122">There's no way to upgrade the connection from an anonymous connection to one with a certificate.</span></span>

<span data-ttu-id="c33c7-123">Aggiungere anche `app.UseAuthentication();` nel metodo `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="c33c7-123">Also add `app.UseAuthentication();` in the `Startup.Configure` method.</span></span> <span data-ttu-id="c33c7-124">In caso contrario, HttpContext. User non verrà impostato su `ClaimsPrincipal` creato dal certificato.</span><span class="sxs-lookup"><span data-stu-id="c33c7-124">Otherwise, the HttpContext.User will not be set to `ClaimsPrincipal` created from the certificate.</span></span> <span data-ttu-id="c33c7-125">Esempio:</span><span class="sxs-lookup"><span data-stu-id="c33c7-125">For example:</span></span>

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

<span data-ttu-id="c33c7-126">Nell'esempio precedente viene illustrata la modalità predefinita per aggiungere l'autenticazione del certificato.</span><span class="sxs-lookup"><span data-stu-id="c33c7-126">The preceding example demonstrates the default way to add certificate authentication.</span></span> <span data-ttu-id="c33c7-127">Il gestore crea un'entità utente usando le proprietà comuni del certificato.</span><span class="sxs-lookup"><span data-stu-id="c33c7-127">The handler constructs a user principal using the common certificate properties.</span></span>

## <a name="configure-certificate-validation"></a><span data-ttu-id="c33c7-128">Configurare la convalida del certificato</span><span class="sxs-lookup"><span data-stu-id="c33c7-128">Configure certificate validation</span></span>

<span data-ttu-id="c33c7-129">Il gestore `CertificateAuthenticationOptions` dispone di alcune convalide predefinite che rappresentano le convalide minime che è necessario eseguire su un certificato.</span><span class="sxs-lookup"><span data-stu-id="c33c7-129">The `CertificateAuthenticationOptions` handler has some built-in validations that are the minimum validations you should perform on a certificate.</span></span> <span data-ttu-id="c33c7-130">Ognuna di queste impostazioni è abilitata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="c33c7-130">Each of these settings is enabled by default.</span></span>

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a><span data-ttu-id="c33c7-131">AllowedCertificateTypes = concatenato, SelfSigned o tutti (concatenato | SelfSigned)</span><span class="sxs-lookup"><span data-stu-id="c33c7-131">AllowedCertificateTypes = Chained, SelfSigned, or All (Chained | SelfSigned)</span></span>

<span data-ttu-id="c33c7-132">Questo controllo consente di verificare che sia consentito solo il tipo di certificato appropriato.</span><span class="sxs-lookup"><span data-stu-id="c33c7-132">This check validates that only the appropriate certificate type is allowed.</span></span>

### <a name="validatecertificateuse"></a><span data-ttu-id="c33c7-133">ValidateCertificateUse</span><span class="sxs-lookup"><span data-stu-id="c33c7-133">ValidateCertificateUse</span></span>

<span data-ttu-id="c33c7-134">Questo controllo verifica che il certificato presentato dal client disponga dell'utilizzo chiavi avanzato (EKU) di autenticazione client o non EKU affatto.</span><span class="sxs-lookup"><span data-stu-id="c33c7-134">This check validates that the certificate presented by the client has the Client Authentication extended key use (EKU), or no EKUs at all.</span></span> <span data-ttu-id="c33c7-135">Come le specifiche dicono, se non è specificato alcun EKU, tutti i EKU vengono considerati validi.</span><span class="sxs-lookup"><span data-stu-id="c33c7-135">As the specifications say, if no EKU is specified, then all EKUs are deemed valid.</span></span>

### <a name="validatevalidityperiod"></a><span data-ttu-id="c33c7-136">ValidateValidityPeriod</span><span class="sxs-lookup"><span data-stu-id="c33c7-136">ValidateValidityPeriod</span></span>

<span data-ttu-id="c33c7-137">Questo controllo verifica che il certificato sia entro il periodo di validità.</span><span class="sxs-lookup"><span data-stu-id="c33c7-137">This check validates that the certificate is within its validity period.</span></span> <span data-ttu-id="c33c7-138">Per ogni richiesta, il gestore garantisce che un certificato valido al momento della presentazione non sia scaduto durante la sessione corrente.</span><span class="sxs-lookup"><span data-stu-id="c33c7-138">On each request, the handler ensures that a certificate that was valid when it was presented hasn't expired during its current session.</span></span>

### <a name="revocationflag"></a><span data-ttu-id="c33c7-139">RevocationFlag</span><span class="sxs-lookup"><span data-stu-id="c33c7-139">RevocationFlag</span></span>

<span data-ttu-id="c33c7-140">Flag che specifica i certificati nella catena controllati per la revoca.</span><span class="sxs-lookup"><span data-stu-id="c33c7-140">A flag that specifies which certificates in the chain are checked for revocation.</span></span>

<span data-ttu-id="c33c7-141">I controlli di revoca vengono eseguiti solo quando il certificato è concatenato a un certificato radice.</span><span class="sxs-lookup"><span data-stu-id="c33c7-141">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="revocationmode"></a><span data-ttu-id="c33c7-142">RevocationMode</span><span class="sxs-lookup"><span data-stu-id="c33c7-142">RevocationMode</span></span>

<span data-ttu-id="c33c7-143">Flag che specifica la modalità di esecuzione dei controlli di revoca.</span><span class="sxs-lookup"><span data-stu-id="c33c7-143">A flag that specifies how revocation checks are performed.</span></span>

<span data-ttu-id="c33c7-144">La specifica di un controllo online può comportare un ritardo prolungato mentre viene contattata l'autorità di certificazione.</span><span class="sxs-lookup"><span data-stu-id="c33c7-144">Specifying an online check can result in a long delay while the certificate authority is contacted.</span></span>

<span data-ttu-id="c33c7-145">I controlli di revoca vengono eseguiti solo quando il certificato è concatenato a un certificato radice.</span><span class="sxs-lookup"><span data-stu-id="c33c7-145">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a><span data-ttu-id="c33c7-146">È possibile configurare l'app in modo che richieda un certificato solo su determinati percorsi?</span><span class="sxs-lookup"><span data-stu-id="c33c7-146">Can I configure my app to require a certificate only on certain paths?</span></span>

<span data-ttu-id="c33c7-147">Questa operazione non è possibile.</span><span class="sxs-lookup"><span data-stu-id="c33c7-147">This isn't possible.</span></span> <span data-ttu-id="c33c7-148">Tenere presente che lo scambio di certificati è stato eseguito dall'avvio della conversazione HTTPS, che viene eseguito dal server prima che la prima richiesta venga ricevuta sulla connessione, quindi non è possibile definire l'ambito in base a qualsiasi campo di richiesta.</span><span class="sxs-lookup"><span data-stu-id="c33c7-148">Remember the certificate exchange is done that the start of the HTTPS conversation, it's done by the server before the first request is received on that connection so it's not possible to scope based on any request fields.</span></span>

## <a name="handler-events"></a><span data-ttu-id="c33c7-149">Eventi gestore</span><span class="sxs-lookup"><span data-stu-id="c33c7-149">Handler events</span></span>

<span data-ttu-id="c33c7-150">Il gestore dispone di due eventi:</span><span class="sxs-lookup"><span data-stu-id="c33c7-150">The handler has two events:</span></span>

* <span data-ttu-id="c33c7-151">`OnAuthenticationFailed` &ndash; chiamato se si verifica un'eccezione durante l'autenticazione e consente di rispondere.</span><span class="sxs-lookup"><span data-stu-id="c33c7-151">`OnAuthenticationFailed` &ndash; Called if an exception happens during authentication and allows you to react.</span></span>
* <span data-ttu-id="c33c7-152">`OnCertificateValidated` &ndash; chiamato dopo che il certificato è stato convalidato, è stata passata la convalida ed è stata creata un'entità predefinita.</span><span class="sxs-lookup"><span data-stu-id="c33c7-152">`OnCertificateValidated` &ndash; Called after the certificate has been validated, passed validation and a default principal has been created.</span></span> <span data-ttu-id="c33c7-153">Questo evento consente di eseguire una convalida personalizzata e di aumentare o sostituire l'entità di protezione.</span><span class="sxs-lookup"><span data-stu-id="c33c7-153">This event allows you to perform your own validation and augment or replace the principal.</span></span> <span data-ttu-id="c33c7-154">Gli esempi includono:</span><span class="sxs-lookup"><span data-stu-id="c33c7-154">For examples include:</span></span>
  * <span data-ttu-id="c33c7-155">Determinare se il certificato è noto ai servizi.</span><span class="sxs-lookup"><span data-stu-id="c33c7-155">Determining if the certificate is known to your services.</span></span>
  * <span data-ttu-id="c33c7-156">Creazione di un'entità personalizzata.</span><span class="sxs-lookup"><span data-stu-id="c33c7-156">Constructing your own principal.</span></span> <span data-ttu-id="c33c7-157">Si consideri l'esempio seguente in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c33c7-157">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="c33c7-158">Se il certificato in ingresso non soddisfa la convalida aggiuntiva, chiamare `context.Fail("failure reason")` con un motivo dell'errore.</span><span class="sxs-lookup"><span data-stu-id="c33c7-158">If you find the inbound certificate doesn't meet your extra validation, call `context.Fail("failure reason")` with a failure reason.</span></span>

<span data-ttu-id="c33c7-159">Per le funzionalità reali, è probabile che si voglia chiamare un servizio registrato nell'inserimento delle dipendenze che si connette a un database o a un altro tipo di archivio utente.</span><span class="sxs-lookup"><span data-stu-id="c33c7-159">For real functionality, you'll probably want to call a service registered in dependency injection that connects to a database or other type of user store.</span></span> <span data-ttu-id="c33c7-160">Accedere al servizio usando il contesto passato al delegato.</span><span class="sxs-lookup"><span data-stu-id="c33c7-160">Access your service by using the context passed into your delegate.</span></span> <span data-ttu-id="c33c7-161">Si consideri l'esempio seguente in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c33c7-161">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="c33c7-162">Concettualmente, la convalida del certificato è un problema di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="c33c7-162">Conceptually, the validation of the certificate is an authorization concern.</span></span> <span data-ttu-id="c33c7-163">L'aggiunta di un controllo, ad esempio, di un'autorità emittente o di un'identificazione personale in un criterio di autorizzazione, anziché all'interno di `OnCertificateValidated`, è perfettamente accettabile.</span><span class="sxs-lookup"><span data-stu-id="c33c7-163">Adding a check on, for example, an issuer or thumbprint in an authorization policy, rather than inside `OnCertificateValidated`, is perfectly acceptable.</span></span>

## <a name="configure-your-host-to-require-certificates"></a><span data-ttu-id="c33c7-164">Configurare l'host per richiedere i certificati</span><span class="sxs-lookup"><span data-stu-id="c33c7-164">Configure your host to require certificates</span></span>

### <a name="kestrel"></a><span data-ttu-id="c33c7-165">Kestrel</span><span class="sxs-lookup"><span data-stu-id="c33c7-165">Kestrel</span></span>

<span data-ttu-id="c33c7-166">In *Program.cs*configurare gheppio come segue:</span><span class="sxs-lookup"><span data-stu-id="c33c7-166">In *Program.cs*, configure Kestrel as follows:</span></span>

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

### <a name="iis"></a><span data-ttu-id="c33c7-167">IIS</span><span class="sxs-lookup"><span data-stu-id="c33c7-167">IIS</span></span>

<span data-ttu-id="c33c7-168">Completare i passaggi seguenti in Gestione IIS:</span><span class="sxs-lookup"><span data-stu-id="c33c7-168">Complete the following steps In IIS Manager:</span></span>

1. <span data-ttu-id="c33c7-169">Selezionare il sito dalla scheda **connessioni** .</span><span class="sxs-lookup"><span data-stu-id="c33c7-169">Select your site from the **Connections** tab.</span></span>
1. <span data-ttu-id="c33c7-170">Fare doppio clic sull'opzione **Impostazioni SSL** nella finestra **visualizzazione funzionalità** .</span><span class="sxs-lookup"><span data-stu-id="c33c7-170">Double-click the **SSL Settings** option in the **Features View** window.</span></span>
1. <span data-ttu-id="c33c7-171">Selezionare la casella di controllo **Richiedi SSL** e selezionare il pulsante di opzione **Richiedi** nella sezione **certificati client** .</span><span class="sxs-lookup"><span data-stu-id="c33c7-171">Check the **Require SSL** checkbox, and select the **Require** radio button in the **Client certificates** section.</span></span>

![Impostazioni del certificato client in IIS](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a><span data-ttu-id="c33c7-173">Azure e proxy Web personalizzati</span><span class="sxs-lookup"><span data-stu-id="c33c7-173">Azure and custom web proxies</span></span>

<span data-ttu-id="c33c7-174">Vedere l' [host e distribuire la documentazione](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) per informazioni su come configurare il middleware di invio del certificato.</span><span class="sxs-lookup"><span data-stu-id="c33c7-174">See the [host and deploy documentation](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) for how to configure the certificate forwarding middleware.</span></span>
