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
# <a name="configure-certificate-authentication-in-aspnet-core"></a>Configurare l'autenticazione del certificato in ASP.NET Core

`Microsoft.AspNetCore.Authentication.Certificate` contiene un'implementazione simile all' [autenticazione del certificato](https://tools.ietf.org/html/rfc5246#section-7.4.4) per ASP.NET Core. L'autenticazione del certificato viene eseguita a livello di TLS, molto prima che venga mai ASP.NET Core. Più precisamente, si tratta di un gestore di autenticazione che convalida il certificato e quindi fornisce un evento in cui è possibile risolvere il certificato in un `ClaimsPrincipal`. 

[Configurare l'host](#configure-your-host-to-require-certificates) per l'autenticazione del certificato, essere IIS, gheppio, app Web di Azure o qualsiasi altro elemento in uso.

## <a name="proxy-and-load-balancer-scenarios"></a>Scenari di proxy e bilanciamento del carico

L'autenticazione del certificato è uno scenario con stato usato principalmente quando un proxy o un servizio di bilanciamento del carico non gestisce il traffico tra client e server. Se si usa un proxy o un servizio di bilanciamento del carico, l'autenticazione del certificato funziona solo se il proxy o il servizio di bilanciamento del carico:

* Gestisce l'autenticazione.
* Passa le informazioni di autenticazione utente all'app, ad esempio in un'intestazione di richiesta, che agisce sulle informazioni di autenticazione.

Un'alternativa all'autenticazione del certificato negli ambienti in cui vengono usati proxy e servizi di bilanciamento del carico è Active Directory servizi federati (ADFS) con OpenID Connect (OIDC).

## <a name="get-started"></a>Introduzione

Acquisire un certificato HTTPS, applicarlo e [configurare l'host](#configure-your-host-to-require-certificates) per richiedere i certificati.

Nell'app Web aggiungere un riferimento al pacchetto `Microsoft.AspNetCore.Authentication.Certificate`. Quindi, nel metodo `Startup.ConfigureServices`, chiamare `services.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` con le opzioni, fornendo un delegato per `OnCertificateValidated` eseguire qualsiasi convalida supplementare sul certificato client inviato con le richieste. Trasformare le informazioni in un `ClaimsPrincipal` e impostarle sulla proprietà `context.Principal`.

Se l'autenticazione ha esito negativo, questo gestore restituisce una risposta `403 (Forbidden)` piuttosto una `401 (Unauthorized)`, come si può immaginare. Il motivo è che l'autenticazione deve verificarsi durante la connessione TLS iniziale. Quando raggiunge il gestore, è troppo tardi. Non è possibile aggiornare la connessione da una connessione anonima a un'altra con un certificato.

Aggiungere anche `app.UseAuthentication();` nel metodo `Startup.Configure`. In caso contrario, HttpContext. User non verrà impostato su `ClaimsPrincipal` creato dal certificato. Esempio:

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

Nell'esempio precedente viene illustrata la modalità predefinita per aggiungere l'autenticazione del certificato. Il gestore crea un'entità utente usando le proprietà comuni del certificato.

## <a name="configure-certificate-validation"></a>Configurare la convalida del certificato

Il gestore `CertificateAuthenticationOptions` dispone di alcune convalide predefinite che rappresentano le convalide minime che è necessario eseguire su un certificato. Ognuna di queste impostazioni è abilitata per impostazione predefinita.

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a>AllowedCertificateTypes = concatenato, SelfSigned o tutti (concatenato | SelfSigned)

Questo controllo consente di verificare che sia consentito solo il tipo di certificato appropriato.

### <a name="validatecertificateuse"></a>ValidateCertificateUse

Questo controllo verifica che il certificato presentato dal client disponga dell'utilizzo chiavi avanzato (EKU) di autenticazione client o non EKU affatto. Come le specifiche dicono, se non è specificato alcun EKU, tutti i EKU vengono considerati validi.

### <a name="validatevalidityperiod"></a>ValidateValidityPeriod

Questo controllo verifica che il certificato sia entro il periodo di validità. Per ogni richiesta, il gestore garantisce che un certificato valido al momento della presentazione non sia scaduto durante la sessione corrente.

### <a name="revocationflag"></a>RevocationFlag

Flag che specifica i certificati nella catena controllati per la revoca.

I controlli di revoca vengono eseguiti solo quando il certificato è concatenato a un certificato radice.

### <a name="revocationmode"></a>RevocationMode

Flag che specifica la modalità di esecuzione dei controlli di revoca.

La specifica di un controllo online può comportare un ritardo prolungato mentre viene contattata l'autorità di certificazione.

I controlli di revoca vengono eseguiti solo quando il certificato è concatenato a un certificato radice.

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a>È possibile configurare l'app in modo che richieda un certificato solo su determinati percorsi?

Questa operazione non è possibile. Tenere presente che lo scambio di certificati è stato eseguito dall'avvio della conversazione HTTPS, che viene eseguito dal server prima che la prima richiesta venga ricevuta sulla connessione, quindi non è possibile definire l'ambito in base a qualsiasi campo di richiesta.

## <a name="handler-events"></a>Eventi gestore

Il gestore dispone di due eventi:

* `OnAuthenticationFailed` &ndash; chiamato se si verifica un'eccezione durante l'autenticazione e consente di rispondere.
* `OnCertificateValidated` &ndash; chiamato dopo che il certificato è stato convalidato, è stata passata la convalida ed è stata creata un'entità predefinita. Questo evento consente di eseguire una convalida personalizzata e di aumentare o sostituire l'entità di protezione. Gli esempi includono:
  * Determinare se il certificato è noto ai servizi.
  * Creazione di un'entità personalizzata. Si consideri l'esempio seguente in `Startup.ConfigureServices`:

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

Se il certificato in ingresso non soddisfa la convalida aggiuntiva, chiamare `context.Fail("failure reason")` con un motivo dell'errore.

Per le funzionalità reali, è probabile che si voglia chiamare un servizio registrato nell'inserimento delle dipendenze che si connette a un database o a un altro tipo di archivio utente. Accedere al servizio usando il contesto passato al delegato. Si consideri l'esempio seguente in `Startup.ConfigureServices`:

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

Concettualmente, la convalida del certificato è un problema di autorizzazione. L'aggiunta di un controllo, ad esempio, di un'autorità emittente o di un'identificazione personale in un criterio di autorizzazione, anziché all'interno di `OnCertificateValidated`, è perfettamente accettabile.

## <a name="configure-your-host-to-require-certificates"></a>Configurare l'host per richiedere i certificati

### <a name="kestrel"></a>Kestrel

In *Program.cs*configurare gheppio come segue:

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

### <a name="iis"></a>IIS

Completare i passaggi seguenti in Gestione IIS:

1. Selezionare il sito dalla scheda **connessioni** .
1. Fare doppio clic sull'opzione **Impostazioni SSL** nella finestra **visualizzazione funzionalità** .
1. Selezionare la casella di controllo **Richiedi SSL** e selezionare il pulsante di opzione **Richiedi** nella sezione **certificati client** .

![Impostazioni del certificato client in IIS](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a>Azure e proxy Web personalizzati

Vedere l' [host e distribuire la documentazione](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) per informazioni su come configurare il middleware di invio del certificato.
