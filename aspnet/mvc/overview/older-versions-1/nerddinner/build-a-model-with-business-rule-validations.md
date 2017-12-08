---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: Compilare un modello con la convalida di regole Business | Documenti Microsoft
author: microsoft
description: "Passaggio 3 viene illustrato come creare un modello che è possibile utilizzare per entrambe le query e aggiornare il database per l'applicazione NerdDinner."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: dbe6370979f218988c168df3e80314ef9b338fbd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="build-a-model-with-business-rule-validations"></a>Compilare un modello con la convalida di regole Business
====================
da [Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Si tratta di passaggio 3 di una liberazione [esercitazione applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-through come compilare un piccolo, ma completa, un'applicazione web con ASP.NET MVC 1.
> 
> Passaggio 3 viene illustrato come creare un modello che è possibile utilizzare per entrambe le query e aggiornare il database per l'applicazione NerdDinner.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire il [recupero avviato con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [negozio MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner passaggio 3: Compilazione del modello

In un framework model-view-controller il termine "modello" si riferisce agli oggetti che rappresentano i dati dell'applicazione, nonché la logica di dominio corrispondente che integra la convalida e regole di business con esso. Il modello è in molti modi "base" di un'applicazione basata su MVC e come vedremo in un secondo momento fondamentalmente unità il comportamento di esso.

Il framework di MVC ASP.NET supporta l'utilizzo di qualsiasi tecnologia di accesso ai dati e gli sviluppatori possono scegliere da una varietà di disponibili opzioni di dati .NET per implementare i propri modelli tra cui: LINQ to Entities, LINQ to SQL, NHibernate, LLBLGen Pro, SubSonic, WilsonORM o ADO non elaborato. NET DataReader o set di dati.

Per l'applicazione NerdDinner verrà usare LINQ to SQL per creare un modello semplice che corrisponde strettamente abbastanza per il progetto di database e aggiunge alcune regole di business e di logica di convalida personalizzata. Verrà quindi implementata una classe di repository che consente di stoccaggio astratta l'implementazione di persistenza di dati dal resto dell'applicazione e consente di facilmente unit test.

### <a name="linq-to-sql"></a>LINQ to SQL

LINQ to SQL è una ORM (mapping relazionale a oggetti) che viene fornito come parte di .NET 3.5.

LINQ to SQL fornisce un modo semplice per eseguire il mapping di tabelle di database per le classi .NET, che è possibile codificarli. Per l'applicazione NerdDinner useremo si per eseguire il mapping delle tabelle Dinners e RSVP all'interno di questo database Dinner e RSVP classi. Le colonne delle tabelle Dinners e RSVP verranno corrispondono alle proprietà per le classi di Dinner e RSVP. Ogni oggetto Dinner e RSVP rappresenterà una riga separata all'interno delle tabelle Dinners o RSVP nel database.

LINQ to SQL consente di evitare di dover creare manualmente le istruzioni SQL per recuperare e aggiornare Dinner e RSVP oggetti con i dati di database. È invece, verranno definite le classi di Dinner e RSVP, come viene eseguito il mapping/dal database e le relazioni tra di essi. LINQ to SQL verrà viene quindi attenzione di generare la logica di esecuzione SQL appropriata da utilizzare in fase di esecuzione quando è interagire e utilizzarli.

È possibile utilizzare il supporto LINQ in Visual Basic e c# per scrivere query espressiva Dinner e RSVP di recuperare gli oggetti del database. Questo riduce la quantità di codice ai dati è necessario scrivere, e consente di compilare applicazioni effettivamente pulite.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Aggiungere classi LINQ to SQL al progetto

Verrà innanzitutto facendo clic sulla cartella "Modelli" all'interno del progetto e selezionare il **Add -&gt;nuovo elemento** comando di menu:

![](build-a-model-with-business-rule-validations/_static/image1.png)

Verrà visualizzata la finestra di dialogo "Aggiungi nuovo elemento". Si sarà filtrare in base alla categoria "Dati" e selezionare il modello "LINQ per classi di SQL" all'interno di essa:

![](build-a-model-with-business-rule-validations/_static/image2.png)

Viene denominare l'elemento "NerdDinner" e fare clic sul pulsante "Aggiungi". Visual Studio file verrà aggiunto un NerdDinner.dbml sotto la directory \Models e quindi aprire LINQ to Progettazione relazionale oggetti SQL:

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>Creazione classi del modello di dati con LINQ to SQL

LINQ to SQL consente di creare rapidamente le classi di modello di dati dallo schema di database esistente. Cose da fare ciò è possibile aprire il database NerdDinner in Esplora Server e selezionare le tabelle a cui si vuole del modello in essa contenuti:

![](build-a-model-with-business-rule-validations/_static/image4.png)

È quindi possibile trascinare le tabelle nella LINQ all'area di progettazione di SQL. Quando si esegue questo LINQ to SQL vengono create automaticamente Dinner e classi RSVP utilizzando lo schema delle tabelle (con le proprietà di classe che eseguono il mapping alle colonne della tabella di database):

![](build-a-model-with-business-rule-validations/_static/image5.png)

Per impostazione predefinita di LINQ to SQL designer "pluralizza automaticamente" nomi di tabella e colonna durante la creazione di classi basate su uno schema di database. Ad esempio: la tabella "Dinners" dell'esempio precedente ha generato una classe "Dinner". Questa classe di denominazione consente di creare i modelli coerente con le convenzioni di denominazione di .NET e in genere è possibile individuare quelli aventi la correzione della finestra di progettazione questo backup pratico (in particolare quando si aggiunge un numero elevato di tabelle). Se non si desidera che il nome di una classe o proprietà che genera la finestra di progettazione, tuttavia, è possibile eseguirne l'override e impostarlo su qualsiasi nome desiderato. Ciò si realizza tramite la modifica di entità o la proprietà nome in linea all'interno di progettazione o modificarla tramite la griglia delle proprietà.

Per impostazione predefinita, LINQ to SQL designer anche analizza le relazioni di chiave esterna con chiave/primarie delle tabelle e basati su di essi automaticamente create predefinito "relazione associazioni" tra le classi di modello diverso, che viene creato. Ad esempio, quando viene trascinato il Dinners e RSVP le tabelle in LINQ to SQL designer associazione una relazione uno-a-molti tra i due è stato dedotto basata sul fatto che la tabella RSVP aveva una chiave esterna alla tabella Dinners (questo è indicato da una freccia di finestra di progettazione):

![](build-a-model-with-business-rule-validations/_static/image6.png)

L'associazione precedente causerà LINQ to SQL per aggiungere una proprietà "Dinner" fortemente tipizzata per la classe RSVP che gli sviluppatori possono utilizzare per accedere la cena associata a un determinato RSVP. Ciò comporterà anche la classe Dinner disponga di una proprietà di raccolta "RSVPs" che consente agli sviluppatori di recuperare e aggiornare gli oggetti RSVP associati a una particolare cena.

Di seguito è possibile visualizzare un esempio di intellisense in Visual Studio quando si crea un nuovo oggetto RSVP e aggiungerlo a inviate risposte raccolta di una cena. Si noti come LINQ to SQL aggiunto automaticamente un insieme di "Inviate risposte" sull'oggetto Dinner:

![](build-a-model-with-business-rule-validations/_static/image7.png)

Aggiungendo l'oggetto RSVP alla raccolta inviate risposte del Dinner si definiranno LINQ to SQL per associare una relazione di chiave esterna tra la cena e la riga RSVP nel database:

![](build-a-model-with-business-rule-validations/_static/image8.png)

Se non sono come la finestra di progettazione è modellata o denominata di un'associazione di tabella, è possibile eseguirne l'override. È sufficiente fare clic sulla freccia rivolta verso associazione all'interno di progettazione e accedere alle relative proprietà tramite la griglia delle proprietà da rinominare, eliminare o modificarlo. Per l'applicazione NerdDinner, tuttavia, le regole di associazione predefinito funzionano anche per le classi di modello di dati che si sta creando ed è possibile utilizzare solo il comportamento predefinito.

### <a name="nerddinnerdatacontext-class"></a>Classe NerdDinnerDataContext

Visual Studio crea automaticamente le classi .NET che rappresentano i modelli e relazioni di database definite tramite LINQ to SQL designer. Una classe LINQ to SQL DataContext viene inoltre generato per ogni file LINQ to SQL progettazione aggiunto alla soluzione. Poiché il nostro LINQ all'elemento classe SQL "NerdDinner" è un nome, la classe DataContext creata verrà chiamata "NerdDinnerDataContext". Questa classe NerdDinnerDataContext è il modo principale che si interagisce con il database.

La classe NerdDinnerDataContext espone due proprietà: "Dinners" e "RSVPs -" che rappresentano le due tabelle che viene modellati all'interno del database. È possibile usare c# per scrivere query LINQ per le proprietà per gli oggetti query e recuperare Dinner e RSVP dal database.

Il codice seguente viene illustrato come creare un'istanza di un oggetto NerdDinnerDataContext ed eseguire una query LINQ per utilizzarlo per ottenere una sequenza di Dinners che si verificano in futuro. Visual Studio fornisce intellisense completo durante la scrittura di query LINQ e gli oggetti restituiti da quest'ultimo sono fortemente tipizzati e supportano anche intellisense:

![](build-a-model-with-business-rule-validations/_static/image9.png)

Oltre a consentire di eseguire una query per gli oggetti Dinner e RSVP, un NerdDinnerDataContext rileva anche automaticamente qualsiasi modifiche successivamente apportate agli oggetti Dinner e RSVP che è recuperare attraverso di esso. È possibile usare questa funzionalità di salvare facilmente le modifiche al database, senza dover scrivere alcun codice di aggiornamento SQL esplicita.

Ad esempio, il codice riportato di seguito viene illustrato come utilizzare una query LINQ per recuperare un singolo oggetto Dinner dal database, aggiornare due delle proprietà Dinner e quindi salvare le modifiche al database:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

L'oggetto NerdDinnerDataContext nel codice sopra rilevate automaticamente le modifiche alle proprietà apportate all'oggetto Dinner che è recuperato da esso. Quando si chiama il metodo "SubmitChanges ()", eseguirà un'istruzione di "Aggiorna" SQL appropriata al database per rendere persistenti i valori aggiornati nuovamente.

### <a name="creating-a-dinnerrepository-class"></a>Creazione di una classe DinnerRepository

Applicazioni di piccole dimensioni è talvolta avere controller operare direttamente una classe LINQ to SQL DataContext e incorporare le query LINQ entro i controller. Come applicazioni di ottengono maggiore, tuttavia, questo approccio diventa eccessivamente complesso per gestire e testare. Può inoltre comportare averci duplicare le stesse query LINQ in più posizioni.

Un approccio che può rendere più facile da gestire e testare le applicazioni consiste nell'utilizzare un modello "repository". Una classe di repository consente di incapsulare l'esecuzione di query di dati e logica di persistenza e riassunti immediatamente i dettagli di implementazione di persistenza dei dati dall'applicazione. Oltre a rendere più chiara codice dell'applicazione, utilizzando un modello di repository può rendere più semplice modificare le implementazioni di archiviazione di dati in futuro, e ciò consente di facilitare gli unit test di un'applicazione senza che sia necessario un database reale.

Per l'applicazione NerdDinner definiamo una classe DinnerRepository con la seguente firma:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Nota: Più avanti in questo capitolo è sarà estrarre un'interfaccia IDinnerRepository da questa classe e abilitare l'inserimento di dipendenze con il controller. Per iniziare, tuttavia, verrà avviare semplice e funzionano direttamente con la classe DinnerRepository.*

Per implementare questa classe verrà pulsante destro del mouse sulla cartella nostro "Modelli" e scegliere il **Add -&gt;nuovo elemento** comando di menu. Nella finestra di dialogo "Aggiungi nuovo elemento" è possibile selezionare il modello "Classe" e denominare il file "DinnerRepository.cs":

![](build-a-model-with-business-rule-validations/_static/image10.png)

È quindi possibile implementare la classe DinnerRespository utilizzando il codice riportato di seguito:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Il recupero, l'aggiornamento, inserimento ed eliminazione di utilizzo della classe DinnerRepository

Dopo aver creato la classe DinnerRepository, ecco alcuni esempi di codice che illustrano le attività comuni che possiamo fare con esso:

#### <a name="querying-examples"></a>Esempi di query

Il codice seguente recupera un singolo Dinner utilizzando il valore DinnerID:


[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

Il codice seguente recupera tutti i cicli e dinners future su di essi:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>INSERT e Update esempi

Il codice riportato di seguito viene illustrato come aggiungere due dinners di nuovo. Le aggiunte o modifiche nel repository non sono salvate nel database fino a quando non viene chiamato il metodo "Save ()". LINQ to SQL automaticamente esegue il wrapping di tutte le modifiche in una transazione di database, pertanto tutte le modifiche vengono apportate o nessuno di essi quando si salva il repository:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

Il codice seguente recupera un oggetto Dinner esistente e modifica di due proprietà su di esso. Le modifiche vengono salvate nel database quando viene chiamato il metodo "Save ()" per il repository:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

Il codice seguente recupera una cena e quindi aggiunge un RSVP. Ciò avviene tramite la raccolta inviate risposte sull'oggetto Dinner LINQ to SQL creata per noi (perché non esiste una relazione di chiave/esterna-chiave primaria tra i due oggetti nel database). Questa modifica viene mantenuta nel database come una nuova riga nella tabella RSVP quando viene chiamato il metodo "Save ()" per il repository:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Esempio di Elimina

Il codice seguente recupera un oggetto esistente di Dinner e quindi viene contrassegnato da eliminare. Quando viene chiamato il metodo "Save ()" per il repository eseguirà il commit dell'eliminazione al database:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>L'integrazione di convalida e logica della regola Business con le classi del modello

Integrazione di convalida e regole di business logica è una componente fondamentale di qualsiasi applicazione che funziona con i dati.

#### <a name="schema-validation"></a>Convalida dello schema

Quando le classi del modello vengono definite utilizzando LINQ to SQL designer, i tipi di dati delle proprietà nelle classi del modello di dati corrispondono ai tipi di dati della tabella di database. Ad esempio: se la colonna "EventDate" nella tabella Dinners è "datetime", la classe di modello di dati creata da LINQ to SQL sarà di tipo "DateTime" (che è un tipo di dati .NET incorporato). Ciò significa che si verificheranno degli errori di compilazione se si tenta di assegnare un numero intero o un valore booleano a esso dal codice e genererà un errore automaticamente se si tenta di convertire in modo implicito un tipo stringa non valido a esso in fase di esecuzione.

Quando si utilizzano stringhe - che consente di proteggere è da attacchi SQL injection in uso, i valori dei caratteri di escape di SQL verranno gestisce automaticamente anche nella LINQ to SQL.

#### <a name="validation-and-business-rule-logic"></a>Convalida e logica della regola Business

Convalida dello schema è utile come primo passaggio, ma è sufficiente raramente. La maggior parte degli scenari reali richiedono la possibilità di specificare la logica di convalida più completa che può estendersi su più proprietà, eseguire codice e hanno spesso la consapevolezza di stato di un modello (ad esempio: viene creato aggiornate/eliminate, o in uno stato specifico di dominio ad esempio "archiviazione"). Esistono una serie di motivi diversi e Framework che può essere utilizzato per definire e applicare le regole di convalida alle classi di modello e sono disponibili diversi basata su .NET Framework prevedono che può essere usato per aiutare a raggiungere questo. È possibile utilizzare pressoché ciascuno di essi all'interno delle applicazioni ASP.NET MVC.

Ai fini dell'applicazione NerdDinner, si userà un modello relativamente semplice e semplice in cui è necessario esporre una proprietà IsValid e un metodo GetRuleViolations() dell'oggetto modello Dinner. La proprietà IsValid restituirà true o false a seconda se la convalida e regole business sono tutti valide. Il metodo GetRuleViolations() restituirà un elenco di eventuali errori di regole.

È quindi implementato IsValid e GetRuleViolations() per il nostro modello Dinner aggiungendo una classe"parziale" al progetto. Classi parziali possono essere utilizzate per aggiungere metodi/proprietà/eventi alle classi gestite da una finestra di progettazione di Visual Studio (ad esempio, la classe Dinner generata da LINQ to SQL designer) e consentono di evitare lo strumento da creare con il nostro codice. È possibile aggiungere una nuova classe parziale per il progetto facendo clic sulla cartella \Models e quindi selezionare il comando di menu "Aggiungi nuovo elemento". È quindi possibile scegliere il modello "Classe" nella finestra di dialogo "Aggiungi nuovo elemento" e denominarlo Dinner.cs.

![](build-a-model-with-business-rule-validations/_static/image11.png)

Fare clic sul pulsante "Aggiungi" file verrà aggiunto un Dinner.cs al progetto e aprirlo all'interno dell'IDE. È quindi possibile implementare una convalida di regole base di imposizione di framework utilizzando il codice riportato di seguito:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Alcune note relative al codice precedente:

- La classe Dinner è preceduta da una parola chiave "parziale", ovvero il codice in esso contenuto verrà combinato con la classe generata gestiti da LINQ to SQL designer e compilato in una singola classe.
- La classe RuleViolation è una classe helper che verrà aggiunto al progetto che consente di fornire ulteriori dettagli su una violazione delle regole.
- Il metodo Dinner.GetRuleViolations() causa la convalida e regole business da valutare (è possibile implementarli a breve). Restituisce quindi nuovamente una sequenza di oggetti RuleViolation che forniscono ulteriori dettagli su eventuali errori della regola.
- La proprietà Dinner.IsValid fornisce una proprietà di helper utili che indica se l'oggetto Dinner dispone di qualsiasi RuleViolations attivo. È possibile verificare in modo proattivo per gli sviluppatori che utilizzano l'oggetto cena in qualsiasi momento e non genera un'eccezione.
- Il metodo parziale Dinner.OnValidate() è un hook di LINQ to SQL fornisce che consente di ricevere una notifica ogni volta che l'oggetto Dinner sta per essere mantenuta all'interno del database. L'implementazione di OnValidate () precedente assicura che la cena non abbia alcun RuleViolations prima che venga salvato. Se si trova in uno stato non valido genera un'eccezione, che può causare errori di LINQ to SQL per interrompere la transazione.

Questo approccio fornisce un framework semplice che è possibile integrare la convalida e regole di business in. Per ora aggiungere le regole seguenti per il metodo GetRuleViolations():

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Si sta usando la funzionalità "yield return" di c# per restituire una sequenza di qualsiasi RuleViolations. I controlli prima di sei regola precedente applicano semplicemente che le proprietà della stringa nel nostro Dinner non possono essere null o vuoto. Chiama un metodo di supporto PhoneValidator.IsValidNumber() che è possibile aggiungere al progetto per verificare che il ContactPhone numero paese del formato corrispondenze la cena ultima regola è più interessante.

È possibile usare. Supporto delle espressioni regolari di NET per implementare questo supporto di convalida del telefono. Di seguito è un'implementazione PhoneValidator semplice che è possibile aggiungere al progetto che consente di aggiungere i controlli di modello Regex specifiche del paese:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Gestione di convalida e violazioni di logica di Business

Ora che è stato aggiunto il codice sopra riportato convalida e di business regola, qualsiasi tentativo di creare o aggiornare una cena, verranno valutate e applicate le regole per la convalida.

Gli sviluppatori possono scrivere il codice come di seguito per determinare in anticipo se un oggetto Dinner è valido e recuperare un elenco di tutte le violazioni senza generare eccezioni in essa contenuti:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Se si tenta di salvare una cena in uno stato non valido, verrà generata un'eccezione quando si chiama il metodo Save () sul DinnerRepository. Questo errore si verifica poiché LINQ to SQL chiama il metodo parziale Dinner.OnValidate() automaticamente prima Salva le modifiche di Dinner e viene aggiunto il codice Dinner.OnValidate() per generare un'eccezione in presenza di eventuali violazioni delle regole nella cena. È possibile rilevare questa eccezione ed riuscirà a recuperare un elenco di correggere le violazioni di:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Poiché la convalida e regole business vengono implementate all'interno di questo livello di modello e non all'interno del livello di interfaccia utente, essi saranno applicati e usate in tutti gli scenari all'interno dell'applicazione. In un secondo momento è possibile modificare o aggiungere regole business e dispone di tutto il codice che può essere utilizzata con gli oggetti Dinner rispetta tali.

Con la possibilità di modificare le regole di business in un'unica posizione, senza la necessità di queste modifiche ripple in tutta l'applicazione e la logica dell'interfaccia utente, è un segno di un'applicazione ben scritta e un vantaggio che è un framework MVC consente di promuovere.

### <a name="next-step"></a>Passo successivo

Ora abbiamo un modello che è possibile utilizzare per eseguire una query sia aggiornare il database.

Ora aggiungere alcuni controller e visualizzazioni per il progetto che è possibile utilizzare per creare un'esperienza di interfaccia utente HTML intorno a esso.

>[!div class="step-by-step"]
[Precedente](create-a-database.md)
[Successivo](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
