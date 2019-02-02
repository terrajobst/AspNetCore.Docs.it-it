---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Creare un Endpoint OData v4 tramite ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: Open Data Protocol (OData) è un protocollo di accesso di dati per il web. OData offre un metodo uniforme per eseguire query e modificare set di dati tramite operazioni CRUD...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: c6a4aa4eb563fd77d5afd9248175d5f5b7984d19
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667570"
---
# <a name="create-an-odata-v4-endpoint-using-aspnet-web-api"></a>Creare un Endpoint OData v4 tramite l'API Web ASP.NET 

> Open Data Protocol (OData) è un protocollo di accesso di dati per il web. OData offre un metodo uniforme per eseguire query e modificare set di dati tramite operazioni CRUD (creare, leggere, aggiornare ed eliminare).
>
> API Web ASP.NET supporta v3 e v4 del protocollo. È anche possibile avere un endpoint v4 che viene eseguito side-by-side con un endpoint di v3.
>
> Questa esercitazione illustra come creare un endpoint OData v4 che supporta operazioni CRUD.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
>
> - Web API 5.2
> - OData v4
> - Visual Studio 2017 (download di Visual Studio 2017 [qui](https://visualstudio.microsoft.com/downloads/))
> - Entity Framework 6
> - .NET 4.7.2
>
> ## <a name="tutorial-versions"></a>Versioni dell'esercitazione
>
> Per OData versione 3, vedere [creazione di un Endpoint OData v3](../odata-v3/creating-an-odata-endpoint.md).

## <a name="create-the-visual-studio-project"></a>Creare il progetto di Visual Studio

In Visual Studio, dai **File** dal menu **New** &gt; **progetto**.

Espandere **Installed** &gt; **Visual C#**  &gt; **Web**e selezionare il **applicazione Web ASP.NET (.NET Framework)**  modello. Denominare il progetto &quot;ProductService&quot;.

[![](create-an-odata-v4-endpoint/_static/image7.png)](create-an-odata-v4-endpoint/_static/image7.png)

Scegliere **OK**.



[![](create-an-odata-v4-endpoint/_static/image8.png)](create-an-odata-v4-endpoint/_static/image8.png)

Selezionare il **vuoto** modello. Sotto **aggiungere le cartelle e di base di riferimenti per:**, selezionare **API Web**. Scegliere **OK**.

## <a name="install-the-odata-packages"></a>Installare i pacchetti di OData

Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** &gt;  **Console di Gestione pacchetti**. Nella finestra della Console di gestione pacchetti, digitare:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

Questo comando consente di installare gli ultimi pacchetti NuGet di OData.

## <a name="add-a-model-class"></a>Aggiungere una classe del modello

Oggetto *modello* è un oggetto che rappresenta un'entità di dati nell'applicazione.

In Esplora soluzioni fare clic sulla cartella Models. Dal menu di scelta rapida, selezionare **Add** &gt; **classe**.

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> Per convenzione, le classi del modello vengono inserite nella cartella Models, ma non è necessario seguono questa convenzione nei propri progetti.


Assegnare alla classe il nome `Product`. Nel file Product.cs, sostituire il codice boilerplate con gli elementi seguenti:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

Il `Id` proprietà è la chiave di entità. I client possono eseguire query sulle entità tramite chiave. Per ottenere il prodotto con ID 5, ad esempio, l'URI è `/Products(5)`. Il `Id` proprietà sarà anche la chiave primaria nel database di back-end.

## <a name="enable-entity-framework"></a>Abilitare Entity Framework

Per questa esercitazione, useremo Code First di Entity Framework (EF) per creare il database back-end.

> [!NOTE]
> API Web OData non richiede Entity Framework. Usare qualsiasi livello di accesso ai dati che può tradurre le entità di database in modelli.


Prima di tutto installare il pacchetto NuGet per Entity Framework. Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** &gt;  **Console di Gestione pacchetti**. Nella finestra della Console di gestione pacchetti, digitare:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

Aprire il file Web. config e aggiungere la sezione seguente all'interno di **configuration** elemento, dopo il **configSections** elemento.

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

Questa impostazione consente di aggiungere una stringa di connessione per un database LocalDB. Questo database verrà utilizzato quando si esegue l'app in locale.

Successivamente, aggiungere una classe denominata `ProductsContext` nella cartella Models:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

Nel costruttore, `"name=ProductsContext"` fornisce il nome della stringa di connessione.

## <a name="configure-the-odata-endpoint"></a>Configurare l'endpoint OData

Aprire il file App\_Start/WebApiConfig.cs. Aggiungere il codice seguente **usando** istruzioni:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

Quindi aggiungere il codice seguente per il **registrare** metodo:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

Questo codice esegue due operazioni:

- Crea un Entity Data Model (EDM).
- Aggiunge una route.

Un modello EDM è un modello di dati astratto. EDM viene utilizzato per creare il documento di metadati di servizio. Il **ODataConventionModelBuilder** classe crea un modello EDM utilizzando le convenzioni di denominazione predefinito. Questo approccio richiede il minor quantità di codice. Se si desidera maggiore controllo sul modello EDM, è possibile usare la **ODataModelBuilder** classe per creare il modello EDM mediante l'aggiunta di proprietà, le chiavi e le proprietà di navigazione in modo esplicito.

Oggetto *route* indica come indirizzare le richieste HTTP all'endpoint API Web. Per creare una route di OData v4, chiamare il **MapODataServiceRoute** metodo di estensione.

Se l'applicazione dispone di più endpoint OData, creare una route separata per ognuna. Assegnare ogni route un nome di route univoco e un prefisso.

## <a name="add-the-odata-controller"></a>Aggiungere il controller OData

Oggetto *controller* è una classe che gestisce le richieste HTTP. Creare un controller separato per ogni set di entità nel servizio OData. In questa esercitazione si creerà un controller, per il `Product` entità.

In Esplora soluzioni fare doppio clic su cartella controller e selezionare **Add** &gt; **classe**. Assegnare alla classe il nome `ProductsController`.

> [!NOTE]
> La versione di questa esercitazione per OData v3 usi il **Aggiungi Controller** scaffolding. Attualmente, non è nessuna scaffolding per OData v4.


Sostituire il codice boilerplate in ProductsController.cs con il codice seguente.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

Il controller viene utilizzato il `ProductsContext` classe per accedere al database tramite Entity Framework. Si noti che il controller esegue l'override di **Dispose** metodo per eliminare le **ProductsContext**.

Questo è il punto di partenza per il controller. Successivamente, si aggiungerà metodi per tutte le operazioni CRUD.

## <a name="query-the-entity-set"></a>Eseguire una query del set di entità

Aggiungere i metodi seguenti alla `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

La versione senza parametri del `Get` metodo restituisce l'intera raccolta di prodotti. Il `Get` metodo con un *chiave* parametro Cerca un prodotto in base alla chiave (in questo caso, il `Id` proprietà).

Il **[EnableQuery]** attributo consente ai client di modificare la query, usando le opzioni di query, ad esempio $filter $sort e $page. Per altre informazioni, vedere [che supportano le opzioni di Query OData](../supporting-odata-query-options.md).

## <a name="add-an-entity-to-the-entity-set"></a>Aggiungere un'entità per il set di entità

Per consentire ai client di aggiungere un nuovo prodotto per il database, aggiungere il metodo seguente alla `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="update-an-entity"></a>Aggiornare un'entità

OData supporta due diverse semantiche per aggiornare un'entità, PATCH e PUT.

- PATCH esegue un aggiornamento parziale. Il client specifica solo le proprietà da aggiornare.
- PUT sostituisce l'intera entità.

Lo svantaggio di PUT è che il client deve inviare i valori per tutte le proprietà dell'entità, inclusi i valori che non vengano modificate. Il [specifica OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) dichiara che PATCH è preferita.

In ogni caso, ecco il codice per i metodi sia PATCH e PUT:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

Nel caso di PATCH, il controller Usa il **Delta&lt;T&gt;**  tipo per tenere traccia delle modifiche.

## <a name="delete-an-entity"></a>Eliminare un'entità

Per abilitare i client eliminare un prodotto dal database, aggiungere il metodo seguente alla `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
