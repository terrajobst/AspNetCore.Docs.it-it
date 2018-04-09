---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Introduzione a AJAX Control Toolkit (c#) | Documenti Microsoft
author: microsoft
description: Informazioni su tutte le che necessarie per iniziare l'utilizzo di AJAX Control Toolkit.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: e6a7a8d45f32a33eaacf3c42b52a02d2ada1aab6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="get-started-with-the-ajax-control-toolkit-c"></a>Introduzione a AJAX Control Toolkit (c#)
====================
by [Microsoft](https://github.com/microsoft)

> Informazioni su tutte le che necessarie per iniziare l'utilizzo di AJAX Control Toolkit.


AJAX Control Toolkit contiene più di 30 disponibile i controlli che è possibile utilizzare nelle applicazioni ASP.NET. In questa esercitazione informazioni su come scaricare AJAX Control Toolkit e aggiungere i controlli casella degli strumenti di Visual Studio e Visual Web Developer Express.

## <a name="downloading-the-ajax-control-toolkit"></a>Download di AJAX Control Toolkit

Il [AJAX Control Toolkit](http://devexpress.com/act) è un progetto open source sviluppato dai membri della community di ASP.NET e il team ASP.NET. 


[![Download di AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)

**Figura 01**: download di AJAX Control Toolkit ([fare clic per visualizzare l'immagine ingrandita](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))


Dopo aver scaricato il file, è necessario sbloccare il file. Il file, selezionare le proprietà e scegliere il **Unblock** pulsante (vedere la figura 2).


[![Il file ZIP Toolkit controllo AJAX di sblocco](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)

**Figura 02**: il file ZIP Toolkit controllo AJAX di sblocco ([fare clic per visualizzare l'immagine ingrandita](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))


Dopo aver sbloccato il file, è possibile decomprimere il file: il pulsante destro e selezionare il **Estrai tutto** opzione di menu. A questo punto, sono pronti aggiungere il toolkit nella casella degli strumenti di Visual Studio e Visual Web Developer.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Aggiunta di AJAX Control Toolkit nella casella degli strumenti

Il modo più semplice per utilizzare AJAX Control Toolkit consiste nell'aggiungere il toolkit per la casella degli strumenti di Visual Studio e Visual Web Developer (vedere la figura 3). In questo modo, è possibile semplicemente trascinare un controllo toolkit in una pagina quando si desidera utilizzarlo.


[![AJAX Control Toolkit viene visualizzato nella casella degli strumenti](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)

**Figura 03**: AJAX Control Toolkit viene visualizzato nella casella degli strumenti ([fare clic per visualizzare l'immagine ingrandita](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))


In primo luogo, è necessario aggiungere una scheda di AJAX Control Toolkit nella casella degli strumenti. Seguire questi passaggi.

1. Creare un nuovo sito Web ASP.NET selezionando l'opzione del menu File, nuovo sito Web. Fare doppio clic su default. aspx nella finestra Esplora soluzioni per aprire il file nell'editor.
2. Il pulsante destro della casella degli strumenti sotto la scheda Impostazioni generali e selezionare l'opzione di menu **Aggiungi scheda** (vedere la figura 4).
3. Immettere una nuova scheda denominata AJAX Control Toolkit.


[![Aggiunta di una nuova scheda](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)

**Figura 04**: aggiunta di una nuova scheda ([fare clic per visualizzare l'immagine ingrandita](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))


Successivamente, è necessario aggiungere i controlli di AJAX Control Toolkit per la nuova scheda. Attenersi ai passaggi riportati di seguito.

- Fare clic sotto la scheda AJAX Control Toolkit e selezionare l'opzione di menu **Scegli elementi (vedere Figura 5)**.
- Passare al percorso in cui è stato decompresso AJAX Control Toolkit e selezionare l'assembly AjaxControlToolkit. dll.


[![Scegliere gli elementi da aggiungere alla casella degli strumenti](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)

**Figura 05**: scegliere gli elementi da aggiungere alla casella degli strumenti ([fare clic per visualizzare l'immagine ingrandita](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))


Dopo aver completato questi passaggi, tutti i controlli toolkit verrà visualizzato nella casella degli strumenti.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>L'aggiornamento a una nuova versione del Toolkit

Se si usa una versione precedente del Toolkit e ora devono essere spostati in una versione più recente qui sono le procedure consigliate:

- I file binari, eliminare la versione precedente dell'assembly AjaxControlToolkit. dll dalla cartella Bin del sito Web.
- Gli elementi della casella degli strumenti - eliminare la scheda AJAX Control Toolkit e seguire i passaggi precedenti per creare di nuovo la scheda con la nuova versione dell'assembly AjaxControlToolkit. dll.

> [!div class="step-by-step"]
> [avanti](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
