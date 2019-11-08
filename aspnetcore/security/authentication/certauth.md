---
title: Configurare l'autenticazione del certificato in ASP.NET Core
author: blowdart
description: Informazioni su come configurare l'autenticazione del certificato in ASP.NET Core per IIS e HTTP. sys.
monikerRange: '>= aspnetcore-3.0'
ms.author: bdorrans
ms.date: 11/07/2019
uid: security/authentication/certauth
ms.openlocfilehash: 0db23c325f0b1f5a6500e3b2549db170e3df97c5
ms.sourcegitcommit: 68d804d60e104c81fe77a87a9af70b5df2726f60
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/08/2019
ms.locfileid: "73830712"
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

Nell'app Web aggiungere un riferimento al pacchetto `Microsoft.AspNetCore.Authentication.Certificate`. Quindi, nel metodo `Startup.ConfigureServices`, chiamare `services.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).AddCertificate(...);` con le opzioni, fornendo un delegato per `OnCertificateValidated` eseguire qualsiasi convalida supplementare sul certificato client inviato con le richieste. Trasformare le informazioni in un `ClaimsPrincipal` e impostarle sulla proprietà `context.Principal`.

Se l'autenticazione ha esito negativo, questo gestore restituisce una risposta `403 (Forbidden)` piuttosto una `401 (Unauthorized)`, come si può immaginare. Il motivo è che l'autenticazione deve verificarsi durante la connessione TLS iniziale. Quando raggiunge il gestore, è troppo tardi. Non è possibile aggiornare la connessione da una connessione anonima a un'altra con un certificato.

Aggiungere anche `app.UseAuthentication();` nel metodo `Startup.Configure`. In caso contrario, il `HttpContext.User` non verrà impostato su `ClaimsPrincipal` creato dal certificato. Esempio:

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
public static void Main(string[] args)
{
    CreateHostBuilder(args).Build().Run();
}

public static IHostBuilder CreateHostBuilder(string[] args)
{
    return Host.CreateDefaultBuilder(args)
               .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                    webBuilder.ConfigureKestrel(o =>
                    {
                        o.ConfigureHttpsDefaults(o => o.ClientCertificateMode = ClientCertificateMode.RequireCertificate);
                    });
                });
}
```

### <a name="iis"></a>IIS

Completare i passaggi seguenti in Gestione IIS:

1. Selezionare il sito dalla scheda **connessioni** .
1. Fare doppio clic sull'opzione **Impostazioni SSL** nella finestra **visualizzazione funzionalità** .
1. Selezionare la casella di controllo **Richiedi SSL** e selezionare il pulsante di opzione **Richiedi** nella sezione **certificati client** .

![Impostazioni del certificato client in IIS](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a>Azure e proxy Web personalizzati

Vedere l' [host e distribuire la documentazione](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) per informazioni su come configurare il middleware di invio del certificato.

### <a name="use-certificate-authentication-in-azure-web-apps"></a>Usare l'autenticazione del certificato in app Web di Azure

Il metodo `AddCertificateForwarding` viene usato per specificare:

* Nome dell'intestazione del client.
* Modalità di caricamento del certificato (utilizzando la proprietà `HeaderConverter`).

Nelle app Web di Azure il certificato viene passato come intestazione di richiesta personalizzata denominata `X-ARR-ClientCert`. Per usarlo, configurare l'invio del certificato nel `Startup.ConfigureServices`:

```csharp
services.AddCertificateForwarding(options =>
{
    options.CertificateHeader = "X-ARR-ClientCert";
    options.HeaderConverter = (headerValue) =>
    {
        X509Certificate2 clientCertificate = null;
        if(!string.IsNullOrWhiteSpace(headerValue))
        {
            byte[] bytes = StringToByteArray(headerValue);
            clientCertificate = new X509Certificate2(bytes);
        }

        return clientCertificate;
    };
});
```

Il metodo `Startup.Configure` aggiunge quindi il middleware. `UseCertificateForwarding` viene chiamato prima delle chiamate a `UseAuthentication` e `UseAuthorization`:

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...
    
    app.UseRouting();

    app.UseCertificateForwarding();
    app.UseAuthentication();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
```

Una classe separata può essere usata per implementare la logica di convalida. Poiché in questo esempio viene usato lo stesso certificato autofirmato, assicurarsi che sia possibile usare solo il certificato. Verificare che le identificazioni personali del certificato client e del certificato del server corrispondano; in caso contrario, è possibile usare qualsiasi certificato e sarà sufficiente per l'autenticazione. Viene usato all'interno del metodo `AddCertificate`. È anche possibile convalidare l'oggetto o l'autorità emittente qui se si usano certificati intermedi o figlio.

```csharp
using System.IO;
using System.Security.Cryptography.X509Certificates;

namespace AspNetCoreCertificateAuthApi
{
    public class MyCertificateValidationService
    {
        public bool ValidateCertificate(X509Certificate2 clientCertificate)
        {
            // Do not hardcode passwords in production code, use thumbprint or key vault
            var cert = new X509Certificate2(Path.Combine("sts_dev_cert.pfx"), "1234");
            if (clientCertificate.Thumbprint == cert.Thumbprint)
            {
                return true;
            }

            return false;
        }
    }
}
```

#### <a name="implement-an-httpclient-using-a-certificate"></a>Implementare un HttpClient usando un certificato

Il client dell'API Web usa un `HttpClient`, che è stato creato usando un'istanza di `IHttpClientFactory`. Questa operazione non consente di definire un gestore per la `HttpClient`, quindi utilizzare un `HttpRequestMessage` per aggiungere il certificato all'intestazione della richiesta di `X-ARR-ClientCert`. Il certificato viene aggiunto come stringa usando il metodo `GetRawCertDataString`. 

```csharp
private async Task<JsonDocument> GetApiDataAsync()
{
    try
    {
        // Do not hardcode passwords in production code, use thumbprint or key vault
        var cert = new X509Certificate2(Path.Combine(_environment.ContentRootPath, "sts_dev_cert.pfx"), "1234");

        var client = _clientFactory.CreateClient();

        var request = new HttpRequestMessage()
        {
            RequestUri = new Uri("https://localhost:44379/api/values"),
            Method = HttpMethod.Get,
        };

        request.Headers.Add("X-ARR-ClientCert", cert.GetRawCertDataString());
        var response = await client.SendAsync(request);

        if (response.IsSuccessStatusCode)
        {
            var responseContent = await response.Content.ReadAsStringAsync();
            var data = JsonDocument.Parse(responseContent);

            return data;
        }

        throw new ApplicationException($"Status code: {response.StatusCode}, Error: {response.ReasonPhrase}");
    }
    catch (Exception e)
    {
        throw new ApplicationException($"Exception {e}");
    }
}
```

Se il certificato corretto viene inviato al server, vengono restituiti i dati. Se non viene inviato alcun certificato o il certificato errato, viene restituito un codice di stato HTTP 403.

### <a name="create-certificates-in-powershell"></a>Creare certificati in PowerShell

La creazione dei certificati è la parte più difficile per la configurazione di questo flusso. È possibile creare un certificato radice usando il cmdlet `New-SelfSignedCertificate` PowerShell. Quando si crea il certificato, usare una password complessa. È importante aggiungere il parametro `KeyUsageProperty` e il parametro `KeyUsage`, come illustrato.

#### <a name="create-root-ca"></a>Crea CA radice

```powershell
New-SelfSignedCertificate -DnsName "root_ca_dev_damienbod.com", "root_ca_dev_damienbod.com" -CertStoreLocation "cert:\LocalMachine\My" -NotAfter (Get-Date).AddYears(20) -FriendlyName "root_ca_dev_damienbod.com" -KeyUsageProperty All -KeyUsage CertSign, CRLSign, DigitalSignature

$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\root_ca_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath root_ca_dev_damienbod.crt
```

#### <a name="install-in-the-trusted-root"></a>Installare nella radice attendibile

Il certificato radice deve essere considerato attendibile nel sistema host. Un certificato radice che non è stato creato da un'autorità di certificazione non sarà considerato attendibile per impostazione predefinita. Il collegamento seguente illustra come eseguire questa operazione in Windows:

https://social.msdn.microsoft.com/Forums/SqlServer/5ed119ef-1704-4be4-8a4f-ef11de7c8f34/a-certificate-chain-processed-but-terminated-in-a-root-certificate-which-is-not-trusted-by-the

#### <a name="intermediate-certificate"></a>Certificato intermedio

È ora possibile creare un certificato intermedio dal certificato radice. Questa operazione non è necessaria per tutti i casi d'uso, ma potrebbe essere necessario creare molti certificati o attivare o disabilitare gruppi di certificati. Il parametro `TextExtension` è necessario per impostare la lunghezza del percorso nei vincoli di base del certificato.

Il certificato intermedio può quindi essere aggiunto al certificato intermedio attendibile nel sistema host Windows.

```powershell
$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

$parentcert = ( Get-ChildItem -Path cert:\LocalMachine\My\"The thumbprint of the root..." )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "intermediate_dev_damienbod.com" -Signer $parentcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "intermediate_dev_damienbod.com" -KeyUsageProperty All -KeyUsage CertSign, CRLSign, DigitalSignature -TextExtension @("2.5.29.19={text}CA=1&pathlength=1")

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\intermediate_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath intermediate_dev_damienbod.crt
```

#### <a name="create-child-certificate-from-intermediate-certificate"></a>Crea certificato figlio da certificato intermedio

Un certificato figlio può essere creato dal certificato intermedio. Si tratta dell'entità finale e non è necessario creare più certificati figlio.

```powershell
$parentcert = ( Get-ChildItem -Path cert:\LocalMachine\My\"The thumbprint from the Intermediate certificate..." )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "child_a_dev_damienbod.com" -Signer $parentcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "child_a_dev_damienbod.com"

$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\child_a_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath child_a_dev_damienbod.crt
```

#### <a name="create-child-certificate-from-root-certificate"></a>Crea certificato figlio dal certificato radice

È anche possibile creare un certificato figlio direttamente dal certificato radice. 

```powershell
$rootcert = ( Get-ChildItem -Path cert:\LocalMachine\My\"The thumbprint from the root cert..." )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "child_a_dev_damienbod.com" -Signer $rootcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "child_a_dev_damienbod.com"

$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\child_a_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath child_a_dev_damienbod.crt
```

#### <a name="example-root---intermediate-certificate---certificate"></a>Esempio di certificato radice-intermedio-certificato

```powershell
$mypwdroot = ConvertTo-SecureString -String "1234" -Force -AsPlainText
$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

New-SelfSignedCertificate -DnsName "root_ca_dev_damienbod.com", "root_ca_dev_damienbod.com" -CertStoreLocation "cert:\LocalMachine\My" -NotAfter (Get-Date).AddYears(20) -FriendlyName "root_ca_dev_damienbod.com" -KeyUsageProperty All -KeyUsage CertSign, CRLSign, DigitalSignature

Get-ChildItem -Path cert:\localMachine\my\0C89639E4E2998A93E423F919B36D4009A0F9991 | Export-PfxCertificate -FilePath C:\git\root_ca_dev_damienbod.pfx -Password $mypwdroot

Export-Certificate -Cert cert:\localMachine\my\0C89639E4E2998A93E423F919B36D4009A0F9991 -FilePath root_ca_dev_damienbod.crt

$rootcert = ( Get-ChildItem -Path cert:\LocalMachine\My\0C89639E4E2998A93E423F919B36D4009A0F9991 )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "child_a_dev_damienbod.com" -Signer $rootcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "child_a_dev_damienbod.com" -KeyUsageProperty All -KeyUsage CertSign, CRLSign, DigitalSignature -TextExtension @("2.5.29.19={text}CA=1&pathlength=1")

Get-ChildItem -Path cert:\localMachine\my\BA9BF91ED35538A01375EFC212A2F46104B33A44 | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\child_a_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\BA9BF91ED35538A01375EFC212A2F46104B33A44 -FilePath child_a_dev_damienbod.crt

$parentcert = ( Get-ChildItem -Path cert:\LocalMachine\My\BA9BF91ED35538A01375EFC212A2F46104B33A44 )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "child_b_from_a_dev_damienbod.com" -Signer $parentcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "child_b_from_a_dev_damienbod.com" 

Get-ChildItem -Path cert:\localMachine\my\141594A0AE38CBBECED7AF680F7945CD51D8F28A | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\child_b_from_a_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\141594A0AE38CBBECED7AF680F7945CD51D8F28A -FilePath child_b_from_a_dev_damienbod.crt
```

Quando si utilizzano i certificati radice, intermedi o figlio, è possibile convalidare i certificati utilizzando l'emittente o l'oggetto in base alle esigenze.

```csharp
using System.Collections.Generic;
using System.IO;
using System.Security.Cryptography.X509Certificates;

namespace AspNetCoreCertificateAuthApi
{
    public class MyCertificateValidationService 
    {
        public bool ValidateCertificate(X509Certificate2 clientCertificate)
        {
            return CheckIfThumbprintIsValid(clientCertificate);
        }

        private bool CheckIfThumbprintIsValid(X509Certificate2 clientCertificate)
        {
            var listOfValidThumbprints = new List<string>
            {
                "141594A0AE38CBBECED7AF680F7945CD51D8F28A",
                "0C89639E4E2998A93E423F919B36D4009A0F9991",
                "BA9BF91ED35538A01375EFC212A2F46104B33A44"
            };

            if (listOfValidThumbprints.Contains(clientCertificate.Thumbprint))
            {
                return true;
            }

            return false;
        }
    }
}
```
