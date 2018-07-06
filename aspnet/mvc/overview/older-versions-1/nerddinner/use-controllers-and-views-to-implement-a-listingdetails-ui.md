---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: Usare controller e visualizzazioni per implementare un'interfaccia utente elenco/dettagli | Microsoft Docs
author: microsoft
description: Passaggio 4 viene illustrato come aggiungere un Controller per l'applicazione che si avvale del modello per fornire agli utenti un'esperienza di navigazione elenco/dettagli dei dati...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 7a0a057efb52a869a72722b24d7283cb883db858
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838585"
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>Usare controller e visualizzazioni per implementare un'interfaccia utente elenco/dettagli
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Si tratta di passaggio 4 di una liberazione [esercitazione sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-dettaglio come compilare una piccola, ma completa, applicazione web con ASP.NET MVC 1.
> 
> Passaggio 4 viene illustrato come aggiungere un Controller per l'applicazione che si avvale del modello per fornire agli utenti un'esperienza di navigazione di elenco/dettagli dei dati per dinners sul nostro sito NerdDinner.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le [Guida introduttiva con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oppure [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner passaggio 4: Controller e visualizzazioni

Con i framework web tradizionale (ASP classico, Web Form ASP.NET, PHP e così via), gli URL in ingresso in genere vengono mappati ai file su disco. Ad esempio: una richiesta per un URL, ad esempio "/ Products. aspx" o "/ Products" potrebbe essere elaborata da un file "Products" o "Products".

Framework MVC basati su Web eseguire il mapping degli URL al codice lato server in modo leggermente diverso. Anziché eseguire il mapping degli URL in ingresso ai file, ma eseguono il mapping degli URL ai metodi nelle classi. Queste classi vengono chiamate "Controller" e sono responsabili per l'elaborazione delle richieste HTTP in ingresso, gestione dell'input dell'utente, eseguire il recupero e salvataggio dei dati e determinare la risposta da inviare al client (visualizzazione del contenuto HTML, scaricare un file, il reindirizzamento a un altro URL e così via).

Ora che è stato compilato un modello di base per l'applicazione di NerdDinner, il passaggio successivo sarà quello di aggiungere un Controller per l'applicazione che si avvale di esso per fornire agli utenti un'esperienza di navigazione di elenco/dettagli dei dati per Dinners sul nostro sito.

### <a name="adding-a-dinnerscontroller-controller"></a>Aggiunta di un Controller DinnersController

È possibile iniziare facendo clic sulla cartella "Controller" all'interno di progetto web e quindi selezionare il **Add -&gt;Controller** comando di menu (è anche possibile eseguire a questo comando, digitando Ctrl + M, Ctrl + C):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

Verrà visualizzata la finestra di dialogo "Aggiungi Controller":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Si verrà denominare il nuovo controller "DinnersController" e fare clic sul pulsante "Aggiungi". Visual Studio aggiungerà quindi un file DinnersController.cs sotto la directory \Controllers:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

Verrà inoltre aperta la nuova classe DinnersController all'interno dell'editor di codice.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>Aggiunta di Index () e i metodi di azione Details() alla classe DinnersController

Si vuole abilitare visitatori che utilizzano la nostra applicazione per sfogliare un elenco di dinners imminenti e consentire loro di fare clic su qualsiasi Dinner nell'elenco per visualizzarne i dettagli specifici. Faremo pubblicando l'URL seguente dall'applicazione:

| **URL** | **Scopo** |
| --- | --- |
| */Dinners/* | Visualizzare un elenco HTML di dinners imminenti |
| */Dinners/dettagli / [id]* | Visualizzare informazioni dettagliate su un dinner specifico indicato da un parametro "id" incorporato nell'URL, che corrisponderà a DinnerID di dinner nel database. Ad esempio: /Dinners/Details/2 verrebbe visualizzata una pagina HTML con i dettagli di Dinner il cui valore DinnerID è 2. |

Implementazioni iniziali degli URL seguenti verranno pubblicate mediante l'aggiunta di due pubblico "metodi di azione" alla nostra classe DinnersController come di seguito:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

Eseguiamo sarà l'applicazione di NerdDinner e usare il browser per richiamarle. Digitando la *"Dinners /"* causerà l'URL del nostro *Index ()* metodo da eseguire e lo invierà la risposta seguente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

Digitando la *"/ Dinners/dettagli/2"* causerà l'URL del nostro *Details()* metodo da eseguire e inviare la risposta seguente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

È lecito chiedersi - come ASP.NET MVC sapere per creare la classe DinnersController e richiamare tali metodi? Tenere presente che è possibile dare un'occhiata veloce di funzionamento del routing.

### <a name="understanding-aspnet-mvc-routing"></a>Informazioni su ASP.NET MVC Routing

ASP.NET MVC include un potente motore di routing di URL che fornisce una notevole flessibilità nel controllo del modo in cui vengono eseguito il mapping degli URL alle classi controller. Consente di personalizzare completamente come ASP.NET MVC sceglie quale classe di controller per creare, il metodo da richiamare su di esso, nonché di configurare diverse modalità che le variabili possono essere analizzate dall'URL/stringa di query e passate al metodo come parametro automaticamente argomenti. Offre la flessibilità necessaria per completamente un sito di ottimizzare per SEO (Ottimizzazione motore di ricerca), nonché qualsiasi struttura di URL da un'applicazione di pubblicazione.

Per impostazione predefinita, i nuovi progetti ASP.NET MVC dotati di un set preconfigurato di regole di routing di URL già registrato. Ciò ci consente di iniziare con facilità in un'applicazione senza dover configurare alcuna operazione in modo esplicito. Le registrazioni regola di routing predefinita sono disponibili all'interno della classe "Applicazione" dei nostri progetti, che è possibile aprire facendo doppio clic il file "Global. asax" nella radice del progetto:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

Le regole di routing di ASP.NET MVC predefinita vengono registrate all'interno del metodo "RegisterRoutes" di questa classe:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

Le route". MapRoute() "chiamata al metodo precedente registra una regola di routing predefinita che viene eseguito il mapping degli URL in ingresso alle classi controller utilizzando il formato dell'URL:" / {controller} / {action} / {id} ", dove"controller"è il nome della classe controller per creare un'istanza,"action"è il nome di un metodo pubblico da richiamare su di esso e "id" è un parametro facoltativo incorporato all'interno dell'URL che può essere passato come argomento al metodo. Il terzo parametro passato alla chiamata al metodo "MapRoute()" è un set di valori predefiniti da utilizzare per i valori di id/controller/azione nel caso in cui non sono presenti nell'URL (Controller = "Home" Action = "Index", Id = "").

Di seguito è una tabella che mostra in che modo un'ampia gamma di URL vengono mappati utilizzando il valore predefinito "<em>/ {controller} / {action} / {id}"</em>regola route:

| **URL** | **Classe controller** | **Metodo di azione** | **Parametri passati** |
| --- | --- | --- | --- |
| */Dinners/Details/2* | DinnersController | Details(ID) | id=2 |
| */Dinners/Edit/5* | DinnersController | Edit(ID) | id=5 |
| */Dinners/Create* | DinnersController | Create) | N/D |
| */ Dinners* | DinnersController | Index) | N/D |
| */Home* | HomeController | Index) | N/D |
| */* | HomeController | Index) | N/D |

Le ultime tre righe mostrano i valori predefiniti (Controller Home, = Action = indice, Id = "") in uso. Poiché il metodo "Index" viene registrato come il nome dell'azione predefinita se non è specificato, il "/ Dinners" e "/Home" URL causa il metodo di azione Index () da richiamare su alle classi Controller. Perché il controller "Home" viene registrato come il controller predefinito se non è specificato, l'URL "/" fa in modo che la classe HomeController deve essere creato e il metodo di azione Index () su di esso da richiamare.

Se non si desidera che queste regole di routing di URL predefinito, la buona notizia è che sono facili da modificare - sufficiente modificarli all'interno del metodo RegisterRoutes precedente. Per l'applicazione di NerdDinner, tuttavia, non ci occuperemo per modificare le regole di routing di URL predefinito, invece sarà sufficiente usare come-è.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>Uso di DinnerRepository dal nostro DinnersController

Ora Sostituiamo la nostra implementazione corrente del DinnersController i metodi di azione Index () e Details() con implementazioni che usano il modello.

Si userà la classe DinnerRepository abbiamo creato in precedenza per implementare il comportamento. Si sarà iniziare aggiungendo un'istruzione "using" che fa riferimento lo spazio dei nomi "NerdDinner.Models" e quindi dichiarare un'istanza del nostro DinnerRepository come campo nella classe DinnerController.

Più avanti in questo capitolo verrà introdotto il concetto di "Dependency Injection" e Mostra un altro modo ai controller ottenere un riferimento a un DinnerRepository che consente una migliore unit test, ma per destra ora ci limiteremo a creare un'istanza del nostro DinnerRepository inline, come di seguito.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

A questo punto siamo pronti generare una risposta HTML con gli oggetti del modello di dati recuperati.

### <a name="using-views-with-our-controller"></a>Uso delle viste con Controller

Sebbene sia possibile scrivere codice all'interno i metodi di azione per assemblare HTML e quindi usare il *Response* metodo helper per inviarlo al client, tale approccio diventa rapidamente piuttosto difficile da gestire. Un approccio migliore è per noi per eseguire solo l'applicazione e per la logica dei dati nei metodi di azione nostro DinnersController e quindi passare i dati necessari per eseguire il rendering di una risposta HTML a un modello di "visualizzazione" separata che è responsabile per l'output alla relativa rappresentazione in HTML di esso. Come vedremo tra breve, un modello "view" è un file di testo che contiene in genere una combinazione di markup HTML e codice incorporato per il rendering.

Separare la logica del controller da eseguito il rendering della visualizzazione offre diversi vantaggi di big Data. In particolare consente di applicare una netta "separazione delle problematiche" tra il codice dell'applicazione e il codice di formattazione o rendering dell'interfaccia utente. Questo rende molto più semplice per la logica dell'applicazione unit test in modo isolato dalla logica di rendering dell'interfaccia utente. Rende più semplice in un secondo momento modificare i modelli per il rendering dell'interfaccia utente senza dover apportare modifiche al codice dell'applicazione. E può risultare più semplice per gli sviluppatori e progettisti di collaborare insieme ai progetti.

È possibile aggiornare la classe DinnersController per indicare che si desidera usare un modello di visualizzazione per inviare una risposta dell'interfaccia utente HTML modificando le firme del metodo nostri due dei metodi di azione dall'avere un tipo restituito "void" invece avere un tipo restituito "ActionResult". È quindi possibile chiamare il *View()* metodo helper nella classe base Controller per restituire un oggetto "ViewResult" simile alla seguente:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

La firma del *View()* metodo helper viene usato in precedenza simile al seguente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

Il primo parametro per il *View()* metodo helper è il nome del file di modello della vista da utilizzare per eseguire il rendering della risposta HTML. Il secondo parametro è un oggetto modello che contiene i dati che il modello di vista necessita per il rendering della risposta HTML.

In metodo di azione Index () qui viene chiamato il *View()* metodo di supporto e che indica che si desidera eseguire il rendering di un elenco HTML di dinners usando un modello di visualizzazione "Index". Si passa il modello di vista una sequenza di oggetti Dinner per generare l'elenco da:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

In metodo di azione Details() si tenta di recuperare un oggetto la cena usando l'id specificato all'interno dell'URL. Se non viene trovato un Dinner valido, chiamiamo il *View()* metodo helper, indicante che si vuole usare un modello di visualizzazione "Dettagli" per il rendering dell'oggetto Dinner recuperato. Se viene richiesto un dinner non valido, si esegue il rendering di un messaggio di errore utili che indica che non sono presenti la cena usando un modello di visualizzazione "NotFound" (una versione di overload e le *View()* metodo helper che richiede solo il nome del modello ):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

È possibile a questo punto implementare i modelli di vista "NotFound", "Dettagli" e "Index".

### <a name="implementing-the-notfound-view-template"></a>Implementare il modello di vista "NotFound"

Inizieremo implementando il modello di visualizzazione: "NotFound" che visualizza un messaggio di errore descrittivo che indica che non è stata trovata la cena richiesta.

Si verrà creare un nuovo modello di visualizzazione posizionando il cursore di testo all'interno di un metodo di azione del controller, quindi fare clic e scegliere il comando di menu "Aggiungi visualizzazione" (è anche possibile eseguire a questo comando, digitando Ctrl + M, Ctrl + V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Verrà visualizzata una finestra di dialogo "Aggiungi visualizzazione", ad esempio sotto. Per impostazione predefinita che la finestra di dialogo popola preventivamente il nome della visualizzazione da creare in base al nome del metodo di azione il cursore è stato quando la finestra di dialogo è stata avviata (in questo caso "Dettagli"). Poiché desideriamo prima di tutto implementare il modello "NotFound", si sarà eseguire l'override di questo nome di visualizzazione e impostarlo invece essere "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Quando si fa clic sul pulsante "Aggiungi", Visual Studio creerà un nuovo modello di visualizzazione "NotFound.aspx" per noi all'interno della directory "\Views\Dinners" (che verrà creata anche se la directory non esiste già):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

Determina anche l'apertura dei nostri nuovo modello di visualizzazione "NotFound.aspx" all'interno dell'editor di codice:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

I modelli di vista per impostazione predefinita hanno due "contenute aree" in cui è possibile aggiungere contenuto e il codice. Il primo consente di personalizzare la "title" della pagina HTML inviata nuovamente all'origine. Il secondo consente di personalizzare il "contenuto principale" della pagina HTML inviato nuovamente all'origine.

Per implementare il modello di visualizzazione "NotFound" viene aggiunto un contenuto di base:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

È quindi possibile eseguire la verifica all'interno del browser. A tale scopo verrà richiesta la *"/ Dinners/dettagli/9999"* URL. Si farà riferimento a un dinner che attualmente non esiste nel database e causerà il metodo di azione DinnersController.Details() per il rendering del nostro modello di visualizzazione "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

Si noterà che nella schermata precedente è che il modello di vista di base è ereditato una serie di HTML che circonda il principale contenuto della schermata. Questo avviene perché il modello di visualizzazione è usando un modello "pagina master" che permette di applicare un layout coerente in tutte le visualizzazioni nel sito. Si parlerà di funzionamento delle pagine master altre in una parte successiva di questa esercitazione.

### <a name="implementing-the-details-view-template"></a>Implementare il modello di vista "Dettagli"

Verrà ora implementata il modello di vista "Dettagli", che genera codice HTML a un singolo modello Dinner.

Si sarà eseguire questa operazione posizionando il cursore del testo all'interno del metodo di azione dettagli e quindi fare clic e scegliere il comando di menu "Aggiungi visualizzazione" (o premere Ctrl + M, Ctrl + V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

Verrà visualizzata la finestra di dialogo "Aggiungi visualizzazione". Il nome della visualizzazione predefinita ("Dettagli") verranno conservati. È inoltre sarà selezionare la casella di controllo "Crea una visualizzazione fortemente tipizzata" nella finestra di dialogo e selezionare (tramite l'elenco a discesa combobox) il nome del tipo di modello che si passa dal Controller alla vista. Per questa visualizzazione si passa un oggetto Dinner (il nome completo per questo tipo è: "NerdDinner.Models.Dinner"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

A differenza del modello precedente, in cui abbiamo scelto di creare una visualizzazione"vuota", questa volta che si sceglierà di automaticamente "lo scaffolding di" la vista usando un modello "Dettagli". È possibile indicarlo modificando l'elenco a discesa "Visualizzare il contenuto" nella finestra di dialogo precedente.

"Lo scaffolding" genererà un'implementazione iniziale del nostro modello di vista Dettagli in base all'oggetto Dinner che viene passato ad esso. Ciò fornisce un modo semplice per noi rapidamente iniziare a usare l'implementazione di modello di visualizzazione.

Quando si fa clic sul pulsante "Aggiungi", Visual Studio creerà un nuovo file di modello di visualizzazione "Details" automaticamente entro la directory "\Views\Dinners":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

Verrà inoltre aperto configura il nuovo modello di visualizzazione "Details" all'interno dell'editor di codice. Contiene un'implementazione di scaffold iniziale di una visualizzazione dettagli basata su un modello cena. Il motore di scaffolding Usa la reflection .NET per esaminare le proprietà pubbliche esposte nella classe passata e verrà aggiunto contenuto appropriato in base a ogni tipo che rileva:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

È possibile richiedere il *"/ Dinners/dettagli/1"* URL per visualizzare questa implementazione di scaffold "Dettagli" come apparirà nel browser. Con questo URL verrà visualizzato uno del dinners che abbiamo aggiunto manualmente al database quando abbiamo creato prima di tutto:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

Questo arrivati rapidamente e costituisce un'implementazione iniziale della visualizzazione Details. aspx. Possiamo quindi andare e modificarlo per personalizzare l'interfaccia utente per la soddisfazione.

Quando si esamina più da vicino il modello Details. aspx, verrà individuato che lo contiene codice HTML statico, nonché per il rendering codice incorporato. &lt;% %&gt; nugget di codice di eseguire codice quando il modello di vista esegue il rendering, e &lt;% = %&gt; nugget di codice eseguirà il codice contenuto all'interno di essi e quindi eseguire il rendering il risultato nel flusso di output del modello.

È possibile scrivere codice all'interno di visualizzazione che accede all'oggetto modello "Dinner" che è stato passato dal controller usando una proprietà di "Modello" fortemente tipizzata. Visual Studio ci offre codice-funzionalità intellisense complete quando si accede a questa proprietà "Modello" all'interno dell'editor:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

È possibile apportare alcune modifiche di lieve in modo che l'origine per il modello di vista Dettagli finale simile al seguente:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Quando si accede il *"/ Dinners/dettagli/1"* URL anche in questo caso disporrà di rendering simile alla seguente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>Implementare il modello di vista "Index"

Verrà ora implementata il modello di vista "Index", che genera un elenco di Dinners imminente. Cose da fare ciò verrà posizionare il cursore del testo all'interno del metodo di azione Index e quindi a destra fare clic su e scegliere il comando di menu "Aggiungi visualizzazione" (o premere Ctrl + M, Ctrl + V).

Nella finestra di dialogo "Aggiungi visualizzazione" verranno mantenere il modello di visualizzazione denominato "Index" e selezionare la casella di controllo "Crea una visualizzazione fortemente tipizzata". Questa volta si sceglierà automaticamente generare un modello di vista "List" e selezionare "NerdDinner.Models.Dinner" come tipo di modello passato alla visualizzazione (quale perché è stato indicato che stiamo creando un scaffold causerà la finestra di dialogo Aggiungi visualizzazione presupporre siamo "List" il passaggio di una sequenza di oggetti Dinner dal Controller alla vista):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Quando si fa clic sul pulsante "Aggiungi", Visual Studio creerà un nuovo file di modello di visualizzazione "Index" per noi entro la directory "\Views\Dinners". Eseguirà "lo scaffolding" un'implementazione iniziale all'interno che fornisce un elenco di tabelle HTML del Dinners si passa alla visualizzazione.

Quando si esegue l'accesso alle applicazioni e il *"Dinners /"* URL l'elenco di dinners eseguirà il rendering come segue:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

La soluzione nella tabella precedente offre un layout griglia dei dati Dinner – che non sono abbastanza è auspicabile per l'elenco di Dinner rivolte. È possibile aggiornare il modello di vista Index. aspx e modificarlo per elencare le colonne di un numero inferiore di dati e usare una &lt;ul&gt; elemento per eseguirne il rendering anziché una tabella usando il codice seguente:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Utilizziamo la parola chiave "var" all'interno dell'istruzione foreach precedente mentre si esegue il ciclo su ciascuna etichetta nel modello. Quelli familiarità con c# 3.0 potrebbe pensare che utilizzando "var" significa che l'oggetto dinner sia con associazione tardiva. Significa invece che il compilatore utilizza inferenza del tipo rispetto alla proprietà "Modello" fortemente tipizzata (che è di tipo "IEnumerable&lt;Dinner&gt;") e la compilazione alla variabile locale "dinner" come tipo Dinner-ovvero otteniamo completi IntelliSense e per tale controllo all'interno di blocchi di codice in fase di compilazione:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Quando facciamo clic su Aggiorna */Dinners* URL nel browser la vista aggiornata avrà l'aspetto simile al seguente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

Questo è un aspetto migliore, ma non è ancora completamente predisposto a questo. L'ultimo passaggio è consentire agli utenti finali di fare clic su singoli Dinners nell'elenco e visualizzare informazioni dettagliate in proposito. In questo implementeremo eseguendo il rendering di elementi di collegamento ipertestuale HTML che si collegano al metodo di azione dettagli sul nostro DinnersController.

È possibile generare questi collegamenti ipertestuali entro la visualizzazione dell'indice in uno dei due modi. La prima consiste nel creare manualmente HTML &lt;una&gt; elementi come il seguente, in cui si incorpora &lt;% %&gt; blocca all'interno di &lt;un&gt; elemento HTML:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

È un approccio alternativo, è possibile usare per sfruttare il metodo helper "Html.ActionLink()" incorporato all'interno di ASP.NET MVC che supporta la creazione a livello di codice HTML &lt;un&gt; elemento che si collega a un altro metodo di azione in un Controller:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

Il primo parametro al metodo helper Html.ActionLink() è il testo di collegamento da visualizzare (in questo caso il titolo della cena), il secondo parametro è il nome Controller dell'azione si desidera generare il collegamento per (in questo caso, il metodo dettagli) e il terzo parametro è un set di parametri da inviare all'azione (implementata come tipo anonimo con nome/valori delle proprietà). In questo caso viene specificato il parametro "id" dinner si desidera collegare e perché il routing degli URL predefinito di regole in ASP.NET MVC è "{Controller} / {Action} / {id}" il metodo helper Html.ActionLink() genererà l'output seguente:

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

Per la nostra visualizzazione index. aspx verrà usata l'approccio di metodo helper Html.ActionLink() e avere ogni dinner nel collegamento elenco all'URL di dettagli appropriati:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

E ora quando viene raggiunto il */Dinners* URL elenco dinner simile al seguente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Quando si fa clic su uno qualsiasi dei Dinners nell'elenco viene visualizzata per visualizzarne i dettagli:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Basato sulle convenzioni di denominazione e la struttura di directory \Views

Applicazioni ASP.NET MVC per impostazione predefinita usano una directory basata sulle convenzioni di denominazione struttura per la risoluzione dei modelli di vista. Ciò consente agli sviluppatori di evitare di dover completi un location path quando si fa riferimento a viste all'interno di una classe Controller. Per impostazione predefinita ASP.NET MVC cercherà il file di modello di visualizzazione all'interno di * \Views\[ControllerName]\* directory sotto l'applicazione.

Ad esempio, questo abbiamo lavorato sulla classe DinnersController – che contiene riferimenti espliciti a tre modelli di visualizzazione: "Index", "Dettagli" e "NotFound". ASP.NET MVC per impostazione predefinita cerca tali viste all'interno di *\Views\Dinners* directory sotto la directory radice dell'applicazione:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Si noti che in precedenza come non vi sono attualmente tre classi controller all'interno del progetto (DinnersController, HomeController e AccountController – gli ultimi due sono stati aggiunti per impostazione predefinita, quando abbiamo creato il progetto), e non vi sono tre le sottodirectory (uno per ogni controller) all'interno della directory \Views.

Viste a cui fa riferimento il controller Home e gli account si risolveranno automaticamente i relativi modelli di visualizzazione dalla rispettiva *\Views\Home* e *\Views\Account* le directory. Il *\Views\Shared* sottodirectory fornisce un modo per archiviare i modelli di visualizzazione che vengono riutilizzati tra più controller all'interno dell'applicazione. Quando ASP.NET MVC tenta di risolvere un modello di vista, verrà controllato prima di tutto all'interno di *\Views\[Controller]* directory specifica, e se non viene trovato il modello di vista si esaminerà il *\Views\ Condiviso* directory.

Quando si parla di denominazione dei modelli di visualizzazione singoli, la procedura consigliata è affinché il modello di vista condividono lo stesso nome del metodo di azione che ha causato il rendering. Ad esempio, sopra la "Index" metodo di azione Usa la visualizzazione "Index" per eseguire il rendering il risultato della visualizzazione e il metodo di azione "Dettagli" Usa la visualizzazione "Dettagli" per eseguire il rendering i relativi risultati. Questo rende facile capire rapidamente quale modello è associato a ogni azione.

Gli sviluppatori non sono necessario specificare in modo esplicito il nome del modello di visualizzazione quando il modello di visualizzazione ha lo stesso nome del metodo di azione da richiamare nel controller. È possibile invece solo passare l'oggetto modello al metodo helper "View()" (senza specificare il nome della vista) e ASP.NET MVC verrà automaticamente dedotto che si desidera usare il *\Views\[ControllerName]\[ActionName]* modello di vista su disco per eseguirne il rendering.

Ciò consente di pulire un po' il codice del controller, quindi evitare di duplicare il nome due volte nel nostro codice:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

Il codice sopra riportato è tutto ciò che serve per implementare un utile elenco/dettagli Dinner una migliore esperienza per il sito.

#### <a name="next-step"></a>Passo successivo

È ora disponibile un interessante Dinner compilato esperienza di esplorazione.

È possibile abilitare form dati CRUD (Create, Read, Update, Delete) supporto per la modifica.

> [!div class="step-by-step"]
> [Precedente](build-a-model-with-business-rule-validations.md)
> [Successivo](provide-crud-create-read-update-delete-data-form-entry-support.md)
