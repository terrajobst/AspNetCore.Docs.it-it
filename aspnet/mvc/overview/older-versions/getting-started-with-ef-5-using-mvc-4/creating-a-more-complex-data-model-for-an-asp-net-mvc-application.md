---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: "Creazione di un modello di dati più complesso per un'applicazione ASP.NET MVC (4 di 10) | Documenti Microsoft"
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: f81f3d80-3674-4d8e-a9b1-87feed1a93c9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: accb5ddab8df67dfa29038541dc0cd72eaac173c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application-4-of-10"></a>Creazione di un modello di dati più complesso per un'applicazione ASP.NET MVC (4 di 10)
====================
Da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 4 utilizzando Code First di Entity Framework 5 e Visual Studio 2012. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni dall'inizio o [scaricare un progetto di avvio per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si esegue un problema, è possibile risolvere, [scaricare il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. In genere, è possibile trovare la soluzione al problema di mediante un confronto tra il codice per il codice completato. Per alcuni errori comuni e come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Nelle esercitazioni precedenti è stato utilizzato un modello di dati semplici che è stato composto da tre entità. In questa esercitazione si aggiungeranno ulteriori entità e relazioni e verrà personalizzato il modello di dati specificando le regole di mapping del database, convalida e formattazione. Si noterà due modi per personalizzare il modello di dati: aggiungendo attributi alle classi di entità e aggiungendo codice alla classe contesto di database.

Al termine dell'operazione, le classi di entità verranno costituiscono il modello di dati completato che è illustrato nella figura seguente:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Personalizzare il modello di dati utilizzando gli attributi

In questa sezione si noterà come personalizzare il modello di dati utilizzando gli attributi che specificano la formattazione, convalida e regole di mapping del database. Quindi in diversi nelle sezioni seguenti si creeranno completo `School` modello di dati aggiungendo gli attributi alle classi è già create e la creazione di nuove classi per i tipi di entità rimanenti nel modello.

### <a name="the-datatype-attribute"></a>L'attributo di tipo di dati

Per le date di registrazione studenti, tutte le pagine web attualmente Visualizza il tempo insieme alla data, anche se tutto è rilevante per questo campo è la data. Utilizzando gli attributi di annotazione dei dati, è possibile apportare una modifica che in ogni visualizzazione che mostra i dati per correggere il formato di visualizzazione del codice. Per vedere un esempio di come eseguire l'operazione, verrà aggiunto un attributo di `EnrollmentDate` proprietà la `Student` classe.

In *Models\Student.cs*, aggiungere un `using` istruzione per il `System.ComponentModel.DataAnnotations` dello spazio dei nomi e aggiungere `DataType` e `DisplayFormat` gli attributi di `EnrollmentDate` proprietà, come illustrato nell'esempio seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,13-14)]

Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributo viene utilizzato per specificare un tipo di dati che è più specifico di tipo intrinseco del database. In questo caso si vuole inserire solo tenere traccia delle date, non la data e ora. Il [enumerazione DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) fornisce per molti tipi di dati, ad esempio *data, ora, numero di telefono, valuta, EmailAddress* e altro ancora. L'attributo `DataType` può anche consentire all'applicazione di fornire automaticamente le funzionalità specifiche del tipo. Ad esempio, un `mailto:` possibile creare un collegamento per [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), e un selettore data può essere fornito per [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) nei browser che supportano [HTML5](http://html5.org/). Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributi genera HTML 5 [dati -](http://ejohn.org/blog/html-5-data-attributes/) (si pronuncia *dash dati*) gli attributi che è in grado di riconoscere i browser HTML 5. Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributi non forniscono alcuna convalida.

`DataType.Date` non specifica il formato della data visualizzata. Per impostazione predefinita, viene visualizzato il campo dei dati in base ai formati predefiniti in base al server [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

L'attributo `DisplayFormat` viene usato per specificare in modo esplicito il formato della data:


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


Il `ApplyFormatInEditMode` impostazione specifica che la formattazione specificata deve essere applicata anche quando il valore viene visualizzato in una casella di testo per la modifica. (Non è consigliabile che per alcuni campi, ad esempio, per i valori di valuta, non è il simbolo di valuta nella casella di testo per la modifica.)

È possibile utilizzare il [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attributo da se stesso, ma in genere è consigliabile utilizzare il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) anche l'attributo. Il `DataType` attributo fornisce il *semantica* dei dati come anziché come eseguirne il rendering in una schermata e fornisce i seguenti vantaggi che non si ottengono con `DisplayFormat`:

- Il browser è possibile abilitare le funzionalità di HTML5 (ad esempio per visualizzare un controllo di calendario il simbolo di valuta delle impostazioni locali appropriata, i collegamenti di posta elettronica, e così via.).
- Per impostazione predefinita, il browser verrà eseguito il rendering di dati utilizzando il formato corretto in base il [internazionali](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- Il [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributo è possibile abilitare MVC scegliere il modello di campo a destra per il rendering dei dati (il [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) se utilizzato da se stessa viene utilizzato il modello di stringa). Per ulteriori informazioni, vedere Brad Wilson [ASP.NET MVC 2 Templates](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Anche se scritte per MVC 2, in questo articolo ancora si applica alla versione corrente di ASP.NET MVC).

Se si utilizza il `DataType` attributo con un campo Data, è necessario specificare il `DisplayFormat` attributo anche per garantire che il campo viene visualizzato correttamente in browser Chrome. Per ulteriori informazioni, vedere [questo thread StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Eseguire nuovamente la pagina di indice per studenti e notare che volte non vengono più visualizzate per le date di registrazione. Lo stesso sarà true per qualsiasi vista che utilizza il `Student` modello.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>L'elemento StringLengthAttribute

È anche possibile specificare le regole di convalida dei dati e i messaggi utilizzando gli attributi. Si supponga che si desidera garantire che gli utenti non immettere più di 50 caratteri per un nome. Per aggiungere questa limitazione, è necessario [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) gli attributi di `LastName` e `FirstMidName` proprietà, come illustrato nell'esempio seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

Il [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attributo non impedisce a un utente di immettere lo spazio vuoto per un nome. È possibile utilizzare il [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) attributo per applicare restrizioni per l'input. Ad esempio il codice seguente richiede il primo carattere da maiuscolo e i caratteri rimanenti alfabetico:

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

Il [MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) attributo fornisce funzionalità simili a quelle di [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attributo ma non è disponibile sul lato client convalida.

Eseguire l'applicazione e fare clic su di **studenti** scheda. Viene visualizzato l'errore seguente:

*Il modello di esecuzione del backup il contesto 'SchoolContext' è stato modificato dopo la creazione del database. Provare a utilizzare migrazioni Code First per aggiornare il database ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).*

Il modello di database è stato modificato in modo che richiede una modifica nello schema del database e di Entity Framework ha rilevato che. Utilizzare le migrazioni per aggiornare lo schema senza perdere i dati aggiunti al database tramite l'interfaccia utente. Se è stato modificato i dati che è stati creati dal `Seed` (metodo), che verranno modificate allo stato originale perché il [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) metodo che si sta utilizzando il `Seed` metodo. ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) è equivalente a un'operazione "upsert" da terminologia dei database.)

In Package Manager Console (PMC), immettere i comandi seguenti:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

Il `add-migration MaxLengthOnNames` comando crea un file denominato  *&lt;timeStamp&gt;\_MaxLengthOnNames.cs*. Questo file contiene codice che aggiornerà il database in base al modello di dati corrente. Il timestamp anteposto al nome del file migrazioni utilizzato da Entity Framework per ordinare le migrazioni. Dopo aver creato le migrazioni più, se si elimina il database o se si distribuisce il progetto tramite le migrazioni, tutte le migrazioni sono applicate nell'ordine in cui sono stati creati.

Eseguire il **crea** pagina e immettere il nome di più di 50 caratteri. Non appena si superano i 50 caratteri, la convalida lato client viene immediatamente un messaggio di errore.

![Errore val di lato client](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>L'attributo di colonna

Inoltre, è possibile utilizzare gli attributi per controllare il mapping tra le classi e le proprietà per il database. Si supponga che sia stato usato il nome `FirstMidName` per il nome del primo campo perché il campo può contenere anche un cognome. Ma si desidera che la colonna di database sia denominato `FirstName`, perché gli utenti che scrittura di query ad hoc sul database sono abituati a tale nome. Per rendere questo mapping, è possibile utilizzare il `Column` attributo.

Il `Column` attributo specifica che quando viene creato il database, la colonna del `Student` tabella in cui viene eseguito il mapping per il `FirstMidName` proprietà verrà denominata `FirstName`. In altre parole, quando il codice fa riferimento a `Student.FirstMidName`, i dati provengono dal o aggiornati nel `FirstName` colonna del `Student` tabella. Se non si specificano nomi di colonna, essi sono assegnato lo stesso nome come nome della proprietà.

Aggiungere un tramite l'istruzione per [System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) e l'attributo del nome di colonna per il `FirstMidName` proprietà, come illustrato nel codice evidenziato seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

L'aggiunta del [colonna attributo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) consente di modificare il modello di backup SchoolContext, in modo che non corrisponda al database. Immettere i comandi seguenti in PMC per creare un'altra migrazione:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

In **Esplora Server** (**Esplora Database** se si utilizza Express per Web), fare doppio clic su di *Student* tabella.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

L'immagine seguente mostra il nome della colonna originale che aveva prima dell'applicazione le prime due migrazioni. Oltre al nome di colonna, la modifica da `FirstMidName` a `FirstName`, le due colonne nome sono stati modificati da `MAX` lunghezza a 50 caratteri.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

È possibile inoltre modificare database mapping utilizzando il [API Fluent](https://msdn.microsoft.com/data/jj591617), come verrà illustrato più avanti in questa esercitazione.

> [!NOTE]
> Se si tenta di compilare prima del completamento della creazione di tutte queste classi di entità, si potrebbero verificare errori di compilazione.


## <a name="create-the-instructor-entity"></a>Creare l'entità Instructor

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Creare *Models\Instructor.cs*, sostituendo il codice del modello con il codice seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Si noti che molte proprietà sono la stessa nella `Student` e `Instructor` entità. Nel [implementazione ereditarietà](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) esercitazione più avanti in questa serie, sarà il refactoring tramite ereditarietà per eliminare questa ridondanza.

### <a name="the-required-and-display-attributes"></a>Obbligatorio e visualizzare gli attributi

Gli attributi di `LastName` proprietà specificare che si tratta di un campo obbligatorio, che la didascalia della casella di testo deve essere "Last Name" (anziché il nome della proprietà, che dovrebbe essere "Cognome" senza spazio), e che il valore non può contenere più di 50 caratteri.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

Il [StringLength attributo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) imposta la lunghezza massima del database e fornisce lato client e lato server della convalida di ASP.NET MVC. È inoltre possibile specificare la lunghezza minima della stringa in questo attributo, ma il valore minimo non ha alcun impatto sullo schema di database. Il [attributo obbligatorio](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) non è necessario per i tipi di valore, ad esempio DateTime, int, double e float. Tipi di valore non sono assegnati un valore null, in modo che siano necessari implicitamente. È possibile rimuovere il [attributo obbligatorio](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) e sostituirlo con un parametro di lunghezza minima per il `StringLength` attributo:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs?highlight=2)]

È possibile inserire più attributi su una riga, pertanto potrebbe anche scrivere la classe istruttore come indicato di seguito:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-fullname-calculated-property"></a>La proprietà calcolata FullName

`FullName`è una proprietà calcolata che restituisce un valore che viene creato concatenando due altre proprietà. Pertanto ha solo un `get` della funzione di accesso e nessun `FullName` colonna verrà generata nel database.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>Corsi di e le proprietà di navigazione OfficeAssignment

Il `Courses` e `OfficeAssignment` sono proprietà di navigazione. Come spiegato in precedenza, vengono in genere definiti come [virtuale](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) in modo che sia possibile usufruire di una funzionalità di Entity Framework chiamata [caricamento lazy](https://msdn.microsoft.com/magazine/hh205756.aspx). Inoltre, se una proprietà di navigazione può contenere più entità, il tipo deve implementare il [ICollection&lt;T&gt; ](https://msdn.microsoft.com/library/92t2ye13.aspx) interfaccia. (Ad esempio [IList&lt;T&gt; ](https://msdn.microsoft.com/library/5y536ey6.aspx) ma non qualifica [IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx) perché `IEnumerable<T>` non implementa [Aggiungi ](https://msdn.microsoft.com/library/63ywd54z.aspx).

Un docente può indicare un numero qualsiasi di corsi, pertanto `Courses` è definito come una raccolta di `Course` entità. Regole business è stato un docente può solo avere al massimo un ufficio, pertanto `OfficeAssignment` è definito come una singola `OfficeAssignment` entità (che può essere `null` se non viene assegnato).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>Creare l'entità OfficeAssignment

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Creare *Models\OfficeAssignment.cs* con il codice seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Compilare il progetto, che consente di salvare le modifiche e di verifica che non sono state apportate qualsiasi copia e Incolla il compilatore può intercettare errori.

### <a name="the-key-attribute"></a>L'attributo chiave

È una relazione uno-a-zero-o-uno tra il `Instructor` e `OfficeAssignment` entità. L'assegnazione dell'ufficio esiste solo in relazione a istruttore viene assegnato a, e pertanto la chiave primaria è anche la chiave esterna per il `Instructor` entità. Ma Entity Framework non riconoscono automaticamente `InstructorID` del database primario chiave di questa entità perché il relativo nome non rispetta il `ID` o *classname* `ID` convenzione di denominazione. Pertanto, il `Key` attributo viene utilizzato per identificare come la chiave:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

È inoltre possibile utilizzare il `Key` attributo se l'entità dispone di chiave primaria, ma si desidera assegnare un nome di proprietà qualcosa di diverso da `classnameID` o `ID`. Per impostazione predefinita la chiave EF considera come non generati dal database perché la colonna è la relazione di identificazione.

### <a name="the-foreignkey-attribute"></a>L'attributo ForeignKey

Quando è presente una relazione uno-a-zero-o-uno o una relazione uno a uno tra due entità (come tra tali `OfficeAssignment` e `Instructor`), EF non è possibile scoprire quale estremità della relazione è l'entità e quale fine è dipendente. Le relazioni uno a uno dispongono di una proprietà di navigazione di riferimento in ogni classe a altra classe. Il [ForeignKey attributo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) può essere applicato alla classe dipendenti per stabilire la relazione. Se si omette il [ForeignKey attributo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), viene visualizzato l'errore seguente quando si tenta di creare la migrazione:

Impossibile determinare l'entità finale principale di un'associazione tra i tipi 'ContosoUniversity.Models.OfficeAssignment' e 'ContosoUniversity.Models.Instructor'. L'entità finale principale di questa associazione deve essere configurato in modo esplicito utilizzando l'API fluent della relazione o le annotazioni dei dati.

Più avanti nell'esercitazione vi mostreremo come configurare la relazione con l'API fluent.

### <a name="the-instructor-navigation-property"></a>La proprietà di navigazione Instructor

Il `Instructor` entità dispone di un valore nullable `OfficeAssignment` proprietà di navigazione (perché un docente potrebbe non disporre di un'assegnazione di office) e `OfficeAssignment` entità dispone di un non-nullable `Instructor` proprietà di navigazione (perché non è un'assegnazione di office esistere senza un docente - `InstructorID` non ammette valori null). Quando un `Instructor` entità è correlata una `OfficeAssignment` entità, ogni entità disporrà di un riferimento a altro nella relativa proprietà di navigazione.

È possibile inserire un `[Required]` attributo sulla proprietà di navigazione istruttore per specificare che deve essere presente un insegnante correlati, ma non è necessario eseguire questa operazione perché la chiave esterna InstructorID (che è anche la chiave per questa tabella) è di tipo non nullable.

## <a name="modify-the-course-entity"></a>Modificare l'entità Course

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

In *Models\Course.cs*, sostituire il codice aggiunto in precedenza con il codice seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

L'entità corso ha una proprietà di chiave esterna `DepartmentID` che punta al correlata `Department` entità e prevede un `Department` proprietà di navigazione. Entity Framework non è necessario aggiungere una proprietà di chiave esterna per il modello di dati quando si dispone di una proprietà di navigazione per un'entità correlata. Entity Framework crea automaticamente le chiavi esterne nel database ovunque siano necessarie. Ma con la chiave esterna nel modello di dati è possibile eseguire gli aggiornamenti più semplice ed efficiente. Ad esempio, quando si recupera un'entità course per consentire la modifica di `Department` entità è null se non si caricarlo, pertanto quando si aggiorna l'entità course, è necessario recuperare prima la `Department` entità. Quando la proprietà di chiave esterna `DepartmentID` viene incluso nel modello di dati, non è necessario recuperare il `Department` entità prima di aggiornare.

### <a name="the-databasegenerated-attribute"></a>L'attributo DatabaseGenerated

Il [DatabaseGenerated attributo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) con il [Nessuno](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) parametro il `CourseID` proprietà specifica che i valori di chiave primaria vengono forniti dall'utente anziché generati dal database.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

Per impostazione predefinita, Entity Framework si presuppone che i valori di chiave primaria vengono generati dal database. Che è quello desiderato nella maggior parte degli scenari. Tuttavia, per `Course` entità, verrà usare un numero di corso specificato dall'utente, ad esempio una serie di 1000 per reparto, una serie di 2000 per un altro reparto e così via.

### <a name="foreign-key-and-navigation-properties"></a>Chiave esterna e le proprietà di navigazione

La proprietà di chiave esterna e le proprietà di navigazione nel `Course` entità riflettere le relazioni seguenti:

- Un corso viene assegnato a un reparto, pertanto non c'è un `DepartmentID` chiave esterna e un `Department` proprietà di navigazione per i motivi indicati in precedenza. 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- Un corso può avere qualsiasi numero di studenti, pertanto la `Enrollments` proprietà di navigazione è una raccolta: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Un corso può dedicare da istruttori più, pertanto la `Instructors` proprietà di navigazione è una raccolta: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="creating-the-department-entity"></a>Creazione dell'entità di reparto

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

Creare *Models\Department.cs* con il codice seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>L'attributo di colonna

In precedenza è stato utilizzato il [colonna attributo](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) per modificare il mapping di nomi di colonna. Nel codice per il `Department` entità, il `Column` dell'attributo utilizzato per modificare SQL mapping dei tipi in modo che la colonna verrà definita utilizzando SQL Server [money](https://msdn.microsoft.com/library/ms179882.aspx) tipo nel database:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Mapping della colonna in genere non è necessario, poiché Entity Framework in genere seleziona il tipo di dati di SQL Server appropriato in base al tipo CLR definito per la proprietà. CLR `decimal` mapping a un Server SQL del tipo `decimal` tipo. Ma in questo caso, si sa che è possibile che la colonna verrà contenente gli importi in valuta e [money](https://msdn.microsoft.com/library/ms179882.aspx) è più appropriato per tale tipo di dati.

### <a name="foreign-key-and-navigation-properties"></a>Chiave esterna e le proprietà di navigazione

Le proprietà di navigazione e di chiave esterne rifletteranno le relazioni seguenti:

- Un reparto può non avere o un amministratore e un amministratore è sempre un docente. Pertanto il `InstructorID` proprietà è inclusa come chiave esterna per il `Instructor` entità e un punto interrogativo viene aggiunto dopo il `int` digitare designazione per contrassegnare la proprietà come nullable. La proprietà di navigazione è denominata `Administrator` ma contiene un `Instructor` entità: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Un reparto può avere molti corsi, pertanto non c'è un `Courses` proprietà di navigazione: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

 > [!NOTE]
 > Per convenzione, Entity Framework consente l'eliminazione a catena per chiavi esterne non nullable e per le relazioni molti-a-molti. Ciò può comportare le regole delete cascade circolare, verrà generata un'eccezione quando viene eseguito il codice di inizializzatore. Ad esempio, se non è stato definito il `Department.InstructorID` proprietà ammette valori null, si otterrebbe il seguente messaggio di eccezione quando viene eseguito l'inizializzatore: "la relazione referenziale comporterà un riferimento ciclico non è consentito." Se le regole business necessario `InstructorID` proprietà come non nullable, è necessario utilizzare l'API fluent seguente per disabilitare eliminazione a catena della relazione: 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]


## <a name="modifying-the-student-entity"></a>Modifica dell'entità di Student

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

In *Models\Student.cs*, sostituire il codice aggiunto in precedenza con il codice seguente. Le modifiche sono evidenziate.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=12,15,24-27)]

## <a name="the-enrollment-entity"></a>L'entità di registrazione

 In *Models\Enrollment.cs*, sostituire il codice aggiunto in precedenza con il codice seguente

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs?highlight=16)]

### <a name="foreign-key-and-navigation-properties"></a>Chiave esterna e le proprietà di navigazione

La proprietà di chiave esterna e le proprietà di navigazione rifletteranno le relazioni seguenti:

- È un record di registrazione per un singolo corso, pertanto non c'è un `CourseID` proprietà di chiave esterna e un `Course` proprietà di navigazione: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]
- È un record di registrazione per un singolo studente, pertanto non c'è un `StudentID` proprietà di chiave esterna e un `Student` proprietà di navigazione: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

### <a name="many-to-many-relationships"></a>Relazioni molti-a-molti

È una relazione molti-a-molti tra la `Student` e `Course` entità e `Enrollment` entità funziona come una tabella di join molti-a-molti *con payload* nel database. Ciò significa che il `Enrollment` tabella contiene dati aggiuntivi oltre a chiavi esterne per tabelle unite in join (in questo caso, una chiave primaria e una `Grade` proprietà).

Nella figura seguente viene illustrato l'aspetto queste relazioni in un diagramma di entità. (Questo diagramma è stato generato utilizzando il [Power Tools di Entity Framework](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); la creazione del diagramma non fa parte dell'esercitazione, appena viene utilizzato come un'illustrazione.)

![Student-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

Ogni linea della relazione dispone di un 1 al fine di uno e un asterisco (\*) in un'altra, per indicare una relazione uno-a-molti.

Se il `Enrollment` tabella non include informazioni di livello, sufficiente contenere le due chiavi esterne `CourseID` e `StudentID`. In tal caso, corrisponde a una tabella di join molti-a-molti *senza payload* (o un *tabella join pure*) nel database, e non è necessario creare una classe di modello per tale affatto. Il `Instructor` e `Course` le entità dispongono di tale tipo di relazione molti-a-molti e come si può notare, non vi è alcuna classe di entità tra di essi:

![Instructor-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

Una tabella di join è necessario nel database, tuttavia, come illustrato nel diagramma di database seguenti:

![Instructor-Course_many-to-many_relationship_tables](https://asp.net/media/2577802/Windows-Live-Writer_Creating-a.NET-MVC-Application-4-of-10h1_B662_Instructor-Course_many-to-many_relationship_tables_03e042cf-db89-4b4c-985a-e458351ada76.png)

Entity Framework crea automaticamente il `CourseInstructor` e si leggere e aggiornare indirettamente mediante la lettura e aggiornamento di `Instructor.Courses` e `Course.Instructors` le proprietà di navigazione.

## <a name="entity-diagram-showing-relationships"></a>Entità diagramma che illustra le relazioni

Nella figura seguente viene illustrato il diagramma che strumenti di Entity Framework Power creare per il modello School completato.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

Oltre alle linee di relazione molti-a-molti (\* a \*) e le linee di relazione uno-a-molti (da 1 a \*), sarà possibile visualizzare la linea della relazione uno-a-zero-o-uno (1 per 0..1) tra il `Instructor` e `OfficeAssignment` le entità e la riga zero-o-relazione uno-a-molti (0..1 a \*) tra le entità Instructor e reparto.

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>Personalizzare il modello di dati mediante l'aggiunta di codice per il contesto del Database

Successivamente, verrà aggiunto nuove entità per il `SchoolContext` classe e personalizzare alcune del mapping tramite [API fluent](https://msdn.microsoft.com/data/jj591617) chiamate. (L'API è "Microsoft Office fluent" in quanto viene spesso utilizzato mettendo una serie di chiamate al metodo in una singola istruzione).

In questa esercitazione si utilizzerà l'API fluent solo per mapping del database che si può fare con gli attributi. Tuttavia, è possibile utilizzare anche l'API fluent per specificare la maggior parte della formattazione, convalida e regole di mapping che è possibile eseguire utilizzando gli attributi. Alcuni attributi, ad esempio `MinimumLength` non può essere applicata con l'API fluent. Come accennato in precedenza, `MinimumLength` non modifica lo schema, si applica solo una regola di convalida sul lato client e server

Alcuni sviluppatori preferiscono utilizzare l'API fluent esclusivamente in modo che mantengano le classi di entità "pulito". È possibile combinare gli attributi e l'API fluent se si desidera, e sono disponibili alcune personalizzazioni che è possono solo tramite l'API fluent, ma in genere di è consigliabile scegliere uno di questi due approcci e usare in che modo coerente il più possibile.

Per aggiungere nuove entità per i dati del modello e di eseguire il mapping del database che non è stata svolta utilizzando gli attributi, sostituire il codice in *DAL\SchoolContext.cs* con il codice seguente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

La nuova istruzione nel [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) metodo consente di configurare la tabella di join molti-a-molti:

- Per la relazione molti-a-molti tra la `Instructor` e `Course` entità, il codice specifica i nomi di tabella e di colonna per la tabella di join. Codice prima di tutto possibile configurare la relazione molti-a-molti per l'utente senza questo codice, ma se non lo si chiama, si otterrà i nomi predefiniti, ad esempio `InstructorInstructorID` per il `InstructorID` colonna.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

Il codice seguente viene fornito un esempio di come è possibile utilizzare API fluent anziché agli attributi per specificare la relazione tra il `Instructor` e `OfficeAssignment` entità:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

Per informazioni relative alle operazioni vengono eseguite le istruzioni "fluent API" dietro le quinte, vedere il [API Fluent](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) post di blog.

## <a name="seed-the-database-with-test-data"></a>Valore di inizializzazione del Database con dati di Test

Sostituire il codice di *Migrations\Configuration.cs* file con il codice seguente per fornire i dati iniziali per le nuove entità creata.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

Come osservato nella prima esercitazione, semplicemente la maggior parte di questo codice aggiorna o crea nuovi oggetti entità e carica i dati di esempio nella proprietà come necessario per i test. Si noti, tuttavia, come la `Course` entità che dispone di una relazione molti-a-molti con la `Instructor` entità, viene gestita:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

Quando si crea un `Course` dell'oggetto, si inizializza il `Instructors` proprietà di navigazione come una raccolta vuota usando il codice `Instructors = new List<Instructor>()`. In questo modo è possibile aggiungere `Instructor` le entità correlate a questo `Course` utilizzando il `Instructors.Add` metodo. Se non è stato creato un elenco vuoto, non sarà in grado di aggiungere queste relazioni, in quanto il `Instructors` proprietà sarà null e non avrebbe un `Add` metodo. È anche possibile aggiungere l'inizializzazione di elenco al costruttore.

## <a name="add-a-migration-and-update-the-database"></a>Aggiungere una migrazione e aggiornare il Database

Da PMC, immettere il `add-migration` comando:

`PM> add-Migration Chap4`

Se si tenta di aggiornare il database a questo punto, si otterrà l'errore seguente:

*L'istruzione ALTER TABLE è in conflitto con il vincolo FOREIGN KEY "FK\_dbo. Corso\_dbo. Reparto\_DepartmentID ". Si è verificato il conflitto in una tabella del database "ContosoUniversity", "dbo. Reparto", colonna 'DepartmentID'.*

Modificare il &lt; *timestamp&gt;\_Chap4.cs* file e apportare le modifiche di codice seguente (è possibile aggiungere un'istruzione SQL e modificare un `AddColumn` istruzione):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

(Assicurarsi di impostare come commento o di eliminare quello esistente `AddColumn` riga quando si aggiunge uno nuovo, o si otterrà un errore quando si immette il `update-database` comando.)

A volte quando si esegue con dati esistenti, è necessario inserire dati stub nel database per soddisfare i vincoli di chiave esterna, e che cosa si sta eseguendo ora. Il codice generato viene aggiunto un non-nullable `DepartmentID` chiave esterna per il `Course` tabella. Se sono già presenti righe di `Course` tabella quando viene eseguito il codice, il `AddColumn` operazione avrà esito negativo perché SQL Server non riconosce il valore da inserire nella colonna che non può essere null. Di conseguenza è stato modificato il codice per fornire la nuova colonna un valore predefinito e aver creato un reparto stipendi denominato "Temp" come il reparto predefinito. Di conseguenza, se sono presenti `Course` righe quando si esegue questo codice, verranno tutti correlati al reparto "Temp".

Quando il `Seed` esecuzione del metodo, inserire righe nel `Department` tabella e sarà correlata esistente `Course` righe per i nuovi `Department` righe. Se non si aggiunti qualsiasi corsi nell'interfaccia utente, potrebbe quindi non è più necessario il reparto "Temp" o il valore predefinito sul `Course.DepartmentID` colonna. Per consentire la possibilità che un utente potrebbe essere aggiunto corsi mediante l'applicazione, inoltre si desidera aggiornare il `Seed` codice del metodo per garantire che tutti i `Course` righe (non solo quelli inseriti da precedenti esecuzioni del `Seed` metodo) hanno valido `DepartmentID` valori prima di rimuovere il valore predefinito valore dalla colonna ed eliminare il reparto "Temp".

Dopo aver modificato il &lt; *timestamp&gt;\_Chap4.cs* file, immettere il `update-database` comando PMC per eseguire la migrazione.

> [!NOTE]
> È possibile ottenere altri errori durante la migrazione di dati e apportare le modifiche dello schema. Se si verificano errori di migrazione non è possibile risolvere, è possibile cambiare la stringa di connessione nel *Web. config* file o eliminare il database. L'approccio più semplice consiste nel rinominare il database in *Web. config* file. Ad esempio, modificare il nome del database in CU\_testare, come illustrato nell'esempio seguente:
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.xml?highlight=1-2)]
> 
>  Con un nuovo database, non sono presenti dati per eseguire la migrazione e `update-database` comando è molto più probabile completata senza errori. Per istruzioni su come eliminare il database, vedere [come eliminare un Database da Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).


Aprire il database in **Esplora Server** come avveniva in precedenza ed espandere il **tabelle** nodo per visualizzare tutte le tabelle che sono state create. (Se sono ancora **Esplora Server** aprire dal momento precedente, fare clic il **aggiornamento** pulsante.)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Non è stato creato una classe di modello per il `CourseInstructor` tabella. Come spiegato in precedenza, si tratta di una tabella di join per la relazione molti-a-molti tra la `Instructor` e `Course` entità.

Fare doppio clic sul `CourseInstructor` tabella e selezionare **Mostra dati tabella** per verificare che disponga di dati in essa contenuti in seguito al `Instructor` entità è stato aggiunto per il `Course.Instructors` proprietà di navigazione.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="summary"></a>Riepilogo

È ora disponibile un modello di dati più complesso e il database corrispondente. Nell'esercitazione seguente si verranno fornite altre informazioni sui vari modi per accedere ai dati correlati.

Collegamenti ad altre risorse di Entity Framework, vedere il [mappa del contenuto ASP.NET dati accesso](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Precedente](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Successivo](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
