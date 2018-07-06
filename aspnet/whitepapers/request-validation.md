---
uid: whitepapers/request-validation
title: Richiesta di convalida - prevenzione degli attacchi tramite Script | Microsoft Docs
author: rick-anderson
description: In questo documento descrive la funzionalità di convalida richiesta di ASP.NET in cui, per impostazione predefinita, l'applicazione viene impedita l'elaborazione submitt di contenuto HTML non codificato...
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 0dfbfcae70792c57d530fc5e6fb73f8f96ec6e02
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809745"
---
<a name="request-validation---preventing-script-attacks"></a>Richiesta di convalida - prevenzione degli attacchi tramite Script
====================
> In questo documento descrive la funzionalità di ASP.NET in cui, per impostazione predefinita, l'applicazione viene impedita l'elaborazione del contenuto HTML non codificato inviato al server di convalida delle richieste. Questa funzionalità di convalida richiesta può essere disabilitata quando l'applicazione è stata progettata per elaborare in modo sicuro i dati HTML.
> 
> Si applica a ASP.NET 1.1 e ASP.NET 2.0.


Convalida delle richieste, una funzionalità di ASP.NET sin dalla versione 1.1, impedisce al server di accettare contenuto HTML non codificato che lo contiene. Questa funzionalità è progettata per aiutare a evitare alcuni attacchi script injection in base al quale il codice di script client o HTML può essere inconsapevolmente inviato a un server, archiviati e quindi presentato ad altri utenti. È comunque consigliabile che convalida i dati di tutti gli input e codificarli con HTML, quando appropriato.

Ad esempio, si crea una pagina Web che richiede l'indirizzo di posta elettronica dell'utente e quindi archivia tale indirizzo in un database di posta elettronica. Se l'utente immette &lt;SCRIPT&gt;avviso ("hello from script")&lt;/script&gt; anziché un indirizzo di posta elettronica valido, quando viene visualizzato tale data, questo script può essere eseguito se il contenuto non è stato codificato correttamente. La funzionalità di convalida richiesta di ASP.NET consente di evitare questo tipo di problema.

## <a name="why-this-feature-is-useful"></a>Perché questa funzionalità è utile

Molti siti non sono a conoscenza che sono aperti per attacchi intrusivi negli script semplice. Se lo scopo di questi attacchi è per infettare il sito visualizzando HTML o potenzialmente eseguire lo script client a reindirizzare l'utente al sito un pirata informatico, attacchi intrusivi negli script sono un problema che devono fare i conti con gli sviluppatori Web.

Attacchi intrusivi negli script rappresentano un problema di tutti gli sviluppatori web, se si usa ASP.NET, ASP o altre tecnologie di sviluppo web.

La funzionalità di convalida delle richieste ASP.NET in modo proattivo impedisce questi attacchi, non consentendo il contenuto HTML non codificato essere elaborati dal server, a meno che lo sviluppatore decide di consentire che il contenuto.

## <a name="what-to-expect-error-page"></a>Cosa accade: pagina di errore

Cattura di schermata seguente mostra alcuni esempi di codice ASP.NET:

![](request-validation/_static/image1.png)

L'esecuzione di genera questo codice in una pagina semplice che consente di immettere il testo nella casella di testo, fare clic sul pulsante e visualizzare il testo nel controllo etichetta:

![](request-validation/_static/image2.png)

Tuttavia, era, ad esempio JavaScript, `<script>alert("hello!")</script>` immissione e inviato si otterrebbero un'eccezione:

![](request-validation/_static/image3.png)

Il messaggio di errore indica che un 'potenzialmente pericolosi Request. Form è stato rilevato valore' e fornisce altre informazioni, vedere la descrizione esattamente ciò che si è verificato e come modificare il comportamento. Ad esempio:

Convalida delle richieste ha rilevato un valore di input potenzialmente pericolosa client e l'elaborazione della richiesta è stata interrotta. Questo valore potrebbe indicare un tentativo di compromettere la sicurezza dell'applicazione, ad esempio un attacco di scripting intersito. È possibile disabilitare la convalida della richiesta impostando `validateRequest=false` nella direttiva di pagina o nella sezione di configurazione. Tuttavia, è consigliabile che l'applicazione in modo esplicito controllare tutti gli input in questo caso.

## <a name="disabling-request-validation-on-a-page"></a>La disabilitazione della convalida richiesta in una pagina

Per disabilitare la convalida delle richieste in una pagina è necessario impostare il `validateRequest` attributo della direttiva della pagina per `false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> Quando la convalida delle richieste è disabilitata, il contenuto può essere inviato a una pagina. è responsabilità dello sviluppatore della pagina per verificare che il contenuto viene codificata o elaborate correttamente.

## <a name="disabling-request-validation-for-your-application"></a>La disabilitazione della convalida richiesta per l'applicazione

Per disabilitare la convalida della richiesta per l'applicazione, è necessario modificare o creare un file Web. config per l'applicazione e impostare l'attributo validateRequest del `<pages />` sezione `false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

Se si vuole disabilitare la convalida delle richieste per tutte le applicazioni nel server, è possibile apportare questa modifica al file Machine. config.

> [!CAUTION]
> Quando la convalida delle richieste è disabilitata, è possibile inviare contenuto dell'applicazione. è responsabilità dello sviluppatore dell'applicazione per verificare che il contenuto viene codificata o elaborate correttamente.

Il codice seguente viene modificato per disattivare la convalida delle richieste:

![](request-validation/_static/image4.png)

Ora, se il codice JavaScript seguente è stata immessa nella casella di testo `<script>alert("hello!")</script>` il risultato sarà:

![](request-validation/_static/image5.png)

Per evitare questa situazione, con la convalida delle richieste disabilitata, è necessario in formato HTML codificare il contenuto.

## <a name="how-to-html-encode-content"></a>Come codificare il contenuto in formato HTML

Se è stata disabilitata la convalida della richiesta, è buona norma codifica HTML del contenuto che verrà archiviato per un uso futuro. La codifica HTML sostituirà automaticamente qualsiasi '&lt;'o'&gt;' (insieme a diversi altri simboli) con il codice HTML corrispondente rappresentazione con codifica. Ad esempio, '&lt;'viene sostituito da'&amp;lt;' e '&gt;'viene sostituito da'&amp;gt;'. I browser usano questi codici speciali per la visualizzazione di '&lt;'o'&gt;' nel browser.

Il contenuto può essere facilmente codificata in formato HTML sul server utilizzando il `Server.HtmlEncode(string)` API. Contenuto può anche essere facilmente-decodificato in HTML, vale a dire, ripristinato l'utilizzo di codice HTML standard di `Server.HtmlDecode(string)` (metodo).

![](request-validation/_static/image6.png)

Risultante:

![](request-validation/_static/image7.png)
