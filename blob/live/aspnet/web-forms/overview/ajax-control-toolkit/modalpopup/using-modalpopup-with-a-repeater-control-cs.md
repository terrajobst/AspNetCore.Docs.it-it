---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
title: Utilizzo ModalPopup con un controllo Repeater (c#) | Documenti Microsoft
author: wenz
description: "Il controllo ModalPopup AJAX Control Toolkit offre un modo semplice per creare un popup modale tramite mezzi sul lato client. È anche possibile usare questo Contr...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d686d84a-1c58-492e-8a77-3eb5a0cfe918
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: d945b8decf4debbcbf415163e486e2bce8097701
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="using-modalpopup-with-a-repeater-control-c"></a><span data-ttu-id="0eebc-104">Utilizzo ModalPopup con un controllo Repeater (c#)</span><span class="sxs-lookup"><span data-stu-id="0eebc-104">Using ModalPopup with a Repeater Control (C#)</span></span>
====================
<span data-ttu-id="0eebc-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0eebc-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0eebc-106">[Scaricare codice](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="0eebc-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)</span></span>

> <span data-ttu-id="0eebc-107">Il controllo ModalPopup AJAX Control Toolkit offre un modo semplice per creare un popup modale tramite mezzi sul lato client.</span><span class="sxs-lookup"><span data-stu-id="0eebc-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="0eebc-108">È inoltre possibile utilizzare il controllo all'interno di un controllo repeater.</span><span class="sxs-lookup"><span data-stu-id="0eebc-108">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="0eebc-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="0eebc-109">Overview</span></span>

<span data-ttu-id="0eebc-110">Il controllo ModalPopup AJAX Control Toolkit offre un modo semplice per creare un popup modale tramite mezzi sul lato client.</span><span class="sxs-lookup"><span data-stu-id="0eebc-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="0eebc-111">È inoltre possibile utilizzare il controllo all'interno di un controllo repeater.</span><span class="sxs-lookup"><span data-stu-id="0eebc-111">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="0eebc-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="0eebc-112">Steps</span></span>

<span data-ttu-id="0eebc-113">Prima di tutto, un'origine dati è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="0eebc-113">First of all, a data source is required.</span></span> <span data-ttu-id="0eebc-114">Questo esempio viene utilizzato il database AdventureWorks e Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="0eebc-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="0eebc-115">Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa express edition) ed è anche disponibile come download separato in [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="0eebc-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="0eebc-116">Il database AdventureWorks fa parte dei database di esempio e gli esempi di SQL Server 2005 (download all'indirizzo [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="0eebc-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="0eebc-117">Il modo più semplice per configurare il database è di utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e collegare il `AdventureWorks.mdf` file di database.</span><span class="sxs-lookup"><span data-stu-id="0eebc-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span> <span data-ttu-id="0eebc-118">In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è stato chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è l'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="0eebc-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="0eebc-119">Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.</span><span class="sxs-lookup"><span data-stu-id="0eebc-119">If your setup differs, you have to adapt the connection information for the database.</span></span> <span data-ttu-id="0eebc-120">Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="0eebc-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample1.aspx)]

<span data-ttu-id="0eebc-121">Quindi, aggiungere un'origine dati per la pagina.</span><span class="sxs-lookup"><span data-stu-id="0eebc-121">Then, add a data source to the page.</span></span> <span data-ttu-id="0eebc-122">Per utilizzare una quantità limitata di dati, selezionare solo le prime cinque voci della tabella Vendor del database AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="0eebc-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="0eebc-123">Se si utilizza l'Assistente per Visual Studio per creare l'origine dati, è importante che un bug nella versione corrente come prefisso il nome della tabella (`Vendor`) con `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="0eebc-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="0eebc-124">Il markup seguente viene illustrata la sintassi corretta:</span><span class="sxs-lookup"><span data-stu-id="0eebc-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample2.aspx)]

<span data-ttu-id="0eebc-125">Successivamente, aggiungere un pannello che funge da finestra popup modale.</span><span class="sxs-lookup"><span data-stu-id="0eebc-125">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="0eebc-126">Contiene un `Button` controllo a chiudere la finestra popup:</span><span class="sxs-lookup"><span data-stu-id="0eebc-126">It contains a `Button` control to close the popup again:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample3.aspx)]

<span data-ttu-id="0eebc-127">Per fare in modo che la finestra popup all'interno di repeater, il `ModalPopupExtender` controllo deve essere inserito all'interno di `<ItemTemplate>` sezione del ripetitore.</span><span class="sxs-lookup"><span data-stu-id="0eebc-127">In order to make the popup work within the repeater, the `ModalPopupExtender` control must be put within the `<ItemTemplate>` section of the repeater.</span></span> <span data-ttu-id="0eebc-128">Il pannello è di fuori di repeater, ma il programma di estensione si trova all'interno.</span><span class="sxs-lookup"><span data-stu-id="0eebc-128">So the panel is outside the repeater, but the extender is inside.</span></span> <span data-ttu-id="0eebc-129">Di seguito è riportato il markup per il controllo repeater:</span><span class="sxs-lookup"><span data-stu-id="0eebc-129">Here is the markup for the repeater:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample4.aspx)]

<span data-ttu-id="0eebc-130">Quindi, ogni elemento nell'origine dati viene visualizzato con un pulsante accanto che attiva il popup modale.</span><span class="sxs-lookup"><span data-stu-id="0eebc-130">Then, every item in the data source is displayed with a button next to it that triggers the modal popup.</span></span>


<span data-ttu-id="0eebc-131">[![Il popup modale può essere attivato per ogni voce relativa all'origine dati](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0eebc-131">[![The modal popup can be triggered for every data source entry](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="0eebc-132">Il popup modale può essere attivato per ogni voce relativa all'origine dati ([fare clic per visualizzare l'immagine ingrandita](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0eebc-132">The modal popup can be triggered for every data source entry ([Click to view full-size image](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="0eebc-133">[Precedente](launching-a-modal-popup-window-from-server-code-cs.md)
[Successivo](handling-postbacks-from-a-modalpopup-cs.md)</span><span class="sxs-lookup"><span data-stu-id="0eebc-133">[Previous](launching-a-modal-popup-window-from-server-code-cs.md)
[Next](handling-postbacks-from-a-modalpopup-cs.md)</span></span>
