---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
title: "Iterazione #1-creare l'applicazione (c#) | Microsoft Docs"
author: microsoft
description: 'Nella prima iterazione, verranno create Contact Manager nel modo più semplice possibile. Viene aggiunto il supporto per le operazioni di base dei database: crea, lettura, aggiornamento e D....'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: db0f160b-901c-46d3-865e-7ab6cd4ed68d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
msc.type: authoredcontent
ms.openlocfilehash: aa79379c721456a62ee62e37781e51f61e16d3bc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403045"
---
<a name="iteration-1--create-the-application-c"></a>Iterazione #1-creare l'applicazione (c#)
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare il codice](iteration-1-create-the-application-cs/_static/contactmanager_1_cs1.zip)

> Nella prima iterazione, verranno create Contact Manager nel modo più semplice possibile. Viene aggiunto il supporto per le operazioni di base dei database: crea, lettura, aggiornamento ed eliminazione (CRUD).


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Creazione di un'applicazione di gestione dei contatti ASP.NET MVC (VB)

In questa serie di esercitazioni, creiamo un'intera applicazione di gestione dei contatti dall'inizio alla fine. L'applicazione Contact Manager consente di archiviare le informazioni di contatto - nomi, i numeri di telefono e indirizzi di posta elettronica - per un elenco di persone.

Si compilerà l'applicazione a varie iterazioni. A ogni iterazione, abbiamo migliorare gradualmente l'applicazione. L'obiettivo di questo approccio iterazione più è che consentono di comprendere il motivo per ogni modifica.

- Iterazione #1 - creare l'applicazione. Nella prima iterazione, verranno create Contact Manager nel modo più semplice possibile. Viene aggiunto il supporto per le operazioni di base dei database: crea, lettura, aggiornamento ed eliminazione (CRUD).

- Iterazione #2 - migliorare l'applicazione aspetto interessante. In questa iterazione è migliorare l'aspetto dell'applicazione modificando il valore predefinito di pagina master visualizzazione MVC ASP.NET e foglio di stile CSS.

- Iterazione #3 - aggiungere la convalida dei form. Nella terza iterazione, aggiungiamo la convalida dei form di base. Persone è impedire l'invio di un modulo senza completare i campi del modulo richiesto. È anche convalidare gli indirizzi di posta elettronica e numeri di telefono.

- Iterazione #4 - rendere l'applicazione a regime. In questa terza iterazione, possiamo usufruire dei diversi modelli di progettazione di software per renderne più semplice da gestire e modificare l'applicazione Contact Manager. Ad esempio, è eseguire il refactoring l'applicazione per usare il modello di Repository e il modello di inserimento delle dipendenze.

- Iterazione #5 - creare unit test. Nell'iterazione del quinto, si semplifica l'applicazione di manutenzione e la modifica mediante l'aggiunta di unit test. È simulare le classi di modello di dati e generare unit test per i controller e logica di convalida.

- Iterazione #6 - usare lo sviluppo basato su test. In questa iterazione sesta, viene aggiunto nuove funzionalità all'applicazione scrivendo unit test prima e scrivere codice sull'unit test. In questa iterazione, viene aggiunto gruppi di contatti.

- Iterazione #7 - aggiungere funzionalità Ajax. Nell'iterazione del settimo, aggiungendo il supporto per Ajax è migliorare la velocità di risposta e le prestazioni dell'applicazione.

## <a name="this-iteration"></a>Questa iterazione

In questa prima iterazione, creiamo l'applicazione di base. L'obiettivo consiste nella compilazione di Contact Manager nel modo più semplice e rapido possibile. Nelle iterazioni successive, è migliorare la progettazione dell'applicazione.

L'applicazione Contact Manager è un'applicazione basata su database di base. È possibile utilizzare l'applicazione per creare nuovi contatti, modificare i contatti esistenti ed eliminare i contatti.

In questa iterazione è completare i passaggi seguenti:

1. Applicazione ASP.NET MVC
2. Creare un database in cui archiviare i contatti
3. Generare una classe di modello per il database di Microsoft Entity Framework
4. Creare un'azione del controller e la visualizzazione che consente di elencare tutti i contatti nel database
5. Creare una vista che permette di creare un nuovo contatto nel database e le azioni del controller
6. Creare una vista che consente di modificare un contatto esistente nel database e le azioni del controller
7. Creare una vista che consente di eliminare un contatto esistente nel database e le azioni del controller

## <a name="software-prerequisites"></a>Prerequisiti software

Nelle applicazioni ASP.NET MVC, è necessario disporre di Visual Studio 2008 o Visual Web Developer 2008 installati nel computer (Visual Web Developer è una versione gratuita di Visual Studio che non include tutte le funzionalità avanzate di Visual Studio). È possibile scaricare la versione di valutazione di Visual Studio 2008 o Visual Web Developer dall'indirizzo seguente:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> Per le applicazioni ASP.NET MVC con Visual Web Developer, è necessario disporre di Visual Web Developer Service Pack 1 installato. Senza Service Pack 1, è possibile creare progetti di applicazione Web.


Framework di MVC ASP.NET. È possibile scaricare il framework ASP.NET MVC dall'indirizzo seguente:

[https://www.asp.net/mvc](../../../index.md)

In questa esercitazione, utilizziamo Microsoft Entity Framework per accedere a un database. Entity Framework è incluso in .NET Framework 3.5 Service Pack 1. È possibile scaricare questo service pack dal percorso seguente:

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp; displaylang = en](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

In alternativa all'esecuzione di ognuno di questi download uno alla volta, è possibile sfruttare l'installazione guidata piattaforma Web (PI Web). È possibile scaricare l'installazione guidata piattaforma Web dall'indirizzo seguente:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>Progetto ASP.NET MVC

Progetto di applicazione Web ASP.NET MVC. Avviare Visual Studio e selezionare l'opzione di menu **File, nuovo progetto**. Il **nuovo progetto** (vedere la figura 1) viene visualizzata la finestra. Selezionare il **Web** tipo di progetto e il **applicazione Web ASP.NET MVC** modello. Denominare il nuovo progetto *ContactManager* e fare clic sul pulsante OK.


Assicurarsi di aver selezionato nell'elenco a discesa nella parte superiore di .NET Framework 3.5 a destra il **nuovo progetto** finestra di dialogo. In caso contrario, il modello di applicazione Web MVC ASP.NET ha vinto t visualizzato.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image1.jpg)](iteration-1-create-the-application-cs/_static/image1.png)

**Figura 01**: finestra di dialogo Nuovo progetto di ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image2.png))


Applicazione ASP.NET MVC, il **Crea progetto Unit Test** viene visualizzata la finestra. È possibile utilizzare questa finestra di dialogo per indicare che si desidera creare e aggiungere un progetto unit test alla soluzione quando si crea l'applicazione ASP.NET MVC. Anche se abbiamo vinto t la creazione di unit test in questa iterazione, è necessario selezionare l'opzione **Sì, è possibile creare un progetto di unit test** perché si prevede di aggiungere unit test in un'iterazione successiva. Aggiunta di un progetto di Test quando si crea un nuovo progetto ASP.NET MVC è molto più semplice rispetto all'aggiunta di un progetto di Test dopo aver creato il progetto ASP.NET MVC.

> [!NOTE] 
> 
> Poiché Visual Web Developer non supporta progetti di Test, non si ottiene la finestra di dialogo Crea progetto Unit Test quando si utilizza Visual Web Developer.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image2.jpg)](iteration-1-create-the-application-cs/_static/image3.png)

**Figura 02**: finestra di dialogo di creazione progetto Unit Test ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image4.png))


Applicazione MVC ASP.NET viene visualizzato nella finestra Esplora soluzioni di Visual Studio (vedere la figura 3). Se non si t, vedere la finestra Esplora soluzioni, quindi è possibile aprire questa finestra selezionando l'opzione di menu **Esplora soluzioni, visualizzazione**. Si noti che la soluzione contiene due progetti: il progetto ASP.NET MVC e il progetto di Test. Il progetto ASP.NET MVC viene denominato ContactManager e il progetto di Test è denominato ContactManager.Tests.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image3.jpg)](iteration-1-create-the-application-cs/_static/image5.png)

**Figura 03**: finestra di Esplora soluzioni ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image6.png))


## <a name="deleting-the-project-sample-files"></a>Eliminare i file di esempio di progetto

Il modello di progetto ASP.NET MVC include i file di esempio per i controller e visualizzazioni. Prima di creare una nuova applicazione MVC ASP.NET, è necessario eliminare questi file. È possibile eliminare file e cartelle in Esplora soluzioni facendo clic su un file o cartella e selezionando l'opzione di menu **Elimina**.

È necessario eliminare i file seguenti dal progetto ASP.NET MVC:

- \Controllers\HomeController.cs

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

E, è necessario eliminare i file seguente dal progetto di Test:

\Controllers\HomeControllerTest.cs

## <a name="creating-the-database"></a>Creazione del Database

L'applicazione Contact Manager è un'applicazione web basato su database. Utilizziamo un database per archiviare le informazioni di contatto.

Il framework di MVC ASP.NET con qualsiasi database moderni, inclusi i database di Microsoft SQL Server, Oracle, MySQL e IBM DB2. In questa esercitazione, si usa un database Microsoft SQL Server. Quando si installa Visual Studio, sono disponibili con l'opzione di installazione di Microsoft SQL Server Express che è una versione gratuita del database Microsoft SQL Server.

Creare un nuovo database facendo clic con l'App\_cartella di dati nella finestra Esplora soluzioni e selezionando l'opzione di menu **Aggiungi, elemento nuovo**. Nel **Aggiungi nuovo elemento** finestra di dialogo, seleziona la **Data** categoria e il **Database di SQL Server** modello (vedere la figura 4). Denominare il nuovo database ContactManagerDB.mdf e fare clic sul pulsante OK.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image4.jpg)](iteration-1-create-the-application-cs/_static/image7.png)

**Figura 04**: crea un nuovo database di Microsoft SQL Server Express ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image8.png))


Dopo aver creato il nuovo database, il database viene visualizzato nell'App\_cartella di dati nella finestra Esplora soluzioni. Fare doppio clic sul file ContactManager.mdf per aprire la finestra di Esplora Server e connettersi al database.

> [!NOTE] 
> 
> Nel caso di Microsoft Visual Web Developer la finestra di Esplora Server è denominata Esplora Database.


È possibile usare la finestra Esplora Server per creare nuovi oggetti di database quali tabelle del database, viste, trigger e stored procedure. Fare doppio clic sulla cartella tabelle e selezionare l'opzione di menu **Aggiungi nuova tabella**. La progettazione di tabelle di Database viene visualizzata (vedere la figura 5).


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image5.jpg)](iteration-1-create-the-application-cs/_static/image9.png)

**Figura 05**: la progettazione di tabelle di Database ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image10.png))


È necessario creare una tabella che contiene le colonne seguenti:

<a id="0.1_table01"></a>


| **Nome della colonna** | **Tipo di dati** | **Consenti valori null** |
| --- | --- | --- |
| Id | int | False |
| FirstName | nvarchar (50) | False |
| LastName | nvarchar (50) | False |
| Telefono | nvarchar (50) | False |
| Email | nvarchar(255 | False |


La prima colonna, la colonna Id, è speciale. È necessario contrassegnare la colonna Id come una colonna Identity e una colonna chiave primaria. Si indica che una colonna è una colonna Identity espandendo proprietà colonna (aspetto nella parte inferiore della figura 6) e lo scorrimento fino alla proprietà specifica Identity. Impostare il **(identità)** il valore della proprietà **Yes**.

Una colonna contrassegnata come colonna chiave primaria, selezionare la colonna e facendo clic sul pulsante con l'icona di una chiave. Dopo che una colonna è contrassegnata come colonna chiave primaria, accanto alla colonna viene visualizzata un'icona di una chiave (vedere la figura 6).

Dopo aver completato la creazione della tabella, fare clic sul pulsante Salva (il pulsante con un'icona di un disco floppy) per salvare la nuova tabella. Denominare la nuova tabella *contatti*.

Dopo la creazione della tabella di database dei contatti di fine, è necessario aggiungere alcuni record alla tabella. Fare doppio clic nella tabella di contatti nella finestra di Esplora Server e selezionare l'opzione di menu **Mostra dati tabella**. Immettere uno o più contatti nella griglia visualizzata.

## <a name="creating-the-data-model"></a>Creazione del modello di dati

L'applicazione ASP.NET MVC è costituita da modelli, visualizzazioni e controller. Si inizia creando una classe di modello che rappresenta la tabella dei contatti creata nella sezione precedente.

In questa esercitazione verrà usato il Framework di entità di Microsoft per generare automaticamente una classe di modello dal database.

> [!NOTE] 
> 
> Il framework ASP.NET MVC non è associato a Microsoft Entity Framework in alcun modo. È possibile usare ASP.NET MVC con tecnologie di accesso ai database alternativi NHibernate, LINQ to SQL o ADO.NET incluse.


Seguire questi passaggi per creare le classi di modello di dati:

1. La cartella Models nella finestra Esplora soluzioni e scegliere **Aggiungi, elemento nuovo**. Ad esempio, la logica di accesso del database è incorporata destra nelle azioni del controller.
2. Ciò significa che non è possibile modificare il codice di accesso ai dati senza modificare il controller. In iterazioni successive, vengono illustrati i modelli di progettazione software che è possibile implementare per rendere più resilienti per modificare la gestione di contatto. La procedura guidata Entity Data Model viene visualizzata (vedere la figura 7).
3. Nel **Scegli contenuto Model** passaggio, seleziona **genera da database** (vedere la figura 7).
4. Nel **Seleziona connessione dati** passaggio, selezionare il database ContactManagerDB.mdf e immettere il nome *ContactManagerDBEntities* per le impostazioni di connessione Entity (vedere la figura 8).
5. Nel **Scegli oggetti di Database** passaggio, selezionare la casella di controllo tabelle (vedere la figura 9). Il modello di dati includerà tutte le tabelle contenute nel database (vi è solo una, la tabella Contacts). Immettere lo spazio dei nomi *modelli*. Fare clic sul pulsante Fine per completare la procedura guidata.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image6.jpg)](iteration-1-create-the-application-cs/_static/image11.png)

**Figura 06**: finestra di dialogo di Aggiungi nuovo elemento ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image12.png))


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image7.jpg)](iteration-1-create-the-application-cs/_static/image13.png)

**Figura 07**: Scegli contenuto Model ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image14.png))


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image8.jpg)](iteration-1-create-the-application-cs/_static/image15.png)

**Figura 08**: Seleziona connessione dati ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image16.png))


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image9.jpg)](iteration-1-create-the-application-cs/_static/image17.png)

**Figura 09**: Seleziona oggetti di Database ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image18.png))


Dopo aver completato la procedura guidata Entity Data Model, verrà visualizzato Entity Data Model Designer. La finestra di progettazione consente di visualizzare una classe che corrisponde a ogni tabella da modellare. Dovrebbe essere una classe denominata Contacts.

La procedura guidata Entity Data Model genera i nomi delle classi in base ai nomi delle tabelle di database. È quasi sempre necessario modificare il nome della classe generata dalla procedura guidata. La classe Contacts nella finestra di progettazione e scegliere l'opzione di menu **Rinomina**. Modificare il nome della classe dai contatti (plurali) al contatto (singolo). Dopo aver modificato il nome della classe, la classe risulterà a quello della figura 10.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image10.jpg)](iteration-1-create-the-application-cs/_static/image19.png)

**Figura 10**: classe Contact The ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image20.png))


A questo punto, è stata creata il nostro modello di database. È possibile usare la classe di contatto per rappresentare un record di contatto specifico nel nostro database.

## <a name="creating-the-home-controller"></a>Creazione del Controller Home

Il passaggio successivo consiste nel creare il controller Home. Il controller Home è il controller predefinito richiamato in un'applicazione ASP.NET MVC.

Creare la classe controller Home page facendo clic sulla cartella controller in Esplora soluzioni e selezionando l'opzione di menu **Controller, Aggiungi** (vedere la figura 11). Si noti che la casella di controllo etichettato **aggiungere i metodi di azione per gli scenari Create, Update e dettagli**. Assicurarsi che sia selezionata questa casella di controllo prima di scegliere la **Add** pulsante.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image11.jpg)](iteration-1-create-the-application-cs/_static/image21.png)

**Figura 11**: aggiunta del controller Home ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image22.png))


Quando si crea il controller Home, si ottiene la classe nel listato 1.

**Listato 1 - controllers\homecontroller.cs.**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample1.cs)]

## <a name="listing-the-contacts"></a>Elenca i contatti

Per visualizzare i record nella tabella del database dei contatti, è necessario creare un'azione Index () e una visualizzazione indice.

Il controller Home contiene già un'azione Index (). È necessario modificare questo metodo in modo che risulti come listato 2.

**Listato 2 - controllers\homecontroller.cs.**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample2.cs)]

Si noti che la classe controller Home nel listato 2 contiene un campo privato denominato \_entità. Il \_campo entità rappresenta le entità del modello di dati. Usiamo il \_entità campo per comunicare con il database.

Il metodo Index () restituisce una visualizzazione che rappresenta tutti i contatti dalla tabella di database dei contatti. L'espressione \_entità. ContactSet.ToList() restituisce l'elenco dei contatti come un elenco generico.

Ora che abbiamo creato il controller di indice, è quindi necessario creare la visualizzazione dell'indice. Prima di creare la visualizzazione dell'indice, compilare l'applicazione selezionando l'opzione di menu **compilazione, Compila soluzione**. È sempre necessario compilare il progetto prima di aggiungere una visualizzazione in ordine per un elenco delle classi del modello da visualizzare nella **Aggiungi visualizzazione** finestra di dialogo.

Si crea la visualizzazione dell'indice facendo clic sul metodo Index () e selezionando l'opzione di menu **Aggiungi visualizzazione** (vedere la figura 12). Selezionando questa opzione di menu si apre la **Aggiungi visualizzazione** finestra di dialogo (vedere la figura 13).


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image12.jpg)](iteration-1-create-the-application-cs/_static/image23.png)

**Figura 12**: aggiunta della visualizzazione Index ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image24.png))


Nel **Aggiungi visualizzazione** finestra di dialogo, selezionare la casella di controllo etichettato **creare una visualizzazione fortemente tipizzata**. Selezionare la classe di dati di visualizzazione ContactManager.Models.Contact e la Visualizza elenco del contenuto. Selezionando queste opzioni genera una vista che visualizza un elenco di record di contatto.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image13.jpg)](iteration-1-create-the-application-cs/_static/image25.png)

**Figura 13**: finestra di dialogo la visualizzazione aggiunta ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image26.png))


Quando si sceglie la **Add** pulsante, la visualizzazione dell'indice nel listato 3 viene generato. Si noti che il &lt;% @ % pagina&gt; direttiva che viene visualizzato nella parte superiore del file. La visualizzazione dell'indice eredita il ViewPage&lt;IEnumerable&lt;ContactManager.Models.Contact&gt; &gt; classe. In altre parole, la classe del modello nella visualizzazione rappresenta un elenco di entità Contact.

Il corpo della visualizzazione Index contiene un ciclo foreach che scorre ogni contatto rappresentato dalla classe modello. Il valore di ogni proprietà della classe contatto viene visualizzato all'interno di una tabella HTML.

**Listato 3 - Views\Home\Index.aspx. (non modificato)**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample3.aspx)]

È necessario apportare una modifica alla vista Index. Perché non si sta creando una visualizzazione dei dettagli, è possibile rimuovere il collegamento di dettagli. Trovare e rimuovere il codice seguente dalla visualizzazione Index:

{id elemento =. % Di ID})&gt;

Dopo aver modificato la visualizzazione dell'indice, è possibile eseguire l'applicazione Contact Manager. Selezionare l'opzione di menu Debug, Avvia debug o è sufficiente premere F5. La prima volta che si esegue l'applicazione, otterrai la finestra di dialogo nella figura 14. Selezionare l'opzione **modificare il file Web. config per abilitare il debug** e fare clic sul pulsante OK.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image14.jpg)](iteration-1-create-the-application-cs/_static/image27.png)

**Figura 14**: abilitazione del debug ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image28.png))


La visualizzazione dell'indice viene restituita per impostazione predefinita. In questa vista sono elencati tutti i dati dalla tabella di database dei contatti (vedere Figura 15).


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image15.jpg)](iteration-1-create-the-application-cs/_static/image29.png)

**Figura 15**: Visualizza l'indice ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image30.png))


Si noti che la visualizzazione dell'indice include un collegamento con l'etichetta Crea nuovo nella parte inferiore della visualizzazione. Nella sezione successiva imparare a creare nuovi contatti.

## <a name="creating-new-contacts"></a>Creazione di nuovi contatti

Per consentire agli utenti di creare nuovi contatti, è necessario aggiungere due azioni create () del controller Home. È necessario creare una sola azione create () che restituisce un form HTML per la creazione di un nuovo contatto. È necessario creare una seconda azione create () che esegue l'inserimento effettivo del database del nuovo contatto.

I nuovi metodi Create () che è necessario aggiungere al controller Home sono contenuti nel listato 4.

**Listato 4 - Controllers\HomeController.cs (con i metodi di creazione)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample4.cs)]

Il primo metodo Create () può essere richiamato con una richiesta HTTP GET, mentre il secondo metodo Create () può essere richiamato solo da una richiesta HTTP POST. In altre parole, il secondo metodo Create () può essere richiamato solo quando si registra un form HTML. Il primo metodo Create () restituisce semplicemente una vista che contiene il form HTML per la creazione di un nuovo contatto. Il secondo metodo Create () è molto più interessante: aggiunge il nuovo contatto nel database.

Si noti che il secondo metodo Create () è stato modificato per accettare un'istanza della classe Contact. I valori di form inseriti dal form HTML vengono automaticamente associati a questa classe di contatto dal framework ASP.NET MVC. Ogni campo del form dal form HTML creare viene assegnato a una proprietà del parametro di contatto.

Si noti che il parametro contatto è decorato con un attributo [Bind]. L'attributo [Bind] viene usato per escludere la proprietà Id contatto dall'associazione. Poiché la proprietà Id rappresenta una proprietà Identity, non abbiamo t desidera impostare la proprietà Id.

Nel corpo del metodo Create (), Entity Framework consente di inserire il nuovo contatto nel database. Il nuovo contatto viene aggiunto al set esistente di contatti e viene chiamato il metodo SaveChanges () per reinserire le modifiche al database sottostante.

È possibile generare un form HTML per la creazione di nuovi contatti facendo clic su uno dei due metodi Create () e selezionando l'opzione di menu **Aggiungi visualizzazione** (vedere Figura 16).


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image16.jpg)](iteration-1-create-the-application-cs/_static/image31.png)

**Figura 16**: aggiunta della visualizzazione di creazione ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image32.png))


Nel **Aggiungi visualizzazione** finestra di dialogo, seleziona la **ContactManager.Models.Contact** classe e il **Create** opzione per visualizzare il contenuto (vedere Figura 17). Quando si fa clic il **Add** pulsante, una creazione vista viene generata automaticamente.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image17.jpg)](iteration-1-create-the-application-cs/_static/image33.png)

**Figura 17**: viene visualizzata una pagina explode ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image34.png))


Crea visualizzazione contiene i campi del modulo per ognuna delle proprietà della classe di contatto. Il codice per la visualizzazione di creazione è incluso nel listato 5.

**Listato 5 - Views\Home\Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample5.aspx)]

Dopo avere modificato i metodi Create () e aggiungere la visualizzazione di creazione, è possibile eseguire l'applicazione Contact Manager e creare nuovi contatti. Scegliere il **Crea nuovo** collegamento visualizzato nella vista Index per passare alla visualizzazione di creazione. Verrà visualizzata la vista nella figura 18.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image18.jpg)](iteration-1-create-the-application-cs/_static/image35.png)

**Figura 18**: The Create View ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image36.png))


## <a name="editing-contacts"></a>Modifica dei contatti

Aggiunta della funzionalità per la modifica di un record di contatto è molto simile all'aggiunta di funzionalità per la creazione di nuovi record dei contatti. In primo luogo, è necessario aggiungere due nuovi metodi di modifica alla classe controller Home. Questi nuovi metodi Edit () sono contenuti nel listato 6.

**Listato 6 - Controllers\HomeController.cs (con i metodi di modifica)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample6.cs)]

Il primo metodo Edit () viene richiamato da un'operazione HTTP GET. Un parametro Id viene passato al metodo che rappresenta l'Id del record di contatto in fase di modifica. Entity Framework viene usato per recuperare un contatto che corrisponde all'ID. Viene restituita una visualizzazione contenente un form HTML per la modifica di un record.

Il secondo metodo Edit () esegue l'aggiornamento effettivo per il database. Questo metodo accetta un'istanza della classe contatto come parametro. Il framework ASP.NET MVC associa i campi del modulo dal modulo di modifica a questa classe automaticamente. Si noti che non si t include l'attributo [Bind] durante la modifica di un contatto (è necessario il valore della proprietà Id).

Entity Framework consente di salvare il contatto modificato nel database. Il contatto originale deve essere recuperato dal database prima di tutto. Successivamente, viene chiamato il metodo di Entity Framework ApplyPropertyChanges() per registrare le modifiche apportate al contatto. Infine, viene chiamato il metodo SaveChanges () di Entity Framework per rendere permanenti le modifiche al database sottostante.

È possibile generare la vista che contiene il modulo di modifica facendo clic sul metodo Edit () e selezionando l'opzione di menu Aggiungi visualizzazione. Nella finestra di dialogo Aggiungi visualizzazione, selezionare la **ContactManager.Models.Contact** classe e il **modificare** visualizzare il contenuto (vedere la figura 19).


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image19.jpg)](iteration-1-create-the-application-cs/_static/image37.png)

**Figura 19**: aggiunta di una visualizzazione di modifica ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image38.png))


Quando si fa clic sul pulsante Aggiungi, viene generata automaticamente una nuova visualizzazione di modifica. Il form HTML generato contiene i campi corrispondenti a ognuna delle proprietà della classe contatto (vedere listato 7).

**Listato 7 - Views\Home\Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>L'eliminazione di contatti

Se si desidera eliminare contatti, quindi è necessario aggiungere due azioni Delete () alla classe controller Home. La prima azione Delete () consente di visualizzare un form di conferma delete. La seconda azione Delete () esegue l'eliminazione effettiva.

> [!NOTE] 
> 
> In un secondo momento, in iterazione #7, viene modificato di Contact Manager, in modo che supporti un unico passaggio Elimina Ajax.


I due nuovi metodi Delete () sono contenuti nel listato 8.

**Listato 8 - Controllers\HomeController.cs (metodi di eliminazione)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample8.cs)]

Il primo metodo Delete () restituisce un formato di conferma per l'eliminazione di un record di contatto dal database (vedere Figure20). Il secondo metodo Delete () esegue l'operazione di eliminazione sul database. Dopo il contatto originale è stato recuperato dal database, vengono chiamati i metodi di DeleteObject () di Entity Framework e SaveChanges () per eseguire l'eliminazione di database.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image20.jpg)](iteration-1-create-the-application-cs/_static/image39.png)

**Figura 20**: la visualizzazione di conferma delete ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image40.png))


È necessario modificare la visualizzazione dell'indice, in modo che contenga un collegamento per l'eliminazione di record dei contatti (vedere Figura 21). È necessario aggiungere il codice seguente per la stessa cella di tabella che contiene il collegamento di modifica:

HTML. ActionLink ({id elemento =. % Di ID})&gt;


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image21.jpg)](iteration-1-create-the-application-cs/_static/image41.png)

**Figura 21**: indicizzare la vista con collegamento di modifica ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image42.png))


Successivamente, è necessario creare la visualizzazione di conferma di eliminazione. Fare doppio clic il metodo Delete () nella classe controller Home e selezionare l'opzione di menu Aggiungi visualizzazione. La finestra di dialogo Aggiungi visualizzazione viene visualizzata (vedere la figura 22).

A differenza nel caso le visualizzazioni elenco, creazione e modifica, la finestra di dialogo Aggiungi visualizzazione non contiene un'opzione per creare una visualizzazione di eliminazione. In alternativa, selezionare la **ContactManager.Models.Contact** classi di dati e il **vuota** visualizzare il contenuto. Selezione della vista vuota opzione content richiederà di creare la vista noi stessi.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image22.jpg)](iteration-1-create-the-application-cs/_static/image43.png)

**Figura 22**: aggiunta della visualizzazione di conferma delete ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image44.png))


Il contenuto della visualizzazione Delete è contenuto nel listato 9. In questa vista contiene un modulo che viene confermata o meno un contatto specifico deve essere eliminato (vedere Figura 21).

**Listato 9 - Views\Home\Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>La modifica del nome del Controller predefinito

Potrebbe essere necessario creare anche è che il nome della nostra classe di controller per l'utilizzo dei contatti è possibile denominare la classe HomeController. Il controller denominato t non deve ContactController?

È abbastanza semplice risolvere questo problema. Innanzitutto, è necessario effettuare il refactoring il nome del controller Home. Aprire la classe HomeController nell'Editor di codice di Visual Studio, pulsante destro del mouse sul nome della classe e selezionare l'opzione di menu **effettuare il refactoring, ridenominazione**. Selezionando questa opzione di menu viene visualizzata la finestra di dialogo di ridenominazione.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image23.jpg)](iteration-1-create-the-application-cs/_static/image45.png)

**Figura 23**: il Refactoring di un nome del controller ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image46.png))


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image24.jpg)](iteration-1-create-the-application-cs/_static/image47.png)

**Figura 24**: tramite la finestra di dialogo di ridenominazione ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image48.png))


Se si rinomina la classe controller, Visual Studio aggiornerà il nome della cartella nella cartella Views anche. Visual Studio verrà rinominare la cartella \Views\Home nella cartella \Views\Contact.

Dopo aver apportato questa modifica, l'applicazione non sarà più possibile un controller Home. Quando si esegue l'applicazione, si otterrà la pagina di errore nella figura 25.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-cs/_static/image25.jpg)](iteration-1-create-the-application-cs/_static/image49.png)

**Figura 25**: nessun controller predefinito ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-1-create-the-application-cs/_static/image50.png))


È necessario aggiornare la route predefinita nel file Global. asax per utilizzare il controller contattare invece il controller Home. Aprire il file Global. asax e modificare il controller predefinito usato dalla route predefinita (vedere il listato 10).

**Listato 10 - Global.asax.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample10.cs)]

Dopo aver apportato queste modifiche, il responsabile del contatto verrà eseguito correttamente. A questo punto, utilizzerà la classe di controller contatto come il controller predefinito.

## <a name="summary"></a>Riepilogo

In questa prima iterazione, abbiamo creato un'applicazione Contact Manager nel modo più rapido possibili. Abbiamo sfruttato di Visual Studio per generare automaticamente il codice iniziale per il controller e visualizzazioni. Abbiamo inoltre sfruttato di Entity Framework per generare automaticamente le classi di modello di database.

Attualmente, è possibile elencare, creare, modificare ed eliminare record dei contatti con l'applicazione di gestione dei contatti. In altre parole, è possibile eseguire tutte le operazioni di base dei database richieste da un'applicazione web basato su database.

Sfortunatamente, l'applicazione presenta alcuni problemi. Prima e si sono riluttanti a ammetterlo, l'applicazione Contact Manager non è l'applicazione più interessante. È necessario che alcune attività di progettazione. Nell'iterazione successiva, esamineremo come è possibile modificare la pagina master visualizzazione predefinita e un foglio di stile CSS per migliorare l'aspetto dell'applicazione.

In secondo luogo, non è stata implementata alcuna convalida del form. Ad esempio, vi è nulla a che non consentono di inviare il modulo di contatto di creazione senza dover immettere i valori per uno qualsiasi dei campi del modulo. Inoltre, è possibile immettere gli indirizzi di posta elettronica e numeri di telefono non valido. Iniziamo affrontare il problema di convalida dei form nell'iterazione #3.

Infine e ancora più importante, l'iterazione corrente dell'applicazione Contact Manager non può essere facilmente modificato o mantenuto. Ad esempio, la logica di accesso del database è incorporata destra nelle azioni del controller. Ciò significa che non è possibile modificare il codice di accesso ai dati senza modificare il controller. In iterazioni successive, vengono illustrati i modelli di progettazione software che è possibile implementare per rendere più resilienti per modificare la gestione di contatto.

> [!div class="step-by-step"]
> [avanti](iteration-2-make-the-application-look-nice-cs.md)
