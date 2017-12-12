---
uid: web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
title: Risoluzione dei problemi HTTP 405 errori dopo la pubblicazione Web API 2 applicazioni | Documenti Microsoft
author: rmcmurray
description: In questa esercitazione viene descritto come risolvere gli errori HTTP 405 dopo la pubblicazione di un'applicazione API Web a un server web di produzione.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2014
ms.topic: article
ms.assetid: 07ec7d37-023f-43ea-b471-60b08ce338f7
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
msc.type: authoredcontent
ms.openlocfilehash: 2455bbed891179466de8fb6ade3b0a8e66eadee6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="troubleshooting-http-405-errors-after-publishing-web-api-2-applications"></a>Risoluzione dei problemi HTTP 405 errori dopo la pubblicazione Web API 2 applicazioni
====================
da [Robert McMurray](https://github.com/rmcmurray)

> In questa esercitazione viene descritto come risolvere gli errori HTTP 405 dopo la pubblicazione di un'applicazione API Web a un server web di produzione.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - [Internet Information Services (IIS)](https://www.iis.net/) (versione 7 o versioni successive)
> - [API Web](../../index.md) (versione 1 o 2)


Le applicazioni Web API utilizzano in genere alcuni verbi HTTP comuni: GET, POST, PUT, DELETE e talvolta PATCH. Detto questo, gli sviluppatori possono essere eseguite in situazioni in cui tali verbi implementate da un altro modulo IIS sul server di produzione, ottenendo una situazione in cui verrà restituito un controller API Web che funziona correttamente in Visual Studio o in un server di sviluppo un HTTP 405 errore quando viene distribuito a un server di produzione. Fortunatamente il problema è risolto facilmente, ma la risoluzione richiede una spiegazione del motivo per cui si è verificato il problema.

## <a name="what-causes-http-405-errors"></a>Qual è la causa HTTP 405 da parte degli errori

Il primo passo verso imparare risolvere i problemi degli errori HTTP 405 è comprendere ciò che effettivamente indica un errore HTTP 405. La gestione principale del documento per HTTP è [RFC 2616](http://www.ietf.org/rfc/rfc2616.txt), che definisce il codice di stato HTTP 405 come ***metodo non consentito***e descrivono ulteriormente questo codice di stato in una situazione in cui &quot;il metodo specificato nella riga della richiesta non è consentito per la risorsa identificata dall'URI della richiesta.&quot; In altre parole, il verbo HTTP non è consentito per l'URL specifico che ha richiesto un client HTTP.

Come una breve revisione, ecco alcuni dei metodi HTTP utilizzati più come definito in RFC 2616, RFC 4918 e 5789 RFC:

| Metodo HTTP | Descrizione |
| --- | --- |
| **OTTIENI** | Questo metodo viene utilizzato per recuperare dati da un URI e probabilmente il metodo HTTP più utilizzati. |
| **HEAD** | Questo metodo è simile al metodo GET, ad eccezione del fatto che in realtà non recupera i dati dall'URI della richiesta: semplicemente recupera lo stato HTTP. |
| **INSERISCI** | Questo metodo viene in genere utilizzato per inviare i nuovi dati all'URI; POST viene spesso utilizzato per inviare i dati. |
| **PUT** | Questo metodo viene in genere utilizzato per dati non elaborati all'URI; PUT viene spesso utilizzato per inviare i dati JSON o XML per applicazioni Web API. |
| **ELIMINARE** | Questo metodo viene utilizzato per rimuovere i dati da un URI. |
| **OPZIONI** | Questo metodo viene in genere utilizzato per recuperare l'elenco di metodi HTTP supportati per un URI. |
| **COPIA DI SPOSTAMENTO** | Questi due metodi vengono utilizzati con WebDAV e il loro scopo è facile comprensione. |
| **MKCOL** | Questo metodo viene utilizzato con WebDAV e viene utilizzato per creare una raccolta (ad esempio, una directory) all'URI specificato. |
| **PROPPATCH PROPFIND** | Questi due metodi vengono utilizzati con WebDAV e vengono utilizzati per eseguire una query o impostare le proprietà per un URI. |
| **SBLOCCA BLOCCO** | Questi due metodi vengono utilizzati con WebDAV e vengono utilizzate per bloccare/sbloccare la risorsa identificata dall'URI della richiesta durante la creazione. |
| **PATCH** | Questo metodo viene utilizzato per modificare una risorsa HTTP esistente. |

Quando uno di questi metodi HTTP è configurato per l'utilizzo nel server, il server risponde con il codice di stato HTTP e altri dati che sono appropriati per la richiesta. (Ad esempio, un metodo GET potrebbe ricevere un HTTP 200 ***OK*** risposta e un metodo PUT potrebbe ricevere un HTTP 201 ***creato*** risposta.)

Se il metodo HTTP non è configurato per l'utilizzo nel server, il server risponde con un 501 HTTP ***non implementato*** errore.

Tuttavia, quando un metodo HTTP è configurato per l'utilizzo nel server, ma è stata disabilitata per un URI specificato, il server risponde con un HTTP 405 ***metodo non consentito*** errore.

## <a name="example-http-405-error"></a>Esempio di HTTP 405 errore

Il seguente esempio richiesta e risposta HTTP illustrano una situazione in cui un client HTTP sta tentando di inserire un valore a un'applicazione API Web in un server web e il server restituisce un errore HTTP che gli Stati che il metodo PUT non è consentiti:


Richiesta HTTP:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample1.cmd)]


Risposta HTTP:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample2.cmd)]


In questo esempio, il client HTTP ha inviato una richiesta JSON valida per l'URL per un'applicazione API Web in un server web, ma il server ha restituito un messaggio di errore HTTP 405 che indica che il metodo PUT non è consentito per l'URL. Al contrario, se l'URI della richiesta non corrisponde a una route per l'applicazione Web API, il server restituirà un HTTP 404 ***non trovato*** errore.

## <a name="resolving-http-405-errors"></a>Risoluzione HTTP 405 da parte degli errori

Esistono diversi motivi per cui un verbo HTTP specifico può non essere consentito, ma non esiste uno scenario principale prevede che è la causa principale di questo errore in IIS: più gestori definiti per lo stesso verbo/metodo e uno dei gestori sta bloccando il gestore previsto l'elaborazione della richiesta. Tramite una spiegazione, IIS elabora i gestori da primo a ultimo in base alle voci di gestore ordine i file ApplicationHost. config e Web. config, dove la prima corrispondenza combinazione di percorso, verbo, risorse, ecc., consentirà di gestire la richiesta.

Nell'esempio seguente è tratto dal file ApplicationHost. config per un server IIS che è stata la restituzione di un errore HTTP 405 quando si utilizza il metodo PUT per inviare dati a un'applicazione Web API. In questo estratto, vengono definiti diversi gestori HTTP e ogni gestore include un set diverso di metodi HTTP per i quali è configurato: l'ultima voce nell'elenco è il gestore di contenuto statico, il gestore predefinito che viene utilizzata dopo altri gestori hanno avuto un chanc e per esaminare la richiesta:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample3.xml)]

Nell'esempio precedente, il gestore WebDAV e il gestore di URL senza estensione di ASP.NET (che viene utilizzato per l'API Web) vengono definiti in modo chiaro per elenchi distinti di metodi HTTP. Si noti che il gestore della DLL ISAPI sia configurato per tutti i metodi HTTP, anche se questa configurazione non necessariamente verrà generato un errore. Tuttavia, le impostazioni di configurazione come questa devono essere considerate durante la risoluzione degli errori HTTP 405.

Nell'esempio precedente, il gestore della DLL ISAPI non è il problema. in realtà, il problema non è stato definito nel file ApplicationHost. config per il server IIS, il problema è stato causato da una voce che è stata effettuata nel file Web. config quando è stata creata l'applicazione di API Web in Visual Studio. Nel seguente estratto dal file Web. config dell'applicazione mostra la posizione del problema:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample4.xml)]

In questo estratto, il gestore di URL senza estensione di ASP.NET viene ridefinito per includere ulteriori metodi HTTP che verranno utilizzati con l'applicazione Web API. Tuttavia, poiché è stato definito un set di metodi HTTP simile per il gestore WebDAV, si verifica un conflitto. In questo caso specifico, il gestore WebDAV è definito e caricato da IIS, anche se WebDAV è disabilitata per il sito Web che include l'applicazione Web API. Durante l'elaborazione di una richiesta PUT HTTP, IIS chiama il modulo WebDAV perché è definito per il verbo PUT. Quando viene chiamato il modulo WebDAV, che controlla la configurazione e che si vede che è disabilitato, pertanto verrà restituito un HTTP 405 ***metodo non consentito*** errore per qualsiasi richiesta che è simile a una richiesta di WebDAV. Per risolvere questo problema, è necessario rimuovere WebDAV dall'elenco dei moduli HTTP per il sito Web in cui è definito l'applicazione Web API. Nell'esempio seguente vengono mostrate le operazioni che potrà aspetto analogo al seguente:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample5.xml)]

Questo scenario viene spesso rilevato dopo che un'applicazione viene pubblicata da un ambiente di sviluppo in un ambiente di produzione, e questo errore si verifica perché l'elenco dei gestori/moduli è diverso tra gli ambienti di sviluppo e produzione. Ad esempio, se si utilizza Visual Studio 2012 o 2013 per sviluppare un'applicazione API Web, IIS Express 8 è il server web predefinito per il test. Il server web di sviluppo è una versione ridotta delle funzionalità IIS completa fornita con un prodotto server, e il server di sviluppo web contiene alcune modifiche che sono stati aggiunti per scenari di sviluppo. Ad esempio, il modulo WebDAV è spesso installato in un server web di produzione in cui è in esecuzione la versione completa di IIS, anche se potrebbe non essere in uso effettivo. La versione di sviluppo di IIS (IIS Express), viene installato il modulo WebDAV, ma le voci per il modulo WebDAV intenzionalmente impostate come commento, in modo senza mai caricare il modulo WebDAV in IIS Express a meno che non si modifica in modo specifico la configurazione di IIS Express impostazioni per aggiungere funzionalità WebDAV all'installazione di IIS Express. Di conseguenza, l'applicazione web potrebbe funzionare correttamente nel computer di sviluppo, ma si possono verificarsi degli errori HTTP 405 quando si pubblica l'applicazione API Web nel server web di produzione.

## <a name="summary"></a>Riepilogo

HTTP 405 errori sono causati da un metodo HTTP da un server web per un URL richiesto non è consentito. Questa condizione si verifica spesso quando è stato definito un gestore specifico di un verbo specifico e tale gestore esegue l'override di gestore che si prevede di elaborare la richiesta.

Se si verifica una situazione in cui viene visualizzato un messaggio di errore 501 HTTP, il che significa che la funzionalità specifica non è stata implementata nel server, spesso questo significa che non vi è alcun gestore definito nelle impostazioni di IIS che corrisponde alla richiesta HTTP, che probabilmente indica che un elemento non è stato installato correttamente nel sistema o un elemento modificato le impostazioni di IIS in modo che nessun gestore definito il metodo di supporto HTTP specifici. Per risolvere il problema, è necessario reinstallare qualsiasi applicazione che sta tentando di utilizzare un metodo HTTP per il quale non include il modulo corrispondente o definizioni di gestore.
