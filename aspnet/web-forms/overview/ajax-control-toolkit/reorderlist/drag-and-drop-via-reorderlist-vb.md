---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: Trascinamento della selezione tramite ReorderList (VB) | Microsoft Docs
author: wenz
description: /Data-Access/tutorials/Master-Detail-Using-a-Bulleted-List-of-master-Records-with-a-Details-DataList-vb
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: d8b10ecd4c061082aba5cefe9d92b8616e9f40e6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389163"
---
<a name="drag-and-drop-via-reorderlist-vb"></a><span data-ttu-id="f5ddf-103">Operazione di trascinamento della selezione tramite ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="f5ddf-103">Drag and Drop via ReorderList (VB)</span></span>
====================
<span data-ttu-id="f5ddf-104">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f5ddf-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f5ddf-105">[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f5ddf-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span></span>

> <span data-ttu-id="f5ddf-106">Il controllo ReorderList in AJAX Control Toolkit fornisce un elenco che possa essere riordinato in base all'utente tramite trascinamento della selezione.</span><span class="sxs-lookup"><span data-stu-id="f5ddf-106">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="f5ddf-107">L'ordine corrente dell'elenco dovrà essere resi persistenti nel server.</span><span class="sxs-lookup"><span data-stu-id="f5ddf-107">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="f5ddf-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="f5ddf-108">Overview</span></span>

<span data-ttu-id="f5ddf-109">Il `ReorderList` controllo in AJAX Control Toolkit fornisce un elenco che possa essere riordinato in base all'utente tramite trascinamento della selezione.</span><span class="sxs-lookup"><span data-stu-id="f5ddf-109">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="f5ddf-110">L'ordine corrente dell'elenco dovrà essere resi persistenti nel server.</span><span class="sxs-lookup"><span data-stu-id="f5ddf-110">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="f5ddf-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="f5ddf-111">Steps</span></span>

<span data-ttu-id="f5ddf-112">Il `ReorderList` controllo supporta il data binding da un database all'elenco.</span><span class="sxs-lookup"><span data-stu-id="f5ddf-112">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="f5ddf-113">Soprattutto, supporta anche la scrittura delle modifiche all'ordine l'elemento di elenco nuovamente all'archivio dati.</span><span class="sxs-lookup"><span data-stu-id="f5ddf-113">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="f5ddf-114">In questo esempio Usa Microsoft SQL Server 2005 Express Edition come archivio dati.</span><span class="sxs-lookup"><span data-stu-id="f5ddf-114">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="f5ddf-115">Il database è una parte facoltativa (e gratuita) di un'installazione di Visual Studio, compresa la express edition.</span><span class="sxs-lookup"><span data-stu-id="f5ddf-115">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="f5ddf-116">È anche disponibile come download separato sotto [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="f5ddf-116">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="f5ddf-117">In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è anche l'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="f5ddf-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="f5ddf-118">Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.</span><span class="sxs-lookup"><span data-stu-id="f5ddf-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="f5ddf-119">Il modo più semplice per configurare il database è di utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span><span class="sxs-lookup"><span data-stu-id="f5ddf-119">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="f5ddf-120">Connettersi al server, fare doppio clic su `Databases` e creare un nuovo database (pulsante destro del mouse e scegliere `New Database`) denominato `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="f5ddf-120">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="f5ddf-121">In questo database, creare una nuova tabella denominata `AJAX` con le quattro colonne seguenti:</span><span class="sxs-lookup"><span data-stu-id="f5ddf-121">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="f5ddf-122">`id` (integer chiave, primario, identità, non NULL)</span><span class="sxs-lookup"><span data-stu-id="f5ddf-122">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="f5ddf-123">`char` (char (1), NULL)</span><span class="sxs-lookup"><span data-stu-id="f5ddf-123">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="f5ddf-124">`description` (varchar (50), NULL)</span><span class="sxs-lookup"><span data-stu-id="f5ddf-124">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="f5ddf-125">`position` (int, valore NULL)</span><span class="sxs-lookup"><span data-stu-id="f5ddf-125">`position` (int, NULL)</span></span>


<span data-ttu-id="f5ddf-126">[![Il layout della tabella di AJAX](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f5ddf-126">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="f5ddf-127">Il layout della tabella AJAX ([fare clic per visualizzare l'immagine con dimensioni normali](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f5ddf-127">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span></span>


<span data-ttu-id="f5ddf-128">Successivamente, compilare la tabella con un paio di valori.</span><span class="sxs-lookup"><span data-stu-id="f5ddf-128">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="f5ddf-129">Si noti che il `position` colonna contiene l'ordinamento degli elementi.</span><span class="sxs-lookup"><span data-stu-id="f5ddf-129">Note that the `position` column holds the sort order of the elements.</span></span>


<span data-ttu-id="f5ddf-130">[![I dati iniziali della tabella di AJAX](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="f5ddf-130">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span></span>

<span data-ttu-id="f5ddf-131">I dati iniziali della tabella di AJAX ([fare clic per visualizzare l'immagine con dimensioni normali](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="f5ddf-131">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span></span>


<span data-ttu-id="f5ddf-132">Il passaggio successivo è necessario per generare un `SqlDataSource` controllo per comunicare con il nuovo database e la relativa tabella.</span><span class="sxs-lookup"><span data-stu-id="f5ddf-132">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="f5ddf-133">L'origine dati deve supportare le `SELECT` e `UPDATE` comandi SQL.</span><span class="sxs-lookup"><span data-stu-id="f5ddf-133">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="f5ddf-134">Quando viene modificata in seguito l'ordine degli elementi dell'elenco, il `ReorderList` controllo Invia automaticamente i due valori dell'origine dati `Update` comando: la nuova posizione e l'ID dell'elemento.</span><span class="sxs-lookup"><span data-stu-id="f5ddf-134">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="f5ddf-135">Pertanto, l'origine dati esigenze un `<UpdateParameters>` sezione per questi due valori:</span><span class="sxs-lookup"><span data-stu-id="f5ddf-135">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="f5ddf-136">Il `ReorderList` controllo deve impostare gli attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f5ddf-136">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="f5ddf-137">`AllowReorder`: Se gli elementi dell'elenco possono essere riorganizzati</span><span class="sxs-lookup"><span data-stu-id="f5ddf-137">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="f5ddf-138">`DataSourceID`: L'ID dell'origine dati</span><span class="sxs-lookup"><span data-stu-id="f5ddf-138">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="f5ddf-139">`DataKeyField`: Il nome della colonna chiave primaria nell'origine dati</span><span class="sxs-lookup"><span data-stu-id="f5ddf-139">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="f5ddf-140">`SortOrderField`: La colonna di origine dati che fornisce l'ordinamento per gli elementi dell'elenco</span><span class="sxs-lookup"><span data-stu-id="f5ddf-140">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="f5ddf-141">Nel `<DragHandleTemplate>` e `<ItemTemplate>` sezioni, il layout dell'elenco è possibile ottimizzare.</span><span class="sxs-lookup"><span data-stu-id="f5ddf-141">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="f5ddf-142">Inoltre, l'associazione dati è possibile utilizzando il `Eval()` metodo, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="f5ddf-142">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="f5ddf-143">Le informazioni di stile CSS riportato di seguito (indicate nel `<DragHandleTemplate>` sezione del `ReorderList` controllo) garantisce che il puntatore del mouse cambia in modo appropriato quando posiziona il mouse sul quadratino di trascinamento:</span><span class="sxs-lookup"><span data-stu-id="f5ddf-143">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

<span data-ttu-id="f5ddf-144">Infine, un `ScriptManager` controllo Inizializza ASP.NET AJAX per la pagina:</span><span class="sxs-lookup"><span data-stu-id="f5ddf-144">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="f5ddf-145">Eseguire questo esempio nel browser e ridisporre un po' gli elementi dell'elenco.</span><span class="sxs-lookup"><span data-stu-id="f5ddf-145">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="f5ddf-146">Quindi, ricaricare la pagina e/o esaminare il database.</span><span class="sxs-lookup"><span data-stu-id="f5ddf-146">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="f5ddf-147">Le posizioni modificate sono state mantenute e si riflettono anche in base ai valori di `position` colonna nel database e che tutto senza alcun codice, con un semplice utilizzo dei markup.</span><span class="sxs-lookup"><span data-stu-id="f5ddf-147">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


<span data-ttu-id="f5ddf-148">[![I dati delle modifiche del database in base al nuovo ordine di elemento di elenco](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="f5ddf-148">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span></span>

<span data-ttu-id="f5ddf-149">I dati delle modifiche del database in base al nuovo elenco di elementi ordine ([fare clic per visualizzare l'immagine con dimensioni normali](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="f5ddf-149">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f5ddf-150">Precedente</span><span class="sxs-lookup"><span data-stu-id="f5ddf-150">Previous</span></span>](using-postbacks-with-reorderlist-vb.md)
