---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
title: Creare un'applicazione di Database film in 15 minuti con MVC ASP.NET (VB) | Documenti Microsoft
author: StephenWalther
description: "Stephen Walther compila un'intera basato su database applicazione MVC ASP.NET dall'inizio completamento. In questa esercitazione è un ottimo inizio per gli utenti che sono nuovi t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: e4ba9786-734c-4eb3-91bb-089793325d0d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
msc.type: authoredcontent
ms.openlocfilehash: 4dbb3804bbb0ccb80506a592f1efb585c5748c2f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-vb"></a>Creare un'applicazione di Database film in 15 minuti con MVC ASP.NET (VB)
====================
da [Stephen Walther](https://github.com/StephenWalther)

[Scaricare il codice](http://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_VB.zip)

> Stephen Walther compila un'intera basato su database applicazione MVC ASP.NET dall'inizio completamento. In questa esercitazione è un ottimo inizio per gli utenti che hanno familiarità con Framework di MVC ASP.NET e che desiderano avere un'idea del processo di compilazione di un'applicazione ASP.NET MVC.


Lo scopo di questa esercitazione consiste nell'avere un'idea di "che cos'è ad esempio" per compilare un'applicazione ASP.NET MVC. In questa esercitazione avviare la creazione di un'intera applicazione MVC ASP.NET dall'inizio completamento. Illustrano come compilare un'applicazione semplice basata su database che illustra come è possibile elencare, creare e modificare i record del database.

Per semplificare il processo di compilazione dell'applicazione, l'utente verrà reindirizzato a sfruttare le funzionalità di scaffolding di Visual Studio 2008. Invieremo Visual Studio genera il codice iniziale e il contenuto per il controller, modelli e visualizzazioni.

Ha familiarità con le pagine ASP o ASP.NET, quindi è necessario trovare ASP.NET MVC molto familiare. Le visualizzazioni ASP.NET MVC sono molto simile a quello di pagine in un'applicazione Active Server Pages. E, analogamente a un'applicazione Web Form ASP.NET tradizionale, ASP.NET MVC fornisce l'accesso completo alla vasta gamma di linguaggi e le classi fornite da .NET framework.

Spero è che in questa esercitazione verrà avere un'idea di come l'esperienza di creazione di un'applicazione ASP.NET MVC è diverso da quello di compilazione di un'applicazione Active Server Pages o Web Form ASP.NET e simili.

## <a name="overview-of-the-movie-database-application"></a>Panoramica dell'applicazione di Database film

Poiché l'obiettivo è semplificare le operazioni, verrà creata un'applicazione di film Database molto semplice. Questa semplice applicazione di Database di film permetterà di eseguire tre operazioni:

1. Elenco di un set di record del database film
2. Creare un nuovo record di database di film
3. Modificare un record di database esistente del film

Nuovamente, poiché si desidera semplificare le operazioni, l'utente verrà reindirizzato sfruttare il numero minimo di funzionalità del framework ASP.NET MVC necessari per compilare l'applicazione. Ad esempio, è non possibile sfruttare lo sviluppo.

Per creare l'applicazione, è necessario completare i passaggi seguenti:

1. Creare il progetto di applicazione Web ASP.NET MVC
2. Creare il database
3. Creare il modello di database
4. Creare il controller MVC ASP.NET
5. Creazione di visualizzazioni MVC ASP.NET

## <a name="preliminaries"></a>Operazioni preliminari

È necessario Visual Studio 2008 o Visual Web Developer 2008 Express per compilare un'applicazione ASP.NET MVC. È inoltre necessario scaricare il framework di MVC ASP.NET.

Se non si dispone di Visual Studio 2008, è possibile scaricare una versione di valutazione di 90 giorni di Visual Studio 2008 da un sito Web:

[https://msdn.microsoft.com/en-us/VS2008/Products/cc268305.aspx](https://msdn.microsoft.com/en-us/vs2008/products/cc268305.aspx)

In alternativa, è possibile creare ASP.NET MVC applicazioni con Visual Web Developer Express 2008. Se si decide di utilizzare Visual Web Developer Express è necessario Service Pack 1 installato. È possibile scaricare Visual Web Developer 2008 Express con Service Pack 1 da un sito Web:

[https://www.microsoft.com/downloads/details.aspx?FamilyID=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

Dopo aver installato Visual Studio 2008 o Visual Web Developer 2008, è necessario installare il framework di MVC ASP.NET. Il framework di MVC ASP.NET è possibile scaricare dal sito Web seguente:

[https://www.ASP.NET/MVC/](../../../index.md)

> [!NOTE] 
> 
> Anziché essere scaricato singolarmente il framework ASP.NET e framework di MVC ASP.NET, è possibile sfruttare l'installazione guidata piattaforma Web. L'installazione guidata piattaforma Web è un'applicazione che consente di gestire facilmente le applicazioni installate nel computer:
> 
> [https://www.microsoft.com/Web/Gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)


## <a name="creating-an-aspnet-mvc-web-application-project"></a>Creazione di un progetto di applicazione Web ASP.NET MVC

Iniziamo creando un nuovo progetto applicazione Web ASP.NET MVC in Visual Studio 2008. Selezionare l'opzione di menu **File, nuovo progetto** per visualizzare la finestra di dialogo Nuovo progetto nella figura 1. Selezionare Visual Basic come linguaggio di programmazione e selezionare il modello di progetto applicazione Web ASP.NET MVC. Assegnare al progetto il nome MovieApp e fare clic sul pulsante OK.


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.png)

**Figura 01**: la finestra di dialogo Nuovo progetto il ([fare clic per visualizzare l'immagine ingrandita](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.png))


Assicurarsi di selezionare .NET Framework 3.5 nell'elenco a discesa nella parte superiore della finestra di dialogo Nuovo progetto o il modello di progetto applicazione Web MVC ASP.NET non sarà più visualizzato.


Quando si crea un nuovo progetto applicazione Web MVC, Visual Studio viene richiesto di creare un progetto di test unità separata. Verrà visualizzata la finestra di dialogo nella figura 2. Perché è non essere la creazione di test in questa esercitazione a causa dei limiti di tempo (e, Sì, dovremmo riteniamo leggermente su questo può accadere) selezionare il **n** opzione e fare clic su di **OK** pulsante.

> [!NOTE] 
> 
> Visual Web Developer non supporta i progetti di test.


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.png)

**Figura 02**: finestra di dialogo di creazione progetto Unit Test ([fare clic per visualizzare l'immagine ingrandita](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.png))


Un'applicazione MVC ASP.NET include un set standard di cartelle: una cartella di modelli, visualizzazioni e controller. È possibile visualizzare questo set standard di cartelle nella finestra Esplora soluzioni. È necessario aggiungere i file per ciascuna delle cartelle di modelli, visualizzazioni e controller per compilare l'applicazione di Database di film.

Quando si crea una nuova applicazione MVC con Visual Studio, si ottiene un'applicazione di esempio. Poiché si desidera iniziare da zero, è necessario eliminare il contenuto per questa applicazione di esempio. È necessario eliminare il file seguente e la cartella seguente:

- Controllers\HomeController.vb
- Views\Home

## <a name="creating-the-database"></a>Creazione del Database

È necessario creare un database per contenere il record del database film. Per fortuna, Visual Studio include un database gratuito denominato SQL Server Express. Seguire questi passaggi per creare il database:

1. Fare doppio clic su App\_cartella di dati nella finestra Esplora soluzioni e selezionare l'opzione di menu **Aggiungi, elemento nuovo**.
2. Selezionare il **dati** categoria e selezionare il **Database di SQL Server** modello (vedere la figura 3).
3. Nome del nuovo database *MoviesDB.mdf* e fare clic su di **Aggiungi** pulsante.

Dopo aver creato il database, è possibile connettersi al database facendo doppio clic sul file di MoviesDB.mdf situato nell'App\_cartella dati. Doppio clic sul file MoviesDB.mdf apre la finestra di Esplora Server.

> [!NOTE] 
> 
> La finestra di Esplora Server è denominata la finestra Esplora Database nel caso di Visual Web Developer.


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.png)

**Figura 03**: creazione di un Database di Microsoft SQL Server ([fare clic per visualizzare l'immagine ingrandita](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.png))


Successivamente, è necessario creare una nuova tabella di database. All'interno della finestra di Esplora server, fare doppio clic sulla cartella tabelle e scegliere l'opzione di menu **Aggiungi nuova tabella**. Selezionando questa opzione di menu, viene visualizzata la progettazione di tabelle di database. Creare le colonne di database seguenti:

<a id="0.2_table01"></a>


| **Nome colonna** | **Tipo di dati** | **Consenti valori null** |
| --- | --- | --- |
| Id | Int | False |
| Titolo | Nvarchar (100) | False |
| Director | Nvarchar (100) | False |
| DateReleased | DateTime | False |


La prima colonna, la colonna Id, ha due proprietà speciali. In primo luogo, è necessario contrassegnare la colonna Id come colonna chiave primaria. Dopo aver selezionato la colonna Id, fare clic su di **Imposta chiave primaria** pulsante (si trova l'icona simile a una chiave). In secondo luogo, è necessario contrassegnare la colonna Id come una colonna Identity. Nella finestra proprietà di colonna, scorrere fino alla sezione specifica identità ed espanderlo. Modifica il **identità** il valore della proprietà **Sì**. Al termine, la tabella dovrebbe essere mostrato nella figura 4.


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.png)

**Figura 04**: tabella di database di filmati ([fare clic per visualizzare l'immagine ingrandita](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.png))


Il passaggio finale consiste per salvare la nuova tabella. Fare clic sul pulsante Salva (l'icona dell'unità disco floppy) e assegnare alla nuova tabella il nome di film.

Dopo aver completato la creazione della tabella, aggiungere alcuni record di film alla tabella. La tabella di filmati nella finestra di Esplora Server e scegliere l'opzione di menu **Mostra dati tabella**. Immettere un elenco di filmati preferiti (vedere Figura 5).


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.png)

**Figura 05**: immissione record filmato ([fare clic per visualizzare l'immagine ingrandita](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.png))


## <a name="creating-the-model"></a>Creazione del modello

Successivamente, è necessario creare un set di classi che rappresentano il database. È necessario creare un modello di database. L'utente verrà reindirizzato sfruttare Microsoft Entity Framework per generare automaticamente le classi per il modello di database.

> [!NOTE] 
> 
> Il framework di MVC ASP.NET non è correlato a Microsoft Entity Framework. È possibile creare classi di modello del database che possono sfruttare un'ampia gamma di Mapping relazionale a oggetti (o / M) gli strumenti inclusi LINQ to SQL, Subsonic e NHibernate.


Seguire questi passaggi per avviare la procedura guidata Entity Data Model:

1. Fare clic sulla cartella modelli nella finestra Esplora soluzioni e selezionare l'opzione di menu **Aggiungi, elemento nuovo**.
2. Selezionare il **dati** categoria e selezionare il **ADO.NET Entity Data Model** modello.
3. Denominare il modello di dati *MoviesDBModel.edmx* e fare clic su di **Aggiungi** pulsante.

Dopo aver fatto clic sul pulsante Aggiungi viene visualizzata la procedura guidata Entity Data Model (vedere Figura 6). Seguire questi passaggi per completare la procedura guidata:

1. Nel **Scegli contenuto Model** passaggio, seleziona il **genera da database** opzione.
2. Nel **Seleziona connessione dati** di istruzioni, utilizzare il *MoviesDB.mdf* connessione dati e il nome *MoviesDBEntities* per le impostazioni di connessione. Fare clic su di **Avanti** pulsante.
3. Nel **Seleziona oggetti di Database** passaggio, espandere il nodo tabelle, selezionare la tabella di film. Immettere lo spazio dei nomi *MovieApp.Models* e fare clic su di **fine** pulsante.


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.png)

**Figura 06**: generazione di un modello di database con la procedura guidata Entity Data Model ([fare clic per visualizzare l'immagine ingrandita](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.png))


Dopo aver completato la procedura guidata Entity Data Model, verrà visualizzata la finestra di progettazione Entity Data Model. La finestra di progettazione deve essere visualizzato nella tabella di database di filmati (vedere la figura 7).


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.png)

**Figura 07**: la progettazione di Entity Data Model ([fare clic per visualizzare l'immagine ingrandita](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.png))


È necessario apportare una modifica prima di continuare. La procedura guidata Entity Data genera una classe di modello denominata filmati che rappresenta la tabella di database di film. Perché la classe di filmati verrà usato per rappresentare un filmato specifico, è necessario modificare il nome della classe da *film* anziché *filmati* (singolare anziché plurale).

Fare doppio clic sul nome della classe nell'area di progettazione e modificare il nome della classe di filmati in film. Dopo aver apportato questa modifica, fare clic su di **salvare** pulsante (icona del disco floppy) per generare la classe di film.

## <a name="creating-the-aspnet-mvc-controller"></a>Creazione di Controller MVC ASP.NET

Il passaggio successivo consiste nel creare il controller MVC ASP.NET. Un controller è responsabile del controllo come un utente interagisce con un'applicazione MVC ASP.NET.

Attenersi ai passaggi riportati di seguito.

1. Nella finestra Esplora soluzioni, fare clic sulla cartella controller e selezionare l'opzione di menu **Aggiungi, Controller**.
2. Nella finestra di dialogo Aggiungi Controller, immettere il nome *HomeController* e selezionare la casella di controllo con etichettata **aggiungere metodi di azione per gli scenari di creazione, aggiornamento e dettaglio** (vedere la figura 8).
3. Fare clic su di **Aggiungi** pulsante per aggiungere il nuovo controller al progetto.

Dopo aver completato questi passaggi, viene creato il controller nel listato 1. Si noti che contiene i metodi, denominati indice, i dettagli, creare, modificare. Nelle sezioni seguenti, verrà aggiunto il codice necessario per ottenere questi metodi per lavorare.


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image15.png)

**Figura 08**: aggiunge un nuovo Controller MVC ASP.NET ([fare clic per visualizzare l'immagine ingrandita](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image16.png))


**Elenco 1 – Controllers\HomeController.vb**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample1.vb)]

## <a name="listing-database-records"></a>Elenco di record del Database

Il metodo Index () del controller Home è il metodo predefinito per un'applicazione MVC ASP.NET. Quando si esegue un'applicazione ASP.NET MVC, il metodo di Index () è il primo metodo controller che viene chiamato.

Utilizzeremo il metodo Index () per visualizzare l'elenco di record dalla tabella di database film. L'utente verrà reindirizzato vantaggio del database di classi del modello creati in precedenza per recuperare i record del database film con il metodo Index ().

Ho modificato la classe HomeController listato 2 in modo che contenga un nuovo campo privato denominato \_db. La classe MoviesDBEntities rappresenta il modello di database e questa classe verrà utilizzata per comunicare con il database.

Ho modificato anche il metodo Index () nel listato 2. Il metodo Index () utilizza la classe MoviesDBEntities per recuperare tutti i record di film dalla tabella di database film. L'espressione  *\_db. MovieSet.ToList()* restituisce un elenco di tutti i record di film dalla tabella di database film.

L'elenco di film viene passato alla visualizzazione. Come visualizzare i dati, tutto ciò che viene passata al metodo View() viene passata alla visualizzazione.

**Elenco di 2 – Controllers/HomeController.vb (metodo indice modificato)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample2.vb)]

Il metodo Index () restituisce una visualizzazione denominata indice. È necessario creare questa vista per visualizzare l'elenco di record del database film. Attenersi ai passaggi riportati di seguito.


È necessario compilare il progetto (selezionare l'opzione di menu **di compilazione, Compila soluzione**) prima dell'apertura di **Aggiungi visualizzazione** finestra di dialogo o nessuna classe verranno visualizzato nel **visualizzare dati classe** elenco a discesa.


1. Il metodo Index () nell'editor di codice e scegliere l'opzione di menu **Aggiungi visualizzazione** (vedere Figura 9).
2. Nella finestra di dialogo Aggiungi visualizzazione, verificare che la casella di controllo con etichetta **creare una visualizzazione fortemente tipizzata** è selezionata.
3. Dal **visualizzare il contenuto** elenco a discesa selezionare il valore *elenco*.
4. Dal **visualizzare dati classe** elenco a discesa selezionare il valore *MovieApp.Movie*.
5. Fare clic sul pulsante Aggiungi per creare il nuovo (vedere la figura 10).

Dopo aver completato questi passaggi, nella cartella Views\Home viene aggiunta una nuova vista denominata Index.aspx. Il contenuto della visualizzazione dell'indice è incluse nel listato 3.


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image17.png)

**Figura 09**: aggiunta di una vista da un'azione del controller ([fare clic per visualizzare l'immagine ingrandita](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image18.png))


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image19.png)

**Figura 10**: creazione di una nuova vista con la finestra di dialogo Aggiungi visualizzazione ([fare clic per visualizzare l'immagine ingrandita](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image20.png))


[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample3.aspx)]

La visualizzazione dell'indice consente di visualizzare tutti i record di film dalla tabella di database di filmati all'interno di una tabella HTML. La vista contiene un ciclo For Each che scorre ogni film rappresentato dalla proprietà ViewData.Model. Se si esegue l'applicazione premendo il tasto F5, si noterà la pagina web nella figura 11.


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image21.png)

**Figura 11**: Visualizza l'indice ([fare clic per visualizzare l'immagine ingrandita](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image22.png))


## <a name="creating-new-database-records"></a>Creazione di nuovi record di Database

La visualizzazione dell'indice creato nella sezione precedente include un collegamento per la creazione di nuovi record del database. Proseguire e implementare la logica e creare la vista per la creazione di nuovi record di database di film.

Il controller Home contiene due metodi denominati metodo di creazione. Il primo metodo di creazione metodo non ha parametri. Questo overload del metodo di creazione metodo viene utilizzato per visualizzare il form HTML per la creazione di un nuovo record di database di film.

Il secondo metodo di metodo di creazione ha un parametro FormCollection. Questo overload del metodo di creazione metodo viene chiamato quando l'invio del form HTML per la creazione di un film di nuovo al server. Si noti che questo secondo metodo di metodo di creazione dispone di un attributo AcceptVerbs che impedisce che il metodo viene chiamato a meno che non viene eseguita un'operazione HTTP Post.

Questo esempio di metodo di creazione secondo metodo è stato modificato nella classe HomeController aggiornata nel listato 4. La nuova versione del metodo di creazione metodo accetta un parametro di film e contiene la logica per l'inserimento di un nuovo filmato nella tabella di database film.

> [!NOTE] 
> 
> Si noti l'attributo di associazione. Poiché non si desidera aggiornare la proprietà Id film da form HTML, è necessario escludere in modo esplicito questa proprietà.


**Listato 4 – Controllers\HomeController.vb (metodo Create modificato)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample4.vb)]

Visual Studio è facile creare il form per la creazione di un nuovo database film registrare (vedere Figura 12). Attenersi ai passaggi riportati di seguito.

1. Il metodo di metodo di creazione nell'editor di codice e scegliere l'opzione di menu **Aggiungi visualizzazione**.
2. Verificare che la casella di controllo con etichetta **creare una visualizzazione fortemente tipizzata** è selezionata.
3. Dal **visualizzare il contenuto** elenco a discesa selezionare il valore *crea*.
4. Dal **visualizzare dati classe** elenco a discesa selezionare il valore *MovieApp.Movie*.
5. Fare clic su di **Aggiungi** pulsante per creare la nuova vista.


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image23.png)

**Figura 12**: aggiunta della visualizzazione di creazione ([fare clic per visualizzare l'immagine ingrandita](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image24.png))


Visual Studio genera automaticamente la visualizzazione nel listato 5. Questa vista contiene un form HTML che include i campi corrispondenti a ogni proprietà della classe film.

**Elenco di 5-Views\Home\Create.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample5.aspx)]

> [!NOTE] 
> 
> Il form HTML generato dalla finestra di dialogo Aggiungi visualizzazione genera un campo modulo Id. Poiché la colonna Id è una colonna Identity, è possibile rimuovere non è necessario il campo del form.


Dopo aver aggiunto la visualizzazione di creazione, è possibile aggiungere nuovi record del film al database. Eseguire l'applicazione premendo il tasto F5 e fare clic sul collegamento Crea nuovo per visualizzare il modulo nella figura 13. Se è completato e inviato il form, viene creato un nuovo record di database di film.

Si noti che si ottiene automaticamente la convalida dei form. Se non si elencano immettere una data di rilascio per un film o si immette una data di rilascio valido, viene visualizzata di nuovo il modulo e il campo Data di rilascio viene evidenziato.


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image25.png)

**Figura 13**: creazione di un nuovo record di database filmato ([fare clic per visualizzare l'immagine ingrandita](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image26.png))


## <a name="editing-existing-database-records"></a>Modifica di record di Database esistenti

Nelle sezioni precedenti, è illustrato come è possibile elencare e creare nuovi record di database. In questa sezione finale, trattati come è possibile modificare i record del database esistente.

In primo luogo, è necessario generare il modulo di modifica. Questo passaggio è facile, poiché Visual Studio genera il modulo di modifica per noi automaticamente. Aprire la classe HomeController.vb nell'editor di codice di Visual Studio ed eseguire la procedura seguente:

1. Il metodo Edit() nell'editor di codice e scegliere l'opzione di menu **Aggiungi visualizzazione** (vedere Figura 14).
2. Selezionare la casella di controllo con etichettata **creare una visualizzazione fortemente tipizzata**.
3. Dal **visualizzare il contenuto** elenco a discesa selezionare il valore *modifica*.
4. Dal **visualizzare dati classe** elenco a discesa selezionare il valore *MovieApp.Movie*.
5. Fare clic su di **Aggiungi** pulsante per creare la nuova vista.

Completare questi passaggi aggiunge una nuova vista denominata Edit nella cartella Views\Home. Questa vista contiene un form HTML per la modifica di un record di film.


[![La finestra di dialogo Nuovo progetto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image27.png)

**Nella figura 14**: aggiunta della visualizzazione di modifica ([fare clic per visualizzare l'immagine ingrandita](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image28.png))


> [!NOTE] 
> 
> La visualizzazione di modifica contiene un campo di form HTML che corrisponde alla proprietà Id film. Non si desidera modificare il valore della proprietà Id di persone, è necessario rimuovere il campo del form.


Infine, è necessario modificare il controller Home in modo che supporti la modifica di un record di database. La classe HomeController aggiornata è contenuta nel listato 6.

**Elenco 6 – Controllers\HomeController.vb (metodi di modifica)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample6.vb)]

Nel listato 6 è stata aggiunta logica aggiuntiva per entrambi gli overload del metodo Edit(). Il primo metodo Edit() restituisce il record di database filmato che corrisponde al parametro Id passato al metodo. Il secondo overload esegue gli aggiornamenti a un record di film nel database.

Si noti che è necessario recuperare il film originale e quindi chiamare ApplyPropertyChanges(), per aggiornare il filmato esistente nel database.

## <a name="summary"></a>Riepilogo

Lo scopo di questa esercitazione è stata per avere un'idea dell'esperienza di creazione di un'applicazione MVC ASP.NET. E speriamo che si sia scoperto che la compilazione di applicazione web MVC ASP.NET è molto simile all'esperienza di creazione di un'applicazione ASP o ASP.NET.

In questa esercitazione sono state esaminate solo le funzionalità di base del framework di MVC ASP.NET. Nelle esercitazioni future, si vengono approfonditi argomenti quali controller, le azioni del controller, visualizzazioni, visualizzare i dati e gli helper HTML.

>[!div class="step-by-step"]
[Precedente](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs.md)
