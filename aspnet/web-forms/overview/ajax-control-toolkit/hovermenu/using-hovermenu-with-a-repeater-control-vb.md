---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
title: Utilizzo di HoverMenu con un controllo Repeater (VB) | Documenti Microsoft
author: wenz
description: 'Il controllo HoverMenu AJAX Control Toolkit fornisce un effetto popup semplice: quando il puntatore del mouse viene posizionato su un elemento, una finestra popup viene visualizzato dalla specifica...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7f07c112-cd4f-4427-9699-57cfab2791fd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: aa107a7483683d965f3a510e6a43f174a386da0c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="using-hovermenu-with-a-repeater-control-vb"></a><span data-ttu-id="02383-103">Utilizzo di HoverMenu con un controllo Repeater (VB)</span><span class="sxs-lookup"><span data-stu-id="02383-103">Using HoverMenu with a Repeater Control (VB)</span></span>
====================
<span data-ttu-id="02383-104">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="02383-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="02383-105">[Scaricare codice](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="02383-105">[Download Code](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)</span></span>

> <span data-ttu-id="02383-106">Il controllo HoverMenu AJAX Control Toolkit fornisce un effetto popup semplice: quando il puntatore del mouse viene posizionato su un elemento, una finestra popup viene visualizzato in una posizione specificata.</span><span class="sxs-lookup"><span data-stu-id="02383-106">The HoverMenu control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="02383-107">È inoltre possibile utilizzare il controllo all'interno di un controllo repeater.</span><span class="sxs-lookup"><span data-stu-id="02383-107">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="02383-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="02383-108">Overview</span></span>

<span data-ttu-id="02383-109">Il `HoverMenu` controllo AJAX Control Toolkit fornisce un effetto popup semplice: quando il puntatore del mouse viene posizionato su un elemento, una finestra popup viene visualizzato in una posizione specificata.</span><span class="sxs-lookup"><span data-stu-id="02383-109">The `HoverMenu` control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="02383-110">È inoltre possibile utilizzare il controllo all'interno di un controllo repeater.</span><span class="sxs-lookup"><span data-stu-id="02383-110">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="02383-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="02383-111">Steps</span></span>

<span data-ttu-id="02383-112">Prima di tutto, un'origine dati è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="02383-112">First of all, a data source is required.</span></span> <span data-ttu-id="02383-113">Questo esempio viene utilizzato il database AdventureWorks e Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="02383-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="02383-114">Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa express edition) ed è anche disponibile come download separato sotto [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="02383-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="02383-115">Il database AdventureWorks fa parte di database di esempio e gli esempi di SQL Server 2005 (download all'indirizzo [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="02383-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="02383-116">Il modo più semplice per configurare il database consiste nell'utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e collegare il `AdventureWorks.mdf` file di database.</span><span class="sxs-lookup"><span data-stu-id="02383-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="02383-117">In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è stato chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è l'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="02383-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="02383-118">Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.</span><span class="sxs-lookup"><span data-stu-id="02383-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="02383-119">Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="02383-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample1.aspx)]

<span data-ttu-id="02383-120">Quindi, aggiungere un'origine dati per la pagina.</span><span class="sxs-lookup"><span data-stu-id="02383-120">Then, add a data source to the page.</span></span> <span data-ttu-id="02383-121">Per utilizzare una quantità limitata di dati, selezionare solo le prime cinque voci della tabella Vendor del database AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="02383-121">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="02383-122">Se si utilizza l'Assistente per Visual Studio per creare l'origine dati, è importante che un bug nella versione corrente come prefisso il nome della tabella (`Vendor`) con `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="02383-122">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="02383-123">Il markup seguente viene illustrata la sintassi corretta:</span><span class="sxs-lookup"><span data-stu-id="02383-123">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample2.aspx)]

<span data-ttu-id="02383-124">Successivamente, aggiungere un pannello che funge da finestra popup modale:</span><span class="sxs-lookup"><span data-stu-id="02383-124">Next, add a panel which serves as the modal popup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample3.aspx)]

<span data-ttu-id="02383-125">A questo punto, il `HoverMenuExtender` entra in gioco.</span><span class="sxs-lookup"><span data-stu-id="02383-125">Now, the `HoverMenuExtender` comes into play.</span></span> <span data-ttu-id="02383-126">In modo che ogni elemento nell'origine dati Ottiene il proprio popup, l'estensione deve essere inserito all'interno del ripetitore `<ItemTemplate>` sezione.</span><span class="sxs-lookup"><span data-stu-id="02383-126">So that every element in the data source gets its own popup, the extender must be put within the repeater's `<ItemTemplate>` section.</span></span> <span data-ttu-id="02383-127">Questo è il markup:</span><span class="sxs-lookup"><span data-stu-id="02383-127">Here is the markup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample4.aspx)]

<span data-ttu-id="02383-128">Ora ogni elemento nell'origine dati consente di visualizzare una finestra popup a destra (`PopupPosition` attributo) dopo un ritardo di 50 millisecondi (`PopDelay` attributo).</span><span class="sxs-lookup"><span data-stu-id="02383-128">Now every item in the data source displays a popup to the right (`PopupPosition` attribute) after a delay of 50 milliseconds (`PopDelay` attribute).</span></span>


<span data-ttu-id="02383-129">[![Viene visualizzato il menu di passaggio del mouse accanto a ogni elemento nel repeater](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="02383-129">[![The hover menu appears next to each item in the repeater](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="02383-130">Viene visualizzato il menu di passaggio del mouse accanto a ogni elemento nel repeater ([fare clic per visualizzare l'immagine ingrandita](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="02383-130">The hover menu appears next to each item in the repeater ([Click to view full-size image](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="02383-131">Precedente</span><span class="sxs-lookup"><span data-stu-id="02383-131">Previous</span></span>](using-hovermenu-with-a-repeater-control-cs.md)
