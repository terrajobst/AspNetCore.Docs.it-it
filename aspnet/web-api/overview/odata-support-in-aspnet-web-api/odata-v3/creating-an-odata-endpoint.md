---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Creazione di un Endpoint di OData v3 con Web API 2 | Documenti Microsoft
author: MikeWasson
description: "Open Data Protocol (OData) è un protocollo di accesso ai dati per il web. OData fornisce un metodo uniforme per strutturare i dati, eseguire query sui dati e modificare i dati..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 33fe4d764bf9bf64c852f1269255925b5cc42536
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2018
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a>Creazione di un Endpoint di OData v3 con Web API 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Il [Open Data Protocol](http://www.odata.org/) (OData) è un protocollo di accesso ai dati per il web. OData fornisce un metodo uniforme per strutturare i dati, eseguire query sui dati e di modificare il set di dati tramite operazioni CRUD (creare, leggere, aggiornare ed eliminare). OData supporta AtomPub (XML) e formati JSON. OData definisce inoltre un modo per esporre i metadati sui dati. I client possono usare i metadati per individuare le informazioni sul tipo e le relazioni per il set di dati.
> 
> ASP.NET Web API rende più semplice creare un endpoint OData per un set di dati. È possibile controllare esattamente quali operazioni OData supportati dall'endpoint. È possibile ospitare più endpoint OData, insieme a endpoint non OData. Si dispone di controllo completo sul modello di dati e logica di business back-end, livello dati.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web API 2
> - OData versione 3
> - Entity Framework 6
> - [Web Fiddler debug Proxy (facoltativo)](http://www.fiddler2.com)
> 
> È stato aggiunto il supporto di Web API OData in [ASP.NET e aggiornamento di 2012.2 degli strumenti Web](https://go.microsoft.com/fwlink/?LinkId=282650). Tuttavia, questa esercitazione Usa lo scaffolding che è stato aggiunto in Visual Studio 2013.


In questa esercitazione si creerà un endpoint OData semplice che i client possono eseguire query. Si creerà anche un client c# per l'endpoint. Dopo aver completato questa esercitazione, il set successivo di esercitazioni viene illustrato come aggiungere ulteriori funzionalità, tra cui le relazioni di entità, azioni, quindi espandere $/ $.

- [Creare il progetto di Visual Studio](#create-project)
- [Aggiungere un modello di entità](#add-model)
- [Aggiungere un Controller OData](#add-controller)
- [Aggiungere tutte le Route EDM](#edm)
- [Valore di inizializzazione del Database (facoltativo)](#seed-db)
- [Esplorare l'OData Endpoint](#explore)
- [Formati di serializzazione di OData](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a>Creare il progetto di Visual Studio

In questa esercitazione si creerà un endpoint OData che supporta operazioni CRUD di base. L'endpoint espone una singola risorsa, un elenco di prodotti. Le esercitazioni successive verranno aggiunti ulteriori funzionalità.

Avviare Visual Studio e selezionare **nuovo progetto** dalla pagina iniziale. O dal **File** dal menu **New** e quindi **progetto**.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il nodo Visual c#. In **Visual c#**selezionare **Web**. Selezionare **dell'applicazione Web ASP.NET** modello.

![](creating-an-odata-endpoint/_static/image1.png)

Nel **nuovo progetto ASP.NET** finestra di dialogo Seleziona il **vuoto** modello. In &quot;aggiungere cartelle e i riferimenti per core... &quot;, controllare **API Web**. Fare clic su **OK**.

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a>Aggiungere un modello di entità

Un *modello* è un oggetto che rappresenta i dati nell'applicazione. Per questa esercitazione, è necessario un modello che rappresenta un prodotto. Il modello corrisponde a questo tipo di entità OData.

In Esplora soluzioni, fare clic sulla cartella Models. Dal menu di scelta rapida, selezionare **Aggiungi** selezionare **classe**.

![](creating-an-odata-endpoint/_static/image3.png)

Nel **Aggiungi nuovo** elemento finestra di dialogo, assegnare il nome della classe &quot;prodotto&quot;.

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> Per convenzione, le classi del modello vengono inserite nella cartella Models. Non è necessario seguire questa convenzione nei propri progetti, ma verrà usato per questa esercitazione.


Nel file Product.cs, aggiungere la seguente definizione di classe:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

La proprietà ID è la chiave di entità. I client possono eseguire query prodotti da ID. Questo campo potrebbe anche essere la chiave primaria nel database back-end.

Compilare il progetto ora. Nel passaggio successivo, si userà alcune funzionalità di scaffolding di Visual Studio utilizza la reflection per trovare il tipo di prodotto.

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a>Aggiungere un Controller OData

Oggetto *controller* è una classe che gestisce le richieste HTTP. È possibile definire un controller separato per ogni set di entità in è un servizio OData. In questa esercitazione si creerà un singolo controller.

In Esplora soluzioni, fare clic sulla cartella controller. Selezionare **Aggiungi** e quindi selezionare **Controller**.

![](creating-an-odata-endpoint/_static/image5.png)

Nel **aggiungere lo scaffolding** finestra di dialogo Seleziona &quot;Web API 2 OData Controller con azioni, mediante Entity Framework&quot;.

![](creating-an-odata-endpoint/_static/image6.png)

Nel **Aggiungi Controller** finestra di dialogo, nome del controller "ProductsController". Selezionare il &quot;utilizzare azioni asincrone del controller&quot; casella di controllo. Nel **modello** elenco a discesa, selezionare la classe di prodotto.

![](creating-an-odata-endpoint/_static/image7.png)

Fare clic su di **nuovo contesto dati...**  pulsante. Lasciare il nome predefinito per il tipo di contesto dati e fare clic su **Aggiungi**.

![](creating-an-odata-endpoint/_static/image8.png)

Nella finestra di dialogo Aggiungi Controller per aggiungere il controller, fare clic su Aggiungi.

![](creating-an-odata-endpoint/_static/image9.png)

Nota: Se viene visualizzato un messaggio di errore indicante che &quot;si è verificato un errore durante il recupero del tipo... &quot;, assicurarsi che è stato compilato il progetto di Visual Studio dopo aver aggiunto la classe di prodotto. Lo scaffolding utilizza la reflection per trovare la classe.

![](creating-an-odata-endpoint/_static/image10.png)

Lo scaffolding aggiunge due file di codice al progetto:

- Sarà Products.cs definisce il controller API Web che implementa l'endpoint OData.
- ProductServiceContext.cs fornisce metodi per eseguire query sul database sottostante, mediante Entity Framework.

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a>Aggiungere tutte le Route EDM

In Esplora soluzioni, espandere l'applicazione\_avviare cartella e aprire il file denominato WebApiConfig.cs. Questa classe contiene codice di configurazione per l'API Web. Sostituire il codice con quanto segue:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

Questo codice eseguite due operazioni:

- Crea un Entity Data Model (EDM) per l'endpoint OData.
- Aggiunge una route per l'endpoint.

Un modello EDM è un modello di dati astratto. EDM è utilizzato per creare il documento di metadati e definire gli URI per il servizio. Il **ODataConventionModelBuilder** crea un modello EDM tramite un set di convenzioni di denominazione predefinite EDM. Questo approccio richiede il minor quantità di codice. Se si desidera controllare più EDM, è possibile utilizzare il **ODataModelBuilder** classe per creare il modello EDM aggiungendo in modo esplicito le proprietà, le chiavi e le proprietà di navigazione.

Il **EntitySet** metodo aggiunge una set di entità a EDM:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

La stringa "Prodotti" definisce il nome del set di entità. Il nome del controller deve corrispondere al nome del set di entità. In questa esercitazione, il set di entità viene denominato "Prodotti" e il controller `ProductsController`. Se è denominato "ProductSet" del set di entità, è necessario denominare il controller `ProductSetController`. Si noti che un endpoint può avere più set di entità. Chiamare **EntitySet&lt;T&gt;**  impostata per ogni entità e quindi definire un controller corrispondente.

Il **MapODataRoute** metodo aggiunge una route per l'endpoint OData.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

Il primo parametro è un nome descrittivo per la route. I client del servizio non vengono visualizzati il nome specificato. Il secondo parametro è il prefisso URI per l'endpoint. Dato questo codice, l'URI per il set di entità Products è http://*hostname*  /odata/prodotti. L'applicazione può avere più di un endpoint OData. Per ogni endpoint, chiamare **MapODataRoute** e fornire un nome univoco della route e un prefisso URI univoco.

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a>Valore di inizializzazione del Database (facoltativo)

In questo passaggio, si utilizzerà Entity Framework per il seeding del database con alcuni dati di test. Questo passaggio è facoltativo, ma consente di testare l'endpoint OData immediatamente.

Dal **strumenti** dal menu **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**. Nella finestra della Console di gestione pacchetti, immettere il comando seguente:

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

Aggiunge una cartella denominata migrazioni e un file di codice denominato Configuration.cs.

![](creating-an-odata-endpoint/_static/image12.png)

Aprire il file e aggiungere il codice seguente per il `Configuration.Seed` metodo.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

Nella finestra della Console di gestione pacchetti, immettere i comandi seguenti:

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

Questi comandi generano codice che crea il database e quindi esegue il codice.

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a>Esplorare l'OData Endpoint

In questa sezione si userà il [Proxy di debug Web Fiddler](http://www.fiddler2.com) per inviare richieste all'endpoint ed esaminare i messaggi di risposta. Ciò consentirà di comprendere le capacità di un endpoint OData.

In Visual Studio, premere F5 per avviare il debug. Per impostazione predefinita, Visual Studio apre il browser in `http://localhost:*port*`, dove *porta* è il numero di porta configurato nelle impostazioni del progetto.

È possibile modificare il numero di porta nelle impostazioni del progetto. In Esplora soluzioni, fare clic sul progetto e selezionare **proprietà**. Nella finestra Proprietà selezionare **Web**. Immettere il numero di porta in **Url progetto**.

### <a name="service-document"></a>Documento di servizio

Il *documento di servizio* contiene un elenco di set di entità per l'endpoint OData. Per ottenere il documento di servizio, inviare una richiesta GET all'URI del servizio radice.

Tramite Fiddler, immettere l'URI seguente nel **Composer** scheda: `http://localhost:port/odata/`, dove *porta* è il numero di porta.

![](creating-an-odata-endpoint/_static/image13.png)

Fare clic su di **Execute** pulsante. Fiddler invia una richiesta HTTP GET per l'applicazione. Si dovrebbe essere la risposta nell'elenco di sessioni di Web. Se tutto funziona, il codice di stato sarà 200.

![](creating-an-odata-endpoint/_static/image14.png)

Fare doppio clic nell'elenco le sessioni Web per visualizzare i dettagli del messaggio di risposta nella scheda controlli la risposta.

![](creating-an-odata-endpoint/_static/image15.png)

Il messaggio di risposta HTTP non elaborato dovrebbe essere simile al seguente:

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

Per impostazione predefinita, l'API Web restituisce il documento di servizio in formato AtomPub. Per richiedere JSON, aggiungere la seguente intestazione alla richiesta HTTP:

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

La risposta HTTP contiene ora un payload JSON:

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a>Documento di metadati di servizio

Il *documento metadati del servizio* viene descritto il modello di dati del servizio, utilizzando un linguaggio XML denominato Conceptual Schema Definition Language (CSDL). Il documento dei metadati viene mostrata la struttura dei dati nel servizio e può essere utilizzato per generare codice client.

Per ottenere il documento di metadati, inviare una richiesta GET al `http://localhost:port/odata/$metadata`. Ecco i metadati per l'endpoint illustrata in questa esercitazione.

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a>Set di entità

Per ottenere il set di entità di prodotti, inviare una richiesta GET al `http://localhost:port/odata/Products`.

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a>Entità

Per ottenere un singolo prodotto, inviare una richiesta GET al `http://localhost:port/odata/Products(1)`, dove "1" è l'ID prodotto.

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a>Formati di serializzazione di OData

OData supporta diversi formati di serializzazione:

- Pubblicazione Atom (XML)
- JSON "light" (introdotto in OData v3)
- JSON "verbose" (OData v2)

Per impostazione predefinita, Web API formato AtomPubJSON "light". 

Per ottenere il formato AtomPub, impostare l'intestazione Accept su "application/atom + xml". Di seguito è riportato un corpo di risposta di esempio:

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

È possibile visualizzare uno svantaggio ovvio il formato Atom: è molto più dettagliato rispetto a JSON light. Tuttavia, se si dispone di un client che riconosce AtomPub, il client potrebbe preferire il formato JSON.

Di seguito è la versione light di JSON della stessa entità:

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

Il formato JSON light è stata introdotta nella versione 3 del protocollo OData. Per garantire la compatibilità con le versioni precedenti, un client può richiedere il formato JSON "verbose" meno recente. Per richiedere un JSON dettagliato, impostare l'intestazione Accept `application/json;odata=verbose`. Di seguito è la versione dettagliata:

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

Questo formato fornisce più metadati nel corpo della risposta, che è possibile aggiungere un notevole sovraccarico per un'intera sessione. Inoltre, aggiunge un livello di riferimento indiretto eseguendo il wrapping dell'oggetto in una proprietà denominata "d".

## <a name="next-steps"></a>Passaggi successivi

- [Aggiungere le relazioni di entità](working-with-entity-relations.md)
- [Aggiungere le azioni OData](odata-actions.md)
- [Chiamare il servizio OData da un Client .NET](calling-an-odata-service-from-a-net-client.md)
