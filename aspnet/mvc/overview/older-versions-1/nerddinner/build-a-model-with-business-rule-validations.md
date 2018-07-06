---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: Creare un modello con convalide delle regole Business | Microsoft Docs
author: microsoft
description: Passaggio 3 viene illustrato come creare un modello che sia possibile usare per entrambe le query e aggiornare il database per la nostra applicazione NerdDinner.
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: 4bed4dd794c7c34551cd3c7543e08ed12d83505a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836775"
---
<a name="build-a-model-with-business-rule-validations"></a>Creare un modello con convalide delle regole Business
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 3 di una liberazione [esercitazione sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-dettaglio come compilare una piccola, ma completa, applicazione web con ASP.NET MVC 1.
> 
> Passaggio 3 viene illustrato come creare un modello che sia possibile usare per entrambe le query e aggiornare il database per la nostra applicazione NerdDinner.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le [Guida introduttiva con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oppure [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner passaggio 3: Compilazione del modello

In un framework model-view-controller il termine "modello" si riferisce agli oggetti che rappresentano i dati dell'applicazione, nonché la logica di dominio corrispondente che integra la convalida e regole di business con esso. Il modello è per molti aspetti "base" di un'applicazione basata su MVC e come si vedrà in seguito fondamentalmente unità il comportamento di esso.

Il framework ASP.NET MVC supporta l'utilizzo di qualsiasi tecnologia di accesso ai dati e gli sviluppatori possono scegliere da un'ampia gamma di opzioni di dati .NET avanzate per implementare i propri modelli tra cui: LINQ to Entities, LINQ to SQL, NHibernate, LLBLGen Pro, SubSonic, WilsonORM o ADO non elaborato. NET DataReaders o set di dati.

Per la nostra applicazione NerdDinner si intende usare LINQ to SQL per creare un modello semplice che corrisponde in modo abbastanza preciso per la progettazione di database e aggiunge alcune regole di business e per la logica di convalida personalizzata. Quindi implementeremo una classe di repository che aiuta astratta da subito l'implementazione di persistenza dei dati dal resto dell'applicazione e consente di facilmente unit test.

### <a name="linq-to-sql"></a>LINQ to SQL

LINQ to SQL è un ORM (mapper relazionale a oggetti) che viene fornito come parte di .NET 3.5.

LINQ to SQL fornisce un modo semplice per eseguire il mapping di tabelle di database per le classi di .NET che è possibile codificarli. Per la nostra applicazione NerdDinner useremo, eseguire il mapping di tabelle Dinners e RSVP all'interno di database per le classi di Dinner e RSVP. Le colonne delle tabelle Dinners e RSVP verranno corrispondono alle proprietà per le classi di Dinner e RSVP. Ogni oggetto Dinner e RSVP rappresenterà una riga separata all'interno delle tabelle Dinners o RSVP nel database.

LINQ to SQL consente di evitare di dover creare manualmente le istruzioni SQL per recuperare e aggiornare la cena e RSVP oggetti con i dati del database. Le classi di Dinner e RSVP, invece, si definirà come vengono mappati al/dal database e le relazioni tra di essi. LINQ to SQL verrà quindi richiede attenzione di generare la logica di esecuzione SQL appropriata da utilizzare in fase di esecuzione quando si interagisce e usarli.

È possibile usare il supporto del linguaggio LINQ in Visual Basic e c# per scrivere le query espressive che recuperano Dinner e RSVP oggetti dal database. Ciò riduce al minimo la quantità di codice ai dati è necessario scrivere, e consente di compilare applicazioni effettivamente pulite.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Aggiungere classi LINQ to SQL al progetto

Verrà iniziare facendo clic sulla cartella "Models" all'interno del progetto e selezionare il **Add -&gt;nuovo elemento** comando di menu:

![](build-a-model-with-business-rule-validations/_static/image1.png)

Verrà visualizzata la finestra di dialogo "Aggiungi nuovo elemento". Verrà filtrare in base alla categoria "Dati" e selezionare il modello "Classi di LINQ a SQL" all'interno di esso:

![](build-a-model-with-business-rule-validations/_static/image2.png)

Si sarà denominare l'elemento "NerdDinner" e fare clic sul pulsante "Aggiungi". Visual Studio verrà aggiunto un file NerdDinner.dbml sotto la directory \Models e quindi aperto LINQ da Progettazione relazionale oggetti SQL:

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>La creazione classi di modello di dati con LINQ to SQL

LINQ to SQL consente di creare rapidamente le classi di modello di dati dallo schema di database esistente. Cose da fare ciò si sarà aprire il database di NerdDinner in Esplora Server e selezionare le tabelle si desidera modellare in esso:

![](build-a-model-with-business-rule-validations/_static/image4.png)

È quindi possibile trascinare le tabelle in LINQ all'area di progettazione di SQL. Quando si esegue questo LINQ to SQL creerà automaticamente la cena e classi RSVP usando lo schema delle tabelle (con proprietà della classe che eseguono il mapping alle colonne della tabella di database):

![](build-a-model-with-business-rule-validations/_static/image5.png)

Per impostazione predefinita, LINQ to SQL designer automaticamente "plurali" nomi di tabella e colonna durante la creazione di classi basate su uno schema di database. Ad esempio: la tabella "Dinners" dell'esempio precedente ha prodotto in una classe "Dinner". Questa classe di denominazione consente di creare i modelli coerente con le convenzioni di denominazione .NET e trova in genere che la sua la correzione della finestra di progettazione di pratico (in particolare quando si aggiunge un numero elevato di tabelle). Se non si desidera che il nome di una classe o proprietà che genera la finestra di progettazione, tuttavia, è sempre possibile eseguirne l'override e impostarlo su qualsiasi nome desiderato. È possibile farlo modificando le entità/proprietà nome inline all'interno della finestra di progettazione o modificarla tramite la griglia delle proprietà.

Per impostazione predefinita la progettazione di LINQ to SQL inoltre analizza le relazioni di chiave esterna/chiave primarie delle tabelle e basato su di essi automaticamente crea predefinito "le associazioni di relazione" tra le classi di modello diverso che viene creato. Ad esempio, quando abbiamo trascinato il Dinners e RSVP tabelle in LINQ to SQL designer un'associazione di relazione uno-a-molti tra i due è stato dedotto basata sul fatto che la tabella RSVP aveva una chiave esterna alla tabella Dinners (ciò viene indicato da una freccia di finestra di progettazione):

![](build-a-model-with-business-rule-validations/_static/image6.png)

L'associazione precedente causerà LINQ to SQL per aggiungere una proprietà di "Dinner" fortemente tipizzata per la classe RSVP che gli sviluppatori possono usare per accedere la cena associata a un determinato RSVP. Ciò comporterà anche la classe Dinner avere una proprietà di raccolta "RSVPs" che consente agli sviluppatori di recuperare e aggiornare gli oggetti RSVP associati a una particolare cena.

Di seguito è possibile vedere un esempio di intellisense in Visual Studio quando si crea un nuovo oggetto RSVP e aggiungerlo alla raccolta di servizi di un Dinner. Si noti come LINQ to SQL aggiunta automaticamente una raccolta di "Servizi" sull'oggetto Dinner:

![](build-a-model-with-business-rule-validations/_static/image7.png)

Aggiungendo l'oggetto RSVP alla raccolta di servizi di Dinner quale diciamo LINQ to SQL per associare una relazione di chiave esterna tra la cena e la riga RSVP nel nostro database:

![](build-a-model-with-business-rule-validations/_static/image8.png)

Se non sono quelli come la finestra di progettazione ha modellato o denominata di un'associazione di tabella, è possibile eseguirne l'override. È sufficiente fare clic sulla freccia rivolta verso associazione all'interno della finestra di progettazione e accedere alle relative proprietà tramite la griglia delle proprietà da rinominare, eliminare o modificarlo. Per la nostra applicazione NerdDinner, tuttavia, le regole di associazione predefiniti adatti per le classi di modello di dati che stiamo creando ed è possibile usare semplicemente il comportamento predefinito.

### <a name="nerddinnerdatacontext-class"></a>Classe NerdDinnerDataContext

Visual Studio creerà automaticamente le classi .NET che rappresentano i modelli e relazioni di database definite tramite LINQ to SQL designer. Una classe LINQ to SQL DataContext viene inoltre generato per ogni file LINQ to SQL progettazione aggiunto alla soluzione. Poiché è denominato nostro LINQ all'elemento di classe SQL "NerdDinner", la classe DataContext creata verrà chiamata "NerdDinnerDataContext". Questa classe NerdDinnerDataContext è il modo principale che si interagirà con il database.

La classe NerdDinnerDataContext espone due proprietà: "Dinners" e "RSVPs -" che rappresentano le due tabelle che viene modellati all'interno del database. È possibile usare c# per scrivere query LINQ rispetto a quelle proprietà per query e recuperare oggetti Dinner e RSVP dal database.

Il codice seguente viene illustrato come creare un'istanza di un oggetto NerdDinnerDataContext ed eseguire una query LINQ su di essa per ottenere una sequenza di Dinners che si verificano in futuro. Visual Studio offre funzionalità complete di intellisense durante la scrittura di query LINQ e gli oggetti restituiti da quest'ultimo sono fortemente tipizzati e supportano anche intellisense:

![](build-a-model-with-business-rule-validations/_static/image9.png)

Oltre a consentire di eseguire una query per gli oggetti Dinner e RSVP, un NerdDinnerDataContext rileva anche automaticamente eventuali modifiche successivamente apportate agli oggetti Dinner e RSVP che recuperiamo attraverso di esso. È possibile usare questa funzionalità per salvare facilmente le modifiche nel database, senza dover scrivere alcun codice di aggiornamento SQL esplicita.

Ad esempio, il codice seguente illustra come usare una query LINQ per recuperare un singolo oggetto Dinner dal database, aggiornare due delle proprietà Dinner e quindi salvare le modifiche nel database:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

L'oggetto NerdDinnerDataContext nel codice sopra rilevate automaticamente le modifiche alle proprietà apportate all'oggetto Dinner recuperato da quest'ultimo. Quando si chiama il metodo "SubmitChanges ()", eseguirà appropriato "Aggiorna" per un'istruzione SQL del database per rendere persistenti i valori aggiornati nuovamente.

### <a name="creating-a-dinnerrepository-class"></a>Creazione di una classe DinnerRepository

Applicazioni di piccole dimensioni è a volte avere i controller vengono applicati direttamente a una classe LINQ to SQL DataContext e incorporare le query LINQ entro i controller. Applicazioni di ottengono più grandi, tuttavia, questo approccio diventa complesso da gestire e testare. Può anche causare a noi la duplicazione delle stesse query LINQ in più posizioni.

Un approccio che può rendere più facile da gestire e testare le applicazioni consiste nell'utilizzare un modello di "repository". Una classe di repository consente di incapsulare eseguire query sui dati e logica di persistenza ed estratti da subito i dettagli di implementazione della persistenza dei dati dall'applicazione. Oltre a rendere più chiaro il codice dell'applicazione, usando un modello di repository rendono più semplice modificare le implementazioni di archiviazione di dati in futuro e può contribuire a semplificare gli unit test di un'applicazione senza la necessità di un database effettivo.

Per la nostra applicazione NerdDinner si definirà una classe DinnerRepository con la seguente firma:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Nota: In questo capitolo verranno estrarre un'interfaccia IDinnerRepository da questa classe e abilitare l'inserimento di dipendenze con esso ai controller. Per iniziare, però, si intende avviare semplice e limitarti a lavorare direttamente con la classe DinnerRepository.*

Per implementare questa classe verrà del mouse sulla cartella nostro "Models" e scegliere il **Add -&gt;nuovo elemento** comando di menu. Nella finestra di dialogo "Aggiungi nuovo elemento" è possibile selezionare il modello "Classe" e denominare il file "DinnerRepository.cs":

![](build-a-model-with-business-rule-validations/_static/image10.png)

È quindi possibile implementare la classe DinnerRespository usando il codice seguente:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Il recupero, aggiornamento, inserimento ed eliminazione utilizzando la classe DinnerRepository

Ora che è stata creata la classe DinnerRepository, esaminiamo alcuni esempi di codice che illustrano le attività comuni che consente di eseguire è:

#### <a name="querying-examples"></a>Esempi di query

Il codice seguente recupera una singola cena usando il valore DinnerID:


[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

Il codice seguente recupera tutti i cicli e dinners imminente su di essi:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>Esempi di aggiornamento e inserimento

Il codice seguente viene illustrato come aggiungere due nuove dinners. Le aggiunte/modifiche nel repository non sono il commit nel database fino a quando non viene chiamato il metodo "Save ()" su di esso. LINQ to SQL esegue automaticamente il wrapping tutte le modifiche in una transazione di database, in modo che si verificano tutte le modifiche o nessuno di essi è quando si salva il repository:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

Il codice seguente recupera un oggetto Dinner esistente e modifica di due proprietà su di esso. Le modifiche vengono salvate nel database quando viene chiamato il metodo "Save ()" nel repository:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

Il codice seguente recupera un dinner e quindi aggiunge un RSVP ad esso. Ciò avviene utilizzando l'insieme di servizi per l'oggetto Dinner che LINQ to SQL creato per noi (poiché non esiste una relazione di chiave/esterna-chiave primaria tra i due oggetti nel database). Questa modifica è persistente nel database come nuova riga della tabella RSVP quando viene chiamato il metodo "Save ()" nel repository:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Esempio di Delete

Il codice seguente recupera un oggetto Dinner esistente e contrassegna quindi che venga eliminato. Quando viene chiamato il metodo "Save ()" nel repository eseguirà il commit di eliminazione nel database:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>L'integrazione di convalida e logica della regola Business con le classi del modello

L'integrazione di convalida e regole di business per la logica è una parte fondamentale di qualsiasi applicazione che funziona con i dati.

#### <a name="schema-validation"></a>Convalida dello schema

Quando le classi del modello vengono definite utilizzando LINQ to SQL designer, i tipi di dati delle proprietà nelle classi del modello di dati corrispondono ai tipi di dati della tabella di database. Ad esempio: se la colonna "L'elemento EventDate" nella tabella Dinners "datetime", la classe di modello di dati creata da LINQ to SQL sarà di tipo "DateTime" (che è un tipo di dati .NET incorporato). Ciò significa che si verificheranno errori di compilazione se si tenta di assegnare un numero intero o un valore booleano a esso dal codice e verrà generato automaticamente alcun errore se si prova a convertire implicitamente un tipo stringa non valido a esso in fase di esecuzione.

LINQ to SQL verrà gestisce anche automaticamente i valori dei caratteri di escape di SQL per l'utente quando si utilizzano stringhe - che consente di proteggere è da attacchi SQL injection quando viene usato.

#### <a name="validation-and-business-rule-logic"></a>Convalida e logica della regola Business

Convalida dello schema può essere utilizzata come primo passaggio, ma è raramente sufficiente. La maggior parte degli scenari reali richiedono la possibilità di specificare la logica di convalida più completa che può estendersi su più proprietà, eseguire il codice e spesso essere a conoscenza dello stato del modello (ad esempio: viene creato/aggiornato/eliminato, o in uno stato specifico di dominio ad esempio "archiviate"). Sono disponibili un'ampia gamma di Framework che può essere utilizzato per definire e applicare le regole di convalida alle classi di modello e modelli differenti, e sono presenti diversi basati su .NET Framework disponibili che può essere utilizzato per facilitare questa operazione. È possibile usare praticamente qualsiasi di esse all'interno delle applicazioni ASP.NET MVC.

Ai fini della nostra applicazione NerdDinner, si userà un modello relativamente semplice e diretta in cui è esporre una proprietà IsValid e un metodo GetRuleViolations() nell'oggetto modello cena. La proprietà IsValid restituirà true o false a seconda del fatto che le regole di convalida e business sono tutti valide. Il metodo GetRuleViolations() restituirà un elenco di eventuali errori di regole.

Implementeremo IsValid e GetRuleViolations() per il nostro modello cena mediante l'aggiunta di una classe"parziale" per il progetto. Classi parziali possono essere utilizzate per aggiungere metodi/proprietà/eventi alle classi gestite da una finestra di progettazione di Visual Studio (ad esempio, la classe Dinner generata da LINQ to SQL designer) e consentono di evitare lo strumento da barcamenarsi tra il nostro codice. È possibile aggiungere una nuova classe parziale per il progetto facendo clic sulla cartella \Models e quindi selezionare il comando di menu "Aggiungi nuovo elemento". È quindi possibile scegliere il modello "Classe" nella finestra di dialogo "Aggiungi nuovo elemento" e denominarlo Dinner.cs.

![](build-a-model-with-business-rule-validations/_static/image11.png)

Facendo clic sul pulsante "Aggiungi" file verrà aggiunto un Dinner.cs per il progetto e aprirlo all'interno dell'IDE. È quindi possibile implementare un framework applicazione regola/convalida di base usando il codice riportato di seguito:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Alcune note relative al codice precedente:

- La classe Dinner è preceduta da una parola chiave "parziale", ovvero il codice in esso contenuto verrà combinato con la classe generati/gestiti per la progettazione di LINQ to SQL e compilato in una singola classe.
- La classe RuleViolation è una classe di supporto che verrà aggiunto al progetto che consente di fornire ulteriori dettagli sulla violazione di una regola.
- Il metodo Dinner.GetRuleViolations() causa la convalida e le regole business da valutare (che verrà implementato li a breve). Quindi restituisce nuovamente una sequenza di oggetti RuleViolation che forniscono altri dettagli su eventuali errori di regole.
- La proprietà Dinner.IsValid fornisce una proprietà di helper utili che indica se l'oggetto Dinner ha qualsiasi RuleViolations attivo. È possibile controllare in modo proattivo da uno sviluppatore che usa l'oggetto cena in qualsiasi momento (e non genera un'eccezione).
- Il metodo parziale Dinner.OnValidate() è una funzione hook fornite da LINQ to SQL che consente di ricevere una notifica ogni volta che l'oggetto Dinner sta per essere resi persistenti all'interno del database. La nostra implementazione OnValidate () sopra riportato assicura che la cena non abbia alcun RuleViolations prima che venga salvato. Se si trova in uno stato non valido genera un'eccezione, che provoca il LINQ a SQL per interrompere la transazione.

Questo approccio fornisce un framework semplice che è possibile integrare la convalida e regole di business in. Per ora è possibile aggiungere le regole al nostro metodo GetRuleViolations() seguenti:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Utilizziamo la funzionalità del linguaggio c# "yield return" per restituire una sequenza di qualsiasi RuleViolations. I controlli prima di sei regola precedente impongono semplicemente che le proprietà della stringa sul nostro Dinner non possono essere null o vuoto. L'ultima regola è un po' più interessante e chiama un metodo helper PhoneValidator.IsValidNumber() che è possibile aggiungere al progetto per verificare che il ContactPhone numero paese del formato corrispondenze la cena.

È possibile usare. Supporto delle espressioni regolari di NET per implementare questo supporto della convalida phone. Di seguito è una semplice implementazione PhoneValidator che possiamo aggiungere al progetto che permette di aggiungere i controlli modello di espressione regolare specifiche del paese:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>La gestione delle violazioni per la logica di Business e convalida

Ora che abbiamo aggiunto la convalida e business regola codice precedente, qualsiasi tentativo di creare o aggiornare un Dinner, verranno valutate e applicate le regole per la logica di convalida.

Gli sviluppatori possono scrivere il codice come di seguito per determinare in modo proattivo se un oggetto Dinner è valido e recuperare un elenco di tutte le violazioni in esso senza generare qualsiasi eccezione:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Se si prova a salvare una cena in uno stato non valido, verrà generata un'eccezione quando si chiama il metodo Save () per il DinnerRepository. Ciò si verifica perché LINQ to SQL chiama automaticamente il metodo parziale Dinner.OnValidate() prima Salva le modifiche di Dinner e abbiamo aggiunto codice alla Dinner.OnValidate() per generare un'eccezione in presenza di eventuali violazioni delle regole nella cena. È possibile rilevare questa eccezione e riuscirà a recuperare un elenco di violazioni per risolvere:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Poiché le regole di convalida e business vengono implementate all'interno del livello del modello e non all'interno del livello dell'interfaccia utente, verranno applicate e usate in tutti gli scenari all'interno dell'applicazione. Siamo in un secondo momento possiamo modificare o aggiungere le regole di business e abbiamo tutto il codice che funziona con gli oggetti Dinner viene riconosciuto da.

Con la flessibilità necessaria per modificare le regole di business in un'unica posizione, senza la necessità di queste modifiche ripple in tutta l'applicazione e per la logica dell'interfaccia utente, è un segno di un'applicazione ben scritta e dei vantaggi che favorisce un framework MVC.

### <a name="next-step"></a>Passo successivo

A questo punto, abbiamo un modello che è possibile usare per eseguire una query sia aggiornare il database.

Ora aggiungiamo alcuni controlli e visualizzazioni per il progetto che è possibile usare per creare un'esperienza dell'interfaccia utente HTML intorno a esso.

> [!div class="step-by-step"]
> [Precedente](create-a-database.md)
> [Successivo](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
