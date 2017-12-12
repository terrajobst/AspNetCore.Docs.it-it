---
uid: whitepapers/request-validation
title: Richiedere la convalida - prevenzione degli attacchi Script | Documenti Microsoft
author: rick-anderson
description: "In questo documento descrive la funzionalità di convalida richiesta di ASP.NET in cui, per impostazione predefinita, l'applicazione viene impedito di elaborazione submitt di contenuto HTML non codificato..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 61a96b75fdc29bdd1510ed689ee0356ef30e03fc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="request-validation---preventing-script-attacks"></a>Richiedere la convalida - prevenzione degli attacchi di Script
====================
> Questo documento descrive la funzionalità di ASP.NET in cui, per impostazione predefinita, l'applicazione viene impedito di elaborazione del contenuto HTML non codificato di inviata al server di convalida delle richieste. Questa funzionalità di convalida richiesta può essere disabilitata quando l'applicazione è stato progettato per elaborare in modo sicuro dati HTML.
> 
> Si applica a ASP.NET 1.1 e 2.0 di ASP.NET.


Convalida della richiesta, una funzionalità di ASP.NET versione 1.1, impedisce il server di accettare contenuto HTML non codificato che lo contiene. Questa funzionalità è progettata per aiutare a evitare alcuni attacchi script injection, in base al quale il codice di script client o HTML può essere involontario inviati a un server, archiviati e quindi presentato ad altri utenti. È comunque consigliabile che la convalida dei dati di tutti gli input e la codifica HTML quando appropriato.

Ad esempio, si crea una pagina Web che richiede un indirizzo di posta elettronica e quindi archivia tale indirizzo di posta elettronica in un database. Se l'utente immette &lt;SCRIPT&gt;avviso ("hello dallo script")&lt;/SCRIPT&gt; anziché un indirizzo di posta elettronica valido, quando i dati viene presentati, questo script può essere eseguito se il contenuto non è stato codificato correttamente. La funzionalità di convalida richiesta di ASP.NET consente di evitare questo inconveniente.

## <a name="why-this-feature-is-useful"></a>Perché questa funzionalità è utile

Molti siti non sono in grado di riconoscere che sono vulnerabili agli attacchi intrusivi nel codice script semplice. Se lo scopo di tali attacchi è per infettare il sito visualizzando HTML o potenzialmente eseguire lo script client per reindirizzare l'utente al sito un pirata informatico, attacchi intrusivi nel codice di script sono un problema che gli sviluppatori Web devono competere con.

Attacchi intrusivi nel codice script rappresentano un problema di tutti gli sviluppatori web, se si utilizza ASP.NET, ASP o altre tecnologie di sviluppo web.

La funzionalità di convalida delle richieste ASP.NET in modo proattivo impedisce questi attacchi, non consentendo contenuto HTML non codificato essere elaborati dal server, a meno che lo sviluppatore decide di consentire che il contenuto.

## <a name="what-to-expect-error-page"></a>Cosa accade: pagina di errore

Cattura di schermata seguente mostra alcuni esempi di codice ASP.NET:

![](request-validation/_static/image1.png)

L'esecuzione di genera questo codice in una pagina semplice che consente di immettere del testo nella casella di testo, fare clic sul pulsante e visualizzare il testo nel controllo etichetta:

![](request-validation/_static/image2.png)

Tuttavia, sono ad esempio JavaScript, `<script>alert("hello!")</script>` per essere immessi e inviati si otterrebbe un'eccezione:

![](request-validation/_static/image3.png)

Il messaggio di errore indica che un 'potenzialmente pericolosi Request. Form è stato rilevato valore' e fornisce altre informazioni, vedere la descrizione esattamente ciò che si è verificato e come modificare il comportamento. Ad esempio:

Convalida della richiesta ha rilevato un valore di input potenzialmente pericoloso client e l'elaborazione della richiesta è stata interrotta. Questo valore potrebbe indicare un tentativo di compromettere la sicurezza dell'applicazione, ad esempio un attacco di script tra siti. È possibile disabilitare la convalida della richiesta impostando `validateRequest=false` nell'istruzione Page o nella sezione di configurazione. Tuttavia, è consigliabile che l'applicazione in modo esplicito verificare tutti gli input in questo caso.

## <a name="disabling-request-validation-on-a-page"></a>Disabilitare la convalida della richiesta in una pagina

Per disabilitare la convalida della richiesta in una pagina è necessario impostare il `validateRequest` attributo della direttiva della pagina per `false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> Quando la convalida della richiesta è disabilitata, il contenuto può essere inviato a una pagina. è responsabilità dello sviluppatore della pagina per verificare che il contenuto con codifica o elaborate correttamente.

## <a name="disabling-request-validation-for-your-application"></a>Sulla disabilitazione della convalida richiesta per l'applicazione

Per disabilitare la convalida della richiesta per l'applicazione, è necessario modificare o creare un file Web. config per l'applicazione e impostare l'attributo validateRequest del `<pages />` sezione a `false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

Se si desidera disabilitare la convalida della richiesta per tutte le applicazioni nel server, è possibile apportare questa modifica al file Machine. config.

> [!CAUTION]
> Quando la convalida della richiesta è disabilitata, è possibile inviare contenuto dell'applicazione. è responsabilità dello sviluppatore dell'applicazione per assicurarsi che il contenuto con codifica o elaborate correttamente.

Il codice seguente viene modificato per disattivare la convalida della richiesta:

![](request-validation/_static/image4.png)

Se è stato immesso il codice JavaScript seguente nella casella di testo `<script>alert("hello!")</script>` il risultato sarebbe:

![](request-validation/_static/image5.png)

Per evitare questa situazione, con la convalida delle richieste disattivata, abbiamo base in formato HTML codificare il contenuto.

## <a name="how-to-html-encode-content"></a>Come codificare contenuto in formato HTML

Se è stata disabilitata la convalida della richiesta, è buona norma contenuto con codifica HTML che verrà archiviato per un utilizzo futuro. La codifica HTML verrà automaticamente sostituito qualsiasi '&lt;'o'&gt;' (insieme a diversi altri simboli) con codice HTML corrispondente rappresentazione con codifica. Ad esempio, '&lt;'viene sostituito da'&amp;lt;' e '&gt;'viene sostituito da'&amp;gt;'. I browser usano questi codici speciali per visualizzare il '&lt;'o'&gt;' nel browser.

Il contenuto può essere facilmente codificata in formato HTML sul server utilizzando il `Server.HtmlEncode(string)` API. Contenuto può inoltre essere facilmente HTML decodifica, vale a dire ripristinato l'utilizzo di HTML standard di `Server.HtmlDecode(string)` metodo.

![](request-validation/_static/image6.png)

Risultante:

![](request-validation/_static/image7.png)
