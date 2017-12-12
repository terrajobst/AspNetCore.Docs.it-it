---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: Contenuti di negoziazione in ASP.NET Web API | Documenti Microsoft
author: MikeWasson
description: "Descrive la modalità di implementazione negoziazione del contenuto HTTP in ASP.NET Web API."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/20/2012
ms.topic: article
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: ca373af6754e82889dc100b63f73b76aaa4e4f27
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="content-negotiation-in-aspnet-web-api"></a>Negoziazione del contenuto in ASP.NET Web API
====================
da [Mike Wasson](https://github.com/MikeWasson)

In questo articolo viene descritto come ASP.NET Web API implementa negoziazione del contenuto.

La specifica di HTTP (RFC 2616) definisce negoziazione del contenuto come "il processo di selezione la migliore rappresentazione di una risposta specificata quando sono disponibili più rappresentazioni". Il meccanismo principale per la negoziazione del contenuto HTTP sono queste intestazioni della richiesta:

- **Accettare:** quali tipi di supporto sono accettabili per la risposta, ad esempio "application/json", "application/xml" o un tipo di supporto personalizzata, ad esempio &quot;application/vnd.example+xml&quot;
- **Charset Accept:** i set di caratteri accettabili, ad esempio UTF-8 o ISO 8859-1.
- **Codifica:** le codifiche di contenuto sono accettabili, ad esempio gzip.
- **Accept-Language:** la lingua preferita naturale, ad esempio "en-us".

Il server può inoltre esaminare altre parti della richiesta HTTP. Ad esempio, se la richiesta contiene un'intestazione X-Requested-With, che indica una richiesta AJAX, il server potrebbe predefinito per JSON se è presente alcuna intestazione Accept.

In questo articolo verranno esaminate le intestazioni Accept e Accept-Charset di utilizzo delle API Web. (In questo momento, non vi è alcun supporto predefinito per Accept-Encoding o Accept-Language).

## <a name="serialization"></a>Serializzazione

Se un controller API Web restituisce una risorsa come tipo CLR, la pipeline serializza il valore restituito e di scriverla nel corpo della risposta HTTP.

Si consideri ad esempio l'azione del controller seguente:

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

Un client può inviare la richiesta HTTP:

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

In risposta, il server potrebbe inviare:

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

In questo esempio, il client ha richiesto JSON, Javascript o "qualsiasi" (\*/\*). Il server è disponibile una rappresentazione JSON del `Product` oggetto. Si noti che l'intestazione Content-Type nella risposta è impostata su &quot;application/json&quot;.

Un controller può restituire anche un **HttpResponseMessage** oggetto. Per specificare un oggetto CLR per il corpo della risposta, chiamare il **CreateResponse** metodo di estensione:

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

Questa opzione garantisce un maggiore controllo sui dettagli della risposta. È possibile impostare il codice di stato, aggiungere le intestazioni HTTP e così via.

L'oggetto che serializza la risorsa viene chiamato un *formattatore di media*. Formattatori di Media derivano il **MediaTypeFormatter** classe. API Web fornisce formattatori di media per XML e JSON ed è possibile creare formattatori personalizzati per supportare altri tipi di supporti. Per informazioni sulla scrittura di un formattatore personalizzato, vedere [formattatori di Media](media-formatters.md).

## <a name="how-content-negotiation-works"></a>Funzionamento di negoziazione come contenuto

Ottiene prima di tutto, la pipeline di **IContentNegotiator** servizio il **HttpConfiguration** oggetto. Ottiene anche l'elenco di formattatori di media dal **HttpConfiguration.Formatters** insieme.

Successivamente, la pipeline chiama **IContentNegotiatior.Negotiate**, passando:

- Il tipo di oggetto da serializzare
- La raccolta di formattatori di media
- La richiesta HTTP

Il **Negotiate** restituisce due tipi di informazioni:

- Il formattatore da utilizzare
- Il tipo di supporto per la risposta

Se non viene trovato alcun formattatore, il **Negotiate** restituisce **null**e l'errore del client riceve HTTP 406 (non valida).

Il codice seguente viene illustrato come un controller può richiamare direttamente negoziazione del contenuto:

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

Questo codice è equivalente per la pipeline viene eseguita automaticamente.

## <a name="default-content-negotiator"></a>Negoziatore del contenuto predefinito

Il **DefaultContentNegotiator** classe fornisce l'implementazione predefinita di **IContentNegotiator**. Per selezionare un formattatore Usa diversi criteri.

In primo luogo, il formattatore deve essere in grado di serializzare il tipo. Questa operazione viene verificata chiamando **MediaTypeFormatter.CanWriteType**.

Successivamente, il negoziatore del contenuto esamina ciascun formattatore e valuta l'accuratezza corrisponde alla richiesta HTTP. Per valutare la corrispondenza, il negoziatore del contenuto esamina due operazioni sul formattatore:

- Il **SupportedMediaTypes** insieme, che contiene un elenco di tipi di supporto. Negoziatore del contenuto tenta di associare l'elenco di intestazione Accept della richiesta. Si noti che l'intestazione Accept può includere gli intervalli. Ad esempio, "text/plain" è una corrispondenza per il testo /\* o \* / \*.
- Il **MediaTypeMappings** insieme, che contiene un elenco di **MediaTypeMapping** oggetti. Il **MediaTypeMapping** classe fornisce un modo generico per la corrispondenza di richieste HTTP con i tipi di supporto. Ad esempio, è possibile mappare un'intestazione HTTP personalizzata per un determinato tipo di supporto.

Se sono presenti più corrispondenze, la corrispondenza con il server wins fattore qualità più elevata. Ad esempio:

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

In questo esempio, application/json è un fattore di qualità implicita pari a 1,0, pertanto è preferibile su application/xml.

Se non vengono trovate corrispondenze, il negoziatore del contenuto tenta di corrispondenza per il tipo di supporto del corpo della richiesta, se presente. Ad esempio, se la richiesta contiene i dati JSON, negoziatore del contenuto Cerca un formattatore JSON.

Se non sono ancora presenti corrispondenze, il negoziatore del contenuto sceglie semplicemente il primo formattatore in grado di serializzare il tipo.

## <a name="selecting-a-character-encoding"></a>Selezionare una codifica dei caratteri

Dopo aver selezionato un formattatore, negoziatore del contenuto sceglie la migliore codifica dei caratteri esaminando il **SupportedEncodings** proprietà, il formattatore e trovare una corrispondenza con l'intestazione Accept-Charset nella richiesta (se presente).
