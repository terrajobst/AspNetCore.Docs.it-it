---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Inserimento di dipendenze di ASP.NET MVC 4 | Documenti Microsoft
author: rick-anderson
description: 'Nota: Questa pratica si presuppone di conoscenza di base dei filtri ASP.NET MVC e ASP.NET MVC 4. Se si utilizzano i filtri ASP.NET MVC 4 prima di, è consigliato...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: e6c24d03039f0e6005948a73348589627c9df2df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877658"
---
# <a name="aspnet-mvc-4-dependency-injection"></a>Inserimento di dipendenze di ASP.NET MVC 4

Da [categorie Web Team](https://twitter.com/webcamps)

[Download Web categorie Kit di formazione](https://aka.ms/webcamps-training-kit)

Questa pratica presuppone avere conoscenze di base **ASP.NET MVC** e **filtri ASP.NET MVC 4**. Se non è stato utilizzato **filtri ASP.NET MVC 4** in precedenza, è consigliabile esaminare **filtri azione personalizzati di ASP.NET MVC** le esercitazioni pratiche.

> [!NOTE]
> Tutto il codice di esempio e i frammenti di codice inclusi in Web categorie Training Kit, disponibile in [versioni Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Il progetto specifico per questa esercitazione è disponibile all'indirizzo [ASP.NET MVC 4 Dependency Injection](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).

In **oggetto orientato alla programmazione** paradigma di interazione tra gli oggetti in un modello di collaborazione in cui sono presenti i collaboratori e i consumer. Naturalmente, questo modello di comunicazione genera dipendenze tra oggetti e componenti, diventare difficile da gestire quando si aumenta la complessità.

![Classe di dipendenze e la complessità del modello](aspnet-mvc-4-dependency-injection/_static/image1.png "classe dipendenze e la complessità del modello")

*Dipendenze di classe e complessità del modello*

Probabile che hanno sentito parlare di **modello di Factory** e la separazione tra l'interfaccia e l'implementazione di servizi, in cui gli oggetti client sono spesso responsabili della posizione del servizio.

Il modello di inserimento di dipendenze è una particolare implementazione di inversione di controllo. **Inversione di controllo (IoC)** significa che gli oggetti non creano altri oggetti su cui si basano per svolgere il proprio lavoro. Invece, ricevono gli oggetti che devono essere da un'origine esterna (ad esempio, un file di configurazione xml).

**Dependency Injection (DI)** significa che questo scopo, senza l'intervento dell'oggetto, in genere un componente del framework che passa i parametri del costruttore e impostare le proprietà.

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>Il modello di progettazione dipendenza Injection (DI)

In generale, l'obiettivo di inserimento di dipendenze è che una classe client (ad esempio *dei giocatori di golf*) richiede un elemento che soddisfa un'interfaccia (ad esempio *IClub*). Non è rilevante che cos'è il tipo concreto (ad esempio *WoodClub, IronClub, WedgeClub* o *PutterClub*), è richiesto un altro utente per gestire questa (ad esempio, una buona *caddy*). Il Resolver di dipendenza in ASP.NET MVC consente di registrare la logica di dipendenza in un altro (ad esempio, un contenitore o un *elenco di associazioni*).

![Diagramma di inserimento di dipendenze](aspnet-mvc-4-dependency-injection/_static/image2.png "illustrazione di inserimento di dipendenze")

*Inserimento di dipendenze - analogia Golf*

I vantaggi dell'utilizzo di modello di inserimento di dipendenze e inversione di controllo sono i seguenti:

- Riduce di accoppiamenti di classi
- Aumenta il riutilizzo di codice
- Migliora la gestibilità del codice
- Migliora la verifica delle applicazioni

> [!NOTE]
> Inserimento di dipendenze talvolta viene confrontato con il modello di progettazione Factory astratta, ma è presente una leggera differenza tra entrambi gli approcci. SEN è un Framework di uso sottostante per risolvere le dipendenze chiamando le factory e i servizi registrati.


Per comprendere il modello di inserimento di dipendenza, dopo che si imparerà in questa esercitazione per applicarlo in ASP.NET MVC 4. Si inizierà mediante l'inserimento di dipendenza nel **controller** per includere un servizio di accesso ai database. Successivamente, si applicherà l'inserimento di dipendenze per il **viste** per utilizzare un servizio e visualizzare le informazioni. Infine, sarà possibile estendere il valore DI ASP.NET MVC 4 filtri, inserendo un filtro azione personalizzato nella soluzione.

In questa pratica, si apprenderà come:

- Integrazione di ASP.NET MVC 4 con Unity per l'inserimento di dipendenze mediante pacchetti NuGet
- Utilizzare l'inserimento di dipendenze all'interno di un Controller MVC ASP.NET
- Utilizzare l'inserimento di dipendenze all'interno di una visualizzazione ASP.NET MVC
- Utilizzare l'inserimento di dipendenze all'interno di un filtro azione MVC ASP.NET

> [!NOTE]
> Questa esercitazione Usa pacchetto NuGet Unity.Mvc3 per la risoluzione delle dipendenze, ma è possibile adattare qualsiasi Framework di inserimento di dipendenza per funzionare con ASP.NET MVC 4.


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

Questa pratica include gli esercizi seguenti:

1. [Esercizio 1: Inserimento di un Controller](#Exercise1)
2. [Esercizio 2: Inserimento di una vista](#Exercise2)
3. [Esercizio 3: Inserimento di filtri](#Exercise3)

> [!NOTE]
> Ogni esercizio è accompagnato da un **fine** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato l'esercitazione dall'inizio. Se è necessario ulteriore assistenza utilizza gli esercizi, è possibile utilizzare questa soluzione come guida.


Tempo stimato per completare questa esercitazione: **30 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>Esercizio 1: Inserimento di un Controller

In questo esercizio, si apprenderà come usare Dependency Injection in ASP.NET MVC Controllers integrando Unity utilizzando un pacchetto NuGet. Per questo motivo, i servizi verranno incluse nel controller di MvcMusicStore per separare la logica di accesso ai dati. I servizi creerà una nuova dipendenza nel costruttore del controller, viene risolto mediante l'inserimento di dipendenza con l'aiuto di **Unity**.

Questo approccio viene illustrato come generare meno applicazioni a cui sono più flessibili e facili da gestire e testare. Inoltre, si apprenderà come integrare ASP.NET MVC con Unity.

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>Informazioni sul servizio StoreManager

L'archivio di musica MVC fornito nella soluzione begin ora include un servizio che gestisce i dati di archivio Controller denominati **StoreService**. Di seguito si noterà che l'implementazione del servizio di archiviazione. Si noti che tutti i metodi di restituiscono le entità del modello.

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

**StoreController** utilizza ora di inizio soluzione **StoreService**. Tutti i riferimenti di dati sono stati rimossi da **StoreController**ed è ora possibile modificare il provider di accesso ai dati corrente senza modificare qualsiasi metodo che utilizza **StoreService**.

Di seguito che sono il **StoreController** implementazione presenta una dipendenza con **StoreService** all'interno del costruttore di classe.

> [!NOTE]
> La dipendenza presentata in questo esercizio è correlata a **Inversion of Control** (IoC).
> 
> Il **StoreController** costruttore della classe riceve un **IStoreService** parametro di tipo, è essenziale per eseguire chiamate al servizio dall'interno della classe. Tuttavia, **StoreController** non implementa il costruttore predefinito (senza alcun parametro) che qualsiasi controller è necessario per l'utilizzo di MVC ASP.NET.
> 
> Per risolvere la dipendenza, il controller deve essere creato da una factory di tipo abstract (una classe che restituisce qualsiasi oggetto del tipo specificato).


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> Quando la classe tenta di creare il StoreController senza inviare l'oggetto servizio, perché non esiste alcun costruttore dichiarato, si verificherà un errore.


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>Attività 1: esecuzione dell'applicazione

In questa attività si eseguirà l'applicazione iniziale, che include il servizio nel Controller dell'archivio che separa l'accesso ai dati dalla logica dell'applicazione.

Quando si esegue l'applicazione, si riceverà un'eccezione, come il servizio controller non viene passato come parametro per impostazione predefinita:

1. Aprire il **iniziare** soluzione si trova **inserendo Source\Ex01 Controller\Begin**.

   1. È necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. Premere **Ctrl + F5** per eseguire l'applicazione senza eseguire il debug. Verrà visualizzato il messaggio di errore &quot; **Nessun costruttore senza parametri definito per questo oggetto**&quot;:

    ![Errore durante l'esecuzione dell'applicazione ASP.NET MVC iniziare](aspnet-mvc-4-dependency-injection/_static/image3.png "errore durante l'esecuzione dell'applicazione ASP.NET MVC iniziare")

    *Errore durante l'esecuzione dell'applicazione ASP.NET MVC iniziare*
3. Chiudere il browser.

Nei passaggi seguenti verranno usati la soluzione di archiviazione musica per inserire la dipendenza da che questo controller è necessario.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>Attività 2 - Unity inclusi nella soluzione MvcMusicStore

In questa attività si includerà **Unity.Mvc3** pacchetto NuGet per la soluzione.

> [!NOTE]
> Unity.Mvc3 pacchetto è stato progettato per ASP.NET MVC 3, ma è completamente compatibile con ASP.NET MVC 4.
> 
> Unity è un contenitore dell'inserimento di dipendenza leggera ed estendibile con supporto facoltativo per l'istanza e digitare l'intercettazione. È un contenitore generico per l'utilizzo in qualsiasi tipo di applicazione .NET. Fornisce le funzionalità comuni meccanismi di inserimento di dipendenza tra, vedere: creazione di oggetti, astrazione dei requisiti in base alla specifica delle dipendenze in fase di esecuzione e la flessibilità, rinviando la configurazione del componente al contenitore.


1. Installare **Unity.Mvc3** pacchetto NuGet nel **MvcMusicStore** progetto. A tale scopo, aprire il **Package Manager Console** da **vista** | **altre finestre**.
2. Eseguire il seguente comando.

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![Installazione del pacchetto NuGet Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "l'installazione del pacchetto NuGet Unity.Mvc3")

    *Installazione del pacchetto NuGet Unity.Mvc3*
3. Una volta il **Unity.Mvc3** viene installato, esplorare i file e cartelle vengono aggiunti automaticamente per semplificare la configurazione di Unity.

    ![Installato il pacchetto di Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 pacchetto installato")

    *Pacchetto Unity.Mvc3 installato*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a>Attività 3 - registrazione Unity nell'applicazione Global.asax.cs\_avviare

In questa attività si aggiornerà il **applicazione\_avviare** metodo si trova **Global.asax.cs** per chiamare l'inizializzatore di avvio automatico di Unity e quindi aggiornare la registrazione di file di programma di avvio automatico il servizio e il Controller da utilizzare per l'inserimento di dipendenze.

1. Verrà associato a questo punto, il programma di avvio che è il file che inizializza il contenitore di Unity e Resolver di dipendenza. A tale scopo, aprire **Global.asax.cs** e aggiungere il seguente codice evidenziato all'interno di **applicazione\_avviare** metodo.

    (- Frammento di codice *ASP.NET dipendenza Injection Lab - Ex01 - inizializzare Unity*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. Aprire **Bootstrapper.cs** file.
3. Includere gli spazi dei nomi seguenti: **MvcMusicStore.Services** e **MusicStore.Controllers**.

    (- Frammento di codice *ASP.NET dipendenza Injection Lab - Ex01 - programma di avvio automatico aggiungendo degli spazi dei nomi*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. Sostituire **BuildUnityContainer** del contenuto con il codice seguente che registra Controller di archiviazione e il servizio di archiviazione di metodo.

    (- Frammento di codice *ASP.NET dipendenza Injection Lab - Ex01 - archivio registro Controller e il servizio*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Attività 4: esecuzione dell'applicazione

In questa attività verrà eseguita l'applicazione per verificare che può ora essere caricato dopo l'inclusione di Unity.

1. Premere **F5** per eseguire l'applicazione, l'applicazione deve caricare ora senza visualizzare alcun messaggio di errore.

    ![Applicazione eseguita con l'inserimento di dipendenze](aspnet-mvc-4-dependency-injection/_static/image6.png "applicazione eseguita con l'inserimento di dipendenze")

    *Applicazione in esecuzione con l'inserimento di dipendenze*
2. Passare a **e delle archiviazioni**. Verranno quindi richiamati **StoreController**, che è ora possibile creare utilizzando **Unity**.

    ![MVC negozio](aspnet-mvc-4-dependency-injection/_static/image7.png "negozio MVC")

    *Negozio MVC*
3. Chiudere il browser.

Negli esercizi seguenti si apprenderà come estendere l'ambito di inserimento di dipendenze per poterlo utilizzare all'interno di visualizzazioni MVC ASP.NET e i filtri dell'azione.

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>Esercizio 2: Inserimento di una vista

In questo esercizio, si apprenderà come usare l'inserimento di dipendenze di una vista con le nuove funzionalità di ASP.NET MVC 4 per l'integrazione con Unity. A tale scopo, chiamare un servizio personalizzato all'interno di archivio esplorazione della vista, che consente di visualizzare un messaggio e un'immagine riportata di seguito.

Verrà quindi integrare il progetto con Unity e creare un resolver di dipendenza personalizzata per l'inserimento di dipendenze.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>Attività 1 - Creazione di una vista che utilizza un servizio

In questa attività si creerà una vista che esegue una chiamata al servizio per generare una nuova dipendenza. Il servizio consiste in un semplice servizio di messaggistica incluso in questa soluzione.

1. Aprire il **iniziare** soluzione si trova nel **inserendo Source\Ex02 View\Begin** cartella. In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
      > 
      > Per altre informazioni, vedere questo articolo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Includono il **MessageService.cs** e **IMessageService.cs** classi disponibili nel **origine \Assets** cartella **/servizi**. A tale scopo, fare doppio clic su **servizi** cartella e selezionare **Aggiungi elemento esistente**. Passare al percorso dei file e li include.

    ![Aggiunta di SMS e l'interfaccia di servizio](aspnet-mvc-4-dependency-injection/_static/image8.png "aggiunta Message Service e interfaccia di servizio")

    *Aggiunta del servizio di messaggi e l'interfaccia di servizio*

    > [!NOTE]
    > Il **IMessageService** interfaccia definisce due proprietà implementate dal **MessageService** classe. Queste proprietà -**messaggio** e **ImageUrl**-memorizzare il messaggio e l'URL dell'immagine da visualizzare.
3. Creare la cartella **/pagine** cartella principale del progetto e quindi aggiungere la classe esistente **MyBasePage.cs** da **Source\Assets**. La pagina base che è eredita presenta la struttura seguente.

    ![Cartella pagine](aspnet-mvc-4-dependency-injection/_static/image9.png "cartella pagine")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. Aprire **Browse.cshtml** visualizzare da **/viste o all'archivio** cartella e renderlo ereditare **MyBasePage.cs**.

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. Nel **Sfoglia** visualizzare, aggiungere una chiamata a **MessageService** per visualizzare un'immagine e un messaggio recuperato dal servizio.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>Attività 2 - tra cui un Resolver di dipendenza personalizzata e un attivatore della pagina di visualizzazione personalizzata

Nell'attività precedente, sono inserite una nuova dipendenza all'interno di una vista per eseguire una chiamata al servizio all'interno. A questo punto, si risolverà tale dipendenza implementando le interfacce di ASP.NET MVC Dependency Injection **IViewPageActivator** e **resolver di dipendenza**. Verranno incluse nella soluzione un'implementazione di **resolver di dipendenza** che gestirà il recupero del servizio con Unity. Quindi, si includerà un'altra implementazione personalizzata di **IViewPageActivator** interfaccia che sia possibile risolvere la creazione delle viste.

> [!NOTE]
> Poiché ASP.NET MVC 3, l'implementazione per l'inserimento di dipendenze ha semplificato le interfacce per registrare i servizi. **Resolver di dipendenza** e **IViewPageActivator** fanno parte delle funzionalità di ASP.NET MVC 3 per l'inserimento di dipendenze.
> 
> **-Resolver di dipendenza** interfaccia sostituisce la precedente IMvcServiceLocator. Gli implementatori di resolver di dipendenza devono restituire un'istanza del servizio o una raccolta di servizio.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> **-IViewPageActivator** interfaccia fornisce un controllo più accurato su come visualizzare le pagine vengono create istanze tramite l'inserimento di dipendenze. Le classi che implementano **IViewPageActivator** interfaccia possa creare istanze di visualizzazione utilizzando le informazioni di contesto.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. Creare il /**factory** cartella nella cartella radice del progetto.
2. Includere **CustomViewPageActivator.cs** alla soluzione da **/origini asset/** a **factory** cartella. A tale scopo, fare doppio clic su di **/Factories** cartella, selezionare **Aggiungi | Elemento esistente** e quindi selezionare **CustomViewPageActivator.cs**. Questa classe implementa il **IViewPageActivator** interfaccia per contenere il contenitore di Unity.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **CustomViewPageActivator** è responsabile per la gestione della creazione di una vista utilizzando un contenitore di Unity.
3. Includere **UnityDependencyResolver.cs** file **origini/asset** a **/Factories** cartella. A tale scopo, fare doppio clic su di **/Factories** cartella, selezionare **Aggiungi | Elemento esistente** e quindi selezionare **UnityDependencyResolver.cs** file.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > **UnityDependencyResolver** classe è un DependencyResolver personalizzato per Unity. Quando un servizio non viene trovato all'interno del contenitore di Unity, il sistema di risoluzione di base è invocated.

Nell'attività seguente verranno registrate entrambe le implementazioni per informare il percorso dei servizi e le viste del modello.

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>Attività 3 - registrazione per l'inserimento di dipendenze all'interno del contenitore di Unity

In questa attività, inserire tutte le operazioni precedenti per far funzionare l'inserimento di dipendenze.

Fino a ora la soluzione contiene gli elementi seguenti:

- Oggetto **Sfoglia** visualizzazione che eredita da **MyBaseClass** e utilizza **MessageService**.
- Una classe intermedia -**MyBaseClass**-che ha dichiarato per l'interfaccia del servizio di inserimento di dipendenze.
- Un servizio - **MessageService** - e la relativa interfaccia **IMessageService**.
- Un resolver di dipendenza personalizzata per Unity - **UnityDependencyResolver** -che gestisce il recupero del servizio.
- Un attivatore della pagina di visualizzazione - **CustomViewPageActivator** -che crea la pagina.

Per inserire **Sfoglia** visualizzazione, verrà ora registrato il resolver di dipendenza personalizzata nel contenitore di Unity.

1. Aprire **Bootstrapper.cs** file.
2. Registrare un'istanza di **MessageService** nel contenitore di Unity per inizializzare il servizio:

    (- Frammento di codice *ASP.NET dipendenza Injection Lab - Ex02 - registro Message Service*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. Aggiungere un riferimento a **MvcMusicStore.Factories** dello spazio dei nomi.

    (- Frammento di codice *ASP.NET dipendenza Injection Lab - Ex02 - factory Namespace*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. Registrare **CustomViewPageActivator** come un attivatore della pagina di visualizzazione nel contenitore Unity:

    (- Frammento di codice *ASP.NET dipendenza Injection Lab - Ex02 - registro CustomViewPageActivator*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. Sostituire i resolver di dipendenza predefinito di ASP.NET MVC 4 con un'istanza di **UnityDependencyResolver**. A tale scopo, sostituire **Initialise** metodo contenuto con il codice seguente:

    (- Frammento di codice *del Resolver di dipendenza aggiornamento ASP.NET dipendenza Injection Lab - Ex02 -*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > ASP.NET MVC fornisce una classe resolver di dipendenza predefinito. Per utilizzare i resolver di dipendenza personalizzato come quello a cui che è stata creata per unity, il sistema di risoluzione deve essere sostituito.

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Attività 4: esecuzione dell'applicazione

In questa attività verrà eseguita l'applicazione per verificare che il Browser archivio utilizza il servizio e Mostra l'immagine e il recupero del messaggio:

1. Premere **F5** per eseguire l'applicazione.
2. Fare clic su **Rock** nel Menu di generi e vedere come il **MessageService** è stato inserito nella vista e caricato il messaggio di benvenuto e l'immagine. In questo esempio, si sta immettendo a &quot; **Rock**&quot;:

    ![MVC negozio - vista Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC negozio - vista attacco intrusivo nel codice")

    *MVC negozio - vista attacco intrusivo nel codice*
3. Chiudere il browser.

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>Esercizio 3: Inserimento di filtri azione

Nell'esercitazione pratica precedente **filtri azione personalizzati** chi ha familiarità con i filtri personalizzazione e l'inserimento. In questo esercizio, si apprenderà come inserire i filtri con inserimento di dipendenze mediante il contenitore di Unity. A tale scopo, si aggiungerà alla soluzione di negozio un filtro azione personalizzato che consente di tenere traccia di attività del sito.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>Attività 1 - tra cui il filtro di rilevamento nella soluzione

In questa attività, verranno incluse nell'archivio di musica un filtro azione personalizzato per gli eventi di traccia. Come filtro azione personalizzato concetti siano già considerati nel laboratorio precedente &quot;filtri azione personalizzati&quot;, verrà semplicemente includono la classe di filtro dalla cartella Assets di questa esercitazione e quindi creare un Provider di filtri per Unity:

1. Aprire il **iniziare** soluzione si trova nel **Source\Ex03 - inserendo azione Filter\Begin** cartella. In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
      > 
      > Per altre informazioni, vedere questo articolo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Includere **TraceActionFilter.cs** file **origini/asset** a **/filtri** cartella.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > Questo filtro azione personalizzata esegue la traccia di ASP.NET. È possibile controllare &quot;locale di ASP.NET MVC 4 e filtri azione dinamici&quot; Lab per più riferimento.
3. Aggiungere la classe vuota **FilterProvider.cs** al progetto nella cartella   **/filtri.**
4. Aggiungere il **System** e **Microsoft.Practices.Unity** spazi dei nomi in **FilterProvider.cs**.

    (- Frammento di codice *Provider ASP.NET dipendenza Injection Lab - Ex03 - filtro con l'aggiunta di spazi dei nomi*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. Assicurarsi che la classe erediti dal **IFilterProvider** interfaccia.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. Aggiungere un **IUnityContainer** proprietà il **FilterProvider** classe e quindi creare un costruttore di classe per assegnare il contenitore.

    (- Frammento di codice *il costruttore di Provider filtro ASP.NET dipendenza Injection Lab - Ex03 -*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > Il costruttore della classe provider filtro non sta creando un **nuovo** all'interno dell'oggetto. Il contenitore viene passato come parametro e la dipendenza viene risolto da Unity.
7. Nel **FilterProvider** classe, implementare il metodo **GetFilters** da **IFilterProvider** interfaccia.

    (- Frammento di codice *ASP.NET dipendenza Injection Lab - Ex03 - filtro Provider GetFilters*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>Attività 2 - registrazione e attivazione del filtro

In questa attività verrà abilitata la registrazione del sito. A tale scopo, è necessario registrare il filtro in **Bootstrapper.cs BuildUnityContainer** metodo per avviare la registrazione:

1. Aprire **Web. config** si trova nella radice del progetto e abilitare il rilevamento di traccia al gruppo System. Web.

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. Aprire **Bootstrapper.cs** alla radice del progetto.
3. Aggiungere un riferimento di **MvcMusicStore.Filters** dello spazio dei nomi.

    (- Frammento di codice *ASP.NET dipendenza Injection Lab - Ex03 - programma di avvio automatico aggiungendo degli spazi dei nomi*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. Selezionare il **BuildUnityContainer** (metodo) e registrare il filtro del contenitore di Unity. È necessario registrare il provider di filtri, nonché il filtro di azione.

    (- Frammento di codice *ASP.NET dipendenza Injection Lab - Ex03 - registro FilterProvider e ActionFilter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Attività 3: esecuzione dell'applicazione

In questa attività, eseguire l'applicazione e i test verranno che il filtro azione personalizzato è la traccia dell'attività:

1. Premere **F5** per eseguire l'applicazione.
2. Fare clic su **Rock** all'interno del Menu generi. Se si desidera, è possibile passare alla generi più.

    ![Negozio](aspnet-mvc-4-dependency-injection/_static/image11.png "Negozio")

    *Music Store*
3. Passare a **/Trace.axd** per visualizzare la traccia dell'applicazione di pagina e quindi fare clic su **Visualizza dettagli**.

    ![Log di analisi](aspnet-mvc-4-dependency-injection/_static/image12.png "Log di analisi")

    *Log di analisi*

    ![Traccia dell'applicazione - dettagli richiesta](aspnet-mvc-4-dependency-injection/_static/image13.png "applicazione Trace - dettagli richiesta")

    *Traccia dell'applicazione - dettagli della richiesta*
4. Chiudere il browser.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa pratica si è appreso come utilizzare Dependency Injection in ASP.NET MVC 4 grazie all'integrazione con un pacchetto NuGet di Unity. A tal fine, è stato utilizzato l'inserimento di dipendenze all'interno di controller, visualizzazioni e filtri azione.

Sono stati trattati i concetti seguenti:

- Funzionalità di inserimento di dipendenze di ASP.NET MVC 4
- Integrazione con Unity usando pacchetto NuGet Unity.Mvc3
- Inserimento di dipendenze nel controller
- Inserimento di dipendenze nelle viste
- Inserimento di dipendenze dei filtri azione

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice a: installazione di Visual Studio Express 2012 per Web

È possibile installare **Microsoft Visual Studio Express 2012 per Web** o un altro &quot;Express&quot; versione utilizzando il **[installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web di Microsoft*.

1. Passare a [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **installa**. Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.
3. Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.

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
7. Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per Web, passare al **avviare** schermata e iniziare la scrittura &quot; **VS Express**&quot;, quindi fare clic su di **Visual Studio Express per Web** il riquadro.

    ![VS Express per il riquadro Web](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *VS Express per il riquadro Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Appendice b: utilizzo dei frammenti di codice

Con i frammenti di codice, si dispone di tutto il codice che necessario a portata di mano. Il documento lab consentirà di determinare esattamente quando è possibile utilizzarli, come illustrato nella figura seguente.

![Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto](aspnet-mvc-4-dependency-injection/_static/image19.png "frammenti di codice tramite Visual Studio per inserire codice nel progetto")

*Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto*

***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***

1. Posizionare il cursore dove si desidera inserire il codice.
2. Iniziare a digitare il nome del frammento di codice (senza spazi o segni meno).
3. Osservare come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).
5. Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.

![Iniziare a digitare il nome del frammento di codice](aspnet-mvc-4-dependency-injection/_static/image20.png "inizia a digitare il nome del frammento di codice")

*Iniziare a digitare il nome del frammento di codice*

![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-dependency-injection/_static/image21.png "premere Tab per selezionare il frammento evidenziato")

*Premere Tab per selezionare il frammento di codice evidenziata*

![Premere Tab nuovamente e il frammento di codice verranno espansi](aspnet-mvc-4-dependency-injection/_static/image22.png "espanderà premere Tab nuovamente e il frammento di codice")

*Premere Tab nuovamente e il frammento di codice verranno espansi*

***Per aggiungere un frammento di codice utilizzando il mouse (c#, Visual Basic e XML)*** 1. Fare clic in cui si desidera inserire il frammento di codice.

1. Selezionare **Inserisci frammento di codice** seguito da **frammenti di codice**.
2. Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.

![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice](aspnet-mvc-4-dependency-injection/_static/image23.png "rapida in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice")

*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice*

![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-dependency-injection/_static/image24.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso")

*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso*
