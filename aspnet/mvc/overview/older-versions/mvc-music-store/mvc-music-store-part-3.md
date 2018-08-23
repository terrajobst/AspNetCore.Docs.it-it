---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Parte 3: Visualizzazioni e ViewModel | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 3 riguarda le visualizzazioni e ViewModel.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 828ff18abcc5932f82be71a45ebde589eeb051fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832291"
---
<a name="part-3-views-and-viewmodels"></a>Parte 3: Visualizzazioni e ViewModel
====================
da [Jon Galloway](https://github.com/jongalloway)

> Store la musica MVC è un'applicazione di esercitazione introduce e dettagliata spiega come usare ASP.NET MVC e Visual Studio per lo sviluppo web.  
>   
> La Store musica MVC è un'implementazione di archivio di esempio leggero che vende gli album musicali online e ne implementa amministrazione sito di base, accesso dell'utente e la funzionalità di carrello acquisti.  
>   
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 3 riguarda le visualizzazioni e ViewModel.


Finora è stato semplicemente stato restituisce stringhe dalle azioni del controller. Che è una buona soluzione per avere un'idea di come funzionano i controller, ma è non modo in cui è preferibile creare un'applicazione web reale. Si intende trovare un modo migliore per generare codice HTML al browser che visitano il sito: uno in cui è possibile usare i file di modello per personalizzare più facilmente il contenuto HTML invia nuovamente. Questo è esattamente cosa viste.

## <a name="adding-a-view-template"></a>Aggiunta di un modello di vista

Per usare un modello di vista, si sarà modificare il metodo indice HomeController per restituire un ActionResult e restituisse View(), come di seguito:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

La modifica precedente indica che invece di restituire una stringa, ma invece di utilizzare una "visualizzazione" per generare un risultato.

A questo punto si aggiungerà un modello di visualizzazione appropriato per il progetto. A tale scopo è sarà posizionare il cursore del testo all'interno del metodo di azione Index, quindi fare doppio clic e selezionare "Aggiungi visualizzazione". Verrà visualizzata la finestra di dialogo Aggiungi visualizzazione:

![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)

La finestra di dialogo "Aggiungi visualizzazione" consente di generare rapidamente e facilmente i file di modello di visualizzazione. Per impostazione predefinita "Aggiungi visualizzazione" finestra di dialogo popola preventivamente il nome del modello di visualizzazione da creare in modo che corrisponda al metodo di azione che verrà usato. Poiché è stato usato il menu di scelta rapida "Aggiungi visualizzazione" all'interno del metodo di azione Index () del nostro HomeController, finestra di dialogo "Aggiungi visualizzazione" precedente è "Index" come nome della vista prepopolato per impostazione predefinita. Non è necessario modificare le opzioni in questa finestra di dialogo, fare clic sul pulsante Aggiungi.

Quando si fa clic sul pulsante Aggiungi, Visual Web Developer creerà un index. cshtml nuovo modello di vista automaticamente nella directory \Views\Home, creando la cartella se non esiste già.

![](mvc-music-store-part-3/_static/image2.png)

Il nome e cartella la posizione del file "Index. cshtml" è importante e segue le convenzioni di denominazione di ASP.NET MVC predefinita. Il nome della directory, \Views\Home, corrisponde al controller - denominata HomeController. Il nome del modello di visualizzazione, indice, corrisponde al metodo di azione del controller che prevede di visualizzare la vista.

ASP.NET MVC consente di evitare di dover specificare esplicitamente il nome o il percorso di un modello di visualizzazione quando si usa questa convenzione di denominazione per restituire una visualizzazione. Eseguirà per impostazione predefinita il rendering il modello di vista \Views\Home\Index.cshtml quando si scrive codice simile al seguente nel nostro HomeController:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

Visual Web Developer creato e aperto il modello di vista "Index. cshtml" dopo che è stato fatto clic sul pulsante "Aggiungi" nella finestra di dialogo "Aggiungi visualizzazione". Il contenuto di index. cshtml è illustrato di seguito.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

In questa vista è tramite la sintassi Razor, che è più concisa rispetto al motore di visualizzazione Web Form utilizzato in Web Form ASP.NET e le versioni precedenti di ASP.NET MVC. Il motore di visualizzazione Web Form è ancora disponibile in ASP.NET MVC 3, ma molti sviluppatori trovano che il motore di visualizzazione Razor sia adeguato particolarmente bene lo sviluppo di ASP.NET MVC.

Le prime tre righe impostano il titolo della pagina usando ViewBag. Verrà osservare il funzionamento in modo più dettagliato a breve, ma prima di tutto è possibile aggiornare il testo di intestazione di testo e visualizzare la pagina. Aggiorna il &lt;h2&gt; tag scrivo "Questa è la Home Page", come illustrato di seguito.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

È in esecuzione nell'applicazione viene illustrato che il nuovo testo sia visibile nella home page.

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>Utilizzo di un Layout per gli elementi comuni del sito

La maggior parte dei siti Web con contenuto che verrà condivise tra molte pagine: esplorazione, piè di pagina, immagini del logo, i riferimenti di foglio di stile e così via. Il motore di visualizzazione Razor semplifica questa attività gestire utilizzando una pagina denominata \_layout. cshtml che è stato creato automaticamente per noi all'interno della cartella condivisa/viste /.

![](mvc-music-store-part-3/_static/image4.png)

Fare doppio clic su questa cartella per visualizzare il contenuto, come illustrato di seguito.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

Verrà visualizzato il contenuto dai nostri singole visualizzazioni per il @RenderBodycomando () e qualsiasi contenuto comune che si desidera visualizzare all'esterno che possono essere aggiunti al \_markup di layout. cshtml. Sarebbe auspicabile nostro Store musica MVC per avere un'intestazione comune con collegamenti per l'area di Store e la Home page in tutte le pagine del sito, in modo che verrà aggiunto al modello direttamente sopra che @RenderBodyistruzione ().

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>L'aggiornamento del foglio di stile

Il modello di progetto vuoto include un file CSS molto semplice che include solo gli stili utilizzati per visualizzare i messaggi di convalida. La finestra di progettazione ha fornito alcuni CSS e immagini aggiuntive per definire l'aspetto per il sito, quindi si aggiungerà quelle nell'ora.

Il file CSS e immagini aggiornati sono inclusi nella directory del contenuto di Assets.zip MvcMusicStore disponibile all'indirizzo [MVC-musica-Store](https://github.com/evilDave/MVC-Music-Store). Abbiamo selezionare entrambi i sistemi operativi in Windows Explorer e trascinarli nella cartella del contenuto della nostra soluzione in Visual Web Developer, come illustrato di seguito:

![](mvc-music-store-part-3/_static/image5.png)

Verrà chiesto di confermare se si desidera sovrascrivere il file CSS esistente. Fare clic su Sì.

![](mvc-music-store-part-3/_static/image6.png)

La cartella del contenuto dell'applicazione verrà ora visualizzato come segue:

![](mvc-music-store-part-3/_static/image7.png)

A questo punto è possibile eseguire l'applicazione e visualizzare l'aspetto di tali modifiche nella Home page.

![](mvc-music-store-part-3/_static/image8.png)

- È opportuno esaminare che cosa è cambiato: metodo di azione Index del HomeController The trovato e visualizzato il modello \Views\Home\Index.cshtmlView, anche se il codice chiamato "View() restituito", perché il modello di visualizzazione seguita la convenzione di denominazione standard.
- Home Page Visualizza un messaggio di benvenuto semplice che è definito all'interno del modello di visualizzazione \Views\Home\Index.cshtml.
- Usa la Home Page nostro \_modello di layout. cshtml, e in modo che il messaggio di benvenuto è contenuto all'interno del layout del sito standard HTML.

## <a name="using-a-model-to-pass-information-to-our-view"></a>Utilizzo di un modello per passare le informazioni di visualizzazione

Un modello di visualizzazione che visualizza solo HTML hardcoded non dovrà creare un sito web molto interessante. Per creare un sito web dinamico, è opportuno invece passare le informazioni dalle azioni del controller per i modelli di visualizzazione.

Nel modello Model-View-Controller, il termine A che modello fa riferimento oggetti che rappresentano i dati nell'applicazione. Spesso, gli oggetti del modello corrispondono alle tabelle del database, ma sono privi di.

Metodi di azione del controller che restituiscono un ActionResult possono passare un oggetto del modello alla visualizzazione. In questo modo un Controller per il pacchetto correttamente tutte le informazioni necessarie per generare una risposta e quindi passare tali informazioni a un modello di vista da utilizzare per generare la risposta appropriata in HTML. Questo è più facile da comprendere per vederla in azione, bene, cominciamo.

Prima di tutto si creerà alcune classi di modello per rappresentare gli album e generi entro lo store. Iniziamo creando una classe al genere. Fare doppio clic su cartella "Models" all'interno del progetto, scegliere l'opzione "Aggiungi classe" e denominare il file "Genre.cs".

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

Quindi aggiungere una proprietà nome di stringa pubblica alla classe che è stata creata:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*Nota: nel caso ve lo chiediate, il {get; set;} effettua notazione uso della funzionalità di proprietà implementate automaticamente di #. Questo ci offre i vantaggi di una proprietà senza la necessità di dichiarare un campo sottostante.*

Successivamente, seguire gli stessi passaggi per creare una classe di Album (denominata Album.cs) che presenta un titolo e una proprietà di genere:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

A questo punto è possibile modificare il StoreController per utilizzare le viste che mostrano informazioni dinamiche dal modello. Se - a scopo dimostrativo subito - denominato nostro album in base all'ID richiesta, si sono visualizzare tali informazioni come la visualizzazione seguente.

![](mvc-music-store-part-3/_static/image10.png)

Si inizierà modificando l'azione Details di Store in modo che visualizzi le informazioni per un singolo album. Aggiungere un'istruzione "using" in cima il **StoreControllers** classe per includere lo spazio dei nomi MvcMusicStore.Models, in modo che non è necessario digitare MvcMusicStore.Models.Album ogni volta che si vuole usare la classe di album. La sezione "using" di tale classe dovrebbe ora essere visualizzato come indicato di seguito.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

Successivamente, verrà aggiornata l'azione del controller Details in modo che restituisca un ActionResult anziché una stringa, come è stato fatto con il metodo di indice di HomeController.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

A questo punto è possibile modificare la logica per restituire un oggetto di Album alla visualizzazione. Più avanti in questa esercitazione verranno recuperati i dati da un database, ma per ora si userà "fittizio di dati" per iniziare.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*Nota: Se non si ha familiarità con c#, si potrebbe supporre che utilizzo di var significa che la variabile di album sia con associazione tardiva. Non è corretto: il compilatore c# usa l'inferenza del tipo basata su ciò che abbiamo stiamo assegnazione alla variabile per determinare che tale album JE typu Album e la compilazione alla variabile locale album come un tipo di Album così ottenere controllo in fase di compilazione e l'editor di Visual Studio code supporto tecnico.*

Creare ora un modello di vista che usa i nostri Album per generare una risposta HTML. Prima di procedere è necessario compilare il progetto in modo che la finestra di dialogo Aggiungi visualizzazione sappia sulla nostra classe Album appena creata. È possibile compilare il progetto selezionando il Debug⇨Build MvcMusicStore voce di menu (per supplementare, è possibile usare lo scelta rapida Ctrl + MAIUSC + B per compilare il progetto).

![](mvc-music-store-part-3/_static/image11.png)

Ora che è stato configurato le classi di supporta, siamo pronti a compilare il modello di visualizzazione. Fare doppio clic all'interno del metodo di dettagli e scegliere "Aggiungi visualizzazione" dal menu di scelta rapida.

![](mvc-music-store-part-3/_static/image12.png)

Si intende creare un nuovo modello di visualizzazione come fatto in precedenza con la classe HomeController. Poiché stiamo creando si dal StoreController verrà per impostazione predefinita generato in un file \Views\Store\Index.cshtml.

A differenza di prima, si intende controllare la casella di controllo di visualizzazione "Crea oggetto fortemente tipizzato". Verrà quindi selezionare la classe "Album" all'interno di "Data-classe di visualizzazione" drop-downlist. In questo modo la finestra di dialogo "Aggiungi visualizzazione" creare un modello di visualizzazione che prevede che un Album oggetto verrà passato in modo da usare.

![](mvc-music-store-part-3/_static/image13.png)

Quando si fa clic sul pulsante "Aggiungi" nostro modello di vista \Views\Store\Details.cshtml verrà creato, che contiene il codice seguente.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

Si noti che la prima riga, che indica che questa vista è fortemente tipizzata per la classe di Album. Il motore di visualizzazione Razor riconosce che si è stato passato un oggetto di Album, poter accedere facilmente alle proprietà dei modelli e ha anche il vantaggio di IntelliSense nell'editor di Visual Web Developer.

Aggiorna il &lt;h2&gt; contrassegnare in modo che visualizzi proprietà titolo dell'Album modificando tale riga deve essere visualizzato come indicato di seguito.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

Si noti che IntelliSense viene attivata quando si immette il periodo dopo il @Model (parola chiave), che mostra le proprietà e metodi che supporta la classe di Album.

È possibile ora eseguire nuovamente il progetto e visitare l'URL di Store/dettagli/5. Vedremo i dettagli di un Album come di seguito.

![](mvc-music-store-part-3/_static/image14.png)

A questo punto ci assicureremo che un aggiornamento analogo per il metodo di azione Store Sfoglia. Aggiornare il metodo in modo che restituisca un ActionResult e modificare la logica del metodo in modo che lo crea un nuovo oggetto Genre e lo restituisce alla visualizzazione.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

Fare doppio clic nel metodo Sfoglia e selezionare "Aggiungi in corso di visualizzazione" dal menu di scelta rapida, quindi aggiungere una visualizzazione fortemente tipizzata aggiungere una classe fortemente tipizzata per la classe Genre.

![](mvc-music-store-part-3/_static/image15.png)

Aggiorna il &lt;h2&gt; elemento nella visualizzazione del codice (in /Views/Store/Browse.cshtml) per visualizzare le informazioni al genere.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

A questo punto è possibile eseguire nuovamente il progetto e passare a/Store/Sfoglia? Genre = URL del Disco. Si vedrà nella pagina Sfoglia visualizzata nel modo seguente.

![](mvc-music-store-part-3/_static/image16.png)

Infine, creiamo un aggiornamento leggermente più complesso per la **Store indice** metodo di azione e visualizzazione per visualizzare un elenco di tutti i generi nel nostro store. Che faremo usando un elenco di generi come l'oggetto modello, anziché semplicemente un genere singolo.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

Nel metodo di azione Index Store e scegliere Aggiungi visualizzazione come prima, selezionare il genere come classe di modello e fare clic sul pulsante Aggiungi.

![](mvc-music-store-part-3/_static/image17.png)

Prima di tutto si modificherà il @model dichiarazione per indicare che la vista si aspettano Genre diversi oggetti anziché uno solo. Modificare la prima riga del /Store/Index.cshtml come segue:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

Ciò indica al motore di visualizzazione Razor che verranno usate con un oggetto modello che può contenere più oggetti al genere. Utilizziamo un oggetto IEnumerable&lt;Genre&gt; anziché un elenco&lt;Genre&gt; poiché è più generico, che consente di modificare il tipo di modello in un secondo momento per qualsiasi tipo di oggetto che supporta l'interfaccia IEnumerable.

Successivamente, verranno esaminati in ciclo gli oggetti di genere del modello come illustrato nel seguente codice completate.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

Si noti che è presente il supporto IntelliSense completo quando si immette questo codice, in modo che quando si digita "@Model." Noteremo tutti i metodi e proprietà supportate da un oggetto IEnumerable di tipo genere.

![](mvc-music-store-part-3/_static/image18.png)

Nel nostro ciclo "foreach", Visual Web Developer sa che ogni elemento è di tipo Genre, noterete IntelliSense per ogni tipo di genere.

![](mvc-music-store-part-3/_static/image19.png)

Successivamente, la funzionalità di scaffolding esaminato l'oggetto Genre e determinata che ognuna avrà una proprietà Name, scorre in ciclo e li scrive in modo. Genera anche i collegamenti di modifica, dettagli e Delete per ogni singolo elemento. Prenderemo in cui sfruttare in un secondo momento nel nostro gestore dell'archivio, ma per ora ci piacerebbe avere invece un elenco semplice.

Quando si esegue l'applicazione e passare a /Store, vediamo che viene visualizzato il numero e l'elenco dei generi.

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>Aggiunta di collegamenti tra pagine

L'URL /Store che elenca generi attualmente Elenca i nomi di Sottogeneri semplicemente come testo normale. È possibile modificare questo, in modo che invece che testo normale è invece disponibile il collegamento di nomi di genere sull'URL appropriato Store/esplorazione, in modo che facendo clic su un genere musica, ad esempio "Disco" per passare alla/Store/Sfoglia? genre = URL del Disco. È possibile aggiornare il modello di vista \Views\Store\Index.cshtml all'output di questi collegamenti tramite il codice come di seguito **(non digitare quanto segue in, dobbiamo migliorare su di esso)**:

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

Funziona, ma ciò potrebbe causare problemi in un secondo momento, poiché si basa su una stringa hardcoded. Ad esempio, se si vuole rinominare il Controller, è necessario eseguire la ricerca tramite il codice alla ricerca di collegamenti che devono essere aggiornati.

Un approccio alternativo, che è possibile usare sia per sfruttare i vantaggi di un metodo di HTML Helper. ASP.NET MVC include metodi HTML Helper disponibili mediante il codice del modello di visualizzazione per eseguire una serie di attività comuni come segue. Il metodo helper Html.ActionLink() è particolarmente utile e rende più semplice creare HTML &lt;un&gt; collega e si occupa di dettagli fastidiosi, ad esempio assicurandosi che i percorsi URL siano URL correttamente codificati.

Html.ActionLink() dispone di diversi overload diversi per specificare quante più informazioni in base alle esigenze per i collegamenti. Nel caso più semplice, si dovranno fornire solo il testo del collegamento e il metodo di azione a cui passare quando si fa clic sul collegamento ipertestuale nel client. Ad esempio, è possibile collegare a "/ Store /" metodo Index () nella pagina dei dettagli di Store con il testo del collegamento "Vai al Store Index" tramite la chiamata seguente:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*Nota: In questo caso, non dobbiamo specificare il nome del controller perché si sta semplicemente il collegamento a un'altra azione all'interno del controller stesso che esegue il rendering della visualizzazione corrente.*

I collegamenti alla pagina di esplorazione saranno necessario passare un parametro, tuttavia, quindi verrà usato un altro overload del metodo HTML. ActionLink che accetta tre parametri:

- 1. Testo del collegamento, che verrà visualizzato il nome genere
- 2. Nome azione del controller (Sfoglia)
- 3. Valori dei parametri di route, che specifica il nome (genere) e il valore (nome genere)

L'inserimento che tutti gli elementi, ecco come verranno scritti i collegamenti alla vista Index Store:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

A questo punto quando si esegue nuovamente il progetto e accedere all'URL /Store/ si verrà visualizzato un elenco dei generi. Ogni genere è un collegamento ipertestuale: quando si fa clic potrebbero essere necessari al nostro/Store/Sfoglia? genre =*[genre]* URL.

![](mvc-music-store-part-3/_static/image3.jpg)

Il codice HTML per l'elenco di genere è simile alla seguente:

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-2.md)
> [Successivo](mvc-music-store-part-4.md)
