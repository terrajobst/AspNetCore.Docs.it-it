---
title: Middleware Riscrittura URL in ASP.NET Core
author: guardrex
description: Informazioni sulla riscrittura e il reindirizzamento di URL con il middleware Riscrittura URL nelle applicazioni ASP.NET Core.
ms.author: riande
ms.date: 08/17/2017
uid: fundamentals/url-rewriting
ms.openlocfilehash: d9f33f34f75fe7bf534146c5a426335e74635018
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49326069"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a>Middleware Riscrittura URL in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex) e [Mikael Mengistu](https://github.com/mikaelm12)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

La riscrittura URL è l'azione di modificare gli URL di richiesta in base a una o più regole predefinite. Il processo di riscrittura URL crea un'astrazione tra i percorsi delle risorse e i relativi indirizzi, in modo che i percorsi e gli indirizzi non risultino strettamente collegati. La riscrittura URL risulta utile in diversi scenari:

* Spostamento o sostituzione temporanea o permanente di risorse server pur mantenendo localizzatori stabili di queste risorse.
* Suddivisione dell'elaborazione delle richieste tra diverse app o tra diverse aree di un'unica app.
* Rimozione, aggiunta o riorganizzazione di segmenti URL nelle richieste in ingresso.
* Ottimizzazione degli URL pubblici per l'ottimizzazione motore di ricerca (SEO, Search Engine Optimization).
* Autorizzazione dell'uso di URL pubblici brevi per consentire agli utenti di prevedere il contenuto trovato seguendo un collegamento.
* Reindirizzamento delle richieste non protette a endpoint protetti.
* Blocco dell'hotlinking di immagini.

È possibile definire regole per la modifica dell'URL con diverse modalità, tra cui Regex, le regole di modulo Apache mod_rewrite, le regole di modulo IIS Rewrite e l'uso di logica delle regole personalizzata. Questo documento presenta la riscrittura URL con istruzioni per l'uso del middleware Riscrittura URL nelle applicazioni ASP.NET Core.

> [!NOTE]
> La riscrittura URL può ridurre le prestazioni di un'app. Ove possibile, limitare il numero e la complessità delle regole.

## <a name="url-redirect-and-url-rewrite"></a>Reindirizzamento URL e riscrittura URL

La differenza tra i termini *reindirizzamento URL* e *riscrittura URL* può apparire minima, ma ha implicazioni importanti nell'offerta di risorse ai clienti. Il middleware Riscrittura URL di ASP.NET Core è in grado di svolgere entrambe le funzioni.

Un *reindirizzamento URL* è un'operazione lato client, in cui viene richiesto al client di accedere a una risorsa a un altro indirizzo. Questa operazione richiede una sequenza di andata e ritorno al server. L'URL di reindirizzamento restituito al client viene visualizzato nella barra degli indirizzi del browser quando il client effettua una nuova richiesta per la risorsa. 

Se `/resource` viene *reindirizzato* a `/different-resource` il client richiede `/resource`. Il server risponde che il client deve ottenere la risorsa in `/different-resource` con un codice di stato che indica se il reindirizzamento è temporaneo o permanente. Il client esegue una nuova richiesta per la risorsa all'URL di reindirizzamento.

![Un endpoint di servizio WebAPI è stato modificato temporaneamente dalla versione 1 (v1) alla versione 2 (v2) nel server. Un client esegue una richiesta al servizio nel percorso della versione 1, ovvero /v1/api. Il server restituisce una risposta 302 (Trovato) con il nuovo percorso temporaneo del servizio versione 2, ovvero /v2/api. Il client esegue una seconda richiesta al servizio all'URL di reindirizzamento. Il server risponde con un codice di stato 200 (OK).](url-rewriting/_static/url_redirect.png)

Quando si reindirizzano le richieste a un URL diverso, indicare se il reindirizzamento è temporaneo o permanente. Il codice di stato 301 (Spostato permanentemente) viene usato se la risorsa ha un nuovo URL definitivo e si vuole indicare al client che tale URL dovrà essere usato per tutte le richieste future della risorsa. *Quando riceve un codice di stato 301 il client può memorizzare la risposta nella cache.* Il codice di stato 302 (Trovato) viene usato quando il reindirizzamento è temporaneo o comunque soggetto a modifiche, in modo che il client non dovrà archiviare e riusare l'URL di reindirizzamento in futuro. Per altre informazioni, vedere [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (RFC 2616: Definizioni dei codici di stato).

Una *riscrittura URL* è un'operazione lato server che rende disponibile una risorsa da un indirizzo risorsa alternativo. La riscrittura URL non richiede una sequenza di andata e ritorno al server. L'URL riscritto non viene restituito al client e non viene visualizzato nella barra degli indirizzi del browser. Quando `/resource` viene *riscritto* in `/different-resource`, il client richiede `/resource` e il server recupera *internamente* la risorsa in `/different-resource`. Anche se il client fosse in grado di recuperare la risorsa nell'URL riscritto, non verrà informato dell'esistenza della risorsa nell'URL riscritto quando effettua la richiesta e riceve la risposta.

![Un endpoint servizio WebAPI è stato modificato dalla versione 1 (v1) alla versione 2 (v2) sul server. Un client esegue una richiesta al servizio nel percorso della versione 1, ovvero /v1/api. L'URL della richiesta viene riscritto per accedere al servizio nel percorso della versione 2, ovvero /v2/api. Il servizio risponde al client con un codice di stato 200 (OK).](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>App di esempio per la riscrittura URL

È possibile esplorare le funzionalità del middleware Riscrittura URL con l'[app di esempio di riscrittura URL](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/). L'app esegue le regole di riscrittura e reindirizzamento e visualizza l'URL riscritto o reindirizzato.

## <a name="when-to-use-url-rewriting-middleware"></a>Quando usare il middleware Riscrittura URL

Usare il middleware Riscrittura URL quando non è possibile usare il modulo [URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite) con IIS in Windows Server, il modulo [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/) in Apache Server, la [riscrittura URL in Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/) o quando l'app è ospitata nel [server HTTP.sys](xref:fundamentals/servers/httpsys) (chiamato in precedenza [WebListener](xref:fundamentals/servers/weblistener)). I motivi principali per l'uso delle tecnologie di riscrittura URL basate su server in IIS, Apache o Nginx sono il fatto che il middleware non supporta tutte le funzionalità di questi moduli e il fatto che le prestazioni del middleware possono risultare inferiori a quelle dei moduli. Tuttavia alcune funzionalità dei moduli server non funzionano con i progetti ASP.NET Core, ad esempio i vincoli `IsFile` e `IsDirectory` del modulo IIS Rewrite. In questi scenari è necessario usare il middleware.

## <a name="package"></a>Pacchetto

Per includere il middleware nel progetto, aggiungere un riferimento al pacchetto [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/). Questa funzionalità è disponibile per le app destinate ad ASP.NET Core 1.1 o versione successiva.

## <a name="extension-and-options"></a>Estensioni e opzioni

È possibile definire regole di riscrittura e reindirizzamento URL personalizzate creando un'istanza della classe [RewriteOptions](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptions) con metodi di estensione per le singole regole. Concatenare più regole nell'ordine in cui si vuole che vengano elaborate. Le opzioni `RewriteOptions` vengono passare al middleware Riscrittura URL quando questo viene aggiunto alla pipeline della richiesta con `app.UseRewriter(options);`.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1")
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true)
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")
        .Add(RedirectXMLRequests)
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="redirect-non-www-to-www"></a>Reindirizzare non-www su www

Sono disponibili tre opzioni che consentono all'app di reindirizzare richieste non-`www` su `www`:

* [AddRedirectToWwwPermanent(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowwwpermanent) &ndash; Reindirizza in modo permanente la richiesta al sottodominio `www` se la richiesta è non-`www`. Reindirizza con un codice di stato [Status308PermanentRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status308permanentredirect).
* [AddRedirectToWww(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; Reindirizza la richiesta al sottodominio `www` se la richiesta in ingresso è non-`www`. Reindirizza con un codice di stato [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect).
* [AddRedirectToWww(RewriteOptions, Int32)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; Reindirizza la richiesta al sottodominio `www` se la richiesta in ingresso è non-`www`. Consente di specificare il codice di stato per la risposta. Usare i campi della classe [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) per le assegnazioni ad `AddRedirectToWww`.

::: moniker-end

### <a name="url-redirect"></a>Renidirizzamento URL

Per reindirizzare le richieste, usare `AddRedirect`. Il primo parametro contiene l'espressione regolare per la corrispondenza in base al percorso dell'URL in ingresso. Il secondo parametro è la stringa di sostituzione. Il terzo parametro, se presente, specifica il codice di stato. Se il codice di stato non è specificato, assume il valore 302 (Trovato), che indica che la risorsa è stata temporaneamente spostata o sostituita.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

::: moniker-end

In un browser con gli strumenti di sviluppo abilitati, creare una richiesta all'app di esempio con il percorso `/redirect-rule/1234/5678`. L'espressione regolare definisce la corrispondenza con il percorso di richiesta in `redirect-rule/(.*)` e il percorso viene sostituito con `/redirected/1234/5678`. L'URL di reindirizzamento viene restituito al client con il codice di stato 302 (Trovato). Il browser invia una nuova richiesta all'URL di reindirizzamento, che viene visualizzata nella barra degli indirizzi del browser. Dato che nessuna regola dell'app di esempio presenta una corrispondenza con l'URL di reindirizzamento, la seconda richiesta riceve una risposta 200 (OK) dall'app e il corpo della risposta visualizza l'URL di reindirizzamento. Quando un URL viene *reindirizzato* viene eseguita una sequenza di andata e ritorno al server.

> [!WARNING]
> Prestare attenzione quando si definiscono le regole di reindirizzamento. Le regole di reindirizzamento vengono valutate in ogni richiesta all'app, anche dopo un reindirizzamento. È facile creare inavvertitamente un ciclo di reindirizzamento infinito.

Richiesta originale: `/redirect-rule/1234/5678`

![Finestra del browser con gli strumenti di sviluppo per il rilevamento di richieste e risposte](url-rewriting/_static/add_redirect.png)

La parte dell'espressione racchiusa tra parentesi è denominata *gruppo Capture*. Il punto (`.`) dell'espressione significa *corrispondenza con qualsiasi carattere*. L'asterisco (`*`) indica *corrispondenza con il carattere precedente per zero o più volte*. Di conseguenza gli ultimi due i segmenti di percorso dell'URL `1234/5678` vengono acquisiti tramite il gruppo Capture `(.*)`. Qualsiasi valore visualizzato nell'URL della richiesta dopo `redirect-rule/` viene acquisito da questo gruppo Capture.

Nella stringa di sostituzione i gruppi acquisiti vengono inseriti nella stringa con il simbolo di dollaro (`$`) seguito dal numero di sequenza dell'acquisizione. Il primo valore del gruppo Capture viene ottenuto con `$1`, il secondo con `$2` e la procedura continua in sequenza per i gruppi Capture presenti nell'espressione regolare. Nell'espressione regolare della regola di reindirizzamento dell'app di esempio è presente un solo gruppo acquisito, pertanto nella stringa di sostituzione è presente un solo gruppo inserito, ovvero `$1`. Quando viene applicata la regola, l'URL diventa `/redirected/1234/5678`.

### <a name="url-redirect-to-a-secure-endpoint"></a>Reindirizzamento URL a un endpoint protetto

Usare `AddRedirectToHttps` per reindirizzare le richieste HTTP allo stesso host e allo stesso percorso mediante il protocollo HTTPS (`https://`). Se il codice di stato non viene specificato, il middleware imposta il valore predefinito 302 (Trovato). Se la porta non viene specificata, il middleware imposta il valore predefinito `null`, che indica che il protocollo diventa `https://` e il client accede alla risorsa sulla porta 443. L'esempio indica come impostare il codice di stato su 301 (Spostato permanentemente) e come cambiare il numero di porta in 5001.

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

Usare `AddRedirectToHttpsPermanent` per reindirizzare le richieste non protette allo stesso host e allo stesso percorso con il protocollo di protezione HTTPS (`https://` sulla porta 443). Il middleware imposta il codice stato su 301 (Spostato permanentemente).

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> Per il reindirizzamento a HTTPS quando non sono richieste regole di reindirizzamento aggiuntive, si consiglia di usare il middleware di reindirizzamento HTTPS. Per altre informazioni, vedere l'argomento [Imporre HTTPS](xref:security/enforcing-ssl#require-https).

L'app di esempio indica come usare `AddRedirectToHttps` o `AddRedirectToHttpsPermanent`. Aggiungere il metodo di estensione a `RewriteOptions`. Eseguire una richiesta non sicura all'app su qualsiasi URL. Ignorare l'avviso di sicurezza del browser indicante che il certificato autofirmato non è attendibile oppure creare un'eccezione per rendere affidabile il certificato.

Richiesta originale con `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`

![Finestra del browser con gli strumenti di sviluppo per il rilevamento di richieste e risposte](url-rewriting/_static/add_redirect_to_https.png)

Richiesta originale con `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`

![Finestra del browser con gli strumenti di sviluppo per il rilevamento di richieste e risposte](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>Riscrittura URL

Usare `AddRewrite` per creare una regola di riscrittura URL. Il primo parametro contiene l'espressione regolare per la corrispondenza in base al percorso dell'URL in ingresso. Il secondo parametro è la stringa di sostituzione. Il terzo parametro, `skipRemainingRules: {true|false}`, indica al middleware se ignorare le regole di riscrittura aggiuntive quando viene applicata la regola corrente.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

::: moniker-end

Richiesta originale: `/rewrite-rule/1234/5678`

![Finestra del browser con gli strumenti di sviluppo per il rilevamento di richiesta e risposta](url-rewriting/_static/add_rewrite.png)

Il primo elemento che si nota nell'espressione regolare è l'accento circonflesso (`^`) all'inizio dell'espressione. Questo carattere indica che la corrispondenza inizia all'inizio del percorso dell'URL.

Nell'esempio precedente con la regola di reindirizzamento, `redirect-rule/(.*)`, non è presente un accento circonflesso all'inizio dell'espressione regolare, pertanto nel percorso può essere presente qualsiasi carattere prima di `redirect-rule/` e la corrispondenza viene comunque rilevata.

| Path                               | Corrispondenza con |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | Yes   |
| `/my-cool-redirect-rule/1234/5678` | Yes   |
| `/anotherredirect-rule/1234/5678`  | Yes   |

La regola di riscrittura, `^rewrite-rule/(\d+)/(\d+)`, rileva la corrispondenza dei percorsi solo se iniziano con `rewrite-rule/`. Osservare la differenza nella corrispondenza tra la regola di riscrittura in basso e la regola di reindirizzamento in alto.

| Path                              | Corrispondenza con |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | Yes   |
| `/my-cool-rewrite-rule/1234/5678` | No    |
| `/anotherrewrite-rule/1234/5678`  | No    |

Dopo la parte `^rewrite-rule/` dell'espressione sono presenti due gruppi Capture, `(\d+)/(\d+)`. `\d` indica *corrispondenza con una cifra (numero)*. Il segno più (`+`) indica *corrispondenza con uno o più dei caratteri precedenti*. Di conseguenza l'URL deve contenere un numero seguito da una barra seguita a sua volta da un altro numero. Questi gruppi Capture vengono inseriti nell'URL riscritto come `$1` e `$2`. La stringa di sostituzione della regola di riscrittura inserisce i gruppi acquisiti nella stringa di query. Il percorso richiesto `/rewrite-rule/1234/5678` viene riscritto per ottenere la risorsa in `/rewritten?var1=1234&var2=5678`. Se nella richiesta originale è presente una stringa di query, la stringa viene mantenuta quando l'URL viene riscritto.

Non viene eseguita nessuna sequenza di andata e ritorno al server per ottenere la risorsa. Se la risorsa esiste viene recuperata e restituita al client con il codice stato 200 (OK). Poiché il client non è reindirizzato, l'URL nella barra degli indirizzi del browser non cambia. Dal punto di vista del client, l'operazione di riscrittura URL non si è verificata.

> [!NOTE]
> Se possibile, usare sempre `skipRemainingRules: true`, perché la corrispondenza tra le regole è un processo dispendioso che rallenta il tempo di risposta dell'app. Per velocizzare il tempo di risposta dell'app:
> * Ordinare le regole di riscrittura dalla regola che origina più corrispondenze a quella che origina meno corrispondenze.
> * Quando viene rilevata una corrispondenza e non sono necessarie altre operazioni di elaborazione delle regole, ignorare l'elaborazione delle regole rimanenti.

### <a name="apache-modrewrite"></a>Modulo Apache mod_rewrite

Applicare le regole del modulo Apache mod_rewrite con `AddApacheModRewrite`. Assicurarsi che il file delle regole venga distribuito con l'app. Per altre informazioni ed esempi di regole mod_rewrite, vedere [Modulo Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).

::: moniker range=">= aspnetcore-2.0"

`StreamReader` viene usato per leggere le regole del file di regole *ApacheModRewrite.txt*.

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Il primo parametro accetta `IFileProvider`, che viene reso disponibile tramite [Dependency Injection](dependency-injection.md) (Inserimento di dipendenze). `IHostingEnvironment` viene inserito per specificare `ContentRootFileProvider`. Il secondo parametro è il percorso del file di regole, ovvero *ApacheModRewrite.txt* nell'app di esempio.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

::: moniker-end

L'app di esempio reindirizza le richieste da `/apache-mod-rules-redirect/(.\*)` a `/redirected?id=$1`. Il codice di stato della risposta è 302 (Trovato).

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

Richiesta originale: `/apache-mod-rules-redirect/1234`

![Finestra del browser con gli strumenti di sviluppo per il rilevamento di richieste e risposte](url-rewriting/_static/add_apache_mod_redirect.png)

Il middleware supporta le seguenti variabili server del modulo Apache mod_rewrite:

* CONN_REMOTE_ADDR
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_FORWARDED
* HTTP_HOST
* HTTP_REFERER
* HTTP_USER_AGENT
* HTTPS
* IPV6
* QUERY_STRING
* REMOTE_ADDR
* REMOTE_PORT
* REQUEST_FILENAME
* REQUEST_METHOD
* REQUEST_SCHEME
* REQUEST_URI
* SCRIPT_FILENAME
* SERVER_ADDR
* SERVER_PORT
* SERVER_PROTOCOL
* TIME
* TIME_DAY
* TIME_HOUR
* TIME_MIN
* TIME_MON
* TIME_SEC
* TIME_WDAY
* TIME_YEAR

### <a name="iis-url-rewrite-module-rules"></a>Regole di IIS URL Rewrite Module

Per usare regole valide per IIS URL Rewrite Module, usare `AddIISUrlRewrite`. Assicurarsi che il file delle regole venga distribuito con l'app. Non indirizzare il middleware all'uso del file *web.config* personalizzato quando è in esecuzione in Windows Server IIS. Con IIS queste regole devono essere archiviate fuori dal file *web.config*, per evitare conflitti con il modulo IIS Rewrite. Per altre informazioni ed esempi di regole di IIS URL Rewrite Module, vedere [Using URL Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) (Uso di URL Rewrite Module 2.0) e [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference) (Guida di riferimento per la configurazione di URL Rewrite Module).

::: moniker range=">= aspnetcore-2.0"

Un elemento `StreamReader` viene usato per leggere le regole del file di regole *IISUrlRewrite.xml*.

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Il primo parametro accetta `IFileProvider` e il secondo parametro è il percorso del file di regole XML, ovvero *IISUrlRewrite.xml* nell'app di esempio.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

::: moniker-end

L'app di esempio riscrive le richieste da `/iis-rules-rewrite/(.*)` a `/rewritten?id=$1`. La risposta viene inviata al client con un codice di stato 200 (OK).

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

Richiesta originale: `/iis-rules-rewrite/1234`

![Finestra del browser con gli strumenti di sviluppo per il rilevamento di richiesta e risposta](url-rewriting/_static/add_iis_url_rewrite.png)

Se è presente un modulo IIS Rewrite attivo con regole di livello server configurate che possono avere effetti indesiderati nell'app, è possibile disabilitare il modulo IIS Rewrite per un'app. Per altre informazioni, vedere [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules) (Disabilitazione di moduli IIS).

#### <a name="unsupported-features"></a>Funzionalità non supportate

::: moniker range=">= aspnetcore-2.0"

Il middleware rilasciato con ASP.NET Core 2.x non supporta le seguenti funzionalità di IIS URL Rewrite Module:

* Regole in uscita
* Variabili server personalizzate
* Caratteri jolly
* LogRewrittenUrl

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Il middleware rilasciato con ASP.NET Core 1.x non supporta le seguenti funzionalità di IIS URL Rewrite Module:

* Regole globali
* Regole in uscita
* Mapping di riscrittura
* Azione CustomResponse
* Variabili server personalizzate
* Caratteri jolly
* Action:CustomResponse
* LogRewrittenUrl

::: moniker-end

#### <a name="supported-server-variables"></a>Variabili server supportate

Il middleware supporta le seguenti variabili server di IIS URL Rewrite Module:

* CONTENT_LENGTH
* CONTENT_TYPE
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_HOST
* HTTP_REFERER
* HTTP_URL
* HTTP_USER_AGENT
* HTTPS
* LOCAL_ADDR
* QUERY_STRING
* REMOTE_ADDR
* REMOTE_PORT
* REQUEST_FILENAME
* REQUEST_URI

> [!NOTE]
> È anche possibile ottenere un `IFileProvider` tramite un `PhysicalFileProvider`. Questo approccio può garantire maggior flessibilità per la posizione dei file delle regole di riscrittura. Assicurarsi che i file di regole di riscrittura vengano distribuiti nel server in corrispondenza del percorso specificato.
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>Regola basata su metodo

Usare `Add(Action<RewriteContext> applyRule)` per implementare logica della regola personalizzata in un metodo. `RewriteContext` espone `HttpContext` per l'uso nel metodo. `context.Result` determina la modalità di gestione dell'elaborazione pipeline aggiuntiva.

| context.Result                       | Operazione                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| `RuleResult.ContinueRules` (impostazione predefinita) | Continuare ad applicare le regole                                         |
| `RuleResult.EndResponse`             | Interrompere l'applicazione delle regole e inviare la risposta                       |
| `RuleResult.SkipRemainingRules`      | Interrompere l'applicazione delle regole e inviare il contesto al middleware seguente |

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

::: moniker-end

L'app di esempio visualizza un metodo che reindirizza le richieste per i percorsi che terminano con *.xml*. Se si esegue una richiesta per `/file.xml`, questa viene reindirizzata a `/xmlfiles/file.xml`. Il codice di stato è 301 (Spostato permanentemente). Per un reindirizzamento è necessario impostare in modo esplicito il codice di stato della risposta; in caso contrario viene restituito il codice di stato 200 (OK) e il reindirizzamento non viene eseguito sul client.

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

Richiesta originale: `/file.xml`

![Finestra del browser con strumenti di sviluppo per il rilevamento di richieste e risposte per file.xml](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a>Regola basata su IRule

Usare `Add(IRule)` per implementare logica della regola personalizzata in una classe derivata da `IRule`. L'uso di un elemento `IRule` garantisce maggiore flessibilità rispetto all'approccio con una regola basata su un metodo. La classe derivata può includere un costruttore, in cui è possibile passare parametri per il metodo `ApplyRule`.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

::: moniker-end

I valori dei parametri nell'app di esempio per `extension` e `newPath` vengono verificati e devono soddisfare diverse condizioni. `extension` deve contenere un valore e tale valore deve essere *.png*, *.jpg* o *.gif*. Se `newPath` non è valido, viene generato un evento `ArgumentException`. Se si esegue una richiesta per *image.png*, questa viene reindirizzata a `/png-images/image.png`. Se si esegue una richiesta per *image.jpg*, questa viene reindirizzata a `/jpg-images/image.jpg`. Il codice di stato viene impostato su 301 (Spostato permanentemente) e l'elemento `context.Result` viene impostato per l'interruzione dell'elaborazione delle regole e l'invio della risposta.

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

Richiesta originale: `/image.png`

![Finestra del browser con gli strumenti di sviluppo per il rilevamento di richieste e risposte per image.png](url-rewriting/_static/add_redirect_png_requests.png)

Richiesta originale: `/image.jpg`

![Finestra del browser con gli strumenti di sviluppo per il rilevamento di richieste e risposte per image.jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>Esempi di espressioni regolari

| Obiettivo | Esempio di stringa espressione regolare<br>e corrispondenza | Esempio di stringa di sostituzione<br>e output |
| ---- | :-----------------------------: | :------------------------------------: |
| Riscrivere il percorso nella stringa di query | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| Rimuovere la barra finale | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| Applicare la barra finale | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| Evitare la riscrittura di richieste specifiche | `^(.*)(?<!\.axd)$` o `^(?!.*\.axd$)(.*)$`<br>Sì: `/resource.htm`<br>No: `/resource.axd` | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| Ridisporre i segmenti di URL | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| Sostituire un segmento di URL | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a>Risorse aggiuntive

* [Avvio dell'applicazione](startup.md)
* [Middleware](xref:fundamentals/middleware/index)
* [Espressioni regolari in .NET](/dotnet/articles/standard/base-types/regular-expressions)
* [Linguaggio di espressioni regolari - Riferimento rapido](/dotnet/articles/standard/base-types/quick-ref)
* [Modulo Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)
* [Using Url Rewrite Module 2.0 (for IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) (Uso di URL Rewrite Module 2.0 per IIS)
* [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference) (Guida di riferimento per la configurazione di URL Rewrite Module)
* [Forum di IIS URL Rewrite Module](https://forums.iis.net/1152.aspx)
* [Keep a simple URL structure](https://support.google.com/webmasters/answer/76329?hl=en) (Mantenere una struttura URL semplice)
* [10 URL Rewriting Tips and Tricks](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/) (10 suggerimenti e consigli per la riscrittura URL)
* [To slash or not to slash](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html) (Uso del carattere barra)
