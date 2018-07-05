---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: Introduzione ad ASP.NET MVC 3 (VB) | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 1a870f10d39980335c7acb4a543907fecd408eb2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372353"
---
<a name="intro-to-aspnet-mvc-3-vb"></a>Introduzione ad ASP.NET MVC 3 (VB)
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul collegamento seguente: [instalace Webové Platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime e strumenti di supportano)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul collegamento seguente: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un progetto di Visual Web Developer con codice sorgente Visual Basic.NET è disponibile a complemento di questo argomento. [Scaricare la versione VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce c#, passare al [c# versione](../cs/intro-to-aspnet-mvc-3.md) di questa esercitazione.


Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul collegamento seguente: [instalace Webové Platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:

- [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime e strumenti di supportano)

Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul collegamento seguente: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Un progetto di Visual Web Developer con codice sorgente Visual Basic è disponibile a complemento di questo argomento. [Scaricare la versione VB qui](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824). Se si preferisce CSharp, passare al [CSharp versione](../cs/intro-to-aspnet-mvc-3.md) di questa esercitazione.

## <a name="what-youll-build"></a>Scopo dell'esercitazione

Si sarà implementare una semplice applicazione di elenco di film che supporta la creazione, modifica e l'elenco di film da un database. Di seguito sono riportate due schermate dell'applicazione che verrà compilata. Include una pagina che visualizza un elenco di film da un database:

[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)

L'applicazione consente anche di aggiungere, modificare ed eliminare filmati, nonché le informazioni relative a quelli singoli. Tutti gli scenari di immissione di dati includono la convalida per garantire che i dati archiviati nel database siano corretti.

[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="skills-youll-learn"></a>Competenze

Ecco cosa si apprenderà:

- Come creare un nuovo progetto ASP.NET MVC
- Come creare un nuovo database usando Entity Framework code first
- Come creare ASP.NET MVC controller e visualizzazioni
- Come recuperare e visualizzare i dati
- Come modificare i dati e abilitare la convalida dei dati

## <a name="getting-started"></a>Introduzione

Iniziare eseguendo Visual Web Developer 2010 Express ("VWD" breve) e selezionare **nuovo progetto** dalle **avviare** pagina.

Visual Web Developer è un IDE, o ambiente di sviluppo integrato. Come si usa Microsoft Word per scrivere documenti, si userà un IDE per creare applicazioni. In Visual Web Developer è presente una barra degli strumenti nella parte superiore che mostra le diverse opzioni disponibili per l'utente. È inoltre disponibile un menu che fornisce un altro modo per eseguire attività nell'IDE. (Ad esempio, invece di selezionare **nuovo progetto** dal **avviare** pagina, è possibile usare il menu e selezionare **File** &gt; **nuovo progetto**.)

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a>Creazione della prima applicazione

È possibile creare applicazioni che usano la scelta di Visual Basic o Visual c# come linguaggio di programmazione. Per questa esercitazione, selezionare Visual Basic a sinistra, quindi selezionare **applicazione Web ASP.NET MVC 3**. Denominare il progetto "MvcMovie" e quindi fare clic su **OK**.

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

Nel **nuovo progetto ASP.NET MVC 3** finestra di dialogo **applicazione Internet**. Lasciare **Razor** come motore di visualizzazione predefinito.

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

Fare clic su **OK**. Visual Web Developer usato un modello predefinito per il progetto ASP.NET MVC che appena creato, in modo che sia subito un'applicazione funzionante non esegue alcuna operazione. Si tratta di un semplice "Hello World!" progetto che 's un buon punto di partenza dell'applicazione.

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

Scegliere **Avvia debug** dal menu **Debug**.

![](intro-to-aspnet-mvc-3/_static/image11.png)

Si noti che il tasto di scelta rapida per avviare il debug F5.

F5 fa in modo che Visual Web Developer avviare un server web di sviluppo ed eseguire l'applicazione web. VWD quindi avvia un browser e verrà visualizzata la home page dell'applicazione. Si noti che la barra degli indirizzi del browser afferma `localhost` e non o simili `example.com`. Infatti, `localhost` fa sempre riferimento al proprio computer locale, consentendo in questo caso è in esecuzione l'applicazione appena compilato. Quando VWD viene eseguito un progetto web, viene usata una porta casuale per il progetto. Nell'immagine seguente, il numero di porta casuale è 43246. Il progetto userà probabilmente un numero di porta diverso.

![](intro-to-aspnet-mvc-3/_static/image12.png)

Impostazione predefinita questo modello predefinito include è due pagine alla visita e una pagina di accesso basic. È possibile modificare il comportamento dell'applicazione e scopri un po' su ASP.NET MVC nel processo. Chiudere il browser e modificare codice.

> [!div class="step-by-step"]
> [avanti](adding-a-controller.md)
