---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Inserimento di dipendenze di ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: 'Nota: In questo laboratorio pratico presuppone che conoscenze di base dei filtri ASP.NET MVC e ASP.NET MVC 4. Se si utilizzano i filtri ASP.NET MVC 4 prima di, rec...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 715444a6fbf491d7b99918294cfd2d0d0216cd09
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388044"
---
# <a name="aspnet-mvc-4-dependency-injection"></a>Inserimento di dipendenze di ASP.NET MVC 4

da [Camp Web Team](https://twitter.com/webcamps)

[Download Web Camp Kit di formazione](https://aka.ms/webcamps-training-kit)

Questa pratica si presuppone conoscenze di base **ASP.NET MVC** e **filtri ASP.NET MVC 4**. Se non è stato utilizzato **filtri ASP.NET MVC 4** in precedenza, è consigliabile esaminare **filtri azione personalizzati di ASP.NET MVC** laboratorio pratico.

> [!NOTE]
> Tutto il codice di esempio e frammenti di codice sono inclusi nel Web Camp Kit di formazione, disponibile in [Microsoft-Web/WebCampTrainingKit versioni](https://aka.ms/webcamps-training-kit). Il progetto specifico per questo lab è disponibile all'indirizzo [inserimento delle dipendenze di ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).

Nelle **programmazione orientata a oggetti di oggetto** paradigma, tali oggetti interagiscono in un modello per la collaborazione in cui sono presenti i collaboratori e utenti. Naturalmente, questo modello di comunicazione genera dipendenze tra oggetti e componenti, diventare difficile da gestire quando si aumenta la complessità.

![Classe dipendenze e la complessità del modello](aspnet-mvc-4-dependency-injection/_static/image1.png "classe dipendenze e la complessità del modello")

*Le dipendenze di classe e la complessità del modello*

Probabile che abbiano già sentito parlare di **modello di Factory** e la separazione tra l'interfaccia e l'implementazione tramite servizi, in cui gli oggetti client sono spesso responsabili della posizione del servizio.

Il modello di inserimento delle dipendenze è una particolare implementazione di inversione del controllo. **Inversione di controllo (IoC)** significa che gli oggetti non creano altri oggetti su cui si basano per svolgere il proprio lavoro. Al contrario, ricevono gli oggetti necessari da un'origine esterna (ad esempio, un file di configurazione xml).

**Inserimento delle dipendenze** significa che questa viene eseguita senza l'intervento da parte dell'oggetto, in genere da un componente di framework che passa i parametri del costruttore e impostare le proprietà.

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>Lo schema progettuale di inserimento delle dipendenze

A livello generale, l'obiettivo di inserimento delle dipendenze è che una classe client (ad esempio *dei giocatori di golf*) richiede un elemento che soddisfa un'interfaccia (ad esempio *IClub*). Non è rilevante che cos'è il tipo concreto (ad esempio *WoodClub, IronClub, WedgeClub* o *PutterClub*), vuole che qualcun altro che presentavano (ad esempio, una buona *caddy*). Il Resolver delle dipendenze in ASP.NET MVC in modo da poter registrare la logica di dipendenza in un'altra posizione (ad esempio, un contenitore o un *contenitore di fiori*).

![Diagramma di inserimento delle dipendenze](aspnet-mvc-4-dependency-injection/_static/image2.png "illustrazione di inserimento delle dipendenze")

*Inserimento delle dipendenze - analogia Golf*

Vantaggi dell'uso di modello di inserimento delle dipendenze e inversione di controllo sono i seguenti:

- Consente di ridurre di accoppiamenti di classi
- Aumenta il riutilizzo di codice
- Migliora la gestibilità del codice
- Migliora la verifica delle applicazioni

> [!NOTE]
> Inserimento di dipendenze in alcuni casi viene confrontato con schema progettuale Factory astratta, ma è presente una leggera differenza tra entrambi gli approcci. L'inserimento delle dipendenze è un Framework lavoro dietro a risolvere le dipendenze chiamando le factory e i servizi registrati.


Dopo avere appreso il modello di inserimento delle dipendenze, imparerai in tutto questo lab di applicarlo in ASP.NET MVC 4. Si inizierà con inserimento delle dipendenze nel **controller** per includere un servizio di accesso ai database. Successivamente, si applicherà l'inserimento delle dipendenze per il **viste** per utilizzare un servizio e visualizzare le informazioni. Infine, si estenderà l'inserimento delle dipendenze per i filtri di ASP.NET MVC 4, inserimento di un filtro azioni personalizzato nella soluzione.

In questo laboratorio pratico, si apprenderà come:

- Integrazione di ASP.NET MVC 4 con Unity per l'inserimento delle dipendenze tramite pacchetti NuGet
- Usare l'inserimento delle dipendenze all'interno di un Controller MVC ASP.NET
- Usare l'inserimento delle dipendenze all'interno di una visualizzazione ASP.NET MVC
- Usare l'inserimento delle dipendenze all'interno di un filtro di azione MVC ASP.NET

> [!NOTE]
> Questa esercitazione Usa pacchetto NuGet Unity.Mvc3 per la risoluzione delle dipendenze, ma è possibile adattare qualsiasi Framework di inserimento delle dipendenze per lavorare con ASP.NET MVC 4.


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

In questo laboratorio pratico include gli esercizi seguenti:

1. [Esercizio 1: Inserimento di un Controller](#Exercise1)
2. [Esercizio 2: Inserimento di una vista](#Exercise2)
3. [Esercizio 3: Inserimento di filtri](#Exercise3)

> [!NOTE]
> Ogni esercizio è accompagnato da un **End** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato gli esercizi. Se ti serve assistenza aggiuntiva esaminando gli esercizi, è possibile usare questa soluzione come guida.


Tempo stimato per completare questa esercitazione: **30 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>Esercizio 1: Inserimento di un Controller

In questo esercizio, si apprenderà come usare l'inserimento di dipendenze in ASP.NET MVC Controllers grazie all'integrazione con un pacchetto NuGet di Unity. Per questo motivo, si includerà servizi nei controller MvcMusicStore per separare la logica di accesso ai dati. I servizi creerà una nuova dipendenza nel costruttore del controller, che verrà risolto con l'aiuto di inserimento delle dipendenze **Unity**.

Questo approccio illustrerà come generare meno applicazioni a cui sono più flessibili e facili da gestire e testare. Si apprenderà anche come integrare ASP.NET MVC con Unity.

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>Informazioni sul servizio StoreManager

La Store musica MVC fornito nella soluzione begin ora include un servizio che gestisce i dati di Store Controller denominati **StoreService**. Di seguito si noterà che l'implementazione del servizio Store. Si noti che tutti i metodi di restituiscono le entità del modello.

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

**StoreController** dall'inizio soluzione utilizza ora **StoreService**. Tutti i riferimenti di dati sono stati rimossi da **StoreController**e a questo punto è possibile modificare il provider di accesso ai dati corrente senza modificare qualsiasi metodo che utilizza **StoreService**.

Si noterà che il **StoreController** implementazione presenta una dipendenza con **StoreService** all'interno del costruttore di classe.

> [!NOTE]
> La dipendenza presentata in questo esercizio è correlata a **Inversion of Control** (IoC).
> 
> Il **StoreController** costruttore di classe riceve un' **IStoreService** parametro di tipo, è essenziale eseguire chiamate al servizio all'interno della classe. Tuttavia **StoreController** non implementa il costruttore predefinito (senza alcun parametro) che qualsiasi controller è necessario per lavorare con ASP.NET MVC.
> 
> Per risolvere la dipendenza, il controller deve essere creato da una factory astratta (una classe che restituisce qualsiasi oggetto del tipo specificato).


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> Si otterrà un errore quando la classe tenta di creare il StoreController senza inviare l'oggetto di servizio, come non vi è alcun costruttore senza parametri dichiarati.


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>Attività 1: esecuzione dell'applicazione

In questa attività si eseguirà l'applicazione iniziale, che include il servizio nel Controller del Store che separa l'accesso ai dati dalla logica dell'applicazione.

Quando si esegue l'applicazione, si riceverà un'eccezione, come il servizio controller non viene passato come parametro per impostazione predefinita:

1. Aprire il **Begin** soluzione che si trova **inserimento Source\Ex01 Controller\Begin**.

   1. È necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Premere **Ctrl + F5** per eseguire l'applicazione senza eseguire il debug. Verrà visualizzato il messaggio di errore &quot; **alcun costruttore senza parametri per questo oggetto definito**&quot;:

    ![Errore durante l'esecuzione dell'applicazione ASP.NET MVC iniziano](aspnet-mvc-4-dependency-injection/_static/image3.png "errore durante l'esecuzione dell'applicazione ASP.NET MVC Begin")

    *Errore durante l'esecuzione dell'applicazione ASP.NET MVC Begin*
3. Chiudere il browser.

Nei passaggi seguenti funzionerà sulla soluzione Music Store per inserire la dipendenza da che questo controller è necessaria.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>Attività 2 - Unity inclusi nella soluzione MvcMusicStore

In questa attività includerà **Unity.Mvc3** pacchetto NuGet per la soluzione.

> [!NOTE]
> Pacchetto Unity.Mvc3 è stata progettata per ASP.NET MVC 3, ma è completamente compatibile con ASP.NET MVC 4.
> 
> Unity è ad esempio un contenitore di inserimento delle dipendenze leggera ed estendibile con supporto facoltativo e intercettazione di tipo. È un contenitore per utilizzo generico per l'uso in qualsiasi tipo di applicazione .NET. Fornisce tutte le caratteristiche comuni presenti in meccanismi di inserimento delle dipendenze tra cui: creazione di oggetti, l'astrazione dei requisiti, specificando dipendenze in fase di esecuzione e la flessibilità, rinviando la configurazione del componente per il contenitore.


1. Installare **Unity.Mvc3** pacchetto NuGet nel **MvcMusicStore** progetto. A tale scopo, aprire il **Console di gestione pacchetti** dalla **View** | **Other Windows**.
2. Eseguire il seguente comando.

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![Installare il pacchetto NuGet Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "installare il pacchetto NuGet Unity.Mvc3")

    *Installare il pacchetto NuGet Unity.Mvc3*
3. Una volta il **Unity.Mvc3** pacchetto viene installato, esplorare i file e cartelle vengono automaticamente aggiunti per semplificare la configurazione di Unity.

    ![Pacchetto Unity.Mvc3 installata](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 pacchetto installato")

    *Pacchetto Unity.Mvc3 installato*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a>Attività 3: registrazione di Unity in Global.asax.cs applicazione\_Start

In questa attività si aggiornerà il **Application\_avviare** metodo che si trova **Global.asax.cs** per chiamare l'inizializzatore del programma di bootstrap di Unity e quindi aggiornare la registrazione di file di programma di avvio automatico il servizio e il Controller che verrà usato per l'inserimento delle dipendenze.

1. A questo punto, si collegherà il programma di avvio che è il file che inizializza il contenitore di Unity e Resolver di dipendenza. A tale scopo, aprire **Global.asax.cs** e aggiungere il codice evidenziato seguente all'interno di **Application\_avviare** (metodo).

    (Code - Snippet *ASP.NET Dependency Injection Lab - Ex01 - inizializzare Unity*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. Aprire **Bootstrapper.cs** file.
3. Includere gli spazi dei nomi seguenti: **MvcMusicStore.Services** e **MusicStore.Controllers**.

    (Code - Snippet *ASP.NET Dependency Injection Lab - Ex01 - programma di avvio aggiunta degli spazi dei nomi*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. Sostituire **BuildUnityContainer** del contenuto con il codice seguente che esegue la registrazione servizio Store e Store Controller metodo.

    (Code - Snippet *ASP.NET Dependency Injection Lab - Ex01 - Store Register Controller e il servizio*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Attività 4: esecuzione dell'applicazione

In questa attività si eseguirà l'applicazione per verificare che può ora essere caricato dopo l'inclusione di Unity.

1. Premere **F5** per eseguire l'applicazione, l'applicazione dovrebbe ora caricare senza visualizzare alcun messaggio di errore.

    ![Esecuzione dell'applicazione con inserimento delle dipendenze](aspnet-mvc-4-dependency-injection/_static/image6.png "applicazione eseguita con inserimento delle dipendenze")

    *Applicazione in esecuzione con inserimento delle dipendenze*
2. Passare a **/Store**. Questa operazione richiamerà **StoreController**, che è ora possibile creare utilizzando **Unity**.

    ![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")

    *MVC Music Store*
3. Chiudere il browser.

Negli esercizi seguenti si apprenderà come estendere l'ambito di inserimento delle dipendenze per utilizzarla all'interno delle visualizzazioni ASP.NET MVC e i filtri di azione.

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>Esercizio 2: Inserimento di una vista

In questo esercizio, si apprenderà come usare l'inserimento di dipendenze in una vista con le nuove funzionalità di ASP.NET MVC 4 per l'integrazione con Unity. Per farlo, si chiamerà un servizio personalizzato all'interno di Store esplorazione della vista, che viene visualizzato un messaggio e un'immagine riportata di seguito.

Si verrà quindi integrare il progetto con Unity e creare un resolver di dipendenza personalizzate per l'inserimento di dipendenze.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>Attività 1: creazione di una vista che utilizza un servizio

In questa attività si creerà una vista che esegue una chiamata al servizio per generare una nuova dipendenza. Il servizio presuppone l'uso in un servizio di messaggistica semplice incluso in questa soluzione.

1. Aprire il **Begin** soluzione che si trova nel **inserimento Source\Ex02 View\Begin** cartella. In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
      > 
      > Per altre informazioni, vedere questo articolo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Includono il **MessageService.cs** e il **IMessageService.cs** classi che si trovano nel **\Assets dell'origine** cartella **/servizi**. A tale scopo, fare doppio clic su **Services** cartella e selezionare **Aggiungi elemento esistente**. Passare al percorso dei file e includerli.

    ![Aggiunta di Message Service e interfaccia di servizio](aspnet-mvc-4-dependency-injection/_static/image8.png "aggiunta Message Service e l'interfaccia di servizio")

    *Aggiunta del servizio di messaggi e l'interfaccia di servizio*

    > [!NOTE]
    > Il **IMessageService** interfaccia definisce due proprietà implementata dalle **messaggio** classe. Queste proprietà:**messaggi** e **ImageUrl**-memorizzare il messaggio e l'URL dell'immagine da visualizzare.
3. Creare la cartella **/Pages** cartella radice del progetto e quindi aggiungere la classe esistente **MyBasePage.cs** dalla **Source\Assets**. Pagina base che è erediterà dalla presenta la struttura seguente.

    ![Cartella delle pagine](aspnet-mvc-4-dependency-injection/_static/image9.png "cartella delle pagine")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. Aprire **Browse.cshtml** visualizzare dal **/viste/Store** cartella e impostarlo come ereditare **MyBasePage.cs**.

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. Nel **esplorare** consente di visualizzare, aggiungere una chiamata a **messaggio** per visualizzare un'immagine e un messaggio recuperato dal servizio.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>Attività 2 - tra cui un Resolver di dipendenza personalizzata e un attivatore della pagina di visualizzazione personalizzata

Nell'attività precedente, inserita una nuova dipendenza all'interno di una vista per eseguire una chiamata al servizio all'interno. A questo punto, si risolverà tale dipendenza implementando le interfacce di inserimento delle dipendenze di ASP.NET MVC **IViewPageActivator** e **IDependencyResolver**. Verranno inclusi nella soluzione un'implementazione di **IDependencyResolver** che gestirà il recupero di servizio con Unity. Quindi, si includerà un'altra implementazione personalizzata di **IViewPageActivator** interfaccia che consentirà di risolvere la creazione delle visualizzazioni.

> [!NOTE]
> Poiché ASP.NET MVC 3, l'implementazione per l'inserimento di dipendenze aveva semplificato le interfacce per registrare i servizi. **Resolver di dipendenza** e **IViewPageActivator** fanno parte delle funzionalità di ASP.NET MVC 3 per l'inserimento delle dipendenze.
> 
> **-IDependencyResolver** interfaccia sostituisce il precedente IMvcServiceLocator. Gli implementatori di resolver di dipendenza devono restituire un'istanza del servizio o una raccolta di servizio.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> **-IViewPageActivator** interfaccia fornisce un controllo più accurato sul modo in cui visualizzare le pagine vengono create istanze tramite inserimento delle dipendenze. Le classi che implementano **IViewPageActivator** interfaccia può creare istanze di viste mediante le informazioni di contesto.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. Creare il /**factory** cartella nella cartella radice del progetto.
2. Includere **CustomViewPageActivator.cs** per la soluzione dall'inizio **/origini/risorse/** al **factory** cartella. A tale scopo, fare doppio clic il **/Factories** cartella, selezionare **Add | Elemento esistente** e quindi selezionare **CustomViewPageActivator.cs**. Questa classe implementa il **IViewPageActivator** interface per mantenere il contenitore di Unity.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **CustomViewPageActivator** ha la responsabilità di gestire la creazione di una vista usando un contenitore di Unity.
3. Includere **UnityDependencyResolver.cs** del file da **origini/asset** al **/Factories** cartella. A tale scopo, fare doppio clic il **/Factories** cartella, selezionare **Add | Elemento esistente** e quindi selezionare **UnityDependencyResolver.cs** file.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > **UnityDependencyResolver** classe è un DependencyResolver personalizzato per Unity. Quando un servizio non viene trovato all'interno del contenitore di Unity, è invocated il resolver di base.

Nell'attività seguente verranno registrate entrambe le implementazioni per consentire al modello di conoscere il percorso dei servizi e le viste.

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>Attività 3: registrazione per l'inserimento delle dipendenze nel contenitore di Unity

In questa attività si inseriranno tutte le operazioni precedenti per far funzionare l'inserimento di dipendenze.

Fino a questo punto la soluzione contiene gli elementi seguenti:

- Oggetto **esplorare** vista da cui eredita **MyBaseClass** e utilizza **messaggio**.
- Una classe intermedia -**MyBaseClass**-con inserimento delle dipendenze dichiarato per l'interfaccia del servizio.
- Un servizio - **messaggio** - e la relativa interfaccia **IMessageService**.
- Un resolver di dipendenza personalizzate per Unity - **UnityDependencyResolver** -che riguarda il recupero del servizio.
- Un attivatore della pagina di visualizzazione - **CustomViewPageActivator** -che crea la pagina.

Per inserire **esplorare** visualizzazione, è ora registrerà il resolver di dipendenza personalizzate nel contenitore di Unity.

1. Aprire **Bootstrapper.cs** file.
2. Registrare un'istanza di **messaggio** nel contenitore di Unity per inizializzare il servizio:

    (Code - Snippet *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. Aggiungere un riferimento a **MvcMusicStore.Factories** dello spazio dei nomi.

    (Code - Snippet *ASP.NET Dependency Injection Lab - Ex02 - factory Namespace*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. Registrare **CustomViewPageActivator** come un attivatore della pagina di visualizzazione nel contenitore di Unity:

    (Code - Snippet *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. Sostituire i resolver di dipendenza predefinito di ASP.NET MVC 4 con un'istanza di **UnityDependencyResolver**. A tale scopo, sostituire **Initialise** metodo contenuto con il codice seguente:

    (Code - Snippet *Lab - Ex02 - inserimento delle dipendenze ASP.NET Update Resolver di dipendenza*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > ASP.NET MVC fornisce una classe resolver di dipendenza predefinito. Per utilizzare i resolver di dipendenza personalizzate come quello che è stata creata per unity, il resolver deve essere sostituito.

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Attività 4: esecuzione dell'applicazione

In questa attività si eseguirà l'applicazione per verificare che il Browser Store Usa il servizio e visualizza l'immagine e il messaggio recuperato:

1. Premere **F5** per eseguire l'applicazione.
2. Fare clic su **Rock** nel Menu di generi e vedere come il **messaggio** è stata inserita alla visualizzazione e caricato il messaggio di benvenuto e l'immagine. In questo esempio, si sta attivando per &quot; **Rock**&quot;:

    ![MVC Music Store - vista Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - inserimento della visualizzazione")

    *MVC Music Store - inserimento della visualizzazione*
3. Chiudere il browser.

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>Esercizio 3: Inserimento filtri azione

Nel precedente lab pratici **filtri azione personalizzati** utente abbia familiarità con la personalizzazione di filtri e dell'inserimento. In questo esercizio, si apprenderà come inserire i filtri con inserimento delle dipendenze mediante il contenitore di Unity. A tale scopo, si aggiungerà alla soluzione di Music Store un filtro azioni personalizzato che consente di tenere traccia di attività del sito.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>Attività 1: incluso il filtro di rilevamento nella soluzione

In questa attività verranno incluse di Music Store un filtro azione personalizzato per gli eventi di traccia. Come filtro azioni personalizzato concetti siano già considerati nell'ambiente di laboratorio precedente &quot;filtri azione personalizzati&quot;, verrà infatti sufficiente includere la classe di filtro dalla cartella Assets di questa esercitazione e quindi creare un Provider di filtri per Unity:

1. Aprire il **Begin** soluzione che si trova nel **Source\Ex03 - inserimento azione Filter\Begin** cartella. In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
      > 
      > Per altre informazioni, vedere questo articolo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Includere **TraceActionFilter.cs** del file da **/origini/asset** al **/Filtra** cartella.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > Questo filtro azioni personalizzato esegue l'analisi ASP.NET. È possibile controllare &quot;ASP.NET MVC 4 locali e i filtri azione dinamici&quot; Lab per altre informazioni di riferimento.
3. Aggiungere la classe vuota **FilterProvider.cs** al progetto nella cartella   **/filtri.**
4. Aggiungere il **System** e **Microsoft.Practices.Unity** gli spazi dei nomi **FilterProvider.cs**.

    (Code - Snippet *Dependency Injection Lab - Ex03 - filtro del Provider di ASP.NET aggiunta degli spazi dei nomi*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. Rendere la classe ereditare **IFilterProvider** interfaccia.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. Aggiungere un **IUnityContainer** proprietà di **FilterProvider** classe e quindi creare un costruttore di classe per assegnare il contenitore.

    (Code - Snippet *ASP.NET Dependency Injection Lab - Ex03 - filtro Provider costruttore*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > Il costruttore della classe provider filtro non crea una **nuovo** all'interno dell'oggetto. Il contenitore viene passato come parametro e la dipendenza sia stato risolto da Unity.
7. Nel **FilterProvider** classe, implementare il metodo **GetFilters** dalla **IFilterProvider** interfaccia.

    (Code - Snippet *ASP.NET Dependency Injection Lab - Ex03 - filtro Provider GetFilters*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>Attività 2: registrazione e attivazione del filtro

In questa attività si abiliterà il rilevamento di sito. A tale scopo, è necessario registrare il filtro nella **Bootstrapper.cs BuildUnityContainer** metodo per avviare la registrazione:

1. Aprire **Web. config** che si trova nella radice del progetto e attivare il rilevamento traccia al gruppo di System. Web.

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. Aprire **Bootstrapper.cs** alla radice del progetto.
3. Aggiungere un riferimento per la **MvcMusicStore.Filters** dello spazio dei nomi.

    (Code - Snippet *ASP.NET Dependency Injection Lab - Ex03 - programma di avvio aggiunta degli spazi dei nomi*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. Selezionare il **BuildUnityContainer** (metodo) e registrare il filtro del contenitore di Unity. È necessario registrare il provider di filtri, nonché il filtro delle azioni.

    (Code - Snippet *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider e ActionFilter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Attività 3: esecuzione dell'applicazione

In questa attività, eseguire l'applicazione e i test verranno che il filtro azioni personalizzato è la traccia dell'attività:

1. Premere **F5** per eseguire l'applicazione.
2. Fare clic su **Rock** all'interno del Menu di generi. Se si desidera, è possibile esplorare per altri generi.

    ![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")

    *Music Store*
3. Passare a **/Trace.axd** per visualizzare la traccia dell'applicazione e quindi fare clic su **Visualizza dettagli**.

    ![Log di traccia dell'applicazione](aspnet-mvc-4-dependency-injection/_static/image12.png "Log di traccia dell'applicazione")

    *Log di traccia dell'applicazione*

    ![Traccia delle applicazioni - dettagli della richiesta](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - dettagli richiesta")

    *Traccia delle applicazioni - dettagli della richiesta*
4. Chiudere il browser.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa pratica si è appreso come usare l'inserimento delle dipendenze in ASP.NET MVC 4 grazie all'integrazione con un pacchetto NuGet di Unity. A tale scopo, è stato usato l'inserimento delle dipendenze all'interno di controller, visualizzazioni e filtri dell'azione.

Sono stati trattati i concetti seguenti:

- Funzionalità di inserimento delle dipendenze di ASP.NET MVC 4
- Integrazione con Unity tramite Unity.Mvc3 Mobileengagement
- Inserimento delle dipendenze in controller
- Inserimento delle dipendenze in visualizzazioni
- Inserimento di dipendenze dei filtri azione

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice a: installazione di Visual Studio Express 2012 per Web

È possibile installare **Microsoft Visual Studio Express 2012 per Web** o da un'altra &quot;Express&quot; versione utilizzando il **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web Microsoft*.

1. Passare a [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e cercare il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **Installa ora**. Se non hai **instalace Webové Platformy** si verrà reindirizzati per scaricarlo e installarlo prima di tutto.
3. Una volta **instalace Webové Platformy** è aperto, fare clic su **installare** per avviare il programma di installazione.

    ![Installa Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "installa Visual Studio Express")

    *Installa Visual Studio Express*
4. Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.

    ![Accettare le condizioni di licenza](aspnet-mvc-4-dependency-injection/_static/image15.png)

    *Accettare le condizioni di licenza*
5. Attendere finché non viene completato il processo di download e l'installazione.

    ![Stato dell'installazione](aspnet-mvc-4-dependency-injection/_static/image16.png)

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](aspnet-mvc-4-dependency-injection/_static/image17.png)

    *Installazione completata*
7. Fare clic su **Exit** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per Web, fare clic per il **avviare** schermata e iniziare la scrittura &quot; **Visual Studio Express**&quot;, quindi fare clic sul **Visual Studio Express per Web** il riquadro.

    ![Visual Studio Express per il riquadro Web](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *Visual Studio Express per il riquadro Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Appendice b: uso dei frammenti di codice

Con i frammenti di codice, hai tutto il codice che necessario a tua disposizione. Il documento lab indicherà esattamente quando usarli, come illustrato nella figura seguente.

![Uso di frammenti di codice di Visual Studio per inserire codice nel progetto](aspnet-mvc-4-dependency-injection/_static/image19.png "frammenti di codice con Visual Studio per inserire codice nel progetto")

*Uso di frammenti di codice di Visual Studio per inserire codice nel progetto*

***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***

1. Posizionare il cursore in cui si vuole inserire il codice.
2. Iniziare a digitare il nome del frammento di codice (senza spazi o trattini).
3. Guarda come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento di codice corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).
5. Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.

![Iniziare a digitare il nome di frammento](aspnet-mvc-4-dependency-injection/_static/image20.png "inizia a digitare il nome del frammento di codice")

*Iniziare a digitare il nome del frammento di codice*

![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-dependency-injection/_static/image21.png "premere Tab per selezionare il frammento di codice evidenziata")

*Premere Tab per selezionare il frammento di codice evidenziata*

![Il frammento di codice e premere nuovamente Tab espanderà](aspnet-mvc-4-dependency-injection/_static/image22.png "si espanderà il frammento di codice e premere nuovamente Tab")

*Il frammento di codice e premere nuovamente Tab espanderà*

***Per aggiungere un frammento di codice usando il mouse (c#, Visual Basic e XML)*** 1. Pulsante destro del mouse in cui si desidera inserire il frammento di codice.

1. Selezionare **Inserisci frammento** aggiungendo **frammenti di codice**.
2. Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.

![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento](aspnet-mvc-4-dependency-injection/_static/image23.png "rapida in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice")

*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice*

![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-dependency-injection/_static/image24.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa")

*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa*
