---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: Fornire CRUD (Create, leggere, aggiornare ed eliminare) dati formano supporto voce | Microsoft Docs
author: microsoft
description: Passaggio 5 viene illustrato come sfruttare ulteriormente la classe DinnersController da abilitare il supporto per la modifica, creazione ed eliminazione Dinners con esso anche.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: 45d74249a34fc7e37e9776a398615d2f613a7582
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832739"
---
<a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>Fornire CRUD (Create, leggere, aggiornare ed eliminare) dati formano il supporto di ingresso
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Si tratta di passaggio 5 di una liberazione [esercitazione sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-dettaglio come compilare una piccola, ma completa, applicazione web con ASP.NET MVC 1.
> 
> Passaggio 5 viene illustrato come sfruttare ulteriormente la classe DinnersController da abilitare il supporto per la modifica, creazione ed eliminazione Dinners con esso anche.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le [Guida introduttiva con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oppure [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner passaggio 5: Creare, aggiornare ed eliminare gli scenari di Form

Abbiamo introdotto i controller e visualizzazioni e ha descritto come utilizzarle per implementare un'esperienza di elenco/dettagli per Dinners nel sito. Il passaggio successivo sarà possibile eseguire ulteriori nostra classe DinnersController e abilitare il supporto per la modifica, creazione ed eliminazione Dinners con esso anche.

### <a name="urls-handled-by-dinnerscontroller"></a>URL gestito da DinnersController

Aggiunto in precedenza i metodi di azione di DinnersController che implementato il supporto per due URL: */Dinners* e */Dinners/dettagli / [id]*.

| **URL** | **VERBO** | **Scopo** |
| --- | --- | --- |
| */Dinners/* | GET | Visualizzare un elenco HTML di dinners imminente. |
| */Dinners/dettagli / [id]* | GET | Visualizzare informazioni dettagliate su una specifica cena. |

Ora si aggiungeranno i metodi di azione per implementare tre URL aggiuntivi: <em>/Dinners/modifica / [id], / Dinners/creazione,</em>e<em>/Dinners/Delete / [id]</em>. Questi URL abiliterà il supporto per la modifica Dinners esistente, creazione nuovo Dinners ed eliminazione Dinners.

Il supporto delle interazioni di verbo HTTP GET e HTTP POST con questi nuovi URL. Richieste GET HTTP a questi URL visualizzerà la visualizzazione HTML iniziale dei dati (un modulo compilato con i dati di Dinner nel caso di "Modifica", un modulo vuoto in caso di "creazione" e una schermata di conferma delete nel caso di eliminazione""). Le richieste HTTP POST agli URL seguenti verranno save/update/delete dati Dinner nel nostro DinnerRepository (e da lì nel database).

| **URL** | **VERBO** | **Scopo** |
| --- | --- | --- |
| */Dinners/modifica / [id]* | GET | Visualizzare un form HTML modificabile popolato con dati cena. |
| INSERISCI | Salvare le modifiche di form per un particolare Dinner al database. |
| */Dinners/Create* | GET | Visualizzare un form HTML vuoto che consente agli utenti di definire nuove Dinners. |
| INSERISCI | Creare un nuovo Dinner e salvarlo nel database. |
| */Dinners/delete / [id]* | GET | Visualizzazione Elimina la schermata di conferma. |
| INSERISCI | Elimina la cena specificata dal database. |

### <a name="edit-support"></a>Supporto di modifica

Si inizia implementando lo scenario di "Modifica".

#### <a name="the-http-get-edit-action-method"></a>Il metodo di azione di modifica HTTP-GET

Si inizierà implementando il comportamento di "GET" HTTP, il modifica del metodo di azione. Questo metodo è richiamato quando la */Dinners/modifica / [id]* è richiesto l'URL. La nostra implementazione avrà un aspetto simile:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

Il codice precedente Usa la DinnerRepository per recuperare un oggetto Dinner. Esegue quindi il rendering di un modello di vista utilizzando l'oggetto Dinner. Poiché non è stato passato in modo esplicito un nome modello per il *View()* metodo helper, userà il percorso predefinito basato sulle convenzioni per risolvere il modello di vista: /Views/Dinners/Edit.aspx.

Creare ora questo modello di visualizzazione. Faremo facendo clic all'interno del metodo di modifica e selezionando il comando di menu di scelta rapida "Aggiungi visualizzazione":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

Nella finestra di dialogo "Aggiungi visualizzazione" si indicherà che si sta passando un oggetto di Dinner per il modello di visualizzazione come il modello e sceglie di auto-scaffold un modello di "Modifica":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Quando si fa clic sul pulsante "Aggiungi", Visual Studio aggiungerà un nuovo file di modello di visualizzazione "Edit" per noi all'interno della directory "\Views\Dinners". Si aprirà anche configura il nuovo modello di visualizzazione "Edit" all'interno di editor del codice-– popolata con un'iniziale "Modifica" scaffold implementazione, ad esempio riportata di seguito:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

È possibile apportare alcune modifiche a scaffolding predefiniti "Modifica" generati e aggiornare il modello di visualizzazione di modifica per ottenere il contenuto seguente (che consente di rimuovere alcune delle proprietà che non si desidera esporre):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Quando si esegue l'applicazione e richiedere il *"/ Dinners/modifica/1"* URL si verrà visualizzata la pagina seguente:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

Il markup HTML generato da visualizzazione l'aspetto seguente. È HTML standard, con un &lt;form&gt; elemento che esegue una richiesta HTTP POST per il */Dinners/Edit/1* URL quando "Salva" &lt;tipo di input = "submit" /&gt; pulsante è premuto. Un HTML &lt;input di tipo = "text" /&gt; elemento è stato output per ogni proprietà modificabili:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Html.BeginForm() e i metodi Html Helper Html.TextBox()

Il modello di visualizzazione "Edit" Usa diversi metodi di "Helper Html": Html.ValidationSummary(), Html.BeginForm(), Html.TextBox() e Html.ValidationMessage(). Oltre alla generazione di markup HTML per noi, questi metodi di supporto forniscono la convalida e gestione degli errori predefiniti supportano.

##### <a name="htmlbeginform-helper-method"></a>Metodo helper Html.BeginForm()

Il metodo helper Html.BeginForm() è ciò che il codice HTML di output &lt;form&gt; elemento nel nostro codice. Nel nostro modello di vista Edit. aspx si noterà che vengono applicate un'istruzione "using" quando si usa questo metodo c#. La parentesi graffa aperta indica l'inizio della &lt;form&gt; contenuto e la parentesi graffa di chiusura è ciò che indica la fine delle &lt;/form&gt; elemento:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

In alternativa, se si trova l'istruzione "using" approccio non naturali per uno scenario simile al seguente, è possibile usare una combinazione di Html.BeginForm() e Html.EndForm() (che esegue la stessa operazione):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

La chiamata a Html.BeginForm() senza parametri determinerà l'output di un elemento di formato che esegue una richiesta HTTP-POST all'URL della richiesta corrente. Vale a dire il motivo per cui la visualizzazione di modifica genera una *&lt;azione modulo = "/ Dinners/modifica/1" metodo = "post"&gt;* elemento. È stato possibile in alternativa trascorsi i parametri espliciti per Html.BeginForm() se volessimo post a un URL diverso.

##### <a name="htmltextbox-helper-method"></a>Metodo helper Html.TextBox()

La vista Edit. aspx viene utilizzato il metodo helper Html.TextBox() per generare &lt;input di tipo = "text" /&gt; elementi:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

Il metodo Html.TextBox() precedente accetta un singolo parametro, che viene viene utilizzato per specificare entrambi gli attributi di nome/id del &lt;input di tipo = "text" /&gt; elemento di output, nonché la proprietà del modello per popolare il valore nella casella di testo da. Ad esempio, l'oggetto Dinner è passati alla visualizzazione di modifica ha un valore della proprietà "Title" di ".NET future", e quindi il metodo Html.TextBox("Title") chiamare output: *&lt;input con id = "Title" name = "Title" type = "text" value = ".NET Futures" /&gt;*.

In alternativa, è possibile usare il primo parametro Html.TextBox() per specificare il nome o un id dell'elemento e quindi passare in modo esplicito il valore da usare come secondo parametro:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

Spesso si voglia eseguire la formattazione personalizzata per il valore di output. Il metodo statico String integrata in .NET è utile per questi scenari. Il modello di vista Edit. aspx utilizza questo per formattare il valore EventDate, ovvero di tipo Data/ora, in modo che non visualizza i secondi per il periodo di tempo:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Un terzo parametro Html.TextBox() facoltativamente utilizzabile da altri attributi HTML di output. Il frammento di codice seguente viene illustrato come eseguire il rendering di una dimensione aggiuntiva = "30" e un attributo class = "mycssclass" attributo on la &lt;input di tipo = "text" /&gt; elemento. Si noti come abbiamo stiamo escape il nome dell'attributo di classe con un "@" character because "classe" è una parola chiave riservata in c#:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>L'implementazione del metodo di azione di modifica HTTP-POST

È ora disponibile la versione HTTP-GET del nostro metodo di azione di modifica implementata. Quando un utente richiede la */Dinners/Edit/1* URL ricevono una pagina HTML simile al seguente:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

Premendo il pulsante "Salva" fa in modo che un post dei form per il */Dinners/Edit/1* URL, e invia il codice HTML &lt;input&gt; costituiscono i valori usando il verbo POST HTTP. Verrà ora implementata il comportamento di richiesta HTTP POST il modifica del metodo di azione, che gestirà la cena di salvataggio.

Inizieremo aggiungendo un metodo di azione "Modifica" overload al nostro DinnersController che dispone dell'attributo un "AcceptVerbs" che indica che gestisce gli scenari di richiesta HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

Quando l'attributo [AcceptVerbs] viene applicato ai metodi di overload di azione, ASP.NET MVC gestisce automaticamente le richieste di invio al metodo di azione appropriato in base al verbo HTTP in ingresso. Le richieste HTTP POST a <em>/Dinners/modifica / [id]</em> URL verrà inviata al metodo di modifica precedente, mentre tutte le altre richieste di verbo HTTP per <em>/Dinners/modifica / [id]</em>passa gli URL per il primo metodo di modifica è (che è stata implementata non avere un attributo [AcceptVerbs]).

| **Sul lato dell'argomento: Il motivo per cui differenziare tramite verbi HTTP?** |
| --- |
| Ci si potrebbe chiedere: il motivo per cui siamo usando un singolo URL originali e graficamente il comportamento tramite il verbo HTTP? Perché non sono solo due URL distinti per gestire il caricamento e salvataggio di modifiche? Ad esempio: /Dinners/modifica / [id] per visualizzare il modulo iniziale e /Dinners/Save / [id] per gestire l'invio del modulo per salvare il file? Lo svantaggio con pubblicazione due URL distinti è che nei casi in cui si registra per /Dinners/Save/2 e, necessario visualizzare nuovamente il form HTML a causa di un errore di input, l'utente finale finirà con l'URL Dinners/Save/2 nella barra degli indirizzi del proprio browser (dal momento che è stato l'URL di invio del form). Se l'utente finale inserisce un segnalibro in questa pagina redisplayed nel proprio elenco di Preferiti del browser, o copia/incolla l'URL e invia tramite posta elettronica a un amico, finiranno per il salvataggio di un URL che non funzionerà in futuro (dal momento che tale URL dipende da valori post). Tramite l'esposizione di un singolo URL (ad esempio: /Dinners/Edit/[id]) originali e graficamente l'elaborazione dal verbo HTTP, è sicuro per gli utenti finali per la pagina di modifica del segnalibro e/o inviare l'URL ad altri utenti. |

#### <a name="retrieving-form-post-values"></a>Recupero di valori Post del Form

Esistono svariati modi, post che è possibile accedere pubblicato i parametri del modulo nel nostro "Modifica" metodo HTTP POST. Un approccio semplice consiste semplicemente usare la proprietà richiesta sulla classe di base Controller per accedere alla raccolta di form e recuperare direttamente i valori registrati:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

L'approccio precedente è un po' verbose, tuttavia, in particolare dopo abbiamo aggiunto per la logica di gestione degli errori.

Un approccio migliore per questo scenario consiste nello sfruttare integrato *UpdateModel()* metodo helper nella classe base Controller. Supporta l'aggiornamento di proprietà di un oggetto che viene passato usando i parametri del modulo in ingresso. Usa la reflection per determinare i nomi delle proprietà sull'oggetto e quindi automaticamente converte e assegna valori ad essi in base ai valori di input inviati dal client.

Utilizziamo il metodo UpdateModel() potremmo per semplificare l'HTTP-POST Modifica azione utilizzando questo codice:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

A questo punto è possibile visitare il */Dinners/Edit/1* URL e modificare il titolo del nostro Dinner:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Quando si fa clic sul pulsante "Salva", si eseguiranno un post dei form per l'operazione di modifica, e i valori aggiornati verranno mantenuti nel database. Si verrà quindi reindirizzati all'URL di informazioni dettagliate per la cena (che verrà visualizzati i valori appena salvati):

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>Gestione degli errori di modifica

Il corrente HTTP-POST implementazione operazione viene eseguita correttamente, tranne quando sono presenti errori.

Quando un utente commette un errore la modifica di un modulo, è necessario assicurarsi che il modulo viene nuovamente visualizzato con un messaggio di errore informativo che contiene le istruzioni per risolvere il problema. Sono inclusi i casi in cui un utente finale invia l'input non corretto (ad esempio: una stringa di data in formato non corretto), come anche casi in cui il formato di input è valido ma non esiste una violazione della regola business. Quando si verificano errori che il form deve mantenere i dati di input originariamente specificato in modo che gli utenti non devono inserire di nuovo le modifiche apportate manualmente dall'utente. Questo processo deve essere ripetuto molte volte in base alle esigenze fino a quando il form viene completata correttamente.

ASP.NET MVC include alcune interessanti funzionalità che semplificano la gestione degli errori e una nuova visualizzazione form. Per visualizzare queste funzionalità in azione, è possibile aggiornare il metodo di azione di modifica con il codice seguente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

Il codice sopra riportato è simile alla nostra implementazione precedente, ad eccezione del fatto che abbiamo ora si esegue il wrapping un blocco di gestione degli errori try/catch intorno a nostro lavoro. Se si verifica un'eccezione quando si chiama UpdateModel() o quando si prova a salvare DinnerRepository (che genererà un'eccezione se l'oggetto Dinner che stiamo tentando di salvare non è valido a causa di una violazione della regola all'interno del nostro modello), il blocco di gestione degli errori catch verranno eseguire. All'interno di esso è eseguire un ciclo su eventuali violazioni delle regole esistenti nell'oggetto Dinner e aggiungerli a un oggetto ModelState (che verrà trattata tra breve). Abbiamo quindi visualizzare di nuovo la visualizzazione.

Per visualizzare questo utilizzo è possibile eseguire nuovamente l'applicazione, modificare un Dinner e modificarlo per creare un titolo vuoto, un EventDate di "BOGUS" e un numero di telefono Regno Unito con un valore del paese di Stati Uniti. Quando si preme il pulsante "Salva" il metodo HTTP POST modifica non sarà in grado di salvare la cena (perché sono presenti errori) e verrà visualizzata nuovamente il modulo:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

L'applicazione ha un'esperienza soddisfacente errore. Gli elementi di testo con l'input non valido vengono evidenziati in rosso e vengono visualizzati i messaggi di errore di convalida per l'utente finale su essi. Il modulo anche mantiene i dati di input originariamente specificato dall'utente, in modo che non hanno il ricarico alcuna operazione.

Come fare, ci si potrebbe chiedere, si è verificato questo? Come è stata stessi evidenziati in rosso e in grado di restituire i valori utente immesso originariamente le caselle di testo del titolo e l'elemento EventDate ContactPhone? E come i messaggi di errore visualizzati nell'elenco nella parte superiore? La buona notizia è che questo non si verifica per magia, piuttosto era poiché è stato usato alcune delle funzionalità ASP.NET MVC incorporate che agevolano l'errore e la convalida dell'input gli scenari di gestione.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>Informazioni su ModelState e i metodi HTML Helper di convalida

Le classi controller hanno una raccolta di proprietà "ModelState" che fornisce un modo per indicare che sono presenti errori con un oggetto modello passato a una visualizzazione. Le voci di errore all'interno dell'insieme di ModelState identificano il nome della proprietà del modello con il tipo di problema (ad esempio: "Title", "L'elemento EventDate" o "ContactPhone") e consentire un messaggio di errore Converto da specificare (ad esempio: "Titolo è obbligatorio").

Il *UpdateModel()* metodo helper popola automaticamente la raccolta di ModelState quando si verificano errori durante il tentativo di assegnare i valori del form per le proprietà dell'oggetto modello. Ad esempio, la cena EventDate proprietà oggetto è di tipo DateTime. Quando il metodo UpdateModel() è Impossibile assegnare il valore della stringa "BOGUS" ad esso nello scenario precedente, aggiungere il metodo UpdateModel() si è verificato una voce alla raccolta ModelState che indica un errore di assegnazione con tale proprietà.

Gli sviluppatori possono anche scrivere codice per aggiungere in modo esplicito le voci di errore nella raccolta ModelState, ad esempio che stiamo facendo seguito nel nostro "catch" errore gestione blocco che viene popolata ModelState con voci basate sulle violazioni delle regole attive di Oggetto Dinner:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>Integrazione di HTML Helper con ModelState

I metodi helper HTML - come Html.TextBox() - controllano la raccolta di ModelState durante il rendering di output. Se si verifica un errore per l'elemento, esecuzione del rendering il valore immesso dall'utente e una classe di errore CSS.

Ad esempio, nella nostra vista "Modifica" utilizziamo il metodo helper Html.TextBox() per eseguire il rendering EventDate del nostro oggetto Dinner:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Quando la vista è stato eseguito il rendering nello scenario di errore, il metodo Html.TextBox() controllato l'insieme di ModelState per vedere se si sono verificati errori associati alla proprietà "L'elemento EventDate" della nostro oggetto Dinner. Quando determinato che si è verificato un errore, viene eseguito il rendering di input dell'utente inviati ("BOGUS") come valore e aggiunta una classe di errore di css per il &lt;input di tipo = "textbox" /&gt; markup è generato:

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

È possibile personalizzare l'aspetto della classe css errore per la ricerca, tuttavia si desidera. La classe di errore CSS predefinita: "input-errore di convalida" – è definita nel *\content\site.css* foglio di stile e ha un aspetto simile al seguente:

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

Questa regola CSS è ciò che ha causato i elementi di input non validi viene evidenziata, come di seguito:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Metodo Helper Html.ValidationMessage()

Il metodo helper Html.ValidationMessage() è utilizzabile per il messaggio di errore ModelState associato alla proprietà un particolare modello di output:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

Il codice precedente restituisce:  *&lt;span classe = "errore di convalida campo"&gt; non è valido il valore 'BOGUS' &lt; /span&gt;*

Il metodo helper Html.ValidationMessage() supporta anche un secondo parametro che consente agli sviluppatori di ignorare il messaggio di testo di errore visualizzato:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

Il codice precedente restituisce:  <em>&lt;span classe = "errore di convalida campo"&gt;\*&lt;/span&gt;</em>anziché il testo di errore predefinito quando un errore è presente per il L'elemento EventDate proprietà.

##### <a name="htmlvalidationsummary-helper-method"></a>Metodo Helper Html.ValidationSummary()

Il metodo di helper Html.ValidationSummary() può essere usato per eseguire il rendering di un messaggio di riepilogo degli errori, accompagnato da un &lt;ul&gt;&lt;li /&gt;&lt;/ul&gt; elenco dei messaggi di errore dettagliati tutti i messaggi nel Raccolta di ModelState:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

Il metodo helper Html.ValidationSummary() accetta un parametro: stringa facoltativa che definisce un messaggio di riepilogo degli errori da visualizzare sopra l'elenco di errori dettagliati:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

Facoltativamente, è possibile utilizzare CSS per eseguire l'override come appare l'elenco errori.

#### <a name="using-a-addruleviolations-helper-method"></a>Utilizzo di un metodo AddRuleViolations Helper

La nostra implementazione HTTP-POST modifica iniziale utilizzata un'istruzione foreach all'interno del blocco catch per eseguire un ciclo su violazioni delle regole dell'oggetto Dinner e aggiungerli alla raccolta ModelState del controller:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

È possibile inviare questo codice così una piccola aggiungendo un "ControllerHelpers" classe al progetto NerdDinner e implementare un metodo di estensione "AddRuleViolations" all'interno che consente di aggiungere un metodo helper alla classe di ASP.NET MVC ModelStateDictionary. Questo metodo di estensione possibile incapsulare la logica necessaria popolare ModelStateDictionary con un elenco di errori RuleViolation:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

È quindi possibile aggiornare il metodo di azione HTTP-POST modifica per usare questo metodo di estensione per popolare la raccolta di ModelState con violazioni delle regole nostro Dinner.

#### <a name="complete-edit-action-method-implementations"></a>Completare le implementazioni del metodo di azione di modifica

Il codice seguente implementa tutta la logica di controller necessaria per questo scenario di modifica:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

Il bello dell'implementazione della modifica è che la classe Controller né il modello di visualizzazione è necessario conoscere le specifiche di convalida o le regole di business vengono applicate dal nostro modello di Dinner. È possibile aggiungere altre regole per il modello in futuro e non è necessario apportare modifiche al codice al nostro controller o visualizzazione affinché possano essere supportati. Questo ci fornisce la flessibilità necessaria per evolvere facilmente i requisiti dell'applicazione in futuro con un minimo di modifiche al codice.

### <a name="create-support"></a>Creare il supporto

È stata completata che implementa il comportamento di "Modifica" della nostra classe DinnersController. Ora passiamo a implementare il supporto di "Crea" su di esso, in modo da consentire agli utenti di aggiungere nuovi Dinners.

#### <a name="the-http-get-create-action-method"></a>HTTP-GET Crea metodo di azione

Inizieremo dall'implementazione del comportamento HTTP "GET" il metodo di azione di creazione. Questo metodo verrà chiamato quando un utente visita il *Dinners/crea* URL. L'implementazione è simile a:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

Il codice precedente crea un nuovo oggetto Dinner e assegna la proprietà l'elemento EventDate come una settimana in futuro. Esegue quindi il rendering di una vista che si basa sul nuovo oggetto Dinner. Poiché non è stato passato in modo esplicito un nome per il *View()* metodo helper, userà il percorso predefinito basato sulle convenzioni per risolvere il modello di vista: /Views/Dinners/Create.aspx.

Creare ora questo modello di visualizzazione. È possibile farlo facendo clic all'interno del metodo di azione Create e selezionando il comando di menu di scelta rapida "Aggiungi visualizzazione". Nella finestra di dialogo "Aggiungi visualizzazione" si indicherà che si sta passando un oggetto Dinner per il modello di vista e scelta di auto-scaffold un modello di "Crea":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Quando si fa clic sul pulsante "Aggiungi", Visual Studio salvare una nuova visualizzazione "Tornarci" basato su scaffold nella directory "\Views\Dinners" e aprirlo all'interno dell'IDE:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

È possibile apportare alcune modifiche al "Crea" scaffold file predefinito che è stato generato automaticamente e modificarlo backup con l'aspetto seguente:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

E quando si esegue l'accesso alle applicazioni e il *"Dinners/Create"* URL all'interno del browser eseguirà il rendering dell'interfaccia utente, ad esempio seguito dalla nostra implementazione di azione di creazione:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>Implementa il protocollo HTTP-POST Crea metodo di azione

È disponibile la versione HTTP-GET di implementato il metodo di azione Create. Quando un utente fa clic sul pulsante "Salva" esegue un post dei form per il *Dinners/creare* URL, e invia il codice HTML &lt;input&gt; costituiscono i valori usando il verbo POST HTTP.

Verrà ora implementata il comportamento di richiesta HTTP POST del nostro Crea metodo di azione. Inizieremo aggiungendo un metodo di azione "Crea" overload al nostro DinnersController che dispone dell'attributo un "AcceptVerbs" che indica che gestisce gli scenari di richiesta HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Esistono diversi modi, che è possibile accedere ai parametri di modulo inviati entro il metodo HTTP-POST attivato "Crea".

Un approccio consiste nel creare un nuovo oggetto Dinner e quindi usare il *UpdateModel()* metodo helper (come abbiamo fatto con l'azione di modifica) per popolarlo con i valori di modulo. È quindi possibile aggiungerlo al nostro DinnerRepository, renderlo persistente nel database e reindirizzerà l'utente per l'azione di dettagli per mostrare la cena appena creata usando il codice seguente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

In alternativa, è possibile usare un approccio in cui è disponibile il metodo di azione create () accettano un oggetto Dinner come un parametro del metodo. ASP.NET MVC verranno quindi automaticamente crea un'istanza di un nuovo oggetto Dinner per noi, popolare le proprietà usando gli input di modulo e passarlo al metodo di azione:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

Il metodo di azione precedente verifica che l'oggetto Dinner è stato popolato con i valori post del form controllando la proprietà ModelState. Verrà restituito false se sono presenti problemi di conversione di input (ad esempio: una stringa di "BOGUS" per la proprietà EventDate), e se sono presenti problemi il metodo di azione rivisualizza il form.

Se i valori di input sono validi, quindi il metodo di azione tenta di aggiungere e salvare il nuovo Dinner il DinnerRepository. Effettua il wrapping di queste operazioni all'interno di un blocco try/catch e viene visualizzata nuovamente il form se sono presenti eventuali violazioni delle regole di business (che potrebbe causare il metodo dinnerRepository.Save() generare un'eccezione).

Per verificare questo comportamento nell'azione di gestione degli errori, è possibile richiedere il *Dinners/crea* URL e immettere le informazioni di informazioni dettagliate su una nuova cena. Input non corretto o valori provocherà il modulo di creazione essere visualizzata nuovamente con gli errori, come evidenziati di seguito:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Si noti come il modulo Crea rispetta le regole di convalida e di business stesso esattamente come il modulo di modifica. Infatti, le regole di convalida e business sono state definite nel modello e non sono state incorporate all'interno dell'interfaccia utente o il controller dell'applicazione. Ciò significa che possiamo in un secondo momento modifica/evolvere la convalida o le regole di business in una singola posizionano e chiedere di applicare in tutta l'applicazione. Non abbiamo di modificare qualsiasi codice all'interno di uno la modifica o creare metodi di azione per rispettare automaticamente eventuali nuove regole o modifiche a quelli esistenti.

Quando abbiamo correggere i valori di input e fare clic sul pulsante "Salva" anche in questo caso, l'aggiunta al DinnerRepository avrà esito positivo e un nuovo Dinner verrà aggiunto al database. Si verrà quindi reindirizzati per il */Dinners/dettagli / [id]* URL – in cui si verrà visualizzati con i dettagli di Dinner appena creato:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Il supporto dell'eliminazione

Questo punto, aggiungere il supporto "Delete" al nostro DinnersController.

#### <a name="the-http-get-delete-action-method"></a>Il metodo di azione Delete HTTP-GET

Inizieremo implementando il comportamento del nostro metodo di azione delete HTTP GET. Questo metodo verrà chiamato quando un utente visita il */Dinners/Delete / [id]* URL. Di seguito è riportata l'implementazione:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

Il metodo di azione tenta di recuperare la cena da eliminare. Se la cena esiste, esegue il rendering di una vista basata sull'oggetto Dinner. Se l'oggetto non esiste (o è già stato eliminato) restituisce una visualizzazione che esegue il rendering di "NotFound" visualizzazione modello creato in precedenza per il metodo di azione "Dettagli".

È possibile creare il modello di vista "Delete" facendo clic all'interno del metodo di azione di eliminazione e selezione del comando di menu di scelta rapida "Aggiungi visualizzazione". Nella finestra di dialogo "Aggiungi visualizzazione" si indicherà che si sta passando un oggetto di Dinner per il modello di visualizzazione come il modello e scegliere di creare un modello vuoto:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Quando si fa clic sul pulsante "Aggiungi", Visual Studio aggiungerà un nuovo file di modello di visualizzazione "Delete.aspx" automaticamente entro la directory "\Views\Dinners". Verrà aggiunto al modello per implementare una schermata di conferma delete alcuni HTML e codice simile al seguente:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

Il codice precedente visualizza il titolo del Dinner deve essere eliminato e gli output una &lt;form&gt; elemento che esegue una richiesta POST all'URL di /Dinners/Delete / [id] se l'utente finale fa clic sul pulsante "Elimina" all'interno di esso.

Quando si esegue l'accesso alle applicazioni e il *"/ Dinners/Delete / [id]"* URL per un Dinner valido dell'oggetto esegue il rendering dell'interfaccia utente simile alla seguente:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Sul lato dell'argomento: Il motivo per cui facciamo una richiesta POST?** |
| --- |
| Ci si potrebbe chiedere: siamo arrivati tramite le operazioni di creazione di un &lt;form&gt; entro la schermata di conferma eliminazione? Perché non utilizzare semplicemente un collegamento ipertestuale standard a cui collegarsi a un metodo di azione che esegue l'operazione di eliminazione? Il motivo è che è opportuno prestare attenzione alla protezione da crawler web e per individuare gli URL e causare inavvertitamente i dati da eliminare quando si seguono i collegamenti di motori di ricerca. HTTP-GET basato su URL sono considerati "sicuri" per poter accedere/ricerca per indicizzazione e che sono autorizzati a non seguire quelle HTTP-POST. Una buona regola consiste nell'assicurarsi che si tiene sempre distruttive o operazioni di modifica dei dati protetti da richieste HTTP POST. |

#### <a name="implementing-the-http-post-delete-action-method"></a>L'implementazione del metodo di azione Delete HTTP-POST

È ora disponibile la versione di HTTP-GET di implementato il metodo di azione Delete che viene visualizzata una schermata di conferma delete. Quando un utente finale fa clic sul pulsante "Elimina" eseguirà un post dei form per il */Dinners/Dinner / [id]* URL.

Verrà ora implementata il comportamento HTTP "POST" del metodo di azione delete usando il codice seguente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

La versione HTTP-POST il metodo di azione Delete tenta di recuperare l'oggetto dinner da eliminare. Se non riesce a trovarla (perché è già stata eliminata) viene eseguito il rendering di modelli "NotFound". Se viene rilevata la cena, la elimina dal DinnerRepository. Esegue quindi il rendering di un modello "Eliminato".

Per implementare il modello "Eliminato" verrà fare clic con il metodo di azione del mouse e scegliere il menu di scelta rapida "Aggiungi visualizzazione". Illustreremo la vista "Eliminato" e averlo da un modello vuoto (e non accettano un oggetto modello fortemente tipizzato). Parte del contenuto HTML verrà quindi aggiunto a esso:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

E quando si esegue l'accesso alle applicazioni e il *"/ Dinners/Delete / [id]"* URL per una valida cena conferma eliminazione oggetto eseguirà il rendering nostro Dinner schermata, come di seguito:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Quando si fa clic sul pulsante "Elimina" viene eseguita una richiesta HTTP POST per il */Dinners/Delete / [id]* URL, che elimina la cena dal database e visualizzare il modello di visualizzazione "Eliminato":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Sicurezza dell'associazione del modello

Si sono affermato due diversi modi per usare le funzionalità di associazione di modelli predefinite di ASP.NET MVC. La prima di tutto utilizzando il metodo UpdateModel() per aggiornare le proprietà in un oggetto modello esistente e il secondo usando il supporto di ASP.NET MVC per gli oggetti modello in passati come parametri di metodo di azione. Entrambe queste tecniche sono molto potente ed estremamente utile.

Questa capacità implica la responsabilità. È importante essere sempre paranoico sulla sicurezza quando si accettano qualsiasi input dell'utente, e questo vale anche quando gli oggetti di associazione di input del form. È necessario prestare attenzione a HTML codificare sempre i valori immessi dall'utente per evitare attacchi intrusivi nel codice HTML e JavaScript e prestare attenzione alla attacchi SQL injection (Nota: si sta usando LINQ to SQL per la nostra applicazione, che codifica automaticamente i parametri per evitare questi tipi di attacchi). È mai consigliabile basarsi sulla convalida sul lato client da solo e utilizzare sempre la convalida sul lato server per proteggersi da pirati informatici che tentano di inviare valori falsi.

Un elemento di sicurezza aggiuntive per assicurarsi che si pensa quando si usano le funzionalità di associazione di ASP.NET MVC è l'ambito degli oggetti di binding. In particolare, si desidera assicurarsi di che comprendere le implicazioni di sicurezza delle proprietà che è consentita per essere associato e verificare che sia consentito solo le proprietà che devono essere effettivamente aggiornabile da un utente finale da aggiornare.

Per impostazione predefinita, il metodo UpdateModel() tenterà di aggiornare tutte le proprietà nell'oggetto modello che corrispondono ai valori di parametro form in ingresso. Analogamente, gli oggetti passati come parametri di metodo di azione anche per impostazione predefinita possono avere tutte le relative proprietà impostate tramite parametri del modulo.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Blocco dell'associazione in base a una base all'utilizzo

È possibile bloccare i criteri di associazione in base a una base all'utilizzo da fornendo un'esplicita "include elenco" di proprietà che possono essere aggiornate. Questa operazione può essere eseguita passando un parametro di matrice di stringa aggiuntiva al metodo UpdateModel() come di seguito:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Gli oggetti passati come parametri di metodo di azione inoltre supportano l'attributo [Bind] che consente a un "Includi elenco" di consentito specificare, ad esempio sotto le proprietà:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Blocco dell'associazione in base al tipo

È inoltre possibile bloccare le regole di associazione in base al tipo. In questo modo è possibile specificare le regole di associazione una sola volta e quindi li applica in tutti gli scenari (inclusi scenari di parametro di metodo sia UpdateModel e azione) tra tutti i controller e metodi di azione.

È possibile personalizzare le regole di associazione per tipo aggiungendo un attributo [Bind] in un tipo o registrandolo all'interno del file Global. asax dell'applicazione (utile per scenari in cui non si è proprietari di tipo). È quindi possibile usare Include l'associazione dell'attributo ed Exclude proprietà per controllare le proprietà che sono associabili per la particolare classe o interfaccia.

Si verrà usare questa tecnica per la classe Dinner nella nostra applicazione NerdDinner e aggiungere un attributo [Bind] che limita l'elenco di proprietà associabili ai seguenti:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Si noti che non si consente la raccolta di servizi deve essere modificato tramite l'associazione, né si consente il DinnerID o HostedBy proprietà da impostare tramite l'associazione. Per motivi di sicurezza è verrà invece solo modificare queste proprietà particolare usando codice esplicito entro i metodi di azione.

### <a name="crud-wrap-up"></a>CRUD conclusioni

ASP.NET MVC include numerose funzionalità che semplificano l'implementazione di scenari di registrazione di form. Abbiamo utilizzato un'ampia gamma di queste funzionalità per fornire il supporto dell'interfaccia utente CRUD sul nostro DinnerRepository.

Usiamo un approccio incentrato sul modello per implementare la nostra applicazione. Ciò significa che tutti i prodotti convalida e regole di business per la logica è definita entro il livello di modello e non nel nostro controller o viste. La classe Controller né i nostri modelli di visualizzazione che conosca le regole di business specifico imposta dalla nostra classe di modello Dinner.

Verrà mantenere pulito l'architettura delle applicazioni e rendono più semplice eseguire il test. È possibile aggiungere regole di business aggiuntive per il livello del modello in futuro e *non è necessario apportare modifiche al codice* al nostro Controller o visualizzazione affinché possano essere supportati. Ciò richiede di fornire una notevole flessibilità per sviluppare e modificare l'applicazione in futuro.

Nostro DinnersController ora consente a Dinner listati/dettagli, nonché creare, modificare ed eliminare il supporto. Il codice completo per la classe sono disponibili di seguito:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>Passo successivo

È ora disponibile il supporto basic CRUD (Create, Read, Update e Delete) a implementare la classe DinnersController.

Ora esaminiamo come utilizzare le classi di ViewData e ViewModel per abilitare ancora più dettagliato dell'interfaccia utente sul nostro form.

> [!div class="step-by-step"]
> [Precedente](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [Successivo](use-viewdata-and-implement-viewmodel-classes.md)
