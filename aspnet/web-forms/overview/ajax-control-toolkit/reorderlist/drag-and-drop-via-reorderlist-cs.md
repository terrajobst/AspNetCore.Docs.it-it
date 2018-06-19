---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
title: Trascinamento della selezione tramite ReorderList (c#) | Documenti Microsoft
author: wenz
description: Il controllo ReorderList AJAX Control Toolkit fornisce un elenco che può essere riordinato dall'utente tramite trascinamento della selezione. L'ordine dell'elenco è...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6350ee8e-11d6-4aff-b51c-942878014835
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 42464d10f119e0ba51d5eebf2a67e76e3e419bda
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873456"
---
<a name="drag-and-drop-via-reorderlist-c"></a>Trascinamento della selezione tramite ReorderList (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)

> Il controllo ReorderList AJAX Control Toolkit fornisce un elenco che può essere riordinato dall'utente tramite trascinamento della selezione. L'ordine dell'elenco deve essere resa persistente nel server.


## <a name="overview"></a>Panoramica

Il `ReorderList` controllo AJAX Control Toolkit fornisce un elenco che può essere riordinato dall'utente tramite trascinamento della selezione. L'ordine dell'elenco deve essere resa persistente nel server.

## <a name="steps"></a>Passaggi

Il `ReorderList` controllo supporta l'associazione di dati da un database all'elenco. Soprattutto, supporta anche la scrittura delle modifiche all'ordine l'elemento dell'elenco all'archivio dati.

Questo esempio si utilizza Microsoft SQL Server 2005 Express Edition come archivio dati. Il database è una parte facoltativa (e disponibile) di un'installazione di Visual Studio, inclusa l'edizione express. È inoltre disponibile come download separato sotto [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è stato chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è l'impostazione predefinita. Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.

Il modo più semplice per configurare il database consiste nell'utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ). Connettersi al server, fare doppio clic su `Databases` e creare un nuovo database (pulsante destro del mouse e scegliere `New Database`) denominato `Tutorials`.

In questo database, creare una nuova tabella denominata `AJAX` con le quattro colonne seguenti:

- `id` (chiave, integer primario, identità, non NULL)
- `char` (char (1), NULL)
- `description` (varchar (50), NULL)
- `position` (int, NULL)


[![Il layout della tabella AJAX](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)

Il layout della tabella AJAX ([fare clic per visualizzare l'immagine ingrandita](drag-and-drop-via-reorderlist-cs/_static/image3.png))


Compilare la tabella con una coppia di valori. Si noti che il `position` colonna contiene l'ordinamento degli elementi.


[![I dati iniziali della tabella AJAX](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)

I dati iniziali della tabella AJAX ([fare clic per visualizzare l'immagine ingrandita](drag-and-drop-via-reorderlist-cs/_static/image6.png))


Il passaggio successivo è necessario per generare un `SqlDataSource` controllo per comunicare con il nuovo database e la relativa tabella. L'origine dati deve supportare il `SELECT` e `UPDATE` comandi SQL. Quando viene modificata in seguito l'ordine degli elementi dell'elenco, il `ReorderList` controllo Invia automaticamente i due valori all'origine dei dati `Update` comando: la nuova posizione e l'ID dell'elemento. Pertanto, l'origine dati deve un `<UpdateParameters>` sezione per questi due valori:

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample1.aspx)]

Il `ReorderList` controllo deve impostare gli attributi seguenti:

- `AllowReorder`: Se gli elementi dell'elenco possono essere modificati
- `DataSourceID`: L'ID dell'origine dati
- `DataKeyField`: Il nome della colonna chiave primaria nell'origine dati
- `SortOrderField`: La colonna di origine dati che fornisce l'ordinamento per gli elementi dell'elenco

Nel `<DragHandleTemplate>` e `<ItemTemplate>` sezioni, il layout dell'elenco è possibile ottimizzare. Inoltre, l'associazione dati è possibile utilizzare il `Eval()` (metodo), come illustrato di seguito:

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample2.aspx)]

Le informazioni di stile CSS riportato di seguito (cui fa riferimento il `<DragHandleTemplate>` sezione del `ReorderList` controllo) consente di verificare che il puntatore del mouse cambia in modo appropriato quando viene posizionato sul quadratino di trascinamento:

[!code-css[Main](drag-and-drop-via-reorderlist-cs/samples/sample3.css)]

Infine, un `ScriptManager` controllo Inizializza ASP.NET AJAX per la pagina:

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample4.aspx)]

Eseguire questo esempio nel browser e ridisporre gli elementi dell'elenco a un bit. Quindi, ricaricare la pagina e/o esaminare il database. Le posizioni modificate sia state mantenute e vengono applicate anche in base ai valori di `position` colonna nel database e che tutto senza alcun codice, solo tramite markup.


[![I dati delle modifiche del database in base al nuovo ordine di elemento di elenco](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)

I dati delle modifiche del database in base all'elenco nuovo elemento ordine ([fare clic per visualizzare l'immagine ingrandita](drag-and-drop-via-reorderlist-cs/_static/image9.png))

> [!div class="step-by-step"]
> [Precedente](using-postbacks-with-reorderlist-cs.md)
> [Successivo](using-postbacks-with-reorderlist-vb.md)
