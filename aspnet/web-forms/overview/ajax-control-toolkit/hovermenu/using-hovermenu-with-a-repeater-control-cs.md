---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
title: Uso di HoverMenu con un controllo Repeater (c#) | Microsoft Docs
author: wenz
description: 'Il controllo di HoverMenu in AJAX Control Toolkit fornisce un effetto popup semplice: quando il puntatore del mouse viene posizionato su un elemento, viene visualizzata un finestra popup a dalla specifica...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e7700e7b-edc3-4183-a713-70e507cc7490
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 33f65a132ea9f0a939ab70ac249b397ea883ac8c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835304"
---
<a name="using-hovermenu-with-a-repeater-control-c"></a><span data-ttu-id="d713f-103">Uso di HoverMenu con un controllo Repeater (c#)</span><span class="sxs-lookup"><span data-stu-id="d713f-103">Using HoverMenu with a Repeater Control (C#)</span></span>
====================
<span data-ttu-id="d713f-104">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d713f-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d713f-105">[Scaricare il codice](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d713f-105">[Download Code](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1CS.pdf)</span></span>

> <span data-ttu-id="d713f-106">Il controllo di HoverMenu in AJAX Control Toolkit fornisce un effetto popup semplice: quando il puntatore del mouse viene posizionato su un elemento, viene visualizzata un finestra popup in una posizione specificata.</span><span class="sxs-lookup"><span data-stu-id="d713f-106">The HoverMenu control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="d713f-107">È anche possibile usare questo controllo all'interno di un controllo repeater.</span><span class="sxs-lookup"><span data-stu-id="d713f-107">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="d713f-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="d713f-108">Overview</span></span>

<span data-ttu-id="d713f-109">Il `HoverMenu` controllo in AJAX Control Toolkit fornisce un effetto popup semplice: quando il puntatore del mouse viene posizionato su un elemento, viene visualizzata un finestra popup in una posizione specificata.</span><span class="sxs-lookup"><span data-stu-id="d713f-109">The `HoverMenu` control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="d713f-110">È anche possibile usare questo controllo all'interno di un controllo repeater.</span><span class="sxs-lookup"><span data-stu-id="d713f-110">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="d713f-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="d713f-111">Steps</span></span>

<span data-ttu-id="d713f-112">Prima di tutto, è richiesta un'origine dati.</span><span class="sxs-lookup"><span data-stu-id="d713f-112">First of all, a data source is required.</span></span> <span data-ttu-id="d713f-113">Questo esempio Usa il database AdventureWorks e Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="d713f-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="d713f-114">Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa l'edizione express) ed è anche disponibile come download separato in [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="d713f-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="d713f-115">Il database AdventureWorks è parte di SQL Server 2005 Samples and Sample Databases (download all'indirizzo [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="d713f-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="d713f-116">Il modo più semplice per configurare il database è di utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e collegare il `AdventureWorks.mdf` file di database.</span><span class="sxs-lookup"><span data-stu-id="d713f-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="d713f-117">In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è anche l'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="d713f-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="d713f-118">Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.</span><span class="sxs-lookup"><span data-stu-id="d713f-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="d713f-119">Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="d713f-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample1.aspx)]

<span data-ttu-id="d713f-120">Quindi, aggiungere un'origine dati alla pagina.</span><span class="sxs-lookup"><span data-stu-id="d713f-120">Then, add a data source to the page.</span></span> <span data-ttu-id="d713f-121">Per usare una quantità limitata di dati, selezionare solo le prime cinque voci della tabella Vendor del database AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="d713f-121">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="d713f-122">Se si usa l'Assistente per Visual Studio per creare l'origine dati, è importante che un bug nella versione corrente non prefisso il nome della tabella (`Vendor`) con `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="d713f-122">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="d713f-123">Il markup seguente mostra la sintassi corretta:</span><span class="sxs-lookup"><span data-stu-id="d713f-123">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample2.aspx)]

<span data-ttu-id="d713f-124">Successivamente, aggiungere un pannello che funge da finestra popup modale:</span><span class="sxs-lookup"><span data-stu-id="d713f-124">Next, add a panel which serves as the modal popup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample3.aspx)]

<span data-ttu-id="d713f-125">A questo punto, il `HoverMenuExtender` entra in gioco.</span><span class="sxs-lookup"><span data-stu-id="d713f-125">Now, the `HoverMenuExtender` comes into play.</span></span> <span data-ttu-id="d713f-126">In modo che ogni elemento nell'origine dati Ottiene il proprio controllo popup, il programma di estensione deve essere inserita all'interno di repeater `<ItemTemplate>` sezione.</span><span class="sxs-lookup"><span data-stu-id="d713f-126">So that every element in the data source gets its own popup, the extender must be put within the repeater's `<ItemTemplate>` section.</span></span> <span data-ttu-id="d713f-127">Questo è il markup:</span><span class="sxs-lookup"><span data-stu-id="d713f-127">Here is the markup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample4.aspx)]

<span data-ttu-id="d713f-128">A questo punto ogni elemento nell'origine dati consente di visualizzare una finestra popup a destra (`PopupPosition` attributo) dopo un ritardo di 50 millisecondi (`PopDelay` attributo).</span><span class="sxs-lookup"><span data-stu-id="d713f-128">Now every item in the data source displays a popup to the right (`PopupPosition` attribute) after a delay of 50 milliseconds (`PopDelay` attribute).</span></span>


<span data-ttu-id="d713f-129">[![Viene visualizzato il menu di passaggio del mouse accanto a ogni elemento nel repeater](using-hovermenu-with-a-repeater-control-cs/_static/image2.png)](using-hovermenu-with-a-repeater-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d713f-129">[![The hover menu appears next to each item in the repeater](using-hovermenu-with-a-repeater-control-cs/_static/image2.png)](using-hovermenu-with-a-repeater-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="d713f-130">Viene visualizzato il menu di passaggio del mouse accanto a ogni elemento nel repeater ([fare clic per visualizzare l'immagine con dimensioni normali](using-hovermenu-with-a-repeater-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d713f-130">The hover menu appears next to each item in the repeater ([Click to view full-size image](using-hovermenu-with-a-repeater-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d713f-131">avanti</span><span class="sxs-lookup"><span data-stu-id="d713f-131">Next</span></span>](using-hovermenu-with-a-repeater-control-vb.md)
