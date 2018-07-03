---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: Uso di postback con ReorderList (c#) | Microsoft Docs
author: wenz
description: Il controllo ReorderList in AJAX Control Toolkit fornisce un elenco che possa essere riordinato in base all'utente tramite trascinamento della selezione. Ogni volta che l'elenco viene riordinato, un ordine di acquisto...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 71e806b7915c010cec66931d87bd8c1f3b6d1fb3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365633"
---
<a name="using-postbacks-with-reorderlist-c"></a>Uso di postback con ReorderList (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)

> Il controllo ReorderList in AJAX Control Toolkit fornisce un elenco che possa essere riordinato in base all'utente tramite trascinamento della selezione. Ogni volta che l'elenco viene riordinato, un postback informa il server della modifica.


## <a name="overview"></a>Panoramica

Il `ReorderList` controllo in AJAX Control Toolkit fornisce un elenco che possa essere riordinato in base all'utente tramite trascinamento della selezione. Ogni volta che l'elenco viene riordinato, un postback informa il server della modifica.

## <a name="steps"></a>Passaggi

Esistono varie fonti di dati possibili per il `ReorderList` controllo. Uno consiste nell'usare un `XmlDataSource` controllo:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

Per associare il codice XML per un `ReorderList` devono essere impostati controllo e abilitare i postback, gli attributi seguenti:

- `DataSourceID`: L'ID dell'origine dati
- `SortOrderField`: La proprietà da ordinare
- `AllowReorder`: Se si desidera consentire all'utente di riordinare gli elementi dell'elenco
- `PostBackOnReorder`: Se si desidera creare un postback ogni volta che l'elenco viene ridisposto

Ecco il markup per il controllo appropriato:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

All'interno di `ReorderList` controllo, i dati specifici dell'origine dati può essere associato usando la `Eval()` metodo:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

In una posizione arbitraria nella pagina, un'etichetta conterrà le informazioni quando si è verificato il riordinamento ultimo:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

Questa etichetta viene riempita con il testo nel codice lato server, la gestione del postback:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

Infine, in modo da attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito nella pagina:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]


[![Ogni riordinamento attiva un postback](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)

Ogni riordinamento attiva un postback ([fare clic per visualizzare l'immagine con dimensioni normali](using-postbacks-with-reorderlist-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](drag-and-drop-via-reorderlist-cs.md)
