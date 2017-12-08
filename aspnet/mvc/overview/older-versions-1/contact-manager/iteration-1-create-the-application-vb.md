---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
title: 'Iterazione #1: creare l''applicazione (VB) | Documenti Microsoft'
author: microsoft
description: "Nella prima iterazione, verranno create Contact Manager in modo più semplice possibile. Viene aggiunto il supporto per le operazioni di database basic: creazione, lettura, aggiornamento e D...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 5b033582-1646-42c2-b20d-7edc8814e970
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
msc.type: authoredcontent
ms.openlocfilehash: 11d3d4f174207f5370849fdf4517f272b4b6bc6b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="iteration-1--create-the-application-vb"></a>Iterazione #1: creare l'applicazione (VB)
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare il codice](iteration-1-create-the-application-vb/_static/contactmanager_1_vb1.zip)

> Nella prima iterazione, verranno create Contact Manager in modo più semplice possibile. Viene aggiunto il supporto per le operazioni di database basic: creazione, lettura, aggiornamento ed eliminazione (CRUD).


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Creazione di un'applicazione ASP.NET MVC di gestione dei contatti (VB)

In questa serie di esercitazioni, si compila un'intera applicazione di gestione dei contatti dall'inizio completamento. L'applicazione Contact Manager consente di archiviare le informazioni di contatto - nomi, i numeri di telefono e indirizzi di posta elettronica: per un elenco di persone.

È compilare l'applicazione più iterazioni. A ogni iterazione, gradualmente è migliorare l'applicazione. L'obiettivo di questo approccio iterazione più è che consentono di comprendere il motivo per ogni modifica.

- Iterazione #1 - creare l'applicazione. Nella prima iterazione, verranno create Contact Manager in modo più semplice possibile. Viene aggiunto il supporto per le operazioni di database basic: creazione, lettura, aggiornamento ed eliminazione (CRUD).

- Iterazione #2 - verificare l'applicazione l'aspetto. In questa iterazione, è migliorare l'aspetto dell'applicazione modificando il valore predefinito di pagina master di visualizzazione ASP.NET MVC e foglio di stile CSS.

- Iterazione #3 - aggiungere la convalida dei form. Nella terza iterazione, è aggiungere la convalida di form di base. È impedire agli utenti di inviare un modulo senza completare i campi modulo necessari. È inoltre possibile convalidare gli indirizzi di posta elettronica e numeri di telefono.

- Iterazione #4: verificare l'applicazione ad accoppiamento debole. In questa terza iterazione, possiamo usufruire dei diversi modelli di progettazione di software per renderne più semplice gestire e modificare l'applicazione Gestione contatti. Ad esempio, si effettua il refactoring l'applicazione di utilizzare il modello di Repository e il modello di inserimento di dipendenze.

- Iterazione #5 - creare unit test. Nella quinta iterazione, si rende l'applicazione di più facile da gestire e modificare tramite l'aggiunta di unit test. È simulare il nostro classi del modello di dati e generare unit test per i controller e logica di convalida.

- Iterazione &#6; - utilizzare sviluppo basato su test. In questa iterazione sesto è aggiungere nuove funzionalità per l'applicazione scrivendo unit test prima e la scrittura di codice per gli unit test. In questa iterazione, è aggiungere gruppi di contatti.

- Iterazione #7 - aggiunta di funzionalità Ajax. Nella settima iterazione, è migliorare la velocità di risposta e prestazioni dell'applicazione aggiunta del supporto per Ajax.

## <a name="this-iteration"></a>Questa iterazione

In questa prima iterazione, si compila l'applicazione di base. L'obiettivo consiste nel compilare il responsabile del contatto nel modo più semplice e rapido possibile. Nelle iterazioni successive, è migliorare la progettazione dell'applicazione.

L'applicazione Contact Manager è un'applicazione basata su database di base. Per creare nuovi contatti, modificare i contatti esistenti ed eliminare contatti, è possibile utilizzare l'applicazione.

In questa iterazione, è la procedura seguente:

1. Applicazione MVC ASP.NET
2. Creare un database per archiviare i contatti
3. Generare una classe di modello per il database con Entity Framework Microsoft
4. Creare un'azione del controller e la visualizzazione che consente di elencare tutti i contatti nel database
5. Creazione di azioni del controller e una visualizzazione che consente di creare un nuovo contatto nel database
6. Creazione di azioni del controller e una visualizzazione che consente di modificare un contatto esistente nel database
7. Creazione di azioni del controller e una visualizzazione che consente di eliminare un contatto esistente nel database

## <a name="software-prerequisites"></a>Prerequisiti software

Nelle applicazioni ASP.NET MVC, è necessario disporre di Visual Studio 2008 o Visual Web Developer 2008 installato nel computer (Visual Web Developer è una versione gratuita di Visual Studio che non include tutte le funzionalità avanzate di Visual Studio). È possibile scaricare la versione di valutazione di Visual Studio 2008 o Visual Web Developer dall'indirizzo seguente:

[https://www.ASP.NET/downloads/Essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> Per le applicazioni ASP.NET MVC con Visual Web Developer, è necessario disporre di Visual Web Developer Service Pack 1 installato. Senza Service Pack 1, è possibile creare progetti di applicazione Web.


Framework di MVC ASP.NET. È possibile scaricare il framework ASP.NET MVC dall'indirizzo seguente:

[https://www.ASP.NET/MVC](../../../index.md)

In questa esercitazione, verrà usato il Framework di entità di Microsoft per accedere a un database. Entity Framework è incluso in .NET Framework 3.5 Service Pack 1. È possibile scaricare questo service pack dal percorso seguente:

[https://www.microsoft.com/downloads/details.aspx?FamilyID=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang = en](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

In alternativa all'esecuzione di ognuno di questi download uno alla volta, è possibile sfruttare l'installazione guidata piattaforma Web (PI Web). È possibile scaricare l'installazione guidata piattaforma Web dall'indirizzo seguente:

[https://www.ASP.NET/downloads/Essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>Progetto ASP.NET MVC

Progetto di applicazione Web ASP.NET MVC. Avviare Visual Studio e selezionare l'opzione di menu **File, nuovo progetto**. Il **nuovo progetto** (vedere la figura 1) viene visualizzata la finestra. Selezionare il **Web** tipo di progetto e **applicazione Web ASP.NET MVC** modello. Nome del nuovo progetto *ContactManager* e fare clic sul pulsante OK.


Assicurarsi di disporre di .NET Framework 3.5 sia selezionata nell'elenco a discesa nella parte superiore destra del **nuovo progetto** finestra di dialogo. In caso contrario, il modello di applicazione Web ASP.NET MVC vinto t visualizzato.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image1.jpg)](iteration-1-create-the-application-vb/_static/image1.png)

**Figura 01**: finestra di dialogo Nuovo progetto il ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image2.png))


Applicazione MVC ASP.NET il **Crea progetto Unit Test** viene visualizzata la finestra. È possibile utilizzare questa finestra di dialogo per indicare che si desidera creare e aggiungere un progetto di unit test alla soluzione quando si crea un'applicazione ASP.NET MVC. Anche se è stata acquisita t la creazione di unit test in questa iterazione, è necessario selezionare l'opzione **Sì, crea un progetto di unit test** perché si prevede di aggiungere unit test in un'iterazione successiva. Aggiunta di un progetto di Test quando si crea un nuovo progetto ASP.NET MVC è molto più semplice rispetto all'aggiunta di un progetto di Test dopo aver creato il progetto ASP.NET MVC.

> [!NOTE] 
> 
> Poiché Visual Web Developer non supporta i progetti di Test, non si ottiene la finestra di dialogo Crea progetto Unit Test quando si utilizza Visual Web Developer.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image2.jpg)](iteration-1-create-the-application-vb/_static/image3.png)

**Figura 02**: finestra di dialogo di creazione progetto Unit Test ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image4.png))


Applicazione MVC ASP.NET viene visualizzata nella finestra Esplora soluzioni di Visual Studio (vedere la figura 3). Se non si t, vedere la finestra Esplora soluzioni è possibile aprire questa finestra, selezionare l'opzione di menu **Visualizza, Esplora soluzioni**. Si noti che la soluzione contiene due progetti: il progetto ASP.NET MVC e il progetto di Test. Il progetto ASP.NET MVC è denominato ContactManager e il progetto di Test viene denominato ContactManager.Tests.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image3.jpg)](iteration-1-create-the-application-vb/_static/image5.png)

**Figura 03**: finestra di Esplora soluzioni ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image6.png))


## <a name="deleting-the-project-sample-files"></a>L'eliminazione dei file di esempio di progetto

Il modello di progetto ASP.NET MVC include i file di esempio per controller e visualizzazioni. Prima di creare una nuova applicazione MVC ASP.NET, è necessario eliminare tali file. È possibile eliminare file e cartelle nella finestra Esplora soluzioni destro del mouse su un file o cartella e selezionando l'opzione di menu **eliminare**.

È necessario eliminare i file seguenti dal progetto ASP.NET MVC:

- \Controllers\HomeController.vb

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

Inoltre, è necessario eliminare il file seguente dal progetto di Test:

\Controllers\HomeControllerTest.vb

## <a name="creating-the-database"></a>Creazione del Database

L'applicazione Contact Manager è un'applicazione web basata su database. Si utilizza un database per archiviare le informazioni di contatto.

Il framework di MVC ASP.NET con qualsiasi moderna database, inclusi i database di Microsoft SQL Server, Oracle, MySQL e IBM DB2. In questa esercitazione, si utilizza un database di Microsoft SQL Server. Quando si installa Visual Studio, sono disponibili con l'opzione di installazione di Microsoft SQL Server Express che è una versione gratuita di database di Microsoft SQL Server.

Creare un nuovo database facendo clic con il App\_cartella dati nella finestra Esplora soluzioni e selezionando l'opzione di menu **Aggiungi, elemento nuovo**. Nel **Aggiungi nuovo elemento** finestra di dialogo Seleziona il **dati** categoria e **Database di SQL Server** modello (vedere la figura 4). Denominare il nuovo database ContactManagerDB.mdf e fare clic sul pulsante OK.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image4.jpg)](iteration-1-create-the-application-vb/_static/image7.png)

**Figura 04**: creazione di un nuovo database di Microsoft SQL Server Express ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image8.png))


Dopo aver creato il nuovo database, il database viene visualizzato nell'App\_cartella di dati nella finestra Esplora soluzioni. Fare doppio clic sul file di ContactManager.mdf per aprire la finestra di Esplora Server e connettersi al database.

> [!NOTE] 
> 
> La finestra di Esplora Server è denominata la finestra Esplora Database nel caso di Microsoft Visual Web Developer.


È possibile utilizzare la finestra di Esplora Server per creare nuovi oggetti di database quali tabelle, viste, trigger e stored procedure. Fare doppio clic sulla cartella tabelle e selezionare l'opzione di menu **Aggiungi nuova tabella**. La progettazione di tabelle di Database viene visualizzato (vedere Figura 5).


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image5.jpg)](iteration-1-create-the-application-vb/_static/image9.png)

**Figura 05**: la progettazione di tabelle di Database ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image10.png))


È necessario creare una tabella che contiene le colonne seguenti:

<a id="0.2_table01"></a>


| **Nome colonna** | **Tipo di dati** | **Consenti valori null** |
| --- | --- | --- |
| Id | int | False |
| FirstName | nvarchar (50) | False |
| LastName | nvarchar (50) | False |
| Telefono | nvarchar (50) | False |
| Posta elettronica | nvarchar (255) | False |


La prima colonna, la colonna Id, è speciale. È necessario contrassegnare la colonna Id come una colonna Identity e una colonna chiave primaria. Si indica che una colonna è una colonna di identità mediante l'espansione delle proprietà di colonna (aspetto nella parte inferiore della figura 6) e individuare la proprietà specifica identità di scorrimento. Impostare il **(identità)** il valore della proprietà **Sì**.

Una colonna contrassegnata come colonna chiave primaria, selezionare la colonna e fare clic sul pulsante con l'icona di una chiave. Dopo che una colonna è contrassegnata come colonna chiave primaria, viene visualizzata un'icona di una chiave accanto alla colonna (vedere Figura 6).

Dopo aver completato la creazione della tabella, fare clic sul pulsante Salva (il pulsante con un'icona di un disco floppy) per salvare la nuova tabella. Denominare la nuova tabella *contatti*.

Dopo la creazione della tabella di database dei contatti di fine, è consigliabile aggiungere alcuni record alla tabella. La tabella contatti nella finestra di Esplora Server e scegliere l'opzione di menu **Mostra dati tabella**. Nella griglia in cui viene visualizzata, immettere uno o più contatti.

## <a name="creating-the-data-model"></a>Creazione del modello di dati

L'applicazione ASP.NET MVC è costituita da modelli, visualizzazioni e controller. Si inizierà creando una classe modello che rappresenta la tabella contatti creato nella sezione precedente.

In questa esercitazione, verrà usato il Framework di entità di Microsoft per generare una classe di modello dal database automaticamente.

> [!NOTE] 
> 
> Il framework di MVC ASP.NET non è correlato a Entity Framework Microsoft in alcun modo. È possibile utilizzare ASP.NET MVC con tecnologie di accesso ai database alternativo inclusi NHibernate, LINQ to SQL o ADO.NET.


Seguire questi passaggi per creare le classi di modello di dati:

1. Fare clic sulla cartella modelli nella finestra Esplora soluzioni e selezionare **Aggiungi, elemento nuovo**. Il **Aggiungi nuovo elemento** viene visualizzata la finestra (vedere Figura 6).
2. Selezionare il **dati** categoria e **ADO.NET Entity Data Model** modello. Nome del modello di dati *ContactManagerModel.edmx* e fare clic su di **Aggiungi** pulsante. La procedura guidata Entity Data Model, viene visualizzato (vedere la figura 7).
3. Nel **Scegli contenuto Model** passaggio seleziona **genera da database** (vedere la figura 7).
4. Nel **Seleziona connessione dati** passaggio, selezionare il database ContactManagerDB.mdf e immettere il nome *ContactManagerDBEntities* per le impostazioni di connessione Entity (vedere la figura 8).
5. Nel **Seleziona oggetti di Database** passaggio, selezionare la casella di controllo con l'etichetta di tabelle (vedere Figura 9). Il modello di dati includerà tutte le tabelle contenute nel database (è disponibile una sola tabella Contacts). Immettere lo spazio dei nomi *modelli*. Fare clic sul pulsante Fine per completare la procedura guidata.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image6.jpg)](iteration-1-create-the-application-vb/_static/image11.png)

**Figura 06**: finestra di dialogo di Aggiungi nuovo elemento ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image12.png))


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image7.jpg)](iteration-1-create-the-application-vb/_static/image13.png)

**Figura 07**: Scegli contenuto Model ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image14.png))


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image8.jpg)](iteration-1-create-the-application-vb/_static/image15.png)

**Figura 08**: Seleziona connessione dati ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image16.png))


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image9.jpg)](iteration-1-create-the-application-vb/_static/image17.png)

**Figura 09**: Seleziona oggetti di Database ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image18.png))


Dopo aver completato la procedura guidata Entity Data Model, verrà visualizzato Entity Data Model Designer. La finestra di progettazione consente di visualizzare una classe che corrisponde a ogni tabella da modellare. Dovrebbe essere una classe denominata contatti.

La procedura guidata Entity Data Model genera i nomi delle classi in base ai nomi di tabella di database. È quasi sempre necessario modificare il nome della classe generata dalla procedura guidata. La classe contatti nella finestra di progettazione e scegliere l'opzione di menu **rinominare**. Modificare il nome della classe di contatti (plurali) al contatto (singolo). Dopo aver modificato il nome della classe, la classe dovrebbe essere visualizzato come nella figura 10.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image10.jpg)](iteration-1-create-the-application-vb/_static/image19.png)

**Figura 10**: il campo class ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image20.png))


A questo punto, è stato creato il modello di database. È possibile usare la classe di contatto per rappresentare un record di contatto specifico nel database.

## <a name="creating-the-home-controller"></a>Creazione di Controller Home

Il passaggio successivo consiste nel creare il controller Home. Il controller Home è il predefinito richiamato in un'applicazione MVC ASP.NET.

Creare la classe controller Home facendo clic sulla cartella controller nella finestra Esplora soluzioni e selezionando l'opzione di menu **Aggiungi, Controller** (vedere Figura 11). Si noti la casella di controllo con etichettata **aggiungere metodi di azione per gli scenari di creazione, aggiornamento e dettaglio**. Assicurarsi che questa casella di controllo è selezionata prima di scegliere il **Aggiungi** pulsante.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image11.jpg)](iteration-1-create-the-application-vb/_static/image21.png)

**Figura 11**: aggiunta del controller Home ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image22.png))


Quando si crea il controller Home, si ottiene la classe nel listato 1.

**Elenco 1 - Controllers\HomeController.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample1.vb)]

## <a name="listing-the-contacts"></a>Elenca i contatti

Per visualizzare i record nella tabella di database dei contatti, è necessario creare un'azione Index () e la visualizzazione di un indice.

Il controller Home contiene già un'azione Index (). È necessario modificare questo metodo in modo che risulti simile listato 2.

**Elenco di 2 - Controllers\HomeController.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample2.vb)]

Si noti che la classe controller Home nel listato 2 contiene un campo privato denominato \_entità. Il \_campo entità rappresenta l'entità del modello di dati. Utilizziamo la \_entità campo per comunicare con il database.

Il metodo Index () restituisce una vista che rappresenta tutti i contatti dalla tabella di database dei contatti. L'espressione \_entità. ContactSet.ToList() restituisce l'elenco dei contatti come un elenco generico.

Ora che abbiamo creato il controller di indice, è quindi necessario creare la visualizzazione dell'indice. Prima di creare la visualizzazione dell'indice, compilare l'applicazione selezionando l'opzione di menu **di compilazione, Compila soluzione**. È sempre necessario compilare il progetto prima di aggiungere una visualizzazione in ordine per un elenco di classi del modello da visualizzare nel **Aggiungi visualizzazione** finestra di dialogo.

Creare la visualizzazione dell'indice facendo clic il metodo Index () e selezionando l'opzione di menu **Aggiungi visualizzazione** (vedere Figura 12). Selezionando questa opzione di menu viene visualizzata la **Aggiungi visualizzazione** finestra di dialogo (vedere Figura 13).


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image12.jpg)](iteration-1-create-the-application-vb/_static/image23.png)

**Figura 12**: aggiunta della visualizzazione dell'indice ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image24.png))


Nel **Aggiungi visualizzazione** finestra di dialogo, selezionare la casella di controllo con etichettata **creare una visualizzazione fortemente tipizzata**. Selezionare la classe dati visualizzazione ContactManager.Contact e l'elenco di visualizzazione del contenuto. Selezionando queste opzioni genera una visualizzazione che consente di visualizzare un elenco di record di contatto.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image13.jpg)](iteration-1-create-the-application-vb/_static/image25.png)

**Figura 13**: finestra di dialogo di Aggiungi visualizzazione ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image26.png))


Quando si sceglie il **Aggiungi** pulsante, la visualizzazione dell'indice nel listato 3 viene generato. Si noti il &lt;% @ Page %&gt; direttiva che viene visualizzato nella parte superiore del file. La visualizzazione dell'indice eredita il ViewPage&lt;IEnumerable&lt;ContactManager.Models.Contact&gt; &gt; classe. In altre parole, la classe di modello nella vista rappresenta un elenco di entità Contact.

Il corpo della vista indice contiene un ciclo foreach che scorre ogni contatto rappresentato dalla classe di modello. Il valore di ogni proprietà della classe di contatto viene visualizzato all'interno di una tabella HTML.

**Elenco di 3 - Views\Home\Index.aspx (non modificato)**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample3.aspx)]

È necessario apportare una modifica per la visualizzazione dell'indice. Poiché è non stiamo creando una visualizzazione dettagli, è possibile rimuovere il collegamento di dettagli. Trovare e rimuovere il codice seguente la visualizzazione dell'indice:

{ID elemento =. %} ID)&gt;

Dopo aver modificato la visualizzazione dell'indice, è possibile eseguire l'applicazione Gestione contatti. Selezionare l'opzione di menu Debug, Avvia debug o premere F5. La prima volta che si esegue l'applicazione, di ottenere la finestra di dialogo nella figura 14. Selezionare l'opzione **modificare il file Web. config per abilitare il debug** e fare clic sul pulsante OK.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image14.jpg)](iteration-1-create-the-application-vb/_static/image27.png)

**Nella figura 14**: abilitazione del debug ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image28.png))


La visualizzazione dell'indice viene restituita per impostazione predefinita. Questa vista sono elencati tutti i dati dalla tabella di database contatti (vedere Figura 15).


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image15.jpg)](iteration-1-create-the-application-vb/_static/image29.png)

**Figura 15**: Visualizza l'indice ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image30.png))


Si noti che la visualizzazione dell'indice include un collegamento con l'etichetta Crea nuovo nella parte inferiore della visualizzazione. Nella sezione successiva, imparare a creare nuovi contatti.

## <a name="creating-new-contacts"></a>Creazione di nuovi contatti

Per consentire agli utenti di creare nuovi contatti, è necessario aggiungere due azioni di metodo di creazione del controller Home. È necessario creare un'azione di metodo di creazione che restituisce un form HTML per la creazione di un nuovo contatto. È necessario creare una seconda azione di metodo di creazione che esegue l'inserimento di un database effettivo del nuovo contatto.

I nuovi metodi di metodo di creazione che è necessario aggiungere al controller Home sono contenuti nel listato 4.

**Elenco di 4 - Controllers\HomeController.vb (con i metodi di creazione)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample4.vb)]

Il primo metodo di metodo di creazione può essere richiamato con una richiesta GET HTTP, mentre il secondo metodo di metodo di creazione può essere richiamato solo da un POST HTTP. In altre parole, il secondo metodo di metodo di creazione può essere richiamato solo quando si registra un form HTML. Il primo metodo di metodo di creazione restituisce semplicemente una vista che contiene il form HTML per la creazione di un nuovo contatto. Il secondo metodo di metodo di creazione è molto più interessante: aggiunge il nuovo contatto per il database.

Si noti che il secondo metodo di metodo di creazione è stato modificato per accettare un'istanza della classe di contatto. I valori del form registrati dal modulo HTML sono associati a questa classe contatto dal framework ASP.NET MVC automaticamente. Ogni campo del form HTML creare viene assegnato a una proprietà del parametro di contatto.

Si noti che il parametro contatto è decorato con un attributo [Bind]. L'attributo [Bind viene usato per escludere la proprietà Id contatto dall'associazione. Poiché la proprietà Id rappresenta una proprietà Identity, non abbiamo t desidera impostare la proprietà Id.

Nel corpo del metodo di metodo di creazione, Entity Framework viene utilizzato per inserire il nuovo contatto nel database. Il nuovo contatto viene aggiunto al set esistente di contatti e viene chiamato il metodo SaveChanges () per inviare nuovamente tali modifiche al database sottostante.

È possibile generare un form HTML per la creazione di nuovi contatti destro del mouse su uno dei due metodi di metodo di creazione e selezionando l'opzione di menu **Aggiungi visualizzazione** (vedere Figura 16).


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image16.jpg)](iteration-1-create-the-application-vb/_static/image31.png)

**Figura 16**: aggiunta della visualizzazione di creazione ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image32.png))


Nel **Aggiungi visualizzazione** finestra di dialogo Seleziona il **ContactManager.Contact** classe e **crea** opzione per visualizzare il contenuto (vedere Figura 17). Quando si sceglie il **Aggiungi** pulsante, una Creazione visualizzazione viene generato automaticamente.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image17.jpg)](iteration-1-create-the-application-vb/_static/image33.png)

**Figura 17**: visualizzare una pagina esplosi ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image34.png))


Crea visualizzazione contiene i campi del form per ogni proprietà della classe di contatto. Il codice per la visualizzazione di creazione è incluso nell'elenco di 5.

**Elenco di 5 - Views\Home\Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample5.aspx)]

Dopo avere modificato i metodo di creazione metodi e aggiungere la visualizzazione di creazione, è possibile eseguire l'applicazione di gestione di contatto e creare nuovi contatti. Fare clic su di **Crea nuovo** collegamento visualizzato nella visualizzazione per passare alla visualizzazione di creazione dell'indice. Verrà visualizzata la vista nella figura 18.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image18.jpg)](iteration-1-create-the-application-vb/_static/image35.png)

**Figura 18**: The Create View ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image36.png))


## <a name="editing-contacts"></a>Modifica di contatti

Aggiunta della funzionalità per la modifica di record di un contatto è molto simile all'aggiunta di funzionalità per la creazione di nuovi record dei contatti. In primo luogo, è necessario aggiungere due nuovi metodi di modifica per la classe controller Home. Questi nuovi metodi Edit() sono contenuti nel listato 6.

**Elenco di 6 - Controllers\HomeController.vb (con i metodi di modifica)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample6.vb)]

Il primo metodo Edit() viene richiamato da un'operazione HTTP GET. Viene passato un parametro di Id per questo metodo che rappresenta l'Id del record del contatto da modificare. Entity Framework viene utilizzato per recuperare un contatto che corrisponde all'ID. Viene restituita una visualizzazione contenente un form HTML per la modifica di un record.

Il secondo metodo Edit() esegue l'aggiornamento effettivo per il database. Questo metodo accetta un'istanza della classe contatto come parametro. Il framework ASP.NET MVC associa i campi del modulo dal modulo di modifica per questa classe automaticamente. Si noti che si t includono l'attributo [Bind] quando si modifica un contatto (è necessario il valore della proprietà Id).

Entity Framework viene utilizzato per salvare il contatto modificato nel database. Il contatto originale deve essere recuperato dal database prima di tutto. Successivamente, il metodo di Entity Framework ApplyPropertyChanges() viene chiamato per registrare le modifiche al contatto. Infine, il metodo SaveChanges () di Entity Framework viene chiamato per rendere permanenti le modifiche al database sottostante.

È possibile generare la vista che contiene il modulo di modifica facendo clic il metodo Edit() e selezionando l'opzione di menu Aggiungi visualizzazione. Nella finestra di dialogo Aggiungi visualizzazione, selezionare il **ContactManager.Models.Contact** classe e **modifica** visualizzare il contenuto (vedere Figura 19).


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image19.jpg)](iteration-1-create-the-application-vb/_static/image37.png)

**Figura 19**: aggiunta di una visualizzazione di modifica ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image38.png))


Quando si fa clic sul pulsante Aggiungi, viene generata automaticamente una nuova visualizzazione di modifica. Il form HTML generato contiene i campi corrispondenti a ogni proprietà della classe contatto (vedere listato 7).

**Elenco 7 - Views\Home\Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>Eliminazione di contatti

Se si desidera eliminare i contatti è necessario aggiungere due azioni Delete () per la classe controller Home. La prima azione Delete () consente di visualizzare un modulo di conferma eliminazione. La seconda azione Delete () esegue l'effettiva eliminazione.

> [!NOTE] 
> 
> In un secondo momento, nell'iterazione #7, è modificare il responsabile del contatto in modo che supporti un uno passaggio eliminare Ajax.


I due nuovi metodi Delete () sono contenuti nel listato 8.

**Elenco di 8 - Controllers\HomeController.vb (metodi di eliminazione)**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample8.vb)]

Il primo metodo Delete () restituisce un modulo di conferma per eliminazione di un contatto record dal database (vedere Figure20). Il secondo metodo Delete () esegue l'operazione di eliminazione effettiva nel database. Dopo il contatto originale è stato recuperato dal database, vengono chiamati i metodi di Entity Framework DeleteObject() e SaveChanges () per eseguire l'eliminazione di database.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image20.jpg)](iteration-1-create-the-application-vb/_static/image39.png)

**Figura 20**: la visualizzazione di conferma eliminazione ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image40.png))


È necessario modificare la visualizzazione dell'indice in modo che contenga un collegamento per l'eliminazione di record dei contatti (vedere Figura 21). È necessario aggiungere il codice seguente alla stessa cella di tabella che contiene il collegamento di modifica:

{ID elemento =. %} ID)&gt;


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image21.jpg)](iteration-1-create-the-application-vb/_static/image41.png)

**Figura 21**: indicizzare la vista con collegamento di modifica ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image42.png))


Successivamente, è necessario creare la visualizzazione di conferma eliminazione. Il metodo Delete () nella classe controller Home e scegliere l'opzione di menu Aggiungi visualizzazione. La finestra di dialogo Aggiungi visualizzazione viene visualizzata (vedere Figura 22).

A differenza nel caso le visualizzazioni elenco, crea e modifica, la finestra di dialogo Aggiungi visualizzazione non contiene un'opzione per creare una visualizzazione di eliminazione. In alternativa, selezionare il **ContactManager.Models.Contact** classe di dati e **vuoto** visualizzare il contenuto. Selezionare la vista vuota opzione content richiederà di creare la vista effettuata.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image22.jpg)](iteration-1-create-the-application-vb/_static/image43.png)

**Figura 22**: aggiunta della visualizzazione di conferma eliminazione ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image44.png))


Il contenuto della visualizzazione di eliminazione è contenuto nel listato 9. Questa vista contiene un modulo che conferma o meno un contatto specifico deve essere eliminato (vedere Figura 21).

**Elenco di 9 - Views\Home\Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>La modifica del nome del Controller predefinito

Potrebbe reca fastidio che il nome della classe controller per l'utilizzo con i contatti è denominata classe HomeController. T rilevanti al controller di nome ContactController?

Questo problema è abbastanza semplice da risolvere. In primo luogo, è necessario effettuare il refactoring il nome del controller Home. Aprire la classe HomeController nell'Editor di codice di Visual Studio, fare clic con il pulsante destro il nome della classe e selezionare l'opzione di menu **rinominare**. Selezionare questa opzione di menu per aprire la finestra di dialogo Rinomina.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image23.jpg)](iteration-1-create-the-application-vb/_static/image45.png)

**Nella figura 23**: il Refactoring di un nome del controller ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image46.png))


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image24.jpg)](iteration-1-create-the-application-vb/_static/image47.png)

**Figura 24**: tramite la finestra di dialogo Rinomina ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image48.png))


Se si rinomina la classe controller, Visual Studio aggiornerà il nome della cartella nella cartella Views anche. Visual Studio verrà rinominare la cartella di \Views\Home nella cartella \Views\Contact.

Dopo aver apportato questa modifica, l'applicazione non disporrà più un controller Home. Quando si esegue l'applicazione, si otterrà la pagina di errore nella figura 25.


[![La finestra di dialogo Nuovo progetto](iteration-1-create-the-application-vb/_static/image25.jpg)](iteration-1-create-the-application-vb/_static/image49.png)

**Figura 25**: nessun controller predefinito ([fare clic per visualizzare l'immagine ingrandita](iteration-1-create-the-application-vb/_static/image50.png))


È necessario aggiornare la route predefinita nel file Global. asax per utilizzare il controller di contatto anziché il controller Home. Aprire il file Global. asax e modificare il controller predefinito utilizzato per la route predefinita (vedere Listato 10).

**Elenco di 10 - Global.asax.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample10.vb)]

Dopo aver apportato queste modifiche, il responsabile del contatto verrà eseguito correttamente. A questo punto, utilizza la classe controller contatto come il controller predefinito.

## <a name="summary"></a>Riepilogo

In questa prima iterazione, abbiamo creato un'applicazione di gestione di contatto di base in modo più rapido possibile. Abbiamo sfruttato Visual Studio per generare automaticamente il codice iniziale per il controller e visualizzazioni. Abbiamo anche sfruttato Entity Framework per generare automaticamente le classi di modello il database.

Attualmente, è possibile elencare, creare, modificare ed eliminare record dei contatti con l'applicazione Gestione contatti. In altre parole, è possibile eseguire tutte le operazioni di database di base necessarie per un'applicazione web basata su database.

Sfortunatamente, l'applicazione presenta alcuni problemi. Primo e si sono riluttanti a ammettere questo, l'applicazione Contact Manager non è l'applicazione più interessante. È necessario che alcune attività di progettazione. Nell'iterazione successiva, esamineremo come è possibile modificare la pagina master visualizzazione predefinita e il foglio di stile CSS per migliorare l'aspetto dell'applicazione.

In secondo luogo, non è stata implementata alcuna convalida del form. Ad esempio, non è nulla a che non consentono di inviare il modulo Crea senza l'immissione di valori per uno qualsiasi dei campi del modulo. Inoltre, è possibile immettere gli indirizzi di posta elettronica e numeri di telefono non valido. Iniziamo affrontare il problema di convalida del form nell'iterazione #3.

Infine e in particolare, l'iterazione corrente dell'applicazione Contact Manager essere facilmente modificata o mantenuto. Ad esempio, la logica di accesso del database è salvata destro in azioni il controller. Ciò significa che non è possibile modificare il codice di accesso ai dati senza modificare il controller. Nelle iterazioni successive, è esplorare i modelli di progettazione software che è possibile implementare per rendere più flessibile per modificare il responsabile del contatto.

>[!div class="step-by-step"]
[Precedente](iteration-7-add-ajax-functionality-cs.md)
[Successivo](iteration-2-make-the-application-look-nice-vb.md)
