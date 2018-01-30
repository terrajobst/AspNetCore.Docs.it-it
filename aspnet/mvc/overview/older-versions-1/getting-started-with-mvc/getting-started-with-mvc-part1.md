---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Introduzione a ASP.NET MVC | Documenti Microsoft
author: shanselman
description: Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Creare un'applicazione web semplice la lettura e scrittura da un database.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 08c30f4aab77bff64ed3ab874d13cc5dc863fc99
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
<a name="intro-to-aspnet-mvc"></a>Introduzione a ASP.NET MVC
====================
da [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Una versione aggiornata se è disponibile in questa esercitazione [qui](../../getting-started/introduction/getting-started.md) utilizzando [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Nuova esercitazione Usa ASP.NET MVC 5, che offre numerosi miglioramenti introdotti in questa esercitazione.
> 
> 
> Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Si creerà un'applicazione web semplice la lettura e scrittura da un database. Visitare il [area risorse di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.


Verifichiamo la prima applicazione Web ASP.NET MVC utilizzando [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/). Verrà effettuata una piccola applicazione elenco di film che creerà segnalare il problema e l'elenco di film.

## <a name="what-youll-build"></a>Cosa Compilerai

Di seguito sono riportate due schermate dell'applicazione, sarà necessario creare. È una tabella semplice dei filmati con diverse colonne.

[![Elenco di film - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

E sarà necessario un modulo di creazione in modo possiamo aggiungere all'elenco di film.

[![Creare un filmato - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>Si apprenderà competenze

In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web di MVC ASP.NET utilizzando Visual Studio. Si apprenderà:

- Come creare un nuovo progetto MVC ASP.NET
- Come creare un nuovo Database con SQL Server
- Come creare ASP.NET MVC controller e visualizzazioni
- Come recuperare e visualizzare i dati
- Come modificare i dati e abilitare la convalida dei dati
- Come aggiornare lo schema del database

## <a name="get-started"></a>Introduzione

Iniziare eseguendo Visual Web Developer 2010 Express (che chiamerò "VWD" d'ora in avanti) e selezionare Nuovo progetto, dalla schermata Start.

Visual Web Developer è un IDE, o un ambiente di sviluppo integrato. Proprio come si usa Microsoft Word per scrivere documenti, si userà un IDE per creare applicazioni. È una barra degli strumenti nella parte superiore che mostra le diverse opzioni disponibili si, nonché i menu potrebbe anche avere utilizzato per selezionare File | Nuovo progetto.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>Creazione di un'applicazione

È possibile creare applicazioni con Visual Basic o Visual c#. Per il momento, selezionare Visual c# a sinistra, quindi selezionare "Applicazione Web ASP.NET MVC 2". Denominare il progetto "Filmati" e fare clic su OK.

[![Nuovo progetto](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

Sul lato destro è Esplora soluzioni che mostra tutti i file e cartelle nell'applicazione. La finestra grande al centro è possibile modificare il codice e dedicare del tempo. Visual Studio utilizzato un modello predefinito per il progetto ASP.NET MVC che appena creato, in modo che sia un'applicazione funzionante ora senza eseguire alcuna operazione. Si tratta di un semplice "Hello World! progetto e è un buon punto di partenza per l'applicazione.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

Selezionare il pulsante "play" alla barra degli strumenti.

![Avvia debug](getting-started-with-mvc-part1/_static/image11.png)

È una freccia verde che punta a destra che compila il programma e avviare l'applicazione in un web browser.

*Nota: È possibile invece premere F5 o selezionare Debug -&gt;Avvia debug dal menu "Debug".*

In questo modo Visual Web Developer avviare un server web di sviluppo ed eseguire l'applicazione web (non sono presenti configurazione o una procedura manuale necessaria per abilitare questa opzione). Verrà quindi avviato un browser e configurarlo per esaminare l'home page dell'applicazione. Notare che la barra degli indirizzi del browser viene visualizzato "localhost" e non un risultato simile example.com sotto. Ciò avviene perché localhost punta sempre al computer locale, che in questo caso è in esecuzione l'applicazione che abbiamo appena creato.

[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

All'esterno della casella tale modello predefinito offre è due pagine da visitare e una pagina di accesso di base. Si modifica il funzionamento di questa applicazione e un po' informazioni su MVC ASP.NET nel processo. Chiudere il browser e consente di modificare il codice.

>[!div class="step-by-step"]
[avanti](getting-started-with-mvc-part2.md)
