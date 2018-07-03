---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: Creazione di route personalizzate (VB) | Microsoft Docs
author: microsoft
description: Informazioni su come aggiungere route personalizzati a un'applicazione ASP.NET MVC. In questa esercitazione descrive come modificare la tabella di route predefinito nel file Global. asax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: b2bca002434274e334509d21f6857e094d391198
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362018"
---
<a name="creating-custom-routes-vb"></a>Creazione di route personalizzate (VB)
====================
da [Microsoft](https://github.com/microsoft)

> Informazioni su come aggiungere route personalizzati a un'applicazione ASP.NET MVC. In questa esercitazione descrive come modificare la tabella di route predefinito nel file Global. asax.


In questa esercitazione descrive come aggiungere una route personalizzata a un'applicazione ASP.NET MVC. Descrive come modificare la tabella di route predefinito nel file Global. asax con una route personalizzata.

Nelle applicazioni ASP.NET MVC, la tabella di route predefinite funzioneranno senza problemi. Tuttavia, è possibile scoprire che si sono specializzati esigenze di routine. In tal caso, è possibile creare una route personalizzata.

Si supponga, ad esempio, che si compila un'applicazione blog. Si potrebbe voler gestire le richieste in ingresso con un aspetto simile al seguente:

/Archive/12-25-2009

Quando un utente immette la richiesta, si desidera restituire la voce del blog che corrisponde alla data 12/25/2009. Per gestire questo tipo di richiesta, è necessario creare una route personalizzata.

Il file Global. asax nel listato 1 contiene una nuova route personalizzata, denominata Blog, che gestisce le richieste che hanno l'aspetto /Archive/*data voce*.

**Listato 1 - Global. asax (con route personalizzate)**

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

L'ordine delle route che aggiungono alla tabella di route è importante. La nuova route Blog personalizzata viene aggiunto prima route esistente predefinita. Se è invertito l'ordine, quindi verrà chiamata la route predefinita sempre anziché route personalizzata.

La route di Blog personalizzata corrisponde a qualsiasi richiesta che inizia con/archiviazione /. Pertanto, corrisponde a tutti gli URL seguenti:

- /Archive/12-25-2009

- / Archiviazione/10-6-2004

- / Archiviazione/apple

Route personalizzata esegue il mapping della richiesta in ingresso a un controller denominato archivio e richiama l'azione Entry(). Quando viene chiamato il metodo Entry(), la data di inizio viene passata come un parametro denominato entryDate.

È possibile usare la route personalizzata Blog con il controller nel listato 2.

**Listato 2 - ArchiveController.vb**

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

Si noti che il metodo Entry() nel listato 2 accetta un parametro di tipo DateTime. Il framework MVC è in grado di convertire automaticamente la data di inizio dall'URL in un valore DateTime. Se il parametro della data voce dall'URL non può essere convertito in un oggetto DateTime, viene generato un errore (vedere la figura 1).

**Figura 1. errore di conversione del parametro**


[![La finestra di dialogo Nuovo progetto](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)

**Figura 01**: errore di conversione del parametro ([fare clic per visualizzare l'immagine con dimensioni normali](creating-custom-routes-vb/_static/image2.png))


## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione è stata per illustrare come è possibile creare una route personalizzata. Si è appreso come aggiungere una route personalizzata alla tabella di route nel file Global. asax che rappresenta le voci di blog. È stato illustrato come eseguire il mapping di richieste per le voci di blog a un controller denominato ArchiveController e un'azione del controller denominato Entry().

> [!div class="step-by-step"]
> [Precedente](asp-net-mvc-controller-overview-vb.md)
> [Successivo](creating-a-route-constraint-vb.md)
