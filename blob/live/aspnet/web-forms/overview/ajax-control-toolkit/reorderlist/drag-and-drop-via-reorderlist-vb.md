---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: Trascinamento della selezione tramite ReorderList (VB) | Documenti Microsoft
author: wenz
description: /Data-Access/tutorials/Master-Detail-Using-a-Bulleted-List-of-master-Records-with-a-Details-DataList-vb
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: e193a31fc86b7e8733d0b2fba371d99c62783d6c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="drag-and-drop-via-reorderlist-vb"></a><span data-ttu-id="9048b-103">Trascinamento della selezione tramite ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="9048b-103">Drag and Drop via ReorderList (VB)</span></span>
====================
<span data-ttu-id="9048b-104">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9048b-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9048b-105">[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="9048b-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span></span>

> <span data-ttu-id="9048b-106">Il controllo ReorderList AJAX Control Toolkit fornisce un elenco che può essere riordinato dall'utente tramite trascinamento della selezione.</span><span class="sxs-lookup"><span data-stu-id="9048b-106">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="9048b-107">L'ordine dell'elenco deve essere resa persistente nel server.</span><span class="sxs-lookup"><span data-stu-id="9048b-107">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="9048b-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="9048b-108">Overview</span></span>

<span data-ttu-id="9048b-109">Il `ReorderList` controllo AJAX Control Toolkit fornisce un elenco che può essere riordinato dall'utente tramite trascinamento della selezione.</span><span class="sxs-lookup"><span data-stu-id="9048b-109">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="9048b-110">L'ordine dell'elenco deve essere resa persistente nel server.</span><span class="sxs-lookup"><span data-stu-id="9048b-110">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="9048b-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="9048b-111">Steps</span></span>

<span data-ttu-id="9048b-112">Il `ReorderList` controllo supporta l'associazione di dati da un database all'elenco.</span><span class="sxs-lookup"><span data-stu-id="9048b-112">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="9048b-113">Soprattutto, supporta anche la scrittura delle modifiche all'ordine l'elemento dell'elenco all'archivio dati.</span><span class="sxs-lookup"><span data-stu-id="9048b-113">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="9048b-114">Questo esempio si utilizza Microsoft SQL Server 2005 Express Edition come archivio dati.</span><span class="sxs-lookup"><span data-stu-id="9048b-114">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="9048b-115">Il database è una parte facoltativa (e disponibile) di un'installazione di Visual Studio, inclusa l'edizione express.</span><span class="sxs-lookup"><span data-stu-id="9048b-115">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="9048b-116">È inoltre disponibile come download separato in [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="9048b-116">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="9048b-117">In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è stato chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è l'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="9048b-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="9048b-118">Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.</span><span class="sxs-lookup"><span data-stu-id="9048b-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="9048b-119">Il modo più semplice per configurare il database è di utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span><span class="sxs-lookup"><span data-stu-id="9048b-119">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="9048b-120">Connettersi al server, fare doppio clic su `Databases` e creare un nuovo database (pulsante destro del mouse e scegliere `New Database`) denominato `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="9048b-120">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="9048b-121">In questo database, creare una nuova tabella denominata `AJAX` con le quattro colonne seguenti:</span><span class="sxs-lookup"><span data-stu-id="9048b-121">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="9048b-122">`id`(chiave, integer primario, identità, non NULL)</span><span class="sxs-lookup"><span data-stu-id="9048b-122">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="9048b-123">`char`(char (1), NULL)</span><span class="sxs-lookup"><span data-stu-id="9048b-123">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="9048b-124">`description`(varchar (50), NULL)</span><span class="sxs-lookup"><span data-stu-id="9048b-124">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="9048b-125">`position`(int, NULL)</span><span class="sxs-lookup"><span data-stu-id="9048b-125">`position` (int, NULL)</span></span>


<span data-ttu-id="9048b-126">[![Il layout della tabella AJAX](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9048b-126">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="9048b-127">Il layout della tabella AJAX ([fare clic per visualizzare l'immagine ingrandita](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9048b-127">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span></span>


<span data-ttu-id="9048b-128">Compilare la tabella con una coppia di valori.</span><span class="sxs-lookup"><span data-stu-id="9048b-128">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="9048b-129">Si noti che il `position` colonna contiene l'ordinamento degli elementi.</span><span class="sxs-lookup"><span data-stu-id="9048b-129">Note that the `position` column holds the sort order of the elements.</span></span>


<span data-ttu-id="9048b-130">[![I dati iniziali della tabella di AJAX](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="9048b-130">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span></span>

<span data-ttu-id="9048b-131">I dati iniziali della tabella AJAX ([fare clic per visualizzare l'immagine ingrandita](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="9048b-131">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span></span>


<span data-ttu-id="9048b-132">Il passaggio successivo è necessario per generare un `SqlDataSource` controllo per comunicare con il nuovo database e la relativa tabella.</span><span class="sxs-lookup"><span data-stu-id="9048b-132">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="9048b-133">L'origine dati deve supportare il `SELECT` e `UPDATE` comandi SQL.</span><span class="sxs-lookup"><span data-stu-id="9048b-133">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="9048b-134">Quando viene modificata in seguito l'ordine degli elementi dell'elenco, il `ReorderList` controllo Invia automaticamente i due valori all'origine dei dati `Update` comando: la nuova posizione e l'ID dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="9048b-134">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="9048b-135">Pertanto, l'origine dati deve un `<UpdateParameters>` sezione per questi due valori:</span><span class="sxs-lookup"><span data-stu-id="9048b-135">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="9048b-136">Il `ReorderList` controllo deve impostare gli attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="9048b-136">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="9048b-137">`AllowReorder`: Se gli elementi dell'elenco possono essere modificati</span><span class="sxs-lookup"><span data-stu-id="9048b-137">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="9048b-138">`DataSourceID`: L'ID dell'origine dati</span><span class="sxs-lookup"><span data-stu-id="9048b-138">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="9048b-139">`DataKeyField`: Il nome della colonna chiave primaria nell'origine dati</span><span class="sxs-lookup"><span data-stu-id="9048b-139">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="9048b-140">`SortOrderField`: La colonna di origine dati che fornisce l'ordinamento per gli elementi dell'elenco</span><span class="sxs-lookup"><span data-stu-id="9048b-140">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="9048b-141">Nel `<DragHandleTemplate>` e `<ItemTemplate>` sezioni, il layout dell'elenco è possibile ottimizzare.</span><span class="sxs-lookup"><span data-stu-id="9048b-141">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="9048b-142">Inoltre, l'associazione dati è possibile utilizzare il `Eval()` (metodo), come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="9048b-142">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="9048b-143">Le informazioni di stile CSS riportato di seguito (cui fa riferimento il `<DragHandleTemplate>` sezione del `ReorderList` controllo) consente di verificare che il puntatore del mouse cambia in modo appropriato quando viene posizionato sul quadratino di trascinamento:</span><span class="sxs-lookup"><span data-stu-id="9048b-143">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

<span data-ttu-id="9048b-144">Infine, un `ScriptManager` controllo Inizializza ASP.NET AJAX per la pagina:</span><span class="sxs-lookup"><span data-stu-id="9048b-144">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="9048b-145">Eseguire questo esempio nel browser e ridisporre gli elementi dell'elenco a un bit.</span><span class="sxs-lookup"><span data-stu-id="9048b-145">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="9048b-146">Quindi, ricaricare la pagina e/o esaminare il database.</span><span class="sxs-lookup"><span data-stu-id="9048b-146">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="9048b-147">Le posizioni modificate sia state mantenute e vengono applicate anche in base ai valori di `position` colonna nel database e che tutto senza alcun codice, solo tramite markup.</span><span class="sxs-lookup"><span data-stu-id="9048b-147">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


<span data-ttu-id="9048b-148">[![I dati delle modifiche del database in base all'ordine di elemento di elenco nuovo](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="9048b-148">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span></span>

<span data-ttu-id="9048b-149">I dati delle modifiche del database in base all'elenco nuovo elemento ordine ([fare clic per visualizzare l'immagine ingrandita](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="9048b-149">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="9048b-150">Precedente</span><span class="sxs-lookup"><span data-stu-id="9048b-150">Previous</span></span>](using-postbacks-with-reorderlist-vb.md)
