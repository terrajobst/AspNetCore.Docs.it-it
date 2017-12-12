---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
title: Creazione di classi di modello con LINQ to SQL (VB) | Documenti Microsoft
author: microsoft
description: "L'obiettivo di questa esercitazione è illustrare un metodo di creazione di classi del modello per un'applicazione MVC ASP.NET. In questa esercitazione, è illustrato come compilare modello c..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: a4a25a75-d71f-4509-98b4-df72e748985a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
msc.type: authoredcontent
ms.openlocfilehash: 972d5b11049825e84e070ef1c4b2b90116654397
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="creating-model-classes-with-linq-to-sql-vb"></a>Creazione di classi di modello con LINQ to SQL (VB)
====================
da [Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_VB.pdf)

> L'obiettivo di questa esercitazione è illustrare un metodo di creazione di classi del modello per un'applicazione MVC ASP.NET. In questa esercitazione è imparare generare classi del modello ed eseguire l'accesso al database sfruttando Microsoft LINQ to SQL.


L'obiettivo di questa esercitazione è illustrare un metodo di creazione di classi del modello per un'applicazione MVC ASP.NET. In questa esercitazione è imparare generare classi del modello ed eseguire l'accesso al database sfruttando Microsoft LINQ to SQL.

In questa esercitazione è compilare un'applicazione di database basic film. Si inizierà creando l'applicazione di database film nel modo più semplice e rapido possibile. È di eseguire tutte accesso ai dati direttamente dalle azioni del controller.

Quindi, imparare a usare il modello di Repository. Utilizzando il modello di Repository richiede un po' più lavoro. Tuttavia, il vantaggio di utilizzare questo modello è che consente di creare applicazioni che sono adattabili per modificare e testare con facilità.

## <a name="what-is-a-model-class"></a>Che cos'è una classe di modello?

Un modello MVC contiene la logica dell'applicazione che non è inclusa in una visualizzazione MVC o controller MVC. In particolare, un modello MVC contiene tutte le business dell'applicazione e la logica di accesso ai dati.

È possibile utilizzare un'ampia gamma di tecnologie per implementare la logica di accesso ai dati. Ad esempio, è possibile compilare le classi di accesso ai dati utilizzando le classi ADO.NET, NHibernate, Subsonic o Microsoft Entity Framework.

In questa esercitazione, usare LINQ to SQL per eseguire una query e aggiornare il database. LINQ to SQL fornisce un metodo molto semplice di interagire con un database di Microsoft SQL Server. Tuttavia, è importante comprendere che il framework di MVC ASP.NET non è correlato a LINQ to SQL in alcun modo. ASP.NET MVC è compatibile con qualsiasi tecnologia di accesso ai dati.

## <a name="create-a-movie-database"></a>Creare un Database di film

In questa esercitazione, per illustrare come è possibile creare classi di modello, ovvero è compilare una semplice applicazione di database di film. Il primo passaggio consiste nel creare un nuovo database. Fare doppio clic su App\_cartella di dati nella finestra Esplora soluzioni e selezionare l'opzione di menu **Aggiungi, elemento nuovo**. Selezionare il modello di Database di SQL Server, assegnare il nome MoviesDB.mdf e fare clic su di **Aggiungi** pulsante (vedere Figura 1).


[![Aggiunta di un nuovo Database di SQL Server](creating-model-classes-with-linq-to-sql-vb/_static/image2.png)](creating-model-classes-with-linq-to-sql-vb/_static/image1.png)

**Figura 01**: aggiunta di un nuovo Database di SQL Server ([fare clic per visualizzare l'immagine ingrandita](creating-model-classes-with-linq-to-sql-vb/_static/image3.png))


Dopo aver creato il nuovo database, è possibile aprire il database da doppio clic sul file MoviesDB.mdf nell'App\_cartella dati. Doppio clic sul file MoviesDB.mdf apre la finestra di Esplora Server (vedere la figura 2).

|  | La finestra di Esplora Server è denominata la finestra Esplora Database quando si utilizza Visual Web Developer. |
| --- | --- |


[![Utilizzare la finestra di Esplora Server](creating-model-classes-with-linq-to-sql-vb/_static/image5.png)](creating-model-classes-with-linq-to-sql-vb/_static/image4.png)

**Figura 02**: utilizzando la finestra di Esplora Server ([fare clic per visualizzare l'immagine ingrandita](creating-model-classes-with-linq-to-sql-vb/_static/image6.png))


È necessario aggiungere una tabella di database che rappresenta il film. Fare doppio clic sulla cartella tabelle e selezionare l'opzione di menu **Aggiungi nuova tabella**. Selezionare questa opzione di menu per aprire Progettazione tabelle (vedere la figura 3).


[![Utilizzare la finestra di Esplora Server](creating-model-classes-with-linq-to-sql-vb/_static/image8.png)](creating-model-classes-with-linq-to-sql-vb/_static/image7.png)

**Figura 03**: la progettazione di tabelle ([fare clic per visualizzare l'immagine ingrandita](creating-model-classes-with-linq-to-sql-vb/_static/image9.png))


È necessario aggiungere le seguenti colonne alla tabella relativa al database:

| **Nome colonna** | **Tipo di dati** | **Consenti valori null** |
| --- | --- | --- |
| Id | Int | False |
| Titolo | Nvarchar (200) | False |
| Director | nvarchar (50) | False |

È necessario eseguire due operazioni speciali per la colonna Id. In primo luogo, è necessario contrassegnare la colonna Id come una colonna chiave primaria, selezionare la colonna in Progettazione tabelle e scegliendo l'icona di una chiave. LINQ to SQL è necessario specificare le colonne chiave primaria quando l'esecuzione di inserimenti o aggiornamenti al database.

Successivamente, è necessario contrassegnare la colonna Id come una colonna Identity assegnando il valore Sì per il **identità** proprietà (vedere la figura 3). Una colonna Identity è una colonna che viene assegnata automaticamente un nuovo numero di ogni volta che si aggiunge una nuova riga di dati in una tabella.

Dopo aver apportato queste modifiche, salvare la tabella con il nome tblMovie. È possibile salvare la tabella facendo clic sul pulsante Salva.

## <a name="create-linq-to-sql-classes"></a>Creare classi LINQ to SQL

Il nostro modello MVC conterrà LINQ alle classi di SQL che rappresentano la tabella di database tblMovie. È il modo più semplice per creare queste classi LINQ to SQL per la cartella Modelli destro, scegliere **Aggiungi, elemento nuovo**, selezionare il modello LINQ to SQL Classes, denominare le classi Movie.dbml e fare clic su di **Aggiungi**pulsante (vedere la figura 4).


[![Creazione di LINQ alle classi di SQL](creating-model-classes-with-linq-to-sql-vb/_static/image11.png)](creating-model-classes-with-linq-to-sql-vb/_static/image10.png)

**Figura 04**: creazione di classi LINQ to SQL ([fare clic per visualizzare l'immagine ingrandita](creating-model-classes-with-linq-to-sql-vb/_static/image12.png))


Immediatamente dopo aver creato il film classi LINQ to SQL, viene visualizzata la finestra di Progettazione relazionale oggetti. È possibile trascinare le tabelle del database dalla finestra Esplora Server Object Relational Designer per creare classi LINQ to SQL che rappresentano le tabelle di database specifico. È necessario aggiungere la tabella di database tblMovie in Object Relational Designer (vedere la figura 4).


[![Utilizzando Progettazione relazionale oggetti](creating-model-classes-with-linq-to-sql-vb/_static/image14.png)](creating-model-classes-with-linq-to-sql-vb/_static/image13.png)

**Figura 05**: utilizzando Object Relational Designer ([fare clic per visualizzare l'immagine ingrandita](creating-model-classes-with-linq-to-sql-vb/_static/image15.png))


Per impostazione predefinita, Object Relational Designer crea una classe con il nome della tabella di database che si trascina la finestra di progettazione stesso. Tuttavia, non si desidera chiamare tblMovie la classe. Pertanto, fare clic sul nome della classe nella finestra di progettazione e modificare il nome della classe per i film.

Infine, ricordarsi di scegliere il **salvare** pulsante (l'immagine di disco floppy) per salvare il LINQ alle classi di SQL. In caso contrario, le classi LINQ to SQL non viene generato da Progettazione relazionale oggetti.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Utilizzo di LINQ to SQL in un'azione del Controller

Ora che è disponibile il nostro classi LINQ to SQL, è possibile usare queste classi per recuperare i dati dal database. In questa sezione informazioni su come utilizzare LINQ alle classi SQL direttamente all'interno di un'azione del controller. Si verrà visualizzato l'elenco di film dalla tabella di database tblMovies in una visualizzazione MVC.

In primo luogo, è necessario modificare la classe HomeController. Questa classe è reperibile nella cartella controller dell'applicazione. Modificare la classe in modo che risulti come se la classe nel listato 1.

**Elenco 1:`Controllers\HomeController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample1.vb)]

L'azione Index () nel listato 1 Usa una classe LINQ to SQL DataContext (il MovieDataContext) per rappresentare il database MoviesDB. La classe MoveDataContext generata da Visual Studio Object Relational Designer.

L'oggetto DataContext per recuperare tutti i film dalla tabella di database tblMovies viene eseguita una query LINQ. L'elenco di film viene assegnato a una variabile locale denominata film. Infine, l'elenco di film viene passato alla vista dati della visualizzazione.

Per visualizzare i film, che è quindi necessario modificare la visualizzazione dell'indice. È possibile trovare la visualizzazione dell'indice nella cartella Views\Home\. Aggiornare la visualizzazione dell'indice in modo che risulti come se la visualizzazione nel listato 2.

**Elenco di 2:`Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample2.aspx)]

Si noti che la visualizzazione dell'indice modificata include un &lt;% @ import dello spazio dei nomi %&gt; direttiva nella parte superiore della visualizzazione. Questa direttiva si importa lo spazio dei nomi MvcApplication1. Questo spazio dei nomi è necessario per poter funzionare con le classi di modello, in particolare, la classe di film, nella visualizzazione.

La visualizzazione nel listato 2 contiene un ciclo For Each che scorre tutti gli elementi rappresentati dalla proprietà ViewData.Model. Il valore della proprietà Title viene visualizzato per ogni film.

Si noti che il valore della proprietà ViewData.Model viene eseguito il cast in un oggetto IEnumerable. Ciò è necessario per scorrere il contenuto di ViewData.Model. Di seguito di un'altra opzione consiste nel creare una visualizzazione fortemente tipizzata. Quando si crea una visualizzazione fortemente tipizzata, si cast della proprietà ViewData.Model a un determinato tipo in una classe code-behind di una vista.

Se si esegue l'applicazione dopo aver modificato la classe HomeController e la visualizzazione dell'indice, si otterrà una pagina vuota. Si otterrà una pagina vuota perché non sono presenti record di film nella tabella di database tblMovies.

Per aggiungere record alla tabella di database tblMovies, fare doppio clic su tabella di database tblMovies nella finestra di Esplora Server (finestra di Esplora Database in Visual Web Developer) e selezionare l'opzione di menu **Mostra dati tabella**. È possibile inserire i record di film utilizzando la griglia visualizzata (vedere Figura 5).


[![Inserimento di filmati](creating-model-classes-with-linq-to-sql-vb/_static/image17.png)](creating-model-classes-with-linq-to-sql-vb/_static/image16.png)

**Figura 06**: inserimento di filmati ([fare clic per visualizzare l'immagine ingrandita](creating-model-classes-with-linq-to-sql-vb/_static/image18.png))


Dopo la tabella tblMovies è aggiungere alcuni record di database e si esegue l'applicazione, si verrà visualizzata la pagina nella figura 7. Tutti i record del database film vengono visualizzati in un elenco puntato.


[![Visualizzazione di filmati con la visualizzazione dell'indice](creating-model-classes-with-linq-to-sql-vb/_static/image20.png)](creating-model-classes-with-linq-to-sql-vb/_static/image19.png)

**Figura 07**: visualizzazione di filmati con la visualizzazione dell'indice ([fare clic per visualizzare l'immagine ingrandita](creating-model-classes-with-linq-to-sql-vb/_static/image21.png))


## <a name="using-the-repository-pattern"></a>Utilizzando il modello di Repository

Nella sezione precedente, abbiamo utilizzato LINQ per classi SQL direttamente all'interno di un'azione del controller. Uso della classe MovieDataContext direttamente dall'azione del controller Index (). Non è verificato un errore in questo modo nel caso di un'applicazione semplice. Tuttavia, per lavorare direttamente sui LINQ to SQL in una classe controller crea problemi quando è necessario compilare un'applicazione più complessa.

Utilizzo di LINQ to SQL all'interno di una classe controller rende difficile passare tecnologie di accesso ai dati in futuro. Ad esempio, si potrebbe decidere di passa dall'utilizzo di Microsoft LINQ to SQL all'utilizzo di Entity Framework Microsoft come la tecnologia di accesso ai dati. In tal caso, sarà necessario riscrivere tutti i controller che accede al database all'interno dell'applicazione.

Utilizzo di LINQ to SQL all'interno di una classe controller anche rende difficile generare unit test per l'applicazione. In genere, non si desidera interagire con un database durante l'esecuzione di unit test. Si desidera utilizzare gli unit test per testare la logica dell'applicazione e non il server di database.

Per compilare un'applicazione MVC che modifica più adattabile alle future e che può essere testato più facilmente, è consigliabile utilizzare il modello di Repository. Quando si utilizza il modello di Repository, crei una classe separata repository che contiene tutti della logica di accesso del database.

Quando si crea la classe di repository, verrà creata un'interfaccia che rappresenta tutti i metodi usati dalla classe di repository. All'interno di un controller di scrivere il codice all'interfaccia anziché il repository. In questo modo, è possibile implementare il repository utilizzando tecnologie di accesso ai dati diversi in futuro.

L'interfaccia listato 3 denominato IMovieRepository e rappresenta un singolo metodo denominato ListAll().

**Elenco di 3:`Models\IMovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample3.vb)]

La classe repository listato 4 implementa l'interfaccia IMovieRepository. Si noti che contenga un metodo denominato ListAll() che corrisponde al metodo richiesto dall'interfaccia IMovieRepository.

**Elenco di 4:`Models\MovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample4.vb)]

Infine, la classe MoviesController listato 5 utilizza il modello di Repository. Non è più Usa LINQ alle classi SQL direttamente.

**Elenco di 5:`Controllers\MoviesController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample5.vb)]

Si noti che la classe MoviesController listato 5 dispone di due costruttori. Il primo costruttore, il costruttore senza parametri, viene chiamato quando l'applicazione è in esecuzione. Questo costruttore crea un'istanza della classe MovieRepository e lo passa al secondo costruttore.

Il secondo costruttore con un solo parametro: un parametro IMovieRepository. Questo costruttore assegna semplicemente il valore del parametro a un campo a livello di classe denominato \_repository.

La classe MoviesController è sfruttando la possibilità di un modello di progettazione software denominato modello di inserimento di dipendenze. In particolare, è usando un costruttore Dependency Injection. Altre informazioni su questo modello, leggere l'articolo seguente di Martin Fowler:

[http://martinfowler.com/articles/injection.HTML](http://martinfowler.com/articles/injection.html)

Si noti che tutto il codice nella classe MoviesController (fatta eccezione per il primo costruttore) interagisce con l'interfaccia IMovieRepository anziché la classe MovieRepository effettiva. Il codice interagisce con un'interfaccia astratta anziché un'implementazione concreta dell'interfaccia.

Se si desidera modificare la tecnologia di accesso ai dati utilizzata dall'applicazione è semplicemente possibile implementare l'interfaccia IMovieRepository con una classe che utilizza la tecnologia di accesso del database alternativo. Ad esempio, è possibile creare una classe EntityFrameworkMovieRepository o SubSonicMovieRepository. Poiché la classe controller programmata all'interfaccia, è possibile passare una nuova implementazione di IMovieRepository alla classe controller e la classe continui a funzionare.

Inoltre, se si desidera testare la classe MoviesController, è possibile passare una classe di repository fittizio film per il MoviesController. È possibile implementare la classe IMovieRepository con una classe che non è effettivamente accedere al database ma contiene tutti i metodi dell'interfaccia IMovieRepository obbligatori. In questo modo, è possibile unit test di classe MoviesController senza effettivamente l'accesso a un database reale.

## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione è stata per illustrare come è possibile creare le classi del modello MVC sfruttando Microsoft LINQ to SQL. Sono state presentate due strategie per la visualizzazione dei dati del database in un'applicazione MVC ASP.NET. In primo luogo, è creato LINQ alle classi di SQL e le classi utilizzate direttamente all'interno di un'azione del controller. Utilizzo di LINQ alle classi SQL all'interno di un controller permette di rapidamente e facilmente visualizzare dati di database in un'applicazione MVC.

Successivamente, è esplorare un percorso più complessa, ma è sicuramente più virtuoso, per la visualizzazione dei dati del database. Verrà sfruttato il modello di Repository e inserito tutta la logica di accesso ai database in una classe separata del repository. In questo controller, è stato scritto tutto il codice in un'interfaccia anziché una classe concreta. Il vantaggio del modello di Repository è che consente di modificare con facilità in futuro tecnologie di accesso ai database e consente di testare facilmente il nostro classi controller.

>[!div class="step-by-step"]
[Precedente](creating-model-classes-with-the-entity-framework-vb.md)
[Successivo](displaying-a-table-of-database-data-vb.md)
