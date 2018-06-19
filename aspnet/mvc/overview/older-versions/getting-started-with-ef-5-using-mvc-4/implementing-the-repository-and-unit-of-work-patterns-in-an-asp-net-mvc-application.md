---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: Implementare il Repository e l'unità di lavoro modelli in un'applicazione ASP.NET MVC (9 di 10) | Documenti Microsoft
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1f870b61658686769304a7809bde62e66da3bd0c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875877"
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>Implementare il Repository e l'unità di lavoro modelli in un'applicazione ASP.NET MVC (9 di 10)
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 4 utilizzando Code First di Entity Framework 5 e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni dall'inizio o [scaricare un progetto di avvio per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si esegue un problema, è possibile risolvere, [scaricare il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. In genere, è possibile trovare la soluzione al problema di mediante un confronto tra il codice per il codice completato. Per alcuni errori comuni e su come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Nell'esercitazione precedente ereditarietà è utilizzato per ridurre il codice ridondante nel `Student` e `Instructor` le classi di entità. In questa esercitazione si noterà alcuni modi per usare il repository e l'unità di lavoro modelli per le operazioni CRUD. Come l'esercitazione precedente, in questo si modificherà il funzionamento del codice con pagine è già creati anziché crearne nuove pagine.

## <a name="the-repository-and-unit-of-work-patterns"></a>Il Repository e l'unità di lavoro modelli

Il repository e l'unità di lavoro modelli servono per creare un livello di astrazione tra il livello di accesso ai dati e il livello di logica di business di un'applicazione. L'implementazione di questi modelli può essere utile per isolare l'applicazione dalle modifiche nell'archivio dati e può semplificare il testing unità automatizzato o lo sviluppo basato su test (TDD).

In questa esercitazione viene implementato una classe di repository per ogni tipo di entità. Per il `Student` tipo di entità, si creerà un'interfaccia del repository e una classe di repository. Quando si crea un'istanza di repository nel controller, si userà l'interfaccia in modo che il controller accetterà un riferimento a qualsiasi oggetto che implementa l'interfaccia del repository. Quando il controller viene eseguito in un server web, riceve un repository che funziona con Entity Framework. Quando il controller viene eseguito in una classe di unit test, riceve un repository che funziona con i dati archiviati in modo che è possibile modificare facilmente di test, ad esempio una raccolta in memoria.

Più avanti nell'esercitazione si utilizzerà più repository e una classe di unità di lavoro per il `Course` e `Department` tipi di entità nel `Course` controller. La classe di unità di lavoro coordina il lavoro di più repository creando una classe di contesto di singolo database condivisa da tutti gli elementi. Se si desidera essere in grado di eseguire unit test automatici, si potrebbe creare e utilizzare le interfacce per queste classi nello stesso modo per il `Student` repository. Tuttavia, per mantenere semplice l'esercitazione, verranno creare e usare queste classi senza interfacce.

Nella figura seguente viene illustrato un modo per realizzare le relazioni tra il controller e le classi di contesto rispetto a non utilizzare affatto il repository o un'unità di lavoro modello.

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

Unit test non vengono creati in questa serie di esercitazioni. Per un'introduzione ai TDD con un'applicazione MVC che utilizza il modello di repository, vedere [procedura dettagliata: utilizzo TDD con ASP.NET MVC](https://msdn.microsoft.com/library/ff847525.aspx). Per ulteriori informazioni sul modello di repository, vedere le risorse seguenti:

- [Il modello di Repository](https://msdn.microsoft.com/library/ff649690.aspx) su MSDN.
- [Utilizzo di modelli di Repository e unità di lavoro con Entity Framework 4.0](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) nel blog del team di Entity Framework.
- [Agile Entity Framework 4 Repository](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) serie di post sul blog di Julie Lerman.
- [La creazione di Account in un'applicazione HTML5/jQuery immediatamente](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) nel blog di Dan Wahlin.

> [!NOTE]
> Esistono diversi modi per implementare il repository e l'unità di lavoro modelli. È possibile utilizzare le classi del repository con o senza una classe di unità di lavoro. È possibile implementare un singolo repository per tutti i tipi di entità, o uno per ogni tipo. Se si implementa uno per ogni tipo, è possibile utilizzare classi separate, una classe base generica e le classi derivate, o una classe base e le classi derivate. È possibile includere la logica di business nel repository o applicare restrizioni a logica di accesso ai dati. È anche possibile compilare un livello di astrazione nella classe di contesto di database utilizzando [IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx) sono interfacce anziché [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) tipi per i set di entità. L'approccio all'implementazione di un livello di astrazione illustrato in questa esercitazione è una delle opzioni da prendere in considerazione, non una raccomandazione per tutti gli ambienti e scenari.


## <a name="creating-the-student-repository-class"></a>Creazione della classe di Repository per studenti

Nel *DAL* cartella, creare un file di classe denominato *IStudentRepository.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

Questo codice dichiara un tipico set di metodi CRUD, inclusi i due metodi di lettura, ovvero uno che restituisce tutti `Student` entità e una che consente di individuare un singolo `Student` entità in base all'ID.

Nel *DAL* cartella, creare un file di classe denominato *StudentRepository.cs* file. Sostituire il codice esistente con il codice seguente, che implementa il `IStudentRepository` interfaccia:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

Il contesto di database viene definito in una variabile di classe e il costruttore richiede l'oggetto chiamante di passare un'istanza del contesto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

È possibile creare un'istanza di un nuovo contesto nel repository, ma quindi se si utilizza più repository in un controller, ogni sarebbe necessario un contesto separato. In un secondo momento si userà più repository di `Course` controller, è possibile notare come un'unità di lavoro classe può garantire che tutti i repository utilizzano lo stesso contesto.

Implementa il repository [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) ed elimina il contesto del database è stato illustrato in precedenza nel controller e i metodi CRUD effettuano chiamate nel contesto del database nell'analogo a quello illustrato in precedenza.

## <a name="change-the-student-controller-to-use-the-repository"></a>Modificare il Controller di studenti per usare il Repository

In *StudentController.cs*, sostituire il codice attualmente nella classe con il codice seguente. Le modifiche sono evidenziate.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

Il controller ora dichiara una variabile di classe per un oggetto che implementa il `IStudentRepository` interfaccia anziché la classe di contesto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

Il costruttore (senza parametri) predefinito crea una nuova istanza di contesto e un costruttore facoltativo consente al chiamante di passare un contesto di istanza.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(Se si utilizza *inserimento di dipendenze*, o SEN, non sarà più necessario il costruttore predefinito perché il software DI assicurarsi che l'oggetto repository corretto sarebbe garantito.)

I metodi CRUD, il repository è ora denominato anziché il contesto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

E `Dispose` metodo elimina ora il repository anziché il contesto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

Esecuzione del sito e fare clic su di **studenti** scheda.

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

La pagina analizza e funziona allo stesso modo come avveniva prima è stato modificato il codice per utilizzare il repository e anche le altre pagine studente funzionano allo stesso modo. Tuttavia, è un'importante differenza nella modalità di `Index` metodo del controller non di filtro e ordinamento. La versione originale di questo metodo è contenuto il codice seguente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

L'aggiornamento `Index` metodo contiene il codice seguente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

È stato modificato solo il codice evidenziato.

Nella versione originale del codice, `students` è tipizzato come un `IQueryable` oggetto. La query non viene inviata al database finché non viene convertito in una raccolta con un metodo, ad esempio `ToList`, che non viene eseguita fino a quando la visualizzazione dell'indice accede al modello di studenti. Il `Where` metodo nel codice originale precedente diventa un `WHERE` clausola nella query SQL che viene inviata al database. A sua volta, ciò significa che solo le entità selezionate vengono restituite dal database. Tuttavia, come risultato della modifica `context.Students` a `studentRepository.GetStudents()`, il `students` variabile dopo questa istruzione è un `IEnumerable` raccolta che include tutti gli studenti nel database. Il risultato finale dell'applicazione di `Where` metodo è lo stesso, ma ora il lavoro viene svolto in memoria nel server web e non dal database. Per le query che restituiscono grandi volumi di dati, questo può risultare inefficiente.

> [!TIP]
> 
> **Visual Studio IQueryable. IEnumerable**
> 
> Dopo aver implementato il repository come illustrato di seguito, anche se si immettono il **ricerca** casella query inviata a SQL Server restituisce tutte le righe di Student poiché non include i criteri di ricerca:
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> Questa query restituisce tutti i dati dello studente in quanto il repository ha eseguito la query senza conoscere i criteri di ricerca. Il processo di ordinamento, l'applicazione di criteri di ricerca e selezione di un subset dei dati per il paging (che mostra solo 3 righe in questo caso) viene eseguito in memoria in seguito quando la `ToPagedList` metodo viene chiamato sul `IEnumerable` insieme.
> 
> Nella versione precedente del codice (prima dell'implementazione di repository), la query non verrà inviata al database fino a dopo aver applicato i criteri di ricerca, quando `ToPagedList` viene chiamato sul `IQueryable` oggetto.
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> Quando ToPagedList viene chiamato su un `IQueryable` dell'oggetto, la query inviata a SQL Server specifica la stringa di ricerca, di conseguenza vengono restituite solo le righe che soddisfano i criteri di ricerca e venga applicato alcun filtro non deve essere eseguita in memoria.
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> (L'esercitazione seguente viene illustrato come esaminare le query inviate a SQL Server).


Nella sezione seguente viene illustrato come implementare i metodi che consentono di specificare che il lavoro deve essere eseguito dal database repository.

È stato creato un livello di astrazione tra il controller e il contesto di database di Entity Framework. Se si prevede di eseguire unit test con questa applicazione automatici, è possibile creare una classe di repository alternativi in un progetto di unit test che implementa `IStudentRepository` *.* Invece di chiamare il contesto per leggere e scrivere dati, questa classe repository fittizio potrebbe modificare le raccolte in memoria per le funzioni di controller di test.

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>Implementare un Repository generico e una classe di unità di lavoro

Creazione di una classe di repository per ogni tipo di entità potrebbe comportare una notevole quantità di codice ridondante e potrebbe verificarsi aggiornamenti parziali. Si supponga, ad esempio aggiornare due tipi diversi di entità come parte della stessa transazione. Se ogni utilizza un'istanza del contesto di database separati, uno potrebbe avere esito positivo e l'altro potrebbe non riuscire. Un modo per ridurre al minimo il codice ridondante è usare un repository generico e un modo per garantire che tutti i repository utilizzano lo stesso contesto di database (e pertanto tutti gli aggiornamenti di coordinate) consiste nell'utilizzare una classe di unità di lavoro.

In questa sezione dell'esercitazione, si creerà un `GenericRepository` classe e un `UnitOfWork` classe e utilizzarle nel `Course` controller per accedere a entrambi il `Department` e `Course` set di entità. Come spiegato in precedenza, per semplificare questa parte dell'esercitazione, che non si interfacce per queste classi. Ma se prevede di usarli per facilitare TDD, vengono in genere implementati in con interfacce allo stesso modo è stato eseguito il `Student` repository.

### <a name="create-a-generic-repository"></a>Creare un Repository generico

Nel *DAL* cartella, creare *GenericRepository.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

Le variabili di classe vengono dichiarate per il contesto del database e per il set di entità che viene creata un'istanza nel repository per:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

Il costruttore accetta un'istanza del contesto di database e inizializza la variabile di set di entità:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

Il `Get` metodo espressioni lambda sono usate per consentire al codice chiamante specificare una condizione di filtro e una colonna per ordinare i risultati in base e un parametro di stringa consente al chiamante di specificare un elenco delimitato da virgole di proprietà di navigazione per il caricamento eager:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

Il codice `Expression<Func<TEntity, bool>> filter` significa che il chiamante fornirà un'espressione lambda in base il `TEntity` tipo e l'espressione restituirà un valore booleano. Ad esempio, se il repository viene creata un'istanza per il `Student` tipo di entità, il codice nel metodo chiamante può specificare `student => student.LastName == "Smith` &quot; per il `filter` parametro.

Il codice `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` significa anche che il chiamante fornirà un'espressione lambda. Ma in questo caso, l'input per l'espressione è un `IQueryable` dell'oggetto per il `TEntity` tipo. L'espressione restituirà una versione ordinata di tale `IQueryable` oggetto. Ad esempio, se il repository viene creata un'istanza per il `Student` tipo di entità, il codice nel metodo chiamante può specificare `q => q.OrderBy(s => s.LastName)` per il `orderBy` parametro.

Il codice di `Get` metodo crea un `IQueryable` dell'oggetto e quindi applica l'espressione di filtro, se presente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

Successivamente si applica le espressioni di caricamento eager dopo l'analisi di elenco delimitato da virgole:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

Infine, viene applicato il `orderBy` espressione se è presente e restituisce i risultati; in caso contrario restituisce i risultati della query non ordinata:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

Quando si chiama il `Get` (metodo), è possibile eseguire il filtro e ordinamento di `IEnumerable` raccolta restituita dal metodo anziché fornire parametri per queste funzioni. Tuttavia, ordinamento e filtro lavoro quindi possono essere eseguite in memoria nel server web. Tramite questi parametri, assicurarsi che il lavoro viene svolto dal database anziché il server web. Un'alternativa consiste nel creare le classi derivate per tipi di entità specifico e aggiungere specializzate `Get` metodi, ad esempio `GetStudentsInNameOrder` o `GetStudentsByName`. In un'applicazione complessa, tuttavia, questo può comportare un numero elevato di tali classi derivate e i metodi specializzati, che può essere più attività per la gestione.

Il codice di `GetByID`, `Insert`, e `Update` metodi è simile a quello visualizzato nel repository non generica. (Che non è fornito un parametro di caricamento eager nel `GetByID` firma, perché non è possibile eseguire il caricamento immediato con il `Find` metodo.)

Sono previste due overload di `Delete` metodo:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

Uno di questi consente di passare in solo l'ID dell'entità da eliminare e uno che accetta un'istanza di entità. Come osservato nel [la gestione della concorrenza](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) esercitazione per la gestione di concorrenza è necessario un `Delete` metodo che accetta un'istanza di entità che include il valore originale di una proprietà di rilevamento.

Questo repository generico gestirà requisiti CRUD tipici. Quando un determinato tipo di entità è previsti requisiti speciali, ad esempio più complesso il filtro o ordinamento, è possibile creare una classe derivata che ha metodi aggiuntivi per quel tipo.

## <a name="creating-the-unit-of-work-class"></a>Creare la classe di unità di lavoro

La classe di unità di lavoro ha uno scopo: per assicurarsi che quando si utilizza più repository, condividono un contesto di database singolo. In questo modo, quando è stata completata un'unità di lavoro è possibile chiamare il `SaveChanges` metodo su quell'istanza del contesto e verificare che tutte le modifiche verranno coordinate. Tutte le esigenze di classe è un `Save` (metodo) e una proprietà per ogni tipo di repository. Ogni proprietà repository restituisce un'istanza del repository è stata creata un'istanza utilizzando la stessa istanza di contesto di database come altre istanze del repository.

Nel *DAL* cartella, creare un file di classe denominato *UnitOfWork.cs* e sostituire il codice del modello con il codice seguente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

Il codice crea le variabili di classe per il contesto del database e di ogni repository. Per il `context` variabile, un nuovo contesto viene creata un'istanza:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

Ogni proprietà repository controlla se il repository esiste già. In caso contrario, viene creata un'istanza del repository, passando l'istanza del contesto. Di conseguenza, tutti i repository condividono la stessa istanza di contesto.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

Il `Save` chiamate al metodo `SaveChanges` nel contesto del database.

Come qualsiasi classe che crea un'istanza di un contesto di database in una variabile di classe, il `UnitOfWork` implementa `IDisposable` ed elimina il contesto.

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>Modificare il Controller corso per l'utilizzo della classe UnitOfWork e repository

Sostituire il codice attualmente presenti in *CourseController.cs* con il codice seguente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

Questo codice aggiunge una variabile di classe per la `UnitOfWork` classe. (Se si utilizzano le interfacce qui, è non inizializzare la variabile qui; invece, si implementa un modello di due costruttori allo stesso modo il `Student` repository.)

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

Nel resto della classe, tutti i riferimenti per il contesto del database sono sostituiti dai riferimenti nel repository appropriati, utilizzando `UnitOfWork` proprietà per accedere al repository. Il `Dispose` metodo elimina il `UnitOfWork` istanza.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

Esecuzione del sito e fare clic su di **corsi** scheda.

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

La pagina analizza e funziona allo stesso modo era prima che le modifiche e le altre pagine corso anche il funzionamento è uguale.

## <a name="summary"></a>Riepilogo

Sono state implementate del repository e l'unità di lavoro modelli. È stato utilizzato espressioni lambda come parametri del metodo nel repository generico. Per ulteriori informazioni su come usare queste espressioni con un `IQueryable` , vedere [IQueryable(T) interfaccia (LINQ)](https://msdn.microsoft.com/library/bb351562.aspx) in MSDN Library. Nella prossima esercitazione verrà illustrato come gestire alcune i scenari avanzati.

Collegamenti ad altre risorse di Entity Framework, vedere il [mappa del contenuto ASP.NET dati accesso](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Successivo](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
