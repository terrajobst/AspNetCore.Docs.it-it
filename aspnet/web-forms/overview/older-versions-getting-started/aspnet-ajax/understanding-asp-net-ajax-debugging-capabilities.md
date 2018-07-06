---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: Informazioni sulle funzionalità di debug di ASP.NET AJAX | Microsoft Docs
author: scottcate
description: La possibilità di eseguire il debug di codice è una competenza che ogni sviluppatore deve avere nella loro arsenale indipendentemente dalla tecnologia in uso. Sebbene molti sviluppatori sono...
ms.author: aspnetcontent
ms.date: 03/28/2008
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: 9d420a41f50d06541d04a1dd3cb78a2e6beaaa9a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813828"
---
<a name="understanding-aspnet-ajax-debugging-capabilities"></a>Informazioni sulle funzionalità di debug di ASP.NET AJAX
====================
da [Scott Cate](https://github.com/scottcate)

[Scaricare PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> La possibilità di eseguire il debug di codice è una competenza che ogni sviluppatore deve avere nella loro arsenale indipendentemente dalla tecnologia in uso. Sebbene molti sviluppatori sono abituati a usare Visual Studio .NET o Web Developer Express per eseguire il debug di applicazioni ASP.NET che utilizzano codice VB.NET o c#, alcune non sono consapevoli che è anche estremamente utile per il debug di codice lato client, ad esempio JavaScript. Lo stesso tipo di tecniche utilizzate per eseguire il debug di applicazioni .NET può essere applicato anche alle applicazioni basate su AJAX e in particolare le applicazioni ASP.NET AJAX.


## <a name="debugging-aspnet-ajax-applications"></a>Debug di applicazioni ASP.NET AJAX

Dan Wahlin

La possibilità di eseguire il debug di codice è una competenza che ogni sviluppatore deve avere nella loro arsenale indipendentemente dalla tecnologia in uso. È ovvio che comprendere le diverse opzioni di debug che sono disponibili possono salvare una notevole quantità di tempo in un progetto e forse anche alcuni problemi. Sebbene molti sviluppatori sono abituati a usare Visual Studio .NET o Web Developer Express per eseguire il debug di applicazioni ASP.NET che utilizzano codice VB.NET o c#, alcune non sono consapevoli che è anche estremamente utile per il debug di codice lato client, ad esempio JavaScript. Lo stesso tipo di tecniche utilizzate per eseguire il debug di applicazioni .NET può essere applicato anche alle applicazioni basate su AJAX e in particolare le applicazioni ASP.NET AJAX.

In questo articolo si vedrà come Visual Studio 2008 e diversi altri strumenti sono utilizzabile per il debug di applicazioni ASP.NET AJAX per individuare rapidamente i bug e altri problemi. Questa discussione includerà informazioni sull'abilitazione di Internet Explorer 6 o versione successiva per debug, l'utilizzo di Visual Studio 2008 ed Esplora Script per avanzare nel codice, nonché utilizzando altri strumenti gratuiti, ad esempio Web Development Helper. Scoprirai anche come eseguire il debug di applicazioni ASP.NET AJAX in Firefox usando che un'estensione denominata Firebug che consente esaminare il codice JavaScript direttamente nel browser senza altri strumenti. Infine, illustreranno le classi nella libreria ASP.NET AJAX che possa facilitare diverse attività di debug, ad esempio traccia e le istruzioni di asserzione di codice.

Prima di provare a eseguire il debug delle pagine visualizzate in Internet Explorer sono disponibili alcuni passaggi di base, che è necessario eseguire per abilitare la funzionalità per il debug. Esaminiamo ora alcuni requisiti di configurazione di base che devono essere eseguite per iniziare.

## <a name="configuring-internet-explorer-for-debugging"></a>Configurazione di Internet Explorer per eseguire il debug

La maggior parte delle persone non sono interessate a visualizzare i problemi di JavaScript in un sito Web visualizzate con Internet Explorer. In effetti, l'utente medio non sarebbe anche sapere cosa fare se è stato illustrato un messaggio di errore. Di conseguenza, le opzioni di debug sono state disabilitate per impostazione predefinita nel browser. Tuttavia, è molto semplice attivare il debug e usarle durante lo sviluppo di nuove applicazioni AJAX.

Per abilitare la funzionalità di debug, passare a Strumenti Opzioni Internet dal menu di Internet Explorer e selezionare la scheda Avanzate. All'interno della sezione esplorazione assicurarsi che gli elementi seguenti sono deselezionati:

- Disabilita debugging degli script (Internet Explorer)
- Disabilitare il debug (altro) degli script

Sebbene non obbligatorio, se si sta provando a eseguire il debug di un'applicazione che probabilmente vorrai eventuali errori JavaScript nella pagina per essere immediatamente visibile e ovvio. È possibile forzare tutti gli errori da visualizzare una finestra di messaggio selezionando la casella di controllo "Visualizza una notifica su ogni errore di script". Si tratta di un'ottima opzione per attivare anche se si sta sviluppando un'applicazione, rapidamente può diventare fastidiosa se si sta semplicemente esaminano altri siti Web poiché le probabilità di incontrare errori JavaScript ci sono buone.

Figura 1 mostra quali di Internet Explorer avanzata della finestra dovrebbe essere dopo che è stato configurato correttamente per il debug.


[![Configurazione di Internet Explorer per eseguire il debug.](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**Figura 1**: configurazione di Internet Explorer per eseguire il debug.  ([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))


Al termine di debug è stato attivato, si noterà una nuova voce di menu visualizzato nel menu di visualizzazione denominato Debugger di Script. Sono disponibili due opzioni disponibili tra cui Open e interruzione all'istruzione successiva. Quando viene selezionato Open verrà richiesto per eseguire il debug la pagina in Visual Studio 2008 (si noti che Visual Web Developer Express consente inoltre di debug). Se è in esecuzione Visual Studio .NET è possibile scegliere di utilizzare tale istanza o per creare una nuova istanza. Quando viene selezionato l'interruzione all'istruzione successiva verrà richiesto per eseguire il debug della pagina quando viene eseguito il codice JavaScript. Se il codice JavaScript eseguito nell'evento onLoad della pagina è possibile aggiornare la pagina per attivare una sessione di debug. Se il codice JavaScript viene eseguito dopo che un pulsante, il debugger verrà eseguito immediatamente dopo aver scelto il pulsante.

> *> [!NOTE] Se si esegue in Windows Vista con accesso controllo utente (UAC) abilitato e si dispone di Visual Studio 2008, impostato per l'esecuzione come amministratore, Visual Studio non riuscirà a connettersi al processo quando viene richiesto di associare. Per risolvere questo problema, avviare Visual Studio prima di tutto e usare tale istanza per il debug.*


Anche se la sezione successiva illustrerà come eseguire il debug di una pagina ASP.NET AJAX direttamente da all'interno di Visual Studio 2008, usando l'opzione del Debugger di Script di Internet Explorer è utile quando una pagina è già aperta e si vuole controllarlo più completo.

## <a name="debugging-with-visual-studio-2008"></a>Debug con Visual Studio 2008

Visual Studio 2008 fornisce funzionalità di debug che gli sviluppatori in tutto il mondo si basano su tutti i giorni per il debug delle applicazioni .NET. Il debugger predefinito consente di spostarsi nel codice, visualizzazione dati dell'oggetto, espressione di controllo per variabili specifiche, monitorare lo stack di chiamate e molto altro ancora. Oltre a debug del codice c# o VB.NET, il debugger è inoltre utile per il debug di applicazioni ASP.NET AJAX e consentirà di avanzare nel codice JavaScript di una riga. I dettagli che seguono lo stato attivo su tecniche che possono essere utilizzate per eseguire il debug di file di script lato client, anziché fornire un discorso sul processo di debug di applicazioni utilizzando Visual Studio 2008.

Il processo di debug di una pagina in Visual Studio 2008 può essere avviato in diversi modi. In primo luogo, è possibile utilizzare l'opzione Debugger di Script di Internet Explorer menzionata nella sezione precedente. Questo funziona bene quando è già caricata una pagina nel browser e si vuole iniziare a eseguire il debug. In alternativa, è possibile fare clic su una pagina aspx in Esplora soluzioni e scegliere Imposta come pagina iniziale dal menu di scelta. Se si è abituati al debug delle pagine ASP.NET quindi probabilmente termine di questa prima. Una volta che viene premuto F5 è possibile eseguire il debug della pagina. Tuttavia, mentre è in genere possibile impostare un punto di interruzione in qualsiasi punto desiderato nel codice c# o VB.NET, che non è sempre nel caso di JavaScript come si vedrà successivamente.

*Embedded e gli script esterni*

Il debugger di Visual Studio 2008 considera JavaScript incorporato in una pagina diversa rispetto ai file JavaScript esterni. Con i file di script esterni, è possibile aprire il file e impostare un punto di interruzione su una qualsiasi riga che scelto. Facendo clic nell'area di barra grigia a sinistra della finestra dell'editor di codice, è possibile impostare punti di interruzione. Quando JavaScript è incorporata direttamente in una pagina con il `<script>` tag, l'impostazione di un punto di interruzione facendo clic sulla barra grigia non è un'opzione. Tenta di impostare un punto di interruzione su una riga di script incorporati comporterà un avviso che indica "Non un percorso valido per un punto di interruzione".

È possibile aggirare questo problema spostando il codice in un file con estensione js esterno e farvi riferimento usando l'attributo src del &lt;script&gt; tag:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

Cosa accade se spostando il codice in un file esterno non è un'opzione o richiede più di lavoro, vale la pena? Anche se non è possibile impostare un punto di interruzione usando l'editor, è possibile aggiungere l'istruzione del debugger direttamente nel codice in cui si desidera avviare il debug. È anche possibile usare la classe Sys disponibile nella libreria ASP.NET AJAX per forzare l'esecuzione del debug per avviare. Informazioni sulla classe Sys più avanti in questo articolo verrà illustrato.

Un esempio d'uso di `debugger` (parola chiave) viene visualizzata nel listato 1. Questo esempio vengono portati forzatamente il debugger per interrompere a destra prima che venga effettuata una chiamata a una funzione di aggiornamento.

**Listato 1. Usando la parola chiave del debugger per forzare l'interruzione del debugger di Visual Studio .NET.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

Una volta raggiunto l'istruzione del debugger si verrà richiesto di eseguire il debug la pagina usando Visual Studio .NET e può iniziare a esaminare il codice. Mentre questo è in questo può verificarsi un problema con l'accesso ai file di script libreria ASP.NET AJAX usati quindi nella pagina di esaminare tramite Visual Studio. Esplora Script di NET.

## <a name="using-visual-studio-net-windows-to-debug"></a>Usando Visual Studio .NET Windows per eseguire il Debug

Una volta che viene avviata una sessione di debug e di iniziare l'esame del codice utilizzando il tasto F11 predefinito, è possibile riscontrare la finestra di dialogo di errore visualizzato nel vedere la figura 2, a meno che tutti i file di script utilizzati nella pagina sono aperti e disponibili per il debug.


[![Finestra di dialogo errore visualizzato quando codice sorgente non è disponibile per il debug.](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**Figura 2**: finestra di dialogo di errore visualizzato quando codice sorgente non è disponibile per il debug.  ([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))


Questa finestra di dialogo viene visualizzato perché Visual Studio .NET non sa come accedere a codice sorgente di alcuni degli script di cui viene fatto riferimento dalla pagina. Ciò può essere piuttosto frustrante inizialmente, è disponibile una semplice correzione. Dopo aver avviato una sessione di debug e raggiungere un punto di interruzione, passare alla finestra di Esplora Script di Windows eseguire il Debug dal menu di Visual Studio 2008 o usare il tasto di scelta rapida Ctrl + Alt + N.

> *> [!NOTE] Se non è possibile visualizzare il menu di Esplora Script elencato, passare a strumenti* *Personalizza* *comandi del menu di Visual Studio .NET. Individuare la voce di Debug nella sezione categorie e farvi clic per visualizzare tutte le voci di menu disponibili. Nella casella comandi, scorrere verso il basso Esplora Script e quindi trascinare il di Debug* *menu Windows indicato in precedenza. Questa operazione renderà la voce di menu di Esplora Script disponibili ogni volta che si esegue Visual Studio .NET.*


Esplora Script è utilizzabile per visualizzare tutti gli script usati in una pagina e aprirli in editor del codice. Dopo aver aperto Esplora Script, fare doppio clic nella pagina aspx in fase di debug per aprirlo nella finestra dell'editor di codice. Eseguire la stessa azione per tutti gli altri script mostrato in Esplora Script. Dopo che tutti gli script sono aperti nella finestra del codice è possibile premere F11 (e Usa gli altri tasti di scelta di debug) per esaminare il codice. Figura 3 mostra un esempio di Esplora Script. Elencato il file corrente in fase di debug (Demo.aspx), nonché due script personalizzato e due script inserito in modo dinamico nella pagina da ScriptManager di ASP.NET AJAX.


[![Esplora Script consente di accedere facilmente agli script usato in una pagina.](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**Figura 3**. Esplora Script consente di accedere facilmente agli script usato in una pagina.  ([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))


Diversi altri utenti windows possono essere utilizzate anche per fornire informazioni utili durante l'esecuzione di codice in una pagina. Ad esempio, è possibile usare la finestra variabili locali per visualizzare i valori delle diverse variabili utilizzate nella pagina finestra di controllo immediato per valutare condizioni o variabili specifiche e visualizzare l'output. È anche possibile usare la finestra di Output per visualizzare le istruzioni di traccia scritte utilizzando la funzione Sys.Debug.trace (che verrà illustrata più avanti in questo articolo) o una funzione di debug. writeln di Internet Explorer.

Mentre si procede attraverso codice che usa il debugger è possibile spostare il mouse sulle variabili con il codice per visualizzare il valore di cui sono assegnati. Tuttavia, il debugger di script in alcuni casi non verrà visualizzato alcun avviso quando si passa il mouse su una determinata variabile JavaScript. Per visualizzare il valore, evidenziare l'istruzione o la variabile che si desidera visualizzare nella finestra dell'editor di codice e quindi passa il mouse su di esso. Sebbene questa tecnica non funziona in ogni situazione, molte volte sarà in grado di visualizzare il valore senza dover esaminare una finestra di debug diverso, ad esempio la finestra variabili locali.

Esercitazione video che illustra alcune delle funzionalità descritte di seguito può essere visualizzata in [ http://www.xmlforasp.net ](http://www.xmlforasp.net).

## <a name="debugging-with-web-development-helper"></a>Debug con Web Development Helper

Anche se Visual Studio 2008 (e Visual Web Developer Express 2008) sono molto in grado di supportare gli strumenti di debug, sono disponibili opzioni aggiuntive che possono essere usate anche che sono più leggero. Uno degli strumenti più recenti di rilascio è Web Development Helper. Microsoft Nikhil Kothari (uno degli architetti di ASP.NET AJAX chiave presso Microsoft) ha scritto questo strumento eccellente che è possibile eseguire molte attività diverse dal semplice debug alla visualizzazione dei messaggi di richiesta e risposta HTTP. Può essere scaricato da Web Development Helper [ http://projects.nikhilk.net/Projects/WebDevHelper.aspx ](http://projects.nikhilk.net/Projects/WebDevHelper.aspx).

Helper di sviluppo Web può essere utilizzato direttamente all'interno di Internet Explorer che rende più semplice da utilizzare. Viene avviato selezionando Strumenti Web Development Helper dal menu di Internet Explorer. Lo strumento verrà aperta nella parte inferiore del browser che è interessante poiché non è necessario lasciare il browser per eseguire diverse attività, ad esempio la registrazione dei messaggi richiesta e risposta HTTP. Figura 4 mostra Web Development Helper aspetto in azione.


[![Web Development Helper](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**Figura 4**: Web Development Helper ([fare clic per visualizzare l'immagine con dimensioni normali](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))


Helper di sviluppo per Web non è uno strumento che verrà usato per eseguire codice riga per riga come con Visual Studio 2008. Tuttavia, può essere utilizzato per visualizzare l'output di traccia, facilmente valutare variabili in uno script o esplorare i dati sono all'interno di un oggetto JSON. È anche molto utile per la visualizzazione dati passati da e verso una pagina ASP.NET AJAX e un server.

Una volta Web Development Helper è aperto in Internet Explorer, è necessario abilitare selezionando lo Script Abilita debug dal menu Web Development helper come illustrato in precedenza nella figura 4. In questo modo lo strumento per intercettare errori che si verificano durante l'esecuzione di una pagina. Permette anche di accedere facilmente ai messaggi di traccia che vengono visualizzati nella pagina. Per visualizzare le informazioni di traccia o eseguire i comandi script per testare le diverse funzioni all'interno di una pagina, selezionare lo Script Visualizza Script Console nel menu Web Development Helper. Ciò fornisce accesso a una finestra di comando e una semplice finestra controllo immediato.

*Visualizzazione di messaggi di traccia e dati dell'oggetto JSON*

Finestra di controllo immediato è utilizzabile per eseguire i comandi di script o anche caricare o salvare gli script usati per testare le diverse funzioni in una pagina. La finestra di comando Visualizza i messaggi di traccia o debug scritti dalla pagina visualizzata. Listato 2 viene illustrato come scrivere un messaggio di traccia con funzione di debug. writeln di Internet Explorer.

**Listato 2. Scrive un messaggio di traccia sul lato client utilizzando la classe Debug.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

Se la proprietà LastName contiene un valore di Doe, Web Development Helper visualizzerà il messaggio "il nome di persona: Doe" nella finestra di comando della console dello script (presupponendo che sia stato abilitato il debug). Web Development Helper aggiunge anche un oggetto di primo livello debugService nelle pagine che possono essere utilizzate per scrivere le informazioni di traccia o visualizzare il contenuto degli oggetti JSON. Listato 3 viene illustrato un esempio d'uso traccia funzione della classe debugService.

**Listato 3. Usando classi debugService di Web Development Helper scrivere un messaggio di traccia.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

Una funzionalità interessante della classe debugService è che funzionerà anche se non è abilitato il debug di Internet Explorer, rendendo più semplice accedere sempre ai dati di traccia quando Web Development Helper è in esecuzione. Quando lo strumento non viene usato per eseguire il debug di una pagina, istruzioni di traccia verranno ignorate poiché la chiamata a window.debugService restituirà false.

La classe debugService consente inoltre a essere visualizzati tramite la finestra di controllo di Web Development Helper dati dell'oggetto JSON. Listato 4 consente di creare un semplice oggetto JSON contenente i dati di persona. Dopo aver creato l'oggetto, viene eseguita una chiamata al debugService della classe ispezionare (funzione) per consentire l'oggetto JSON da analizzare visivamente.

**Listato 4. Utilizzo della funzione debugService.inspect per visualizzare i dati dell'oggetto JSON.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

La chiamata alla funzione GetPerson() nella pagina o tramite la finestra controllo immediato comporterà la finestra di dialogo di controllo di oggetti visualizzati come illustrato nella figura 5. Proprietà all'interno dell'oggetto possono essere modificate dinamicamente evidenziandoli, modificando il valore visualizzato nella casella di testo valore e quindi facendo clic sul collegamento di aggiornamento. L'uso del controllo oggetto rende più semplice visualizzare i dati dell'oggetto JSON e sperimentare con l'applicazione di diversi valori alle proprietà.

*Debug degli errori*

Oltre a consentire i dati di traccia e gli oggetti JSON da visualizzare, Web Development helper può anche facilitare il debug degli errori in una pagina. Se viene rilevato un errore, verrà richiesto di continuare alla riga successiva del codice o il debug dello script (vedere la figura 6). La finestra gli errori di Script di finestra di dialogo Mostra a stack di chiamate completata, nonché i numeri di riga in modo che sia possibile identificare facilmente in cui i problemi sono in uno script.


[![Usando la finestra di controllo di oggetto per visualizzare un oggetto JSON.](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**Figura 5**: usando la finestra di controllo di oggetto per visualizzare un oggetto JSON.  ([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))


Se si seleziona l'opzione di debug consente di eseguire istruzioni di script direttamente nella finestra controllo immediato dell'Helper di sviluppo Web per visualizzare il valore delle variabili, scrivere oggetti JSON, oltre a informazioni. Se viene eseguita nuovamente la stessa azione che ha generato l'errore ed è disponibile nel computer Visual Studio 2008, verrà richiesto di avviare una sessione di debug in modo che è possibile eseguire il codice riga per riga come descritto nella sezione precedente.


[![Finestra di dialogo Errore dello Script del supporto di sviluppo Web](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**Figura 6**: finestra di dialogo Errore di Script di Web Development Helper ([fare clic per visualizzare l'immagine con dimensioni normali](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))


*Ispezione messaggi di richiesta e risposta*

Durante il debug delle pagine ASP.NET AJAX è spesso utile visualizzare i messaggi di richiesta e risposta inviati tra una pagina e un server. La visualizzazione del contenuto all'interno di messaggi consente di vedere se i dati appropriati viene passati, nonché le dimensioni dei messaggi. Web Development Helper offre un'eccellente funzionalità di logger di messaggio HTTP che rende più semplice visualizzare i dati come testo non elaborato o in un formato più leggibile.

Per visualizzare i messaggi di richiesta e risposta di ASP.NET AJAX, è necessario abilitare il logger HTTP, selezionare registrazione HTTP abilitare HTTP dal menu di scelta Web Development Helper. Una volta abilitata, tutti i messaggi inviati dalla pagina corrente possono essere visualizzati nel Visualizzatore di log HTTP sono accessibili selezionando i log HTTP mostrano HTTP.

Visualizzazione del testo non elaborato, inviato in ogni messaggio di richiesta/risposta è certamente molto utile e un'opzione in Web Development Helper, risulta spesso più semplice visualizzare i dati del messaggio in un formato più grafico. Dopo che è stata abilitata la registrazione HTTP e i messaggi sono stati registrati, i dati di messaggio possono essere visualizzati facendo doppio clic sul messaggio in Visualizzatore di log HTTP. In questo modo è possibile visualizzare tutte le intestazioni associate a un messaggio, nonché il messaggio effettivo contenuto. Figura 7 illustra un esempio di un messaggio di richiesta e un messaggio di risposta visualizzata nella finestra del Visualizzatore di Log HTTP.


[![Utilizzo del Visualizzatore di Log HTTP per visualizzare i dati di messaggio di richiesta e risposta.](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**Figura 7**: utilizzo del Visualizzatore di Log HTTP per visualizzare i dati di messaggio di richiesta e risposta.  ([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))


Il Visualizzatore di Log HTTP automaticamente analizza gli oggetti JSON e li visualizza mediante una visualizzazione albero fare in modo rapido e visualizzare i dati di proprietà dell'oggetto. Quando un UpdatePanel viene utilizzato in una pagina ASP.NET AJAX, il Visualizzatore suddivide ogni parte del messaggio in singole parti come illustrato nella figura 8. Si tratta di un'ottima funzionalità che rende molto più semplice visualizzare e comprendere quali sono le novità rispetto alla visualizzazione dei dati del messaggio non elaborato il messaggio.


[![Un messaggio di risposta UpdatePanel visualizzato nel Visualizzatore di Log HTTP.](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**Figura 8**: messaggio di risposta di un UpdatePanel visualizzati nel Visualizzatore di Log HTTP.  ([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))


Esistono diversi altri strumenti che possono essere utilizzati per visualizzare i messaggi di richiesta e risposta oltre Web Development Helper. Un'altra opzione valida è Fiddler, disponibile gratuitamente [ http://www.fiddlertool.com ](http://www.fiddlertool.com). Sebbene Fiddler non sarà presentata in questo caso, è anche una scelta ottimale quando è necessario esaminare attentamente i dati e le intestazioni del messaggio.

## <a name="debugging-with-firefox-and-firebug"></a>Debug con Firefox e Firebug

Anche se Internet Explorer è comunque il browser più ampiamente usato, altri browser, ad esempio Firefox sono diventati molto diffusa e vengono usate più. Di conseguenza, è opportuno visualizzare ed eseguire il debug delle pagine ASP.NET AJAX in Firefox, nonché Internet Explorer per assicurarsi che le applicazioni funzionino correttamente. Sebbene Firefox non è possibile collegare direttamente in Visual Studio 2008 per eseguire il debug, ha un'estensione denominata Firebug che può essere utilizzato per il debug delle pagine. Firebug può essere scaricato gratuitamente visitando [ http://www.getfirebug.com ](http://www.getfirebug.com).

Firebug fornisce un ambiente di debug completa che può essere utilizzato per scorrere il codice riga per riga, tutti gli script usati all'interno di una pagina di accesso, visualizzare le strutture di DOM, visualizzare gli stili CSS e anche tenere traccia di eventi che si verificano in una pagina. Una volta installato, Firebug sono accessibili selezionando Strumenti Firebug Open Firebug dal menu di Firefox. Ad esempio Web Development Helper, Firebug viene utilizzato direttamente nel browser anche se può essere utilizzato anche come un'applicazione autonoma.

Firebug è in esecuzione, è possibile impostare punti di interruzione su una qualsiasi riga di un file JavaScript se lo script è incorporato in una pagina o non. Per impostare un punto di interruzione, innanzitutto caricare la pagina appropriata per eseguire il debug in Firefox. Dopo la pagina viene caricata, selezionare lo script per eseguire il debug dall'elenco a discesa gli script del Firebug. Verranno visualizzati tutti gli script utilizzati dalla pagina. Viene impostato un punto di interruzione facendo clic nell'area di sulla riga del Firebug grigia sulla barra delle applicazioni in cui il punto di interruzione deve diventare deve come si farebbe in Visual Studio 2008.

Dopo aver impostato un punto di interruzione in Firebug è possibile eseguire l'azione necessaria per eseguire lo script che è necessario eseguire il debug, ad esempio facendo clic su un pulsante o l'aggiornamento del browser per attivare l'evento onLoad. L'esecuzione si arresterà automaticamente sulla riga contenente il punto di interruzione. Figura 9 mostra un esempio di un punto di interruzione è stata attivata nonché di apportare modifiche.


[![Gestione di punti di interruzione nonché di apportare modifiche.](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**Figura 9**: gestisce i punti di interruzione nonché di apportare modifiche.  ([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))


Una volta che viene raggiunto un punto di interruzione è possibile eseguire l'istruzione, Esegui istruzione/routine o uscire da codice usando i pulsanti freccia. Mentre si procede attraverso codice, le variabili dello script vengono visualizzate nella parte destra del debugger che consente di visualizzare i valori e il drill-down in oggetti. Firebug include inoltre un elenco di riepilogo a discesa di Stack di chiamate per visualizzare i passaggi di esecuzione dello script che hanno causato la riga corrente in fase di debug.

Firebug include anche una finestra della console che può essere usata per testare le istruzioni di script diverso, valutare le variabili e visualizzare l'output di traccia. È possibile accedervi facendo clic sulla scheda Console nella parte superiore della finestra Firebug. La pagina in fase di debug può anche essere "verificata" per visualizzare la struttura DOM e il contenuto facendo clic sulla scheda Inspect. Come si passaggio del mouse in diversi elementi DOM visualizzati nella finestra di controllo la parte corretta della pagina verrà evidenziato rendendo più semplice visualizzare in cui l'elemento viene usato nella pagina. I valori di attributo associati a un elemento specificato possono essere modificati "live" sperimentare applicando larghezze differenti, stili, e così via a un elemento. Questa è una funzionalità interessante che evita di dover costantemente passare tra l'editor del codice sorgente e il browser Firefox per visualizzare come semplici modifiche interessano una pagina.

Figura 10 è illustrato un esempio dell'uso della finestra di ispezione DOM per individuare un elemento textbox denominato txtCountry nella pagina. Il controllo Firebug è anche utilizzabile per visualizzare gli stili CSS utilizzati in una pagina, nonché gli eventi che si verificano, ad esempio rilevamento spostamenti del mouse, pulsanti, oltre a informazioni.


[![Utilizzo di ispezione DOM del Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**Figura 10**: finestra di ispezione DOM dell'usando Firebug.  ([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))


Firebug consente in modo più semplice eseguire rapidamente il debug di una pagina direttamente in Firefox, nonché uno strumento eccellente per analizzare diversi elementi all'interno della pagina.

## <a name="debugging-support-in-aspnet-ajax"></a>Supporto per il debug in ASP.NET AJAX

La libreria ASP.NET AJAX include molte classi diverse che possono essere usate per semplificare il processo di aggiunta di funzionalità AJAX in una pagina Web. È possibile utilizzare queste classi per individuare elementi all'interno di una pagina e modificarli, aggiungere nuovi controlli, chiamare i servizi Web e anche la gestione degli eventi. La libreria ASP.NET AJAX contiene inoltre classi che possono essere usate per migliorare il processo di debug di pagine. In questa sezione si illustreranno la classe Sys e vedere come può essere usato nelle applicazioni.

*Usando la classe Sys*

La classe Sys (una classe JavaScript che si trova nello spazio dei nomi Sys) utilizzabile per eseguire diverse funzioni diverse tra cui la scrittura dell'output di traccia, l'esecuzione di asserzioni di codice e forzare l'errore in modo che è possibile eseguire il debug del codice. Viene usato per eseguire test condizionali (, ampiamente nei file di debug della libreria ASP.NET AJAX (installati per impostazione predefinita in C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0) chiamato asserzioni) che assicurarsi che i parametri vengono passati in modo corretto per funzioni, gli oggetti contengano i dati previsti e per scrivere istruzioni di traccia.

La classe Sys espone diverse funzioni diverse che possono essere utilizzate per gestire la traccia, le asserzioni di codice o errori, come illustrato nella tabella 1.

**Tabella 1. Funzioni della classe Sys.**

| **Nome funzione** | **Descrizione** |
| --- | --- |
| Assert (condition, messaggio, displayCaller) | Indica che il parametro di condizione è true. Se la condizione da testare è false, una finestra di messaggio da utilizzare per visualizzare il valore di parametro del messaggio. Se il parametro displayCaller è true, il metodo visualizza anche informazioni relative al chiamante. |
| clearTrace() | Cancella l'output di istruzioni di operazioni di traccia. |
| Fail(Message) | Fa sì che il programma di arrestare l'esecuzione e interrompere il debugger. Il parametro del messaggio è utilizzabile per specificare un motivo dell'errore. |
| Trace(Message) | Scrive il parametro del messaggio nell'output di traccia. |
| traceDump (oggetto, nome) | Restituisce i dati di un oggetto in un formato leggibile. Il parametro name è utilizzabile per fornire un'etichetta per il dump di traccia. Eventuali oggetti secondari all'interno dell'oggetto viene eseguito il dump verranno scritta per impostazione predefinita. |

Traccia sul lato client può essere utilizzata in modo analogo a come le funzionalità di traccia disponibili in ASP.NET. Offre diversi tipi di messaggi essere facilmente visibile senza interrompere il flusso dell'applicazione. Listato 5 viene illustrato un esempio di utilizzo della funzione Sys.Debug.trace per scrivere nel log di traccia. Questa funzione accetta semplicemente il messaggio che deve essere scritto come un parametro.

**Listato 5. Utilizzo della funzione Sys.Debug.trace.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

Se si esegue il codice illustrato nel listato 5 che non verrà visualizzato alcun output di traccia nella pagina. L'unico modo per visualizzarlo è usare una finestra della console disponibile in Visual Studio .NET, Web Development Helper o Firebug. Se si desidera visualizzare l'output di traccia nella pagina è necessario aggiungere un tag TextArea e assegnargli un id di elemento TraceConsole come indicato di seguito:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

Qualsiasi istruzione Sys.Debug.trace nella pagina verranno scritto il TraceConsole TextArea.

Nei casi in cui si vogliono visualizzare i dati contenuti all'interno di un oggetto JSON è possibile utilizzare funzioni traceDump della classe Sys. Questa funzione accetta due parametri, incluso l'oggetto che deve essere eseguito il dump nella console di traccia e un nome che può essere utilizzato per identificare l'oggetto nell'output di traccia. Listato 6 viene illustrato un esempio di utilizzo della funzione traceDump.

**Listato 6. Utilizzo della funzione Sys.Debug.traceDump.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

Figura 11 mostra l'output dalla chiamata alla funzione Sys.Debug.traceDump. Si noti che oltre a scrivere i dati dell'oggetto Person, viene scritto anche i dati di indirizzo sub-dell'oggetto.

Oltre alla traccia, la classe Sys è anche utilizzabile per eseguire codice asserzioni. Le asserzioni vengono usate per verificare che siano soddisfatte condizioni specifiche durante l'esecuzione di un'applicazione. La versione di debug degli script libreria ASP.NET AJAX contengono diverse assert (istruzioni) per testare una varietà di condizioni.

Listato 7 viene illustrato un esempio di utilizzo della funzione Sys.Debug.assert per testare una condizione. Il codice verifica se l'oggetto indirizzo è null prima di aggiornare un oggetto Person.


[![Output della funzione Sys.Debug.traceDump.](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**Figura 11**: Output della funzione Sys.Debug.traceDump.  ([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))


**Listato 7. Utilizzo della funzione di debug. Assert.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

Tre parametri vengono passati tra cui la condizione da valutare, il messaggio da visualizzare se l'asserzione restituisce false e o meno informazioni sul chiamante devono essere visualizzate. Nei casi in cui un'asserzione non riesce, verrà visualizzato il messaggio, nonché informazioni sul chiamante se il terzo parametro è true. Figura 12 illustra un esempio della finestra di dialogo di errore visualizzato se l'asserzione mostrato nel listato 7 ha esito negativo.

La funzione finale per coprire è Sys.Debug.fail. Quando si vuole forzare il codice di errore in una determinata riga in uno script è possibile aggiungere una chiamata Sys.Debug.fail anziché l'istruzione del debugger in genere utilizzata nelle applicazioni JavaScript. La funzione Sys.Debug.fail accetta un singolo parametro stringa che rappresenta il motivo dell'errore, come indicato di seguito:


[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]


[![Un messaggio di errore Sys.Debug.assert.](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**Figura 12**: Sys.Debug.assert un messaggio di errore.  ([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))


Quando viene rilevata un'istruzione di Sys.Debug.fail durante l'esecuzione di uno script, il valore del parametro del messaggio verrà visualizzato nella console di un'applicazione di debug, ad esempio Visual Studio 2008 e verrà richiesto il debug dell'applicazione. Un caso in cui può essere piuttosto utile è quando è Impossibile impostare un punto di interruzione con Visual Studio 2008 per uno script inline, ma vuole che il codice per arrestare in particolare riga affinché sia possibile esaminare il valore delle variabili.

*Informazioni sulla proprietà del controllo ScriptManager ScriptMode*

La libreria ASP.NET AJAX include debug e rilascio delle versioni degli script che vengono installate in C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 per impostazione predefinita. Gli script di debug vengono correttamente formattata, facile da leggere e avranno diverse chiamate Sys.Debug.assert sparsi in essi mentre gli script di rilascio sono spazi vuoti vengono rimossi e usano la classe Sys con moderazione per ridurre al minimo le dimensioni complessive.

Il controllo ScriptManager aggiunto alle pagine ASP.NET AJAX legge l'attributo di debug dell'elemento di compilazione in Web. config per determinare quali versioni degli script di libreria da caricare. Tuttavia, è possibile controllare se il debug o rilascio degli script vengono caricati (libreria script o i propri script personalizzati), modificare la proprietà ScriptMode. ScriptMode accetti un'enumerazione ScriptMode i cui membri includono automaticamente, Debug, rilascio ed eredita.

ScriptMode assume un valore di Auto, il che significa che ScriptManager verrà verificata l'attributo di debug nel file Web. config. Quando il debug è false ScriptManager caricherà la versione degli script libreria ASP.NET AJAX. Quando viene soddisfatta debug verrà caricata la versione di debug degli script. Impossibile modificare la proprietà ScriptMode per eseguire il Debug o rilascio forzerà ScriptManager per caricare gli script appropriati indipendentemente dal valore dell'attributo di debug ha in Web. config. Listato 8 illustra un esempio di utilizzo del controllo ScriptManager di caricare script di debug dalla libreria ASP.NET AJAX.

**Listato 8. Il caricamento degli script di debug con ScriptManager**.


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

È inoltre possibile caricare versioni diverse (debug o release) di script personalizzati con proprietà di script di ScriptManager insieme al componente ScriptReference come illustrato nel listato 9.

**Listato 9. Il caricamento di script personalizzato con ScriptManager.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> Se si sta caricando script personalizzati tramite il componente ScriptReference è necessario notificare ScriptManager quando lo script ha terminato il caricamento, aggiungendo il codice seguente nella parte inferiore dello script:


[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

Il codice illustrato nel listato 9 indica a ScriptManager di cercare una versione di debug dello script di persona, in modo che cercherà automaticamente Person.debug.js anziché Person.js. Se il file Person.debug.js non viene trovato, che verrà generato un errore.

Nel caso in cui si desidera un debug o versione finale di uno script personalizzato da caricare in base al valore della proprietà ScriptMode impostato nel controllo ScriptManager, è possibile impostare proprietà ScriptMode del controllo ScriptReference su eredita. In questo modo la versione corretta dello script personalizzato da caricare in base alla proprietà ScriptMode di ScriptManager come illustrato nel listato 10. Poiché la proprietà ScriptMode del controllo ScriptManager è impostata su Debug, lo script Person.debug.js sarà caricato ed usato nella pagina.

**Listato 10. Eredità di ScriptMode di ScriptManager per gli script personalizzati.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

Usando in modo appropriato la proprietà ScriptMode puoi più facilmente il debug di applicazioni e semplificare il processo complessivo. Gli script di rilascio della libreria ASP.NET AJAX sono piuttosto difficili esaminare e leggere poiché la formattazione del codice è stato rimosso mentre gli script di debug vengono formattati in modo specifico per scopi di debug.

## <a name="conclusion"></a>Conclusione

Tecnologia Microsoft per ASP.NET AJAX fornisce una base solida per la compilazione di applicazioni compatibili con AJAX che possono migliorare l'esperienza complessiva dell'utente finale. Tuttavia, come con altre tecnologie di programmazione, bug e altri problemi dell'applicazione sicuramente si verificheranno. Conoscere le diverse opzioni di debug disponibili è possibile salvare una grande quantità di tempo e il risultato in un prodotto più stabile.

In questo articolo è stato introdotto il concetto di diverse tecniche per il debug delle pagine ASP.NET AJAX, tra cui Internet Explorer con Visual Studio 2008, Web Development Helper e Firebug. Questi strumenti possono semplificare il processo di debug generale in quanto è possibile accedere ai dati della variabile, esaminare codice riga per riga e visualizzare le istruzioni di traccia. Oltre a strumenti di debug diversi illustrati, è stato anche illustrato come classe Sys. debug della libreria ASP.NET AJAX è utilizzabile in un'applicazione e come la classe ScriptManager è utilizzabile per caricare il debug o rilascio delle versioni degli script.

## <a name="bio"></a>Informazioni biografiche

Dan Wahlin (Microsoft Most Valuable Professional per ASP.NET e servizi Web XML) è un .NET development instructor e architettura di consulente in formazione tecnica di interfaccia ([www.interfacett.com)](http://www.interfacett.com). Dan ha fondato il codice XML per il sito Web di ASP.NET Developers ([www.XMLforASP.NET](http://www.XMLforASP.NET)), si trova in ufficio del relatore di INETA e come relatore a diverse conferenze. Dan coautore DNA Windows Professional (Wrox), ASP.NET: suggerimenti, esercitazioni e codice (SAM), ASP.NET 1.1 Insider soluzioni, Professional ASP.NET 2.0 AJAX (Wrox), MVP di ASP.NET 2.0 HACK e create XML per sviluppatori ASP.NET (SAM). Quando non sta attualmente scrivendo codice, articoli o libri, Dan piace la scrittura e la registrazione della musica e la riproduzione di golf e pallacanestro con sua moglie e dei bambini.

Scott Cate ha collaborato con tecnologie Web di Microsoft dal 1997 ed è il presidente di myKB.com ([www.myKB.com](http://www.myKB.com)) ed è specializzato nella scrittura di ASP.NET basato su applicazioni incentrate su soluzioni Software di Knowledge Base. È possibile contattare Scott tramite posta elettronica all'indirizzo [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) o il suo blog all'indirizzo [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Precedente](understanding-asp-net-ajax-web-services.md)
