---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Introduzione a ASP.NET Web API 2 (c#)
author: MikeWasson
description: HTTP non è disponibile solo per mettere a disposizione le pagine web. È anche una potente piattaforma per la compilazione di API che espongono servizi e i dati. HTTP è semplice, flessibile e ubiq...
ms.author: riande
ms.date: 11/28/2017
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7bec95af4532535f0d620bfe6862958907466874
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444259"
---
<a name="get-started-with-aspnet-web-api-2-c"></a>Introduzione a ASP.NET Web API 2 (c#)
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Download progetto completato](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

HTTP non è disponibile solo per mettere a disposizione le pagine web. HTTP è anche una potente piattaforma per la compilazione di API che espongono servizi e i dati. HTTP è semplice, flessibile e universale. Quasi tutte le piattaforme che è possibile pensare dispone di una libreria HTTP, in modo che i servizi HTTP possono raggiungere una vasta gamma di client, inclusi i browser, dispositivi mobili e applicazioni desktop tradizionali.

API Web ASP.NET è un framework per la compilazione di API web in .NET Framework. In questa esercitazione si utilizzerà API Web ASP.NET per creare un'API web che restituisce un elenco di prodotti.

## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione

- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
- API Web 2

Visualizzare [creare un'API web con ASP.NET Core e Visual Studio per Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) per una versione più recente di questa esercitazione.

## <a name="create-a-web-api-project"></a>Creare un progetto API Web

In questa esercitazione si utilizzerà API Web ASP.NET per creare un'API web che restituisce un elenco di prodotti. La pagina web front-end Usa jQuery per visualizzare i risultati.

![](tutorial-your-first-web-api/_static/image1.png)

Avviare Visual Studio e selezionare **nuovo progetto** dalle **avviare** pagina. E viceversa, il **File** dal menu **New** e quindi **progetto**.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere le **Visual c#** nodo. Sotto **Visual c#**, selezionare **Web**. Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET**. Denominare il progetto "ProductsApp" e fare clic su **OK**.

![](tutorial-your-first-web-api/_static/image2.png)

Nel **nuovo progetto ASP.NET** finestra di dialogo, seleziona la **vuota** modello. Sotto &quot;aggiungere le cartelle e riferimenti per di base&quot;, controllare **API Web**. Fare clic su **OK**.

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> È anche possibile creare un progetto API Web usando il &quot;API Web&quot; modello. Il modello API Web Usa MVC di ASP.NET per fornire pagine della Guida dell'API. Sto usando il modello vuoto per questa esercitazione perché si desidera mostrare API Web senza MVC. In generale, non è necessario conoscere ASP.NET MVC per utilizzare l'API Web.


## <a name="adding-a-model"></a>Aggiunta di un modello

Un *modello* è un oggetto che rappresenta i dati nell'applicazione. API Web ASP.NET può serializzare automaticamente il modello JSON, XML o un altro formato e quindi scrivere i dati serializzati nel corpo del messaggio di risposta HTTP. Fino a quando un client può leggere il formato di serializzazione, è possibile deserializzare l'oggetto. La maggior parte dei client consente di analizzare XML o JSON. Inoltre, il client può indicare quale formato deve impostando l'intestazione Accept del messaggio di richiesta HTTP.

Iniziamo creando un modello semplice che rappresenta un prodotto.

Se Esplora soluzioni non è già visibile, scegliere il **View** menu e selezionare **Esplora soluzioni**. In Esplora soluzioni fare clic sulla cartella Models. Dal menu di scelta rapida, selezionare **Add** quindi selezionare **classe**.

![](tutorial-your-first-web-api/_static/image4.png)

Denominare la classe &quot;prodotto&quot;. Aggiungere le seguenti proprietà per il `Product` classe.

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a>Aggiunta di un controller

Nell'API Web, un *controller* è un oggetto che gestisce le richieste HTTP. Si aggiungerà un controller che può restituire un elenco di prodotti o un singolo prodotto specificato dall'ID.

> [!NOTE]
> Se si usa ASP.NET MVC, si ha familiarità con i controller. Controller Web API sono simili ai controller MVC, ma ereditano la **ApiController** classe anziché il **Controller** classe.

Nelle **Esplora soluzioni**, fare clic sulla cartella Controllers. Selezionare **Add** e quindi selezionare **Controller**.

![](tutorial-your-first-web-api/_static/image5.png)

Nel **Add Scaffold** finestra di dialogo, seleziona **Web API Controller - Empty**. Fare clic su **Aggiungi**.

![](tutorial-your-first-web-api/_static/image6.png)

Nel **Aggiungi Controller** finestra di dialogo nome al controller &quot;ProductsController&quot;. Fare clic su **Aggiungi**.

![](tutorial-your-first-web-api/_static/image7.png)

Lo scaffolding consente di creare un file denominato ProductsController.cs nella cartella controller.

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> Non è necessario inserire i controller in una cartella denominata controller. Il nome della cartella è semplicemente un modo pratico per organizzare i file di origine.


Se non è già aperto questo file, fare doppio clic sul file per aprirlo. Sostituire il codice in questo file con il codice seguente:

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

Per semplificare l'esempio, i prodotti vengono archiviati in una matrice fissa all'interno della classe controller. Naturalmente, in un'applicazione reale, si sarebbe eseguire query su un database o usare un'altra origine dati esterna.

Il controller definisce due metodi che restituiscono i prodotti:

- Il `GetAllProducts` metodo viene restituito l'intero elenco di prodotti come un' **IEnumerable&lt;prodotto&gt;**  tipo.
- Il `GetProduct` metodo cerca un singolo prodotto dal relativo ID.

La procedura è terminata. Si dispone di un'API web di lavoro. Ogni metodo nel controller corrisponde a uno o più URI:

| Metodo del controller | URI |
| --- | --- |
| GetAllProducts | prodotti/api / |
| GetProduct | /api/products/*id* |

Per il `GetProduct` metodo, il *id* nell'URI è un segnaposto. Per ottenere il prodotto con ID 5, ad esempio, l'URI è `api/products/5`.

Per altre informazioni su come API Web instrada le richieste HTTP per i metodi del controller, vedere [Routing in API Web ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="calling-the-web-api-with-javascript-and-jquery"></a>Chiamare l'API Web con Javascript e jQuery

In questa sezione si aggiungerà una pagina HTML che viene utilizzato AJAX per chiamare l'API web. Useremo jQuery per effettuare le chiamate AJAX e aggiornare la pagina con i risultati.

In Esplora soluzioni fare clic sul progetto e selezionare **Add**, quindi selezionare **nuovo elemento**.

![](tutorial-your-first-web-api/_static/image9.png)

Nel **Aggiungi nuovo elemento** finestra di dialogo, seleziona la **Web** nodo sotto **Visual c#** e quindi selezionare il **pagina HTML** elemento. Denominare la pagina &quot;index. HTML&quot;.

![](tutorial-your-first-web-api/_static/image10.png)

Sostituire tutto il contenuto in questo file con il codice seguente:

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

È possibile ottenere jQuery in vari modi. In questo esempio, ho utilizzato il [rete CDN Microsoft Ajax](../../../ajax/cdn/overview.md). È anche possibile scaricarlo dal [ http://jquery.com/ ](http://jquery.com/), ASP.NET "API Web" e modello di progetto include anche jQuery.

### <a name="getting-a-list-of-products"></a>Come ottenere un elenco di prodotti

Per ottenere un elenco di prodotti, inviare una richiesta HTTP GET a &quot;/api/prodotti&quot;.

Il componente aggiuntivo jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) funzione invia una richiesta AJAX. Per la risposta contiene una matrice di oggetti JSON. Il `done` funzione specifica un callback che viene chiamato se la richiesta ha esito positivo. Nella richiamata, si aggiorna DOM con le informazioni sul prodotto.

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a>Recupero di un prodotto base all'ID

Per ottenere un prodotto dall'ID, inviare una richiesta HTTP GET a &quot;/API/prodotti/*id*&quot;, dove *id* è l'ID prodotto.

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

È comunque chiamare `getJSON` per inviare la richiesta AJAX, ma questa volta l'ID è put nell'URI della richiesta. La risposta da questa richiesta è una rappresentazione JSON di un singolo prodotto.

## <a name="running-the-application"></a>Esecuzione dell'applicazione

Premere F5 per avviare il debug dell'applicazione. La pagina web dovrebbe essere simile al seguente:

![](tutorial-your-first-web-api/_static/image11.png)

Per ottenere un prodotto dall'ID, immettere l'ID e fare clic su Cerca:

![](tutorial-your-first-web-api/_static/image12.png)

Se si immette un ID non valido, il server restituisce un errore HTTP:

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a>Utilizzo F12 per visualizzare la richiesta e risposta HTTP

Quando si lavora con un servizio HTTP, può essere molto utile per visualizzare la richiesta HTTP e i messaggi di richiesta. È possibile farlo usando gli strumenti di sviluppo F12 di Internet Explorer 9. Personalizzazione di Internet Explorer 9, premere **F12** per aprire gli strumenti. Fare clic sui **Network** scheda e premere **Avvia acquisizione**. Tornare ora alla pagina web e premere **F5** per ricaricare la pagina web. Internet Explorer consente di acquisire il traffico HTTP tra il browser e il server web. La visualizzazione riepilogo Mostra tutto il traffico di rete per una pagina:

![](tutorial-your-first-web-api/_static/image14.png)

Individuare la voce per l'URI relativo "api/prodotti /". Selezionare questa voce e fare clic su **passare alla visualizzazione dettagliata**. Nella visualizzazione dettagli sono disponibili schede per visualizzare le intestazioni di richiesta e risposta e corpi. Ad esempio, se si sceglie la **intestazioni della richiesta** scheda, è possibile vedere che il client ha richiesto &quot;application/json&quot; nell'intestazione Accept.

![](tutorial-your-first-web-api/_static/image15.png)

Se si seleziona la scheda di corpo della risposta, è possibile visualizzare come l'elenco dei prodotti è stato serializzato in JSON. Altri browser hanno una funzionalità simile. È un altro strumento utile [Fiddler](http://www.fiddler2.com/fiddler2/), un proxy di debug web. È possibile usare Fiddler per visualizzare il traffico HTTP, nonché per comporre le richieste HTTP, che offre il controllo completo su intestazioni HTTP nella richiesta.

## <a name="see-this-app-running-on-azure"></a>Vedere questa App in esecuzione in Azure

Si desidera vedere il sito completo in esecuzione come un'app web in tempo reale? È possibile distribuire una versione completa dell'app al tuo account Azure, facendo semplicemente clic sul pulsante seguente.

[![](https://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

È necessario un account di Azure per distribuire questa soluzione in Azure. Se non hai già un account, sono disponibili le opzioni seguenti:

- [Aprire un account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -ricevono crediti è possibile usare per provare i servizi di Azure a pagamento e anche dopo che sono abituati fino è possibile mantenere l'account e usare i servizi Azure gratuiti.
- [Attivare i benefici della sottoscrizione MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -la sottoscrizione MSDN si accumulano crediti ogni mese in cui è possibile usare per i servizi di Azure a pagamento.

## <a name="next-steps"></a>Passaggi successivi

- Per un esempio più completo di un servizio HTTP che supporta azioni di POST, PUT e DELETE e scrive in un database, vedere [usando API Web 2 con Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).
- Per altre informazioni sulla creazione di applicazioni web fluide e reattive all'inizio di un servizio HTTP, vedere [applicazione a pagina singola ASP.NET](../../../single-page-application/index.md).
- Per informazioni su come distribuire un progetto web Visual Studio in servizio App di Azure, vedere [creare un'app web ASP.NET in servizio App di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
