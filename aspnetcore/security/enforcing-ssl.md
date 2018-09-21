---
title: Imporre HTTPS in ASP.NET Core
author: rick-anderson
description: Illustra come richiedere HTTPS/TLS in un ASP.NET Core web app.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 6e16191b1a4627e683fd2281e5556b2a6e84c082
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523144"
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

<a name="require"></a>
## <a name="require-https"></a>La richiesta di HTTPS

::: moniker range=">= aspnetcore-2.1"

Si consiglia di tutta la produzione di ASP.NET Core web App chiamata:

* Il Middleware di reindirizzamento HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) per reindirizzare tutte le richieste HTTP a HTTPS.
* [UseHsts](#hsts), protocollo HTTP Strict Transport Security (HSTS).

### <a name="usehttpsredirection"></a>UseHttpsRedirection

Il codice seguente chiama `UseHttpsRedirection` nella `Startup` classe:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

Il codice evidenziato sopra:

* Usa il valore predefinito [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).
* Usa il valore predefinito [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) a meno che non viene sottoposto a override per il `ASPNETCORE_HTTPS_PORT` variabile di ambiente oppure [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).

> [!WARNING] 
>Una porta deve essere disponibile per il middleware per reindirizzare a HTTPS. Se non è disponibile alcuna porta, non viene eseguito il reindirizzamento a HTTPS. È possibile specificare la porta HTTPS da uno qualsiasi dell'impostazione seguente:
> 
>* `HttpsRedirectionOptions.HttpsPort` 
>* Il `ASPNETCORE_HTTPS_PORT` variabile di ambiente. 
>* In fase di sviluppo, un url HTTPS nel *launchsettings. JSON*. 
>* Un url HTTPS configurato direttamente in Kestrel o HttpSys. 

L'evidenziato seguente codice chiama [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) per configurare le opzioni del middleware:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

La chiamata `AddHttpsRedirection` è necessario modificare i valori di solo ` HttpsPort` o ` RedirectStatusCode`;

Il codice evidenziato sopra:

* Set [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) a `Status307TemporaryRedirect`, ovvero il valore predefinito.
* Nastavuje port HTTPS in 5001. Il valore predefinito è 443.

I meccanismi seguenti impostate automaticamente la porta:

* Il middleware può individuare le porte tramite [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) quando vengono applicate le condizioni seguenti:

   * Kestrel o HTTP. sys è usate direttamente con gli endpoint HTTPS (si applica anche all'esecuzione dell'app con il debugger di Visual Studio Code).
   * Solo **una sola porta HTTPS** viene usata dall'app.

* Viene usato Visual Studio:
   * IIS Express è abilitato per HTTPS.
   * *launchsettings. JSON* imposta il `sslPort` per IIS Express.

> [!NOTE]
> Quando un'app viene eseguita dietro un proxy inverso (ad esempio, IIS, IIS Express), `IServerAddressesFeature` non è disponibile. La porta deve essere configurata manualmente. Quando la porta non è impostata, le richieste non vengono reindirizzate.

La porta può essere configurata impostando il [impostazione di configurazione Host Web https_port](xref:fundamentals/host/web-host#https-port):

**Chiave**: https_port  
**Tipo**: *string*  
**Predefinito**: non è impostato un valore predefinito.  
**Impostare usando**: `UseSetting`  
**Variabile di ambiente**: `<PREFIX_>HTTPS_PORT` (il prefisso è `ASPNETCORE_` quando si usa l'Host Web.)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> La porta può essere configurata indirettamente impostando l'URL con il `ASPNETCORE_URLS` variabile di ambiente. La variabile di ambiente consente di configurare il server e quindi il middleware indirettamente consente di individuare la porta HTTPS tramite `IServerAddressesFeature`.

Se non è impostata alcuna porta:

* Le richieste non vengono reindirizzate.
* Il middleware registra l'avviso "Impossibile determinare la porta https per reindirizzamento".

> [!NOTE]
> Un'alternativa all'uso del Middleware di reindirizzamento HTTPS (`UseHttpsRedirection`) consiste nell'usare Middleware riscrittura URL (`AddRedirectToHttps`). `AddRedirectToHttps` può anche impostare il codice di stato e la porta quando viene eseguito il reindirizzamento. Per altre informazioni, vedere [Middleware riscrittura URL](xref:fundamentals/url-rewriting).
>
> Quando si reindirizzano a HTTPS senza la necessità per le regole di reindirizzamento supplementare, è consigliabile usare il Middleware di reindirizzamento HTTPS (`UseHttpsRedirection`) descritti in questo argomento.

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

<a name="hsts"></a>
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

Per gli ambienti di produzione l'implementazione di HTTPS per la prima volta, impostare il valore HSTS iniziale su un valore ridotto. Impostare il valore delle ore a non più di un singolo giorno nel caso in cui sia necessario ripristinare l'infrastruttura HTTPS a HTTP. Dopo che si è ritenuto affidabile la sostenibilità della configurazione di HTTPS, aumentare il valore di max-age HSTS; un valore di uso comune è un anno. 

Il codice seguente:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Imposta il parametro precaricamento dell'intestazione Strict-Transport-Security. Precaricamento non fa parte del [specifica RFC HSTS](https://tools.ietf.org/html/rfc6797), ma è supportato dal web browser per precaricare siti HSTS nella nuova installazione. Per altre informazioni, vedere [https://hstspreload.org/](https://hstspreload.org/).
* Abilita [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), cui viene applicato il criterio HSTS per ospitare sottodomini. 
* Imposta in modo esplicito il parametro di validità massima dell'intestazione Strict-Transport-Security per 60 giorni. Se non impostato, il valore predefinito è 30 giorni. Vedere le [direttiva max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) per altre informazioni.
* Aggiunge `example.com` all'elenco degli host da escludere.

`UseHsts` esclude gli host di loopback seguenti:

* `localhost` : L'indirizzo di loopback IPv4.
* `127.0.0.1` : L'indirizzo di loopback IPv4.
* `[::1]` : L'indirizzo di loopback IPv6.

Nell'esempio precedente viene illustrato come aggiungere altri host.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a>Rifiutare esplicitamente di HTTPS nella creazione del progetto

I modelli di applicazione ASP.NET Core web 2.1 o versioni successive (da Visual Studio o la riga di comando di dotnet) abilitano [il reindirizzamento HTTPS](#require) e [HSTS](#hsts). Per le distribuzioni che non richiedono HTTPS, è possibile rifiutare esplicitamente di HTTPS. Ad esempio, alcuni servizi di back-end in cui HTTPS viene gestito esternamente sulla rete perimetrale, tramite HTTPS in ogni nodo non è necessaria.

Per rifiutare esplicitamente di HTTPS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Deselezionare i **Configura per HTTPS** casella di controllo.

![Diagramma dell'entità](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli) 

Usare l'opzione `--no-https`. Esempio:

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a>Come configurare un certificato dello sviluppatore per Docker

Visualizzare [questo problema su GitHub](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end

## <a name="additional-information"></a>Informazioni aggiuntive

* [Supporto browser di OWASP HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
