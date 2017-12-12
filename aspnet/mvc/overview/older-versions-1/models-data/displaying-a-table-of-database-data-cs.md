---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: Visualizza una tabella di dati del Database (c#) | Documenti Microsoft
author: microsoft
description: In questa esercitazione illustrano due metodi per la visualizzazione di un set di record del database. Visualizzare due metodi di formattazione di un set di record del database in un elemento HTML ta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 37ea081df2ee26e186669b815a4d769e1976ae9c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="displaying-a-table-of-database-data-c"></a>Visualizza una tabella di dati del Database (c#)
====================
da [Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> In questa esercitazione illustrano due metodi per la visualizzazione di un set di record del database. Visualizzare due metodi di formattazione di un set di record del database in una tabella HTML. Visualizzare in primo luogo, come è possibile formattare i record di database direttamente in una vista. Successivamente, viene illustrato come è possibile sfruttare parziali durante la formattazione di record del database.


L'obiettivo di questa esercitazione è illustrare come è possibile visualizzare una tabella HTML di dati del database in un'applicazione MVC ASP.NET. È in primo luogo, imparare a utilizzare gli strumenti di scaffolding inclusi in Visual Studio per generare una vista che consente di visualizzare automaticamente un set di record. Successivamente, si imparare a usare un elemento parziale come modello quando si formatta i record del database.

## <a name="create-the-model-classes"></a>Creare le classi modello

Verrà visualizzato il set di record dalla tabella di database film. La tabella di database di filmati contiene le colonne seguenti:

<a id="0.3_table01"></a>


| **Nome colonna** | **Tipo di dati** | **Consenti valori null** |
| --- | --- | --- |
| Id | Int | False |
| Titolo | Nvarchar (200) | False |
| Director | Nvarchar (50) | False |
| DateReleased | DateTime | False |


Per rappresentare la tabella di filmati nell'applicazione ASP.NET MVC, è necessario creare una classe di modello. In questa esercitazione, verrà usato il Framework di entità di Microsoft per creare il modello di classi.

> [!NOTE] 
> 
> In questa esercitazione, verrà usato il Framework di entità di Microsoft. Tuttavia, è importante comprendere che è possibile utilizzare un'ampia gamma di tecnologie diverse per interagire con un database da un'applicazione ASP.NET MVC inclusi LINQ to SQL, NHibernate o ADO.NET.


Seguire questi passaggi per avviare la procedura guidata Entity Data Model:

1. Fare clic sulla cartella modelli nella finestra Esplora soluzioni e selezionare l'opzione di menu **Aggiungi, elemento nuovo**.
2. Selezionare il **dati** categoria e selezionare il **ADO.NET Entity Data Model** modello.
3. Denominare il modello di dati *MoviesDBModel.edmx* e fare clic su di **Aggiungi** pulsante.

Dopo aver fatto clic sul pulsante Aggiungi viene visualizzata la procedura guidata Entity Data Model (vedere la figura 1). Seguire questi passaggi per completare la procedura guidata:

1. Nel **Scegli contenuto Model** passaggio, seleziona il **genera da database** opzione.
2. Nel **Seleziona connessione dati** di istruzioni, utilizzare il *MoviesDB.mdf* connessione dati e il nome *MoviesDBEntities* per le impostazioni di connessione. Fare clic su di **Avanti** pulsante.
3. Nel **Seleziona oggetti di Database** passaggio, espandere il nodo tabelle, selezionare la tabella di film. Immettere lo spazio dei nomi *modelli* e fare clic su di **fine** pulsante.


[![Creazione di LINQ alle classi di SQL](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)

**Figura 01**: creazione di classi LINQ to SQL ([fare clic per visualizzare l'immagine ingrandita](displaying-a-table-of-database-data-cs/_static/image2.png))


Dopo aver completato la procedura guidata Entity Data Model, verrà visualizzata la finestra di progettazione Entity Data Model. La finestra di progettazione deve essere visualizzato l'entità di filmati (vedere la figura 2).


[![Entity Data Model Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)

**Figura 02**: la progettazione di Entity Data Model ([fare clic per visualizzare l'immagine ingrandita](displaying-a-table-of-database-data-cs/_static/image4.png))


È necessario apportare una modifica prima di continuare. La procedura guidata Entity Data genera una classe di modello denominata *filmati* che rappresenta la tabella di database di film. Perché la classe di filmati verrà usato per rappresentare un filmato specifico, è necessario modificare il nome della classe da *film* anziché *filmati* (singolare anziché plurale).

Fare doppio clic sul nome della classe nell'area di progettazione e modificare il nome della classe di filmati in film. Dopo aver apportato questa modifica, fare clic su di **salvare** pulsante (icona del disco floppy) per generare la classe di film.

## <a name="create-the-movies-controller"></a>Creare il Controller di filmati

Ora che è disponibile un modo per rappresentare i record del database, è possibile creare un controller che restituisce la raccolta di filmati. All'interno della finestra Esplora soluzioni di Visual Studio, fare clic sulla cartella controller e selezionare l'opzione di menu **Aggiungi, Controller** (vedere la figura 3).


[![L'aggiunta di Menu Controller](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)

**Figura 03**: Aggiungi Menu Controller ([fare clic per visualizzare l'immagine ingrandita](displaying-a-table-of-database-data-cs/_static/image6.png))


Quando il **Aggiungi Controller** viene visualizzata la finestra, immettere il nome del controller MovieController (vedere la figura 4). Fare clic su di **Aggiungi** pulsante per aggiungere il nuovo controller.


[![La finestra di dialogo Aggiungi Controller](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)

**Figura 04**: finestra di dialogo di Aggiungi Controller ([fare clic per visualizzare l'immagine ingrandita](displaying-a-table-of-database-data-cs/_static/image8.png))


È necessario modificare l'azione Index () esposta dal controller di film in modo che venga restituito il set di record del database. Modificare il controller in modo che risulti simile controller nel listato 1.

**Elenco 1 – Controllers\MovieController.cs**

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

Nel listato 1, la classe MoviesDBEntities viene utilizzata per rappresentare il database MoviesDB. Per utilizzare questa classe, è necessario importare lo spazio dei nomi MvcApplication1.Models simile al seguente:

utilizzando MvcApplication1.Models;

L'espressione *entità. MovieSet.ToList()* restituisce il set di tutti i filmati dalla tabella di database film.

## <a name="create-the-view"></a>Creare la vista

Il modo più semplice per visualizzare un set di record del database in una tabella HTML è per poter sfruttare lo scaffolding fornito da Visual Studio.

Compilare l'applicazione selezionando l'opzione di menu **di compilazione, Compila soluzione**. È necessario compilare l'applicazione prima dell'apertura di **Aggiungi visualizzazione** finestra di dialogo o le classi di dati non verranno visualizzati nella finestra di dialogo.

L'azione Index () e scegliere l'opzione di menu **Aggiungi visualizzazione** (vedere Figura 5).


[![Aggiunta di una vista](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)

**Figura 05**: aggiunta di una vista ([fare clic per visualizzare l'immagine ingrandita](displaying-a-table-of-database-data-cs/_static/image10.png))


Nel **Aggiungi visualizzazione** finestra di dialogo, selezionare la casella di controllo con etichettata **creare una visualizzazione fortemente tipizzata**. Selezionare la classe di film come il **visualizzare dati classe**. Selezionare *elenco* come il **visualizzare il contenuto** (vedere Figura 6). Selezionando queste opzioni genererà una visualizzazione fortemente tipizzato che consente di visualizzare un elenco di film.


[![La finestra di dialogo Aggiungi visualizzazione](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)

**Figura 06**: finestra di dialogo di Aggiungi visualizzazione ([fare clic per visualizzare l'immagine ingrandita](displaying-a-table-of-database-data-cs/_static/image12.png))


Dopo aver selezionato il **Aggiungi** pulsante, la visualizzazione nel listato 2 viene generato automaticamente. Questa vista contiene il codice necessario per scorrere la raccolta di filmati e visualizzare ciascuna delle proprietà di un film.

**Elenco di 2 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

È possibile eseguire l'applicazione selezionando l'opzione di menu **Debug, Avvia debug** (o premendo il tasto F5). Esegue l'applicazione avvia Internet Explorer. Se si passa all'URL /Movie quindi verrà visualizzata la pagina nella figura 7.


[![Una tabella di filmati](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)

**Figura 07**: una tabella di filmati ([fare clic per visualizzare l'immagine ingrandita](displaying-a-table-of-database-data-cs/_static/image14.png))


Se non sono necessariamente l'aspetto della griglia di record del database nella figura 7 è semplicemente possibile modificare la visualizzazione dell'indice. Ad esempio, è possibile modificare il *DateReleased* intestazione *data di rilascio* modificando la visualizzazione dell'indice.

## <a name="create-a-template-with-a-partial"></a>Creare un modello con un elemento parziale

Quando una visualizzazione ottiene troppo complessa, è consigliabile avviare la vista di rilievo in parziali. Utilizzando parziali rende più facile da comprendere e gestire le visualizzazioni. Si creerà un elemento parziale che è possibile utilizzare come modello per formattare ogni record database film.

Seguire questi passaggi per creare parziale:

1. La cartella Views\Movie e scegliere l'opzione di menu **Aggiungi visualizzazione**.
2. Selezionare la casella di controllo con etichettata *creare una visualizzazione parziale (con estensione ascx)*.
3. Assegnare un nome parziale *MovieTemplate*.
4. Selezionare la casella di controllo con etichettata **creare una visualizzazione fortemente tipizzata**.
5. Selezionare come filmato il *visualizzare dati classe*.
6. Selezionare vuoto come il *visualizzare il contenuto*.
7. Fare clic su di **Aggiungi** pulsante per aggiungere al progetto parziale.

Dopo aver completato questi passaggi, modificare il MovieTemplate parziale come elenco di 3.

**Elenco di 3: Views\Movie\MovieTemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

Parziale listato 3 contiene un modello per una singola riga di record.

La visualizzazione dell'indice modificata listato 4 utilizza la MovieTemplate parziale.

**Listato 4 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

La vista listato 4 contiene un ciclo foreach che scorre tutti i film. Per ogni film, il MovieTemplate parziale viene utilizzata per formattare il film. Il MovieTemplate rendering viene eseguito chiamando il metodo di supporto RenderPartial().

La visualizzazione dell'indice modificata esegue il rendering molto stessa tabella HTML di record del database. Tuttavia, la vista è stata notevolmente semplificata.


Il metodo RenderPartial() è diverso rispetto alla maggior parte dei metodi di supporto perché non restituisce una stringa. Pertanto, è necessario chiamare il metodo RenderPartial() usando &lt;Html.RenderPartial() %; %&gt; anziché &lt;% = Html.RenderPartial(); %&gt;.


## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione è illustrare come è possibile visualizzare un set di record del database in una tabella HTML. In primo luogo, è stato descritto come restituire un set di record del database da un'azione del controller per sfruttare i vantaggi di Entity Framework Microsoft. Successivamente, è stato descritto come utilizzare lo scaffolding di Visual Studio per generare una visualizzazione contenente una raccolta di elementi automaticamente. Infine, è stato descritto come semplificare la visualizzazione sfruttando un parziale. È stato descritto come utilizzare un elemento parziale come modello in modo che è possibile formattare ogni record di database.

>[!div class="step-by-step"]
[Precedente](creating-model-classes-with-linq-to-sql-cs.md)
[Successivo](performing-simple-validation-cs.md)
