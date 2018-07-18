---
title: Imporre HTTPS in ASP.NET Core
author: rick-anderson
description: Illustra come richiedere HTTPS/TLS in un ASP.NET Core web app.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 331c17de33b5c13221385ffb4282bc16bde32289
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095717"
---
# <a name="enforce-https-in-aspnet-core"></a>Imporre HTTPS in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questo documento illustra come:

* La richiesta di HTTPS per tutte le richieste.
* Reindirizzare tutte le richieste HTTP a HTTPS.

> [!WARNING]
> Effettuare **non** utilizzare [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) nelle API Web che ricevono le informazioni riservate. `RequireHttpsAttribute` Usa i codici di stato HTTP per reindirizzare i browser da HTTP a HTTPS. I client dell'API non possono comprendere o rispettare effettua il reindirizzamento da HTTP a HTTPS. Tali client possono inviare le informazioni su HTTP. Le API Web devono essere:
>
> * È in ascolto su HTTP.
> * Chiudere la connessione con il codice di stato 400 (Bad Request) e non rispondere alla richiesta.

<a name="require"></a>
## <a name="require-https"></a>La richiesta di HTTPS

::: moniker range=">= aspnetcore-2.1"

Si consiglia di tutte le app web ASP.NET Core chiamerà il Middleware di reindirizzamento HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) per reindirizzare tutte le richieste HTTP a HTTPS.

Il codice seguente chiama `UseHttpsRedirection` nella `Startup` classe:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

Il codice seguente chiama [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) per configurare le opzioni del middleware:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

Il codice evidenziato sopra:

* Set [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) a `Status307TemporaryRedirect`, ovvero il valore predefinito. Le app di produzione devono chiamare [UseHsts](#hsts).
* Nastavuje port HTTPS in 5001. Il valore predefinito è 443.

I meccanismi seguenti impostate automaticamente la porta:

* Il middleware può individuare le porte tramite [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) quando vengono applicate le condizioni seguenti:
  - Kestrel o HTTP. sys è usate direttamente con gli endpoint HTTPS (si applica anche all'esecuzione dell'app con il debugger di Visual Studio Code).
  - Solo **una sola porta HTTPS** viene usata dall'app.
* Viene usato Visual Studio:
  - IIS Express è abilitato per HTTPS.
  - *launchsettings. JSON* imposta il `sslPort` per IIS Express.

> [!NOTE]
> Quando un'app viene eseguita dietro un proxy inverso (ad esempio, IIS, IIS Express), `IServerAddressesFeature` non è disponibile. La porta deve essere configurata manualmente. Quando la porta non è impostata, le richieste non vengono reindirizzate.

La porta può essere configurata impostando il:

* La variabile di ambiente `ASPNETCORE_HTTPS_PORT`.
* `http_port` chiave di configurazione host (ad esempio, tramite *hostsettings.json* o un argomento della riga di comando).
* [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport). Vedere l'esempio precedente viene illustrato come impostare la porta in 5001.

> [!NOTE]
> La porta può essere configurata indirettamente impostando l'URL con il `ASPNETCORE_URLS` variabile di ambiente. La variabile di ambiente consente di configurare il server e quindi il middleware indirettamente consente di individuare la porta HTTPS tramite `IServerAddressesFeature`.

Se non è impostata alcuna porta:

* Le richieste non vengono reindirizzate.
* Il middleware registra un avviso.

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

Per ogni [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) è un miglioramento della sicurezza con consenso esplicito specificato da un'applicazione web usando una speciale intestazione della risposta. Una volta che un browser supportato riceve questa intestazione browser impedirà tutte le comunicazioni che vengano inviati tramite HTTP per il dominio specificato e invia tutte le comunicazioni tramite HTTPS. Impedisce anche click-through richieste nei browser HTTPS.

ASP.NET Core 2.1 o versioni successive implementa HSTS con il `UseHsts` metodo di estensione. Il codice seguente chiama `UseHsts` quando l'app non è presente nel [modalità di sviluppo](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` non è consigliato in fase di sviluppo poiché l'intestazione HSTS sia altamente memorizzabile nella cache dal browser. Per impostazione predefinita, `UseHsts` esclude l'indirizzo di loopback locale.

Il codice seguente:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Imposta il parametro precaricamento dell'intestazione Strict-Transport-Security. Precaricamento non è in parte i [specifica RFC HSTS](https://tools.ietf.org/html/rfc6797), ma è supportato dal web browser per precaricare siti HSTS nella nuova installazione. Per altre informazioni, vedere [https://hstspreload.org/](https://hstspreload.org/).
* Abilita [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), cui viene applicato il criterio HSTS per ospitare sottodomini. 
* Imposta in modo esplicito il parametro di durata massima dell'intestazione Strict-Transport-Security per 60 giorni. Se non impostato, il valore predefinito è 30 giorni. Vedere le [direttiva max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) per altre informazioni.
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

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a>Come configurare un certificato dello sviluppatore per Docker

Visualizzare [questo problema su GitHub](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end
