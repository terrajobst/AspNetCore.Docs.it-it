---
uid: web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
title: Risoluzione dei problemi relativi a HTTP 405 errori dopo la pubblicazione Web API 2 applicazioni | Microsoft Docs
author: rmcmurray
description: Questa esercitazione descrive come risolvere gli errori HTTP 405 dopo la pubblicazione di un'applicazione API Web a un server web di produzione.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2014
ms.topic: article
ms.assetid: 07ec7d37-023f-43ea-b471-60b08ce338f7
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
msc.type: authoredcontent
ms.openlocfilehash: 8027644e9430d49962e61db21b9e21426eabd136
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366119"
---
<a name="troubleshooting-http-405-errors-after-publishing-web-api-2-applications"></a>Risoluzione dei problemi relativi a HTTP 405 errori dopo la pubblicazione Web API 2 applicazioni
====================
da [Robert McMurray](https://github.com/rmcmurray)

> Questa esercitazione descrive come risolvere gli errori HTTP 405 dopo la pubblicazione di un'applicazione API Web a un server web di produzione.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - [Internet Information Services (IIS)](https://www.iis.net/) (versione 7 o versioni successive)
> - [API Web](../../index.md) (versione 1 o 2)


Le applicazioni Web API usano in genere alcuni verbi HTTP comuni: GET, POST, PUT, DELETE e talvolta di PATCH. Premesso questo, gli sviluppatori possono essere eseguite in situazioni in cui tali verbi sono implementate da un altro modulo IIS sul server di produzione, che conduce a una situazione in cui verrà restituito un controller Web API che funziona correttamente in Visual Studio o in un server di sviluppo un HTTP 405 errore quando viene distribuita in un server di produzione. Fortunatamente è semplice risolto questo problema, ma la soluzione richiede una spiegazione del motivo per cui si è verificato il problema.

## <a name="what-causes-http-405-errors"></a>Qual è la causa HTTP 405 da parte degli errori

Il primo passo per imparare a problemi degli errori HTTP 405 consiste nel comprendere ciò che effettivamente indica un errore HTTP 405. L'oggetto principale del documento per HTTP è [RFC 2616](http://www.ietf.org/rfc/rfc2616.txt), che definisce il codice di stato HTTP 405 quando ***metodo non consentito***e descrivono ulteriormente questo codice di stato come una situazione in cui &quot;il metodo specificato nella riga della richiesta non è consentita per la risorsa identificata dall'URI della richiesta.&quot; In altre parole, il verbo HTTP non è consentito per l'URL specifico che ha richiesto un client HTTP.

Come una breve revisione, ecco alcuni dei metodi HTTP utilizzati più di base a quanto definito in RFC 2616, 4918 RFC e RFC 5789:

| Metodo HTTP | Descrizione |
| --- | --- |
| **OTTIENI** | Questo metodo viene utilizzato per recuperare dati da un URI che probabilmente il metodo HTTP usato nella maggior parte. |
| **HEAD** | Questo metodo è molto simile al metodo GET, ad eccezione del fatto che è effettivamente non recuperare i dati dall'URI della richiesta: recupera semplicemente lo stato HTTP. |
| **INSERISCI** | Questo metodo viene in genere utilizzato per inviare nuovi dati per l'URI; POST viene spesso utilizzato per inviare i dati. |
| **PUT** | Questo metodo viene in genere utilizzato per inviare i dati non elaborati all'URI; PUT viene spesso utilizzato per inviare dati JSON o XML alle applicazioni API Web. |
| **DELETE** | Questo metodo viene utilizzato per rimuovere i dati da un URI. |
| **OPZIONI** | Questo metodo viene in genere usato per recuperare l'elenco di metodi HTTP supportati per un URI. |
| **COPIA LO SPOSTAMENTO** | Questi due metodi vengono usati con WebDAV e il loro scopo è facilmente comprensibile. |
| **MKCOL** | Questo metodo viene utilizzato con WebDAV e viene utilizzato per creare una raccolta (ad esempio, una directory) nell'URI specificato. |
| **PROPFIND PROPPATCH** | Questi due metodi vengono usati con WebDAV e vengono utilizzati per eseguire una query o impostare le proprietà per un URI. |
| **SBLOCCA BLOCCO** | Questi due metodi vengono usati con WebDAV e vengono usate per bloccare/sbloccare la risorsa identificata dall'URI della richiesta durante la creazione. |
| **PATCH** | Questo metodo viene utilizzato per modificare una risorsa HTTP esistente. |

Quando uno di questi metodi HTTP è configurato per l'uso nel server, il server risponde con il codice di stato HTTP e altri dati che sono appropriati per la richiesta. (Ad esempio, un metodo GET potrebbe ricevere un messaggio HTTP 200 ***OK*** risposta e un metodo PUT potrebbe ricevere un HTTP 201 ***Created*** risposta.)

Se il metodo HTTP non è configurato per l'uso nel server, il server risponde con un HTTP di 501 ***non è implementato*** errore.

Tuttavia, quando un metodo HTTP è configurato per l'uso nel server, ma è stata disabilitata per un URI specificato, il server risponde con un HTTP 405 ***metodo non consentito*** errore.

## <a name="example-http-405-error"></a>HTTP 405 da parte di esempio errore

La seguente richiesta HTTP di esempio e la risposta illustrano una situazione in cui un client HTTP sta tentando di inserire un valore a un'applicazione API Web in un server web e il server restituisce un errore HTTP che gli Stati che il metodo PUT non è consentiti:


Richiesta HTTP:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample1.cmd)]


Risposta HTTP:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample2.cmd)]


In questo esempio, il client HTTP ha inviato una richiesta JSON valida all'URL di un'applicazione API Web in un server web, ma il server ha restituito un messaggio di errore HTTP 405 che indica che il metodo PUT non è consentito per l'URL. Al contrario, se l'URI della richiesta non corrisponde una route per l'applicazione API Web, il server restituirà un errore HTTP 404 ***trovata*** errore.

## <a name="resolving-http-405-errors"></a>Risoluzione HTTP 405 da parte degli errori

Esistono diversi motivi per cui un verbo HTTP specifico potrebbe non essere consentito, ma è uno scenario principale prevede che è la causa principale di questo errore in IIS: più gestori definiti per lo stesso verbo/metodo, e uno dei gestori sta bloccando il gestore previsto da l'elaborazione della richiesta. Tramite spiegazioni, IIS elabora i gestori da primo a ultimo basato sulle voci gestore ordine nei file ApplicationHost. config e Web. config, dove verrà utilizzata la combinazione di corrispondenza prima del percorso, verbo, risorse e così via, per gestire la richiesta.

Nell'esempio seguente è tratto da un file ApplicationHost. config per un server IIS che è stato restituito un errore HTTP 405 quando si usa il metodo PUT per inviare dati a un'applicazione API Web. In questo estratto sono definiti diversi gestori HTTP e ogni gestore ha un diverso set di metodi HTTP per cui è configurata: l'ultima voce nell'elenco è il gestore di contenuto statico, ovvero il gestore predefinito che viene usato dopo altri gestori hanno avuto un chanc e per esaminare la richiesta:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample3.xml)]

Nell'esempio precedente, il gestore WebDAV e il gestore di URL senza estensione di ASP.NET (che viene usato per l'API Web) vengono definiti in modo chiaro per elenchi distinti di metodi HTTP. Si noti che il gestore di DLL ISAPI sia configurato per tutti i metodi HTTP, anche se questa configurazione non necessariamente genera un errore. Tuttavia, le impostazioni di configurazione di questo tipo devono essere presi in considerazione durante la risoluzione degli errori HTTP 405.

Nell'esempio precedente, il gestore di DLL ISAPI non è il problema. in effetti, il problema non è stato definito nel file ApplicationHost. config per il server IIS, il problema è stato causato da una voce che è stata apportata nel file Web. config quando l'applicazione API Web è stato creato in Visual Studio. Nel seguente estratto dal file Web. config dell'applicazione mostra la posizione del problema:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample4.xml)]

In questo estratto, il gestore di URL senza estensione di ASP.NET viene ridefinito in modo da includere metodi HTTP aggiuntivi che verranno usati con l'applicazione API Web. Tuttavia, poiché viene definito un set di metodi HTTP simile per il gestore WebDAV, si verifica un conflitto. In questo caso specifico, il gestore di WebDAV è definito e caricato da IIS, anche se WebDAV è disabilitata per il sito Web che include l'applicazione API Web. Durante l'elaborazione di una richiesta PUT HTTP, IIS chiama il modulo WebDAV perché è definito per il verbo PUT. Quando viene chiamato il modulo WebDAV, controlla la configurazione e rileva che è disabilitato, pertanto restituirà un HTTP 405 ***metodo non consentito*** errore per tutte le richieste che è simile a una richiesta di WebDAV. Per risolvere questo problema, è necessario rimuovere WebDAV dall'elenco dei moduli HTTP per il sito Web in cui è definito l'applicazione API Web. L'esempio seguente illustra che cosa che potrebbe essere aspetto:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample5.xml)]

Questo scenario viene rilevato spesso dopo che un'applicazione viene pubblicata da un ambiente di sviluppo in un ambiente di produzione, e ciò si verifica perché l'elenco di gestori/moduli è diverso tra gli ambienti di sviluppo e produzione. Ad esempio, se si usa Visual Studio 2012 o 2013 per lo sviluppo di un'applicazione API Web, IIS Express 8 è il server web predefinito per il test. Questo server web di sviluppo è una versione ridotta di tutte le funzionalità IIS fornito con un prodotto server, e il server web di sviluppo contiene alcune modifiche che sono state aggiunte per scenari di sviluppo. Ad esempio, il modulo WebDAV è spesso installato in un server web di produzione che esegue la versione completa di IIS, anche se potrebbe non essere in uso effettivo. La versione di sviluppo di IIS (IIS Express), viene installato il modulo WebDAV, ma le voci per il modulo WebDAV intenzionalmente impostare come commento, in modo che il modulo WebDAV non è mai caricato in IIS Express a meno che non si modifica in modo specifico la configurazione di IIS Express impostazioni per aggiungere funzionalità WebDAV all'installazione di IIS Express. Di conseguenza, l'applicazione web potrebbe funzionare correttamente nel computer di sviluppo, ma si potrebbero verificare errori HTTP 405 quando si pubblica l'applicazione API Web nel server web di produzione.

## <a name="summary"></a>Riepilogo

HTTP 405 da parte degli errori vengono generati quando un metodo HTTP non è consentito da un server web per un URL richiesto. Questa condizione si verifica spesso quando è stato definito un gestore specifico per un verbo specifico e tale gestore viene eseguito l'override del gestore che si prevede di elaborare la richiesta.

Se si verifica una situazione in cui viene visualizzato un messaggio di errore HTTP 501, il che significa che la funzionalità specifica non è stata implementata nel server, spesso questo significa che non vi è alcun gestore definito nelle impostazioni IIS che corrisponde alla richiesta HTTP, che probabilmente indica che qualcosa non è stato installato correttamente nel sistema, o un elemento ha modificato le impostazioni di IIS in modo che esistono che nessun gestore definito tale metodo di supporto HTTP specifico. Per risolvere il problema, è necessario reinstallare tutte le applicazioni che sta tentando di usare un metodo HTTP per il quale non dispone di alcun modulo corrispondente o definizioni di gestore.
