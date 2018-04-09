---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Parte 3: Viste e ViewModel | Documenti Microsoft'
author: jongalloway
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 3 vengono descritte le visualizzazioni e ViewModel.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 497c2898db2e03b0650982c3ad1e6b5ac5ca639d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="part-3-views-and-viewmodels"></a>Parte 3: Viste e ViewModel
====================
da [Jon Galloway](https://github.com/jongalloway)

> L'archivio di musica MVC è un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di MVC ASP.NET e Visual Studio per lo sviluppo web.  
>   
> L'archivio di musica MVC è un'implementazione dell'archivio di esempio semplice che vende album musicali online e ne implementa amministrazione sito di base, account utente e funzionalità di carrello acquisti.  
>   
> Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 3 vengono descritte le visualizzazioni e ViewModel.


Finora è stato semplicemente stato restituisce stringhe da azioni del controller. Che è un modo utile per avere un'idea del funzionamento del controller, ma è non come si desidera compilare un'applicazione web reale. È possibile passare a un metodo migliore per generare HTML al browser che visitano il sito: uno in cui è possibile utilizzare i file di modello per personalizzare più facilmente il contenuto HTML invia nuovamente. Questo è esattamente cosa viste.

## <a name="adding-a-view-template"></a>Aggiunta di un modello di visualizzazione

Per utilizzare un modello di visualizzazione, è modificherai il metodo HomeController indice per restituire un ActionResult e restituire View(), come di seguito:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

La modifica precedente indica che invece che ha restituito una stringa, invece di voler utilizzare una "visualizzazione" per generare un risultato.

A questo punto verrà aggiunto un modello di visualizzazione appropriato al progetto. A tale scopo si sarà posizionare il cursore di testo all'interno del metodo di azione di indice, quindi fare doppio clic e selezionare "Aggiungi visualizzazione". Verrà visualizzata la finestra di dialogo Aggiungi visualizzazione:

![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)

La finestra di dialogo "Aggiungi visualizzazione" consente di visualizzare i file di modello di generare rapidamente e facilmente. Per impostazione predefinita "Aggiungi visualizzazione" finestra di dialogo pre-popola il nome del modello di visualizzazione da creare in modo che corrisponda il metodo di azione che verrà utilizzato. Poiché è utilizzato il menu di scelta rapida "Aggiungi visualizzazione" all'interno del metodo di azione Index () del nostro HomeController, la finestra di dialogo "Aggiungi visualizzazione" sopra è "Index" come nome della vista pre-popolato per impostazione predefinita. Non è necessario modificare le opzioni in questa finestra di dialogo, quindi fare clic sul pulsante Aggiungi.

Quando si fa clic sul pulsante Aggiungi, Visual Web Developer verrà creato un nuovo cshtml modello di visualizzazione per noi nella directory \Views\Home, la creazione della cartella se non esiste già.

![](mvc-music-store-part-3/_static/image2.png)

Il percorso di nome e la cartella del file "Cshtml" è importante e segue le convenzioni di denominazione predefinito MVC ASP.NET. Il nome della directory, \Views\Home, il controller - denominato HomeController corrispondente. Il nome del modello di visualizzazione, indice, corrisponde a metodo di azione del controller che prevede di visualizzare la vista.

ASP.NET MVC consente di evitare di dover specificare in modo esplicito il nome o il percorso di un modello di visualizzazione quando si usa questa convenzione di denominazione per restituire una visualizzazione. Per impostazione predefinita restituirà il modello di visualizzazione \Views\Home\Index.cshtml quando si scrive codice come riportato di seguito nel nostro HomeController:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

Visual Web Developer creato e aperto il modello di visualizzazione "Cshtml" dopo che è stato fatto clic sul pulsante "Aggiungi" nella finestra di dialogo "Aggiungi visualizzazione". Il contenuto di cshtml è illustrato di seguito.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

Questa vista utilizza la sintassi Razor, che è più concisa rispetto al motore di visualizzazione Web Form utilizzato in versioni precedenti di ASP.NET MVC e Web Form ASP.NET. Il motore di visualizzazione Web Form è ancora disponibile in ASP.NET MVC 3, ma molti sviluppatori trovano che il motore di visualizzazione Razor rientri di sviluppo ASP.NET MVC benissimo.

Le prime tre righe impostano il titolo della pagina utilizzando ViewBag. Viene osservare il funzionamento in modo più dettagliato breve, ma prima si aggiorna il testo di intestazione di testo e visualizzare la pagina. Aggiornamento di &lt;h2&gt; tag, ad esempio "Questa è la Home Page", come illustrato di seguito.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

Viene illustrata l'applicazione che il nuovo testo sia visibile nella home page è in esecuzione.

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>Utilizzo di un Layout per gli elementi comuni del sito

La maggior parte dei siti Web con contenuto che viene condiviso tra più pagine: navigazione, piè di pagina, immagini di logo, i riferimenti di foglio di stile e così via. Il motore di visualizzazione Razor è semplice da gestire con una pagina denominata \_cshtml che è stato creato automaticamente per noi all'interno della cartella condivisa/visualizzazioni.

![](mvc-music-store-part-3/_static/image4.png)

Fare doppio clic su questa cartella per visualizzare il contenuto, che vengono mostrate di seguito.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

Verrà visualizzato il contenuto dai nostri singole visualizzazioni per il @RenderBodycomando () e qualsiasi contenuto comune che si desidera venga visualizzato all'esterno che possono essere aggiunti al \_cshtml markup. È opportuno negozio musica MVC di un'intestazione comune con i collegamenti per l'area Home page e l'archivio in tutte le pagine del sito, in modo che verrà aggiunto al modello direttamente sopra che @RenderBodyistruzione ().

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>L'aggiornamento del foglio di stile

Il modello di progetto vuoto include un file CSS molto semplice che include solo gli stili utilizzati per visualizzare i messaggi di convalida. La finestra di progettazione fornisce alcuni CSS e immagini aggiuntive per definire l'aspetto per il sito, in modo che quelli ora verrà aggiunto.

I file CSS e immagini aggiornati sono inclusi nella directory del contenuto di Assets.zip MvcMusicStore disponibile all'indirizzo [negozio di musica MVC](https://github.com/evilDave/MVC-Music-Store). Verrà selezionare entrambi gli elementi in Esplora risorse e trascinarli nella cartella del contenuto della soluzione in Visual Web Developer, come illustrato di seguito:

![](mvc-music-store-part-3/_static/image5.png)

Verrà chiesto di confermare se si desidera sovrascrivere il file Site.css esistente. Fare clic su Sì.

![](mvc-music-store-part-3/_static/image6.png)

La cartella del contenuto dell'applicazione verrà visualizzato come segue:

![](mvc-music-store-part-3/_static/image7.png)

Ora consente di eseguire l'applicazione e visualizzare le modifiche nella Home page.

![](mvc-music-store-part-3/_static/image8.png)

- Si esamini cosa è cambiato: metodo di azione del HomeController l'indice individuato visualizzati e il modello \Views\Home\Index.cshtmlView, anche se il codice chiamato "View() restituito", poiché il modello di visualizzazione di seguito la convenzione di denominazione standard.
- Home Page di visualizzazione di un messaggio di benvenuto semplice che è definito all'interno del modello di visualizzazione \Views\Home\Index.cshtml.
- Utilizza la Home Page il nostro \_cshtml modello, quindi il messaggio di benvenuto è contenuto all'interno del layout standard sito HTML.

## <a name="using-a-model-to-pass-information-to-our-view"></a>Utilizzo di un modello per passare le informazioni per la visualizzazione

Per rendere un sito web molto interessanti, non sarà un modello di visualizzazione che visualizza solo hardcoded HTML. Per creare un sito web dynamic, è opportuno invece passare le informazioni dalle azioni del controller per i modelli di visualizzazione.

Nel modello Model-View-Controller, il termine A che modello fa riferimento oggetti che rappresentano i dati nell'applicazione. Spesso gli oggetti del modello corrispondono alle tabelle del database, ma non necessario.

Metodi di azione del controller che restituiscono un ActionResult possono passare un oggetto modello nella vista. In questo modo un Controller per il pacchetto correttamente tutte le informazioni necessarie per generare una risposta e quindi passare tali informazioni a un modello di visualizzazione da utilizzare per generare la risposta HTML appropriata. Questo è più facile da comprendere per visualizzarlo in azione, pertanto è possibile iniziare subito.

Verranno innanzitutto create alcune classi di modello per rappresentare generi e album all'interno di questo archivio. Iniziamo creando una classe di genere. Fare clic sulla cartella di "Modelli" all'interno del progetto, scegliere l'opzione "Aggiungi classe" e denominare il file "Genre.cs".

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

Aggiungere quindi una stringa pubblica proprietà nome alla classe che è stata creata:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*Nota: nel caso dovrai più chiederti, il {get; impostare;} effettua notazione uso della funzionalità di proprietà implementate automaticamente #. Ciò offre i vantaggi di una proprietà senza richiedere di dichiarare un campo sottostante.*

Successivamente, la stessa procedura per creare una classe Album (denominata Album.cs) che dispone di un titolo e una proprietà di genere:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

Ora che potremo modificare il StoreController per l'utilizzo di viste che consentono di visualizzare informazioni dinamiche dal modello in esame. Se - per scopi dimostrativi - ora è denominato nostri album in base all'ID di richiesta, è possibile visualizzare tali informazioni come la visualizzazione seguente.

![](mvc-music-store-part-3/_static/image10.png)

Si inizierà modificando l'azione di dettagli archivio in modo che visualizzi le informazioni per un singolo album. Aggiungere un'istruzione "using" in cima al **StoreControllers** classe per includere lo spazio dei nomi MvcMusicStore.Models, in modo che non è necessario digitare MvcMusicStore.Models.Album ogni volta che si desidera utilizzare la classe album. La sezione "using" di tale classe verranno visualizzati come indicato di seguito.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

Successivamente, verrà aggiornata l'azione del controller dettagli in modo che restituisca un ActionResult anziché una stringa, come è stato fatto con il metodo di indice del HomeController.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

A questo punto è possibile modificare la logica per restituire un oggetto Album alla visualizzazione. Più avanti in questa esercitazione verranno recuperati i dati da un database, ma per ora verrà utilizzato "fittizio dati" per iniziare.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*Nota: Se non si ha familiarità con c#, si può presupporre che utilizzando var significa che la variabile album sia con associazione tardiva. Che non è corretto, il compilatore c# è utilizzando l'inferenza del tipo in base a ciò che è in corso assegnazione alla variabile per determinare che tale album è di tipo Album e la compilazione la variabile locale album come un tipo di Album, in modo che i risultati ottenuti controllo in fase di compilazione e l'editor di Visual Studio code supporto.*

Creare ora un modello di visualizzazione che utilizza il nostro Album per generare una risposta HTML. Prima che è necessario compilare il progetto in modo che la finestra di dialogo Aggiungi visualizzazione conosce la classe Album appena creata. È possibile compilare il progetto selezionando il Debug⇨Build MvcMusicStore voce di menu (per supplementare, è possibile utilizzare il collegamento Ctrl-MAIUSC + B per compilare il progetto).

![](mvc-music-store-part-3/_static/image11.png)

Ora che è stato configurato il nostro classi di supporto, si è pronti a compilare il modello di visualizzazione. Fare clic all'interno del metodo di dettagli e selezionare "Aggiungi visualizzazione..." dal menu di scelta rapida.

![](mvc-music-store-part-3/_static/image12.png)

Verrà per creare un nuovo modello di visualizzazione, come abbiamo visto prima con la classe HomeController. Poiché si sta creando, dal StoreController verrà per impostazione predefinita generato in un file \Views\Store\Index.cshtml.

A differenza precedente, verrà per controllare la casella di controllo di visualizzazione "Crea oggetto fortemente tipizzato". Verrà quindi selezionare la classe "Album" all'interno di rilascio-downlist "Classe dati visualizzazione". In questo modo, la finestra di dialogo "Aggiungi visualizzazione" creare un modello di visualizzazione che prevede che un Album oggetto verrà passato in modo da utilizzare.

![](mvc-music-store-part-3/_static/image13.png)

Quando si fa clic sul pulsante "Aggiungi" nostro modello di visualizzazione \Views\Store\Details.cshtml verrà creato, contenente il codice seguente.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

Si noti la prima riga, che indica che questa vista è fortemente tipizzata per la classe Album. Il motore di visualizzazione Razor consapevole del fatto che è stato superato un oggetto Album, in modo da poter accedere facilmente le proprietà del modello e disporre anche il vantaggio di IntelliSense nell'editor di Visual Web Developer.

Aggiornamento di &lt;h2&gt; tag in modo da visualizzare proprietà del titolo dell'Album modificando la riga come segue.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

Si noti che IntelliSense viene attivato quando si immette il periodo dopo il @Model (parola chiave), che mostra le proprietà e metodi supportati dalla classe Album.

Verrà ora eseguire nuovamente il progetto e visitare l'URL di archivio/dettagli/5. Verranno esaminati i dettagli di un Album come di seguito.

![](mvc-music-store-part-3/_static/image14.png)

Ora ci accerteremo che un aggiornamento analogo per il metodo di azione archivio Sfoglia. Il metodo di aggiornamento in modo che restituisca un ActionResult e modificare la logica del metodo in modo che crea un nuovo oggetto Genre e viene ripristinata la vista.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

Fare clic nel metodo Sfoglia e selezionare "Aggiungi visualizzazione..." dal menu di scelta rapida, quindi aggiungere una visualizzazione fortemente tipizzata aggiungere una classe fortemente tipizzata per la classe Genre.

![](mvc-music-store-part-3/_static/image15.png)

Aggiornamento di &lt;h2&gt; elemento nella visualizzazione codice (/Views/Store/Browse.cshtml) per visualizzare le informazioni di genere.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

Ora consente di eseguire nuovamente il progetto e passare a/archivio/Sfoglia? Genre = URL di individuazione. Verranno esaminati nella pagina Sfoglia visualizzata nel modo seguente.

![](mvc-music-store-part-3/_static/image16.png)

Infine, questo punto, eseguire un aggiornamento leggermente più complesso per il **archiviare indice** metodo di azione e visualizzazione per visualizzare un elenco di tutti i generi il Negozio. Che faremo utilizzando un elenco di generi come l'oggetto modello, anziché solo un singolo genere.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

Fare clic nel metodo di azione di indice dell'archivio e selezionare Aggiungi visualizzazione, come prima, selezionare il genere come classe di modello e fare clic sul pulsante Aggiungi.

![](mvc-music-store-part-3/_static/image17.png)

Innanzitutto si modificherà il @model dichiarazione per indicare che la vista prevede Genre diversi oggetti anziché uno solo. Modifica la prima riga del /Store/Index.cshtml come segue:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

In questo modo il motore di visualizzazione Razor che utilizzeranno con un oggetto modello che può contenere più oggetti di genere. Verrà usato un oggetto IEnumerable&lt;Genre&gt; anziché un elenco&lt;Genre&gt; perché è più generico, che consente di modificare il tipo di modello in un secondo momento per qualsiasi tipo di oggetto che supporta l'interfaccia IEnumerable.

Successivamente, verrà scorrere gli oggetti di genere nel modello, come illustrato nel seguente codice completate.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

Si noti che è disponibile supporto IntelliSense completo quando si immette questo codice, in modo che quando si digita "@Model." Vediamo tutti i metodi e proprietà supportate da un oggetto IEnumerable di tipo genere.

![](mvc-music-store-part-3/_static/image18.png)

Il ciclo "foreach", all'interno di Visual Web Developer sa che ogni elemento è di tipo genere, pertanto vediamo IntelliSense per ogni tipo di genere.

![](mvc-music-store-part-3/_static/image19.png)

Successivamente, la funzionalità di scaffolding esaminato l'oggetto Genre e determinata che ogni avrà una proprietà Name, pertanto consente di scorrere e li scrive. Genera inoltre i collegamenti di modifica, dettagli e Delete per ogni singolo elemento. È possibile sfruttare che più avanti in questo gestore dell'archivio, ma per ora desideriamo dispone invece di un elenco semplice.

Quando si esegue l'applicazione e passare a /Store, che il numero e l'elenco di generi è visualizzato.

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>Aggiunta di collegamenti tra le pagine

L'URL /Store che elenca generi attualmente Elenca i nomi di genere semplicemente come testo normale. È necessario modificare questo, in modo che anziché il testo normale è invece disponibile il collegamento di nomi di genere all'URL di archivio/Sfoglia appropriato, in modo che facendo clic su un genere musica, ad esempio "Disco" si sposterà/archivio/Sfoglia? genre = Disco URL. È possibile aggiornare il modello di visualizzazione \Views\Store\Index.cshtml all'output di questi collegamenti utilizzando codice simile di sotto **(non di tipo in - si intende migliorare su di esso)**:

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

Che funziona, ma può causare dei problemi in un secondo momento, poiché si basa su una stringa hardcoded. Ad esempio, se si desidera rinominare il Controller, è sarebbe necessario cercare il codice alla ricerca di collegamenti che devono essere aggiornati.

Un approccio alternativo, che è possibile utilizzare sia per sfruttare i vantaggi di un metodo HTML Helper. ASP.NET MVC include i metodi HTML Helper disponibili mediante il codice del modello di visualizzazione per eseguire una serie di attività comuni come segue. Il metodo di supporto Html.ActionLink() è particolarmente utile e semplice generare HTML &lt;un&gt; collega e si occupa di dettagli fastidiosi come assicurandosi che i percorsi URL siano URL correttamente codificati.

Html.ActionLink() dispone di diversi overload diversi per specificare le informazioni necessarie per i collegamenti. Nel caso più semplice, si dovranno fornire solo il testo del collegamento e il metodo di azione a cui passare quando si fa clic sul collegamento ipertestuale nel client. Ad esempio, è possibile collegare "Archivio /" metodo Index () nella pagina dei dettagli di archivio con il testo del collegamento "Vai per l'archivio indice" tramite la chiamata seguente:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*Nota: In questo caso, non occorre specificare il nome del controller in quanto è in corso solo il collegamento a un'altra azione all'interno del controller stesso che esegue il rendering di visualizzazione corrente.*

I collegamenti alla pagina Sfoglia saranno necessario passare un parametro, tuttavia, utilizzeremo un altro overload del metodo HTML. ActionLink che accetta tre parametri:

- 1. Testo del collegamento, che verrà visualizzato il nome di genere
- 2. Nome dell'azione controller (Sfoglia)
- 3. Valori dei parametri di route, che specifica il nome (Genre) e il valore (nome genere)

Inserimento che tutti insieme, ecco come è necessario scrivere tali collegamenti per la visualizzazione dell'indice di archivio:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

Eseguire nuovamente il progetto e accedere all'URL /Store/ è verrà visualizzato un elenco di generi. Ogni genere è un collegamento ipertestuale: quando si fa clic potrebbero essere necessari per l'archivio/Sfoglia? genre =*[genre]* URL.

![](mvc-music-store-part-3/_static/image3.jpg)

Il codice HTML per l'elenco di genere è simile al seguente:

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-2.md)
> [Successivo](mvc-music-store-part-4.md)
