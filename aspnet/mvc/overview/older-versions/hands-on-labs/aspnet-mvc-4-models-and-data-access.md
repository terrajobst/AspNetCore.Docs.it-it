---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: Accesso ai dati e i modelli ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: 'Nota: In questo laboratorio pratico presuppone che conoscenze di base di ASP.NET MVC. Se non è stato usato ASP.NET MVC prima, ti consigliamo di andare su ASP.NET MVC 4...'
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: bbb726f9f4abb2f9c72153a8c7986c5c83a9a840
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818372"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a>Accesso ai dati e i modelli ASP.NET MVC 4

da [Camp Web Team](https://twitter.com/webcamps)

[Download Web Camp Kit di formazione](https://aka.ms/webcamps-training-kit)

Questa pratica si presuppone conoscenze di base **ASP.NET MVC**. Se non è stato utilizzato **ASP.NET MVC** in precedenza, è consigliabile esaminare **concetti di base di ASP.NET MVC 4** laboratorio pratico.

Questa esercitazione illustra i miglioramenti e nuove funzionalità descritte in precedenza applicando modifiche minime a un'applicazione Web di esempio fornita nella cartella di origine.

> [!NOTE]
> Tutto il codice di esempio e frammenti di codice sono inclusi nel Web Camp Kit di formazione, disponibile all'indirizzo [Microsoft-Web/WebCampTrainingKit versioni](https://aka.ms/webcamps-training-kit). Il progetto specifico per questo lab è disponibile all'indirizzo [modelli di ASP.NET MVC 4 e l'accesso ai dati](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).

Nelle **nozioni di base di ASP.NET MVC** pratica, si hanno stati passando dati hardcoded verso i controller per i modelli di vista. Tuttavia, per compilare un'applicazione Web reale, si potrebbe voler usare un database effettivo.

In questo laboratorio pratico illustrerà come utilizzare un motore di database per archiviare e recuperare i dati necessari per l'applicazione Music Store. A tale scopo, verrà iniziare con un database esistente e creare il modello di dati di entità da esso. In questo laboratorio soddisfano i **Database First** approccio, nonché **Code First** approccio.

Tuttavia, è anche possibile usare la **Model First** approccio, creare lo stesso modello con gli strumenti e quindi generare il database da quest'ultimo.

![Database rispetto a prima. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First Visual Studio. A partire dal modello")

*Database rispetto a prima. A partire dal modello*

Dopo la generazione del modello, verranno apportate le modifiche appropriate nel StoreController per fornire le visualizzazioni di Store con i dati acquisiti dal database, invece di usare dati hardcoded. Non è necessario apportare modifiche ai modelli di visualizzazione perché il StoreController verrà restituito il ViewModel stesso per i modelli di vista, anche se questa volta i dati in provengono dal database.

**L'approccio Code First**

L'approccio Code First consente di definire il modello dal codice senza generare le classi che sono collegate a livello generale con il framework.

Nel codice in primo luogo, gli oggetti modello sono definiti con oggetti poco, &quot;Plain Old CLR Object&quot;. Oggetti poco è semplici classi semplici che non hanno ereditarietà viene applicata e non implementano le interfacce. È possibile generare automaticamente il database da essi o sia possibile usare un database esistente e generare i mapping della classe dal codice.

Vantaggi dell'uso di questo approccio è che il modello rimane indipendente dal framework di persistenza (in questo caso, Entity Framework), come le classi poco non sono collegate con il framework di mapping.

> [!NOTE]
> Questo Lab è basato su ASP.NET MVC 4 e una versione dell'applicazione di esempio di Music Store personalizzate e ridotta a icona adatta solo le funzionalità illustrate in questo laboratorio pratico.
> 
> Se si vuole esplorare l'intera **Music Store** applicazione di esercitazione è possibile trovarlo nel [MVC-musica-Store](https://github.com/evilDave/MVC-Music-Store).


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Sono necessari gli elementi seguenti per completare questa esercitazione:

- [Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (leggere [appendice A](#AppendixA) per istruzioni su come installarlo).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

**Installazione di frammenti di codice**

Per praticità, gran parte del codice che vengono gestiti insieme questo lab è disponibile come frammenti di codice di Visual Studio. Per installare i frammenti di codice eseguiti **.\Source\Setup\CodeSnippets.vsi** file.

Se non ha familiarità con i frammenti di codice di Visual Studio e si vuole imparare a usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice c: Using Code Snippets](#AppendixC)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

In questo laboratorio pratico include gli esercizi seguenti:

1. [Esercizio 1: Aggiunta di un Database](#Exercise1)
2. [Esercizio 2: Creazione di un Database tramite Code First](#Exercise2)
3. [Esercizio 3: Eseguire query sul Database con parametri](#Exercise3)

> [!NOTE]
> Ogni esercizio è accompagnato da un **End** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato gli esercizi. Se ti serve assistenza aggiuntiva esaminando gli esercizi, è possibile usare questa soluzione come guida.


Tempo stimato per completare questa esercitazione: **35 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a>Esercizio 1: Aggiunta di un Database

In questo esercizio, si apprenderà come aggiungere un database con le tabelle dell'applicazione MusicStore alla soluzione per gestire i dati. Una volta che il database viene generato con il modello e aggiunto alla soluzione, si modificherà la classe StoreController per fornire il modello di visualizzazione con i dati acquisiti dal database, anziché usare valori hard-coded.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a>Attività 1: aggiunta di un Database

In questa attività si aggiungerà un database già creato con le tabelle principali dell'applicazione MusicStore alla soluzione.

1. Aprire il **Begin** soluzione disponibile all'indirizzo **origine/Ex1-AddingADatabaseDBFirst/inizio/** cartella.

   1. È necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aggiungere **MvcMusicStore** file di database. In questo laboratorio pratico, si userà un database già creato denominato **MvcMusicStore.mdf**. A tale scopo, fare doppio clic su **App\_Data** cartella, scegliere **Add** e quindi fare clic su **elemento esistente**. Passare a **\Source\Assets** e selezionare il **MvcMusicStore.mdf** file.

    ![Aggiunta di un elemento esistente](aspnet-mvc-4-models-and-data-access/_static/image2.png "aggiungendo un elemento esistente")

    *Aggiunta di un elemento esistente*

    ![File di database MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf file di database")

    *File di database MvcMusicStore.mdf*

    Il database è stato aggiunto al progetto. Anche quando il database si trova all'interno della soluzione, è possibile eseguire query e aggiornarlo come lo era ospitato in un server di database diverso.

    ![Database MvcMusicStore in Esplora soluzioni](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Esplora soluzioni")

    *Database MvcMusicStore in Esplora soluzioni*
3. Verificare la connessione al database. A tale scopo, fare doppio clic su **MvcMusicStore.mdf** per stabilire una connessione.

    ![La connessione a MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "ci si connette a MvcMusicStore.mdf")

    *La connessione a MvcMusicStore.mdf*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a>Attività 2: creazione di un modello di dati

In questa attività si creerà un modello di dati per interagire con i database aggiunto nell'attività precedente.

1. Creare un modello di dati che rappresenta il database. A tale scopo, nella scelta di Esplora soluzioni il **modelli** cartella, scegliere **Add** e quindi fare clic su **nuovo elemento**. Nel **Aggiungi nuovo elemento** finestra di dialogo, seleziona la **Data** modello e quindi il **ADO.NET Entity Data Model** elemento. Modificare il nome del modello di dati in **StoreDB.edmx** e fare clic su **Add**.

    ![Aggiunta di StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "aggiunta StoreDB ADO.NET Entity Data Model")

    *Aggiunta di StoreDB ADO.NET Entity Data Model*
2. Il **procedura guidata Entity Data Model** verranno visualizzati. Questa procedura guidata guiderà è la creazione del livello del modello. Poiché il modello deve essere creato come base l'esistente recentyl database aggiunto, selezionare **genera da database** e fare clic su **successivo**.

    ![Scelta del contenuto dei modelli](aspnet-mvc-4-models-and-data-access/_static/image7.png "scegliendo il contenuto del modello")

    *Scelta di contenuto del modello*
3. Poiché si desidera generare un modello da un database, è necessario specificare la connessione da utilizzare. Fare clic su **nuova connessione**.
4. Selezionare **File di Database di Microsoft SQL Server** e fare clic su **continua**.

    ![Scegliere l'origine dati](aspnet-mvc-4-models-and-data-access/_static/image8.png "scegliere l'origine dati")

    *Finestra di dialogo origine dati Scegli*
5. Fare clic su **esplorare** e selezionare il database **MvcMusicStore.mdf** individuata nel **App\_dati** cartella e fare clic su **OK**.

    ![Le proprietà di connessione](aspnet-mvc-4-models-and-data-access/_static/image9.png "le proprietà di connessione")

    *Proprietà di connessione*
6. La classe generata deve avere lo stesso nome come stringa di connessione di entità, quindi modificarne il nome in **MusicStoreEntities** e fare clic su **successivo**.

    ![Scegliere la connessione dati](aspnet-mvc-4-models-and-data-access/_static/image10.png "scegliendo la connessione dati")

    *Scegliere la connessione dati*
7. Scegliere gli oggetti di database da utilizzare. Poiché il modello di entità verrà usato solo le tabelle del database, selezionare la **tabelle** opzione e assicurarsi che il **includere colonne chiavi esterne nel modello** e **Rendi plurali o singolari generato i nomi di oggetto** risulteranno selezionate anche le opzioni. Modificare il modello Namespace per **MvcMusicStore.Model** e fare clic su **fine**.

    ![Scegliere gli oggetti di database](aspnet-mvc-4-models-and-data-access/_static/image11.png "scegliendo gli oggetti di database")

    *Scegliere gli oggetti di database*

    > [!NOTE]
    > Se viene visualizzata una finestra di dialogo di avviso di sicurezza, fare clic su **OK** per eseguire il modello e generare le classi per l'entità del modello.
8. Verrà visualizzato un diagramma di entità per il database, mentre verrà creata una classe separata che esegue il mapping di ogni tabella nel database. Ad esempio, il **album** tabella sarà rappresentata da un **Album** (classe), ogni colonna della tabella in cui verrà eseguito il mapping a una proprietà di classe. Ciò consentirà di eseguire query e utilizzare gli oggetti che rappresentano le righe nel database.

    ![Diagramma entità](aspnet-mvc-4-models-and-data-access/_static/image12.png "diagramma entità")

    *Diagramma entità*

    > [!NOTE]
    > I modelli T4 (con estensione TT) eseguire il codice per generare le classi di entità e sovrascriveranno le classi esistenti con lo stesso nome. In questo esempio, le classi &quot;Album&quot;, &quot;Genre&quot; e &quot;artista&quot; sono stati sovrascritti con il codice generato.


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a>Attività 3: compilazione dell'applicazione

In questa attività, si verificherà che, anche se la generazione del modello è stato rimosso il **Album**, **Genre** e **artista** le classi modello, il progetto venga compilato correttamente tramite le nuove classi di modello di dati.

1. Compilare il progetto selezionando il **compilare** voce di menu e quindi **compilazione MvcMusicStore**.

    ![Compilazione del progetto](aspnet-mvc-4-models-and-data-access/_static/image13.png "compilando il progetto")

    *Compilazione del progetto*
2. Il progetto venga compilato correttamente. Il motivo per cui ancora funziona? Funziona perché le tabelle del database dispone di campi che includono le proprietà che erano in uso nelle classi rimosse **Album** e **Genre**.

    ![Le compilazioni ha avuto esito positivo](aspnet-mvc-4-models-and-data-access/_static/image14.png "compilazioni ha avuto esito positivo")

    *Le compilazioni sono riuscite*
3. Mentre la finestra di progettazione consente di visualizzare le entità sotto forma di diagramma, sono in effetti le classi di c#. Espandere la **StoreDB.edmx** nodo in Esplora soluzioni e quindi **StoreDB.tt**, si noterà la nuova entità generate.

    ![I file generati](aspnet-mvc-4-models-and-data-access/_static/image15.png "i file generati")

    *File generati*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Attività 4: eseguire query sul Database

In questa attività si aggiornerà la classe StoreController in modo che, invece di usare dati hardcoded, eseguirà una query del database per recuperare le informazioni.

1. Aprire **Controllers\StoreController.cs** e aggiungere il campo seguente alla classe per contenere un'istanza del **MusicStoreEntities** classe, denominata **storeDB**:

    (Code - Snippet *modelli e accesso ai dati - storeDB Ex1*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. Il **MusicStoreEntities** classe espone una proprietà di raccolta per ogni tabella nel database. Update **esplorare** metodo di azione per recuperare un genere con tutte le **album**.

    (Code - Snippet *modelli e accesso ai dati - esplorazione di Store Ex1*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > Si usa una funzionalità di .NET chiamati **LINQ** (language integrated query) per scrivere espressioni di query fortemente tipizzata in base a queste raccolte - eseguire il codice sul database e restituire gli oggetti che è possibile programmare relazione.
    > 
    > Per altre informazioni su LINQ, visitare il [sito msdn](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
3. Update **indice** metodo di azione per recuperare tutti i generi.

    (Code - Snippet *modelli e accesso ai dati - indice Store Ex1*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. Update **indice** metodo di azione per recuperare tutti i generi e trasformare la raccolta a un elenco.

    (Code - Snippet *modelli e accesso ai dati - Ex1 Store GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Attività 5: esecuzione dell'applicazione

In questa attività, si controllerà che generi archiviati nel database anziché quelle di hardcoded a questo punto verrà visualizzata la pagina di indice Store. Non è necessario modificare il modello di vista, perché il **StoreController** restituisce le stesse entità come in precedenza, anche se questa volta i dati in provengono dal database.

1. Ricompilare la soluzione e premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Verificare che il menu del **generi** non è più un elenco hardcoded e i dati vengono recuperati direttamente dal database.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    *Esplorazione generi dal database*
3. Ora passare a qualsiasi genere e verificare che gli album vengono popolati dal database.

    ![Dal database di esplorazione album](aspnet-mvc-4-models-and-data-access/_static/image17.png "esplorazione album dal database")

    *Esplorazione di album dal database*

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a>Esercizio 2: Creazione di un Database tramite codice prima di tutto

In questo esercizio, si apprenderà come usare l'approccio Code First per creare un database con le tabelle dell'applicazione MusicStore e come accedere ai relativi dati.

Una volta che viene generato il modello, si modificherà il StoreController per fornire il modello di visualizzazione con i dati acquisiti dal database, anziché usare valori hardcoded.

> [!NOTE]
> Se si hanno completato l'esercizio 1 e si abbia già familiarità con l'approccio Database First, ora si apprenderà come ottenere gli stessi risultati con un altro processo. Le attività che sono in comune con esercizio 1 sono state contrassegnate per rendere più facile la lettura. Se non si hanno completato l'esercizio 1 ma si vogliono acquisire l'approccio Code First, è possibile iniziare da questo esercizio e Ottieni una copertura completa dell'argomento.


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a>Attività 1: il popolamento dei dati di esempio

In questa attività si popolerà il database con dati di esempio quando viene inizialmente creato utilizzando Code First.

1. Aprire il **Begin** soluzione disponibile all'indirizzo **origine/Ex2-CreatingADatabaseCodeFirst/inizio/** cartella. In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aggiungere il **SampleData.cs** del file per il **modelli** cartella. A tale scopo, fare doppio clic su **modelli** cartella, scegliere **Add** e quindi fare clic su **elemento esistente**. Passare a **\Source\Assets** e selezionare il **SampleData.cs** file.

    ![Codice di popolare i dati di esempio](aspnet-mvc-4-models-and-data-access/_static/image18.png "codice di popolare i dati di esempio")

    *Codice di popolare i dati di esempio*
3. Aprire il **Global.asax.cs** del file e aggiungere quanto segue *usando* istruzioni.

    (Code - Snippet *modelli e accesso ai dati - Ex2 globale Asax using*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. Nel **Application\_Start ()** metodo aggiungere la riga seguente per impostare l'inizializzatore del database.

    (Code - Snippet *modelli e accesso ai dati - Ex2 globale Asax SetInitializer*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a>Attività 2: configurare la connessione al Database

Ora che già stato aggiunto un database per il progetto, verrà scritto **Web. config** file la stringa di connessione.

1. Aggiungere una stringa di connessione al **Web. config**. A tale scopo, aprire **Web. config** alla radice del progetto e sostituire la stringa di connessione denominata DefaultConnection con la riga seguente nel **&lt;connectionStrings&gt;** sezione:

    ![Percorso del file Web. config](aspnet-mvc-4-models-and-data-access/_static/image19.png "percorso del file Web. config")

    *Percorso del file Web. config*

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a>Attività 3: uso del modello

Ora che si è già configurato la connessione al database, si collegherà il modello con le tabelle del database. In questa attività si creerà una classe che verrà collegata al database con Code First. Tenere presente che esiste una classe di modello POCO esistente che deve essere modificata.

> [!NOTE]
> Se è stata completata esercizio 1, si noterà che questo passaggio è stato eseguito da una procedura guidata. In questo codice prima, si creerà manualmente le classi che verranno collegate alle entità di dati.

1. Aprire la classe di modello POCO **Genre** dalla **modelli** cartella del progetto e includere un ID. Usare una proprietà int con nome **GenreId**.

    (Code - Snippet *modelli e accesso ai dati - Genre primo codice Ex2*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > Per lavorare con le convenzioni Code First, la classe Genre deve avere una proprietà di chiave primaria che verrà rilevata automaticamente.
    > 
    > Altre informazioni sulle convenzioni prima del codice in questo [articolo di msdn](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
2. A questo punto, aprire la classe di modello POCO **Album** dalla **modelli** cartella del progetto e includono le chiavi esterne, creare proprietà con i nomi **GenreId** e  **ArtistId**. Questa classe esiste già il **GenreId** per la chiave primaria.

    (Code - Snippet *modelli e accesso ai dati - Album primo codice Ex2*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. Aprire la classe di modello POCO **artista** e includere le **ArtistId** proprietà.

    (Code - Snippet *modelli e accesso ai dati - artista primo codice Ex2*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. Fare doppio clic il **modelli** cartella di progetto e selezionare **Add | Classe**. Denominare il file **MusicStoreEntities.cs**. Quindi, fare clic su **Add.**

    ![Aggiunta di una classe](aspnet-mvc-4-models-and-data-access/_static/image20.png "aggiunta di una classe")

    *Aggiunta di un nuovo elemento*

    ![Aggiunta di un class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "aggiunta class2")

    *Aggiunta di una classe*
5. La classe appena creato, aprire **MusicStoreEntities.cs**e includere gli spazi dei nomi **Data. Entity** e **System.Data.Entity.Infrastructure**.

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. Sostituire la dichiarazione di classe per estendere il **DbContext** classe: dichiarare un pubblico **DBSet** ed eseguire l'override **OnModelCreating** (metodo). Dopo questo passaggio si otterrà una classe di dominio che viene collegata del modello con Entity Framework. Per farlo, sostituire il codice della classe con quanto segue:

    (Code - Snippet *modelli e accesso ai dati - MusicStoreEntities primo codice Ex2*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> Con Entity Framework **DbContext** e **DBSet** sarà in grado di eseguire query sulla classe POCO Genre. Estendendo **OnModelCreating** metodo, si specifica nel **codice** come Genre verrà mappato a una tabella di database. È possibile trovare altre informazioni su DBContext e DBSet in questo articolo di msdn: [collegamento](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Attività 4: eseguire query sul Database

In questa attività si aggiornerà la classe StoreController in modo che, invece di usare dati hardcoded, verrà recuperato dal database.

> [!NOTE]
> Questa attività è in comune con esercizio 1.
> 
> Se è stato completato l'esercizio 1 si noterà questi passaggi sono gli stessi in entrambi gli approcci (Database prima di tutto o Code first). Sono diversi in modalità di collegamento dati con il modello, ma l'accesso alle entità di dati è ancora trasparente dal controller.


1. Aprire **Controllers\StoreController.cs** e aggiungere il campo seguente alla classe per contenere un'istanza del **MusicStoreEntities** classe, denominata **storeDB**:

    (Code - Snippet *modelli e accesso ai dati - storeDB Ex1*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. Il **MusicStoreEntities** classe espone una proprietà di raccolta per ogni tabella nel database. Update **esplorare** metodo di azione per recuperare un genere con tutte le **album**.

    (Code - Snippet *modelli e accesso ai dati - esplorazione di Store Ex2*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > Si usa una funzionalità di .NET chiamati **LINQ** (language integrated query) per scrivere espressioni di query fortemente tipizzata in base a queste raccolte - eseguire il codice sul database e restituire gli oggetti che è possibile programmare relazione.
    > 
    > Per altre informazioni su LINQ, visitare il [sito msdn](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
3. Update **indice** metodo di azione per recuperare tutti i generi.

    (Code - Snippet *modelli e accesso ai dati - indice Store Ex2*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. Update **indice** metodo di azione per recuperare tutti i generi e trasformare la raccolta a un elenco.

    (Code - Snippet *modelli e accesso ai dati - Ex2 Store GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Attività 5: esecuzione dell'applicazione

In questa attività, si controllerà che generi archiviati nel database anziché quelle di hardcoded a questo punto verrà visualizzata la pagina di indice Store. Non è necessario modificare il modello di vista, perché il **StoreController** restituisce lo stesso **StoreIndexViewModel** come indicato in precedenza, ma questa volta i dati in provengono dal database.

1. Ricompilare la soluzione e premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Verificare che il menu del **generi** non è più un elenco hardcoded e i dati vengono recuperati direttamente dal database.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    *Esplorazione generi dal database*
3. Ora passare a qualsiasi genere e verificare che gli album vengono popolati dal database.

    ![Dal database di esplorazione album](aspnet-mvc-4-models-and-data-access/_static/image23.png "esplorazione album dal database")

    *Esplorazione di album dal database*

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a>Esercizio 3: Eseguire query sul Database con parametri

In questo esercizio, si apprenderà come eseguire query sul database utilizzando i parametri e come usare Data Shaping risultati di Query, una funzionalità che consente di ridurre il numero del database accede ai dati durante il recupero in modo più efficiente.

> [!NOTE]
> Per altre informazioni sulla forma del risultato di Query, visitare il [articolo di msdn](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a>Attività 1 - modifica StoreController per recuperare gli album da Database

In questa attività si modificherà il **StoreController** classe per accedere al database per recuperare gli album da un genere specifico.

1. Aprire il **iniziare** soluzione disponibile all'indirizzo il **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** cartella se si desidera utilizzare l'approccio Code First o **Source\ Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin** cartella se si desidera utilizzare l'approccio Database-First. In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aprire il **StoreController** classe per modificare le **Sfoglia** metodo di azione. A questo scopo, nella **Esplora soluzioni**, espandere il **controller** cartella e fare doppio clic su **StoreController.cs**.
3. Modifica il **esplorare** metodo di azione per recuperare gli album per un genere specifico. A tale scopo, sostituire il codice seguente:

    (Code - Snippet *modelli e accesso ai dati - Ex3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> Per popolare una raccolta di entità, è necessario usare il **inclusione** metodo per specificare che si desidera recuperare gli album troppo. È possibile usare il. **Single ()** estensione nelle query LINQ perché in questo caso è previsto uno solo di genere per un album. Il **Single ()** metodo accetta un'espressione Lambda come parametro, che in questo caso specifica un singolo oggetto Genre in modo che il relativo nome corrisponde al valore definito.
> 
> Si sarà possibile avvalersi di una funzionalità che consente di indicare altre entità correlate che si desidera caricare anche quando l'oggetto Genre viene recuperato. Questa funzionalità è detta **forma del risultato di Query**e consente di ridurre il numero di volte necessario per accedere al database per recuperare le informazioni. In questo scenario, si dovrà prelettura album per il genere recuperate.
> 
> La query include **Genres.Include (&quot;album&quot;)** per indicare che si desidera anche album correlati. Questo comporta in un'applicazione più efficiente, poiché consente di recuperare dati Genre sia Album nella richiesta singolo database.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Attività 2: esecuzione dell'applicazione

In questa attività verranno di eseguire l'applicazione e recuperare gli album di un genere specificato dal database.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL **/Store/Sfoglia? genre = Pop** per verificare che i risultati vengono recuperati dal database.

    ![Esplorazione in base al genere](aspnet-mvc-4-models-and-data-access/_static/image24.png "esplorazione in base al genere")

    *Esplorazione/Store/Sfoglia? genre = Pop*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a>Attività 3: accesso a album da Id

In questa attività si ripeterà la procedura precedente per ottenere gli album dal rispettivo ID.

1. Chiudere il browser, se necessario, per tornare a Visual Studio. Aprire il **StoreController** classe per modificare le **dettagli** metodo di azione. A questo scopo, nella **Esplora soluzioni**, espandere il **controller** cartella e fare doppio clic su **StoreController.cs**.
2. Modifica il **informazioni dettagliate** metodo di azione per recuperare i dettagli di album basato sulla loro **Id**. A tale scopo, sostituire il codice seguente:

    (Code - Snippet *modelli e accesso ai dati - Ex3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Attività 4: esecuzione dell'applicazione

In questa attività verrà eseguire l'applicazione in un web browser e ottenere i dettagli di album dal rispettivo ID.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL **/Store/Details/51** o cercare i generi e selezionare un album per verificare che i risultati vengono recuperati dal database.

    ![Esplorazione delle informazioni dettagliate](aspnet-mvc-4-models-and-data-access/_static/image25.png "esplorazione dettagli")

    *Esplorazione /Store/Details/51*

> [!NOTE]
> Inoltre, è possibile distribuire questa applicazione per siti Web di Azure seguenti [appendice b: pubblicazione un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completato questo laboratorio pratico si è appreso le nozioni di base dei modelli di MVC ASP.NET e l'accesso ai dati, utilizzando il **Database First** approccio, nonché **Code First** approccio:

- Come aggiungere un database alla soluzione per gestire i dati
- Come aggiornare i controller per fornire i modelli di visualizzazione con i dati acquisiti dal database anziché a livello di codice
- Come eseguire query sul database utilizzando i parametri
- Come usare la Query risultato Data Shaping, una funzionalità che consente di ridurre il numero di accessi al database, il recupero dei dati in modo più efficiente
- Come usare gli approcci sia Database First e Code First in Microsoft Entity Framework per collegare il database con il modello

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice a: installazione di Visual Studio Express 2012 per Web

È possibile installare **Microsoft Visual Studio Express 2012 per Web** o da un'altra &quot;Express&quot; versione utilizzando il **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web Microsoft*.

1. Passare a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e cercare il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **Installa ora**. Se non hai **instalace Webové Platformy** si verrà reindirizzati per scaricarlo e installarlo prima di tutto.
3. Una volta **instalace Webové Platformy** è aperto, fare clic su **installare** per avviare il programma di installazione.

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
7. Fare clic su **Exit** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per Web, fare clic per il **avviare** schermata e iniziare la scrittura &quot; **Visual Studio Express**&quot;, quindi fare clic sul **Visual Studio Express per Web** il riquadro.

    ![Visual Studio Express per il riquadro Web](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    *Visual Studio Express per il riquadro Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Appendice b: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web

In questa appendice spiega come creare un nuovo sito web dal portale di gestione di Azure e pubblicare l'applicazione che è stato ottenuto seguendo il lab, sfruttando la funzionalità di pubblicazione di distribuzione Web fornita da Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Attività 1: creazione di un nuovo sito Web di Windows portale di Azure

1. Andare alla [portale di gestione di Windows Azure](https://manage.windowsazure.com/) e accedere usando le credenziali di Microsoft associate alla sottoscrizione.

    > [!NOTE]
    > Con Windows Azure Puoi ospitare gratuitamente 10 siti Web ASP.NET e la scalabilità orizzontale man mano che aumenta il traffico. È possibile effettuare l'iscrizione [qui](http://aka.ms/aspnet-hol-azure).

    ![Accedere al portale di Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "accedere al portale di Azure")

    *Accedere al portale di gestione di Azure*
2. Fare clic su **New** sulla barra dei comandi.

    ![Creazione di un nuovo sito Web](aspnet-mvc-4-models-and-data-access/_static/image32.png "creando un nuovo sito Web")

    *Creazione di un nuovo sito Web*
3. Fare clic su **Compute** | **sito Web**. Quindi selezionare **creazione rapida** opzione. Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.

    > [!NOTE]
    > Un sito Web di Microsoft Azure è l'host per un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire. L'opzione Creazione rapida consente di distribuire un'applicazione web completa per il sito Web Microsoft Azure all'esterno del portale. Non include i passaggi per la configurazione di un database.

    ![Creazione di un nuovo sito Web utilizzando Creazione rapida](aspnet-mvc-4-models-and-data-access/_static/image33.png "creando un nuovo sito Web mediante Creazione rapida")

    *Creazione di un nuovo sito Web utilizzando Creazione rapida*
4. Attendere finché il nuovo **sito Web** viene creato.
5. Dopo aver creato il sito Web fare clic sul collegamento sotto i **URL** colonna. Verificare che il nuovo sito Web sia in funzione.

    ![Passare al nuovo sito web](aspnet-mvc-4-models-and-data-access/_static/image34.png "passare al nuovo sito web")

    *Passare al nuovo sito web*

    ![Sito Web in esecuzione](aspnet-mvc-4-models-and-data-access/_static/image35.png "sito Web in esecuzione")

    *Sito Web in esecuzione*
6. Tornare al portale e fare clic sul nome del sito web con il **nome** colonna per visualizzare le pagine di gestione.

    ![Aprire le pagine di gestione sito web](aspnet-mvc-4-models-and-data-access/_static/image36.png "aprire le pagine di gestione sito web")

    *Aprire le pagine di gestione sito Web*
7. Nel **Dashboard** nella pagina il **riepilogo rapido** fare clic sui **Scarica profilo di pubblicazione** collegamento.

    > [!NOTE]
    > Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in un sito Web di Azure per ogni metodo di pubblicazione abilitato. Il profilo di pubblicazione contiene l'URL, le credenziali dell'utente e stringhe di database necessarie per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di questi programmi per pubblicazione di applicazioni web in siti Web di Windows Azure.

    ![Download del sito web di profilo di pubblicazione](aspnet-mvc-4-models-and-data-access/_static/image37.png "scaricando il sito web di profilo di pubblicazione")

    *Profilo di pubblicazione scaricato il sito Web*
8. Scaricare il file di profilo di pubblicazione in una posizione nota. In questo esercizio ulteriormente si vedrà come usare questo file per pubblicare un'applicazione web a un siti Web di Windows Azure da Visual Studio.

    ![Salvare il file di profilo di pubblicazione](aspnet-mvc-4-models-and-data-access/_static/image38.png "salvataggio del profilo di pubblicazione")

    *Salvare il file di profilo di pubblicazione*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Attività 2: configurare il Server di Database

Se l'applicazione Usa SQL Server database è necessario creare un Database di SQL server. Se si vuole distribuire una semplice applicazione che non Usa SQL Server è possibile ignorare questa attività.

1. È necessario un server di Database SQL per archiviare il database dell'applicazione. È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure all'indirizzo **database Sql** | **server** | **del Server Dashboard**. Se non si dispone di un server creato, è possibile crearne una usando il **Add** pulsante sulla barra dei comandi. Annotare il **nome server e URL, nome dell'account di accesso amministratore e la password**, come verranno usati nelle attività successiva. Non creare il database, come verrà creato in una fase successiva.

    ![Dashboard di Server di Database SQL](aspnet-mvc-4-models-and-data-access/_static/image39.png "Dashboard di Server di Database SQL")

    *Dashboard di Server di Database SQL*
2. Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server del **indirizzi IP consentiti**. A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP dal **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo e scegliere il ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) pulsante.

    ![Aggiunta indirizzo IP del Client](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    *Aggiunta indirizzo IP del Client*
3. Una volta il **indirizzo IP del Client** viene aggiunto a indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.

    ![Confermare le modifiche](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    *Confermare le modifiche*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web

1. Tornare alla soluzione ASP.NET MVC 4. Nel **Esplora soluzioni**, fare clic sul progetto sito web e selezionare **Publish**.

    ![Pubblicazione dell'applicazione](aspnet-mvc-4-models-and-data-access/_static/image43.png "pubblicazione dell'applicazione")

    *Pubblicazione del sito web*
2. Importare il profilo di pubblicazione che è stato salvato nella prima attività.

    ![L'importazione del profilo di pubblicazione](aspnet-mvc-4-models-and-data-access/_static/image44.png "l'importazione del profilo di pubblicazione")

    *Importazione del profilo di pubblicazione*
3. Fare clic su **convalidare la connessione**. Dopo aver completata la convalida fare clic su **successivo**.

    > [!NOTE]
    > La convalida è stata completata una volta che visualizzato un segno di spunta verde accanto al pulsante convalida connessione.

    ![La convalida connessione](aspnet-mvc-4-models-and-data-access/_static/image45.png "convalida della connessione")

    *Convalida della connessione*
4. Nel **impostazioni** nella pagina il **database** sezione, fare clic sul pulsante accanto alla casella di testo della connessione di database (vale a dire **DefaultConnection**).

    ![Configurazione della distribuzione Web](aspnet-mvc-4-models-and-data-access/_static/image46.png "configurazione della distribuzione Web")

    *Configurazione della distribuzione Web*
5. Configurare la connessione al database come segue:

   - Nel **nome Server** digitare l'URL server di Database SQL tramite il *tcp:* prefisso.
   - Nelle **nome utente** digitare il nome dell'account di accesso amministratore server.
   - Nelle **Password** digitare la password dell'account di accesso amministratore server.
   - Digitare un nuovo nome del database.

     ![Configurazione di stringa di connessione di destinazione](aspnet-mvc-4-models-and-data-access/_static/image47.png "configurazione stringa di connessione di destinazione")

     *Configurazione di stringa di connessione di destinazione*
6. Fare quindi clic su **OK**. Quando viene richiesto di creare il database, fare clic su **Sì**.

    ![Creazione del database](aspnet-mvc-4-models-and-data-access/_static/image48.png "creazione della stringa di database")

    *Creazione del database*
7. La stringa di connessione che si userà per connettersi al Database SQL di Windows Azure viene visualizzata all'interno di connessione predefinita nella casella di testo. Scegliere quindi **Avanti**.

    ![Stringa di connessione che punta al Database SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "stringa di connessione che punta al Database SQL")

    *Stringa di connessione che punta al Database SQL*
8. Nel **Preview** pagina, fare clic su **Publish**.

    ![Pubblicazione dell'applicazione web](aspnet-mvc-4-models-and-data-access/_static/image50.png "pubblicazione dell'applicazione web")

    *Pubblicazione dell'applicazione web*
9. Al termine del processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Appendice c: uso dei frammenti di codice

Con i frammenti di codice, hai tutto il codice che necessario a tua disposizione. Il documento lab indicherà esattamente quando usarli, come illustrato nella figura seguente.

![Uso di frammenti di codice di Visual Studio per inserire codice nel progetto](aspnet-mvc-4-models-and-data-access/_static/image51.png "frammenti di codice con Visual Studio per inserire codice nel progetto")

*Uso di frammenti di codice di Visual Studio per inserire codice nel progetto*

***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***

1. Posizionare il cursore in cui si vuole inserire il codice.
2. Iniziare a digitare il nome del frammento di codice (senza spazi o trattini).
3. Guarda come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento di codice corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).
5. Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.

![Iniziare a digitare il nome di frammento](aspnet-mvc-4-models-and-data-access/_static/image52.png "inizia a digitare il nome del frammento di codice")

*Iniziare a digitare il nome del frammento di codice*

![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-models-and-data-access/_static/image53.png "premere Tab per selezionare il frammento di codice evidenziata")

*Premere Tab per selezionare il frammento di codice evidenziata*

![Il frammento di codice e premere nuovamente Tab espanderà](aspnet-mvc-4-models-and-data-access/_static/image54.png "si espanderà il frammento di codice e premere nuovamente Tab")

*Il frammento di codice e premere nuovamente Tab espanderà*

***Per aggiungere un frammento di codice usando il mouse (c#, Visual Basic e XML)*** 1. Pulsante destro del mouse in cui si desidera inserire il frammento di codice.

1. Selezionare **Inserisci frammento** aggiungendo **frammenti di codice**.
2. Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.

![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento](aspnet-mvc-4-models-and-data-access/_static/image55.png "rapida in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice")

*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice*

![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-models-and-data-access/_static/image56.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa")

*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa*
