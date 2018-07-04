---
uid: web-api/overview/advanced/http-cookies
title: I cookie HTTP nell'API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 5885586df1d0f67d4e7e04ad88bc4fd1af71dc80
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368405"
---
<a name="http-cookies-in-aspnet-web-api"></a>Cookie HTTP nell'API Web ASP.NET
====================
da [Mike Wasson](https://github.com/MikeWasson)

In questo argomento viene descritto come inviare e ricevere i cookie HTTP nell'API Web.

## <a name="background-on-http-cookies"></a>Informazioni sull'utilizzo di cookie HTTP

Questa sezione viene fornita una breve panoramica di come vengono implementati i cookie a livello HTTP. Per informazioni dettagliate, consultare [RFC 6265](http://tools.ietf.org/html/rfc6265).

Un cookie è costituito da dati che invia un server nella risposta HTTP. Il client (facoltativamente) archivia il cookie e lo restituisce nel subsequet richieste. In questo modo il client e server condividere lo stato. Per impostare un cookie, il server include un'intestazione Set-Cookie nella risposta. Il formato di un cookie è una coppia nome-valore, con gli attributi facoltativi. Ad esempio:

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

Di seguito è riportato un esempio con attributi:

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

Per restituire un cookie al server, il client include un'intestazione Cookie nelle richieste successive.

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

Una risposta HTTP può includere più intestazioni di Set-Cookie.

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

Il client restituisce più cookie con una singola intestazione Cookie.

[!code-console[Main](http-cookies/samples/sample5.cmd)]

L'ambito e la durata di un cookie sono controllate da attributi seguenti nell'intestazione Set-Cookie:

- **Dominio**: indica al client di dominio che deve ricevere il cookie. Ad esempio, se il dominio è "example.com", il client restituisce il cookie a ogni sottodominio di example.com. Se non specificato, il dominio è il server di origine.
- **Percorso**: limita il cookie nel percorso specificato all'interno del dominio. Se non specificato, viene utilizzato il percorso dell'URI della richiesta.
- **Scadenza**: imposta una data di scadenza del cookie. Il client elimina il cookie alla scadenza.
- **Max-Age**: imposta la durata massima per il cookie. Il client elimina il cookie quando raggiunge la durata massima.

Se entrambe `Expires` e `Max-Age` sono impostate, `Max-Age` ha la precedenza. Se non è impostato, il client elimina il cookie al termine della sessione corrente. (Il significato esatto di "sessione" è determinato dall'agente utente).

Tuttavia, tenere presente che i client potrebbero ignorare i cookie. Ad esempio, un utente può disabilitare i cookie per motivi di privacy. I client possono eliminare i cookie prima di scadere, o limitare il numero di cookie archiviato. Per motivi di privacy, i client spesso rifiutare i cookie "third party", dove il dominio non corrisponde il server di origine. In breve, il server necessario non affidarsi a ricevendo i cookie che verrà impostato.

## <a name="cookies-in-web-api"></a>Cookie in API Web

Per aggiungere un cookie a una risposta HTTP, creare un **CookieHeaderValue** istanza che rappresenta il cookie. Chiamare quindi il **AddCookies** metodo di estensione, che viene definito nel **System.NET. http. HttpResponseHeadersExtensions** (classe), aggiungere il cookie.

Ad esempio, il codice seguente aggiunge un cookie all'interno di un'azione del controller:

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

Si noti che **AddCookies** accetta una matrice di **CookieHeaderValue** istanze.

Per estrarre i cookie dalla richiesta del client, chiamare il **GetCookies** metodo:

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

Oggetto **CookieHeaderValue** contiene una raccolta di **CookieState** istanze. Ciascuna **CookieState** rappresenta un cookie. Usare il metodo di un indicizzatore per ottenere un **CookieState** in base al nome, come illustrato.

## <a name="structured-cookie-data"></a>Dati dei Cookie strutturato

Molti browser Limita numero di cookie che si archivierà&#8212;sia il numero totale e il numero per ogni dominio. Pertanto, può essere utile inserire i dati strutturati in un cookie single, anziché impostare più cookie.

> [!NOTE]
> RFC 6265 non definisce la struttura dei dati del cookie.


Usando il **CookieHeaderValue** (classe), è possibile passare un elenco di coppie nome-valore per i dati del cookie. Queste coppie nome-valore sono codificate come dati del form con codifica URL nell'intestazione Set-Cookie:

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

Il codice precedente produce l'intestazione Set-Cookie seguente:

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

Il **CookieState** classe fornisce un metodo di un indicizzatore per la lettura dei valori secondario da un cookie nel messaggio di richiesta:

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>Esempio: Impostare e recuperare i cookie in un gestore di messaggi

Negli esempi precedenti è stato illustrato come utilizzare i cookie provenienti da all'interno di un controller API Web. Un'altra opzione consiste nell'usare [gestori di messaggi](http-message-handlers.md). In precedenza nella pipeline di controller vengono richiamati i gestori di messaggi. Un gestore di messaggi può leggere i cookie dalla richiesta prima che la richiesta raggiunga il controller o aggiungere i cookie nella risposta dopo che il controller ha generato la risposta.

![](http-cookies/_static/image2.png)

Il codice seguente illustra un gestore di messaggi per la creazione degli ID sessione. L'ID di sessione viene archiviato in un cookie. Il gestore verifica la richiesta per il cookie di sessione. Se la richiesta non include il cookie, il gestore genera un ID sessione nuovo. In entrambi i casi, il gestore memorizza l'ID di sessione nel **HttpRequestMessage.Properties** contenitore delle proprietà. Aggiunge anche il cookie di sessione nella risposta HTTP.

Questa implementazione non convalida che l'ID di sessione dal client sia stato effettivamente rilasciato dal server. Non usarlo come una forma di autenticazione. Il punto dell'esempio è illustrare la gestione dei cookie HTTP.

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

Un controller può ottenere l'ID di sessione dal **HttpRequestMessage.Properties** contenitore delle proprietà.

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
