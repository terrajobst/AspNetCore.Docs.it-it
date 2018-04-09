---
uid: mvc/overview/getting-started/introduction/getting-started
title: Introduzione a ASP.NET MVC 5 | Documenti Microsoft
author: Rick-Anderson
description: 'Nota: Una versione aggiornata di questa esercitazione è disponibile qui utilizzando Visual Studio 2015. Nuova esercitazione Usa ASP.NET Core MVC 6, che fornisce molti improvem...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 0f1fd2026691d3bc0e81b20a9731879d7a6041bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-aspnet-mvc-5"></a>Introduzione ad ASP.NET MVC 5
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 In questa esercitazione verrà fornite le nozioni di base della creazione di un'app web ASP.NET MVC 5 utilizzando [Visual Studio 2017](https://www.visualstudio.com/). Origine finale per l'esercitazione si trovano in [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)


 In questa esercitazione è stato scritto dal [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , e [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )

 È necessario un account Azure per distribuire l'app in Azure:

 - È possibile [aprire un account Azure, gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -si ottiene crediti è possibile utilizzare per provare i servizi di Azure a pagamento e anche dopo l'uso massimo è possibile mantenere l'account e utilizzare senza servizi di Azure.
 - È possibile [attivare i benefici per sottoscrittori MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Your sottoscrizione MSDN offre crediti ogni mese in cui è possibile utilizzare per i servizi di Azure a pagamento.


## <a name="getting-started"></a>Introduzione

Avviare l'installazione ed eseguendo [Visual Studio 2017](https://www.visualstudio.com/).

Visual Studio è un IDE, o un ambiente di sviluppo integrato. Proprio come si usa Microsoft Word per scrivere documenti, si userà un IDE per creare applicazioni. In Visual Studio è disponibile un elenco lungo il bordo inferiore che mostra le diverse opzioni disponibili per l'utente. È inoltre disponibile un menu che consente a un altro modo per eseguire attività nell'IDE. (Ad esempio, anziché selezionare **nuovo progetto** dal **avviare** pagina, è possibile utilizzare il menu e selezionare **File** &gt; **nuovo progetto**.)


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a>Creazione di un'applicazione

Fare clic su **nuovo progetto**, quindi selezionare Visual c# a sinistra, quindi **Web** e quindi selezionare **applicazione Web ASP.NET (.NET Framework)**. Denominare il progetto "MvcMovie" e quindi fare clic su **OK**.

![](getting-started/_static/image2.png)

Nel **nuovo progetto ASP.NET** finestra di dialogo, fare clic su **MVC** e quindi fare clic su **OK**.

![](getting-started/_static/image3.png)

Visual Studio utilizzato un modello predefinito per il progetto ASP.NET MVC che appena creato, in modo che sia un'applicazione funzionante ora senza eseguire alcuna operazione. Si tratta di un semplice "Hello World!" progetto e 's un buon punto di partenza dell'applicazione.

![](getting-started/_static/image4.png)

Premere F5 per avviare il debug. F5 fa sì che Visual Studio avviare [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) ed eseguire l'app web. Visual Studio, quindi, viene avviato un browser e verrà visualizzata la home page dell'applicazione. Si noti che la barra degli indirizzi del browser afferma `localhost:port#` e non è `example.com`. Ciò accade perché `localhost` punta sempre al computer locale, che in questo caso è in esecuzione l'applicazione appena compilato. Quando Visual Studio viene eseguito un progetto web, viene utilizzata una porta casuale per il server web. Nell'immagine seguente, il numero di porta è 1234. Quando si esegue l'applicazione, verrà visualizzato un numero di porta diverso.

![](getting-started/_static/image5.png)

Subito tale modello predefinito consente principale, contatti e sulle pagine. L'immagine precedente non viene visualizzato il **Home**, **su** e **contatto** collegamenti. A seconda delle dimensioni della finestra del browser, potrebbe essere necessario fare clic sull'icona di navigazione per vedere i collegamenti.

![](getting-started/_static/image6.png)  

L'applicazione fornisce inoltre il supporto per registrare e accedere. Il passaggio successivo consiste nella modifica il funzionamento di questa applicazione e un po' informazioni su ASP.NET MVC. Chiudere l'applicazione ASP.NET MVC e modificare il codice.

Per un elenco di esercitazioni corrente, vedere [MVC consigliato articoli](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Vedere l'App in esecuzione in Azure

Si desidera vedere il sito completo in esecuzione come un'app web in tempo reale? È possibile distribuire una versione completa dell'app al proprio account Azure, facendo semplicemente clic sul pulsante seguente.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

È necessario un account Azure per distribuire questa soluzione in Azure. Se si dispone già di un account, sono disponibili le opzioni seguenti:

- [Aprire un account Azure, gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -si ottiene crediti è possibile utilizzare per provare i servizi di Azure a pagamento e anche dopo l'uso massimo è possibile mantenere l'account e utilizzare senza servizi di Azure.
- [Attivare i benefici per sottoscrittori MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Your sottoscrizione MSDN offre crediti ogni mese in cui è possibile utilizzare per i servizi di Azure a pagamento.

> [!div class="step-by-step"]
> [avanti](adding-a-controller.md)
