---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: "Informazioni sulle funzionalità di debug di ASP.NET AJAX | Documenti Microsoft"
author: scottcate
description: "La possibilità di eseguire il debug di codice è una competenza che ogni sviluppatore deve avere nella loro arsenal indipendentemente dalla tecnologia in uso. Anche se molti sviluppatori sono..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: 426d0182978faf7fc7516203fcc84ef0152790ba
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="understanding-aspnet-ajax-debugging-capabilities"></a>Informazioni sulle funzionalità di debug di ASP.NET AJAX
====================
da [Scott Care](https://github.com/scottcate)

[Scarica il PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> La possibilità di eseguire il debug di codice è una competenza che ogni sviluppatore deve avere nella loro arsenal indipendentemente dalla tecnologia in uso. Anche se molti sviluppatori sono abituati a utilizzare Visual Studio .NET o Web Developer Express per eseguire il debug di applicazioni ASP.NET che utilizzano codice VB.NET o c#, alcune non sono a conoscenza che è inoltre estremamente utile per il debug del codice sul lato client, ad esempio JavaScript. Lo stesso tipo di tecniche utilizzate per eseguire il debug di applicazioni .NET possa essere applicato anche agli applicazioni con supporto AJAX e in particolare le applicazioni ASP.NET AJAX.


## <a name="debugging-aspnet-ajax-applications"></a>Debug di ASP.NET AJAX applicazioni

Dan Wahlin

La possibilità di eseguire il debug di codice è una competenza che ogni sviluppatore deve avere nella loro arsenal indipendentemente dalla tecnologia in uso. È ovvio che comprendere le diverse opzioni di debug disponibili risparmiare una notevole quantità di tempo in un progetto e forse anche alcuni problemi. Anche se molti sviluppatori sono abituati a utilizzare Visual Studio .NET o Web Developer Express per eseguire il debug di applicazioni ASP.NET che utilizzano codice VB.NET o c#, alcune non sono a conoscenza che è inoltre estremamente utile per il debug del codice sul lato client, ad esempio JavaScript. Lo stesso tipo di tecniche utilizzate per eseguire il debug di applicazioni .NET possa essere applicato anche agli applicazioni con supporto AJAX e in particolare le applicazioni ASP.NET AJAX.

In questo articolo verrà visualizzato come Visual Studio 2008 e numerosi altri strumenti sono utilizzabile per eseguire il debug delle applicazioni ASP.NET AJAX per individuare rapidamente i bug e altri problemi. Questo argomento include informazioni sull'abilitazione di Internet Explorer 6 o versione successiva per il debug, l'utilizzo di Visual Studio 2008 ed Esplora Script per avanzare nel codice, nonché utilizzando altri strumenti, ad esempio il supporto di sviluppo Web gratuiti. Inoltre, si apprenderà come eseguire il debug di applicazioni ASP.NET AJAX in Firefox utilizzando che un'estensione denominata Firebug che consente avanzare nel codice JavaScript direttamente nel browser senza altri strumenti. Infine, verrà presentata alle classi della libreria AJAX di ASP.NET che consente di varie attività di debug, ad esempio funzionalità di traccia e le istruzioni di asserzione di codice.

Prima di provare a eseguire il debug delle pagine visualizzate in Internet Explorer sono disponibili alcuni passaggi di base, che è necessario eseguire per abilitare la funzionalità per il debug. Esaminiamo ora alcuni requisiti di installazione di base che devono essere eseguite per iniziare.

## <a name="configuring-internet-explorer-for-debugging"></a>Configurazione di Internet Explorer per il debug

La maggior parte degli utenti non sono interessati a visualizzare i problemi di JavaScript in un sito Web visualizzato in Internet Explorer. In realtà, l'utente medio anche direbbe che cosa fare se vedevano un messaggio di errore. Di conseguenza, le opzioni di debug sono disattivate per impostazione predefinita nel browser. Tuttavia, è molto semplice attivare il debug e inserirlo da utilizzare durante lo sviluppo di nuove applicazioni AJAX.

Per abilitare la funzionalità di debug, passare a Strumenti Opzioni Internet dal menu di Internet Explorer e selezionare la scheda Avanzate. All'interno della sezione di esplorazione assicurarsi che siano deselezionati gli elementi seguenti:

- Disattiva debugging degli script (Internet Explorer)
- Disabilitare il debug (Other) di script

Sebbene non obbligatorio, se si sta tentando di eseguire il debug di un'applicazione è opportuno eventuali errori JavaScript nella pagina per essere immediatamente visibile e ovvio. È possibile imporre a tutti gli errori da visualizzare una finestra di messaggio selezionando la casella di controllo "Visualizza notifica su ogni errore di script". Mentre questa è un'ottima opzione per attivare mentre si sta sviluppando un'applicazione, può risultare fastidioso se si sta esaminano solo altri siti Web poiché il possibilità che si verificano errori JavaScript sono buone.

Figura 1 mostra quali di Internet Explorer avanzate della finestra dovrebbe essere dopo che è stata configurata correttamente per il debug.


[![Configurazione di Internet Explorer per il debug.](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**Figura 1**: configurazione di Internet Explorer per il debug.  ([Fare clic per visualizzare l'immagine ingrandita](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))


Dopo il debug è stato attivato, si noterà una nuova voce di menu visualizzati nel menu Vista denominato Script Debugger. Sono disponibili due opzioni disponibili tra cui l'interruzione e aprire alla successiva istruzione. Quando si seleziona Apri verrà richiesto per eseguire il debug la pagina in Visual Studio 2008 (Nota Visual Web Developer Express può anche essere utilizzato per il debug). Se è in esecuzione Visual Studio .NET è possibile scegliere di utilizzare tale istanza o per creare una nuova istanza. Quando si seleziona l'interruzione all'istruzione successiva verrà richiesto per eseguire il debug della pagina quando viene eseguito il codice JavaScript. Se il codice JavaScript eseguito nell'evento onLoad della pagina è possibile aggiornare la pagina per avviare una sessione di debug. Se il codice JavaScript viene eseguito dopo un pulsante, il debugger verrà eseguito immediatamente dopo che viene scelto il pulsante.

> *>[!NOTE] se si eseguono in Windows Vista con accesso controllo utente (UAC) abilitata, e si dispone di Visual Studio 2008 impostato per l'esecuzione come amministratore, Visual Studio non riuscirà a connettersi al processo quando viene richiesto di allegare. Per risolvere questo problema, prima di avviare Visual Studio e utilizzare tale istanza per il debug.*


Anche se la sezione successiva verrà illustrato come eseguire il debug di una pagina ASP.NET AJAX direttamente all'interno di Visual Studio 2008, utilizzando l'opzione Debugger di Script di Internet Explorer è utile quando una pagina è già aperta e si desidera più completamente esaminarlo.

## <a name="debugging-with-visual-studio-2008"></a>Debug con Visual Studio 2008

Visual Studio 2008 fornisce funzionalità di debug che gli sviluppatori di tutto il mondo si basano su tutti i giorni per il debug delle applicazioni .NET. Il debugger incorporato consente di avanzare nel codice, visualizzazione di dati dell'oggetto, espressioni di controllo per variabili specifiche, monitorare lo stack di chiamate e molto altro ancora. Oltre a debug del codice c# o VB.NET, il debugger è inoltre utile per il debug di applicazioni ASP.NET AJAX e consente di avanzare nel codice JavaScript riga per riga. I dettagli che seguono lo stato attivo sulle tecniche che possono essere usate per eseguire il debug di file di script sul lato client, anziché fornire una discorso sul processo di debug di applicazioni utilizzando Visual Studio 2008.

Il processo di debug di una pagina in Visual Studio 2008 può essere avviato in diversi modi. In primo luogo, è possibile utilizzare l'opzione di Internet Explorer Script Debugger riportato nella sezione precedente. Questo funziona bene quando è già caricata una pagina nel browser e si desidera avviare il debug. In alternativa, è possibile fare clic su una pagina aspx in Esplora soluzioni e selezionare il menu Imposta come pagina iniziale. Se si è abituati a debug le pagine ASP.NET quindi probabilmente effettuata in precedenza. Quando si preme F5 può eseguire il debug della pagina. Tuttavia, mentre è in genere possibile impostare un punto di interruzione in qualsiasi punto desiderato nel codice VB.NET o c#, che non è sempre nel caso di JavaScript come si vedrà accanto.

*Incorporate e gli script esterni*

Il debugger di Visual Studio 2008 considera JavaScript incorporato in una pagina diversa rispetto ai file JavaScript esterni. I file di script esterni, è possibile aprire il file e impostare un punto di interruzione in qualsiasi riga desiderato. Facendo clic nell'area barra grigia a sinistra della finestra dell'editor di codice, è possibile impostare punti di interruzione. Quando JavaScript è incorporata direttamente in una pagina con il `<script>` tag, l'impostazione di un punto di interruzione, fare clic sulla barra grigia non è un'opzione. Tenta di impostare un punto di interruzione su una riga di script incorporati comporterà un avviso che indica "This non is un percorso valido per un punto di interruzione".

È possibile risolvere questo problema spostando il codice in un file js esterni e farvi riferimento utilizzando l'attributo src del &lt;script&gt; tag:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

Cosa accade se spostando il codice in un file esterno non è un'opzione o richiede interventi vale la pena? Mentre non è possibile impostare un punto di interruzione tramite l'editor, è possibile aggiungere l'istruzione debugger direttamente nel codice in cui si desidera avviare il debug. È inoltre possibile utilizzare la classe di Sys. debug disponibile nella libreria ASP.NET AJAX per forzare l'esecuzione del debug per avviare. Informazioni sulla classe Sys più avanti in questo articolo verrà illustrato.

Un esempio di utilizzo di `debugger` parola chiave viene visualizzato nel listato 1. In questo esempio forza il debugger per interrompere destra prima che venga effettuata una chiamata a una funzione di aggiornamento.

**Elenco 1. Utilizza la parola chiave debugger per forzare l'interruzione del debugger di Visual Studio .NET.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

Quando viene raggiunto l'istruzione debugger verrà richiesto di eseguire il debug la pagina utilizzando Visual Studio .NET sia possibile iniziare l'esecuzione del codice. Mentre questo è può verificarsi un problema con l'accesso ai file di script libreria ASP.NET AJAX usati nella pagina di questo punto un quadro utilizzando Visual Studio. Esplora Script della rete.

## <a name="using-visual-studio-net-windows-to-debug"></a>Utilizzo di Visual Studio .NET Windows per il Debug

Una volta che viene avviata una sessione di debug e di iniziare l'esame del codice premendo F11 predefinito, che possono verificarsi la finestra di dialogo di errore visualizzato vedere la figura 2, a meno che non tutti i file di script utilizzati nella pagina aperta e disponibile per il debug.


[![Messaggio di errore visualizzato quando codice sorgente non è disponibile per il debug.](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**Figura 2**: finestra di dialogo di errore visualizzato quando codice sorgente non è disponibile per il debug.  ([Fare clic per visualizzare l'immagine ingrandita](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))


Questa finestra di dialogo viene visualizzato perché Visual Studio .NET non sa come ottenere il codice sorgente di alcuni degli script di cui viene fatto riferimento dalla pagina. Ciò può essere limitativi inizialmente, è una soluzione semplice. Dopo aver avviato una sessione di debug e raggiungere un punto di interruzione, utilizzare la finestra Esplora Script di Windows eseguire il Debug dal menu di Visual Studio 2008 o utilizzare i tasti di scelta rapida Ctrl + Alt + N.

> *>[!NOTE] Se non è possibile visualizzare il menu di Esplora Script elencato, passare a strumenti* *Personalizza* *comandi del menu di Visual Studio .NET. Individuare la voce di Debug nella sezione categorie e farvi clic sopra per visualizzare tutte le voci di menu disponibili. Nell'elenco di comandi, scorrere Esplora Script e quindi trascinare il del Debug* *menu Windows indicato in precedenza. Questa operazione renderà la voce di menu di Esplora Script disponibili ogni volta che si esegue Visual Studio .NET.*


Esplora Script consente di visualizzare tutti gli script utilizzati in una pagina e aprirli nell'editor di codice. Una volta aperto Esplora Script, fare doppio clic sulla pagina aspx in fase di debug per aprirlo nella finestra dell'editor di codice. Eseguire la stessa operazione per tutti gli altri script mostrato in Esplora Script. Una volta tutti gli script sono aperti nella finestra del codice è possibile premere F11 (e utilizzare gli altri tasti di scelta di debug) per esaminare il codice. Figura 3 mostra un esempio di Esplora Script. Elenca il file corrente in fase di debug (Demo.aspx) nonché due script personalizzato e due script inseriti in modo dinamico la pagina da ScriptManager di AJAX ASP.NET.


[![Esplora Script consente di accedere agli script in una pagina.](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**Figura 3**. Esplora Script consente di accedere agli script in una pagina.  ([Fare clic per visualizzare l'immagine ingrandita](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))


Diversi altri windows possono essere utilizzate anche per fornire informazioni utili durante l'esecuzione di codice in una pagina. Ad esempio, utilizzare la finestra variabili locali per visualizzare i valori di variabili utilizzate nella pagina, finestra di controllo immediato per valutare variabili specifiche o le condizioni e visualizzare l'output. È inoltre possibile utilizzare la finestra di Output per visualizzare le istruzioni di traccia scritte utilizzando la funzione Sys.Debug.trace (che verrà trattata più avanti in questo articolo) o una funzione di debug. writeln di Internet Explorer.

Durante l'esecuzione di codice utilizzando il debugger è possibile spostare il mouse sulle variabili nel codice per visualizzare il valore a cui sono assegnati. Tuttavia, il debugger di script occasionalmente non verrà visualizzata alcuna operazione quando si passa il mouse su una variabile JavaScript specificata. Per visualizzare il valore, evidenziare l'istruzione o una variabile che si desidera visualizzare nella finestra dell'editor di codice e quindi passa il mouse su di esso. Sebbene questa tecnica non funziona in ogni situazione, molte volte sarà in grado di visualizzare il valore senza dover effettuare ricerche in una finestra di debug diversi, ad esempio la finestra variabili locali.

Un'esercitazione video che illustrano alcune delle funzionalità descritte di seguito può essere visualizzata in [http://www.xmlforasp.net](http://www.xmlforasp.net).

## <a name="debugging-with-web-development-helper"></a>Debug con supporto di sviluppo Web

Anche se molto in grado di supportare gli strumenti di debug (Visual Studio 2008 e Visual Web Developer Express 2008), sono disponibili opzioni aggiuntive che possono essere utilizzate anche che sono più leggero. Uno degli strumenti più recenti per essere rilasciata è l'Helper di sviluppo Web. Microsoft Nikhil Kothari (una delle chiavi architetti di ASP.NET AJAX in Microsoft) scritti questo strumento eccellente che è possibile eseguire molte attività diverse dalla semplice di debug per la visualizzazione dei messaggi di richiesta e risposta HTTP. Supporto di sviluppo Web può essere scaricato da [http://projects.nikhilk.net/Projects/WebDevHelper.aspx](http://projects.nikhilk.net/Projects/WebDevHelper.aspx).

Supporto di sviluppo Web può essere utilizzato direttamente all'interno di Internet Explorer che consente di utilizzare facilmente. Viene avviato, selezionare strumenti Web Development Helper dal menu di Internet Explorer. Lo strumento verrà aperta nella parte inferiore del browser che è utile poiché non è necessario lasciare il browser per eseguire diverse attività, ad esempio la registrazione di messaggi di richiesta e risposta HTTP. Figura 4 mostra l'aspetto Helper di sviluppo Web in azione.


[![Supporto di sviluppo Web](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**Figura 4**: supporto di sviluppo Web ([fare clic per visualizzare l'immagine ingrandita](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))


Supporto di sviluppo Web non è uno strumento da usare per eseguire il codice riga per riga come con Visual Studio 2008. Tuttavia, può essere utilizzato per visualizzare l'output di traccia, valutare le variabili in uno script o facilmente esplorare dati sono all'interno di un oggetto JSON. È anche molto utile per la visualizzazione dei dati passati da e verso una pagina ASP.NET AJAX e un server.

Una volta aperto in Internet Explorer Helper di sviluppo Web, è necessario abilitare selezionando Script Abilita debug dal menu di supporto di sviluppo Web come illustrato nella figura 4. In questo modo lo strumento per intercettare errori che si verificano durante l'esecuzione di una pagina. Consente inoltre di accedere facilmente ai messaggi di traccia di output della pagina. Per visualizzare informazioni di traccia o eseguire i comandi script per testare diverse funzioni all'interno di una pagina, selezionare Script Visualizza Script Console dal menu di supporto di sviluppo Web. Fornisce l'accesso a una finestra di comando e una semplice finestra controllo immediata.

*Visualizzazione di messaggi di traccia e dati dell'oggetto JSON*

Per eseguire i comandi di script o anche caricare o salvare gli script utilizzati per testare diverse funzioni in una pagina, è possibile utilizzare la finestra controllo immediato. La finestra di comando consente di visualizzare i messaggi di traccia o di debug scritti da una pagina che viene visualizzato. Il listato 2 viene illustrato come scrivere un messaggio di traccia con funzione di debug. writeln di Internet Explorer.

**Elenco di 2. Scrive un messaggio di traccia sul lato client utilizzando la classe Debug.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

Se la proprietà LastName contiene un valore di Doe, supporto di sviluppo Web verrà visualizzato il messaggio "il nome di persona: Doe" nella finestra di comando della console di script (supponendo che sia stato abilitato il debug). Helper di sviluppo Web aggiunge anche un oggetto di primo livello debugService nelle pagine che possono essere usate per scrivere le informazioni di traccia o visualizzare il contenuto degli oggetti JSON. Listato 3 viene illustrato un esempio di utilizzo funzione di traccia della classe debugService.

**Elenco di 3. Utilizzo classe debugService del supporto di sviluppo Web per la scrittura di un messaggio di traccia.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

Una funzionalità interessante della classe debugService è che funzionerà anche se il debug non è abilitato in Internet Explorer, semplificando così accedere sempre ai dati di traccia quando il supporto di sviluppo Web è in esecuzione. Quando lo strumento non viene utilizzato per eseguire il debug di una pagina, le istruzioni di traccia verranno ignorate poiché la chiamata a window.debugService restituirà false.

La classe debugService consente inoltre di dati dell'oggetto JSON può essere visualizzata utilizzando la finestra di controllo del supporto di sviluppo Web. Listato 4 consente di creare un semplice oggetto JSON contenente i dati utente. Una volta creato l'oggetto, viene eseguita una chiamata per il debugService della classe controllare funzione per consentire all'oggetto JSON da analizzare in modo visivo.

**Listato 4. Utilizzo della funzione debugService.inspect per visualizzare i dati dell'oggetto JSON.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

Chiamare la funzione GetPerson() della pagina o tramite la finestra controllo immediata comporterà la finestra di dialogo di controllo di oggetti visualizzati come mostrato nella figura 5. È possibile modificare in modo dinamico proprietà all'interno dell'oggetto vengono evidenziati, modificando il valore visualizzato nella casella di testo valore e quindi facendo clic sul collegamento di aggiornamento. Utilizzando il controllo oggetto rende più semplice visualizzare i dati dell'oggetto JSON e provare l'applicazione di valori diversi per le proprietà.

*Errori di debug*

Oltre a consentire di dati di traccia e gli oggetti JSON da visualizzare, supporto di sviluppo Web può anche facilitare il debug di errori in una pagina. Se viene rilevato un errore, verrà richiesto di continuare alla riga successiva del codice o eseguire il debug di script (vedere Figura 6). L'errore di Script finestra di dialogo nella finestra vengono visualizzati dello stack di chiamate completo nonché numeri di riga in modo sia possibile identificare facilmente in cui i problemi sono all'interno di uno script.


[![Utilizza la finestra di controllo di oggetto per visualizzare un oggetto JSON.](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**Figura 5**: utilizzando la finestra di controllo di oggetto per visualizzare un oggetto JSON.  ([Fare clic per visualizzare l'immagine ingrandita](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))


Se si seleziona l'opzione di debug consente di eseguire istruzioni di script direttamente nella finestra di controllo immediato del supporto di sviluppo Web per visualizzare il valore di variabili, scrivere oggetti JSON, oltre a informazioni. Se la stessa azione che ha generato l'errore viene eseguita nuovamente e Visual Studio 2008 è disponibile nel computer, verrà richiesto di avviare una sessione di debug in modo che è possibile eseguire il codice riga per riga come descritto nella sezione precedente.


[![Finestra di dialogo Errore dello Script del supporto di sviluppo Web](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**Figura 6**: messaggio di errore di Script del supporto di sviluppo Web ([fare clic per visualizzare l'immagine ingrandita](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))


*Esaminare i messaggi di richiesta e risposta*

Durante il debug di pagine ASP.NET AJAX è spesso utile visualizzare i messaggi di richiesta e risposta inviati tra una pagina e il server. La visualizzazione del contenuto all'interno di messaggi consente di vedere se vengono passati i dati appropriati e la dimensione dei messaggi. Helper di sviluppo Web offre un'eccellente funzionalità di logger di messaggi HTTP che rende più semplice visualizzare i dati come testo non elaborato o in un formato più leggibile.

Per visualizzare i messaggi di richiesta e risposta di ASP.NET AJAX, è necessario abilitare il logger HTTP, selezionare registrazione HTTP abilitare HTTP dal menu di supporto di sviluppo Web. Una volta abilitata, tutti i messaggi inviati dalla pagina corrente possono essere visualizzati nel Visualizzatore di log HTTP accessibili selezionando registri HTTP Mostra HTTP.

Sebbene sicuramente utile visualizzare il testo non elaborato inviato in ogni messaggio di richiesta/risposta e un'opzione nel supporto di sviluppo Web, è spesso più semplice visualizzare i dati del messaggio in formato grafico più. Dopo la registrazione HTTP è stata abilitata e sono stati registrati messaggi, i dati di messaggi possono essere visualizzati facendo doppio clic sul messaggio in Visualizzatore di log HTTP. Questa operazione consente di visualizzare tutte le intestazioni associate a un messaggio, nonché del messaggio effettivo contenuto. Figura 7 illustra un esempio di un messaggio di richiesta e un messaggio di risposta visualizzato nella finestra del Visualizzatore di Log HTTP.


[![Utilizzo del Visualizzatore di Log HTTP per visualizzare i dati dei messaggi di richiesta e risposta.](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**Figura 7**: utilizzo del Visualizzatore di Log HTTP per visualizzare i dati dei messaggi di richiesta e risposta.  ([Fare clic per visualizzare l'immagine ingrandita](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))


Il Visualizzatore di Log HTTP automaticamente analizza oggetti JSON e li visualizza utilizzando una visualizzazione albero rende rapida e semplice visualizzare i dati di proprietà dell'oggetto. Quando viene usato un UpdatePanel in una pagina ASP.NET AJAX, il Visualizzatore interrompe ogni parte del messaggio in singole parti, come illustrato nella figura 8. Si tratta di un'ottima funzionalità che rende molto più semplice visualizzare e comprendere ciò che è contenuto il messaggio rispetto alla visualizzazione dei dati del messaggio non elaborato.


[![Un messaggio di risposta UpdatePanel visualizzato nel Visualizzatore di Log HTTP.](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**Figura 8**: messaggio di risposta di un UpdatePanel visualizzati nel Visualizzatore di Log HTTP.  ([Fare clic per visualizzare l'immagine ingrandita](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))


Sono disponibili diversi altri strumenti che possono essere usati per visualizzare i messaggi di richiesta e risposta oltre a supporto di sviluppo Web. Un'altra opzione valida è Fiddler disponibile gratuitamente presso [http://www.fiddlertool.com](http://www.fiddlertool.com). Sebbene Fiddler non sarà presentata in questo caso, è anche un'ottima scelta quando è necessario esaminare attentamente i dati e le intestazioni del messaggio.

## <a name="debugging-with-firefox-and-firebug"></a>Debug con Firefox e Firebug

Quando Internet Explorer è ancora il browser più diffuso, altri browser, ad esempio Firefox sono diventate molto diffuso e vengono utilizzati più. Di conseguenza, sarà possibile visualizzare ed eseguire il debug delle pagine ASP.NET AJAX in Firefox, nonché Internet Explorer consente di verificare che le applicazioni funzionino correttamente. Anche se per il debug, Firefox non è possibile associare direttamente in Visual Studio 2008, l'estensione chiamato Firebug che può essere utilizzato per il debug delle pagine. Può essere scaricato gratuitamente Firebug passando a [http://www.getfirebug.com](http://www.getfirebug.com).

Firebug fornisce un ambiente di debug completa che può essere utilizzato per esaminare il codice riga per riga, tutti gli script utilizzati all'interno di una pagina di accesso, visualizzare strutture di DOM, visualizzare gli stili CSS e anche gli eventi di traccia che si verificano in una pagina. Una volta installato, Firebug accessibili selezionando Strumenti Firebug aprire Firebug dal menu di Firefox. Come supporto di sviluppo Web, Firebug viene utilizzato direttamente nel browser, ma può essere usato come applicazione autonoma.

Una volta Firebug è in esecuzione, è possibile impostare punti di interruzione in qualsiasi riga di un file JavaScript se lo script incorporato in una pagina o non. Per impostare un punto di interruzione, è necessario caricare prima pagina appropriata per eseguire il debug in Firefox. Una volta che viene caricata la pagina, selezionare lo script per eseguire il debug dall'elenco a discesa script del Firebug. Verranno visualizzati tutti gli script utilizzati dalla pagina. Un punto di interruzione è impostato, fare clic nell'area di nella riga del Firebug barra grigia in cui il punto di interruzione deve andare deve come si farebbe in Visual Studio 2008.

Dopo aver impostato un punto di interruzione in Firebug è possibile eseguire l'azione necessaria per eseguire lo script che è necessario eseguire il debug, ad esempio un pulsante o aggiornamento del browser per attivare l'evento onLoad. Esecuzione verrà arrestati automaticamente sulla riga contenente il punto di interruzione. Figura 9 è illustrato un esempio di un punto di interruzione che è stata attivata in Firebug.


[![Gestione punti di interruzione apportare modifiche.](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**Figura 9**: gestisce i punti di interruzione Firebug.  ([Fare clic per visualizzare l'immagine ingrandita](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))


Una volta raggiunto un punto di interruzione è possibile eseguire l'istruzione, istruzione/routine o uscire da codice utilizzando i pulsanti freccia. Durante l'esecuzione di codice, le variabili dello script vengono visualizzate nella parte destra del debugger che consente di visualizzare i valori e drill-down in oggetti. Firebug include inoltre un elenco di riepilogo a discesa di Stack di chiamate per visualizzare i passaggi per l'esecuzione dello script che hanno provocato alla riga corrente in corso il debug.

Firebug include anche una finestra della console che può essere utilizzata per verificare le istruzioni di script diverso, valutare le variabili e visualizzare l'output di traccia. È possibile accedervi facendo clic sulla scheda nella parte superiore della finestra Firebug della Console. La pagina in fase di debug può anche essere "controllata" per visualizzare la struttura DOM e il contenuto, fare clic sulla scheda controlla. Come passaggio del mouse in diversi elementi DOM visualizzato nella finestra di controllo la parte appropriata della pagina verrà evidenziata semplificando vedere dove l'elemento viene usato nella pagina. I valori di attributo associati a un determinato elemento possono essere modificati "live" per provare l'applicazione a un elemento di larghezze diverse, stili e così via. Si tratta di una funzionalità interessante che evita di dover passare costantemente editor del codice sorgente e il browser Firefox per visualizzare l'effetto delle modifiche semplice come una pagina.

Figura 10 è illustrato un esempio di utilizzo del controllo del DOM per individuare una casella di testo denominato txtCountry nella pagina. Il controllo Firebug consente anche di visualizzare gli stili CSS utilizzati in una pagina, nonché eventi che si verificano ad esempio il rilevamento di movimenti del mouse, pulsanti, oltre a informazioni.


[![Utilizzo di ispezione DOM del Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**Figura 10**: controllo DOM del Firebug utilizzando.  ([Fare clic per visualizzare l'immagine ingrandita](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))


Firebug fornisce un modo più semplice eseguire rapidamente il debug di una pagina direttamente in Firefox, nonché uno strumento eccellente per esaminare i diversi elementi all'interno della pagina.

## <a name="debugging-support-in-aspnet-ajax"></a>Supporto per il debug in ASP.NET AJAX

La libreria ASP.NET AJAX include molte classi diverse che possono essere usate per semplificare il processo di aggiunta di funzionalità AJAX in una pagina Web. È possibile utilizzare queste classi per individuare gli elementi all'interno di una pagina e modificarli, aggiungere nuovi controlli, chiamare i servizi Web e anche la gestione degli eventi. Libreria AJAX ASP.NET contiene anche le classi che possono essere utilizzate per migliorare il processo di debug di pagine. In questa sezione si verrà presentata la classe Sys e vedere come può essere usato nelle applicazioni.

*Utilizzo della classe Sys*

La classe Sys (una classe di JavaScript si trova nello spazio dei nomi Sys) può essere utilizzata per eseguire numerose funzioni diverse, tra cui la scrittura dell'output di traccia, l'esecuzione di asserzioni di codice e l'utilizzo forzato del codice in modo che sia possibile eseguire il debug. È usato per eseguire il test condizionale (, ampiamente nei file di debug della libreria ASP.NET AJAX (installati per impostazione predefinita in C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0) chiamato asserzioni) che verificare i parametri vengono passati in modo corretto per le funzioni, oggetti che contengono i dati previsti e per scrivere istruzioni di traccia.

La classe Sys espone numerose funzioni diverse che possono essere utilizzate per gestire la traccia, le asserzioni di codice o errori, come illustrato nella tabella 1.

**Tabella 1. Funzioni di classi di Sys.**

| **Nome funzione** | **Descrizione** |
| --- | --- |
| Assert (condizione, messaggio, displayCaller) | Indica che il parametro di condizione è true. Se la condizione testata è false, una finestra di messaggio da utilizzare per visualizzare il valore di parametro del messaggio. Se il parametro displayCaller è true, il metodo visualizza anche informazioni sul chiamante. |
| clearTrace() | Cancella l'output di istruzioni di operazioni di analisi. |
| Fail(Message) | Fa sì che il programma arrestare l'esecuzione e interrompere il debugger. Il parametro del messaggio consente di specificare un motivo dell'errore. |
| Trace(Message) | Scrive il parametro del messaggio di output di traccia. |
| traceDump (oggetto, nome) | Genera dati di un oggetto in un formato leggibile. Per fornire un'etichetta per il dump di traccia, è possibile utilizzare il parametro name. Tutti gli oggetti secondari all'interno dell'oggetto dumping verranno scritto per impostazione predefinita. |

Traccia sul lato client può essere utilizzata in modo analogo a come la funzionalità di traccia disponibile in ASP.NET. Consente di messaggi diversi a essere visualizzate facilmente senza interrompere il flusso dell'applicazione. Nel listato 5 viene mostrato un esempio di utilizzo della funzione Sys.Debug.trace per scrivere nel log di traccia. Questa funzione accetta semplicemente il messaggio che deve essere scritto come parametro.

**Elenco di 5. Utilizzo della funzione Sys.Debug.trace.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

Se si esegue il codice mostrato nel listato 5 che non verrà visualizzato alcun output di traccia nella pagina. L'unico modo per visualizzarlo consiste nell'utilizzare una finestra della console disponibile in Visual Studio .NET, il supporto di sviluppo Web o Firebug. Se si desidera visualizzare l'output di traccia nella pagina è necessario aggiungere un tag TextArea e assegnarle un id di TraceConsole come indicato di seguito:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

Tutte le istruzioni Sys.Debug.trace nella pagina verranno scritto il TraceConsole TextArea.

Nei casi in cui si desidera visualizzare i dati contenuti all'interno di un oggetto JSON è possibile utilizzare una funzione traceDump della classe Sys. Questa funzione accetta due parametri, tra cui l'oggetto che deve essere eseguito nella console di traccia e un nome che può essere utilizzato per identificare l'oggetto nell'output di traccia. Elenco 6 viene illustrato un esempio di utilizzo della funzione traceDump.

**Elenco di 6. Utilizzo della funzione Sys.Debug.traceDump.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

Figura 11 mostra l'output dalla chiamata alla funzione Sys.Debug.traceDump. Si noti che oltre a scrivere i dati dell'oggetto utente, viene scritto anche i dati di indirizzo sub-dell'oggetto.

Oltre alle funzionalità di traccia, la classe Sys anche utilizzabile per eseguire codice asserzioni. Le asserzioni vengono usate per eseguire il test che vengono soddisfatte specifiche condizioni durante l'esecuzione di un'applicazione. La versione di debug degli script libreria ASP.NET AJAX contengono diverse assert (istruzioni) per testare una varietà di condizioni.

Elenco 7 mostra un esempio di utilizzo della funzione Sys.Debug.assert per testare una condizione. Il codice verifica se l'oggetto indirizzo è null prima di aggiornare un oggetto Person.


[![Output della funzione Sys.Debug.traceDump.](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**Figura 11**: Output della funzione Sys.Debug.traceDump.  ([Fare clic per visualizzare l'immagine ingrandita](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))


**Elenco di 7. Utilizzo della funzione di debug. Assert.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

Tre parametri vengono passati tra cui la condizione da valutare, il messaggio da visualizzare se l'asserzione restituisce false e o meno informazioni sul chiamante devono essere visualizzate. Nei casi in cui un'asserzione ha esito negativo, verrà visualizzato il messaggio nonché informazioni relative al chiamante se il terzo parametro è true. Figura 12 illustra un esempio della finestra di dialogo di errore che viene visualizzato se l'asserzione nel listato 7 non riesce.

La funzione finale per coprire è Sys.Debug.fail. Quando si desidera forzare il codice in una determinata riga in uno script è possibile aggiungere una chiamata Sys.Debug.fail anziché l'istruzione debugger in genere utilizzata nelle applicazioni JavaScript. La funzione Sys.Debug.fail accetta un singolo parametro stringa che rappresenta il motivo dell'errore, come indicato di seguito:


[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]


[![Un messaggio di errore Sys.Debug.assert.](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**Figura 12**: Sys.Debug.assert un messaggio di errore.  ([Fare clic per visualizzare l'immagine ingrandita](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))


Quando viene rilevata un'istruzione Sys.Debug.fail durante l'esecuzione di uno script, il valore del parametro dei messaggi verrà visualizzato nella console di un'applicazione di debug, ad esempio Visual Studio 2008 e verrà richiesto il debug dell'applicazione. Un caso in cui può essere utile è quando è Impossibile impostare un punto di interruzione con Visual Studio 2008 per uno script inline, ma desidera che il codice di arresto in particolare riga affinché sia possibile esaminare il valore di variabili.

*Informazioni sulla proprietà del controllo ScriptManager ScriptMode*

La libreria ASP.NET AJAX include di debug e rilascio delle versioni di script che vengono installate in C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 per impostazione predefinita. Gli script di debug sono correttamente formattata, facili da leggere e avranno diverse chiamate Sys.Debug.assert sparsi in essi mentre gli script di rilascio degli spazi vuoti vengono rimossi e utilizzano la classe Sys con cautela per ridurre al minimo le dimensioni complessive.

Il controllo ScriptManager aggiunto alle pagine ASP.NET AJAX legge l'attributo di debug dell'elemento di compilazione in Web. config per determinare le versioni di script di libreria da caricare. Tuttavia, è possibile controllare se il debug o release gli script vengono caricati (gli script della libreria o script personalizzati) modificando la proprietà ScriptMode. ScriptMode accetta un'enumerazione ScriptMode i cui membri includono eredita, Debug, rilascio e automaticamente.

ScriptMode assume un valore di Auto, il che significa che lo ScriptManager controllerà l'attributo di debug nel file Web. config. Quando è false debug ScriptManager caricherà la versione degli script della libreria ASP.NET AJAX. La versione di debug degli script verrà caricata quando debug è true. La modifica della proprietà ScriptMode per rilasciare o eseguire il Debug forzerà ScriptManager di caricare script appropriati indipendentemente dal valore dell'attributo di debug è in Web. config. Nel listato 8 viene illustrato un esempio dell'utilizzo del controllo ScriptManager di caricare script di debug della libreria ASP.NET AJAX.

**Elenco di 8. Il caricamento degli script di debug con lo ScriptManager**.


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

È anche possibile caricare versioni diverse (debug o release) di script personalizzati tramite proprietà, gli script di ScriptManager con il componente ScriptReference, come illustrato nel listato 9.

**Elenco di 9. Durante il caricamento gli script personalizzati utilizzando lo ScriptManager.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> Se si sta caricando gli script personalizzati utilizzando il componente ScriptReference è necessario notificare lo ScriptManager quando lo script ha terminato il caricamento mediante l'aggiunta di codice seguente nella parte inferiore dello script:


[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

Il codice listato 9 indica lo ScriptManager per la ricerca per una versione di debug dello script di persona, pertanto verrà automaticamente eseguita una ricerca Person.debug.js anziché Person.js. Se il file Person.debug.js non viene trovato, che verrà generato un errore.

Nei casi in cui si desidera un debug o versione finale di uno script personalizzato da caricare in base al valore della proprietà ScriptMode impostato nel controllo ScriptManager, è possibile impostare proprietà ScriptMode del controllo ScriptReference su eredita. In questo modo la versione corretta dello script personalizzato da caricare in base alla proprietà di ScriptMode di ScriptManager come illustrato nell'elenco di 10. Perché la proprietà ScriptMode del controllo ScriptManager è impostata su Debug, lo script Person.debug.js verrà caricato e utilizzato nella pagina.

**Elenco di 10. Eredita il ScriptMode da ScriptManager per gli script personalizzati.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

Utilizzando in modo appropriato la proprietà ScriptMode consente più facilmente il debug di applicazioni e semplificare il processo generale. Gli script di versione della libreria ASP.NET AJAX sono difficili da eseguire e leggere poiché la formattazione del codice è stata rimossa durante il debug script vengono formattati in modo specifico per scopi di debug.

## <a name="conclusion"></a>Conclusione

Tecnologia Microsoft per ASP.NET AJAX fornisce una base solida per la creazione di applicazioni compatibili con AJAX che possono migliorare l'esperienza complessiva dell'utente finale. Tuttavia, come con qualsiasi tecnologia di programmazione, bug e altri problemi di applicazioni certamente si verificheranno. Conoscere le diverse opzioni di debug disponibili di risparmiare tempo e risultati in un prodotto più stabile.

In questo articolo è stato introdotto il concetto di diverse tecniche per il debug di pagine ASP.NET AJAX, incluso Internet Explorer con Visual Studio 2008, supporto di sviluppo Web e Firebug. Questi strumenti è possono semplificare il processo generale di debug poiché è possibile accedere ai dati della variabile, esaminare il codice riga per riga e visualizzare le istruzioni di traccia. Oltre agli strumenti di debug diversi descritti, è stato anche illustrato come utilizzare classe Sys. debug della libreria ASP.NET AJAX in un'applicazione e come utilizzare la classe ScriptManager di caricare debug o rilascio delle versioni degli script.

## <a name="bio"></a>Biografia

Dan Wahlin (Microsoft Most Valuable Professional per ASP.NET e servizi Web XML) è un sviluppo instructor e architettura consulente .NET al Training tecniche di interfaccia ([www.interfacett.com)](http://www.interfacett.com). Dan fondata il codice XML per il sito Web gli sviluppatori di ASP.NET ([www.XMLforASP.NET](http://www.XMLforASP.NET)), è l'ufficio INETA e pronuncia conferenze diversi. Dan co-autore Professional Windows DNA (Wrox), ASP.NET: i suggerimenti, esercitazioni e codice (SAM), ASP.NET 1.1 Insider soluzioni, Professional ASP.NET 2.0 AJAX (Wrox), modifiche alla MVP di ASP.NET 2.0 e XML creati per gli sviluppatori di ASP.NET (SAM). Quando ha non scrive codice, gli articoli o ai libri, Dan gode di scrittura e la registrazione di musica e la riproduzione di golf e basket con la moglie e bambini.

Categoria Scott lavora con tecnologie Web di Microsoft dal 1997 ed è il vicepresidente myKB.com ([www.myKB.com](http://www.myKB.com)) in cui si è specializzato nella scrittura ASP.NET basato su applicazioni con stato attivo sulle soluzioni Software Knowledge Base. Scott possano essere contattati tramite posta elettronica al [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) o il suo blog all'indirizzo [ScottCate.com](http://ScottCate.com)

>[!div class="step-by-step"]
[Precedente](understanding-asp-net-ajax-web-services.md)
