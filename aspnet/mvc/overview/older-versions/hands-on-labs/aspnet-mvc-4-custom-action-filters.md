---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Filtri azione personalizzati di ASP.NET MVC 4 | Documenti Microsoft
author: rick-anderson
description: ASP.NET MVC fornisce i filtri di azione per l'esecuzione di logica di filtro prima o dopo la chiamata di un metodo di azione. I filtri azione sono attributi personalizzati che...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 8b135b23aea64b0c7c7d4368eef9ee80914159e4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>Filtri azione personalizzati di ASP.NET MVC 4

Da [categorie Web Team](https://twitter.com/webcamps)

[Download Web categorie Kit di formazione](https://aka.ms/webcamps-training-kit)

ASP.NET MVC fornisce i filtri di azione per l'esecuzione di logica di filtro prima o dopo la chiamata di un metodo di azione. Filtri azione sono attributi personalizzati che forniscono un modo per aggiungere un comportamento pre-azione e post-azione ai metodi di azione del controller.

In questo laboratorio pratico si creerà un attributo di filtro azione personalizzato nella soluzione MvcMusicStore per intercettare le richieste del controller e registrare l'attività di un sito in una tabella di database. Sarà in grado di aggiungere il filtro di registrazione per l'inserimento di un'azione o un controller. Infine, si noterà che la visualizzazione del log che mostra l'elenco dei visitatori.

Questa pratica presuppone avere conoscenze di base **ASP.NET MVC**. Se non è stato utilizzato **ASP.NET MVC** in precedenza, è consigliabile esaminare **nozioni di base di ASP.NET MVC 4** le esercitazioni pratiche.

> [!NOTE]
> Tutto il codice di esempio e i frammenti di codice inclusi in Web categorie Training Kit, disponibile in [versioni Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Il progetto specifico per questa esercitazione è disponibile all'indirizzo [filtri azione personalizzati di ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questa pratica, si apprenderà come:

- Creare un attributo di filtro azione personalizzata per estendere le funzionalità di filtro
- Applicare un attributo di filtro personalizzato per l'inserimento di un livello specifico
- Registrare un filtri azione personalizzati a livello globale

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

1. [Esercizio 1: La registrazione delle azioni](#Exercise1)
2. [Esercizio 2: Gestione di più filtri azione](#Exercise2)

Tempo stimato per completare questa esercitazione: **30 minuti**.

> [!NOTE]
> Ogni esercizio è accompagnato da un **fine** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato l'esercitazione dall'inizio. Se è necessario ulteriore assistenza utilizza gli esercizi, è possibile utilizzare questa soluzione come guida.


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>Esercizio 1: La registrazione delle azioni

In questo esercizio, si apprenderà come creare un filtro azione personalizzata del log tramite i provider di filtri ASP.NET MVC 4. A tale scopo si applicherà un filtro di registrazione al sito MusicStore che registrerà tutte le attività nel controller selezionato.

Il filtro verrà esteso **ActionFilterAttributeClass** ed eseguire l'override **OnActionExecuting** per intercettare ogni richiesta e quindi eseguire le azioni di registrazione. Informazioni di contesto sulle richieste HTTP, l'esecuzione di metodi, i risultati e i parametri vengono fornita da ASP.NET MVC **ActionExecutingContext** classe **.**

> [!NOTE]
> ASP.NET MVC 4 ha anche il provider di filtri predefiniti è possibile utilizzare senza creare un filtro personalizzato. ASP.NET MVC 4 fornisce i seguenti tipi di filtri:
> 
> - **Autorizzazione** filtrare, che prende decisioni di protezione sulla possibilità di eseguire un metodo di azione, ad esempio l'autenticazione o la convalida delle proprietà della richiesta.
> - **Azione** filtro che esegue il wrapping l'esecuzione del metodo di azione. Questo filtro è possibile eseguire ulteriori elaborazioni, ad esempio fornire dati aggiuntivi al metodo di azione, controllare il valore restituito o annullare l'esecuzione del metodo di azione
> - **Risultato** filtro che esegue il wrapping di esecuzione dell'oggetto ActionResult. Questo filtro è possibile eseguire un'ulteriore elaborazione del risultato, ad esempio modificare la risposta HTTP.
> - **Eccezione** filtro, viene eseguita se si verifica un'eccezione non gestita generata in un punto nel metodo di azione, iniziando con i filtri di autorizzazione e terminando con l'esecuzione del risultato. I filtri eccezioni è utilizzabile per attività quali la registrazione o la visualizzazione di una pagina di errore.
> 
> Per ulteriori informazioni sui provider di filtri, visitare il sito Web MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>Informazioni sulla funzionalità di registrazione MVC musica archivio applicazioni

Questa soluzione di negozio dispone di una nuova tabella di modello di dati per la registrazione del sito, **ActionLog**, con i seguenti campi: nome del controller che ha ricevuto una richiesta, l'azione chiamate, indirizzo IP del Client e timestamp.

![Modello di dati. Tabella ActionLog. ](aspnet-mvc-4-custom-action-filters/_static/image1.png "Modello di dati. Tabella ActionLog.")

*Modello di dati - tabella ActionLog*

La soluzione offre una visualizzazione MVC ASP.NET per il log azioni che è reperibile in **MvcMusicStores/visualizzazioni/ActionLog**:

![Visualizzazione del Log azioni](aspnet-mvc-4-custom-action-filters/_static/image2.png "Visualizza registro azioni")

*Visualizzazione del Log azioni*

Con questa struttura, tutto il lavoro verterà sulla interrompere una richiesta del controller e l'esecuzione della registrazione tramite il filtro personalizzato.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>Attività 1 - Creazione di un filtro personalizzato per rilevare la richiesta di un Controller

In questa attività si creerà una classe di attributi di filtro personalizzato che contiene la logica di registrazione. A tale scopo si estenderà ASP.NET MVC **ActionFilterAttribute** classe e implementare l'interfaccia **IActionFilter**.

> [!NOTE]
> Il **ActionFilterAttribute** è la classe base per tutti i filtri di attributo. Fornisce i metodi seguenti per eseguire una logica specifica dopo e prima dell'esecuzione dell'azione del controller:
> 
> - **OnActionExecuting**(filterContext ActionExecutingContext): solo prima dell'azione metodo viene chiamato.
> - **OnActionExecuted**(filterContext ActionExecutedContext): viene chiamato il metodo di azione e prima che il risultato viene eseguito (prima di eseguire il rendering di visualizzazione).
> - **OnResultExecuting**(filterContext ResultExecutingContext): solo prima il risultato dell'esecuzione (prima di eseguire il rendering di visualizzazione).
> - **OnResultExecuted**(filterContext ResultExecutedContext): dopo l'esecuzione del risultato (dopo il rendering).
> 
> Eseguendo l'override di uno di questi metodi in una classe derivata, è possibile eseguire il proprio codice di filtro.


1. Aprire il **iniziare** soluzione disponibile all'indirizzo **\Source\Ex01-LoggingActions\Begin** cartella.

   1. È necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
      > 
      > Per altre informazioni, vedere questo articolo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Aggiungere una nuova classe c# nel **filtri** cartella e denominarla *CustomActionFilter.cs*. Questa cartella archivia tutti i filtri personalizzati.
3. Aprire **CustomActionFilter.cs** e aggiungere un riferimento a **System** e **MvcMusicStore.Models** gli spazi dei nomi:

    (- Frammento di codice *filtri azione personalizzati di ASP.NET MVC 4 - Ex1 CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. Ereditare il **CustomActionFilter** classe **ActionFilterAttribute** e quindi rendere **CustomActionFilter** classe implementare **IActionFilter** interfaccia.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. Rendere **CustomActionFilter** classe eseguire l'override del metodo **OnActionExecuting** e aggiungere la logica necessaria per registrare l'esecuzione del filtro. A tale scopo, aggiungere il seguente codice evidenziato all'interno di **CustomActionFilter** classe.

    (- Frammento di codice *filtri azione personalizzati di ASP.NET MVC 4 - Ex1 LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > **OnActionExecuting** Usa metodo **Entity Framework** per aggiungere un nuovo registro ActionLog. Crea e inserisce una nuova istanza di entità con le informazioni di contesto da **filterContext**.
    > 
    > Ulteriori informazioni su **ControllerContext** classe in [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>Attività 2: inserimento di un intercettore di codice in una classe Controller dell'archivio

In questa attività si aggiungerà il filtro personalizzato per inserirlo a tutte le classi controller e azioni del controller che verranno registrate. Allo scopo di questo esercizio, la classe di Controller di archiviazione avrà un log.

Il metodo **OnActionExecuting** da **ActionLogFilterAttribute** filtro personalizzato viene eseguito quando viene chiamato un elemento inserito.

È anche possibile intercettare un metodo del controller specifico.

1. Aprire il **StoreController** in **MvcMusicStore\Controllers** e aggiungere un riferimento al **filtri** dello spazio dei nomi:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. Inserire il filtro personalizzato **CustomActionFilter** in **StoreController** classe aggiungendo **[CustomActionFilter]** attributo prima della dichiarazione di classe.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > Quando un filtro viene inserito in una classe controller, vengono inseriti anche tutte le relative azioni. Se si desidera applicare il filtro solo per un set di azioni, è necessario inserire **[CustomActionFilter]** a ciascuno di essi:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Attività 3: esecuzione dell'applicazione

In questa attività, si verificherà che il filtro di registrazione funziona. Si avvia l'applicazione e visitare lo store e quindi si controllerà le attività registrate.

1. Premere **F5** per eseguire l'applicazione.
2. Passare a **/ActionLog** per visualizzare lo stato iniziale di visualizzazione log:

    ![Registra lo stato di arresto prima attività pagina](aspnet-mvc-4-custom-action-filters/_static/image3.png "registrare lo stato di arresto prima dell'attività di pagina")

    *Stato di arresto di log prima dell'attività di pagina*

   > [!NOTE]
   > Per impostazione predefinita, verrà sempre indicato un unico elemento che viene generato durante il recupero di generi esistente per il menu.
   > 
   > Per motivi di semplicità ci stiamo pulizia di **ActionLog** tabella ogni volta che l'applicazione viene eseguita in modo visualizzerà solo i registri di verifica dell'attività ogni particolare.
   > 
   > Potrebbe essere necessario rimuovere il codice seguente dal **sessione\_avviare** metodo (nel **Global. asax** classe), per salvare un log cronologico per tutte le azioni eseguite all'interno dell'archivio Controller.
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. Fare clic su uno del **generi** dal menu ed eseguire alcune azioni, ad esempio la navigazione di un album di disponibile.
4. Passare a **/ActionLog** e se il log è vuoto premere **F5** per aggiornare la pagina. Verificare che sono state rilevate le visite:

    ![Log azioni con attività registrati](aspnet-mvc-4-custom-action-filters/_static/image4.png "log azioni con attività di registrazione")

    *Log azioni con attività di registrazione*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>Esercizio 2: Gestione di più filtri azione

In questo esercizio si verrà aggiunto un secondo filtro azioni personalizzato alla classe StoreController e definire l'ordine specifico in cui verranno eseguiti entrambi i filtri. Quindi si aggiornerà il codice per registrare il filtro a livello globale.

Sono disponibili diverse opzioni per prendere in considerazione quando si definisce l'ordine di esecuzione dei filtri. Ad esempio, la proprietà di ordine e ambito dei filtri:

È possibile definire un **ambito** per ognuno dei filtri, ad esempio, è possibile definire l'ambito tutti i filtri dell'azione da eseguire all'interno di **Controller ambito**e tutti i filtri di autorizzazione per l'esecuzione in **ambito globale** . Gli ambiti di dispongono di un ordine di esecuzione definito.

Inoltre, ogni filtro azioni presenta una proprietà di ordine che viene utilizzata per determinare l'ordine di esecuzione nell'ambito del filtro.

Per ulteriori informazioni sull'ordine di esecuzione filtri azione personalizzati, visitare questo articolo MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>Attività 1: Creazione di un nuovo filtro azione personalizzato

In questa attività si creerà un nuovo filtro azione personalizzato per inserire la classe StoreController, imparare a gestire l'ordine di esecuzione dei filtri.

1. Aprire il **iniziare** soluzione disponibile all'indirizzo **\Source\Ex02-ManagingMultipleActionFilters\Begin** cartella. In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.

    1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
    2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
    3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

        > [!NOTE]
        > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
        > 
        > Per altre informazioni, vedere questo articolo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Aggiungere una nuova classe c# nel **filtri** cartella e denominarla *MyNewCustomActionFilter.cs*
3. Aprire **MyNewCustomActionFilter.cs** e aggiungere un riferimento a **System** e **MvcMusicStore.Models** dello spazio dei nomi:

    (- Frammento di codice *filtri azione personalizzati di ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. Sostituire la dichiarazione di classe predefinita con il codice seguente.

    (- Frammento di codice *filtri azione personalizzati di ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > Questo filtro azioni personalizzato è pressoché lo stesso di quello creato nell'esercizio precedente. La differenza principale è che è il *&quot;registrati da&quot;* attributo aggiornato con il nome di questa nuova classe per identificare il filtro di cui è registrato nel registro.

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>Attività 2: Inserire un nuovo intercettore di codice nella classe StoreController

In questa attività, si verrà aggiungere un nuovo filtro personalizzato nella classe StoreController ed eseguire la soluzione per verificare come entrambi i filtri usati insieme.

1. Aprire il **StoreController** classe si trova in **MvcMusicStore\Controllers** e inserire il nuovo filtro personalizzato **MyNewCustomActionFilter** in  **StoreController** classe come è illustrato nel codice seguente.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. A questo punto, eseguire l'applicazione per vedere il funzionamento di questi due filtri azione personalizzati. A tale scopo, premere **F5** e attendere l'avvio dell'applicazione.
3. Passare a **/ActionLog** per visualizzare lo stato iniziale di visualizzazione di log.

    ![Registra lo stato di arresto prima attività pagina](aspnet-mvc-4-custom-action-filters/_static/image5.png "registrare lo stato di arresto prima dell'attività di pagina")

    *Stato di arresto di log prima dell'attività di pagina*
4. Fare clic su uno del **generi** dal menu ed eseguire alcune azioni, ad esempio la navigazione di un album di disponibile.
5. Verificare che questa fase. le visite è sono rilevate due volte: una volta per ognuno dei filtri azione personalizzati aggiunti nel **che controller di archiviazione** classe.

    ![Log azioni con attività registrati](aspnet-mvc-4-custom-action-filters/_static/image6.png "log azioni con attività di registrazione")

    *Log azioni con attività di registrazione*
6. Chiudere il browser.

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>Attività 3: Gestione di ordinamento di filtro

In questa attività si apprenderà come gestire l'ordine di esecuzione dei filtri utilizzando la proprietà di ordine.

1. Aprire il **StoreController** classe si trova in **MvcMusicStore\Controllers** e specificare il **ordine** in entrambi i filtri proprietà come illustrato di seguito.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. A questo punto, verificare come vengono eseguiti i filtri in base al valore della proprietà relativo ordine. Si noterà che il filtro con il valore più piccolo dell'ordine (**CustomActionFilter**) è il primo che viene eseguito. Premere **F5** e attendere l'avvio dell'applicazione.
3. Passare a **/ActionLog** per visualizzare lo stato iniziale di visualizzazione di log.

    ![Registra lo stato di arresto prima attività pagina](aspnet-mvc-4-custom-action-filters/_static/image7.png "registrare lo stato di arresto prima dell'attività di pagina")

    *Stato di arresto di log prima dell'attività di pagina*
4. Fare clic su uno del **generi** dal menu ed eseguire alcune azioni, ad esempio la navigazione di un album di disponibile.
5. Verificare che questa volta, sono state rilevate le visite ordinati in base al valore dell'ordine dei filtri: **CustomActionFilter** registri prima.

    ![Log azioni con attività registrati](aspnet-mvc-4-custom-action-filters/_static/image8.png "log azioni con attività di registrazione")

    *Log azioni con attività di registrazione*
6. A questo punto, si aggiorna il valore di ordine dei filtri e verificare come viene modificato l'ordine di registrazione. Nel **StoreController** classe, aggiornare il valore dell'ordine dei filtri come illustrato di seguito.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. Eseguire nuovamente l'applicazione premendo **F5**.
8. Fare clic su uno del **generi** dal menu ed eseguire alcune azioni, ad esempio la navigazione di un album di disponibile.
9. Verificare che questo periodo, i log creati **MyNewCustomActionFilter** filtro viene visualizzata per primo.

    ![Log azioni con attività registrati](aspnet-mvc-4-custom-action-filters/_static/image9.png "log azioni con attività di registrazione")

    *Log azioni con attività di registrazione*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>Attività 4: Registrazione di filtri a livello globale

In questa attività si aggiornerà la soluzione per registrare il nuovo filtro (**MyNewCustomActionFilter**) come filtro globale. In questo modo verrà attivato per tutti i temporanei azioni nell'applicazione e non solo di quelli StoreController come l'attività precedente.

1. In **StoreController** classe, rimuovere **[MyNewCustomActionFilter]** attributo e la proprietà order **[CustomActionFilter]**. Dovrebbe essere simile al seguente:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. Aprire **Global. asax** file e individuare il **applicazione\_avviare** metodo. Si noti che ogni volta che viene avviata l'applicazione sta registrando i filtri globali chiamando **RegisterGlobalFilters** metodo all'interno di **FilterConfig** classe.

    ![Registrazione di filtri globali in Global. asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "la registrazione di filtri globali in Global. asax")

    *Registrazione di filtri globali in Global. asax*
3. Aprire **FilterConfig.cs** file all'interno di **App\_avviare** cartella.
4. Aggiungere un riferimento mediante System; utilizzando MvcMusicStore.Filters; spazio dei nomi.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. Aggiornamento **RegisterGlobalFilters** metodo aggiunta del filtro personalizzato. A tale scopo, aggiungere il codice evidenziato:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. Eseguire l'applicazione premendo **F5**.
7. Fare clic su uno del **generi** dal menu ed eseguire alcune azioni, ad esempio la navigazione di un album di disponibile.
8. Verificare che ora **[MyNewCustomActionFilter]** viene viene inserito in HomeController e ActionLogController troppo.

    ![Log azioni con attività registrati](aspnet-mvc-4-custom-action-filters/_static/image11.png "log azioni con attività di registrazione")

    *Log azioni con attività globale registrato*

> [!NOTE]
> Inoltre, è possibile distribuire l'applicazione per siti Web di Azure seguenti [pubblicazione appendice b: un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa pratica si è appreso come estendere un filtro di azione per eseguire azioni personalizzate. Inoltre, si è appreso come inserire qualsiasi filtro ai controller di pagina. Sono stati utilizzati i seguenti concetti:

- Come creare filtri azione personalizzati con la classe ActionFilterAttribute MVC ASP.NET
- Come inserire i filtri in ASP.NET MVC controller
- Come gestire l'ordinamento di filtro utilizzando la proprietà Order
- Come registrare i filtri a livello globale

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice a: installazione di Visual Studio Express 2012 per Web

È possibile installare **Microsoft Visual Studio Express 2012 per Web** o un altro &quot;Express&quot; versione utilizzando il **[installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web di Microsoft*.

1. Passare a [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **installa**. Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.
3. Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.

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
7. Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per Web, passare al **avviare** schermata e iniziare la scrittura &quot; **VS Express**&quot;, quindi fare clic su di **Visual Studio Express per Web** il riquadro.

    ![VS Express per il riquadro Web](aspnet-mvc-4-custom-action-filters/_static/image16.png)

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

    ![Accedere al portale Windows Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "accedere al portale Windows Azure")

    *Accedere al portale di gestione di Azure*
2. Fare clic su **nuovo** sulla barra dei comandi.

    ![Creazione di un nuovo sito Web](aspnet-mvc-4-custom-action-filters/_static/image18.png "creando un nuovo sito Web")

    *Creazione di un nuovo sito Web*
3. Fare clic su **calcolo** | **sito Web**. Selezionare quindi **creazione rapida** opzione. Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.

    > [!NOTE]
    > Un sito Web di Windows Azure è l'host per un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire. L'opzione Creazione rapida consente di distribuire un'applicazione web completa per il sito Web Microsoft Azure dall'esterno al portale. Non include i passaggi per configurare un database.

    ![Creazione di un nuovo sito Web utilizzando Creazione rapida](aspnet-mvc-4-custom-action-filters/_static/image19.png "creando un nuovo sito Web utilizzando Creazione rapida")

    *Creazione di un nuovo sito Web utilizzando Creazione rapida*
4. Attendere finché il nuovo **sito Web** viene creato.
5. Una volta creato il sito Web, fare clic sul collegamento sotto il **URL** colonna. Verificare il funzionamento del nuovo sito Web.

    ![Esplorazione per il nuovo sito web](aspnet-mvc-4-custom-action-filters/_static/image20.png "esplorazione per il nuovo sito web")

    *Esplorazione per il nuovo sito web*

    ![Sito Web in esecuzione](aspnet-mvc-4-custom-action-filters/_static/image21.png "sito Web in esecuzione")

    *Sito Web in esecuzione*
6. Tornare al portale e fare clic sul nome del sito web sotto il **nome** colonna per visualizzare le pagine di gestione.

    ![Aprire le pagine di gestione del sito web](aspnet-mvc-4-custom-action-filters/_static/image22.png "apertura delle pagine di gestione del sito web")

    *Aprire le pagine di gestione del sito Web*
7. Nel **Dashboard** nella pagina di **riepilogo rapido** fare clic su di **Scarica profilo di pubblicazione** collegamento.

    > [!NOTE]
    > Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in un sito Web di Windows Azure per ogni metodo di pubblicazione abilitato. Il profilo di pubblicazione contiene gli URL, le credenziali dell'utente e le stringhe di database necessari per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di queste applicazioni per pubblicazione di applicazioni web per siti Web di Windows Azure.

    ![Profilo di pubblicazione del sito web di download](aspnet-mvc-4-custom-action-filters/_static/image23.png "profilo di pubblicazione del sito web di download")

    *Profilo di pubblicazione del sito Web di download*
8. Scaricare il file del profilo di pubblicazione in un percorso noto. In questo esercizio si ulteriormente verrà visualizzato come utilizzare questo file per pubblicare un'applicazione web a un siti Web di Windows Azure da Visual Studio.

    ![Salvare il file del profilo di pubblicazione](aspnet-mvc-4-custom-action-filters/_static/image24.png "il salvataggio del profilo di pubblicazione")

    *Salvare il file del profilo di pubblicazione*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Attività 2: configurare il Server di Database

Se l'applicazione viene utilizzato SQL Server database, è necessario creare un server di Database SQL. Se si desidera distribuire un'applicazione semplice che non utilizza SQL Server è possibile ignorare questa attività.

1. È necessario un server di Database SQL per archiviare il database dell'applicazione. È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure all'indirizzo **database Sql** | **server** | **del Server Dashboard**. Se non si dispone di un server creato, è possibile creare tramite il **Aggiungi** pulsante sulla barra dei comandi. Annotare il **nome server e URL, il nome di accesso di amministratore e la password**, come verranno utilizzati in operazioni successive. Non creare il database, come verrà creato in una fase successiva.

    ![Dashboard del Server Database SQL](aspnet-mvc-4-custom-action-filters/_static/image25.png "Dashboard del Server Database SQL")

    *Dashboard del Server Database SQL*
2. Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server di **indirizzi IP consentiti**. A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP da **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo e fare clic su di ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) pulsante.

    ![Aggiunta indirizzo IP del Client](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *Aggiunta indirizzo IP del Client*
3. Una volta il **indirizzo IP del Client** viene aggiunto agli indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.

    ![Confermare le modifiche](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *Confermare le modifiche*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web

1. Tornare alla soluzione ASP.NET MVC 4. Nel **Esplora**, fare clic sul progetto sito web e selezionare **pubblica**.

    ![La pubblicazione dell'applicazione](aspnet-mvc-4-custom-action-filters/_static/image29.png "la pubblicazione dell'applicazione")

    *Pubblicazione del sito web*
2. Importare il profilo di pubblicazione che è stato salvato nella prima attività.

    ![L'importazione del profilo di pubblicazione](aspnet-mvc-4-custom-action-filters/_static/image30.png "l'importazione del profilo di pubblicazione")

    *L'importazione di profilo di pubblicazione*
3. Fare clic su **convalidare la connessione**. Una volta completata la convalida, fare clic su **Avanti**.

    > [!NOTE]
    > Dopo aver visualizzato un segno di spunta verde visualizzata accanto al pulsante convalida connessione, la convalida è stata completata.

    ![Convalida della connessione](aspnet-mvc-4-custom-action-filters/_static/image31.png "convalida della connessione")

    *Convalida della connessione*
4. Nel **impostazioni** nella pagina il **database** fare clic sul pulsante accanto casella di testo della connessione di database (ad esempio **DefaultConnection**).

    ![Configurazione della distribuzione Web](aspnet-mvc-4-custom-action-filters/_static/image32.png "configurazione della distribuzione Web")

    *Configurazione della distribuzione Web*
5. Configurare la connessione al database come segue:

   - Nel **nome Server** digitare l'URL server di Database SQL utilizzando il *tcp:* prefisso.
   - In **nome utente** digitare il nome di accesso di amministratore di server.
   - In **Password** digitare la password dell'account di accesso amministratore server.
   - Digitare un nuovo nome di database.

     ![Configurazione di stringa di connessione di destinazione](aspnet-mvc-4-custom-action-filters/_static/image33.png "configurazione stringa di connessione di destinazione")

     *Configurazione di stringa di connessione di destinazione*
6. Fare quindi clic su **OK**. Quando viene richiesto di creare il database fare clic su **Sì**.

    ![Creazione del database](aspnet-mvc-4-custom-action-filters/_static/image34.png "creazione della stringa di database")

    *Creazione del database*
7. La stringa di connessione che si utilizzerà per connettersi al Database SQL di Windows Azure viene visualizzata all'interno di casella di testo di connessione predefinito. Scegliere quindi **Avanti**.

    ![Stringa di connessione che punta al Database SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "stringa di connessione che punta al Database SQL")

    *Stringa di connessione che punta al Database SQL*
8. Nel **anteprima** pagina, fare clic su **pubblica**.

    ![Pubblicare l'applicazione web](aspnet-mvc-4-custom-action-filters/_static/image36.png "pubblicare l'applicazione web")

    *Pubblicare l'applicazione web*
9. Una volta terminato il processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Appendice c: utilizzo dei frammenti di codice

Con i frammenti di codice, si dispone di tutto il codice che necessario a portata di mano. Il documento lab consentirà di determinare esattamente quando è possibile utilizzarli, come illustrato nella figura seguente.

![Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto](aspnet-mvc-4-custom-action-filters/_static/image37.png "frammenti di codice tramite Visual Studio per inserire codice nel progetto")

*Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto*

***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***

1. Posizionare il cursore dove si desidera inserire il codice.
2. Iniziare a digitare il nome del frammento di codice (senza spazi o segni meno).
3. Osservare come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).
5. Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.

![Iniziare a digitare il nome del frammento di codice](aspnet-mvc-4-custom-action-filters/_static/image38.png "inizia a digitare il nome del frammento di codice")

*Iniziare a digitare il nome del frammento di codice*

![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-custom-action-filters/_static/image39.png "premere Tab per selezionare il frammento evidenziato")

*Premere Tab per selezionare il frammento di codice evidenziata*

![Premere Tab nuovamente e il frammento di codice verranno espansi](aspnet-mvc-4-custom-action-filters/_static/image40.png "espanderà premere Tab nuovamente e il frammento di codice")

*Premere Tab nuovamente e il frammento di codice verranno espansi*

***Per aggiungere un frammento di codice utilizzando il mouse (c#, Visual Basic e XML)*** 1. Fare clic in cui si desidera inserire il frammento di codice.

1. Selezionare **Inserisci frammento di codice** seguito da **frammenti di codice**.
2. Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.

![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice](aspnet-mvc-4-custom-action-filters/_static/image41.png "rapida in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice")

*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice*

![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-custom-action-filters/_static/image42.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso")

*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso*
