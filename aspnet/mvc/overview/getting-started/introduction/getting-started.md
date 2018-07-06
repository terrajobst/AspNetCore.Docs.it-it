---
uid: mvc/overview/getting-started/introduction/getting-started
title: Introduzione ad ASP.NET MVC 5 | Microsoft Docs
author: Rick-Anderson
description: 'Nota: Una versione aggiornata di questa esercitazione è disponibile qui utilizzando Visual Studio 2015. La nuova esercitazione Usa ASP.NET Core MVC 6, che offre molte improvem...'
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 85d5d3292ff99ade6995c710e2728c41255def4c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823216"
---
<a name="getting-started-with-aspnet-mvc-5"></a>Introduzione ad ASP.NET MVC 5
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 Questa esercitazione insegnerà le nozioni di base della creazione di un'app web ASP.NET MVC 5 con [Visual Studio 2017](https://www.visualstudio.com/). Trovano origine finale per l'esercitazione su [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)


 Questa esercitazione è stato scritto dal [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , e [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )

 È necessario un account di Azure per distribuire l'app in Azure:

 - È possibile [aprire un account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -ricevono crediti è possibile usare per provare i servizi di Azure a pagamento e anche dopo che sono abituati fino è possibile mantenere l'account e usare i servizi Azure gratuiti.
 - È possibile [attivare i benefici della sottoscrizione MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -la sottoscrizione MSDN si accumulano crediti ogni mese in cui è possibile usare per i servizi di Azure a pagamento.


## <a name="getting-started"></a>Introduzione

Per iniziare, installare ed eseguire [Visual Studio 2017](https://www.visualstudio.com/).

Visual Studio è un IDE, o ambiente di sviluppo integrato. Come si usa Microsoft Word per scrivere documenti, si userà un IDE per creare applicazioni. In Visual Studio è un elenco lungo il bordo inferiore che mostra le diverse opzioni disponibili per l'utente. È inoltre disponibile un menu che fornisce un altro modo per eseguire attività nell'IDE. (Ad esempio, invece di selezionare **nuovo progetto** dal **avviare** pagina, è possibile usare il menu e selezionare **File** &gt; **nuovo progetto**.)


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a>Creazione della prima applicazione

Fare clic su **nuovo progetto**, quindi selezionare Visual c# a sinistra, quindi **Web** e quindi selezionare **applicazione Web ASP.NET (.NET Framework)**. Denominare il progetto "MvcMovie" e quindi fare clic su **OK**.

![](getting-started/_static/image2.png)

Nel **nuovo progetto ASP.NET** finestra di dialogo, fare clic su **MVC** e quindi fare clic su **OK**.

![](getting-started/_static/image3.png)

Visual Studio usato un modello predefinito per il progetto ASP.NET MVC che appena creato, in modo che sia subito un'applicazione funzionante non esegue alcuna operazione. Si tratta di un semplice "Hello World!" progetto che 's un buon punto di partenza dell'applicazione.

![](getting-started/_static/image4.png)

Premere F5 per avviare il debug. F5 fa in modo che Visual Studio avviare [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) ed eseguire l'app web. Quindi, Visual Studio avvia un browser e verrà visualizzata la home page dell'applicazione. Si noti che la barra degli indirizzi del browser afferma `localhost:port#` e non o simili `example.com`. Infatti, `localhost` fa sempre riferimento al proprio computer locale, consentendo in questo caso è in esecuzione l'applicazione appena compilato. Quando Visual Studio viene eseguito un progetto web, viene usata una porta casuale per il server web. Nell'immagine seguente, il numero di porta è 1234. Quando si esegue l'applicazione, si noterà un numero di porta diverso.

![](getting-started/_static/image5.png)

Impostazione predefinita questo modello predefinito offre pagine Home, contatto e sulle. L'immagine precedente non viene visualizzato il **Home**, **sulle** e **contatto** collegamenti. A seconda delle dimensioni della finestra del browser, potrebbe essere necessario fare clic sull'icona di spostamento per visualizzare i collegamenti seguenti.

![](getting-started/_static/image6.png)  

L'applicazione fornisce inoltre supporto per registrare e accedere. Il passaggio successivo è modificare il funzionamento di questa applicazione e un po' informazioni su ASP.NET MVC. Chiudere l'applicazione ASP.NET MVC e modificare codice.

Per un elenco di esercitazioni correnti, vedere [MVC consigliata gli articoli](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Vedere questa App in esecuzione in Azure

Si desidera vedere il sito completo in esecuzione come un'app web in tempo reale? È possibile distribuire una versione completa dell'app al tuo account Azure, facendo semplicemente clic sul pulsante seguente.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

È necessario un account di Azure per distribuire questa soluzione in Azure. Se non hai già un account, sono disponibili le opzioni seguenti:

- [Aprire un account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -ricevono crediti è possibile usare per provare i servizi di Azure a pagamento e anche dopo che sono abituati fino è possibile mantenere l'account e usare i servizi Azure gratuiti.
- [Attivare i benefici della sottoscrizione MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -la sottoscrizione MSDN si accumulano crediti ogni mese in cui è possibile usare per i servizi di Azure a pagamento.

> [!div class="step-by-step"]
> [avanti](adding-a-controller.md)
