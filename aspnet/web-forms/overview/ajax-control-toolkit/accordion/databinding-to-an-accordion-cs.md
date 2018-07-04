---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: Data Binding a un controllo Accordion (c#) | Microsoft Docs
author: wenz
description: Il controllo Accordion in AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta. I pannelli vengono dichiarati in genere w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: 8441eb15d369085b93297ae896d595a40d4099fd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394022"
---
<a name="databinding-to-an-accordion-c"></a><span data-ttu-id="f985a-104">Data Binding a un controllo Accordion (c#)</span><span class="sxs-lookup"><span data-stu-id="f985a-104">Databinding to an Accordion (C#)</span></span>
====================
<span data-ttu-id="f985a-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f985a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f985a-106">[Scaricare il codice](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="f985a-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)</span></span>

> <span data-ttu-id="f985a-107">Il controllo Accordion in AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta.</span><span class="sxs-lookup"><span data-stu-id="f985a-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="f985a-108">I pannelli in genere vengono dichiarati all'interno della pagina, ma l'associazione a un'origine dati offre una maggiore flessibilità.</span><span class="sxs-lookup"><span data-stu-id="f985a-108">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="f985a-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="f985a-109">Overview</span></span>

<span data-ttu-id="f985a-110">Il controllo Accordion in AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta.</span><span class="sxs-lookup"><span data-stu-id="f985a-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="f985a-111">I pannelli in genere vengono dichiarati all'interno della pagina, ma l'associazione a un'origine dati offre una maggiore flessibilità.</span><span class="sxs-lookup"><span data-stu-id="f985a-111">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="f985a-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="f985a-112">Steps</span></span>

<span data-ttu-id="f985a-113">Prima di tutto, è richiesta un'origine dati.</span><span class="sxs-lookup"><span data-stu-id="f985a-113">First of all, a data source is required.</span></span> <span data-ttu-id="f985a-114">Questo esempio Usa il database AdventureWorks e Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="f985a-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="f985a-115">Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa l'edizione express) ed è anche disponibile come download separato in [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="f985a-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="f985a-116">Il database AdventureWorks è parte di SQL Server 2005 Samples and Sample Databases (download all'indirizzo [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="f985a-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="f985a-117">Il modo più semplice per configurare il database è di utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e collegare il `AdventureWorks.mdf` file di database.</span><span class="sxs-lookup"><span data-stu-id="f985a-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="f985a-118">In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è anche l'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="f985a-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="f985a-119">Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.</span><span class="sxs-lookup"><span data-stu-id="f985a-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="f985a-120">Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="f985a-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

<span data-ttu-id="f985a-121">Quindi, aggiungere un'origine dati alla pagina.</span><span class="sxs-lookup"><span data-stu-id="f985a-121">Then, add a data source to the page.</span></span> <span data-ttu-id="f985a-122">Per usare una quantità limitata di dati, selezionare solo le prime cinque voci della tabella Vendor del database AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="f985a-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="f985a-123">Se si usa l'Assistente per Visual Studio per creare l'origine dati, è importante che un bug nella versione corrente non prefisso il nome della tabella (`Vendor`) con `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="f985a-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="f985a-124">Il markup seguente mostra la sintassi corretta:</span><span class="sxs-lookup"><span data-stu-id="f985a-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

<span data-ttu-id="f985a-125">Ricordare il nome (ID) dell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="f985a-125">Remember the name (ID) of the data source.</span></span> <span data-ttu-id="f985a-126">Questa identificazione molto deve quindi essere usata nel `DataSourceID` proprietà del controllo Accordion:</span><span class="sxs-lookup"><span data-stu-id="f985a-126">This very identification must then be used in the `DataSourceID` property of the Accordion control:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

<span data-ttu-id="f985a-127">All'interno del controllo Accordion, è possibile fornire modelli per varie parti del controllo, compresa l'intestazione (`<HeaderTemplate>`) e il contenuto (`<ContentTemplate>`).</span><span class="sxs-lookup"><span data-stu-id="f985a-127">Within the Accordion control, you can provide templates for various parts of the control, including the header (`<HeaderTemplate>`) and the content (`<ContentTemplate>`).</span></span> <span data-ttu-id="f985a-128">All'interno di questi elementi, restituire solo i dati dai dati di origine, utilizzando il `DataBinder.Eval()` metodo:</span><span class="sxs-lookup"><span data-stu-id="f985a-128">Within these elements, just output the data from the data source, using the `DataBinder.Eval()` method:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

<span data-ttu-id="f985a-129">Quando la pagina viene caricata, l'origine dati deve essere associato ai accordion con questo codice lato server:</span><span class="sxs-lookup"><span data-stu-id="f985a-129">When the page is loaded, the data source must be bound to the accordion with this server-side code:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

<span data-ttu-id="f985a-130">Per concludere questo esempio, è necessario definire le due classi CSS a cui fanno riferimento nel controllo Accordion (nelle relative proprietà `HeaderCssClass` e `ContentCssClass`).</span><span class="sxs-lookup"><span data-stu-id="f985a-130">To conclude this sample, you need to define the two CSS classes that are referenced in the Accordion control (in its properties `HeaderCssClass` and `ContentCssClass`).</span></span> <span data-ttu-id="f985a-131">Inserire il seguente markup nel `<head>` sezione della pagina:</span><span class="sxs-lookup"><span data-stu-id="f985a-131">Put the following markup in the `<head>` section of the page:</span></span>

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]


<span data-ttu-id="f985a-132">[![I dati di controllo accordion provengono direttamente dall'origine dati](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f985a-132">[![The data in the accordion comes directly from the data source](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)</span></span>

<span data-ttu-id="f985a-133">I dati di controllo accordion provengono direttamente dall'origine dati ([fare clic per visualizzare l'immagine con dimensioni normali](databinding-to-an-accordion-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f985a-133">The data in the accordion comes directly from the data source ([Click to view full-size image](databinding-to-an-accordion-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f985a-134">avanti</span><span class="sxs-lookup"><span data-stu-id="f985a-134">Next</span></span>](dynamically-adding-an-accordion-pane-cs.md)
