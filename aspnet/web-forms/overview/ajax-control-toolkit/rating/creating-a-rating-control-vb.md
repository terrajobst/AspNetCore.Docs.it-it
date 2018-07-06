---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: Creazione di un controllo Rating (VB) | Microsoft Docs
author: wenz
description: Molti siti Web, di e-commerce a siti di community, offrono agli utenti di articoli di frequenza o elementi. Ciò in genere richiede alcune attività di codifica, ma non è disponibile il...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: cb8bd8ab400bf5633b8218e724ad86cf7271699d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811973"
---
<a name="creating-a-rating-control-vb"></a>Creazione di un controllo Rating (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)

> Molti siti Web, di e-commerce a siti di community, offrono agli utenti di articoli di frequenza o elementi. Ciò in genere richiede alcune attività di codifica, ma è disponibile il Toolkit di controllo a disposizione.


## <a name="overview"></a>Panoramica

Molti siti Web, di e-commerce a siti di community, offrono agli utenti di articoli di frequenza o elementi. Ciò in genere richiede alcune attività di codifica, ma è disponibile il Toolkit di controllo a disposizione.

## <a name="steps"></a>Passaggi

Prima di tutto, è necessario (almeno) due tipi di immagini: uno per una piena la voce rating e uno per un elemento vuoto classificazione. Un elemento di classificazione è in genere un smile o una stella. Per questo scenario, sono disponibili tre file, smiley.png ed empty.png e faccina done.png durante il download del codice sorgente per questa esercitazione.

Quindi, creare un nuovo file ASP.NET e iniziare con l'aggiunta di un `ScriptManager` controllo ad esso:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

Aggiungere quindi il `Rating` controllo da ASP.NET AJAX Control Toolkit. Gli attributi seguenti devono essere impostate per questo esempio:

- `CurrentRating` la valutazione iniziale da usare
- `MaxRating` la classificazione massima
- `EmptyStarCssClass` la classe CSS da utilizzare quando un elemento di classificazione (star) è vuota
- `FilledStarCssClass` la classe CSS da utilizzare quando viene compilato un elemento di classificazione (star)
- `StarCssClass` la classe CSS da utilizzare per un stat visibile
- `WaitingStarCssClass` la classe CSS da utilizzare durante una classificazione a stelle viene inviata al server

Ed ecco il markup che crea un controllo rating con cinque elementi (smileys) di cui non viene compilato inizialmente:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

Le tre classi CSS di cui si fa riferimento a questo punto necessario visualizzare i file di immagine appropriata, che è possibile farlo usando lo stile CSS con facilità:

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

Assicurarsi che si fornisce la larghezza e l'altezza delle tre immagini, in caso contrario, la visualizzazione potrebbe avere un aspetto un po' messed backup.

Infine, il risultato della valutazione dovrebbe essere visualizzato all'utente (o, almeno salvato in un database). In questo caso, aggiungere un'etichetta per l'output di un messaggio di testo e un pulsante di invio per inviare nuovamente il form di classificazione nel server:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

Nel codice lato server, accedere al controllo Rating tramite relativi `ID` e quindi accedere ai relativi `CurrentRating` proprietà che è il numero di elementi di classificazione selezionata, nel nostro esempio, un valore compreso tra 0 e 5.

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

Salvare la pagina e caricarla nel browser. Quando si posiziona gli elementi di classificazione (inizialmente vuota), si verifica un effetto di JavaScript: le modifiche di classificazione. Quando si fa clic sul set di stelle, viene mantenuta la classificazione corrente. Infine, quando si invia il form, il codice lato server restituisce la classificazione selezionata.


[![Creazione di un sistema di valutazione con quantità minima di codice](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)

Creazione di un sistema di valutazione con quantità minima di codice ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-rating-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](creating-a-rating-control-cs.md)
