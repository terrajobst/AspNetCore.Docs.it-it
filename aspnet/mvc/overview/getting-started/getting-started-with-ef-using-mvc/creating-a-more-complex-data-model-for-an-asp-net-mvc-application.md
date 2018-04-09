---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: Creazione di un modello di dati più complesso per un'applicazione ASP.NET MVC | Documenti Microsoft
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 5 con Code First di Entity Framework 6 e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 46f7f3c9-274f-4649-811d-92222a9b27e2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: fd8bf6502b0dd261505a86a2ed86d4c3f42e8755
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application"></a>Creazione di un modello di dati più complesso per un'applicazione MVC ASP.NET
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [Scarica il PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 5 con Visual Studio 2013 e Code First di Entity Framework 6. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Nelle esercitazioni precedenti è stato utilizzato un modello di dati semplici che è stato composto da tre entità. In questa esercitazione si aggiungeranno ulteriori entità e relazioni e verrà personalizzato il modello di dati specificando le regole di mapping del database, convalida e formattazione. Si noterà due modi per personalizzare il modello di dati: aggiungendo attributi alle classi di entità e aggiungendo codice alla classe contesto di database.

Al termine dell'operazione le classi di entità verranno incluse nel modello di dati completato, illustrato nella figura seguente:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Personalizzare il modello di dati usando gli attributi

In questa sezione si apprenderà come personalizzare il modello di dati usando attributi che specificano regole di formattazione, convalida e mapping del database. Quindi in diversi nelle sezioni seguenti si creeranno completo `School` modello di dati aggiungendo gli attributi alle classi è già create e la creazione di nuove classi per i tipi di entità rimanenti nel modello.

### <a name="the-datatype-attribute"></a>L'attributo di tipo di dati

Per le date di iscrizione degli studenti, tutte le pagine Web attualmente visualizzano l'ora oltre alla data, anche se l'unico elemento rilevante di questo campo è la data. Mediante gli attributi di annotazione dei dati è possibile modificare il codice per correggere il formato di visualizzazione in tutte le visualizzazioni che visualizzano i dati. Per un esempio di come eseguire questa operazione si aggiunge un attributo alla proprietà `EnrollmentDate` nella classe `Student`.

In *Models\Student.cs*, aggiungere un `using` istruzione per il `System.ComponentModel.DataAnnotations` dello spazio dei nomi e aggiungere `DataType` e `DisplayFormat` gli attributi di `EnrollmentDate` proprietà, come illustrato nell'esempio seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,12-13)]

Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributo viene utilizzato per specificare un tipo di dati che è più specifico di tipo intrinseco del database. In questo caso si vuole tenere traccia solo della data e non di data e ora. Il [enumerazione DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) fornisce per molti tipi di dati, ad esempio *data, ora, numero di telefono, valuta, EmailAddress* e altro ancora. L'attributo `DataType` può anche consentire all'applicazione di fornire automaticamente le funzionalità specifiche del tipo. Ad esempio, un `mailto:` possibile creare un collegamento per [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), e un selettore data può essere fornito per [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) nei browser che supportano [HTML5](http://html5.org/). Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributi genera HTML 5 [dati -](http://ejohn.org/blog/html-5-data-attributes/) (si pronuncia *dash dati*) gli attributi che è in grado di riconoscere i browser HTML 5. Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributi non forniscono alcuna convalida.

`DataType.Date` non specifica il formato della data visualizzata. Per impostazione predefinita, viene visualizzato il campo dei dati in base ai formati predefiniti in base al server [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

L'attributo `DisplayFormat` viene usato per specificare in modo esplicito il formato della data:


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


Il `ApplyFormatInEditMode` impostazione specifica che la formattazione specificata deve essere applicata anche quando il valore viene visualizzato in una casella di testo per la modifica. (Non è consigliabile che per alcuni campi, ad esempio, per i valori di valuta, non è il simbolo di valuta nella casella di testo per la modifica.)

È possibile utilizzare il [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attributo da se stesso, ma in genere è consigliabile utilizzare il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) anche l'attributo. Il `DataType` attributo fornisce il *semantica* dei dati come anziché come eseguirne il rendering in una schermata e fornisce i seguenti vantaggi che non si ottengono con `DisplayFormat`:

- Il browser può abilitare le funzionalità HTML5, ad esempio per visualizzare un controllo di calendario, il simbolo della valuta appropriato per le impostazioni locali, i collegamenti alla posta elettronica, alcune istanze di convalida lato client e così via.
- Per impostazione predefinita, il browser verrà eseguito il rendering di dati utilizzando il formato corretto in base il [internazionali](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributo è possibile abilitare MVC scegliere il modello di campo a destra per il rendering dei dati (il [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) utilizza il modello di stringa). Per ulteriori informazioni, vedere Brad Wilson [ASP.NET MVC 2 Templates](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Anche se scritte per MVC 2, in questo articolo ancora si applica alla versione corrente di ASP.NET MVC).

Se si utilizza il `DataType` attributo con un campo Data, è necessario specificare il `DisplayFormat` attributo anche per garantire che il campo viene visualizzato correttamente in browser Chrome. Per ulteriori informazioni, vedere [questo thread StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Per ulteriori informazioni sulle modalità di gestione di altri formati di data in MVC, visitare [MVC 5 Introduzione: esaminando il metodi di modifica e visualizzazione di modifica](../introduction/examining-the-edit-methods-and-edit-view.md) e nella pagina di ricerca &quot;internazionali&quot;.

Eseguire nuovamente la pagina di indice per studenti e notare che volte non vengono più visualizzate per le date di registrazione. Lo stesso sarà true per qualsiasi vista che utilizza il `Student` modello.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>L'elemento StringLengthAttribute

È anche possibile specificare regole di convalida dei dati e messaggi di errore di convalida mediante gli attributi. Il [StringLength attributo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) imposta la lunghezza massima del database e fornisce lato client e lato server della convalida di ASP.NET MVC. È anche possibile specificare la lunghezza minima della stringa in questo attributo, ma il valore minimo non ha alcun effetto sullo schema del database.

Ad esempio si supponga di voler limitare a 50 il numero massimo di caratteri che gli utenti possono immettere per un nome. Per aggiungere questa limitazione, è necessario [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) gli attributi di `LastName` e `FirstMidName` proprietà, come illustrato nell'esempio seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

Il [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attributo non impedisce a un utente di immettere lo spazio vuoto per un nome. È possibile utilizzare il [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) attributo per applicare restrizioni per l'input. Ad esempio il codice seguente richiede il primo carattere da maiuscolo e i caratteri rimanenti alfabetico:

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

Il [MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) attributo fornisce funzionalità simili a quelle di [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attributo ma non è disponibile sul lato client convalida.

Eseguire l'applicazione e fare clic su di **studenti** scheda. Viene visualizzato l'errore seguente:

*Il modello di esecuzione del backup il contesto 'SchoolContext' è stato modificato dopo la creazione del database. Provare a utilizzare migrazioni Code First per aggiornare il database ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).*

Il modello di database è stato modificato in modo che richiede una modifica nello schema del database e di Entity Framework ha rilevato che. Utilizzare le migrazioni per aggiornare lo schema senza perdere i dati aggiunti al database tramite l'interfaccia utente. Se è stato modificato i dati che è stati creati dal `Seed` (metodo), che verranno modificate allo stato originale perché il [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) metodo che si sta utilizzando il `Seed` metodo. ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) è equivalente a un'operazione "upsert" da terminologia dei database.)

Nella console di Gestione pacchetti immettere i comandi seguenti:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

Il `add-migration` comando crea un file denominato  *&lt;timeStamp&gt;\_MaxLengthOnNames.cs*. Il metodo `Up` di questo file contiene codice che aggiorna il database per adattarlo al modello di dati corrente. Il comando `update-database` ha eseguito tale codice.

Il timestamp anteposto al nome del file migrazioni utilizzato da Entity Framework per ordinare le migrazioni. È possibile creare più migrazioni prima di eseguire il `update-database` comando, quindi tutte le migrazioni sono applicati nell'ordine in cui sono stati creati.

Eseguire il **crea** pagina e immettere il nome di più di 50 caratteri. Quando si fa clic su **Crea** la convalida lato client visualizza un messaggio di errore.

![Errore val di lato client](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>L'attributo di colonna

È possibile usare gli attributi anche per controllare il mapping delle classi e delle proprietà nel database. Si supponga di aver usato il nome `FirstMidName` per il campo first-name (Nome) perché il campo potrebbe contenere anche un secondo nome. Tuttavia si vuole che la colonna di database sia denominata `FirstName`, perché gli utenti che scrivono query ad hoc per il database sono abituati a tale nome. Per eseguire questo mapping è possibile usare l'attributo `Column`.

L'attributo `Column` specifica che quando viene creato il database, la colonna della tabella `Student` mappata sulla proprietà `FirstMidName` verrà denominata `FirstName`. In altri termini, quando il codice fa riferimento a `Student.FirstMidName` i dati provengono dalla colonna `FirstName` della tabella `Student` o vengono aggiornati in tale colonna. Se non si specificano nomi di colonna, essi sono assegnato lo stesso nome come nome della proprietà.

Nel *Student.cs* file, aggiungere un `using` istruzione per [System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) e aggiungere l'attributo del nome di colonna per il `FirstMidName` proprietà, come illustrato di codice evidenziato di seguito:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

L'aggiunta del [colonna attributo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) consente di modificare il modello di backup SchoolContext, in modo che non corrisponda al database. Immettere i comandi seguenti in PMC per creare un'altra migrazione:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

In **Esplora Server**, aprire il *Student* Progettazione tabelle facendo doppio clic su di *Student* tabella.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

L'immagine seguente mostra il nome della colonna originale che aveva prima dell'applicazione le prime due migrazioni. Oltre al nome di colonna, la modifica da `FirstMidName` a `FirstName`, le due colonne nome sono stati modificati da `MAX` lunghezza a 50 caratteri.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

È possibile inoltre modificare database mapping utilizzando il [API Fluent](https://msdn.microsoft.com/data/jj591617), come verrà illustrato più avanti in questa esercitazione.

> [!NOTE]
> Se si prova a compilare prima di aver creato tutte le classi di entità delle sezioni seguenti, possono verificarsi errori di compilazione.


## <a name="complete-changes-to-the-student-entity"></a>Completare le modifiche all'entità per studenti

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

In *Models\Student.cs*, sostituire il codice aggiunto in precedenza con il codice seguente. Le modifiche sono evidenziate.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=11,13,15,18,22,25-32)]

### <a name="the-required-attribute"></a>L'attributo obbligatorio

Il [attributo obbligatorio](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) rende i campi obbligatori di nome proprietà. Il `Required attribute` non è necessario per i tipi di valore, ad esempio DateTime, int, double e float. Tipi di valore non sono assegnati un valore null, pertanto sono intrinsecamente vengono considerati come i campi obbligatori. È possibile rimuovere il [attributo obbligatorio](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) e sostituirlo con un parametro di lunghezza minima per il `StringLength` attributo:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs?highlight=2)]

### <a name="the-display-attribute"></a>L'attributo di visualizzazione

L'attributo `Display` specifica che la didascalia delle caselle di testo deve essere "First Name" (Nome), "Last Name" (Cognome), "Full Name" (Nome e cognome) ed "Enrollment Date" (Data di iscrizione) anziché il nome della proprietà (senza spazi tra le parole) in ogni istanza.

### <a name="the-fullname-calculated-property"></a>La proprietà calcolata FullName

`FullName` è una proprietà calcolata che restituisce un valore creato concatenando altre due proprietà. Pertanto ha solo un `get` della funzione di accesso e nessun `FullName` colonna verrà generata nel database.

## <a name="create-the-instructor-entity"></a>Creare l'entità Instructor

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Creare *Models\Instructor.cs*, sostituendo il codice del modello con il codice seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs)]

Si noti che molte proprietà sono uguali nelle entità `Student` e `Instructor`. Nell'esercitazione [Implementing Inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) (Implementazione dell'ereditarietà) più avanti in questa serie si effettuerà il refactoring di questo codice per eliminare la ridondanza.

È possibile inserire più attributi su una riga, pertanto potrebbe anche scrivere la classe istruttore come indicato di seguito:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>Corsi di e le proprietà di navigazione OfficeAssignment

Le proprietà `Courses` e `OfficeAssignment` sono proprietà di navigazione. Come spiegato in precedenza, vengono in genere definiti come [virtuale](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) in modo che sia possibile usufruire di una funzionalità di Entity Framework chiamata [caricamento lazy](https://msdn.microsoft.com/magazine/hh205756.aspx). Inoltre, se una proprietà di navigazione può contenere più entità, il tipo deve implementare il [ICollection&lt;T&gt; ](https://msdn.microsoft.com/library/92t2ye13.aspx) interfaccia. Ad esempio [IList&lt;T&gt; ](https://msdn.microsoft.com/library/5y536ey6.aspx) ma non qualifica [IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx) perché `IEnumerable<T>` non implementa [Aggiungi ](https://msdn.microsoft.com/library/63ywd54z.aspx).

Un docente può indicare un numero qualsiasi di corsi, pertanto `Courses` è definito come una raccolta di `Course` entità.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

Regole business è stato un docente può solo avere al massimo un ufficio, pertanto `OfficeAssignment` è definito come una singola `OfficeAssignment` entità (che può essere `null` se non viene assegnato).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>Creare l'entità OfficeAssignment

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Creare *Models\OfficeAssignment.cs* con il codice seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Compilare il progetto, che consente di salvare le modifiche e di verifica che non sono state apportate qualsiasi copia e Incolla il compilatore può intercettare errori.

### <a name="the-key-attribute"></a>L'attributo chiave

È una relazione uno-a-zero-o-uno tra il `Instructor` e `OfficeAssignment` entità. L'assegnazione dell'ufficio esiste solo in relazione a istruttore viene assegnato a, e pertanto la chiave primaria è anche la chiave esterna per il `Instructor` entità. Ma Entity Framework non riconoscono automaticamente `InstructorID` del database primario chiave di questa entità perché il relativo nome non rispetta il `ID` o *classname* `ID` convenzione di denominazione. Per identificare l'entità come chiave viene usato l'attributo `Key`:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

È inoltre possibile utilizzare il `Key` attributo se l'entità dispone di chiave primaria, ma si desidera assegnare un nome di proprietà qualcosa di diverso da `classnameID` o `ID`. Per impostazione predefinita la chiave EF considera come non generati dal database perché la colonna è la relazione di identificazione.

### <a name="the-foreignkey-attribute"></a>L'attributo ForeignKey

Quando è presente una relazione uno-a-zero-o-uno o una relazione uno a uno tra due entità (come tra tali `OfficeAssignment` e `Instructor`), EF non è possibile scoprire quale estremità della relazione è l'entità e quale fine è dipendente. Le relazioni uno a uno dispongono di una proprietà di navigazione di riferimento in ogni classe a altra classe. Il [ForeignKey attributo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) può essere applicato alla classe dipendenti per stabilire la relazione. Se si omette il [ForeignKey attributo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), viene visualizzato l'errore seguente quando si tenta di creare la migrazione:

*Impossibile determinare l'entità finale principale di un'associazione tra i tipi 'ContosoUniversity.Models.OfficeAssignment' e 'ContosoUniversity.Models.Instructor'. L'entità finale principale di questa associazione deve essere configurato in modo esplicito utilizzando l'API fluent della relazione o le annotazioni dei dati.*

Più avanti nell'esercitazione si noterà come configurare la relazione con l'API fluent.

### <a name="the-instructor-navigation-property"></a>La proprietà di navigazione Instructor

Il `Instructor` entità dispone di un valore nullable `OfficeAssignment` proprietà di navigazione (perché un docente potrebbe non disporre di un'assegnazione di office) e `OfficeAssignment` entità dispone di un non-nullable `Instructor` proprietà di navigazione (perché non è un'assegnazione di office esistere senza un docente - `InstructorID` non ammette valori null). Quando un `Instructor` entità è correlata una `OfficeAssignment` entità, ogni entità disporrà di un riferimento a altro nella relativa proprietà di navigazione.

È possibile inserire un `[Required]` attributo sulla proprietà di navigazione istruttore per specificare che deve essere presente un insegnante correlati, ma non è necessario eseguire questa operazione perché la chiave esterna InstructorID (che è anche la chiave per questa tabella) è di tipo non nullable.

## <a name="modify-the-course-entity"></a>Modificare l'entità Course

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

In *Models\Course.cs*, sostituire il codice aggiunto in precedenza con il codice seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

L'entità corso ha una proprietà di chiave esterna `DepartmentID` che punta al correlata `Department` entità e prevede un `Department` proprietà di navigazione. In Entity Framework non è necessario aggiungere una proprietà di chiave esterna al modello di dati se è disponibile una proprietà di navigazione per un'entità correlata. Entity Framework crea automaticamente le chiavi esterne nel database ovunque siano necessarie. Tuttavia il fatto di avere la chiave esterna nel modello di dati può rendere più semplici ed efficienti gli aggiornamenti. Ad esempio, quando si recupera un'entità course per consentire la modifica di `Department` entità è null se non si caricarlo, pertanto quando si aggiorna l'entità course, è necessario recuperare prima la `Department` entità. Quando la proprietà di chiave esterna `DepartmentID` viene incluso nel modello di dati, non è necessario recuperare il `Department` entità prima di aggiornare.

### <a name="the-databasegenerated-attribute"></a>L'attributo DatabaseGenerated

Il [DatabaseGenerated attributo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) con il [Nessuno](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) parametro il `CourseID` proprietà specifica che i valori di chiave primaria vengono forniti dall'utente anziché generati dal database.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

Per impostazione predefinita, Entity Framework si presuppone che i valori di chiave primaria vengono generati dal database. Questa è la condizione ottimale nella maggior parte degli scenari. Tuttavia, per `Course` entità, verrà usare un numero di corso specificato dall'utente, ad esempio una serie di 1000 per reparto, una serie di 2000 per un altro reparto e così via.

### <a name="foreign-key-and-navigation-properties"></a>Chiave esterna e le proprietà di navigazione

La proprietà di chiave esterna e le proprietà di navigazione nel `Course` entità riflettere le relazioni seguenti:

- Un corso viene assegnato a un solo reparto, pertanto sono presenti una chiave esterna `DepartmentID` e una proprietà di navigazione `Department` per i motivi indicati in precedenza. 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- Un corso può avere un numero qualsiasi di studenti iscritti, pertanto la proprietà di navigazione `Enrollments` è una raccolta: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Un corso può essere impartito da più insegnanti, pertanto la proprietà di navigazione `Instructors` è una raccolta: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="create-the-department-entity"></a>Creare l'entità di reparto

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

Creare *Models\Department.cs* con il codice seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>L'attributo di colonna

In precedenza è stato utilizzato il [colonna attributo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) per modificare il mapping di nomi di colonna. Nel codice per il `Department` entità, il `Column` dell'attributo utilizzato per modificare SQL mapping dei tipi in modo che la colonna verrà definita utilizzando SQL Server [money](https://msdn.microsoft.com/library/ms179882.aspx) tipo nel database:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Mapping della colonna in genere non è necessario, poiché Entity Framework in genere seleziona il tipo di dati di SQL Server appropriato in base al tipo CLR definito per la proprietà. Il tipo CLR `decimal` esegue il mapping a un tipo SQL Server `decimal`. Ma in questo caso, si sa che è possibile che la colonna verrà contenente gli importi in valuta e [money](https://msdn.microsoft.com/library/ms179882.aspx) è più appropriato per tale tipo di dati. Per ulteriori informazioni sui tipi di dati CLR e come corrispondano ai tipi di dati di SQL Server, vedere [SqlClient per tipi Entity Framework](https://msdn.microsoft.com/library/bb896344.aspx).

### <a name="foreign-key-and-navigation-properties"></a>Chiave esterna e le proprietà di navigazione

Le proprietà di chiave esterna e le proprietà di navigazione riflettono le relazioni seguenti:

- Un reparto può avere o meno un amministratore e un amministratore è sempre un insegnante. Pertanto il `InstructorID` proprietà è inclusa come chiave esterna per il `Instructor` entità e un punto interrogativo viene aggiunto dopo il `int` digitare designazione per contrassegnare la proprietà come nullable. La proprietà di navigazione è denominata `Administrator` ma contiene un `Instructor` entità: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Un reparto può avere molti corsi, pertanto non c'è un `Courses` proprietà di navigazione: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > Per convenzione, Entity Framework consente l'eliminazione a catena per le chiavi esterne non nullable e per le relazioni molti-a-molti. Ciò può determinare regole di eliminazione a catena circolari, che generano un'eccezione quando si prova ad aggiungere una migrazione. Ad esempio, se non è stato definito il `Department.InstructorID` proprietà ammette valori null, si otterrebbe il seguente messaggio di eccezione: "la relazione referenziale comporterà un riferimento ciclico non è consentito." Se le regole business necessario `InstructorID` proprietà sia non nullable, è necessario utilizzare la seguente istruzione di Microsoft Office fluent API per disattivare eliminazione a catena della relazione: 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]


## <a name="modify-the-enrollment-entity"></a>Modificare l'entità di registrazione

![Enrollment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

 In *Models\Enrollment.cs*, sostituire il codice aggiunto in precedenza con il codice seguente

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=1,15)]

### <a name="foreign-key-and-navigation-properties"></a>Chiave esterna e le proprietà di navigazione

Le proprietà di chiave esterna e le proprietà di navigazione riflettono le relazioni seguenti:

- Un record di iscrizione è relativo a un singolo corso, pertanto sono presenti una proprietà di chiave esterna `CourseID` e una proprietà di navigazione `Course`: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs)]
- Un record di iscrizione è relativo a un singolo studente, pertanto sono presenti una proprietà di chiave esterna `StudentID` e una proprietà di navigazione `Student`: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]

### <a name="many-to-many-relationships"></a>Relazioni molti-a-molti

È una relazione molti-a-molti tra la `Student` e `Course` entità e `Enrollment` entità funziona come una tabella di join molti-a-molti *con payload* nel database. Ciò significa che il `Enrollment` tabella contiene dati aggiuntivi oltre a chiavi esterne per tabelle unite in join (in questo caso, una chiave primaria e una `Grade` proprietà).

La figura seguente illustra l'aspetto di queste relazioni in un diagramma di entità. (Questo diagramma è stato generato utilizzando il [Power Tools di Entity Framework](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); la creazione del diagramma non fa parte dell'esercitazione, appena viene utilizzato come un'illustrazione.)

![Student-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

Ogni linea della relazione dispone di un 1 al fine di uno e un asterisco (\*) in un'altra, per indicare una relazione uno-a-molti.

Se il `Enrollment` tabella non include informazioni di livello, sufficiente contenere le due chiavi esterne `CourseID` e `StudentID`. In tal caso, corrisponde a una tabella di join molti-a-molti *senza payload* (o un *tabella join pure*) nel database, e non è necessario creare una classe di modello per tale affatto. Il `Instructor` e `Course` le entità dispongono di tale tipo di relazione molti-a-molti e come si può notare, non vi è alcuna classe di entità tra di essi:

![Instructor-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

Una tabella di join è necessario nel database, tuttavia, come illustrato nel diagramma di database seguenti:

![Instructor-Course_many-to-many_relationship_tables](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Entity Framework crea automaticamente il `CourseInstructor` e si leggere e aggiornare indirettamente mediante la lettura e aggiornamento di `Instructor.Courses` e `Course.Instructors` le proprietà di navigazione.

## <a name="entity-diagram-showing-relationships"></a>Diagramma dell'entità che visualizza le relazioni

La figura seguente visualizza il diagramma creato da Entity Framework Power Tools per il modello School completato.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

Oltre alle linee di relazione molti-a-molti (\* a \*) e le linee di relazione uno-a-molti (da 1 a \*), sarà possibile visualizzare la linea della relazione uno-a-zero-o-uno (1 per 0..1) tra il `Instructor` e `OfficeAssignment` le entità e la riga zero-o-relazione uno-a-molti (0..1 a \*) tra le entità Instructor e reparto.

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>Personalizzare il modello di dati mediante l'aggiunta di codice per il contesto del Database

Successivamente, verrà aggiunto nuove entità per il `SchoolContext` classe e personalizzare alcune del mapping tramite [API fluent](https://msdn.microsoft.com/data/jj591617) chiamate. L'API è "Microsoft Office fluent" in quanto viene spesso utilizzato mettendo una serie di chiamate al metodo in una singola istruzione, come nell'esempio seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

In questa esercitazione si utilizzerà l'API fluent solo per mapping del database che si può fare con gli attributi. È tuttavia possibile usare l'API Fluent anche per specificare la maggior parte delle regole di formattazione, convalida e mapping specificabili tramite gli attributi. Alcuni attributi quali `MinimumLength` non possono essere applicati con l'API Fluent. Come accennato in precedenza, `MinimumLength` non modifica lo schema, si applica solo una regola di convalida sul lato client e server

Alcuni sviluppatori preferiscono usare solo l'API Fluent, per dare un aspetto "ordinato" alle classi di entità. È possibile usare un misto di attributi e API Fluent e alcune personalizzazioni sono eseguibili solo mediante l'API Fluent, ma in generale è consigliabile scegliere uno dei due approcci e usarlo in modo il più possibile coerente.

Per aggiungere nuove entità per i dati del modello e di eseguire il mapping del database che non è stata svolta utilizzando gli attributi, sostituire il codice in *DAL\SchoolContext.cs* con il codice seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

La nuova istruzione nel [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) metodo consente di configurare la tabella di join molti-a-molti:

- Per la relazione molti-a-molti tra la `Instructor` e `Course` entità, il codice specifica i nomi di tabella e di colonna per la tabella di join. Codice prima di tutto possibile configurare la relazione molti-a-molti per l'utente senza questo codice, ma se non lo si chiama, si otterrà i nomi predefiniti, ad esempio `InstructorInstructorID` per il `InstructorID` colonna.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

Il codice seguente viene fornito un esempio di come è possibile utilizzare API fluent anziché agli attributi per specificare la relazione tra il `Instructor` e `OfficeAssignment` entità:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

Per informazioni relative alle operazioni vengono eseguite le istruzioni "fluent API" dietro le quinte, vedere il [API Fluent](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) post di blog.

## <a name="seed-the-database-with-test-data"></a>Assegnare valori di inizializzazione al database con dati di test

Sostituire il codice di *Migrations\Configuration.cs* file con il codice seguente per fornire i dati iniziali per le nuove entità creata.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

Come osservato nella prima esercitazione, semplicemente la maggior parte di questo codice aggiorna o crea nuovi oggetti entità e carica i dati di esempio nella proprietà come necessario per i test. Si noti, tuttavia, come la `Course` entità che dispone di una relazione molti-a-molti con la `Instructor` entità, viene gestita:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

Quando si crea un `Course` dell'oggetto, si inizializza il `Instructors` proprietà di navigazione come una raccolta vuota usando il codice `Instructors = new List<Instructor>()`. In questo modo è possibile aggiungere `Instructor` le entità correlate a questo `Course` utilizzando il `Instructors.Add` metodo. Se non è stato creato un elenco vuoto, non sarà in grado di aggiungere queste relazioni, in quanto il `Instructors` proprietà sarà null e non avrebbe un `Add` metodo. È anche possibile aggiungere l'inizializzazione di elenco al costruttore.

## <a name="add-a-migration-and-update-the-database"></a>Aggiungere una migrazione e aggiornare il Database

Da PMC, immettere il `add-migration` comando (non il `update-database` ancora comando):

`add-Migration ComplexDataModel`

Se si prova a eseguire il comando `update-database` in questa fase (evitare di farlo), si ottiene il seguente errore:

*L'istruzione ALTER TABLE è in conflitto con il vincolo FOREIGN KEY "si intende\_dbo. Corso\_dbo. Reparto\_DepartmentID ". Si è verificato il conflitto in una tabella del database "ContosoUniversity", "dbo. Reparto", colonna 'DepartmentID'.*

Talvolta quando si esegue con dati esistenti, è necessario inserire dati stub nel database per soddisfare i vincoli di chiave esterna, e che cosa è necessario effettuare ora. Il codice generato nei componenti di ComplexDataModel `Up` metodo aggiunge un non-nullable `DepartmentID` chiave esterna per il `Course` tabella. Poiché sono già presenti righe di `Course` tabella quando viene eseguito il codice, il `AddColumn` operazione avrà esito negativo perché SQL Server non riconosce il valore da inserire nella colonna che non può essere null. Pertanto è necessario modificare il codice per fornire la nuova colonna un valore predefinito e creare un reparto stipendi denominato "Temp" come il reparto predefinito. Di conseguenza, esistente `Course` righe saranno tutti correlate al reparto "Temp" dopo il `Up` esecuzione del metodo. Può essere correlata ai reparti corretti il `Seed` metodo.

Modificare il &lt; *timestamp&gt;\_ComplexDataModel.cs* file, impostare come commento la riga di codice che aggiunge la colonna DepartmentID è presente nella tabella Course e aggiungere il codice evidenziato di seguito (impostati come commento riga viene inoltre evidenziata):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

Quando il `Seed` esecuzione del metodo, inserire righe nel `Department` tabella e sarà correlata esistente `Course` righe per i nuovi `Department` righe. Se non si aggiunti qualsiasi corsi nell'interfaccia utente, potrebbe quindi non è più necessario il reparto "Temp" o il valore predefinito sul `Course.DepartmentID` colonna. Per consentire la possibilità che un utente potrebbe essere aggiunto corsi mediante l'applicazione, inoltre si desidera aggiornare il `Seed` codice del metodo per garantire che tutti i `Course` righe (non solo quelli inseriti da precedenti esecuzioni del `Seed` metodo) hanno valido `DepartmentID` valori prima di rimuovere il valore predefinito valore dalla colonna ed eliminare il reparto "Temp".

Dopo aver modificato il &lt; *timestamp&gt;\_ComplexDataModel.cs* file, immettere il `update-database` comando PMC per eseguire la migrazione.

[!code-powershell[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.ps1)]

> [!NOTE]
> È possibile ottenere altri errori durante la migrazione di dati e apportare le modifiche dello schema. Se si verificano errori di migrazione che non si riesce a risolvere, è possibile modificare il nome del database nella stringa di connessione o eliminare il database. L'approccio più semplice consiste nel rinominare il database in *Web. config* file. L'esempio seguente mostra il nome modificato in CU\_Test:
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample36.xml?highlight=1)]
> 
> Con un nuovo database, non sono presenti dati per eseguire la migrazione e `update-database` comando è molto più probabile completata senza errori. Per istruzioni su come eliminare il database, vedere [come eliminare un Database da Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).
> 
> Se il problema persiste, in alternativa, che è possibile provare è reinizializzare il database immettendo il comando seguente in PMC:
> 
> `update-database -TargetMigration:0`


Aprire il database in **Esplora Server** come avveniva in precedenza ed espandere il **tabelle** nodo per visualizzare tutte le tabelle che sono state create. (Se sono ancora **Esplora Server** aprire dal momento precedente, fare clic il **aggiornamento** pulsante.)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image16.png)

Non è stato creato una classe di modello per il `CourseInstructor` tabella. Come spiegato in precedenza, si tratta di una tabella di join per la relazione molti-a-molti tra la `Instructor` e `Course` entità.

Fare doppio clic sul `CourseInstructor` tabella e selezionare **Mostra dati tabella** per verificare che disponga di dati in essa contenuti in seguito al `Instructor` entità è stato aggiunto per il `Course.Instructors` proprietà di navigazione.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image17.png)

## <a name="summary"></a>Riepilogo

Ora sono presenti un modello di dati più complesso e il database corrispondente. Nell'esercitazione seguente si verranno fornite altre informazioni sui vari modi per accedere ai dati correlati.

Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione ed è stato possibile apportare miglioramenti. È inoltre possibile richiedere nuovi argomenti in [Mostra Me come con codice](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Collegamenti ad altre risorse di Entity Framework, vedere il [accesso ai dati ASP.NET - risorse](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Successivo](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
