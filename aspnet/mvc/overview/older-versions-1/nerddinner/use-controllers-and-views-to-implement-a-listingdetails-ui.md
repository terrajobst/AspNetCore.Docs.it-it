---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: Usare controller e visualizzazioni per implementare un'interfaccia utente/dettagli | Documenti Microsoft
author: microsoft
description: Passaggio 4 viene illustrato come aggiungere un Controller per l'applicazione che si avvale del modello per fornire agli utenti un'esperienza di esplorazione dati elenco o dettagli...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: ac3568941eeef24bd9857c5787471aadea15fc7f
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2018
ms.locfileid: "30875734"
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>Usare controller e visualizzazioni per implementare un'interfaccia utente/dettagli
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Si tratta di passaggio 4 di una liberazione [esercitazione applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-through come compilare un piccolo, ma completa, un'applicazione web con ASP.NET MVC 1.
> 
> Passaggio 4 viene illustrato come aggiungere un Controller per l'applicazione che si avvale del modello per fornire agli utenti un'esperienza di navigazione/dettagli della pubblicazione di dati per dinners sono disponibili sul sito NerdDinner.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire il [recupero avviato con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [negozio MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner passaggio 4: Controller e visualizzazioni

Con il framework di web tradizionale (ASP classico, Web Form ASP.NET, PHP e così via), gli URL in ingresso vengono in genere associati ai file su disco. Ad esempio: una richiesta per un URL come "/ Products" o "/ Products.php" possono essere elaborate da un file "Products" o "Products.php".

Framework MVC basata sul Web di eseguire il mapping degli URL al codice server in modo leggermente diverso. Anziché eseguire il mapping degli URL in ingresso ai file, gli URL sono invece associati ai metodi nelle classi. Queste classi vengono chiamate "Controller" e sono responsabili per l'elaborazione delle richieste HTTP in ingresso, gestione dell'input dell'utente, il recupero e il salvataggio dei dati e determinare la risposta da inviare al client (visualizzazione del contenuto HTML, scaricare un file, il reindirizzamento a un altro URL e così via).

Ora che è stato compilato un modello di base per l'applicazione NerdDinner, il passaggio successivo sarà per aggiungere un Controller per l'applicazione che si avvale di esso per fornire agli utenti un'esperienza di navigazione/dettagli della pubblicazione di dati per Dinners sono disponibili sul sito.

### <a name="adding-a-dinnerscontroller-controller"></a>Aggiunta di un Controller DinnersController

Verrà innanzitutto facendo clic sulla cartella "Controller" all'interno del progetto web e quindi selezionare il **Add -&gt;Controller** comando di menu (è possibile eseguire anche questo comando, digitando Ctrl + M, Ctrl + C):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

Verrà visualizzata la finestra di dialogo "Aggiungi Controller":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Viene denominare il nuovo controller "DinnersController" e fare clic sul pulsante "Aggiungi". Visual Studio aggiungerà quindi un file DinnersController.cs sotto la directory \Controllers:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

Verrà inoltre aperta la nuova classe DinnersController all'interno dell'editor di codice.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>Aggiunta di Index () e i metodi di azione Details() alla classe DinnersController

È necessario abilitare i visitatori che utilizzano l'applicazione per sfogliare un elenco di dinners future e consentire loro di fare clic su qualsiasi Dinner nell'elenco per visualizzare dettagli specifici. Faremo pubblicando l'URL seguente dall'applicazione:

| **URL** | **Scopo** |
| --- | --- |
| */Dinners/* | Visualizzare un elenco HTML di dinners future |
| */Dinners/dettagli / [id]* | Visualizzare informazioni dettagliate su una cena specifica indicato da un parametro "id" incorporato all'interno dell'URL, che consente la corrispondenza DinnerID di dinner nel database. Ad esempio: /Dinners/Details/2 Visualizza una pagina HTML con le informazioni di Dinner il cui valore DinnerID è 2. |

Le implementazioni iniziale di questi URL verranno pubblicate aggiungendo due pubblico "metodi di azione" per la classe DinnersController come di seguito:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

È quindi sarà eseguire l'applicazione NerdDinner e utilizzare il visualizzatore per richiamarli. Digitando il *"Dinners /"* URL causerà il nostro *Index ()* metodo da eseguire e si invierà la risposta seguente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

Digitando il *"/ Dettagli/Dinners/2"* URL causerà il nostro *Details()* metodo per eseguire e restituire la risposta seguente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

È possibile chiedersi - come ASP.NET MVC sapere come creare la classe DinnersController e richiamare tali metodi? Tenere presente che si esaminare rapidamente il funzionamento del routing.

### <a name="understanding-aspnet-mvc-routing"></a>Routing comprensione ASP.NET MVC

ASP.NET MVC include un potente motore di routing di URL che fornisce una notevole flessibilità per controllare la modalità di mapping degli URL alle classi controller. Consente di personalizzare completamente come ASP.NET MVC sceglie la classe controller da creare, il metodo da richiamare su di esso, nonché di configurare diversi modi in cui variabili possono essere analizzate dall'URL o la stringa di query e passate come parametro al metodo automaticamente argomenti. Offre la flessibilità necessaria per completamente un sito di ottimizzare per SEO (Ottimizzazione motore di ricerca), nonché qualsiasi struttura di URL che vogliamo da un'applicazione di pubblicazione.

Per impostazione predefinita, i nuovi progetti ASP.NET MVC vengono forniti con un set preconfigurato di regole di routing URL già registrato. Ciò consente di avviare rapidamente e facilmente in un'applicazione senza dover configurare in modo esplicito un valore. Le registrazioni regola di routing predefinita sono reperibile all'interno della classe "Applicazione" dei nostri progetti, che è possibile aprire facendo doppio clic sul file "Global. asax" nella radice del progetto:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

Le regole di routing di ASP.NET MVC predefinite sono registrate all'interno del metodo "RegisterRoutes" di questa classe:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

Le route". MapRoute() "precedente chiamata al metodo registra una regola di routing predefinita che viene eseguito il mapping degli URL in ingresso alle classi controller utilizzando il formato di URL:" / {controller} / {action} / {id} ", dove"controller"è il nome della classe controller per creare un'istanza,"azione"è il nome di un metodo pubblico da richiamare su di esso e "id" è un parametro facoltativo incorporato all'interno dell'URL che può essere passato come argomento al metodo. Il terzo parametro passato alla chiamata al metodo "MapRoute()" è un set di valori predefiniti da utilizzare per i valori di id/azione/controller nel caso in cui non sono presenti nell'URL (Controller = "Home" Action = "Index", Id = "").

Di seguito è una tabella in cui viene illustrato come un'ampia gamma di URL vengono mappati mediante il valore predefinito "<em>/ {controller} / {action} / {id}"</em>regola route:

| **URL** | **Classe controller** | **Metodo di azione** | **Parametri passati** |
| --- | --- | --- | --- |
| */Dinners/Details/2* | DinnersController | Details(ID) | id=2 |
| */Dinners/Edit/5* | DinnersController | Edit(ID) | id=5 |
| */Dinners/Create* | DinnersController | Metodo di creazione | N/D |
| */ Dinners* | DinnersController | Index) | N/D |
| */Home* | HomeController | Index) | N/D |
| */* | HomeController | Index) | N/D |

Le ultime tre righe mostrano i valori predefiniti (Controller Home, = azione = indice, Id = "") in uso. Poiché il metodo "Index" viene registrato come il nome dell'azione predefinita se non è specificata, il "/ Dinners" e "/ Home" causa il metodo di azione Index () gli URL da richiamare per le classi Controller. Dal momento che il controller "Home" è registrato come controller predefinito se non è specificata, l'URL "/" genera HomeController deve essere creato e il metodo di azione Index () in modo da essere richiamato.

Se non si desidera che queste regole di routing URL predefinito, buone notizie sono che sono facili da modificare - modificarle solo all'interno del metodo RegisterRoutes precedente. Per l'applicazione NerdDinner, tuttavia, si propone di modificare le regole di routing l'URL predefinito: invece solo utilizzeremo come-è.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>Utilizzo di DinnerRepository dal nostro DinnersController

Ora sostituire l'implementazione corrente del DinnersController dei metodi di azione Index () e Details() con le implementazioni che utilizzano il modello.

Verrà utilizzata la classe DinnerRepository è compilato in precedenza per implementare il comportamento. Verrà inizia ad aggiungere un'istruzione "using" che fa riferimento lo spazio dei nomi "NerdDinner.Models" e quindi dichiarare un'istanza del nostro DinnerRepository come campo nella classe DinnerController.

Più avanti in questo capitolo viene introdotto il concetto di "Dependency Injection" e Mostra un altro modo per il controller ottenere un riferimento a un DinnerRepository che consente una migliore unit test, ma per destra ora ci limiteremo a creare un'istanza del nostro DinnerRepository inline come di seguito.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

Si è ora pronti per generare una risposta HTML con gli oggetti modello di dati recuperati.

### <a name="using-views-with-our-controller"></a>Utilizzo delle viste con il Controller

Sebbene sia possibile scrivere codice all'interno di assemblare HTML e quindi utilizzare i metodi di azione di *Response* metodo helper per inviare al client, tale approccio diventa rapidamente piuttosto difficile da gestire. Un approccio migliore è riservata per eseguire solo l'applicazione e la logica di dati all'interno di metodi di azione il nostro DinnersController e quindi passare i dati necessari per eseguire il rendering di una risposta HTML a un modello di "visualizzazione" separato è responsabile per l'output la rappresentazione HTML di esso. Come verrà illustrato più avanti, un modello "view" è un file di testo che contiene in genere una combinazione di markup HTML e codice incorporato per il rendering.

Separare la logica di controller da eseguito il rendering della visualizzazione offre diversi vantaggi. In particolare consente di applicare una netta "separazione delle problematiche" tra il codice dell'applicazione e il codice di formattazione/rendering dell'interfaccia utente. Questo rende molto più semplice per logica dell'applicazione unit test in modalità isolata dalla logica di rendering dell'interfaccia utente. Rende più semplice in un secondo momento modificare i modelli per il rendering dell'interfaccia utente senza la necessità di apportare modifiche al codice dell'applicazione. E rende più semplice per sviluppatori e progettisti di collaborare su progetti.

È possibile aggiornare la classe DinnersController per indicare che si desidera utilizzare un modello di visualizzazione per la restituzione di una risposta dell'interfaccia utente HTML modificando le firme del metodo i due metodi di azione dalla presenza di un tipo restituito "void" invece di avere un tipo restituito "ActionResult". È quindi possibile chiamare il *View()* metodo helper nella classe base Controller per restituire un oggetto "ViewResult" come il seguente:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

La firma del *View()* metodo helper viene usato in precedenza è simile di sotto:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

Il primo parametro di *View()* metodo di supporto è il nome del file di modello di visualizzazione si desidera utilizzare per eseguire il rendering della risposta HTML. Il secondo parametro è un oggetto modello che contiene i dati necessarie per il modello di visualizzazione per eseguire il rendering della risposta HTML.

All'interno di questo metodo di azione Index () venga chiamato il *View()* metodo di supporto e che indica che si desidera eseguire il rendering di un elenco HTML di dinners utilizzando un modello di visualizzazione "Index". Il modello di visualizzazione viene passato una sequenza di oggetti Dinner per generare l'elenco da:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

All'interno del metodo di azione di Details() si tenta di recuperare un oggetto Dinner usando l'id specificato nell'URL. Se viene trovata una cena valida è definito il *View()* metodo di supporto, che indica di voler utilizzare un modello di visualizzazione "Dettagli" per il rendering dell'oggetto Dinner recuperato. Se viene richiesto un dinner non valido, è il rendering di un messaggio di errore utile che indica che la cena non esiste un modello di visualizzazione "NotFound" (e una versione di overload di *View()* metodo helper che accetta solo il nome del modello ):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

Verrà ora implementata i modelli di visualizzazione "NotFound", "Dettagli" e "Index".

### <a name="implementing-the-notfound-view-template"></a>Implementare il modello di visualizzazione "NotFound"

Inizieremo implementando il modello di visualizzazione: "NotFound" che visualizza un messaggio di errore descrittivo che indica che è Impossibile trovare la cena richiesta.

È sarà creare un nuovo modello di visualizzazione posizionando il cursore di testo all'interno di un metodo di azione del controller, quindi fare clic e scegliere il comando di menu "Aggiungi visualizzazione" (è anche possibile eseguire a questo comando, digitando Ctrl + M, Ctrl + V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Verrà visualizzata una finestra di dialogo "Aggiungi vista" come di seguito. Per impostazione predefinita che la finestra di dialogo verrà pre-popolare il nome della vista da creare in base al nome del metodo di azione il cursore è stato quando la finestra di dialogo è stata avviata (in questo caso "Dettagli"). Poiché si desidera implementare il modello "NotFound", viene eseguire l'override di questo nome di visualizzazione e impostarlo come invece "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Quando si fa clic sul pulsante "Aggiungi", Visual Studio creerà un nuovo modello di visualizzazione "NotFound.aspx" per noi all'interno della directory "\Views\Dinners" (che vengono create anche se la directory non esiste già):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

Verrà aperto anche il nuovo modello di visualizzazione "NotFound.aspx" all'interno dell'editor di codice:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

Per impostazione predefinita i modelli di visualizzazione hanno due "contenuto" in cui è possibile aggiungere contenuto e codice. Il primo consente di personalizzare il "title" della pagina HTML inviato nuovamente. Il secondo consente di personalizzare il "contenuto principale" della pagina HTML inviato nuovamente.

Per implementare il modello di visualizzazione "NotFound" verrà aggiunto alcuni contenuti di base:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

È quindi possibile eseguire la verifica all'interno del browser. Per eseguire questa operazione verrà richiesta la *"/ Dinners/dettagli/9999"* URL. Si farà riferimento a una cena che attualmente non esiste nel database e causerà un metodo di azione DinnersController.Details() eseguire il rendering di modelli di visualizzazione "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

Si noterà che nella cattura di schermata sopra è che il modello di visualizzazione di base è ereditata una serie di HTML che racchiude il contenuto principale sullo schermo. Questo avviene perché il modello di visualizzazione è un modello "pagina master" che consente di applicare un layout coerenza in tutte le visualizzazioni nel sito. Si parlerà di funzionamento delle pagine master altre in una parte di questa esercitazione successiva.

### <a name="implementing-the-details-view-template"></a>Implementare il modello di visualizzazione "Dettagli"

Verrà ora implementata il modello di visualizzazione "Dettagli" – che genererà HTML per un singolo modello Dinner.

Si verrà eseguire questa operazione posizionando il cursore di testo all'interno del metodo di azione, i dettagli e quindi fare clic e scegliere il comando di menu "Aggiungi visualizzazione" (o premere Ctrl + M, Ctrl + V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

Verrà visualizzata la finestra di dialogo "Aggiungi visualizzazione". Mantenere il nome di visualizzazione predefinito ("Dettagli"). È anche selezionare la casella di controllo "Crea visualizzazione fortemente tipizzata" nella finestra di dialogo e selezionare (utilizzando l'elenco a discesa combobox) il nome del tipo di modello che viene passato dal Controller alla visualizzazione. Per questa vista viene passato un oggetto Dinner (il nome completo per questo tipo è: "NerdDinner.Models.Dinner"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

A differenza del modello precedente, in cui è stato scelto di creare una visualizzazione"vuoto", verrà scelto automaticamente al momento "lo scaffolding" la vista usando un modello "Dettagli". Modificando l'elenco a discesa "Visualizza contenuto" nella finestra di dialogo precedente, è possibile indicare tale condizione.

"Lo scaffolding" genererà un'implementazione iniziale di questo modello di visualizzazione dettagli in base all'oggetto Dinner che viene passato a esso. Ciò fornisce un modo semplice per iniziare rapidamente sull'implementazione di modelli di visualizzazione.

Quando si fa clic sul pulsante "Aggiungi", Visual Studio creerà un nuovo file di modello di visualizzazione "Details" automaticamente entro la directory "\Views\Dinners":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

Inoltre viene visualizzato il nuovo modello di visualizzazione "Details" all'interno dell'editor di codice. Contiene un'implementazione di pagina iniziale di una visualizzazione di dettagli in base a un modello Dinner. Il motore di scaffolding utilizza la reflection .NET per esaminare le proprietà pubbliche esposte nella classe passata e verrà aggiunto il contenuto appropriato in base a ogni tipo che viene trovata:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

È possibile richiedere il *"/ Dettagli/Dinners/1"* URL per visualizzare l'aspetto questa implementazione scaffold "Dettagli" nel browser. Con questo URL verrà visualizzato uno del dinners che è aggiunto manualmente al database quando si crea:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

Questo ci Ottiene rapidamente e fornisce un'implementazione iniziale della vista nostri Details. È quindi possibile passare e modificarlo per personalizzare l'interfaccia utente per la soddisfazione.

Quando si esamina il modello Details più da vicino, verrà individuato che contiene codice HTML statico, nonché il codice per il rendering incorporato. &lt;% %&gt; parti di codice eseguire codice quando il modello di visualizzazione esegue il rendering, e &lt;% = %&gt; parti di codice eseguire il codice contenuto all'interno di essi e quindi eseguire il rendering il risultato nel flusso di output del modello.

La visualizzazione che accede all'oggetto modello "Dinner" che è stato passato da questo controller utilizzando una proprietà di "Modello" fortemente tipizzata, è possibile scrivere codice. Visual Studio offre con intellisense di codice completo quando si accede a questa proprietà "Modello" all'interno dell'editor:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

Verifichiamo alcune modifiche di entità in modo che l'origine per il modello di visualizzazione dei dettagli finale simile sotto:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Quando si accede di *"/ Dettagli/Dinners/1"* URL nuovamente verrà ora eseguire il rendering come il seguente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>Implementare il modello di visualizzazione "Index"

Verrà ora implementata il modello di visualizzazione "Index", che genera un elenco di Dinners future. Attività, questo verrà posizionare il cursore di testo all'interno del metodo di azione di indice e quindi a destra fare clic su e scegliere il comando di menu "Aggiungi visualizzazione" (o premere Ctrl + M, Ctrl + V).

Nella finestra di dialogo "Aggiungi visualizzazione" viene mantenere il modello di visualizzazione denominato "Index" e selezionare la casella di controllo "Crea una visualizzazione fortemente tipizzata". Questa volta si sceglierà automaticamente generare un modello di visualizzazione "List" e selezionare "NerdDinner.Models.Dinner" come tipo di modello passato alla visualizzazione (cui perché è stato indicato che verrà creato un "elenco" scaffold causerà la finestra di dialogo Aggiungi visualizzazione presuppongono che si siano passaggio di una sequenza di oggetti di Dinner da questo Controller alla visualizzazione):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Quando si fa clic sul pulsante "Aggiungi", Visual Studio creerà un nuovo file di modello di visualizzazione "Index" automaticamente entro la directory "\Views\Dinners". Sarà "lo scaffolding" implementazione iniziale all'interno che fornisce un elenco delle tabelle HTML del Dinners è passare alla visualizzazione.

Quando si esegue l'accesso alle applicazioni e il *"Dinners /"* URL esegue il rendering di lista di dinners come illustrato di seguito:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

La soluzione nella tabella precedente offre un layout di griglia di dati relativi al Dinner: non sono abbastanza auspicabile per i consumer affiancate listato Dinner. È possibile aggiornare il modello di visualizzazione Index.aspx e modificarlo per elencare le colonne di un numero inferiore di dati e utilizzare un &lt;ul&gt; elemento per eseguirne il rendering anziché una tabella utilizzando il codice riportato di seguito:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Si utilizza la parola chiave "var" all'interno dell'istruzione foreach precedente come il ciclo su ciascuna etichetta nel modello in esame. Familiarità con c# 3.0 quelli potrebbe pensare che l'utilizzo di "var" indica che l'oggetto dinner sia ad associazione tardiva. In alternativa, significa che il compilatore utilizza l'inferenza del tipo in base alla proprietà "Modello" fortemente tipizzata (che è di tipo "IEnumerable&lt;Dinner&gt;") e la compilazione la variabile locale "dinner" come un tipo Dinner, pertanto è completi IntelliSense e cercare all'interno di blocchi di codice in fase di compilazione:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Quando viene raggiunto l'aggiornamento di */Dinners* URL nel browser la visualizzazione aggiornata avrà l'aspetto come il seguente:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

Questa ricerca migliore: ma non è ancora completamente non esiste. L'ultimo passaggio consiste nell'abilitare gli utenti finali fare clic su Dinners singoli nell'elenco e visualizzare i dettagli su di essi. Questo verrà implementato mediante il rendering HTML di un collegamento ipertestuale che collega al metodo di azione dettagli nel nostro DinnersController.

È possibile generare questi collegamenti ipertestuali entro la visualizzazione dell'indice in uno dei due modi. La prima consiste nel creare manualmente HTML &lt;un&gt; elementi come il seguente, in cui si incorpora &lt;% %&gt; blocchi all'interno di &lt;un&gt; elemento HTML:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

È un approccio alternativo, è possibile usare per sfruttare il metodo di supporto "Html.ActionLink()" incorporato all'interno di MVC ASP.NET che supporta la creazione a livello di codice HTML &lt;un&gt; elemento che si collega a un altro metodo di azione in un Controller:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

Il primo parametro al metodo di supporto Html.ActionLink() è il testo di collegamento da visualizzare (in questo caso il titolo della cena), il secondo parametro è il nome del Controller azione si desidera generare il collegamento per (in questo caso, il metodo dettagli) e il terzo parametro è un set di parametri da inviare all'azione (implementato come un tipo anonimo con proprietà nome/valore). In questo caso ci stiamo specificando il parametro di "id" di dinner si desidera effettuare il collegamento e poiché il routing degli URL predefinito di regole in ASP.NET MVC è "{Controller} / {Action} / {id}" il metodo di supporto Html.ActionLink() verrà generato il seguente output:

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

Per la visualizzazione Index.aspx verrà usata l'approccio di metodo di supporto Html.ActionLink() e dispone di ciascuna etichetta nel collegamento elenco all'URL di dettagli appropriati:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

E ora quando viene raggiunto il */Dinners* lista dinner è simile di sotto di URL:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Quando si fa clic su uno qualsiasi dei Dinners nell'elenco verrà esplorazione per visualizzare i dettagli su di esso:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Basato sulle convenzioni di denominazione e la struttura di directory \Views

Per impostazione predefinita le applicazioni ASP.NET MVC utilizzano una directory basato sulle convenzioni di denominazione struttura per la risoluzione dei modelli di visualizzazione. Ciò consente agli sviluppatori di evitare di dover completi un percorso quando si fa riferimento a viste appartenenti all'interno di una classe Controller. Per impostazione predefinita ASP.NET MVC cercherà il file di modello di visualizzazione all'interno di * \Views\[ControllerName]\* directory sotto l'applicazione.

Ad esempio, si abbiamo lavorato nella classe DinnersController – che fa riferimento in modo esplicito a tre modelli di visualizzazione: "Index", "Dettagli" e "NotFound". ASP.NET MVC per impostazione predefinita cercherà tali viste all'interno di *\Views\Dinners* directory sotto la directory radice dell'applicazione:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Si noti in precedenza come non sono attualmente tre classi controller all'interno del progetto (DinnersController, HomeController e AccountController: gli ultimi due sono stati aggiunti per impostazione predefinita, quando abbiamo creato il progetto), e non vi sono tre sottodirectory (uno per ogni controller) all'interno della directory \Views.

Viste a cui fa riferimento il controller Home e gli account si risolveranno automaticamente i modelli di visualizzazione dalla rispettiva *\Views\Home* e *\Views\Account* directory. Il *\Views\Shared* sottodirectory fornisce un modo per archiviare i modelli di visualizzazione che vengono riutilizzati in più controller all'interno dell'applicazione. Quando ASP.NET MVC tenta di risolvere un modello di visualizzazione, dapprima il controllo all'interno di *\Views\[Controller]* directory specifica, e se non si trova il modello di visualizzazione si esaminerà il *\Views\ Condiviso* directory.

Quando si tratta di denominazione dei modelli di visualizzazione, la procedura consigliata è che il modello di visualizzazione condividono lo stesso nome del metodo di azione che ha causato il rendering. Ad esempio, sopra il nostro "Index" metodo di azione utilizza la vista "Index" per il rendering di risultato della visualizzazione e il metodo di azione "Dettagli" Usa "Dettagli" per eseguire il rendering i risultati. Questo semplifica visualizzare rapidamente il modello è associato a ogni azione.

Gli sviluppatori non è necessario specificare in modo esplicito il nome del modello di visualizzazione quando il modello di visualizzazione ha lo stesso nome del metodo di azione nel controller di cui viene richiamato. È possibile invece solo passare l'oggetto modello al metodo di supporto "View()" (senza specificare il nome della vista) e ASP.NET MVC dedurrà automaticamente che si desidera utilizzare il *\Views\[ControllerName]\[ActionName]* il modello di visualizzazione sul disco per eseguirne il rendering.

Ciò consente di pulire un po' il codice del controller ed evitare la duplicazione due volte nel codice:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

Il codice sopra riportato è tutto ciò che è necessario per implementare un nice Dinner elenco o dettagli esperienza per il sito.

#### <a name="next-step"></a>Passo successivo

È ora disponibile una cena nice compilato esperienza di esplorazione.

Verrà ora la maschera dati CRUD (Create, Read, Update, Delete) supporto per la modifica.

> [!div class="step-by-step"]
> [Precedente](build-a-model-with-business-rule-validations.md)
> [Successivo](provide-crud-create-read-update-delete-data-form-entry-support.md)
