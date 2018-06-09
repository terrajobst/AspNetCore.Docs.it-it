---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Creare un Endpoint di OData v4 con ASP.NET Web API 2.2 | Documenti Microsoft
author: MikeWasson
description: Open Data Protocol (OData) è un protocollo di accesso ai dati per il web. OData fornisce un metodo uniforme per query e modificare i set di dati tramite operazioni CRUD...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/24/2014
ms.topic: article
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: a3f94818f9674b0e1e9a45b2a6cc9455edc79726
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2018
ms.locfileid: "26508050"
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a>Creare un Endpoint di OData v4 con ASP.NET Web API 2.2
====================
da [Mike Wasson](https://github.com/MikeWasson)

> Open Data Protocol (OData) è un protocollo di accesso ai dati per il web. OData fornisce un metodo uniforme per query e modificare i set di dati tramite operazioni CRUD (creare, leggere, aggiornare ed eliminare).
> 
> API Web ASP.NET supporta v3 e v4 del protocollo. È inoltre possibile impostare un endpoint v4 eseguito side-by-side con un endpoint v3.
> 
> In questa esercitazione viene illustrato come creare un endpoint di OData v4 che supporta operazioni CRUD.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - 2.2 API Web
> - OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Versioni dell'esercitazione
> 
> Per OData versione 3, vedere [creazione di un Endpoint di OData v3](../odata-v3/creating-an-odata-endpoint.md).


## <a name="create-the-visual-studio-project"></a>Creare il progetto di Visual Studio

In Visual Studio, dal **File** dal menu **New** &gt; **progetto**.

Espandere **installato** &gt; **modelli** &gt; **Visual c#** &gt; **Web**e selezionare il  **Applicazione Web ASP.NET** modello. Denominare il progetto &quot;ProductService&quot;.

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

Nel **nuovo progetto** finestra di dialogo Seleziona il **vuoto** modello. In &quot;aggiungere cartelle e i riferimenti di base... &quot;, fare clic su **API Web**. Fare clic su **OK**.

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a>Installare i pacchetti di OData

Dal **strumenti** dal menu **Gestione pacchetti NuGet** &gt; **Package Manager Console**. Nella finestra della Console di gestione pacchetti, digitare:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

Questo comando consente di installare i pacchetti di OData NuGet più recenti.

## <a name="add-a-model-class"></a>Aggiungere una classe modello

Oggetto *modello* è un oggetto che rappresenta un'entità di dati nell'applicazione.

In Esplora soluzioni, fare clic sulla cartella Models. Dal menu di scelta rapida, selezionare **Aggiungi** &gt; **classe**.

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> Per convenzione, le classi del modello vengono inserite nella cartella Models, ma non è necessario seguire questa convenzione nei propri progetti.


Assegnare alla classe il nome `Product`. Nel file Product.cs, sostituire il codice di boilerplate con gli elementi seguenti:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

Il `Id` proprietà è la chiave di entità. I client possono eseguire query entità tramite chiave. Per ottenere il prodotto con ID 5, ad esempio, l'URI è `/Products(5)`. Il `Id` proprietà sarà anche la chiave primaria nel database back-end.

## <a name="enable-entity-framework"></a>Abilitare Entity Framework

Per questa esercitazione, si userà Code First di Entity Framework (EF) per creare il database back-end.

> [!NOTE]
> Web API OData non richiede Entity Framework. Utilizzare qualsiasi livello di accesso ai dati che si traduce le entità di database in modelli.


In primo luogo, è possibile installare il pacchetto NuGet per Entity Framework. Dal **strumenti** dal menu **Gestione pacchetti NuGet** &gt; **Package Manager Console**. Nella finestra della Console di gestione pacchetti, digitare:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

Aprire il file Web. config e aggiungere la seguente sezione all'interno di **configurazione** elemento, dopo il **configSections** elemento.

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

Questa impostazione viene aggiunta una stringa di connessione per un database LocalDB. Quando si esegue l'app localmente, verrà utilizzato il database.

Successivamente, aggiungere una classe denominata `ProductsContext` nella cartella Models:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

Nel costruttore, `"name=ProductsContext"` fornisce il nome della stringa di connessione.

## <a name="configure-the-odata-endpoint"></a>Configurare l'OData Endpoint

Aprire il file App\_Start/WebApiConfig.cs. Aggiungere il seguente **utilizzando** istruzioni:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

Quindi aggiungere il codice seguente per il **registrare** metodo:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

Questo codice eseguite due operazioni:

- Crea un Entity Data Model (EDM).
- Aggiunge una route.

Un modello EDM è un modello di dati astratto. EDM è utilizzato per creare il documento di metadati del servizio. Il **ODataConventionModelBuilder** classe crea un modello EDM utilizzando le convenzioni di denominazione predefinito. Questo approccio richiede il minor quantità di codice. Se si desidera controllare più EDM, è possibile utilizzare il **ODataModelBuilder** classe per creare il modello EDM aggiungendo in modo esplicito le proprietà, le chiavi e le proprietà di navigazione.

Oggetto *route* spiega come indirizzare le richieste HTTP all'endpoint API Web. Per creare una route di OData v4, chiamare il **MapODataServiceRoute** metodo di estensione.

Se l'applicazione presenta più endpoint OData, creare una route separata per ogni. Assegnare ogni route di un nome univoco della route e un prefisso.

## <a name="add-the-odata-controller"></a>Aggiungere il Controller OData

Oggetto *controller* è una classe che gestisce le richieste HTTP. Creerai un controller separato per ogni set di entità nel servizio OData. In questa esercitazione si creerà un controller, per il `Product` entità.

In Esplora soluzioni, fare clic sulla cartella controller e selezionare **Aggiungi** &gt; **classe**. Assegnare alla classe il nome `ProductsController`.

> [!NOTE]
> La versione di questa esercitazione per OData v3 utilizzi il **Aggiungi Controller** lo scaffolding. Attualmente non è disponibile alcun lo scaffolding per OData v4.


Sostituire il codice boilerplate in ProductsController.cs con il codice seguente.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

Il controller viene utilizzato il `ProductsContext` classe per accedere al database tramite Entity Framework. Si noti che il controller esegue l'override di **Dispose** metodo per eliminare il **ProductsContext**.

Questo è il punto inizio per il controller. Successivamente, aggiungeremo metodi per tutte le operazioni CRUD.

## <a name="querying-the-entity-set"></a>L'esecuzione di query del Set di entità

Aggiungere i metodi seguenti per `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

La versione senza parametri di `Get` metodo restituisce l'intera raccolta di prodotti. Il `Get` metodo con un *chiave* parametro Cerca un prodotto in base alla chiave (in questo caso, il `Id` proprietà).

Il **[EnableQuery]** attributo consente ai client di modificare la query, utilizzando le opzioni di query, ad esempio $filter $sort e $page. Per ulteriori informazioni, vedere [che supporta le opzioni di Query OData](../supporting-odata-query-options.md).

## <a name="adding-an-entity-to-the-entity-set"></a>Aggiunta di un'entità per il Set di entità

Per consentire ai client di aggiungere un nuovo prodotto per il database, aggiungere il metodo seguente alla `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a>Aggiornamento di un'entità

OData supporta due diverse semantiche per l'aggiornamento di un'entità, PATCH e PUT.

- PATCH esegue un aggiornamento parziale. Il client specifica solo le proprietà da aggiornare.
- PUT sostituisce l'intera entità.

Lo svantaggio di PUT è che il client deve inviare i valori per tutte le proprietà dell'entità, inclusi i valori che non vengano modificate. Il [specifica OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) si afferma che PATCH è preferito.

In ogni caso, ecco il codice per i metodi di PATCH e PUT:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

Nel caso di applicazione della PATCH si utilizza il controller di **Delta&lt;T&gt;**  tipo per tenere traccia delle modifiche.

## <a name="deleting-an-entity"></a>Eliminazione di un'entità

Per consentire ai client di eliminare un prodotto dal database, aggiungere il metodo seguente alla `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
