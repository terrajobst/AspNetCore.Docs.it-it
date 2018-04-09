---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Creazione di route personalizzate (c#) | Documenti Microsoft
author: microsoft
description: Informazioni su come aggiungere route personalizzate per un'applicazione MVC ASP.NET. In questa esercitazione imparare a modificare la tabella di route predefinita nel file Global. asax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: 573b6a3360124feea92788ff7a3de363840fa1ef
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="creating-custom-routes-c"></a>Creazione di route personalizzate (c#)
====================
by [Microsoft](https://github.com/microsoft)

> Informazioni su come aggiungere route personalizzate per un'applicazione MVC ASP.NET. In questa esercitazione imparare a modificare la tabella di route predefinita nel file Global. asax.


In questa esercitazione informazioni su come aggiungere una route personalizzata a un'applicazione ASP.NET MVC. Informazioni su come modificare la tabella di route predefinita nel file Global. asax con una route personalizzata.

Per molte applicazioni ASP.NET MVC semplice, la tabella di route predefinita funzionerà senza problemi. Tuttavia, si potrebbe individuare si dispongano di routine esigenze specifiche. In tal caso, è possibile creare una route personalizzata.

Si supponga, ad esempio, si compila un'applicazione di blog. È consigliabile gestire le richieste in ingresso che è simile al seguente:

/Archive/12-25-2009

Quando un utente immette la richiesta, si desidera restituire il post di blog che corrisponde alla data 25/12/2009. Per gestire questo tipo di richiesta, è necessario creare una route personalizzata.

Il file Global. asax nel listato 1 contiene una nuova route personalizzata, denominata Blog, che gestisce le richieste simili /Archive/*data*.

**Elenco 1 - Global. asax (con route personalizzati)**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

L'ordine di route che aggiungono alla tabella di route è importante. La nuova route Blog personalizzata viene aggiunto prima la route predefinita esistente. Se è invertito l'ordine, quindi verrà chiamata la route predefinita sempre invece di route personalizzati.

La route di Blog personalizzata corrisponde a qualsiasi richiesta che inizia con archivio /. In tal caso, corrisponde tutti gli URL seguenti:

- /Archive/12-25-2009

- /Archive/10-6-2004

- / Archiviazione/apple

La route personalizzata viene eseguito il mapping della richiesta in ingresso a un controller denominato archivio e richiama l'azione Entry(). Quando viene chiamato il metodo Entry(), la data di inizio viene passata come un parametro denominato entryDate.

È possibile utilizzare la route personalizzata Blog con il controller nel listato 2.

**Il listato 2 - ArchiveController.cs**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

Si noti che il metodo Entry() nel listato 2 accetta un parametro di tipo DateTime. Il framework di MVC è abbastanza per convertire la data di inizio dall'URL in un valore DateTime automaticamente. Se il parametro della data voce dall'URL non può essere convertito in un oggetto DateTime, viene generato un errore (vedere la figura 1).

**Figura 1 - errore dalla conversione del parametro**


[![La finestra di dialogo Nuovo progetto](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)

**Figura 01**: errore di conversione del parametro ([fare clic per visualizzare l'immagine ingrandita](creating-custom-routes-cs/_static/image2.png))


## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione è dimostrare come creare una route personalizzata. È stato descritto come aggiungere una route personalizzata per la tabella di routing nel file Global. asax che rappresenta i post di blog. Viene illustrato come mappare le richieste di post di blog di un controller denominato ArchiveController e un'azione del controller denominato Entry().

> [!div class="step-by-step"]
> [Precedente](aspnet-mvc-controllers-overview-cs.md)
> [Successivo](creating-a-route-constraint-cs.md)
