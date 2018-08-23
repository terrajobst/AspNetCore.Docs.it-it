---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Parte 1: File -> Nuovo progetto | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 1 illustra panoramica e File/nuovo progetto.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: d2e4b36c9029e86eea9b09974839e96e9aa39ced
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838664"
---
<a name="part-1-file--new-project"></a>Parte 1: File -> Nuovo progetto
====================
da [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks viene illustrato come saliente è davvero semplice per creare applicazioni potenti e scalabili per la piattaforma .NET. Illustra come usare le nuove funzionalità in ASP.NET 4 per creare un negozio online, tra cui acquisti, estrazione e l'amministrazione.
> 
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 1 illustra panoramica e File/nuovo progetto.


## <a id="_Toc260221666"></a>  Panoramica

Questa esercitazione viene fornita un'introduzione a Web Form ASP.NET. Si sarà possibile avviare lentamente, in modo che l'esperienza di sviluppo di livello web per principianti è corretto.

L'applicazione che sarà compilata si è un semplice negozio online.

![](tailspin-spyworks-part-1/_static/image1.jpg)


I visitatori possono sfogliare i prodotti per categoria:

![](tailspin-spyworks-part-1/_static/image2.jpg)

È possibile visualizzare un singolo prodotto e aggiungerlo al carrello acquisti:

![](tailspin-spyworks-part-1/_static/image3.jpg)

È possibile esaminarne carrello, la rimozione di tutti gli elementi che non sono più necessarie:

![](tailspin-spyworks-part-1/_static/image4.jpg)

Verrà richiesto loro di procedere con l'acquisto

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

Dopo l'ordinamento, visualizzano una schermata di conferma semplice:

![](tailspin-spyworks-part-1/_static/image7.jpg)


Iniziamo creando un nuovo progetto di Web Form ASP.NET in Visual Studio 2010 e verrà aggiunto in modo incrementale le funzionalità per creare un'applicazione funziona completa. Lungo il percorso, si affronterà l'accesso al database, le visualizzazioni elenco e griglia, le pagine di aggiornamento dati, la convalida dei dati, utilizzando le pagine master per layout delle pagine coerente, AJAX, convalida, l'appartenenza degli utenti e altro ancora.

È possibile seguire la procedura dettagliata, oppure è possibile scaricare l'applicazione completata [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

È possibile usare Visual Studio 2010 o le gratuito Visual Web Developer 2010 dal [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/). Per compilare l'applicazione, è possibile usare SQL Server o gratuita SQL Server Express per ospitare il database.

## <a id="_Toc260221667"></a>  File / nuovo progetto

Si inizierà selezionando il nuovo progetto dal menu File in Visual Studio. Verrà visualizzata la finestra di dialogo Nuovo progetto.

![](tailspin-spyworks-part-1/_static/image8.jpg)

Scegliamo Visual c# / modelli Web Raggruppa in base a sinistra e quindi scegliere il modello "Applicazione Web ASP.NET" nella colonna centrale. Denominare il progetto TailspinSpyworks e premere il pulsante OK.

![](tailspin-spyworks-part-1/_static/image9.jpg)

Verrà creato il progetto. Diamo un'occhiata le cartelle in cui sono inclusi nell'applicazione in Esplora soluzioni sul lato destro.

![](tailspin-spyworks-part-1/_static/image10.jpg)

La soluzione vuota non è completamente vuota, che aggiunge una struttura di cartelle di base:

![](tailspin-spyworks-part-1/_static/image1.png)

Notare le convenzioni implementate dal modello di progetto predefiniti di ASP.NET 4.

- La cartella "Account" implementa un'interfaccia utente di base per ASP. Sottosistema di appartenenza di NET.
- La cartella "Scripts" funge da archivio per i file JavaScript lato client e i file con estensione js jQuery core vengono resi disponibili per impostazione predefinita.
- La cartella "Stili" viene usata per organizzare gli oggetti visivi di sito web (fogli di stile CSS)

Quando si preme F5 per eseguire l'applicazione e il rendering della pagina default. aspx viene visualizzato quanto segue.

![](tailspin-spyworks-part-1/_static/image11.jpg)

Il primo miglioramento dell'applicazione sarà quello di sostituire il file Style. CSS del modello Web Form predefinito con i file di immagine associata che verranno eseguito il rendering di asthetics visual che intendiamo per la nostra applicazione Tailspin Spyworks e classi CSS.

Al termine dell'operazione la pagina default. aspx viene eseguito il rendering come segue.

![](tailspin-spyworks-part-1/_static/image12.jpg)

Si noti che i collegamenti di immagine nella parte superiore destra della pagina e le voci di menu che sono stati aggiunti alla pagina master. Solo i collegamenti "Accedi" e "Account" puntano alle pagine esistenti (generate dal modello predefinito) e il resto delle pagine che verrà implementato poiché noi creiamo la nostra applicazione.

Eseguiremo, inoltre, per spostare la pagina Master per la directory di stili. Anche se questa è solo una preferenza può avere operazioni un po' più semplice se si decide di apportare in futuro "skinable" la nostra applicazione.

Dopo questa operazione che è necessario modificare la pagina master i riferimenti in tutti i file con estensione aspx generato per l'impostazione predefinita le pagine Web Form ASP.NET.

> [!div class="step-by-step"]
> [avanti](tailspin-spyworks-part-2.md)
