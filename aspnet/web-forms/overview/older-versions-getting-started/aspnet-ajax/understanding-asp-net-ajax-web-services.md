---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
title: Informazioni sui servizi Web ASP.NET AJAX | Documenti Microsoft
author: scottcate
description: Servizi Web sono parte integrante di .NET framework che forniscono una soluzione multipiattaforma per lo scambio di dati tra i sistemi distribuiti. Anche se Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 3332d6e7-e2e1-4144-b805-e71d51e7e415
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
msc.type: authoredcontent
ms.openlocfilehash: 0b9f61f895fea1960ebd25780454b86d5c3ba1bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889342"
---
<a name="understanding-aspnet-ajax-web-services"></a>Informazioni sui servizi Web ASP.NET AJAX
====================
da [Scott categorie](https://github.com/scottcate)

[Scarica il PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial05_Web_Services_with_MS_Ajax_cs.pdf)

> Servizi Web sono parte integrante di .NET framework che forniscono una soluzione multipiattaforma per lo scambio di dati tra i sistemi distribuiti. Anche se servizi Web vengono in genere utilizzati per consentire diversi sistemi operativi, modelli a oggetti e linguaggi di programmazione per inviare e ricevere dati, possono anche essere utilizzati per inserire dati in una pagina ASP.NET AJAX in modo dinamico o inviare i dati da una pagina a un sistema back-end. Tutto ciò può essere eseguita senza ricorrere per operazioni di postback.


## <a name="calling-web-services-with-aspnet-ajax"></a>Chiamata dei servizi Web con ASP.NET AJAX

Dan Wahlin

Servizi Web sono parte integrante di .NET framework che forniscono una soluzione multipiattaforma per lo scambio di dati tra i sistemi distribuiti. Anche se servizi Web vengono in genere utilizzati per consentire diversi sistemi operativi, modelli a oggetti e linguaggi di programmazione per inviare e ricevere dati, possono anche essere utilizzati per inserire dati in una pagina ASP.NET AJAX in modo dinamico o inviare i dati da una pagina a un sistema back-end. Tutto ciò può essere eseguita senza ricorrere per operazioni di postback.

Anche se il controllo UpdatePanel di ASP.NET AJAX fornisce un modo semplice per AJAX abilitare tutte le pagine ASP.NET, può accadere quando è necessario accedere in modo dinamico i dati nel server senza utilizzare un UpdatePanel. In questo articolo si noterà come effettuare questa operazione mediante la creazione e utilizzo di servizi Web all'interno di pagine ASP.NET AJAX.

Questo articolo è incentrato sulle funzionalità disponibili in ASP.NET AJAX Extensions core, nonché un controllo del servizio Web abilitata in ASP.NET AJAX Toolkit chiamato il AutoCompleteExtender. Gli argomenti trattati includono la definizione dei servizi Web di supporto AJAX, la creazione di proxy client e chiamare i servizi Web con JavaScript. Si noterà come servizio Web è possibile chiamare direttamente ai metodi di pagina ASP.NET.

## <a name="web-services-configuration"></a>Configurazione dei servizi Web

Quando un nuovo progetto di sito Web viene creato con Visual Studio 2008, il file Web. config ha un numero di nuove funzionalità che possono essere poco note agli utenti di versioni precedenti di Visual Studio. Il prefisso "asp" alcune di queste modifiche vengono mappate a controlli ASP.NET AJAX possono essere utilizzati nelle pagine mentre altri vengono definite obbligatori HttpHandlers e moduli HTTP. Elenco 1 mostra le modifiche apportate al `<httpHandlers>` elemento nel file Web. config che influisce sulle chiamate al servizio Web. Il valore predefinito di che gestore HTTP utilizzato per elaborare le chiamate con estensione asmx rimosse e sostituito con una classe ScriptHandlerFactory presente nell'assembly System.Web.Extensions.dll. System.Web.Extensions.dll contiene tutte le funzionalità di base utilizzate da ASP.NET AJAX.

**Elenco 1. Configurazione del gestore del servizio Web ASP.NET AJAX**

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample1.xml)]

Questo gestore HTTP sostituzione viene eseguita per consentire chiamate JavaScript Object Notation (JSON) essere composto da pagine ASP.NET AJAX a servizi Web .NET utilizzando un proxy del servizio Web di JavaScript. ASP.NET AJAX invia i messaggi JSON ai servizi Web anziché le chiamate SOAP Simple Object Access Protocol () standard, in genere associate a servizi Web. Il risultato più piccolo messaggi di richiesta e risposta generali. Consente inoltre di un'elaborazione più efficiente sul lato client dei dati poiché la libreria JavaScript di ASP.NET AJAX è ottimizzata per l'utilizzo di oggetti JSON. Elenco di 2 e 3 elenco mostra esempi di messaggi di richiesta e risposta serializzati in formato JSON del servizio Web. Il messaggio di richiesta listato 2 passa un parametro di paese e il valore di "Belgio" mentre il messaggio di risposta nel listato 3 passa una matrice di oggetti Customer e le relative proprietà.

**Elenco di 2. Messaggio di richiesta del servizio Web serializzato in JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample2.json)]

> *> [!NOTE] il nome dell'operazione è definito come parte dell'URL al servizio web. Inoltre, i messaggi di richiesta non vengono sempre inviati tramite JSON. Servizi Web possono utilizzare l'attributo ScriptMethod con il parametro UseHttpGet impostato su true, che fa sì che i parametri deve essere passato tramite un parametri della stringa di query.*


**Elenco di 3. Messaggio di risposta del servizio Web serializzato in JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample3.json)]

Nella sezione successiva si noterà come creare servizi Web in grado di gestire i messaggi di richiesta JSON e risponde con tipi semplici e complessi.

## <a name="creating-ajax-enabled-web-services"></a>Creazione di servizi Web compatibili con AJAX

Il framework ASP.NET AJAX fornisce diversi modi per chiamare i servizi Web. È possibile utilizzare il controllo AutoCompleteExtender (disponibile in ASP.NET AJAX Toolkit) o JavaScript. Tuttavia, prima di chiamare un servizio è necessario abilitare AJAX, in modo che può essere chiamato da codice di script client.

Se si ha familiarità con servizi Web ASP.NET, sono disponibili semplici da creare e abilitare AJAX servizi. .NET framework è supportata la creazione di servizi Web ASP.NET fin dalla versione iniziale 2002 e ASP.NET AJAX Extensions forniscono funzionalità AJAX aggiuntive che si basa il set predefinito di .NET framework di funzionalità. Visual Studio .NET 2008 Beta 2 include il supporto incorporato per la creazione di file del servizio Web ASMX e deriva automaticamente associato code-beside classi dalla classe WebService. Quando si aggiungono metodi nella classe è necessario applicare l'attributo WebMethod affinché possano essere chiamato dagli utenti del servizio Web.

Listato 4 viene illustrato un esempio di utilizzo dell'attributo WebMethod a un metodo denominato GetCustomersByCountry().

**Listato 4. Utilizzando l'attributo WebMethod in un servizio Web**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample4.cs)]

Il metodo GetCustomersByCountry() accetta un parametro di paese e restituisce un cliente come matrice di oggetti. Il valore passato al metodo del paese viene inoltrato a una classe di livello aziendale che a sua volta chiama una classe di livello dati per recuperare i dati dal database, inserire le proprietà dell'oggetto Customer dati e restituire la matrice.

## <a name="using-the-scriptservice-attribute"></a>Utilizzo dell'attributo ScriptService

Durante l'aggiunta di WebMethod attributo consente al metodo GetCustomersByCountry() deve essere chiamato da client che inviano messaggi SOAP standard per il servizio Web, quindi non consente chiamate JSON da applicazioni ASP.NET AJAX. Per permettere le chiamate JSON da eseguire è necessario applicare l'estensione di ASP.NET AJAX `ScriptService` attributo alla classe di servizio Web. Questo consente a un servizio Web inviare messaggi di risposta formattati mediante JSON e sul lato client script chiamare un servizio mediante l'invio di messaggi JSON.

Nel listato 5 mostra un esempio di applicazione dell'attributo ScriptService a una classe di servizio Web denominata CustomersService.

**Elenco di 5. Utilizzo dell'attributo ScriptService per AJAX-attivare un servizio Web**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample5.cs)]

L'attributo ScriptService agisce come un indicatore che segnala che può essere chiamato dal codice di script AJAX. In realtà non gestisce le attività di serializzazione o deserializzazione JSON che si verificano in background. Eseguono la maggior parte dell'elaborazione JSON di ScriptHandlerFactory (configurato nel file Web. config) e altre classi correlate.

## <a name="using-the-scriptmethod-attribute"></a>Utilizzo dell'attributo ScriptMethod

L'attributo ScriptService è l'unico attributo di ASP.NET AJAX che deve essere definito in un servizio Web .NET in modo tale da utilizzare per le pagine ASP.NET AJAX. Tuttavia, un altro attributo denominato ScriptMethod può anche essere applicato direttamente alla metodi Web in un servizio. ScriptMethod definisce tre proprietà, tra cui `UseHttpGet`, `ResponseFormat` e `XmlSerializeString`. Modifica dei valori di queste proprietà può essere utile nei casi in cui il tipo di richiesta accettata da un metodo Web deve essere modificate a GET, quando un metodo Web deve restituire dati XML non elaborati in forma di un `XmlDocument` o `XmlElement` oggetto o quando i dati restituiti da un  servizio deve sempre essere serializzato in formato XML anziché JSON.

La proprietà può essere utilizzata quando il metodo Web deve accettare di UseHttpGet ottenere richieste opposizione alle richieste POST. Le richieste vengono inviate tramite un URL con parametri di input di metodo Web convertiti in parametri di stringa di query. UseHttpGet proprietà valore predefinito è false e deve solo essere impostato su `true` quando operazioni sono note risultano sicure, quando i dati sensibili non viene passati a un servizio Web. Elenco 6 mostra un esempio di utilizzo dell'attributo ScriptMethod con la proprietà UseHttpGet.

**Elenco di 6. Utilizzo dell'attributo ScriptMethod con la proprietà UseHttpGet.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample6.cs)]

Un esempio delle intestazioni inviato quando viene chiamato il metodo Web HttpGetEcho del listato 6 presenti Avanti:

`GET /CustomerViewer/DemoService.asmx/HttpGetEcho?input=%22Input Value%22 HTTP/1.1`

Oltre a consentire metodi Web accettare le richieste HTTP GET, il ScriptMethod attributo può essere utilizzato anche quando le risposte XML devono essere restituito da un servizio anziché JSON. Ad esempio, un servizio Web può recuperare un feed RSS da un sito remoto e restituirlo come un oggetto XmlDocument o XmlElement. Elaborazione del codice XML dei dati possono essere quindi sul client.

Elenco 7 mostra un esempio di utilizzo della proprietà ResponseFormat per specificare che i dati XML devono essere restituiti da un metodo Web.

**Elenco di 7. Utilizzo dell'attributo ScriptMethod con la proprietà ResponseFormat.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample7.cs)]

La proprietà di ResponseFormat può anche essere utilizzata insieme alla proprietà XmlSerializeString. La proprietà XmlSerializeString ha un valore predefinito è false, il che significa che tutti i tipi restituiti, ad eccezione di stringhe restituite dal metodo Web vengono serializzati come XML quando la `ResponseFormat` è impostata su `ResponseFormat.Xml`. Quando `XmlSerializeString` è impostato su `true`, tutti i tipi restituiti da un metodo Web vengono serializzati come XML, inclusi i tipi di stringa. Se la proprietà ResponseFormat ha un valore di `ResponseFormat.Json` la proprietà XmlSerializeString viene ignorata.

Nel listato 8 viene illustrato un esempio dell'utilizzo della proprietà XmlSerializeString per forzare le stringhe per essere serializzato come XML.

**Elenco di 8. Utilizzo dell'attributo ScriptMethod con la proprietà XmlSerializeString**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample8.cs)]

Il valore restituito dalla chiamata al metodo Web GetXmlString mostrato nel listato 8 viene riportato una:

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample9.cs)]

Anche se il formato JSON predefinito consente di ridurre la dimensione complessiva dei messaggi di richiesta e risposta e, in modo più browser, più rapidamente viene utilizzato dai client AJAX ASP.NET, le proprietà ResponseFormat e XmlSerializeString possono essere utilizzato quando client le applicazioni, ad esempio Internet Explorer 5 o versione successiva prevedono dati XML restituito da un metodo Web.

## <a name="working-with-complex-types"></a>Utilizzo di tipi complessi

Nel listato 5 è stato illustrato un esempio di restituzione di un tipo complesso denominato cliente da un servizio Web. La classe Customer definisce diversi tipi semplici internamente come proprietà, ad esempio FirstName e LastName. I tipi complessi usati come un parametro di input o il tipo restituito in un metodo Web compatibili con AJAX vengono serializzati in JSON automaticamente prima dell'invio al lato client. Tuttavia, i tipi complessi annidati, ovvero definito internamente all'interno di un altro tipo, non sono disponibili al client come oggetti autonomi per impostazione predefinita.

In casi in cui un tipo complesso annidato utilizzato da un servizio Web deve inoltre essere utilizzato in una pagina client, aggiungere l'attributo di ASP.NET AJAX GenerateScriptType al servizio Web. Ad esempio, la classe CustomerDetails listato 9 contiene proprietà indirizzo e Gender cui ***rappresentano tipi complessi annidati.***

**Elenco 9. La classe CustomerDetails illustrata di seguito contiene due tipi complessi annidati.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample10.cs)]

Gli oggetti indirizzo e il sesso definiti all'interno della classe CustomerDetails listato 9 non verranno automaticamente resi disponibili per l'utilizzo sul lato client tramite JavaScript poiché sono i tipi annidati (indirizzo è una classe e il sesso è un'enumerazione). In situazioni in cui un tipo annidato utilizzato all'interno di un servizio Web deve essere disponibile sul lato client, è possibile utilizzare l'attributo GenerateScriptType indicato in precedenza (vedere Listato 10). Questo attributo può essere aggiunto più volte in casi in cui i diversi tipi complessi annidati vengono restituiti da un servizio. Può essere applicato direttamente alla classe di servizio Web o di sopra di metodi Web specifici.

**Elenco di 10. Utilizzo dell'attributo GenerateScriptService per definire i tipi annidati che devono essere disponibili al client.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample11.cs)]

Applicando la `GenerateScriptType` attributo per i tipi di servizio Web, l'indirizzo e il sesso verrà reso automaticamente disponibile per l'utilizzo dal codice ASP.NET AJAX JavaScript sul lato client. Nel listato 11 è illustrato un esempio di codice JavaScript che viene automaticamente generato e inviato al client, aggiungere l'attributo GenerateScriptType su un servizio Web. Si noterà come usare tipi complessi annidati in un secondo momento nell'articolo.

**Elenco di 11. Tipi complessi annidati resi disponibili a una pagina ASP.NET AJAX.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample12.cs)]

Ora che è stato illustrato come creare servizi Web e renderli accessibili alle pagine ASP.NET AJAX, diamo un'occhiata a come creare e utilizzare i proxy di JavaScript in modo che i dati è possibile recuperare o inviare ai servizi Web.

## <a name="creating-javascript-proxies"></a>Creazione di proxy di JavaScript

La chiamata a un servizio Web standard (.NET o un'altra piattaforma) in genere prevede la creazione di un oggetto proxy che si protegge dalla complessità di inviare messaggi di richiesta e risposta SOAP. Con chiamate al servizio Web di ASP.NET AJAX, proxy di JavaScript possono essere creati e utilizzati per chiamare facilmente i servizi senza doversi preoccupare di serializzare e deserializzare i messaggi JSON. Proxy di JavaScript può essere generato automaticamente utilizzando il controllo ScriptManager di AJAX ASP.NET.

La creazione di un proxy JavaScript che può chiamare servizi Web viene eseguita utilizzando proprietà di servizi di ScriptManager. Questa proprietà consente di definire uno o più servizi che è possibile chiamare in modo asincrono una pagina ASP.NET AJAX per inviare o ricevere dati senza richiedere operazioni di postback. Definire un servizio utilizzando ASP.NET AJAX `ServiceReference` controllo e assegnando l'URL del servizio Web per il controllo `Path` proprietà. Elenco di 12, viene illustrato un esempio per fare riferimento a un servizio denominato CustomersService.asmx.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample13.aspx)]

**Elenco di 12. Definizione di un servizio Web utilizzato in una pagina ASP.NET AJAX.**

L'aggiunta di un riferimento di CustomersService.asmx tramite il controllo ScriptManager provoca un proxy JavaScript essere generati dinamicamente e a cui fa riferimento la pagina. Il proxy incorporato mediante il &lt;script&gt; tag e caricata in modo dinamico chiamando il file CustomersService.asmx e aggiunta /js alla fine di esso. Nell'esempio seguente viene illustrato come il proxy JavaScript incorporato nella pagina durante il debug è disabilitato in Web. config:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample14.html)]

> *> [!NOTE] Se si desidera visualizzare il codice effettivo di proxy JavaScript che viene generato è possibile digitare l'URL del servizio Web .NET desiderato nella casella Indirizzo Internet Explorer e aggiungere /js alla fine di esso.*


Se è abilitato il debug in Web. config di che una versione di debug del proxy JavaScript verrà incorporata alla pagina come illustrato sotto:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample15.html)]

Il proxy JavaScript creato da ScriptManager possono inoltre essere incorporato direttamente nella pagina anziché a cui viene fatto riferimento tramite il &lt;script&gt; attributo src del tag. Questa operazione può essere eseguita impostando la proprietà di InlineScript del controllo ServiceReference su true (il valore predefinito è false). Può essere utile quando un proxy non è condivise tra più pagine e quando si desidera ridurre il numero di chiamate di rete al server. Quando InlineScript è impostato su true dello script proxy non memorizzare nella cache dal browser in modo il valore predefinito false è consigliato nei casi in cui il proxy utilizzato da più pagine in un'applicazione AJAX ASP.NET. Usare la proprietà di InlineScript l'esempio successivo:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample16.aspx)]

## <a name="using-javascript-proxies"></a>Utilizzo di proxy di JavaScript

Una volta che un servizio Web viene fatto riferimento da una pagina ASP.NET AJAX tramite il controllo ScriptManager, può essere effettuata una chiamata al servizio Web e i dati restituiti possono essere gestiti utilizzando funzioni di callback. Un servizio Web è chiamato dal relativo spazio dei nomi di riferimento (se presente), il nome di classe e nome del metodo Web. È possibile definire tutti i parametri passati al servizio Web insieme a una funzione di callback che gestisce i dati restituiti.

Viene illustrato un esempio di utilizzo di un proxy JavaScript per chiamare un metodo Web denominato GetCustomersByCountry() listato 13. La funzione GetCustomersByCountry() viene chiamata quando un utente finale fa clic su un pulsante nella pagina.

**Elenco di 13. Chiamata di un servizio Web con un proxy JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample17.js)]

Lo spazio dei nomi InterfaceTraining fa riferimento a questa chiamata, CustomersService classe e metodo Web GetCustomersByCountry definita nel servizio. Passa un valore del paese ottenuto da una casella di testo, nonché una funzione di callback denominato OnWSRequestComplete che deve essere richiamato al termine della chiamata asincrona al servizio Web. OnWSRequestComplete gestisce la matrice di oggetti restituiti dal servizio e li converte in una tabella che viene visualizzata nella pagina. L'output generato dalla chiamata è illustrato nella figura 1.


[![Associazione di dati ottenuti eseguendo una chiamata AJAX asincrona a un servizio Web.](understanding-asp-net-ajax-web-services/_static/image2.png)](understanding-asp-net-ajax-web-services/_static/image1.png)

**Figura 1**: associazione di dati ottenuto eseguendo una chiamata AJAX asincrona a un servizio Web.  ([Fare clic per visualizzare l'immagine ingrandita](understanding-asp-net-ajax-web-services/_static/image3.png))


Proxy di JavaScript possono anche apportare unidirezionale chiamate ai servizi Web nei casi in cui deve essere chiamato un metodo Web, ma il proxy non deve attendere una risposta. Potrebbe ad esempio, si desidera chiamare un servizio Web per avviare un processo, ad esempio un flusso di lavoro ma non attende un valore restituito dal servizio. Nei casi in cui una chiamata unidirezionale deve essere apportata a un servizio, la funzione di callback listato 13 può semplicemente essere omessa. Poiché non è definita alcuna funzione di callback non attenderanno il completamento dell'oggetto proxy per il servizio Web restituire i dati.

## <a name="handling-errors"></a>Gestione degli errori

Chiamate asincrone a servizi Web possono verificarsi diversi tipi di errori, ad esempio la rete in caso di inattività, il servizio Web non è disponibile o un'eccezione viene restituito. Fortunatamente, gli oggetti proxy JavaScript generati da ScriptManager consentono più callback da definire per gestire errori e problemi oltre ai callback di esito positivo illustrato in precedenza. Una funzione di callback di errore può essere definita immediatamente dopo la funzione di callback standard nella chiamata al metodo Web come listato 14.

**Listato 14. Definizione di una funzione di callback di errore e la visualizzazione di errori.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample18.js)]

Gli eventuali errori che si verificano quando viene chiamato il servizio Web verranno attivata la funzione di callback OnWSRequestFailed() da chiamare che accetta un oggetto che rappresenta l'errore come parametro. L'oggetto errore espone numerose funzioni diverse per determinare la causa dell'errore, nonché un eventuale timeout della chiamata. Listato 14 mostra un esempio di utilizzo delle funzioni di errore diversi e la figura 2 mostra un esempio dell'output generato dalle funzioni.


[![Output generato chiamando le funzioni di errore ASP.NET AJAX.](understanding-asp-net-ajax-web-services/_static/image5.png)](understanding-asp-net-ajax-web-services/_static/image4.png)

**Figura 2**: Output generato chiamando le funzioni di errore ASP.NET AJAX.  ([Fare clic per visualizzare l'immagine ingrandita](understanding-asp-net-ajax-web-services/_static/image6.png))


## <a name="handling-xml-data-returned-from-a-web-service"></a>La gestione dei dati XML restituiti da un servizio Web

In precedenza si è visto come un metodo Web può restituire dati XML non elaborati tramite l'attributo ScriptMethod con la relativa proprietà ResponseFormat. Quando ResponseFormat è impostata su ResponseFormat.Xml, i dati restituiti dal servizio Web viene serializzati in formato XML anziché JSON. Può essere utile quando i dati XML devono essere passati direttamente al client per l'elaborazione con JavaScript o XSLT. Al momento, Internet Explorer 5 o versione successiva fornisce il migliore modello a oggetti di sul lato client per l'analisi e filtraggio dei dati XML a causa del relativo supporto incorporato per MSXML.

Recupero di dati XML da un servizio Web non è diverso dal recupero di altri tipi di dati. Avviare richiamando il proxy JavaScript per chiamare la funzione appropriata e definire una funzione di callback. Dopo la chiamata restituisce è quindi possibile elaborare i dati nella funzione di callback.

Elenco 15 mostra un esempio di chiamata di un metodo Web denominato GetRssFeed() che restituisce un oggetto XmlElement. GetRssFeed() accetta un singolo parametro che rappresenta l'URL per il feed RSS per recuperare.

**Elenco di 15. Utilizzo di dati XML restituiti da un servizio Web.**

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample19.html)]

In questo esempio passa un URL a un feed RSS ed elabora i dati XML restituiti nella funzione OnWSRequestComplete(). OnWSRequestComplete() controlla innanzitutto se il browser è Internet Explorer per sapere se il parser MSXML è disponibile. Questo caso, un'istruzione XPath viene utilizzata per individuare tutti &lt;elemento&gt; tag all'interno del feed RSS. Ogni elemento, quindi scorrere e associato &lt;titolo&gt; e &lt;collegamento&gt; tag vengono Trova ed elaborati per visualizzare i dati di ogni elemento. Figura 3 mostra un esempio dell'output generato dalla chiamata ASP.NET AJAX tramite un proxy JavaScript per il metodo Web GetRssFeed().

## <a name="handling-complex-types"></a>Gestione dei tipi complessi

Tipi complessi accettato o restituiti da un servizio Web vengono automaticamente esposti tramite un proxy JavaScript. Tuttavia, tipi complessi annidati non sono accessibili direttamente sul lato client, a meno che non è applicato l'attributo GenerateScriptType il servizio come illustrato in precedenza. Motivo per cui si desidera utilizzare un tipo complesso annidato sul lato client?

Per rispondere a questa domanda, si supponga che una pagina ASP.NET AJAX consente di visualizzare i dati dei clienti e agli utenti finali di aggiornare l'indirizzo del cliente. Se il servizio Web specifica che il tipo di indirizzo (un tipo complesso definito all'interno di una classe CustomerDetails) può essere inviato al client il processo di aggiornamento è diviso in funzioni separate per una migliore codice riutilizzo.


[![Output creazione dalla chiamata a un servizio Web che restituisce dati RSS.](understanding-asp-net-ajax-web-services/_static/image8.png)](understanding-asp-net-ajax-web-services/_static/image7.png)

**Figura 3**: creazione di chiamare un servizio Web che restituisce i dati RSS di Output.  ([Fare clic per visualizzare l'immagine ingrandita](understanding-asp-net-ajax-web-services/_static/image9.png))


Elenco di 16 mostra un esempio di codice sul lato client che richiama un oggetto indirizzo definito in uno spazio dei nomi di modello, vi collocherà i dati aggiornati e assegna alla proprietà dell'indirizzo dell'oggetto CustomerDetails. L'oggetto CustomerDetails viene quindi passato al servizio Web per l'elaborazione.

**Elenco di 16. Utilizzo di tipi complessi annidati**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample20.js)]

## <a name="creating-and-using-page-methods"></a>Creazione e utilizzo di metodi di pagina

Servizi Web forniscono un modo eccellente per esporre servizi riutilizzabili a un'ampia gamma di client, incluse le pagine ASP.NET AJAX. Tuttavia, potrebbe essere casi in cui una pagina deve recuperare i dati che non verranno mai utilizzati o condiviso da altre pagine. In questo caso, l'esecuzione di un file con estensione ASMX per consentire alla pagina di accesso ai dati può sembrare eccessivi poiché il servizio è usato solo da una singola pagina.

ASP.NET AJAX fornisce un altro meccanismo per le chiamate di tipo di servizio Web senza creare file con estensione asmx autonomo. Questa operazione viene eseguita utilizzando una tecnica detta "metodi di pagina". Pagina metodi sono statici (condivisi in Visual Basic.NET) incorporato direttamente in un file di pagina o di tipo code-beside che presentano l'attributo WebMethod applicata ad essi. Applicando l'attributo WebMethod possono essere chiamati tramite un oggetto JavaScript speciale denominato PageMethods che viene creata dinamicamente in fase di esecuzione. L'oggetto PageMethods funge da proxy che si protegge dal processo di serializzazione/deserializzazione di JSON. Si noti che per utilizzare l'oggetto PageMethods è necessario impostare proprietà EnablePageMethods di ScriptManager su true.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample21.aspx)]

Elenco di 17 mostra un esempio di definizione di due metodi di pagina in una classe di tipo code-beside ASP.NET. Questi metodi di recuperano dati da una classe di livello aziendale situata nell'App\_cartella del codice del sito Web.

**Elenco 17. Definizione di metodi di pagina.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample22.cs)]

Quando ScriptManager rileva la presenza di metodi Web nella pagina viene generato un riferimento dinamico per l'oggetto PageMethods indicato in precedenza. Chiama un metodo Web viene eseguito tramite fare riferimento alla classe PageMethods seguita dal nome del metodo e i dati di parametro necessari che devono essere passati. Elenco di 18, vengono illustrati esempi di chiamata dei due metodi di pagina illustrati in precedenza.

**Elenco di 18. Chiamare i metodi di pagina con l'oggetto PageMethods JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample23.js)]

Utilizzando l'oggetto PageMethods è molto simile all'utilizzo di un oggetto proxy JavaScript. È innanzitutto necessario specificare tutti i dati di parametro che devono essere passati al metodo di pagina e quindi definiscono la funzione di callback che deve essere chiamata al termine della chiamata asincrona. È inoltre possibile specificare un callback di errore (vedere Listato 14 per un esempio di gestione degli errori).

## <a name="the-autocompleteextender-and-the-aspnet-ajax-toolkit"></a>Il AutoCompleteExtender e ASP.NET AJAX Control Toolkit

Il Toolkit di AJAX ASP.NET (disponibile dal [ http://ajax.asp.net ](http://ajax.asp.net)) offre diversi controlli che possono essere utilizzati per accedere ai servizi Web. In particolare, il toolkit contiene un controllo utile denominato `AutoCompleteExtender` che può essere utilizzato per chiamare i servizi Web e visualizzare i dati nelle pagine senza scrivere alcun codice JavaScript affatto.

Il controllo AutoCompleteExtender può essere utilizzato per estendere le funzionalità esistenti di una casella di testo e consentire a più utenti di individuare facilmente i dati che cercano. Durante la digitazione in una casella di testo il controllo può essere utilizzato per richiedere un servizio Web e visualizzare i risultati sotto la casella di testo in modo dinamico. Figura 4 mostra un esempio di utilizzo del controllo AutoCompleteExtender per visualizzare gli ID cliente per un'applicazione di supporto. L'utente digita caratteri diversi nella casella di testo, verranno visualizzati diversi elementi sottostanti in base al relativo input. Gli utenti possono quindi selezionare l'id cliente desiderato.

Utilizzando il AutoCompleteExtender all'interno di una pagina ASP.NET AJAX, è necessario aggiungere l'assembly AjaxControlToolkit. dll nella cartella bin del sito Web. Dopo aver aggiunto l'assembly del toolkit, sarà possibile farvi riferimento nel file Web. config in modo che i controlli in che esso contenute sono disponibili a tutte le pagine in un'applicazione. Questa operazione può essere eseguita aggiungendo il seguente tag all'interno di Web. config &lt;controlli&gt; tag:

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample24.xml)]

Nei casi in cui è necessario utilizzare solo il controllo in una pagina è possibile farvi riferimento aggiungendo la direttiva di riferimento nella parte superiore di una pagina, come indicato di seguito anziché l'aggiornamento di Web. config:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample25.aspx)]


[![Utilizzo del controllo AutoCompleteExtender.](understanding-asp-net-ajax-web-services/_static/image11.png)](understanding-asp-net-ajax-web-services/_static/image10.png)

**Figura 4**: utilizzo del controllo AutoCompleteExtender.  ([Fare clic per visualizzare l'immagine ingrandita](understanding-asp-net-ajax-web-services/_static/image12.png))


Una volta il sito Web è stato configurato per usare il Toolkit di AJAX ASP.NET, un AutoCompleteExtender controllo può essere aggiunto alla pagina molto come aggiungere un controllo server ASP.NET standard. Elenco di 19 mostra un esempio di utilizzo del controllo per chiamare un servizio Web.

**Elenco 19. Utilizzo del controllo ASP.NET AJAX Toolkit AutoCompleteExtender.**

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample26.aspx)]

Il AutoCompleteExtender ha proprietà diverse tra cui le proprietà standard di ID e runat presenti nei controlli del server. Oltre a queste, è possibile definire il numero di caratteri viene eseguita una query di un tipo di utente finale prima che il servizio Web per i dati. La proprietà MinimumPrefixLength mostrata nell'elenco di 19, il servizio di essere chiamato ogni volta che viene digitato un carattere nella casella di testo. È opportuno prestare attenzione impostando questo valore perché ogni volta che l'utente digita un carattere, il servizio Web verrà chiamato per cercare valori che corrispondono ai caratteri nella casella di testo. Chiamare il servizio Web, nonché il metodo Web di destinazione viene definita utilizzando le proprietà ServicePath e ServiceMethod rispettivamente. Infine, la proprietà TargetControlID identifica quale casella di testo per il controllo AutoCompleteExtender per eseguire l'hook.

Il servizio Web chiamato deve avere l'attributo ScriptService applicato come indicato in precedenza e il metodo Web di destinazione deve accettare due parametri denominati prefixText e count. Il parametro prefixText rappresenta i caratteri digitati dall'utente finale e il parametro count rappresenta il numero di elementi per restituire (il valore predefinito è 10). Elenco di 20 viene illustrato un esempio del metodo Web GetCustomerIDs chiamato dal controllo AutoCompleteExtender illustrato in precedenza in elenco 19. Il metodo Web chiama un metodo di livello aziendale che a sua volta chiama un tipo di dati di livello metodo che gestisce il filtro dei dati e restituire i risultati corrispondenti. Il codice per il metodo di livello dati è illustrato nell'elenco 21.

**Elenco di 20. Filtraggio dei dati inviati dal controllo AutoCompleteExtender.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample27.cs)]

**Elenco 21. Filtro dei risultati in base all'input dell'utente finale.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample28.cs)]

## <a name="conclusion"></a>Conclusione

ASP.NET AJAX fornisce un supporto eccellente per richiamare servizi Web senza scrivere una quantità di codice JavaScript personalizzato per gestire i messaggi di richiesta e risposta. In questo articolo si è visto come servizi Web .NET di abilitare AJAX per consentire loro di elaborare i messaggi JSON e come definire il proxy JavaScript utilizzando il controllo ScriptManager. Si è visto anche come proxy può essere usato per chiamare i servizi Web, JavaScript di gestire i tipi semplici e complessi e gestire gli errori. Infine, è stato illustrato come utilizzare i metodi di pagina per semplificare il processo di creazione e rendendo chiamate al servizio Web e come il controllo AutoCompleteExtender grado di fornire agli utenti finali durante la digitazione. Anche se l'UpdatePanel disponibile in ASP.NET AJAX sarà certamente controllo ideale per molti programmatori AJAX a causa la semplicità, conoscere come chiamare servizi Web tramite il proxy JavaScript può risultare utile in molte applicazioni.

## <a name="bio"></a>Bio

Dan Wahlin (Microsoft Most Valuable Professional per ASP.NET e servizi Web XML) è un sviluppo docente e l'architettura consulente .NET al Training tecniche di interfaccia ([http://www.interfacett.com](http://www.interfacett.com)). Dan fondata il codice XML per il sito Web gli sviluppatori di ASP.NET ([www.XMLforASP.NET](http://www.XMLforASP.NET)), è l'ufficio INETA e pronuncia conferenze diversi. Dan co-autore Professional Windows DNA (Wrox), ASP.NET: i suggerimenti, esercitazioni e codice (SAM), ASP.NET 1.1 Insider soluzioni, Professional ASP.NET 2.0 AJAX (Wrox), modifiche alla MVP di ASP.NET 2.0 e XML creati per gli sviluppatori di ASP.NET (SAM). Quando ha non scrive codice, gli articoli o ai libri, Dan gode di scrittura e la registrazione di musica e la riproduzione di golf e basket con la moglie e bambini.

Categoria Scott lavora con tecnologie Web di Microsoft dal 1997 ed è il vicepresidente myKB.com ([www.myKB.com](http://www.myKB.com)) in cui si è specializzato nella scrittura ASP.NET basato su applicazioni con stato attivo sulle soluzioni Software Knowledge Base. Scott possano essere contattati tramite posta elettronica al [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) o il suo blog all'indirizzo [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Precedente](understanding-asp-net-ajax-localization.md)
> [Successivo](understanding-asp-net-ajax-debugging-capabilities.md)
