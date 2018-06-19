---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: Nozioni fondamentali su ASP.NET MVC 4 | Documenti Microsoft
author: rick-anderson
description: Questa pratica è basato sull'archivio musica MVC (Model View Controller), un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di ASP.NET MV...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 225dff4663e0e556cfb8966f1078848b4c2b47a5
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/18/2018
ms.locfileid: "34306767"
---
# <a name="aspnet-mvc-4-fundamentals"></a>Nozioni fondamentali su ASP.NET MVC 4

Da [categorie Web Team](https://twitter.com/webcamps)

[Download Web categorie Kit di formazione](https://aka.ms/webcamps-training-kit)

Questa pratica è basato su archivio musica MVC (Model View Controller), un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di MVC ASP.NET e Visual Studio. All'interno del laboratorio si apprenderà semplicità, ancora potenza di utilizzo congiunto di queste tecnologie. Si inizierà con una semplice applicazione e verrà compilato finché non si dispone di un completamente funzionale applicazione Web ASP.NET MVC 4.

In questo laboratorio funziona con ASP.NET MVC 4.

Se si desidera esplorare la versione di ASP.NET MVC 3 dell'esercitazione applicazione, sarà possibile trovarlo in [negozio di musica MVC](https://github.com/evilDave/MVC-Music-Store).

Questa pratica, si presuppone che lo sviluppatore ha esperienza nelle tecnologie di sviluppo Web, ad esempio HTML e JavaScript.

> [!NOTE]
> Tutto il codice di esempio e i frammenti di codice sono inclusi nel Web categorie Training Kit, disponibile all'indirizzo [versioni Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Il progetto specifico per questa esercitazione è disponibile all'indirizzo [nozioni di base di ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>L'applicazione del Negozio

L'applicazione web di negozio che verrà compilata in tutto questo Lab è costituito da tre parti principali: market, estrazione e l'amministrazione. Visitatori sarà in grado di individuare album per genere, aggiungere album al carrello, esaminare la selezione e infine procedere con l'estrazione di accesso e completare l'ordine. Inoltre, gli amministratori di archivio sarà in grado di gestire gli album di disponibili, nonché le relative proprietà principale.

![Schermate di negozio](aspnet-mvc-4-fundamentals/_static/image1.png "schermate del Negozio")

*Schermate del Negozio*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>ASP.NET MVC 4 Essentials

Applicazione di archiviazione di file musicali sarà creati usando **Controller MVC (Model View)**, un modello architettonico che suddivide un'applicazione in tre componenti principali:

- **Modelli**: gli oggetti modello rappresentano le parti dell'applicazione che implementano la logica di dominio. Spesso, anche gli oggetti del modello recupero e archiviano lo stato del modello in un database.
- **Visualizzazioni:** visualizzazioni sono i componenti che consentono di visualizzare l'interfaccia utente (UI). In genere, questa interfaccia viene creata da dati del modello. Un esempio potrebbe essere la visualizzazione di modifica dell'album che consente di visualizzare le caselle di testo e un elenco a discesa in base allo stato corrente di un oggetto Album.
- **Controller:** controller sono i componenti che gestiscono l'interazione dell'utente, modificare il modello e selezionano infine una visualizzazione per il rendering dell'interfaccia utente. In un'applicazione MVC la visualizzazione presenta solo le informazioni, mentre il controller gestisce e risponde all'input e all'interazione dell'utente.

Il modello MVC consente di creare applicazioni che separano i diversi aspetti dell'applicazione (logica di input, logica di business e logica dell'interfaccia utente), fornendo un accoppiamento libero tra questi elementi. Questa separazione consente di gestire la complessità quando si compila un'applicazione, in quanto consente di concentrarsi su un aspetto dell'implementazione alla volta. Inoltre, il modello MVC rende semplice testare le applicazioni, incoraggiando anche l'utilizzo di sviluppo basato su test (TDD) per la creazione di applicazioni.

Il **ASP.NET MVC** framework offre un'alternativa al modello Web Form ASP.NET per la creazione di applicazioni Web basate su ASP.NET MVC. Il **ASP.NET MVC** framework è un framework di presentazione leggero e facile da testare che (come con applicazioni basate sul Web Form) è integrato con le funzionalità ASP.NET esistenti, ad esempio pagine master e basata sull'appartenenza autenticazione in modo da tutte le potenzialità di .NET framework core. Ciò è utile se si ha già familiarità con Web Form ASP.NET perché tutte le raccolte già in uso sono anche disponibili in ASP.NET MVC 4.

Inoltre, l'accoppiamento debole tra i tre componenti principali di un'applicazione MVC Alza di livello anche lo sviluppo parallelo. Ad esempio, uno sviluppatore può utilizzare la visualizzazione, uno sviluppatore secondo gli strumenti per la logica di controller e un terzo può concentrarsi sulla logica di business nel modello.

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questa pratica, si apprenderà come:

- Creare un'applicazione ASP.NET MVC da zero in base l'esercitazione musica archivio applicazioni
- Aggiungere controller per gestire gli URL per la Home page del sito e per esplorare le funzionalità principali
- Aggiungere una visualizzazione per personalizzare il contenuto visualizzato unitamente al proprio stile
- Aggiungere le classi di modello per contenere e gestire la logica di dati e di dominio
- Utilizzare il modello di modello di visualizzazione per passare le informazioni da azioni del controller per i modelli di visualizzazione
- Esplorare il nuovo modello di ASP.NET MVC 4 per le applicazioni internet

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

È necessario che gli elementi seguenti per completare questa esercitazione:

- [Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (lettura [appendice](#AppendixA) per istruzioni sulla modalità di installazione)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

**L'installazione di frammenti di codice**

Per praticità, gran parte del codice che vengono gestiti in questa esercitazione è disponibile come frammenti di codice di Visual Studio. Per installare i frammenti di codice eseguire **.\Source\Setup\CodeSnippets.vsi** file.

Se non si ha familiarità con i frammenti di codice di Visual Studio e si desidera per informazioni su come usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice c: con i frammenti di codice](#AppendixC)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questa pratica include gli esercizi seguenti:

1. [Esercizio 1: Creazione progetto di applicazione Web ASP.NET MVC MusicStore](#Exercise1)
2. [Esercizio 2: Creazione di un Controller](#Exercise2)
3. [Esercizio 3: Passaggio di parametri a un Controller](#Exercise3)
4. [Esercizio 4: Creazione di una vista](#Exercise4)
5. [Esercizio 5: Creazione di un modello di visualizzazione](#Exercise5)
6. [Esercizio 6: Utilizzo di parametri nella visualizzazione](#Exercise6)
7. [Esercizio 7: Un'introduzione ad ASP.NET MVC 4 nuovo modello](#Exercise7)

> [!NOTE]
> Ogni esercizio è accompagnato da un **fine** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato l'esercitazione dall'inizio. Se è necessario ulteriore assistenza utilizza gli esercizi, è possibile utilizzare questa soluzione come guida.


Tempo stimato per completare questa esercitazione: **60 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>Esercizio 1: Creazione progetto di applicazione Web ASP.NET MVC MusicStore

In questo esercizio, si apprenderà come creare un'applicazione ASP.NET MVC in Visual Studio Express 2012 per Web, nonché l'organizzazione della cartella principale. Inoltre, si apprenderà come aggiungere un nuovo Controller e una stringa semplice di visualizzarlo nella home page dell'applicazione.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>Attività 1 - Creazione del progetto di applicazione Web ASP.NET MVC

1. In questa attività si creerà un progetto di applicazione MVC ASP.NET vuoto utilizzando il modello MVC Visual Studio. Avviare **Visual Studio Express per Web**.
2. Scegliere **Nuovo progetto** dal menu **File**.
3. Nel **nuovo progetto** finestra di dialogo selezionare il **applicazione Web ASP.NET MVC 4** progetto di tipo, che si trova in **Visual c#,** **Web** modello elenco.
4. Modifica il **nome** a *MvcMusicStore*.
5. Impostare il percorso della soluzione all'interno di un nuovo **iniziare** cartella nella cartella di origine di questo esercizio, ad esempio **\Source\Ex01-CreatingMusicStoreProject\Begin [YOUR-HOL-PATH]**. Fare clic su **OK**.

    ![Creare una finestra di dialogo Nuovo progetto](aspnet-mvc-4-fundamentals/_static/image2.png "creare una finestra di dialogo Nuovo progetto")

    *Creare una finestra di dialogo Nuovo progetto*
6. Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo selezionare il **base** modello e assicurarsi che il **motore di visualizzazione** selezionato è **Razor**. Fare clic su **OK**.

    ![Finestra di dialogo Nuovo progetto ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "finestra di dialogo Nuovo progetto ASP.NET MVC 4")

    *Finestra di dialogo Nuovo progetto ASP.NET MVC 4*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>Attività 2: esplorare la struttura della soluzione

Il framework di MVC ASP.NET include un modello di progetto di Visual Studio che consente di creare applicazioni Web che supporta il modello MVC. Questo modello consente di creare una nuova applicazione Web MVC ASP.NET con le cartelle necessarie, modelli di elementi e le voci di file di configurazione.

In questa attività si esaminerà la struttura della soluzione per comprendere gli elementi coinvolti e le relative relazioni. Le cartelle seguenti vengono inclusi nel tutti l'applicazione ASP.NET MVC, perché per impostazione predefinita il framework di MVC ASP.NET utilizza un &quot;convenzione configurazione&quot; approccio e rende alcuni presupposti in base a nomi di cartella convenzioni.

1. Una volta creato il progetto, esaminare la struttura di cartelle che è stata creata in Esplora soluzioni sul lato destro:

    ![Struttura di cartelle di MVC ASP.NET in Esplora soluzioni](aspnet-mvc-4-fundamentals/_static/image4.png "struttura di cartelle di MVC ASP.NET in Esplora soluzioni")

    *Struttura di cartelle di MVC ASP.NET in Esplora soluzioni*

   1. **Controller**. Questa cartella contiene le classi controller. In un'applicazione MVC di base, i controller sono responsabili per la gestione di interazione dell'utente finale, la modifica del modello e infine scegliendo una visualizzazione per il rendering dell'interfaccia utente.

       > [!NOTE]
       > Il framework MVC richiede che i nomi di tutti i controller terminino con &quot;Controller&quot;-ad esempio HomeController, LoginController o ProductController.
   2. **Modelli**. Questa cartella viene fornita per le classi che rappresentano il modello di applicazione per l'applicazione Web MVC. Questo include in genere il codice che definisce gli oggetti e la logica per l'interazione con l'archivio dati. In genere, gli oggetti modello effettivo sarà nelle librerie di classi separato. Tuttavia, quando si crea una nuova applicazione, si includono le classi e spostarle in librerie di classi separate in un momento successivo nel ciclo di sviluppo.
   3. **Viste**. Questa cartella è la posizione consigliata per le viste, i componenti responsabili per la visualizzazione dell'interfaccia utente dell'applicazione. Le visualizzazioni utilizzano file con estensione aspx, ascx, cshtml e master, oltre a qualsiasi altro file che sono correlato al rendering delle visualizzazioni. Cartella Views contiene una cartella per ogni controller. la cartella è denominata con il prefisso del nome del controller. Ad esempio, se si dispone di un controller denominato **HomeController**, la cartella Views conterrà una cartella Home. Per impostazione predefinita, quando il framework di MVC ASP.NET carica una visualizzazione, la ricerca di un file con estensione aspx con il nome della visualizzazione richiesta nella cartella Views\controllerName (**viste [ControllerName] [azione] aspx**) o (**viste [ControllerName] [Azione] cshtml**) per le visualizzazioni Razor.

      > [!NOTE]
      > Oltre alle cartelle elencate in precedenza, un'applicazione Web MVC utilizza il **Global. asax** impostazioni predefinite di file per impostare il routing degli URL globale e viene utilizzato il **Web. config** file per configurare l'applicazione.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>Attività 3: aggiunta di un HomeController

Nelle applicazioni ASP.NET che non utilizzano il framework di MVC, interazione dell'utente è organizzata intorno alle pagine e intorno alla generazione e la gestione degli eventi da tali pagine. Al contrario, interazione dell'utente con le applicazioni ASP.NET MVC è organizzata intorno a controller e i relativi metodi di azione.

D'altra parte, framework di MVC ASP.NET esegue il mapping degli URL alle classi che vengono definite i controller. I controller di elaborare le richieste in ingresso, gestire l'input dell'utente e le interazioni, eseguire la logica di applicazione appropriata e determinare la risposta da inviare al client (visualizzazione del contenuto HTML, scaricare un file, reindirizzare a un altro URL e così via). Nel caso di visualizzazione HTML, una classe controller chiama in genere un componente di visualizzazione separato per generare il markup HTML per la richiesta. In un'applicazione MVC la visualizzazione presenta solo le informazioni, mentre il controller gestisce e risponde all'input e all'interazione dell'utente.

In questa attività si aggiungerà una classe Controller che gestirà gli URL per la Home page del sito Negozio.

1. Fare doppio clic su **controller** cartella in Esplora soluzioni, selezionare **Aggiungi** e quindi **Controller** comando:

    ![Aggiungere un comando Controller](aspnet-mvc-4-fundamentals/_static/image5.png "aggiungere un comando Controller")

    *Aggiungere il comando Controller*
2. Il **Aggiungi Controller** viene visualizzata la finestra. Denominare il controller *HomeController* e premere **Aggiungi**.

    ![Finestra di dialogo Controller Aggiungi](aspnet-mvc-4-fundamentals/_static/image6.png "Controller finestra di dialogo Aggiungi")

    *Aggiungere una finestra di dialogo Controller*
3. Il file **HomeController.cs** viene creato nel **controller** cartella. Per avere la **HomeController** restituire una stringa nella relativa operazione sull'indice, sostituire il **indice** metodo con il codice seguente:

    (- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Ex1 HomeController indice*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Attività 4: esecuzione dell'applicazione

In questa attività verrà provare l'applicazione in un web browser.

1. Premere **F5** per eseguire l'applicazione. La compilazione del progetto e viene avviato il Server Web IIS locale. Locale Server Web IIS verrà aperto automaticamente un web browser che punta all'URL del server Web.

    ![Applicazione in esecuzione in un web browser](aspnet-mvc-4-fundamentals/_static/image7.png "applicazione in esecuzione in un web browser")

    *Applicazione in esecuzione in un web browser*

    > [!NOTE]
    > Il Server Web IIS locale verrà eseguito il sito Web su un numero casuale porta libera. Nella figura precedente, il sito è in esecuzione in `http://localhost:50103/`, pertanto è usando porta 50103. Il numero di porta può variare.
2. Chiudere il browser.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>Esercizio 2: Creazione di un Controller

In questo esercizio, si apprenderà come aggiornare il controller per implementare la funzionalità semplice dell'applicazione Negozio. Tale controller verrà definiti i metodi di azione per gestire le richieste specifiche seguenti:

- Una pagina di elenco di generi di musica nell'archivio di musica
- Una pagina di esplorazione che elenca tutti gli album musica per un particolare genere
- Una pagina di dettagli che contiene informazioni su un album musicali specifici

Per l'ambito di questo esercizio, tali azioni restituirà semplicemente una stringa a questo punto.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>Attività 1 - aggiunta di una nuova classe StoreController

In questa attività si aggiungerà un nuovo Controller.

1. Se non è già aperta, avviare **Visual Studio Express per Web 2012**.
2. Nel **File** menu, scegliere **Apri progetto**. Nella finestra di dialogo Apri progetto individuare **Source\Ex02 CreatingAController\Begin**selezionare **Begin.sln** e fare clic su **aprire**. In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
3. Aggiungere il nuovo controller. A tale scopo, fare doppio clic su di **controller** cartella in Esplora soluzioni, selezionare **Aggiungi** e quindi la **Controller** comando. Modifica il **nome Controller** a *StoreController*, fare clic su **Aggiungi**.

    ![Finestra di dialogo Controller Aggiungi](aspnet-mvc-4-fundamentals/_static/image8.png "Controller finestra di dialogo Aggiungi")

    *Aggiungere una finestra di dialogo Controller*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>Attività 2: Modifica azioni del StoreController

In questa attività si modificherà i Controller i metodi chiamati **azioni**. Le azioni sono responsabili per la gestione delle richieste di URL e determinare il contenuto che deve essere inviato nuovamente il browser o l'utente che ha richiamato l'URL.

1. Il **StoreController** classe esiste già un **indice** metodo. Si utilizzerà più avanti in questa esercitazione per implementare la pagina che elenca tutti i generi dell'archivio di musica. Per il momento, è sufficiente sostituire il **indice** (metodo) con il codice seguente che restituisce una stringa &quot;Hello da Store.Index()&quot;:

    (- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - indice StoreController Ex2*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. Aggiungere **Sfoglia** e **dettagli** metodi. A tale scopo, aggiungere il codice seguente per il **StoreController**:

    (- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Attività 3: esecuzione dell'applicazione

In questa attività verrà provare l'applicazione in un web browser.

1. Premere **F5** per eseguire l'applicazione.
2. Avvia il progetto nel **Home** pagina. Modificare l'URL per verificare l'implementazione di ogni azione.

    1. **/ Archiviare**. Si noterà  **&quot;Hello da Store.Index()&quot;**.
    2. **/ Archivio/Sfoglia**. Si noterà  **&quot;Hello da Store.Browse()&quot;**.
    3. **/ / Dettagli archivio**. Si noterà  **&quot;Hello da Store.Details()&quot;**.

        ![Esplorazione StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "esplorazione StoreBrowse")

        *Esplorazione /Store/Browse*
3. Chiudere il browser.

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>Esercizio 3: Passaggio di parametri a un Controller

Fino ad ora, si hanno stato restituzione stringhe costanti verso i controller. In questo esercizio si apprenderà come passare i parametri a un Controller utilizzando l'URL e la stringa di query e apportando quindi le azioni di metodo rispondere con il testo nel browser.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>Attività 1 - aggiunta parametro Genre StoreController

In questa attività, si utilizzerà il **querystring** per l'invio di parametri per il **Sfoglia** metodo di azione di **StoreController**.

1. Se non è già aperta, avviare **Visual Studio Express per Web**.
2. Nel **File** menu, scegliere **Apri progetto**. Nella finestra di dialogo Apri progetto individuare **Source\Ex03 PassingParametersToAController\Begin**selezionare **Begin.sln** e fare clic su **aprire**. In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
3. Aprire **StoreController** classe. A tale scopo, nel **Esplora**, espandere il **controller** cartella e fare doppio clic su **StoreController.cs**.
4. Modifica il **Sfoglia** (metodo), aggiunta di un parametro di stringa per richiedere per un genere specifico. ASP.NET MVC verranno automaticamente passare qualsiasi stringa di query o parametri denominati invio del form **genere** a questo metodo di azione quando viene richiamato. A tale scopo, sostituire il **Sfoglia** metodo con il codice seguente:

    (- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> Si utilizza il **HttpUtility** metodo di utilità per impedisce agli utenti di inserire Javascript nella vista con un collegamento come   **/archivio/Sfoglia? Genre =&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.
> 
> Per ulteriori informazioni, visitare [questo articolo di msdn](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Attività 2: esecuzione dell'applicazione

In questa attività si provare l'applicazione in un web browser e si utilizzerà il **genre** parametro.

1. Premere **F5** per eseguire l'applicazione.
2. Avvia il progetto nel **Home** pagina. Modificare l'URL in   */archiviare/Sfoglia? Genre = Disco* per verificare che l'azione riceve il parametro genere.

    ![Esplorazione StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "esplorazione StoreBrowseGenre = Disco")

    *Esplorazione /Store/Browse? Genre = Disco*
3. Chiudere il browser.

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>Attività 3: aggiunta di un parametro Id incorporato nell'URL

In questa attività, si utilizzerà il **URL** per passare un **Id** parametro per il **dettagli** il metodo di azione del **StoreController**. Il valore predefinito di ASP.NET MVC convenzione di routing consiste nel considerare il segmento di un URL dopo il nome del metodo di azione come un parametro denominato **Id**. Se il metodo di azione presenta un parametro denominato Id, quindi ASP.NET MVC automaticamente passerà il segmento URL all'utente come parametro. Nell'URL **archivio/dettagli/5**, **Id** verrà interpretato come **5**.

1. Modifica il **dettagli** metodo il **StoreController**, aggiungendo un **int** parametro denominato **id**. A tale scopo, sostituire il **dettagli** metodo con il codice seguente:

    (- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Attività 4: esecuzione dell'applicazione

In questa attività si provare l'applicazione in un web browser e si utilizzerà il **Id** parametro.

1. Premere **F5** per eseguire l'applicazione.
2. Avvia il progetto nel **Home** pagina. Modificare l'URL in */Store/Details/5* per verificare che l'azione riceve il parametro id.

    ![Esplorazione StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "esplorazione StoreDetails5")

    *Esplorazione /Store/Details/5*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>Esercizio 4: Creazione di una vista

Hanno finora è stato restituire stringhe da azioni del controller. Sebbene questo sia un modo utile per conoscere il funzionamento di controller, non è come vengono compilate le applicazioni Web reali. Le visualizzazioni sono componenti che forniscono un approccio migliore per generare codice HTML al browser con l'utilizzo di file di modello.

In questo esercizio si apprenderà come aggiungere una pagina master layout per la configurazione di un modello di contenuto HTML comune, un foglio di stile per migliorare l'aspetto del sito e, infine, un modello di visualizzazione per abilitare HomeController restituire l'HTML.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a>Attività 1: modificare il file \_cshtml

Il file **~/Views/Shared/\_cshtml** consente di configurare un modello per HTML comuni da utilizzare per l'intero sito Web. In questa attività si aggiungerà una pagina master layout con un'intestazione comune con i collegamenti all'area Home page e l'archivio.

1. Se non è già aperta, avviare **Visual Studio Express per Web**.
2. Nel **File** menu, scegliere **Apri progetto**. Nella finestra di dialogo Apri progetto individuare **Source\Ex04 CreatingAView\Begin**selezionare **Begin.sln** e fare clic su **aprire**. In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
3. Il file  <strong>\_cshtml</strong> contiene il layout di contenitore HTML per tutte le pagine nel sito. Include il <strong>&lt;html&gt;</strong> elemento per la risposta HTML, nonché il <strong>&lt;head&gt;</strong> e <strong>&lt;corpo&gt;</strong> elementi. <strong>@RenderBody()</strong> all'interno dell'HTML corpo identificare le aree di visualizzazione modelli saranno in grado di inserire il contenuto dinamico.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. Aggiungere un'intestazione comune con i collegamenti all'area Home page e l'archivio in tutte le pagine nel sito. A tale scopo, aggiungere il codice seguente sotto &lt;corpo&gt; istruzione.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. Includere un elemento div per eseguire il rendering nella sezione corpo di ogni pagina. Sostituire  <strong>@RenderBody()</strong> con il seguente codice higlighted: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > Non tutti sanno. Visual Studio 2012 include frammenti di codice che rendono più semplice aggiungere codice di uso comune in HTML, file di codice e altro ancora! Provarlo digitando **&lt;div&gt;** e premendo **scheda** due volte per inserire un completo **div** tag.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>Attività 2: aggiunta foglio di stile CSS

Il modello di progetto vuoto include un file CSS molto semplice che include solo gli stili utilizzati per visualizzare i form di base e i messaggi di convalida. Si utilizzerà per migliorare l'aspetto del sito aggiuntivi CSS e immagini (potenzialmente fornite da una finestra di progettazione).

In questa attività si aggiungerà un foglio di stile CSS per definire gli stili del sito.

1. Il file CSS e le immagini sono inclusi nel **Source\Assets\Content** cartella di questa esercitazione. Per aggiungerle all'applicazione, trascinare i propri contenuti da un **Esplora** finestra all'interno di **Esplora soluzioni** in Visual Studio Express per Web, come illustrato di seguito:

    ![Trascinare il contenuto di stile](aspnet-mvc-4-fundamentals/_static/image12.png "trascinando il contenuto di stile")

    *Trascinare il contenuto di stile*
2. Verrà visualizzata una finestra di dialogo di avviso, in cui viene chiesto di confermare se sostituire **Site** file e alcune immagini esistente. Controllare **applica a tutti gli elementi** e fare clic su **Sì**.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>Attività 3: aggiunta di un modello di visualizzazione

In questa attività si aggiungerà un modello di visualizzazione per generare la risposta HTML che verrà utilizzata la pagina master layout e CSS aggiunti in questo esercizio.

1. Per utilizzare un modello di visualizzazione durante l'esplorazione delle home page del sito, è necessario innanzitutto indicare che invece di restituire una stringa, il **HomeController indice** metodo restituirà un **vista**. Aprire **HomeController** classe e modificare il relativo **indice** per restituire un **ActionResult**, e restituire **View()**.

    (- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Ex4 HomeController indice*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. A questo punto, è necessario aggiungere un modello di visualizzazione appropriato. A tale scopo, **destro** all'interno di **indice** metodo di azione e selezionare **Aggiungi visualizzazione**. Verrà visualizzata la **Aggiungi visualizzazione** finestra di dialogo.

    ![Aggiunta di una vista all'interno del metodo indice](aspnet-mvc-4-fundamentals/_static/image13.png "aggiunta di una vista all'interno del metodo di indice")

    *Aggiunta di una vista all'interno del metodo di indice*
3. Il **Aggiungi visualizzazione** finestra di dialogo verrà visualizzata per generare un file di modello di visualizzazione. Per impostazione predefinita, questa finestra di dialogo pre-consente di popolare il nome del modello di visualizzazione in modo che corrisponda il metodo di azione che verrà utilizzato. Perché è stata utilizzata il **Aggiungi visualizzazione** menu di scelta rapida all'interno di **indice** il metodo di azione all'interno di HomeController, il **Aggiungi visualizzazione** finestra di dialogo ha indice come il nome della visualizzazione predefinita. Fare clic su **Aggiungi**.

    ![Visualizza finestra di dialogo Aggiungi](aspnet-mvc-4-fundamentals/_static/image14.png "Visualizza finestra di dialogo Aggiungi")

    *Visualizza finestra di dialogo Aggiungi*
4. Visual Studio genera un **cshtml** il modello di visualizzazione all'interno di **Views\Home** cartella e quindi lo apre.

    ![Visualizzazione dell'indice creato Home](aspnet-mvc-4-fundamentals/_static/image15.png "Vista Home indice creato")

    *Visualizzazione dell'indice iniziale creato*

    > [!NOTE]
    > nome e percorso del **cshtml** file è rilevante e segue le convenzioni di denominazione predefinito MVC ASP.NET.
    > 
    > La cartella \Views\**Home** corrisponde al nome del controller (**Home** Controller). Il nome del modello di visualizzazione (**indice**), il metodo di azione di controller che prevede di visualizzare la vista corrispondente.
    > 
    > In questo modo, ASP.NET MVC consente di evitare la necessità di specificare in modo esplicito il nome o il percorso di un modello di visualizzazione quando si utilizza questa convenzione di denominazione per restituire una visualizzazione.
5. Il modello di visualizzazione generato si basa sul  **\_cshtml** modello definito in precedenza. Aggiornare la proprietà ViewBag **Home**e modificare il contenuto principale per **questa è la Home Page**, come illustrato nel codice seguente:

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. Selezionare **MvcMusicStore** progetto in Esplora soluzioni e premere **F5** per eseguire l'applicazione.

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>Attività 4: verifica

Per verificare di avere eseguito correttamente tutti i passaggi nell'esercizio precedente, procedere come segue:

Con l'applicazione in un browser, si noti che:

1. Metodo di azione Index del HomeController trovare e visualizzare il **\Views\Home\Index.cshtml** visualizzare modello, anche se il codice chiamato **restituire View()**, poiché il modello di visualizzazione seguito il convenzione di denominazione standard.
2. Home Page Visualizza il messaggio di benvenuto definito all'interno di **\Views\Home\Index.cshtml** modello di visualizzazione.
3. Utilizza la Home Page di  **\_cshtml** modello, quindi il messaggio di benvenuto è contenuto all'interno del layout standard sito HTML.

    ![Home visualizzazione dell'indice utilizzando il LayoutPage e lo stile definito](aspnet-mvc-4-fundamentals/_static/image16.png "Home visualizzazione dell'indice utilizzando il LayoutPage e lo stile definito")

    *Visualizzazione dell'indice iniziale utilizzando il LayoutPage e lo stile definito*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>Esercizio 5: Creazione di un modello di visualizzazione

Finora, apportate alle visualizzazioni visualizzare hardcoded HTML, ma, per creare applicazioni web dinamiche, il modello di visualizzazione deve ricevere informazioni dal Controller. È una tecnica comune da utilizzare a tale scopo la **ViewModel** modello, che consente a un Controller di tutte le informazioni necessarie per generare la risposta HTML appropriata del pacchetto.

In questo esercizio, si apprenderà come creare una classe ViewModel e aggiungere le proprietà obbligatorie: il numero di generi un elenco di tali generi nell'archivio. Si verrà aggiornato anche il StoreController per utilizzare l'elemento creato ViewModel e, infine, si creerà un nuovo modello di visualizzazione che consente di visualizzare le proprietà menzionate nella pagina.

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>Attività 1 - Creazione di una classe ViewModel

In questa attività si creerà una classe ViewModel che consente di implementare lo scenario di elenco genre archivio.

1. Se non è già aperta, avviare **Visual Studio Express per Web**.
2. Nel **File** menu, scegliere **Apri progetto**. Nella finestra di dialogo Apri progetto individuare **Source\Ex05 CreatingAViewModel\Begin**selezionare **Begin.sln** e fare clic su **aprire**. In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
3. Creare un **ViewModel** cartella per contenere l'elemento ViewModel. A tale scopo, fare doppio clic su di primo livello **MvcMusicStore** progetto, selezionare **Aggiungi** e quindi **nuova cartella**.

    ![Aggiunta di una nuova cartella](aspnet-mvc-4-fundamentals/_static/image17.png "aggiunta una nuova cartella")

    *Aggiunta di una nuova cartella*
4. Denominare la cartella *ViewModel*.

    ![Cartella ViewModel in Esplora soluzioni](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModel cartella in Esplora soluzioni")

    *Cartella ViewModel in Esplora soluzioni*
5. Creare un **ViewModel** classe. A tale scopo, fare clic su di **ViewModel** cartella creato di recente, selezionare **Aggiungi** e quindi **nuovo elemento**. In **codice**, scegliere il **classe** elemento e denominare il file *StoreIndexViewModel.cs*, quindi fare clic su **Aggiungi**.

    ![Aggiunta di una nuova classe](aspnet-mvc-4-fundamentals/_static/image19.png "aggiunta una nuova classe")

    *Aggiunta di una nuova classe*

    ![Creazione classe StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel creazione classe")

    *Creazione classe StoreIndexViewModel*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>Attività 2: aggiunta di proprietà alla classe ViewModel

Esistono due parametri deve essere passato dal StoreController il modello di visualizzazione per generare la risposta HTML previsto: il numero di generi un elenco di tali generi nell'archivio.

In questa attività si aggiungeranno tali 2 proprietà per il **StoreIndexViewModel** classe: **NumberOfGenres** (integer) e **generi** (un elenco di stringhe).

1. Aggiungere **NumberOfGenres** e **generi** proprietà per il **StoreIndexViewModel** classe. A tale scopo, aggiungere le seguenti 2 righe alla definizione di classe:

    (- Frammento di codice *ASP.NET MVC 4 nozioni di base - proprietà Ex5 StoreIndexViewModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> Il **{get; impostare;}**  utilizza la notazione di # funzionalità proprietà implementate automaticamente. Fornisce i vantaggi di una proprietà senza richiedere di dichiarare un campo sottostante.

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>Attività 3: aggiornamento StoreController per utilizzare il StoreIndexViewModel

Il **StoreIndexViewModel** classe incapsula le informazioni necessarie per passare da **StoreController**del **indice** a un modello di visualizzazione per generare una risposta (metodo) .

In questa attività si aggiornerà il **StoreController** per utilizzare il **StoreIndexViewModel**.

1. Aprire **StoreController** classe.

    ![Apertura classe StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "StoreController apertura classe")

    *Classe StoreController apertura*
2. Per utilizzare il **StoreIndexViewModel** classe il **StoreController**, aggiungere lo spazio dei nomi seguente all'inizio del **StoreController** codice:

    (- Frammento di codice *ASP.NET MVC 4 nozioni di base - StoreIndexViewModel Ex5 utilizzando ViewModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. Modifica il **StoreController**del **indice** il metodo di azione in modo che crea e popola un **StoreIndexViewModel** dell'oggetto e quindi li passa a un modello di visualizzazione per generare una risposta HTML con esso.

    > [!NOTE]
    > In Lab &quot;accesso ai dati e i modelli di MVC ASP.NET&quot; si scriverà il codice che recupera l'elenco di generi di archivio da un database. Nel codice seguente, si creerà un **elenco** di generi dati fittizi in grado di popolare il **StoreIndexViewModel**.
    > 
    > Dopo la creazione e configurazione del **StoreIndexViewModel** dell'oggetto, verrà passato come argomento per il **vista** metodo. Ciò indica che il modello di visualizzazione verrà utilizzata tale oggetto per generare una risposta HTML con esso.
4. Sostituire il **indice** metodo con il codice seguente:

    (- Frammento di codice *ASP.NET MVC 4 nozioni di base - metodo Ex5 StoreController indice*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> Se non si ha familiarità con c#, potrebbe presumere che l'utilizzo **var** significa che il **viewModel** variabile è ad associazione tardiva. Che non è corretto, il compilatore c# viene usata in base a cui assegnare alla variabile di inferenza del tipo per determinare che **viewModel** è di tipo **StoreIndexViewModel**. Inoltre, per la compilazione locale **viewModel** variabile come un **StoreIndexViewModel** tipo di controllo in fase di compilazione di get e il supporto di editor di codice di Visual Studio.

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>Attività 4: creazione di un modello di visualizzazione che utilizza StoreIndexViewModel

In questa attività si creerà un modello di visualizzazione che verrà utilizzato un oggetto StoreIndexViewModel passato dal Controller per visualizzare un elenco di generi.

1. Prima di creare il nuovo modello di visualizzazione, è possibile compilare il progetto in modo che il **finestra di dialogo Aggiungi visualizzazione** viene a conoscenza di **StoreIndexViewModel** classe. Compilare il progetto selezionando il **compilare** voce di menu e quindi **compilare MvcMusicStore**.

    ![Compilazione del progetto](aspnet-mvc-4-fundamentals/_static/image22.png "la compilazione del progetto")

    *Compilazione del progetto*
2. Creare un nuovo modello di visualizzazione. A tale scopo, fare doppio clic all'interno di **indice** (metodo) e selezionare **Aggiungi visualizzazione**.

    ![Aggiunta di una vista](aspnet-mvc-4-fundamentals/_static/image23.png "aggiunta di una vista")

    *Aggiunta di una visualizzazione*
3. Poiché il **finestra di dialogo Aggiungi visualizzazione** da cui è stato richiamato il **StoreController**, verranno aggiunti il modello di visualizzazione per impostazione predefinita in un **\Views\Store\Index.cshtml** file. Controllare il **creare una fortemente tipizzato-visualizzazione** casella di controllo e quindi selezionare **StoreIndexViewModel** come il **classe modello**. Inoltre, assicurarsi che il motore di visualizzazione selezionato sia **Razor**. Fare clic su **Aggiungi**.

    ![Visualizza finestra di dialogo Aggiungi](aspnet-mvc-4-fundamentals/_static/image24.png "Visualizza finestra di dialogo Aggiungi")

    *Visualizza finestra di dialogo Aggiungi*

    Il **\Views\Store\Index.cshtml** Visualizza file di modello viene creato e aperto. In base alle informazioni disponibili per il **Aggiungi visualizzazione** finestra di dialogo nell'ultimo passaggio, la vista modello si aspetta di ricevere un **StoreIndexViewModel** istanza dei dati da utilizzare per generare una risposta HTML. Si noterà che il modello eredita un `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in c#.

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>Attività 5: aggiornamento dei modelli di visualizzazione

In questa attività, è possibile aggiornare il modello di visualizzazione creato nell'ultima attività per recuperare il numero di generi e i relativi nomi all'interno della pagina.

> [!NOTE]
> Si utilizzerà sintassi @ (noto anche come &quot;un elevato numero di codice&quot;) per eseguire codice all'interno del modello di visualizzazione.

1. Nel **cshtml** file, all'interno di **archivio** cartella, sostituire il codice con quanto segue:

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
2. Scorrere l'elenco in genere **StoreIndexViewModel** e creare un elemento HTML **&lt;ul&gt;** elenco tramite un **foreach** ciclo.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. Premere **F5** per eseguire l'applicazione e selezionare **e delle archiviazioni**. Verrà visualizzato l'elenco di generi passato il **StoreIndexViewModel** dall'oggetto di **StoreController** per il modello di visualizzazione.

    ![Vista che visualizza un elenco di generi](aspnet-mvc-4-fundamentals/_static/image26.png "vista che visualizza un elenco di generi")

    *Visualizza un elenco di generi di visualizzazione*
4. Chiudere il browser.

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>Esercizio 6: Utilizzo di parametri nella visualizzazione

Nell'esercizio 3 è stato descritto come passare i parametri per il Controller. In questo esercizio, si apprenderà come usare questi parametri nel modello di visualizzazione. A tale scopo, verranno presentate prima alle classi di modello che consentiranno di gestire la logica di dati e di dominio. Inoltre, si apprenderà come creare i collegamenti a pagine all'interno dell'applicazione ASP.NET MVC senza doversi preoccupare di elementi come i percorsi URL codifica.

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>Attività 1: aggiunta di classi modello

A differenza di ViewModel, che vengono creati solo per passare le informazioni dal Controller alla visualizzazione, le classi del modello vengono compilate per contenere e gestire la logica di dati e di dominio. In questa attività si aggiungeranno due classi di modello per rappresentare questi concetti: **Genre** e **Album**.

1. Se non è già aperta, avviare **Visual Studio Express per il Web**
2. Nel **File** menu, scegliere **Apri progetto**. Nella finestra di dialogo Apri progetto individuare **Source\Ex06 UsingParametersInView\Begin**selezionare **Begin.sln** e fare clic su **aprire**. In alternativa, è possibile continuare con la soluzione ottenuta dopo aver completato l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
3. Aggiungere un **Genre** classe del modello. A tale scopo, fare doppio clic sul **modelli** cartella il **Esplora**, selezionare **Aggiungi** e quindi il **nuovo elemento** opzione. In **codice**, scegliere il **classe** elemento e denominare il file *Genre.cs*, quindi fare clic su **Aggiungi**.

    ![Aggiunta di una classe](aspnet-mvc-4-fundamentals/_static/image27.png "aggiunta di una classe")

    *Aggiunta di un nuovo elemento*

    ![Aggiungere la classe di modello Genre](aspnet-mvc-4-fundamentals/_static/image28.png "aggiungere classe di modello Genre")

    *Aggiungere la classe di modello Genre*
4. Aggiungere un **nome** proprietà alla classe genere. A tale scopo, aggiungere il codice seguente:

    (- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Genre Ex6*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. Seguire la stessa procedura come prima, aggiungere un **Album** classe. A tale scopo, fare doppio clic sul **modelli** cartella il **Esplora**, selezionare **Aggiungi** e quindi il **nuovo elemento** opzione. In **codice**, scegliere il **classe** elemento e denominare il file *Album.cs*, quindi fare clic su **Aggiungi**.
6. Aggiungere due proprietà alla classe Album: **Genre** e **titolo**. A tale scopo, aggiungere il codice seguente:

    (- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Album Ex6*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>Attività 2: aggiunta di un StoreBrowseViewModel

Oggetto **StoreBrowseViewModel** verrà utilizzato in questa attività per visualizzare album che corrispondono a un genere selezionato. In questa attività verrà creare questa classe e quindi aggiungere due proprietà consentono di gestire il **Genre** e il relativo **Album**dell'elenco.

1. Aggiungere un **StoreBrowseViewModel** classe. A tale scopo, fare doppio clic sul **ViewModel** cartella la **Esplora**, selezionare **Aggiungi** e quindi il **nuovo elemento** opzione. In **codice**, scegliere il **classe** elemento e denominare il file *StoreBrowseViewModel.cs*, quindi fare clic su **Aggiungi**.
2. Aggiungere un riferimento ai modelli in **StoreBrowseViewModel** classe. A tale scopo, aggiungere il seguente utilizzo dello spazio dei nomi:

    (- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Ex6 UsingModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. Aggiungere le due proprietà **StoreBrowseViewModel** classe: **Genre** e **album**. A tale scopo, aggiungere il codice seguente:

    (- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Ex6 ModelProperties*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> Che cos'è **elenco&lt;Album&gt;**  ?: utilizza questa definizione di **elenco&lt;T&gt;**  tipo, in cui **T** vincola la agli elementi di questo tipo di **elenco** appartiene, in questo caso **Album** (o uno qualsiasi dei relativi discendenti).
> 
> La possibilità di progettare classi e metodi che rinviano la specifica di uno o più tipi fino a quando la classe o il metodo viene dichiarato e creare un'istanza dal codice client è una funzionalità del linguaggio c# denominata **Generics**.
> 
> **Elenco&lt;T&gt;**  equivale a generico di **ArrayList** tipo ed è disponibile nel **System.Collections.Generic** dello spazio dei nomi. Uno dei vantaggi dell'utilizzo di **generics** poiché il tipo è specificato, non necessario svolgere operazioni, ad esempio il cast degli elementi nel controllo del tipo **Album** come si farebbe con un **ArrayList**.

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>Attività 3: utilizzando il nuovo ViewModel di StoreController

In questa attività si modificherà il **StoreController**del **Sfoglia** e **dettagli** per utilizzare i nuovi metodi di azione **StoreBrowseViewModel** .

1. Aggiungere un riferimento alla cartella modelli in **StoreController** classe. A tale scopo, espandere il **controller** cartella il **Esplora** e aprire il **StoreController** classe. Quindi aggiungere il codice seguente:

    (- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Ex6 UsingModelInController*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. Sostituire il **Sfoglia** il metodo di azione per utilizzare il **StoreViewBrowseController** classe. Si creerà un genere e due nuovi oggetti album con dati fittizi (nel laboratorio pratico successiva si utilizzerà dati reali da un database). A tale scopo, sostituire il **Sfoglia** metodo con il codice seguente:

    (- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Ex6 BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. Sostituire il **dettagli** il metodo di azione per utilizzare il **StoreViewBrowseController** classe. Verrà creato un nuovo **Album** oggetto da restituire per il **vista**. A tale scopo, sostituire il **dettagli** metodo con il codice seguente:

    (- Frammento di codice *nozioni fondamentali su ASP.NET MVC 4 - Ex6 DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>Attività 4: aggiunta di un modello di visualizzazione esplorazione

In questa attività si aggiungerà un **Sfoglia** visualizzazione per mostrare gli album trovati per un genere specifico.

1. Prima di creare il nuovo modello di visualizzazione, è necessario compilare il progetto in modo che il **Aggiungi visualizzazione** finestra di dialogo viene a conoscenza di **ViewModel** classe da utilizzare. Compilare il progetto selezionando il **compilare** voce di menu e quindi **compilare MvcMusicStore**.
2. Aggiungere un **Sfoglia** visualizzazione. A tale scopo, fare clic del mouse il **Sfoglia** il metodo di azione del **StoreController** e fare clic su **Aggiungi visualizzazione**.
3. Nel **Aggiungi visualizzazione** finestra di dialogo, verificare che il nome della visualizzazione è **Sfoglia**. Controllare il **creare una visualizzazione fortemente tipizzata** casella di controllo e selezionare **StoreBrowseViewModel** dal **classe modello** elenco a discesa. Lasciare gli altri campi con il valore predefinito. Fare quindi clic su **Aggiungi**.

    ![Aggiunta di una vista di esplorazione](aspnet-mvc-4-fundamentals/_static/image29.png "aggiunta di una vista di esplorazione")

    *Aggiunta di una vista di esplorazione*
4. Modificare il **Browse.cshtml** per visualizzare le informazioni del genere, l'accesso di **StoreBrowseViewModel** oggetto passato per il modello di visualizzazione. A tale scopo, sostituire il contenuto con quanto segue: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Attività 5: esecuzione dell'applicazione

In questa attività, si verificherà che il **Sfoglia** che consente di recuperare gli album dal **Sfoglia** azione del metodo.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in   **/archiviare/Sfoglia? Genre = Disco** per verificare che l'azione restituisce due album.

    ![Esplorazione album Disco archivio](aspnet-mvc-4-fundamentals/_static/image30.png "esplorazione album Disco archivio")

    *Esplorazione album Disco archivio*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>Attività 6 - visualizzazione di informazioni su una specifica Album

In questa attività verrà implementata la **/dettagli archivio** visualizzazione per visualizzare informazioni su un album specifico. In questo laboratorio pratico, tutti gli elementi verranno visualizzati su album già inclusa nel **vista** modello. In questo caso, anziché creare un **StoreDetailsViewModel** (classe), si utilizzerà corrente **StoreBrowseViewModel** passando Album a tale modello.

1. Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio. Aggiungere un nuovo **dettagli** visualizzare per il **StoreController**del **dettagli** metodo di azione. A tale scopo, fare doppio clic su di **dettagli** metodo il **StoreController** classe e fare clic su **Aggiungi visualizzazione**.
2. Nel **Aggiungi visualizzazione** finestra di dialogo, verificare che il **nome vista** è **dettagli**. Controllare il **creare una visualizzazione fortemente tipizzata** casella di controllo e selezionare **Album** dal **classe modello** elenco a discesa. Lasciare gli altri campi con il valore predefinito. Fare quindi clic su **Aggiungi**. Verrà creata e aprire un **\Views\Store\Details.cshtml** file.

    ![Aggiunta di una visualizzazione dettagli](aspnet-mvc-4-fundamentals/_static/image31.png "aggiunta di una visualizzazione dettagli")

    *Aggiunta di una visualizzazione dettagli*
3. Modificare il **Details.cshtml** file per visualizzare le informazioni dell'Album, l'accesso di **Album** oggetto passato per il modello di visualizzazione. A tale scopo, sostituire il contenuto con quanto segue: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Attività 7: esecuzione dell'applicazione

In questa attività, si verificherà che il **dettagli** vista recupera le informazioni di Album dal **dettagli azione** metodo.

1. Premere **F5** per eseguire l'applicazione.
2. Avvia il progetto nel **Home** pagina. Modificare l'URL in **/Store/Details/5** per verificare le informazioni dell'album.

    ![Visualizzazione Dettagli album](aspnet-mvc-4-fundamentals/_static/image32.png "esplorazione album dettaglio")

    *Esplorazione dettaglio dell'Album*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>Attività 8: aggiunta di collegamenti tra le pagine

In questa attività si aggiungerà un collegamento nella visualizzazione archivio per disporre di un collegamento in nome di ogni genere appropriati **archivio/Sfoglia** URL. In questo modo, quando si fa clic su un genere, ad esempio **Disco**, verrà visualizzata **archivio/Sfoglia? genre = Disco** URL.

1. Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio. Aggiornamento di **indice** pagina per aggiungere un collegamento per il **Sfoglia** pagina. A tale scopo, nel **Esplora** espandere la **viste** cartella, il **archivio** cartella e fare doppio clic il **cshtml** pagina.
2. Aggiungere un collegamento alla visualizzazione Sfoglia che indica il genere selezionato. A tale scopo, sostituire il codice evidenziato di seguito all'interno di **&lt;li&gt;** tag: (c#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > un altro approccio potrebbe essere il collegamento direttamente alla pagina, con un codice simile al seguente:
   > 
   > &lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;
   > 
   > Sebbene questo approccio funziona, dipende dalla stringa hardcoded. Se in un secondo momento, si rinomina il Controller, sarà necessario modificare manualmente questa istruzione. Un'alternativa migliore consiste nell'usare un **HTML Helper** metodo. ASP.NET MVC include un metodo HTML Helper che è disponibile per tali attività. Il **Html.ActionLink()** metodo helper semplifica la compilazione HTML **&lt;un&gt;** collegamenti, assicurandosi che i percorsi URL siano URL correttamente codificati.
   > 
   > Htlm.ActionLink dispone di diversi overload. In questo esercizio si utilizzerà uno che accetta tre parametri:
   > 
   > 1. Testo del collegamento, che verrà visualizzato il nome di genere
   > 2. Nome dell'azione controller (**Sfoglia**)
   > 3. I valori di parametro, che specifica il nome di route (**Genre**) e il valore (**nome genere**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>Attività 9: esecuzione dell'applicazione

In questa attività si eseguirà il test con un collegamento a appropriato, verrà visualizzata ogni genere **archivio/Sfoglia** URL.

1. Premere **F5** per eseguire l'applicazione.
2. Il progetto viene avviato nella Home page. Modificare l'URL in **e delle archiviazioni** per verificare che ogni genere collegamenti appropriati **archivio/Sfoglia** URL.

    ![Esplorazione generi con collegamenti a pagina Sfoglia](aspnet-mvc-4-fundamentals/_static/image33.png "generi esplorazione con collegamenti a pagina Sfoglia")

    *Esplorazione generi con collegamenti a pagina Sfoglia*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>Attività 10 - tramite Raccolta ViewModel dinamica per passare i valori

In questa attività si apprenderà un metodo semplice e potente per passare i valori tra il Controller e la visualizzazione senza apportare modifiche nel modello. ASP.NET MVC 4 viene fornita la raccolta &quot;ViewModel&quot;, che può essere assegnato qualsiasi valore dinamico e si accede all'interno di controller e visualizzazioni nonché.

Verrà utilizzata la raccolta dinamica ViewBag per passare un elenco di &quot; **stella generi** &quot; dal controller alla visualizzazione. La visualizzazione dell'indice di archivio avrà accesso a **ViewModel** e visualizzare le informazioni.

1. Chiudere il browser, se necessario, per tornare alla finestra di Visual Studio. Aprire **StoreController.cs** e modificare **indice** metodo per creare un elenco di generi di stella in ViewModel raccolta:

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > È anche possibile usare la sintassi **ViewBag [&quot;Starred&quot;]** per accedere alle proprietà.
2. L'icona a stella **&quot;starred.png&quot;** è incluso nel **Source\Assets\Images** cartella di questa esercitazione. Per aggiungerlo all'applicazione, trascinare i propri contenuti da un **Esplora** finestra all'interno di **Esplora** in Visual Web Developer Express, come illustrato di seguito:

    ![Immagine di stella di aggiunta alla soluzione](aspnet-mvc-4-fundamentals/_static/image34.png "immagine di stella di aggiunta alla soluzione")

    *Immagine di stella aggiunta alla soluzione*
3. Aprire la visualizzazione **Store/Index.cshtml** e modificare il contenuto. È di lettura di &quot;stella&quot; proprietà il **ViewBag** insieme per verificare se il nome del genere corrente è nell'elenco. In questo caso saranno visualizzati un'icona a stella destra al collegamento di genere.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>Attività 11 - esecuzione dell'applicazione

In questa attività si testerà che generi i contrassegnata da indica visualizzino un'icona a stella.

1. Premere **F5** per eseguire l'applicazione.
2. Avvia il progetto nel **Home** pagina. Modificare l'URL in **e delle archiviazioni** per verificare che ogni genere in evidenza è l'etichetta respecting:

    ![Esplorazione generi con elementi contrassegnata da indica](aspnet-mvc-4-fundamentals/_static/image35.png "esplorazione generi con contrassegnata da indica elementi")

    *Esplorazione generi con contrassegnata da indica elementi*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>Esercizio 7: Un'introduzione ad ASP.NET MVC 4 nuovo modello

In questo esercizio verranno esplorati i miglioramenti nei modelli di progetto ASP.NET MVC 4, osservare le funzionalità più rilevanti del nuovo modello.

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>Attività 1: Esplorare il modello di applicazione ASP.NET MVC 4 Internet

1. Se non è già aperta, avviare **Visual Studio Express per il Web**
2. Selezionare il **File | New | Progetto** comando di menu. Nel **nuovo progetto** finestra di dialogo Seleziona il **Visual c# | Web** modello nel riquadro sinistro della struttura ad albero e scegliere il **applicazione Web ASP.NET MVC 4**. **Nome** il progetto *MusicStore* e aggiornare il **Nome soluzione** a *iniziare*, quindi selezionare un percorso (o lasciare il valore predefinito) e fare clic su **OK** .

    ![Creazione di un nuovo progetto ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "la creazione di un nuovo progetto ASP.NET MVC 4")

    *Creazione di un nuovo progetto ASP.NET MVC 4*
3. Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo Seleziona il **applicazione Internet** modello di progetto e fare clic su **OK**. Si noti che come il motore di visualizzazione, è possibile selezionare Razor o ASPX.

    ![Crea una nuova applicazione Internet ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image37.png "crea una nuova applicazione Internet ASP.NET MVC 4")

    *Crea una nuova applicazione Internet ASP.NET MVC 4*

    > [!NOTE]
    > La sintassi Razor è stato introdotto in ASP.NET MVC 3. L'obiettivo consiste nel ridurre al minimo il numero di caratteri e sequenze di tasti richieste in un file, consentendo un rapido e fluido codifica del flusso di lavoro. Razor sfrutta esistente C# /VB (o altro) linguistiche e offre una sintassi di modello che consente un straordinario HTML costruzione del flusso di lavoro.
4. Premere **F5** per eseguire la soluzione e visualizzare il modello rinnovato. È possibile controllare le funzionalità seguenti:

    1. **Modelli di stile moderno**

        I modelli sono stati rinnovati, fornendo più stili dall'aspetto moderno.

        ![Modelli ASP.NET MVC 4 modificato nello stile](aspnet-mvc-4-fundamentals/_static/image38.png "modificato nello stile modelli ASP.NET MVC 4")

        *Modelli ASP.NET MVC 4 modificato nello stile*
    2. **Rendering adattivo**

        Estrarre il ridimensionamento della finestra del browser e notare come il layout di pagina si adatta dinamicamente per le nuove dimensioni della finestra. Questi modelli utilizzano la tecnica di rendering adattivo per eseguire correttamente il rendering in piattaforme mobili e desktop senza personalizzazione.

        ![Modello di progetto ASP.NET MVC 4 in formati diversi browser](aspnet-mvc-4-fundamentals/_static/image39.png "il modello di progetto ASP.NET MVC 4 in formati diversi browser")

        *Modello di progetto ASP.NET MVC 4 in formati diversi browser*
5. Chiudere il browser per arrestare il debugger e tornare a Visual Studio.
6. Si è ora possibile esplorare la soluzione e verificare alcune delle nuove funzionalità introdotte da ASP.NET MVC 4 nel modello di progetto.

    ![ASP.NET MVC4-internet---modello di progetto applicazione](aspnet-mvc-4-fundamentals/_static/image40.png "il modello di progetto ASP.NET MVC 4 applicazione Internet")

    *Il modello di progetto ASP.NET MVC 4 applicazione Internet*

   1. **Tag HTML5**

       Sfogliare le viste di modello per individuare il markup nuovo tema, ad esempio aprire **About.cshtml** visualizzare all'interno di **Home** cartella.

       ![Nuovo modello, utilizzando il markup Razor e HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "nuovo modello, utilizzando il markup Razor e HTML5")

       *Nuovo modello, utilizzando il markup Razor e HTML5*
   2. **Librerie JavaScript inclusione**

      1. **jQuery**: jQuery semplifica l'attraversamento documento HTML, la gestione degli eventi, l'animazione e interazioni Ajax.
      2. **jQuery UI**: questa libreria fornisce astrazioni per applicazione widget basato sulla libreria JavaScript jQuery e animazione, effetti avanzate e interazione di basso livello.

         > [!NOTE]
         > È possibile acquisire informazioni jQuery e jQuery UI in [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).
      3. **Knockout.js**: modello predefinito di ASP.NET MVC 4 include ora **Knockout.js**, un framework JavaScript MVVM che consente di creare applicazioni web avanzate e ad alte scritte in JavaScript e HTML. Ad esempio in ASP.NET MVC 3, jQuery e jQuery librerie di interfaccia utente sono incluse anche in ASP.NET MVC 4.

          > [!NOTE]
          > È possibile ottenere ulteriori informazioni sulla libreria Knockout. js in questo collegamento: [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/).
      4. **Modernizr**: questa libreria viene eseguito automaticamente, rendendo il sito compatibile con browser meno recenti quando si utilizzano le tecnologie di HTML5 e CSS3.

          > [!NOTE]
          > È possibile ottenere ulteriori informazioni sulla libreria Modernizr in questo collegamento: [ http://www.modernizr.com/ ](http://www.modernizr.com/).
   3. **SimpleMembership inclusi nella soluzione**

       SimpleMembership è stato progettato come sostituzione per il sistema di provider di ruoli ASP.NET e appartenenza precedente. Include molte nuove funzionalità che rendono più semplice per lo sviluppatore a pagine web protette in modo più flessibile.

       Il modello Internet già dispone di configurare alcuni aspetti da integrare SimpleMembership, ad esempio, il AccountController è pronto per l'utilizzo OAuthWebSecurity (per la registrazione dell'account OAuth, account di accesso, gestione e così via) e la sicurezza del Web.

       ![SimpleMembership inclusi nella soluzione](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership inclusi nella soluzione")

       *SimpleMembership inclusi nella soluzione*

       > [!NOTE]
       > Ulteriori informazioni su [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.

> [!NOTE]
> Inoltre, è possibile distribuire l'applicazione per siti Web di Azure seguenti [pubblicazione appendice b: un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa pratica si è appreso le nozioni di base di ASP.NET MVC:

- Elementi di base di un'applicazione MVC e come interagiscono
- Come creare un'applicazione MVC ASP.NET
- Come aggiungere e configurare i controller per gestire i parametri passati tramite l'URL e la stringa di query
- Come aggiungere una pagina master layout per la configurazione di un modello di contenuto HTML comune, un foglio di stile per migliorare l'aspetto e un modello di visualizzazione per visualizzare il contenuto HTML
- Come utilizzare il modello ViewModel per passare le proprietà per il modello di visualizzazione per visualizzare informazioni dinamiche
- Come utilizzare i parametri passati ai controller nel modello di visualizzazione
- Come aggiungere collegamenti a pagine all'interno dell'applicazione ASP.NET MVC
- Come aggiungere e utilizzare le proprietà dinamiche in una vista
- I miglioramenti nei modelli di progetto ASP.NET MVC 4

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice a: installazione di Visual Studio Express 2012 per Web

È possibile installare **Microsoft Visual Studio Express 2012 per Web** o un altro &quot;Express&quot; versione utilizzando il **[installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web di Microsoft*.

1. Passare a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **installa**. Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.
3. Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.

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
7. Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per Web, passare al **avviare** schermata e iniziare la scrittura &quot; **VS Express**&quot;, quindi fare clic su di **Visual Studio Express per Web** il riquadro.

    ![VS Express per il riquadro Web](aspnet-mvc-4-fundamentals/_static/image47.png)

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

    ![Accedere al portale Windows Azure](aspnet-mvc-4-fundamentals/_static/image48.png "accedere al portale Windows Azure")

    *Accedere al portale di gestione di Azure*
2. Fare clic su **nuovo** sulla barra dei comandi.

    ![Creazione di un nuovo sito Web](aspnet-mvc-4-fundamentals/_static/image49.png "creando un nuovo sito Web")

    *Creazione di un nuovo sito Web*
3. Fare clic su **calcolo** | **sito Web**. Selezionare quindi **creazione rapida** opzione. Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.

    > [!NOTE]
    > Un sito Web di Windows Azure è l'host per un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire. L'opzione Creazione rapida consente di distribuire un'applicazione web completa per il sito Web Microsoft Azure dall'esterno al portale. Non include i passaggi per configurare un database.

    ![Creazione di un nuovo sito Web utilizzando Creazione rapida](aspnet-mvc-4-fundamentals/_static/image50.png "creando un nuovo sito Web utilizzando Creazione rapida")

    *Creazione di un nuovo sito Web utilizzando Creazione rapida*
4. Attendere finché il nuovo **sito Web** viene creato.
5. Una volta creato il sito Web, fare clic sul collegamento sotto il **URL** colonna. Verificare il funzionamento del nuovo sito Web.

    ![Esplorazione per il nuovo sito web](aspnet-mvc-4-fundamentals/_static/image51.png "esplorazione per il nuovo sito web")

    *Esplorazione per il nuovo sito web*

    ![Sito Web in esecuzione](aspnet-mvc-4-fundamentals/_static/image52.png "sito Web in esecuzione")

    *Sito Web in esecuzione*
6. Tornare al portale e fare clic sul nome del sito web sotto il **nome** colonna per visualizzare le pagine di gestione.

    ![Aprire le pagine di gestione del sito web](aspnet-mvc-4-fundamentals/_static/image53.png "apertura delle pagine di gestione del sito web")

    *Aprire le pagine di gestione del sito Web*
7. Nel **Dashboard** nella pagina di **riepilogo rapido** fare clic su di **Scarica profilo di pubblicazione** collegamento.

    > [!NOTE]
    > Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in un sito Web di Windows Azure per ogni metodo di pubblicazione abilitato. Il profilo di pubblicazione contiene gli URL, le credenziali dell'utente e le stringhe di database necessari per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di queste applicazioni per pubblicazione di applicazioni web per siti Web di Windows Azure.

    ![Profilo di pubblicazione del sito web di download](aspnet-mvc-4-fundamentals/_static/image54.png "profilo di pubblicazione del sito web di download")

    *Profilo di pubblicazione del sito Web di download*
8. Scaricare il file del profilo di pubblicazione in un percorso noto. In questo esercizio si ulteriormente verrà visualizzato come utilizzare questo file per pubblicare un'applicazione web a un siti Web di Windows Azure da Visual Studio.

    ![Salvare il file del profilo di pubblicazione](aspnet-mvc-4-fundamentals/_static/image55.png "il salvataggio del profilo di pubblicazione")

    *Salvare il file del profilo di pubblicazione*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Attività 2: configurare il Server di Database

Se l'applicazione viene utilizzato SQL Server database, è necessario creare un server di Database SQL. Se si desidera distribuire un'applicazione semplice che non utilizza SQL Server è possibile ignorare questa attività.

1. È necessario un server di Database SQL per archiviare il database dell'applicazione. È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure all'indirizzo **database Sql** | **server** | **del Server Dashboard**. Se non si dispone di un server creato, è possibile creare tramite il **Aggiungi** pulsante sulla barra dei comandi. Annotare il **nome server e URL, il nome di accesso di amministratore e la password**, come verranno utilizzati in operazioni successive. Non creare il database, come verrà creato in una fase successiva.

    ![Dashboard del Server Database SQL](aspnet-mvc-4-fundamentals/_static/image56.png "Dashboard del Server Database SQL")

    *Dashboard del Server Database SQL*
2. Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server di **indirizzi IP consentiti**. A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP da **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo e fare clic su di ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) pulsante.

    ![Aggiunta indirizzo IP del Client](aspnet-mvc-4-fundamentals/_static/image58.png)

    *Aggiunta indirizzo IP del Client*
3. Una volta il **indirizzo IP del Client** viene aggiunto agli indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.

    ![Confermare le modifiche](aspnet-mvc-4-fundamentals/_static/image59.png)

    *Confermare le modifiche*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web

1. Tornare alla soluzione ASP.NET MVC 4. Nel **Esplora**, fare clic sul progetto sito web e selezionare **pubblica**.

    ![La pubblicazione dell'applicazione](aspnet-mvc-4-fundamentals/_static/image60.png "la pubblicazione dell'applicazione")

    *Pubblicazione del sito web*
2. Importare il profilo di pubblicazione che è stato salvato nella prima attività.

    ![L'importazione del profilo di pubblicazione](aspnet-mvc-4-fundamentals/_static/image61.png "l'importazione del profilo di pubblicazione")

    *L'importazione di profilo di pubblicazione*
3. Fare clic su **convalidare la connessione**. Una volta completata la convalida, fare clic su **Avanti**.

    > [!NOTE]
    > Dopo aver visualizzato un segno di spunta verde visualizzata accanto al pulsante convalida connessione, la convalida è stata completata.

    ![Convalida della connessione](aspnet-mvc-4-fundamentals/_static/image62.png "convalida della connessione")

    *Convalida della connessione*
4. Nel **impostazioni** nella pagina il **database** fare clic sul pulsante accanto casella di testo della connessione di database (ad esempio **DefaultConnection**).

    ![Configurazione della distribuzione Web](aspnet-mvc-4-fundamentals/_static/image63.png "configurazione della distribuzione Web")

    *Configurazione della distribuzione Web*
5. Configurare la connessione al database come segue:

   - Nel **nome Server** digitare l'URL server di Database SQL utilizzando il *tcp:* prefisso.
   - In **nome utente** digitare il nome di accesso di amministratore di server.
   - In **Password** digitare la password dell'account di accesso amministratore server.
   - Digitare un nuovo nome di database, ad esempio: *MVC4SampleDB*.

     ![Configurazione di stringa di connessione di destinazione](aspnet-mvc-4-fundamentals/_static/image64.png "configurazione stringa di connessione di destinazione")

     *Configurazione di stringa di connessione di destinazione*
6. Fare quindi clic su **OK**. Quando viene richiesto di creare il database fare clic su **Sì**.

    ![Creazione del database](aspnet-mvc-4-fundamentals/_static/image65.png "creazione della stringa di database")

    *Creazione del database*
7. La stringa di connessione che si utilizzerà per connettersi al Database SQL di Windows Azure viene visualizzata all'interno di casella di testo di connessione predefinito. Scegliere quindi **Avanti**.

    ![Stringa di connessione che punta al Database SQL](aspnet-mvc-4-fundamentals/_static/image66.png "stringa di connessione che punta al Database SQL")

    *Stringa di connessione che punta al Database SQL*
8. Nel **anteprima** pagina, fare clic su **pubblica**.

    ![Pubblicare l'applicazione web](aspnet-mvc-4-fundamentals/_static/image67.png "pubblicare l'applicazione web")

    *Pubblicare l'applicazione web*
9. Una volta terminato il processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.

    ![Applicazione pubblicata in Azure](aspnet-mvc-4-fundamentals/_static/image68.png "applicazione pubblicata in Windows Azure")

    *Applicazione pubblicata in Windows Azure*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Appendice c: utilizzo dei frammenti di codice

Con i frammenti di codice, si dispone di tutto il codice che necessario a portata di mano. Il documento lab consentirà di determinare esattamente quando è possibile utilizzarli, come illustrato nella figura seguente.

![Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto](aspnet-mvc-4-fundamentals/_static/image69.png "frammenti di codice tramite Visual Studio per inserire codice nel progetto")

*Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto*

***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***

1. Posizionare il cursore dove si desidera inserire il codice.
2. Iniziare a digitare il nome del frammento di codice (senza spazi o segni meno).
3. Osservare come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).
5. Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.

![Iniziare a digitare il nome del frammento di codice](aspnet-mvc-4-fundamentals/_static/image70.png "inizia a digitare il nome del frammento di codice")

*Iniziare a digitare il nome del frammento di codice*

![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-fundamentals/_static/image71.png "premere Tab per selezionare il frammento evidenziato")

*Premere Tab per selezionare il frammento di codice evidenziata*

![Premere Tab nuovamente e il frammento di codice verranno espansi](aspnet-mvc-4-fundamentals/_static/image72.png "espanderà premere Tab nuovamente e il frammento di codice")

*Premere Tab nuovamente e il frammento di codice verranno espansi*

***Per aggiungere un frammento di codice utilizzando il mouse (c#, Visual Basic e XML)*** 1. Fare clic in cui si desidera inserire il frammento di codice.

1. Selezionare **Inserisci frammento di codice** seguito da **frammenti di codice**.
2. Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.

![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice](aspnet-mvc-4-fundamentals/_static/image73.png "rapida in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice")

*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice*

![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-fundamentals/_static/image74.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso")

*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso*
