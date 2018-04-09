---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: Utilizzo di postback con ReorderList (VB) | Documenti Microsoft
author: wenz
description: Il controllo ReorderList AJAX Control Toolkit fornisce un elenco che può essere riordinato dall'utente tramite trascinamento della selezione. Ogni volta che l'elenco viene riordinato, un ordine di acquisto...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: ef43471f7d8cc94c1a82a368e27acef05f474f81
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="using-postbacks-with-reorderlist-vb"></a><span data-ttu-id="e09ce-104">Utilizzo di postback con ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="e09ce-104">Using Postbacks with ReorderList (VB)</span></span>
====================
<span data-ttu-id="e09ce-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e09ce-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e09ce-106">[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="e09ce-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span></span>

> <span data-ttu-id="e09ce-107">Il controllo ReorderList AJAX Control Toolkit fornisce un elenco che può essere riordinato dall'utente tramite trascinamento della selezione.</span><span class="sxs-lookup"><span data-stu-id="e09ce-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="e09ce-108">Ogni volta che l'elenco viene riordinato, un postback informa il server della modifica.</span><span class="sxs-lookup"><span data-stu-id="e09ce-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>


## <a name="overview"></a><span data-ttu-id="e09ce-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="e09ce-109">Overview</span></span>

<span data-ttu-id="e09ce-110">Il `ReorderList` controllo AJAX Control Toolkit fornisce un elenco che può essere riordinato dall'utente tramite trascinamento della selezione.</span><span class="sxs-lookup"><span data-stu-id="e09ce-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="e09ce-111">Ogni volta che l'elenco viene riordinato, un postback informa il server della modifica.</span><span class="sxs-lookup"><span data-stu-id="e09ce-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="e09ce-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="e09ce-112">Steps</span></span>

<span data-ttu-id="e09ce-113">Esistono più possibili origini dati per il `ReorderList` controllo.</span><span class="sxs-lookup"><span data-stu-id="e09ce-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="e09ce-114">Uno consiste nell'utilizzare un `XmlDataSource` controllo:</span><span class="sxs-lookup"><span data-stu-id="e09ce-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="e09ce-115">Per associare il codice XML per un `ReorderList` impostare controllo e abilitare i postback, gli attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="e09ce-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- <span data-ttu-id="e09ce-116">`DataSourceID`: L'ID dell'origine dati</span><span class="sxs-lookup"><span data-stu-id="e09ce-116">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="e09ce-117">`SortOrderField`: La proprietà di ordinamento</span><span class="sxs-lookup"><span data-stu-id="e09ce-117">`SortOrderField`: The property to sort by</span></span>
- <span data-ttu-id="e09ce-118">`AllowReorder`: Se si desidera consentire all'utente di riordinare gli elementi dell'elenco</span><span class="sxs-lookup"><span data-stu-id="e09ce-118">`AllowReorder`: Whether to allow the user to reorder the list elements</span></span>
- <span data-ttu-id="e09ce-119">`PostBackOnReorder`: Se si desidera creare un postback ogni volta che l'elenco viene riordinato</span><span class="sxs-lookup"><span data-stu-id="e09ce-119">`PostBackOnReorder`: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="e09ce-120">Di seguito è riportato il codice appropriato per il controllo:</span><span class="sxs-lookup"><span data-stu-id="e09ce-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="e09ce-121">All'interno di `ReorderList` controllo, i dati specifici dell'origine dati può essere associato usando il `Eval()` metodo:</span><span class="sxs-lookup"><span data-stu-id="e09ce-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

<span data-ttu-id="e09ce-122">In una posizione arbitraria nella pagina, un'etichetta conterrà le informazioni quando si è verificato il riordino ultimo:</span><span class="sxs-lookup"><span data-stu-id="e09ce-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="e09ce-123">Questa etichetta viene riempita con il testo nel codice sul lato server, la gestione del postback:</span><span class="sxs-lookup"><span data-stu-id="e09ce-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

<span data-ttu-id="e09ce-124">Infine, per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito nella pagina:</span><span class="sxs-lookup"><span data-stu-id="e09ce-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]


<span data-ttu-id="e09ce-125">[![Ogni riordinamento attiva un postback](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e09ce-125">[![Each reordering triggers a postback](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="e09ce-126">Ogni tipo di riordinamento attiva un postback ([fare clic per visualizzare l'immagine ingrandita](using-postbacks-with-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e09ce-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e09ce-127">[Precedente](drag-and-drop-via-reorderlist-cs.md)
> [Successivo](drag-and-drop-via-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e09ce-127">[Previous](drag-and-drop-via-reorderlist-cs.md)
[Next](drag-and-drop-via-reorderlist-vb.md)</span></span>
