---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: Fornire CRUD (Create, leggere, aggiornare ed eliminare) dati formano supporto voce | Documenti Microsoft
author: microsoft
description: Passaggio 5 di seguito viene illustrato come richiedere la classe DinnersController ulteriormente da abilitare il supporto per la modifica, la creazione e l'eliminazione anche di Dinners con esso.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: bd906282db5c620476966ffbe09cecb5ade66ee4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876748"
---
<a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>Fornire CRUD (Create, leggere, aggiornare ed eliminare) supporto voce Form dati
====================
by [Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 5 di una liberazione [esercitazione applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-through come compilare un piccolo, ma completa, un'applicazione web con ASP.NET MVC 1.
> 
> Passaggio 5 di seguito viene illustrato come richiedere la classe DinnersController ulteriormente da abilitare il supporto per la modifica, la creazione e l'eliminazione anche di Dinners con esso.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire il [recupero avviato con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [negozio MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner passaggio 5: Creare, aggiornare ed eliminare gli scenari di Form

È stato introdotto controller e visualizzazioni e trattati come utilizzarle per implementare un'esperienza di elenco o dettagli per Dinners nel sito. Il passaggio successivo sarà possibile eseguire un'ulteriore classe DinnersController e abilitare il supporto per la modifica, la creazione e l'eliminazione anche di Dinners con esso.

### <a name="urls-handled-by-dinnerscontroller"></a>URL gestito da DinnersController

Metodi di azione DinnersController che implementato il supporto per due URL viene aggiunto in precedenza: */Dinners* e */Dinners/dettagli / [id]*.

| **URL** | **VERB** | **Scopo** |
| --- | --- | --- |
| */Dinners/* | GET | Visualizzare un elenco HTML di dinners future. |
| */Dinners/Details/[id]* | GET | Visualizzare informazioni dettagliate su una cena specifica. |

A questo punto si aggiungeranno i metodi di azione per implementare tre ulteriori URL: <em>/Dinners/modifica / [id], Dinners/Create,</em>e<em>/Dinners/Delete / [id]</em>. Questi URL verranno abilitare il supporto per modifica Dinners esistente, creazione nuova Dinners ed eliminazione Dinners.

Interazioni di verbo HTTP GET e HTTP POST con questi nuovi URL saranno supportati. Le richieste HTTP GET di questi URL verranno visualizzato visualizzazione HTML iniziale dei dati (un modulo compilato con i dati di Dinner nel caso di "Modifica", un modulo vuoto in caso di "creazione" e una schermata di conferma eliminazione nel caso di "delete"). Le richieste HTTP POST di questi URL verranno salvare, aggiornare o eliminare i dati di Dinner nel nostro DinnerRepository (e da lì al database).

| **URL** | **VERB** | **Scopo** |
| --- | --- | --- |
| */Dinners/Edit/[id]* | GET | Visualizzare un form HTML modificabile popolato con dati Dinner. |
| INSERISCI | Salvare le modifiche di form per un particolare Dinner al database. |
| */Dinners/Create* | GET | Visualizzare un form HTML vuoto che consente agli utenti di definire nuove Dinners. |
| INSERISCI | Creare un nuovo Dinner e salvarlo nel database. |
| */Dinners/Delete/[id]* | GET | Visualizza eliminazione schermata di conferma. |
| INSERISCI | Elimina la cena specificata dal database. |

### <a name="edit-support"></a>Supporto di modifica

Si inizierà dall'implementazione dello scenario "Modifica".

#### <a name="the-http-get-edit-action-method"></a>Il metodo di azione di modifica HTTP-GET

Si inizierà dall'implementazione del metodo di azione di modifica comportamento HTTP "GET". Questo metodo verrà richiamato quando il */Dinners/modifica / [id]* URL è richiesto. L'implementazione avrà un aspetto simile:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

Il codice precedente utilizza il DinnerRepository per recuperare un oggetto Dinner. Esegue quindi il rendering di un modello di visualizzazione utilizzando l'oggetto Dinner. Poiché non è possibile passato in modo esplicito un nome modello per il *View()* metodo di supporto, userà il percorso predefinito basato sulle convenzioni per risolvere il modello di visualizzazione: /Views/Dinners/Edit.aspx.

Creare ora il modello di visualizzazione. Si eseguirà facendo clic all'interno del metodo di modifica e selezionando il comando di menu "Aggiungi visualizzazione" contesto:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

Nella finestra di dialogo "Aggiungi visualizzazione" si verrà indica che si passa un oggetto di Dinner a questo modello di visualizzazione come il modello e sceglie di auto-scaffold un modello "Edit":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Quando si fa clic sul pulsante "Aggiungi", Visual Studio aggiungerà un nuovo file di modello di visualizzazione "Edit" per noi all'interno della directory "\Views\Dinners". Inoltre viene visualizzato il nuovo modello di visualizzazione "Edit" all'interno del codice editor – popolato con un'iniziale "Modifica" scaffold implementazione come di seguito:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

Di seguito apportare alcune modifiche per il valore predefinito "edit" lo scaffolding generato e aggiornare il modello di visualizzazione di modifica per il contenuto (che consente di rimuovere alcune delle proprietà che non si desidera esporre):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Quando si esegue la richiesta e l'applicazione di *"/ Modifica/Dinners/1"* URL è verrà visualizzata la pagina seguente:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

Il markup HTML generato per la visualizzazione è simile di sotto. È HTML standard, con un &lt;modulo&gt; elemento che esegue una richiesta POST HTTP al */Dinners/Edit/1* URL quando "Salva" &lt;input di tipo = "submit" /&gt; pulsante è premuto. HTML &lt;input di tipo = "text" /&gt; elemento è stato output per ogni proprietà modificabili:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Html.BeginForm() e metodi Html Helper Html.TextBox()

Il modello di visualizzazione "Edit" utilizza diversi metodi di "Helper Html": Html.ValidationSummary(), Html.BeginForm(), Html.TextBox() e Html.ValidationMessage(). Oltre a generare il markup HTML per noi, questi metodi di supporto forniscono convalida e gestione degli errori incorporato supporta.

##### <a name="htmlbeginform-helper-method"></a>Metodo helper Html.BeginForm()

Il metodo di supporto Html.BeginForm() è l'output HTML &lt;modulo&gt; elemento nel nostro codice. Nel nostro modello di visualizzazione Edit si noterà che viene applicata un'istruzione "using" quando si utilizza questo metodo c#. La parentesi graffa aperta indica l'inizio del &lt;modulo&gt; contenuto e la parentesi graffa di chiusura è ciò che indica la fine del &lt;/form&gt; elemento:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

In alternativa, se l'istruzione "using" approccio non naturale per uno scenario simile al seguente, è possibile utilizzare una combinazione di Html.BeginForm() e Html.EndForm() (che esegue la stessa operazione):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

La chiamata a Html.BeginForm() senza parametri determinerà l'output di un elemento di modulo che esegue un POST HTTP all'URL della richiesta corrente. Vale a dire perché la visualizzazione di modifica genera un *&lt;azione modulo = "/ Modifica/Dinners/1" metodo = "post"&gt;* elemento. È stato possibile in alternativa superati parametri espliciti per Html.BeginForm() se si desidera inviare a un URL diverso.

##### <a name="htmltextbox-helper-method"></a>Metodo helper Html.TextBox()

La vista Edit viene utilizzato il metodo di supporto Html.TextBox() per generare &lt;input di tipo = "text" /&gt; elementi:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

Metodo Html.TextBox() accetta un solo parametro – utilizzato per specificare entrambi gli attributi di nome/id il &lt;input di tipo = "text" /&gt; elemento di output, nonché la proprietà del modello per popolare il valore della casella di testo da. Ad esempio, l'oggetto Dinner è passati per la visualizzazione di modifica contiene un valore di proprietà "Title" di ".NET future" e pertanto il metodo Html.TextBox("Title") chiamare output: *&lt;input con id = "Title" nome = "Title" type = "text" value = ".NET future" /&gt;*.

In alternativa, è possibile utilizzare il primo parametro Html.TextBox() per specificare il nome o un id dell'elemento e quindi passare in modo esplicito nel valore da usare come secondo parametro:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

Spesso è opportuno eseguire la formattazione personalizzata per il valore di output. Il metodo statico String integrata in .NET è utile per questi scenari. Il modello di visualizzazione Edit utilizza questo per formattare il valore EventDate (che è di tipo DateTime) in modo che non visualizza i secondi per il periodo di tempo:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Un terzo parametro Html.TextBox() consente, facoltativamente, per restituire gli attributi HTML aggiuntivi. Nel frammento di codice seguente viene illustrato come eseguire il rendering di una dimensione aggiuntiva = attributo "30" e una classe = "mycssclass" attributo on il &lt;input di tipo = "text" /&gt; elemento. Si noti come ci stiamo escape il nome dell'attributo della classe utilizzando un "@" character because "classe" è una parola chiave riservata in c#:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>Implementazione del metodo di azione di modifica HTTP-POST

È ora disponibile la versione HTTP-GET del metodo di azione di modifica implementata. Quando un utente richiede la */Dinners/Edit/1* ricevono una pagina HTML simile al seguente URL:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

Premendo il pulsante "Salva" provoca un post del form per il */Dinners/Edit/1* , URL e invia il codice HTML &lt;input&gt; costituiscono i valori utilizzando il verbo HTTP POST. Verrà ora implementata il comportamento di HTTP POST del metodo di azione di modifica, che gestirà la cena di salvataggio.

Inizieremo aggiungendo un metodo di azione overload "Modifica" per il nostro DinnersController che presenti un attributo "AcceptVerbs" che indica che gestisce gli scenari di HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

Quando i metodi di azione overload viene applicato l'attributo [AcceptVerbs], ASP.NET MVC gestisce automaticamente le richieste di invio al metodo di azione appropriato in base al verbo HTTP in ingresso. Le richieste HTTP POST per <em>/Dinners/modifica / [id]</em> URL verrà inviata al metodo di modifica precedente, mentre tutte le altre richieste di verbi HTTP a <em>/Dinners/modifica / [id]</em>URL verrà inviata al primo metodo di modifica (che è stata implementata ha un attributo [AcceptVerbs]).

| **Sul lato dell'argomento: Differenziare perché tramite i verbi HTTP?** |
| --- |
| Si potrebbe chiedere: perché siamo utilizzando un singolo URL e differenziare il comportamento tramite il verbo HTTP? Perché non solo ha due URL separato per gestire il caricamento e salvataggio delle modifiche? Ad esempio: /Dinners/modifica / [id] per visualizzare il modulo iniziale e /Dinners/Save / [id] per gestire il post del form per salvarlo? Lo svantaggio con pubblicazione due URL separato è che nei casi in cui si registra per /Dinners/Save/2 e quindi è necessario visualizzare nuovamente il form HTML a causa di un errore di input, l'utente finale finirà con l'URL di salvataggio/Dinners/2 nella barra degli indirizzi del browser (dal momento che è stato l'URL inviato il form). Se l'utente finale segnalibri questa pagina redisplayed per i Preferiti del browser o copia/incolla l'URL e invia tramite posta elettronica a un amico, finiranno per salvare un URL che non funziona in futuro (poiché tale URL dipende dai valori post). Esponendo un solo URL (ad esempio: /Dinners/Edit/[id]) e distinguere l'elaborazione di esso dal verbo HTTP, è sicuro agli utenti finali di segnalibro una pagina di modifica e/o inviare l'URL ad altri utenti. |

#### <a name="retrieving-form-post-values"></a>Recupero di valori Post del Form

Esistono diversi modi per accedere registrato i parametri del modulo all'interno di questo metodo "Edit" POST HTTP. Un approccio semplice consiste semplicemente usare la proprietà richiesta sulla classe di base Controller per l'accesso alla raccolta di form e recuperare direttamente i valori registrati:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

L'approccio precedente è un po' dettagliato, tuttavia, in particolare quando viene aggiunta la logica di gestione degli errori.

Un approccio migliore per questo scenario consiste nello sfruttare predefinito *UpdateModel()* metodo helper nella classe di base Controller. Supporta l'aggiornamento delle proprietà di un oggetto che viene passato tramite i parametri del modulo in ingresso. Utilizza la reflection per determinare i nomi di proprietà sull'oggetto e quindi automaticamente converte e assegna valori a essi in base ai valori di input inviati dal client.

È possibile usare il metodo UpdateModel() per semplificare il nostro HTTP-POST Modifica azione utilizzando il codice seguente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

È ora possibile visitare il */Dinners/Edit/1* URL e modificare il titolo del nostro Dinner:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Quando si fa clic sul pulsante "Salva", verranno eseguite un post del form per l'azione di modifica e i valori aggiornati vengono rese persistenti nel database. Si verrà quindi reindirizzati all'URL di dettagli per la cena (che verrà visualizzati i valori appena salvati):

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>Gestione degli errori di modifica

Il nostro corrente HTTP-POST implementazione operazione viene eseguita correttamente, tranne quando sono presenti errori.

Quando un utente apporta una modifica di un modulo di errore, è necessario assicurarsi che il form viene visualizzata di nuovo con un messaggio di errore informativo che contiene le istruzioni per risolvere il problema. Sono inclusi i casi in cui un utente finale invia l'input non corretto (ad esempio: una stringa di data in formato non corretto), nonché i casi in cui il formato di input è valido, ma si verifica una violazione della regola business. Quando si verificano errori che di modulo deve mantenere i dati di input originariamente specificato in modo che non sia necessario ricaricare manualmente le modifiche apportate dall'utente. Questo processo deve essere ripetuto il numero di volte in base alle esigenze fino a quando il form è stata completata correttamente.

ASP.NET MVC include alcune funzionalità incorporate nice che semplificano la gestione degli errori e una nuova visualizzazione form. Per visualizzare queste funzionalità in azione possibile aggiornare il metodo di azione di modifica con il codice seguente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

Il codice sopra riportato è simile per l'implementazione precedente, ad eccezione del fatto che è ora esegue il wrapping un blocco di gestione degli errori try/catch intorno del lavoro. Se si verifica un'eccezione quando si chiama UpdateModel() o quando si tenta e salvare DinnerRepository (che verrà generata un'eccezione se si sta tentando di salvare l'oggetto di Dinner non è valida a causa di una violazione delle regole all'interno di questo modello), il blocco di gestione degli errori catch verrà eseguire. All'interno di esso è eseguire un ciclo su eventuali violazioni delle regole presenti nell'oggetto Dinner e aggiungerli a un oggetto ModelState (che verranno descritti più avanti). È quindi visualizzare di nuovo la visualizzazione.

Per vedere questo utilizzo consente di eseguire nuovamente l'applicazione, modificare una cena e modificarlo per creare un titolo vuoto, un EventDate di "BOGUS" e un numero di telefono Regno Unito con il valore paese USA. Quando si preme il pulsante "Salva" il metodo HTTP POST modifica non sarà in grado di salvare la cena (perché sono presenti errori) e verrà nuovamente visualizzato il form:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

L'applicazione avrà un'esperienza di errore ragionevole. Gli elementi di testo con l'input non valido vengono evidenziati in rosso e vengono visualizzati messaggi di errore di convalida per l'utente finale su di essi. Il modulo anche mantiene i dati di input originariamente specificato dall'utente, in modo che non sia necessario ricaricare nulla.

Si potrebbe chiedere, questo si è verificato? Come stessi evidenziati in rosso e sapere per restituire i valori utente immesso in origine in caselle di testo del titolo, EventDate e ContactPhone? E come i messaggi di errore viene visualizzati nell'elenco nella parte superiore? Buone notizie sono che ciò non si verifica rapida - piuttosto era perché sono stati utilizzati alcune delle funzionalità di ASP.NET MVC predefinite che facilitano la convalida dell'input e errore scenari di gestione.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>Informazioni sui ModelState e i metodi HTML Helper di convalida

Classi controller dispongono di una raccolta di proprietà "ModelState", che fornisce un modo per indicare l'esistano di errori con un oggetto modello passato a una vista. Le voci di errore all'interno della raccolta ModelState identificano il nome della proprietà del modello con il problema (ad esempio: "Title", "EventDate" o "ContactPhone") e consentire un messaggio di errore umane adatto essere specificato (ad esempio: "Titolo è obbligatorio").

Il *UpdateModel()* metodo helper popola automaticamente la raccolta ModelState quando si verificano errori durante il tentativo di assegnare i valori del form per le proprietà dell'oggetto modello. Ad esempio, il valore dell'oggetto Dinner EventDate è di tipo DateTime. Quando il metodo UpdateModel(): Impossibile assegnare il valore di stringa "BOGUS" nello scenario precedente, il metodo UpdateModel() aggiunto una voce alla raccolta ModelState indicante un errore di assegnazione si è verificato con tale proprietà.

Gli sviluppatori possono anche scrivere codice per aggiungere in modo esplicito le voci di errore nella raccolta ModelState come obiettivo nel nostro "catch" errore gestione blocco che è popolamento ModelState insieme con le voci nelle violazioni delle regole attive in base di sotto di Oggetto Dinner:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>Integrazione di HTML Helper con ModelState

Metodi helper HTML, ad esempio Html.TextBox() - controllano la raccolta di ModelState durante il rendering di output. In caso di errore per l'elemento, il rendering sia effettuato il valore immesso dall'utente e una classe di errore CSS.

Ad esempio, la visualizzazione "Modifica" si sta usando il metodo di supporto Html.TextBox() per eseguire il rendering EventDate dell'oggetto Dinner:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Quando la vista è stato eseguito il rendering nello scenario di errore, il metodo Html.TextBox() controllato l'insieme di ModelState per vedere se si sono verificati errori associati alla proprietà "EventDate" dell'oggetto Dinner. Quando è stato rilevato che si è verificato un errore all'input dell'utente inviati ("BOGUS") come valore sottoposto a rendering e aggiunta di una classe di errore css per il &lt;input di tipo = "textbox" /&gt; markup è generato:

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

È possibile personalizzare l'aspetto della classe css errore per la ricerca, tuttavia si desidera. Classe di errore: "input-errore di convalida" – CSS predefinito è definita nel *\content\site.css* come il foglio di stile e l'aspetto seguente:

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

Questa regola CSS è ciò che ha determinato il nostro gli elementi di input non validi viene evidenziata come di seguito:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Metodo Helper Html.ValidationMessage()

Il metodo helper di Html.ValidationMessage() può essere utilizzato per restituire il messaggio di errore ModelState associato a una proprietà di modello specifico:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

Restituisce il codice sopra riportato:  *&lt;span classe = "errore di convalida campo"&gt; il valore 'BOGUS' non è valido &lt; /span&gt;*

Il metodo di supporto Html.ValidationMessage() supporta anche un secondo parametro che consente agli sviluppatori di ignorare il messaggio di testo di errore visualizzato:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

Restituisce il codice sopra riportato:  <em>&lt;span classe = "errore di convalida campo"&gt;\*&lt;/span&gt;</em>anziché il testo di errore predefinito quando un errore sia presente per il Proprietà EventDate.

##### <a name="htmlvalidationsummary-helper-method"></a>Metodo Helper Html.ValidationSummary()

Il metodo di supporto Html.ValidationSummary() può essere utilizzato per eseguire il rendering di un messaggio di errore di riepilogo, accompagnato da un &lt;ul&gt;&lt;li /&gt;&lt;/ul&gt; elenco di errore dettagliato di tutti i messaggi di ModelState raccolta:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

Il metodo di supporto Html.ValidationSummary() accetta un parametro: stringa facoltativa che definisce un messaggio di riepilogo degli errori da visualizzare sopra l'elenco di errori dettagliati:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

Facoltativamente, è possibile utilizzare CSS per eseguire l'override di aspetto analogo al seguente elenco errori.

#### <a name="using-a-addruleviolations-helper-method"></a>Utilizzo di un metodo AddRuleViolations Helper

L'implementazione HTTP-POST modifica iniziale utilizzata un'istruzione foreach il blocco catch per eseguire il ciclo sugli eventuali violazioni delle regole dell'oggetto Dinner e li aggiunge alla raccolta ModelState del controller:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

È possibile rendere il codice di un nastro di pulizia little aggiungendo un "ControllerHelpers" classe al progetto NerdDinner e implementare un metodo di estensione "AddRuleViolations" all'interno che consente di aggiungere un metodo helper alla classe ModelStateDictionary MVC ASP.NET. Questo metodo di estensione può incapsulare la logica necessaria popolare un elenco di errori RuleViolation ModelStateDictionary:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

È quindi possibile aggiornare il metodo di azione HTTP-POST modificare per utilizzare questo metodo di estensione per popolare la raccolta ModelState con il nostro delle violazioni delle regole di Dinner.

#### <a name="complete-edit-action-method-implementations"></a>Completare le implementazioni del metodo di azione di modifica

Il codice seguente implementa la logica controller necessari per lo scenario di modifica:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

Interessante l'implementazione di modifica è la classe Controller né il modello di visualizzazione è necessario conoscere la convalida specifico o le regole di business vengono applicate dal nostro modello Dinner. È possibile aggiungere regole aggiuntive per il modello in futuro e non è necessario apportare eventuali modifiche al codice dei controller o visualizzazione affinché possano essere supportati. Questo offre la flessibilità di sviluppare facilmente i requisiti dell'applicazione in futuro con un minimo di modifiche al codice.

### <a name="create-support"></a>Creare il supporto

È stato completato che implementa il comportamento di "Modifica" della classe organizzazione DinnersController. Ora passiamo implementare il supporto di "Crea" su di esso, in modo da consentire agli utenti di aggiungere nuovi Dinners.

#### <a name="the-http-get-create-action-method"></a>HTTP-GET creare il metodo di azione

Inizieremo dall'implementazione del comportamento HTTP "GET" il metodo di azione di creazione. Questo metodo verrà chiamato quando un utente visita il *Dinners/crea* URL. L'implementazione è simile:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

Il codice precedente crea un nuovo oggetto Dinner e assegna la proprietà EventDate come una settimana in futuro. Esegue quindi il rendering di una vista che si basa sul nuovo oggetto Dinner. Poiché non è possibile passato in modo esplicito un nome per il *View()* metodo di supporto, userà il percorso predefinito basato sulle convenzioni per risolvere il modello di visualizzazione: /Views/Dinners/Create.aspx.

Creare ora il modello di visualizzazione. È possibile farlo facendo clic all'interno del metodo di azione Create e selezionando il comando di menu "Aggiungi visualizzazione" contesto. Nella finestra di dialogo "Aggiungi visualizzazione" si verrà indica che si passa un oggetto Dinner per il modello di visualizzazione e sceglie di auto-scaffold un modello di "Crea":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Quando si fa clic sul pulsante "Aggiungi", Visual Studio salvare una nuova visualizzazione "Create.aspx" basato su scaffold nella directory "\Views\Dinners" e aprirlo all'interno dell'IDE:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

Si apportare alcune modifiche al file di predefinito "Crea" scaffold che è stato generato automaticamente e modificarlo backup, come illustrato di seguito:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

E ora quando si esegue l'accesso alle applicazioni e il *"Dinners/crea"* URL all'interno del browser verrà eseguito il rendering dell'interfaccia utente simile sotto l'implementazione di azioni di creazione:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>Implementazione HTTP-POST creare il metodo di azione

È installata la versione HTTP-GET del metodo di azione Crea implementato. Quando un utente fa clic sul pulsante "Salva" esegue un post del form per il *Dinners/crea* URL e invia il codice HTML &lt;input&gt; costituiscono i valori utilizzando il verbo HTTP POST.

Verrà ora implementata il comportamento di HTTP POST di questo metodo di azione di creazione. Inizieremo aggiungendo un metodo di overload "Crea" azione per il nostro DinnersController che presenti un attributo "AcceptVerbs" che indica che gestisce gli scenari di HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Esistono diversi modi che i parametri di modulo registrato per accedere all'interno di questo metodo "Crea" HTTP-POST abilitato.

Un approccio consiste nel creare un nuovo oggetto Dinner e quindi usare il *UpdateModel()* metodo helper (come abbiamo visto con l'azione di modifica) per popolarlo con i valori del form inseriti. È possibile aggiungerlo al nostro DinnerRepository, renderlo persistente nel database e reindirizzerà l'utente per l'azione di dettagli per visualizzare la cena appena creata utilizzando il codice riportato di seguito:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

In alternativa, è possibile usare un approccio in cui è presente il metodo di azione del metodo di creazione accettano un oggetto Dinner come parametro di metodo. ASP.NET MVC verranno quindi automaticamente creare un'istanza di un nuovo oggetto Dinner per noi, popolare le proprietà usando gli input del form e passarlo al metodo di azione:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

Metodo di azione precedente verifica che l'oggetto Dinner è stato popolato con i valori post del form selezionando la proprietà ModelState.IsValid. Questo comando restituisce false se sono presenti problemi di conversione di input (ad esempio: una stringa di "BOGUS" per la proprietà EventDate), e se sono presenti eventuali problemi di metodo di azione rivisualizzata il form.

Se i valori di input sono validi, quindi il metodo di azione tenta di aggiungere e salvare il nuovo Dinner il DinnerRepository. Esegue il wrapping di questa operazione all'interno di un blocco try/catch e rivisualizzare il form se sono presenti eventuali violazioni delle regole business (che potrebbe causare il metodo dinnerRepository.Save() generare un'eccezione).

Per verificare questo comportamento nell'azione di gestione degli errori, è possibile richiedere il *Dinners/crea* URL e riempimento informazioni dettagliate su un nuovo Dinner. Input non corretto o valori in modo che il modulo Crea essere visualizzata nuovamente con gli errori evidenziati come:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Si noti come il modulo Crea rispetta le regole di convalida e di business stesso esattamente come il modulo di modifica. In questo modo la convalida e regole business sono state definite nel modello e non sono state incorporate all'interno dell'interfaccia utente o il controller dell'applicazione. Di conseguenza è possibile in un secondo momento modifica/evolvere la convalida o regole di business in un singolo inseriscono e applicare tali in tutta l'applicazione. È non sarà necessario modificare qualsiasi codice all'interno di uno la modifica o creare metodi di azione per rispettare automaticamente eventuali nuove regole o modifiche a quelli esistenti.

Correggere i valori di input e fare clic sul pulsante "Salva", l'aggiunta di DinnerRepository avrà esito positivo e un nuovo Dinner verrà aggiunto al database. Quindi si verrà reindirizzati al */Dinners/dettagli / [id]* URL: è in cui verrà visualizzate con i dettagli di Dinner appena creato:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Eliminare il supporto

Aggiungere ora "Elimina" supporto per il nostro DinnersController.

#### <a name="the-http-get-delete-action-method"></a>Il metodo di azione Delete HTTP-GET

Inizieremo implementando il comportamento del metodo di azione delete HTTP GET. Questo metodo verrà chiamato quando un utente visita il */Dinners/Delete / [id]* URL. Di seguito è riportata l'implementazione:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

Il metodo di azione tenta di recuperare la cena da eliminare. Se la cena esistente, esegue il rendering di una vista in base all'oggetto Dinner. Se l'oggetto non esiste (o è già stato eliminato) restituisce una visualizzazione che esegue il rendering "NotFound" visualizzazione modello creato in precedenza per il metodo di azione "Dettagli".

Facendo clic all'interno del metodo di azione di eliminazione e selezionando il comando di menu "Aggiungi visualizzazione" contesto, è possibile creare il modello di visualizzazione "Elimina". Nella finestra di dialogo "Aggiungi visualizzazione" si verrà indica che si passa un oggetto di Dinner a questo modello di visualizzazione come il modello e scegliere di creare un modello vuoto:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Quando si fa clic sul pulsante "Aggiungi", Visual Studio aggiungerà un nuovo file di modello di visualizzazione "Delete.aspx" per noi la directory "\Views\Dinners". Verrà aggiunto al modello per implementare una schermata di conferma eliminazione alcune HTML e il codice come il seguente:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

Il titolo di Dinner da eliminare e l'output viene visualizzato il codice sopra riportato una &lt;modulo&gt; elemento che esegue un POST all'URL di /Dinners/Delete / [id] se l'utente finale fa clic sul pulsante "Elimina" all'interno di esso.

Quando si esegue l'accesso alle applicazioni e il *"/ Dinners/Delete / [id]"* URL per un Dinner valido dell'oggetto esegue il rendering dell'interfaccia utente come il seguente:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Sul lato dell'argomento: Perché facciamo un POST?** |
| --- |
| Si potrebbe chiedere: siamo arrivati un'operazione di creazione di un &lt;modulo&gt; all'interno di schermata di conferma l'eliminazione? Uso di un collegamento ipertestuale standard a cui collegarsi a un metodo di azione che esegue l'operazione di eliminazione effettiva? Il motivo è che si desidera prestare attenzione a evitare crawler web e gli URL di individuazione e causando inavvertitamente i dati da eliminare quando si seguono i collegamenti di motori di ricerca. HTTP-GET basato su URL sono considerati "sicuro" per consentire l'accesso o ricerca per indicizzazione, e che sono autorizzati a seguire non di quelli HTTP-POST. Una buona regola consiste nel fatto che è sempre inserito distruttiva o operazioni di modifica dei dati protetti da richieste HTTP POST. |

#### <a name="implementing-the-http-post-delete-action-method"></a>Implementazione del metodo di azione Delete HTTP-POST

È ora disponibile la versione di HTTP-GET del metodo di azione Delete implementato che visualizza una schermata di conferma eliminazione. Quando un utente finale fa clic sul pulsante "Elimina" eseguirà un post del form per il */Dinners Dinner / [id]* URL.

Verrà ora implementata il comportamento HTTP "POST" del metodo di azione delete usando il codice riportato di seguito:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

La versione HTTP-POST del metodo di azione di eliminazione tenta di recuperare l'oggetto dinner da eliminare. Se non la trova (perché è già stato eliminato) viene eseguito il rendering di modelli "NotFound". Se viene rilevata la cena, eliminata dal DinnerRepository. Esegue quindi il rendering di un modello "Eliminato".

Per implementare il modello "Eliminato" verrà fare clic con il metodo di azione del mouse e scegliere il menu di scelta rapida "Aggiungi visualizzazione". Si sarà la vista "Eliminato" e fare in modo da un modello vuoto (e non accettano un oggetto modello fortemente tipizzato). Verrà quindi aggiunto contenuto HTML a esso:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

E ora quando si esegue l'accesso alle applicazioni e il *"/ Dinners/Delete / [id]"* URL per una valida cena conferma eliminazione oggetto esegue il rendering di nostri Dinner schermata come di seguito:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Quando si fa clic sul pulsante "Elimina" viene eseguita una richiesta HTTP POST al */Dinners/Delete / [id]* URL, che elimina la cena dal database e visualizzare il modello di visualizzazione "Eliminato":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Associazione di sicurezza del modello

Abbiamo discusso due diversi modi per utilizzare le funzionalità di associazione di modelli predefinite di ASP.NET MVC. La prima mediante il metodo UpdateModel() per aggiornare le proprietà su un oggetto modello esistente e il secondo mediante il supporto di ASP.NET MVC per il passaggio di oggetti del modello in come parametri di metodo di azione. Entrambe queste tecniche sono molto potenti ed estremamente utili.

Questo power implica responsabilità. È importante essere sempre accurata sulla sicurezza per l'accettazione di qualsiasi input dell'utente, e ciò vale anche quando l'associazione di oggetti di input del form. È necessario prestare attenzione a HTML codificare sempre i valori immessi dall'utente per evitare attacchi intrusivi nel codice HTML e JavaScript e prestare attenzione di attacchi SQL injection (Nota: si sta usando LINQ to SQL per l'applicazione, che codifica automaticamente i parametri per evitare questi tipi di attacchi). Dovrebbero mai basarsi sulla convalida sul lato client da solo e utilizzano sempre la convalida sul lato server per impedire ai pirati informatici il tentativo di inviare valori falsi.

Un elemento di sicurezza aggiuntive per assicurarsi che si pensa quando si utilizza la funzionalità di associazione di ASP.NET MVC è l'ambito degli oggetti a cui che si esegue l'associazione. In particolare, si desidera rendere è importante che comprendere le implicazioni di sicurezza delle proprietà che si consente a essere associato e assicurarsi che consentire solo le proprietà che devono essere effettivamente aggiornabile da un utente finale da aggiornare.

Per impostazione predefinita, il metodo UpdateModel() tenterà di aggiornare tutte le proprietà che corrispondono ai valori di parametro di formato in ingresso per l'oggetto modello. Analogamente, gli oggetti passati come parametri di metodo di azione anche per impostazione predefinita possono avere tutte le relative proprietà impostate tramite i parametri del modulo.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Il blocco di associazione in base all'utilizzo

È possibile bloccare i criteri di associazione in base all'utilizzo da fornire un'esplicita "include elenco" di proprietà che possono essere aggiornate. Questa operazione può essere eseguita passando un parametro di matrice di stringa aggiuntiva per il metodo UpdateModel() come illustrato di seguito:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Gli oggetti passati come parametri di metodo di azione inoltre supportano l'attributo [Bind che consente a un "include elenco" di proprietà essere specificati come di seguito è consentito:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Il blocco di associazione in base al tipo

È inoltre possibile bloccare le regole di associazione in base al tipo. Ciò consente di specificare le regole di associazione di una volta e quindi quindi applicare in tutti gli scenari (inclusi scenari di parametro di metodo UpdateModel sia azione) in tutti i controller e metodi di azione.

È possibile personalizzare le regole di associazione per ogni tipo aggiungendo un attributo [Bind] su un tipo o registrandolo all'interno del file Global. asax dell'applicazione (utile per scenari in cui non si dispone di tipo). È quindi possibile utilizzare Include l'associazione dell'attributo ed Exclude proprietà per controllare quali proprietà sono associabile per la particolare classe o interfaccia.

Verrà usata questa tecnica per la classe Dinner nell'applicazione NerdDinner e aggiungere un attributo [Bind] che limita l'elenco di proprietà associabili ai seguenti:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Notare che non si consente la raccolta inviate risposte essere modificato tramite l'associazione, né si consente il DinnerID o HostedBy impostare le proprietà tramite associazione. Per motivi di sicurezza è verrà invece solo modificare queste proprietà particolare utilizzando codice esplicito all'interno i metodi di azione.

### <a name="crud-wrap-up"></a>Conclusioni CRUD

ASP.NET MVC include numerose funzionalità che semplificano l'implementazione di scenari di registrazione del modulo. Un'ampia gamma di queste funzionalità è utilizzata per fornire il supporto dell'interfaccia utente CRUD sopra il nostro DinnerRepository.

Si sta usando un approccio incentrata sul modello per implementare l'applicazione. Ciò significa che tutti i convalida e regole di business logica viene definita all'interno di questo livello del modello: e non all'interno dei controller o viste. La classe Controller né visualizza i modelli di conoscono le regole di business specifico imposizione dalla classe modello Dinner.

Verrà mantenere l'architettura dell'applicazione pulito e rendono più semplice eseguire il test. È possibile aggiungere regole di business aggiuntive per il livello del modello in futuro e *non è necessario apportare alcuna modifica al codice* dei Controller o visualizzazione affinché possano essere supportati. Si tratta di qualcosa di fornire una notevole flessibilità di sviluppare e modificare l'applicazione in futuro.

Il nostro DinnersController ora consente Dinner elenchi o dettagli, nonché creare, modificare ed eliminare il supporto. Il codice completo per la classe può essere disponibile di seguito:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>Passo successivo

È ora disponibile il supporto di base CRUD (Create, Read, Update e Delete) a implementare la classe DinnersController.

Ora esaminare in che modo utilizzare classi ViewData e ViewModel attivare arricchire dell'interfaccia utente sul form.

> [!div class="step-by-step"]
> [Precedente](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [Successivo](use-viewdata-and-implement-viewmodel-classes.md)
