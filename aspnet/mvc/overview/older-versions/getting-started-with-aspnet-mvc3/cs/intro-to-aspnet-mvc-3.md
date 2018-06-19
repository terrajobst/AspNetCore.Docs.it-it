---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Introduzione a ASP.NET MVC 3 (c#) | Documenti Microsoft
author: Rick-Anderson
description: In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 9b965df6175051a084de35627160161c116be42d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868929"
---
<a name="intro-to-aspnet-mvc-3-c"></a>Introduzione a ASP.NET MVC 3 (c#)
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > È disponibile una versione aggiornata di questa esercitazione [qui](../../../getting-started/introduction/getting-started.md) che utilizza ASP.NET MVC 5 e Visual Studio 2013. È molto più semplice da seguire, più sicuro e vengono illustrate altre funzionalità.
> 
> 
> In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul seguente collegamento: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti utilizzando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supportano runtime + tools)
> 
> Se si utilizza Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul seguente collegamento: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un progetto di Visual Web Developer con il codice sorgente c# è disponibile a complemento di questo argomento. [Scaricare la versione c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce Visual Basic, passare il [versione Visual Basic](../vb/intro-to-aspnet-mvc-3.md) di questa esercitazione.


## <a name="what-youll-build"></a>Cosa Compilerai

Viene quindi implementato una semplice applicazione di elenco di film che supporta la creazione, modifica e l'elenco di film da un database. Di seguito sono riportate due schermate dell'applicazione, sarà necessario creare. Include una pagina che visualizza un elenco di film da un database:

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

L'applicazione consente inoltre di aggiungere, modificare ed eliminare i film, nonché le informazioni relative a singole. Tutti gli scenari di immissione di dati includono la convalida per verificare che i dati archiviati nel database siano corretti.

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a>Si apprenderà competenze

Ecco cosa si apprenderà:

- Come creare un nuovo progetto ASP.NET MVC.
- Come creare ASP.NET MVC controller e visualizzazioni.
- Come creare un nuovo database utilizzando il paradigma di Entity Framework Code First.
- Come recuperare e visualizzare i dati.
- Come modificare i dati e abilitare la convalida dei dati.

## <a name="getting-started"></a>Introduzione

Iniziare eseguendo Visual Web Developer 2010 Express ("Visual Web Developer" forma abbreviata) e selezionare **nuovo progetto** dal **avviare** pagina.

Visual Web Developer è un IDE, o un ambiente di sviluppo integrato. Proprio come si usa Microsoft Word per scrivere documenti, si userà un IDE per creare applicazioni. In Visual Web Developer è presente una barra degli strumenti nella parte superiore che mostra le diverse opzioni disponibili per l'utente. È inoltre disponibile un menu che consente a un altro modo per eseguire attività nell'IDE. (Ad esempio, anziché selezionare **nuovo progetto** dal **avviare** pagina, è possibile utilizzare il menu e selezionare **File** &gt; **nuovo progetto**.)

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a>Creazione di un'applicazione

È possibile creare applicazioni che usano Visual Basic o Visual c# come linguaggio di programmazione. Selezionare Visual c# a sinistra e quindi **applicazione Web di ASP.NET MVC 3**. Denominare il progetto "MvcMovie" e quindi fare clic su **OK**. (Se si preferisce Visual Basic, passare il [versione Visual Basic](../vb/intro-to-aspnet-mvc-3.md) di questa esercitazione.)

![](intro-to-aspnet-mvc-3/_static/image5.png)

Nel **nuovo progetto ASP.NET MVC 3** nella finestra di dialogo **applicazione Internet**. Controllare **HTML5 utilizzare markup** e lasciare **Razor** come motore di visualizzazione predefinito.

![](intro-to-aspnet-mvc-3/_static/image6.png)

Fare clic su **OK**. Visual Web Developer utilizzato un modello predefinito per il progetto ASP.NET MVC che appena creato, è un'applicazione funzionante ora senza eseguire alcuna operazione. Si tratta di un semplice "Hello World!" progetto e 's un buon punto di partenza dell'applicazione.

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

Scegliere **Avvia debug** dal menu **Debug**.

![](intro-to-aspnet-mvc-3/_static/image9.png)

Si noti che il tasto di scelta rapida per avviare il debug F5.

F5 causa Visual Web Developer avviare un server web di sviluppo ed eseguire l'applicazione web. Quindi, in Visual Web Developer viene avviato un browser e verrà visualizzata la home page dell'applicazione. Si noti che la barra degli indirizzi del browser afferma `localhost` e non è `example.com`. Ciò accade perché `localhost` punta sempre al computer locale, che in questo caso è in esecuzione l'applicazione appena compilato. Quando Visual Web Developer viene eseguito un progetto web, viene utilizzata una porta casuale per il server web. Nell'immagine seguente, il numero di porta casuale è 43246. Quando si esegue l'applicazione, si noterà probabilmente un numero di porta diverso.

![](intro-to-aspnet-mvc-3/_static/image10.png)

Subito tale modello predefinito offre due pagine da visitare e una pagina di accesso di base. Il passaggio successivo consiste nella modifica il funzionamento di questa applicazione e un po' informazioni su MVC ASP.NET nel processo. Chiudere il browser e modificare il codice.

> [!div class="step-by-step"]
> [avanti](adding-a-controller.md)
