---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
title: Introduzione a AJAX Control Toolkit (VB) | Microsoft Docs
author: microsoft
description: Scopri tutto che devi sapere per iniziare a usare AJAX Control Toolkit.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 9f8fa166-49a2-402c-b236-20caef0c658f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
msc.type: authoredcontent
ms.openlocfilehash: 6041087df3a15ef42d2364881f08a991f4eeb4fc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367774"
---
<a name="get-started-with-the-ajax-control-toolkit-vb"></a>Introduzione a AJAX Control Toolkit (VB)
====================
da [Microsoft](https://github.com/microsoft)

> Scopri tutto che devi sapere per iniziare a usare AJAX Control Toolkit.


AJAX Control Toolkit contiene più di 30 controlli gratuiti che è possibile usare nelle applicazioni ASP.NET. In questa esercitazione descrive come scaricare AJAX Control Toolkit e aggiungere i controlli toolkit per la casella degli strumenti di Visual Studio e Visual Web Developer Express.

## <a name="downloading-the-ajax-control-toolkit"></a>Download di AJAX Control Toolkit

Il [AJAX Control Toolkit](http://devexpress.com/act) è un progetto open source sviluppato dai membri della community ASP.NET e il team di ASP.NET.


[![Download di AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)

**Figura 01**: download di AJAX Control Toolkit ([fare clic per visualizzare l'immagine con dimensioni normali](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))


Dopo aver scaricato il file, è necessario sbloccare il file. Fare doppio clic su file, selezionare le proprietà e scegliere il **Unblock** pulsante (vedere la figura 2).


[![Il file ZIP Toolkit controllo AJAX di sblocco](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)

**Figura 02**: il file ZIP Toolkit controllo AJAX di sblocco ([fare clic per visualizzare l'immagine con dimensioni normali](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))


Dopo aver sbloccato il file, è possibile decomprimere il file: il pulsante destro e selezionare il **Estrai tutto** l'opzione di menu. A questo punto, siamo pronti aggiungere il toolkit alla casella degli strumenti di Visual Studio e Visual Web Developer.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Aggiunta di AJAX Control Toolkit alla casella degli strumenti

Il modo più semplice usare AJAX Control Toolkit consiste nell'aggiungere il toolkit per la casella degli strumenti di Visual Studio e Visual Web Developer (vedere la figura 3). In questo modo, è possibile semplicemente trascinare un controllo toolkit in una pagina quando si desidera utilizzarla.


[![AJAX Control Toolkit viene visualizzato nella casella degli strumenti](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)

**Figura 03**: AJAX Control Toolkit viene visualizzato nella casella degli strumenti ([fare clic per visualizzare l'immagine con dimensioni normali](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))


In primo luogo, è necessario aggiungere una scheda di AJAX Control Toolkit alla casella degli strumenti. Seguire questi passaggi.

1. Creare un nuovo sito Web ASP.NET selezionando l'opzione di menu File, nuovo sito Web. Fare doppio clic su default. aspx nella finestra Esplora soluzioni per aprire il file nell'editor.
2. Fare doppio clic su casella degli strumenti sotto la scheda Impostazioni generali e selezionare l'opzione di menu **Aggiungi scheda** (vedere la figura 4).
3. Immettere una nuova scheda denominata AJAX Control Toolkit.


[![Aggiunta di una nuova scheda](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)

**Figura 04**: aggiungere una nuova scheda ([fare clic per visualizzare l'immagine con dimensioni normali](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))


Successivamente, è necessario aggiungere i controlli di AJAX Control Toolkit per la nuova scheda. Attenersi ai passaggi riportati di seguito.

- Fare clic sotto la scheda di AJAX Control Toolkit e selezionare l'opzione di menu **Scegli elementi (vedere la figura 5)**.
- Passare al percorso in cui è stata decompressa AJAX Control Toolkit e selezionare l'assembly di AjaxControlToolkit. dll.


[![Scegliere gli elementi da aggiungere alla casella degli strumenti](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)

**Figura 05**: scegliere gli elementi da aggiungere alla casella degli strumenti ([fare clic per visualizzare l'immagine con dimensioni normali](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))


Dopo aver completato questi passaggi, tutti i controlli toolkit verranno visualizzati nella casella degli strumenti.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>L'aggiornamento a una nuova versione del Toolkit

Se si usa una versione precedente del Toolkit, è possibile spostare in una versione più recente di seguito sono riportati i passaggi consigliati:

- I file binari - eliminare la versione precedente dell'assembly AjaxControlToolkit. dll dalla cartella Bin del sito Web.
- Gli elementi della casella degli strumenti - Elimina la scheda di AJAX Control Toolkit e seguire i passaggi precedenti per creare di nuovo la scheda con la nuova versione dell'assembly AjaxControlToolkit. dll.

> [!div class="step-by-step"]
> [Precedente](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
> [Successivo](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
