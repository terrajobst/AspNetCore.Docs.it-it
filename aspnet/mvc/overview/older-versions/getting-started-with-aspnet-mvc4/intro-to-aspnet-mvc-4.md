---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: Introduzione a ASP.NET MVC 4 | Documenti Microsoft
author: Rick-Anderson
description: Una versione aggiornata, se in questa esercitazione è disponibile qui con Visual Studio 2013. Nuova esercitazione Usa ASP.NET MVC 5, che offre numerosi miglioramenti rispetto t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 519bac22ba2607931c5f3123b9b567859a2b3d1c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869049"
---
<a name="intro-to-aspnet-mvc-4"></a>Introduzione a ASP.NET MVC 4
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> Una versione aggiornata se è disponibile in questa esercitazione [qui](../../getting-started/introduction/getting-started.md) utilizzando [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Nuova esercitazione Usa ASP.NET MVC 5, che offre numerosi miglioramenti introdotti in questa esercitazione.
> 
> In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web ASP.NET MVC 4 tramite Microsoft [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) o Visual Web Developer 2010 Express Service Pack 1. È consigliabile Visual Studio 2012, non dovrai installare nulla per completare l'esercitazione. Se si utilizza Visual Studio 2010 è necessario installare i componenti seguenti. È possibile installare tutti gli elementi facendo clic sui collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Programma di installazione WPI per ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare il [programma di installazione WPI per ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) e il: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
> 
> Un progetto di Visual Web Developer con il codice sorgente c# è disponibile a complemento di questo argomento. [Scaricare la versione c#](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).
> 
> Nell'esercitazione si esegue l'applicazione in Visual Studio. È inoltre possibile rendere l'applicazione disponibile su Internet mediante la distribuzione di un provider di hosting. Microsoft offre l'hosting web gratuito per fino a 10 siti web in un [senza account di valutazione di Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Per informazioni su come distribuire un progetto web di Visual Studio a un sito Web di Windows Azure, vedere [creare e distribuire un sito web ASP.NET e il Database SQL con Visual Studio](https://docs.microsoft.com/dotnet/azure/). Inoltre, tale esercitazione viene illustrato come utilizzare le migrazioni di Entity Framework Code First per distribuire il database di SQL Server in Database SQL di Azure (precedentemente denominato SQL Azure).
> 
> In questa esercitazione è stato scritto dal Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="what-youll-build"></a>Cosa Compilerai

> [!NOTE]
> Una versione aggiornata se è disponibile in questa esercitazione [qui](../../getting-started/introduction/getting-started.md) utilizzando [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Nuova esercitazione Usa ASP.NET MVC 5, che offre numerosi miglioramenti introdotti in questa esercitazione.


Viene quindi implementato una semplice applicazione di elenco di film che supporta la creazione, modifica, la ricerca e l'elenco di film da un database. Di seguito sono riportate due schermate dell'applicazione, sarà necessario creare. Include una pagina che visualizza un elenco di film da un database:

![](intro-to-aspnet-mvc-4/_static/image1.png)

L'applicazione consente inoltre di aggiungere, modificare ed eliminare i film, nonché le informazioni relative a singole. Tutti gli scenari di immissione di dati includono la convalida per verificare che i dati archiviati nel database siano corretti.

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>Introduzione

Iniziare eseguendo Visual Studio Express 2012 o Visual Web Developer 2010 Express. La maggior parte delle schermate di questo utilizzo di serie di Visual Studio Express 2012, ma è possibile completare questa esercitazione con Visual Studio 2010 SP1, Visual Studio 2012, Visual Studio Express 2012 o Visual Web Developer 2010 Express. Selezionare **nuovo progetto** dal **avviare** pagina.

Visual Studio è un IDE, o un ambiente di sviluppo integrato. Proprio come si usa Microsoft Word per scrivere documenti, si userà un IDE per creare applicazioni. In Visual Studio è presente una barra degli strumenti nella parte superiore che mostra le diverse opzioni disponibili per l'utente. È inoltre disponibile un menu che consente a un altro modo per eseguire attività nell'IDE. (Ad esempio, anziché selezionare **nuovo progetto** dal **avviare** pagina, è possibile utilizzare il menu e selezionare **File** &gt; **nuovo progetto**.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>Creazione di un'applicazione

È possibile creare applicazioni che usano Visual Basic o Visual c# come linguaggio di programmazione. Selezionare Visual c# a sinistra e quindi **applicazione Web ASP.NET MVC 4**. Denominare il progetto &quot;MvcMovie&quot; e quindi fare clic su **OK**.

![](intro-to-aspnet-mvc-4/_static/image4.png)

Nel **nuovo progetto ASP.NET MVC 4** nella finestra di dialogo **applicazione Internet**. Lasciare **Razor** come motore di visualizzazione predefinito.

![](intro-to-aspnet-mvc-4/_static/image5.png)

Fare clic su **OK**. Visual Studio utilizzato un modello predefinito per il progetto ASP.NET MVC che appena creato, in modo che sia un'applicazione funzionante ora senza eseguire alcuna operazione. Questo è un semplice &quot;Hello World!&quot; progetto e un buon punto di partenza dell'applicazione.

![](intro-to-aspnet-mvc-4/_static/image6.png)

Scegliere **Avvia debug** dal menu **Debug**.

![](intro-to-aspnet-mvc-4/_static/image7.png)

Si noti che il tasto di scelta rapida per avviare il debug F5.

F5 fa sì che Visual Studio avviare IIS Express ed eseguire l'applicazione web. Visual Studio, quindi, viene avviato un browser e verrà visualizzata la home page dell'applicazione. Si noti che la barra degli indirizzi del browser afferma `localhost` e non è `example.com`. Ciò accade perché `localhost` punta sempre al computer locale, che in questo caso è in esecuzione l'applicazione appena compilato. Quando Visual Studio viene eseguito un progetto web, viene utilizzata una porta casuale per il server web. Nell'immagine seguente, il numero di porta è 41788. Quando si esegue l'applicazione, si noterà probabilmente un numero di porta diverso.

![](intro-to-aspnet-mvc-4/_static/image8.png)

Subito tale modello predefinito consente principale, contatti e sulle pagine. Inoltre fornisce il supporto per registrare e accedere e collegamenti a Facebook e Twitter. Il passaggio successivo consiste nella modifica il funzionamento di questa applicazione e un po' informazioni su ASP.NET MVC. Chiudere il browser e modificare il codice.

> [!div class="step-by-step"]
> [avanti](adding-a-controller.md)
