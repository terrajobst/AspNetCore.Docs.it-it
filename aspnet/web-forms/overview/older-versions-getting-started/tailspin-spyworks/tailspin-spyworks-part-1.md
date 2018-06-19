---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Parte 1: File -> Nuovo progetto | Documenti Microsoft'
author: JoeStagner
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 1 copre panoramica e File/nuovo progetto.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: a1b9681516e626b6a0eec420b168a74e05d88afb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892465"
---
<a name="part-1-file--new-project"></a>Parte 1: File -> Nuovo progetto
====================
da [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks viene illustrato come particolarmente semplice è creare potenti applicazioni scalabili per la piattaforma .NET. Illustra come usare le nuove caratteristiche in ASP.NET 4 per creare un archivio online, inclusi gli acquisti, estrazione e l'amministrazione.
> 
> Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 1 copre panoramica e File/nuovo progetto.


## <a id="_Toc260221666"></a>  Panoramica

In questa esercitazione viene fornita un'introduzione a ASP.NET WebForms. Si verrà avviata lenta, pertanto l'esperienza di sviluppo web di livello per principianti è accettabile.

L'applicazione che è da generare è un semplice negozio online.

![](tailspin-spyworks-part-1/_static/image1.jpg)


I visitatori possono sfogliare i prodotti per categoria:

![](tailspin-spyworks-part-1/_static/image2.jpg)

È possibile visualizzare un singolo prodotto e aggiungerlo al carrello:

![](tailspin-spyworks-part-1/_static/image3.jpg)

È possibile esaminarne carrello, la rimozione di tutti gli elementi che non si desidera più:

![](tailspin-spyworks-part-1/_static/image4.jpg)

Procedere con l'acquisto verrà visualizzato un messaggio a

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

Dopo aver ordinato, viene visualizzato una schermata di conferma semplice:

![](tailspin-spyworks-part-1/_static/image7.jpg)


Iniziare creando un nuovo progetto di Web Form ASP.NET in Visual Studio 2010 e funzionalità per creare un'applicazione funzionante completezza in modo incrementale verrà aggiunto. Inoltre, ci occuperemo accesso al database, le visualizzazioni elenco e griglia, pagine di aggiornamento dati, la convalida dei dati, utilizzando le pagine master per layout di pagina coerente, AJAX, convalida, l'appartenenza dell'utente e altro ancora.

È possibile seguire la procedura dettagliata, oppure è possibile scaricare l'applicazione finita da [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

È possibile utilizzare Visual Studio 2010 o il liberi Visual Web Developer 2010 da [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/). Per compilare l'applicazione, è possibile utilizzare SQL Server o il disponibile SQL Server Express per ospitare il database.

## <a id="_Toc260221667"></a>  File / nuovo progetto

Si inizierà selezionando il nuovo progetto dal menu File in Visual Studio. Verrà visualizzata la finestra di dialogo Nuovo progetto.

![](tailspin-spyworks-part-1/_static/image8.jpg)

È possibile selezionare Visual c# / modelli Web gruppo a sinistra e quindi scegliere il modello "Applicazione Web ASP.NET" nella colonna centrale. Denominare il progetto TailspinSpyworks e fare clic sul pulsante OK.

![](tailspin-spyworks-part-1/_static/image9.jpg)

Verrà creato il progetto. Esaminiamo le cartelle in cui sono inclusi nell'applicazione in Esplora soluzioni sul lato destro.

![](tailspin-spyworks-part-1/_static/image10.jpg)

La soluzione vuota non è completamente vuota e aggiunge una struttura di cartelle di base:

![](tailspin-spyworks-part-1/_static/image1.png)

Tenere presente le convenzioni implementate dal modello di progetto predefinito di ASP.NET 4.

- La cartella "Account" implementa un'interfaccia utente di base per ASP. Sottosistema di appartenenza della rete.
- La cartella "Scripts" funge da repository per il file JavaScript sul lato client e i file con estensione js jQuery core vengono resi disponibili per impostazione predefinita.
- La cartella "Stili" viene usata per organizzare elementi visivi del sito web (fogli di stile CSS)

Quando si preme F5 per eseguire l'applicazione e il rendering della pagina aspx viene visualizzato quanto segue.

![](tailspin-spyworks-part-1/_static/image11.jpg)

Il miglioramento dell'applicazione prima è necessario sostituire il file Style.css dal modello Web Form predefinito con i file di immagine associata che verranno eseguito il rendering di asthetics visual desiderati per l'applicazione Tailspin Spyworks e di classi CSS.

Al termine dell'operazione nostra pagina aspx esegue il rendering simile al seguente.

![](tailspin-spyworks-part-1/_static/image12.jpg)

Notare i collegamenti di immagine nella parte superiore destra della pagina e le voci di menu che sono stati aggiunti alla pagina master. Solo i collegamenti di "Accedi" e "Account" puntano per le pagine esistenti (generate tramite il modello predefinito) e il resto delle pagine che verrà implementata mentre si compila l'applicazione.

Verrà inoltre spostare la pagina Master per la directory di stili. Anche se questo è solo una preferenza può avere elementi un po' più semplice se si decide di eseguire l'applicazione "skinable" in futuro.

Dopo questa operazione che è necessario modificare la pagina master i riferimenti in tutti i file con estensione aspx generato per l'impostazione predefinita le pagine Web Form ASP.NET.

> [!div class="step-by-step"]
> [avanti](tailspin-spyworks-part-2.md)
