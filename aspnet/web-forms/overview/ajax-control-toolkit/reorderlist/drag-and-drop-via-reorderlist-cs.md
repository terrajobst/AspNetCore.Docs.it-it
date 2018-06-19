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
<a name="drag-and-drop-via-reorderlist-c"></a><span data-ttu-id="0ea48-104">Trascinamento della selezione tramite ReorderList (c#)</span><span class="sxs-lookup"><span data-stu-id="0ea48-104">Drag and Drop via ReorderList (C#)</span></span>
====================
<span data-ttu-id="0ea48-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0ea48-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0ea48-106">[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="0ea48-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span></span>

> <span data-ttu-id="0ea48-107">Il controllo ReorderList AJAX Control Toolkit fornisce un elenco che può essere riordinato dall'utente tramite trascinamento della selezione.</span><span class="sxs-lookup"><span data-stu-id="0ea48-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="0ea48-108">L'ordine dell'elenco deve essere resa persistente nel server.</span><span class="sxs-lookup"><span data-stu-id="0ea48-108">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="0ea48-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="0ea48-109">Overview</span></span>

<span data-ttu-id="0ea48-110">Il `ReorderList` controllo AJAX Control Toolkit fornisce un elenco che può essere riordinato dall'utente tramite trascinamento della selezione.</span><span class="sxs-lookup"><span data-stu-id="0ea48-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="0ea48-111">L'ordine dell'elenco deve essere resa persistente nel server.</span><span class="sxs-lookup"><span data-stu-id="0ea48-111">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="0ea48-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="0ea48-112">Steps</span></span>

<span data-ttu-id="0ea48-113">Il `ReorderList` controllo supporta l'associazione di dati da un database all'elenco.</span><span class="sxs-lookup"><span data-stu-id="0ea48-113">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="0ea48-114">Soprattutto, supporta anche la scrittura delle modifiche all'ordine l'elemento dell'elenco all'archivio dati.</span><span class="sxs-lookup"><span data-stu-id="0ea48-114">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="0ea48-115">Questo esempio si utilizza Microsoft SQL Server 2005 Express Edition come archivio dati.</span><span class="sxs-lookup"><span data-stu-id="0ea48-115">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="0ea48-116">Il database è una parte facoltativa (e disponibile) di un'installazione di Visual Studio, inclusa l'edizione express.</span><span class="sxs-lookup"><span data-stu-id="0ea48-116">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="0ea48-117">È inoltre disponibile come download separato sotto [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="0ea48-117">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="0ea48-118">In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è stato chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è l'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="0ea48-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="0ea48-119">Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.</span><span class="sxs-lookup"><span data-stu-id="0ea48-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="0ea48-120">Il modo più semplice per configurare il database consiste nell'utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span><span class="sxs-lookup"><span data-stu-id="0ea48-120">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="0ea48-121">Connettersi al server, fare doppio clic su `Databases` e creare un nuovo database (pulsante destro del mouse e scegliere `New Database`) denominato `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="0ea48-121">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="0ea48-122">In questo database, creare una nuova tabella denominata `AJAX` con le quattro colonne seguenti:</span><span class="sxs-lookup"><span data-stu-id="0ea48-122">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="0ea48-123">`id` (chiave, integer primario, identità, non NULL)</span><span class="sxs-lookup"><span data-stu-id="0ea48-123">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="0ea48-124">`char` (char (1), NULL)</span><span class="sxs-lookup"><span data-stu-id="0ea48-124">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="0ea48-125">`description` (varchar (50), NULL)</span><span class="sxs-lookup"><span data-stu-id="0ea48-125">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="0ea48-126">`position` (int, NULL)</span><span class="sxs-lookup"><span data-stu-id="0ea48-126">`position` (int, NULL)</span></span>


<span data-ttu-id="0ea48-127">[![Il layout della tabella AJAX](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0ea48-127">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="0ea48-128">Il layout della tabella AJAX ([fare clic per visualizzare l'immagine ingrandita](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0ea48-128">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span></span>


<span data-ttu-id="0ea48-129">Compilare la tabella con una coppia di valori.</span><span class="sxs-lookup"><span data-stu-id="0ea48-129">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="0ea48-130">Si noti che il `position` colonna contiene l'ordinamento degli elementi.</span><span class="sxs-lookup"><span data-stu-id="0ea48-130">Note that the `position` column holds the sort order of the elements.</span></span>


<span data-ttu-id="0ea48-131">[![I dati iniziali della tabella AJAX](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="0ea48-131">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)</span></span>

<span data-ttu-id="0ea48-132">I dati iniziali della tabella AJAX ([fare clic per visualizzare l'immagine ingrandita](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="0ea48-132">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span></span>


<span data-ttu-id="0ea48-133">Il passaggio successivo è necessario per generare un `SqlDataSource` controllo per comunicare con il nuovo database e la relativa tabella.</span><span class="sxs-lookup"><span data-stu-id="0ea48-133">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="0ea48-134">L'origine dati deve supportare il `SELECT` e `UPDATE` comandi SQL.</span><span class="sxs-lookup"><span data-stu-id="0ea48-134">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="0ea48-135">Quando viene modificata in seguito l'ordine degli elementi dell'elenco, il `ReorderList` controllo Invia automaticamente i due valori all'origine dei dati `Update` comando: la nuova posizione e l'ID dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="0ea48-135">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="0ea48-136">Pertanto, l'origine dati deve un `<UpdateParameters>` sezione per questi due valori:</span><span class="sxs-lookup"><span data-stu-id="0ea48-136">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="0ea48-137">Il `ReorderList` controllo deve impostare gli attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="0ea48-137">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="0ea48-138">`AllowReorder`: Se gli elementi dell'elenco possono essere modificati</span><span class="sxs-lookup"><span data-stu-id="0ea48-138">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="0ea48-139">`DataSourceID`: L'ID dell'origine dati</span><span class="sxs-lookup"><span data-stu-id="0ea48-139">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="0ea48-140">`DataKeyField`: Il nome della colonna chiave primaria nell'origine dati</span><span class="sxs-lookup"><span data-stu-id="0ea48-140">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="0ea48-141">`SortOrderField`: La colonna di origine dati che fornisce l'ordinamento per gli elementi dell'elenco</span><span class="sxs-lookup"><span data-stu-id="0ea48-141">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="0ea48-142">Nel `<DragHandleTemplate>` e `<ItemTemplate>` sezioni, il layout dell'elenco è possibile ottimizzare.</span><span class="sxs-lookup"><span data-stu-id="0ea48-142">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="0ea48-143">Inoltre, l'associazione dati è possibile utilizzare il `Eval()` (metodo), come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="0ea48-143">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="0ea48-144">Le informazioni di stile CSS riportato di seguito (cui fa riferimento il `<DragHandleTemplate>` sezione del `ReorderList` controllo) consente di verificare che il puntatore del mouse cambia in modo appropriato quando viene posizionato sul quadratino di trascinamento:</span><span class="sxs-lookup"><span data-stu-id="0ea48-144">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-cs/samples/sample3.css)]

<span data-ttu-id="0ea48-145">Infine, un `ScriptManager` controllo Inizializza ASP.NET AJAX per la pagina:</span><span class="sxs-lookup"><span data-stu-id="0ea48-145">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="0ea48-146">Eseguire questo esempio nel browser e ridisporre gli elementi dell'elenco a un bit.</span><span class="sxs-lookup"><span data-stu-id="0ea48-146">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="0ea48-147">Quindi, ricaricare la pagina e/o esaminare il database.</span><span class="sxs-lookup"><span data-stu-id="0ea48-147">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="0ea48-148">Le posizioni modificate sia state mantenute e vengono applicate anche in base ai valori di `position` colonna nel database e che tutto senza alcun codice, solo tramite markup.</span><span class="sxs-lookup"><span data-stu-id="0ea48-148">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


<span data-ttu-id="0ea48-149">[![I dati delle modifiche del database in base al nuovo ordine di elemento di elenco](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="0ea48-149">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)</span></span>

<span data-ttu-id="0ea48-150">I dati delle modifiche del database in base all'elenco nuovo elemento ordine ([fare clic per visualizzare l'immagine ingrandita](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="0ea48-150">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0ea48-151">[Precedente](using-postbacks-with-reorderlist-cs.md)
> [Successivo](using-postbacks-with-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="0ea48-151">[Previous](using-postbacks-with-reorderlist-cs.md)
[Next](using-postbacks-with-reorderlist-vb.md)</span></span>
