---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
title: Creazione di classi di modello con Entity Framework (c#) | Microsoft Docs
author: microsoft
description: In questa esercitazione descrive come usare ASP.NET MVC con Entity Framework Microsoft. Informazioni su come usare la procedura guidata Entity creare Da un'entità ADO.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 61644169-e8b1-45dd-bf96-9c2301b69879
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
msc.type: authoredcontent
ms.openlocfilehash: bd94c3dd39fc83f0d412f3e646c237b58819bd11
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363442"
---
<a name="creating-model-classes-with-the-entity-framework-c"></a>Creazione di classi di modello con Entity Framework (c#)
====================
da [Microsoft](https://github.com/microsoft)

> In questa esercitazione descrive come usare ASP.NET MVC con Entity Framework Microsoft. Descrive come usare la procedura guidata Entity per creare un modello ADO.NET Entity Data Model. Nel corso di questa esercitazione, creiamo un'applicazione web che illustra come selezionare, inserire, aggiornare ed eliminare i dati del database tramite Entity Framework.


L'obiettivo di questa esercitazione è illustrare come è possibile creare classi di accesso ai dati mediante Microsoft Entity Framework quando si compila un'applicazione ASP.NET MVC. Questa esercitazione si presuppone alcuna conoscenza di Microsoft Entity Framework. Al termine di questa esercitazione, si sarà informazioni su come usare Entity Framework per selezionare, inserire, aggiornare ed eliminare i record del database.

Microsoft Entity Framework è uno strumento di Mapping relazionale oggetti (O/RM) che consente di generare automaticamente un livello di accesso ai dati da un database. Entity Framework consente di evitare la necessità di creazione manuale delle classi di accesso dati.

Per illustrare come è possibile usare Microsoft Entity Framework con MVC ASP.NET, verrà compilata un'applicazione di esempio semplice. Si creerà un'applicazione di Database di film che consente di visualizzare e modificare i record di database di film.

Questa esercitazione si presuppone che sia installato Visual Studio 2008 o Visual Web Developer 2008 con Service Pack 1. È necessario Service Pack 1 per usare Entity Framework. È possibile scaricare Visual Studio 2008 Service Pack 1 o Visual Web Developer con Service Pack 1 dall'indirizzo seguente:

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)


> [!NOTE] 
> 
> Non c'è alcuna connessione essenziali tra ASP.NET MVC ed Entity Framework Microsoft. Sono disponibili numerose alternative a Entity Framework che è possibile usare ASP.NET MVC. Ad esempio, è possibile compilare le classi del modello MVC usando altri strumenti O/RM, ad esempio Microsoft LINQ to SQL o NHibernate SubSonic.


## <a name="creating-the-movie-sample-database"></a>Creazione del Database di esempio del film

L'applicazione di Database di film utilizza una tabella di database denominata film che contiene le colonne seguenti:

| Nome colonna | Tipo di dati | Consenti valori null? | È una chiave primaria? |
| --- | --- | --- | --- |
| Id | int | False | True |
| Titolo | Nvarchar(100) | False | False |
| Director | Nvarchar(100) | False | False |

È possibile aggiungere questa tabella per un progetto MVC ASP.NET seguendo questa procedura:

1. Fare doppio clic su App\_cartella di dati nella finestra Esplora soluzioni e selezionare l'opzione di menu **Aggiungi, elemento nuovo.**
2. Dal **Aggiungi nuovo elemento** finestra di dialogo **Database SQL Server**, assegnare il nome MoviesDB.mdf al database e fare clic sul **Aggiungi** pulsante.
3. Fare doppio clic sul file MoviesDB.mdf per aprire la finestra di Esplora Server/Esplora Database.
4. Espandere la connessione al database MoviesDB.mdf, pulsante destro del mouse sulla cartella tabelle e selezionare l'opzione di menu **Aggiungi nuova tabella**.
5. In Progettazione tabelle, aggiungere le colonne Id, Title e direttore.
6. Scegliere il **salvare** pulsante (con l'icona di unità disco floppy) per salvare la nuova tabella con il nome di film.

Dopo aver creato la tabella di database di film, è necessario aggiungere alcuni dati di esempio per la tabella. Fare doppio clic nella tabella di film e selezionare l'opzione di menu **Mostra dati tabella**. È possibile immettere i dati dei film fittizio nella griglia visualizzata.

## <a name="creating-the-adonet-entity-data-model"></a>Creazione di ADO.NET Entity Data Model

Per usare Entity Framework, è necessario creare un Entity Data Model. È possibile sfruttare i vantaggi di Visual Studio *procedura guidata Entity Data Model* per generare automaticamente un Entity Data Model da un database.

Attenersi ai passaggi riportati di seguito.

1. La cartella Models nella finestra Esplora soluzioni e scegliere l'opzione di menu **Aggiungi, elemento nuovo**.
2. Nel **Aggiungi nuovo elemento** finestra di dialogo, selezionare la categoria di dati (vedere la figura 1).
3. Selezionare il **ADO.NET Entity Data Model** modello, assegnare il nome MoviesDBModel.edmx Entity Data Model e scegliere il **Add** pulsante. Facendo clic sui **Add** pulsante Avvia la creazione guidata modello di dati.
4. Nel **Scegli contenuto Model** passaggio, scegliere il **Generate da un database** opzione e fare clic sui **Avanti** pulsante (vedere la figura 2).
5. Nel **Seleziona connessione dati** passaggio, selezionare la connessione al database MoviesDB.mdf, immettere le entità di impostazioni di connessione MoviesDBEntities assegnare un nome e fare clic sui **successivo** pulsante (vedere la figura 3).
6. Nel **Scegli oggetti di Database** passaggio, selezionare la tabella di database di film e fare clic sui **fine** pulsante (vedere la figura 4).

Dopo aver completato questi passaggi, verrà visualizzata la finestra di ADO.NET Entity Data Model Designer (Entity Designer).

**Figura 1: creazione di un nuovo Entity Data Model**

![clip_image002](creating-model-classes-with-the-entity-framework-cs/_static/image1.jpg)

**Figura 2: scegliere il passaggio di contenuto modello**

![clip_image004](creating-model-classes-with-the-entity-framework-cs/_static/image2.jpg)

**Figura 3: scegliere la connessione dati**

![clip_image006](creating-model-classes-with-the-entity-framework-cs/_static/image3.jpg)

**Figura 4 – Scegli oggetti di Database**

![clip_image008](creating-model-classes-with-the-entity-framework-cs/_static/image4.jpg)

#### <a name="modifying-the-adonet-entity-data-model"></a>Modifica di ADO.NET Entity Data Model

Dopo aver creato un Entity Data Model, è possibile modificare il modello, sfruttando i vantaggi di Entity Designer (vedere la figura 5). È possibile aprire la finestra di progettazione entità in qualsiasi momento facendo doppio clic sul file MoviesDBModel.edmx contenuto nella cartella Models all'interno della finestra Esplora soluzioni.

**Figura 5 – ADO.NET Entity Data Model Designer**

![clip_image010](creating-model-classes-with-the-entity-framework-cs/_static/image5.jpg)

Ad esempio, è possibile utilizzare Entity Designer per modificare i nomi delle classi generate dalla procedura guidata dati modello di entità. La procedura guidata creata una nuova classe di accesso di dati denominata film. In altre parole, la procedura guidata assegnato alla classe il nome stesso della tabella di database. Perché questa classe verrà usata per rappresentare una particolare istanza di film, assegnare la classe da film al film.

Se si desidera rinominare una classe di entità, è possibile fare doppio clic sul nome della classe in Entity Designer e immettere un nuovo nome (vedere la figura 6). In alternativa, è possibile modificare il nome di un'entità nella finestra delle proprietà dopo aver selezionato un'entità in Entity Designer.

**Figura 6: modificare un nome di entità**

![clip_image012](creating-model-classes-with-the-entity-framework-cs/_static/image6.jpg)

Ricordarsi di salvare il modello di dati di entità dopo aver apportato una modifica, fare clic sul pulsante Salva (l'icona del disco floppy). Dietro le quinte, Entity Designer genera un set di classi c#. È possibile visualizzare queste classi, aprire il file MoviesDBModel.Designer.cs dalla finestra Esplora soluzioni.


Non modificare il codice nel file Designer.cs poiché le modifiche verranno sovrascritte la volta successiva che si utilizza Entity Designer. Se si desidera estendere le funzionalità delle classi di entità definito nel file Designer.cs, è possibile creare *classi parziali* in file separati.


#### <a name="selecting-database-records-with-the-entity-framework"></a>Selezione dei record di Database con Entity Framework

Iniziamo compilazione dell'applicazione di Database di film mediante la creazione di una pagina che visualizza un elenco di record di film. Il controller Home nel listato 1 espone un'azione denominata index (). L'azione Index () restituisce tutti i record di film dalla tabella di database di film, sfruttando i vantaggi di Entity Framework.

**Listato 1 – controllers\homecontroller.cs.**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample1.cs)]

Si noti che il controller nel listato 1 include un costruttore. Il costruttore inizializza un campo a livello di classe denominato \_db. Il \_db campo rappresenta l'entità del database generata da Microsoft Entity Framework. Il \_db è un'istanza della classe MoviesDBEntities che è stata generata da Entity Designer.


Per usare la classe theMoviesDBEntities nel controller Home, è necessario importare lo spazio dei nomi MovieEntityApp.Models (*MVCProjectName*. Modelli).


Il \_db campo viene usato all'interno dell'azione Index () per recuperare i record dalla tabella di database di film. L'espressione \_db. MovieSet rappresenta tutti i record dalla tabella di database di film. Il metodo ToList () viene utilizzato per convertire il set di film in un insieme generico di oggetti filmato (elenco&lt;film&gt;).

Con l'aiuto di LINQ to Entities vengono recuperati i record di film. L'azione Index () nel listato 1 Usa LINQ *sintassi di metodo* per recuperare il set di record di database. Se si preferisce, è possibile usare LINQ *sintassi di query* invece. Le due istruzioni seguenti eseguire la stessa operazione:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample2.cs)]

Usare qualsiasi sintassi LINQ, la sintassi del metodo o la sintassi di query, che trovano più intuitiva. Non c'è alcuna differenza nelle prestazioni tra i due approcci: l'unica differenza è lo stile.

La visualizzazione nel listato 2 consente di visualizzare i record di film.

**Listato 2 – Views\Home\Index.aspx.**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample3.aspx)]

La visualizzazione nel listato 2 contiene un' **foreach** ciclo che esegue l'iterazione attraverso ogni record di film e visualizza i valori delle proprietà di titolo e direttore del record film. Si noti che accanto a ogni record viene visualizzato un collegamento di modifica ed eliminazione. Inoltre, un collegamento di aggiungere film viene visualizzata nella parte inferiore della visualizzazione (vedere la figura 7).

**Figura 7: la visualizzazione dell'indice**

![clip_image014](creating-model-classes-with-the-entity-framework-cs/_static/image7.jpg)

La visualizzazione dell'indice è un *tipizzate visualizzazione*. La visualizzazione dell'indice include un' &lt;% @ Page %&gt; direttiva con un *Inherits* attributo che esegue il cast di proprietà del modello per una raccolta fortemente tipizzata di elenco generica di oggetti filmato (elenco&lt;film).

## <a name="inserting-database-records-with-the-entity-framework"></a>Inserimento di record di Database con Entity Framework

È possibile utilizzare Entity Framework per rendono più semplice inserire nuovi record in una tabella di database. Listato 3 contiene due azioni di nuovo aggiunte alla classe controller Home che è possibile usare per inserire nuovi record nella tabella di database di film.

**Listato 3 – Controllers\HomeController.cs (Aggiungi metodi)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample4.cs)]

La prima azione Add () restituisce semplicemente una vista. La vista contiene un modulo per l'aggiunta di un nuovo database di film di record (vedere la figura 8). Quando si invia il form, viene richiamata la seconda azione Add ().

Si noti che la seconda azione Add () è decorata con l'attributo AcceptVerbs. Questa azione può essere richiamata solo quando si esegue un'operazione HTTP POST. In altre parole, questa azione può essere richiamata solo durante la registrazione di un form HTML.

La seconda azione Add () consente di creare una nuova istanza della classe Entity Framework film con l'aiuto del metodo TryUpdateModel() MVC ASP.NET. Il metodo TryUpdateModel() accetta i campi di FormCollection passato al metodo Add () e assegna i valori di questi campi modulo HTML alla classe del film.


Quando si usa Entity Framework, è necessario fornire un elenco"white" delle proprietà quando si utilizzano i metodi UpdateModel o TryUpdateModel per aggiornare le proprietà di una classe di entità.


Successivamente, l'azione di Add () esegue alcune convalide modulo semplice. L'azione di verifica che il titolo e direttore proprietà hanno valori. Se si verifica un errore di convalida, viene aggiunto un messaggio di errore di convalida per ModelState.

Se non sono presenti errori di convalida viene aggiunto un nuovo record di film alla tabella di database di film con l'aiuto di Entity Framework. Il nuovo record viene aggiunto al database con le due righe di codice seguenti:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample5.cs)]

La prima riga del codice aggiunge la nuova entità film al set di film rilevati da Entity Framework. La seconda riga di codice salva tutte le modifiche sono state apportate per i film rilevati nel database sottostante.

**Figura 8: la visualizzazione aggiunta**

![clip_image016](creating-model-classes-with-the-entity-framework-cs/_static/image8.jpg)

#### <a name="updating-database-records-with-the-entity-framework"></a>L'aggiornamento dei record di Database con Entity Framework

È possibile seguire quasi lo stesso approccio per modificare un record di database con Entity Framework come l'approccio che abbiamo adottato solo per inserire un nuovo record di database. Listato 4 contiene due nuove azioni controller denominate Edit (). La prima azione Edit () restituisce un form HTML per la modifica di un record di film. La seconda azione Edit () tenta di aggiornare il database.

**Listato 4 – Controllers\HomeController.cs (metodi Edit)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample6.cs)]

La seconda azione Edit () inizia recuperando il record di film dal database che corrisponde all'Id del film in fase di modifica. LINQ seguente all'istruzione entità acquisisce il primo record del database che corrisponde a un Id specifico:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample7.cs)]

Successivamente, il metodo TryUpdateModel() consente di assegnare i valori dei campi HTML per le proprietà dell'entità film. Si noti che non viene fornito un elenco elementi consentiti per specificare l'esatta proprietà da aggiornare.

Successivamente, viene eseguita una semplice convalida per verificare che sia il titolo del film Director proprietà hanno valori. Se delle proprietà manca un valore, quindi è aggiunto un messaggio di errore di convalida ModelState e ModelState restituisce il valore false.

Infine, se non sono presenti errori di convalida, la tabella di database di film sottostante viene aggiornata con eventuali modifiche chiamando il metodo SaveChanges ().

Quando si modificano i record del database, è necessario passare l'Id del record da modificare per l'azione del controller che esegue l'aggiornamento del database. In caso contrario, l'azione del controller non riconoscerà il record da aggiornare nel database sottostante. Visualizzazione di modifica, contenuta nel listato 5, include un campo del form nascosto che rappresenta l'Id del record del database in corso di modifica.

**Listato 5 – Views\Home\Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>L'eliminazione di record di Database con Entity Framework

L'operazione finale del database, che è necessario affrontare in questa esercitazione, è l'eliminazione di record del database. Per eliminare un record di database particolare, è possibile usare l'azione del controller nel listato 6.

**Listato 6 - \Controllers\HomeController.cs (azione di eliminazione)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample9.cs)]

L'azione Delete () prima di tutto recupera il film entità che corrisponde all'Id passato all'azione. Successivamente, il film viene eliminato dal database chiamando il metodo DeleteObject () seguito dal metodo SaveChanges (). Infine, l'utente viene reindirizzato nuovamente alla vista Index.

## <a name="summary"></a>Riepilogo

Lo scopo di questa esercitazione è dimostrare come creare applicazioni web basate su database, sfruttando i vantaggi di ASP.NET MVC ed Entity Framework Microsoft. Si è appreso come creare un'applicazione che consente di selezionare, inserire, aggiornare ed eliminare i record del database.

Innanzitutto, abbiamo parlato di come è possibile usare la procedura guidata Entity Data Model per generare un Entity Data Model dall'interno di Visual Studio. Successivamente, descrive come usare LINQ to Entities per recuperare un set di record di database da una tabella di database. Infine, abbiamo utilizzato Entity Framework per inserire, aggiornare ed eliminare i record del database.

> [!div class="step-by-step"]
> [avanti](creating-model-classes-with-linq-to-sql-cs.md)
