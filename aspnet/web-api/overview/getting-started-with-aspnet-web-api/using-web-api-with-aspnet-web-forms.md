---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Using Web API with ASP.NET Web Forms | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: af918671a8bcc97a0050ea033ccd14dd96416e73
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/01/2017
ms.locfileid: "26536080"
---
<a name="using-web-api-with-aspnet-web-forms"></a>Using Web API with ASP.NET Web Forms
====================
da [Mike Wasson](https://github.com/MikeWasson)

Anche se viene creato un pacchetto di ASP.NET Web API with ASP.NET MVC, è facile aggiungere API Web a un'applicazione Web Form ASP.NET tradizionale. In questa esercitazione vengono illustrati i passaggi.

## <a name="overview"></a>Panoramica

Per utilizzare l'API Web in un'applicazione Web Form, sono disponibili due passaggi principali:

- Aggiungere un controller API Web da cui deriva il **ApiController** classe.
- Aggiungere una tabella di route per il **applicazione\_avviare** metodo.

## <a name="create-a-web-forms-project"></a>Creare un progetto di Web Form

Avviare Visual Studio e selezionare **nuovo progetto** dal **avviare** pagina. O dal **File** dal menu **New** e quindi **progetto**.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il **Visual c#** nodo. In **Visual c#** selezionare **Web**. Nell'elenco dei modelli di progetto, selezionare **applicazioni Web Form ASP.NET**. Immettere un nome per il progetto e fare clic su **OK**.

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a>Creare il modello e il Controller

In questa esercitazione Usa le classi di modello e il controller stesso come il [Introduzione](tutorial-your-first-web-api.md) esercitazione.

Aggiungere innanzitutto una classe di modello. In **Esplora**, fare clic sul progetto e selezionare **Aggiungi classe**. Nome della classe di prodotto, quindi aggiungere l'implementazione del seguente:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

Successivamente, aggiungere un controller API Web al progetto., A *controller* è l'oggetto che gestisce le richieste HTTP per l'API Web.

In **Esplora**, fare clic sul progetto. Selezionare **Aggiungi nuovo elemento**.

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

In **modelli installati**, espandere **Visual c#** e selezionare **Web**. Quindi, nell'elenco dei modelli, selezionare **classe Controller di Web API**. Denominare il controller "ProductsController" e fare clic su **Aggiungi**.

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

Il **Aggiungi nuovo elemento** procedura guidata creerà un file denominato ProductsController.cs. Eliminare i metodi inclusi nella procedura guidata e aggiungere i metodi seguenti:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

Per ulteriori informazioni sul codice in questo controller, vedere il [Introduzione](tutorial-your-first-web-api.md) esercitazione.

## <a name="add-routing-information"></a>Aggiungere le informazioni di Routing

Successivamente, aggiungeremo una route URI in modo che gli URI del form &quot;/api/prodotti/&quot; vengono indirizzati al controller.

In **Esplora**, fare doppio clic su Global. asax per aprire il file code-behind Global.asax.cs. Aggiungere il seguente **utilizzando** istruzione.

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

Quindi aggiungere il codice seguente per il **applicazione\_avviare** metodo:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

Per ulteriori informazioni sulle tabelle di routine, vedere [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="add-client-side-ajax"></a>Aggiungere AJAX lato Client

Questo è tutto che è necessario creare una web API che i client possono accedere. Ora aggiungere una pagina HTML che utilizza jQuery per chiamare l'API.

Verificare che la pagina master (ad esempio, *Site. master*) include un `ContentPlaceHolder` con `ID="HeadContent"`:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

Aprire il file Default.aspx. Sostituire il testo boilerplate nella sezione di contenuto principale, come illustrato:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

Successivamente, aggiungere un riferimento al file di origine jQuery nel `HeaderContent` sezione:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

Nota: È possibile facilmente aggiungere il riferimento allo script trascinando e rilasciando il file da **Esplora** nella finestra dell'editor di codice.

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

Sotto il tag di script jQuery, aggiungere il blocco di script seguenti:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

Quando si carica il documento, lo script effettua una richiesta di AJAX per &quot;api/prodotti&quot;. La richiesta restituisce un elenco di prodotti in formato JSON. Lo script aggiunge le informazioni sul prodotto per la tabella HTML.

Quando si esegue l'applicazione, dovrebbe essere simile al seguente:

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
