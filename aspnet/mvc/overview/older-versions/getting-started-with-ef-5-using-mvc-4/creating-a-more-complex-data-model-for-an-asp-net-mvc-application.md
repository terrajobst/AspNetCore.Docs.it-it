---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: Creazione di un modello di dati più complesso per un'applicazione ASP.NET MVC (4 di 10) | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: f81f3d80-3674-4d8e-a9b1-87feed1a93c9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: beda9585a3050e35c250d0cb7ab1dc8a464efa3d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816243"
---
<a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application-4-of-10"></a>Creazione di un modello di dati più complesso per un'applicazione ASP.NET MVC (4 di 10)
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto completato](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni a partire dall'inizio oppure [scarica un progetto iniziale per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si verifica un problema è possibile risolvere, [Scarica il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. Confrontando il codice per il codice completo è generalmente possibile trovare la soluzione al problema. Per alcuni errori comuni e come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Nelle esercitazioni precedenti si è lavorato con un semplice modello di dati che è stato composto da tre entità. In questa esercitazione si aggiungeranno altre entità e relazioni e si personalizzerà il modello di dati specificando regole di formattazione, convalida e database mapping. Si vedrà due modi per personalizzare il modello di dati: aggiungendo attributi alle classi di entità e aggiungendo codice alla classe contesto di database.

Al termine dell'operazione le classi di entità verranno incluse nel modello di dati completato, illustrato nella figura seguente:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Personalizzare il modello di dati usando gli attributi

In questa sezione si apprenderà come personalizzare il modello di dati usando attributi che specificano regole di formattazione, convalida e mapping del database. Quindi in diverse delle sezioni seguenti si creerà l'intero `School` modello di dati aggiungendo attributi alle classi è già create e la creazione di nuove classi per i tipi di entità rimanenti nel modello.

### <a name="the-datatype-attribute"></a>L'attributo di tipo di dati

Per le date di iscrizione degli studenti, tutte le pagine Web attualmente visualizzano l'ora oltre alla data, anche se l'unico elemento rilevante di questo campo è la data. Mediante gli attributi di annotazione dei dati è possibile modificare il codice per correggere il formato di visualizzazione in tutte le visualizzazioni che visualizzano i dati. Per un esempio di come eseguire questa operazione si aggiunge un attributo alla proprietà `EnrollmentDate` nella classe `Student`.

In *Models\Student.cs*, aggiungere un `using` istruzione per il `System.ComponentModel.DataAnnotations` dello spazio dei nomi e aggiungere `DataType` e `DisplayFormat` gli attributi di `EnrollmentDate` proprietà, come illustrato nell'esempio seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,13-14)]

Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributo viene usato per specificare un tipo di dati che è più specifico del tipo intrinseco del database. In questo caso si vuole tenere traccia solo della data e non di data e ora. Il [enumerazione DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) fornisce per molti tipi di dati, ad esempio *Date, Time, PhoneNumber, Currency, EmailAddress* e altro ancora. L'attributo `DataType` può anche consentire all'applicazione di fornire automaticamente le funzionalità specifiche del tipo. Ad esempio, un `mailto:` possibile creare un collegamento per [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), e un selettore data può essere fornito per [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) nei browser che supportano [HTML5](http://html5.org/). Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributi emette HTML 5 [data -](http://ejohn.org/blog/html-5-data-attributes/) (si pronuncia *dash di dati*) gli attributi HTML browser HTML5. Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributi non forniscono alcuna convalida.

`DataType.Date` non specifica il formato della data visualizzata. Per impostazione predefinita, viene visualizzato il campo dati in base ai formati predefiniti del server [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

L'attributo `DisplayFormat` viene usato per specificare in modo esplicito il formato della data:


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


Il `ApplyFormatInEditMode` impostazione specifica che la formattazione specificata deve essere applicata anche quando il valore viene visualizzato in una casella di testo per la modifica. (Non è consigliabile che per alcuni campi, ad esempio, per i valori di valuta, è possibile evitare il simbolo di valuta nella casella di testo per la modifica.)

È possibile usare la [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attributo da solo, ma in genere è consigliabile utilizzare il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) anche dell'attributo. Il `DataType` attributo fornisce il *semantica* dei dati come rispetto alla modalità di rendering in una schermata e offre i vantaggi seguenti che non si ottengono con `DisplayFormat`:

- Il browser può abilitare le funzionalità HTML5 (ad esempio per visualizzare un controllo di calendario, il simbolo della valuta appropriato delle impostazioni locali, collegamenti di posta elettronica, e così via.).
- Per impostazione predefinita, il browser eseguirà il rendering dei dati usando il formato corretto in base il [delle impostazioni locali](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributo consente di abilitare MVC scegliere il modello di campo corretto per il rendering dei dati (la [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) se usato da se stessa Usa il modello di stringa). Per altre informazioni, vedere Brad Wilson [ASP.NET MVC 2 Templates](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Anche se scritte per MVC 2, in questo articolo ancora si applica alla versione corrente di ASP.NET MVC.)

Se si usa la `DataType` attributo con un campo Data, è necessario specificare il `DisplayFormat` attributi anche per garantire che il campo correttamente il rendering nel browser Chrome. Per altre informazioni, vedere [thread di StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Eseguire di nuovo la pagina di indice degli studenti e notare che volte non vengono più visualizzate per le date di registrazione. Lo stesso sarà true per qualsiasi vista che utilizza il `Student` modello.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>Dell'oggetto StringLengthAttribute

È anche possibile specificare regole di convalida dei dati e messaggi usando gli attributi. Ad esempio si supponga di voler limitare a 50 il numero massimo di caratteri che gli utenti possono immettere per un nome. Per aggiungere questa limitazione, aggiungere [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attributi per il `LastName` e `FirstMidName` proprietà, come illustrato nell'esempio seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

Il [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) non impedisce a un utente di immettere spazi vuoti per un nome di attributo. È possibile usare la [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) attributo per applicare restrizioni all'input. Ad esempio il codice seguente richiede il primo carattere sia maiuscolo e i caratteri rimanenti siano caratteri alfabetici:

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

Il [MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) attributo fornisce funzionalità simili a quelle di [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attributo ma non è disponibile sul lato client convalida.

Eseguire l'applicazione, quindi scegliere il **studenti** scheda. Viene visualizzato l'errore seguente:

*Il modello supporta il contesto 'SchoolContext' è stato modificato dal momento della creazione del database. È consigliabile usare migrazioni Code First per aggiornare il database ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).*

Il modello di database è stato modificato in modo da richiedere una modifica dello schema del database ed Entity Framework ha rilevato che. Si userà migrations per aggiornare lo schema senza perdere i dati che sono state aggiunte al database con l'interfaccia utente. Se è stato modificato i dati che è stati creati dal `Seed` (metodo), che verrà modificata allo stato originale perché il [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) metodo che si sta utilizzando il `Seed` (metodo). ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) equivale a un'operazione "upsert" da terminologia dei database.)

Nella console di Gestione pacchetti immettere i comandi seguenti:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

Il `add-migration MaxLengthOnNames` comando crea un file denominato  *&lt;timeStamp&gt;\_MaxLengthOnNames.cs*. Questo file contiene codice che aggiorna il database in modo che corrisponda il modello di dati corrente. Il timestamp anteposto al nome del file delle migrazioni viene usato da Entity Framework per ordinare le migrazioni. Dopo aver creato le migrazioni di più, se si elimina il database o se si distribuisce il progetto tramite le migrazioni, tutte le migrazioni vengono applicate nell'ordine in cui sono stati creati.

Eseguire la **Create** pagina e immettere un nome con più di 50 caratteri. Non appena si superano i 50 caratteri, la convalida lato client mostra immediatamente un messaggio di errore.

![client side errore val](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>L'attributo di colonna

È possibile usare gli attributi anche per controllare il mapping delle classi e delle proprietà nel database. Si supponga di aver usato il nome `FirstMidName` per il campo first-name (Nome) perché il campo potrebbe contenere anche un secondo nome. Tuttavia si vuole che la colonna di database sia denominata `FirstName`, perché gli utenti che scrivono query ad hoc per il database sono abituati a tale nome. Per eseguire questo mapping è possibile usare l'attributo `Column`.

L'attributo `Column` specifica che quando viene creato il database, la colonna della tabella `Student` mappata sulla proprietà `FirstMidName` verrà denominata `FirstName`. In altri termini, quando il codice fa riferimento a `Student.FirstMidName` i dati provengono dalla colonna `FirstName` della tabella `Student` o vengono aggiornati in tale colonna. Se non si specificano nomi di colonna, essi vengono ha lo stesso nome come nome della proprietà.

Aggiungere un'usando informativa [System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) e l'attributo del nome di colonna per il `FirstMidName` proprietà, come illustrato nel codice evidenziato seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

L'aggiunta del [attributo di colonna](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) cambia il modello di backup SchoolContext, pertanto non corrisponderà al database. Immettere i comandi seguenti nella console di gestione pacchetti per creare un'altra migrazione:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

Nella **Esplora Server** (**Esplora Database** se si usa Express per Web), fare doppio clic il *studente* tabella.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

L'immagine seguente mostra il nome della colonna originale come accadeva prima di applicare le prime due migrazioni. Oltre al nome di colonna da modificare `FirstMidName` al `FirstName`, le due colonne con nomi sono stati modificati da `MAX` lunghezza fino a 50 caratteri.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

È inoltre possibile apportare database modifiche di mapping utilizzando il [API Fluent](https://msdn.microsoft.com/data/jj591617), come si vedrà più avanti in questa esercitazione.

> [!NOTE]
> Se si prova a compilare prima di aver creato tutte queste classi di entità, si potrebbero ottenere errori del compilatore.


## <a name="create-the-instructor-entity"></a>Creare l'entità Instructor

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Creare *Models\Instructor.cs*, sostituendo il codice del modello con il codice seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Si noti che molte proprietà sono uguali nelle entità `Student` e `Instructor`. Nel [implementazione di ereditarietà](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) esercitazione più avanti in questa serie si effettuerà il refactoring utilizzando l'ereditarietà per eliminare questa ridondanza.

### <a name="the-required-and-display-attributes"></a>Obbligatorio e gli attributi di visualizzazione

Gli attributi nel `LastName` proprietà specificare che si tratta di un campo obbligatorio, che la didascalia per la casella di testo deve essere "Last Name" (anziché il nome della proprietà, che dovrebbe essere "LastName" senza spazio) e che il valore non può contenere più di 50 caratteri.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

Il [attributo StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) imposta la lunghezza massima del database e sono disponibili sul lato client e lato server convalida per ASP.NET MVC. È anche possibile specificare la lunghezza minima della stringa in questo attributo, ma il valore minimo non ha alcun effetto sullo schema del database. Il [attributo obbligatorio](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) non è necessaria per i tipi di valore, ad esempio DateTime, int, double e float. I tipi di valore non è possibile assegnare un valore null, pertanto sono intrinsecamente necessari. È possibile rimuovere il [attributo obbligatorio](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) e sostituirlo con un parametro di lunghezza minima per il `StringLength` attributo:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs?highlight=2)]

È possibile inserire più attributi su una riga, pertanto è anche possibile scrivere la classe instructor come indicato di seguito:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-fullname-calculated-property"></a>La proprietà calcolata FullName

`FullName` è una proprietà calcolata che restituisce un valore creato concatenando altre due proprietà. Di conseguenza ha solo un `get` funzione di accesso e nessun `FullName` colonna verrà generata nel database.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>Corsi e le proprietà di navigazione OfficeAssignment

Le proprietà `Courses` e `OfficeAssignment` sono proprietà di navigazione. Come è stato illustrato in precedenza, sono in genere definiti come [virtuale](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) in modo che si possono sfruttare i vantaggi di una funzionalità di Entity Framework chiamato [caricamento lazy](https://msdn.microsoft.com/magazine/hh205756.aspx). Inoltre, se una proprietà di navigazione può contenere più entità, il tipo deve implementare il [ICollection&lt;T&gt; ](https://msdn.microsoft.com/library/92t2ye13.aspx) interfaccia. (Ad esempio [IList&lt;T&gt; ](https://msdn.microsoft.com/library/5y536ey6.aspx) ma non qualifica [IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx) poiché `IEnumerable<T>` non implementa [Add ](https://msdn.microsoft.com/library/63ywd54z.aspx).

Un insegnante può tenere un numero qualsiasi di corsi, pertanto `Courses` viene definito come una raccolta di `Course` entità. Le regole di business è stato un insegnante non può avere più di un ufficio, pertanto `OfficeAssignment` viene definito come un'unica `OfficeAssignment` entity (che può essere `null` se non è assegnato alcun ufficio).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>Creare l'entità OfficeAssignment

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Creare *Models\OfficeAssignment.cs* con il codice seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Compilare il progetto, che consente di salvare le modifiche e verifica che è stata apportata qualsiasi copia e Incolla il compilatore può intercettare gli errori.

### <a name="the-key-attribute"></a>L'attributo chiave

È presente una relazione uno-a-zero-o-uno tra i `Instructor` e il `OfficeAssignment` entità. Un'assegnazione di ufficio esiste solo in relazione all'insegnante viene assegnato a, e pertanto la chiave primaria è anche la chiave esterna per il `Instructor` entità. Ma Entity Framework non riconosce automaticamente `InstructorID` del database primario la chiave di entità perché il nome non rispetta il `ID` oppure *classname* `ID` convenzione di denominazione. Per identificare l'entità come chiave viene usato l'attributo `Key`:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

È anche possibile usare la `Key` se l'entità dispone di chiave primaria propria ma si vuole assegnare la proprietà un nome diverso da quello dell'attributo `classnameID` o `ID`. Per impostazione predefinita EF considera la chiave come non generata database perché la colonna è per una relazione di identificazione.

### <a name="the-foreignkey-attribute"></a>L'attributo perché ForeignKey

Quando si verifica una relazione uno-a-zero-o-uno o una relazione uno a uno tra due entità (tale esempio tra `OfficeAssignment` e `Instructor`), Entity Framework non può essere usata la cui finale della relazione è l'entità e quale entità finale dipendente. Le relazioni uno a uno dispongono di una proprietà di navigazione di riferimento in ogni classe per l'altra classe. Il [attributo perché ForeignKey](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) possono essere applicati alla classe di dipendenti per stabilire la relazione. Se si omette il [attributo perché ForeignKey](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), viene visualizzato l'errore seguente quando si prova a creare la migrazione:

Impossibile determinare l'entità finale principale di un'associazione tra i tipi 'ContosoUniversity.Models.OfficeAssignment' e 'ContosoUniversity.Models.Instructor'. Entità finale principale di questa associazione deve essere configurato in modo esplicito usando l'API fluent relazione o annotazioni dei dati.

Più avanti nell'esercitazione, ecco come configurare questa relazione con l'API fluent.

### <a name="the-instructor-navigation-property"></a>Proprietà di navigazione Instructor

Il `Instructor` entità dispone di un valore nullable `OfficeAssignment` proprietà di navigazione (perché un docente potrebbe non avere un ufficio assegnato) e il `OfficeAssignment` entità dispone di un non-nullable `Instructor` proprietà di navigazione (perché non è un'assegnazione di ufficio esiste senza un insegnante: `InstructorID` è non nullable). Quando un `Instructor` entità è correlata una `OfficeAssignment` entità, ogni entità include un riferimento a un nella relativa proprietà di navigazione.

È possibile inserire un `[Required]` attributo nella proprietà di navigazione Instructor per specificare che deve essere presente un insegnante correlato, ma non è necessario eseguire l'operazione perché la chiave esterna InstructorID (che è anche la chiave per questa tabella) è non nullable.

## <a name="modify-the-course-entity"></a>Modificare l'entità Course

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Nelle *Models\Course.cs*, sostituire il codice aggiunto in precedenza con il codice seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

L'entità course ha una proprietà di chiave esterna `DepartmentID` che punta all'oggetto correlato `Department` entità e si ha un `Department` proprietà di navigazione. In Entity Framework non è necessario aggiungere una proprietà di chiave esterna al modello di dati se è disponibile una proprietà di navigazione per un'entità correlata. Entity Framework crea automaticamente chiavi esterne nel database ovunque siano necessarie. Tuttavia il fatto di avere la chiave esterna nel modello di dati può rendere più semplici ed efficienti gli aggiornamenti. Ad esempio, quando si recupera un'entità course per consentire la modifica di `Department` entità è null se non viene caricata, pertanto, quando si aggiorna l'entità course, è necessario innanzitutto recuperare la `Department` entità. Quando la proprietà di chiave esterna `DepartmentID` viene incluso nel modello di dati, non è necessario recuperare il `Department` entità prima di aggiornare.

### <a name="the-databasegenerated-attribute"></a>Attributo DatabaseGenerated

Il [attributo DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) con il [None](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) parametro il `CourseID` proprietà indica che valori di chiave primaria vengono forniti dall'utente anziché generati dal database.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

Per impostazione predefinita, Entity Framework presuppone che i valori di chiave primaria vengano generati dal database. Questa è la condizione ottimale nella maggior parte degli scenari. Tuttavia, per `Course` entità, è possibile usare un numero di corso specificato dall'utente, ad esempio una serie 1000 per un solo reparto, serie 2000 per un altro reparto e così via.

### <a name="foreign-key-and-navigation-properties"></a>Chiave esterna e le proprietà di navigazione

La proprietà di chiave esterna e le proprietà di navigazione nel `Course` entità riflettono le relazioni seguenti:

- Un corso viene assegnato a un solo reparto, pertanto sono presenti una chiave esterna `DepartmentID` e una proprietà di navigazione `Department` per i motivi indicati in precedenza. 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- Un corso può avere un numero qualsiasi di studenti iscritti, pertanto la proprietà di navigazione `Enrollments` è una raccolta: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Un corso può essere impartito da più insegnanti, pertanto la proprietà di navigazione `Instructors` è una raccolta: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="creating-the-department-entity"></a>Creare l'entità Department

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

Creare *Models\Department.cs* con il codice seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>L'attributo di colonna

In precedenza è stato utilizzato il [attributo di colonna](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) per modificare il mapping di nomi di colonna. Nel codice per il `Department` entità, il `Column` attributo viene utilizzato per modificare SQL tipo mapping tra i dati in modo che la colonna venga definita usando SQL Server [denaro](https://msdn.microsoft.com/library/ms179882.aspx) tipo nel database:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Mapping di colonne a livello generale non è necessario, perché Entity Framework sceglie in genere il tipo di dati di SQL Server appropriato in base al tipo CLR definito per la proprietà. Il tipo CLR `decimal` esegue il mapping a un tipo SQL Server `decimal`. Ma in questo caso si sa che la colonna verrà importi in valuta e il [denaro](https://msdn.microsoft.com/library/ms179882.aspx) risulta più appropriato per tale tipo di dati.

### <a name="foreign-key-and-navigation-properties"></a>Chiave esterna e le proprietà di navigazione

Le proprietà di chiave esterna e le proprietà di navigazione riflettono le relazioni seguenti:

- Un reparto può avere o meno un amministratore e un amministratore è sempre un insegnante. Di conseguenza il `InstructorID` proprietà è inclusa come chiave esterna per il `Instructor` entità e un punto interrogativo viene aggiunto dopo il `int` digitare designazione per contrassegnare la proprietà come nullable. La proprietà di navigazione è denominata `Administrator` ma contiene un `Instructor` entità: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Un reparto può avere molti corsi, pertanto non c'è un `Courses` proprietà di navigazione: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > Per convenzione, Entity Framework consente l'eliminazione a catena per le chiavi esterne non nullable e per le relazioni molti-a-molti. Ciò può comportare regole di eliminazione a catena circolari, che generano un'eccezione quando viene eseguito il codice inizializzatore. Ad esempio, se non è stato definito il `Department.InstructorID` proprietà come nullable, si otterrebbe il seguente messaggio di eccezione quando viene eseguita l'inizializzatore: "la relazione referenziale comporterà un riferimento ciclico non consentito." Se le regole business richiedono `InstructorID` proprietà come non nullable, sarebbe necessario usare l'API fluent seguente per disabilitare l'eliminazione a catena sulla relazione: 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]


## <a name="modifying-the-student-entity"></a>Modificare l'entità Student

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

Nelle *Models\Student.cs*, sostituire il codice aggiunto in precedenza con il codice seguente. Le modifiche sono evidenziate.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=12,15,24-27)]

## <a name="the-enrollment-entity"></a>L'entità Enrollment

 Nelle *Models\Enrollment.cs*, sostituire il codice aggiunto in precedenza con il codice seguente

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs?highlight=16)]

### <a name="foreign-key-and-navigation-properties"></a>Chiave esterna e le proprietà di navigazione

Le proprietà di chiave esterna e le proprietà di navigazione riflettono le relazioni seguenti:

- Un record di iscrizione è relativo a un singolo corso, pertanto sono presenti una proprietà di chiave esterna `CourseID` e una proprietà di navigazione `Course`: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]
- Un record di iscrizione è relativo a un singolo studente, pertanto sono presenti una proprietà di chiave esterna `StudentID` e una proprietà di navigazione `Student`: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

### <a name="many-to-many-relationships"></a>Relazioni molti-a-molti

È presente una relazione molti-a-molti tra il `Student` e `Course` entità e il `Enrollment` entità funziona come una tabella di join molti-a-molti *con payload* nel database. Ciò significa che il `Enrollment` tabella contiene dati aggiuntivi oltre alle chiavi esterne delle tabelle di join (in questo caso, una chiave primaria e una `Grade` proprietà).

La figura seguente illustra l'aspetto di queste relazioni in un diagramma di entità. (Questo diagramma è stato generato usando il [Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); la creazione del diagramma non fa parte dell'esercitazione, è sufficiente usato come esempio.)

![Student-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

Ogni riga della relazione dispone di un 1 in un'entità finale e un asterisco (\*) a altra, che indica una relazione uno-a-molti.

Se il `Enrollment` tabella non include informazioni sul livello, è sufficiente contenere le due chiavi esterne `CourseID` e `StudentID`. In tal caso, corrisponde a una tabella di join molti-a-molti *senza payload* (o una *tabella di join pura*) nel database, e non sarebbe necessario creare una classe di modello per tale affatto. Il `Instructor` e `Course` le entità dispongono di tale tipo di relazione molti-a-molti e come può notare, non vi è alcuna classe di entità tra di essi:

![Instructor-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

È necessaria una tabella di join del database, tuttavia, come illustrato nel diagramma di database seguenti:

![Instructor Course_many a many_relationship_tables](https://asp.net/media/2577802/Windows-Live-Writer_Creating-a.NET-MVC-Application-4-of-10h1_B662_Instructor-Course_many-to-many_relationship_tables_03e042cf-db89-4b4c-985a-e458351ada76.png)

Entity Framework crea automaticamente il `CourseInstructor` tabella ed è di lettura e aggiornarlo indirettamente tramite la lettura e l'aggiornamento di `Instructor.Courses` e `Course.Instructors` le proprietà di navigazione.

## <a name="entity-diagram-showing-relationships"></a>Diagramma dell'entità che visualizza le relazioni

La figura seguente visualizza il diagramma creato da Entity Framework Power Tools per il modello School completato.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

Oltre a linee delle relazioni molti-a-molti (\* al \*) e le linee di relazione uno-a-molti (1 per \*), è possibile visualizzare qui la linea della relazione uno-a-zero-o-uno (da 1 a 0..1) tra il `Instructor` e `OfficeAssignment` le entità e la linea della relazione zero-o-uno-a-molti (0..1 a \*) tra le entità Instructor e Department.

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>Personalizzare il modello di dati aggiungendo codice nel contesto di Database

Ora si aggiungerà le nuove entità per il `SchoolContext` classe e personalizzare i mapping utilizzando alcune [API fluent](https://msdn.microsoft.com/data/jj591617) chiamate. (L'API è "API fluent" perché viene spesso usata unendo una serie di chiamate al metodo in una singola istruzione).

In questa esercitazione si userà l'API fluent solo per il mapping del database eseguibili con attributi. È tuttavia possibile usare l'API Fluent anche per specificare la maggior parte delle regole di formattazione, convalida e mapping specificabili tramite gli attributi. Alcuni attributi quali `MinimumLength` non possono essere applicati con l'API Fluent. Come accennato in precedenza, `MinimumLength` non modifica lo schema, si applica solo una regola di convalida lato client e server

Alcuni sviluppatori preferiscono usare solo l'API Fluent, per dare un aspetto "ordinato" alle classi di entità. È possibile usare un misto di attributi e API Fluent e alcune personalizzazioni sono eseguibili solo mediante l'API Fluent, ma in generale è consigliabile scegliere uno dei due approcci e usarlo in modo il più possibile coerente.

Per aggiungere le nuove entità per i dati del modello ed eseguire il mapping dei database che non è stata effettuata usando gli attributi, sostituire il codice nel *DAL\SchoolContext.cs* con il codice seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

La nuova istruzione nel [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) metodo consente di configurare la tabella di join molti-a-molti:

- Per la relazione molti-a-molti tra il `Instructor` e `Course` entità, il codice specifica i nomi di tabelle e colonne per la tabella di join. Codice prima di tutto possibile configurare la relazione molti-a-molti per l'utente senza questo codice, ma se non lo si chiama, si otterranno i nomi predefiniti, ad esempio `InstructorInstructorID` per il `InstructorID` colonna.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

Il codice seguente viene fornito un esempio di come è possibile utilizzare l'API fluent anziché agli attributi per specificare la relazione tra il `Instructor` e `OfficeAssignment` entità:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

Per informazioni relative alle operazioni vengono eseguite le istruzioni "API fluent" dietro le quinte, vedere la [API Fluent](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) post di blog.

## <a name="seed-the-database-with-test-data"></a>Assegnare valori di inizializzazione al database con dati di test

Sostituire il codice nel *Migrations\Configuration.cs* file con il codice seguente per fornire i dati di valore di inizializzazione per le nuove entità create.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

Come illustrato nella prima esercitazione, è sufficiente la maggior parte di questo codice aggiorna o crea nuovi oggetti entità e carica i dati di esempio all'interno delle proprietà in base alle esigenze per i test. Si noti, tuttavia, come la `Course` entità che dispone di una relazione molti-a-molti con la `Instructor` entità, viene gestito:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

Quando si crea una `Course` dell'oggetto, si inizializza il `Instructors` proprietà di navigazione come una raccolta vuota usando il codice `Instructors = new List<Instructor>()`. Questo rende possibile l'aggiunta `Instructor` le entità correlate a questa `Course` usando il `Instructors.Add` (metodo). Se non è stato creato un elenco vuoto, non sarà possibile aggiungere queste relazioni, in quanto il `Instructors` proprietà sarà null e non sarebbe necessario un `Add` (metodo). È anche possibile aggiungere l'inizializzazione elenco al costruttore.

## <a name="add-a-migration-and-update-the-database"></a>Aggiungere una migrazione e aggiornare il Database

Dalla console di gestione pacchetti, immettere il `add-migration` comando:

`PM> add-Migration Chap4`

Se si prova ad aggiornare il database a questo punto, si otterrà l'errore seguente:

*L'istruzione ALTER TABLE è in conflitto con il vincolo FOREIGN KEY "FK\_dbo. Corso\_dbo. Reparto\_DepartmentID ". Il conflitto si è verificato nel database "ContosoUniversity", la tabella "dbo. Reparto", colonna 'DepartmentID'.*

Modificare il &lt; *timestamp&gt;\_Chap4.cs* file e apportare le modifiche al codice seguente (è possibile aggiungere un'istruzione SQL e modificare un `AddColumn` istruzione):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

(Assicurarsi di impostare come commento o eliminare quello esistente `AddColumn` riga quando si aggiunge una nuova, oppure si otterrà un errore quando si immette il `update-database` comando.)

In alcuni casi quando si eseguono migrazioni con dati esistenti, è necessario inserire dati stub nel database per soddisfare i vincoli di chiave esterni e questo è ciò che si sta eseguendo ora. Il codice generato aggiunge un che non ammettano `DepartmentID` chiave esterna per il `Course` tabella. Se sono già presenti righe di `Course` tabella quando viene eseguito il codice, il `AddColumn` operazione avrà esito negativo perché SQL Server non determina il valore da inserire nella colonna che non può essere null. Di conseguenza è stato modificato il codice per dare alla nuova colonna un valore predefinito e aver creato un reparto stub denominato "Temp" come il reparto predefinito. Di conseguenza, se sono presenti `Course` delle righe quando si esegue questo codice, saranno tutti correlati al reparto "Temp".

Quando il `Seed` viene eseguito il metodo inserisce righe nel `Department` tabella che sarà correlata esistente `Course` le righe di cui questi nuovi `Department` righe. Se è stato aggiunto alcun corso nell'interfaccia utente, potrebbe quindi non è più necessario il reparto "Temp" o il valore predefinito sul `Course.DepartmentID` colonna. Per consentire la possibilità che qualcuno potrebbe essere stato aggiunto corsi tramite l'applicazione, potrebbe anche voler aggiornare il `Seed` codice del metodo per garantire che tutti i `Course` righe (non solo quelli inseriti da esecuzioni precedenti del `Seed` metodo) hanno valido `DepartmentID` valori prima di rimuovere il valore predefinito dalla colonna valore ed eliminare il reparto "Temp".

Dopo aver completato la modifica di &lt; *timestamp&gt;\_Chap4.cs* file, immettere il `update-database` comando nella console di gestione pacchetti per eseguire la migrazione.

> [!NOTE]
> È possibile ottenere altri errori durante la migrazione dei dati e apportare le modifiche dello schema. Se si verificano errori di migrazione non è possibile risolvere, è possibile modificare la stringa di connessione nel *Web. config* file o eliminare il database. L'approccio più semplice consiste nel rinominare il database in *Web. config* file. Ad esempio, modificare il nome del database in unità di capacità\_testare, come illustrato nell'esempio seguente:
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.xml?highlight=1-2)]
> 
>  Con un nuovo database, non sono presenti dati per eseguire la migrazione e il `update-database` comando è molto probabile che venga completato senza errori. Per istruzioni su come eliminare il database, vedere [come eliminare un Database da Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).


Aprire il database in **Esplora Server** eseguita in precedenza, quindi espandere il **tabelle** nodo per verificare che tutte le tabelle sono state create. (Se continuano a verificarsi **Esplora Server** aperto dall'operazione precedente, fare clic il **aggiornare** pulsante.)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Non è stata creata la classe modello per il `CourseInstructor` tabella. Come spiegato in precedenza, si tratta di una tabella di join per la relazione molti-a-molti tra il `Instructor` e `Course` entità.

Fare doppio clic sul `CourseInstructor` tabella e selezionare **Mostra dati tabella** per verificare che disponga di dati in esso in seguito al `Instructor` entità è stato aggiunto per il `Course.Instructors` proprietà di navigazione.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="summary"></a>Riepilogo

Ora sono presenti un modello di dati più complesso e il database corrispondente. Nell'esercitazione si apprenderà informazioni sui vari modi per accedere ai dati correlati.

Sono disponibili collegamenti ad altre risorse di Entity Framework nel [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Successivo](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
