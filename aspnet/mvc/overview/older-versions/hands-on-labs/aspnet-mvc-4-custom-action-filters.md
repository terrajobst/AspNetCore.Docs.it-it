---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Filtri azione personalizzati di ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC fornisce i filtri di azione per l'esecuzione di logica di filtro prima o dopo che viene chiamato un metodo di azione. I filtri azione sono gli attributi personalizzati che...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 0170fda6849c1dfb53b44908ea55ba2cad0dd067
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832194"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>Filtri azione personalizzati di ASP.NET MVC 4

da [Camp Web Team](https://twitter.com/webcamps)

[Download Web Camp Kit di formazione](https://aka.ms/webcamps-training-kit)

ASP.NET MVC fornisce i filtri di azione per l'esecuzione di logica di filtro prima o dopo che viene chiamato un metodo di azione. I filtri azione sono gli attributi personalizzati che forniscono un modo per aggiungere un comportamento pre-azione e post-azione ai metodi di azione del controller.

In questo laboratorio pratico si creerà un attributo di filtro azione personalizzato nella soluzione MvcMusicStore per intercettare le richieste del controller e registrare l'attività di un sito in una tabella di database. Sarà possibile aggiungere il filtro di registrazione tramite l'inserimento di qualsiasi controller o azione. Infine, si noterà che la visualizzazione di log che mostra l'elenco di visitatori.

Questa pratica si presuppone conoscenze di base **ASP.NET MVC**. Se non è stato utilizzato **ASP.NET MVC** in precedenza, è consigliabile esaminare **concetti di base di ASP.NET MVC 4** laboratorio pratico.

> [!NOTE]
> Tutto il codice di esempio e frammenti di codice sono inclusi nel Web Camp Kit di formazione, disponibile in [Microsoft-Web/WebCampTrainingKit versioni](https://aka.ms/webcamps-training-kit). Il progetto specifico per questo lab è disponibile all'indirizzo [filtri azione personalizzati di ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico, si apprenderà come:

- Creare un attributo di filtro azione personalizzato per estendere le funzionalità di filtro
- Applicare un attributo di filtro personalizzato tramite l'inserimento di un livello specifico
- Registrare un filtri azione personalizzati a livello globale

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

1. [Esercizio 1: La registrazione delle azioni](#Exercise1)
2. [Esercizio 2: Gestione di più filtri azione](#Exercise2)

Tempo stimato per completare questa esercitazione: **30 minuti**.

> [!NOTE]
> Ogni esercizio è accompagnato da un **End** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato gli esercizi. Se ti serve assistenza aggiuntiva esaminando gli esercizi, è possibile usare questa soluzione come guida.


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>Esercizio 1: La registrazione delle azioni

In questo esercizio, si apprenderà come creare un filtro azione personalizzata per il log tramite provider di filtri di ASP.NET MVC 4. A tale scopo si applicherà un filtro di registrazione al sito MusicStore che registrerà tutte le attività nei controller selezionato.

Il filtro estenderà **ActionFilterAttributeClass** ed eseguire l'override **OnActionExecuting** metodo rilevi ogni richiesta e quindi eseguire le azioni di registrazione. Le informazioni di contesto su richieste HTTP, l'esecuzione di metodi, i risultati e i parametri verrà fornita da ASP.NET MVC **ActionExecutingContext** classe **.**

> [!NOTE]
> ASP.NET MVC 4 include anche provider di filtri predefiniti è possibile usare senza creare un filtro personalizzato. ASP.NET MVC 4 fornisce i tipi di filtri seguenti:
> 
> - **Autorizzazione** filtrare, rendendo decisioni relative alla sicurezza sulla possibilità di eseguire un metodo di azione, ad esempio l'autenticazione o la convalida delle proprietà della richiesta.
> - **Azione** filtro che esegue il wrapping dell'esecuzione del metodo azione. Questo filtro può eseguire un'ulteriore elaborazione, ad esempio fornire dati aggiuntivi al metodo di azione, controllare il valore restituito o annullare l'esecuzione del metodo di azione
> - **Risultato** filtro che esegue il wrapping di esecuzione dell'oggetto ActionResult. Questo filtro è possibile eseguire un'ulteriore elaborazione del risultato, ad esempio la modifica della risposta HTTP.
> - **Eccezione** filtro, che viene eseguita se si verifica un'eccezione non gestita generata in un punto nel metodo di azione, inizia con i filtri di autorizzazione e termina l'esecuzione del risultato. I filtri eccezioni sono utilizzabile per attività quali la registrazione o la visualizzazione di una pagina di errore.
> 
> Per altre informazioni sui provider di filtri, visitare il sito Web MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>Informazioni sulla funzionalità di registrazione MVC Music Store applicazioni

Questa soluzione di Music Store è una nuova tabella di modello di dati per la registrazione del sito **ActionLog**, con i campi seguenti: nome del controller che ha ricevuto una richiesta, azione di chiamate, indirizzo IP del Client e timestamp.

![Modello di dati. Tabella ActionLog. ](aspnet-mvc-4-custom-action-filters/_static/image1.png "Modello di dati. Tabella ActionLog.")

*Modello di dati - ActionLog tabella*

La soluzione fornisce una visualizzazione MVC ASP.NET per il log delle azioni che può essere trovato in **MvcMusicStores/viste/ActionLog**:

![Visualizzazione di Log di azione](aspnet-mvc-4-custom-action-filters/_static/image2.png "Visualizza registro azioni")

*Visualizzazione del Log azioni*

Con questa struttura di data, tutte le operazioni si concentrerà sulle interrompere le richieste del controller ed eseguire la registrazione tramite il filtro personalizzato.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>Attività 1: creazione di un filtro personalizzato per rilevare richieste del Controller

In questa attività si creerà una classe di attributo di filtro personalizzato che conterrà la logica di registrazione. A tale scopo verrà estesa in ASP.NET MVC **ActionFilterAttribute** classe e implementare l'interfaccia **IActionFilter**.

> [!NOTE]
> Il **ActionFilterAttribute** è la classe base per tutti i filtri di attributo. Fornisce i metodi seguenti per eseguire una logica specifica dopo e prima dell'esecuzione dell'azione del controller:
> 
> - **OnActionExecuting**(ActionExecutingContext filterContext): appena prima dell'azione di metodo viene chiamato.
> - **OnActionExecuted**(ActionExecutingContext filterContext): dopo la chiamata del metodo di azione e prima che il risultato viene eseguito (prima della visualizzazione render).
> - **OnResultExecuting**(ResultExecutingContext filterContext): solo prima che venga eseguito il risultato (prima della visualizzazione render).
> - **OnResultExecuted**(ResultExecutedContext filterContext): dopo l'esecuzione del risultato (dopo il rendering).
> 
> Eseguendo l'override di uno di questi metodi in una classe derivata, è possibile eseguire il proprio codice di filtro.


1. Aprire il **Begin** soluzione disponibile all'indirizzo **\Source\Ex01-LoggingActions\Begin** cartella.

   1. È necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
      > 
      > Per altre informazioni, vedere questo articolo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Aggiungere una nuova classe c# nel **filtri** cartella e denominarlo *CustomActionFilter.cs*. Questa cartella archivierà tutti i filtri personalizzati.
3. Aprire **CustomActionFilter.cs** e aggiungere un riferimento a **System** e **MvcMusicStore.Models** gli spazi dei nomi:

    (Code - Snippet *filtri azione personalizzati di ASP.NET MVC 4 - Ex1 CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. Ereditano le **CustomActionFilter** classe **ActionFilterAttribute** e quindi apportare **CustomActionFilter** implementano **IActionFilter** interfaccia.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. Rendere **CustomActionFilter** classe eseguire l'override del metodo **OnActionExecuting** e aggiungere la logica necessaria per registrare l'esecuzione del filtro. A questo scopo, aggiungere il codice evidenziato seguente all'interno **CustomActionFilter** classe.

    (Code - Snippet *filtri azione personalizzati di ASP.NET MVC 4 - Ex1 LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > **OnActionExecuting** metodo consiste nell'usare **Entity Framework** per aggiungere un nuovo registro ActionLog. Crea e inserisce una nuova istanza di entità con le informazioni di contesto provenienti **filterContext**.
    > 
    > Altre informazioni, vedere **ControllerContext** classe [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>Attività 2: inserimento di un intercettore di codice nella classe Controller Store

In questa attività si aggiungerà il filtro personalizzato inserendo, tutte le classi controller e azioni del controller che verranno registrate. Ai fini di questo esercizio, la classe Controller Store avrà un log.

Il metodo **OnActionExecuting** dalla **ActionLogFilterAttribute** filtro personalizzato viene eseguito quando viene chiamato un elemento inserito.

È anche possibile intercettare un metodo del controller specifico.

1. Aprire il **StoreController** alla **MvcMusicStore\Controllers** e aggiungere un riferimento al **filtri** dello spazio dei nomi:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. Inserire il filtro personalizzato **CustomActionFilter** nelle **StoreController** classe aggiungendo **[CustomActionFilter]** attributo prima della dichiarazione di classe.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > Quando un filtro viene inserito in una classe controller, vengono inseriti anche tutte le relative azioni. Se si desidera applicare il filtro solo per un set di azioni, è necessario inserire **[CustomActionFilter]** a ciascuno di essi:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Attività 3: esecuzione dell'applicazione

In questa attività si testerà il funzionamento di filtro di registrazione. Si verrà avviare l'applicazione e visitare l'archivio e quindi si controllerà le attività registrate.

1. Premere **F5** per eseguire l'applicazione.
2. Passare a **/ActionLog** per visualizzare lo stato iniziale di visualizzazione log:

    ![Registra lo stato di arresto prima dell'attività di pagina](aspnet-mvc-4-custom-action-filters/_static/image3.png "registra lo stato di arresto prima dell'attività di pagina")

    *Stato di arresto di log prima dell'attività di pagina*

   > [!NOTE]
   > Per impostazione predefinita, verrà sempre visualizzato un unico elemento che viene generato quando si recuperano generi esistente per il menu di scelta.
   > 
   > Per motivi di semplicità stiamo sottoponendo a pulizia le **ActionLog** tabella ogni volta che l'applicazione viene eseguita in modo che mostrerà solo i log di verifica dell'attività ogni particolare.
   > 
   > Potrebbe essere necessario rimuovere il codice seguente dal **sessione\_avviare** metodo (nel **Global. asax** classe), in modo da salvare un log cronologico per tutte le azioni eseguite all'interno di Store Controller.
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. Fare clic su uno dei **generi** dal menu di scelta ed eseguire alcune azioni, ad esempio la navigazione di un album disponibile.
4. Passare a **/ActionLog** e se il log è premere vuota **F5** per aggiornare la pagina. Verificare che sono state rilevate le visite:

    ![Log delle azioni con attività registrate](aspnet-mvc-4-custom-action-filters/_static/image4.png "log azioni con attività registrate")

    *Log delle azioni con attività registrate*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>Esercizio 2: Gestione di più filtri azione

In questo esercizio verrà aggiungere un secondo filtro azioni personalizzato alla classe StoreController e definire l'ordine specifico in cui verranno eseguiti entrambi i filtri. Si aggiornerà quindi il codice per registrare il filtro a livello globale.

Sono disponibili diverse opzioni per prendere in considerazione quando si definisce l'ordine di esecuzione dei filtri. Ad esempio, la proprietà dell'ordine e ambito dei filtri:

È possibile definire un **ambito** per ognuno dei filtri, ad esempio, è possibile definire l'ambito tutti i filtri dell'azione da eseguire all'interno di **ambito Controller**e tutti i filtri di autorizzazione per l'esecuzione in **ambito globale** . Gli ambiti di avere un ordine di esecuzione definito.

Inoltre, ogni filtro azioni presenta una proprietà dell'ordine che viene usata per determinare l'ordine di esecuzione nell'ambito del filtro.

Per altre informazioni sull'ordine di esecuzione filtri azione personalizzati, vedere questo articolo di MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>Attività 1: Creazione di un nuovo filtro azioni personalizzato

In questa attività si creerà un nuovo filtro azioni personalizzato per inserire la classe StoreController, imparare a gestire l'ordine di esecuzione dei filtri.

1. Aprire il **Begin** soluzione disponibile all'indirizzo **\Source\Ex02-ManagingMultipleActionFilters\Begin** cartella. In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.

    1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
    2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
    3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

        > [!NOTE]
        > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
        > 
        > Per altre informazioni, vedere questo articolo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Aggiungere una nuova classe c# nel **filtri** cartella e denominarlo *MyNewCustomActionFilter.cs*
3. Aprire **MyNewCustomActionFilter.cs** e aggiungere un riferimento a **System** e il **MvcMusicStore.Models** dello spazio dei nomi:

    (Code - Snippet *filtri azione personalizzati di ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. Sostituire la dichiarazione di classe predefinita con il codice seguente.

    (Code - Snippet *filtri azione personalizzati di ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > Questo filtro azioni personalizzato è quasi uguale a quella creata nell'esercizio precedente. La differenza principale è che ha il *&quot;registrate dal&quot;* attributo aggiornato con il nome di questa nuova classe per identificare il filtro registrato nel log.

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>Attività 2: Inserire un nuovo intercettore di codice nella classe StoreController

In questa attività è verranno aggiunge un nuovo filtro personalizzato nella classe StoreController ed eseguire la soluzione per verificare l'interagiscono come entrambi i filtri.

1. Aprire il **StoreController** classe disponibile all'indirizzo **MvcMusicStore\Controllers** e inserire il nuovo filtro personalizzato **MyNewCustomActionFilter** in  **StoreController** classe, ad esempio è illustrata nel codice seguente.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. A questo punto, eseguire l'applicazione per vedere come funzionano queste due filtri azione personalizzati. A tale scopo, premere **F5** e attendere l'avvio dell'applicazione.
3. Passare a **/ActionLog** per visualizzare lo stato iniziale di visualizzazione di log.

    ![Registra lo stato di arresto prima dell'attività di pagina](aspnet-mvc-4-custom-action-filters/_static/image5.png "registra lo stato di arresto prima dell'attività di pagina")

    *Stato di arresto di log prima dell'attività di pagina*
4. Fare clic su uno dei **generi** dal menu di scelta ed eseguire alcune azioni, ad esempio la navigazione di un album disponibile.
5. Verificare che questa fase. le visite è sono rilevate due volte: una volta per ciascuno dei filtri delle azioni personalizzato aggiunto nel **controller di archiviazione** classe.

    ![Log delle azioni con attività registrate](aspnet-mvc-4-custom-action-filters/_static/image6.png "log azioni con attività registrate")

    *Log delle azioni con attività registrate*
6. Chiudere il browser.

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>Attività 3: Gestione di ordinamento di filtro

In questa attività si apprenderà come gestire l'ordine di esecuzione dei filtri utilizzando la proprietà dell'ordine.

1. Aprire il **StoreController** classe disponibile all'indirizzo **MvcMusicStore\Controllers** e specificare il **ordine** proprietà in entrambi i filtri, ad esempio illustrato di seguito.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. A questo punto, verificare come vengono eseguiti i filtri a seconda del valore della proprietà relativa dell'ordine. Si noterà che il filtro con il valore dell'ordine più piccolo (**CustomActionFilter**) è il primo che viene eseguito. Premere **F5** e attendere l'avvio dell'applicazione.
3. Passare a **/ActionLog** per visualizzare lo stato iniziale di visualizzazione di log.

    ![Registra lo stato di arresto prima dell'attività di pagina](aspnet-mvc-4-custom-action-filters/_static/image7.png "registra lo stato di arresto prima dell'attività di pagina")

    *Stato di arresto di log prima dell'attività di pagina*
4. Fare clic su uno dei **generi** dal menu di scelta ed eseguire alcune azioni, ad esempio la navigazione di un album disponibile.
5. Verificare che questa volta, sono state rilevate le visite ordinati in base al valore dell'ordine dei filtri: **CustomActionFilter** registri prima.

    ![Log delle azioni con attività registrate](aspnet-mvc-4-custom-action-filters/_static/image8.png "log azioni con attività registrate")

    *Log delle azioni con attività registrate*
6. A questo punto, aggiornare il valore dell'ordine dei filtri e verificare come cambia l'ordine di registrazione. Nel **StoreController** classe, aggiornare il valore di ordine dei filtri, come illustrato di seguito.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. Eseguire nuovamente l'applicazione premendo **F5**.
8. Fare clic su uno dei **generi** dal menu di scelta ed eseguire alcune azioni, ad esempio la navigazione di un album disponibile.
9. Verificare che questo periodo, i log creati da **MyNewCustomActionFilter** filtro viene visualizzato per primo.

    ![Log delle azioni con attività registrate](aspnet-mvc-4-custom-action-filters/_static/image9.png "log azioni con attività registrate")

    *Log delle azioni con attività registrate*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>Attività 4: La registrazione di filtri a livello globale

In questa attività si aggiornerà la soluzione per registrare il nuovo filtro (**MyNewCustomActionFilter**) come filtro globale. In questo modo, si verrà attivato per tutte le eseguite le azioni nell'applicazione e non solo di quelli StoreController come nell'attività precedente.

1. Nelle **StoreController** classe, rimuovere **[MyNewCustomActionFilter]** attributo e la proprietà dell'ordine dalla **[CustomActionFilter]**. Dovrebbe essere simile al seguente:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. Open **Global. asax** del file e individuare il **Application\_avviare** (metodo). Si noti che ogni volta che viene avviata l'applicazione registra i filtri globali chiamando **RegisterGlobalFilters** metodo interno **FilterConfig** classe.

    ![La registrazione di filtri globali in Global. asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "la registrazione di filtri globali in Global. asax")

    *La registrazione di filtri globali in Global. asax*
3. Aprire **FilterConfig.cs** all'interno del file **App\_avviare** cartella.
4. Aggiungere un riferimento all'uso di System; usando MvcMusicStore.Filters; spazio dei nomi.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. Update **RegisterGlobalFilters** metodo aggiungendo il filtro personalizzato. A tale scopo, aggiungere il codice evidenziato:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. Eseguire l'applicazione premendo **F5**.
7. Fare clic su uno dei **generi** dal menu di scelta ed eseguire alcune azioni, ad esempio la navigazione di un album disponibile.
8. Verificare che ora **[MyNewCustomActionFilter]** è che venga introdotto in HomeController e ActionLogController troppo.

    ![Log delle azioni con attività registrate](aspnet-mvc-4-custom-action-filters/_static/image11.png "log azioni con attività registrate")

    *Log delle azioni con l'attività globale registrato*

> [!NOTE]
> Inoltre, è possibile distribuire questa applicazione per siti Web di Azure seguenti [appendice b: pubblicazione un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa pratica si è appreso come estendere un filtro azione per eseguire azioni personalizzate. Si è appreso anche come inserire i filtri ai controller di pagina. Sono stati usati i concetti seguenti:

- Come creare filtri azione personalizzati con la classe ActionFilterAttribute MVC ASP.NET
- Come inserire i filtri nel controller ASP.NET MVC
- Come gestire filtro ordinamento tramite la proprietà dell'ordine
- Come registrare i filtri a livello globale

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice a: installazione di Visual Studio Express 2012 per Web

È possibile installare **Microsoft Visual Studio Express 2012 per Web** o da un'altra &quot;Express&quot; versione utilizzando il **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web Microsoft*.

1. Passare a [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e cercare il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **Installa ora**. Se non hai **instalace Webové Platformy** si verrà reindirizzati per scaricarlo e installarlo prima di tutto.
3. Una volta **instalace Webové Platformy** è aperto, fare clic su **installare** per avviare il programma di installazione.

    ![Installa Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "installa Visual Studio Express")

    *Installa Visual Studio Express*
4. Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.

    ![Accettare le condizioni di licenza](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *Accettare le condizioni di licenza*
5. Attendere finché non viene completato il processo di download e l'installazione.

    ![Stato dell'installazione](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *Installazione completata*
7. Fare clic su **Exit** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per Web, fare clic per il **avviare** schermata e iniziare la scrittura &quot; **Visual Studio Express**&quot;, quindi fare clic sul **Visual Studio Express per Web** il riquadro.

    ![Visual Studio Express per il riquadro Web](aspnet-mvc-4-custom-action-filters/_static/image16.png)

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

    ![Accedere al portale di Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "accedere al portale di Azure")

    *Accedere al portale di gestione di Azure*
2. Fare clic su **New** sulla barra dei comandi.

    ![Creazione di un nuovo sito Web](aspnet-mvc-4-custom-action-filters/_static/image18.png "creando un nuovo sito Web")

    *Creazione di un nuovo sito Web*
3. Fare clic su **Compute** | **sito Web**. Quindi selezionare **creazione rapida** opzione. Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.

    > [!NOTE]
    > Un sito Web di Microsoft Azure è l'host per un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire. L'opzione Creazione rapida consente di distribuire un'applicazione web completa per il sito Web Microsoft Azure all'esterno del portale. Non include i passaggi per la configurazione di un database.

    ![Creazione di un nuovo sito Web utilizzando Creazione rapida](aspnet-mvc-4-custom-action-filters/_static/image19.png "creando un nuovo sito Web mediante Creazione rapida")

    *Creazione di un nuovo sito Web utilizzando Creazione rapida*
4. Attendere finché il nuovo **sito Web** viene creato.
5. Dopo aver creato il sito Web fare clic sul collegamento sotto i **URL** colonna. Verificare che il nuovo sito Web sia in funzione.

    ![Passare al nuovo sito web](aspnet-mvc-4-custom-action-filters/_static/image20.png "passare al nuovo sito web")

    *Passare al nuovo sito web*

    ![Sito Web in esecuzione](aspnet-mvc-4-custom-action-filters/_static/image21.png "sito Web in esecuzione")

    *Sito Web in esecuzione*
6. Tornare al portale e fare clic sul nome del sito web con il **nome** colonna per visualizzare le pagine di gestione.

    ![Aprire le pagine di gestione sito web](aspnet-mvc-4-custom-action-filters/_static/image22.png "aprire le pagine di gestione sito web")

    *Aprire le pagine di gestione sito Web*
7. Nel **Dashboard** nella pagina il **riepilogo rapido** fare clic sui **Scarica profilo di pubblicazione** collegamento.

    > [!NOTE]
    > Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in un sito Web di Azure per ogni metodo di pubblicazione abilitato. Il profilo di pubblicazione contiene l'URL, le credenziali dell'utente e stringhe di database necessarie per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di questi programmi per pubblicazione di applicazioni web in siti Web di Windows Azure.

    ![Download del sito web di profilo di pubblicazione](aspnet-mvc-4-custom-action-filters/_static/image23.png "scaricando il sito web di profilo di pubblicazione")

    *Profilo di pubblicazione scaricato il sito Web*
8. Scaricare il file di profilo di pubblicazione in una posizione nota. In questo esercizio ulteriormente si vedrà come usare questo file per pubblicare un'applicazione web a un siti Web di Windows Azure da Visual Studio.

    ![Salvare il file di profilo di pubblicazione](aspnet-mvc-4-custom-action-filters/_static/image24.png "salvataggio del profilo di pubblicazione")

    *Salvare il file di profilo di pubblicazione*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Attività 2: configurare il Server di Database

Se l'applicazione Usa SQL Server database è necessario creare un Database di SQL server. Se si vuole distribuire una semplice applicazione che non Usa SQL Server è possibile ignorare questa attività.

1. È necessario un server di Database SQL per archiviare il database dell'applicazione. È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure all'indirizzo **database Sql** | **server** | **del Server Dashboard**. Se non si dispone di un server creato, è possibile crearne una usando il **Add** pulsante sulla barra dei comandi. Annotare il **nome server e URL, nome dell'account di accesso amministratore e la password**, come verranno usati nelle attività successiva. Non creare il database, come verrà creato in una fase successiva.

    ![Dashboard di Server di Database SQL](aspnet-mvc-4-custom-action-filters/_static/image25.png "Dashboard di Server di Database SQL")

    *Dashboard di Server di Database SQL*
2. Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server del **indirizzi IP consentiti**. A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP dal **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo e scegliere il ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) pulsante.

    ![Aggiunta indirizzo IP del Client](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *Aggiunta indirizzo IP del Client*
3. Una volta il **indirizzo IP del Client** viene aggiunto a indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.

    ![Confermare le modifiche](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *Confermare le modifiche*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web

1. Tornare alla soluzione ASP.NET MVC 4. Nel **Esplora soluzioni**, fare clic sul progetto sito web e selezionare **Publish**.

    ![Pubblicazione dell'applicazione](aspnet-mvc-4-custom-action-filters/_static/image29.png "pubblicazione dell'applicazione")

    *Pubblicazione del sito web*
2. Importare il profilo di pubblicazione che è stato salvato nella prima attività.

    ![L'importazione del profilo di pubblicazione](aspnet-mvc-4-custom-action-filters/_static/image30.png "l'importazione del profilo di pubblicazione")

    *Importazione del profilo di pubblicazione*
3. Fare clic su **convalidare la connessione**. Dopo aver completata la convalida fare clic su **successivo**.

    > [!NOTE]
    > La convalida è stata completata una volta che visualizzato un segno di spunta verde accanto al pulsante convalida connessione.

    ![La convalida connessione](aspnet-mvc-4-custom-action-filters/_static/image31.png "convalida della connessione")

    *Convalida della connessione*
4. Nel **impostazioni** nella pagina il **database** sezione, fare clic sul pulsante accanto alla casella di testo della connessione di database (vale a dire **DefaultConnection**).

    ![Configurazione della distribuzione Web](aspnet-mvc-4-custom-action-filters/_static/image32.png "configurazione della distribuzione Web")

    *Configurazione della distribuzione Web*
5. Configurare la connessione al database come segue:

   - Nel **nome Server** digitare l'URL server di Database SQL tramite il *tcp:* prefisso.
   - Nelle **nome utente** digitare il nome dell'account di accesso amministratore server.
   - Nelle **Password** digitare la password dell'account di accesso amministratore server.
   - Digitare un nuovo nome del database.

     ![Configurazione di stringa di connessione di destinazione](aspnet-mvc-4-custom-action-filters/_static/image33.png "configurazione stringa di connessione di destinazione")

     *Configurazione di stringa di connessione di destinazione*
6. Fare quindi clic su **OK**. Quando viene richiesto di creare il database, fare clic su **Sì**.

    ![Creazione del database](aspnet-mvc-4-custom-action-filters/_static/image34.png "creazione della stringa di database")

    *Creazione del database*
7. La stringa di connessione che si userà per connettersi al Database SQL di Windows Azure viene visualizzata all'interno di connessione predefinita nella casella di testo. Scegliere quindi **Avanti**.

    ![Stringa di connessione che punta al Database SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "stringa di connessione che punta al Database SQL")

    *Stringa di connessione che punta al Database SQL*
8. Nel **Preview** pagina, fare clic su **Publish**.

    ![Pubblicazione dell'applicazione web](aspnet-mvc-4-custom-action-filters/_static/image36.png "pubblicazione dell'applicazione web")

    *Pubblicazione dell'applicazione web*
9. Al termine del processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Appendice c: uso dei frammenti di codice

Con i frammenti di codice, hai tutto il codice che necessario a tua disposizione. Il documento lab indicherà esattamente quando usarli, come illustrato nella figura seguente.

![Uso di frammenti di codice di Visual Studio per inserire codice nel progetto](aspnet-mvc-4-custom-action-filters/_static/image37.png "frammenti di codice con Visual Studio per inserire codice nel progetto")

*Uso di frammenti di codice di Visual Studio per inserire codice nel progetto*

***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***

1. Posizionare il cursore in cui si vuole inserire il codice.
2. Iniziare a digitare il nome del frammento di codice (senza spazi o trattini).
3. Guarda come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento di codice corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).
5. Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.

![Iniziare a digitare il nome di frammento](aspnet-mvc-4-custom-action-filters/_static/image38.png "inizia a digitare il nome del frammento di codice")

*Iniziare a digitare il nome del frammento di codice*

![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-custom-action-filters/_static/image39.png "premere Tab per selezionare il frammento di codice evidenziata")

*Premere Tab per selezionare il frammento di codice evidenziata*

![Il frammento di codice e premere nuovamente Tab espanderà](aspnet-mvc-4-custom-action-filters/_static/image40.png "si espanderà il frammento di codice e premere nuovamente Tab")

*Il frammento di codice e premere nuovamente Tab espanderà*

***Per aggiungere un frammento di codice usando il mouse (c#, Visual Basic e XML)*** 1. Pulsante destro del mouse in cui si desidera inserire il frammento di codice.

1. Selezionare **Inserisci frammento** aggiungendo **frammenti di codice**.
2. Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.

![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento](aspnet-mvc-4-custom-action-filters/_static/image41.png "rapida in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice")

*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice*

![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-custom-action-filters/_static/image42.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa")

*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa*
