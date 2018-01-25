---
uid: web-api/samples-list
title: Esempi di API Web elenco | Documenti Microsoft
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/18/2012
ms.topic: article
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: 1e1f43bbeedfc052f0b3a3924f51b544a5a79dca
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="web-api-samples-list"></a>Elenco di esempi di API Web
====================
## <a name="httpclient-samples"></a>Esempi di HttpClient

**Esempio di tradurre Bing** | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fBingTranslateSample%2fReadMe.txt)

Viene illustrato come chiamare il [servizio Microsoft Translator](https://msdn.microsoft.com/library/ff512419.aspx) utilizzando il **HttpClient** classe. L'API del servizio Microsoft Translator richiede un token di OAuth, l'applicazione ottiene inviando una richiesta al server di token di Azure per ogni richiesta per il servizio di conversione. Il risultato dal server di token viene inserito nella richiesta inviata al servizio di traduzione. Prima di eseguire questo esempio, è necessario ottenere un [chiave dell'applicazione da Azure Marketplace](https://msdn.microsoft.com/library/hh454950.aspx) e inserire le informazioni nella classe di esempio AccessTokenMessageHandler.

**Esempio di mappe Google** | [descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fGoogleMapsSample%2fReadMe.txt)

Usa **HttpClient** per scaricare una mappa di Redmond, WA da [Google Maps API](https://developers.google.com/maps/), viene salvato come file locale e consente di aprire il Visualizzatore di immagini predefinito.

**Esempio di Client di Twitter** | [descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fTwitterSample%2fReadMe.txt)

Viene illustrato come scrivere un semplice client di Twitter con **HttpClient**. Nell'esempio viene utilizzato un **HttpMessageHandler** per inserire le informazioni di autenticazione OAuth in uscita **HttpRequestMessage**. Il risultato da Twitter viene letto utilizzando JSON.NET. Prima di eseguire questo esempio, è necessario ottenere un [chiave applicazione dal Twitter](https://dev.twitter.com/)e inserire le informazioni nella classe di esempio OAuthMessageHandler.

**Esempio World Bank** | [descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [origine VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt) | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt)

Viene illustrato come recuperare i dati dal sito di dati della banca mondiale, l'utilizzo di JSON.NET per analizzare il risultato.

## <a name="web-api-samples"></a>Esempi di API Web

**Introduzione a ASP.NET Web API** | [origine di Visual Studio 2012](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

Viene illustrato come creare una web API che supporta le richieste GET HTTP di base. Contiene il codice sorgente per l'esercitazione [del primo ASP.NET Web API](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

**ASP.NET Web API JavaScript scenari, i commenti** | [origine di Visual Studio 2012](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

Viene illustrato come utilizzare l'API Web ASP.NET per creare web API che supportano i client del browser e possono essere chiamate facilmente jQuery.

**Contatto Manager** | [origine di Visual Studio 2010](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

Questo esempio utilizza l'API Web ASP.NET per compilare un'applicazione Gestione contatti semplice. L'applicazione è costituita da un contatto manager API web utilizzato da un'applicazione ASP.NET MVC e un'applicazione Windows Phone per visualizzare e gestire un elenco di contatti.

**Esempio di invio in batch** | [descrizione dettagliata](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fHostedBatchSample%2fReadMe.txt)

Viene illustrato come implementare l'invio in batch HTTP all'interno di ASP.NET. Il batch costituito da inserimento di più richieste HTTP all'interno di un corpo di entità multipart MIME singola, che viene quindi inviato al server come un POST HTTP. Le richieste vengono elaborate singolarmente e le risposte vengono inserite in un altro corpo dell'entità multipart MIME, che viene restituito al client.

**Contenuto di esempio Controller** | [descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) | [origine VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt) | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt)

Viene illustrato come leggere e scrivere le entità di richiesta e risposta in modo asincrono utilizzando i flussi. Il controller di esempio ha due azioni: un'operazione PUT che legge il corpo entità della richiesta in modo asincrono e li archivia in un file locale e la lettura che restituisce il contenuto del file locale.

**Esempio di sistema di risoluzione Assembly personalizzato** | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fCustomAssemblyResolverSample%2fReadMe.txt)

Viene illustrato come modificare l'API Web ASP.NET per supportare l'individuazione dei controller da un assembly di libreria caricato in modo dinamico. L'esempio implementa un oggetto personalizzato **IAssembliesResolver** che chiama l'implementazione predefinita e quindi aggiunge l'assembly della libreria di risultati predefinito.

**Esempio formattatore tipo di supporto personalizzato** | [descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [origine di Visual Studio 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomMediaTypeFormatterSample%2fReadMe.txt)

Viene illustrato come creare un formattatore di tipo multimediale personalizzato mediante il **BufferedMediaTypeFormatter** classe di base. Questa classe di base è destinata a formattatori che utilizzano read sincrone principalmente e le operazioni di scrittura. Oltre a mostrare il formattatore di media type, nell'esempio viene illustrato come associare registrandolo come parte di **HttpConfiguration** per l'applicazione. Si noti che è anche possibile usare il **MediaTypeFormatter** classe base direttamente, per le operazioni di scrittura e di lettura formattatori che usano principalmente asincrona.

**Esempio di associazione di parametro personalizzato** | [descrizione dettagliata](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [origine di Visual Studio 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomParameterBinding%2fReadMe.txt)

Viene illustrato come personalizzare il processo di associazione di parametro, ossia il processo che determina come le informazioni da una richiesta sono associate ai parametri di azione. In questo esempio, il controller Home prevede quattro azioni:

1. BindPrincipal viene illustrato come associare un parametro di IPrincipal da un'entità personalizzata generica, non da un messaggio HTTP GET.
2. BindCustomComplexTypeFromUriOrBody viene illustrato come associare un parametro di tipo complesso, che può provenire dal corpo del messaggio o dalla richiesta URI di un messaggio POST HTTP.
3. BindCustomComplexTypeFromUriWithRenamedProperty viene illustrato come associare un parametro di tipo complesso con una proprietà rinominata viene fornito nella richiesta URI di un messaggio POST HTTP.
4. PostMultipleParametersFromBody viene illustrato come associare più parametri dal corpo di un messaggio POST;

**File di esempio carica** | [descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fFileUploadSample%2fReadMe.txt)

Viene illustrato come caricare file in un **ApiController** tramite caricamento File Multipart MIME e come configurare le notifiche di stato con **HttpClient** utilizzando **ProgressNotificationHandler**. Il controller legge il contenuto di un caricamento di file HTML in modo asincrono e scrive una o più parti di corpo di un file locale. La risposta contiene informazioni sui file caricato (o file).

**Caricamento di esempio di archivio Blob di Azure file** | [descrizione dettagliata](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/61dfed023e50#Samples%2fNet45%2fCS%2fWebApi%2fAzureBlobsFileUploadSample%2fReadMe.txt)

In questo esempio è simile all'esempio di caricare File, ma anziché salvare i file caricati sul disco locale, in modo asincrono carica i file [archivio Blob di Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs) utilizzando [Windows Azure SDK per .NET](https://www.windowsazure.com/develop/net/). Fornisce inoltre un meccanismo per l'elenco dei BLOB presenti in un [contenitore di archiviazione Blob di Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). È possibile provare l'esempio in esecuzione per **emulatore di archiviazione Azure** fornito con Azure SDK. Se dispone di un [Account di archiviazione Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs), è possibile eseguire anche il servizio di archiviazione reale.

**Esempio della Pipeline del gestore di messaggi HTTP** | [descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [origine di Visual Studio 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fHttpMessageHandlerPipelineSample%2fReadMe.txt)

Viene illustrato come associare **HttpMessageHandler** istanze sul client (**HttpClient**) e server (ASP.NET Web API). Nell'esempio viene utilizzato lo stesso gestore nel client e server. Sebbene sia raro che lo stesso gestore esatto verrebbe eseguito in entrambe le posizioni, il modello a oggetti è lo stesso sul lato client e server.

**Esempio di caricare JSON** | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fJsonUploadSample%2fReadMe.txt)

Viene illustrato come caricare e scaricare JSON da e verso un **ApiController**. Nell'esempio viene utilizzato un numero minimo **ApiController** e si accede tramite **HttpClient**.

**Esempio di mashup** | [descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMashupSample%2fReadMe.txt)

Viene illustrato come accedere a più siti remoti in modo asincrono dall'interno un **ApiController** azione. Ogni volta che l'azione viene raggiunto, le richieste vengono eseguite in modo asincrono, in modo che nessun thread sono bloccati.

**Esempio di analisi di memoria** | [descrizione dettagliata](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [origine di Visual Studio 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fMemoryTracingSample%2fReadMe.txt)

Questo progetto di esempio crea un pacchetto Nuget che installerà un writer di traccia in memoria in applicazioni ASP.NET Web API.

**Esempio di MongoDB** | [descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMongoSample%2fReadMe.txt)

Viene illustrato come utilizzare MongoDB come archivio permanente per un **ApiController**, utilizzando un modello di repository.

**Esempio di processore corpo della risposta** | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fResponseEntityProcessorSample%2fReadMe.txt)

Viene illustrato come copiare un'entità di risposta (ovvero, un corpo della risposta HTTP) in un file locale prima di essere trasmesso al client ed eseguire ulteriori elaborazioni su tale file in modo asincrono. L'esempio implementa un **HttpMessageHandler** che include l'entità di risposta con uno che sia scrive stesso l'output come di consueto e in un file locale.

**Caricare file di esempio XDocument** | [descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [origine di Visual Studio 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fUploadXDocumentSample%2fReadMe.txt)

Viene illustrato come caricare un'istanza XDocument per un **ApiController** utilizzando **PushStreamContent** e **HttpClient**.

**Esempio di convalida** | [origine di Visual Studio 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fValidationSample%2fReadMe.txt)

Viene illustrato come è possibile utilizzare gli attributi di convalida su modelli in ASP.NET WebAPI per convalidare il contenuto della richiesta HTTP. Viene illustrato come contrassegnare come obbligatorio, come utilizzare entrambe definite dal framework e gli attributi di convalida personalizzata per annotare il modello, di proprietà e come restituire le risposte di errore per gli stati di modello non valido.

**Web Form Sample** | [descrizione dettagliata](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [origine di Visual Studio 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fWebFormSample%2fReadMe.txt)

Viene illustrato un ApiController aggiunto a un progetto di Web Form.

**[Esempio RestBugs](https://github.com/howarddierking/RestBugs)**

RestBugs è un bug semplice applicazione in cui viene illustrato come utilizzare l'API Web ASP.NET e la nuova libreria HTTP Client per creare un sistema basato su hypermedia di rilevamento. L'esempio include le implementazioni di client e server mediante ASP.NET Web API. Il server utilizza un formattatore Razor personalizzato per generare rappresentazioni di risorse. L'esempio fornisce anche un server di node.js per illustrare i vantaggi che derivano dall'utilizzo di una progettazione hypermedia per separare i client e server.

## <a name="web-api-extensions-preview-samples"></a>Esempi di anteprima di estensioni Web API

**Esempio di query OData** | [descrizione dettagliata](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [origine di Visual Studio 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataQueryableSample%2fReadMe.txt)

Viene illustrato come introdurre le query OData nell'API Web ASP.NET utilizzando il `[Queryable]` attributo o l'uso di **ODataQueryOptions** parametro azione che consente l'azione da controllare manualmente la query prima dell'esecuzione.

Il CustomerController viene illustrato l'utilizzo di attributi [Queryable] e il OrderController viene illustrato come utilizzare il parametro ODataQueryOptions. Il ResponseController è simile al CustomerController, ma anziché l'operazione GET restituzione `IEnumerable<Customer>` restituisce un **HttpResponseMessage**. Ciò consente di aggiungere i campi di intestazione aggiuntivo, modificare il codice di stato e così via, pur continuando a utilizzare la funzionalità di query. L'esempio illustra le query che utilizzano $orderby, $skip, $top, Any (), all () e $filter.

**Esempio di un servizio OData** | [descrizione dettagliata](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [origine di Visual Studio 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataServiceSample%2fReadMe.txt)

In questo esempio viene illustrato come creare un servizio OData composta da tre entità e tre i controller API Web. I controller forniscono vari livelli di funzionalità in termini che espongono la funzionalità di OData:

Il SupplierController espone un subset delle funzionalità incluse Query, verrà restituito dalla chiave e di creare, mediante la gestione di queste richieste:

- OTTENERE /Suppliers
- OTTENERE /Suppliers(key)
- GET /Suppliers? $filter =... &amp;$orderby =... &amp;$top =... &amp;$skip =...
- POST /Suppliers

Espone ProductsController GET, PUT, POST, DELETE e PATCH mediante l'implementazione di un'azione per ognuna di queste operazioni direttamente.

Il ProductFamilesController sfrutta la classe di base EntitySetController che espone un modello utile per l'implementazione di un servizio OData.

Inoltre il servizio OData espone un documento $metadata che consente i dati di utilizzato dai client di WCF Data Services e altri client che accettano il formato $metadata.
