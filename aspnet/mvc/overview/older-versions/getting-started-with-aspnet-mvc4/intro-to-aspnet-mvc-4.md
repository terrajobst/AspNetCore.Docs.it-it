---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: Introduzione ad ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Una versione aggiornata se questa esercitazione è disponibile qui utilizzando Visual Studio 2013. La nuova esercitazione Usa ASP.NET MVC 5, che offre numerosi miglioramenti rispetto t...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: ea3d1517192ded0e5372c49897bb1fec33324b6f
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912397"
---
<a name="intro-to-aspnet-mvc-4"></a>Introduzione ad ASP.NET MVC 4
====================
da [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Una versione aggiornata se è disponibile in questa esercitazione [Ecco](../../getting-started/introduction/getting-started.md) utilizzando [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). La nuova esercitazione Usa ASP.NET MVC 5, che offre numerosi miglioramenti in questa esercitazione.
>
> Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web ASP.NET MVC 4 con Microsoft [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) o Visual Web Developer 2010 Express Service Pack 1. È consigliabile Visual Studio 2012, non dovrai installare nulla per completare l'esercitazione. Se si usa Visual Studio 2010 è necessario installare i componenti riportati di seguito. È possibile installare tutti gli elementi facendo clic sui collegamenti seguenti:
>
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Programma di installazione WPI per ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
>
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare il [programma di installazione WPI per ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) e il: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
>
> Un progetto di Visual Web Developer con codice sorgente c# è disponibile a complemento di questo argomento. [Scaricare la versione c#](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).
>
> In questa esercitazione si esegue l'applicazione in Visual Studio. Si può anche rendere disponibile l'applicazione su Internet tramite distribuzione su un provider di hosting. Microsoft offre l'hosting web gratuito per fino a 10 siti web in un [account di valutazione di Microsoft Azure gratuito](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Per informazioni su come distribuire un progetto web Visual Studio per un sito Web di Microsoft Azure, vedere [crea e Distribuisci un sito web ASP.NET e Database SQL con Visual Studio](https://docs.microsoft.com/dotnet/azure/). Tale esercitazione illustra anche come usare le migrazioni di Entity Framework Code First per distribuire il database di SQL Server in Database SQL di Azure (precedentemente SQL Azure).
>
> Questa esercitazione è stato scritto da Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="what-youll-build"></a>Scopo dell'esercitazione

> [!NOTE]
> Una versione aggiornata se è disponibile in questa esercitazione [Ecco](../../getting-started/introduction/getting-started.md) utilizzando [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). La nuova esercitazione Usa ASP.NET MVC 5, che offre numerosi miglioramenti in questa esercitazione.


Si sarà implementare una semplice applicazione di elenco di film che supporta la creazione, modifica, la ricerca e l'elenco di film da un database. Di seguito sono riportate due schermate dell'applicazione che verrà compilata. Include una pagina che visualizza un elenco di film da un database:

![](intro-to-aspnet-mvc-4/_static/image1.png)

L'applicazione consente anche di aggiungere, modificare ed eliminare filmati, nonché le informazioni relative a quelli singoli. Tutti gli scenari di immissione di dati includono la convalida per garantire che i dati archiviati nel database siano corretti.

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>Introduzione

Iniziare eseguendo Visual Studio Express 2012 o Visual Web Developer 2010 Express. La maggior parte delle schermate di questo utilizzo di serie Visual Studio Express 2012, ma è possibile completare questa esercitazione con Visual Studio 2010 SP1, Visual Studio 2012, Visual Studio Express 2012 o Visual Web Developer 2010 Express. Selezionare **nuovo progetto** dalle **avviare** pagina.

Visual Studio è un IDE, o ambiente di sviluppo integrato. Come si usa Microsoft Word per scrivere documenti, si userà un IDE per creare applicazioni. In Visual Studio è presente una barra degli strumenti nella parte superiore che mostra le diverse opzioni disponibili per l'utente. È inoltre disponibile un menu che fornisce un altro modo per eseguire attività nell'IDE. (Ad esempio, invece di selezionare **nuovo progetto** dal **avviare** pagina, è possibile usare il menu e selezionare **File** &gt; **nuovo progetto**.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>Creazione della prima applicazione

È possibile creare applicazioni che usano Visual Basic o Visual c# come linguaggio di programmazione. Selezionare Visual c# a sinistra e quindi selezionare **applicazione Web ASP.NET MVC 4**. Denominare il progetto &quot;MvcMovie&quot; e quindi fare clic su **OK**.

![](intro-to-aspnet-mvc-4/_static/image4.png)

Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo **applicazione Internet**. Lasciare **Razor** come motore di visualizzazione predefinito.

![](intro-to-aspnet-mvc-4/_static/image5.png)

Scegliere **OK**. Visual Studio usato un modello predefinito per il progetto ASP.NET MVC che appena creato, in modo che sia subito un'applicazione funzionante non esegue alcuna operazione. Si tratta di una semplice &quot;Hello World!&quot; progetto che di un buon punto di partenza dell'applicazione.

![](intro-to-aspnet-mvc-4/_static/image6.png)

Scegliere **Avvia debug** dal menu **Debug**.

![](intro-to-aspnet-mvc-4/_static/image7.png)

Si noti che il tasto di scelta rapida per avviare il debug F5.

F5 fa in modo che Visual Studio avviare IIS Express ed eseguire l'applicazione web. Quindi, Visual Studio avvia un browser e verrà visualizzata la home page dell'applicazione. Si noti che la barra degli indirizzi del browser afferma `localhost` e non o simili `example.com`. Infatti, `localhost` fa sempre riferimento al proprio computer locale, consentendo in questo caso è in esecuzione l'applicazione appena compilato. Quando Visual Studio viene eseguito un progetto web, viene usata una porta casuale per il server web. Nell'immagine seguente, il numero di porta è 41788. Quando si esegue l'applicazione, si noterà probabilmente un numero di porta diverso.

![](intro-to-aspnet-mvc-4/_static/image8.png)

Impostazione predefinita questo modello predefinito offre pagine Home, contatto e sulle. Inoltre fornisce il supporto per registrare e accedere e collegamenti a Facebook e Twitter. Il passaggio successivo è modificare il funzionamento di questa applicazione e un po' informazioni su ASP.NET MVC. Chiudere il browser e modificare codice.

> [!div class="step-by-step"]
> [avanti](adding-a-controller.md)
