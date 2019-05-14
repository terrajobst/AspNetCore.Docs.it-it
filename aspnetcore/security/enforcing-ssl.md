---
title: Imporre HTTPS in ASP.NET Core
author: rick-anderson
description: Informazioni su come richiedere HTTPS/TLS in un'app web ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: security/enforcing-ssl
ms.openlocfilehash: e27e0c31b128cbd7d71bf7b83a2d33cc89ea3ab1
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610434"
---
# <a name="enforce-https-in-aspnet-core"></a>Imporre HTTPS in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questo documento illustra come:

* La richiesta di HTTPS per tutte le richieste.
* Reindirizzare tutte le richieste HTTP a HTTPS.

Nessuna API può impedire a un client di inviare i dati sensibili alla prima richiesta.

> [!WARNING]
> Effettuare **non** utilizzare [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) nelle API Web che ricevono le informazioni riservate. `RequireHttpsAttribute` Usa i codici di stato HTTP per reindirizzare i browser da HTTP a HTTPS. I client dell'API non possono comprendere o rispettare effettua il reindirizzamento da HTTP a HTTPS. Tali client possono inviare le informazioni su HTTP. Le API Web devono essere:
>
> * È in ascolto su HTTP.
> * Chiudere la connessione con il codice di stato 400 (Bad Request) e non rispondere alla richiesta.

## <a name="require-https"></a>Richiedere HTTPS

::: moniker range=">= aspnetcore-2.1"

È consigliabile che la produzione di ASP.NET Core web App chiamata:

* Middleware di reindirizzamento HTTPS (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) per reindirizzare le richieste HTTP a HTTPS.
* Middleware HSTS ([UseHsts](#http-strict-transport-security-protocol-hsts)) per inviare le intestazioni HTTP Strict Transport Security protocollo (HSTS) ai client.

> [!NOTE]
> Le app distribuite in una configurazione proxy inverso consentono il proxy per la gestione di sicurezza della connessione (HTTPS). Se il proxy gestisce anche il reindirizzamento HTTPS, non è necessario usare il Middleware di reindirizzamento HTTPS. Se il server proxy gestisce inoltre la scrittura delle intestazioni HSTS (ad esempio, [HSTS native supportano in IIS 10.0 (1709) o versione successiva](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), Middleware HSTS non è richiesto dall'app. Per altre informazioni, vedere [Opt-out di HTTPS/HSTS al momento della creazione progetto](#opt-out-of-httpshsts-on-project-creation).

### <a name="usehttpsredirection"></a>UseHttpsRedirection

Il codice seguente chiama `UseHttpsRedirection` nella `Startup` classe:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

Il codice evidenziato sopra:

* Usa il valore predefinito [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).
* Usa il valore predefinito [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) a meno che non viene sottoposto a override per il `ASPNETCORE_HTTPS_PORT` variabile di ambiente oppure [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).

È consigliabile utilizzare reindirizzamenti temporanei anziché di reindirizzamenti permanenti. La memorizzazione nella cache di collegamento, rendendo instabile il comportamento può causare in ambienti di sviluppo. Se si preferisce inviare un codice di stato come reindirizzamento permanente quando l'app si trova in un ambiente non di sviluppo, vedere la [configurare i reindirizzamenti permanenti nell'ambiente di produzione](#configure-permanent-redirects-in-production) sezione. È consigliabile usare [HSTS](#http-strict-transport-security-protocol-hsts) per segnalare ai client solo di proteggere risorse richieste devono essere inviate all'app (solo nell'ambiente di produzione).

### <a name="port-configuration"></a>Configurazione della porta

Una porta deve essere disponibile per il middleware per reindirizzare una richiesta non sicura a HTTPS. Se non è disponibile alcuna porta:

* Non si verifica il reindirizzamento a HTTPS.
* Il middleware registra l'avviso "Impossibile determinare la porta https per reindirizzamento".

Specificare la porta HTTPS usando uno degli approcci seguenti:

* Impostare [HttpsRedirectionOptions.HttpsPort](#options).
* Impostare il `ASPNETCORE_HTTPS_PORT` variabile di ambiente oppure [impostazione di configurazione Host Web https_port](xref:fundamentals/host/web-host#https-port):

  **Chiave**: `https_port`  
  **Tipo**: *string*  
  **Predefinito**: non è impostato nessun valore predefinito.  
  **Impostare usando**: `UseSetting`  
  **Variabile di ambiente**: `<PREFIX_>HTTPS_PORT` (È il prefisso `ASPNETCORE_` quando si usano i [Host Web](xref:fundamentals/host/web-host).)

  Quando si configura un <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> in `Program`:

  [!code-csharp[](enforcing-ssl/sample-snapshot/Program.cs?name=snippet_Program&highlight=10)]
* Indicare una porta con lo schema sicuro usando la `ASPNETCORE_URLS` variabile di ambiente. La variabile di ambiente consente di configurare il server. Il middleware indirettamente individua la porta HTTPS tramite <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>. Questo approccio non funziona nelle distribuzioni di proxy inverso.
* In fase di sviluppo, impostare un URL HTTPS *launchsettings. JSON*. Abilitare HTTPS quando si usa IIS Express.
* Configurare un endpoint di URL HTTPS per una distribuzione edge rivolte al pubblico di [Kestrel](xref:fundamentals/servers/kestrel) server oppure [HTTP. sys](xref:fundamentals/servers/httpsys) server. Solo **una sola porta HTTPS** viene usata dall'app. Il middleware consente di individuare la porta tramite <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.

> [!NOTE]
> Quando un'app viene eseguita in una configurazione proxy inverso, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> non è disponibile. Impostare la porta usando uno degli altri approcci descritti in questa sezione.

Se Kestrel o HTTP. sys è utilizzato come un server perimetrale rivolte al pubblico, Kestrel o HTTP. sys deve essere configurato per l'ascolto su entrambi:

* La porta sicura in cui viene reindirizzato il client (in genere, 443 in 5001 in fase di sviluppo e produzione).
* La porta non protetta (in genere, 80 nell'ambiente di produzione) e 5000 in fase di sviluppo.

La porta non protetta deve essere accessibile dal client affinché l'app per ricevere una richiesta non sicura e reindirizzare il client alla porta sicura.

Per altre informazioni, vedere [configurazione dell'endpoint di Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) o <xref:fundamentals/servers/httpsys>.

### <a name="deployment-scenarios"></a>Scenari di distribuzione

Eventuali firewall tra il client e il server deve inoltre essere le porte di comunicazione aperto per il traffico.

Se le richieste vengono inoltrate in una configurazione proxy inverso, usare [Middleware delle intestazioni inoltrate](xref:host-and-deploy/proxy-load-balancer) prima di chiamare il Middleware di reindirizzamento HTTPS. Gli aggiornamenti di Middleware delle intestazioni inoltrate le `Request.Scheme`, usando il `X-Forwarded-Proto` intestazione. I permessi di middleware redirect URI e altri criteri di sicurezza per funzionare correttamente. Quando non viene usato Middleware delle intestazioni inoltrate, l'app back-end potrebbe non ricevere lo schema appropriato e finire in un ciclo di reindirizzamento. Un messaggio di errore comuni di utente finale è che si sono verificati troppi reindirizzamenti.

Durante la distribuzione in servizio App di Azure, seguire le indicazioni fornite in [esercitazione: Associare un certificato SSL personalizzato esistente ad App Web di Azure](/azure/app-service/app-service-web-tutorial-custom-ssl).

### <a name="options"></a>Opzioni

L'evidenziato seguente codice chiama [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) per configurare le opzioni del middleware:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

La chiamata `AddHttpsRedirection` è necessario modificare i valori di solo `HttpsPort` o `RedirectStatusCode`.

Il codice evidenziato sopra:

* Set [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) a <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, ovvero il valore predefinito. Usare i campi del <xref:Microsoft.AspNetCore.Http.StatusCodes> classe per le assegnazioni ai `RedirectStatusCode`.
* Nastavuje port HTTPS in 5001. Il valore predefinito è 443.

#### <a name="configure-permanent-redirects-in-production"></a>Configurare i reindirizzamenti permanenti nell'ambiente di produzione

Valore predefinito è il middleware per l'invio di un [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) con tutti i reindirizzamenti. Se si preferisce inviare un codice di stato come reindirizzamento permanente quando l'app si trova in un ambiente non di sviluppo, eseguire il wrapping le opzioni di configurazione del middleware in un controllo condizionale per un ambiente non di sviluppo.

Quando si configura un `IWebHostBuilder` nelle *Startup.cs*:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IHostingEnvironment (stored in _env) is injected into the Startup class.
    if (!_env.IsDevelopment())
    {
        services.AddHttpsRedirection(options =>
        {
            options.RedirectStatusCode = StatusCodes.Status308PermanentRedirect;
            options.HttpsPort = 443;
        });
    }
}
```

## <a name="https-redirection-middleware-alternative-approach"></a>Approccio alternativo Middleware reindirizzamento HTTPS

Un'alternativa all'uso del Middleware di reindirizzamento HTTPS (`UseHttpsRedirection`) consiste nell'usare Middleware riscrittura URL (`AddRedirectToHttps`). `AddRedirectToHttps` può anche impostare il codice di stato e la porta quando viene eseguito il reindirizzamento. Per altre informazioni, vedere [Middleware riscrittura URL](xref:fundamentals/url-rewriting).

Quando si reindirizzano a HTTPS senza la necessità per le regole di reindirizzamento supplementare, è consigliabile usare il Middleware di reindirizzamento HTTPS (`UseHttpsRedirection`) descritti in questo argomento.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

Il [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) viene utilizzato per richiedere HTTPS. `[RequireHttpsAttribute]` può decorare i controller o metodi o possono essere applicati a livello globale. Per applicare l'attributo a livello globale, aggiungere il codice seguente al `ConfigureServices` in `Startup`:

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Il codice evidenziato sopra è necessario utilizzano tutte le richieste `HTTPS`; di conseguenza, le richieste HTTP vengono ignorate. Il codice evidenziato seguente reindirizza tutte le richieste HTTP a HTTPS:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Per altre informazioni, vedere [Middleware riscrittura URL](xref:fundamentals/url-rewriting). Il middleware consente anche all'app per impostare il codice di stato o il codice di stato e la porta quando viene eseguito il reindirizzamento.

Richiesta di HTTPS a livello globale (`options.Filters.Add(new RequireHttpsAttribute());`) è una protezione ottimale. Applicando la `[RequireHttps]` attributo a tutte le pagine Razor/controller non è considerata sicura come richiesta di HTTPS a livello globale. Non è possibile garantire la `[RequireHttps]` attributo viene applicato quando vengono aggiunti nuovi controller e le pagine Razor.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="http-strict-transport-security-protocol-hsts"></a>Protocollo HTTP Strict Transport Security (HSTS)

Per ogni [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) è un miglioramento della sicurezza con consenso esplicito specificato da un'app web tramite l'uso di un'intestazione di risposta. Quando un [browser che supporti HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) riceve questa intestazione:

* Il browser memorizza la configurazione per il dominio che impedisce l'invio di tutte le comunicazioni su HTTP. Il browser forza tutte le comunicazioni tramite HTTPS.
* Il browser impedisce all'utente di utilizzo di certificati non attendibili o non validi. Il browser Disabilita prompt che permettono all'utente di considerare temporaneamente attendibile questo certificato.

Poiché HSTS viene applicato per il client lo presenta alcune limitazioni:

* Il client deve supportare HSTS.
* HSTS richiede almeno una richiesta HTTPS con esito positivo per stabilire i criteri HSTS.
* L'applicazione deve controllare ogni richiesta HTTP e reindirizzare o rifiutare la richiesta HTTP.

ASP.NET Core 2.1 o versioni successive implementa HSTS con il `UseHsts` metodo di estensione. Il codice seguente chiama `UseHsts` quando l'app non è presente nel [modalità di sviluppo](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` non è consigliato in fase di sviluppo perché le impostazioni HSTS sono altamente memorizzabile nella cache dal browser. Per impostazione predefinita, `UseHsts` esclude l'indirizzo di loopback locale.

Per gli ambienti di produzione, l'implementazione di HTTPS per la prima volta, impostare iniziale [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) su un valore ridotto usando uno del <xref:System.TimeSpan> metodi. Impostare il valore delle ore a non più di un singolo giorno nel caso in cui sia necessario ripristinare l'infrastruttura HTTPS a HTTP. Dopo che si è ritenuto affidabile la sostenibilità della configurazione di HTTPS, aumentare il valore di max-age HSTS; un valore di uso comune è un anno.

Il codice seguente:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Imposta il parametro precaricamento dell'intestazione Strict-Transport-Security. Precaricamento non fa parte del [specifica RFC HSTS](https://tools.ietf.org/html/rfc6797), ma è supportato dal web browser per precaricare siti HSTS nella nuova installazione. Per altre informazioni, vedere [https://hstspreload.org/](https://hstspreload.org/).
* Abilita [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), cui viene applicato il criterio HSTS per ospitare sottodomini.
* Imposta in modo esplicito il parametro di validità massima dell'intestazione Strict-Transport-Security per 60 giorni. Se non impostato, il valore predefinito è 30 giorni. Vedere le [direttiva max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) per altre informazioni.
* Aggiunge `example.com` all'elenco degli host da escludere.

`UseHsts` esclude gli host di loopback seguenti:

* `localhost`: L'indirizzo di loopback IPv4.
* `127.0.0.1`: L'indirizzo di loopback IPv4.
* `[::1]`: L'indirizzo di loopback IPv6.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="opt-out-of-httpshsts-on-project-creation"></a>Rifiutare esplicitamente di HTTPS/HSTS sulla creazione di progetti

In alcuni scenari di servizio back-end in cui la protezione della connessione viene gestita nei dispositivi perimetrali pubbliche della rete, la configurazione di sicurezza della connessione in ogni nodo non è obbligatorio. Generata dai modelli in Visual Studio o da app Web di [dotnet nuove](/dotnet/core/tools/dotnet-new) comando enable [il reindirizzamento HTTPS](#require-https) e [HSTS](#http-strict-transport-security-protocol-hsts). Per le distribuzioni che non richiedono questi scenari, è possibile rifiutare esplicitamente di HTTPS/HSTS quando l'app viene creata dal modello.

Per rifiutare esplicitamente di HTTPS/HSTS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Deselezionare i **Configura per HTTPS** casella di controllo.

![Nuova applicazione Web ASP.NET Core finestra di dialogo che mostra la configura per HTTPS casella di controllo deselezionata.](enforcing-ssl/_static/out.png)

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli) 

Usare l'opzione `--no-https`. Esempio:

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a>Considerare attendibile il certificato di sviluppo di ASP.NET Core HTTPS in Windows e macOS

.NET core SDK include un certificato di sviluppo HTTPS. Il certificato viene installato come parte dell'esperienza della prima esecuzione. Ad esempio, `dotnet --info` genera un output simile al seguente:

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

L'installazione di .NET Core SDK include l'installazione del certificato di sviluppo HTTPS ASP.NET Core nell'archivio certificati utente locale. Il certificato è stato installato, ma non è attendibile. Per considerare attendibile il certificato di eseguire l'operazione una tantum per l'esecuzione di dotnet `dev-certs` strumento:

```console
dotnet dev-certs https --trust
```

Il comando seguente consente di visualizzare informazioni della Guida sullo strumento `dev-certs`:

```console
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a>Come configurare un certificato dello sviluppatore per Docker

Visualizzare [questo problema su GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6199).

::: moniker-end

<a name="wsl"></a>

## <a name="trust-https-certificate-from-windows-subsystem-for-linux"></a>Considerare attendibile il certificato HTTPS dal sottosistema Windows per Linux

Il sottosistema Windows per Linux (WSL) genera un certificato autofirmato HTTPS. Per configurare l'archivio certificati Windows per considerare attendibile il certificato WSL:

* Eseguire il comando seguente per esportare il certificato WSL generato: `dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`
* In una finestra del sottosistema Windows per Linux, eseguire il comando seguente: `ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`

  Il comando precedente imposta le variabili di ambiente in modo Linux Usa il certificato attendibile di Windows.

## <a name="additional-information"></a>Informazioni aggiuntive

* <xref:host-and-deploy/proxy-load-balancer>
* [Host ASP.NET Core in Linux con Apache: Configurazione di HTTPS](xref:host-and-deploy/linux-apache#https-configuration)
* [Host ASP.NET Core in Linux con Nginx: Configurazione di HTTPS](xref:host-and-deploy/linux-nginx#https-configuration)
* [Come configurare SSL in IIS](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [Supporto browser di OWASP HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
