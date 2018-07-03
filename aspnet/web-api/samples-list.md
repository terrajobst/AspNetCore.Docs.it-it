---
uid: web-api/samples-list
title: Elenco di esempi di API Web | Microsoft Docs
author: rick-anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/18/2012
ms.topic: article
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: 6c9fff024df6c485b8d904d39cb0a4e5b5bf7e44
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362564"
---
<a name="web-api-samples-list"></a>Elenco di esempi di API Web
====================
## <a name="httpclient-samples"></a>Esempi di HttpClient

**Esempio di tradurre Bing** | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fBingTranslateSample%2fReadMe.txt)

Viene illustrato come chiamare le [servizio Microsoft Translator](https://msdn.microsoft.com/library/ff512419.aspx) usando la **HttpClient** classe. L'API del servizio Microsoft Translator richiede un token OAuth, che l'applicazione ottiene inviando una richiesta al server di token di Azure per ogni richiesta al servizio Microsoft translator. Il risultato dal server di token viene inserito nella richiesta inviata al servizio di traduzione. Prima di eseguire questo esempio, è necessario ottenere un [chiave dell'applicazione da Azure Marketplace](https://msdn.microsoft.com/library/hh454950.aspx) e inserire le informazioni nella classe sample AccessTokenMessageHandler.

**Esempio di Google Maps** | [una descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fGoogleMapsSample%2fReadMe.txt)

Viene utilizzato **HttpClient** per scaricare una mappa di Redmond, WA dal [API Google Maps](https://developers.google.com/maps/), salvarlo come file locale e apre il Visualizzatore di immagini predefinite.

**Esempio di Client di Twitter** | [una descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fTwitterSample%2fReadMe.txt)

Viene illustrato come scrivere un semplice client di Twitter usando **HttpClient**. L'esempio Usa un' **HttpMessageHandler** per inserire le informazioni di autenticazione OAuth in uscita **HttpRequestMessage**. Il risultato di Twitter viene letto usando JSON.NET. Prima di eseguire questo esempio, è necessario ottenere un [chiave dell'applicazione da Twitter](https://dev.twitter.com/)e immettere le informazioni nella classe sample OAuthMessageHandler.

**Esempio della banca mondiale** | [una descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [VS 2010 origine](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt) | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt)

Viene illustrato come recuperare i dati dal sito di dati della banca mondiale, utilizzo di JSON.NET per analizzare il risultato.

## <a name="web-api-samples"></a>Esempi di API Web

**Introduzione all'API Web ASP.NET** | [origine di Visual Studio 2012](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

Viene illustrato come creare una web API che supporta le richieste GET HTTP di base. Contiene il codice sorgente per l'esercitazione [la prima API Web ASP.NET](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

**Gli scenari di ASP.NET Web API JavaScript, i commenti** | [origine di Visual Studio 2012](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

Viene illustrato come usare API Web ASP.NET per creare API web che supportano i client del browser e possono essere chiamate con facilità tramite jQuery.

**Gestione contatti** | [origine di Visual Studio 2010](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

Questo esempio Usa l'API Web ASP.NET per compilare un'applicazione semplice gestione contatti. L'applicazione è costituita da una gestione contatti API web che viene usato da un'applicazione ASP.NET MVC e un'applicazione Windows Phone per visualizzare e gestire un elenco di contatti.

**L'invio in batch campione** | [una descrizione dettagliata](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fHostedBatchSample%2fReadMe.txt)

Viene illustrato come implementare l'invio in batch HTTP all'interno di ASP.NET. La funzionalità batch costituito da inserimento di più richieste HTTP all'interno di un corpo di entità multipart MIME singola, che viene quindi inviato al server come una richiesta HTTP POST. Le richieste vengono elaborate singolarmente e le risposte vengono inserite in un altro corpo di entità multipart MIME, che viene restituito al client.

**Controller di esempio del contenuto** | [una descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) | [VS 2010 origine](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt) | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt)

Viene illustrato come leggere e scrivere entità di richiesta e risposta in modo asincrono utilizzando i flussi. Il controller di esempio ha due azioni: un'azione PUT che legge in modo asincrono il corpo di entità di richiesta e li archivia in un file locale e un'azione GET che restituisce il contenuto del file locale.

**Esempio di Resolver di Assembly personalizzati** | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fCustomAssemblyResolverSample%2fReadMe.txt)

Viene illustrato come modificare l'API Web ASP.NET per supportare l'individuazione dei controller da un assembly caricato in modo dinamico della libreria. L'esempio implementa una classe personalizzata **IAssembliesResolver** che chiama l'implementazione predefinita e quindi aggiunge l'assembly della libreria per i risultati predefiniti.

**Esempio di formattatore personalizzato supporti tipo** | [una descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [origine di Visual Studio 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomMediaTypeFormatterSample%2fReadMe.txt)

Viene illustrato come creare un formattatore di tipo multimediale personalizzato usando il **BufferedMediaTypeFormatter** classe di base. Questa classe di base è destinata a formattatori che principalmente utilizzano read sincrone e operazioni di scrittura. Oltre che mostra il formattatore di media type, il codice di esempio mostra come agganciare registrandolo come parte del **HttpConfiguration** per l'applicazione. Si noti che è anche possibile usare la **MediaTypeFormatter** classe di base direttamente, per i formattatori che usano principalmente asincrona di lettura e operazioni di scrittura.

**Esempio di associazione personalizzato di Parameter** | [una descrizione dettagliata](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [origine di Visual Studio 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomParameterBinding%2fReadMe.txt)

Viene illustrato come personalizzare il processo di associazione di parametro, ossia il processo che determina il modo in cui le informazioni da una richiesta sono associate ai parametri di azione. In questo esempio, il controller Home prevede quattro azioni:

1. BindPrincipal illustra come associare un parametro di IPrincipal da un'entità generica personalizzata, non da un messaggio HTTP GET.
2. BindCustomComplexTypeFromUriOrBody illustra come associare un parametro di tipo complesso, che può provenire dal corpo del messaggio o dall'URI di richiesta di un messaggio di richiesta HTTP POST.
3. BindCustomComplexTypeFromUriWithRenamedProperty illustra come associare un parametro di tipo complesso con una proprietà rinominata che deriva dall'URI di richiesta di un messaggio di richiesta HTTP POST.
4. PostMultipleParametersFromBody illustra come associare più parametri dal corpo di un messaggio POST.

**Esempio di caricamento del file** | [una descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fFileUploadSample%2fReadMe.txt)

Viene illustrato come caricare file in un' **ApiController** tramite caricamento File Multipart MIME e su come impostare le notifiche di stato di avanzamento con **HttpClient** usando **ProgressNotificationHandler**. Il controller legge il contenuto di un'operazione di caricamento file HTML in modo asincrono e scrive una o più parti di corpo di un file locale. La risposta contiene informazioni sul file caricato (o file).

**File Upload to Azure Blob Store Sample** | [una descrizione dettagliata](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/61dfed023e50#Samples%2fNet45%2fCS%2fWebApi%2fAzureBlobsFileUploadSample%2fReadMe.txt)

In questo esempio è simile all'esempio di caricamento File, ma anziché salvare i file caricati sul disco locale, in modo asincrono carica i file [Azure Blob Store](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs) utilizzando [Windows Azure SDK per .NET](https://www.windowsazure.com/develop/net/). Fornisce inoltre un meccanismo per elencare i BLOB presenti in un' [contenitore di archiviazione Blob di Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). È possibile provare il codice di esempio eseguito **emulatore di archiviazione Azure** fornito con il SDK di Azure. Se si dispone di un [Account di archiviazione Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs), è possibile eseguire anche il servizio di archiviazione effettivo.

**Esempio di Pipeline del gestore messaggi HTTP** | [una descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [origine di Visual Studio 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fHttpMessageHandlerPipelineSample%2fReadMe.txt)

Viene illustrato come associare **HttpMessageHandler** istanze sul client (**HttpClient**) e server (API Web ASP.NET). Nell'esempio viene utilizzato lo stesso gestore sia il client di server. Sebbene sia raro che lo stesso gestore esatto verrebbe eseguito in entrambe le posizioni, il modello a oggetti è lo stesso sul lato client e server.

**Esempio JSON di caricare** | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fJsonUploadSample%2fReadMe.txt)

Viene illustrato come caricare e scaricare JSON da e verso un' **ApiController**. L'esempio Usa un minimo **ApiController** e si accede tramite **HttpClient**.

**Esempio di mashup** | [una descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMashupSample%2fReadMe.txt)

Viene illustrato come accedere ai siti remoti più in modo asincrono da un **ApiController** azione. Ogni volta che viene raggiunto l'azione, le richieste vengono eseguite in modo asincrono, in modo che nessun thread sono bloccati.

**Esempio di analisi di memoria** | [una descrizione dettagliata](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [origine di Visual Studio 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fMemoryTracingSample%2fReadMe.txt)

Questo progetto di esempio crea un pacchetto Nuget che installerà un writer di traccia personalizzato in memoria in applicazioni ASP.NET Web API.

**Esempio di MongoDB** | [una descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMongoSample%2fReadMe.txt)

Viene illustrato come usare MongoDB come archivio permanente per un **ApiController**, usando un modello di repository.

**Esempio di processore del corpo di risposta** | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fResponseEntityProcessorSample%2fReadMe.txt)

Viene illustrato come copiare un'entità di risposta (vale a dire, un corpo di risposta HTTP) in un file locale prima di essere trasmesso al client ed eseguire un'elaborazione aggiuntiva su tale file in modo asincrono. L'esempio implementa un **HttpMessageHandler** che esegue il wrapping l'entità di risposta con uno che sia scrive stesso per l'output come di consueto e in un file locale.

**Carica esempio XDocument** | [una descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fUploadXDocumentSample%2fReadMe.txt)

Illustra come caricare un'istanza XDocument a un **ApiController** utilizzando **PushStreamContent** e **HttpClient**.

**Esempio di convalida** | [origine di Visual Studio 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fValidationSample%2fReadMe.txt)

Viene illustrato come è possibile usare gli attributi di convalida nei modelli nell'API Web ASP.NET per convalidare il contenuto della richiesta HTTP. Viene illustrato come contrassegnare le proprietà come obbligatorio, come usare entrambi definiti dal framework e gli attributi di convalida personalizzati per annotare il modello e come restituire le risposte di errore per gli stati del modello non valido.

**Web Form Sample** | [una descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [origine di Visual Studio 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fWebFormSample%2fReadMe.txt)

Viene illustrato un ApiController aggiunto a un progetto di Web Form.

**[Esempio RestBugs](https://github.com/howarddierking/RestBugs)**

RestBugs è un bug semplice applicazione che illustra come usare la nuova libreria di HTTP Client e API Web ASP.NET per creare un sistema basato su ipermedia. L'esempio include le implementazioni di client e server, usando l'API Web ASP.NET. Il server utilizza un formattatore personalizzato di Razor per generare rappresentazioni della risorsa. L'esempio fornisce anche un server Node. js per illustrare i vantaggi che derivano dall'uso di una progettazione ipermedia separare i client e server.

## <a name="web-api-extensions-preview-samples"></a>Esempi le estensioni API Web

**Esempio sottoponibili a query OData** | [una descrizione dettagliata](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [origine di Visual Studio 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataQueryableSample%2fReadMe.txt)

Viene illustrato come introdurre le query OData nell'API Web ASP.NET usando il `[Queryable]` dell'attributo o l'uso di **ODataQueryOptions** parametro di azione che consente l'azione da eseguire una verifica manuale della query prima che viene eseguito.

Il CustomerController viene illustrato l'utilizzo di attributi [Queryable] e il OrderController illustra come usare il parametro ODataQueryOptions. Il ResponseController è simile al CustomerController, ma anziché l'azione GET restituzione `IEnumerable<Customer>` restituisce un **HttpResponseMessage**. Ciò consente di aggiungere i campi di intestazione aggiuntivi, modificare il codice di stato e così via, continuando comunque a utilizzare la funzionalità di query. L'esempio illustra le query che utilizzano $orderby $skip, $top, Any (), all () e $filter.

**Esempio di servizio OData** | [una descrizione dettagliata](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [origine di Visual Studio 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataServiceSample%2fReadMe.txt)

Questo esempio viene illustrato come creare un servizio OData costituito da tre entità e tre i controller API Web. I controller di forniscano diversi livelli di funzionalità in termini che espongono la funzionalità di OData:

Il SupplierController espone un subset di funzionalità che includono Query, ottenere dalla chiave e creazione, tramite la gestione di queste richieste:

- OTTENERE /Suppliers
- OTTENERE /Suppliers(key)
- GET /Suppliers? $filter =... &amp;$orderby =... &amp;$top =... &amp;$skip =...
- /Suppliers POST

L'oggetto espone ProductsController GET, PUT, POST, DELETE e PATCH implementando un'azione per ognuna di queste operazioni direttamente.

Il ProductFamilesController sfrutta la classe di base EntitySetController che espone un modello utile per l'implementazione di un servizio OData.

Inoltre il servizio OData espone un documento $metadata che consente ai dati di utilizzato dai client di WCF Data Services e altri client che accettano il formato $metadata.
