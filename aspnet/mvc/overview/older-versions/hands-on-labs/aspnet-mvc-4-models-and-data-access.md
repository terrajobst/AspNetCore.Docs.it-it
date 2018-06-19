---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: Accesso ai dati e i modelli ASP.NET MVC 4 | Documenti Microsoft
author: rick-anderson
description: 'Nota: Questa pratica presuppone che la knowledge base di ASP.NET MVC. Se si utilizza ASP.NET MVC prima, ti consigliamo di andare su ASP.NET MVC 4...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 88b3316b116962dd35031f4b971dbfe31ed0e010
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/18/2018
ms.locfileid: "34306738"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a>Accesso ai dati e i modelli ASP.NET MVC 4

Da [categorie Web Team](https://twitter.com/webcamps)

[Download Web categorie Kit di formazione](https://aka.ms/webcamps-training-kit)

Questa pratica presuppone avere conoscenze di base **ASP.NET MVC**. Se non è stato utilizzato **ASP.NET MVC** in precedenza, è consigliabile esaminare **nozioni di base di ASP.NET MVC 4** le esercitazioni pratiche.

Questa esercitazione illustra i miglioramenti e nuove funzionalità descritte in precedenza tramite l'applicazione di modifiche di lieve entità a un'applicazione Web di esempio fornita nella cartella di origine.

> [!NOTE]
> Tutto il codice di esempio e i frammenti di codice sono inclusi nel Web categorie Training Kit, disponibile all'indirizzo [versioni Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Il progetto specifico per questa esercitazione è disponibile all'indirizzo [accesso ai dati e i modelli di ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).

In **nozioni fondamentali su MVC ASP.NET** pratica, si hanno stato passando dati hardcoded verso i controller per i modelli di visualizzazione. Ma, per compilare un'applicazione Web reale, è possibile utilizzare un database reale.

Questa pratica verrà descritto come utilizzare un motore di database per archiviare e recuperare i dati necessari per l'applicazione del Negozio. Per raggiungere questo obiettivo, si avvia con un database esistente e creare il modello di dati di entità da esso. In questo laboratorio soddisferà la **Database First** approccio, nonché **Code First** approccio.

Tuttavia, è inoltre possibile utilizzare il **Model First** approccio, creare lo stesso modello utilizzando gli strumenti e quindi generare il database da esso.

![Visual Studio prima di database. Prima del modello](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First Visual Studio. Primo modello")

*Visual Studio prima di database. Primo modello*

Dopo aver generato il modello, verranno apportate le modifiche appropriate nella StoreController per fornire le viste di archivio con i dati acquisiti dal database, invece di usare dati hardcoded. Non occorre di apportare qualsiasi modifica ai modelli di visualizzazione perché il StoreController verrà restituito il ViewModel stessa per i modelli di visualizzazione, anche se questo tempo i dati provengono dal database.

**Il primo approccio di codice**

L'approccio Code First consente di definire il modello dal codice senza generare le classi che sono in genere associate con il framework.

Nel codice in primo luogo, gli oggetti del modello sono definiti con POCOs, &quot;Plain Old CLR Object&quot;. POCOs sono semplici classi normale che non dispone di alcun tipo di ereditarietà e non implementano le interfacce. È possibile generare automaticamente il database da essi oppure è possibile utilizzare un database esistente e generare il mapping della classe dal codice.

I vantaggi dell'utilizzo di questo approccio è che il modello rimane indipendente dal framework di persistenza (in questo caso, Entity Framework), come le classi POCOs non sono collegate con il framework di mapping.

> [!NOTE]
> Questo Lab è basato su ASP.NET MVC 4 e una versione dell'applicazione di esempio musica archivio personalizzato e adatta solo le funzionalità illustrate in questo laboratorio pratico ridotta a icona.
> 
> Se si desidera esplorare l'intero **negozio** applicazione di esercitazione sarà possibile trovarlo in [negozio di musica MVC](https://github.com/evilDave/MVC-Music-Store).


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

È necessario che gli elementi seguenti per completare questa esercitazione:

- [Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (lettura [appendice](#AppendixA) per istruzioni su come installarlo).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

**L'installazione di frammenti di codice**

Per praticità, gran parte del codice che vengono gestiti in questa esercitazione è disponibile come frammenti di codice di Visual Studio. Per installare i frammenti di codice eseguire **.\Source\Setup\CodeSnippets.vsi** file.

Se non si ha familiarità con i frammenti di codice di Visual Studio e si desidera per informazioni su come usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice c: con i frammenti di codice](#AppendixC)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questa pratica include gli esercizi seguenti:

1. [Esercizio 1: Aggiunta di un Database](#Exercise1)
2. [Esercizio 2: Creazione di un Database mediante Code First](#Exercise2)
3. [Esercizio 3: Eseguire query sul Database con parametri](#Exercise3)

> [!NOTE]
> Ogni esercizio è accompagnato da un **fine** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato l'esercitazione dall'inizio. Se è necessario ulteriore assistenza utilizza gli esercizi, è possibile utilizzare questa soluzione come guida.


Tempo stimato per completare questa esercitazione: **35 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a>Esercizio 1: Aggiunta di un Database

In questo esercizio, si apprenderà come aggiungere un database con le tabelle dell'applicazione MusicStore alla soluzione per poter utilizzare i dati. Una volta che il database viene generato con il modello e aggiunto alla soluzione, si modificherà la classe StoreController per fornire il modello di visualizzazione con i dati acquisiti dal database, anziché utilizzare valori hardcoded.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a>Attività 1: aggiunta di un Database

In questa attività si aggiungerà un database già creato con le tabelle principale dell'applicazione MusicStore alla soluzione.

1. Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex1-AddingADatabaseDBFirst/Begin/** cartella.

   1. È necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aggiungere **MvcMusicStore** file di database. In questo laboratorio pratico, utilizzare un database già creato denominato **MvcMusicStore.mdf**. A tale scopo, fare doppio clic su **App\_dati** cartella, scegliere **Aggiungi** e quindi fare clic su **elemento esistente**. Passare a **\Source\Assets** e selezionare il **MvcMusicStore.mdf** file.

    ![Aggiunta di un elemento esistente](aspnet-mvc-4-models-and-data-access/_static/image2.png "aggiunta di un elemento esistente")

    *Aggiunta di un elemento esistente*

    ![File di database MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf file di database")

    *File di database MvcMusicStore.mdf*

    Il database è stato aggiunto al progetto. Anche quando il database si trova all'interno della soluzione, è possibile eseguire una query e aggiornarli quando è stato ospitato in un server database diverso.

    ![Database MvcMusicStore in Esplora soluzioni](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Esplora soluzioni")

    *Database MvcMusicStore in Esplora soluzioni*
3. Verificare la connessione al database. A tale scopo, fare doppio clic su **MvcMusicStore.mdf** per stabilire una connessione.

    ![Connessione a MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "la connessione a MvcMusicStore.mdf")

    *Connessione a MvcMusicStore.mdf*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a>Attività 2: creazione di un modello di dati

In questa attività si creerà un modello di dati per interagire con il database aggiunto nell'attività precedente.

1. Creare un modello di data che rappresenta il database. A tale scopo, in rapida di Esplora soluzioni di **modelli** cartella, scegliere **Aggiungi** e quindi fare clic su **nuovo elemento**. Nel **Aggiungi nuovo elemento** finestra di dialogo Seleziona il **dati** modello e quindi la **ADO.NET Entity Data Model** elemento. Modificare il nome del modello di dati in **StoreDB.edmx** e fare clic su **Aggiungi**.

    ![Aggiunta di StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "aggiunta StoreDB ADO.NET Entity Data Model")

    *Aggiunta di StoreDB ADO.NET Entity Data Model*
2. Il **procedura guidata Entity Data Model** verranno visualizzati. Questa procedura guidata vi assisterà tramite la creazione del livello di modello. Poiché il modello deve essere creato in base il recentyl database esistenti aggiunti selezionare **genera da database** e fare clic su **Avanti**.

    ![Scegliere il contenuto del modello](aspnet-mvc-4-models-and-data-access/_static/image7.png "scegliendo il contenuto del modello")

    *Scegliere il contenuto del modello*
3. Poiché si desidera generare un modello da un database, è necessario specificare la connessione da utilizzare. Fare clic su **nuova connessione**.
4. Selezionare **File di Database di Microsoft SQL Server** e fare clic su **continua**.

    ![Scegli origine dati](aspnet-mvc-4-models-and-data-access/_static/image8.png "Scegli origine dati")

    *Scegliere una finestra di dialogo origine dati*
5. Fare clic su **Sfoglia** e selezionare il database **MvcMusicStore.mdf** trova nella **App\_dati** cartella e fare clic su **OK**.

    ![Le proprietà di connessione](aspnet-mvc-4-models-and-data-access/_static/image9.png "le proprietà di connessione")

    *Proprietà di connessione*
6. La classe generata deve avere lo stesso nome come stringa di connessione entity, modificare il nome su **MusicStoreEntities** e fare clic su **Avanti**.

    ![Scegliere la connessione dati](aspnet-mvc-4-models-and-data-access/_static/image10.png "scelta la connessione dati")

    *Scegliere la connessione dati*
7. Scegliere gli oggetti di database da utilizzare. Il modello di entità utilizzerà solo le tabelle del database, selezionare il **tabelle** opzione e verificare che il **Includi colonne chiavi esterne nel modello** e **Rendi plurali o singolari generato i nomi di oggetto** risulteranno selezionate anche le opzioni. Modificare il modello Namespace per **MvcMusicStore.Model** e fare clic su **fine**.

    ![Scegliere gli oggetti di database](aspnet-mvc-4-models-and-data-access/_static/image11.png "scegliendo gli oggetti di database")

    *Scegliere gli oggetti di database*

    > [!NOTE]
    > Se viene visualizzata una finestra di dialogo Avviso di sicurezza, fare clic su **OK** per eseguire il modello e generare le classi per l'entità del modello.
8. Verrà visualizzato un diagramma di entità per il database, mentre verrà creata una classe separata che esegue il mapping di ogni tabella nel database. Ad esempio, il **album** tabella sarà rappresentata da un **Album** (classe), ogni colonna della tabella in cui verrà eseguito il mapping a una proprietà di classe. Ciò consentirà di eseguire query e utilizzare gli oggetti che rappresentano le righe nel database.

    ![Diagramma di entità](aspnet-mvc-4-models-and-data-access/_static/image12.png "diagramma entità")

    *Diagramma di entità*

    > [!NOTE]
    > I modelli T4 (con estensione TT) eseguire codice per generare le classi di entità e sovrascriveranno le classi esistenti con lo stesso nome. In questo esempio, le classi &quot;Album&quot;, &quot;Genre&quot; e &quot;artista&quot; sono stati sovrascritti con il codice generato.


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a>Attività 3: compilazione dell'applicazione

In questa attività, si verificherà che, anche se la generazione del modello è stato rimosso il **Album**, **Genre** e **artista** classi del modello, il progetto venga compilato correttamente tramite le nuove classi di modello di dati.

1. Compilare il progetto selezionando il **compilare** voce di menu e quindi **compilare MvcMusicStore**.

    ![Compilazione del progetto](aspnet-mvc-4-models-and-data-access/_static/image13.png "la compilazione del progetto")

    *Compilazione del progetto*
2. Il progetto venga compilato correttamente. Perché ancora funziona? Funziona perché le tabelle di database dispone di campi che includono le proprietà che si sono utilizzato nelle classi rimosse **Album** e **Genre**.

    ![Compilazioni ha avuto esito positivo](aspnet-mvc-4-models-and-data-access/_static/image14.png "compilazioni ha avuto esito positivo")

    *Compilazioni completate*
3. Mentre la finestra di progettazione consente di visualizzare le entità in un formato di diagramma, sono in effetti classi c#. Espandere il **StoreDB.edmx** nodo in Esplora soluzioni e quindi **StoreDB.tt**, si noterà che la nuova entità generate.

    ![I file generati](aspnet-mvc-4-models-and-data-access/_static/image15.png "i file generati")

    *File generati*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Attività 4: eseguire query sul Database

In questa attività si aggiornerà la classe StoreController in modo che, invece di usare dati hardcoded, invierà una query del database per recuperare le informazioni.

1. Aprire **Controllers\StoreController.cs** e aggiungere il campo seguente alla classe per contenere un'istanza del **MusicStoreEntities** classe, denominata **storeDB**:

    (- Frammento di codice *modelli e accesso ai dati - Ex1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. Il **MusicStoreEntities** classe espone una raccolta di proprietà per ogni tabella nel database. Aggiornamento **Sfoglia** il metodo di azione per recuperare un genere con tutte le **album**.

    (- Frammento di codice *modelli e l'accesso ai dati - Sfoglia archivio Ex1*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > Si utilizza una funzionalità di .NET denominata **LINQ** (language integrated query) per scrivere espressioni di query fortemente tipizzata in base a queste raccolte - di cui eseguire il codice nel database e verranno restituiti gli oggetti che è possibile programmare nei confronti.
    > 
    > Per ulteriori informazioni su LINQ, visitare il [sito msdn](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
3. Aggiornamento **indice** per recuperare tutti i generi i metodo di azione.

    (- Frammento di codice *modelli e l'accesso ai dati - indice archivio Ex1*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. Aggiornamento **indice** metodo di azione per recuperare tutti i generi e trasformare la raccolta a un elenco.

    (- Frammento di codice *modelli e l'accesso ai dati - Ex1 archivio GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Attività 5: esecuzione dell'applicazione

In questa attività si verifica che la pagina di indice dell'archivio visualizzerà i generi archiviati nel database anziché hardcoded quelli. Non è necessario modificare il modello di visualizzazione perché il **StoreController** restituisce le stesse entità come in precedenza, anche se questo tempo i dati provengono dal database.

1. Ricompilare la soluzione e premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Verificare che il menu di **generi** non è più un elenco hardcoded e i dati vengono recuperati direttamente dal database.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    *Esplorazione generi dal database*
3. Ora individuare qualsiasi genere e verificare che gli album vengono popolati dal database.

    ![Esplorazione album dal database](aspnet-mvc-4-models-and-data-access/_static/image17.png "esplorazione album dal database")

    *Esplorazione album dal database*

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a>Esercizio 2: Creazione di un Database tramite codice prima di tutto

In questo esercizio, si apprenderà come usare l'approccio Code First per creare un database con le tabelle dell'applicazione MusicStore e come accedere ai dati.

Dopo la generazione del modello, si modificherà il StoreController per fornire il modello di visualizzazione con i dati acquisiti dal database, anziché utilizzare valori hardcoded.

> [!NOTE]
> Se si hanno completato l'esercizio 1 e già stato utilizzato il Database primo approccio, ora si apprenderà come ottenere gli stessi risultati con un altro processo. Le attività in comune con esercizio 1 sono state contrassegnate per semplificare la lettura. Se non sono state completate esercizio 1, ma si desidera acquisire il codice primo approccio, è possibile iniziare da questo esercizio e ottenere una copertura completa dell'argomento.


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a>Attività 1: il popolamento dei dati di esempio

In questa attività verrà compilato il database con dati di esempio, quando viene inizialmente creato utilizzando Code First.

1. Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex2-CreatingADatabaseCodeFirst/Begin/** cartella. In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aggiungere il **SampleData.cs** file per il **modelli** cartella. A tale scopo, fare doppio clic su **modelli** cartella, scegliere **Aggiungi** e quindi fare clic su **elemento esistente**. Passare a **\Source\Assets** e selezionare il **SampleData.cs** file.

    ![Dati di esempio popolano codice](aspnet-mvc-4-models-and-data-access/_static/image18.png "codice di popolare i dati di esempio")

    *Codice di popolare i dati di esempio*
3. Aprire il **Global.asax.cs** file e aggiungere le seguenti *utilizzando* istruzioni.

    (- Frammento di codice *modelli e l'accesso ai dati - Ex2 globale Asax using*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. Nel **applicazione\_Start** metodo aggiungere la riga seguente per impostare l'inizializzatore del database.

    (- Frammento di codice *modelli e l'accesso ai dati - Ex2 globale Asax SetInitializer*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a>Attività 2: configurare la connessione al Database

Ora che già stato aggiunto un database al progetto, viene scritto nel **Web. config** file la stringa di connessione.

1. Aggiungere una stringa di connessione in **Web. config**. A tale scopo, aprire **Web. config** alla radice del progetto e sostituire la stringa di connessione denominata DefaultConnection con la riga nel **&lt;connectionStrings&gt;** sezione:

    ![Percorso del file Web. config](aspnet-mvc-4-models-and-data-access/_static/image19.png "percorso del file Web. config")

    *percorso del file Web. config*

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a>Attività 3: utilizzo del modello

Ora che già stato configurato la connessione al database, si procederà al collegamento del modello con le tabelle di database. In questa attività si creerà una classe che verrà collegata al database con Code First. Tenere presente che esiste una classe di modello POCO esistente che deve essere modificata.

> [!NOTE]
> Se è stato completato esercizio 1, si noterà che questo passaggio è stato eseguito da una procedura guidata. In questo modo Code First, si creerà manualmente le classi che verranno collegate alle entità di dati.

1. Aprire la classe modello POCO **Genre** da **modelli** cartella del progetto e includere un ID. Utilizzare una proprietà int con il nome **GenreId**.

    (- Frammento di codice *modelli e l'accesso ai dati - Ex2 codice prima Genre*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > Per utilizzare le convenzioni di Code First, la classe Genre deve avere una proprietà di chiave primaria che verrà rilevata automaticamente.
    > 
    > Altre informazioni sulle convenzioni prima del codice in questo [articolo msdn](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
2. A questo punto, aprire la classe di modello POCO **Album** da **modelli** cartella del progetto e includere le chiavi esterne, creare proprietà con i nomi **GenreId** e  **ArtistId**. Questa classe dispone già di **GenreId** per la chiave primaria.

    (- Frammento di codice *modelli e l'accesso ai dati - Ex2 codice primo Album*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. Aprire la classe modello POCO **artista** e includere il **ArtistId** proprietà.

    (- Frammento di codice *modelli e l'accesso ai dati - Ex2 codice prima artista*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. Fare doppio clic su di **modelli** cartella del progetto e selezionare **Aggiungi | Classe**. Nome del file **MusicStoreEntities.cs**. Quindi, fare clic su **Aggiungi.**

    ![Aggiunta di una classe](aspnet-mvc-4-models-and-data-access/_static/image20.png "aggiunta di una classe")

    *Aggiunta di un nuovo elemento*

    ![Aggiunta di class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "aggiunta class2")

    *Aggiunta di una classe*
5. Aprire la classe appena creato, **MusicStoreEntities.cs**e includere gli spazi dei nomi **System.Data.Entity** e **System.Data.Entity.Infrastructure**.

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. Sostituire la dichiarazione di classe per estendere il **DbContext** classe: dichiarare un pubblico **DBSet** ed eseguire l'override **OnModelCreating** metodo. Dopo questo passaggio si otterrà una classe di dominio che si collegherà il modello con Entity Framework. A tale scopo, sostituire il codice della classe con le operazioni seguenti:

    (- Frammento di codice *modelli e l'accesso ai dati - Ex2 codice prima MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> Con Entity Framework **DbContext** e **DBSet** sarà in grado di eseguire query sulla classe POCO genere. Estendendo **OnModelCreating** metodo, si specifica nel **codice** come genere verrà mappato a una tabella di database. È possibile trovare ulteriori informazioni su DBContext e DBSet in questo articolo di msdn: [collegamento](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Attività 4: eseguire query sul Database

In questa attività si aggiornerà la classe StoreController in modo che, invece di usare dati hardcoded, verrà recuperato dal database.

> [!NOTE]
> Questa attività è in comune con esercizio 1.
> 
> Se è stato completato l'esercizio 1, si noterà questi passaggi sono gli stessi in entrambi gli approcci (prima del Database o prima del codice). Sono diversi in modalità con cui i dati sono collegati con il modello, ma l'accesso alle entità di dati è ancora visibile dal controller.


1. Aprire **Controllers\StoreController.cs** e aggiungere il campo seguente alla classe per contenere un'istanza del **MusicStoreEntities** classe, denominata **storeDB**:

    (- Frammento di codice *modelli e accesso ai dati - Ex1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. Il **MusicStoreEntities** classe espone una raccolta di proprietà per ogni tabella nel database. Aggiornamento **Sfoglia** il metodo di azione per recuperare un genere con tutte le **album**.

    (- Frammento di codice *modelli e l'accesso ai dati - Sfoglia archivio Ex2*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > Si utilizza una funzionalità di .NET denominata **LINQ** (language integrated query) per scrivere espressioni di query fortemente tipizzata in base a queste raccolte - di cui eseguire il codice nel database e verranno restituiti gli oggetti che è possibile programmare nei confronti.
    > 
    > Per ulteriori informazioni su LINQ, visitare il [sito msdn](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
3. Aggiornamento **indice** per recuperare tutti i generi i metodo di azione.

    (- Frammento di codice *modelli e l'accesso ai dati - indice archivio Ex2*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. Aggiornamento **indice** metodo di azione per recuperare tutti i generi e trasformare la raccolta a un elenco.

    (- Frammento di codice *modelli e l'accesso ai dati - Ex2 archivio GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Attività 5: esecuzione dell'applicazione

In questa attività si verifica che la pagina di indice dell'archivio visualizzerà i generi archiviati nel database anziché hardcoded quelli. Non è necessario modificare il modello di visualizzazione perché il **StoreController** restituisce lo stesso **StoreIndexViewModel** come in precedenza, ma questa volta i dati provengono dal database.

1. Ricompilare la soluzione e premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Verificare che il menu di **generi** non è più un elenco hardcoded e i dati vengono recuperati direttamente dal database.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    *Esplorazione generi dal database*
3. Ora individuare qualsiasi genere e verificare che gli album vengono popolati dal database.

    ![Esplorazione album dal database](aspnet-mvc-4-models-and-data-access/_static/image23.png "esplorazione album dal database")

    *Esplorazione album dal database*

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a>Esercizio 3: Eseguire query sul Database con parametri

In questo esercizio, si apprenderà come eseguire query sul database utilizzando i parametri e come utilizzare la forma del risultato di Query, una funzionalità che riduce il numero database accede il recupero dei dati in modo più efficiente.

> [!NOTE]
> Per ulteriori informazioni sulla forma del risultato di Query, visitare il seguente [articolo msdn](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a>Attività 1 - modifica StoreController per recuperare gli album da Database

In questa attività si modificherà il **StoreController** classe per accedere al database per recuperare gli album da un genere specifico.

1. Aprire il **iniziare** soluzione si trova in corrispondenza di **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** cartella se si desidera utilizzare Code First approccio o **Source\ Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin** cartella se si desidera utilizzare l'approccio di Database-First. In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aprire il **StoreController** classe per modificare il **Sfoglia** metodo di azione. A tale scopo, nel **Esplora**, espandere il **controller** cartella e fare doppio clic su **StoreController.cs**.
3. Modifica il **Sfoglia** metodo di azione per recuperare gli album per un genere specifico. A tale scopo, sostituire il codice seguente:

    (- Frammento di codice *modelli e l'accesso ai dati - Ex3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> Per popolare una raccolta di entità, è necessario utilizzare il **Include** per specificare che si desidera recuperare gli album troppo. È possibile utilizzare il. **Single()** estensione in LINQ perché in questo caso in cui è previsto il genere di un solo per un album. Il **Single()** metodo accetta un'espressione Lambda come un parametro che specifica in questo caso un singolo oggetto Genre in modo che il nome corrisponde al valore definito.
> 
> Si avvale di una funzionalità che consente di indicare altre entità correlate che si desidera caricare anche quando l'oggetto Genre viene recuperato. Questa funzionalità è denominata **forma del risultato di Query**e consente di ridurre il numero di volte necessario per accedere al database per recuperare le informazioni. In questo scenario, è possibile recuperare preventivamente gli album per il genere recuperate.
> 
> La query include **Genres.Include (&quot;album&quot;)** per indicare che si desidera anche album correlati. Verrà creato in un'applicazione più efficiente, poiché consente di recuperare dati sia Genre e Album nella richiesta singolo database.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Attività 2: esecuzione dell'applicazione

In questa attività, verranno eseguire l'applicazione e saranno recuperate album di un genere specifico dal database.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **archivio/Sfoglia? genre = Pop** per verificare che i risultati vengono recuperati dal database.

    ![Esplorazione per genere](aspnet-mvc-4-models-and-data-access/_static/image24.png "esplorazione per genere")

    *Esplorazione archivio/Sfoglia? genre = Pop*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a>Attività 3 - accesso album da Id

In questa attività si ripeterà la procedura precedente per ottenere gli album base all'ID.

1. Chiudere il browser, se necessario, per tornare a Visual Studio. Aprire il **StoreController** classe per modificare il **dettagli** metodo di azione. A tale scopo, nel **Esplora**, espandere il **controller** cartella e fare doppio clic su **StoreController.cs**.
2. Modifica il **dettagli** recuperare album Dettagli metodo di azione in base alle loro **Id**. A tale scopo, sostituire il codice seguente:

    (- Frammento di codice *modelli e l'accesso ai dati - Ex3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Attività 4: esecuzione dell'applicazione

In questa attività verrà eseguire l'applicazione in un web browser e ottenere i dettagli di album in base al relativo ID.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **/Store/Details/51** o individuare i generi e selezionare un album per verificare che i risultati vengono recuperati dal database.

    ![Visualizzazione dettagli](aspnet-mvc-4-models-and-data-access/_static/image25.png "visualizzazione dettagli")

    *Esplorazione /Store/Details/51*

> [!NOTE]
> Inoltre, è possibile distribuire l'applicazione per siti Web di Azure seguenti [pubblicazione appendice b: un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Per il completamento di questa pratica si è appreso le nozioni di base di accesso ai dati e i modelli di MVC ASP.NET, usando il **Database First** approccio, nonché **Code First** approccio:

- Come aggiungere un database alla soluzione per poter utilizzare i relativi dati
- Come aggiornare i controller per fornire i modelli di visualizzazione con i dati acquisiti dal database anziché a livello di codice
- Come eseguire query sul database utilizzando i parametri
- Come utilizzare la Query risultato Data Shaping, una funzionalità che riduce il numero di accessi al database, il recupero dei dati in modo più efficiente
- Come utilizzare il primo Database e di Code First in Entity Framework di Microsoft per collegare il database con il modello

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice a: installazione di Visual Studio Express 2012 per Web

È possibile installare **Microsoft Visual Studio Express 2012 per Web** o un altro &quot;Express&quot; versione utilizzando il **[installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web di Microsoft*.

1. Passare a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **installa**. Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.
3. Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.

    ![Installa Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "installa Visual Studio Express")

    *Installa Visual Studio Express*
4. Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.

    ![Accettare le condizioni di licenza](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    *Accettare le condizioni di licenza*
5. Attendere finché non viene completato il processo di download e l'installazione.

    ![Stato dell'installazione](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    *Installazione completata*
7. Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per Web, passare al **avviare** schermata e iniziare la scrittura &quot; **VS Express**&quot;, quindi fare clic su di **Visual Studio Express per Web** il riquadro.

    ![VS Express per il riquadro Web](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    *VS Express per il riquadro Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Appendice b: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web

Questa appendice mostra come creare un nuovo sito web dal portale di gestione di Windows Azure e pubblicare l'applicazione, ottenute seguendo il lab, sfruttando la funzionalità di pubblicazione distribuzione Web fornita da Microsoft Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Attività 1 - Creazione di un nuovo sito Web da Windows Azure Portal

1. Passare al [il portale di gestione di Windows Azure](https://manage.windowsazure.com/) e accedere utilizzando le credenziali di Microsoft associate alla sottoscrizione.

    > [!NOTE]
    > Con Windows Azure è possibile ospitare gratuito di 10 siti Web ASP.NET e quindi aumentare in base al traffico. È possibile iscriversi [qui](http://aka.ms/aspnet-hol-azure).

    ![Accedere al portale Windows Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "accedere al portale Windows Azure")

    *Accedere al portale di gestione di Azure*
2. Fare clic su **nuovo** sulla barra dei comandi.

    ![Creazione di un nuovo sito Web](aspnet-mvc-4-models-and-data-access/_static/image32.png "creando un nuovo sito Web")

    *Creazione di un nuovo sito Web*
3. Fare clic su **calcolo** | **sito Web**. Selezionare quindi **creazione rapida** opzione. Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.

    > [!NOTE]
    > Un sito Web di Windows Azure è l'host per un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire. L'opzione Creazione rapida consente di distribuire un'applicazione web completa per il sito Web Microsoft Azure dall'esterno al portale. Non include i passaggi per configurare un database.

    ![Creazione di un nuovo sito Web utilizzando Creazione rapida](aspnet-mvc-4-models-and-data-access/_static/image33.png "creando un nuovo sito Web utilizzando Creazione rapida")

    *Creazione di un nuovo sito Web utilizzando Creazione rapida*
4. Attendere finché il nuovo **sito Web** viene creato.
5. Una volta creato il sito Web, fare clic sul collegamento sotto il **URL** colonna. Verificare il funzionamento del nuovo sito Web.

    ![Esplorazione per il nuovo sito web](aspnet-mvc-4-models-and-data-access/_static/image34.png "esplorazione per il nuovo sito web")

    *Esplorazione per il nuovo sito web*

    ![Sito Web in esecuzione](aspnet-mvc-4-models-and-data-access/_static/image35.png "sito Web in esecuzione")

    *Sito Web in esecuzione*
6. Tornare al portale e fare clic sul nome del sito web sotto il **nome** colonna per visualizzare le pagine di gestione.

    ![Aprire le pagine di gestione del sito web](aspnet-mvc-4-models-and-data-access/_static/image36.png "apertura delle pagine di gestione del sito web")

    *Aprire le pagine di gestione del sito Web*
7. Nel **Dashboard** nella pagina di **riepilogo rapido** fare clic su di **Scarica profilo di pubblicazione** collegamento.

    > [!NOTE]
    > Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in un sito Web di Windows Azure per ogni metodo di pubblicazione abilitato. Il profilo di pubblicazione contiene gli URL, le credenziali dell'utente e le stringhe di database necessari per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di queste applicazioni per pubblicazione di applicazioni web per siti Web di Windows Azure.

    ![Profilo di pubblicazione del sito web di download](aspnet-mvc-4-models-and-data-access/_static/image37.png "profilo di pubblicazione del sito web di download")

    *Profilo di pubblicazione del sito Web di download*
8. Scaricare il file del profilo di pubblicazione in un percorso noto. In questo esercizio si ulteriormente verrà visualizzato come utilizzare questo file per pubblicare un'applicazione web a un siti Web di Windows Azure da Visual Studio.

    ![Salvare il file del profilo di pubblicazione](aspnet-mvc-4-models-and-data-access/_static/image38.png "il salvataggio del profilo di pubblicazione")

    *Salvare il file del profilo di pubblicazione*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Attività 2: configurare il Server di Database

Se l'applicazione viene utilizzato SQL Server database, è necessario creare un server di Database SQL. Se si desidera distribuire un'applicazione semplice che non utilizza SQL Server è possibile ignorare questa attività.

1. È necessario un server di Database SQL per archiviare il database dell'applicazione. È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure all'indirizzo **database Sql** | **server** | **del Server Dashboard**. Se non si dispone di un server creato, è possibile creare tramite il **Aggiungi** pulsante sulla barra dei comandi. Annotare il **nome server e URL, il nome di accesso di amministratore e la password**, come verranno utilizzati in operazioni successive. Non creare il database, come verrà creato in una fase successiva.

    ![Dashboard del Server Database SQL](aspnet-mvc-4-models-and-data-access/_static/image39.png "Dashboard del Server Database SQL")

    *Dashboard del Server Database SQL*
2. Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server di **indirizzi IP consentiti**. A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP da **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo e fare clic su di ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) pulsante.

    ![Aggiunta indirizzo IP del Client](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    *Aggiunta indirizzo IP del Client*
3. Una volta il **indirizzo IP del Client** viene aggiunto agli indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.

    ![Confermare le modifiche](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    *Confermare le modifiche*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web

1. Tornare alla soluzione ASP.NET MVC 4. Nel **Esplora**, fare clic sul progetto sito web e selezionare **pubblica**.

    ![La pubblicazione dell'applicazione](aspnet-mvc-4-models-and-data-access/_static/image43.png "la pubblicazione dell'applicazione")

    *Pubblicazione del sito web*
2. Importare il profilo di pubblicazione che è stato salvato nella prima attività.

    ![L'importazione del profilo di pubblicazione](aspnet-mvc-4-models-and-data-access/_static/image44.png "l'importazione del profilo di pubblicazione")

    *L'importazione di profilo di pubblicazione*
3. Fare clic su **convalidare la connessione**. Una volta completata la convalida, fare clic su **Avanti**.

    > [!NOTE]
    > Dopo aver visualizzato un segno di spunta verde visualizzata accanto al pulsante convalida connessione, la convalida è stata completata.

    ![Convalida della connessione](aspnet-mvc-4-models-and-data-access/_static/image45.png "convalida della connessione")

    *Convalida della connessione*
4. Nel **impostazioni** nella pagina il **database** fare clic sul pulsante accanto casella di testo della connessione di database (ad esempio **DefaultConnection**).

    ![Configurazione della distribuzione Web](aspnet-mvc-4-models-and-data-access/_static/image46.png "configurazione della distribuzione Web")

    *Configurazione della distribuzione Web*
5. Configurare la connessione al database come segue:

   - Nel **nome Server** digitare l'URL server di Database SQL utilizzando il *tcp:* prefisso.
   - In **nome utente** digitare il nome di accesso di amministratore di server.
   - In **Password** digitare la password dell'account di accesso amministratore server.
   - Digitare un nuovo nome di database.

     ![Configurazione di stringa di connessione di destinazione](aspnet-mvc-4-models-and-data-access/_static/image47.png "configurazione stringa di connessione di destinazione")

     *Configurazione di stringa di connessione di destinazione*
6. Fare quindi clic su **OK**. Quando viene richiesto di creare il database fare clic su **Sì**.

    ![Creazione del database](aspnet-mvc-4-models-and-data-access/_static/image48.png "creazione della stringa di database")

    *Creazione del database*
7. La stringa di connessione che si utilizzerà per connettersi al Database SQL di Windows Azure viene visualizzata all'interno di casella di testo di connessione predefinito. Scegliere quindi **Avanti**.

    ![Stringa di connessione che punta al Database SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "stringa di connessione che punta al Database SQL")

    *Stringa di connessione che punta al Database SQL*
8. Nel **anteprima** pagina, fare clic su **pubblica**.

    ![Pubblicare l'applicazione web](aspnet-mvc-4-models-and-data-access/_static/image50.png "pubblicare l'applicazione web")

    *Pubblicare l'applicazione web*
9. Una volta terminato il processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Appendice c: utilizzo dei frammenti di codice

Con i frammenti di codice, si dispone di tutto il codice che necessario a portata di mano. Il documento lab consentirà di determinare esattamente quando è possibile utilizzarli, come illustrato nella figura seguente.

![Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto](aspnet-mvc-4-models-and-data-access/_static/image51.png "frammenti di codice tramite Visual Studio per inserire codice nel progetto")

*Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto*

***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***

1. Posizionare il cursore dove si desidera inserire il codice.
2. Iniziare a digitare il nome del frammento di codice (senza spazi o segni meno).
3. Osservare come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).
5. Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.

![Iniziare a digitare il nome del frammento di codice](aspnet-mvc-4-models-and-data-access/_static/image52.png "inizia a digitare il nome del frammento di codice")

*Iniziare a digitare il nome del frammento di codice*

![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-models-and-data-access/_static/image53.png "premere Tab per selezionare il frammento evidenziato")

*Premere Tab per selezionare il frammento di codice evidenziata*

![Premere Tab nuovamente e il frammento di codice verranno espansi](aspnet-mvc-4-models-and-data-access/_static/image54.png "espanderà premere Tab nuovamente e il frammento di codice")

*Premere Tab nuovamente e il frammento di codice verranno espansi*

***Per aggiungere un frammento di codice utilizzando il mouse (c#, Visual Basic e XML)*** 1. Fare clic in cui si desidera inserire il frammento di codice.

1. Selezionare **Inserisci frammento di codice** seguito da **frammenti di codice**.
2. Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.

![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice](aspnet-mvc-4-models-and-data-access/_static/image55.png "rapida in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice")

*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice*

![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-models-and-data-access/_static/image56.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso")

*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso*
