---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: Associazione dati per Accordion (c#) | Documenti Microsoft
author: wenz
description: Il controllo Accordion AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta. I pannelli vengono in genere dichiarati w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: a3ec242c4d5312026ddbc8282ef1b4c3142915a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="databinding-to-an-accordion-c"></a>Associazione dati per Accordion (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)

> Il controllo Accordion AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta. I pannelli vengono in genere dichiarati all'interno della pagina, ma l'associazione a un'origine dati offre una maggiore flessibilità.


## <a name="overview"></a>Panoramica

Il controllo Accordion AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta. I pannelli vengono in genere dichiarati all'interno della pagina, ma l'associazione a un'origine dati offre una maggiore flessibilità.

## <a name="steps"></a>Passaggi

Prima di tutto, un'origine dati è obbligatoria. Questo esempio viene utilizzato il database AdventureWorks e Microsoft SQL Server 2005 Express Edition. Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa express edition) ed è anche disponibile come download separato sotto [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Il database AdventureWorks fa parte di database di esempio e gli esempi di SQL Server 2005 (download all'indirizzo [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Il modo più semplice per configurare il database consiste nell'utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e collegare il `AdventureWorks.mdf` file di database.

In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è stato chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è l'impostazione predefinita. Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.

Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il `<form>` elemento):

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

Quindi, aggiungere un'origine dati per la pagina. Per utilizzare una quantità limitata di dati, selezionare solo le prime cinque voci della tabella Vendor del database AdventureWorks. Se si utilizza l'Assistente per Visual Studio per creare l'origine dati, è importante che un bug nella versione corrente come prefisso il nome della tabella (`Vendor`) con `Purchasing`. Il markup seguente viene illustrata la sintassi corretta:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

Ricordare il nome (ID) dell'origine dati. Questa identificazione molto deve quindi essere utilizzata nel `DataSourceID` proprietà del controllo Accordion:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

All'interno del controllo Accordion, è possibile fornire modelli per varie parti del controllo, compresa l'intestazione (`<HeaderTemplate>`) e il contenuto (`<ContentTemplate>`). All'interno di questi elementi, restituire solo i dati dai dati di origine, utilizzando il `DataBinder.Eval()` metodo:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

Quando viene caricata la pagina, l'origine dati deve essere associato al accordion con questo codice lato server:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

Per concludere in questo esempio, è necessario definire le due classi CSS a cui fa riferimento nel controllo Accordion (nelle proprietà `HeaderCssClass` e `ContentCssClass`). Inserire il markup seguente nel `<head>` sezione della pagina:

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]


[![I dati nella accordion derivano direttamente dall'origine dati](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)

I dati di accordion provengono direttamente dall'origine dati ([fare clic per visualizzare l'immagine ingrandita](databinding-to-an-accordion-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](dynamically-adding-an-accordion-pane-cs.md)
