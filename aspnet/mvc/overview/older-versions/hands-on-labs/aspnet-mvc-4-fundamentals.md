---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: Nozioni fondamentali su ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Questa pratica è basata su MVC (Model View Controller) Music Store, un'applicazione di esercitazione che presenta e illustra come usare ASP.NET MV dettagliate...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 3a282d02ba929eb86571e92f190550614962524d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386575"
---
# <a name="aspnet-mvc-4-fundamentals"></a>Nozioni fondamentali su ASP.NET MVC 4

da [Camp Web Team](https://twitter.com/webcamps)

[Download Web Camp Kit di formazione](https://aka.ms/webcamps-training-kit)

Questa pratica si basa su MVC (Model View Controller) Music Store, un'applicazione di esercitazione che introduce e dettagliata spiega come usare ASP.NET MVC e Visual Studio. Nell'intero laboratorio si apprenderà la semplicità, ancora alimentazione dell'utilizzo congiunto di queste tecnologie. Si inizierà con una semplice applicazione e si farà finché non possiedi un completamente funzionale applicazione Web ASP.NET MVC 4.

Questo Lab è compatibile con ASP.NET MVC 4.

Se si vuole esplorare la versione di ASP.NET MVC 3 dell'applicazione dell'esercitazione, è possibile trovarlo nel [MVC-musica-Store](https://github.com/evilDave/MVC-Music-Store).

In questo laboratorio pratico presuppone che lo sviluppatore abbia esperienza nelle tecnologie di sviluppo Web, ad esempio HTML e JavaScript.

> [!NOTE]
> Tutto il codice di esempio e frammenti di codice sono inclusi nel Web Camp Kit di formazione, disponibile all'indirizzo [Microsoft-Web/WebCampTrainingKit versioni](https://aka.ms/webcamps-training-kit). Il progetto specifico per questo lab è disponibile all'indirizzo [nozioni di base di ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>L'applicazione Music Store

L'applicazione web di Music Store che sarà compilato in tutto questo Lab è costituito da tre parti principali: market, estrazione e l'amministrazione. I visitatori sarà in grado di esplorare gli album in base al genere, aggiungere gli album al carrello acquisti, esaminare la selezione e infine procedono alla cassa per eseguire l'accesso e completare l'ordine. Inoltre, gli amministratori di store saranno in grado di gestire gli album disponibili, nonché le relative proprietà principali.

![Le schermate di Music Store](aspnet-mvc-4-fundamentals/_static/image1.png "screening di Music Store")

*Schermate di Music Store*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>Essentials di ASP.NET MVC 4

Applicazione Music Store verrà compilata usando **Model View Controller (MVC)**, uno schema architetturale che separa un'applicazione in tre componenti principali:

- **I modelli**: gli oggetti modello sono le parti dell'applicazione che implementano la logica di dominio. Spesso, anche gli oggetti modello recupero e archiviano lo stato del modello in un database.
- **Visualizzazioni:** viste sono elencati i componenti che consentono di visualizzare l'interfaccia utente (UI). In genere, questa interfaccia utente viene creata da dati del modello. Un esempio sarebbe visualizzazione di modifica che consente di visualizzare le caselle di testo e un elenco di riepilogo a discesa basato sullo stato corrente di un oggetto di Album di album.
- **I controller:** controller sono i componenti che gestiscono l'interazione dell'utente, modificano il modello e selezionano infine una visualizzazione per eseguire il rendering dell'interfaccia utente. In un'applicazione MVC la visualizzazione presenta solo le informazioni, mentre il controller gestisce e risponde all'input e all'interazione dell'utente.

Il pattern MVC consente di creare applicazioni che separano i diversi aspetti dell'applicazione (logica di input, logica di business e logica dell'interfaccia utente), fornendo un accoppiamento libero tra questi elementi. Questa separazione consente di gestire la complessità quando si compila un'applicazione, in quanto consente di concentrarsi su un aspetto dell'implementazione alla volta. Inoltre, il modello MVC rende semplice testare le applicazioni, incoraggiando anche l'utilizzo dello sviluppo basato su test (TDD) per la creazione di applicazioni.

Il **ASP.NET MVC** framework offre un'alternativa al modello Web Form ASP.NET per la creazione di applicazioni Web basate su ASP.NET MVC. Il **ASP.NET MVC** è un framework di presentazione leggero e facile da testare che (come con le applicazioni basate su form Web) integrato con funzionalità ASP.NET esistenti, ad esempio le pagine master e basata sull'appartenenza autenticazione in modo che si ottiene tutta la potenza di .NET core. Ciò è utile se si ha già familiarità con Web Form ASP.NET perché tutte le librerie già in uso sono anche disponibili in ASP.NET MVC 4.

Inoltre, l'accoppiamento libero tra i tre componenti principali di un'applicazione MVC favorisce anche lo sviluppo parallelo. Ad esempio, uno sviluppatore può lavorare sulla visualizzazione, uno sviluppatore secondo gli strumenti per la logica del controller e un terzo può concentrarsi sulla logica di business nel modello.

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico, si apprenderà come:

- Creare un'applicazione ASP.NET MVC da zero basata su Esercitazione l'applicazione Music Store
- Aggiungere controller per gestire gli URL per la Home page del sito e per esplorare le funzionalità principali
- Aggiungere una visualizzazione per personalizzare il contenuto visualizzato insieme ai relativi stile
- Aggiungere le classi di modello per contenere e la gestione dei dati e dominio per la logica
- Utilizzare il modello di vista per passare informazioni dalle azioni del controller per i modelli di vista
- Esplorare il nuovo modello di ASP.NET MVC 4 per applicazioni internet

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Sono necessari gli elementi seguenti per completare questa esercitazione:

- [Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (leggere [appendice A](#AppendixA) per istruzioni su come installarla)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

**Installazione di frammenti di codice**

Per praticità, gran parte del codice che vengono gestiti insieme questo lab è disponibile come frammenti di codice di Visual Studio. Per installare i frammenti di codice eseguiti **.\Source\Setup\CodeSnippets.vsi** file.

Se non ha familiarità con i frammenti di codice di Visual Studio e si vuole imparare a usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice c: Using Code Snippets](#AppendixC)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

In questo laboratorio pratico include gli esercizi seguenti:

1. [Esercizio 1: Creazione progetto di applicazione Web ASP.NET MVC MusicStore](#Exercise1)
2. [Esercizio 2: Creazione di un Controller](#Exercise2)
3. [Esercizio 3: Il passaggio di parametri a un Controller](#Exercise3)
4. [Esercizio 4: Creazione di una vista](#Exercise4)
5. [Esercizio 5: Creazione di un modello di visualizzazione](#Exercise5)
6. [Esercizio 6: Utilizzo di parametri nella visualizzazione](#Exercise6)
7. [Esercizio 7: Una visione generale di nuovo modello di ASP.NET MVC 4](#Exercise7)

> [!NOTE]
> Ogni esercizio è accompagnato da un **End** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato gli esercizi. Se ti serve assistenza aggiuntiva esaminando gli esercizi, è possibile usare questa soluzione come guida.


Tempo stimato per completare questa esercitazione: **60 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>Esercizio 1: Creazione progetto di applicazione Web ASP.NET MVC MusicStore

In questo esercizio, si apprenderà come creare un'applicazione ASP.NET MVC in Visual Studio Express 2012 per Web nonché relativa organizzazione della cartella principale. Inoltre, si apprenderà come aggiungere un nuovo Controller e renderla di visualizzare una semplice stringa nella home page dell'applicazione.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>Attività 1: creazione del progetto di applicazione Web ASP.NET MVC

1. In questa attività si creerà un progetto di applicazione MVC ASP.NET vuoto usando il modello MVC Visual Studio. Avviare **Visual Studio Express per Web**.
2. Scegliere **Nuovo progetto** dal menu **File**.
3. Nel **nuovo progetto** finestra di dialogo per selezionare il **applicazione Web ASP.NET MVC 4** tipo, che si trova nel progetto **Visual c#** **Web** modello elenco.
4. Modifica il **Name** al *MvcMusicStore*.
5. Impostare il percorso della soluzione all'interno di una nuova **Begin** nella cartella di origine di questo esercizio, ad esempio **\Source\Ex01-CreatingMusicStoreProject\Begin [YOUR-PASQUA-PATH]**. Fare clic su **OK**.

    ![Creare una finestra di dialogo Nuovo progetto](aspnet-mvc-4-fundamentals/_static/image2.png "Crea finestra di dialogo Nuovo progetto")

    *Creare una finestra di dialogo Nuovo progetto*
6. Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo per selezionare la **base** modello e assicurarsi che il **motore di visualizzazione** selezionata sia **Razor**. Fare clic su **OK**.

    ![Finestra di dialogo Nuovo progetto ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "finestra di dialogo Nuovo progetto ASP.NET MVC 4")

    *Finestra di dialogo Nuovo progetto ASP.NET MVC 4*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>Attività 2: esplorare la struttura della soluzione

Il framework ASP.NET MVC include un modello di progetto di Visual Studio che consente di creare applicazioni Web che supportano il modello MVC. Questo modello crea una nuova applicazione Web MVC ASP.NET con le cartelle necessarie, modelli di elementi e le voci di file di configurazione.

In questa attività, si esaminerà la struttura della soluzione per comprendere gli elementi che sono coinvolti e le relative relazioni. Le cartelle seguenti sono inclusi in tutto l'applicazione ASP.NET MVC perché il framework ASP.NET MVC per impostazione predefinita Usa una &quot;convenzione rispetto alle configurazioni&quot; approccio e la imposta alcuni presupposti predefiniti in base alla denominazione di cartella convenzioni.

1. Una volta creato il progetto, esaminare la struttura di cartelle che è stata creata in Esplora soluzioni sul lato destro:

    ![Struttura di cartelle MVC ASP.NET in Esplora soluzioni](aspnet-mvc-4-fundamentals/_static/image4.png "struttura di cartelle MVC ASP.NET in Esplora soluzioni")

    *Struttura di cartelle MVC ASP.NET in Esplora soluzioni*

   1. **I controller**. Questa cartella contiene le classi controller. In un'applicazione MVC di base, i controller sono tenuti a gestisce l'interazione dell'utente finale, la modifica del modello e infine scegliendo una vista per eseguire il rendering dell'interfaccia utente.

       > [!NOTE]
       > Il framework MVC richiede i nomi di tutti i controller terminino con &quot;Controller&quot;-ad esempio HomeController, LoginController o ProductController.
   2. **I modelli**. Questa cartella è disponibile per le classi che rappresentano il modello di applicazione per l'applicazione Web MVC. Ciò in genere include codice che definisce gli oggetti e la logica per l'interazione con l'archivio dati. In genere, gli oggetti modello effettivo sarà nelle librerie di classi separata. Tuttavia, quando si crea una nuova applicazione, si includono le classi e quindi spostarle in librerie di classi separate in un secondo momento nel ciclo di sviluppo.
   3. **Viste**. Questa cartella è il percorso consigliato per viste, i componenti responsabile della visualizzazione dell'interfaccia utente dell'applicazione. Le visualizzazioni utilizzano i file con estensione aspx, ascx, cshtml e. master, oltre a eventuali altri file correlati al rendering delle visualizzazioni. Cartella Views contiene una cartella per ogni controller. la cartella è denominata con il prefisso nome del controller. Ad esempio, se si dispone di un controller denominato **HomeController**, la cartella Views conterrà una cartella Home. Per impostazione predefinita, quando il framework di MVC ASP.NET carica una visualizzazione, Cerca un file con estensione aspx con il nome della visualizzazione richiesta nella cartella Views\controllerName (**viste [ControllerName] [Action]. aspx**) o (**viste [ControllerName] [Action]. cshtml**) per le visualizzazioni Razor.

      > [!NOTE]
      > Oltre alle cartelle elencate in precedenza, un'applicazione Web MVC utilizza il **Global. asax** per impostare il routing degli URL globale per impostazione predefinita e Usa le **Web. config** file per configurare l'applicazione.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>Attività 3: aggiunta di una classe HomeController

Nelle applicazioni ASP.NET che non usano il framework di MVC l'interazione dell'utente è organizzata intorno alle pagine e intorno alla generazione e gestione di eventi da tali pagine. Al contrario, l'interazione dell'utente con le applicazioni MVC ASP.NET è organizzata intorno a controller e i relativi metodi di azione.

D'altra parte, framework di MVC ASP.NET esegue il mapping degli URL alle classi che vengono definite i controller. I controller di elaborare le richieste in ingresso, gestiscono input dell'utente e le interazioni, eseguire la logica dell'applicazione appropriati e determinare la risposta da inviare al client (visualizzazione del contenuto HTML, scaricare un file, il reindirizzamento a un URL diverso e così via). Nel caso di visualizzazione HTML, una classe controller chiama in genere un componente di visualizzazione separato per generare il markup HTML per la richiesta. In un'applicazione MVC la visualizzazione presenta solo le informazioni, mentre il controller gestisce e risponde all'input e all'interazione dell'utente.

In questa attività si aggiungerà una classe Controller che gestirà l'URL alla Home page del sito Music Store.

1. Fare doppio clic su **controller** cartella all'interno di Esplora soluzioni, scegliere **Add** e quindi **Controller** comando:

    ![Aggiungere un comando del Controller](aspnet-mvc-4-fundamentals/_static/image5.png "aggiungere un comando del Controller")

    *Aggiungi comando Controller*
2. Il **Aggiungi Controller** viene visualizzata la finestra. Denominare il controller *HomeController* , quindi premere **Add**.

    ![Aggiungi finestra di dialogo Controller](aspnet-mvc-4-fundamentals/_static/image6.png "Controller finestra di dialogo Aggiungi")

    *Aggiungi finestra di dialogo Controller*
3. Il file **HomeController.cs** viene creato nel **controller** cartella. Per avere la **HomeController** restituiscono una stringa nella relativa operazione sull'indice, sostituire il **indice** metodo con il codice seguente:

    (Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - indice di HomeController Ex1*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Attività 4: esecuzione dell'applicazione

In questa attività proverà orizzontalmente l'applicazione in un web browser.

1. Premere **F5** per eseguire l'applicazione. La compilazione del progetto e avvia Server Web IIS locale. Locale Server Web IIS verranno automaticamente aprire un web browser puntando all'URL del server Web.

    ![Applicazione in esecuzione in un web browser](aspnet-mvc-4-fundamentals/_static/image7.png "applicazione in esecuzione in un web browser")

    *Applicazione in esecuzione in un web browser*

    > [!NOTE]
    > Il Server Web IIS locale verrà eseguito il sito Web in un numero di porta disponibile casuale. Nella figura precedente, il sito viene eseguito in `http://localhost:50103/`, pertanto utilizza porta 50103. Il numero di porta può variare.
2. Chiudere il browser.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>Esercizio 2: Creazione di un Controller

In questo esercizio, si apprenderà come aggiornare il controller per implementare la funzionalità semplice dell'applicazione Music Store. Tale controller definirà i metodi di azione per gestire ognuna delle richieste specifiche seguenti:

- Una pagina di presentazione di generi musica in di Music Store
- Una pagina di esplorazione che elenca tutti i album musicali per un particolare genere
- Una pagina che contiene informazioni su un album musicali specifici

Per l'ambito di questo esercizio, tali azioni restituirà semplicemente una stringa a questo punto.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>Attività 1: aggiunta di una nuova classe StoreController

In questa attività si aggiungerà un nuovo Controller.

1. Se non è già aperta, avviare **Visual Studio Express per Web 2012**.
2. Nel **File** menu, scegliere **aprire il progetto**. Nella finestra di dialogo Apri progetto individuare **Source\Ex02 CreatingAController\Begin**, selezionare **Begin.sln** e fare clic su **Open**. In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
3. Aggiungere il nuovo controller. A tale scopo, fare doppio clic sui **controller** cartella all'interno di Esplora soluzioni, scegliere **Add** e quindi il **Controller** comando. Modifica il **nome del Controller** a *StoreController*, fare clic su **Add**.

    ![Aggiungi finestra di dialogo Controller](aspnet-mvc-4-fundamentals/_static/image8.png "Controller finestra di dialogo Aggiungi")

    *Aggiungi finestra di dialogo Controller*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>Attività 2 - modificare le azioni del StoreController

In questa attività si modificherà i metodi del Controller che vengono chiamati **azioni**. Le azioni sono responsabili per la gestione delle richieste di URL e determinare il contenuto che deve essere inviato nuovamente al browser o all'utente che ha richiamato l'URL.

1. Il **StoreController** classe esiste già un' **indice** (metodo). Si utilizzerà più avanti in questo laboratorio per implementare la pagina che elenca tutti i generi dell'archivio di file musicali. Per il momento, è sufficiente sostituire il **indice** con il codice seguente che restituisce una stringa &quot;Hello from Store.Index()&quot;:

    (Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - indice StoreController Ex2*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. Aggiungere **esplorare** e **dettagli** metodi. A tale scopo, aggiungere il codice seguente per il **StoreController**:

    (Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Attività 3: esecuzione dell'applicazione

In questa attività proverà orizzontalmente l'applicazione in un web browser.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nel **Home** pagina. Modificare l'URL per verificare l'implementazione di ogni azione.

    1. **/ Store**. Si noterà  **&quot;Hello from Store.Index()&quot;**.
    2. **/ Store/esplorazione**. Si noterà  **&quot;Hello from Store.Browse()&quot;**.
    3. **/ Store/Details**. Si noterà  **&quot;Hello from Store.Details()&quot;**.

        ![Esplorazione StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "esplorazione StoreBrowse")

        *Esplorazione /Store/Browse*
3. Chiudere il browser.

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>Esercizio 3: Il passaggio di parametri a un Controller

Fino ad ora, si hanno restituiva le stringhe costanti verso i controller. In questa esercitazione si apprenderà come passare i parametri a un Controller usando l'URL e la stringa di query e apportando quindi le azioni di metodo rispondere con il testo nel browser.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>Attività 1: aggiunta parametro Genre StoreController

In questa attività si userà il **querystring** inviare parametri per il **Sfoglia** metodo di azione il **StoreController**.

1. Se non è già aperta, avviare **Visual Studio Express per Web**.
2. Nel **File** menu, scegliere **aprire il progetto**. Nella finestra di dialogo Apri progetto individuare **Source\Ex03 PassingParametersToAController\Begin**, selezionare **Begin.sln** e fare clic su **Open**. In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
3. Aprire **StoreController** classe. A questo scopo, nella **Esplora soluzioni**, espandere il **controller** cartella e fare doppio clic su **StoreController.cs**.
4. Modifica il **esplorare** metodo, aggiunta di un parametro di stringa per richiedere per un genere specifico. ASP.NET MVC verranno automaticamente passare qualsiasi stringa di query o parametri denominati invio del form **genre** a questo metodo di azione quando viene richiamato. A tale scopo, sostituire il **esplorare** metodo con il codice seguente:

    (Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> Si usa la **HttpUtility** metodo di utilità per impedisce agli utenti di inserimento di Javascript in vista con un collegamento, ad esempio   **/Store/Sfoglia? Genre =&lt;script&gt;Window. Location = '[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.
> 
> Per altre informazioni, visita [questo articolo di msdn](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Attività 2: esecuzione dell'applicazione

In questa attività, provare l'applicazione in un web browser e verrà utilizzato il **genre** parametro.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nel **Home** pagina. Modificare l'URL in   */Store/Sfoglia? Genre = Disco* per verificare che l'azione riceve il parametro genre.

    ![Esplorazione StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "esplorazione StoreBrowseGenre = Disco")

    *Esplorazione /Store/Browse? Genre = Disco*
3. Chiudere il browser.

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>Attività 3: aggiunta di un parametro Id incorporato nell'URL

In questa attività si userà il **URL** per passare un **Id** parametro per il **dettagli** il metodo di azione del **StoreController**. Impostazione predefinita di ASP.NET MVC convenzione di routing consiste nel considerare il segmento di URL dopo il nome del metodo di azione come un parametro denominato **Id**. Se il metodo di azione presenta un parametro denominato Id, quindi ASP.NET MVC automaticamente passerà il segmento URL all'utente come parametro. Nell'URL **Store/dettagli/5**, **Id** viene interpretato come **5**.

1. Modifica il **dettagli** metodo per il **StoreController**, l'aggiunta un' **int** parametro denominato **id**. A tale scopo, sostituire il **dettagli** metodo con il codice seguente:

    (Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Attività 4: esecuzione dell'applicazione

In questa attività, provare l'applicazione in un web browser e verrà utilizzato il **Id** parametro.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nel **Home** pagina. Modificare l'URL */Store/Details/5* per verificare che l'azione riceve il parametro id.

    ![Esplorazione StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "esplorazione StoreDetails5")

    *Esplorazione /Store/Details/5*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>Esercizio 4: Creazione di una vista

Finora si hanno restituiva le stringhe dalle azioni del controller. Sebbene questo sia un modo utile per comprendere come funzionano i controller, non è modo in cui le applicazioni Web reali vengono compilate. Le visualizzazioni sono componenti che forniscono un approccio migliore per la generazione di codice HTML al browser con l'uso di file di modello.

In questa esercitazione si apprenderà come aggiungere una pagina master layout per la configurazione di un modello per il contenuto HTML comune, un foglio di stile per migliorare l'aspetto del sito e, infine, un modello di vista per abilitare HomeController restituire HTML.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a>Attività 1: modifica del file \_layout. cshtml

Il file **~/Views/Shared/\_layout. cshtml** consente di configurare un modello per il codice HTML comune da usare tra l'intero sito Web. In questa attività si aggiungerà una pagina master layout con un'intestazione comune con i collegamenti all'area di Store e la Home page.

1. Se non è già aperta, avviare **Visual Studio Express per Web**.
2. Nel **File** menu, scegliere **aprire il progetto**. Nella finestra di dialogo Apri progetto individuare **Source\Ex04 CreatingAView\Begin**, selezionare **Begin.sln** e fare clic su **Open**. In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
3. Il file  <strong>\_layout. cshtml</strong> contiene il layout del contenitore HTML per tutte le pagine nel sito. Include il <strong>&lt;html&gt;</strong> (elemento) per la risposta HTML, così come il <strong>&lt;head&gt;</strong> e <strong>&lt;corpo&gt;</strong> elementi. <strong>@RenderBody()</strong> all'interno dell'HTML body identificare le aree di tale vista modelli saranno in grado di compilare con contenuto dinamico.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. Aggiungere un'intestazione comune con i collegamenti nell'area di Store e la Home page in tutte le pagine nel sito. Per farlo, aggiungere il codice seguente sotto &lt;corpo&gt; istruzione.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. Includere un elemento div per eseguire il rendering di sezione del corpo di ogni pagina. Sostituire  <strong>@RenderBody()</strong> con il seguente codice higlighted: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > Lo sapevi? Visual Studio 2012 include frammenti di codice che rendono più semplice aggiungere codice di uso comune in HTML, file di codice e altro ancora! Provarlo digitando **&lt;div&gt;** e premendo **scheda** due volte per inserire un completo **div** tag.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>Attività 2 - aggiunta foglio di stile CSS

Il modello di progetto vuoto include un file CSS molto semplice che include solo gli stili utilizzati per visualizzare forme di base e i messaggi di convalida. Si userà per migliorare l'aspetto del sito aggiuntivi CSS e immagini (potenzialmente fornite da una finestra di progettazione).

In questa attività si aggiungerà un foglio di stile CSS per definire gli stili del sito.

1. I file CSS e immagini sono inclusi nel **Source\Assets\Content** cartella della presente esercitazione. Per aggiungerle all'applicazione, trascinare i propri contenuti da un **Windows Explorer** finestra all'interno di **Esplora soluzioni** in Visual Studio Express per Web, come illustrato di seguito:

    ![Trascinamento di contenuto di stile](aspnet-mvc-4-fundamentals/_static/image12.png "il trascinamento di contenuto stile")

    *Trascinamento di contenuto stile*
2. Verrà visualizzata una finestra di dialogo di avviso, in cui viene chiesto di confermare se sostituire **CSS** file e alcune immagini esistente. Controllare **applica a tutti gli elementi** e fare clic su **Yes**.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>Attività 3: aggiunta di un modello di vista

In questa attività si aggiungerà un modello di vista per generare la risposta HTML che utilizzerà la pagina master layout e CSS aggiunti in questo esercizio.

1. Per usare un modello di vista durante l'esplorazione delle home page del sito, è necessario innanzitutto indicare che invece di restituire una stringa, il **indice HomeController** metodo restituirà una **visualizzazione**. Aprire **HomeController** classe e modificare relativo **indice** metodo restituisca un' **ActionResult**, e restituisse **View()**.

    (Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - indice di HomeController Ex4*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. A questo punto, è necessario aggiungere un modello di visualizzazione appropriato. A tale scopo **fare doppio clic su** all'interno di **indice** metodo di azione e selezionare **Aggiungi visualizzazione**. Verrà visualizzata la **Aggiungi visualizzazione** finestra di dialogo.

    ![Aggiunta di una vista all'interno del metodo di indice](aspnet-mvc-4-fundamentals/_static/image13.png "aggiunta di una vista all'interno del metodo Index")

    *Aggiunta di una vista all'interno del metodo Index*
3. Il **Aggiungi visualizzazione** verrà visualizzata finestra di dialogo per generare un file di modello di visualizzazione. Per impostazione predefinita, questa finestra di dialogo popola preventivamente il nome del modello di visualizzazione in modo che corrisponda al metodo di azione che verrà usato. Poiché è stato utilizzato il **Aggiungi visualizzazione** menu di scelta rapida all'interno del **indice** metodo di azione all'interno di HomeController, il **Aggiungi visualizzazione** dialogo ha indice come il nome della visualizzazione predefinita. Fare clic su **Aggiungi**.

    ![Aggiungi finestra di dialogo di visualizzazione](aspnet-mvc-4-fundamentals/_static/image14.png "Visualizza finestra di dialogo Aggiungi")

    *Visualizza finestra di dialogo Aggiungi*
4. Visual Studio genera una **index. cshtml** modello di visualizzazione all'interno di **Views\Home** cartella e quindi lo apre.

    ![Visualizzazione dell'indice creato-Home](aspnet-mvc-4-fundamentals/_static/image15.png "visualizzazione pagina iniziale dell'indice creato")

    *Visualizzazione dell'indice iniziale creato*

    > [!NOTE]
    > nome e il percorso dei **index. cshtml** file pertinente e segue le convenzioni di denominazione di ASP.NET MVC predefinita.
    > 
    > La cartella \Views\**casa** corrisponde al nome del controller (**Home** Controller). Il nome del modello di visualizzazione (**indice**), corrisponde al metodo di azione del controller che prevede di visualizzare la vista.
    > 
    > In questo modo, ASP.NET MVC evita di dover specificare esplicitamente il nome o il percorso di un modello di visualizzazione quando si usa questa convenzione di denominazione per restituire una visualizzazione.
5. Il modello di vista generato si basa sul  **\_layout. cshtml** modello definito in precedenza. Aggiornare la proprietà ViewBag **casa**e modificare il contenuto principale per **questa è la Home Page**, come illustrato nel codice seguente:

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. Selezionare **MvcMusicStore** progetto in Esplora soluzioni e premere **F5** per eseguire l'applicazione.

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>Attività 4: verifica

Per verificare che siano state completate correttamente tutti i passaggi nell'esercizio precedente, procedere come segue:

Con l'applicazione in un browser, si noti che:

1. Metodo di azione Index del HomeController trovare e visualizzare il **\Views\Home\Index.cshtml** visualizzare modelli, anche se il codice chiamato **restituire View()**, poiché il modello di vista seguito il convenzione di denominazione standard.
2. La Home Page viene visualizzato il messaggio di benvenuto definito all'interno di **\Views\Home\Index.cshtml** modello di visualizzazione.
3. Home Page è tramite il  **\_layout. cshtml** modello, e in modo che il messaggio di benvenuto è contenuto all'interno del layout del sito standard HTML.

    ![Visualizzazione dell'indice utilizzando il LayoutPage e lo stile definito-Home](aspnet-mvc-4-fundamentals/_static/image16.png "Home visualizzazione dell'indice utilizzando il LayoutPage e lo stile definito")

    *Visualizzazione dell'indice iniziale usando il LayoutPage e lo stile definito*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>Esercizio 5: Creazione di un modello di visualizzazione

Finora, apportate alle visualizzazioni visualizzare HTML hardcoded, ma, per poter creare applicazioni web dinamiche, il modello di vista deve ricevere informazioni dal Controller. Una tecnica comune da utilizzare a tale scopo è il **ViewModel** modello, che consente a un Controller per creare un pacchetto di tutte le informazioni necessarie per generare la risposta appropriata in HTML.

In questo esercizio, si apprenderà come creare una classe ViewModel e aggiungere le proprietà obbligatorie: il numero di generi nell'archivio e un elenco di tali generi. Anche se si aggiornerà il StoreController per l'uso di ViewModel creati e infine, si creerà un nuovo modello di visualizzazione che verrà visualizzate le proprietà indicate nella pagina.

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>Attività 1: creazione di una classe ViewModel

In questa attività si creerà una classe di ViewModel che consente di implementare lo scenario di inserzione genre Store.

1. Se non è già aperta, avviare **Visual Studio Express per Web**.
2. Nel **File** menu, scegliere **aprire il progetto**. Nella finestra di dialogo Apri progetto individuare **Source\Ex05 CreatingAViewModel\Begin**, selezionare **Begin.sln** e fare clic su **Open**. In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
3. Creare un **ViewModel** cartella per contenere l'elemento ViewModel. A tale scopo, fare doppio clic su di primo livello **MvcMusicStore** progetto, selezionare **Add** e quindi **nuova cartella**.

    ![Aggiunta di una nuova cartella](aspnet-mvc-4-fundamentals/_static/image17.png "aggiunta una nuova cartella")

    *Aggiunta di una nuova cartella*
4. Denominare la cartella *ViewModel*.

    ![Cartella ViewModel in Esplora soluzioni](aspnet-mvc-4-fundamentals/_static/image18.png "cartella ViewModel in Esplora soluzioni")

    *Cartella ViewModel in Esplora soluzioni*
5. Creare un **ViewModel** classe. A tale scopo, fare clic sui **ViewModel** cartella creato di recente, selezionare **Add** e quindi **nuovo elemento**. Sotto **codice**, scegliere il **classe** item e denominare il file *StoreIndexViewModel.cs*, quindi fare clic su **Aggiungi**.

    ![Aggiunta di una nuova classe](aspnet-mvc-4-fundamentals/_static/image19.png "aggiunta una nuova classe")

    *Aggiunta di una nuova classe*

    ![Creazione classe StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel Crea classe")

    *Creazione classe StoreIndexViewModel*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>Attività 2: aggiunta di proprietà alla classe ViewModel

Esistono due parametri deve essere passato dal StoreController il modello di vista per generare la risposta HTML previsto: il numero di generi nell'archivio e un elenco di tali generi.

In questa attività si aggiungerà le 2 proprietà per il **StoreIndexViewModel** classe: **NumberOfGenres** (integer) e **generi** (un elenco di stringhe).

1. Aggiungere **NumberOfGenres** e **generi** delle proprietà per il **StoreIndexViewModel** classe. A tale scopo, aggiungere le 2 righe seguente alla definizione della classe:

    (Code - Snippet *ASP.NET MVC 4 Fundamentals - proprietà StoreIndexViewModel Ex5*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> Il **{get; set;}**  notazione si avvale di # funzionalità di proprietà implementate automaticamente. Che offre i vantaggi di una proprietà senza la necessità di dichiarare un campo sottostante.

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>Attività 3: aggiornamento StoreController usare il StoreIndexViewModel

Il **StoreIndexViewModel** classe incapsula le informazioni necessarie per passare dal **StoreController**del **indice** metodo da un modello di vista per generare una risposta .

In questa attività si aggiornerà il **StoreController** da utilizzare il **StoreIndexViewModel**.

1. Aprire **StoreController** classe.

    ![Apertura di classe StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "classe StoreController apertura")

    *Classe StoreController apertura*
2. Per usare la **StoreIndexViewModel** classe la **StoreController**, aggiungere lo spazio dei nomi seguente all'inizio del **StoreController** codice:

    (Code - Snippet *ASP.NET MVC 4 Fundamentals - StoreIndexViewModel Ex5 usando ViewModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. Modifica il **StoreController**del **indice** metodo di azione in modo che si crea e popola una **StoreIndexViewModel** dell'oggetto e quindi li passa a un modello di vista generare una risposta HTML con esso.

    > [!NOTE]
    > Nel laboratorio &quot;l'accesso ai dati e modelli di MVC ASP.NET&quot; si scriverà il codice che recupera l'elenco dei generi store da un database. Nel codice seguente, si creerà una **elenco** di generi dati fittizi in grado di popolare il **StoreIndexViewModel**.
    > 
    > Dopo la creazione e configurazione del **StoreIndexViewModel** dell'oggetto, verrà passato come argomento per il **visualizzazione** (metodo). Ciò indica che il modello di vista utilizzerà tale oggetto per generare una risposta HTML con esso.
4. Sostituire il **indice** metodo con il codice seguente:

    (Code - Snippet *Fundamentals di ASP.NET MVC 4 - metodo Ex5 StoreController Index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> Se non si ha familiarità con c#, si può presupporre che l'utilizzo **var** significa che le **viewModel** variabile è l'associazione tardiva. Non è corretto, il compilatore c# Usa basato su ciò che si assegna alla variabile di inferenza del tipo per determinare che **viewModel** JE typu **StoreIndexViewModel**. Inoltre, per la compilazione locale **viewModel** variabile come una **StoreIndexViewModel** tipo di controllo della fase di compilazione di get e il supporto di editor di codice di Visual Studio.

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>Attività 4: creazione di un modello di visualizzazione che utilizza StoreIndexViewModel

In questa attività si creerà un modello di visualizzazione che verrà utilizzato un oggetto StoreIndexViewModel passato dal Controller per visualizzare un elenco dei generi.

1. Prima di creare il nuovo modello di vista, è possibile compilare il progetto in modo che il **finestra di dialogo Aggiungi visualizzazione** conosce le **StoreIndexViewModel** classe. Compilare il progetto selezionando il **compilare** voce di menu e quindi **compilazione MvcMusicStore**.

    ![Compilazione del progetto](aspnet-mvc-4-fundamentals/_static/image22.png "compilando il progetto")

    *Compilazione del progetto*
2. Creare un nuovo modello di visualizzazione. A tale scopo, fare doppio clic all'interno di **indice** (metodo) e selezionare **Aggiungi visualizzazione**.

    ![Aggiunta di una vista](aspnet-mvc-4-fundamentals/_static/image23.png "aggiunta di una vista")

    *Aggiunta di una visualizzazione*
3. Poiché il **finestra di dialogo Aggiungi visualizzazione** da cui è stato richiamato il **StoreController**, aggiungerà il modello di vista per impostazione predefinita in un **\Views\Store\Index.cshtml** file. Verificare i **creare una fortemente tipizzate-visualizzazione** casella di controllo e quindi selezionare **StoreIndexViewModel** come il **classe modello**. Inoltre, assicurarsi che il motore di visualizzazione selezionato sia **Razor**. Fare clic su **Aggiungi**.

    ![Aggiungi finestra di dialogo di visualizzazione](aspnet-mvc-4-fundamentals/_static/image24.png "Visualizza finestra di dialogo Aggiungi")

    *Visualizza finestra di dialogo Aggiungi*

    Il **\Views\Store\Index.cshtml** viene creato e aperto il file di modello di visualizzazione. Basato sulle informazioni fornite per il **Aggiungi visualizzazione** finestra di dialogo nell'ultimo passaggio, la visualizzazione modello si aspetta di ricevere un **StoreIndexViewModel** istanza come i dati da utilizzare per generare una risposta HTML. Si noterà che il modello eredita un `ViewPage<musicstore.viewmodels.storeindexviewmodel>` nel linguaggio c#.

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>Attività 5: aggiornare il modello di vista

In questa attività si aggiornerà il modello di vista creato nell'ultima attività per recuperare il numero di generi e i relativi nomi all'interno della pagina.

> [!NOTE]
> Si userà sintassi @ (anche detti &quot;nugget di codice&quot;) per eseguire il codice all'interno del modello di visualizzazione.

1. Nel **index. cshtml** nel file, all'interno di **Store** cartella, sostituire il codice con quanto segue:

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. Scorrere l'elenco genre nella **StoreIndexViewModel** e creare un elemento HTML **&lt;ul&gt;** elencate mediante un **foreach** ciclo.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. Premere **F5** per eseguire l'applicazione ed esplorare **/Store**. Verrà visualizzato l'elenco di generi passato il **StoreIndexViewModel** dell'oggetto dal **StoreController** al modello di visualizzazione.

    ![Visualizzazione con un elenco dei generi](aspnet-mvc-4-fundamentals/_static/image26.png "visualizzazione mostra un elenco dei generi")

    *Visualizzazione con un elenco dei generi*
4. Chiudere il browser.

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>Esercizio 6: Utilizzo di parametri nella visualizzazione

Nell'esercizio 3 è stato descritto come passare parametri al Controller. In questo esercizio, si apprenderà come usare questi parametri nel modello di visualizzazione. A tale scopo, verranno presentate innanzitutto alle classi di modello che consentono di gestire la logica di dominio e i dati. Inoltre, si apprenderà come creare collegamenti a pagine all'interno dell'applicazione ASP.NET MVC senza doversi preoccupare di elementi quali i percorsi URL codifica.

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>Attività 1: aggiunta di classi del modello

A differenza di ViewModel, che vengono creati solo per passare le informazioni dal Controller alla visualizzazione, le classi del modello sono create per includono e gestiscono i dati e dominio per la logica. In questa attività si aggiungeranno due classi di modello per rappresentare questi concetti: **Genre** e **Album**.

1. Se non è già aperta, avviare **Visual Studio Express per Web**
2. Nel **File** menu, scegliere **aprire il progetto**. Nella finestra di dialogo Apri progetto individuare **Source\Ex06 UsingParametersInView\Begin**, selezionare **Begin.sln** e fare clic su **Open**. In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
3. Aggiungere un **Genre** classe del modello. A tale scopo, fare doppio clic sui **modelli** cartella nel **Esplora soluzioni**, selezionare **Add** e quindi il **nuovo elemento** opzione. Sotto **codice**, scegliere il **classe** item e denominare il file *Genre.cs*, quindi fare clic su **Aggiungi**.

    ![Aggiunta di una classe](aspnet-mvc-4-fundamentals/_static/image27.png "aggiunta di una classe")

    *Aggiunta di un nuovo elemento*

    ![Aggiungere classe di modello Genre](aspnet-mvc-4-fundamentals/_static/image28.png "aggiungere classe di modello al genere")

    *Aggiungere classe di modello al genere*
4. Aggiungere un **nome** proprietà alla classe del genere. A tale scopo, aggiungere il codice seguente:

    (Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - Genre Ex6*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. Attenersi alla procedura descritta in precedenza, aggiungere un' **Album** classe. A tale scopo, fare doppio clic sui **modelli** cartella nel **Esplora soluzioni**, selezionare **Add** e quindi il **nuovo elemento** opzione. Sotto **codice**, scegliere il **classe** item e denominare il file *Album.cs*, quindi fare clic su **Aggiungi**.
6. Aggiungere due proprietà alla classe Album: **Genre** e **titolo**. A tale scopo, aggiungere il codice seguente:

    (Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - Album Ex6*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>Attività 2: aggiunta di un StoreBrowseViewModel

Oggetto **StoreBrowseViewModel** verrà usato in questa attività per mostrare gli album che corrispondono a un genere selezionato. In questa attività verrà creare questa classe e quindi aggiungere due proprietà che consentono di gestire il **Genre** e il relativo **Album**dell'elenco.

1. Aggiungere un **StoreBrowseViewModel** classe. A tale scopo, fare doppio clic sul **ViewModel** cartella il **Esplora soluzioni**, selezionare **Add** e quindi il **nuovo elemento** opzione. Sotto **codice**, scegliere il **classe** item e denominare il file *StoreBrowseViewModel.cs*, quindi fare clic su **Aggiungi**.
2. Aggiungere un riferimento ai modelli nel **StoreBrowseViewModel** classe. A tale scopo, aggiungere il codice seguente utilizza spazio dei nomi:

    (Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - Ex6 UsingModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. Aggiungere due proprietà consentono **StoreBrowseViewModel** classe: **Genre** e **album**. A tale scopo, aggiungere il codice seguente:

    (Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - Ex6 ModelProperties*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> What ' s **elenco&lt;Album&gt;**  ?: questa definizione Usa i **elenco&lt;T&gt;**  tipo, dove **T** vincola la tipo da cui ottenere elementi di questo **elenco** appartiene, in questo caso **Album** (o uno qualsiasi dei relativi discendenti).
> 
> La possibilità di progettare classi e metodi che rinviano la specifica di uno o più tipi finché non la classe o il metodo viene dichiarato e creare un'istanza dal codice client è una funzionalità del linguaggio c# denominato **Generics**.
> 
> **Elenco&lt;T&gt;**  equivale a generico di **ArrayList** digitare ed è disponibile nel **System.Collections.Generic** dello spazio dei nomi. Uno dei vantaggi dell'utilizzo **generics** è che poiché il tipo è specificato, non occorre occuparsi di operazioni, ad esempio il cast degli elementi nel controllo del tipo **Album** come si farebbe con un' **ArrayList**.

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>Attività 3: usando l'elemento ViewModel di nuovo nel StoreController

In questa attività si modificherà il **StoreController**del **Sfoglia** e **dettagli** metodi di azione per usare il nuovo **StoreBrowseViewModel** .

1. Aggiungere un riferimento alla cartella modelli in **StoreController** classe. A tale scopo, espandere la **controller** cartella nel **Esplora soluzioni** e aprire il **StoreController** classe. Aggiungere quindi il codice seguente:

    (Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - Ex6 UsingModelInController*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. Sostituire il **esplorare** metodo di azione per utilizzare il **StoreViewBrowseController** classe. Si creerà un genere e due nuovi oggetti di album con dati fittizi (nel laboratorio pratico successivo si utilizzeranno dati reali da un database). A tale scopo, sostituire il **esplorare** metodo con il codice seguente:

    (Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - Ex6 BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. Sostituire il **informazioni dettagliate** metodo di azione per utilizzare il **StoreViewBrowseController** classe. Si creerà una nuova **Album** oggetto da restituire per il **visualizzazione**. A tale scopo, sostituire il **dettagli** metodo con il codice seguente:

    (Code - Snippet *nozioni fondamentali su ASP.NET MVC 4 - Ex6 DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>Attività 4: aggiunta di un modello di vista di esplorazione

In questa attività si aggiungerà un **esplorare** visualizzazione per mostrare gli album trovati per un genere specifico.

1. Prima di creare il nuovo modello di vista, è necessario compilare il progetto in modo che il **Aggiungi visualizzazione** finestra di dialogo è a conoscenza di **ViewModel** classe da utilizzare. Compilare il progetto selezionando il **compilare** voce di menu e quindi **compilazione MvcMusicStore**.
2. Aggiungere un **esplorare** visualizzazione. A tale scopo, fare doppio clic nella **esplorare** metodo di azione del **StoreController** e fare clic su **Aggiungi visualizzazione**.
3. Nel **Aggiungi visualizzazione** finestra di dialogo, verificare che il nome della vista sia **Sfoglia**. Verificare i **creare una visualizzazione fortemente tipizzata** casella di controllo e selezionare **StoreBrowseViewModel** dal **classe modello** elenco a discesa. Lasciare gli altri campi con il valore predefinito. Fare quindi clic su **Aggiungi**.

    ![Aggiunta di una visualizzazione esplorazione](aspnet-mvc-4-fundamentals/_static/image29.png "aggiunta di una vista di esplorazione")

    *Aggiunta di una vista di esplorazione*
4. Modificare il **Browse.cshtml** per visualizzare le informazioni del genere, l'accesso di **StoreBrowseViewModel** oggetto passato al modello di visualizzazione. A tale scopo, sostituire il contenuto con quanto segue: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Attività 5: esecuzione dell'applicazione

In questa attività si testerà che il **esplorare** che consente di recuperare gli album dal **Sfoglia** azione del metodo.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in   **/Store/Sfoglia? Genre = Disco** per verificare che l'azione restituisce due album.

    ![Esplorazione Store discoteca album](aspnet-mvc-4-fundamentals/_static/image30.png "esplorazione album discoteca Store")

    *Esplorazione di album discoteca Store*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>Attività 6 - visualizzazione di informazioni su una specifica Album

In questa attività si implementerà il **Store/dettagli** visualizzazione per visualizzare informazioni su un album specifico. In questo laboratorio pratico, tutto ciò che visualizzerà relative all'album è già contenuto nel **vista** modello. In questo caso, invece di crearne una **StoreDetailsViewModel** (classe), si userà corrente **StoreBrowseViewModel** passando all'Album a tale modello.

1. Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio. Aggiungere un nuovo **informazioni dettagliate** visualizzare per il **StoreController**del **dettagli** metodo di azione. A tale scopo, fare doppio clic sul **informazioni dettagliate** metodo nella **StoreController** classe e fare clic su **Aggiungi visualizzazione**.
2. Nel **Aggiungi visualizzazione** finestra di dialogo verificare che il **nome della vista** viene **dettagli**. Verificare i **creare una visualizzazione fortemente tipizzata** casella di controllo e selezionare **Album** dal **classe modello** elenco a discesa. Lasciare gli altri campi con il valore predefinito. Fare quindi clic su **Aggiungi**. Verrà creata e aprire una **\Views\Store\Details.cshtml** file.

    ![Aggiunta di una visualizzazione dettagli](aspnet-mvc-4-fundamentals/_static/image31.png "aggiunta di una visualizzazione dettagli")

    *Aggiunta di una visualizzazione dettagli*
3. Modificare il **Details. cshtml** file per visualizzare le informazioni di un album trae, l'accesso alla **Album** oggetto passato al modello di visualizzazione. A tale scopo, sostituire il contenuto con quanto segue: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Attività 7: esecuzione dell'applicazione

In questa attività si testerà che il **informazioni dettagliate** vista recupera le informazioni di Album dalle **azione in dettaglio** (metodo).

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nel **Home** pagina. Modificare l'URL **/Store/Details/5** per verificare le informazioni dell'album.

    ![Esplorazione dettaglio album](aspnet-mvc-4-fundamentals/_static/image32.png "dettaglio album di esplorazione")

    *Esplorazione dettaglio dell'Album*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>Attività 8: aggiunta di collegamenti tra pagine

In questa attività si aggiungerà un collegamento degli Store per avere un collegamento in nome ogni genere appropriati **/Store/Sfoglia** URL. In questo modo, quando si fa clic in genere, ad esempio **Disco**, passerà a **/Store/Sfoglia? genre = Disco** URL.

1. Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio. Aggiornamento di **indice** pagina per aggiungere un collegamento al **Sfoglia** pagina. A questo scopo, nella **Esplora soluzioni** espandere il **viste** cartella, il **Store** cartella e fare doppio clic il **index. cshtml** pagina.
2. Aggiungere un collegamento alla visualizzazione di esplorazione che indica il genere selezionato. A tale scopo, sostituire il codice evidenziato seguente all'interno di **&lt;li&gt;** tag: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > un altro approccio potrebbe essere collegato direttamente alla pagina, con un codice simile al seguente:
   > 
   > &lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;
   > 
   > Sebbene questo approccio funzioni, a seconda stringa hardcoded. Se in un secondo momento si rinomina il Controller, sarà necessario modificare manualmente questa istruzione. Un'alternativa migliore consiste nell'utilizzare un **HTML Helper** (metodo). ASP.NET MVC include un metodo HTML Helper che è disponibile per tali attività. Il **Html.ActionLink()** metodo helper rende più semplice compilare HTML **&lt;una&gt;** collegamenti, assicurandosi che i percorsi URL siano URL correttamente codificati.
   > 
   > Htlm.ActionLink dispone di diversi overload. In questo esercizio si userà uno che accetta tre parametri:
   > 
   > 1. Testo del collegamento, che verrà visualizzato il nome genere
   > 2. Nome azione del controller (**esplorare**)
   > 3. I valori di parametro, che specifica il nome di route (**Genre**) e il valore (**nome genere**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>Attività 9: esecuzione dell'applicazione

In questa attività si testerà che ogni genere viene visualizzato con un collegamento a appropriato **/Store/Sfoglia** URL.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL **/Store** per verificare che ogni genere collegamenti appropriati **/Store/Sfoglia** URL.

    ![Esplorazione generi con collegamenti alla pagina di esplorazione](aspnet-mvc-4-fundamentals/_static/image33.png "generi di esplorazione con i collegamenti alla pagina di esplorazione")

    *Esplorazione generi con collegamenti alla pagina di esplorazione*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>Attività 10 - usando ViewModel dinamici raccolta per passare i valori

In questa attività si apprenderà un metodo semplice e potente per passare i valori tra il Controller e la visualizzazione senza apportare alcuna modifica nel modello. ASP.NET MVC 4 fornisce la raccolta &quot;ViewModel&quot;, che può essere assegnato a qualsiasi valore dinamico e controller e visualizzazioni nonché accedere direttamente.

Verrà utilizzata per passare un elenco di raccolta dinamica ViewBag &quot; **contrassegnata con stelle generi** &quot; dal controller alla vista. La visualizzazione dell'indice Store avranno accesso a **ViewModel** e visualizzare le informazioni.

1. Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio. Aprire **StoreController.cs** e modificare **indice** metodo per creare un elenco di contrassegnata con stelle generi nella raccolta di ViewModel:

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > È anche possibile usare la sintassi **ViewBag [&quot;Starred&quot;]** per accedere alle proprietà.
2. L'icona a stella **&quot;starred.png&quot;** è incluso nel **Source\Assets\Images** cartella della presente esercitazione. Per aggiungerla all'applicazione, trascinare i propri contenuti da un **Windows Explorer** finestra all'interno di **Esplora soluzioni** in Visual Web Developer Express, come illustrato di seguito:

    ![Immagine star aggiunta alla soluzione](aspnet-mvc-4-fundamentals/_static/image34.png "immagine star aggiunta alla soluzione")

    *Aggiunta di immagine star alla soluzione*
3. Aprire la visualizzazione **Store/Index.cshtml** e modificare il contenuto. Leggeranno i &quot;contrassegnata con stelle&quot; proprietà nel **ViewBag** insieme e chiedere se il nome del genere corrente è nell'elenco. In questo caso si mostrerà un'icona a stella a destra per il collegamento al genere.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>Attività 11: eseguire l'applicazione

In questa attività si testerà che generi con stelle visualizzino un'icona a stella.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nel **Home** pagina. Modificare l'URL **/Store** per verificare che ogni genere in primo piano è associata l'etichetta respecting:

    ![Esplorazione generi con elementi con stelle](aspnet-mvc-4-fundamentals/_static/image35.png "generi di esplorazione con gli elementi con stelle")

    *Esplorazione generi con elementi con stelle*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>Esercizio 7: Una visione generale di nuovo modello di ASP.NET MVC 4

In questo esercizio si esploreranno i miglioramenti nei modelli di progetto ASP.NET MVC 4, come ad esempio funzioni rilevanti al massimo del nuovo modello.

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>Attività 1: Esplorare il modello di applicazione Internet ASP.NET MVC 4

1. Se non è già aperta, avviare **Visual Studio Express per Web**
2. Selezionare il **File | New | Progetto** comando di menu. Nel **nuovo progetto** finestra di dialogo, seleziona il **Visual c# | Web** modello nel riquadro sinistro della struttura ad albero e scegliere il **applicazione Web ASP.NET MVC 4**. **Nome** il progetto *MusicStore* e aggiornare il **Nome soluzione** alla *Begin*, quindi selezionare una località (o lasciare il valore predefinito) e fare clic su **OK** .

    ![Creazione di un nuovo progetto ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "creando un nuovo progetto ASP.NET MVC 4")

    *Creazione di un nuovo progetto ASP.NET MVC 4*
3. Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo, seleziona la **applicazione Internet** modello di progetto e fare clic su **OK**. Si noti che è possibile selezionare Razor o ASPX come il motore di visualizzazione.

    ![Crea una nuova applicazione Internet ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image37.png "crea una nuova applicazione Internet ASP.NET MVC 4")

    *Crea una nuova applicazione Internet ASP.NET MVC 4*

    > [!NOTE]
    > Sintassi Razor è stato introdotto in ASP.NET MVC 3. L'obiettivo consiste nel ridurre al minimo il numero di caratteri e sequenze di tasti richieste in un file, abilitando un veloce e fluido codifica del flusso di lavoro. Razor Usa esistente C# /VB (o altro) competenze linguistiche e offre una sintassi di markup di modello che consente un'incredibile HTML costruzione del flusso di lavoro.
4. Premere **F5** per eseguire la soluzione e visualizzare il modello rinnovato. È possibile estrarre le funzionalità seguenti:

    1. **Modelli di stile moderno**

        I modelli sono stati rinnovati, che fornisce altri stili dall'aspetto moderno.

        ![I modelli ASP.NET MVC 4 modificato nello stile](aspnet-mvc-4-fundamentals/_static/image38.png "modificato nello stile modelli ASP.NET MVC 4")

        *Modelli ASP.NET MVC 4 modificato nello stile*
    2. **Rendering adattivo**

        Consulta il ridimensionamento della finestra del browser e notare come il layout di pagina si adatta dinamicamente per le nuove dimensioni della finestra. Questi modelli utilizzano la tecnica di rendering adattivo per eseguire il rendering correttamente in piattaforme mobili e desktop senza eseguire alcuna personalizzazione.

        ![Modello di progetto ASP.NET MVC 4 in dimensioni di browser differenti](aspnet-mvc-4-fundamentals/_static/image39.png "modello di progetto ASP.NET MVC 4 in dimensioni diversa del browser")

        *Modello di progetto ASP.NET MVC 4 in dimensioni diversa del browser*
5. Chiudere il browser per arrestare il debugger, tornare a Visual Studio.
6. A questo punto si è in grado di esplorare la soluzione e Scopri alcune delle nuove funzionalità introdotte da ASP.NET MVC 4 nel modello di progetto.

    ![ASP.NET MVC4-internet---modello di progetto applicazione](aspnet-mvc-4-fundamentals/_static/image40.png "il modello di progetto applicazione Internet ASP.NET MVC 4")

    *Il modello di progetto applicazione Internet ASP.NET MVC 4*

   1. **Tag HTML5**

       Esplorare le viste del modello per individuare il nuovo tag di tema, ad esempio open **About. cshtml** visualizzare entro **Home** cartella.

       ![Nuovo modello, utilizzando il markup Razor e HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "nuovo modello, utilizzando il markup Razor e HTML5")

       *Nuovo modello, utilizzando il markup Razor e HTML5*
   2. **Librerie JavaScript inclusione**

      1. **jQuery**: jQuery semplifica l'attraversamento dei documenti HTML, la gestione degli eventi, l'animazione e interazioni di Ajax.
      2. **interfaccia utente di jQuery**: questa libreria fornisce astrazioni per themeable widget basato la libreria JavaScript jQuery e animazione, effetti avanzati e interazione di basso livello.

         > [!NOTE]
         > Sono disponibili informazioni jQuery UI e jQuery nelle [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).
      3. **Knockout. js**: modello predefinito di ASP.NET MVC 4 include ora **Knockout. js**, un framework MVVM di JavaScript che consente di creare applicazioni web avanzate e particolarmente efficienti con JavaScript e HTML. Come in ASP.NET MVC 3, jQuery e librerie dell'interfaccia utente di jQuery sono inoltre inclusi in ASP.NET MVC 4.

          > [!NOTE]
          > È possibile ottenere altre informazioni sulla libreria Knockout. js in questo collegamento: [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/).
      4. **Modernizr**: questa libreria viene eseguito automaticamente, rendere un sito Web compatibile con i browser meno recenti quando si usano le tecnologie HTML5 e CSS3.

          > [!NOTE]
          > È possibile ottenere altre informazioni sulla libreria Modernizr in questo collegamento: [ http://www.modernizr.com/ ](http://www.modernizr.com/).
   3. **SimpleMembership incluso nella soluzione**

       SimpleMembership è stata progettata come una sostituzione per il sistema di provider di appartenenze e ruoli ASP.NET precedente. Include numerose nuove funzionalità che rendono più semplice per gli sviluppatori per proteggere le pagine web in modo più flessibile.

       Il modello Internet già dispone di configurare alcuni aspetti da integrare SimpleMembership, ad esempio, il AccountController sono pronti per usare OAuthWebSecurity (per la registrazione dell'account OAuth, account di accesso, gestione e così via) e la sicurezza Web.

       ![SimpleMembership incluso nella soluzione](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership incluso nella soluzione")

       *SimpleMembership incluso nella soluzione*

       > [!NOTE]
       > Trovare altre informazioni sulle [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.

> [!NOTE]
> Inoltre, è possibile distribuire questa applicazione per siti Web di Azure seguenti [appendice b: pubblicazione un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa pratica si è appreso le nozioni di base di ASP.NET MVC:

- Elementi principali di un'applicazione MVC e come interagiscono
- Come creare un'applicazione ASP.NET MVC
- Come aggiungere e configurare i controller per gestire i parametri passati tramite l'URL e la stringa di query
- Come aggiungere una pagina master layout per la configurazione di un modello per il contenuto HTML comune, un foglio di stile per migliorare l'aspetto e un modello di vista per visualizzare il contenuto HTML
- Come usare il modello a ViewModel per passare le proprietà per il modello di vista per visualizzare informazioni dinamiche
- Come usare i parametri passati ai controller nel modello di visualizzazione
- Come aggiungere collegamenti a pagine all'interno dell'applicazione ASP.NET MVC
- Come aggiungere e usare le proprietà dinamiche in una vista
- I miglioramenti nei modelli di progetto ASP.NET MVC 4

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice a: installazione di Visual Studio Express 2012 per Web

È possibile installare **Microsoft Visual Studio Express 2012 per Web** o da un'altra &quot;Express&quot; versione utilizzando il **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web Microsoft*.

1. Passare a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e cercare il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **Installa ora**. Se non hai **instalace Webové Platformy** si verrà reindirizzati per scaricarlo e installarlo prima di tutto.
3. Una volta **instalace Webové Platformy** è aperto, fare clic su **installare** per avviare il programma di installazione.

    ![Installa Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "installa Visual Studio Express")

    *Installa Visual Studio Express*
4. Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.

    ![Accettare le condizioni di licenza](aspnet-mvc-4-fundamentals/_static/image44.png)

    *Accettare le condizioni di licenza*
5. Attendere finché non viene completato il processo di download e l'installazione.

    ![Stato dell'installazione](aspnet-mvc-4-fundamentals/_static/image45.png)

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](aspnet-mvc-4-fundamentals/_static/image46.png)

    *Installazione completata*
7. Fare clic su **Exit** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per Web, fare clic per il **avviare** schermata e iniziare la scrittura &quot; **Visual Studio Express**&quot;, quindi fare clic sul **Visual Studio Express per Web** il riquadro.

    ![Visual Studio Express per il riquadro Web](aspnet-mvc-4-fundamentals/_static/image47.png)

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

    ![Accedere al portale di Azure](aspnet-mvc-4-fundamentals/_static/image48.png "accedere al portale di Azure")

    *Accedere al portale di gestione di Azure*
2. Fare clic su **New** sulla barra dei comandi.

    ![Creazione di un nuovo sito Web](aspnet-mvc-4-fundamentals/_static/image49.png "creando un nuovo sito Web")

    *Creazione di un nuovo sito Web*
3. Fare clic su **Compute** | **sito Web**. Quindi selezionare **creazione rapida** opzione. Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.

    > [!NOTE]
    > Un sito Web di Microsoft Azure è l'host per un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire. L'opzione Creazione rapida consente di distribuire un'applicazione web completa per il sito Web Microsoft Azure all'esterno del portale. Non include i passaggi per la configurazione di un database.

    ![Creazione di un nuovo sito Web utilizzando Creazione rapida](aspnet-mvc-4-fundamentals/_static/image50.png "creando un nuovo sito Web mediante Creazione rapida")

    *Creazione di un nuovo sito Web utilizzando Creazione rapida*
4. Attendere finché il nuovo **sito Web** viene creato.
5. Dopo aver creato il sito Web fare clic sul collegamento sotto i **URL** colonna. Verificare che il nuovo sito Web sia in funzione.

    ![Passare al nuovo sito web](aspnet-mvc-4-fundamentals/_static/image51.png "passare al nuovo sito web")

    *Passare al nuovo sito web*

    ![Sito Web in esecuzione](aspnet-mvc-4-fundamentals/_static/image52.png "sito Web in esecuzione")

    *Sito Web in esecuzione*
6. Tornare al portale e fare clic sul nome del sito web con il **nome** colonna per visualizzare le pagine di gestione.

    ![Aprire le pagine di gestione sito web](aspnet-mvc-4-fundamentals/_static/image53.png "aprire le pagine di gestione sito web")

    *Aprire le pagine di gestione sito Web*
7. Nel **Dashboard** nella pagina il **riepilogo rapido** fare clic sui **Scarica profilo di pubblicazione** collegamento.

    > [!NOTE]
    > Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in un sito Web di Azure per ogni metodo di pubblicazione abilitato. Il profilo di pubblicazione contiene l'URL, le credenziali dell'utente e stringhe di database necessarie per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di questi programmi per pubblicazione di applicazioni web in siti Web di Windows Azure.

    ![Download del sito web di profilo di pubblicazione](aspnet-mvc-4-fundamentals/_static/image54.png "scaricando il sito web di profilo di pubblicazione")

    *Profilo di pubblicazione scaricato il sito Web*
8. Scaricare il file di profilo di pubblicazione in una posizione nota. In questo esercizio ulteriormente si vedrà come usare questo file per pubblicare un'applicazione web a un siti Web di Windows Azure da Visual Studio.

    ![Salvare il file di profilo di pubblicazione](aspnet-mvc-4-fundamentals/_static/image55.png "salvataggio del profilo di pubblicazione")

    *Salvare il file di profilo di pubblicazione*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Attività 2: configurare il Server di Database

Se l'applicazione Usa SQL Server database è necessario creare un Database di SQL server. Se si vuole distribuire una semplice applicazione che non Usa SQL Server è possibile ignorare questa attività.

1. È necessario un server di Database SQL per archiviare il database dell'applicazione. È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure all'indirizzo **database Sql** | **server** | **del Server Dashboard**. Se non si dispone di un server creato, è possibile crearne una usando il **Add** pulsante sulla barra dei comandi. Annotare il **nome server e URL, nome dell'account di accesso amministratore e la password**, come verranno usati nelle attività successiva. Non creare il database, come verrà creato in una fase successiva.

    ![Dashboard di Server di Database SQL](aspnet-mvc-4-fundamentals/_static/image56.png "Dashboard di Server di Database SQL")

    *Dashboard di Server di Database SQL*
2. Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server del **indirizzi IP consentiti**. A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP dal **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo e scegliere il ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) pulsante.

    ![Aggiunta indirizzo IP del Client](aspnet-mvc-4-fundamentals/_static/image58.png)

    *Aggiunta indirizzo IP del Client*
3. Una volta il **indirizzo IP del Client** viene aggiunto a indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.

    ![Confermare le modifiche](aspnet-mvc-4-fundamentals/_static/image59.png)

    *Confermare le modifiche*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web

1. Tornare alla soluzione ASP.NET MVC 4. Nel **Esplora soluzioni**, fare clic sul progetto sito web e selezionare **Publish**.

    ![Pubblicazione dell'applicazione](aspnet-mvc-4-fundamentals/_static/image60.png "pubblicazione dell'applicazione")

    *Pubblicazione del sito web*
2. Importare il profilo di pubblicazione che è stato salvato nella prima attività.

    ![L'importazione del profilo di pubblicazione](aspnet-mvc-4-fundamentals/_static/image61.png "l'importazione del profilo di pubblicazione")

    *Importazione del profilo di pubblicazione*
3. Fare clic su **convalidare la connessione**. Dopo aver completata la convalida fare clic su **successivo**.

    > [!NOTE]
    > La convalida è stata completata una volta che visualizzato un segno di spunta verde accanto al pulsante convalida connessione.

    ![La convalida connessione](aspnet-mvc-4-fundamentals/_static/image62.png "convalida della connessione")

    *Convalida della connessione*
4. Nel **impostazioni** nella pagina il **database** sezione, fare clic sul pulsante accanto alla casella di testo della connessione di database (vale a dire **DefaultConnection**).

    ![Configurazione della distribuzione Web](aspnet-mvc-4-fundamentals/_static/image63.png "configurazione della distribuzione Web")

    *Configurazione della distribuzione Web*
5. Configurare la connessione al database come segue:

   - Nel **nome Server** digitare l'URL server di Database SQL tramite il *tcp:* prefisso.
   - Nelle **nome utente** digitare il nome dell'account di accesso amministratore server.
   - Nelle **Password** digitare la password dell'account di accesso amministratore server.
   - Digitare un nuovo nome di database, ad esempio: *MVC4SampleDB*.

     ![Configurazione di stringa di connessione di destinazione](aspnet-mvc-4-fundamentals/_static/image64.png "configurazione stringa di connessione di destinazione")

     *Configurazione di stringa di connessione di destinazione*
6. Fare quindi clic su **OK**. Quando viene richiesto di creare il database, fare clic su **Sì**.

    ![Creazione del database](aspnet-mvc-4-fundamentals/_static/image65.png "creazione della stringa di database")

    *Creazione del database*
7. La stringa di connessione che si userà per connettersi al Database SQL di Windows Azure viene visualizzata all'interno di connessione predefinita nella casella di testo. Scegliere quindi **Avanti**.

    ![Stringa di connessione che punta al Database SQL](aspnet-mvc-4-fundamentals/_static/image66.png "stringa di connessione che punta al Database SQL")

    *Stringa di connessione che punta al Database SQL*
8. Nel **Preview** pagina, fare clic su **Publish**.

    ![Pubblicazione dell'applicazione web](aspnet-mvc-4-fundamentals/_static/image67.png "pubblicazione dell'applicazione web")

    *Pubblicazione dell'applicazione web*
9. Al termine del processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.

    ![Applicazione pubblicata in Azure](aspnet-mvc-4-fundamentals/_static/image68.png "applicazione pubblicata in Windows Azure")

    *Pubblicata dell'applicazione in Windows Azure*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Appendice c: uso dei frammenti di codice

Con i frammenti di codice, hai tutto il codice che necessario a tua disposizione. Il documento lab indicherà esattamente quando usarli, come illustrato nella figura seguente.

![Uso di frammenti di codice di Visual Studio per inserire codice nel progetto](aspnet-mvc-4-fundamentals/_static/image69.png "frammenti di codice con Visual Studio per inserire codice nel progetto")

*Uso di frammenti di codice di Visual Studio per inserire codice nel progetto*

***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***

1. Posizionare il cursore in cui si vuole inserire il codice.
2. Iniziare a digitare il nome del frammento di codice (senza spazi o trattini).
3. Guarda come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento di codice corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).
5. Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.

![Iniziare a digitare il nome di frammento](aspnet-mvc-4-fundamentals/_static/image70.png "inizia a digitare il nome del frammento di codice")

*Iniziare a digitare il nome del frammento di codice*

![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-fundamentals/_static/image71.png "premere Tab per selezionare il frammento di codice evidenziata")

*Premere Tab per selezionare il frammento di codice evidenziata*

![Il frammento di codice e premere nuovamente Tab espanderà](aspnet-mvc-4-fundamentals/_static/image72.png "si espanderà il frammento di codice e premere nuovamente Tab")

*Il frammento di codice e premere nuovamente Tab espanderà*

***Per aggiungere un frammento di codice usando il mouse (c#, Visual Basic e XML)*** 1. Pulsante destro del mouse in cui si desidera inserire il frammento di codice.

1. Selezionare **Inserisci frammento** aggiungendo **frammenti di codice**.
2. Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.

![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento](aspnet-mvc-4-fundamentals/_static/image73.png "rapida in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice")

*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice*

![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-fundamentals/_static/image74.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa")

*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa*
