---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: Introduzione a ASP.NET MVC 3 (VB) | Documenti Microsoft
author: Rick-Anderson
description: "In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: f0717e8d635818322be8b3242efe756a20351340
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="intro-to-aspnet-mvc-3-vb"></a>Introduzione a ASP.NET MVC 3 (VB)
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

> In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul seguente collegamento: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti utilizzando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supportano runtime + tools)
> 
> Se si utilizza Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul seguente collegamento: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un progetto di Visual Web Developer con codice sorgente VB.NET complemento è disponibile in questo argomento. [Scaricare la versione di Visual Basic.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce c#, passare il [c# versione](../cs/intro-to-aspnet-mvc-3.md) di questa esercitazione.


In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul seguente collegamento: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti utilizzando i collegamenti seguenti:

- [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supportano runtime + tools)

Se si utilizza Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul seguente collegamento: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Un progetto di Visual Web Developer con codice sorgente di Visual Basic è disponibile a complemento di questo argomento. [Scaricare la versione di Visual Basic qui](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824). Se si preferisce CSharp, passare il [CSharp versione](../cs/intro-to-aspnet-mvc-3.md) di questa esercitazione.

## <a name="what-youll-build"></a>Cosa Compilerai

Viene quindi implementato una semplice applicazione di elenco di film che supporta la creazione, modifica e l'elenco di film da un database. Di seguito sono riportate due schermate dell'applicazione, sarà necessario creare. Include una pagina che visualizza un elenco di film da un database:

[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)

L'applicazione consente inoltre di aggiungere, modificare ed eliminare i film, nonché le informazioni relative a singole. Tutti gli scenari di immissione di dati includono la convalida per verificare che i dati archiviati nel database siano corretti.

[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="skills-youll-learn"></a>Si apprenderà competenze

Ecco cosa si apprenderà:

- Come creare un nuovo progetto ASP.NET MVC
- Come creare un nuovo database utilizzando codice-first di Entity Framework
- Come creare ASP.NET MVC controller e visualizzazioni
- Come recuperare e visualizzare i dati
- Come modificare i dati e abilitare la convalida dei dati

## <a name="getting-started"></a>Introduzione

Iniziare eseguendo Visual Web Developer 2010 Express ("VWD" forma abbreviata) e selezionare **nuovo progetto** dal **avviare** pagina.

Visual Web Developer è un IDE, o un ambiente di sviluppo integrato. Proprio come si usa Microsoft Word per scrivere documenti, si userà un IDE per creare applicazioni. In Visual Web Developer è presente una barra degli strumenti nella parte superiore che mostra le diverse opzioni disponibili per l'utente. È inoltre disponibile un menu che consente a un altro modo per eseguire attività nell'IDE. (Ad esempio, anziché selezionare **nuovo progetto** dal **avviare** pagina, è possibile utilizzare il menu e selezionare **File** &gt; **nuovo progetto**.)

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a>Creazione di un'applicazione

È possibile creare applicazioni che usano la scelta di Visual Basic o Visual c# come linguaggio di programmazione. Per questa esercitazione, selezionare Visual Basic a sinistra, quindi selezionare **applicazione Web di ASP.NET MVC 3**. Denominare il progetto "MvcMovie" e quindi fare clic su **OK**.

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

Nel **nuovo progetto ASP.NET MVC 3** nella finestra di dialogo **applicazione Internet**. Lasciare **Razor** come motore di visualizzazione predefinito.

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

Fare clic su **OK**. Visual Web Developer utilizzato un modello predefinito per il progetto ASP.NET MVC che appena creato, è un'applicazione funzionante ora senza eseguire alcuna operazione. Si tratta di un semplice "Hello World!" progetto e 's un buon punto di partenza dell'applicazione.

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

Scegliere **Avvia debug** dal menu **Debug**.

![](intro-to-aspnet-mvc-3/_static/image11.png)

Si noti che il tasto di scelta rapida per avviare il debug F5.

F5 causa Visual Web Developer avviare un server web di sviluppo ed eseguire l'applicazione web. VWD quindi viene avviato un browser e verrà visualizzata la home page dell'applicazione. Si noti che la barra degli indirizzi del browser afferma `localhost` e non è `example.com`. Ciò accade perché `localhost` punta sempre al computer locale, che in questo caso è in esecuzione l'applicazione appena compilato. Quando VWD viene eseguito un progetto web, viene utilizzata una porta casuale per il progetto. Nell'immagine seguente, il numero di porta casuale è 43246. Il progetto utilizzerà probabilmente un numero di porta diverso.

![](intro-to-aspnet-mvc-3/_static/image12.png)

All'esterno della casella tale modello predefinito offre è due pagine da visitare e una pagina di accesso di base. Si modifica il funzionamento di questa applicazione e un po' informazioni su MVC ASP.NET nel processo. Chiudere il browser e modificare il codice.

>[!div class="step-by-step"]
[Successivo](adding-a-controller.md)
