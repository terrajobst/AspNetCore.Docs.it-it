---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: "L'invio di dati di Form HTML nell'API Web ASP.NET: dati Form-urlencoded | Microsoft Docs"
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/15/2012
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 2d01212cc408f8bb66fa3103464c9a1f7a1e21c6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823811"
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>L'invio di dati di Form HTML nell'API Web ASP.NET: dati Form-urlencoded
====================
da [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>Parte 1: Dati di Form-urlencoded

Questo articolo illustra come pubblicare dati form-urlencoded a un controller API Web.

- [Panoramica dei moduli HTML](#overview_of_html_forms)
- [L'invio di tipi complessi](#sending_complex_types)
- [L'invio di dati del Form tramite AJAX](#sending_form_data_via_ajax)
- [L'invio di tipi semplici](#sending_simple_types)

> [!NOTE]
> [Scaricare il progetto completato](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>Panoramica dei moduli HTML

Utilizzo di form HTML a GET o POST per inviare dati al server. Il **metodo** attributo delle **form** elemento fornisce il metodo HTTP:

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

Il metodo predefinito è GET. Se il form Usa GET, il formato dati sono codificati nell'URI come stringa di query. Se il form Usa POST, i dati del modulo viene inseriti nel corpo della richiesta. Per i dati registrati, la **enctype** attributo specifica il formato del corpo della richiesta:

| Enctype | Descrizione |
| --- | --- |
| application/x-www-form-urlencoded | Dati del form sono codificati come coppie nome/valore, simile a una stringa di query URI. Questo è il formato predefinito per POST. |
| multipart/form-data | Dati del form sono codificati come un messaggio MIME multiparte. Utilizzare questo formato, se si sta caricando un file al server. |

Parte 1 di questo articolo vengono esaminati formato x-www-form-urlencoded. [Parte 2](sending-html-form-data-part-2.md) descrive multipart MIME.

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>L'invio di tipi complessi

In genere, si invierà un tipo complesso, composto da valori ricavati da vari controlli del form. Si consideri il modello seguente rappresenta un aggiornamento di stato:

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

Di seguito è un controller API Web che accetta un `Update` oggetto tramite POST.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> Questo controller Usa [basate su azioni routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), quindi il modello di route viene &quot;api / {controller} / {action} / {id}&quot;. Il client invierà i dati da &quot;/api/updates/complex&quot;.


Ora passiamo alla scrittura di un form HTML per gli utenti di inviare un aggiornamento di stato.

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

Si noti che il **azione** attributo nel form è l'URI del nostro azione del controller. Ecco il modulo con alcuni valori immessi:

![](sending-html-form-data-part-1/_static/image1.png)

Quando l'utente sceglie invio, il browser invia una richiesta HTTP simile al seguente:

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

Si noti che il corpo della richiesta contiene i dati del modulo, formattati come coppie nome/valore. API Web, le coppie nome/valore viene convertito automaticamente in un'istanza di `Update` classe.

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>L'invio di dati del Form tramite AJAX

Quando un utente invia un form, il browser si sposta dalla pagina corrente ed esegue il rendering il corpo del messaggio di risposta. È normale quando la risposta è una pagina HTML. Con un'API web, tuttavia, il corpo della risposta è in genere sia vuota o contiene i dati strutturati, ad esempio JSON. In tal caso, è più opportuno inviare i dati del modulo usando AJAX richiedono, in modo che la pagina è possibile elaborare la risposta.

Il codice seguente viene illustrato come pubblicare i dati del modulo tramite jQuery.

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

Il componente aggiuntivo jQuery **inviare** funzione sostituisce le azioni form con una nuova funzione. Sostituisce il comportamento predefinito del pulsante di invio. Il **serializzare** funzione serializza i dati del modulo in coppie nome/valore. Per inviare i dati del form al server, chiamare `$.post()`.

Al completamento della richiesta, il `.success()` o `.error()` gestore viene visualizzato un messaggio appropriato all'utente.

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>L'invio di tipi semplici

Nelle sezioni precedenti, abbiamo inviato un tipo complesso, API Web deserializzata in un'istanza di una classe di modello. È anche possibile inviare i tipi semplici, ad esempio una stringa.

> [!NOTE]
> Prima di inviare un tipo semplice, prendere in considerazione il wrapping invece il valore in un tipo complesso. Ciò offre i vantaggi di convalida del modello sul lato server e rende più semplice estendere il modello, se necessario.


I passaggi di base per l'invio di un tipo semplice sono uguali, ma vi sono due differenze meno evidenti. Nel controller, in primo luogo, è necessario decorare il nome del parametro con il **FromBody** attributo.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

Per impostazione predefinita, API Web prova a ottenere i tipi semplici dall'URI della richiesta. Il **FromBody** attributo indica a Web API per leggere il valore dal corpo della richiesta.

> [!NOTE]
> API Web legge il corpo della risposta in modo che solo un parametro di un'azione può provenire dal corpo della richiesta più di una volta. Se è necessario ottenere valori più dal corpo della richiesta, definire un tipo complesso.


In secondo luogo, il client deve inviare il valore con il formato seguente:

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

In particolare, la parte relativa al nome della coppia nome/valore deve essere vuoto per un tipo semplice. Non tutti i browser supportano ad form HTML, ma questo formato è creare nello script come indicato di seguito:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

Ecco un form di esempio:

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

Ed ecco lo script per inviare il valore di modulo. L'unica differenza da script precedente è l'argomento passato il **post** (funzione).

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

È possibile usare lo stesso approccio per l'invio di una matrice di tipi semplici:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>Risorse aggiuntive

[Parte 2: Caricamento di File e MIME Multipart](sending-html-form-data-part-2.md)
