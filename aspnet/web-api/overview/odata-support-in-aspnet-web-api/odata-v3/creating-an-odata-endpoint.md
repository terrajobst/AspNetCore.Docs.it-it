---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Creazione di un Endpoint OData v3 con API Web 2 | Microsoft Docs
author: MikeWasson
description: Open Data Protocol (OData) è un protocollo di accesso di dati per il web. OData offre un metodo uniforme per strutturare i dati, eseguire query sui dati e modificare i dati...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: bcf089003cfb4233fd2aa70cf669972de46c27b1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362590"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a>Creazione di un Endpoint OData v3 con API Web 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Download progetto completato](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Il [Open Data Protocol](http://www.odata.org/) (OData) è un protocollo di accesso di dati per il web. OData offre un metodo uniforme per strutturare i dati, eseguire query sui dati e modificare il set di dati tramite operazioni CRUD (creare, leggere, aggiornare ed eliminare). OData supporta AtomPub (XML) e formati JSON. OData definisce anche un modo per esporre i metadati relativi a dati. I client possono usare i metadati per individuare le informazioni sul tipo e le relazioni per il set di dati.
> 
> API Web ASP.NET rende più semplice creare un endpoint OData per un set di dati. È possibile controllare esattamente quali operazioni OData supportati dall'endpoint. È possibile ospitare più endpoint OData, insieme a endpoint non OData. Si hanno pieno controllo sul modello di dati, la logica di business back-end e livello dati.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - API Web 2
> - OData versione 3
> - Entity Framework 6
> - [Fiddler Web Debugging Proxy (facoltativo)](http://www.fiddler2.com)
> 
> È stato aggiunto il supporto di Web API OData [ASP.NET e Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650). Tuttavia, questa esercitazione Usa lo scaffolding che è stato aggiunto in Visual Studio 2013.


In questa esercitazione si creerà un semplice endpoint OData che i client possono eseguire query. Si creerà anche un client c# per l'endpoint. Dopo aver completato questa esercitazione, il set successivo di esercitazioni illustrano come aggiungere ulteriori funzionalità, tra cui relazioni tra entità, le azioni, e selezionare $espandere / $.

- [Creare il progetto di Visual Studio](#create-project)
- [Aggiungere un modello di entità](#add-model)
- [Aggiungere un Controller OData](#add-controller)
- [Aggiungere il modello EDM e Route](#edm)
- [Seeding del Database (facoltativo)](#seed-db)
- [Esplorare l'OData Endpoint](#explore)
- [Formati di serializzazione OData](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a>Creare il progetto di Visual Studio

In questa esercitazione si creerà un endpoint OData che supporta operazioni CRUD di base. L'endpoint espone una sola risorsa, un elenco di prodotti. Le esercitazioni successive verranno aggiunti ulteriori funzionalità.

Avviare Visual Studio e selezionare **nuovo progetto** dalla pagina iniziale. E viceversa, il **File** dal menu **New** e quindi **progetto**.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il nodo Visual c#. Sotto **Visual c#**, selezionare **Web**. Selezionare **l'applicazione Web ASP.NET** modello.

![](creating-an-odata-endpoint/_static/image1.png)

Nel **nuovo progetto ASP.NET** finestra di dialogo, seleziona la **vuota** modello. In &quot;aggiungere cartelle e di base di riferimenti per... &quot;, controllare **API Web**. Fare clic su **OK**.

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a>Aggiungere un modello di entità

Un *modello* è un oggetto che rappresenta i dati nell'applicazione. Per questa esercitazione, è necessario un modello che rappresenta un prodotto. Il modello corrisponde al nostro tipo di entità OData.

In Esplora soluzioni fare clic sulla cartella Models. Dal menu di scelta rapida, selezionare **Add** quindi selezionare **classe**.

![](creating-an-odata-endpoint/_static/image3.png)

Nel **Aggiungi nuovo** finestra di dialogo di elemento, denominare la classe &quot;prodotto&quot;.

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> Per convenzione, le classi del modello vengono inserite nella cartella Models. Non devi seguono questa convenzione nei propri progetti, ma è possibile usarlo per questa esercitazione.


Nel file Product.cs, aggiungere la definizione di classe seguente:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

La proprietà ID è la chiave di entità. I client possono eseguire query sui prodotti in base all'ID. Questo campo è anche la chiave primaria nel database di back-end.

A questo punto, compilare il progetto. Nel passaggio successivo, si userà alcune scaffolding di Visual Studio che usa la reflection per trovare il tipo di prodotto.

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a>Aggiungere un Controller OData

Oggetto *controller* è una classe che gestisce le richieste HTTP. È possibile definire un controller separato per ogni set di entità in è un servizio OData. In questa esercitazione si creerà un singolo controller.

In Esplora soluzioni fare clic sulla cartella Controllers. Selezionare **Add** e quindi selezionare **Controller**.

![](creating-an-odata-endpoint/_static/image5.png)

Nel **Add Scaffold** finestra di dialogo, seleziona &quot;Controller Web API 2 OData con azioni, che usa Entity Framework&quot;.

![](creating-an-odata-endpoint/_static/image6.png)

Nel **Aggiungi Controller** finestra di dialogo nome al controller "ProductsController". Selezionare il &quot;usare le azioni del controller asincrono&quot; casella di controllo. Nel **modello** elenco a discesa selezionare la classe di prodotto.

![](creating-an-odata-endpoint/_static/image7.png)

Fare clic su di **nuovo contesto dati...**  pulsante. Lasciare il nome predefinito per il tipo di contesto dei dati e fare clic su **Add**.

![](creating-an-odata-endpoint/_static/image8.png)

Fare clic su Aggiungi nella finestra di dialogo Aggiungi Controller per aggiungere il controller.

![](creating-an-odata-endpoint/_static/image9.png)

Nota: Se viene visualizzato un messaggio di errore che dice &quot;si è verificato un errore di recupero del tipo... &quot;, assicurarsi che è stato compilato il progetto di Visual Studio dopo aver aggiunto la classe di prodotto. Lo scaffolding Usa la reflection per trovare la classe.

![](creating-an-odata-endpoint/_static/image10.png)

Lo scaffolding aggiunge due file di codice al progetto:

- Sarà Products.cs definisce il controller API Web che implementa l'endpoint OData.
- ProductServiceContext.cs fornisce metodi per eseguire query nel database sottostante, tramite Entity Framework.

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a>Aggiungere il modello EDM e Route

In Esplora soluzioni espandere l'App\_avviare cartella e aprire il file WebApiConfig.cs. Questa classe contiene codice di configurazione per l'API Web. Sostituire il codice con quanto segue:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

Questo codice esegue due operazioni:

- Crea un modello EDM (Entity dati Model) per l'endpoint OData.
- Aggiunge una route per l'endpoint.

Un modello EDM è un modello di dati astratto. Il modello EDM viene utilizzato per creare il documento di metadati e definire gli URI per il servizio. Il **ODataConventionModelBuilder** crea un modello EDM, con un set di convenzioni di denominazione predefinito EDM. Questo approccio richiede il minor quantità di codice. Se si desidera maggiore controllo sul modello EDM, è possibile usare la **ODataModelBuilder** classe per creare il modello EDM mediante l'aggiunta di proprietà, le chiavi e le proprietà di navigazione in modo esplicito.

Il **EntitySet** metodo aggiunge una set di entità al modello EDM:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

La stringa "Prodotti" definisce il nome del set di entità. Il nome del controller deve corrispondere al nome del set di entità. In questa esercitazione, il set di entità viene denominato "Prodotti" e il controller `ProductsController`. Se è stata denominata "ProductSet" del set di entità, si potrebbe denominare il controller `ProductSetController`. Si noti che un endpoint può avere più set di entità. Chiamare **EntitySet&lt;T&gt;**  per ogni entità impostata e quindi definire un controller corrispondente.

Il **MapODataRoute** metodo aggiunge una route per l'endpoint OData.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

Il primo parametro è un nome descrittivo per la route. Client del servizio non viene visualizzato il nome specificato. Il secondo parametro è il prefisso URI per l'endpoint. Questo codice, l'URI per il set di entità prodotti è http://<em>hostname</em>  /odata/prodotti. L'applicazione può avere più di un endpoint OData. Per ogni endpoint, chiamare <strong>MapODataRoute</strong> e fornire un nome di route univoco e un prefisso URI univoco.

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a>Seeding del Database (facoltativo)

In questo passaggio si userà Entity Framework per effettuare il seeding del database con alcuni dati di test. Questo passaggio è facoltativo, ma consente di testare l'endpoint OData sin da subito.

Dal **degli strumenti** dal menu **Library Package Manager**, quindi selezionare **Package Manager Console**. Nella finestra della Console di gestione pacchetti immettere il comando seguente:

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

Aggiunge una cartella denominata le migrazioni e il file di codice Configuration.cs.

![](creating-an-odata-endpoint/_static/image12.png)

Aprire il file e aggiungere il codice seguente per il `Configuration.Seed` (metodo).

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

Nella finestra della Console di gestione pacchetti immettere i comandi seguenti:

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

Questi comandi generano codice che crea il database e quindi esegue tale codice.

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a>Esplorare l'OData Endpoint

In questa sezione si userà il [Fiddler Web Debugging Proxy](http://www.fiddler2.com) per inviare richieste all'endpoint ed esaminare i messaggi di risposta. Ciò consentirà di comprendere le capacità di un endpoint OData.

In Visual Studio, premere F5 per avviare il debug. Per impostazione predefinita, Visual Studio apre nel browser l'indirizzo `http://localhost:*port*`, dove *porta* è il numero di porta configurato nelle impostazioni del progetto.

È possibile modificare il numero di porta nelle impostazioni del progetto. In Esplora soluzioni fare clic sul progetto e selezionare **proprietà**. Nella finestra Proprietà, selezionare **Web**. Immettere il numero di porta sotto **Url del progetto**.

### <a name="service-document"></a>Documento di servizio

Il *documento di servizio* contiene un elenco dei set di entità per l'endpoint OData. Per ottenere il documento di servizio, inviare una richiesta GET all'URI del servizio di radice.

Uso di Fiddler, immettere l'URI seguente nel **Composer** scheda: `http://localhost:port/odata/`, dove *porta* è il numero di porta.

![](creating-an-odata-endpoint/_static/image13.png)

Scegliere il **Execute** pulsante. Fiddler invia una richiesta HTTP GET all'applicazione. Verrà visualizzata la risposta nell'elenco di sessioni di Web. Se tutto funziona, il codice di stato sarà 200.

![](creating-an-odata-endpoint/_static/image14.png)

Fare doppio clic la risposta dell'elenco di sessioni Web per visualizzare i dettagli del messaggio di risposta nella scheda Inspectors.

![](creating-an-odata-endpoint/_static/image15.png)

Il messaggio di risposta HTTP non elaborato dovrebbe essere simile al seguente:

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

Per impostazione predefinita, API Web restituisce il documento di servizio in formato AtomPub. Per richiedere JSON, aggiungere la seguente intestazione alla richiesta HTTP:

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

Ora la risposta HTTP contiene un payload JSON:

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a>Documento di metadati di servizio

Il *documento di metadati di servizio* descrive il modello di dati del servizio, usando un linguaggio XML denominato Conceptual Schema Definition Language (CSDL). Il documento di metadati viene illustrata la struttura dei dati nel servizio e può essere utilizzato per generare codice client.

Per ottenere il documento di metadati, inviare una richiesta GET a `http://localhost:port/odata/$metadata`. Ecco i metadati per l'endpoint mostrato in questa esercitazione.

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a>Set di entità

Per ottenere il set di entità prodotti, inviare una richiesta GET a `http://localhost:port/odata/Products`.

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a>Entità

Per ottenere un singolo prodotto, inviare una richiesta GET a `http://localhost:port/odata/Products(1)`, dove "1" è l'ID prodotto.

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a>Formati di serializzazione OData

OData supporta diversi formati di serializzazione:

- Atom Pub (XML)
- JSON "light" (introdotta in OData v3)
- JSON "verbose" (OData v2)

Per impostazione predefinita, API Web Usa AtomPubJSON "light" formato. 

Per ottenere il formato AtomPub, impostare l'intestazione Accept su "application/atom + xml". Di seguito è riportato un corpo della risposta di esempio:

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

È possibile visualizzare uno svantaggio ovvio del formato Atom: è molto più dettagliato rispetto a JSON light. Tuttavia, se si dispone di un client in grado di interpretare AtomPub, il client potrebbe preferire tale formato JSON.

Ecco la versione light di JSON della stessa entità:

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

Il formato JSON light è stata introdotta nella versione 3 del protocollo OData. Per garantire la compatibilità con le versioni precedenti, un client può richiedere il formato JSON "dettagliato" precedente. Per richiedere JSON dettagliato, impostare l'intestazione Accept `application/json;odata=verbose`. Ecco la versione dettagliata:

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

Questo formato fornisce più metadati nel corpo della risposta, che è possibile aggiungere un notevole sovraccarico su un'intera sessione. Inoltre, aggiunge un livello di riferimento indiretto eseguendo il wrapping dell'oggetto in una proprietà denominata "d".

## <a name="next-steps"></a>Passaggi successivi

- [Aggiungere relazioni tra entità](working-with-entity-relations.md)
- [Aggiungere le azioni OData](odata-actions.md)
- [Chiamare il servizio OData da un Client .NET](calling-an-odata-service-from-a-net-client.md)
