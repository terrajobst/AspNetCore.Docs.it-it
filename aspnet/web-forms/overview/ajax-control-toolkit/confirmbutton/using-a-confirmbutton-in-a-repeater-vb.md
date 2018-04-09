---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
title: Utilizzo di un ConfirmButton In un ripetitore (VB) | Documenti Microsoft
author: wenz
description: L'estensione ConfirmButton AJAX Control Toolkit crea Sì/popup quando l'utente fa clic su un pulsante (inclusi controllo LinkButton). Solo se è Sì...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 18c31709-3f9d-4d93-8b01-f1356bf610b4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: 89f412c242a3a5bcd10b72b7f0cbfb23705edb51
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="using-a-confirmbutton-in-a-repeater-vb"></a><span data-ttu-id="09913-104">Utilizzo di un ConfirmButton In un ripetitore (VB)</span><span class="sxs-lookup"><span data-stu-id="09913-104">Using a ConfirmButton In a Repeater (VB)</span></span>
====================
<span data-ttu-id="09913-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="09913-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="09913-106">[Scaricare codice](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="09913-106">[Download Code](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)</span></span>

> <span data-ttu-id="09913-107">L'estensione ConfirmButton AJAX Control Toolkit crea Sì/popup quando l'utente fa clic su un pulsante (inclusi controllo LinkButton).</span><span class="sxs-lookup"><span data-stu-id="09913-107">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="09913-108">Solo se si sceglie Sì, azione del pulsante viene eseguito, annullato in caso contrario.</span><span class="sxs-lookup"><span data-stu-id="09913-108">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="09913-109">È anche possibile in un controllo repeater.</span><span class="sxs-lookup"><span data-stu-id="09913-109">This is also possible in a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="09913-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="09913-110">Overview</span></span>

<span data-ttu-id="09913-111">L'estensione ConfirmButton AJAX Control Toolkit crea Sì/popup quando l'utente fa clic su un pulsante (inclusi controllo LinkButton).</span><span class="sxs-lookup"><span data-stu-id="09913-111">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="09913-112">Solo se si sceglie Sì, azione del pulsante viene eseguito, annullato in caso contrario.</span><span class="sxs-lookup"><span data-stu-id="09913-112">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="09913-113">È anche possibile in un controllo repeater.</span><span class="sxs-lookup"><span data-stu-id="09913-113">This is also possible in a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="09913-114">Passaggi</span><span class="sxs-lookup"><span data-stu-id="09913-114">Steps</span></span>

<span data-ttu-id="09913-115">Prima di tutto, un'origine dati è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="09913-115">First of all, a data source is required.</span></span> <span data-ttu-id="09913-116">Questo esempio viene utilizzato il database AdventureWorks e Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="09913-116">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="09913-117">Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa express edition) ed è anche disponibile come download separato sotto [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="09913-117">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="09913-118">Il database AdventureWorks fa parte di database di esempio e gli esempi di SQL Server 2005 (download all'indirizzo [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="09913-118">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="09913-119">Il modo più semplice per configurare il database consiste nell'utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e collegare il `AdventureWorks.mdf` file di database.</span><span class="sxs-lookup"><span data-stu-id="09913-119">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="09913-120">In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è stato chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è l'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="09913-120">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="09913-121">Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.</span><span class="sxs-lookup"><span data-stu-id="09913-121">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="09913-122">Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="09913-122">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample1.aspx)]

<span data-ttu-id="09913-123">Quindi, un'origine dati è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="09913-123">Then, a data source is required.</span></span> <span data-ttu-id="09913-124">Per ragioni di semplicità, vengono recuperate solo i primi cinque voci nella tabella di fornitori di AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="09913-124">For the sake of simplicity, only the first five entries in AdventureWorks' Vendors table are retrieved.</span></span> <span data-ttu-id="09913-125">Si noti che quando si utilizza la procedura guidata di Visual Studio per creare l'origine dati, il nome della tabella (`Vendors`) è attualmente non correttamente preceduto da `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="09913-125">Note that when using the Visual Studio wizard to create the data source, the table name (`Vendors`) is currently not correctly prefixed with `Purchasing`.</span></span> <span data-ttu-id="09913-126">Il markup seguente è quello corretto:</span><span class="sxs-lookup"><span data-stu-id="09913-126">The following markup is the correct one:</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample2.aspx)]

<span data-ttu-id="09913-127">Questa origine dati può essere utilizzata quindi all'interno di un controllo repeater.</span><span class="sxs-lookup"><span data-stu-id="09913-127">This data source can then be used within a repeater.</span></span> <span data-ttu-id="09913-128">Come al solito, la `DataBinder.Eval()` metodo recupera i dati dall'origine dati.</span><span class="sxs-lookup"><span data-stu-id="09913-128">As usual, the `DataBinder.Eval()` method retrieves data from the data source.</span></span> <span data-ttu-id="09913-129">Il `ConfirmButtonExtender` controllo deve quindi essere inserito all'interno di `<ItemTemplate>` sezione del ripetitore in modo che venga visualizzato per ogni voce nell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="09913-129">The `ConfirmButtonExtender` control must then be placed within the `<ItemTemplate>` section of the repeater so that it appears for every entry in the data source.</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample3.aspx)]


<span data-ttu-id="09913-130">[![Accanto a ogni voce dall'origine dati viene visualizzato il pulsante di conferma](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="09913-130">[![The confirm button appears next to each entry from the data source](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)</span></span>

<span data-ttu-id="09913-131">Il pulsante di conferma viene visualizzato accanto a ogni voce dell'origine dati ([fare clic per visualizzare l'immagine ingrandita](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="09913-131">The confirm button appears next to each entry from the data source ([Click to view full-size image](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="09913-132">Precedente</span><span class="sxs-lookup"><span data-stu-id="09913-132">Previous</span></span>](using-a-confirmbutton-in-a-repeater-cs.md)
