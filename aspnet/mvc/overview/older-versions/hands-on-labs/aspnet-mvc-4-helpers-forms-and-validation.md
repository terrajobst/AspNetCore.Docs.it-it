---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: La convalida, moduli e gli helper di ASP.NET MVC 4 | Documenti Microsoft
author: rick-anderson
description: "In ASP.NET MVC 4 modelli e le esercitazioni pratiche accesso ai dati, è stato durante il caricamento e visualizzazione dei dati dal database. In questo laboratorio pratico, verranno aggiunti per il..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 4dd10430778dc51fef1199315ee02eb2cd4970ba
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-helpers-forms-and-validation"></a>Convalida, moduli e gli helper di ASP.NET MVC 4
====================
da [categorie Web Team](https://twitter.com/webcamps)

> In **accesso ai dati e i modelli di ASP.NET MVC 4** pratica, si è stati durante il caricamento e visualizzazione dei dati dal database. In questo laboratorio pratico, verranno aggiunti per il **negozio** applicazione la possibilità di modificare i dati.
> 
> Con tale scopo, si creerà innanzitutto il controller che supporterà le azioni di creazione, lettura, aggiornamento ed eliminazione (CRUD) di album. Verrà generato un modello di visualizzazione dell'indice usufruire della funzionalità di scaffolding di ASP.NET MVC per visualizzare le proprietà degli album in una tabella HTML. Per migliorare la visualizzazione, si aggiungerà un helper HTML personalizzato che verrà troncato descrizioni lunghe.
> 
> Successivamente, si aggiungerà la modifica e creazione di visualizzazioni che consente di modificare gli album nel database, con l'aiuto di elementi del form come elenchi a discesa.
> 
> Infine, si permettono agli utenti di eliminare un album e inoltre si impedirà l'immissione di dati errati, convalidando l'input.
> 
> > [!NOTE]
> > Questa pratica presuppone avere conoscenze di base **ASP.NET MVC**. Se non è stato utilizzato **ASP.NET MVC** in precedenza, è consigliabile esaminare **nozioni fondamentali su MVC ASP.NET** le esercitazioni pratiche.
> 
> 
> Questa esercitazione illustra i miglioramenti e nuove funzionalità descritte in precedenza tramite l'applicazione di modifiche di lieve entità a un'applicazione Web di esempio fornita nella cartella di origine.
> 
> Tutto il codice di esempio e i frammenti di codice sono inclusi nel Web categorie Training Kit, disponibile all'indirizzo [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questa pratica, si apprenderà come:

- Creare un controller per supportare le operazioni CRUD
- Generare una visualizzazione di indice per visualizzare le proprietà delle entità in una tabella HTML
- Aggiungere un helper HTML personalizzati
- Creare e personalizzare una visualizzazione di modifica
- Distinguere tra i metodi di azione che reagiscono a HTTP-GET o chiamate HTTP-POST
- Aggiungere e personalizzare un'istruzione Create View
- Gestire l'eliminazione di un'entità
- Convalidare l'input dell'utente

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

Se non si ha familiarità con i frammenti di codice di Visual Studio e si desidera per informazioni su come usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice b: tramite i frammenti di codice](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Esercizi seguenti che costituiscono questa pratica:

1. [Creazione di controller di gestione di Store e la visualizzazione dell'indice](#Exercise1)
2. [Aggiunta di un HTML Helper](#Exercise2)
3. [Creazione della visualizzazione di modifica](#Exercise3)
4. [Aggiunta di una vista di creazione](#Exercise4)
5. [Eliminazione di gestione](#Exercise5)
6. [Aggiunta della convalida](#Exercise6)
7. [Utilizzo di jQuery non intrusiva sul lato Client](#Exercise7)

> [!NOTE]
> Ogni esercizio è accompagnato da un **fine** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato l'esercitazione dall'inizio. Se è necessario ulteriore assistenza utilizza gli esercizi, è possibile utilizzare questa soluzione come guida.


Tempo stimato per completare questa esercitazione: **60 minuti**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>Esercizio 1: Creazione di controller di gestione di Store e la visualizzazione dell'indice

In questo esercizio, si apprenderà come creare un nuovo controller per supportare le operazioni CRUD, personalizzare il relativo metodo di azione Index per restituire un elenco di album dal database e infine la generazione di un modello di visualizzazione dell'indice sfruttando la possibilità di scaffolding di ASP.NET MVC funzionalità per visualizzare le proprietà degli album in una tabella HTML.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>Attività 1 - Creazione di StoreManagerController

In questa attività si creerà un nuovo controller chiamato **StoreManagerController** per supportare le operazioni CRUD.

1. Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex1-CreatingTheStoreManagerController/Begin/** cartella.

    1. È necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
    2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
    3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

    > [!NOTE]
    > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aggiunge un nuovo controller. A tale scopo, fare doppio clic su di **controller** cartella in Esplora soluzioni, selezionare **Aggiungi** e quindi la **Controller** comando. Modifica il **Controller** **nome** a **StoreManagerController** e accertarsi che l'opzione **controller MVC con azioni di lettura/scrittura vuote**è selezionata. Fare clic su **Aggiungi**.

    ![Finestra di dialogo Aggiungi controller](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "finestra di dialogo Aggiungi controller")

    *Aggiungere una finestra di dialogo Controller*

    Viene generata una nuova classe Controller. Poiché è indicato per aggiungere le azioni di lettura/scrittura, i metodi stub per gli utenti, le azioni CRUD comuni vengono create con i commenti TODO compilati, verrà richiesto di includere la logica specifica dell'applicazione.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>Attività 2 - personalizzare l'indice StoreManager

In questa attività si personalizzerà il metodo di azione StoreManager indice per restituire una vista con l'elenco di album dal database.

1. Nella classe StoreManagerController, aggiungere le seguenti *utilizzando* direttive.

    (- Frammento di codice *convalida - Ex1 e form e ASP.NET MVC 4 helper utilizzando MvcMusicStore*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. Aggiungere un campo di **StoreManagerController** per contenere un'istanza di **MusicStoreEntities.**

    (- Frammento di codice *gli helper di ASP.NET MVC 4, moduli e convalida - Ex1 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. Implementare l'operazione sull'indice di StoreManagerController per restituire una vista con l'elenco di album.

    La logica di azione del Controller sarà molto simile all'azione di indice del StoreController scritto in precedenza. Utilizzare LINQ per recuperare tutti gli album, incluse le informazioni Genre e artista per la visualizzazione.

    (- Frammento di codice *gli helper di ASP.NET MVC 4, moduli e convalida - Ex1 StoreManagerController indice*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>Attività 3: creazione della visualizzazione dell'indice

In questa attività si creerà il modello di visualizzazione dell'indice per visualizzare l'elenco di album restituito dal **StoreManager** Controller.

1. Prima di creare il nuovo modello di visualizzazione, è necessario compilare il progetto in modo che il **finestra di dialogo Aggiungi visualizzazione** viene a conoscenza di **Album** classe da utilizzare. Selezionare **compilare | Compilare MvcMusicStore** per compilare il progetto.
2. Fare doppio clic all'interno di **indice** metodo di azione e selezionare **Aggiungi visualizzazione**. Verrà visualizzata la **Aggiungi visualizzazione** finestra di dialogo.

    ![Aggiungi visualizzazione](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Aggiungi visualizzazione")

    *Aggiunta di una vista all'interno del metodo di indice*
3. Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della visualizzazione è **indice**. Selezionare il **creare una visualizzazione fortemente tipizzata** opzione e selezionare **Album (MvcMusicStore.Models)** dal **classe modello** elenco a discesa. Selezionare **elenco** dal **modello di scaffolding** elenco a discesa. Lasciare il **motore di visualizzazione** a **Razor** e gli altri campi con valori predefiniti di valore e quindi fare clic su **Aggiungi**.

    ![Aggiunta di una visualizzazione index](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "aggiunta di una vista di indice")

    *Aggiunta di una vista di indice*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>Attività 4: personalizzare la pagina della visualizzazione dell'indice

In questa attività, si potrà regolare il modello di visualizzazione semplice creato con funzionalità di scaffolding di ASP.NET MVC per visualizzare i campi desiderati.

> [!NOTE]
> Il **scaffolding** supporto all'interno di ASP.NET MVC genera un semplice modello di visualizzazione in cui sono elencati tutti i campi nel modello Album. **Lo scaffolding** offre un modo rapido per un'introduzione a una visualizzazione fortemente tipizzata: invece di scrivere manualmente il modello di visualizzazione, lo scaffolding rapidamente genera un modello predefinito e quindi è possibile modificare il codice generato.


1. Esaminare il codice creato. L'elenco di campi generato sarà parte del seguente tabella HTML **Scaffolding** utilizzata per la visualizzazione dei dati tabulari.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. Sostituire il  **&lt;tabella&gt;**  codice con il codice seguente per visualizzare solo il **Genre**, **artista**, **titolo**, e **prezzo** campi. Questo elimina la **payload** e **Album Art URL** colonne. Inoltre, viene modificato GenreId e ArtistId colonne per visualizzare le proprietà di classe collegati di **Artist.Name** e **Genre.Name**e rimuove il **dettagli** collegamento.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. Modificare le descrizioni seguenti.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Attività 5: esecuzione dell'applicazione

In questa attività, si verificherà che il **StoreManager** **indice** modello di visualizzazione viene visualizzato un elenco di album in base alla progettazione dei passaggi precedenti.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **/StoreManager** per verificare che venga visualizzato un elenco di album, che mostra i **titolo**, **artista** e **Genre**.

    ![Esplorazione dell'elenco di album](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "esplorazione dell'elenco di album")

    *Esplorazione dell'elenco di album*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>Esercizio 2: Aggiunta di un HTML Helper

La pagina di indice StoreManager dispone di un potenziale problema: la proprietà Title e Name artista può essere entrambi sufficientemente lungo per lanciare la formattazione della tabella. In questo esercizio si apprenderà come aggiungere un helper HTML personalizzato per troncare il testo.

Nella figura seguente, è possibile visualizzare come viene modificato il formato a causa della lunghezza del testo quando si utilizza una dimensione piccola del browser.

![Esplorazione dell'elenco di album con di testo non troncato](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "esplorazione dell'elenco di album con di testo non troncato")

*Esplorazione dell'elenco di album con di testo non troncato*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>Attività 1 - estensione l'HTML Helper

In questa attività si aggiungerà un nuovo metodo **Truncate** per il **HTML** oggetto esposto all'interno di viste di MVC ASP.NET. A tale scopo, implementare un **metodo di estensione** agli incorporati **System** classe fornita da ASP.NET MVC.

> [!NOTE]
> Per ulteriori informazioni su **metodi di estensione**, visitare questo articolo di msdn. [https://msdn.microsoft.com/en-us/library/bb383977.aspx](https://msdn.microsoft.com/en-us/library/bb383977.aspx).


1. Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex2-AddingAnHTMLHelper/Begin/** cartella. In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.

    1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
    2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
    3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

    > [!NOTE]
    > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aprire la visualizzazione di StoreManager indice. A tale scopo, in Esplora soluzioni espandere la **viste** cartella, quindi il **StoreManager** e aprire il **cshtml** file.
3. Aggiungere il codice seguente sotto il  **@model**  (direttiva) per definire il **Truncate** metodo di supporto.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>Attività 2 - testo troncato nella pagina

In questa attività, si utilizzerà il **Truncate** metodo troncare il testo nel modello di visualizzazione.

1. Aprire la visualizzazione di StoreManager indice. A tale scopo, in Esplora soluzioni espandere la **viste** cartella, quindi il **StoreManager** e aprire il **cshtml** file.
2. Sostituire le righe che mostrano il **Nome artista** dell'Album e **titolo**. A tale scopo, sostituire le righe seguenti.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Attività 3: esecuzione dell'applicazione

In questa attività, si verificherà che il **StoreManager** **indice** modello di visualizzazione tronca titolo e il nome artista dell'Album.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **/StoreManager** per verificare che prolungata testi nel **titolo** e **artista** colonna vengono troncati.

    ![I nomi dei titoli e artisti troncato](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "troncati i nomi dei titoli e artisti")

    *Titoli troncati e i nomi degli artisti*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>Esercizio 3: Creazione della visualizzazione di modifica

In questo esercizio, si apprenderà come creare un form per consentire ai responsabili di archivio di modifica di un Album. Verrà esplorano il **/StoreManager/Edit/id** URL (**id** da id univoco dell'album da modificare), rendendo in tal modo una chiamata HTTP-GET al server.

Il metodo di azione Modifica Controller verranno recuperare Album appropriato dal database, creare un **StoreManagerViewModel** incapsularlo (insieme a un elenco degli artisti e generi) e quindi passare a un modello di visualizzazione per oggetto all'utente, eseguire il rendering della pagina HTML. Questa pagina conterrà un  **&lt;modulo&gt;**  elemento con caselle di testo ed elenchi a discesa per modificare le proprietà dell'Album.

Dopo che l'utente aggiorna i valori del form Album e fa clic il **salvare** pulsante, le modifiche vengono inviate tramite un POST HTTP richiamare **/StoreManager/Edit/id**. Anche se l'URL viene mantenuta come l'ultima chiamata, ASP.NET MVC che identifica questa volta è un POST HTTP e pertanto viene eseguito un altro metodo di azione di modifica (uno decorati con **[HttpPost]**).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>Attività 1: implementazione del metodo di azione di modifica HTTP-GET

In questa attività verrà implementata la versione HTTP-GET del metodo di azione di modifica per recuperare Album appropriato dal database, nonché un elenco di tutti i generi e artisti. Creerà un pacchetto dati nel **StoreManagerViewModel** oggetto definito nell'ultimo passaggio, verrà quindi passato a un modello di visualizzazione per il rendering con la risposta.

1. Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex3-CreatingTheEditView/Begin/** cartella. In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.

    1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
    2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
    3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

    > [!NOTE]
    > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aprire il **StoreManagerController** classe. A tale scopo, espandere il **controller** cartella e fare doppio clic su **StoreManagerController.cs**.
3. Sostituire il **HTTP-GET modifica** il metodo di azione con il codice seguente per recuperare l'oggetto appropriato **Album** , nonché **generi** e **artisti**Elenca.

    (- Frammento di codice *helper di ASP.NET MVC 4, moduli e convalida - Ex3 StoreManagerController HTTP-GET Modifica azione*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > Si utilizza **System** **SelectList** per artisti e generi anziché il **System.Collections.Generic** elenco.
    > 
    > **SelectList** è un modo più semplice per popolare i menu a discesa HTML e gestione di elementi quali la selezione corrente. Creazione di un'istanza e la successiva configurazione di questi oggetti ViewModel nell'azione del controller rende scenario del modulo di modifica più chiara.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>Attività 2: creazione della visualizzazione di modifica

In questa attività si creerà un modello di visualizzazione di modifica che consente di visualizzare in un secondo momento le proprietà album.

1. Creare la visualizzazione di modifica. A tale scopo, fare doppio clic all'interno di **modifica** metodo di azione e selezionare **Aggiungi visualizzazione**.
2. Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della visualizzazione è **modifica**. Controllare il **creare una visualizzazione fortemente tipizzata** casella di controllo e selezionare **Album (MvcMusicStore.Models)** dal **visualizzare dati classe** elenco a discesa. Selezionare **modifica** dal **modello di scaffolding** elenco a discesa. Lasciare gli altri campi con il valore predefinito e quindi fare clic su **Aggiungi**.

    ![Aggiunta di una visualizzazione di modifica](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "aggiunta di una visualizzazione di modifica")

    *Aggiunta di una visualizzazione di modifica*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Attività 3: esecuzione dell'applicazione

In questa attività, si verificherà che il **StoreManager** **modifica** pagina di visualizzazione consente di visualizzare i valori delle proprietà dell'album passato come parametro.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **/StoreManager/Edit/1** per verificare che vengano visualizzati i valori delle proprietà dell'album passato.

    ![Visualizzazione di modifica dell'Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Modifica visualizzazione Album esplorazione")

    *Visualizzazione di modifica dell'Album.*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>Attività 4: implementazione di elenchi a discesa sul modello di Editor Album

In questa attività si aggiungerà gli elenchi a discesa per il modello di visualizzazione creato nell'ultima attività, in modo che l'utente può selezionare da un elenco degli artisti e generi.

1. Sostituisci tutto il **Album** fieldset codice con quanto segue:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > Un **Html.DropDownList** è stato aggiunto supporto per elenchi a discesa per la scelta artisti e generi di eseguire il rendering. I parametri passati al **Html.DropDownList** sono:
    > 
    > 1. Il nome del campo del form (**&quot;ArtistId&quot;**).
    > 2. Il **SelectList** dei valori di elenco a discesa.

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Attività 5: esecuzione dell'applicazione

In questa attività, si verificherà che il **StoreManager** **modifica** pagina di visualizzazione consente di visualizzare gli elenchi a discesa anziché artista e l'ID di genere campi di testo.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **/StoreManager/Edit/1** per verificare che vengano visualizzati gli elenchi a discesa anziché artista e l'ID di genere campi di testo.

    ![Visualizzazione di modifica dell'Album con menu a discesa](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Modifica visualizzazione esplorazione Album con menu a discesa")

    *Visualizzazione di modifica dell'Album, questa volta con menu a discesa*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>Attività 6: implementazione del metodo di azione HTTP-POST modifica

Ora che la visualizzazione di modifica vengono visualizzate come previsto, è necessario implementare il metodo HTTP-POST Modifica azione per salvare le modifiche apportate all'Album.

1. Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio. Aprire **StoreManagerController** dal **controller** cartella.
2. Sostituire **HTTP-POST modifica** codice metodo di azione con la seguente (si noti che il metodo che deve essere sostituito versione di overload che riceve due parametri di):

    (- Frammento di codice *helper di ASP.NET MVC 4, moduli e convalida - Ex3 StoreManagerController HTTP-POST Modifica azione*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > Questo metodo verrà eseguito quando l'utente sceglie il **salvare** pulsante della vista ed esegue un POST HTTP dei valori di form al server per renderle persistenti nel database. L'elemento decorator **[HttpPost]** indica che il metodo deve essere utilizzato per gli scenari HTTP-POST. Il metodo accetta un **Album** oggetto. ASP.NET MVC crea automaticamente l'oggetto di Album da registrato il &lt;modulo&gt; valori.
    > 
    > Il metodo verrà eseguire questi passaggi:
    > 
    > 1. Se il modello è valido:
    > 
    >     1. Aggiornare la voce album nel contesto per contrassegnarlo come un oggetto modificato.
    >     2. Salvare le modifiche e reindirizzare la visualizzazione dell'indice.
    > 2. Se il modello non è valido, popola ViewBag con il **GenreId** e **ArtistId**, verrà restituito la vista con l'oggetto Album ricevuto per consentire all'utente di eseguire qualsiasi aggiornamento necessario.

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Attività 7: esecuzione dell'applicazione

In questa attività, si verificherà che il **StoreManager modifica** pagina Salva effettivamente i dati di Album aggiornati nel database.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **/StoreManager/Edit/1**. Modificare il titolo dell'Album in **carico** e fare clic su **salvare**. Verificare che il titolo dell'album effettivamente modificato nell'elenco degli album.

    ![L'aggiornamento di un album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "l'aggiornamento di un album")

    *L'aggiornamento di un Album*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>Esercizio 4: Aggiunta di una vista di creazione

Ora che il **StoreManagerController** supporta il **modifica** possibilità, in questo esercizio si apprenderà come aggiungere un modello Create View per consentire di memorizzare Manager aggiungere nuovo album all'applicazione.

Come fatto in precedenza con la funzionalità di modifica, implementare lo scenario Create utilizzando due metodi distinti all'interno di **StoreManagerController** classe:

1. Un metodo di azione verrà visualizzato un form vuoto quando l'archivio Gestioni prima visita il **StoreManager/crea** URL.
2. Un secondo metodo di azione gestirà lo scenario in cui il gestore del Negozio seleziona il **salvare** pulsante all'interno del form e invia nuovamente i valori al **StoreManager/crea** URL come un POST HTTP.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>Attività 1: implementazione del metodo di azione HTTP-GET creare

In questa attività verrà implementata la versione HTTP-GET del metodo di azione Create per recuperare un elenco di tutti i generi e artisti, creare pacchetti questi dati in un **StoreManagerViewModel** oggetto, che verrà quindi passato a un modello di visualizzazione.

1. Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex4-AddingACreateView/Begin/** cartella. In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.

    1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
    2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
    3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

    > [!NOTE]
    > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aprire **StoreManagerController** classe. A tale scopo, espandere il **controller** cartella e fare doppio clic su **StoreManagerController.cs**.
3. Sostituire il **crea** codice metodo di azione con quanto segue:

    (- Frammento di codice *convalida - Ex4 StoreManagerController HTTP-GET Create action e form e ASP.NET MVC 4 helper*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>Attività 2: aggiunta della visualizzazione di creazione

In questa attività si aggiungerà il modello Create View che verrà visualizzato un nuovo form Album (vuoto).

1. Fare doppio clic all'interno di **crea** metodo di azione e selezionare **Aggiungi visualizzazione**. Verrà visualizzata la finestra di dialogo Aggiungi visualizzazione.
2. Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della visualizzazione è **crea**. Selezionare il **creare una visualizzazione fortemente tipizzata** opzione e selezionare **Album (MvcMusicStore.Models)** dal **classe modello** elenco a discesa e **crea** dal **modello di scaffolding** elenco a discesa. Lasciare gli altri campi con il valore predefinito e quindi fare clic su **Aggiungi**.

    ![Aggiunta di una vista crea](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "aggiunta-a-Crea-view.png")

    *Aggiunta della visualizzazione di creazione*
3. Aggiornamento di **GenreId** e **ArtistId** campi da utilizzare un elenco a discesa come illustrato di seguito:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Attività 3: esecuzione dell'applicazione

In questa attività, si verificherà che il **StoreManager** **crea** pagina di visualizzazione consente di visualizzare un form Album vuoto.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **StoreManager/creare**. Verificare che un form vuoto viene visualizzato per la compilazione di nuove proprietà Album.

    ![Creare una vista con un form vuoto](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View con un form vuoto")

    *Creare una vista con un form vuoto*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>Metodo di azione di creazione di attività 4: implementazione di HTTP-POST

In questa attività verrà implementata la versione HTTP-POST del metodo di azione di creazione che verrà richiamato quando un utente fa clic il **salvare** pulsante. Il metodo deve salvare il nuovo album nel database.

1. Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio. Aprire **StoreManagerController** classe. A tale scopo, espandere il **controller** cartella e fare doppio clic su **StoreManagerController.cs**.
2. Sostituire **creare HTTP-POST** codice metodo di azione con quanto segue:

    (- Frammento di codice *convalida - Ex4 StoreManagerController HTTP - POST azione Create e form e ASP.NET MVC 4 helper*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > Azione di creazione è molto simile al metodo di azione di modifica precedente ma, anziché impostare l'oggetto come modificata, viene aggiunto al contesto.

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Attività 5: esecuzione dell'applicazione

In questa attività, si verificherà che il **StoreManager creare** pagina di visualizzazione consente di creare un nuovo Album e viene quindi reindirizzata alla visualizzazione StoreManager indice.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **StoreManager/creare**. Compilare tutti i campi modulo con i dati per un nuovo Album, simile a quello nella figura riportata di seguito:

    ![Creazione di un Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "la creazione di un Album")

    *Creazione di un Album*
3. Verificare che l'utente viene reindirizzato alla visualizzazione indice StoreManager che include il nuovo Album appena creato.

    ![Nuovo Album creato](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "Nuovo Album creato")

    *Nuovo Album creato*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>Esercizio 5: Gestisce l'eliminazione

La possibilità di eliminare gli album non è ancora implementata. Questo è ciò che questo esercizio su. Come prima, si verrà implementare lo scenario di eliminazione utilizzando due metodi distinti all'interno di **StoreManagerController** classe:

1. Un metodo di azione verrà visualizzato un modulo di conferma
2. Un secondo metodo di azione gestirà l'invio del modulo

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>Attività 1: implementazione del metodo di azione Delete HTTP-GET

In questa attività si implementerà la versione HTTP-GET del metodo di azione di eliminazione per recuperare le informazioni dell'album.

1. Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex5-HandlingDeletion/Begin/** cartella. In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.

    1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
    2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
    3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

    > [!NOTE]
    > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aprire **StoreManagerController** classe. A tale scopo, espandere il **controller** cartella e fare doppio clic su **StoreManagerController.cs**.
3. L'azione di eliminazione controller è esattamente come azione del controller dettagli archivio precedente: viene eseguita una query di **album** oggetto dal database utilizzando il **id** fornito l'URL e restituisce il appropriato **vista**. A tale scopo, sostituire HTTP-GET **eliminare** codice metodo di azione con quanto segue:

    (- Frammento di codice *helper di ASP.NET MVC 4, moduli e convalida - HTTP-GET di eliminazione gestione Ex5 Elimina azione*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. Fare doppio clic all'interno di **eliminare** metodo di azione e selezionare **Aggiungi visualizzazione**. Verrà visualizzata la finestra di dialogo Aggiungi visualizzazione.
5. Nella finestra di dialogo Aggiungi visualizzazione, verificare che il nome della visualizzazione è **eliminare**. Selezionare il **creare una visualizzazione fortemente tipizzata** opzione e selezionare **Album (MvcMusicStore.Models)** dal **classe modello** elenco a discesa. Selezionare **eliminare** dal **modello di scaffolding** elenco a discesa. Lasciare gli altri campi con il valore predefinito e quindi fare clic su **Aggiungi**.

    ![Aggiunta di una vista Elimina](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "aggiunta di una visualizzazione di eliminazione")

    *Aggiunta di una visualizzazione di eliminazione*
6. Il modello di eliminazione Mostra tutti i campi dal modello. Mostrare solo il titolo dell'album. A tale scopo, sostituire il contenuto della visualizzazione con il codice seguente:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Attività 2: esecuzione dell'applicazione

In questa attività, si verificherà che il **StoreManager** **eliminare** pagina di visualizzazione consente di visualizzare un modulo di conferma l'eliminazione.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **/StoreManager**. Selezionare un album da eliminare facendo **eliminare** e verificare che la nuova vista venga caricata.

    ![Eliminazione di un Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "eliminazione di un Album")

    *Eliminazione di un Album*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>Attività 3-implementazione del metodo di azione Delete HTTP-POST

In questa attività verrà implementata la versione HTTP-POST del metodo di azione Delete che verrà richiamato quando un utente fa clic il **eliminare** pulsante. Il metodo deve eliminare album nel database.

1. Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio. Aprire **StoreManagerController** classe. A tale scopo, espandere il **controller** cartella e fare doppio clic su **StoreManagerController.cs**.
2. Sostituire **HTTP-POST eliminare** codice metodo di azione con quanto segue:

    (- Frammento di codice *helper di ASP.NET MVC 4, moduli e convalida - HTTP-POST di eliminazione gestione Ex5 Elimina azione*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Attività 4: esecuzione dell'applicazione

In questa attività, si verificherà che il **StoreManager Delete** pagina di visualizzazione consente di eliminare un Album e viene quindi reindirizzata alla visualizzazione StoreManager indice.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **/StoreManager**. Selezionare un album da eliminare facendo **eliminare.** Confermare l'eliminazione, fare clic su **eliminare** pulsante:

    ![Eliminazione di un Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "eliminazione di un Album")

    *Eliminazione di un Album*
3. Verificare che album è stato eliminato poiché non viene visualizzato nel **indice** pagina.

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>Esercizio 6: Aggiunta della convalida

Attualmente, i moduli di creazione e modifica che sia non eseguono qualsiasi tipo di convalida. Se l'utente lascia un campo obbligatorio vuoto o lettere tipo nel campo prezzo, il primo errore che verrà visualizzato sarà dal database.

È possibile aggiungere la convalida per l'applicazione aggiungendo le annotazioni dei dati per la classe di modello. Le annotazioni dei dati consentono di descrivere le regole da applicare alle proprietà del modello e MVC ASP.NET si occuperà di applicazione e visualizzazione messaggio appropriato per gli utenti.

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>Attività 1: aggiunta di annotazioni dati

In questa attività si aggiungerà le annotazioni dei dati al modello che consentono la pagina di creazione e modifica Album visualizzare messaggi di convalida quando appropriato.

Per una classe di modello semplice, l'aggiunta di un'annotazione di dati viene gestita solo tramite l'aggiunta di un **utilizzando** istruzione per **System.ComponentModel.DataAnnotation**, quindi inserire un **[obbligatorio]**attributo le proprietà appropriate. Renderebbe l'esempio seguente il **nome** un campo obbligatorio nella visualizzazione di proprietà.

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

È leggermente più complesso in casi come questo tipo di applicazione in cui viene generato l'Entity Data Model. Se si aggiungono le annotazioni dei dati direttamente a classi del modello, essi verranno sovrascritte se si aggiorna il modello dal database. In alternativa, è possibile apportare le classi di utilizzo delle classi parziali di metadati che sarà disponibile per contenere le annotazioni e sono associati al modello utilizzando il **[MetadataType]** attributo.

1. Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex6-AddingValidation/Begin/** cartella. In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.

    1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
    2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
    3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

    > [!NOTE]
    > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Aprire il **Album.cs** dal **modelli** cartella.
3. Sostituire **Album.cs** contenuto con il codice evidenziato di seguito, in modo che risulti simile al seguente:

    > [!NOTE]
    > La riga **[DisplayFormat(ConvertEmptyStringToNull=false)]** indica che le stringhe vuote dal modello di non essere convertite in null quando viene aggiornato il campo dei dati nell'origine dati. Questa impostazione per evitare un'eccezione quando Entity Framework assegna i valori null per il modello prima che i campi di convalida l'annotazione dei dati.

    (- Frammento di codice *convalida - classe parziale di metadati Ex6 Album e form e ASP.NET MVC 4 helper*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > Questo **Album** classe parziale è un **MetadataType** attributo che fa riferimento al **AlbumMetaData** classe per le annotazioni dei dati. Questi sono alcuni degli attributi di annotazione dei dati in uso per annotare il modello di Album:
    > 
    > - Richiesto: indica che la proprietà è un campo obbligatorio
    > - DisplayName - definisce il testo da utilizzare per i campi del form e i messaggi di convalida
    > - DisplayFormat - specifica come vengono visualizzati e formattazione dei campi dati.
    > - StringLength - definisce una lunghezza massima per un campo stringa
    > - Intervallo - assegna un valore minimo e massimo per un campo numerico
    > - ScaffoldColumn - consente di nascondere i campi da moduli di editor

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Attività 2: esecuzione dell'applicazione

In questa attività si testerà convalidano che le pagine di creare e modificare i campi, utilizzando i nomi visualizzati scelti nell'ultima attività.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **StoreManager/creare**. Verificare che i nomi visualizzati corrispondano a quelli nella classe parziale (ad esempio **Album Art URL** anziché **AlbumArtUrl**)
3. Fare clic su **crea**, senza la compilazione del form. Verificare che i messaggi di convalida corrispondente.

    ![Convalidare i campi nella pagina Crea](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "convalidati i campi nella pagina Crea")

    *Campi convalidati nella pagina Crea*
4. È possibile verificare che lo stesso si verifica con la **modifica** pagina. Modificare l'URL in **/StoreManager/Edit/1** e verificare che i nomi visualizzati corrispondano a quelli nella classe parziale (ad esempio **Album Art URL** anziché **AlbumArtUrl**). Vuota il **titolo** e **prezzo** campi e fare clic su **salvare**. Verificare che i messaggi di convalida corrispondente.

    ![Convalidato i campi della pagina di modifica](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *Convalidato i campi della pagina di modifica*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>Esercizio 7: Utilizzo di jQuery non intrusiva sul lato Client

In questo esercizio, si apprenderà come abilitare la convalida jQuery non intrusiva di MVC 4 sul lato client.

> [!NOTE]
> JQuery non intrusiva utilizza prefisso dati ajax JavaScript per richiamare metodi di azione sul server anziché intrusivo creazione script client in linea.


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>Attività 1: esecuzione dell'applicazione prima di attivazione non intrusiva jQuery

In questa attività verrà eseguita l'applicazione prima di includere jQuery per confrontare i modelli di convalida.

1. Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex7-UnobtrusivejQueryValidation/Begin/** cartella. In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.

    1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
    2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
    3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

    > [!NOTE]
    > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Premere **F5** per eseguire l'applicazione.
3. Il progetto viene avviato nella Home page. Sfoglia **StoreManager/crea** e fare clic su **crea** senza compilare il modulo per verificare che i messaggi di convalida:

    ![Convalida del client disabilitata](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "la convalida del Client disabilitato")

    *Convalida del client disabilitata*
4. Nel browser, aprire il codice sorgente HTML:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>Attività 2: convalida del Client non intrusiva abilitazione

In questa attività si abiliterà jQuery **la convalida del client non intrusiva** da **Web. config** file, per impostazione predefinita impostato su false in tutti i nuovi progetti ASP.NET MVC 4. Inoltre, si aggiungeranno che necessari script i riferimenti per rendere jQuery lavoro convalida del Client non intrusiva.

1. Aprire **Web. config** file alla radice del progetto e verificare che il **ClientValidationEnabled** e **UnobtrusiveJavaScriptEnabled** valori di chiavi sono impostati su **true**.

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > È inoltre possibile abilitare la convalida del client dal codice in Global.asax.cs per ottenere gli stessi risultati:
    > 
    > **HtmlHelper.ClientValidationEnabled = true;**
    > 
    > Inoltre, è possibile assegnare l'attributo ClientValidationEnabled in qualsiasi controller di avere un comportamento personalizzato.
2. Aprire **Create.cshtml** in **Views\StoreManager**.
3. Assicurarsi che i seguenti file di script, **jquery.validate** e **jquery.validate.unobtrusive**, viene fatto riferimento in attraverso la visualizzazione di &quot; **~/bundles/jqueryval** &quot; bundle.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > Tutte queste librerie jQuery sono incluse nei nuovi progetti MVC 4. È possibile trovare altre librerie nel **/script** nella cartella del progetto è.
    > 
    > Per rendere questa convalida il funzionamento delle librerie, è necessario aggiungere un riferimento alla libreria di framework jQuery. Poiché questo riferimento è già stato aggiunto nel  **\_cshtml** file, sarà necessario aggiungerlo in questa vista specifica.

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>Attività 3: in esecuzione l'applicazione utilizzando discreto jQuery convalida

In questa attività, si verificherà che il **StoreManager** creare vista modello esegue la convalida lato client utilizzando le librerie di jQuery quando l'utente crea un nuovo album.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Sfoglia **StoreManager/crea** e fare clic su **crea** senza compilare il modulo per verificare che i messaggi di convalida:

    ![Convalida del client con jQuery abilitato](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "la convalida del Client con jQuery abilitato")

    *Convalida del client con jQuery abilitato*
3. Nel browser, aprire il codice sorgente per la visualizzazione di creazione:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

    > [!NOTE]
    > Per ogni regola di convalida del client, jQuery non intrusiva aggiunge un attributo con dati-val -*rulename*=&quot;*messaggio*&quot;. Di seguito è riportato un elenco di tag che Unobtrusive jQuery inserisce nel campo di input html per eseguire la convalida del client:
    > 
    > - Dati val
    > - Numero di dati val
    > - Intervallo di dati val
    > - Dati-val-intervallo-min / dati-val-intervallo-max
    > - Val dati obbligatori
    > - Val-lunghezza dei dati
    > - Dati-val lunghezza max / min dati-val-lunghezza
    > 
    > Tutti i valori di dati vengono riempiti con modello **annotazione dei dati**. Quindi, è possibile eseguire la logica che funziona sul lato server sul lato client. Ad esempio, prezzo e presenta l'annotazione dei dati seguenti nel modello:
    > 
    > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
    > 
    > Dopo aver utilizzato jQuery non intrusiva, il codice generato è:
    >  
    > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa pratica si è appreso come consentire agli utenti di modificare i dati archiviati nel database con l'uso delle operazioni seguenti:

- Le azioni del controller come indice, creazione, modifica, eliminazione
- Funzionalità di scaffolding di ASP.NET MVC per la visualizzazione delle proprietà in una tabella HTML
- Esperienza di helper HTML personalizzati per migliorare l'utente
- Metodi di azione che reagiscono a HTTP-GET o chiamate HTTP-POST
- Un modello condiviso editor di modelli di visualizzazione simili come creare e modificare
- Elementi form come elenchi a discesa
- Annotazioni dei dati per la convalida del modello
- Convalida lato client tramite jQuery non intrusiva libreria

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice a: installazione di Visual Studio Express 2012 per Web

È possibile installare **Microsoft Visual Studio Express 2012 per Web** o un altro &quot;Express&quot; versione utilizzando il  **[installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web di Microsoft*.

1. Passare a [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; *Visual Studio Express 2012 per Web con Windows Azure SDK*&quot;.
2. Fare clic su **installa**. Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.
3. Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.

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
7. Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per Web, passare al **avviare** schermata e iniziare la scrittura &quot; **VS Express**&quot;, quindi fare clic su di **Visual Studio Express per Web** il riquadro.

    ![VS Express per il riquadro Web](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *VS Express per il riquadro Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Appendice b: utilizzo dei frammenti di codice

Con i frammenti di codice, si dispone di tutto il codice che necessario a portata di mano. Il documento lab consentirà di determinare esattamente quando è possibile utilizzarli, come illustrato nella figura seguente.

![Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "frammenti di codice tramite Visual Studio per inserire codice nel progetto")

*Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto*

***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***

1. Posizionare il cursore dove si desidera inserire il codice.
2. Iniziare a digitare il nome del frammento di codice (senza spazi o segni meno).
3. Osservare come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).
5. Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.

![Iniziare a digitare il nome del frammento di codice](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "inizia a digitare il nome del frammento di codice")

*Iniziare a digitare il nome del frammento di codice*

![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "premere Tab per selezionare il frammento evidenziato")

*Premere Tab per selezionare il frammento di codice evidenziata*

![Premere Tab nuovamente e il frammento di codice verranno espansi](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "espanderà premere Tab nuovamente e il frammento di codice")

*Premere Tab nuovamente e il frammento di codice verranno espansi*

***Per aggiungere un frammento di codice utilizzando il mouse (c#, Visual Basic e XML)*** 1. Fare clic in cui si desidera inserire il frammento di codice.

1. Selezionare **Inserisci frammento di codice** seguito da **frammenti di codice**.
2. Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.

![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "rapida in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice")

*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice*

![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso")

*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso*
