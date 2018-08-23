---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: Gli helper di ASP.NET MVC 4, moduli e convalida | Microsoft Docs
author: rick-anderson
description: In ASP.NET MVC 4 modelli e le esercitazioni pratiche accesso dati, si sono stati il caricamento e visualizzazione dei dati dal database. In questo laboratorio pratico, verrà aggiunto ai...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 8671ae8e9408e6f05135fa27d56480477521c4ba
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836504"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a>Gli helper di ASP.NET MVC 4, moduli e convalida

da [Camp Web Team](https://twitter.com/webcamps)

[Download Web Camp Kit di formazione](https://aka.ms/webcamps-training-kit)

Nelle **modelli di ASP.NET MVC 4 e l'accesso ai dati** Lab pratici, sono stati durante il caricamento e visualizzazione dei dati dal database. In questo laboratorio pratico, verrà aggiunto per il **Music Store** applicazione la possibilità di modificare i dati.

Tenendo presente questo obiettivo, si creerà innanzitutto il controller che supporterà le azioni di creazione, lettura, aggiornamento ed eliminazione (CRUD) di album. Si genererà un modello di visualizzazione dell'indice sfruttando i vantaggi della funzionalità di scaffolding di ASP.NET MVC per visualizzare le proprietà degli album in una tabella HTML. Per migliorare tale visualizzazione, si aggiungerà un helper HTML personalizzato che verrà troncato descrizioni lunghe.

Successivamente, si aggiungerà la modifica e creare visualizzazioni che consentirà di modificare gli album nel database, con l'aiuto degli elementi del form, ad esempio elenchi a discesa.

Infine, si consentirà agli utenti di eliminare un album e inoltre si impedirà l'immissione di dati errati, convalidando l'input.

Questa pratica si presuppone conoscenze di base **ASP.NET MVC**. Se non è stato utilizzato **ASP.NET MVC** in precedenza, è consigliabile esaminare **concetti di base di ASP.NET MVC** laboratorio pratico.

Questa esercitazione illustra i miglioramenti e nuove funzionalità descritte in precedenza applicando modifiche minime a un'applicazione Web di esempio fornita nella cartella di origine.

> [!NOTE]
> Tutto il codice di esempio e frammenti di codice sono inclusi nel Web Camp Kit di formazione, disponibile all'indirizzo [Microsoft-Web/WebCampTrainingKit versioni](https://aka.ms/webcamps-training-kit). Il progetto specifico per questo lab è disponibile all'indirizzo [helper di ASP.NET MVC 4, moduli e convalida](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico, si apprenderà come:

- Creare un controller per supportare le operazioni CRUD
- Generare una visualizzazione indice per visualizzare le proprietà dell'entità in una tabella HTML
- Aggiungere un helper HTML personalizzato
- Creare e personalizzare una visualizzazione di modifica
- Distinguere i metodi di azione che reagiscono a HTTP-GET o le chiamate HTTP-POST
- Aggiungere e personalizzare un'istruzione Create View
- Gestire l'eliminazione di un'entità
- Convalida dell'input utente

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

Se non ha familiarità con i frammenti di codice di Visual Studio e si vuole imparare a usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice b: Using Code Snippets](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Gli esercizi seguenti che costituiscono questo laboratorio pratico:

1. [Creazione di controller di gestione di Store e la relativa visualizzazione dell'indice](#Exercise1)
2. [Aggiunta di un HTML Helper](#Exercise2)
3. [Creazione della visualizzazione di modifica](#Exercise3)
4. [Aggiunta di una visualizzazione Create](#Exercise4)
5. [Gestisce l'eliminazione](#Exercise5)
6. [Aggiunta della convalida](#Exercise6)
7. [Uso di jQuery Unobtrusive sul lato Client](#Exercise7)

> [!NOTE]
> Ogni esercizio è accompagnato da un **End** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato gli esercizi. Se ti serve assistenza aggiuntiva esaminando gli esercizi, è possibile usare questa soluzione come guida.


Tempo stimato per completare questa esercitazione: **60 minuti**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>Esercizio 1: Creazione di controller di gestione di Store e la relativa visualizzazione dell'indice

In questo esercizio, si apprenderà come creare un nuovo controller di supportare le operazioni CRUD, personalizzare il metodo di azione Index per restituire un elenco di album dal database e infine la generazione di un modello di visualizzazione dell'indice a sfruttare lo scaffolding di ASP.NET MVC funzionalità per visualizzare le proprietà degli album in una tabella HTML.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>Attività 1: creazione di StoreManagerController

In questa attività si creerà un nuovo controller denominato **StoreManagerController** per supportare le operazioni CRUD.

1. Aprire il **Begin** soluzione disponibile all'indirizzo **origine/Ex1-CreatingTheStoreManagerController/inizio/** cartella.

   1. È necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aggiungere un nuovo controller. A tale scopo, fare doppio clic sui **controller** cartella all'interno di Esplora soluzioni, scegliere **Add** e quindi il **Controller** comando. Modifica il **Controller** **Name** al **StoreManagerController** e assicurarsi che l'opzione **controller MVC con azioni di lettura/scrittura vuote**è selezionato. Fare clic su **Aggiungi**.

    ![Finestra di dialogo Aggiungi controller](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "finestra di dialogo Aggiungi controller")

    *Aggiungi finestra di dialogo Controller*

    Viene generata una nuova classe Controller. Poiché è indicato per aggiungere azioni per la lettura/scrittura, i metodi stub per tali valori, vengono create azioni CRUD comuni con i commenti TODO compilati, verrà richiesto di includere la logica specifica dell'applicazione.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>Attività 2 - personalizzare l'indice StoreManager

In questa attività si personalizzerà il metodo di azione StoreManager Index per restituire una visualizzazione con l'elenco di album dal database.

1. Aggiungere il codice seguente nella classe StoreManagerController *usando* direttive.

    (Code - Snippet *helper di ASP.NET MVC 4, moduli e convalida - Ex1 usando MvcMusicStore*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. Aggiungere un campo per il **StoreManagerController** per contenere un'istanza di **MusicStoreEntities.**

    (Code - Snippet *helper di ASP.NET MVC 4, moduli e convalida - MusicStoreEntities Ex1*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. Implementare l'azione StoreManagerController Index per restituire una visualizzazione con l'elenco di album.

    La logica di azione del Controller sarà molto simile all'azione Index del StoreController scritto in precedenza. Usare LINQ per recuperare tutti gli album, incluse quelle relative al genere e artista per la visualizzazione.

    (Code - Snippet *helper di ASP.NET MVC 4, moduli e convalida - indice StoreManagerController Ex1*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>Attività 3: creare la visualizzazione dell'indice

In questa attività si creerà il modello di visualizzazione dell'indice per visualizzare l'elenco di album restituiti dai **StoreManager** Controller.

1. Prima di creare il nuovo modello di vista, è necessario compilare il progetto in modo che il **finestra di dialogo Aggiungi visualizzazione** conosce le **Album** classe da utilizzare. Selezionare **compilare | Compilare MvcMusicStore** per compilare il progetto.
2. Pulsante destro del mouse all'interno di **indice** metodo di azione e selezionare **Aggiungi visualizzazione**. Verrà visualizzata la **Aggiungi visualizzazione** finestra di dialogo.

    ![Aggiungi visualizzazione](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Aggiungi visualizzazione")

    *Aggiunta di una vista all'interno del metodo Index*
3. Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della vista sia **indice**. Selezionare il **creare una visualizzazione fortemente tipizzata** opzione e selezionare **Album (MvcMusicStore.Models)** dal **classe modello** elenco a discesa. Selezionare **elenco** dalle **modelli di scaffolding** elenco a discesa. Lasciare il **motore di visualizzazione** al **Razor** mentre gli altri campi con valori predefiniti di valore e quindi fare clic su **Add**.

    ![Aggiunta di una visualizzazione indice](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "aggiunta di una visualizzazione indice")

    *Aggiunta di una visualizzazione indice*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>Attività 4: personalizzazione della visualizzazione Index scaffold

In questa attività si regolerà il semplice modello di vista creato con funzionalità di scaffolding di ASP.NET MVC in modo che i campi da visualizzare.

> [!NOTE]
> Il **scaffolding** supporto all'interno di ASP.NET MVC genera un semplice modello di vista in cui sono elencati tutti i campi nel modello di Album. **Lo scaffolding** offre un modo rapido per iniziare a usare in una visualizzazione fortemente tipizzata: invece di dover scrivere manualmente il modello di vista, lo scaffolding rapidamente genera un modello predefinito e quindi è possibile modificare il codice generato.


1. Esaminare il codice creato. L'elenco dei campi generato faranno parte della seguente tabella HTML **Scaffolding** utilizza per la visualizzazione di dati tabulari.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. Sostituire il **&lt;tabella&gt;** codice con il codice seguente per visualizzare solo i **Genre**, **artista**, **titolo Album**, e **prezzo** campi. Questa procedura elimina le **AlbumId** e **Album Art URL** colonne. Inoltre, viene modificato GenreId e ArtistId colonne per visualizzare le relative proprietà di classe collegati di **Artist.Name** e **Genre.Name**e rimuove il **dettagli** collegamento.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. Modificare le descrizioni seguenti.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Attività 5: esecuzione dell'applicazione

In questa attività si testerà che il **StoreManager** **indice** Visualizza modello viene visualizzato un elenco di album in base alla progettazione dei passaggi precedenti.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL **/StoreManager** per verificare che venga visualizzato un elenco di album, che mostra le **titolo**, **artista** e **Genre**.

    ![Esplorare l'elenco di album](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "esplorare l'elenco di album")

    *Esplorare l'elenco di album*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>Esercizio 2: Aggiunta di un HTML Helper

La pagina di indice StoreManager dispone di un potenziale problema: le proprietà di titolo e il nome dell'artista possono essere entrambi sufficientemente lungo per lanciare la formattazione della tabella. In questa esercitazione si apprenderà come aggiungere un helper HTML personalizzato per troncare il testo.

Nella figura seguente, è possibile visualizzare come il formato viene modificato a causa della lunghezza del testo quando si usa la dimensione small del browser.

![Esplorare l'elenco di album con di testo non troncato](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "esplorare l'elenco di album con di testo non troncato")

*Esplorare l'elenco di album con di testo non troncato*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>Attività 1: estendere l'HTML Helper

In questa attività si aggiungerà un nuovo metodo **Truncate** per il **HTML** oggetto esposto all'interno di viste di ASP.NET MVC. A tale scopo, si implementerà un **metodo di estensione** per l'oggetto incorporato **System** classe fornita da ASP.NET MVC.

> [!NOTE]
> Per altre informazioni, vedere **metodi di estensione**, vedere questo articolo di msdn. [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).


1. Aprire il **Begin** soluzione disponibile all'indirizzo **origine/Ex2-AddingAnHTMLHelper/inizio/** cartella. In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aprire la visualizzazione del StoreManager indice. A tale scopo, in Esplora soluzioni espandere la **viste** cartella, il **StoreManager** e aprire il **index. cshtml** file.
3. Aggiungere il codice seguente sotto il <strong>@model</strong> direttiva per definire il <strong>Truncate</strong> metodo helper.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>Attività 2 - testo troncamento nella pagina

In questa attività si userà il **Truncate** metodo per troncare il testo nel modello di visualizzazione.

1. Aprire la visualizzazione del StoreManager indice. A tale scopo, in Esplora soluzioni espandere la **viste** cartella, il **StoreManager** e aprire il **index. cshtml** file.
2. Sostituire le righe che mostrano le **nome dell'artista** e un album trae **titolo**. A tale scopo, sostituire le righe seguenti.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Attività 3: esecuzione dell'applicazione

In questa attività si testerà che il **StoreManager** **indice** Visualizza modello tronca titolo dell'Album e artista.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL **/StoreManager** per verificare tale prolungata testi nel **titolo** e **artista** colonna vengono troncati.

    ![Troncare i nomi dei titoli e gli artisti](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "troncato titoli e gli artisti nomi")

    *Troncato titoli e nomi di artista*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>Esercizio 3: Creazione della visualizzazione di modifica

In questo esercizio, si apprenderà come creare un form per consentire ai responsabili di archivio di modifica di un Album. Si sfoglierà il **/StoreManager/Edit/id** URL (**id** in corso l'id univoco dell'album da modificare), rendendo in tal modo una chiamata HTTP-GET per il server.

Il metodo di azione Modifica Controller verrà recuperare dell'Album appropriato dal database, creare un **StoreManagerViewModel** oggetto incapsularlo (insieme a un elenco degli artisti e generi) e quindi passarlo a un modello di vista eseguire il rendering della pagina HTML all'utente. Questa pagina contiene un **&lt;form&gt;** elemento con caselle di testo ed elenchi a discesa per modificare le proprietà dell'Album.

Dopo che l'utente aggiorna i valori del modulo Album e fa clic il **salvare** pulsante, le modifiche vengono inviate tramite una richiesta HTTP-POST richiamare **/StoreManager/Edit/id**. Anche se l'URL resta invariato come nell'ultima chiamata, ASP.NET MVC che identifica questa volta è una richiesta HTTP-POST e pertanto viene eseguito un altro metodo di azione di modifica (uno dotato **[HttpPost]**).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>Attività 1: l'implementazione del metodo di azione di modifica HTTP-GET

In questa attività si implementerà la versione HTTP-GET del metodo di azione di modifica per il recupero dell'Album appropriato dal database, nonché un elenco di tutti i generi e gli artisti. Verrà creato il pacchetto i dati nel **StoreManagerViewModel** definito nel passaggio precedente, che verrà quindi passato a un modello di vista per eseguire il rendering della risposta con oggetto.

1. Aprire il **Begin** soluzione disponibile all'indirizzo **origine/Ex3-CreatingTheEditView/inizio/** cartella. In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aprire il **StoreManagerController** classe. A tale scopo, espandere la **controller** cartella e fare doppio clic su **StoreManagerController.cs**.
3. Sostituire il **HTTP-GET modificare** metodo di azione con il codice seguente per recuperare l'oggetto appropriato **Album** , nonché **generi** e **alle creazioni degli artisti**sono elencati.

    (Code - Snippet *helper di ASP.NET MVC 4, moduli e convalida - Ex3 StoreManagerController HTTP-GET Modifica azione*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > Si usa **System** **SelectList** alle creazioni degli artisti e generi anziché il **System.Collections.Generic** elenco.
    > 
    > **SelectList** è un modo più semplice per popolare i menu a discesa HTML e gestione di elementi quali la selezione corrente. Creazione di istanze e successiva configurazione di questi oggetti ViewModel nell'azione del controller renderà lo scenario di modulo di modifica più chiara.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>Attività 2: creazione della visualizzazione di modifica

In questa attività si creerà un modello di visualizzazione di modifica in un secondo momento verrà visualizzate le proprietà di album.

1. Creare la visualizzazione di modifica. A tale scopo, fare doppio clic all'interno di **Edit** metodo di azione e selezionare **Aggiungi visualizzazione**.
2. Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della vista sia **modifica**. Controllare la **creare una visualizzazione fortemente tipizzata** casella di controllo e selezionare **Album (MvcMusicStore.Models)** dal **visualizzare dati classe** elenco a discesa. Selezionare **Edit** dalle **modelli di scaffolding** elenco a discesa. Lasciare gli altri campi con il valore predefinito e quindi fare clic su **Add**.

    ![Aggiunta di una visualizzazione di modifica](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "aggiunta di una visualizzazione di modifica")

    *Aggiunta di una visualizzazione di modifica*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Attività 3: esecuzione dell'applicazione

In questa attività si testerà che il **StoreManager** **modificare** visualizzazione pagina vengono visualizzati i valori delle proprietà dell'album passato come parametro.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL **/StoreManager/Edit/1** per verificare che vengano visualizzati i valori delle proprietà dell'album passato.

    ![Esplorazione di visualizzazione di modifica di un album trae](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Modifica visualizzazione un album trae di esplorazione")

    *Visualizzazione di modifica di un album trae di esplorazione*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>Attività 4: implementazione di elenchi a discesa sul modello di Album Editor

In questa attività si aggiungerà elenchi a discesa per il modello di vista creato nell'ultima attività, in modo che l'utente può selezionare da un elenco degli artisti e generi.

1. Sostituire tutte le **Album** fieldset codice con quanto segue:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > Un' **Html.DropDownList** helper è stato aggiunto per il rendering di elenchi a discesa per la scelta alle creazioni degli artisti e generi. I parametri passati al **Html.DropDownList** sono:
    > 
    > 1. Il nome del campo del form (**&quot;ArtistId&quot;**).
    > 2. Il **SelectList** dei valori per l'elenco a discesa.

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Attività 5: esecuzione dell'applicazione

In questa attività si testerà che il **StoreManager** **modificare** visualizzazione pagina vengono visualizzati elenchi a discesa anziché artista e dell'ID di genere campi di testo.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL **/StoreManager/Edit/1** per verificare che vengano visualizzati elenchi a discesa anziché artista e dell'ID di genere campi di testo.

    ![Esplorazione di visualizzazione di modifica di un album trae con elenchi a discesa](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Modifica visualizzazione esplorazione di un album trae con elenchi a discesa")

    *Visualizzazione di modifica di un album trae, questa volta con elenchi a discesa di esplorazione*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>Attività 6: l'implementazione del metodo di azione HTTP-POST modifica

Ora che la visualizzazione di modifica vengono visualizzate come previsto, è necessario implementare il metodo di azione di modifica HTTP-POST per salvare le modifiche apportate all'album.

1. Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio. Aprire **StoreManagerController** dalle **controller** cartella.
2. Sostituire **HTTP-POST modifica** codice del metodo azione con il codice seguente (si noti che il metodo che deve essere sostituito versione sottoposta a overload che riceve due parametri):

    (Code - Snippet *helper di ASP.NET MVC 4, moduli e convalida - Ex3 StoreManagerController HTTP-POST Modifica azione*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > Questo metodo verrà eseguito quando l'utente sceglie il **salvare** pulsante della visualizzazione ed esegue una richiesta HTTP-POST dei valori di form al server per renderle persistenti nel database. L'elemento decorator **[HttpPost]** indica che il metodo deve essere utilizzato per gli scenari HTTP-POST. Il metodo accetta un **Album** oggetto. ASP.NET MVC crea automaticamente l'oggetto di Album da registrato il &lt;form&gt; valori.
    > 
    > Il metodo verrà eseguiti questi passaggi:
    > 
    > 1. Se il modello è valido:
    > 
    >     1. Aggiornare la voce di album nel contesto per contrassegnarlo come un oggetto modificato.
    >     2. Salvare le modifiche e reindirizza alla vista index.
    > 2. Se il modello non è valido, verrà popolato ViewBag con il **GenreId** e **ArtistId**, verrà restituito la visualizzazione con l'oggetto Album ricevuto per consentire all'utente di eseguire qualsiasi aggiornamento obbligatorio.

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Attività 7: esecuzione dell'applicazione

In questa attività si testerà che il **StoreManager modifica** Visualizza pagina Salva effettivamente i dati di Album aggiornati nel database.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL **/StoreManager/Edit/1**. Modificare il titolo Album **Load** e fare clic su **salvare**. Verificare che il titolo dell'album effettivamente modificato nell'insieme di album.

    ![L'aggiornamento di un album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "l'aggiornamento di un album")

    *L'aggiornamento di un Album*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>Esercizio 4: Aggiunta di una visualizzazione Create

Ora che il **StoreManagerController** supporta il **modificare** possibilità, in questo esercizio si apprenderà come aggiungere un modello Create View per consentono di archiviare Manager aggiungere nuovo album all'applicazione.

Come con la funzionalità di modifica, si implementerà lo scenario di creazione usando due metodi distinti all'interno di **StoreManagerController** classe:

1. Un metodo di azione verrà visualizzato un form vuoto quando gestori store visita prima la **StoreManager/crea** URL.
2. Un secondo metodo di azione gestirà lo scenario in cui il gestore dell'archivio fa clic sul **salvare** pulsante all'interno del form e invia nuovamente i valori per il **StoreManager/crea** URL come una richiesta HTTP-POST.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>Attività 1: l'implementazione del metodo di azione HTTP-GET creare

In questa attività si implementerà la versione HTTP-GET del metodo di azione Create per recuperare un elenco di tutti i generi e gli artisti, i pacchetti con questi dati un **StoreManagerViewModel** oggetto, che verrà quindi passato a un modello di visualizzazione.

1. Aprire il **Begin** soluzione disponibile all'indirizzo **origine/Ex4-AddingACreateView/inizio/** cartella. In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aprire **StoreManagerController** classe. A tale scopo, espandere la **controller** cartella e fare doppio clic su **StoreManagerController.cs**.
3. Sostituire il **Create** codice del metodo azione con il codice seguente:

    (Code - Snippet *helper di ASP.NET MVC 4, moduli e convalida - azione Ex4 StoreManagerController HTTP-GET crea*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>Attività 2: aggiunta della visualizzazione Create

In questa attività si aggiungerà il modello Create View che verrà visualizzato un nuovo form Album (vuoto).

1. Pulsante destro del mouse all'interno di **Create** metodo di azione e selezionare **Aggiungi visualizzazione**. Verrà visualizzata la finestra di dialogo Aggiungi visualizzazione.
2. Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della vista sia **Create**. Selezionare il **creare una visualizzazione fortemente tipizzata** opzione e selezionare **Album (MvcMusicStore.Models)** dal **classe modello** elenco a discesa e **crea** dal **modello di scaffolding** elenco a discesa. Lasciare gli altri campi con il valore predefinito e quindi fare clic su **Add**.

    ![Aggiunta di una visualizzazione di creazione](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "aggiunta-a-create-view.png")

    *Aggiunta della visualizzazione Create*
3. Aggiorna il **GenreId** e **ArtistId** campi da usare un elenco di riepilogo a discesa come illustrato di seguito:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Attività 3: esecuzione dell'applicazione

In questa attività si testerà che il **StoreManager** **crea** visualizzazione pagina viene visualizzato un form vuoto di Album.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL **StoreManager/creare**. Verificare che venga visualizzato un form vuoto per riempire le nuove proprietà di Album.

    ![Creare una vista con un form vuoto](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View con un form vuoto")

    *Creare una vista con un form vuoto*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>Attività 4: implementare il protocollo HTTP-POST Crea metodo di azione

In questa attività si implementerà la versione HTTP-POST del metodo di azione di creazione che verrà richiamato quando un utente sceglie il **salvare** pulsante. Il metodo deve salvare il nuovo album nel database.

1. Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio. Aprire **StoreManagerController** classe. A tale scopo, espandere la **controller** cartella e fare doppio clic su **StoreManagerController.cs**.
2. Sostituire **HTTP-POST creare** codice del metodo azione con il codice seguente:

    (Code - Snippet *helper di ASP.NET MVC 4, moduli e convalida - Ex4 StoreManagerController HTTP - POST azione crea*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > L'azione di creazione è molto simile al metodo di azione di modifica precedente, ma anziché impostare l'oggetto come modificata, viene aggiunto al contesto.

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Attività 5: esecuzione dell'applicazione

In questa attività si testerà che il **StoreManager creare** visualizzazione pagina consente di creare un nuovo Album e viene quindi reindirizzata alla vista Index StoreManager.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL **StoreManager/creare**. Compilare tutti i campi modulo con i dati per un nuovo Album, simile a quello nella figura seguente:

    ![Creazione di un Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "la creazione di un Album")

    *Creazione di un Album*
3. Verificare che l'utente viene reindirizzato alla vista Index StoreManager che include il nuovo Album appena creato.

    ![Creato Nuovo Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "Nuovo Album creato")

    *Nuovo Album creato*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>Esercizio 5: Gestisce l'eliminazione

La possibilità di eliminare gli album non è ancora implementata. Questo è ciò che in questo esercizio sarà su. Come in precedenza, si implementerà lo scenario di eliminazione utilizzando due metodi distinti all'interno di **StoreManagerController** classe:

1. Un metodo di azione verrà visualizzato un form di conferma
2. Un secondo metodo di azione gestirà l'invio del modulo

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>Attività 1: l'implementazione del metodo di azione Delete HTTP-GET

In questa attività si implementerà la versione HTTP-GET del metodo di azione Delete per recuperare le informazioni dell'album.

1. Aprire il **Begin** soluzione disponibile all'indirizzo **origine/Ex5-HandlingDeletion/inizio/** cartella. In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aprire **StoreManagerController** classe. A tale scopo, espandere la **controller** cartella e fare doppio clic su **StoreManagerController.cs**.
3. L'azione del controller Delete è esattamente quello utilizzato per l'azione del controller Store dettagli precedente: viene eseguita una query il **album** oggetto dal database utilizzando il **id** forniti l'URL e restituisce il appropriato **vista**. A tale scopo, sostituire l'HTTP-GET **eliminare** codice del metodo azione con il codice seguente:

    (Code - Snippet *helper di ASP.NET MVC 4, moduli e convalida - HTTP-GET di eliminazione gestione Ex5 Elimina azione*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. Pulsante destro del mouse all'interno di **eliminare** metodo di azione e selezionare **Aggiungi visualizzazione**. Verrà visualizzata la finestra di dialogo Aggiungi visualizzazione.
5. Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della vista sia **Elimina**. Selezionare il **creare una visualizzazione fortemente tipizzata** opzione e selezionare **Album (MvcMusicStore.Models)** dal **classe modello** elenco a discesa. Selezionare **eliminare** dalle **modelli di scaffolding** elenco a discesa. Lasciare gli altri campi con il valore predefinito e quindi fare clic su **Add**.

    ![Aggiunta di una visualizzazione di eliminazione](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "aggiunta di una visualizzazione di eliminazione")

    *Aggiunta di una visualizzazione di eliminazione*
6. Il modello di eliminazione Mostra tutti i campi dal modello. Si visualizzerà solo titolo dell'album. A tale scopo, sostituire il contenuto della visualizzazione con il codice seguente:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Attività 2: esecuzione dell'applicazione

In questa attività si testerà che il **StoreManager** **eliminare** visualizzazione pagina viene visualizzato un form di conferma dell'eliminazione.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL **/StoreManager**. Selezionare un album eliminare facendo **Elimina** e verificare che la nuova vista venga caricata.

    ![Eliminazione di un Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "l'eliminazione di un Album")

    *Eliminazione di un Album*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>Attività 3: implementazione del metodo di azione Delete HTTP-POST

In questa attività si implementerà la versione HTTP-POST del metodo di azione Delete che verrà richiamato quando un utente sceglie il **eliminare** pulsante. Il metodo deve l'eliminazione dell'album nel database.

1. Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio. Aprire **StoreManagerController** classe. A tale scopo, espandere la **controller** cartella e fare doppio clic su **StoreManagerController.cs**.
2. Sostituire **HTTP-POST Delete** codice del metodo azione con il codice seguente:

    (Code - Snippet *helper di ASP.NET MVC 4, moduli e convalida - HTTP-POST di eliminazione gestione Ex5 Elimina azione*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Attività 4: esecuzione dell'applicazione

In questa attività si testerà che il **StoreManager Delete** visualizzazione pagina consente di eliminare un Album e viene quindi reindirizzata alla vista Index StoreManager.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL **/StoreManager**. Selezionare un album eliminare facendo **eliminare.** Confermare l'eliminazione facendo **eliminare** pulsante:

    ![Eliminazione di un Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "l'eliminazione di un Album")

    *Eliminazione di un Album*
3. Verificare che sia stato eliminato dell'album poiché non viene visualizzato nei **indice** pagina.

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>Esercizio 6: Aggiunta della convalida

Attualmente, le forme di creazione e modifica che sia non eseguono qualsiasi tipo di convalida. Se l'utente lascia un campo obbligatorio vuoto o lettere tipo del campo prezzo, il primo errore che verrà visualizzato sarà dal database.

È possibile aggiungere la convalida per l'applicazione mediante l'aggiunta di annotazioni dei dati per la classe di modello. Annotazioni dei dati per consentono che descrive le regole desiderate applicate alle proprietà del modello e ASP.NET MVC eseguirà l'applicazione e visualizzazione messaggio appropriato per gli utenti.

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>Attività 1: aggiunta di annotazioni dei dati

In questa attività si aggiungerà le annotazioni dei dati al modello di Album che renderà la pagina di creazione e modifica visualizzare i messaggi di convalida quando appropriato.

Per una classe di modello semplice, aggiunta di un'annotazione dei dati viene gestita solo tramite l'aggiunta di un **usando** istruzione for **System.ComponentModel.DataAnnotation**, quindi inserire un **[obbligatorio]** attributo per le proprietà appropriate. Renderebbe l'esempio seguente il **nome** proprietà un campo obbligatorio nella visualizzazione.

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

Questo è un po' più complesso in casi come questa applicazione in cui viene generato l'Entity Data Model. Se è stato aggiunto le annotazioni dei dati direttamente le classi del modello, essi verranno sovrascritti se si aggiorna il modello dal database. In alternativa, è possibile apportare le classi di uso delle classi parziali che esisterà per contenere le annotazioni e sono associati al modello usando il **[MetadataType]** attributo.

1. Aprire il **Begin** soluzione disponibile all'indirizzo **inizio/origine/Ex6-AddingValidation/** cartella. In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aprire il **Album.cs** dalle **modelli** cartella.
3. Sostituire **Album.cs** del contenuto con il codice evidenziato di seguito, in modo che risulti simile al seguente:

    > [!NOTE]
    > La riga **[DisplayFormat(ConvertEmptyStringToNull=false)]** indica che le stringhe vuote dal modello non verranno convertite in null quando il campo dati viene aggiornato nell'origine dati. Questa impostazione verrà evitare un'eccezione quando Entity Framework assegna i valori null per il modello prima di annotazione dei dati verifica i campi.

    (Code - Snippet *helper di ASP.NET MVC 4, moduli e convalida - classe parziale i metadati di Album Ex6*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > Ciò **Album** classe parziale ha un' **MetadataType** attributo che fa riferimento al **AlbumMetaData** classe per le annotazioni dei dati. Questi sono alcuni degli attributi di annotazione dei dati in uso per annotare il modello di Album:
    > 
    > - Richiesto: indica che la proprietà è un campo obbligatorio
    > - DisplayName - definisce il testo da usare per i campi del form e i messaggi di convalida
    > - DisplayFormat: specifica la modalità di visualizzazione e formattazione dei campi dati.
    > - StringLength - definisce una lunghezza massima per un campo stringa
    > - Intervallo: fornisce un valore minimo e massimo per un campo numerico
    > - ScaffoldColumn - consente di nascondere i campi dall'editor form

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Attività 2: esecuzione dell'applicazione

In questa attività si testerà che le pagine di creazione e modifica è convalidare campi, usando i nomi visualizzati scelti nell'ultima attività.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL **StoreManager/creare**. Verificare che i nomi visualizzati corrispondano a quelli nella classe parziale (ad esempio **URL Art Album** invece di **AlbumArtUrl**)
3. Fare clic su **Create**, senza la compilazione del form. Verificare che i messaggi di convalida corrispondente.

    ![Convalidare i campi nella pagina di creazione](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "convalidati i campi nella pagina Crea")

    *Convalidato i campi nella pagina Crea*
4. È possibile verificare che lo stesso accade con la **modifica** pagina. Modificare l'URL **/StoreManager/Edit/1** e verificare che i nomi visualizzati corrispondano a quelli nella classe parziale (ad esempio **Album Art URL** invece di **AlbumArtUrl**). Vuoto il **Title** e **prezzo** campi e fare clic su **Salva**. Verificare che i messaggi di convalida corrispondente.

    ![Convalidato campi nella pagina di modifica](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *Convalidato campi nella pagina di modifica*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>Esercizio 7: Uso di jQuery Unobtrusive sul lato Client

In questo esercizio, si apprenderà come abilitare MVC 4 Unobtrusive validation di jQuery sul lato client.

> [!NOTE]
> Il componente aggiuntivo jQuery Unobtrusive Usa dati ajax prefisso JavaScript per richiamare i metodi di azione sul server anziché intrusivo che genera gli script client inline.


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>Attività 1: esecuzione dell'applicazione prima di jQuery Unobtrusive abilitazione

In questa attività si eseguirà l'applicazione prima di includere jQuery per confrontare i modelli di convalida.

1. Aprire il **Begin** soluzione disponibile all'indirizzo **inizio/origine/Ex7-UnobtrusivejQueryValidation/** cartella. In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Premere **F5** per eseguire l'applicazione.
3. Il progetto viene avviato nella Home page. Esplorare **StoreManager/creare** e fare clic su **crea** senza compilare il modulo per verificare che i messaggi di convalida:

    ![Convalida del client disabilitata](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "disabilitata la convalida del Client")

    *Disabilitata la convalida del client*
4. Nel browser, aprire il codice sorgente HTML:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>Attività 2: abilitare la convalida del Client non intrusivi

In questa attività si abiliterà jQuery **la convalida del client non intrusivi** dalla **Web. config** file, che è impostato su false in tutti i nuovi progetti ASP.NET MVC 4 per impostazione predefinita. Inoltre, si aggiungerà che la necessità di generare script i riferimenti per rendere jQuery lavoro Unobtrusive Validation di Client.

1. Aprire **Web. config** file alla radice del progetto e assicurarsi che il **ClientValidationEnabled** e **UnobtrusiveJavaScriptEnabled** i valori delle chiavi vengono impostati **true**.

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > È anche possibile abilitare la convalida del client dal codice in Global.asax.cs per ottenere gli stessi risultati:
    > 
    > **HtmlHelper.ClientValidationEnabled = true;**
    > 
    > Inoltre, è possibile assegnare attributi ClientValidationEnabled in qualsiasi controller di avere un comportamento personalizzato.
2. Aprire **create. cshtml** alla **Views\StoreManager**.
3. Assicurarsi che i file di script seguente, **jQuery. Validate** e **jquery.validate.unobtrusive**, viene fatto riferimento in tramite la vista il &quot; **~/bundles/jqueryval** &quot; bundle.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > Tutte queste librerie jQuery sono incluse nei nuovi progetti MVC 4. È possibile trovare altre librerie nel **/Scripts** cartella del progetto.
    > 
    > Per rendere questa convalida il funzionamento delle librerie, è necessario aggiungere un riferimento alla libreria framework jQuery. Poiché il riferimento è già stato aggiunto nel  **\_layout. cshtml** file, è necessario aggiungerlo in questa vista specifica.

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>Attività 3: esegue la convalida jQuery non intrusivo usando Application

In questa attività si testerà che il **StoreManager** creare vista modello esegue la convalida lato client usando le librerie di jQuery quando l'utente crea un nuovo album.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Esplorare **StoreManager/creare** e fare clic su **crea** senza compilare il modulo per verificare che i messaggi di convalida:

    ![Convalida del client con jQuery abilitata](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "convalida del Client con jQuery abilitata")

    *Convalida del client con jQuery abilitata*
3. Nel browser, aprire il codice sorgente per la visualizzazione di creazione:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > Per ogni regola di convalida del client, jQuery Unobtrusive aggiunge un attributo con i dati-- val*rulename*=&quot;*messaggio*&quot;. Ecco un elenco di tag che Unobtrusive jQuery viene inserito nel campo di input html per eseguire la convalida del client:
   > 
   > - Dati val
   > - Numero dei dati-val
   > - Intervallo di dati val
   > - Data-val-intervallo-min / dati-val-intervallo-max
   > - Dati val necessari
   > - I dati a val-lunghezza
   > - Data-val-length-max / min-Data-val-lunghezza
   > 
   > Tutti i valori di data vengono riempiti con modello **annotazione dei dati**. Quindi, tutta la logica che funziona sul lato server può essere eseguita sul lato client. Ad esempio, prezzo e presenta l'annotazione dei dati seguenti nel modello:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > Dopo l'uso di jQuery Unobtrusive, il codice generato è:
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa pratica si è appreso come consentire agli utenti di modificare i dati archiviati nel database con l'uso delle operazioni seguenti:

- Le azioni del controller, ad esempio Index, Create, Edit, Delete
- Funzionalità di scaffolding di ASP.NET MVC per visualizzare le proprietà in una tabella HTML
- Esperienza di helper HTML personalizzati per migliorare l'utente
- Metodi di azione che reagiscono a HTTP-GET o le chiamate HTTP-POST
- Un modello di editor condivisa per i modelli di vista simile, ad esempio creazione e modifica
- Elementi del form, ad esempio elenchi a discesa
- Annotazioni dei dati per la convalida del modello
- Convalida lato client con jQuery Unobtrusive libreria

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice a: installazione di Visual Studio Express 2012 per Web

È possibile installare **Microsoft Visual Studio Express 2012 per Web** o da un'altra &quot;Express&quot; versione utilizzando il **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web Microsoft*.

1. Passare a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e cercare il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **Installa ora**. Se non hai **instalace Webové Platformy** si verrà reindirizzati per scaricarlo e installarlo prima di tutto.
3. Una volta **instalace Webové Platformy** è aperto, fare clic su **installare** per avviare il programma di installazione.

    ![Installa Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "installa Visual Studio Express")

    *Installa Visual Studio Express*
4. Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.

    ![Accettare le condizioni di licenza](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *Accettare le condizioni di licenza*
5. Attendere finché non viene completato il processo di download e l'installazione.

    ![Stato dell'installazione](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *Installazione completata*
7. Fare clic su **Exit** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per Web, fare clic per il **avviare** schermata e iniziare la scrittura &quot; **Visual Studio Express**&quot;, quindi fare clic sul **Visual Studio Express per Web** il riquadro.

    ![Visual Studio Express per il riquadro Web](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *Visual Studio Express per il riquadro Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Appendice b: uso dei frammenti di codice

Con i frammenti di codice, hai tutto il codice che necessario a tua disposizione. Il documento lab indicherà esattamente quando usarli, come illustrato nella figura seguente.

![Uso di frammenti di codice di Visual Studio per inserire codice nel progetto](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "frammenti di codice con Visual Studio per inserire codice nel progetto")

*Uso di frammenti di codice di Visual Studio per inserire codice nel progetto*

***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***

1. Posizionare il cursore in cui si vuole inserire il codice.
2. Iniziare a digitare il nome del frammento di codice (senza spazi o trattini).
3. Guarda come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento di codice corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).
5. Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.

![Iniziare a digitare il nome di frammento](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "inizia a digitare il nome del frammento di codice")

*Iniziare a digitare il nome del frammento di codice*

![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "premere Tab per selezionare il frammento di codice evidenziata")

*Premere Tab per selezionare il frammento di codice evidenziata*

![Il frammento di codice e premere nuovamente Tab espanderà](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "si espanderà il frammento di codice e premere nuovamente Tab")

*Il frammento di codice e premere nuovamente Tab espanderà*

***Per aggiungere un frammento di codice usando il mouse (c#, Visual Basic e XML)*** 1. Pulsante destro del mouse in cui si desidera inserire il frammento di codice.

1. Selezionare **Inserisci frammento** aggiungendo **frammenti di codice**.
2. Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.

![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "rapida in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice")

*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice*

![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa")

*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa*
