---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: Nell'API Web ASP.NET negoziazione del contenuto | Microsoft Docs
author: MikeWasson
description: Viene descritto come API Web ASP.NET implementa la negoziazione del contenuto HTTP.
ms.author: aspnetcontent
ms.date: 05/20/2012
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: 2314a263a12c74e80c08391ae03425955a82458a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810317"
---
<a name="content-negotiation-in-aspnet-web-api"></a>Negoziazione del contenuto nell'API Web ASP.NET
====================
da [Mike Wasson](https://github.com/MikeWasson)

Questo articolo descrive come API Web ASP.NET implementa la negoziazione del contenuto.

La specifica HTTP (RFC 2616) definisce la negoziazione del contenuto come "il processo di selezione la migliore rappresentazione per una determinata risposta quando sono disponibili più rappresentazioni." Il meccanismo principale per la negoziazione del contenuto di tipo HTTP sono le intestazioni della richiesta:

- **Accettare:** quali tipi di supporto sono accettabili per la risposta, ad esempio "application/json", "application/xml" o un tipo di supporto personalizzato, ad esempio &quot;application/vnd.example+xml&quot;
- **Charset Accept:** i set di caratteri sono accettabili, ad esempio UTF-8 o ISO 8859-1.
- **Codifica:** le codifiche di contenuto sono accettabili, ad esempio gzip.
- **Accept-Language:** del linguaggio naturale preferito, ad esempio "en-us".

Il server può anche esaminare altre parti della richiesta HTTP. Ad esempio, se la richiesta contiene un'intestazione X-Requested-With, che indica una richiesta AJAX, il server potrebbe per impostazione predefinita in formato JSON se non è presente alcuna intestazione Accept.

In questo articolo, esamineremo come API Web Usa le intestazioni Accept e Accept-Charset. (A questo punto, non vi è alcun supporto predefinito per Accept-Encoding o Accept-Language).

## <a name="serialization"></a>Serializzazione

Se un controller Web API restituisce una risorsa come tipo CLR, la pipeline serializza il valore restituito e lo scrive nel corpo della risposta HTTP.

Ad esempio, si consideri l'azione del controller seguente:

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

Un client può inviare la richiesta HTTP:

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

In risposta, il server potrebbe inviare:

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

In questo esempio, il client ha richiesto JSON, Javascript o "qualsiasi" (\*/\*). Il server Delivery con una rappresentazione JSON del `Product` oggetto. Si noti che l'intestazione Content-Type nella risposta è impostata su &quot;application/json&quot;.

Un controller può restituire anche un **HttpResponseMessage** oggetto. Per specificare un oggetto CLR per il corpo della risposta, chiamare il **CreateResponse** metodo di estensione:

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

Questa opzione offre maggiore controllo sui dettagli della risposta. È possibile impostare il codice di stato, aggiungere le intestazioni HTTP e così via.

L'oggetto che viene serializzata la risorsa viene chiamato un *formattatore di media*. Formattatori di Media derivano dal **MediaTypeFormatter** classe. API Web fornisce formattatori di media per XML e JSON ed è possibile creare i formattatori personalizzati per supportare altri tipi di supporto. Per informazioni sulla scrittura di un formattatore personalizzato, vedere [formattatori di Media](media-formatters.md).

## <a name="how-content-negotiation-works"></a>Funziona come contenuto di negoziazione

In primo luogo, la pipeline Ottiene il **IContentNegotiator** servizio il **HttpConfiguration** oggetto. Ottiene anche l'elenco di formattatori di file multimediali dal **HttpConfiguration.Formatters** raccolta.

Successivamente, chiama la pipeline **IContentNegotiatior.Negotiate**passando:

- Il tipo di oggetto da serializzare
- La raccolta di formattatori di file multimediali
- La richiesta HTTP

Il **Negotiate** metodo restituisce due tipi di informazioni:

- Il formattatore da utilizzare
- Il tipo di supporto per la risposta

Se non viene trovato alcun formattatore, il **Negotiate** metodo restituisce **null**e l'errore del client riceve HTTP 406 (pagina non valida).

Il codice seguente viene illustrato come un controller può richiamare direttamente la negoziazione del contenuto:

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

Questo codice è equivalente a elementi di cui la pipeline viene eseguita automaticamente.

## <a name="default-content-negotiator"></a>Negoziatore del contenuto predefinita

Il **DefaultContentNegotiator** classe fornisce l'implementazione predefinita di **IContentNegotiator**. Usa criteri diversi per selezionare un formattatore.

In primo luogo, il formattatore deve essere in grado di serializzare il tipo. Questa operazione viene verificata chiamando **MediaTypeFormatter.CanWriteType**.

Successivamente, il negoziatore del contenuto esamina ciascun formattatore e viene valutata come associa la richiesta HTTP. Per valutare la corrispondenza, il negoziatore del contenuto esamina due cose sul formattatore:

- Il **SupportedMediaTypes** insieme che contiene un elenco di tipi di supporto. Negoziatore del contenuto Cerca la corrispondenza con l'elenco con l'intestazione Accept della richiesta. Si noti che l'intestazione Accept può includere intervalli. Ad esempio, "text/plain" è una corrispondenza per il testo /\* oppure \* / \*.
- Il **MediaTypeMappings** insieme che contiene un elenco delle **MediaTypeMapping** oggetti. Il **MediaTypeMapping** classe fornisce un modo generico per corrispondono alle richieste HTTP con i tipi di supporto. Ad esempio, è stato possibile eseguire il mapping di un'intestazione HTTP personalizzata a un tipo di supporti particolare.

Se sono presenti più corrispondenze, la corrispondenza con il server wins factor qualità più elevata. Ad esempio:

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

In questo esempio, applicazione/json è un fattore di qualità implicita pari a 1,0, pertanto è preferito su application/xml.

Se viene trovata alcuna corrispondenza, il negoziatore del contenuto tenta di corrispondenza per il tipo di supporti del corpo della richiesta, se presente. Ad esempio, se la richiesta contiene i dati JSON, il negoziatore del contenuto Cerca un formattatore JSON.

Se non sono ancora presenti corrispondenze, il negoziatore del contenuto sceglie semplicemente il primo formattatore in grado di serializzare il tipo.

## <a name="selecting-a-character-encoding"></a>Selezionare una codifica dei caratteri

Dopo aver selezionato un formattatore, il negoziatore del contenuto sceglie la codifica dei caratteri migliori esaminando il **SupportedEncodings** proprietà il formattatore e trovare una corrispondenza con l'intestazione Accept-Charset nella richiesta (se presente).
