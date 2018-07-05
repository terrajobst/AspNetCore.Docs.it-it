---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Uso dell'API Web con Web Form ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: 91811031b937d3ca05888ce8d4f28bcc33537c76
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400879"
---
<a name="using-web-api-with-aspnet-web-forms"></a>Uso dell'API Web con Web Form ASP.NET
====================
da [Mike Wasson](https://github.com/MikeWasson)

Anche se viene incluso nel pacchetto di API Web ASP.NET con ASP.NET MVC, è facile aggiungere l'API Web a un'applicazione Web Form ASP.NET tradizionale. Questa esercitazione illustra i passaggi.

## <a name="overview"></a>Panoramica

Per usare l'API Web in un'applicazione Web Form, esistono due passaggi principali:

- Aggiungere un controller API Web da cui deriva il **ApiController** classe.
- Aggiungere una tabella di route per il **Application\_avviare** (metodo).

## <a name="create-a-web-forms-project"></a>Creare un progetto di Web Form

Avviare Visual Studio e selezionare **nuovo progetto** dalle **avviare** pagina. E viceversa, il **File** dal menu **New** e quindi **progetto**.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere le **Visual c#** nodo. Sotto **Visual c#**, selezionare **Web**. Nell'elenco dei modelli di progetto, selezionare **applicazione Web Form ASP.NET**. Immettere un nome per il progetto e fare clic su **OK**.

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a>Creare il modello e Controller

Questa esercitazione Usa le stesse classi del modello e controller come il [introduttiva](tutorial-your-first-web-api.md) esercitazione.

In primo luogo, aggiungere una classe di modello. Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Aggiungi classe**. Denominare la classe Product e aggiungere l'implementazione seguente:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

Successivamente, aggiungere un controller API Web al progetto., una *controller* è l'oggetto che gestisce le richieste HTTP per l'API Web.

Nelle **Esplora soluzioni**, fare clic sul progetto. Selezionare **Aggiungi nuovo elemento**.

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

Sotto **modelli installati**, espandere **Visual c#** e selezionare **Web**. Quindi, nell'elenco dei modelli, selezionare **classe Controller API Web**. Denominare il controller "ProductsController" e fare clic su **Add**.

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

Il **Aggiungi nuovo elemento** procedura guidata creerà un file denominato ProductsController.cs. Eliminare i metodi che è inclusa la procedura guidata e aggiungere i metodi seguenti:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

Per altre informazioni sul codice in questo controller, vedere la [introduttiva](tutorial-your-first-web-api.md) esercitazione.

## <a name="add-routing-information"></a>Aggiungere le informazioni di Routing

Successivamente, si aggiungerà una route URI in modo che gli URI nel formato &quot;/API/prodotti/&quot; vengono indirizzati al controller.

Nelle **Esplora soluzioni**, fare doppio clic su Global. asax per aprire il file code-behind Global.asax.cs. Aggiungere il codice seguente **usando** istruzione.

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

Quindi aggiungere il codice seguente per il **Application\_avviare** metodo:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

Per altre informazioni sulle tabelle di routing, vedere [Routing in API Web ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="add-client-side-ajax"></a>Aggiungere AJAX lato Client

Questo è tutto che è necessario creare un'API web che i client possono accedere. A questo punto è possibile aggiungere una pagina HTML che usa jQuery per chiamare l'API.

Assicurarsi che la pagina master (ad esempio, *Site. master*) include una `ContentPlaceHolder` con `ID="HeadContent"`:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

Aprire il file default. aspx. Sostituire il testo boilerplate nella sezione del contenuto principale, come illustrato:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

Successivamente, aggiungere un riferimento al file di origine jQuery nel `HeaderContent` sezione:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

Nota: È possibile facilmente aggiungere il riferimento allo script trascinando e rilasciando il file dalla **Esplora soluzioni** nella finestra dell'editor di codice.

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

Sotto il tag di script jQuery, aggiungere il seguente blocco di script:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

Quando il documento viene caricato, lo script esegue una richiesta AJAX &quot;api/prodotti&quot;. La richiesta restituisce un elenco di prodotti in formato JSON. Lo script aggiunge le informazioni sul prodotto per la tabella HTML.

Quando si esegue l'applicazione, dovrebbe essere simile al seguente:

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
