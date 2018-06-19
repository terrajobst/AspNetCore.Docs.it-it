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
ms.locfileid: "30868399"
---
<a name="using-hovermenu-with-a-repeater-control-vb"></a>Utilizzo di HoverMenu con un controllo Repeater (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)

> Il controllo HoverMenu AJAX Control Toolkit fornisce un effetto popup semplice: quando il puntatore del mouse viene posizionato su un elemento, una finestra popup viene visualizzato in una posizione specificata. È inoltre possibile utilizzare il controllo all'interno di un controllo repeater.


## <a name="overview"></a>Panoramica

Il `HoverMenu` controllo AJAX Control Toolkit fornisce un effetto popup semplice: quando il puntatore del mouse viene posizionato su un elemento, una finestra popup viene visualizzato in una posizione specificata. È inoltre possibile utilizzare il controllo all'interno di un controllo repeater.

## <a name="steps"></a>Passaggi

Prima di tutto, un'origine dati è obbligatoria. Questo esempio viene utilizzato il database AdventureWorks e Microsoft SQL Server 2005 Express Edition. Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa express edition) ed è anche disponibile come download separato sotto [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Il database AdventureWorks fa parte di database di esempio e gli esempi di SQL Server 2005 (download all'indirizzo [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Il modo più semplice per configurare il database consiste nell'utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e collegare il `AdventureWorks.mdf` file di database.

In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è stato chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è l'impostazione predefinita. Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.

Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il `<form>` elemento):

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample1.aspx)]

Quindi, aggiungere un'origine dati per la pagina. Per utilizzare una quantità limitata di dati, selezionare solo le prime cinque voci della tabella Vendor del database AdventureWorks. Se si utilizza l'Assistente per Visual Studio per creare l'origine dati, è importante che un bug nella versione corrente come prefisso il nome della tabella (`Vendor`) con `Purchasing`. Il markup seguente viene illustrata la sintassi corretta:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample2.aspx)]

Successivamente, aggiungere un pannello che funge da finestra popup modale:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample3.aspx)]

A questo punto, il `HoverMenuExtender` entra in gioco. In modo che ogni elemento nell'origine dati Ottiene il proprio popup, l'estensione deve essere inserito all'interno del ripetitore `<ItemTemplate>` sezione. Questo è il markup:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample4.aspx)]

Ora ogni elemento nell'origine dati consente di visualizzare una finestra popup a destra (`PopupPosition` attributo) dopo un ritardo di 50 millisecondi (`PopDelay` attributo).


[![Viene visualizzato il menu di passaggio del mouse accanto a ogni elemento nel repeater](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)

Viene visualizzato il menu di passaggio del mouse accanto a ogni elemento nel repeater ([fare clic per visualizzare l'immagine ingrandita](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](using-hovermenu-with-a-repeater-control-cs.md)
