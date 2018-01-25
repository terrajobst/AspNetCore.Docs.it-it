---
title: Pagine Razor con Entity Framework Core - modello di dati - 5 di 8
author: rick-anderson
description: "In questa esercitazione aggiungere altre entità e relazioni e personalizzare il modello di dati specificando le regole di mapping del database, convalida e formattazione."
ms.author: riande
manager: wpickett
ms.date: 10/25/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 2446f4734e9bb1ab6829001f6e7888c4c14ee1b7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="creating-a-complex-data-model---ef-core-with-razor-pages-tutorial-5-of-8"></a>Creazione di un modello di dati complessi - Core EF esercitazione pagine Razor (5 di 8)

Da [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Nelle esercitazioni precedenti collaborato con un modello di dati di base che è stato composto da tre entità. In questa esercitazione:

* Vengono aggiunte più entità e relazioni.
* Il modello di dati personalizzato specificando le regole di mapping del database, convalida e formattazione.

Le classi di entità per il modello di dati completo è illustrato nella figura seguente:

![Diagramma di entità](complex-data-model/_static/diagram.png)

Se si verificano problemi, è possibile risolvere, scaricare il [app completata per questa fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).

## <a name="customize-the-data-model-with-attributes"></a>Personalizzare il modello di dati con gli attributi

In questa sezione, il modello di dati è stato personalizzato utilizzando gli attributi.

### <a name="the-datatype-attribute"></a>L'attributo di tipo di dati

Attualmente, le pagine di student Visualizza l'ora della data di registrazione. I campi Data visualizzano in genere, solo la data e non l'ora.

Aggiornamento *Models/Student.cs* con il seguente codice evidenziato:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

Il [DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attributo specifica un tipo di dati che è più specifico di tipo intrinseco del database. In questo caso che solo la data deve essere visualizzata, non la data e ora. Il [enumerazione DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) fornisce per molti tipi di dati, ad esempio data, ora, numero di telefono, valuta, EmailAddress, e così via. Il `DataType` attributo può anche consentire all'app di fornire automaticamente le funzionalità specifiche del tipo. Ad esempio:

* Il `mailto:` collegamento viene creato automaticamente per `DataType.EmailAddress`.
* Il selettore data viene fornito per `DataType.Date` nella maggior parte dei browser.

Il `DataType` attributo genera HTML 5 `data-` attributi (si pronuncia dati dash) che utilizzano browser HTML 5. Il `DataType` gli attributi non forniscono la convalida.

`DataType.Date`non specificare il formato della data che viene visualizzato. Per impostazione predefinita, viene visualizzato il campo Data in base ai formati predefiniti in base al server [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).

L'attributo `DisplayFormat` viene usato per specificare in modo esplicito il formato della data:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

Il `ApplyFormatInEditMode` impostazione specifica che la formattazione deve essere applicata anche all'interfaccia di modifica. Alcuni campi non devono utilizzare `ApplyFormatInEditMode`. Ad esempio, il simbolo di valuta in genere non deve essere visualizzato in una casella di testo.

Il `DisplayFormat` attributo può essere utilizzato da solo. È in genere consigliabile utilizzare il `DataType` attributo con il `DisplayFormat` attributo. Il `DataType` attributo fornisce la semantica dei dati anziché come eseguirne il rendering in una schermata. Il `DataType` attributo offre i vantaggi seguenti che non sono disponibili in `DisplayFormat`:

* Il browser è possibile abilitare le funzionalità di HTML5. Ad esempio, Mostra un controllo di calendario, il simbolo di valuta delle impostazioni locali appropriata, i collegamenti di posta elettronica, la convalida dell'input sul lato client e così via.
* Per impostazione predefinita, il browser esegue il rendering di dati utilizzando il formato corretto in base alle impostazioni locali.

Per ulteriori informazioni, vedere il [ \<input > documentazione di supporto di Tag](xref:mvc/views/working-with-forms#the-input-tag-helper).

Eseguire l'app. Passare alla pagina di indice di studenti. Volte in cui non sono più visualizzati. Ogni vista che utilizza il `Student` modello consente di visualizzare la data senza ora.

![Pagina di indice di studenti con date senza tempi di](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>L'attributo StringLength

Regole di convalida dei dati e i messaggi di errore di convalida possono essere specificati con gli attributi. Il [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attributo specifica la lunghezza minima e massima dei caratteri consentiti in un campo dati. Il `StringLength` attributo fornisce anche la convalida lato client e lato server. Il valore minimo non ha alcun impatto sullo schema di database.

Aggiornamento di `Student` modello con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

Il codice precedente limita i nomi a non più di 50 caratteri. Il `StringLength` non impedisca a un utente di immettere lo spazio vuoto per un nome di attributo. Il [RegularExpression](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attributo viene utilizzato per applicare le restrizioni per l'input. Ad esempio, il codice seguente richiede il primo carattere da maiuscolo e i caratteri rimanenti alfabetico:

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

Eseguire l'app:

* Passare alla pagina di studenti.
* Selezionare **Crea nuovo**, immettere un nome più di 50 caratteri.
* Selezionare **crea**, la convalida lato client verrà visualizzato un messaggio di errore.

![Gli studenti indice pagina la visualizzazione di errori di lunghezza stringa](complex-data-model/_static/string-length-errors.png)

In **Esplora oggetti di SQL Server** (sillaba SSOX), aprire Progettazione tabelle Student facendo doppio clic il **Student** tabella.

![Tabella di studenti in sillaba SSOX prima migrazioni](complex-data-model/_static/ssox-before-migration.png)

L'immagine precedente mostra lo schema per il `Student` tabella. I campi nome hanno tipo `nvarchar(MAX)` migrazioni non è stato eseguito nel database. Quando le migrazioni vengono eseguite più avanti in questa esercitazione, i campi nome diventano `nvarchar(50)`.

### <a name="the-column-attribute"></a>L'attributo di colonna

Gli attributi è possono controllare le modalità di mapping di classi e proprietà per il database. In questa sezione, il `Column` attributo viene utilizzato per il mapping del nome del `FirstMidName` proprietà su "FirstName" nel database.

Quando viene creato il database, i nomi delle proprietà nel modello vengono utilizzati per i nomi di colonna (tranne quando la `Column` attributo viene utilizzato).

Il `Student` modello utilizza `FirstMidName` per il nome del primo campo perché il campo può contenere anche un cognome.

Aggiornamento di *Student.cs* file con il codice evidenziato di seguito:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

Con la modifica precedente, `Student.FirstMidName` nell'app esegue il mapping al `FirstName` colonna il `Student` tabella.

L'aggiunta del `Column` attributo modifica il supporto del modello di `SchoolContext`. Il supporto del modello di `SchoolContext` non corrisponde più al database. Se si esegue l'app prima di applicare le migrazioni, viene generata l'eccezione seguente:

```SQL
SqlException: Invalid column name 'FirstName'.
```
Per aggiornare il database:

* Compilare il progetto.
* Aprire una finestra di comando nella cartella del progetto. Immettere i comandi seguenti per creare una nuova migrazione e aggiornare il database:

    ```console
    dotnet ef migrations add ColumnFirstName
    dotnet ef database update
    ```

Il `dotnet ef migrations add ColumnFirstName` comando genera il messaggio di avviso seguente:

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

L'avviso viene generato perché i campi nome sono ora limitate a 50 caratteri. Se un nome nel database di più di 50 caratteri, il 51 fino all'ultimo carattere andrebbero persi.

* Testare l'app.

Aprire la tabella di studenti in sillaba SSOX:

![Tabella di studenti in sillaba SSOX dopo le migrazioni](complex-data-model/_static/ssox-after-migration.png)

Prima dell'applicazione di migrazione, le colonne nome sono di tipo [nvarchar (max)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql). Le colonne nome sono ora `nvarchar(50)`. Il nome della colonna è stato modificato da `FirstMidName` a `FirstName`.

> [!Note]
> Nella sezione seguente, compilazione dell'applicazione in alcune fasi genera errori del compilatore. Le istruzioni specificano quando deve compilare l'applicazione.

## <a name="student-entity-update"></a>Aggiornamento dell'entità per studenti

![Entità Student](complex-data-model/_static/student-entity.png)

Aggiornamento *Models/Student.cs* con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>L'attributo obbligatorio

Il `Required` attributo rende i campi obbligatori di nome proprietà. Il `Required` attributo non è necessaria per i tipi non nullable, ad esempio i tipi di valore (`DateTime`, `int`, `double`, ecc.). Tipi che non possono essere null vengono considerati automaticamente come i campi obbligatori.

Il `Required` attributo potrebbe essere sostituito con un parametro di lunghezza minima nel `StringLength` attributo:

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>L'attributo di visualizzazione

Il `Display` attributo specifica che la didascalia per le caselle di testo deve essere "Nome", "Last Name", "Nome" e "Data di registrazione". Le didascalie predefinita sono Nessuno spazio dividendo le parole, ad esempio "Cognome".

### <a name="the-fullname-calculated-property"></a>La proprietà FullName calcolato

`FullName`è una proprietà calcolata che restituisce un valore che viene creato concatenando due altre proprietà. `FullName`non è possibile impostare, che contiene solo una funzione di accesso get. Non `FullName` colonna viene creata nel database.

## <a name="create-the-instructor-entity"></a>Creare l'entità Instructor

![Entità Instructor](complex-data-model/_static/instructor-entity.png)

Creare *Models/Instructor.cs* con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

Si noti che molte proprietà sono la stessa nella `Student` e `Instructor` entità. Nell'esercitazione di ereditarietà di implementazione più avanti in questa serie, viene effettuato il refactoring di questo codice per eliminare la ridondanza.

Più attributi possono essere su una riga. Il `HireDate` attributi può essere scritto come segue:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>Le proprietà di navigazione CourseAssignments e OfficeAssignment

Il `CourseAssignments` e `OfficeAssignment` sono proprietà di navigazione.

Un docente può indicare un numero qualsiasi di corsi, pertanto `CourseAssignments` è definito come una raccolta.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Se una proprietà di navigazione contiene più entità:

* Deve essere un tipo di elenco in cui le voci possono essere aggiunte, eliminate e aggiornate.

Tipi di proprietà di navigazione includono:

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

Se `ICollection<T>` è specificato, EF Core viene creato un `HashSet<T>` insieme per impostazione predefinita.

Il `CourseAssignment` entità è illustrato nella sezione sulle relazioni molti-a-molti.

Stato che un docente può avere al massimo un ufficio di regole di business di Contoso University. Il `OfficeAssignment` proprietà contiene un singolo `OfficeAssignment` entità. `OfficeAssignment`è null se non è assegnato.

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>Creare l'entità OfficeAssignment

![Entità OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

Creare *Models/OfficeAssignment.cs* con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>L'attributo chiave

Il `[Key]` attributo viene utilizzato per identificare una proprietà come chiave primaria (PK) quando il nome della proprietà non è diverso da classnameID o ID.

È una relazione uno-a-zero-o-uno tra il `Instructor` e `OfficeAssignment` entità. L'assegnazione dell'ufficio esiste solo in relazione a istruttore a che viene assegnato. Il `OfficeAssignment` PK è la chiave esterna (FK) per il `Instructor` entità. Core EF non riconosce automaticamente `InstructorID` come la chiave pubblica di `OfficeAssignment` perché:

* `InstructorID`non rispetta la convenzione di denominazione ID o classnameID.

Pertanto, il `Key` attributo viene utilizzato per identificare `InstructorID` come la chiave pubblica:

```csharp
[Key]
public int InstructorID { get; set; }
```

Per impostazione predefinita, Core EF considera la chiave non generati dal database perché la colonna è la relazione di identificazione.

### <a name="the-instructor-navigation-property"></a>La proprietà di navigazione Instructor

Il `OfficeAssignment` proprietà di navigazione per il `Instructor` entità è nullable perché:

* I tipi di riferimento (ad esempio le classi sono ammette valori null).
* Un docente potrebbe non disporre di un'assegnazione di office.


Il `OfficeAssignment` entità dispone di un non-nullable `Instructor` proprietà di navigazione perché:

* `InstructorID`sono non nullable.
* L'assegnazione dell'ufficio non può esistere senza un docente.

Quando un `Instructor` entità è correlata una `OfficeAssignment` entità, ogni entità dispone di un riferimento a altro nella relativa proprietà di navigazione.

Il `[Required]` attributo può essere applicato per la `Instructor` proprietà di navigazione:

```csharp
[Required]
public Instructor Instructor { get; set; }
```

Il codice precedente specifica che deve essere un insegnante correlati. Il codice precedente non è necessario perché il `InstructorID` chiave esterna (che è anche la chiave pubblica) è di tipo non nullable.

## <a name="modify-the-course-entity"></a>Modificare l'entità Course

![Entità Course](complex-data-model/_static/course-entity.png)

Aggiornamento *Models/Course.cs* con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

Il `Course` entità dispone di una proprietà di chiave esterna (FK) `DepartmentID`. `DepartmentID`punta all'oggetto correlato `Department` entità. Il `Course` entità dispone di un `Department` proprietà di navigazione.

Core EF non richiede una proprietà di chiave esterna per un modello di dati quando il modello dispone di una proprietà di navigazione per un'entità correlata.

Componenti di base di Entity Framework crea automaticamente chiavi esterne nel database ogni volta che sono necessari. Crea Core EF [nascondere le proprietà](https://docs.microsoft.com/ef/core/modeling/shadow-properties) per chiavi esterne creati automaticamente. Con la chiave esterna nel modello di dati possono effettuare aggiornamenti più semplice ed efficiente. Si consideri ad esempio un modello in cui la proprietà di chiave esterna `DepartmentID` è *non* inclusi. Quando un'entità course viene recuperata da modificare:

* Il `Department` entità è null se non viene caricato in modo non esplicito.
* Per aggiornare l'entità course, il `Department` entità deve prima essere recuperato.

Quando la proprietà di chiave esterna `DepartmentID` viene incluso nel modello di dati, non è necessario recuperare il `Department` entità prima dell'aggiornamento.

### <a name="the-databasegenerated-attribute"></a>L'attributo DatabaseGenerated

Il `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attributo specifica che la chiave pubblica viene fornita dall'applicazione anziché generato dal database.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

Per impostazione predefinita, Core EF si presuppone che i valori di chiave primaria vengono generati dal database. DB generato PK valori è in genere l'approccio migliore. Per `Course` entità, l'utente specifica il PK. Ad esempio, un numero di corso, ad esempio una serie di 1000 per il reparto di matematica, una serie di 2000 per il reparto in lingua inglese.

Il `DatabaseGenerated` attributo può essere utilizzato anche per generare i valori predefiniti. Ad esempio, il database è possibile generare automaticamente un campo della data per registrare la data è stata creata o aggiornata una riga. Per ulteriori informazioni, vedere [generato proprietà](https://docs.microsoft.com/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Proprietà di chiave e la navigazione esterna

Le proprietà di chiave esterna (FK) e una proprietà di navigazione nel `Course` entità riflettere le relazioni seguenti:

Un corso viene assegnato a un reparto, pertanto non c'è un `DepartmentID` FK e un `Department` proprietà di navigazione.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

Un corso può avere qualsiasi numero di studenti, pertanto la `Enrollments` proprietà di navigazione è una raccolta:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Un corso può dedicare da istruttori più, pertanto la `CourseAssignments` proprietà di navigazione è una raccolta:

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

`CourseAssignment`viene spiegato [in un secondo momento](#many-to-many-relationships).

## <a name="create-the-department-entity"></a>Creare l'entità di reparto

![Entità reparto](complex-data-model/_static/department-entity.png)

Creare *Models/Department.cs* con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>L'attributo di colonna

In precedenza il `Column` attributo è stato usato per modificare il mapping di nomi di colonna. Nel codice per il `Department` entità, il `Column` attributo viene utilizzato per modificare i mapping dei tipi di dati SQL. Il `Budget` colonna viene definita utilizzando il tipo money di SQL Server nel database:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Mapping della colonna non è in genere necessario. Core EF sceglie in genere il tipo di dati di SQL Server appropriato in base al tipo CLR per la proprietà. CLR `decimal` mapping a un Server SQL del tipo `decimal` tipo. `Budget`è per la valuta, e il tipo di dati money è più adatto per la valuta.

### <a name="foreign-key-and-navigation-properties"></a>Proprietà di chiave e la navigazione esterna

Le proprietà di chiave esterna e navigazione rifletteranno le relazioni seguenti:

* Un reparto può o non dispone di un amministratore.
* Un amministratore è sempre un docente. Pertanto il `InstructorID` proprietà è inclusa come la chiave esterna per il `Instructor` entità.

La proprietà di navigazione è denominata `Administrator` ma contiene un `Instructor` entità:

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

Il punto interrogativo (?) nel codice precedente specifica che la proprietà è nullable.

Un reparto può avere molti corsi, pertanto non c'è una proprietà di navigazione corsi:

```csharp
public ICollection<Course> Courses { get; set; }
```

Nota: Per convenzione, Core EF Abilita eliminazione a catena per chiavi esterne non nullable e per le relazioni molti-a-molti. Eliminazione a catena può comportare regole delete cascade circolare. Circolare eliminazione a catena cause regole un'eccezione quando viene aggiunta una migrazione.

Ad esempio, se il `Department.InstructorID` proprietà non è stata definita come nullable:

* Componenti di base di Entity Framework consente di configurare una regola di delete cascade per eliminare istruttore quando viene eliminato il reparto.
* L'eliminazione istruttore quando viene eliminato il reparto non è il comportamento previsto.

Se le regole di business è necessario il `InstructorID` proprietà essere non nullable, utilizzare la seguente istruzione di Microsoft Office fluent API:

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

Il codice precedente Disabilita eliminazione a catena della relazione insegnante di reparto.

## <a name="update-the-enrollment-entity"></a>Aggiornare l'entità di registrazione

È un record di registrazione per un una corso impiegato da uno studente.

![Entità di registrazione](complex-data-model/_static/enrollment-entity.png)

Aggiornamento *Models/Enrollment.cs* con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>Proprietà di chiave e la navigazione esterna

La proprietà di chiave esterna e le proprietà di navigazione rifletteranno le relazioni seguenti:

È un record di registrazione per un corso, pertanto non c'è un `CourseID` proprietà di chiave esterna e un `Course` proprietà di navigazione:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

È un record di registrazione per uno studente, pertanto non c'è un `StudentID` proprietà di chiave esterna e un `Student` proprietà di navigazione:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Relazioni molti-a-molti

È una relazione molti-a-molti tra la `Student` e `Course` entità. Il `Enrollment` entità funziona come una tabella di join molti-a-molti *con payload* nel database. "Con payload" significa che il `Enrollment` tabella contiene dati aggiuntivi oltre a chiavi esterne per le tabelle unite in join (in questo caso, la chiave pubblica e `Grade`).

Nella figura seguente viene illustrato l'aspetto queste relazioni in un diagramma di entità. (Questo diagramma è stato generato utilizzando EF Power Tools di Entity Framework 6. x. Creazione del diagramma non fa parte dell'esercitazione.)

![Corso di studenti molti-a-molti relazione](complex-data-model/_static/student-course.png)

Ogni linea della relazione dispone di un 1 al fine di uno e un asterisco (*) a altra, per indicare una relazione uno-a-molti.

Se il `Enrollment` tabella non include informazioni di livello, sufficiente contenere le due chiavi esterne (`CourseID` e `StudentID`). Una tabella di join molti-a-molti senza payload viene talvolta definita una tabella di join pura (PJT).

Il `Instructor` e `Course` le entità dispongono di una relazione molti-a-molti con una tabella di join pure.

Nota: Tabelle di join implicito per le relazioni molti-a-molti, ma Core EF non supporta 6. x EF. Per ulteriori informazioni, vedere [molti-a-molti relazioni in Entity Framework Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).

## <a name="the-courseassignment-entity"></a>L'entità CourseAssignment

![Entità CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

Creare *Models/CourseAssignment.cs* con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a>Per corsi di Instructor

![Per corsi di istruttore m:M](complex-data-model/_static/courseassignment.png)

La relazione molti-a-molti per istruttore-corsi:

* È necessaria una tabella di join che deve essere rappresentata da un set di entità.
* È una tabella di join pure (tabella senza payload).

È comune per assegnare un nome di un'entità di join `EntityName1EntityName2`. Ad esempio, la tabella di join istruttore-a-corsi usando questo modello è `CourseInstructor`. Tuttavia, è consigliabile usare un nome che descrive la relazione.

I modelli di dati inizia semplice e aumento delle dimensioni. Join non-payload (PJTs) spesso sviluppare per includere i payload. A partire dal nome di un'entità descrittivo, il nome non deve modificare la tabella di join cambia. Idealmente, l'entità di join avrebbe il nome del relativo naturale (possibilmente singola parola) nel dominio aziendale. Documentazione e i clienti, ad esempio, potrebbero essere collegati con un'entità di join denominata classificazioni. Per la relazione molti-a-molti per istruttore-corsi, `CourseAssignment` è preferibile `CourseInstructor`.

### <a name="composite-key"></a>Chiave composta

Chiavi esterne non sono nullable. Le due chiavi esterne in `CourseAssignment` (`InstructorID` e `CourseID`) insieme identificare in modo univoco ogni riga del `CourseAssignment` tabella. `CourseAssignment`non richiede un PK dedicato. Il `InstructorID` e `CourseID` proprietà funzionano come una chiave composta È l'unico modo per specificare composito usata a EF Core con il *API fluent*. La sezione successiva illustra come configurare il PK composito.

Chiave composta garantisce:

* Per un corso sono consentite più righe.
* Per un insegnante sono consentite più righe.
* Non sono consentite più righe per lo stesso instructor e course.

Il `Enrollment` entità join definisce il proprio PK, i duplicati di questo tipo sono possibili. Per evitare tali duplicati:

* Aggiungere un indice univoco per i campi di chiave esterna, o
* Configurare `Enrollment` con una chiave primaria composta simile a `CourseAssignment`. Per ulteriori informazioni, vedere [indici](https://docs.microsoft.com/ef/core/modeling/indexes).

## <a name="update-the-db-context"></a>Aggiornare il contesto DB

Aggiungere il codice evidenziato di seguito per *Data/SchoolContext.cs*:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

Il codice precedente aggiunge le nuove entità e configura il `CourseAssignment` PK. composito dell'entità

## <a name="fluent-api-alternative-to-attributes"></a>In alternativa Fluent API per gli attributi

Il `OnModelCreating` metodo nel passaggio precedente di codice viene utilizzato il *fluent API* per configurare il comportamento principale di Entity Framework. L'API viene denominata "Microsoft Office fluent" perché viene usata spesso mettendo una serie di chiamate al metodo in una singola istruzione. Il [seguente codice](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) è riportato un esempio di Microsoft Office fluent API:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

In questa esercitazione, l'API fluent viene utilizzato solo per il mapping di database che non può essere eseguito con gli attributi. Tuttavia, l'API fluent è possibile specificare la maggior parte delle formattazioni, convalida e regole di mapping che possono essere eseguite con gli attributi.

Alcuni attributi, ad esempio `MinimumLength` non può essere applicata con l'API fluent. `MinimumLength`non modificare lo schema, si applica solo una regola di convalida della lunghezza minima.

Alcuni sviluppatori preferiscono utilizzare l'API fluent esclusivamente in modo che mantengano le classi di entità "pulito". È possibile combinare gli attributi e l'API fluent. Esistono alcune configurazioni che possono essere eseguite solo con l'API fluent (specificando una chiave primaria composta). Esistono alcune configurazioni che possono essere eseguite solo con gli attributi (`MinimumLength`). La procedura consigliata per l'utilizzo di Microsoft Office fluent API o attributi:

* Scegliere uno di questi due approcci.
* Utilizzare l'approccio scelto in modo coerente il più possibile.

Alcuni degli attributi utilizzati in questa esercitazione vengono utilizzati per:

* Convalida solo (ad esempio, `MinimumLength`).
* Configurazione di base EF solo (ad esempio, `HasKey`).
* Configurazione di convalida e di Core di Entity Framework (ad esempio, `[StringLength(50)]`).

Per ulteriori informazioni sugli attributi e Microsoft Office fluent API, vedere [metodi di configurazione](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).

## <a name="entity-diagram-showing-relationships"></a>Entità diagramma che illustra le relazioni

Nella figura seguente viene illustrato il diagramma creati Power Tools di Entity Framework per il modello School completato.

![Diagramma di entità](complex-data-model/_static/diagram.png)

Il diagramma precedente mostra:

* Più righe di relazione uno-a-molti (da 1 a \*).
* La linea della relazione uno-a-zero-o-uno (da 1 a 0..1) tra il `Instructor` e `OfficeAssignment` entità.
* La riga zero-o-relazione uno-a-molti (0..1 a *) tra il `Instructor` e `Department` entità.

## <a name="seed-the-db-with-test-data"></a>Valore di inizializzazione del database con dati di Test

Aggiornare il codice in *Data/DbInitializer.cs*:

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

Il codice precedente fornisce i dati di inizializzazione per le nuove entità. La maggior parte di questo codice crea nuovi oggetti entità e carica i dati di esempio. I dati di esempio viene utilizzati per il test. Il codice precedente crea le relazioni molti-a-molti seguente:

* `Enrollments`
* `CourseAssignment`

Nota: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) supporterà [seeding dei dati](https://github.com/aspnet/EntityFrameworkCore/issues/629).

## <a name="add-a-migration"></a>Aggiungere una migrazione

Compilare il progetto. Aprire una finestra di comando nella cartella del progetto e immettere il comando seguente:

```console
dotnet ef migrations add ComplexDataModel
```

Il comando precedente viene visualizzato un avviso sulla possibile perdita di dati.

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

Se il `database update` comando viene eseguito, viene generato l'errore seguente:

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

Quando le migrazioni vengono eseguite con i dati esistenti, potrebbero essere presenti vincoli di chiave esterna che non vengono soddisfatte con i dati esistenti. Per questa esercitazione, viene creato un nuovo database, pertanto non vi sono violazioni dei vincoli di chiave esterna. Vedere [correzione vincoli di chiave esterna con dati legacy](#fk) per istruzioni su come correggere le violazioni di chiave esterna nel database corrente.

## <a name="change-the-connection-string-and-update-the-db"></a>Modificare la stringa di connessione e aggiornare il database

Il codice aggiornato `DbInitializer` aggiunge dati di inizializzazione per le nuove entità. Per forzare il Core di Entity Framework per creare un nuovo database vuoto:

* Modificare il nome di stringa di connessione di database in *appSettings. JSON* a ContosoUniversity3. Il nuovo nome deve essere un nome che non è stato utilizzato nel computer.

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
      },
    ```

* In alternativa, eliminare il database utilizzando:

    * **Esplora oggetti di SQL Server** (sillaba SSOX).
    * Il `database drop` comando CLI:

   ```console
   dotnet ef database drop
   ```

Eseguire `database update` nella finestra di comando:

```console
dotnet ef database update
```

Il comando precedente viene eseguito tutte le migrazioni.

Eseguire l'app. Esegue l'app viene eseguita la `DbInitializer.Initialize` metodo. Il `DbInitializer.Initialize` consente di popolare il nuovo database.

Aprire il database in sillaba SSOX:

* Espandere il **tabelle** nodo. Vengono visualizzate le tabelle create.
* Se sillaba SSOX è stato aperto in precedenza, fare clic su di **aggiornamento** pulsante.

![Tabelle in sillaba SSOX](complex-data-model/_static/ssox-tables.png)

Esaminare il **CourseAssignment** tabella:

* Fare doppio clic su di **CourseAssignment** tabella e selezionare **Visualizza dati**.
* Verificare il **CourseAssignment** tabella contiene dati.

![Dati CourseAssignment sillaba SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a>Correzione di vincoli di chiave esterna con dati legacy

In questa sezione è facoltativa.

Quando le migrazioni vengono eseguite con i dati esistenti, potrebbero essere presenti vincoli di chiave esterna che non vengono soddisfatte con i dati esistenti. Con dati di produzione, è necessario eseguire passaggi per la migrazione dei dati esistenti. In questa sezione viene fornito un esempio di correzione di violazioni dei vincoli di chiave esterna. Non apportare queste modifiche al codice senza un backup. Non apportare queste modifiche di codice se è completata la sezione precedente e il database aggiornato.

Il *{timestamp}_ComplexDataModel.cs* file contiene il codice seguente:

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

Il codice precedente aggiunge un non-nullable `DepartmentID` chiave esterna per il `Course` tabella. Il database dall'esercitazione precedente contiene righe di `Course`, in modo che la tabella non può essere aggiornata le migrazioni.

Per rendere il `ComplexDataModel` lavoro di migrazione con i dati esistenti:

* Modificare il codice per fornire la nuova colonna (`DepartmentID`) un valore predefinito.
* Creare un reparto FALSO denominato "Temp" come il reparto predefinito.

### <a name="fix-the-foreign-key-constraints"></a>Correggere i vincoli di chiave esterni

Aggiornamento di `ComplexDataModel` classi `Up` metodo:

* Aprire il *{timestamp}_ComplexDataModel.cs* file.
* Impostare come commento la riga di codice che aggiunge il `DepartmentID` colonna per il `Course` tabella.

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

Aggiungere il codice evidenziato di seguito. Il nuovo codice viene inserito dopo il `.CreateTable( name: "Department"` blocco:[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

Con le modifiche precedenti, esistente `Course` righe saranno correlate al reparto "Temp" dopo il `ComplexDataModel` `Up` esecuzione del metodo.

Un'app di produzione è necessario:

* Includere codice o script per aggiungere `Department` righe e le relative `Course` righe al nuovo `Department` righe.
* Non utilizzare il reparto "Temp" o il valore predefinito per `Course.DepartmentID`.

L'esercitazione successiva illustra i dati correlati.

>[!div class="step-by-step"]
[Precedente](xref:data/ef-rp/migrations)
[Successivo](xref:data/ef-rp/read-related-data)
