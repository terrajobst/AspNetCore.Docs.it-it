---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
title: Creare un'applicazione di Database di film in 15 minuti con ASP.NET MVC (c#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther compila un'intera basate su database applicazione ASP.NET MVC dall'inizio alla fine. Questa esercitazione è un'ottima introduzione per gli utenti che sono nuove t...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: dd1be137-91c5-47a8-8137-fecf0789c7f5
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
msc.type: authoredcontent
ms.openlocfilehash: 16d62102713b0b86aa93284e39c82d3ae492f892
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831309"
---
<a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-c"></a>Creare un'applicazione di Database di film in 15 minuti con ASP.NET MVC (c#)
====================
da [Stephen Walther](https://github.com/StephenWalther)

[Scaricare il codice](http://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_CS.zip)

> Stephen Walther compila un'intera basate su database applicazione ASP.NET MVC dall'inizio alla fine. Questa esercitazione è un'ottima introduzione per gli utenti che hanno familiarità con il Framework di MVC ASP.NET e che desiderano avere un'idea del processo di compilazione di un'applicazione ASP.NET MVC.


Lo scopo di questa esercitazione consiste nel fornire un senso di "che cos'è, ad esempio" per compilare un'applicazione ASP.NET MVC. In questa esercitazione, avviare la creazione di un'intera applicazione ASP.NET MVC dall'inizio alla fine. Mostrerò come creare una semplice applicazione basata su database che illustra come è possibile elencare, creare e modificare i record di database.

Per semplificare il processo di compilazione dell'applicazione, prenderemo in sfruttare le funzionalità di scaffolding di Visual Studio 2008. Lo comunicheremo tramite Visual Studio genera il codice iniziale e il contenuto per il controller, modelli e visualizzazioni.

Se l'utente abbia familiarità con le pagine ASP o ASP.NET, quindi è necessario trovare ASP.NET MVC molto familiare. Le visualizzazioni ASP.NET MVC sono molto simile a quello di pagine in un'applicazione Active Server Pages. E, proprio come un'applicazione Web Form ASP.NET tradizionale, ASP.NET MVC offre accesso completo per la vasta gamma di linguaggi e le classi fornite da .NET framework.

Spero è che questa esercitazione fornirà un'idea del modo in cui l'esperienza di creazione di un'applicazione ASP.NET MVC è diverso da quello l'esperienza di creazione di un'applicazione ASP o ASP.NET Web Form e simili.

## <a name="overview-of-the-movie-database-application"></a>Panoramica dell'applicazione di Database di film

Poiché il nostro obiettivo consiste nel semplificare la procedura, verrà compilata un'applicazione di Database di film molto semplice. Questa semplice applicazione di Database di film ci consentirà di eseguire tre operazioni:

1. Elenca una serie di record di database di film
2. Creare un nuovo record di database di film
3. Modificare un record di database esistenti filmato

Anche in questo caso, poiché si vuole semplificare le operazioni, Daremo sfruttare il numero minimo di funzionalità del framework ASP.NET MVC necessari per compilare l'applicazione. Ad esempio, abbiamo non usufruire di sviluppo basato su test.

Per creare l'applicazione, è necessario completare i passaggi seguenti:

1. Creare il progetto di applicazione Web ASP.NET MVC
2. Creare il database
3. Creare il modello di database
4. Creare il controller ASP.NET MVC
5. Creare visualizzazioni ASP.NET MVC

## <a name="preliminaries"></a>Operazioni preliminari

È necessario Visual Studio 2008 o Visual Web Developer 2008 Express per compilare un'applicazione ASP.NET MVC. È inoltre necessario scaricare il framework ASP.NET MVC.

Se non si è proprietari di Visual Studio 2008, è possibile scaricare una versione di valutazione di 90 giorni di Visual Studio 2008 da un sito Web:

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

In alternativa, è possibile creare ASP.NET MVC applicazioni con Visual Web Developer Express 2008. Se si decide di utilizzare Visual Web Developer Express è necessario Service Pack 1 installato. È possibile scaricare Visual Web Developer 2008 Express con Service Pack 1 da un sito Web:

[https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp; displaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

Dopo aver installato Visual Studio 2008 o Visual Web Developer 2008, è necessario installare il framework ASP.NET MVC. Il framework ASP.NET MVC è possibile scaricare dal sito Web seguente:

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> Invece di scaricare il framework ASP.NET e framework di MVC ASP.NET singolarmente, è possibile sfruttare l'installazione guidata piattaforma Web. L'installazione guidata piattaforma Web è un'applicazione che consente di gestire facilmente le applicazioni installate sono nel computer:
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)


## <a name="creating-an-aspnet-mvc-web-application-project"></a>Creazione di un progetto di applicazione Web ASP.NET MVC

Iniziamo creando un nuovo progetto di applicazione Web ASP.NET MVC in Visual Studio 2008. Selezionare l'opzione di menu **File, nuovo progetto** si noterà la finestra di dialogo Nuovo progetto nella figura 1. Selezionare c# come linguaggio di programmazione e selezionare il modello di progetto applicazione Web ASP.NET MVC. Assegnare al progetto il nome MovieApp e fare clic sul pulsante OK.


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.png)

**Figura 01**: finestra di dialogo Nuovo progetto di ([fare clic per visualizzare l'immagine con dimensioni normali](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.png))


Assicurarsi che si seleziona .NET Framework 3.5 nell'elenco a discesa nella parte superiore della finestra di dialogo Nuovo progetto o il modello di progetto applicazione Web ASP.NET MVC non sarà più visualizzato.


Quando si crea un nuovo progetto di applicazione Web MVC, Visual Studio chiederà di creare un progetto di test unità separata. Viene visualizzata la finestra di dialogo nella figura 2. Perché non verranno creati i test in questa esercitazione a causa di vincoli di tempo (e, Sì, siamo dovrebbe essere un po' colpevole proposito) selezionare il **No** opzione e fare clic sui **OK** pulsante.

> [!NOTE] 
> 
> Visual Web Developer non supporta progetti di test.


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.png)

**Figura 02**: finestra di dialogo di creazione progetto Unit Test ([fare clic per visualizzare l'immagine con dimensioni normali](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.png))


Un'applicazione ASP.NET MVC include un set standard di cartelle: una cartella modelli, visualizzazioni e controller. È possibile visualizzare questo set standard di cartelle in Esplora soluzioni. È necessario aggiungere file a ogni cartella modelli, visualizzazioni e controller per compilare l'applicazione di Database di film.

Quando si crea una nuova applicazione MVC con Visual Studio, si ottiene un'applicazione di esempio. Poiché si vuole iniziare da zero, è necessario eliminare il contenuto per questa applicazione di esempio. È necessario eliminare il file seguente e la cartella seguente:

- Controllers\HomeController.cs
- Views\Home

## <a name="creating-the-database"></a>Creazione del Database

È necessario creare un database per contenere il record di database di film. Per fortuna, Visual Studio include un database gratuito denominato SQL Server Express. Seguire questi passaggi per creare il database:

1. Fare doppio clic su App\_cartella di dati nella finestra Esplora soluzioni e selezionare l'opzione di menu **Aggiungi, elemento nuovo**.
2. Selezionare il **Data** categoria e selezionare il **Database SQL Server** modello (vedere la figura 3).
3. Denominare il nuovo database *MoviesDB.mdf* e fare clic sui **Add** pulsante.

Dopo aver creato il database, è possibile connettersi al database facendo doppio clic sul file MoviesDB.mdf situato nell'App\_cartella dati. Doppio clic sul file MoviesDB.mdf apre la finestra di Esplora Server.

> [!NOTE] 
> 
> La finestra Esplora Server è denominata la finestra Esplora Database nel caso di Visual Web Developer.


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.png)

**Figura 03**: la creazione di un Database Microsoft SQL Server ([fare clic per visualizzare l'immagine con dimensioni normali](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.png))


Successivamente, è necessario creare una nuova tabella di database. All'interno della finestra di Esplora server, fare clic sulla cartella di tabelle e selezionare l'opzione di menu **Aggiungi nuova tabella**. Selezionando questa opzione di menu consente di aprire la finestra di progettazione di tabelle di database. Creare le colonne di database seguenti:

<a id="0.1_table01"></a>


| **Nome della colonna** | **Tipo di dati** | **Consenti valori null** |
| --- | --- | --- |
| Id | Int | False |
| Titolo | Nvarchar(100) | False |
| Director | Nvarchar(100) | False |
| DateReleased | DateTime | False |


La prima colonna, la colonna Id, ha due proprietà speciali. In primo luogo, è necessario contrassegnare la colonna Id come colonna chiave primaria. Dopo aver selezionato la colonna Id, scegliere il **Imposta chiave primaria** pulsante (si trova l'icona simile a una chiave). In secondo luogo, è necessario contrassegnare la colonna Id come una colonna Identity. Nella finestra proprietà di colonna, scorrere fino alla sezione specifica Identity ed espanderlo. Modifica il **Identity** il valore della proprietà **Yes**. Al termine, la tabella dovrebbe essere mostrato nella figura 4.


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.png)

**Figura 04**: tabella di database di film ([fare clic per visualizzare l'immagine con dimensioni normali](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.png))


Il passaggio finale consiste per salvare la nuova tabella. Fare clic sul pulsante Save (l'icona di unità disco floppy) e assegnare alla nuova tabella il nome di film.

Dopo aver completato la creazione della tabella, aggiungere alcuni record di film alla tabella. Fare doppio clic nella tabella di film nella finestra di Esplora Server e selezionare l'opzione di menu **Mostra dati tabella**. Immettere un elenco di filmati preferiti (vedere la figura 5).


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.png)

**Figura 05**: immettere i record di film ([fare clic per visualizzare l'immagine con dimensioni normali](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.png))


## <a name="creating-the-model"></a>Creazione del modello

È quindi necessario creare un set di classi per rappresentare il database. È necessario creare un modello di database. Daremo i vantaggi di Microsoft Entity Framework per generare automaticamente le classi per il modello di database.

> [!NOTE] 
> 
> Il framework ASP.NET MVC non è associato a Microsoft Entity Framework. È possibile creare le classi del modello del database, sfruttando i vantaggi di una varietà di Mapping relazionale a oggetti (o / M) strumenti, tra cui LINQ to SQL, Subsonic e NHibernate.


Seguire questi passaggi per avviare la procedura guidata Entity Data Model:

1. Fare doppio clic su cartella Models nella finestra Esplora soluzioni e selezionare l'opzione di menu **Aggiungi, elemento nuovo**.
2. Selezionare il **Data** categoria e selezionare il **ADO.NET Entity Data Model** modello.
3. Denominare il modello di dati *MoviesDBModel.edmx* e fare clic sui **Add** pulsante.

Dopo aver fatto clic sul pulsante Aggiungi, viene visualizzata la procedura guidata Entity Data Model (vedere la figura 6). Seguire questi passaggi per completare la procedura guidata:

1. Nel **Scegli contenuto Model** passaggio, seleziona la **genera da database** opzione.
2. Nel **Seleziona connessione dati** istruzioni, utilizzare il *MoviesDB.mdf* connessione dati e il nome *MoviesDBEntities* per le impostazioni di connessione. Scegliere il **successivo** pulsante.
3. Nel **Scegli oggetti di Database** passaggio, espandere il nodo tabelle, selezionare la tabella di film. Immettere lo spazio dei nomi *MovieApp.Models* e fare clic sui **fine** pulsante.


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.png)

**Figura 06**: generazione di un modello di database con la procedura guidata Entity Data Model ([fare clic per visualizzare l'immagine con dimensioni normali](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.png))


Dopo aver completato la procedura guidata Entity Data Model, verrà visualizzata la finestra di Entity Data Model Designer. La finestra di progettazione deve visualizzare la tabella di database di film (vedere la figura 7).


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.png)

**Figura 07**: The Entity Data Model Designer ([fare clic per visualizzare l'immagine con dimensioni normali](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.png))


È necessario apportare una modifica prima di continuare. La procedura guidata Entity Data genera una classe di modello denominata *film* che rappresenta la tabella di database di film. Poiché la classe di film verrà usato per rappresentare un filmato specifico, è necessario modificare il nome della classe sia *film* invece di *film* (singolare anziché plurale).

Fare doppio clic sul nome della classe nell'area di progettazione e modificare il nome della classe da film al film. Dopo aver apportato questa modifica, scegliere il **salvare** pulsante (l'icona del disco floppy) per generare la classe di film.

## <a name="creating-the-aspnet-mvc-controller"></a>Creazione del Controller ASP.NET MVC

Il passaggio successivo consiste nel creare il controller MVC ASP.NET. Un controller è responsabile del controllo come un utente interagisce con un'applicazione ASP.NET MVC.

Attenersi ai passaggi riportati di seguito.

1. Nella finestra Esplora soluzioni fare doppio clic su cartella controller e selezionare l'opzione di menu **Controller, Aggiungi**.
2. Nella finestra di dialogo Aggiungi Controller, immettere il nome *HomeController* e selezionare la casella di controllo etichettato **aggiungere metodi di azione per gli scenari Create, Update e dettagli** (vedere la figura 8).
3. Scegliere il **Add** pulsante per aggiungere il nuovo controller al progetto.

Dopo aver completato questi passaggi, viene creato il controller nel listato 1. Si noti che contiene i metodi denominati indice, informazioni dettagliate, creazione e modificare. Nelle sezioni seguenti, si aggiungerà il codice necessario per ottenere questi metodi per l'uso.


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image15.png)

**Figura 08**: aggiunge un nuovo Controller MVC ASP.NET ([fare clic per visualizzare l'immagine con dimensioni normali](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image16.png))


**Listato 1 – controllers\homecontroller.cs.**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample1.cs)]

## <a name="listing-database-records"></a>Elenco record di Database

Il metodo Index () del controller Home è il metodo predefinito per un'applicazione ASP.NET MVC. Quando si esegue un'applicazione ASP.NET MVC, il metodo Index () è il primo metodo di controller che viene chiamato.

Si userà il metodo Index () per visualizzare l'elenco di record dalla tabella di database di film. Daremo i vantaggi del database di classi di modelli creati in precedenza per recuperare i record di database di film con il metodo Index ().

Ho modificato la classe HomeController nel listato 2 in modo che contenga un nuovo campo privato denominato \_db. La classe MoviesDBEntities rappresenta il modello di database e si userà questa classe per comunicare con il database.

Ho anche modificato il metodo Index () nel listato 2. Il metodo Index () utilizza la classe MoviesDBEntities per recuperare tutti i record di film dalla tabella di database di film. L'espressione  *\_db. MovieSet.ToList()* restituisce un elenco di tutti i record di film dalla tabella di database di film.

L'elenco di film viene passato alla visualizzazione. Come visualizzare i dati, tutto ciò che viene passato al metodo View() viene passato alla visualizzazione.

**Listato 2 – HomeController (metodo indice modificato)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample2.cs)]

Il metodo Index () restituisce una visualizzazione denominata indice. È necessario creare questa vista per visualizzare l'elenco di record di database di film. Attenersi ai passaggi riportati di seguito.


È necessario compilare il progetto (selezionare l'opzione di menu **di compilazione, Compila soluzione**) prima di aprire il **Aggiungi visualizzazione** verranno visualizzato nella finestra di dialogo o senza classi il **visualizzare dati classe** Nell'elenco a discesa.


1. Il metodo Index () nell'editor del codice e scegliere l'opzione di menu **Aggiungi visualizzazione** (vedere la figura 9).
2. Nella finestra di dialogo Aggiungi visualizzazione, verificare che la casella di controllo etichettato **creare una visualizzazione fortemente tipizzata** sia selezionata.
3. Dal **visualizzare il contenuto** elenco a discesa selezionare il valore *elenco*.
4. Dal **visualizzare i dati classe** elenco a discesa selezionare il valore *MovieApp.Models.Movie*.
5. Fare clic sul pulsante Aggiungi per creare la nuova visualizzazione (vedere la figura 10).

Dopo aver completato questi passaggi, una visualizzazione nuova denominata index. aspx viene aggiunto alla cartella Views\Home. Il contenuto della visualizzazione Index è incluse nel listato 3.


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image17.png)

**Figura 09**: aggiunta di una vista da un'azione del controller ([fare clic per visualizzare l'immagine con dimensioni normali](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image18.png))


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image19.png)

**Figura 10**: creazione di una nuova vista con la finestra di dialogo Aggiungi visualizzazione ([fare clic per visualizzare l'immagine con dimensioni normali](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image20.png))


**Listato 3 – Views\Home\Index.aspx.**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample3.aspx)]

La visualizzazione dell'indice consente di visualizzare tutti i record di film dalla tabella di database di film all'interno di una tabella HTML. La vista contiene un ciclo foreach che scorre ogni film rappresentato dalla proprietà ViewData.Model. Se si esegue l'applicazione premendo il tasto F5, è possibile vedere la pagina web nella figura 11.


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image21.png)

**Figura 11**: Visualizza l'indice ([fare clic per visualizzare l'immagine con dimensioni normali](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image22.png))


## <a name="creating-new-database-records"></a>Creazione di nuovi record di Database

La visualizzazione dell'indice che abbiamo creato nella sezione precedente include un collegamento per la creazione di nuovi record di database. È possibile procedere e implementare la logica e creare la vista necessari per la creazione di nuovi record di database di film.

Il controller Home contiene due metodi denominati create (). Il primo metodo Create () non ha parametri. Questo overload del metodo Create () viene utilizzato per visualizzare il form HTML per la creazione di un nuovo record di database di film.

Il secondo metodo Create () ha un parametro FormCollection. Questo overload del metodo Create () viene chiamato quando il modulo HTML per la creazione di un nuovo film viene inviato al server. Si noti che questo secondo metodo Create () ha un attributo AcceptVerbs che impedisce che il metodo viene chiamato a meno che non viene eseguita un'operazione HTTP POST.

Questo secondo metodo Create () è stato modificato nella classe HomeController aggiornata nel listato 4. La nuova versione del metodo Create () accetta un parametro di film e contiene la logica per l'inserimento di un nuovo film nella tabella di database di film.

> [!NOTE] 
> 
> Si noti che l'attributo di associazione. Perché non si desidera aggiornare la proprietà Id del film da form HTML, è necessario escludere in modo esplicito questa proprietà.


**Listato 4 – Controllers\HomeController.cs (metodo Create modificato)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample4.cs)]

Visual Studio è facile creare il form per la creazione di un nuovo database di film di record (vedere la figura 12). Attenersi ai passaggi riportati di seguito.

1. Il metodo Create () nell'editor del codice e scegliere l'opzione di menu **Aggiungi visualizzazione**.
2. Verificare che la casella di controllo etichettato **creare una visualizzazione fortemente tipizzata** sia selezionata.
3. Dal **visualizzare il contenuto** elenco a discesa selezionare il valore *crea*.
4. Dal **visualizzare i dati classe** elenco a discesa selezionare il valore *MovieApp.Models.Movie*.
5. Scegliere il **Add** pulsante per creare la nuova vista.


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image23.png)

**Figura 12**: aggiunta della visualizzazione di creazione ([fare clic per visualizzare l'immagine con dimensioni normali](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image24.png))


Visual Studio genera automaticamente la visualizzazione nel listato 5. Questa vista contiene un form HTML che include i campi corrispondenti a ognuna delle proprietà della classe di film.

**Listato 5 – Views\Home\Create.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample5.aspx)]

> [!NOTE] 
> 
> Il form HTML generato dalla finestra di dialogo Aggiungi visualizzazione genera un campo di formato di Id. Poiché la colonna Id è una colonna Identity, non abbiamo bisogno di questo campo del form ed è possibile rimuoverla.


Dopo aver aggiunto la visualizzazione di creazione, è possibile aggiungere nuovi record di film nel database. Eseguire l'applicazione premendo il tasto F5 e fare clic su Crea nuovo collegamento per visualizzare il form nella figura 13. Se si completano e inviare il modulo, viene creato un nuovo record di database di film.

Si noti che si ottiene automaticamente la convalida dei form. Se non si assegnano a immettere una data di rilascio per un film o se si immette una data di rilascio non è valido, quindi viene nuovamente visualizzata la forma e viene evidenziato il campo della data di rilascio.


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image25.png)

**Figura 13**: la creazione di un nuovo record di database di film ([fare clic per visualizzare l'immagine con dimensioni normali](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image26.png))


## <a name="editing-existing-database-records"></a>Modifica di record di Database esistenti

Nelle sezioni precedenti, abbiamo parlato di come è possibile elencare e creare nuovi record di database. In questa sezione finale illustra come è possibile modificare i record di database esistenti.

In primo luogo, è necessario generare il modulo di modifica. Questo passaggio è semplice perché Visual Studio genera il modulo di modifica per noi automaticamente. Aprire la classe HomeController.cs nell'editor di codice di Visual Studio e seguire questa procedura:

1. Il metodo Edit () nell'editor del codice e scegliere l'opzione di menu **Aggiungi visualizzazione** (vedere la figura 14).
2. Selezionare la casella di controllo etichettato **creare una visualizzazione fortemente tipizzata**.
3. Dal **visualizzare il contenuto** elenco a discesa selezionare il valore *modificare*.
4. Dal **visualizzare i dati classe** elenco a discesa selezionare il valore *MovieApp.Models.Movie*.
5. Scegliere il **Add** pulsante per creare la nuova vista.

Completare la procedura aggiunge una nuova vista denominata Edit. aspx nella cartella Views\Home. Questa vista contiene un form HTML per la modifica di un record di film.


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image27.png)

**Figura 14**: aggiunta della visualizzazione di modifica ([fare clic per visualizzare l'immagine con dimensioni normali](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image28.png))


> [!NOTE] 
> 
> Visualizzazione di modifica contiene un campo di form HTML che corrisponde alla proprietà Id del film. Poiché è possibile impedire la modifica del valore della proprietà Id, è necessario rimuovere il campo del form.


Infine, è necessario modificare il controller Home, in modo che supporta la modifica di un record di database. La classe HomeController aggiornata è contenuta nel listato 6.

**Listato 6 – Controllers\HomeController.cs (metodi Edit)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample6.cs)]

Nel listato 6, è stata aggiunta logica aggiuntiva per entrambi gli overload del metodo Edit (). Il primo metodo Edit () restituisce il record di database di film che corrisponde al parametro Id passato al metodo. Il secondo overload esegue gli aggiornamenti a un record di film nel database.

Si noti che è necessario recuperare i film originale e quindi chiamare ApplyPropertyChanges(), per aggiornare il film nel database esistente.

## <a name="summary"></a>Riepilogo

Lo scopo di questa esercitazione è stata per avere un'idea dell'esperienza di creazione di un'applicazione ASP.NET MVC. Mi auguro che sia stato individuato che la compilazione di applicazione web MVC ASP.NET è molto simile all'esperienza di creazione di un'applicazione ASP o ASP.NET.

In questa esercitazione, sono esaminate solo le funzionalità di base del framework ASP.NET MVC. Nelle esercitazioni successive, è approfondire argomenti quali controller, azioni del controller, visualizzazioni, visualizzare i dati e gli helper HTML.

> [!div class="step-by-step"]
> [avanti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb.md)
