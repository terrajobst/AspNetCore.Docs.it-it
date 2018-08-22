---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
title: Creazione di classi di modelli con LINQ to SQL (c#) | Microsoft Docs
author: microsoft
description: L'obiettivo di questa esercitazione è illustrare un metodo di creazione di classi di modello per un'applicazione ASP.NET MVC. In questa esercitazione descrive come compilare modello c...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: f84b4a16-e8bb-49e8-87a0-1832879a3501
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
msc.type: authoredcontent
ms.openlocfilehash: c76e92ebfab1db162151ea5753888a60a4d54e59
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830369"
---
<a name="creating-model-classes-with-linq-to-sql-c"></a>Creazione di classi di modelli con LINQ to SQL (c#)
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_CS.pdf)

> L'obiettivo di questa esercitazione è illustrare un metodo di creazione di classi di modello per un'applicazione ASP.NET MVC. In questa esercitazione descrive come compilare le classi di modello ed eseguire l'accesso al database sfruttando i vantaggi di Microsoft LINQ to SQL.


L'obiettivo di questa esercitazione è illustrare un metodo di creazione di classi di modello per un'applicazione ASP.NET MVC. In questa esercitazione descrive come compilare le classi di modello ed eseguire l'accesso al database sfruttando i vantaggi di Microsoft LINQ to SQL

In questa esercitazione, si compila un'applicazione di database di film base. Si inizia creando l'applicazione di database di film in modo più semplice e rapido possibile. Eseguiamo tutte di accesso ai dati direttamente dalle azioni del controller.

Successivamente, informazioni su come usare il modello di Repository. Usando il modello di Repository richiede un po' più di lavoro. Tuttavia, il vantaggio dell'adozione di questo modello è che consente di compilare applicazioni che sono adattabili a modificare e può essere facilmente testata.

## <a name="what-is-a-model-class"></a>Che cos'è una classe di modello?

Un modello di MVC contiene tutta la logica dell'applicazione che non è inclusa in una visualizzazione MVC o un controller MVC. In particolare, un modello di MVC contiene tutte le business dell'applicazione e logica di accesso ai dati.

È possibile usare un'ampia gamma di tecnologie diverse per implementare la logica di accesso ai dati. Ad esempio, è possibile compilare le classi di accesso dati usando le classi di Microsoft Entity Framework, NHibernate, Subsonic o ADO.NET.

In questa esercitazione usare LINQ to SQL per eseguire query e aggiornare il database. LINQ to SQL consente di accedere con un metodo molto semplice dell'interazione con un database Microsoft SQL Server. Tuttavia, è importante comprendere che il framework ASP.NET MVC non è associato a LINQ to SQL in alcun modo. ASP.NET MVC è compatibile con qualsiasi tecnologia di accesso ai dati.

## <a name="create-a-movie-database"></a>Creare un Database di film

In questa esercitazione, per illustrare come è possibile creare classi di modello, ovvero è compilare una semplice applicazione di database di film. Il primo passaggio consiste nel creare un nuovo database. Fare doppio clic su App\_cartella di dati nella finestra Esplora soluzioni e selezionare l'opzione di menu **Aggiungi, elemento nuovo**. Selezionare il **Database di SQL Server** modello, assegnare il nome MoviesDB.mdf e scegliere il **Add** pulsante (vedere la figura 1).


[![Aggiunta di un nuovo Database di SQL Server](creating-model-classes-with-linq-to-sql-cs/_static/image2.png)](creating-model-classes-with-linq-to-sql-cs/_static/image1.png)

**Figura 01**: aggiunta di un nuovo Database di SQL Server ([fare clic per visualizzare l'immagine con dimensioni normali](creating-model-classes-with-linq-to-sql-cs/_static/image3.png))


Dopo aver creato il nuovo database, è possibile aprire il database facendovi doppio MoviesDB.mdf nell'App\_cartella dati. Doppio clic sul file MoviesDB.mdf apre la finestra di Esplora Server (vedere la figura 2).

La finestra di Esplora Server è denominata la finestra Esplora Database quando si utilizza Visual Web Developer.


[![Uso della finestra di Esplora Server](creating-model-classes-with-linq-to-sql-cs/_static/image5.png)](creating-model-classes-with-linq-to-sql-cs/_static/image4.png)

**Figura 02**: tramite la finestra Esplora Server ([fare clic per visualizzare l'immagine con dimensioni normali](creating-model-classes-with-linq-to-sql-cs/_static/image6.png))


È necessario aggiungere una tabella al database che rappresenta i film. Fare doppio clic sulla cartella tabelle e selezionare l'opzione di menu **Aggiungi nuova tabella**. Selezionando questa opzione di menu consente di aprire la finestra di progettazione tabella (vedere la figura 3).


[![Uso della finestra di Esplora Server](creating-model-classes-with-linq-to-sql-cs/_static/image8.png)](creating-model-classes-with-linq-to-sql-cs/_static/image7.png)

**Figura 03**: la progettazione di tabelle ([fare clic per visualizzare l'immagine con dimensioni normali](creating-model-classes-with-linq-to-sql-cs/_static/image9.png))


È necessario aggiungere le seguenti colonne alla tabella relativa al database:

| **Nome della colonna** | **Tipo di dati** | **Consenti valori null** |
| --- | --- | --- |
| Id | Int | False |
| Titolo | Nvarchar (200) | False |
| Director | nvarchar (50) | False |

È necessario eseguire due operazioni speciali per la colonna Id. In primo luogo, è necessario contrassegnare la colonna Id come una colonna chiave primaria, selezionare la colonna in Progettazione tabelle e scegliendo l'icona di una chiave. LINQ to SQL è necessario specificare le colonne chiave primaria durante l'esecuzione viene inserita o aggiornata nel database.

Successivamente, è necessario contrassegnare la colonna Id come una colonna Identity assegnando il valore Yes per le **Identity** proprietà (vedere la figura 3). Una colonna Identity è una colonna che viene assegnata automaticamente un nuovo numero ogni volta che si aggiunge una nuova riga di dati a una tabella.

## <a name="create-linq-to-sql-classes"></a>Creare classi LINQ to SQL

Il nostro modello MVC conterrà LINQ alle classi di SQL che rappresentano la tabella di database tblMovie. Il modo più semplice per creare queste classi LINQ to SQL è fare clic sulla cartella modelli, selezionare **Aggiungi, elemento nuove**, selezionare il modello LINQ to SQL classi, denominare le classi Movie.dbml e fare clic su di **Add**sul pulsante (vedere la figura 4).


[![Creazione di LINQ alle classi di SQL](creating-model-classes-with-linq-to-sql-cs/_static/image11.png)](creating-model-classes-with-linq-to-sql-cs/_static/image10.png)

**Figura 04**: la creazione di classi LINQ to SQL ([fare clic per visualizzare l'immagine con dimensioni normali](creating-model-classes-with-linq-to-sql-cs/_static/image12.png))


Immediatamente dopo aver creato il film classi LINQ to SQL, viene visualizzata la finestra di Progettazione relazionale oggetti. È possibile trascinare le tabelle del database dalla finestra Esplora Server in Object Relational Designer per creare classi LINQ to SQL che rappresentano le tabelle di database specifico. È necessario aggiungere la tabella di database tblMovie in Object Relational Designer (vedere la figura 5).


[![Usando Object Relational Designer](creating-model-classes-with-linq-to-sql-cs/_static/image14.png)](creating-model-classes-with-linq-to-sql-cs/_static/image13.png)

**Figura 05**: usando Object Relational Designer ([fare clic per visualizzare l'immagine con dimensioni normali](creating-model-classes-with-linq-to-sql-cs/_static/image15.png))


Per impostazione predefinita, Object Relational Designer crea una classe con il nome stesso della tabella di database che si trascina la finestra di progettazione. Tuttavia, non si desidera chiamare la classe `tblMovie`. Pertanto, fare clic sul nome della classe nella finestra di progettazione e modificare il nome della classe per film.

Infine, ricordarsi di scegliere la **salvare** sul pulsante (l'immagine di disco floppy) per salvare il LINQ alle classi di SQL. In caso contrario, le classi LINQ to SQL non viene generato da Progettazione relazionale oggetti.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Utilizzo di LINQ to SQL in un'azione del Controller

Ora che abbiamo nostro classi LINQ to SQL, è possibile usare queste classi per recuperare i dati dal database. In questa sezione descrive come usare LINQ per le classi SQL direttamente all'interno di un'azione del controller. Verrà visualizzato l'elenco di film dalla tabella di database tblMovies in una visualizzazione MVC.

In primo luogo, è necessario modificare la classe HomeController. Questa classe è reperibile nella cartella controller dell'applicazione. Modificare la classe in modo analogo la classe nel listato 1.

**Listato 1: `Controllers\HomeController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample1.cs)]

Il `Index()` azione nel listato 1 Usa una classe LINQ to SQL DataContext (la `MovieDataContext`) per rappresentare il `MoviesDB` database. Il `MoveDataContext` classe è stata generata da Visual Studio Object Relational Designer.

Viene eseguita una query LINQ in DataContext per recuperare tutte le schermate o filmati dal `tblMovies` tabella di database. L'elenco di film viene assegnato a una variabile locale denominata `movies`. Infine, l'elenco di film viene passato alla visualizzazione dei dati di visualizzazione.

Per visualizzare i film, che è quindi necessario modificare la visualizzazione dell'indice. È possibile trovare la visualizzazione dell'indice nel `Views\Home\` cartella. Aggiornare la visualizzazione dell'indice, in modo che risulti come la visualizzazione nel listato 2.

**Listato 2: `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample2.aspx)]

Si noti che la visualizzazione dell'indice modificata include un `<%@ import namespace %>` direttiva all'inizio della visualizzazione. Questa direttiva viene importato il `MvcApplication1.Models namespace`. Questo spazio dei nomi è necessario per lavorare con i `model` classi, in particolare, il `Movie` classe, nella visualizzazione.

La visualizzazione nel listato 2 contiene un' `foreach` ciclo che esegue l'iterazione attraverso tutte le voci rappresentate dal `ViewData.Model` proprietà. Il valore della `Title` proprietà viene visualizzata per ogni `movie`.

Si noti che il valore della `ViewData.Model` proprietà viene eseguito il cast a un `IEnumerable`. Ciò è necessario per scorrere il contenuto di `ViewData.Model`. Ecco un'altra opzione consiste nel creare l'oggetto fortemente tipizzato `view`. Quando si crea un fortemente tipizzate `view`, si esegue il cast di `ViewData.Model` proprietà a un determinato tipo in classe code-behind della vista.

Se si esegue l'applicazione dopo aver modificato il `HomeController` classe e l'indice della visualizzazione si otterrà una pagina vuota. Si otterrà una pagina vuota perché non sono presenti record di film nel `tblMovies` tabella di database.

Per aggiungere record per il `tblMovies` tabella di database, fare doppio clic sul `tblMovies` tabella nella finestra di Esplora Server (finestra di Esplora Database in Visual Web Developer) del database e selezionare l'opzione di menu Mostra dati tabella. È possibile inserire `movie` record tramite la griglia visualizzata (vedere la figura 6).


[![Inserimento di film](creating-model-classes-with-linq-to-sql-cs/_static/image17.png)](creating-model-classes-with-linq-to-sql-cs/_static/image16.png)

**Figura 06**: inserimento di film ([fare clic per visualizzare l'immagine con dimensioni normali](creating-model-classes-with-linq-to-sql-cs/_static/image18.png))


Dopo aver aggiunto alcuni record di database per il `tblMovies` tabella e si esegue l'applicazione, verrà visualizzata la pagina nella figura 7. Tutti i record di database di film vengono visualizzati in un elenco puntato.


[![Visualizzazione di film con la visualizzazione dell'indice](creating-model-classes-with-linq-to-sql-cs/_static/image20.png)](creating-model-classes-with-linq-to-sql-cs/_static/image19.png)

**Figura 07**: visualizzazione di film con la visualizzazione dell'indice ([fare clic per visualizzare l'immagine con dimensioni normali](creating-model-classes-with-linq-to-sql-cs/_static/image21.png))


## <a name="using-the-repository-pattern"></a>Usando il modello di Repository

Nella sezione precedente, abbiamo usato LINQ alle classi SQL direttamente all'interno di un'azione del controller. È stato usato il `MovieDataContext` direttamente dalla classe di `Index()` azione del controller. Non vi sono problemi con questa operazione nel caso di una semplice applicazione. Tuttavia, lavorare direttamente con LINQ to SQL in una classe controller crea problemi quando si vuole compilare un'applicazione più complessa.

Utilizzo di LINQ to SQL all'interno di una classe controller rende difficile passare tecnologie di accesso ai dati in futuro. Ad esempio, si potrebbe decidere di passare dall'uso di Microsoft LINQ to SQL per l'utilizzo di Microsoft Entity Framework come la tecnologia di accesso ai dati. In tal caso, è necessario riscrivere ogni controller che accede al database all'interno dell'applicazione.

Utilizzo di LINQ to SQL all'interno di una classe controller inoltre rende difficile compilare unit test per l'applicazione. In genere, non si desidera interagire con un database durante l'esecuzione di unit test. Si desidera usare gli unit test per testare la logica dell'applicazione e non il server di database.

Per compilare un'applicazione MVC più adattabile a future modifiche e che può essere testato più facilmente, è consigliabile usare il modello di Repository. Quando si usa il modello di Repository, è creare una classe di repository separati che contiene tutta la logica di accesso ai database.

Quando si crea la classe di repository, si crea un'interfaccia che rappresenta tutti i metodi usati dalla classe di repository. Nei controller, scrivere il codice in base all'interfaccia anziché il repository. In questo modo, è possibile implementare il repository utilizzando tecnologie di accesso ai dati diversi in futuro.

L'interfaccia nel listato 3 denominato `IMovieRepository` e rappresenta un singolo metodo denominato `ListAll()`.

**Listato 3: `Models\IMovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample3.cs)]

Implementa la classe di repository nel listato 4 il `IMovieRepository` interfaccia. Si noti che include un metodo denominato `ListAll()` corrispondente per il metodo richiesto per il `IMovieRepository` interfaccia.

**Listato 4: `Models\MovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample4.cs)]

Infine, il `MoviesController` classe elencate nel listato 5 Usa il modello di Repository. Non è più Usa LINQ alle classi SQL direttamente.

**Listato 5: `Controllers\MoviesController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample5.cs)]

Si noti che il `MoviesController` classe elencate nel listato 5 dispone di due costruttori. Il primo costruttore, il costruttore senza parametri, viene chiamato quando l'applicazione è in esecuzione. Questo costruttore crea un'istanza di `MovieRepository` classe e la passa al secondo costruttore.

Il secondo costruttore con un solo parametro: una `IMovieRepository` parametro. Questo costruttore assegna il valore del parametro a un campo a livello di classe denominato `_repository`.

Il `MoviesController` classe è sfruttando i vantaggi di un modello di progettazione software denominato modello di inserimento delle dipendenze. In particolare, utilizza un elemento denominato inserimento delle dipendenze del costruttore. Altre informazioni su questo modello, vedere l'articolo seguente di Martin Fowler:

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Si noti che tutto il codice nel `MoviesController` classe (fatta eccezione per il primo costruttore) interagisce con il `IMovieRepository` interfaccia anziché l'effettivo `MovieRepository` classe. Il codice interagisce con un'interfaccia astratta anziché un'implementazione concreta dell'interfaccia.

Se si desidera modificare la tecnologia di accesso ai dati utilizzata dall'applicazione, è possibile implementare semplicemente il `IMovieRepository` interfaccia con una classe che usa la tecnologia di accesso del database alternativo. Ad esempio, è possibile creare un `EntityFrameworkMovieRepository` classe o un `SubSonicMovieRepository` classe. Poiché la classe controller è programmare in base all'interfaccia, è possibile passare una nuova implementazione di `IMovieRepository` al controller di classe e la classe continuerà a funzionare.

Inoltre, se si vuole testare il `MoviesController` classe, non è possibile passare una classe di repository fittizio film per i `HomeController`. È possibile implementare il `IMovieRepository` classe con una classe che non sono effettivamente accesso del database, ma contiene tutti i metodi richiesti del `IMovieRepository` interfaccia. In questo modo, poter eseguire unit test di `MoviesController` classe senza effettivamente l'accesso a un database effettivo.

## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione è stata per illustrare come è possibile creare classi di modelli MVC, sfruttando i vantaggi di Microsoft LINQ to SQL. Abbiamo esaminato due strategie per la visualizzazione dei dati del database in un'applicazione ASP.NET MVC. In primo luogo, abbiamo creato LINQ alle classi di SQL e le classi utilizzate direttamente all'interno di un'azione del controller. Utilizzo di LINQ alle classi di SQL all'interno di un controller consente rapidamente e facilmente visualizzare dati di database in un'applicazione MVC.

Successivamente, abbiamo esplorato un percorso leggermente più complessa, ma indubbiamente più virtuoso, per la visualizzazione dei dati del database. Abbiamo sfruttato il modello di Repository e tutta la logica di accesso database posizionati in una classe di repository separati. Nel controller, abbiamo scritto tutto il codice in un'interfaccia anziché una classe concreta. Il vantaggio di schema Repository è che ci permette di cambiare facilmente in futuro le tecnologie di accesso di database e ci permette di testare facilmente le classi controller.

> [!div class="step-by-step"]
> [Precedente](creating-model-classes-with-the-entity-framework-cs.md)
> [Successivo](displaying-a-table-of-database-data-cs.md)
