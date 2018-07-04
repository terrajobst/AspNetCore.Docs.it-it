---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Introduzione ad ASP.NET MVC 3 (c#) | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: d4cda748638a6396750fe2ef7713dd0b2a4555d6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379863"
---
<a name="intro-to-aspnet-mvc-3-c"></a>Introduzione ad ASP.NET MVC 3 (c#)
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > È disponibile una versione aggiornata di questa esercitazione [qui](../../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e vengono illustrate altre funzionalità.
> 
> 
> Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul collegamento seguente: [instalace Webové Platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime e strumenti di supportano)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul collegamento seguente: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un progetto di Visual Web Developer con codice sorgente c# è disponibile a complemento di questo argomento. [Scaricare la versione c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce Visual Basic, passare al [versione Visual Basic](../vb/intro-to-aspnet-mvc-3.md) di questa esercitazione.


## <a name="what-youll-build"></a>Scopo dell'esercitazione

Si sarà implementare una semplice applicazione di elenco di film che supporta la creazione, modifica e l'elenco di film da un database. Di seguito sono riportate due schermate dell'applicazione che verrà compilata. Include una pagina che visualizza un elenco di film da un database:

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

L'applicazione consente anche di aggiungere, modificare ed eliminare filmati, nonché le informazioni relative a quelli singoli. Tutti gli scenari di immissione di dati includono la convalida per garantire che i dati archiviati nel database siano corretti.

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a>Competenze

Ecco cosa si apprenderà:

- Come creare un nuovo progetto ASP.NET MVC.
- Come creare ASP.NET MVC controller e visualizzazioni.
- Come creare un nuovo database usando il paradigma Code First di Entity Framework.
- Come recuperare e visualizzare i dati.
- Come modificare i dati e abilitare la convalida dei dati.

## <a name="getting-started"></a>Introduzione

Iniziare eseguendo Visual Web Developer 2010 Express ("Visual Web Developer" breve) e selezionare **nuovo progetto** dalle **avviare** pagina.

Visual Web Developer è un IDE, o ambiente di sviluppo integrato. Come si usa Microsoft Word per scrivere documenti, si userà un IDE per creare applicazioni. In Visual Web Developer è presente una barra degli strumenti nella parte superiore che mostra le diverse opzioni disponibili per l'utente. È inoltre disponibile un menu che fornisce un altro modo per eseguire attività nell'IDE. (Ad esempio, invece di selezionare **nuovo progetto** dal **avviare** pagina, è possibile usare il menu e selezionare **File** &gt; **nuovo progetto**.)

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a>Creazione della prima applicazione

È possibile creare applicazioni che usano Visual Basic o Visual c# come linguaggio di programmazione. Selezionare Visual c# a sinistra e quindi selezionare **applicazione Web ASP.NET MVC 3**. Denominare il progetto "MvcMovie" e quindi fare clic su **OK**. (Se si preferisce Visual Basic, passare al [versione Visual Basic](../vb/intro-to-aspnet-mvc-3.md) di questa esercitazione.)

![](intro-to-aspnet-mvc-3/_static/image5.png)

Nel **nuovo progetto ASP.NET MVC 3** finestra di dialogo **applicazione Internet**. Controllare **markup usare HTML5** lasciando **Razor** come motore di visualizzazione predefinito.

![](intro-to-aspnet-mvc-3/_static/image6.png)

Fare clic su **OK**. Visual Web Developer usato un modello predefinito per il progetto ASP.NET MVC che appena creato, in modo che sia subito un'applicazione funzionante non esegue alcuna operazione. Si tratta di un semplice "Hello World!" progetto che 's un buon punto di partenza dell'applicazione.

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

Scegliere **Avvia debug** dal menu **Debug**.

![](intro-to-aspnet-mvc-3/_static/image9.png)

Si noti che il tasto di scelta rapida per avviare il debug F5.

F5 fa in modo che Visual Web Developer avviare un server web di sviluppo ed eseguire l'applicazione web. Quindi, Visual Web Developer viene avviato un browser e verrà visualizzata la home page dell'applicazione. Si noti che la barra degli indirizzi del browser afferma `localhost` e non o simili `example.com`. Infatti, `localhost` fa sempre riferimento al proprio computer locale, consentendo in questo caso è in esecuzione l'applicazione appena compilato. Quando Visual Web Developer viene eseguito un progetto web, viene usata una porta casuale per il server web. Nell'immagine seguente, il numero di porta casuale è 43246. Quando si esegue l'applicazione, si noterà probabilmente un numero di porta diverso.

![](intro-to-aspnet-mvc-3/_static/image10.png)

Impostazione predefinita questo modello predefinito offre due pagine di visitare e una pagina di accesso basic. Il passaggio successivo è modificare il comportamento dell'applicazione e imparare un po' su ASP.NET MVC nel processo. Chiudere il browser e modificare codice.

> [!div class="step-by-step"]
> [avanti](adding-a-controller.md)
