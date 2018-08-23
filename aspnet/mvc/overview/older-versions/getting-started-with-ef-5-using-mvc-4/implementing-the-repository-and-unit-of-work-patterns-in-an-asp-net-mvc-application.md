---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: Implementazione di Repository e unità di lavoro modelli in un'applicazione ASP.NET MVC (9 di 10) | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 20f82744582faddaffdc5b4785bf208ecf0955a7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835794"
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>Implementazione di Repository e unità di lavoro modelli in un'applicazione ASP.NET MVC (9 di 10)
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto completato](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni a partire dall'inizio oppure [scarica un progetto iniziale per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si verifica un problema è possibile risolvere, [Scarica il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. Confrontando il codice per il codice completo è generalmente possibile trovare la soluzione al problema. Per alcuni errori comuni e come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Nell'esercitazione precedente ereditarietà è utilizzato per ridurre il codice ridondante nel `Student` e `Instructor` classi di entità. In questa esercitazione si noterà alcuni modi per usare il repository di modelli e unità di lavoro per le operazioni CRUD. Come nell'esercitazione precedente, in questo sarà necessario modificare il funzionamento del codice con le pagine è già creati anziché crearne nuove pagine.

## <a name="the-repository-and-unit-of-work-patterns"></a>Il Repository di modelli e unità di lavoro

Il repository di modelli e unità di lavoro servono per creare un livello di astrazione tra il livello di accesso ai dati e il livello di logica di business di un'applicazione. L'implementazione di questi modelli può essere utile per isolare l'applicazione dalle modifiche nell'archivio dati e può semplificare il testing unità automatizzato o lo sviluppo basato su test (TDD).

In questa esercitazione è possibile implementare una classe di repository per ogni tipo di entità. Per il `Student` tipo di entità verrà creata un'interfaccia del repository e una classe di repository. Quando si crea un'istanza del repository nel controller di, si userà l'interfaccia in modo che il controller accetta un riferimento a qualsiasi oggetto che implementa l'interfaccia del repository. Quando il controller viene eseguito in un server web, riceve un repository che funziona con Entity Framework. Quando il controller viene eseguito in una classe di unit test, riceve un repository che funziona con i dati archiviati in modo che è possibile gestire facilmente per i test, ad esempio una raccolta in memoria.

Più avanti nell'esercitazione si userà più repository e unità di lavoro di classe per il `Course` e `Department` tipi di entità nel `Course` controller. L'unità di lavoro classe coordina il lavoro dei repository più creando una classe del contesto di database singolo condivisa da tutti gli elementi. Se si desidera essere in grado di eseguire gli unit test automatici, si potrebbe creare e usare le interfacce per queste classi nello stesso modo è stato fatto per la `Student` repository. Tuttavia, per mantenere semplice l'esercitazione, si sarà creare e usare queste classi senza interfacce.

La figura seguente mostra un modo per realizzare le relazioni tra il controller e le classi del contesto rispetto a non usa affatto il repository o schema unit of work.

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

Unit test non vengono creati in questa serie di esercitazioni. Per un'introduzione alla sviluppo basato su test con un'applicazione MVC che usa il modello di repository, vedere [procedura dettagliata: uso TDD con ASP.NET MVC](https://msdn.microsoft.com/library/ff847525.aspx). Per altre informazioni sul modello di repository, vedere le risorse seguenti:

- [Lo schema Repository](https://msdn.microsoft.com/library/ff649690.aspx) su MSDN.
- [Uso di schemi Unit of Work e Repository con Entity Framework 4.0](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) nel blog del team di Entity Framework.
- [Agile Entity Framework 4 Repository](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) serie di post sul blog di Julie.
- [Creazione di Account in un'applicazione HTML5/jQuery Glance](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) sul blog di Dan Wahlin.

> [!NOTE]
> Esistono diversi modi per implementare il repository di modelli e unità di lavoro. È possibile usare classi di repository con o senza una classe di unità di lavoro. È possibile implementare un unico repository per tutti i tipi di entità o uno per ogni tipo. Se si implementa uno per ogni tipo, è possibile usare classi separate, una classe di base generica e le classi derivate, o una classe base astratta e le classi derivate. È possibile includere la logica di business in un repository o ridurlo alla logica di accesso ai dati. È anche possibile creare un livello di astrazione nella classe del contesto di database usando [IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx) sono interfacce anziché [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) tipi per i set di entità. L'approccio all'implementazione di un livello di astrazione illustrato in questa esercitazione è un'opzione da prendere in considerazione, non una raccomandazione per tutti gli ambienti e scenari.


## <a name="creating-the-student-repository-class"></a>Creazione della classe di Repository per studenti

Nel *DAL* cartella, creare un file di classe denominato *IStudentRepository.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

Questo codice dichiara un set tipico di metodi CRUD, inclusi i due metodi di lettura, ovvero uno che restituisce tutti i `Student` le entità e uno che consente di trovare un singolo `Student` entità dall'ID.

Nel *DAL* cartella, creare un file di classe denominato *StudentRepository.cs* file. Sostituire il codice esistente con il codice seguente, che implementa il `IStudentRepository` interfaccia:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

Il contesto del database è definito in una variabile di classe e il costruttore richiede l'oggetto chiamante di passare un'istanza del contesto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

È possibile creare un'istanza di un nuovo contesto all'interno del repository, ma quindi se si usa più repository in un controller, ognuna finirebbe con un contesto separato. In un secondo momento si userà più repository nel `Course` controller, verrà visualizzato come un'unità di lavoro classe può garantire che tutti i repository utilizzano lo stesso contesto.

Implementa il repository [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) ed elimina il contesto del database, come illustrato in precedenza nel controller e i relativi metodi CRUD effettuano chiamate nel contesto di database nello stesso modo che hai visto in precedenza.

## <a name="change-the-student-controller-to-use-the-repository"></a>Modificare il Controller di studente per l'uso del Repository

Nelle *StudentController.cs*, sostituire il codice attualmente nella classe con il codice seguente. Le modifiche sono evidenziate.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

Il controller a questo punto viene dichiarata una variabile di classe per un oggetto che implementa il `IStudentRepository` interfaccia anziché la classe di contesto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

Il costruttore predefinito (senza parametri) Crea una nuova istanza del contesto e un costruttore facoltativo consente al chiamante di passare un'istanza del contesto.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(Se si usava *inserimento delle dipendenze*, o l'inserimento delle dipendenze, non sarebbe necessario il costruttore predefinito poiché assicura che l'oggetto repository corretto sarebbe sempre essere fornito il software di inserimento delle dipendenze.)

I metodi di CRUD, il repository è ora denominato anziché nel contesto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

E il `Dispose` metodo elimina ora il repository anziché nel contesto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

Eseguire il sito, quindi scegliere il **studenti** scheda.

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

La pagina è simile e funziona esattamente come prima è stato modificato il codice per usare il repository e le altre pagine Student funzionano anche allo stesso modo. Tuttavia, è un'importante differenza nel modo di `Index` esegue il metodo del controller di filtro e ordinamento. La versione originale di questo metodo contiene il codice seguente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

Aggiornato `Index` metodo contiene il codice seguente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

È stato modificato solo il codice evidenziato.

Nella versione originale del codice, `students` tipizzata come una `IQueryable` oggetto. La query non viene inviata al database finché non viene convertito in una raccolta usando un metodo come `ToList`, che non viene eseguita fino a quando non la visualizzazione dell'indice accede al modello di studente. Il `Where` metodo nel codice originale precedente diventa un `WHERE` clausola della query SQL che viene inviata al database. Ciò significa che a sua volta che vengono restituite solo le entità selezionate dal database. Tuttavia, come risultato della modifica `context.Students` a `studentRepository.GetStudents()`, il `students` variabile dopo questa istruzione è un `IEnumerable` raccolta che include tutti gli studenti nel database. Il risultato finale dell'applicazione di `Where` metodo è uguale, ma ora il lavoro viene eseguito in memoria nel server del web e non dal database. Per le query che restituiscono grandi volumi di dati, ciò può risultare inefficiente.

> [!TIP]
> 
> **Visual Studio di IQueryable. IEnumerable**
> 
> Dopo aver implementato il repository come illustrato in questo caso, anche se si immette un elemento all'interno di **ricerca** finestra di query inviata a SQL Server restituisce tutte le righe degli studenti perché non include i criteri di ricerca:
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> Questa query restituisce tutti i dati degli studenti in quanto il repository eseguita la query senza conoscere i criteri di ricerca. Il processo di ordinamento, applicazione di criteri di ricerca e selezione di un subset dei dati per il paging (che mostra solo 3 righe in questo caso) viene eseguito in memoria in seguito quando la `ToPagedList` metodo viene chiamato sul `IEnumerable` raccolta.
> 
> Nella versione precedente del codice (prima dell'implementazione di repository), la query non verrà inviata al database fino a dopo aver applicato i criteri di ricerca, quando `ToPagedList` viene chiamato sul `IQueryable` oggetto.
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> Quando viene chiamato ToPagedList su un `IQueryable` dell'oggetto, la query inviata a SQL Server consente di specificare la stringa di ricerca, di conseguenza vengono restituite solo le righe che soddisfano i criteri di ricerca e applicato alcun filtro non deve essere eseguita in memoria.
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> (L'esercitazione seguente illustra come esaminare le query inviate a SQL Server).


La sezione seguente viene illustrato come implementare i metodi di repository che consentono di specificare che questa operazione deve essere eseguita dal database.

È stato creato un livello di astrazione tra il controller e il contesto del database Entity Framework. Se si prevede di eseguire unit test con questa applicazione automatici, è possibile creare una classe di repository alternativi in un progetto di unit test che implementa `IStudentRepository` *.* Invece di chiamare il contesto per la lettura e scrittura dei dati, questa classe di repository fittizio è stato possibile modificare le raccolte in memoria per le funzioni di controller di test.

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>Implementare un Repository generico e una classe di unità di lavoro

Creazione di una classe di repository per ogni tipo di entità può comportare una grande quantità di codice ridondante e può comportare aggiornamenti parziali. Si supponga, ad esempio, che è necessario aggiornare due tipi di entità diverso come parte della stessa transazione. Se ognuna viene utilizzata un'istanza del contesto di database separate, una potrebbe avere esito positivo e l'altro potrebbe avere esito negativo. Un modo per ridurre al minimo il codice ridondante è usare un repository generico e un modo per garantire che tutti i repository utilizzano lo stesso contesto di database (e coordinano pertanto tutti gli aggiornamenti) consiste nell'usare una classe di unità di lavoro.

In questa sezione dell'esercitazione si creerà una `GenericRepository` classe e un `UnitOfWork` classe e utilizzarle nel `Course` controller per accedere a entrambi il `Department` e il `Course` set di entità. Come spiegato in precedenza, per semplificare questa parte dell'esercitazione, non si creino le interfacce per queste classi. Ma se si prevede di utilizzarli per facilitare la metodologia TDD, vengono in genere implementati in interfacce allo stesso modo è stato eseguito il `Student` repository.

### <a name="create-a-generic-repository"></a>Creare un Repository generico

Nel *DAL* cartella, creare *GenericRepository.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

Variabili di classe vengono dichiarate per il contesto del database e per il set di entità che viene creata un'istanza di repository per:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

Il costruttore accetta un'istanza del contesto del database e inizializza la variabile di set di entità:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

Il `Get` metodo Usa le espressioni lambda per il codice chiamante può specificare una condizione di filtro e una colonna per ordinare i risultati in base e un parametro di stringa consente al chiamante di fornire un elenco delimitato da virgole delle proprietà di navigazione per il caricamento eager:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

Il codice `Expression<Func<TEntity, bool>> filter` significa che il chiamante fornirà un'espressione lambda in base il `TEntity` tipo e questa espressione restituirà un valore booleano. Ad esempio, se il repository viene creata un'istanza per il `Student` tipo di entità, è possibile specificare il codice nel metodo chiamante `student => student.LastName == "Smith` &quot; per il `filter` parametro.

Il codice `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` significa anche che il chiamante fornirà un'espressione lambda. Ma in questo caso, l'input per l'espressione è un' `IQueryable` dell'oggetto per il `TEntity` tipo. L'espressione restituirà una versione ordinata di tale `IQueryable` oggetto. Ad esempio, se il repository viene creata un'istanza per il `Student` tipo di entità, è possibile specificare il codice nel metodo chiamante `q => q.OrderBy(s => s.LastName)` per il `orderBy` parametro.

Il codice nel `Get` metodo crea un `IQueryable` dell'oggetto e quindi applica l'espressione di filtro, se presente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

Le espressioni di caricamento eager viene successivamente applicato dopo l'analisi di elenco delimitato da virgole:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

Infine, si applica il `orderBy` espressione se presente e restituisce i risultati; in caso contrario, restituisce i risultati della query non ordinate:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

Quando si chiama il `Get` metodo, è possibile eseguire il filtraggio e l'ordinamento in base il `IEnumerable` raccolta restituita dal metodo invece di fornire i parametri per queste funzioni. Ma l'ordinamento e filtro lavoro potrebbe quindi essere eseguite in memoria nel server web. Utilizzando questi parametri, si garantisce che per il database anziché il server web viene eseguito il lavoro. Un'alternativa consiste nel creare le classi derivate per i tipi di entità specifica e aggiungere specializzato `Get` metodi, ad esempio `GetStudentsInNameOrder` o `GetStudentsByName`. Tuttavia, in un'applicazione complessa, ciò può comportare un numero elevato di tali classi derivate e i metodi specializzati, che può essere più le operazioni di gestione.

Il codice nel `GetByID`, `Insert`, e `Update` metodi è simile a quello del repository non generica. (Si non è fornendo un parametro di caricamento eager nel `GetByID` firma, perché non è possibile eseguire il caricamento eager con la `Find` (metodo).)

Sono disponibili due overload per il `Delete` metodo:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

Uno di questi consente passare semplicemente l'ID dell'entità da eliminare e uno accetta un'istanza di entità. Come avrete notato nella [la gestione della concorrenza](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) esercitazione per concorrenza di gestione è necessario un `Delete` metodo che accetta un'istanza di entità che include il valore originale di una proprietà di rilevamento.

Questo repository generico gestirà requisiti tipici CRUD. Quando un determinato tipo di entità dispone di requisiti speciali, ad esempio di filtro più complesse o l'ordinamento, è possibile creare una classe derivata che offre ulteriori metodi per quel tipo.

## <a name="creating-the-unit-of-work-class"></a>Creare la classe di unità di lavoro

La classe di unità di lavoro viene usato uno degli scopi: per assicurarsi che quando si usa più repository, che condividono un contesto di database singolo. In questo modo, quando un'unità di lavoro è stata completata è possibile chiamare il `SaveChanges` metodo su quell'istanza del contesto e avere la certezza che tutte le modifiche saranno coordinate. A questo le esigenze di classe è un `Save` (metodo) e una proprietà per ogni repository. Ogni proprietà repository restituisce un'istanza del repository che è stata creata un'istanza usando la stessa istanza di contesto di database come le altre istanze di repository.

Nel *DAL* cartella, creare un file di classe denominato *UnitOfWork.cs* e sostituire il codice del modello con il codice seguente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

Il codice crea le variabili di classe per il contesto del database e ogni repository. Per il `context` variabile, un nuovo contesto viene creata un'istanza:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

Ogni proprietà repository controlla se il repository esiste già. In caso contrario, viene creata un'istanza del repository, passando l'istanza di contesto. Di conseguenza, tutti i repository condividono la stessa istanza di contesto.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

Il `Save` chiamate al metodo `SaveChanges` nel contesto del database.

Come qualsiasi classe che crea un'istanza di un contesto di database in una variabile di classe, il `UnitOfWork` classe implementa `IDisposable` ed elimina il contesto.

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>Modifica del Controller di corsi per usare la classe UnitOfWork e i repository

Sostituire il codice attualmente presenti in *CourseController.cs* con il codice seguente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

Questo codice aggiunge una variabile di classe per il `UnitOfWork` classe. (Se si usa le interfacce in questo caso, si non sarebbe inizializzare la variabile qui; al contrario, si implementa un modello di due costruttori come è stato fatto la `Student` repository.)

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

Nella parte restante della classe, tutti i riferimenti al contesto del database vengono sostituiti dai riferimenti nel repository appropriato, tramite `UnitOfWork` le proprietà per accedere al repository. Il `Dispose` metodo elimina il `UnitOfWork` istanza.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

Eseguire il sito, quindi scegliere il **corsi** scheda.

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

La pagina è simile e funziona esattamente come in precedenza le modifiche e le altre pagine del corso anche funzionano allo stesso modo.

## <a name="summary"></a>Riepilogo

È stato implementato sia il repository di modelli e unità di lavoro. È stato usato le espressioni lambda come parametri di metodo all'interno del repository generico. Per altre informazioni su come utilizzare queste espressioni con un `IQueryable` oggetti, vedere [IQueryable(T) interfaccia (LINQ)](https://msdn.microsoft.com/library/bb351562.aspx) in MSDN Library. Nella prossima esercitazione verrà illustrato come gestire alcuni scenari avanzati.

Sono disponibili collegamenti ad altre risorse di Entity Framework nel [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Successivo](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
