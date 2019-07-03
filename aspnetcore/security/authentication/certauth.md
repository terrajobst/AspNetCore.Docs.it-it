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
# <a name="overview"></a>Panoramica

`Microsoft.AspNetCore.Authentication.Certificate` contiene un'implementazione simile a [l'autenticazione del certificato](https://tools.ietf.org/html/rfc5246#section-7.4.4) per ASP.NET Core. L'autenticazione del certificato si verifica a livello TLS, lungo prima che diventi mai ad ASP.NET Core. Più precisamente, questo è un gestore di autenticazione che convalida il certificato e quindi ti offre un evento in cui è possibile risolvere tale certificato per un `ClaimsPrincipal`. 

[Configurare l'host](#configure-your-host-to-require-certificates) per l'autenticazione del certificato, sia esso IIS, Kestrel, App Web di Azure o qualsiasi altro componente in uso.

## <a name="get-started"></a>Introduzione

Acquisire un certificato HTTPS, metterle in pratica, e [configurare l'host](#configure-your-host-to-require-certificates) per richiedere i certificati.

Nell'app web, aggiungere un riferimento al `Microsoft.AspNetCore.Authentication.Certificate` pacchetto. Quindi nel `Startup.Configure` metodo, chiamare `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` le opzioni, che fornisce un delegato per `OnCertificateValidated` eseguire alcuna convalida del certificato client inviato con le richieste supplementare. Attivare tali informazioni in un `ClaimsPrincipal` e impostarlo sul `context.Principal` proprietà.

Se l'autenticazione ha esito negativo, il gestore restituisce un `403 (Forbidden)` risposta anziché una `401 (Unauthorized)`, come è prevedibile. Il concetto di base è che l'autenticazione dovrebbe verificarsi durante la connessione TLS iniziale. Per il tempo di che raggiunge il gestore è troppo tardi. Non è possibile eseguire l'aggiornamento della connessione da una connessione anonima a uno con un certificato.

Aggiungere inoltre `app.UseAuthentication();` nella `Startup.Configure` (metodo). In caso contrario, HttpContext non essere impostato su `ClaimsPrincipal` creato dal certificato. Ad esempio:

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

L'esempio precedente dimostra la modalità predefinita per aggiungere l'autenticazione del certificato. Il gestore costruisce un'entità utente usando le proprietà comuni del certificato.

## <a name="configure-certificate-validation"></a>Configurazione della convalida del certificato

Il `CertificateAuthenticationOptions` gestore ha alcune convalide incorporate che fanno le convalide minime è consigliabile eseguire in un certificato. Ognuna di queste impostazioni è abilitata per impostazione predefinita.

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a>AllowedCertificateTypes = concatenate, SelfSigned oppure tutti (concatenate | SelfSigned)

Questo controllo verifica che sia consentito solo il tipo di certificato appropriato.

### <a name="validatecertificateuse"></a>ValidateCertificateUse

Questo controllo verifica che il certificato presentato dal client con l'autenticazione Client esteso per l'uso delle chiavi (EKU), o nessun EKU affatto. Come dice le specifiche, se non viene specificato alcun utilizzo chiavi avanzato, tutti gli EKU vengono ritenuti validi.

### <a name="validatevalidityperiod"></a>ValidateValidityPeriod

Questo controllo verifica che il certificato sia entro il relativo periodo di validità. Per ogni richiesta, il gestore assicura che non sia scaduto un certificato che era valido quando viene presentato durante la sessione corrente.

### <a name="revocationflag"></a>RevocationFlag

Un flag che specifica quali certificati nella catena è controllato per la revoca.

Controlli di revoche di certificati vengono eseguiti solo quando il certificato sia concatenato a un certificato radice.

### <a name="revocationmode"></a>RevocationMode

Flag che specifica come vengono eseguiti i controlli di revoche di certificati.

Specifica di un controllo in linea può comportare un ritardo prolungato mentre viene contattato l'autorità di certificazione.

Controlli di revoche di certificati vengono eseguiti solo quando il certificato sia concatenato a un certificato radice.

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a>È possibile configurare l'app per richiedere un certificato solo in determinati percorsi?

Questo non è possibile. Tenere presente che lo scambio del certificato viene eseguito l'avvio della conversazione HTTPS, il termine dal server prima che venga ricevuta la prima richiesta su tale connessione in modo che non è possibile all'ambito basato su tutti i campi di richiesta.

## <a name="handler-events"></a>Eventi del gestore

Il gestore ha due eventi:

* `OnAuthenticationFailed` &ndash; Chiamata eseguita se un'eccezione si verifica durante l'autenticazione e consente di rispondere.
* `OnCertificateValidated` &ndash; Chiamato dopo che il certificato è stato convalidato, ha superato la convalida e sia stata creata un'entità di sicurezza predefinita. Questo evento consente di eseguire la propria convalida e migliorare o sostituire l'entità. Per alcuni esempi includono:
  * Determinare se il certificato è noto ai tuoi servizi.
  * Costruzione di propria entità. Si consideri l'esempio seguente in `Startup.ConfigureServices`:

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

Se si trova il certificato in ingresso non soddisfa la convalida aggiuntiva, chiamare `context.Fail("failure reason")` con un motivo dell'errore.

La funzionalità per il real, si sarà probabilmente necessario chiamare un servizio registrato nell'inserimento delle dipendenze che si connette a un database o di altro tipo di archivio dell'utente. Accedere al servizio utilizzando il contesto passato al delegato. Si consideri l'esempio seguente in `Startup.ConfigureServices`:

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

Concettualmente, la convalida del certificato è un problema di autorizzazione. L'aggiunta di un controllo, ad esempio, un'autorità di certificazione o identificazione personale in un criterio di autorizzazione, anziché all'interno `OnCertificateValidated`, è perfettamente accettabile.

## <a name="configure-your-host-to-require-certificates"></a>Configurare l'host per richiedere i certificati

### <a name="kestrel"></a>Kestrel

Nelle *Program.cs*, configurare Kestrel come indicato di seguito:

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

Completare i passaggi seguenti In Gestione IIS:

1. Selezionare il sito dal **connessioni** scheda.
1. Fare doppio clic il **impostazioni SSL** opzione il **visualizzazione funzionalità** finestra.
1. Controllare la **Richiedi SSL** casella di controllo e selezionare il **richiedono** pulsante di opzione nel **certificati Client** sezione.

![Impostazioni del certificato client in IIS](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a>Azure e i proxy web personalizzata

Vedere le [ospitare e distribuire la documentazione](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) per informazioni su come configurare il certificato del middleware di inoltro.
