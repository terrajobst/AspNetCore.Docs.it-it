---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: Visualizzazione di una tabella di dati del Database (c#) | Microsoft Docs
author: microsoft
description: In questa esercitazione, illustrano due metodi di visualizzazione di un set di record di database. Mostra due metodi di formattazione di un set di record di database in un elemento HTML ta...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: d0d3f6a574a4b923d5da73ccb2ab3bfbd6f305ef
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823862"
---
<a name="displaying-a-table-of-database-data-c"></a>Visualizzazione di una tabella di dati del Database (c#)
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> In questa esercitazione, illustrano due metodi di visualizzazione di un set di record di database. Visualizzare due metodi di formattazione di un set di record di database in una tabella HTML. In primo luogo, illustrato come è possibile formattare i record del database direttamente all'interno di una vista. Successivamente, si dimostrerà come è possibile sfruttare i vantaggi di righe parzialmente eseguite durante la formattazione dei record di database.


L'obiettivo di questa esercitazione è illustrare come è possibile visualizzare una tabella HTML di dati del database in un'applicazione ASP.NET MVC. In primo luogo, descrive come usare gli strumenti di scaffolding inclusi in Visual Studio per generare una vista che consente di visualizzare automaticamente un set di record. Successivamente, informazioni su come usare un elemento parziale come modello durante la formattazione dei record di database.

## <a name="create-the-model-classes"></a>Creare le classi del modello

Verrà visualizzato il set di record dalla tabella di database di film. La tabella di database di film contiene le colonne seguenti:

<a id="0.3_table01"></a>


| **Nome della colonna** | **Tipo di dati** | **Consenti valori null** |
| --- | --- | --- |
| Id | Int | False |
| Titolo | Nvarchar (200) | False |
| Director | Nvarchar (50) | False |
| DateReleased | DateTime | False |


Per rappresentare la tabella di film nell'applicazione ASP.NET MVC, è necessario creare una classe di modello. In questa esercitazione, usiamo Microsoft Entity Framework per creare le classi di modello.

> [!NOTE] 
> 
> In questa esercitazione, si usa Microsoft Entity Framework. Tuttavia, è importante comprendere che è possibile usare un'ampia gamma di tecnologie diverse per interagire con un database da un'applicazione ASP.NET MVC incluso LINQ to SQL, NHibernate o ADO.NET.


Seguire questi passaggi per avviare la procedura guidata Entity Data Model:

1. Fare doppio clic su cartella Models nella finestra Esplora soluzioni e selezionare l'opzione di menu **Aggiungi, elemento nuovo**.
2. Selezionare il **Data** categoria e selezionare il **ADO.NET Entity Data Model** modello.
3. Denominare il modello di dati *MoviesDBModel.edmx* e fare clic sui **Add** pulsante.

Dopo aver fatto clic sul pulsante Aggiungi, viene visualizzata la procedura guidata Entity Data Model (vedere la figura 1). Seguire questi passaggi per completare la procedura guidata:

1. Nel **Scegli contenuto Model** passaggio, seleziona la **genera da database** opzione.
2. Nel **Seleziona connessione dati** istruzioni, utilizzare il *MoviesDB.mdf* connessione dati e il nome *MoviesDBEntities* per le impostazioni di connessione. Scegliere il **successivo** pulsante.
3. Nel **Scegli oggetti di Database** passaggio, espandere il nodo tabelle, selezionare la tabella di film. Immettere lo spazio dei nomi *modelli* e fare clic sui **fine** pulsante.


[![Creazione di LINQ alle classi di SQL](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)

**Figura 01**: la creazione di classi LINQ to SQL ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-a-table-of-database-data-cs/_static/image2.png))


Dopo aver completato la procedura guidata Entity Data Model, verrà visualizzata la finestra di Entity Data Model Designer. La finestra di progettazione deve visualizzare le entità film (vedere la figura 2).


[![Entity Data Model Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)

**Figura 02**: The Entity Data Model Designer ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-a-table-of-database-data-cs/_static/image4.png))


È necessario apportare una modifica prima di continuare. La procedura guidata Entity Data genera una classe di modello denominata *film* che rappresenta la tabella di database di film. Poiché la classe di film verrà usato per rappresentare un filmato specifico, è necessario modificare il nome della classe sia *film* invece di *film* (singolare anziché plurale).

Fare doppio clic sul nome della classe nell'area di progettazione e modificare il nome della classe da film al film. Dopo aver apportato questa modifica, scegliere il **salvare** pulsante (l'icona del disco floppy) per generare la classe di film.

## <a name="create-the-movies-controller"></a>Creare il Controller di film

Ora che abbiamo a disposizione un modo per rappresentare i record del database, è possibile creare un controller che restituisce la raccolta di film. All'interno della finestra Esplora soluzioni di Visual Studio, fare clic sulla cartella controller e selezionare l'opzione di menu **Controller, Aggiungi** (vedere la figura 3).


[![Il Controller Menu Aggiungi](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)

**Figura 03**: il Controller di Menu di aggiunta dei ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-a-table-of-database-data-cs/_static/image6.png))


Quando la **Aggiungi Controller** finestra di dialogo viene visualizzata, immettere il nome del controller MovieController (vedere la figura 4). Scegliere il **Aggiungi** pulsante per aggiungere il nuovo controller.


[![La finestra di dialogo Aggiungi Controller](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)

**Figura 04**: finestra di dialogo Aggiungi Controller The ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-a-table-of-database-data-cs/_static/image8.png))


È necessario modificare l'azione Index () esposto dal controller di film in modo che restituisca il set di record del database. Modificare il controller in modo che risulti come il controller nel listato 1.

**Listato 1 – Controllers\MovieController.cs**

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

Nel listato 1, la classe MoviesDBEntities viene utilizzata per rappresentare il database MoviesDB. Per usare questa classe, è necessario importare lo spazio dei nomi MvcApplication1.Models simile al seguente:

usando MvcApplication1.Models;

L'espressione *entità. MovieSet.ToList()* restituisce il set di tutti i film dalla tabella di database di film.

## <a name="create-the-view"></a>Creare la vista

Il modo più semplice per visualizzare un set di record del database in una tabella HTML è per poter sfruttare lo scaffolding fornito da Visual Studio.

Compilare l'applicazione selezionando l'opzione di menu **compilazione, Compila soluzione**. È necessario compilare l'applicazione prima di aprire la **Aggiungi visualizzazione** finestra di dialogo o le classi di dati non sarà più visualizzato nella finestra di dialogo.

L'azione Index () e scegliere l'opzione di menu **Aggiungi visualizzazione** (vedere la figura 5).


[![Aggiunta di una vista](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)

**Figura 05**: aggiunta di una vista ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-a-table-of-database-data-cs/_static/image10.png))


Nel **Aggiungi visualizzazione** finestra di dialogo, selezionare la casella di controllo etichettato **creare una visualizzazione fortemente tipizzata**. Selezionare la classe di film come le **visualizzare i dati classe**. Selezionare *elenco* come la **visualizzare contenuto** (vedere la figura 6). Selezionando queste opzioni genererà una visualizzazione fortemente tipizzato che consente di visualizzare un elenco di film.


[![La finestra di dialogo Aggiungi visualizzazione](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)

**Figura 06**: finestra di dialogo la visualizzazione aggiunta ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-a-table-of-database-data-cs/_static/image12.png))


Dopo aver selezionato il **Add** pulsante, la visualizzazione nel listato 2 viene generato automaticamente. Questa vista contiene il codice necessario per scorrere la raccolta di film e visualizzare ogni proprietà di un film.

**Listato 2 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

È possibile eseguire l'applicazione selezionando l'opzione di menu **eseguire il Debug, Avvia debug** (o premere F5). Esegue l'applicazione avvia Internet Explorer. Se si passa all'URL /Movie verrà visualizzata la pagina nella figura 7.


[![Una tabella di film](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)

**Figura 07**: una tabella di film ([fare clic per visualizzare l'immagine con dimensioni normali](displaying-a-table-of-database-data-cs/_static/image14.png))


Se non si desidera che tutti gli elementi l'aspetto della griglia di record del database nella figura 7 è possibile modificare semplicemente la visualizzazione dell'indice. Ad esempio, è possibile modificare il *DateReleased* intestazione *data di rilascio* modificando la visualizzazione dell'indice.

## <a name="create-a-template-with-a-partial"></a>Creare un modello con un elemento parziale

Quando una visualizzazione diventa troppo complicata, è consigliabile avviare la vista di rilievo nelle righe parzialmente eseguite. Con righe parzialmente eseguite rende più facile da comprendere e gestire le visualizzazioni. Si creerà un elemento parziale che è possibile usare come modello per formattare ogni record del database di film.

Seguire questi passaggi per creare il parziale:

1. Fare doppio clic nella cartella Views\Movie e selezionare l'opzione di menu **Aggiungi visualizzazione**.
2. Selezionare la casella di controllo etichettato *creare una visualizzazione parziale (con estensione ascx)*.
3. Assegnare un nome parziale *MovieTemplate*.
4. Selezionare la casella di controllo etichettato **creare una visualizzazione fortemente tipizzata**.
5. Selezionare i film come le *visualizzare i dati classe*.
6. Seleziona vuoto come le *visualizzare il contenuto*.
7. Scegliere il **Add** pulsante per aggiungere al progetto parziale.

Dopo aver completato questi passaggi, modificare il MovieTemplate parziale simile al listato 3.

**Listato 3 – Views\Movie\MovieTemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

Parziale nel listato 3 contiene un modello per una singola riga di record.

La visualizzazione dell'indice modificata nel listato 4 utilizza il MovieTemplate parziale.

**Listato 4 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

La visualizzazione nel listato 4 contiene un ciclo foreach che scorre tutti i film. Per ogni film, il MovieTemplate parziale viene utilizzata per formattare il film. Viene eseguito il rendering di MovieTemplate chiamando il metodo helper RenderPartial().

La visualizzazione dell'indice modificata esegue il rendering molto stessa tabella HTML di record di database. Tuttavia, la visualizzazione è stata notevolmente semplificata.


Il metodo RenderPartial() è diverso rispetto alla maggior parte degli altri metodi helper perché non restituisce una stringa. Pertanto, è necessario chiamare il metodo RenderPartial() usando &lt;Html.RenderPartial() %; %&gt; invece di &lt;% = Html.RenderPartial(); %&gt;.


## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione è stata per illustrare come è possibile visualizzare un set di record del database in una tabella HTML. In primo luogo, è stato descritto come restituire un set di record di database da un'azione del controller, sfruttando i vantaggi di Microsoft Entity Framework. Successivamente, si è appreso come usare lo scaffolding di Visual Studio per generare una vista che viene visualizzata automaticamente una raccolta di elementi. Infine, si è appreso come semplificare la visualizzazione, sfruttando i vantaggi di un elemento parziale. Si è appreso come usare un elemento parziale come un modello in modo che è possibile formattare ogni record del database.

> [!div class="step-by-step"]
> [Precedente](creating-model-classes-with-linq-to-sql-cs.md)
> [Successivo](performing-simple-validation-cs.md)
