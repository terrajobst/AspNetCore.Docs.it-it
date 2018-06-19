---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
title: Creazione di classi di modello con Entity Framework (VB) | Documenti Microsoft
author: microsoft
description: In questa esercitazione imparare a usare ASP.NET MVC con Entity Framework Microsoft. Informazioni su come utilizzare la procedura guidata Entity per creare un Da entità ADO.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: ff8322c9-12f3-4e24-aba6-a38046b9bb0d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
msc.type: authoredcontent
ms.openlocfilehash: 3442435c7b2b9ce2ce6bd016ba74fe671eb76f62
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874447"
---
<a name="creating-model-classes-with-the-entity-framework-vb"></a>Creazione di classi di modello con Entity Framework (Visual Basic)
====================
by [Microsoft](https://github.com/microsoft)

> In questa esercitazione imparare a usare ASP.NET MVC con Entity Framework Microsoft. Informazioni su come utilizzare la procedura guidata Entity per creare un ADO.NET Entity Data Model. Nel corso di questa esercitazione, si compila un'applicazione web che illustra come selezionare, inserire, aggiornare ed eliminare dati del database tramite Entity Framework.


L'obiettivo di questa esercitazione è illustrare come è possibile creare classi di accesso di dati mediante Microsoft Entity Framework quando si compila un'applicazione MVC ASP.NET. In questa esercitazione si presuppone alcuna conoscenza di Entity Framework Microsoft. Al termine di questa esercitazione, si sarà in grado utilizzare Entity Framework per selezionare, inserire, aggiornare ed eliminare i record del database.

Microsoft Entity Framework è uno strumento di Mapping relazionale oggetti (O/RM) che consente di generare automaticamente un livello di accesso ai dati da un database. Entity Framework consente di evitare la necessità di compilazione manualmente le classi di accesso ai dati.

> [!NOTE] 
> 
> Non c'è alcuna connessione essenziali tra MVC ASP.NET e Microsoft Entity Framework. Esistono diverse alternative a Entity Framework che è possibile utilizzare with ASP.NET MVC. Ad esempio, è possibile compilare le classi di modello MVC utilizzando altri strumenti O/RM, ad esempio Microsoft LINQ to SQL, NHibernate o SubSonic.


Per illustrare come è possibile utilizzare Microsoft Entity Framework con ASP.NET MVC, verrà creata un'applicazione di esempio semplice. Si creerà un'applicazione di Database filmato che consente di visualizzare e modificare i record del database film.

In questa esercitazione si presuppone la presenza di Visual Studio 2008 o Visual Web Developer 2008 con Service Pack 1. È necessario Service Pack 1 per l'utilizzo di Entity Framework. È possibile scaricare Visual Studio 2008 Service Pack 1 o Visual Web Developer, con Service Pack 1 dal seguente indirizzo:

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)


## <a name="creating-the-movie-sample-database"></a>Creazione del Database di esempio film

L'applicazione di Database di film utilizza una tabella di database denominata filmati che contiene le colonne seguenti:

| Nome colonna | Tipo di dati | Consenti valori null. | È una chiave primaria? |
| --- | --- | --- | --- |
| Id | int | False | True |
| Titolo | nvarchar(100) | False | False |
| Director | nvarchar(100) | False | False |

È possibile aggiungere la tabella a un progetto ASP.NET MVC attenendosi alla procedura seguente:

1. Fare doppio clic su App\_cartella di dati nella finestra Esplora soluzioni e selezionare l'opzione di menu **Aggiungi, elemento nuovo.**
2. Dal **Aggiungi nuovo elemento** nella finestra di dialogo **Database di SQL Server**, fornire il nome MoviesDB.mdf il database e scegliere il **Aggiungi** pulsante.
3. Fare doppio clic sul file MoviesDB.mdf per aprire la finestra di Esplora Server/Esplora Database.
4. Espandere la connessione al database MoviesDB.mdf, fare doppio clic sulla cartella tabelle e selezionare l'opzione di menu **Aggiungi nuova tabella**.
5. In Progettazione tabelle, aggiungere le colonne di tipo Id, titolo e Director.
6. Fare clic su di **salvare** pulsante (con l'icona dell'unità disco floppy) per salvare la nuova tabella con il nome di film.

Dopo aver creato la tabella di database di filmati, aggiungere alcuni dati di esempio per la tabella. La tabella di filmati e scegliere l'opzione di menu **Mostra dati tabella**. È possibile immettere dati film fittizio nella griglia visualizzata.

## <a name="creating-the-adonet-entity-data-model"></a>Creazione di ADO.NET Entity Data Model

Per l'utilizzo di Entity Framework, è necessario creare un Entity Data Model. È possibile trarre vantaggio da Visual Studio *procedura guidata Entity Data Model* per generare automaticamente un Entity Data Model da un database.

Attenersi ai passaggi riportati di seguito.

1. La cartella modelli nella finestra Esplora soluzioni e scegliere l'opzione di menu **Aggiungi, elemento nuovo**.
2. Nel **Aggiungi nuovo elemento** finestra di dialogo, selezionare la categoria di dati (vedere la figura 1).
3. Selezionare il **ADO.NET Entity Data Model** modello, assegnare il nome MoviesDBModel.edmx di Entity Data Model e fare clic su di **Aggiungi** pulsante. Fare clic su di **Aggiungi** pulsante consente di avviare la creazione guidata modello di dati.
4. Nel **Scegli contenuto Model** passaggio, scegliere il **Generate da un database** opzione e fare clic su di **Avanti** pulsante (vedere la figura 2).
5. Nel **Seleziona connessione dati** passaggio, selezionare la connessione al database MoviesDB.mdf, immettere le entità di impostazioni di connessione MoviesDBEntities e fare clic su di **Avanti** pulsante (vedere la figura 3).
6. Nel **Seleziona oggetti di Database** passaggio, selezionare la tabella di database di film e fare clic su di **fine** pulsante (vedere la figura 4).

Dopo aver completato questi passaggi, viene visualizzata la finestra di ADO.NET Entity Data Model Designer (Entity Designer).

**Figura 1: creazione di un nuovo modello di dati di entità**

![clip_image002](creating-model-classes-with-the-entity-framework-vb/_static/image1.jpg)

**Figura 2-scegliere il passaggio di contenuto modello**

![clip_image004](creating-model-classes-with-the-entity-framework-vb/_static/image2.jpg)

**Figura 3 – scegliere la connessione ai dati**

![clip_image006](creating-model-classes-with-the-entity-framework-vb/_static/image3.jpg)

**Figura 4 – Seleziona oggetti di Database**

![clip_image008](creating-model-classes-with-the-entity-framework-vb/_static/image4.jpg)

## <a name="modifying-the-adonet-entity-data-model"></a>Modifica di ADO.NET Entity Data Model

Dopo aver creato un Entity Data Model, è possibile modificare il modello di sfruttare i vantaggi di Entity Designer (vedere Figura 5). È possibile aprire la finestra di progettazione entità in qualsiasi momento facendo doppio clic sul file MoviesDBModel.edmx contenuto nella cartella Models all'interno della finestra Esplora soluzioni.

**Figura 5 – ADO.NET Entity Data Model Designer**

![clip_image010](creating-model-classes-with-the-entity-framework-vb/_static/image5.jpg)

Ad esempio, è possibile utilizzare Entity Designer per modificare i nomi delle classi generate dalla procedura guidata dati di modello di entità. Verrà creata una nuova classe di accesso di dati denominata film. In altre parole, la procedura guidata assegnato alla classe il nome della tabella di database molto stesso. Perché questa classe verrà usato per rappresentare una particolare istanza di film, si dovrebbe rinominare la classe di filmati a film.

Se si desidera rinominare una classe di entità, è possibile fare doppio clic sul nome della classe in Entity Designer e immettere un nuovo nome (vedere Figura 6). In alternativa, è possibile modificare il nome di un'entità nella finestra Proprietà dopo aver selezionato un'entità in Entity Designer.

**Figura 6: modificare un nome di entità**

![clip_image012](creating-model-classes-with-the-entity-framework-vb/_static/image6.jpg)

Ricordarsi di salvare il modello di dati di entità dopo aver apportato una modifica, fare clic sul pulsante Salva (icona del disco floppy). Dietro le quinte, Entity Designer genera un set di classi Visual Basic .NET. È possibile visualizzare queste classi, aprire il file MoviesDBModel.Designer.vb dalla finestra Esplora soluzioni.


Non modificare il codice nel file di Designer poiché le modifiche verranno sovrascritto alla successiva che è utilizzare Entity Designer. Se si desidera estendere le funzionalità delle classi di entità definito nel file di Designer, è possibile creare *classi parziali* in momenti diversi file.


#### <a name="selecting-database-records-with-the-entity-framework"></a>Selezione di record di Database con Entity Framework

Iniziamo compilato l'applicazione di Database di film mediante la creazione di una pagina che visualizza un elenco di film record. Il controller Home nel listato 1 espone un'azione denominata index (). L'azione Index () restituisce tutti i record di film dalla tabella di database film sfruttando di Entity Framework.

**Elenco 1 – Controllers\HomeController.vb**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample1.vb)]

Si noti che il controller nel listato 1 include un costruttore. Il costruttore inizializza un campo a livello di classe denominato \_db. Il \_db campo rappresenta l'entità del database generata da Microsoft Entity Framework. Il \_db è un'istanza della classe MoviesDBEntities che è stata generata da Entity Designer.

Il \_campo db viene utilizzato nell'azione Index () per recuperare i record dalla tabella di database film. L'espressione \_db. MovieSet rappresenta tutti i record dalla tabella di database film. Il metodo ToList () viene utilizzato per convertire il set di filmati in una raccolta generica di oggetti filmato: elenco (di film).

I record di film vengono recuperati con l'aiuto di LINQ to Entities. L'azione Index () nel listato 1 Usa LINQ *la sintassi del metodo* per recuperare il set di record del database. Se si preferisce, è possibile utilizzare LINQ *sintassi della query* invece. Le due istruzioni seguenti eseguono la stessa cosa:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample2.vb)]

Utilizzare la sintassi a seconda del valore LINQ, la sintassi del metodo o sintassi della query, che trovano più intuitiva. Non vi è alcuna differenza nelle prestazioni tra i due approcci, l'unica differenza è lo stile.

La visualizzazione nel listato 2 viene utilizzata per visualizzare i record di film.

**Elenco 2 – Views\Home\Index.aspx.**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample3.aspx)]

La visualizzazione nel listato 2 contiene un **per ogni** ciclo che scorre ogni record di film e visualizza i valori delle proprietà di titolo e Director del record del film. Si noti che accanto a ogni record viene visualizzato un collegamento di modifica e l'eliminazione. Inoltre, un collegamento Aggiungi film viene visualizzata nella parte inferiore della visualizzazione (vedere la figura 7).

**Figura 7: la visualizzazione dell'indice**

![clip_image014](creating-model-classes-with-the-entity-framework-vb/_static/image7.jpg)

La visualizzazione dell'indice è un *tipizzati vista*. La visualizzazione dell'indice è un &lt;% @ Page %&gt; direttiva che include un attributo Inherits. L'attributo Inherits esegue il cast di proprietà ViewData.Model da una raccolta fortemente tipizzata generica elenco di oggetti filmato: un elenco (di film).

## <a name="inserting-database-records-with-the-entity-framework"></a>Inserimento di record di Database con Entity Framework

È possibile utilizzare Entity Framework per semplificarne l'inserimento inserire nuovi record in una tabella di database. Elenco di 3 contiene due nuove azioni di aggiunta alla classe che consente di inserire nuovi record nella tabella di database film controller Home.

**Listato 3 – Controllers\HomeController.vb (Aggiungi metodi)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample4.vb)]

La prima azione Add () restituisce semplicemente una vista. La vista contiene un modulo per l'aggiunta di un nuovo database film registrare (vedere la figura 8). Quando si invia il form, viene richiamata la seconda azione Add ().

Si noti che la seconda azione Add () è decorata con l'attributo AcceptVerbs. Questa azione può essere richiamata solo quando si esegue un'operazione HTTP POST. In altre parole, questa azione può essere richiamata solo durante la registrazione di un form HTML.

La seconda azione Add crea una nuova istanza della classe Entity Framework film con l'aiuto del metodo TryUpdateModel() di MVC ASP.NET. Il metodo TryUpdateModel() accetta i campi di FormCollection passato al metodo Add () e assegna i valori di questi campi di form HTML per la classe di film.


Quando si utilizza Entity Framework, è necessario fornire un elenco"white" delle proprietà quando si utilizza i metodi TryUpdateModel o UpdateModel per aggiornare le proprietà di una classe di entità.


Successivamente, l'azione di Add () esegue la convalida dei form semplice. L'azione di verifica che il titolo e Director proprietà hanno valori. Se si verifica un errore di convalida, viene aggiunto un messaggio di errore di convalida per ModelState.

Se non sono presenti errori di convalida viene aggiunto un nuovo record di film filmati tabella di database con l'aiuto di Entity Framework. Il nuovo record viene aggiunto al database con le due righe di codice seguenti:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample5.vb)]

La prima riga di codice aggiunge la nuova entità di film al set di filmati rilevate da Entity Framework. La seconda riga di codice salva tutte le modifiche apportate per i film rilevati nel database sottostante.

**Figura 8: la vista del componente**

![clip_image016](creating-model-classes-with-the-entity-framework-vb/_static/image8.jpg)

## <a name="updating-database-records-with-the-entity-framework"></a>Aggiornamento dei record di Database con Entity Framework

È possibile seguire quasi lo stesso approccio per modificare un record di database con Entity Framework, come l'approccio che abbiamo adottato solo per inserire un nuovo record di database. Listato 4 contiene due nuove azioni dei controller denominate Edit(). La prima azione Edit() restituisce un form HTML per la modifica di un record di film. La seconda azione Edit() tenta di aggiornare il database.

**Listato 4 – Controllers\HomeController.vb (metodi di modifica)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample6.vb)]

La seconda azione Edit() inizia recuperando il record di film dal database che corrisponde all'Id del filmato in corso di modifica. LINQ seguente all'istruzione entità acquisisce il primo record di database che corrisponde a un Id specifico:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample7.vb)]

Successivamente, il metodo TryUpdateModel() viene utilizzato per assegnare i valori dei campi di form HTML per la proprietà dell'entità di film. Si noti che per specificare le proprietà esatto per l'aggiornamento non viene fornito un elenco vuoto.

Successivamente, una semplice convalida viene eseguita per verificare che sia il titolo del film e Director hanno valori. Se delle proprietà manca un valore, viene aggiunto un messaggio di errore di convalida per ModelState e ModelState.IsValid restituisce il valore false.

Infine, se non sono presenti errori di convalida, la tabella di database sottostante del film viene aggiornata con le modifiche chiamando il metodo SaveChanges ().

Quando si modificano i record del database, è necessario passare l'Id del record in corso di modifica per l'azione del controller che esegue l'aggiornamento del database. In caso contrario, l'azione del controller non riconoscerà il record da aggiornare nel database sottostante. Visualizzazione di modifica, contenuta nel listato 5, include un campo nascosto del modulo che rappresenta l'Id del record di database da modificare.

**Listing 5 – Views\Home\Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Eliminazione di record di Database con Entity Framework

L'operazione finale del database, che è necessario eseguire in questa esercitazione, è l'eliminazione di record del database. Azione del controller nel listato 6 consentono di eliminare un record di database specifico.

**Elenco 6 - \Controllers\HomeController.vb (azione di eliminazione)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample9.vb)]

L'azione Delete () Recupera innanzitutto il film entità che corrisponde all'Id passato all'azione. Successivamente, il film viene eliminato dal database chiamando il metodo DeleteObject() seguito dal metodo SaveChanges (). Infine, l'utente viene reindirizzato alla visualizzazione dell'indice.

## <a name="summary"></a>Riepilogo

Lo scopo di questa esercitazione è stata per illustrare come è possibile compilare applicazioni web basate su database sfruttando di ASP.NET MVC e Microsoft Entity Framework. È stato descritto come compilare un'applicazione che consente di selezionare, inserire, aggiornare ed eliminare i record del database.

In primo luogo, è illustrato come è possibile utilizzare la procedura guidata Entity Data Model per generare un Entity Data Model da Visual Studio. Successivamente, si informazioni su come usare LINQ to Entities per recuperare un set di record del database da una tabella di database. Infine, Entity Framework è usato per inserire, aggiornare ed eliminare i record del database.

> [!div class="step-by-step"]
> [Precedente](validation-with-the-data-annotation-validators-cs.md)
> [Successivo](creating-model-classes-with-linq-to-sql-vb.md)
