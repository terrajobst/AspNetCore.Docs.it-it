---
title: Middleware in ASP.NET Core di riscrittura URL
author: guardrex
description: Introduzione all'URL di riscrittura e reindirizzamento con le istruzioni su come utilizzare il Middleware di riscrittura URL nelle applicazioni ASP.NET Core.
keywords: ASP.NET Core, la riscrittura URL, URL rewrite, URL di reindirizzamento, reindirizzamento dell'URL, middleware, apache_mod
ms.author: riande
manager: wpickett
ms.date: 08/17/2017
ms.topic: article
ms.assetid: e6130638-c410-4161-9921-b658ce988bd1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/url-rewriting
ms.openlocfilehash: 44c78a6304eacc70cdee9bb0d9407376017abcac
ms.sourcegitcommit: 8005eb4051e568d88ee58d48424f39916052e6e2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2017
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a>Middleware in ASP.NET Core di riscrittura URL

Da [Luke Latham](https://github.com/guardrex) e [Mikael Mengistu](https://github.com/mikaelm12)

[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)

La riscrittura URL è l'azione di modifica richiesta URL in base a uno o più regole predefinite. La riscrittura URL crea un'astrazione tra i percorsi delle risorse e i relativi indirizzi in modo che i percorsi e gli indirizzi non sono strettamente collegati. Esistono diversi scenari in cui la riscrittura URL è utile:
* Spostando o sostituendo le risorse del server, temporaneamente o definitivamente, mantenendo i localizzatori stabili per le risorse
* La suddivisione in diverse App o in aree di un'applicazione di elaborazione delle richieste
* La rimozione, l'aggiunta o la riorganizzazione di segmenti di URL delle richieste in ingresso
* L'ottimizzazione di un URL pubblico per Search Engine Optimization (SEO)
* Permette l'utilizzo di un URL pubblico brevi per consentire agli utenti di prevedere il contenuto che individuano seguendo un collegamento
* Reindirizzamento delle richieste non protette per la protezione di endpoint
* Impedendo hotlinking immagine

È possibile definire regole per la modifica dell'URL in diversi modi, tra cui regex, le regole di modulo mod_rewrite Apache, le regole di modulo di IIS di riscrittura e l'utilizzo di logica della regola personalizzata. Questo documento introduce la riscrittura URL con le istruzioni su come utilizzare il Middleware di riscrittura URL nelle applicazioni ASP.NET Core.

> [!NOTE]
> La riscrittura URL può ridurre le prestazioni di un'app. Ove possibile, è necessario limitare il numero e la complessità delle regole.

## <a name="url-redirect-and-url-rewrite"></a>Reindirizzamento dell'URL e l'URL di riscrittura
La differenza nella formulazione tra *reindirizzamento dell'URL* e *URL rewrite* potrebbe sembrare complesso in primo ma ha implicazioni molto importanti per le risorse necessarie per i client. URL di riscrittura Middleware dell'ASP.NET di base è in grado di soddisfare la necessità di entrambi.

Oggetto *reindirizzamento dell'URL* è un'operazione lato client, in cui il client viene richiesto di accedere a una risorsa in un altro indirizzo. Questa operazione richiede un round trip al server e l'URL di reindirizzamento restituito al client viene visualizzata nella barra degli indirizzi del browser quando il client effettua una nuova richiesta per la risorsa. Se `/resource` è *reindirizzato* a `/different-resource`, le richieste del client `/resource`, e il server risponde che il client deve ottenere la risorsa a `/different-resource` con un codice di stato che indica che il reindirizzamento è temporaneo o permanente. Il client esegue una nuova richiesta per la risorsa dell'URL di reindirizzamento.

![Un endpoint del servizio WebAPI è stato modificato temporaneamente dalla versione 1 (v1) alla versione 2 (v2) nel server. Un client effettua una richiesta al servizio in /v1/api di percorso la versione 1. Il server invia una risposta 302 (trovato) con il nuovo percorso temporaneo per il servizio in /v2/api versione 2. Il client esegue una seconda richiesta al servizio all'URL di reindirizzamento. Il server risponde con un codice di stato 200 (OK).](url-rewriting/_static/url_redirect.png)

Quando si reindirizzano le richieste a un URL diverso, indicare se il reindirizzamento è temporaneo o permanente. 301 (spostato permanentemente) codice di stato viene utilizzato in cui la risorsa ha un nuovo URL permanente e si desidera per il client che tutte le richieste successive per la risorsa devono usare il nuovo URL. *Il client può memorizzare nella cache la risposta quando viene ricevuto un codice di 301 stato.* Il codice di stato 302 (trovato) viene utilizzato in cui il reindirizzamento è temporaneo o in genere l'oggetto da modificare, in modo che il client non deve archiviare e riutilizzare l'URL di reindirizzamento in futuro. Per ulteriori informazioni, vedere [RFC 2616: le definizioni dei codici di stato](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

Oggetto *URL rewrite* è un'operazione lato server per fornire una risorsa da un indirizzo di risorse diverso. La riscrittura URL non richiede un round trip al server. L'URL riscritto non viene restituito al client e non sarà più visualizzato nella barra degli indirizzi del browser. Quando `/resource` è *riscritto* a `/different-resource`, le richieste del client `/resource`e il server *internamente* recupera la risorsa a livello `/different-resource`. Anche se il client potrebbe essere in grado di recuperare la risorsa dell'URL riscritto, il client non verrà indicato che la risorsa esista nell'URL riscritto quando si effettua la richiesta e riceve la risposta.

![Un endpoint del servizio WebAPI è stato modificato dalla versione 1 (v1) alla versione 2 (v2) nel server. Un client effettua una richiesta al servizio in /v1/api di percorso la versione 1. L'URL della richiesta viene riscritto per accedere al servizio in /v2/api di percorso la versione 2. Il servizio risponde al client con un codice di stato 200 (OK).](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>App di esempio di riscrittura URL
È possibile esplorare le funzionalità di Middleware di riscrittura URL con il [app di esempio di riscrittura URL](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/). Riscrittura si applica l'app e regole di reindirizzamento e viene visualizzato l'URL riscritto o reindirizzato.

## <a name="when-to-use-url-rewriting-middleware"></a>Quando utilizzare il Middleware di riscrittura URL
Utilizzare il Middleware di riscrittura URL quando non è possibile utilizzare il [modulo URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite) con IIS in Windows Server, il [modulo mod_rewrite Apache](https://httpd.apache.org/docs/2.4/rewrite/) nel Server di Apache, [la riscrittura URL su Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), o l'app è ospitato in [server HTTP.sys](xref:fundamentals/servers/httpsys) (in precedenza denominato [WebListener](xref:fundamentals/servers/weblistener)). Motivi principali per utilizzare il server tecnologie basate su URL riscrittura in IIS, Apache o Nginx sono che il middleware non supporta le funzionalità complete di questi moduli e le prestazioni del middleware probabilmente non corrisponderanno a quello dei moduli. Esistono tuttavia alcune funzionalità dei moduli server che non funzionano con i progetti ASP.NET Core, ad esempio il `IsFile` e `IsDirectory` vincoli del modulo IIS di riscrittura. In questi scenari, utilizzare il middleware.

## <a name="package"></a>Pacchetto
Per includere il middleware nel progetto, aggiungere un riferimento di [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) pacchetto. Questa funzionalità è disponibile per le app destinate a ASP.NET Core 1.1 o versione successiva.

## <a name="extension-and-options"></a>Estensione e le opzioni
Stabilire la riscrittura URL e le regole di reindirizzamento creando un'istanza del `RewriteOptions` classe con metodi di estensione per ognuna delle proprie regole. Concatenare più regole nell'ordine in cui si desidera vengano elaborati. Il `RewriteOptions` vengono passati nel Middleware di riscrittura URL quando viene aggiunto alla pipeline delle richieste con `app.UseRewriter(options);`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1)]

---

### <a name="url-redirect"></a>Reindirizzamento dell'URL
Utilizzare `AddRedirect` per reindirizzare le richieste. Il primo parametro contiene l'espressione regolare per la corrispondenza nel percorso dell'URL in ingresso. Il secondo parametro è la stringa di sostituzione. Il terzo parametro, se presente, specifica il codice di stato. Se non si specifica il codice di stato, per impostazione predefinita in 302 (trovato), che indica che la risorsa è temporaneamente spostata o sostituita.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=2)]

---

In un browser con gli strumenti di sviluppo abilitato, effettuare una richiesta per l'app di esempio con il percorso `/redirect-rule/1234/5678`. L'espressione regolare corrisponde al percorso di richiesta in `redirect-rule/(.*)`, e il percorso viene sostituito con `/redirected/1234/5678`. Il reindirizzamento dell'URL viene inviato al client con codice di stato 302 (trovato). Il browser invia una nuova richiesta all'URL di reindirizzamento, verrà visualizzato nella barra degli indirizzi del browser. Poiché nessuna regola nell'app di esempio corrispondente all'URL di reindirizzamento, la seconda richiesta riceve una risposta di 200 (OK) dall'app e il corpo della risposta viene visualizzato l'URL di reindirizzamento. Un round trip viene effettuato al server quando è un URL *reindirizzato*.

> [!WARNING]
> Prestare attenzione quando si stabiliscono le regole di reindirizzamento. Le regole di reindirizzamento vengono valutate in ogni richiesta per l'app, ad esempio dopo un reindirizzamento. È facile per errore un ciclo di reindirizzamento infinito.

Richiesta originale:`/redirect-rule/1234/5678`

![Finestra del browser con gli strumenti di sviluppo rilevamento le richieste e risposte](url-rewriting/_static/add_redirect.png)

La parte dell'espressione racchiusa tra parentesi viene chiamata un *gruppo capture*. Il punto (`.`) dell'espressione significa *corrisponde a qualsiasi carattere*. L'asterisco (`*`) indica *corrisponde al carattere precedente zero o più volte*. Di conseguenza, i segmenti di percorso ultime due dell'URL, `1234/5678`, vengono acquisite dal gruppo di acquisizione `(.*)`. Qualsiasi valore fornito nell'URL della richiesta dopo `redirect-rule/` viene acquisito da questo gruppo di acquisizione singola.

Nella stringa di sostituzione, i gruppi acquisiti vengono inseriti nella stringa con il segno di dollaro (`$`) seguito dal numero di sequenza dell'acquisizione. Il valore del primo gruppo di acquisizione viene ottenuto con `$1`, il secondo con `$2`, continuando in sequenza per i gruppi di acquisizione all'espressione regolare. È solo un gruppo acquisito in reindirizzamento regola regex nell'app di esempio, viene visualizzato un solo gruppo inserito nella stringa di sostituzione, ovvero `$1`. Quando viene applicata la regola, l'URL diventa `/redirected/1234/5678`.

<a name=url-redirect-to-secure-endpoint></a>
### <a name="url-redirect-to-a-secure-endpoint"></a>Reindirizzamento dell'URL per un endpoint protetto
Utilizzare `AddRedirectToHttps` per reindirizzare le richieste HTTP nello stesso host e lo stesso percorso utilizzando il protocollo HTTPS (`https://`). Se non viene fornito il codice di stato, il middleware predefinito 302 (trovato). Se la porta non è specificata, il middleware per impostazione predefinita `null`, ovvero il protocollo viene modificato in `https://` e il client accede alla risorsa sulla porta 443. Nell'esempio viene illustrato come impostare il codice di stato 301 (spostato permanentemente) e assegnare alla porta 5001.
```csharp
var options = new RewriteOptions()
    .AddRedirectToHttps(301, 5001);

app.UseRewriter(options);
```
Utilizzare `AddRedirectToHttpsPermanent` per reindirizzare le richieste non protette nello stesso host e lo stesso percorso con il protocollo HTTPS sicuro (`https://` sulla porta 443). Il middleware imposta il codice di stato per 301 (spostato permanentemente).

L'app di esempio è in grado di illustrare come utilizzare `AddRedirectToHttps` o `AddRedirectToHttpsPermanent`. Aggiungere il metodo di estensione per il `RewriteOptions`. Per l'app in qualsiasi URL, effettuare una richiesta non protetta. Ignorare l'avviso che non è attendibile il certificato autofirmato di protezione del browser.

Richiesta originale utilizzando `AddRedirectToHttps(301, 5001)`:`/secure`

![Finestra del browser con gli strumenti di sviluppo rilevamento le richieste e risposte](url-rewriting/_static/add_redirect_to_https.png)

Richiesta originale utilizzando `AddRedirectToHttpsPermanent`:`/secure`

![Finestra del browser con gli strumenti di sviluppo rilevamento le richieste e risposte](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>Riscrittura URL
Utilizzare `AddRewrite` per creare una regola per la riscrittura URL. Il primo parametro contiene l'espressione regolare per la corrispondenza sul percorso URL in ingresso. Il secondo parametro è la stringa di sostituzione. Il terzo parametro, `skipRemainingRules: {true|false}`, indica se ignorare le regole di riscrittura aggiuntivo se viene applicata la regola corrente o meno per il middleware.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=6)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=3)]

---

Richiesta originale:`/rewrite-rule/1234/5678`

![Finestra del browser con gli strumenti di sviluppo di rilevamento di richiesta e risposta](url-rewriting/_static/add_rewrite.png)

La prima cosa notare nell'espressione regolare è l'accento circonflesso (`^`) all'inizio dell'espressione. Ciò significa che corrispondenza inizia all'inizio del percorso dell'URL.

Nell'esempio precedente con la regola di reindirizzamento, `redirect-rule/(.*)`, non c'è alcun marcatore all'inizio dell'espressione regolare; pertanto, possono precedere i caratteri `redirect-rule/` in un percorso di una corrispondenza corretta.

| Path                               | Corrispondenza con |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | Sì   |
| `/my-cool-redirect-rule/1234/5678` | Sì   |
| `/anotherredirect-rule/1234/5678`  | Sì   |

La regola di riscrittura `^rewrite-rule/(\d+)/(\d+)`, corrisponde solo percorsi se iniziano con `rewrite-rule/`. Si noti la differenza nella corrispondenza tra la seguente regola di riscrittura e la relativa regola precedente.

| Path                              | Corrispondenza con |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | Sì   |
| `/my-cool-rewrite-rule/1234/5678` | No    |
| `/anotherrewrite-rule/1234/5678`  | No    |

Dopo il `^rewrite-rule/` parte dell'espressione, sono presenti due gruppi di acquisizione, `(\d+)/(\d+)`. Il `\d` indica *corrisponde a una cifra (numero)*. Il segno più (`+`) significa *corrisponde a uno o più il carattere precedente*. Pertanto, l'URL deve contenere un numero seguito da una barra seguita da un altro numero. Questi gruppi vengono inseriti nell'URL riscritto come di acquisizione `$1` e `$2`. La stringa di sostituzione regola di riscrittura inserisce i gruppi acquisiti nella stringa di query. Il percorso della richiesto `/rewrite-rule/1234/5678` viene riscritto per ottenere la risorsa a livello `/rewritten?var1=1234&var2=5678`. Se una stringa di query è presente nella richiesta originale, questi vengono mantenuti quando l'URL è stato riscritto.

Non vi è alcun round trip al server per ottenere la risorsa. Se la risorsa esiste, dispone di recuperata e restituita al client con un codice di 200 stato (OK). Poiché il client non è stato reindirizzato, non modifica l'URL nella barra degli indirizzi del browser. Per quanto riguarda il client, l'operazione di riscrittura URL non si è verificato.

> [!NOTE]
> Utilizzare `skipRemainingRules: true` laddove possibile, perché le regole di corrispondenza sono un processo dispendioso e riduce il tempo di risposta dell'applicazione. Per la risposta più veloce di app:
> * Ordinamento delle regole di riscrittura dalla regola di corrispondenza più di frequente per la regola corrispondente meno frequentemente.
> * Quando si verifica una corrispondenza ed non è necessaria alcuna elaborazione regola aggiuntiva, ignorare l'elaborazione delle regole rimanenti.

### <a name="apache-modrewrite"></a>Apache mod_rewrite
Applicare le regole di Apache mod_rewrite con `AddApacheModRewrite`. Assicurarsi che il file delle regole viene distribuito con l'app. Per ulteriori informazioni ed esempi di regole mod_rewrite, vedere [mod_rewrite Apache](https://httpd.apache.org/docs/2.4/rewrite/).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

A `StreamReader` viene utilizzato per leggere le regole di *ApacheModRewrite.txt* file delle regole.

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=1,7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Il primo parametro accetta un `IFileProvider`, che viene fornito tramite [Dependency Injection](dependency-injection.md). Il `IHostingEnvironment` viene inserito per fornire il `ContentRootFileProvider`. Il secondo parametro è il percorso al file di regole, ovvero *ApacheModRewrite.txt* nell'app di esempio.

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=4)]

---

L'app di esempio reindirizza le richieste da `/apache-mod-rules-redirect/(.\*)` a `/redirected?id=$1`. Il codice di stato della risposta è 302 (trovato).

[!code[Main](url-rewriting/samples/2.x/ApacheModRewrite.txt)]

Richiesta originale:`/apache-mod-rules-redirect/1234`

![Finestra del browser con gli strumenti di sviluppo rilevamento le richieste e risposte](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a>Variabili server supportati
Il middleware supporta variabili Apache mod_rewrite server seguenti:
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
* STRINGA_QUERY
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
* ORA
* TIME_DAY
* TIME_HOUR
* TIME_MIN
* TIME_MON
* TIME_SEC
* TIME_WDAY
* TIME_YEAR

### <a name="iis-url-rewrite-module-rules"></a>Regole di IIS URL Rewrite Module
Per utilizzare le regole che si applicano a IIS URL Rewrite Module, utilizzare `AddIISUrlRewrite`. Assicurarsi che il file delle regole viene distribuito con l'app. Non indirizzare il middleware di utilizzare il *Web. config* file quando è in esecuzione in Windows Server IIS. Con IIS, queste regole devono essere archiviate di fuori del *Web. config* per evitare conflitti con il modulo IIS di riscrittura. Per ulteriori informazioni ed esempi di regole di IIS URL Rewrite Module, vedere [utilizzando Url Rewrite Module 2.0](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) e [URL riscrivere configurazione riferimento al modulo](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

A `StreamReader` viene utilizzato per leggere le regole di *IISUrlRewrite.xml* file delle regole.

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=2,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Il primo parametro accetta un `IFileProvider`, mentre il secondo parametro è il percorso al file di regole XML, ovvero *IISUrlRewrite.xml* nell'app di esempio.

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=5)]

---

L'app di esempio riscrive le richieste provenienti da `/iis-rules-rewrite/(.*)` a `/rewritten?id=$1`. La risposta viene inviata al client con un codice di stato 200 (OK).

[!code-xml[Main](url-rewriting/samples/2.x/IISUrlRewrite.xml)]

Richiesta originale:`/iis-rules-rewrite/1234`

![Finestra del browser con gli strumenti di sviluppo di rilevamento di richiesta e risposta](url-rewriting/_static/add_iis_url_rewrite.png)

Se si dispone di un modulo di riscrivere IIS attivo con le regole a livello di server configurate che avrebbe sulle app in modi indesiderati, è possibile disabilitare il modulo IIS di riscrittura per un'app. Per ulteriori informazioni, vedere [moduli IIS disabilitazione](xref:hosting/iis-modules#disabling-iis-modules).

#### <a name="unsupported-features"></a>Funzionalità non supportate

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Il middleware rilasciato con ASP.NET Core 2. x non supporta le seguenti funzionalità di IIS URL Rewrite Module:
* Regole in uscita
* Variabili Server personalizzato
* Caratteri jolly
* LogRewrittenUrl

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Il middleware rilasciato con ASP.NET Core 1. x non supporta le seguenti funzionalità di IIS URL Rewrite Module:
* Regole globali
* Regole in uscita
* Mappe di riscrittura.
* Azione CustomResponse
* Variabili Server personalizzato
* Caratteri jolly
* Azione: CustomResponse
* LogRewrittenUrl

---

#### <a name="supported-server-variables"></a>Variabili server supportati
Il middleware supporta le seguenti variabili server IIS URL Rewrite Module:
* CONTENT_LENGTH
* CONTENT_TYPE
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_HOST
* HTTP_REFERER
* URL_HTTP
* HTTP_USER_AGENT
* HTTPS
* LOCAL_ADDR
* STRINGA_QUERY
* REMOTE_ADDR
* REMOTE_PORT
* REQUEST_FILENAME
* REQUEST_URI

> [!NOTE]
> È anche possibile ottenere un `IFileProvider` tramite un `PhysicalFileProvider`. Questo approccio può fornire una maggiore flessibilità per la posizione della riscrittura file delle regole. Assicurarsi che i file di regole di riscrittura vengono distribuiti nel server in corrispondenza del percorso che è fornire.
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>Regole basate su metodo
Utilizzare `Add(Action<RewriteContext> applyRule)` per implementare la propria logica della regola in un metodo. Il `RewriteContext` espone il `HttpContext` per l'utilizzo del metodo. Il `context.Result` determina le modalità pipeline l'elaborazione viene gestita.

| contesto. Risultato                       | Azione                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| `RuleResult.ContinueRules` (impostazione predefinita) | Continuare ad applicare le regole                                         |
| `RuleResult.EndResponse`             | Arrestare l'applicazione di regole e inviare la risposta                       |
| `RuleResult.SkipRemainingRules`      | Arrestare l'applicazione di regole e inviare il contesto al middleware successivo |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=6)]

---

L'app di esempio viene illustrato un metodo che reindirizza le richieste per i percorsi che terminano con *XML*. Se si effettua una richiesta `/file.xml`, viene reindirizzata a `/xmlfiles/file.xml`. Il codice di stato è impostato su 301 (spostato permanentemente). Per un tipo di reindirizzamento, è necessario impostare in modo esplicito il codice di stato della risposta; in caso contrario, viene restituito un codice di stato 200 (OK) e non verrà eseguito il reindirizzamento sul client.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet2)]

---

Richiesta originale:`/file.xml`

![Finestra del browser con gli strumenti di sviluppo rilevamento le richieste e risposte relative a file. Xml](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a>Regola di IRule
Utilizzare `Add(IRule)` per implementare la propria logica della regola in una classe che deriva da `IRule`. Utilizzando un `IRule` offre una maggiore flessibilità rispetto all'uso dell'approccio di regole basate su metodo. La classe derivata può includere un costruttore, in cui è possibile passare parametri per il `ApplyRule` metodo.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=7-8)]

---

I valori dei parametri nell'applicazione di esempio per il `extension` e `newPath` vengono controllati per soddisfare diverse condizioni. Il `extension` deve contenere un valore, e il valore deve essere *PNG*, *jpg*, o *GIF*. Se il `newPath` non è valido, un `ArgumentException` viene generata un'eccezione. Se si effettua una richiesta *image.png*, viene reindirizzata a `/png-images/image.png`. Se si effettua una richiesta *image.jpg*, viene reindirizzata a `/jpg-images/image.jpg`. Il codice di stato è impostato su 301 (spostato permanentemente) e `context.Result` è impostata per arrestare le regole di elaborazione e invio della risposta.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/RewriteRule.cs?name=snippet1)]

---

Richiesta originale:`/image.png`

![Finestra del browser con gli strumenti di sviluppo rilevamento le richieste e risposte per image.png](url-rewriting/_static/add_redirect_png_requests.png)

Richiesta originale:`/image.jpg`

![Finestra del browser con gli strumenti di sviluppo rilevamento le richieste e risposte per image.jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>Esempi di espressioni regolari

| Obiettivo | Stringa Regex &<br>Esempio di corrispondenza | Stringa di sostituzione &<br>Esempio di output |
| ---- | :-----------------------------: | :------------------------------------: |
| Percorso di riscrittura nella stringa di query | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| Rimuovere una barra finale | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| Applicare la barra finale | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| Evitare la riscrittura di richieste specifiche | `(.*[^(\.axd)])$`<br>Sì:`/resource.htm`<br>No:`/resource.axd` | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| Ridisporre i segmenti di URL | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| Sostituire un segmento di URL | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a>Risorse aggiuntive
* [Avvio dell'applicazione](startup.md)
* [Middleware](middleware.md)
* [Espressioni regolari in .NET](/dotnet/articles/standard/base-types/regular-expressions)
* [Linguaggio di espressioni regolari - Riferimento rapido](/dotnet/articles/standard/base-types/quick-ref)
* [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)
* [Using Url Rewrite Module 2.0 (IIS)](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [Riferimento di configurazione modulo riscrittura URL](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [Forum IIS URL Rewrite Module](https://forums.iis.net/1152.aspx)
* [Mantenere una struttura semplice di URL](https://support.google.com/webmasters/answer/76329?hl=en)
* [La riscrittura URL 10 suggerimenti e consigli](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [Non a barre o barre](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
