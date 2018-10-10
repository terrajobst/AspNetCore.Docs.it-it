---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Introduzione ad ASP.NET MVC | Microsoft Docs
author: shanselman
description: Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Creare un'applicazione web semplice che legge e scrive da un database.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 2d9c1dd0dd3c9f892b42b0f29ac3361a7f2b638c
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910858"
---
<a name="intro-to-aspnet-mvc"></a>Introduzione ad ASP.NET MVC
====================
da [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Una versione aggiornata se è disponibile in questa esercitazione [Ecco](../../getting-started/introduction/getting-started.md) utilizzando [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). La nuova esercitazione Usa ASP.NET MVC 5, che offre numerosi miglioramenti in questa esercitazione.
>
>
> Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Si creerà una semplice applicazione web che legge e scrive da un database. Visitare il [centro di formazione di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.


Creare la prima applicazione Web MVC ASP.NET utilizzando [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/). Ci assicureremo una piccola applicazione elenco di film che verrà creato e l'elenco di film invia i tuoi commenti.

## <a name="what-youll-build"></a>Scopo dell'esercitazione

Ecco due schermate dell'applicazione che verrà compilata. Si avrà una semplice tabella di film con varie colonne.

[![Elenco di film - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

E sarà necessario un modulo creare in modo che possiamo aggiungere all'elenco di film.

[![Creare un filmato - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>Competenze

Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web ASP.NET MVC con Visual Studio. Si apprenderà come:

- Come creare un nuovo progetto ASP.NET MVC
- Come creare un nuovo Database con SQL Server
- Come creare visualizzazioni e controller MVC ASP.NET
- Come recuperare e visualizzare i dati
- Come modificare i dati e abilitare la convalida dei dati
- Come aggiornare lo schema del database

## <a name="get-started"></a>Introduzione

Iniziare eseguendo Visual Web Developer 2010 Express (che chiamerò "VWD" nel) e selezionare Nuovo progetto dalla schermata Start.

Visual Web Developer è un IDE, o ambiente di sviluppo integrato. Come si usa Microsoft Word per scrivere documenti, si userà un IDE per creare applicazioni. È presente una barra degli strumenti nella parte superiore che mostra le diverse opzioni disponibili si, nonché i menu potrebbe anche avere utilizzato per selezionare File | Nuovo progetto.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>Creazione della prima applicazione

È possibile creare applicazioni con Visual Basic o Visual c#. Per il momento, selezionare Visual c# a sinistra, quindi selezionare "Applicazione Web ASP.NET MVC 2". Denominare il progetto "Movies" e fare clic su OK.

[![Nuovo progetto](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

Sul lato destro è di Esplora soluzioni con tutti i file e cartelle all'interno dell'applicazione. Finestra al centro di big data è possibile modificare il codice e trascorrono la maggior parte del tempo. Visual Studio usato un modello predefinito per il progetto ASP.NET MVC che appena creato, in modo che sia subito un'applicazione funzionante non esegue alcuna operazione. Si tratta di una semplice "Hello World! progetto che è un buon punto di partenza per la nostra applicazione.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

Selezionare il pulsante "play" alla barra degli strumenti.

![Avvia debug](getting-started-with-mvc-part1/_static/image11.png)

È una freccia verde rivolta verso destra che verrà compilato il programma e avviare l'applicazione in un web browser.

*Nota: È possibile in alternativa premere F5 sulla tastiera o selezionare Debug -&gt;Avvia debug dal menu "Debug".*

In questo modo Visual Web Developer avviare un server web di sviluppo ed eseguire l'applicazione web (non esistono alcuna configurazione o passaggi manuali necessari per abilitare questa opzione). Quindi verrà avviato un browser e configurarlo per Sfoglia home page dell'applicazione. Notare che la barra degli indirizzi del browser viene visualizzato "localhost" e non un valore come example.com sotto. Ciò avviene perché localhost fa sempre riferimento al proprio computer locale - che in questo caso è in esecuzione l'applicazione, che abbiamo appena creato.

[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

Impostazione predefinita questo modello predefinito include è due pagine alla visita e una pagina di accesso basic. È possibile modificare il comportamento dell'applicazione e scopri un po' su ASP.NET MVC nel processo. Chiudere il browser e consente di modificare il codice.

> [!div class="step-by-step"]
> [avanti](getting-started-with-mvc-part2.md)
