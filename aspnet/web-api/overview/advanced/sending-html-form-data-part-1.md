---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Invia i dati di Form HTML in ASP.NET Web API: dati di Form-urlencoded | Documenti Microsoft'
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/15/2012
ms.topic: article
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0ed339c4f9d5854ab5a21cdd077a4d494987101f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>Invia i dati di Form HTML in ASP.NET Web API: dati di Form-urlencoded
====================
da [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>Parte 1: Dati di Form-urlencoded

In questo articolo viene illustrato come inviare dati di form-urlencoded a un controller API Web.

- [Panoramica di form HTML](#overview_of_html_forms)
- [L'invio di tipi complessi](#sending_complex_types)
- [L'invio di dati del Form tramite AJAX](#sending_form_data_via_ajax)
- [L'invio di tipi semplici](#sending_simple_types)

> [!NOTE]
> [Scaricare il progetto completato](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>Panoramica di form HTML

Utilizzo di form HTML GET o POST per inviare dati al server. Il **metodo** attributo del **modulo** elemento offre il metodo HTTP:

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

Il metodo predefinito è GET. Se il form Usa GET, il modulo dati sono codificati nell'URI come una stringa di query. Se il form Usa POST, i dati del form viene inseriti nel corpo della richiesta. Per i dati registrati, la **enctype** attributo specifica il formato del corpo della richiesta:

| Enctype | Descrizione |
| --- | --- |
| application/x-www-form-urlencoded | Dati del form viene codificati come coppie nome/valore, simile a una stringa di query URI. Questo è il formato predefinito per POST. |
| multipart/form-data | Dati del form viene codificati come un messaggio MIME multiparte. Utilizzare questo formato, se si sta caricando un file al server. |

Parte 1 di questo articolo vengono esaminati formato x-www-form-urlencoded. [Parte 2](sending-html-form-data-part-2.md) descrive MIME multipart.

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>L'invio di tipi complessi

In genere, si invierà un tipo complesso, costituito da valori ricavati da diversi controlli del form. Si consideri il seguente modello che rappresenta un aggiornamento di stato.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

Di seguito è un controller API Web che accetta un `Update` oggetto tramite POST.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> Usa questo controller [routing basato sul azione](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), pertanto il modello di route è &quot;api / {controller} / {action} / {id}&quot;. Il client registrerà i dati da &quot;/api/updates/complex&quot;.


Ora passiamo alla scrittura di un form HTML per gli utenti di inviare un aggiornamento di stato.

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

Si noti che il **azione** attributo nel form è l'URI dell'azione controller. Di seguito è riportato il form con alcuni valori immessi:

![](sending-html-form-data-part-1/_static/image1.png)

Quando l'utente fa clic su invio, il browser invia una richiesta HTTP simile al seguente:

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

Si noti che il corpo della richiesta contiene dati del form, formattati come coppie nome/valore. API Web, le coppie nome/valore viene convertito automaticamente in un'istanza di `Update` classe.

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>L'invio di dati del Form tramite AJAX

Quando un utente invia un form, il browser si sposta dalla pagina corrente ed esegue il rendering il corpo del messaggio di risposta. È normale quando la risposta è una pagina HTML. Con un'API web, tuttavia, il corpo della risposta è in genere vuoto o contiene dati strutturati, ad esempio JSON. In tal caso, risulta più inviare i dati del modulo utilizzando un AJAX richiedono, in modo che la pagina è possibile elaborare la risposta.

Il codice seguente viene illustrato come registrare i dati del form tramite jQuery.

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

Il jQuery **inviare** funzione sostituisce l'azione del modulo con una nuova funzione. Esegue l'override le impostazioni predefinite del pulsante Invia. Il **serializzare** funzione serializza i dati del form in coppie nome/valore. Per inviare i dati del form al server, chiamare `$.post()`.

Al completamento della richiesta, il `.success()` o `.error()` gestore visualizza un messaggio appropriato per l'utente.

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>L'invio di tipi semplici

Nelle sezioni precedenti, è stato inviato un tipo complesso, API Web deserializzata in un'istanza di una classe di modello. È anche possibile inviare i tipi semplici, ad esempio una stringa.

> [!NOTE]
> Prima di inviare un tipo semplice, considerare il wrapping del valore in un tipo complesso. Questo offre i vantaggi di convalida del modello sul lato server e rende più semplice estendere il modello, se necessario.


I passaggi di base per l'invio di un tipo semplice sono gli stessi, ma vi sono due differenze meno evidenti. Nel controller di in primo luogo, è necessario decorare il nome del parametro con il **FromBody** attributo.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

Per impostazione predefinita, API Web tenta di ottenere i tipi semplici dall'URI della richiesta. Il **FromBody** attributo indica a Web API per leggere il valore dal corpo della richiesta.

> [!NOTE]
> API Web legge il corpo della risposta in modo che solo un parametro di un'azione può provenire dal corpo della richiesta più di una volta. Se è necessario ottenere più valori dal corpo della richiesta, è possibile definire un tipo complesso.


In secondo luogo, il client deve inviare il valore con il formato seguente:

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

In particolare, la parte del nome della coppia nome/valore deve essere vuota per un tipo semplice. Non tutti i browser supportano ad form HTML, ma si crea questo formato nello script come segue:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

Di seguito è un modulo di esempio:

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

Di seguito è lo script per inviare il valore di modulo. L'unica differenza dello script precedente è costituito dall'argomento passato il **post** (funzione).

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

È possibile utilizzare lo stesso approccio per l'invio di una matrice di tipi semplici:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>Risorse aggiuntive

[Parte 2: Caricamento del File e in più parti MIME](sending-html-form-data-part-2.md)
