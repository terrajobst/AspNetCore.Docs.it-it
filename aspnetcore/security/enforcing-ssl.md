---
title: Applicare HTTPS in una base di ASP.NET
author: rick-anderson
description: Viene illustrato come richiedere HTTPS/TLS in un Core di ASP.NET web app.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: edc69443455677ba80ebb0a73e193d4d6741e470
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/12/2018
---
# <a name="enforce-https-in-an-aspnet-core"></a>Applicare HTTPS in una base di ASP.NET

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questo documento illustra come:

- Utilizzare HTTPS per tutte le richieste.
- Reindirizzare tutte le richieste HTTP a HTTPS.

> [!WARNING]
> Eseguire **non** utilizzare `RequireHttpsAttribute` sulle API Web che ricevono informazioni riservate. `RequireHttpsAttribute` Usa i codici di stato HTTP per il reindirizzamento dei browser da HTTP a HTTPS. Client dell'API non possono comprendere o rispettare i reindirizzamenti da HTTP a HTTPS. Tali client possono inviare informazioni su HTTP. API Web dovrebbero:
>
>* È in ascolto su HTTP.
>* Chiudere la connessione con il codice di stato 400 (Bad Request) e non rispondere alla richiesta.

<a name="require"></a>
## <a name="require-https"></a>Utilizzo di HTTPS

::: moniker range=">= aspnetcore-2.1"
È consigliabile tutti ASP.NET Core chiamata di App web `UseHttpsRedirection` per reindirizzare tutte le richieste HTTP a HTTPS. Se `UseHsts` viene chiamato nell'app, deve essere chiamato prima `UseHttpsRedirection`.

Il codice seguente chiama `UseHttpsRedirection` nella `Startup` classe:

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]


Il codice seguente:

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

* Set `RedirectStatusCode`.
* Imposta la porta HTTPS 5001.

::: moniker-end


::: moniker range="< aspnetcore-2.1"

Il [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) viene utilizzato per richiedere HTTPS. `[RequireHttpsAttribute]` può decorare controller o i metodi, oppure può essere applicata a livello globale. Per applicare l'attributo a livello globale, aggiungere il seguente codice al `ConfigureServices` in `Startup`:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Il codice evidenziato precedente richiede l'utilizzano di tutte le richieste `HTTPS`; pertanto, le richieste HTTP vengono ignorate. Il seguente codice evidenziato reindirizza tutte le richieste HTTP a HTTPS:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Per ulteriori informazioni, vedere [Middleware di riscrittura URL](xref:fundamentals/url-rewriting).

Che richiedono HTTPS a livello globale (`options.Filters.Add(new RequireHttpsAttribute());`) è una procedura consigliata. L'applicazione di `[RequireHttps]` attributo a tutte le pagine controller/Razor non è considerata sicura come che richiedono HTTPS a livello globale. Non è possibile garantire il `[RequireHttps]` attributo viene applicato quando vengono aggiunti nuovi controller e le pagine Razor.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a>Protocollo di sicurezza trasporto Strict HTTP (HSTS)

Per ogni [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict trasporto sicurezza (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) è un miglioramento della sicurezza opt-in specificato da un'applicazione web tramite l'utilizzo di un'intestazione di risposta speciali. Una volta un browser supportato riceve questa intestazione tale browser impedirà tutte le comunicazioni da inviati tramite HTTP per il dominio specificato e verrà invece inviato tutte le comunicazioni tramite HTTPS. Impedisce inoltre fare clic su HTTPS tramite richieste su browser.

ASP.NET Core 2.1 o versioni successive implementa HSTS con il `UseHsts` metodo di estensione. Il codice seguente chiama `UseHsts` quando l'app non si trova in [modalità di sviluppo](xref:fundamentals/environments):

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` non è consigliare nello sviluppo perché l'intestazione HSTS è altamente memorizzabile nella cache dal browser. Per impostazione predefinita, UseHsts esclude l'indirizzo di loopback locale.

Il codice seguente:

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Imposta il parametro precaricamento dell'intestazione di sicurezza di trasporto Strict. Precaricamento non è in parte il [specifica RFC HSTS](https://tools.ietf.org/html/rfc6797), ma è supportato dal web browser per precaricare siti HSTS nella nuova installazione. Per altre informazioni, vedere [https://hstspreload.org/](https://hstspreload.org/).
* Abilita [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), che applica i criteri HSTS sottodomini Host. 
* Imposta in modo esplicito il parametro max-age dell'intestazione Strict-trasporto-Security per 60 giorni. Se non impostato, i valori predefiniti per 30 giorni. Vedere la [direttiva max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) per altre informazioni.
* Aggiunge `example.com` all'elenco degli host da escludere.

`UseHsts` esclude gli host di loopback seguenti:

* `localhost` : L'indirizzo di loopback IPv4.
* `127.0.0.1` : L'indirizzo di loopback IPv4.
* `[::1]` : L'indirizzo di loopback IPv6.

Nell'esempio precedente viene illustrato come aggiungere altri host.
::: moniker-end


::: moniker range=">= aspnetcore-2.1"
<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a>Rifiuto HTTPS della creazione del progetto

Le ASP.NET Core 2.1 e versioni successive modelli di applicazione web (in Visual Studio o la riga di comando dotnet) abilitano [il reindirizzamento HTTPS](#require) e [HSTS](#hsts). Per le distribuzioni che non richiedono HTTPS, è possibile rifiutare esplicitamente di HTTPS. Ad esempio, alcuni servizi back-end in HTTPS viene gestita esternamente alla periferia, utilizzando il protocollo HTTPS in corrispondenza di ogni nodo non è necessaria.

Per rifiutare esplicitamente di HTTPS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Deselezionare la **Configura per HTTPS** casella di controllo.

![Diagramma dell'entità](enforcing-ssl/_static/out.png)


#   <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli) 

Usare l'opzione `--no-https`. Esempio:

```cli
dotnet new razor --no-https
```

------

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
## <a name="how-to-setup-a-developer-certificate-for-docker"></a>Come configurare un certificato dello sviluppatore per Docker

Vedere [questo problema GitHub](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end
