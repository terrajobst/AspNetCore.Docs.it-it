---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Introduzione a ASP.NET Web API 2 (c#)
author: MikeWasson
description: HTTP non è disponibile solo per mettere a disposizione le pagine web. È anche una potente piattaforma per la compilazione di API che espongono servizi e dei dati. HTTP è semplice e flessibile e ubiq...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/28/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: d881563cdb6449aada444ef0528061581113a925
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2018
---
<a name="get-started-with-aspnet-web-api-2-c"></a>Introduzione a ASP.NET Web API 2 (c#)
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare il progetto completato](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

HTTP non è disponibile solo per mettere a disposizione le pagine web. HTTP è una potente piattaforma per la compilazione di API che espongono servizi e dei dati. HTTP è semplice, flessibile e universale. Quasi tutte le piattaforme che è possibile considerare dispone di una libreria HTTP, pertanto i servizi HTTP possono raggiungere una vasta gamma di client, inclusi browser e i dispositivi mobili, applicazioni desktop tradizionali.
 
API Web ASP.NET è un framework per la creazione di web API .NET Framework. In questa esercitazione si utilizzerà ASP.NET Web API per creare una web API che restituisce un elenco di prodotti.

 
 ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
  
 - [Visual Studio 2017](https://www.visualstudio.com/downloads/)
 - Web API 2

Vedere [creare un'API web con ASP.NET Core e Visual Studio per Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) per una versione più recente di questa esercitazione.

## <a name="create-a-web-api-project"></a>Creare un progetto di API Web

In questa esercitazione si utilizzerà ASP.NET Web API per creare una web API che restituisce un elenco di prodotti. La pagina web front-end utilizza jQuery per visualizzare i risultati.

![](tutorial-your-first-web-api/_static/image1.png)

Avviare Visual Studio e selezionare **nuovo progetto** dal **avviare** pagina. O dal **File** dal menu **New** e quindi **progetto**.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il **Visual c#** nodo. In **Visual c#** selezionare **Web**. Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET**. Denominare il progetto "ProductsApp" e fare clic su **OK**.

![](tutorial-your-first-web-api/_static/image2.png)

Nel **nuovo progetto ASP.NET** finestra di dialogo Seleziona il **vuoto** modello. In &quot;aggiungere cartelle e i riferimenti per core&quot;, controllare **API Web**. Fare clic su **OK**.

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> È inoltre possibile creare un progetto di API Web utilizzando il &quot;API Web&quot; modello. Il modello API Web utilizza ASP.NET MVC per fornire le pagine della Guida di API. Si utilizza il modello vuoto per questa esercitazione, poiché si desidera mostrare API Web senza MVC. In generale, non è necessario conoscere ASP.NET MVC per utilizzare l'API Web.


## <a name="adding-a-model"></a>Aggiunta di un modello

Un *modello* è un oggetto che rappresenta i dati nell'applicazione. ASP.NET Web API può serializzare automaticamente il modello a JSON, XML o un altro formato e quindi scrivere i dati serializzati nel corpo del messaggio di risposta HTTP. Fino a quando un client può leggere il formato di serializzazione, è possibile deserializzare l'oggetto. La maggior parte dei client consente di analizzare XML o JSON. Inoltre, il client può indicare il formato deve impostando l'intestazione Accept nel messaggio di richiesta HTTP.

Iniziamo creando un modello semplice che rappresenta un prodotto.

Se Esplora soluzioni non è visibile, fare clic su di **vista** dal menu **Esplora**. In Esplora soluzioni, fare clic sulla cartella Models. Dal menu di scelta rapida, selezionare **Aggiungi** selezionare **classe**.

![](tutorial-your-first-web-api/_static/image4.png)

Nome della classe &quot;prodotto&quot;. Aggiungere le seguenti proprietà per il `Product` classe.

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a>Aggiunta di un controller

Nell'API Web, un *controller* è un oggetto che gestisce le richieste HTTP. Verrà aggiunto un controller che può restituire un elenco di prodotti o un singolo prodotto specificato dall'ID.

> [!NOTE]
> Se si utilizza ASP.NET MVC, si ha già familiarità con controller. Controller Web API sono simili ai controller MVC, ma ereditano la **ApiController** classe anziché il **Controller** classe.

In **Esplora**, fare clic sulla cartella controller. Selezionare **Aggiungi** e quindi selezionare **Controller**.

![](tutorial-your-first-web-api/_static/image5.png)

Nel **aggiungere lo scaffolding** finestra di dialogo Seleziona **Controller API Web - vuoto**. Fare clic su **Aggiungi**.

![](tutorial-your-first-web-api/_static/image6.png)

Nel **Aggiungi Controller** finestra di dialogo, nome del controller &quot;ProductsController&quot;. Fare clic su **Aggiungi**.

![](tutorial-your-first-web-api/_static/image7.png)

Lo scaffolding consente di creare un file denominato ProductsController.cs nella cartella controller.

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> Non è necessario inserire i controller in una cartella denominata controller. Il nome della cartella è semplicemente un modo pratico per organizzare i file di origine.


Se questo file non è già aperto, fare doppio clic sul file per aprirlo. Sostituire il codice in questo file con le operazioni seguenti:

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

Per semplificare l'esempio, i prodotti vengono archiviati in una matrice fissa all'interno della classe controller. In un'applicazione reale, naturalmente, sarebbe query a un database o utilizzare un'altra origine dati esterna.

Il controller definisce due metodi che restituiscono i prodotti:

- Il `GetAllProducts` il metodo restituisce l'intero elenco dei prodotti come un **IEnumerable&lt;prodotto&gt;**  tipo.
- Il `GetProduct` metodo cerca un singolo prodotto dal relativo ID.

La procedura è terminata. È un'API web di lavoro. Ogni metodo sul controller corrisponde a uno o più URI:

| Metodo del controller | URI |
| --- | --- |
| GetAllProducts | prodotti/api / |
| GetProduct | /api/products/*id* |

Per il `GetProduct` (metodo), il *id* nell'URI è un segnaposto. Per ottenere il prodotto con ID 5, ad esempio, l'URI è `api/products/5`.

Per ulteriori informazioni su come API Web indirizza le richieste HTTP per i metodi del controller, vedere [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="calling-the-web-api-with-javascript-and-jquery"></a>Chiama l'API Web con Javascript e jQuery

In questa sezione verrà aggiunto una pagina HTML che viene utilizzato AJAX per chiamare l'API web. Per eseguire le chiamate AJAX e aggiornare la pagina dei risultati, si userà jQuery.

In Esplora soluzioni, fare clic sul progetto e selezionare **Aggiungi**, quindi selezionare **nuovo elemento**.

![](tutorial-your-first-web-api/_static/image9.png)

Nel **Aggiungi nuovo elemento** finestra di dialogo Seleziona il **Web** nodo **Visual c#** e quindi selezionare il **pagina HTML** elemento. Denominare la pagina &quot;index.html&quot;.

![](tutorial-your-first-web-api/_static/image10.png)

Sostituire tutto il contenuto in questo file con le operazioni seguenti:

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

Esistono diversi modi per ottenere jQuery. In questo esempio utilizzato il [Microsoft Ajax CDN](../../../ajax/cdn/overview.md). È anche possibile scaricarlo da [ http://jquery.com/ ](http://jquery.com/), ASP.NET "API Web" e il modello di progetto include anche jQuery.

### <a name="getting-a-list-of-products"></a>Come ottenere un elenco di prodotti

Per ottenere un elenco di prodotti, inviare una richiesta HTTP GET per &quot;/api/prodotti&quot;.

Il jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) funzione invia una richiesta AJAX. Per la risposta contiene una matrice di oggetti JSON. Il `done` funzione specifica un callback che viene chiamato se la richiesta ha esito positivo. Nella richiamata, DOM viene aggiornato con le informazioni sul prodotto.

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a>Recupero di un prodotto in base all'ID

Per ottenere un prodotto dall'ID, inviare una richiesta HTTP GET per &quot;/api/prodotti/*id*&quot;, dove *id* è l'ID prodotto.

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

È comunque possibile chiamare `getJSON` per inviare la richiesta AJAX, ma questa volta l'ID è stato inserito in URI della richiesta. La risposta da questa richiesta è una rappresentazione JSON di un singolo prodotto.

## <a name="running-the-application"></a>Esecuzione dell'applicazione

Premere F5 per avviare il debug dell'applicazione. La pagina web dovrebbe essere simile al seguente:

![](tutorial-your-first-web-api/_static/image11.png)

Per ottenere un prodotto dall'ID, immettere l'ID e fare clic su Cerca:

![](tutorial-your-first-web-api/_static/image12.png)

Se si immette un ID non valido, il server restituisce un errore HTTP:

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a>Utilizzo di F12 per visualizzare la richiesta e risposta HTTP

Quando si lavora con un servizio HTTP, può essere molto utile per visualizzare la richiesta HTTP e messaggi di richiesta. È possibile farlo tramite gli strumenti di sviluppo F12 in Internet Explorer 9. In Internet Explorer 9, premere **F12** per aprire gli strumenti. Fare clic su di **rete** scheda e premere **Avvia acquisizione**. Passare alla pagina web e premere **F5** a ricaricare la pagina web. Internet Explorer consente di acquisire il traffico HTTP tra il browser e il server web. La visualizzazione di riepilogo Mostra tutto il traffico di rete per una pagina:

![](tutorial-your-first-web-api/_static/image14.png)

Individuare la voce per l'URI relativo "api/prodotti /". Selezionare questa voce e fare clic su **passare alla visualizzazione dettagliata**. Nella vista di dettaglio, sono disponibili schede per visualizzare le intestazioni di richiesta e risposta e corpi. Ad esempio, se si sceglie il **le intestazioni di richiesta** scheda, è possibile vedere che il client ha richiesto &quot;application/json&quot; nell'intestazione Accept.

![](tutorial-your-first-web-api/_static/image15.png)

Se si fa clic su scheda corpo della risposta, è possibile visualizzare come l'elenco dei prodotti è stato serializzato in JSON. Altri browser hanno una funzionalità simile. Un altro strumento utile è [Fiddler](http://www.fiddler2.com/fiddler2/), un web proxy per debug. È possibile utilizzare Fiddler per visualizzare il traffico HTTP e per comporre le richieste HTTP, che consente il controllo completo di intestazioni HTTP nella richiesta.

## <a name="see-this-app-running-on-azure"></a>Vedere l'App in esecuzione in Azure

Si desidera vedere il sito completo in esecuzione come un'app web in tempo reale? È possibile distribuire una versione completa dell'app al proprio account Azure, facendo semplicemente clic sul pulsante seguente.

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

È necessario un account Azure per distribuire questa soluzione in Azure. Se si dispone già di un account, sono disponibili le opzioni seguenti:

- [Aprire un account Azure, gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -si ottiene crediti è possibile utilizzare per provare i servizi di Azure a pagamento e anche dopo l'uso massimo è possibile mantenere l'account e utilizzare senza servizi di Azure.
- [Attivare i benefici per sottoscrittori MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Your sottoscrizione MSDN offre crediti ogni mese in cui è possibile utilizzare per i servizi di Azure a pagamento.

## <a name="next-steps"></a>Passaggi successivi

- Per un esempio più completo di un servizio HTTP che supporta azioni di POST, PUT e DELETE e scrive in un database, vedere [mediante API Web 2 con Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).
- Per ulteriori informazioni sulla creazione di applicazioni web flessibile ed efficiente all'inizio di un servizio HTTP, vedere [applicazione a pagina singola ASP.NET](../../../single-page-application/index.md).
- Per informazioni su come distribuire un progetto web di Visual Studio in Azure App Service, vedere [creare un'app web ASP.NET in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
