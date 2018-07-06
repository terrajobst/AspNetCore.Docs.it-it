---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
title: Informazioni sui servizi Web ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Servizi Web sono parte integrante di .NET framework che offrono una soluzione cross-platform per lo scambio di dati tra sistemi distribuiti. Anche se Web...
ms.author: aspnetcontent
ms.date: 03/28/2008
ms.assetid: 3332d6e7-e2e1-4144-b805-e71d51e7e415
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
msc.type: authoredcontent
ms.openlocfilehash: b98ef4c27ab7b4b729e9e5b68e7d2642a6418ab6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838601"
---
<a name="understanding-aspnet-ajax-web-services"></a>Informazioni sui servizi Web ASP.NET AJAX
====================
da [Scott Cate](https://github.com/scottcate)

[Scaricare PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial05_Web_Services_with_MS_Ajax_cs.pdf)

> Servizi Web sono parte integrante di .NET framework che offrono una soluzione cross-platform per lo scambio di dati tra sistemi distribuiti. Anche se i servizi Web vengono in genere usati per consentire a diversi sistemi operativi, modelli a oggetti e linguaggi di programmazione per inviare e ricevere dati, possono anche essere utilizzati per inserire dati in una pagina ASP.NET AJAX o inviare dati da una pagina a un sistema back-end in modo dinamico. Tutto questo può avvenire senza alcun intervento per operazioni di postback.


## <a name="calling-web-services-with-aspnet-ajax"></a>Chiamata dei servizi Web con ASP.NET AJAX

Dan Wahlin

Servizi Web sono parte integrante di .NET framework che offrono una soluzione cross-platform per lo scambio di dati tra sistemi distribuiti. Anche se i servizi Web vengono in genere usati per consentire a diversi sistemi operativi, modelli a oggetti e linguaggi di programmazione per inviare e ricevere dati, possono anche essere utilizzati per inserire dati in una pagina ASP.NET AJAX o inviare dati da una pagina a un sistema back-end in modo dinamico. Tutto questo può avvenire senza alcun intervento per operazioni di postback.

Mentre il controllo UpdatePanel ASP.NET AJAX fornisce un modo semplice a AJAX abilitare tutte le pagine ASP.NET, potrebbero esserci casi è necessario accedere in modo dinamico i dati nel server senza usare un controllo UpdatePanel. In questo articolo verrà illustrato come effettuare questa operazione tramite la creazione e utilizzo dei servizi Web all'interno delle pagine ASP.NET AJAX.

Questo articolo è incentrato sulle funzionalità disponibili nelle estensioni AJAX di ASP.NET core, nonché un controllo del servizio Web abilitata in ASP.NET AJAX Toolkit denominato il AutoCompleteExtender. Gli argomenti trattati includono la definizione dei servizi Web compatibili con AJAX, la creazione di proxy client e chiamare servizi Web con JavaScript. Scoprirai anche come servizio Web è possibile chiamare direttamente ai metodi di pagina ASP.NET.

## <a name="web-services-configuration"></a>Configurazione dei servizi Web

Quando viene creato un nuovo progetto di sito Web con Visual Studio 2008, il file Web. config contiene un numero di nuove funzionalità che possono essere poco note agli utenti delle versioni precedenti di Visual Studio. Alcune di queste modifiche vengono mappate il prefisso "asp" ai controlli ASP.NET AJAX possono essere utilizzati nelle pagine mentre altri vengono definite obbligatori HttpHandlers e moduli HTTP. Listato 1 mostra le modifiche apportate al `<httpHandlers>` in Web. config che interessa le chiamate del servizio Web. Il valore predefinito che HttpHandler usata per elaborare le chiamate con estensione asmx viene rimosso e sostituito con una classe ScriptHandlerFactory che si trova nell'assembly System.Web.Extensions.dll. System.Web.Extensions.dll contiene tutte le funzionalità di core usate da ASP.NET AJAX.

**Listato 1. Configurazione del gestore del servizio Web ASP.NET AJAX**

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample1.xml)]

Questa sostituzione HttpHandler viene eseguita per permettere le chiamate JavaScript Object Notation (JSON) essere eseguite da pagine ASP.NET AJAX ai servizi Web .NET usando un proxy del servizio Web di JavaScript. ASP.NET AJAX invia i messaggi JSON ai servizi Web anziché le chiamate SOAP Simple Object Access Protocol () standard in genere associate ai servizi Web. Di conseguenza più piccolo messaggi di richiesta e risposta complessivi. Consente inoltre di elaborazione sul lato client, più efficiente dei dati poiché la libreria JavaScript di ASP.NET AJAX è ottimizzata per funzionare con oggetti JSON. Listato 2 e listato 3 mostrano esempi di messaggi richiesta e risposta serializzati in formato JSON del servizio Web. Il messaggio di richiesta visualizzato nel listato 2 passa un parametro di paese con il valore "Belgio" mentre il messaggio di risposta nel listato 3 passa una matrice di oggetti Customer e le relative proprietà.

**Listato 2. Messaggio di richiesta di servizio Web serializzato in JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample2.json)]

> *> [!NOTE] il nome dell'operazione è definito come parte dell'URL per il servizio web. Inoltre, i messaggi di richiesta non vengono sempre inviati tramite JSON. Servizi Web possono utilizzare l'attributo ScriptMethod con il parametro UseHttpGet impostato su true, che fa sì che i parametri da passare tramite un i parametri della stringa di query.*


**Listato 3. Messaggio di risposta del servizio Web serializzato in JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample3.json)]

Nella sezione successiva verrà illustrato come creare servizi Web in grado di gestire i messaggi di richiesta JSON e risponde con i tipi semplici e complessi.

## <a name="creating-ajax-enabled-web-services"></a>Creazione di servizi Web compatibili con AJAX

Il framework ASP.NET AJAX fornisce diversi metodi per chiamare i servizi Web. È possibile usare il controllo AutoCompleteExtender (disponibile in ASP.NET AJAX Toolkit) o JavaScript. Tuttavia, prima di chiamare un servizio è necessario abilitare AJAX, in modo che può essere chiamata dal codice di script client.

Se si ha familiarità con servizi Web ASP.NET, troverai molto semplice creare e i servizi abilitati AJAX. .NET framework è supportata la creazione di servizi Web ASP.NET fin dal suo rilascio iniziale nel 2002 e ASP.NET AJAX Extensions forniscono funzionalità aggiuntive di AJAX che si basa il set predefinito di .NET framework di funzionalità. Visual Studio .NET 2008 Beta 2 include il supporto incorporato per la creazione di file del servizio Web ASMX e viene generato automaticamente associato code-beside classi dalla classe WebService. Quando si aggiungono metodi nella classe è necessario applicare l'attributo WebMethod affinché possano essere chiamato dagli utenti del servizio Web.

Listato 4 illustra un esempio di applicare l'attributo WebMethod a un metodo denominato GetCustomersByCountry().

**Listato 4. Usando l'attributo WebMethod in un servizio Web**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample4.cs)]

Il metodo GetCustomersByCountry() accetta un parametro di paese e restituisce un cliente come matrice di oggetti. Il valore del paese passato al metodo viene inoltrato a una classe di livello aziendale che a sua volta chiama una classe di livello dati per recuperare i dati dal database, riempire le proprietà dell'oggetto cliente con i dati e restituire la matrice.

## <a name="using-the-scriptservice-attribute"></a>Uso dell'attributo ScriptService

Durante l'aggiunta di WebMethod attributo consente la GetCustomersByCountry() del metodo da chiamare dal client che inviano messaggi SOAP standard al servizio Web, questa funzionalità non consente le chiamate JSON essere eseguita dal immediatamente applicazioni ASP.NET AJAX. Per permettere le chiamate JSON essere apportate è necessario applicare l'estensione di ASP.NET AJAX `ScriptService` attributo alla classe del servizio Web. Questo consente a un servizio Web inviare messaggi di risposta formattati mediante JSON e script lato client chiamare un servizio mediante l'invio di messaggi JSON.

Listato 5 viene illustrato un esempio di applicazione dell'attributo ScriptService a una classe di servizio Web denominata CustomersService.

**Listato 5. Uso dell'attributo ScriptService per abilitare AJAX per un servizio Web**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample5.cs)]

L'attributo ScriptService agisce come un indicatore che segnala che può essere chiamato dal codice di script AJAX. In realtà non gestisce le attività della serializzazione o deserializzazione di JSON che vengono eseguiti dietro le quinte. Il ScriptHandlerFactory (configurato in Web. config) e altre classi correlate svolgono la maggior parte dell'elaborazione di JSON.

## <a name="using-the-scriptmethod-attribute"></a>Usando l'attributo ScriptMethod

L'attributo ScriptService è l'unico attributo di ASP.NET AJAX che deve essere definito in un servizio Web .NET affinché utilizzabile da pagine ASP.NET AJAX. Tuttavia, un altro attributo denominato ScriptMethod può anche essere applicato direttamente ai metodi Web in un servizio. ScriptMethod definisce tre proprietà compresi `UseHttpGet`, `ResponseFormat` e `XmlSerializeString`. Modifica dei valori di queste proprietà può essere utile nei casi in cui il tipo di richiesta accettata da un metodo Web deve essere modificato a GET, quando un metodo Web deve restituire dati XML non elaborati sotto forma di un' `XmlDocument` o `XmlElement` oggetto o quando i dati restituiti da un  servizio deve sempre essere serializzato come XML invece di JSON.

Il UseHttpGet proprietà può essere utilizzata quando un metodo Web deve accettare richieste opposizione alle richieste POST GET. Le richieste vengono inviate tramite un URL con parametri di input di metodo Web convertita in parametri di stringa di query. UseHttpGet proprietà su false per impostazione predefinita e deve solo essere impostato su `true` quando operazioni notoriamente risultano sicure, mentre quando i dati sensibili non viene passati a un servizio Web. Listato 6 viene illustrato un esempio dell'uso dell'attributo ScriptMethod con la proprietà UseHttpGet.

**Listato 6. Usando l'attributo ScriptMethod con la proprietà UseHttpGet.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample6.cs)]

Un esempio delle intestazioni inviate quando viene chiamato il metodo Web HttpGetEcho del listato 6 sono indicato di seguito:

`GET /CustomerViewer/DemoService.asmx/HttpGetEcho?input=%22Input Value%22 HTTP/1.1`

Oltre a consentire i metodi Web accettare le richieste HTTP GET, l'attributo ScriptMethod è anche utilizzabile quando le risposte XML devono essere restituiti da un servizio piuttosto che da JSON. Ad esempio, un servizio Web può recuperare un feed RSS da un sito remoto e restituirlo come un oggetto XmlDocument o XmlElement. L'elaborazione del XML dei dati possono quindi verificarsi nel client.

Listato 7 viene illustrato un esempio di utilizzo della proprietà ResponseFormat per specificare che i dati XML devono essere restituiti da un metodo Web.

**Listato 7. Usando l'attributo ScriptMethod con la proprietà ResponseFormat.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample7.cs)]

La proprietà ResponseFormat può anche essere utilizzata insieme alla proprietà XmlSerializeString. La proprietà XmlSerializeString ha un valore predefinito false, il che significa che restituiscano i tipi tranne le stringhe restituite da un metodo Web sono serializzati come XML durante la `ResponseFormat` è impostata su `ResponseFormat.Xml`. Quando `XmlSerializeString` è impostata su `true`, tutti i tipi restituiti da un metodo Web sono serializzati come XML, inclusi i tipi di stringa. Se la proprietà ResponseFormat ha un valore di `ResponseFormat.Json` la proprietà XmlSerializeString viene ignorata.

Listato 8 illustra un esempio di utilizzo della proprietà XmlSerializeString per forzare le stringhe da serializzare come XML.

**Listato 8. Usando l'attributo ScriptMethod con la proprietà XmlSerializeString**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample8.cs)]

Il valore restituito dalla chiamata al metodo Web GetXmlString mostrato nel listato 8 è indicato di seguito:

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample9.cs)]

Anche se il formato JSON predefinito consente di ridurre la dimensione complessiva di messaggi di richiesta e risposta e viene utilizzato più facilmente dai client di ASP.NET AJAX in modo più browser, le proprietà ResponseFormat e XmlSerializeString possono essere usata quando client le applicazioni, ad esempio Internet Explorer 5 o versione successiva prevedono che i dati XML venga restituito da un metodo Web.

## <a name="working-with-complex-types"></a>Utilizzo di tipi complessi

Listato 5 è stato illustrato un esempio di restituzione di un tipo complesso denominato cliente da un servizio Web. La classe Customer definisce diversi tipi semplici internamente come proprietà, ad esempio FirstName e LastName. I tipi complessi utilizzato come parametro di input o tipo restituito su un metodo Web compatibile con AJAX automaticamente vengono serializzati in JSON prima dell'invio al lato client. Tuttavia, i tipi complessi annidati (quelli definiti internamente all'interno di un altro tipo) non vengono resi disponibili al client come oggetti autonomi per impostazione predefinita.

Nei casi in cui un tipo complesso annidato usato da un servizio Web deve essere usato anche in una pagina client, è possibile aggiungere l'attributo di ASP.NET AJAX GenerateScriptType al servizio Web. Ad esempio, la classe CustomerDetails listato 9 contiene le proprietà di indirizzo e la relativa al sesso cui ***rappresentano tipi complessi annidati.***

**Listato 9. La classe CustomerDetails illustrata di seguito contiene due tipi complessi annidati.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample10.cs)]

Gli oggetti indirizzo e la relativa al sesso definiti all'interno della classe CustomerDetails listato 9 non verranno automaticamente reso disponibili per l'utilizzo sul lato client tramite JavaScript perché sono i tipi annidati (si tratta di una classe e sesso è un'enumerazione). In situazioni in cui un tipo annidato usato all'interno di un servizio Web deve essere disponibile sul lato client, l'attributo GenerateScriptType indicato in precedenza può essere utilizzato (vedere l'elenco di 10). Questo attributo può essere aggiunto più volte nei casi in cui i diversi tipi complessi annidati vengono restituiti da un servizio. Può essere applicato direttamente alla classe del servizio Web o versioni successive i metodi Web specifici.

**Listato 10. Utilizzo dell'attributo GenerateScriptService per definire i tipi annidati che devono essere disponibili al client.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample11.cs)]

Applicando la `GenerateScriptType` attributi ai tipi di servizio Web, l'indirizzo e sesso diventano automaticamente disponibili per l'utilizzo da codice JavaScript di ASP.NET AJAX lato client. Nel listato 11 viene illustrato un esempio di JavaScript che viene automaticamente generato e inviato al client aggiungendo l'attributo GenerateScriptType su un servizio Web. Scoprirai come usare tipi complessi annidati in un secondo momento nell'articolo.

**Listato 11. Tipi complessi annidati resi disponibili per una pagina ASP.NET AJAX.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample12.cs)]

Ora che si è appreso come creare servizi Web e renderli accessibili alle pagine ASP.NET AJAX, diamo un'occhiata a come creare e usare i proxy di JavaScript in modo che i dati possono essere recuperati o inviati ai servizi Web.

## <a name="creating-javascript-proxies"></a>Creazione di proxy di JavaScript

La chiamata a un servizio Web standard (.NET o un'altra piattaforma) in genere comporta la creazione di un oggetto proxy che si protegge dalle complessità di invio di messaggi di richiesta e risposta SOAP. Con le chiamate di servizio Web di ASP.NET AJAX, i proxy di JavaScript possono essere creati e usati per chiamare facilmente i servizi senza doversi preoccupare serializzare e deserializzare i messaggi JSON. I proxy di JavaScript possono essere generati automaticamente usando il controllo ScriptManager di AJAX ASP.NET.

Creazione di un proxy di JavaScript che può chiamare servizi Web viene eseguita utilizzando proprietà di servizi di ScriptManager. Questa proprietà consente di definire uno o più servizi che può chiamare in modo asincrono una pagina ASP.NET AJAX per inviare o ricevere dati senza richiedere operazioni di postback. Si definisce un servizio usando ASP.NET AJAX `ServiceReference` controllo e assegnando l'URL del servizio Web per il controllo `Path` proprietà. Listato 12 illustra un esempio di riferimento a un servizio denominato CustomersService.asmx.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample13.aspx)]

**Listato 12. Definizione di un servizio Web utilizzato in una pagina ASP.NET AJAX.**

Aggiunge un riferimento al CustomersService.asmx tramite il controllo ScriptManager, un proxy di JavaScript essere generati dinamicamente e fa riferimento la pagina. Il proxy è incorporato tramite il &lt;script&gt; tag e caricati dinamicamente chiamando il file CustomersService.asmx e aggiunta /js alla fine di esso. Nell'esempio seguente viene illustrato come il proxy JavaScript è incorporato nella pagina durante il debug è disabilitato nel file Web. config:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample14.html)]

> *> [!NOTE] Se si desidera visualizzare il codice proxy JavaScript effettivo che viene generato è possibile digitare l'URL per il servizio Web .NET desiderato nella casella Indirizzo Internet Explorer e aggiungere /js alla fine di esso.*


Se è abilitato il debug nel file Web. config che una versione di debug del proxy JavaScript verrà incorporata nella pagina come indicato di seguito:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample15.html)]

Il proxy JavaScript creato da ScriptManager è inoltre possibile incorporare direttamente nella pagina anziché di farvi riferimento usando il &lt;script&gt; attributo src del tag. Questa operazione può essere eseguita mediante l'impostazione InlineScript proprietà del controllo ServiceReference su true (impostazione predefinita è false). Ciò può essere utile quando un proxy non viene condivisa tra più pagine e quando si vuole ridurre il numero di chiamate di rete effettuate al server. Quando InlineScript è impostata su true lo script del proxy non memorizzare nella cache dal browser in modo che il valore predefinito false è consigliato nei casi in cui il proxy viene utilizzato da più pagine in un'applicazione AJAX ASP.NET. Un esempio di utilizzo della proprietà di InlineScript viene indicato di seguito:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample16.aspx)]

## <a name="using-javascript-proxies"></a>Usando i proxy di JavaScript

Una volta che un servizio Web viene fatto riferimento da una pagina ASP.NET AJAX tramite il controllo ScriptManager, può essere eseguita una chiamata al servizio Web e i dati restituiti possono essere gestiti usando funzioni di callback. Viene chiamato un servizio Web dal relativo spazio dei nomi di riferimento (se presente), nome della classe e nome del metodo Web. È possibile definire tutti i parametri passati al servizio Web insieme a una funzione di callback che gestisce i dati restituiti.

Viene illustrato un esempio dell'uso di un proxy di JavaScript per chiamare un metodo Web denominato GetCustomersByCountry() listato 13. La funzione GetCustomersByCountry() viene chiamata quando un utente finale fa clic su un pulsante nella pagina.

**Listato 13. Chiamata di un servizio Web con un proxy di JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample17.js)]

Lo spazio dei nomi InterfaceTraining fa riferimento a questa chiamata, CustomersService classe e metodo Web GetCustomersByCountry definito nel servizio. Passa un valore del paese ottenuto da una casella di testo, nonché una funzione di callback denominato OnWSRequestComplete che deve essere richiamato al termine della chiamata servizio Web asincrona. OnWSRequestComplete gestisce la matrice di oggetti Customer restituita dal servizio e li converte in una tabella che viene visualizzata nella pagina. L'output generato dalla chiamata è illustrato nella figura 1.


[![Associazione di dati ottenuti eseguendo una chiamata AJAX asincrona a un servizio Web.](understanding-asp-net-ajax-web-services/_static/image2.png)](understanding-asp-net-ajax-web-services/_static/image1.png)

**Figura 1**: associazione di dati ottenuto eseguendo una chiamata AJAX asincrona a un servizio Web.  ([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-asp-net-ajax-web-services/_static/image3.png))


I proxy di JavaScript possono inoltre effettuare chiamate unidirezionali per servizi Web nei casi in cui deve essere chiamato un metodo Web ma il proxy non deve attendere una risposta. Ad esempio, è possibile chiamare un servizio Web per avviare un processo, ad esempio un flusso di lavoro ma non attende il valore restituito dal servizio. Nei casi in cui una chiamata unidirezionale deve essere eseguita a un servizio, è possibile omettere semplicemente la funzione di callback illustrata nel listato 13. Poiché nessuna funzione di callback viene definita l'oggetto proxy non attenderanno il completamento per il servizio Web restituire i dati.

## <a name="handling-errors"></a>Gestione degli errori

Chiamate asincrone ai servizi Web possono verificarsi diversi tipi di errori, ad esempio la rete inattivo, il servizio Web non disponibile oppure la restituzione di un'eccezione. Fortunatamente, gli oggetti proxy di JavaScript generati da ScriptManager consentono più callback da definire per gestire gli errori oltre il callback di esito positivo illustrato in precedenza. Una funzione di callback di errore può essere definita immediatamente dopo la funzione di callback standard nella chiamata al metodo Web come illustrato nel listato 14.

**Listato 14. La definizione di una funzione di callback di errore e visualizzare gli errori.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample18.js)]

Eventuali errori che si verificano quando viene chiamato il servizio Web verranno attivata la funzione di callback OnWSRequestFailed() da chiamare, che accetta un oggetto che rappresenta l'errore come parametro. L'oggetto errore espone diverse funzioni differenti per determinare la causa dell'errore, nonché se il timeout della chiamata. Listato 14 mostra un esempio di utilizzo delle funzioni di errore diversi e la figura 2 mostra un esempio di output generati dalle funzioni.


[![Output generato dalla chiamata di funzioni di errore ASP.NET AJAX.](understanding-asp-net-ajax-web-services/_static/image5.png)](understanding-asp-net-ajax-web-services/_static/image4.png)

**Figura 2**: Output generato dalla chiamata di funzioni di errore ASP.NET AJAX.  ([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-asp-net-ajax-web-services/_static/image6.png))


## <a name="handling-xml-data-returned-from-a-web-service"></a>La gestione dei dati XML restituiti da un servizio Web

In precedenza è stato illustrato come un metodo Web può restituire dati XML non elaborati usando l'attributo ScriptMethod con la relativa proprietà ResponseFormat. Quando ResponseFormat è impostato su ResponseFormat.Xml, i dati restituiti dal servizio Web viene serializzati come XML anziché come JSON. Ciò può essere utile quando i dati XML devono essere passati direttamente al client per l'elaborazione usando JavaScript o XSLT. Al momento, Internet Explorer 5 o versione successiva fornisce il modello a oggetti lato client ottimo per l'analisi e filtro dei dati XML a causa di un proprio supporto incorporato per MSXML.

Recupero di dati XML da un servizio Web non è diverso rispetto al recupero di altri tipi di dati. Avviare richiamando il proxy di JavaScript per chiamare la funzione appropriata e definire una funzione di callback. Dopo la chiamata restituisce è quindi possibile elaborare i dati nella funzione di callback.

Listato 15 viene illustrato un esempio della chiamata del metodo Web denominato GetRssFeed() che restituisce un oggetto XmlElement. GetRssFeed() accetta un singolo parametro che rappresenta l'URL per il feed RSS per recuperare.

**Listato 15. Utilizzo di dati XML restituiti da un servizio Web.**

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample19.html)]

In questo esempio passa un URL a un feed RSS ed elabora i dati XML restituiti nella funzione OnWSRequestComplete(). OnWSRequestComplete() verifica innanzitutto se il browser viene visualizzato Internet Explorer per sapere se il parser MSXML è disponibile. Se si tratta, un'istruzione XPath serve per individuare tutti i &lt;elemento&gt; tag all'interno del feed RSS. Ogni elemento viene quindi eseguita l'iterazione tramite associate &lt;title&gt; e &lt;collegamento&gt; i tag sono Trova ed elaborati per visualizzare i dati di ogni elemento. Figura 3 mostra un esempio dell'output generato dall'esecuzione di ASP.NET AJAX chiamata attraverso un proxy di JavaScript per il metodo Web GetRssFeed().

## <a name="handling-complex-types"></a>Gestione dei tipi complessi

I tipi complessi accettato o restituiti da un servizio Web vengono esposti automaticamente attraverso un proxy di JavaScript. Tuttavia, i tipi complessi annidati non sono direttamente accessibili sul lato client, a meno che l'attributo GenerateScriptType viene applicato al servizio, come illustrato in precedenza. Motivo per cui si vuole usare un tipo complesso annidato sul lato client?

Per rispondere a questa domanda, si supponga che una pagina ASP.NET AJAX consente di visualizzare i dati dei clienti e consente agli utenti di aggiornare l'indirizzo del cliente. Se il servizio Web specifica che il tipo di indirizzo (un tipo complesso definito all'interno di una classe CustomerDetails) può essere inviato al client il processo di aggiornamento può essere suddivisi in funzioni separate per una migliore riutilizzo del codice.


[![Output di creazione dalla chiamata a un servizio Web che restituisce i dati RSS.](understanding-asp-net-ajax-web-services/_static/image8.png)](understanding-asp-net-ajax-web-services/_static/image7.png)

**Figura 3**: creazione di chiamare un servizio Web che restituisce i dati RSS di Output.  ([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-asp-net-ajax-web-services/_static/image9.png))


Listato 16 mostra un esempio di codice lato client che richiama un oggetto definito in uno spazio dei nomi del modello, vi collocherà i dati aggiornati e lo assegna a proprietà dell'indirizzo dell'oggetto CustomerDetails. L'oggetto CustomerDetails viene quindi passato al servizio Web per l'elaborazione.

**Listato 16. Utilizzo dei tipi complessi annidati**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample20.js)]

## <a name="creating-and-using-page-methods"></a>Creazione e utilizzo di metodi di pagina

Servizi Web offrono un modo eccellente per esporre servizi generiche riutilizzabili a un'ampia gamma di client, incluse le pagine ASP.NET AJAX. Tuttavia, potrebbero esserci casi in cui una pagina deve recuperare i dati che non verranno mai essere usati o condivisi da altre pagine. In questo caso, rendendo un file con estensione ASMX per consentire la pagina di accesso ai dati può sembrare eccessiva poiché il servizio viene usato solo da una singola pagina.

ASP.NET AJAX fornisce un altro meccanismo per eseguire chiamate di simile a servizio Web senza creare file con estensione asmx autonomo. Questa operazione viene eseguita usando una tecnica detta "metodi di pagina". I metodi di pagina sono metodi statici (condivisi in VB.NET) incorporati direttamente in un file di pagina o code-beside che hanno l'attributo WebMethod è applicato a essi. Applicando l'attributo WebMethod possono essere chiamati usando un oggetto JavaScript speciale denominato PageMethods che viene creata dinamicamente in fase di esecuzione. L'oggetto PageMethods funge da proxy che si protegge dal processo di serializzazione/deserializzazione di JSON. Si noti che per usare l'oggetto PageMethods è necessario impostare proprietà EnablePageMethods di ScriptManager su true.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample21.aspx)]

Listato 17 mostra un esempio di definizione dei due metodi di pagina in una classe di tipo code-beside ASP.NET. Questi metodi di recuperano dati da una classe di livello aziendale che si trova nell'App\_cartella del codice del sito Web.

**Listato 17. Che definisce i metodi di pagina.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample22.cs)]

Quando ScriptManager rileva la presenza dei metodi Web nella pagina viene generato un riferimento dinamico per l'oggetto PageMethods indicato in precedenza. Chiamata a un metodo Web è possibile fare riferimento alla classe PageMethods seguita dal nome del metodo e i dati di parametro necessari che devono essere passati. Listato 18 mostra esempi di chiamate i due metodi di pagina mostrati in precedenza.

**Elenco di 18. Chiamata di metodi di pagina con l'oggetto PageMethods JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample23.js)]

Utilizzando l'oggetto PageMethods è molto simile all'uso di un oggetto proxy JavaScript. È innanzitutto necessario specificare tutti i dati di parametro che devono essere passati al metodo di pagina e quindi definiscono la funzione di callback che deve essere chiamata al termine della chiamata asincrona. È inoltre possibile specificare un callback di errore (consultare il listato 14 per un esempio di gestione degli errori).

## <a name="the-autocompleteextender-and-the-aspnet-ajax-toolkit"></a>Il AutoCompleteExtender e ASP.NET AJAX Toolkit

ASP.NET AJAX Control Toolkit (disponibile dal [ http://ajax.asp.net ](http://ajax.asp.net)) offre diversi controlli che possono essere utilizzati per accedere ai servizi Web. In particolare, il toolkit contiene un controllo molto utile denominato `AutoCompleteExtender` che può essere utilizzato per chiamare i servizi Web e visualizzare i dati in pagine senza scrivere alcun codice JavaScript affatto.

Il controllo AutoCompleteExtender può essere utilizzato per estendere le funzionalità esistenti di una casella di testo e consentire agli utenti più di individuare facilmente i dati che cercano. Digitazione in una casella di testo il controllo può essere utilizzato per eseguire query di un servizio Web e Mostra i risultati sotto la casella di testo in modo dinamico. Figura 4 illustra un esempio di utilizzo del controllo AutoCompleteExtender per visualizzare gli ID dei clienti per un'applicazione di supporto. L'utente digita caratteri diversi nella casella di controllo, diversi elementi verranno visualizzati di seguito in base al relativo input. Gli utenti possono quindi selezionare l'id del cliente desiderato.

Usando il AutoCompleteExtender all'interno di una pagina ASP.NET AJAX, è necessario aggiungere l'assembly AjaxControlToolkit. dll alla cartella bin del sito Web. Dopo aver aggiunto l'assembly toolkit, è opportuno per farvi riferimento nel file Web. config in modo che controlli in che esso contenute sono disponibili per tutte le pagine in un'applicazione. Questa operazione può essere eseguita aggiungendo il seguente tag all'interno di Web. config &lt;controlli&gt; tag:

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample24.xml)]

Nei casi in cui è sufficiente utilizzare il controllo in una pagina specifica è possibile farvi riferimento aggiungendo la direttiva di riferimento all'inizio di una pagina, come indicato di seguito piuttosto che l'aggiornamento di Web. config:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample25.aspx)]


[![Utilizzo del controllo AutoCompleteExtender.](understanding-asp-net-ajax-web-services/_static/image11.png)](understanding-asp-net-ajax-web-services/_static/image10.png)

**Figura 4**: uso del controllo AutoCompleteExtender.  ([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-asp-net-ajax-web-services/_static/image12.png))


Dopo aver configurato il sito Web per usare il Toolkit di AJAX ASP.NET, un AutoCompleteExtender controllo può essere aggiunto nella pagina molto analogo all'aggiunta di un controllo server ASP.NET standard. Listato 19 mostra un esempio di utilizzo del controllo per chiamare un servizio Web.

**Listato 19. Utilizzo del controllo di ASP.NET AJAX Toolkit AutoCompleteExtender.**

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample26.aspx)]

Il AutoCompleteExtender ha proprietà diverse tra cui le proprietà standard di ID e runat presenti nei controlli server. Oltre a queste, consente di definire il numero di caratteri viene eseguita una query di un tipo di utente finale prima che il servizio Web per i dati. Proprietà MinimumPrefixLength illustrata nel listato 19, il servizio da chiamare ogni volta che viene digitato un carattere nella casella di testo. È opportuno prestare attenzione a impostare questo valore perché ogni volta che l'utente digita un carattere, il servizio Web verrà chiamato per cercare valori che corrispondono ai caratteri nella casella di testo. Il servizio Web da chiamare, nonché il metodo Web di destinazione viene definita utilizzando le proprietà ServicePath e ServiceMethod rispettivamente. Infine, la proprietà TargetControlID identifica quali nella casella di testo per l'associazione tra il controllo AutoCompleteExtender.

Il servizio Web chiamato devono avere l'attributo ScriptService applicata come illustrato in precedenza e il metodo Web di destinazione deve accettare due parametri denominati prefixText e conteggio. Il parametro prefixText rappresenta i caratteri digitati dall'utente finale e il parametro count rappresenta il numero di elementi da restituire (il valore predefinito è 10). Listato 20 Mostra un esempio del metodo Web GetCustomerIDs chiamato dal controllo AutoCompleteExtender illustrato in precedenza nell'elenco 19. Il metodo Web chiama un metodo di livello aziendale che a sua volta chiama una data di livello metodo che gestisce il filtro dei dati e la restituzione di risultati di corrispondenza. Il codice per il metodo di livello dati è illustrato nel listato 21.

**Elenco di 20. Filtrare i dati inviati dal controllo AutoCompleteExtender.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample27.cs)]

**Listato 21. Filtrare i risultati in base all'input dell'utente finale.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample28.cs)]

## <a name="conclusion"></a>Conclusione

ASP.NET AJAX fornisce un supporto eccellente per la chiamata dei servizi Web senza scrivere una grande quantità di codice JavaScript personalizzato per gestire i messaggi di richiesta e risposta. In questo articolo si è visto come servizi Web di .NET abilitare AJAX per consentire loro di elaborare i messaggi JSON e come definire i proxy di JavaScript usando il controllo ScriptManager. Si è visto anche come JavaScript proxy può essere utilizzato per chiamare i servizi Web, gestire i tipi semplici e complessi e gestire gli errori. Infine, si è visto come i metodi di pagina possono essere usati per semplificare il processo di creazione e l'esecuzione di chiamate di servizio Web e come il controllo AutoCompleteExtender può fornire assistenza agli utenti finali durante la digitazione. Nonostante UpdatePanel di ASP.NET AJAX sarà senza dubbio il controllo di scelta per molti programmatori AJAX a causa la sua semplicità, può essere utile in molte applicazioni che consente di chiamare servizi Web tramite i proxy di JavaScript.

## <a name="bio"></a>Informazioni biografiche

Dan Wahlin (Microsoft Most Valuable Professional per ASP.NET e servizi Web XML) è un .NET development instructor e architettura di consulente in formazione tecnica di interfaccia ([http://www.interfacett.com](http://www.interfacett.com)). Dan ha fondato il codice XML per il sito Web di ASP.NET Developers ([www.XMLforASP.NET](http://www.XMLforASP.NET)), si trova in ufficio del relatore di INETA e come relatore a diverse conferenze. Dan coautore DNA Windows Professional (Wrox), ASP.NET: suggerimenti, esercitazioni e codice (SAM), ASP.NET 1.1 Insider soluzioni, Professional ASP.NET 2.0 AJAX (Wrox), MVP di ASP.NET 2.0 HACK e create XML per sviluppatori ASP.NET (SAM). Quando non sta attualmente scrivendo codice, articoli o libri, Dan piace la scrittura e la registrazione della musica e la riproduzione di golf e pallacanestro con sua moglie e dei bambini.

Scott Cate ha collaborato con tecnologie Web di Microsoft dal 1997 ed è il presidente di myKB.com ([www.myKB.com](http://www.myKB.com)) ed è specializzato nella scrittura di ASP.NET basato su applicazioni incentrate su soluzioni Software di Knowledge Base. È possibile contattare Scott tramite posta elettronica all'indirizzo [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) o il suo blog all'indirizzo [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Precedente](understanding-asp-net-ajax-localization.md)
> [Successivo](understanding-asp-net-ajax-debugging-capabilities.md)
