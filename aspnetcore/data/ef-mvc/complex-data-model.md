---
title: Core di ASP.NET MVC con Entity Framework Core - modello di dati - 5 di 10
author: tdykstra
description: "In questa esercitazione aggiungere altre entità e relazioni e personalizzare il modello di dati specificando le regole di mapping del database, convalida e formattazione."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: 5b5645936504333573950b5bd17f5a037ffd984f
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="creating-a-complex-data-model---ef-core-with-aspnet-core-mvc-tutorial-5-of-10"></a>Creazione di un modello di dati complessi - EF Core con l'esercitazione di base di ASP.NET MVC (5 di 10)

Da [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni web ASP.NET MVC di base con Entity Framework Core e Visual Studio. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](intro.md).

Nelle esercitazioni precedenti, si è lavorato con un modello di dati semplici che è stato composto da tre entità. In questa esercitazione si aggiungeranno ulteriori entità e relazioni e verrà personalizzato il modello di dati specificando le regole di mapping del database, convalida e formattazione.

Al termine dell'operazione, le classi di entità verranno costituiscono il modello di dati completato che è illustrato nella figura seguente:

![Diagramma di entità](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Personalizzare il modello di dati utilizzando gli attributi

In questa sezione si noterà come personalizzare il modello di dati utilizzando gli attributi che specificano la formattazione, convalida e regole di mapping del database. Quindi in diversi nelle sezioni seguenti che verrà creato il modello di dati completo dell'istituto di istruzione aggiungendo attributi alle classi è già stato creato e la creazione di nuove classi per i tipi di entità rimanenti nel modello.

### <a name="the-datatype-attribute"></a>L'attributo di tipo di dati

Per le date di registrazione studenti, tutte le pagine web attualmente Visualizza il tempo insieme alla data, anche se tutto è rilevante per questo campo è la data. Utilizzando gli attributi di annotazione dei dati, è possibile apportare una modifica che in ogni visualizzazione che mostra i dati per correggere il formato di visualizzazione del codice. Per vedere un esempio di come eseguire l'operazione, verrà aggiunto un attributo di `EnrollmentDate` proprietà la `Student` classe.

In *Models/Student.cs*, aggiungere un `using` istruzione per il `System.ComponentModel.DataAnnotations` dello spazio dei nomi e aggiungere `DataType` e `DisplayFormat` gli attributi di `EnrollmentDate` proprietà, come illustrato nell'esempio seguente:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

L'attributo `DataType` viene usato per specificare un tipo di dati che è più specifico del tipo intrinseco del database. In questo caso si vuole inserire solo tenere traccia delle date, non la data e ora. Il `DataType` enumerazione fornisce per molti tipi di dati, ad esempio Date, Time, PhoneNumber, valuta, EmailAddress e altro ancora. L'attributo `DataType` può anche consentire all'applicazione di fornire automaticamente le funzionalità specifiche del tipo. Ad esempio, è possibile creare un collegamento `mailto:` per `DataType.EmailAddress` e fornire un selettore data per `DataType.Date` nei browser che supportano HTML5. Il `DataType` attributo genera HTML 5 `data-` attributi (si pronuncia dati dash) in grado di comprendere i browser HTML 5. Il `DataType` attributi non forniscono alcuna convalida.

`DataType.Date` non specifica il formato della data visualizzata. Per impostazione predefinita, viene visualizzato il campo dei dati in base ai formati predefiniti in base a CultureInfo del server.

L'attributo `DisplayFormat` viene usato per specificare in modo esplicito il formato della data:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

L'impostazione `ApplyFormatInEditMode` specifica che la formattazione deve essere applicata anche quando il valore viene visualizzato in una casella di testo per la modifica. (Non è possibile che per alcuni campi, ad esempio, per i valori di valuta, occorre evitare il simbolo di valuta nella casella di testo per la modifica.)

È possibile utilizzare il `DisplayFormat` attributo da se stesso, ma in genere è consigliabile utilizzare il `DataType` anche l'attributo. Il `DataType` attributo fornisce la semantica dei dati anziché come eseguirne il rendering in una schermata e fornisce i seguenti vantaggi che non si ottengono con `DisplayFormat`:

* Il browser è possibile abilitare le funzionalità di HTML5 (ad esempio per visualizzare un controllo di calendario, il simbolo di valuta delle impostazioni locali appropriata, i collegamenti di posta elettronica, alcuni client-side input convalida e così via).

* Per impostazione predefinita, il browser eseguirà il rendering dei dati usando il formato corretto in base alle impostazioni locali del sistema.

Per ulteriori informazioni, vedere il [ \<input > tag di documentazione di supporto](../../mvc/views/working-with-forms.md#the-input-tag-helper).

Eseguire l'app, passare alla pagina di indice di studenti e notare che volte non vengono più visualizzate per le date di registrazione. Lo stesso sarà true per qualsiasi vista che utilizza il modello di studenti.

![Pagina di indice di studenti con date senza tempi di](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>L'attributo StringLength

È inoltre possibile specificare regole di convalida dei dati e i messaggi di errore di convalida utilizzando gli attributi. Il `StringLength` attributo imposta la lunghezza massima del database e fornisce lato client e lato server della convalida di ASP.NET MVC. È inoltre possibile specificare la lunghezza minima della stringa in questo attributo, ma il valore minimo non ha alcun impatto sullo schema di database.

Si supponga che si desidera garantire che gli utenti non immettere più di 50 caratteri per un nome. Per aggiungere questa limitazione, è necessario `StringLength` gli attributi di `LastName` e `FirstMidName` proprietà, come illustrato nell'esempio seguente:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

Il `StringLength` attributo non impedisce a un utente di immettere lo spazio vuoto per un nome. È possibile utilizzare il `RegularExpression` attributo per applicare restrizioni per l'input. Ad esempio, il codice seguente richiede il primo carattere da maiuscolo e i caratteri rimanenti alfabetico:

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

Il `MaxLength` attributo fornisce funzionalità simili a quelle di `StringLength` attributo ma non è disponibile sul lato client convalida.

Il modello di database è stata modificata in modo che richiede una modifica nello schema del database. Utilizzare le migrazioni per aggiornare lo schema senza perdere i dati che possono essere aggiunti al database tramite l'interfaccia utente dell'applicazione.

Salvare le modifiche e compilare il progetto. Quindi aprire la finestra di comando nella cartella del progetto e immettere i comandi seguenti:

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

Il `migrations add` comando genera un avviso che potrebbe verificarsi una perdita di dati, in quanto la modifica rende la lunghezza massima più breve per due colonne.  Le migrazioni crea un file denominato  *\<timeStamp > _MaxLengthOnNames.cs*. Questo file contiene codice di `Up` metodo che aggiornerà il database in base al modello di dati corrente. Il `database update` comando è stato eseguito il codice.

Il timestamp come preceduto al nome del file migrazioni utilizzato da Entity Framework per ordinare le migrazioni. È possibile creare più migrazioni prima di eseguire il comando update-database, e quindi tutte le migrazioni sono applicate nell'ordine in cui sono stati creati.

Eseguire l'app, selezionare il **studenti** scheda, fare clic su **Crea nuovo**e immettere il nome di più di 50 caratteri. Quando fa clic su **crea**, la convalida lato client verrà visualizzato un messaggio di errore.

![Gli studenti indice pagina la visualizzazione di errori di lunghezza stringa](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a>L'attributo di colonna

Inoltre, è possibile utilizzare gli attributi per controllare il mapping tra le classi e le proprietà per il database. Si supponga che sia stato usato il nome `FirstMidName` per il nome del primo campo perché il campo può contenere anche un cognome. Ma si desidera che la colonna di database sia denominato `FirstName`, perché gli utenti che scrittura di query ad hoc sul database sono abituati a tale nome. Per rendere questo mapping, è possibile utilizzare il `Column` attributo.

Il `Column` attributo specifica che quando viene creato il database, la colonna del `Student` tabella in cui viene eseguito il mapping per il `FirstMidName` proprietà verrà denominata `FirstName`. In altre parole, quando il codice fa riferimento a `Student.FirstMidName`, i dati provengono dal o aggiornati nel `FirstName` colonna del `Student` tabella. Se non si specificano nomi di colonna, essi sono assegnato lo stesso nome come nome della proprietà.

Nel *Student.cs* file, aggiungere un `using` istruzione per `System.ComponentModel.DataAnnotations.Schema` e aggiungere l'attributo del nome di colonna per il `FirstMidName` proprietà, come illustrato nel codice evidenziato seguente:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

L'aggiunta del `Column` attributo modifica il supporto del modello di `SchoolContext`, in modo che non corrisponda al database.

Salvare le modifiche e compilare il progetto. Quindi aprire la finestra di comando nella cartella del progetto e immettere i comandi seguenti per creare un'altra migrazione:

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

In **Esplora oggetti di SQL Server**, aprire Progettazione tabelle Student facendo doppio clic il **Student** tabella.

![Tabella di studenti in sillaba SSOX dopo le migrazioni](complex-data-model/_static/ssox-after-migration.png)

Prima dell'applicazione le prime due migrazioni, le colonne nome sono di tipo nvarchar (max). Sono ora nvarchar (50) e il nome della colonna è stato modificato da FirstMidName per FirstName.

> [!Note]
> Se si tenta di compilare prima del completamento della creazione di tutte le classi di entità nelle sezioni seguenti, si potrebbero verificare errori di compilazione.

## <a name="final-changes-to-the-student-entity"></a>Modifiche finali per l'entità di Student

![Entità Student](complex-data-model/_static/student-entity.png)

In *Models/Student.cs*, sostituire il codice aggiunto in precedenza con il codice seguente. Le modifiche sono evidenziate.

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>L'attributo obbligatorio

Il `Required` attributo rende i campi obbligatori di nome proprietà. Il `Required` attributo non è necessaria per i tipi non nullable, ad esempio i tipi di valore (DateTime, int, double, float, ecc.). Tipi che non possono essere null vengono considerati automaticamente come i campi obbligatori.

È possibile rimuovere il `Required` attributo e sostituirlo con un parametro di lunghezza minima per il `StringLength` attributo:

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>L'attributo di visualizzazione

Il `Display` attributo specifica che la didascalia per le caselle di testo deve essere "Nome", "Last Name", "Nome" e "Data di registrazione" anziché il nome della proprietà in ogni istanza (che non dispone di spazio dividendo le parole).

### <a name="the-fullname-calculated-property"></a>La proprietà FullName calcolato

`FullName`è una proprietà calcolata che restituisce un valore che viene creato concatenando due altre proprietà. Di conseguenza ha solo una funzione di accesso get e nessun `FullName` colonna verrà generata nel database.

## <a name="create-the-instructor-entity"></a>Creare l'entità Instructor

![Entità Instructor](complex-data-model/_static/instructor-entity.png)

Creare *Models/Instructor.cs*, sostituendo il codice del modello con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

Si noti che molte proprietà sono uguali nelle entità Student e Instructor. Nel [implementazione ereditarietà](inheritance.md) esercitazione più avanti in questa serie, sarà il refactoring questo codice per eliminare la ridondanza.

È possibile inserire più attributi su una riga, pertanto è possibile anche scrivere la `HireDate` attributi come indicato di seguito:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>Le proprietà di navigazione CourseAssignments e OfficeAssignment

Il `CourseAssignments` e `OfficeAssignment` sono proprietà di navigazione.

Un docente può indicare un numero qualsiasi di corsi, pertanto `CourseAssignments` è definito come una raccolta.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Se una proprietà di navigazione può contenere più entità, il tipo deve essere un elenco in cui le voci possono essere aggiunte, eliminate e aggiornate.  È possibile specificare `ICollection<T>` o un tipo, ad esempio `List<T>` o `HashSet<T>`. Se si specifica `ICollection<T>`, EF crea un `HashSet<T>` insieme per impostazione predefinita.

Il motivo per cui sono `CourseAssignment` entità vengono illustrati di seguito nella sezione sulle relazioni molti-a-molti.

Regole di business Contoso University stato che un docente può solo avere al massimo un ufficio, pertanto la `OfficeAssignment` proprietà contiene una singola entità OfficeAssignment (che può essere null se non viene assegnato).

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>Creare l'entità OfficeAssignment

![Entità OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

Creare *Models/OfficeAssignment.cs* con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>L'attributo chiave

È una relazione uno-a-zero-o-uno tra istruttore e le entità OfficeAssignment. L'assegnazione dell'ufficio esiste solo in relazione a istruttore a che viene assegnato, e pertanto la chiave primaria è anche la chiave esterna per l'entità Instructor. Ma Entity Framework non riconoscono automaticamente InstructorID come chiave primaria dell'entità perché il nome non rispetta la convenzione di denominazione ID o classnameID. Pertanto, il `Key` attributo viene utilizzato per identificare come la chiave:

```csharp
[Key]
public int InstructorID { get; set; }
```

È inoltre possibile utilizzare il `Key` attributo se l'entità dispone di chiave primaria ma si desidera attribuire un nome di proprietà diverso da classnameID o ID.

Per impostazione predefinita, la chiave EF considera come non generati dal database perché la colonna è la relazione di identificazione.

### <a name="the-instructor-navigation-property"></a>La proprietà di navigazione Instructor

Entità Instructor è nullable `OfficeAssignment` proprietà di navigazione (perché un docente potrebbe non disporre di un'assegnazione di office), e l'entità OfficeAssignment ha non-nullable `Instructor` proprietà di navigazione (perché non è un'assegnazione di office esistere senza un docente - `InstructorID` non ammette valori null). Quando un'entità Instructor dispone di un'entità correlata di OfficeAssignment, ogni entità saranno necessario un riferimento a altro nella relativa proprietà di navigazione.

È possibile inserire un `[Required]` attributo sulla proprietà di navigazione istruttore per specificare che deve essere presente un insegnante correlati, ma non è necessario eseguire questa operazione perché il `InstructorID` chiave esterna (che è anche la chiave per questa tabella) è di tipo non nullable.

## <a name="modify-the-course-entity"></a>Modificare l'entità Course

![Entità Course](complex-data-model/_static/course-entity.png)

In *Models/Course.cs*, sostituire il codice aggiunto in precedenza con il codice seguente. Le modifiche sono evidenziate.

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

L'entità corso ha una proprietà di chiave esterna `DepartmentID` che fa riferimento a entità reparto correlata che ha un `Department` proprietà di navigazione.

Entity Framework non è necessario aggiungere una proprietà di chiave esterna per il modello di dati quando si dispone di una proprietà di navigazione per un'entità correlata.  EF crea chiavi esterne nel database, ovunque siano necessarie e crea automaticamente [nascondere le proprietà](https://docs.microsoft.com/ef/core/modeling/shadow-properties) per loro. Ma con la chiave esterna nel modello di dati è possibile eseguire gli aggiornamenti più semplice ed efficiente. Ad esempio, quando si recupera un'entità del corso di modifica, l'entità Department è null se non si caricarlo, pertanto, quando si aggiorna l'entità course, è necessario prima recuperare l'entità Department. Quando la proprietà di chiave esterna `DepartmentID` viene incluso nel modello di dati, non è necessario recuperare l'entità Department prima di aggiornare.

### <a name="the-databasegenerated-attribute"></a>L'attributo DatabaseGenerated

Il `DatabaseGenerated` attributo con il `None` parametro il `CourseID` proprietà specifica che i valori di chiave primaria vengono forniti dall'utente anziché generati dal database.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

Per impostazione predefinita, Entity Framework si presuppone che i valori di chiave primaria vengono generati dal database. Che è quello desiderato nella maggior parte degli scenari. Tuttavia, per le entità Course, verrà usare un numero di corso specificato dall'utente, ad esempio una serie di 1000 per reparto, una serie di 2000 per un altro reparto e così via.

Il `DatabaseGenerated` attributo può essere utilizzato anche per generare i valori predefiniti, come nel caso di colonne di database utilizzate per registrare la data è stata creata o aggiornata una riga.  Per ulteriori informazioni, vedere [generato proprietà](https://docs.microsoft.com/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Proprietà di chiave e la navigazione esterna

La proprietà di chiave esterna e le proprietà di navigazione nell'entità Course rifletteranno le relazioni seguenti:

Un corso viene assegnato a un reparto, pertanto non c'è un `DepartmentID` chiave esterna e un `Department` proprietà di navigazione per i motivi indicati in precedenza.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

Un corso può avere qualsiasi numero di studenti, pertanto la `Enrollments` proprietà di navigazione è una raccolta:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Un corso può dedicare da istruttori più, pertanto la `CourseAssignments` proprietà di navigazione è una raccolta (il tipo `CourseAssignment` è illustrato [in un secondo momento](#many-to-many-relationships)):

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a>Creare l'entità di reparto

![Entità reparto](complex-data-model/_static/department-entity.png)


Creare *Models/Department.cs* con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>L'attributo di colonna

In precedenza è stato utilizzato il `Column` attributo per modificare il mapping di nomi di colonna. Nel codice per l'entità Department, il `Column` dell'attributo utilizzato per modificare SQL mapping dei tipi in modo che la colonna verrà definita utilizzando il tipo money di SQL Server nel database:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Mapping della colonna in genere non è necessario perché il tipo di dati di SQL Server appropriato in base al tipo CLR definito per la proprietà di sceglie Entity Framework. CLR `decimal` mapping a un Server SQL del tipo `decimal` tipo. Ma in questo caso si sa che la colonna sarà possibile che contenga gli importi in valuta e il tipo di dati money è più appropriato a tale scopo.

### <a name="foreign-key-and-navigation-properties"></a>Proprietà di chiave e la navigazione esterna

Le proprietà di navigazione e di chiave esterne rifletteranno le relazioni seguenti:

Un reparto può non avere o un amministratore e un amministratore è sempre un docente. Pertanto il `InstructorID` proprietà è inclusa come chiave esterna per l'entità Instructor e viene aggiunto un punto interrogativo dopo il `int` digitare designazione per contrassegnare la proprietà come nullable. La proprietà di navigazione denominata `Administrator` ma contiene un'entità Instructor:

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

Un reparto può avere molti corsi, pertanto non c'è una proprietà di navigazione corsi:

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> Per convenzione, Entity Framework consente l'eliminazione a catena per chiavi esterne non nullable e per le relazioni molti-a-molti. Ciò può comportare le regole delete cascade circolare, verrà generata un'eccezione quando si tenta di aggiungere una migrazione. Ad esempio, se la proprietà Department.InstructorID non è stato definito come nullable, EF sarebbe configurare una regola di delete cascade per eliminare istruttore quando si elimina il reparto, ovvero non è auspicabile che si verificano. Se le regole di business è necessario il `InstructorID` proprietà sia non nullable, è necessario utilizzare la seguente istruzione di Microsoft Office fluent API per disattivare eliminazione a catena della relazione:
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a>Modificare l'entità di registrazione

![Entità di registrazione](complex-data-model/_static/enrollment-entity.png)

In *Models/Enrollment.cs*, sostituire il codice aggiunto in precedenza con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>Proprietà di chiave e la navigazione esterna

La proprietà di chiave esterna e le proprietà di navigazione rifletteranno le relazioni seguenti:

È un record di registrazione per un singolo corso, pertanto non c'è un `CourseID` proprietà di chiave esterna e un `Course` proprietà di navigazione:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

È un record di registrazione per un singolo studente, pertanto non c'è un `StudentID` proprietà di chiave esterna e un `Student` proprietà di navigazione:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Relazioni molti-a-molti

Vi è una relazione molti-a-molti tra le entità Student e Course e l'entità di registrazione funziona come una tabella di join molti-a-molti *con payload* nel database. "Con payload" significa che la tabella di registrazione contiene dati aggiuntivi oltre a chiavi esterne per le tabelle unite in join (in questo caso, una chiave primaria e una proprietà di livello).

Nella figura seguente viene illustrato l'aspetto queste relazioni in un diagramma di entità. (Questo diagramma è stato generato utilizzando gli strumenti di Entity Framework Power per Entity Framework 6. x; si crea il diagramma non fa parte dell'esercitazione, appena viene utilizzato come un'illustrazione.)

![Corso di studenti molti-a-molti relazione](complex-data-model/_static/student-course.png)

Ogni linea della relazione dispone di un 1 al fine di uno e un asterisco (*) a altra, per indicare una relazione uno-a-molti.

Se la tabella di registrazione non include informazioni di livello, sarebbe sufficiente contenere le due chiavi esterne CourseID e StudentID. In tal caso, sarebbe una tabella di join molti-a-molti senza payload (o una tabella di join pure) del database. Le entità Instructor e corso dispongono di questo tipo di relazione molti-a-molti e il passaggio successivo consiste nel creare una classe di entità per fungere da una tabella di join senza payload.

(Supporta Entity Framework 6. x tabelle join implicito per le relazioni molti-a-molti, ma Core EF non. Per ulteriori informazioni, vedere il [discussione nel repository GitHub Core EF](https://github.com/aspnet/EntityFramework/issues/1368).) 

## <a name="the-courseassignment-entity"></a>L'entità CourseAssignment

![Entità CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

Creare *Models/CourseAssignment.cs* con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a>Aggiungere i nomi delle entità

È necessaria una tabella di join del database per la relazione molti-a-molti per istruttore-corsi e deve essere rappresentato da un set di entità. È comune per assegnare un nome di un'entità di join `EntityName1EntityName2`, che in questo caso sarebbe `CourseInstructor`. Tuttavia, è consigliabile scegliere un nome che descrive la relazione. I modelli di dati inizia semplice e aumento delle dimensioni, con join no payload spesso recupero payload più tardi. Se si inizia con un nome descrittivo di entità, non è necessario modificare il nome in un secondo momento. Idealmente, l'entità di join avrebbe il nome del relativo naturale (possibilmente singola parola) nel dominio aziendale. Documentazione e i clienti, ad esempio, potrebbero essere collegati tramite classificazioni. Per questa relazione, `CourseAssignment` è una soluzione migliore rispetto alla `CourseInstructor`.

### <a name="composite-key"></a>Chiave composta

Poiché le chiavi esterne non sono nullable e insieme in modo univoco ogni riga della tabella, non è necessario per una chiave primaria separata. Il *InstructorID* e *CourseID* proprietà dovrebbero funzionare come una chiave primaria composta. È l'unico modo per identificare le chiavi primarie composte per Entity Framework utilizzando la *API fluent* (non può essere eseguita con gli attributi). Si noterà come configurare la chiave primaria composta nella sezione successiva.

Chiave composta assicura che possono essere presenti più righe per un corso e più righe per un insegnante, non può avere più righe per la stessa instructor e corso. Il `Enrollment` join entità definisce la propria chiave primaria, sono possibili i duplicati di questo tipo. Per evitare tali duplicati, è possibile aggiungere un indice univoco per i campi di chiave esterni o configurare `Enrollment` con una chiave primaria composta simile a `CourseAssignment`. Per ulteriori informazioni, vedere [indici](https://docs.microsoft.com/ef/core/modeling/indexes).

## <a name="update-the-database-context"></a>Aggiornare il contesto del database

Aggiungere il codice evidenziato di seguito per il *Data/SchoolContext.cs* file:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

Questo codice aggiunge le nuove entità e configura chiave primaria composta CourseAssignment dell'entità.

## <a name="fluent-api-alternative-to-attributes"></a>In alternativa Fluent API per gli attributi

Il codice nel `OnModelCreating` metodo il `DbContext` utilizzato dalla classe il *fluent API* per configurare il comportamento di Entity Framework. L'API viene denominata "Microsoft Office fluent" perché viene usata spesso mettendo una serie di chiamate di metodo insieme in una singola istruzione, come in questo esempio dal [documentazione principale di EF](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

In questa esercitazione, si utilizza l'API fluent solo per mapping del database che si può fare con gli attributi. Tuttavia, è possibile utilizzare anche l'API fluent per specificare la maggior parte della formattazione, convalida e regole di mapping che è possibile eseguire utilizzando gli attributi. Alcuni attributi, ad esempio `MinimumLength` non può essere applicata con l'API fluent. Come accennato in precedenza, `MinimumLength` non modifica lo schema, si applica solo una regola di convalida sul lato client e server.

Alcuni sviluppatori preferiscono utilizzare l'API fluent esclusivamente in modo che mantengano le classi di entità "pulito". È possibile combinare gli attributi e l'API fluent se si desidera, e sono disponibili alcune personalizzazioni che è possono solo tramite l'API fluent, ma in genere di è consigliabile scegliere uno di questi due approcci e usare in che modo coerente il più possibile. Se si utilizzano entrambi, si noti che ogni volta che si verifica un conflitto, Microsoft Office Fluent API esegue l'override di attributi.

Per ulteriori informazioni sugli attributi e Microsoft Office fluent API, vedere [metodi di configurazione](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).

## <a name="entity-diagram-showing-relationships"></a>Entità diagramma che illustra le relazioni

Nella figura seguente viene illustrato il diagramma che strumenti di Entity Framework Power creare per il modello School completato.

![Diagramma di entità](complex-data-model/_static/diagram.png)

Oltre alle linee di relazione uno-a-molti (da 1 a \*), sarà possibile visualizzare la linea della relazione uno-a-zero-o-uno (da 1 a 0.1) tra le entità Instructor e OfficeAssignment e la riga zero-o-relazione uno-a-molti (0.1 a *) tra il Entità Instructor e Department.

## <a name="seed-the-database-with-test-data"></a>Valore di inizializzazione del Database con dati di Test

Sostituire il codice di *Data/DbInitializer.cs* file con il codice seguente per fornire i dati iniziali per le nuove entità creata.

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

Come osservato nella prima esercitazione, la maggior parte di questo codice crea nuovi oggetti entità e carica i dati di esempio nella proprietà come necessario per i test. Si noti la modalità di gestione delle relazioni molti-a-molti: il codice crea relazioni tramite la creazione di entità di `Enrollments` e `CourseAssignment` aggiungere set di entità.

## <a name="add-a-migration"></a>Aggiungere una migrazione

Salvare le modifiche e compilare il progetto. Aprire la finestra di comando nella cartella del progetto, quindi immettere il `migrations add` comando (non eseguire il comando update-database ancora):

```console
dotnet ef migrations add ComplexDataModel
```

Viene visualizzato un avviso sulla possibile perdita di dati.

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

Se si è tentato di eseguire il `database update` comando a questo punto (non ancora), si otterrebbe il seguente errore:

> L'istruzione ALTER TABLE è in conflitto con il vincolo FOREIGN KEY "FK_dbo. Course_dbo. Department_DepartmentID". Si è verificato il conflitto in una tabella del database "ContosoUniversity", "dbo. Reparto", colonna 'DepartmentID'.

A volte quando si esegue con dati esistenti, è necessario inserire dati stub nel database per soddisfare i vincoli di chiave esterna. Il codice generato nei componenti di `Up` metodo aggiunge una chiave esterna a DepartmentID non ammette valori null nella tabella Course. Se sono già presenti righe nella tabella Course quando viene eseguito il codice, il `AddColumn` operazione non riesce perché SQL Server non riconosce il valore da inserire nella colonna che non può essere null. Per questa esercitazione viene eseguita la migrazione in un nuovo database, ma in un'applicazione di produzione è necessario effettuare la migrazione di gestire i dati esistenti, pertanto le indicazioni riportate di seguito viene illustrato un esempio di come eseguire questa operazione.

Per rendere questa migrazione lavorare con i dati esistenti, che è necessario modificare il codice per fornire la nuova colonna un valore predefinito e creare un reparto stipendi denominata "Temp" come il reparto predefinito. Di conseguenza, esistente righe corso saranno tutti correlati al reparto "Temp" dopo il `Up` esecuzione del metodo.

* Aprire il *{timestamp}_ComplexDataModel.cs* file. 

* Impostare come commento la riga di codice che aggiunge la colonna DepartmentID è presente nella tabella Course.

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* Dopo il codice che crea la tabella di reparto, aggiungere il codice evidenziato di seguito:

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

In un'applicazione di produzione, è necessario scrivere codice o script per aggiungere righe di reparto e correlare righe corso per le nuove righe di reparto. È quindi non è più necessario il reparto "Temp" o il valore predefinito nella colonna Course.DepartmentID.

Salvare le modifiche e compilare il progetto.

## <a name="change-the-connection-string-and-update-the-database"></a>Modificare la stringa di connessione e aggiornare il database

Ora è nuovo codice `DbInitializer` classe che aggiunge i dati iniziali per le nuove entità a un database vuoto. Per creare un nuovo database vuoto EF, modificare il nome del database nella stringa di connessione in *appSettings. JSON* ContosoUniversity3 o un altro nome che non è stato utilizzato nel computer in uso.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

Salvare le modifiche apportate ai *appSettings. JSON*.

> [!NOTE]
> Come alternativa alla modifica del nome di database, è possibile eliminare il database. Utilizzare **Esplora oggetti di SQL Server** (sillaba SSOX) o `database drop` comando CLI:
> ```console
> dotnet ef database drop
> ```

Dopo aver modificato il nome del database o eliminare il database, eseguire il `database update` comando nella finestra di comando per eseguire la migrazione.

```console
dotnet ef database update
```

Eseguire l'app affinché i `DbInitializer.Initialize` metodo per eseguire e popolare il nuovo database.

Aprire il database in sillaba SSOX come fatto in precedenza ed espandere il **tabelle** nodo per visualizzare tutte le tabelle che sono state create. (Se è ancora aperto sillaba SSOX dal momento precedente, fare clic su di **aggiornamento** pulsante.)

![Tabelle in sillaba SSOX](complex-data-model/_static/ssox-tables.png)

Eseguire l'app per attivare il codice di inizializzatore che esegue il seeding del database.

Fare doppio clic su di **CourseAssignment** tabella e selezionare **Visualizza dati** per verificare che disponga di dati in essa contenuti.

![Dati CourseAssignment sillaba SSOX](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a>Riepilogo

È ora disponibile un modello di dati più complesso e il database corrispondente. Nell'esercitazione seguente, si apprenderà ulteriori informazioni su come accedere ai dati correlati.

>[!div class="step-by-step"]
[Precedente](migrations.md)
[Successivo](read-related-data.md)  
