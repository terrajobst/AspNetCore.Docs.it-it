---
uid: web-api/overview/advanced/http-cookies
title: I cookie HTTP in ASP.NET Web API | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: e17c51946a268aa13ec035d18dc516928c9f4419
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="http-cookies-in-aspnet-web-api"></a>Cookie HTTP in ASP.NET Web API
====================
da [Mike Wasson](https://github.com/MikeWasson)

In questo argomento viene descritto come inviare e ricevere i cookie HTTP nell'API Web.

## <a name="background-on-http-cookies"></a>Informazioni sui cookie HTTP

Questa sezione viene fornita una breve panoramica delle modalità di implementazione dei cookie al livello HTTP. Per informazioni dettagliate, consultare [6265 RFC](http://tools.ietf.org/html/rfc6265).

Un cookie è una porzione di dati che invia un server nella risposta HTTP. Il client (facoltativamente) archivia il cookie e lo restituisce nel subsequet richieste. In questo modo il client e server condividere lo stato. Per impostare un cookie, il server include un'intestazione Set-Cookie nella risposta. Il formato di un cookie è una coppia nome-valore, con gli attributi facoltativi. Ad esempio:

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

Di seguito è riportato un esempio con attributi:

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

Per restituire un cookie al server, il client di contiene un'intestazione Cookie nelle richieste successive.

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

Una risposta HTTP può includere più intestazioni Set-Cookie.

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

Il client restituisce più cookie con una singola intestazione Cookie.

[!code-console[Main](http-cookies/samples/sample5.cmd)]

L'ambito e la durata di un cookie sono controllate dagli attributi seguenti nell'intestazione Set-Cookie:

- **Dominio**: indica al client di dominio che deve ricevere il cookie. Ad esempio, se il dominio è "example.com", il client restituisce il cookie a ogni sottodominio di example.com. Se non specificato, il dominio è il server di origine.
- **Percorso**: limita il cookie al percorso specificato all'interno del dominio. Se non specificato, viene utilizzato il percorso dell'URI della richiesta.
- **Scadenza**: imposta una data di scadenza del cookie. Dopo la scadenza, il client elimina il cookie.
- **Max-Age**: imposta la durata massima dei cookie. Quando viene raggiunta la durata massima, il client elimina il cookie.

Se entrambi `Expires` e `Max-Age` sono impostate, `Max-Age` ha la precedenza. Se non è impostato, il client elimina il cookie al termine della sessione corrente. (Il significato esatto di "sessione" è determinato dall'agente utente).

Tuttavia, tenere presente che i client possono ignorare i cookie. Ad esempio, un utente potrebbe disabilitare i cookie per motivi di privacy. I client possono eliminare i cookie prima alla scadenza oppure limitare il numero di cookie memorizzato. Per motivi di privacy, i client rifiutare spesso i cookie "third party", dove il dominio non corrisponde il server di origine. In breve, il server non deve basarsi su come ottenere nuovamente i cookie che verrà impostato.

## <a name="cookies-in-web-api"></a>Cookie nel Web API

Per aggiungere un cookie a una risposta HTTP, creare un **CookieHeaderValue** istanza che rappresenta il cookie. Chiamare quindi il **AddCookies** metodo di estensione, è definito nel **System.NET. http. HttpResponseHeadersExtensions** (classe), per aggiungere il cookie.

Ad esempio, il codice seguente aggiunge un cookie all'interno di un'azione del controller:

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

Si noti che **AddCookies** accetta una matrice di **CookieHeaderValue** istanze.

Per estrarre i cookie da una richiesta client, chiamare il **GetCookies** metodo:

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

Oggetto **CookieHeaderValue** contiene una raccolta di **CookieState** istanze. Ogni **CookieState** rappresenta un cookie. Utilizzare il metodo dell'indicizzatore per ottenere un **CookieState** in base al nome, come illustrato.

## <a name="structured-cookie-data"></a>Dati strutturati Cookie

Molti browser Limita numero di cookie che verrà archiviano &#8212; il numero totale sia il numero per ogni dominio. Pertanto, può essere utile inserire i dati strutturati in un unico cookie, anziché impostare più cookie.

> [!NOTE]
> RFC 6265 non definisce la struttura dei dati del cookie.


Utilizzo di **CookieHeaderValue** (classe), è possibile passare un elenco di coppie nome-valore per i dati del cookie. Queste coppie nome-valore sono codificate come dati con codifica URL form nell'intestazione Set-Cookie:

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

Il codice precedente produce l'intestazione Set-Cookie seguente:

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

Il **CookieState** classe fornisce un metodo di un indicizzatore per leggere i valori secondario da un cookie nel messaggio di richiesta:

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>Esempio: Impostare e recuperare i cookie in un gestore di messaggi

Negli esempi precedenti è stati illustrati come utilizzare i cookie dall'interno di un controller API Web. Un'altra opzione consiste nell'utilizzare [gestori di messaggi](http-message-handlers.md). In precedenza nella pipeline di controller vengono richiamati i gestori di messaggi. Un gestore di messaggi può leggere i cookie dalla richiesta prima che la richiesta raggiunga il controller oppure aggiungere i cookie nella risposta dopo che il controller generato la risposta.

![](http-cookies/_static/image2.png)

Il codice seguente viene illustrato un gestore di messaggi per la creazione degli ID sessione. L'ID di sessione viene archiviato in un cookie. Il gestore verifica la richiesta per il cookie di sessione. Se la richiesta non include il cookie, il gestore genera un ID sessione nuovo. In entrambi i casi, il gestore memorizza l'ID di sessione nel **HttpRequestMessage.Properties** contenitore delle proprietà. Aggiunge inoltre il cookie di sessione nella risposta HTTP.

Questa implementazione non convalida che l'ID di sessione dal client sia stato effettivamente rilasciato dal server. Non viene usato come modulo di autenticazione. Il punto dell'esempio è illustrare la gestione dei cookie HTTP.

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

Un controller è possibile ottenere l'ID di sessione dal **HttpRequestMessage.Properties** contenitore delle proprietà.

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
