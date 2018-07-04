---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
title: Uso di ConfirmButton In un controllo Repeater (c#) | Microsoft Docs
author: wenz
description: Il dispositivo extender ConfirmButton in AJAX Control Toolkit crea Sì/Nessun popup quando l'utente fa clic su un pulsante (compresi controllo LinkButton). Solo se è Sì...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a973ed3e-400c-4925-ace2-0b086b479301
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: d3706578427442a7a1034596bdd9bd89eecf5f67
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369751"
---
<a name="using-a-confirmbutton-in-a-repeater-c"></a><span data-ttu-id="c7064-104">Uso di ConfirmButton In un controllo Repeater (c#)</span><span class="sxs-lookup"><span data-stu-id="c7064-104">Using a ConfirmButton In a Repeater (C#)</span></span>
====================
<span data-ttu-id="c7064-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c7064-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c7064-106">[Scaricare il codice](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c7064-106">[Download Code](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1CS.pdf)</span></span>

> <span data-ttu-id="c7064-107">Il dispositivo extender ConfirmButton in AJAX Control Toolkit crea Sì/Nessun popup quando l'utente fa clic su un pulsante (compresi controllo LinkButton).</span><span class="sxs-lookup"><span data-stu-id="c7064-107">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="c7064-108">Solo se si seleziona Sì, del pulsante viene eseguita l'azione in caso contrario, è stata annullata.</span><span class="sxs-lookup"><span data-stu-id="c7064-108">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="c7064-109">Questo è anche possibile in un controllo repeater.</span><span class="sxs-lookup"><span data-stu-id="c7064-109">This is also possible in a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="c7064-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="c7064-110">Overview</span></span>

<span data-ttu-id="c7064-111">Il dispositivo extender ConfirmButton in AJAX Control Toolkit crea Sì/Nessun popup quando l'utente fa clic su un pulsante (compresi controllo LinkButton).</span><span class="sxs-lookup"><span data-stu-id="c7064-111">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="c7064-112">Solo se si seleziona Sì, del pulsante viene eseguita l'azione in caso contrario, è stata annullata.</span><span class="sxs-lookup"><span data-stu-id="c7064-112">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="c7064-113">Questo è anche possibile in un controllo repeater.</span><span class="sxs-lookup"><span data-stu-id="c7064-113">This is also possible in a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="c7064-114">Passaggi</span><span class="sxs-lookup"><span data-stu-id="c7064-114">Steps</span></span>

<span data-ttu-id="c7064-115">Prima di tutto, è richiesta un'origine dati.</span><span class="sxs-lookup"><span data-stu-id="c7064-115">First of all, a data source is required.</span></span> <span data-ttu-id="c7064-116">Questo esempio Usa il database AdventureWorks e Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="c7064-116">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="c7064-117">Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa l'edizione express) ed è anche disponibile come download separato in [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="c7064-117">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="c7064-118">Il database AdventureWorks è parte di SQL Server 2005 Samples and Sample Databases (download all'indirizzo [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="c7064-118">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="c7064-119">Il modo più semplice per configurare il database è di utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e collegare il `AdventureWorks.mdf` file di database.</span><span class="sxs-lookup"><span data-stu-id="c7064-119">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="c7064-120">In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è anche l'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="c7064-120">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="c7064-121">Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.</span><span class="sxs-lookup"><span data-stu-id="c7064-121">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="c7064-122">Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="c7064-122">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample1.aspx)]

<span data-ttu-id="c7064-123">Quindi, un'origine dati è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="c7064-123">Then, a data source is required.</span></span> <span data-ttu-id="c7064-124">Per ragioni di semplicità, vengono recuperate solo le prime cinque voci nella tabella di fornitori di AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="c7064-124">For the sake of simplicity, only the first five entries in AdventureWorks' Vendors table are retrieved.</span></span> <span data-ttu-id="c7064-125">Si noti che quando si usa la procedura guidata di Visual Studio per creare l'origine dati, il nome della tabella (`Vendors`) è attualmente non correttamente preceduto `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="c7064-125">Note that when using the Visual Studio wizard to create the data source, the table name (`Vendors`) is currently not correctly prefixed with `Purchasing`.</span></span> <span data-ttu-id="c7064-126">Il markup seguente sia quella corretta:</span><span class="sxs-lookup"><span data-stu-id="c7064-126">The following markup is the correct one:</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample2.aspx)]

<span data-ttu-id="c7064-127">Questa origine dati può quindi essere utilizzata all'interno di un controllo repeater.</span><span class="sxs-lookup"><span data-stu-id="c7064-127">This data source can then be used within a repeater.</span></span> <span data-ttu-id="c7064-128">Come di consueto, la `DataBinder.Eval()` metodo recupera i dati dall'origine dati.</span><span class="sxs-lookup"><span data-stu-id="c7064-128">As usual, the `DataBinder.Eval()` method retrieves data from the data source.</span></span> <span data-ttu-id="c7064-129">Il `ConfirmButtonExtender` controllo deve quindi essere inserito all'interno di `<ItemTemplate>` sezione del ripetitore in modo che venga visualizzato per ogni voce nell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="c7064-129">The `ConfirmButtonExtender` control must then be placed within the `<ItemTemplate>` section of the repeater so that it appears for every entry in the data source.</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample3.aspx)]


<span data-ttu-id="c7064-130">[![Il pulsante di conferma viene visualizzato accanto a ogni voce dell'origine dati](using-a-confirmbutton-in-a-repeater-cs/_static/image2.png)](using-a-confirmbutton-in-a-repeater-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c7064-130">[![The confirm button appears next to each entry from the data source](using-a-confirmbutton-in-a-repeater-cs/_static/image2.png)](using-a-confirmbutton-in-a-repeater-cs/_static/image1.png)</span></span>

<span data-ttu-id="c7064-131">Il pulsante di conferma viene visualizzato accanto a ogni voce dell'origine dati ([fare clic per visualizzare l'immagine con dimensioni normali](using-a-confirmbutton-in-a-repeater-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c7064-131">The confirm button appears next to each entry from the data source ([Click to view full-size image](using-a-confirmbutton-in-a-repeater-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c7064-132">avanti</span><span class="sxs-lookup"><span data-stu-id="c7064-132">Next</span></span>](using-a-confirmbutton-in-a-repeater-vb.md)
