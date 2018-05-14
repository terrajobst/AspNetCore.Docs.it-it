---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: "Laboratorio pratico: Un ASP.NET: l'integrazione di Web Form ASP.NET, MVC e Web API | Documenti Microsoft"
author: rick-anderson
description: ASP.NET è un framework per la compilazione di siti Web, applicazioni e servizi con le tecnologie specializzate, ad esempio MVC, API Web e altri. Con l'espansione h ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 55109723e566a9f7c66c1a59414377b05dbec760
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a>Laboratorio pratico: Un ASP.NET: l'integrazione di Web Form ASP.NET, MVC e Web API
====================
da [categorie Web Team](https://twitter.com/webcamps)

[Download Web categorie Kit di formazione](http://aka.ms/webcamps-training-kit)

> ASP.NET è un framework per la compilazione di siti Web, applicazioni e servizi con le tecnologie specializzate, ad esempio MVC, API Web e altri. Con l'espansione ASP.NET ha rilevato dopo la creazione e l'espresso necessario disporre di queste tecnologie integrate, sono state eseguite attività di recente in favore di **ASP.NET uno**.
> 
> Visual Studio 2013 introduce un nuovo sistema di progetto unificato che consente di compilare un'applicazione e utilizzare tutte le tecnologie ASP.NET in un unico progetto. Questa funzionalità Elimina la necessità di scegliere una tecnologia all'inizio di un progetto e stick con esso e invece promuove l'uso di diversi framework ASP.NET all'interno di un progetto.
> 
> Tutto il codice di esempio e i frammenti di codice sono inclusi nel Web categorie Training Kit, disponibile all'indirizzo [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Panoramica

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questa esercitazione pratica, si apprenderà come:

- Creare un sito Web basato sul **ASP.NET uno** tipo di progetto
- Utilizzare diversi **ASP.NET** Framework come **MVC** e **API Web** nello stesso progetto
- Identificare i componenti principali di un **ASP.NET** applicazione
- Sfruttare il **lo Scaffolding di ASP.NET** framework per creare automaticamente i controller e visualizzazioni per eseguire operazioni CRUD basato su classi modello
- Esporre lo stesso set di informazioni in formati macchina e leggibile utilizzando lo strumento appropriato per ogni processo

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Di seguito è necessario per completare questa esercitazione pratica:

- [Visual Studio Express 2013 per Web](https://www.microsoft.com/visualstudio/) o versione successiva
- [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

Per eseguire gli esercizi di questa esercitazione pratica, è necessario configurare prima di tutto l'ambiente.

1. Aprire Esplora risorse e passare nel lab **origine** cartella.
2. Fare clic su **Setup.cmd** e selezionare **Esegui come amministratore** per avviare il processo di installazione che la configurazione dell'ambiente e installare i frammenti di codice di Visual Studio per questo ambiente lab.
3. Se viene visualizzata la finestra di dialogo controllo dell'Account utente, confermare l'azione per continuare.

> [!NOTE]
> Assicurarsi di avere selezionato tutte le dipendenze per questa esercitazione prima di eseguire il programma di installazione.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Utilizzo dei frammenti di codice

In tutto il documento lab, verrà invitati a inserire i blocchi di codice. Per praticità, la maggior parte di questo codice viene fornito come Visual Studio frammenti di codice che è possibile accedere da all'interno di Visual Studio 2013 per evitare di dover aggiungerla manualmente.

> [!NOTE]
> Ogni esercizio è accompagnata da una soluzione inizia all'interno di **iniziare** cartella dell'esercizio che consente di seguire ogni esercizio indipendentemente dalle altre. Tenere presente che i frammenti di codice aggiunti durante un esercizio non sono presenti queste soluzioni di avvio e potrebbero non funzionare fino a completare l'esercizio. All'interno del codice sorgente per un esercizio, si avrà inoltre un **fine** cartella che contiene una soluzione di Visual Studio con il codice generato da completare i passaggi nell'esercizio corrispondente. Se è necessario ulteriore assistenza durante l'utilizzo tramite questa esercitazione pratica, è possibile usare queste soluzioni come guida.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questa esercitazione include gli esercizi seguenti:

1. [Creazione di un nuovo progetto di Web Form](#Exercise1)
2. [Creazione di un Controller MVC con Scaffolding](#Exercise2)
3. [Creazione di un Controller API Web mediante Scaffolding](#Exercise3)

Tempo stimato per completare questa esercitazione: **60 minuti**

> [!NOTE]
> Al primo avvio di Visual Studio, è necessario selezionare una delle raccolte di impostazioni predefinite. Ogni raccolta predefinita è progettato in modo che corrisponda a un particolare stile di sviluppo e determina il layout di finestra, il comportamento dell'editor, frammenti di codice IntelliSense e finestre di dialogo. Le procedure descritte in questa esercitazione vengono descritte le azioni necessarie per eseguire una determinata attività in Visual Studio quando si utilizza il **impostazioni generali per lo sviluppo** insieme. Se si sceglie una raccolta di impostazioni diverse per l'ambiente di sviluppo, possono essere presenti differenze nei passaggi che è necessario prendere in considerazione.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a>Esercizio 1: Creazione di un nuovo progetto di Web Form

In questo esercizio si creerà un nuovo sito Web Form in Visual Studio 2013 con il **ASP.NET uno** unificata esperienza di progetto, che consente di integrare facilmente i componenti di Web Form, MVC e Web API nella stessa applicazione. Verrà quindi esplorare la soluzione generata e identificare le parti e infine viene visualizzato il sito Web in azione.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a>Attività 1: creazione di un nuovo sito usando un'esperienza di ASP.NET

In questa attività si inizierà creando un nuovo sito Web in Visual Studio in base il **ASP.NET uno** tipo di progetto. **Uno ASP.NET** unifica tutte le tecnologie di ASP.NET e offre la possibilità di combinare e associarli in base alle esigenze. È quindi riconoscerà i vari componenti di Web Form, MVC e Web API live side-by-side all'interno dell'applicazione.

1. Aprire **Visual Studio Express 2013 per Web** e selezionare **File | Nuovo progetto...**  per avviare una nuova soluzione.

    ![Creazione di un nuovo progetto](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    *Crea un nuovo progetto*
2. Nel **nuovo progetto** nella finestra di dialogo **applicazione Web ASP.NET** sotto il **Visual c# | Web** scheda e accertarsi che **.NET Framework 4.5** è selezionata. Denominare il progetto *MyHybridSite*, scegliere un **percorso** e fare clic su **OK**.

    ![Nuovo progetto applicazione Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    *Crea un nuovo progetto applicazione Web ASP.NET*
3. Nel **nuovo progetto ASP.NET** la finestra di dialogo, seleziona il **Web Form** modello e selezionare il **MVC** e **API Web** opzioni. Inoltre, assicurarsi che il **autenticazione** opzione è impostata su **singoli account utente di**. Fare clic su **OK** per continuare.

    ![Crea un nuovo progetto con il modello Web Form, inclusi i componenti Web API e MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    *Crea un nuovo progetto con il modello Web Form, inclusi i componenti Web API e MVC*
4. È ora possibile esplorare la struttura della soluzione generata.

    ![Esplorazione della soluzione generata](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    *Esplorazione della soluzione generata*

    1. **Account:** questa cartella contiene le pagine Web Form per registrare, accedere a e gestire gli account utente dell'applicazione. Questa cartella viene aggiunta quando le **singoli account utente di** è selezionata l'opzione di autenticazione durante la configurazione del modello di progetto di Web Form.
    2. **Modelli:** questa cartella contiene le classi che rappresentano i dati dell'applicazione.
    3. **Controller** e **viste**: sono necessarie per queste cartelle di **ASP.NET MVC** e **ASP.NET Web API** componenti. Analizzerà le tecnologie MVC e Web API negli esercizi.
    4. Il **Default.aspx**, **Contact.aspx** e **About** file sono pagine Web Form predefinite che è possibile utilizzare come punto di partenza per compilare le pagine specifiche per il applicazione. La logica di programmazione di tali file si trova in un file separato, detto il &quot;codice&quot; file, con un &quot;. aspx&quot; o &quot;. aspx.cs&quot; estensione (a seconda di linguaggio utilizzato). La logica del codice viene eseguito nel server e in modo dinamico produce l'output HTML per la pagina.
    5. Il **Site. master** e **Site.Mobile.Master** pagine definiscono l'aspetto e il comportamento standard di tutte le pagine nell'applicazione.
5. Fare doppio clic su di **Default.aspx** file per esplorare il contenuto della pagina.

    ![Esplorazione della pagina default.](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    *Esplorazione della pagina default.*

    > [!NOTE]
    > Il **pagina** direttiva all'inizio del file definisce gli attributi della pagina Web Form. Ad esempio, il **MasterPageFile** attributo specifica il percorso al master pagina - in questo caso, il *Site. master* pagina e **Inherits** attributo definisce il classe code-behind per la pagina da cui ereditare. Questa classe si trova nel file di base di **CodeBehind** attributo.
    > 
    > Il **asp: Content** controllo contiene il contenuto effettivo della pagina (testo, markup e controlli) e viene eseguito il mapping a un **asp: ContentPlaceHolder** controllo nella pagina master. In questo caso, il contenuto della pagina eseguito all'interno di *MainContent* controllo definite nel *Site. master* pagina.
6. Espandere il **App\_avviare** cartella e notare il **WebApiConfig.cs** file. Visual Studio inclusi i file nella soluzione generata perché inclusa l'API Web quando si configura il progetto con il modello ASP.NET uno.
7. Aprire il **WebApiConfig.cs** file. Nel *WebApiConfig* si noterà che la configurazione associata all'API Web, che esegue il mapping HTTP classe indirizza a **controller API Web**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. Aprire il **RouteConfig.cs** file. All'interno di *RegisterRoutes* metodo si noterà che la configurazione associata a MVC, che esegue il mapping delle route HTTP a **controller MVC**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a>Attività 2: esecuzione della soluzione

In questa attività verranno eseguire la soluzione generata, esplorare l'app e alcune caratteristiche, ad esempio la riscrittura degli URL e l'autenticazione predefinita.

1. Per eseguire la soluzione, premere **F5** oppure fare clic su di **avviare** pulsante Trova sulla barra degli strumenti. La home page dell'applicazione deve aprire nel browser.

    ![Esecuzione della soluzione](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. Verificare che le pagine Web Form vengano richiamate. A tale scopo, aggiungere **/contact.aspx** per l'URL nella barra degli indirizzi e premere **invio**.

    ![URL brevi](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    *URL brevi*

    > [!NOTE]
    > Come si può notare, l'URL viene modificato per **o contattare**. A partire da **ASP.NET 4**, funzionalità di routing di URL sono stati aggiunti a un Web Form, pertanto è possibile scrivere come URL *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* anziché  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*. Per ulteriori informazioni fare riferimento a [Routing degli URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).
3. Verrà ora illustrato il flusso di autenticazione integrato nell'applicazione. A tale scopo, fare clic su **registrare** nell'angolo superiore destro della pagina.

    ![Registrazione di un nuovo utente](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    *Registrazione di un nuovo utente*
4. Nel **registrare** pagina, immettere un **nome utente** e **Password**, quindi fare clic su **registrare**.

    ![Pagina di registrazione](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    *Pagina di registrazione*
5. L'applicazione Registra nuovo account e l'autenticazione dell'utente.

    ![Utente autenticato](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    *Utente autenticato*
6. Tornare a Visual Studio e premere **MAIUSC + F5** per arrestare il debug.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a>Esercizio 2: Creazione di un Controller MVC con Scaffolding

In questo esercizio si sfrutterà il framework di ASP.NET Scaffolding fornito da Visual Studio per creare un controller di ASP.NET MVC 5 con azioni e visualizzazioni Razor per eseguire operazioni CRUD, senza scrivere una singola riga di codice. Il processo di scaffolding userà Code First di Entity Framework per generare il contesto dei dati e lo schema del database nel database SQL.

**Su Entity Framework Code prima**

Entity Framework (EF) è un mapping relazionale a oggetti (ORM) che consente di creare applicazioni di accesso ai dati tramite programmazione con un modello di applicazione concettuale anziché programmando direttamente con uno schema di archiviazione relazionale.

Il flusso di lavoro modello Code First di Entity Framework consente di utilizzare le classi di dominio per rappresentare il modello di Entity Framework si basa su quando si esegue la query, le funzioni di rilevamento delle modifiche e l'aggiornamento. Utilizza il flusso di lavoro di sviluppo Code First, non necessario avviare l'applicazione mediante la creazione di un database o specificare uno schema. In alternativa, è possibile scrivere standard .NET le classi che definiscono gli oggetti modello di dominio più appropriati per l'applicazione ed Entity Framework consente di creare il database per l'utente.

> [!NOTE]
> Maggiori informazioni su Entity Framework [qui](../../../entity-framework.md).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a>Attività 1: creazione di un nuovo modello

Ora si definirà un **persona** (classe), che sarà il modello utilizzato dal processo di scaffolding per creare il controller MVC e le viste. Iniziare creando un **persona** classe di modello e le operazioni CRUD nel controller di verranno automaticamente creata utilizzando le funzionalità di scaffolding.

1. Aprire **Visual Studio Express 2013 per Web** e **MyHybridSite.sln** soluzione si trova nel **origine/Ex2-MvcScaffolding/inizio** cartella. In alternativa, è possibile continuare con la soluzione ottenuta nell'esercizio precedente.
2. In **Esplora**, fare doppio clic su di **modelli** cartella del **MyHybridSite** del progetto e selezionare **Aggiungi | Classe...** .

    ![Aggiunta di una classe modello persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    *Aggiunta di una classe modello persona*
3. Nel **Aggiungi nuovo elemento** al file il nome nella finestra di dialogo *Person* e fare clic su **Aggiungi**.

    ![Creazione della classe di modello di persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    *Creazione della classe di modello di persona*
4. Sostituire il contenuto del **Person** file con il codice seguente. Premere **CTRL + S** per salvare le modifiche.

    (- Frammento di codice *PersonClass BringingTogetherOneAspNet - Ex2 -*)

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. In **Esplora**, fare doppio clic su di **MyHybridSite** progetto e selezionare **compilare**, oppure premere **CTRL + MAIUSC + B** per compilare il progetto.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a>Attività 2: creazione di un Controller MVC

Ora che il **persona** creazione del modello, si utilizzerà lo scaffolding di ASP.NET MVC con Entity Framework per creare le azioni del controller CRUD e le viste per **persona**.

1. In **Esplora**, fare doppio clic sul **controller** cartella del **MyHybridSite** del progetto e selezionare **Aggiungi | Nuovo elemento di scaffolding...** .

    ![Creare un nuovo Controller di scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    *Creare un nuovo Controller di scaffolding*
2. Nel **aggiungere lo scaffolding** nella finestra di dialogo **Controller MVC 5 con visualizzazioni, mediante Entity Framework** e quindi fare clic su **Aggiungi.**

    ![Selezione di Controller MVC 5 con visualizzazioni ed Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    *Selezione di Controller MVC 5 con visualizzazioni ed Entity Framework*
3. Impostare *MvcPersonController* come il **nome Controller**, selezionare il **utilizzare azioni asincrone del controller** opzione e selezionare **persona (MyHybridSite.Models)**  come il **classe modello**.

    ![Aggiunta di un controller MVC con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    *Aggiunta di un controller MVC con scaffolding*
4. In **classe contesto dati**, fare clic su **nuovo contesto dati...** .

    ![Creazione di un nuovo contesto dati](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    *Creazione di un nuovo contesto dati*
5. Nel **nuovo contesto dati** nella finestra di dialogo Nome nuovo contesto dati *PersonContext* e fare clic su **Aggiungi**.

    ![Creare il nuovo PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    *La creazione del nuovo tipo PersonContext*
6. Fare clic su **Aggiungi** per creare il nuovo controller per **persona** con scaffolding. Visual Studio genera quindi le azioni del controller, il contesto dei dati utente e le visualizzazioni Razor.

    ![Dopo la creazione di controller MVC con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    *Dopo la creazione di controller MVC con scaffolding*
7. Aprire il **MvcPersonController.cs** file nel **controller** cartella. Si noti che i metodi di azione CRUD siano stati generati automaticamente.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > Selezionando il **utilizzare azioni asincrone del controller** casella di controllo dallo scaffolding opzioni nei passaggi precedenti, Visual Studio genera metodi di azione asincroni per tutte le azioni che comportano l'accesso per il contesto dei dati utente. Si consiglia di utilizzare metodi di azione asincroni per l'esecuzione prolungata, non associate alla CPU richieste per evitare di bloccare il server Web di esecuzione del lavoro durante l'elaborazione della richiesta.

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a>Attività 3: esecuzione della soluzione

In questa attività verrà eseguita la soluzione per verificare che le viste per **persona** funzionino come previsto. Si aggiungerà un nuovo utente per verificare che è stata salvata nel database.

1. Premere **F5** per eseguire la soluzione.
2. Passare a **/MvcPerson**. La visualizzazione di scaffolding che mostra l'elenco degli utenti dovrebbe essere visualizzato.
3. Fare clic su **Crea nuovo** per aggiungere una nuova persona.

    ![Passare alle visualizzazioni MVC scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    *Passare alle visualizzazioni MVC scaffolding*
4. Nel **crea** visualizzare, fornire un **nome** e un **Age** per utente, fare clic su **crea**.

    ![Aggiunta di una nuova persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    *Aggiunta di una nuova persona*
5. Il nuovo utente viene aggiunto all'elenco. Nell'elenco degli elementi, fare clic su **dettagli** per visualizzare i dettagli della persona. Quindi, nel **dettagli** consente di visualizzare, fare clic su **elenco** per tornare alla visualizzazione elenco.

    ![Visualizzazione dei dettagli dell'utente](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    *Visualizzazione dei dettagli dell'utente*
6. Fare clic su di **eliminare** collegamento per eliminare l'utente. Nel **eliminare** consente di visualizzare, fare clic su **eliminare** per confermare l'operazione.

    ![L'eliminazione di un utente](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    *L'eliminazione di un utente*
7. Tornare a Visual Studio e premere **MAIUSC + F5** per arrestare il debug.

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a>Esercizio 3: Creazione di un Controller API Web mediante Scaffolding

Framework Web API fa parte dello Stack di ASP.NET e progettato per semplificare l'implementazione di servizi HTTP, in genere l'invio e ricezione di dati in formato JSON o XML tramite un'API RESTful.

In questo esercizio, si utilizzerà lo Scaffolding di ASP.NET nuovamente per generare un controller API Web. Si utilizzerà lo stesso **persona** e **PersonContext** classi nell'esercizio precedente per fornire gli stessi dati utente in formato JSON. Verrà visualizzato come è possibile esporre le stesse risorse in modi diversi nella stessa applicazione ASP.NET.

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a>Attività 1: creazione di un Controller API Web

In questa attività verrà creato un nuovo **Controller API Web** che esporrà i dati utente in un formato macchina utilizzabili come JSON.

1. Se non è già aperto, aprirlo **Visual Studio Express 2013 per Web** e aprire il **MyHybridSite.sln** soluzione si trova nel **origine/Ex3-WebAPI/inizio** cartella. In alternativa, è possibile continuare con la soluzione ottenuta nell'esercizio precedente.

    > [!NOTE]
    > Se si inizia con la soluzione Begin da esercizio 3, premere **CTRL + MAIUSC + B** per compilare la soluzione.
2. In **Esplora**, fare doppio clic sul **controller** cartella del **MyHybridSite** del progetto e selezionare **Aggiungi | Nuovo elemento di scaffolding...** .

    ![Creare un nuovo Controller di scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    *Creare un nuovo Controller di scaffolding*
3. Nel **aggiungere lo scaffolding** nella finestra di dialogo **API Web** nel riquadro a sinistra, quindi **Web 2 Controller API con azioni, mediante Entity Framework** nel riquadro centrale e quindi fare clic su  **Aggiungere.**

    ![Selezione di Controller di Web API 2 con azioni ed Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "la selezione di Controller API Web 2 con azioni ed Entity Framework")

    *Selezione di Controller di Web API 2 con azioni ed Entity Framework*
4. Impostare *ApiPersonController* come il **nome Controller**, selezionare il **utilizzare azioni asincrone del controller** opzione e selezionare **persona (MyHybridSite.Models)**  e **PersonContext (MyHybridSite.Models)** come il **modello** e **contesto dati** classi rispettivamente. Fare quindi clic su **Aggiungi**.

    ![Aggiunta di un Controller API Web con lo scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "aggiunta di un controller API Web con lo scaffolding")

    *Aggiunta di un controller API Web con lo scaffolding*
5. Visual Studio genera quindi il **ApiPersonController** classe con le quattro azioni CRUD per lavorare con i dati.

    ![Dopo aver creato il controller API Web con lo scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "dopo aver creato il controller API Web con lo scaffolding")

    *Dopo aver creato il controller API Web con lo scaffolding*
6. Aprire il **ApiPersonController.cs** file ed esaminare il *GetPeople* metodo di azione. Questo metodo esegue una query il campo di database di **PersonContext** tipo per ottenere le persone di dati.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. Osservare ora il commento sopra la definizione di metodo. Fornisce l'URI che espone questa azione che verrà utilizzato nell'attività successiva.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > Per impostazione predefinita, l'API Web è configurata per rilevare le query per il */api* percorso per evitare conflitti con i controller MVC. Se è necessario modificare questa impostazione, fare riferimento a [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a>Attività 2: esecuzione della soluzione

In questa attività verrà utilizzato Internet Explorer **strumenti di sviluppo F12** per controllare l'intera risposta dal controller API Web. Verrà visualizzato come è possibile acquisire il traffico di rete per ottenere informazioni più dettagliate sui dati dell'applicazione.

> [!NOTE]
> Assicurarsi che **Internet Explorer** è selezionata nel **avviare** pulsante Trova sulla barra degli strumenti di Visual Studio.
> 
> ![Opzione di Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> Il **strumenti di sviluppo F12** dispone di una vasta gamma di funzionalità non trattate in questa esercitazione pratica. Se si desidera ottenere altre informazioni, vedere [utilizzando gli strumenti di sviluppo F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).


1. Premere **F5** per eseguire la soluzione.

    > [!NOTE]
    > Per eseguire correttamente questa attività, l'applicazione deve disporre di dati. Se il database è vuoto, è possibile tornare all'attività 3 nell'esercizio 2 e seguire i passaggi su come creare un nuovo utente utilizzando le visualizzazioni MVC.
2. Nel browser, premere **F12** per aprire la **gli strumenti di sviluppo** pannello. Premere **CTRL** + **4** oppure fare clic su di **rete** icona e quindi fare clic su pulsante freccia verde per avviare l'acquisizione del traffico di rete.

    ![Avvio di acquisizione di rete di API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "acquisizione di rete di avvio di Web API")

    *Avvio di acquisizione di rete di API Web*
3. Aggiungere **api/ApiPerson** per l'URL nella barra degli indirizzi del browser. È ora esamina i dettagli della risposta di **ApiPersonController**.

    ![Il recupero dei dati utente tramite l'API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "il recupero dei dati utente tramite l'API Web")

    *Il recupero dei dati utente tramite l'API Web*

    > [!NOTE]
    > Una volta completato il download, verrà richiesto di effettuare un'azione con il file scaricato. Lasciare la finestra di dialogo aperta per poter visualizzare il contenuto della risposta tramite la finestra degli strumenti sviluppatori.
4. Ora si esamina il corpo della risposta. A tale scopo, fare clic su di **dettagli** scheda e quindi fare clic su **corpo della risposta**. È possibile verificare che i dati scaricati sono un elenco di oggetti con le proprietà **Id**, **nome** e **Age** che corrispondono al **persona** classe.

    ![La visualizzazione Web corpo della risposta API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "visualizzazione Web corpo della risposta API")

    *Corpo della risposta di API Web visualizzazione*

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a>Attività 3: aggiunta di pagine della Guida di API Web

Quando si crea un'API Web, è utile creare una pagina della Guida in modo che altri sviluppatori saranno in grado di chiamare l'API. È possibile creare e aggiornare manualmente le pagine della documentazione, ma è preferibile generare automaticamente in modo da evitare di dover eseguire operazioni di manutenzione. In questa attività si utilizzerà un pacchetto Nuget per generare automaticamente le pagine della Guida di API Web alla soluzione.

1. Dal **strumenti** menu in Visual Studio, selezionare **Gestione pacchetti libreria**, quindi fare clic su **Console di gestione pacchetti**.
2. Nel **Package Manager Console** finestra, eseguire il comando seguente:

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > Il **Microsoft.AspNet.WebApi.HelpPage** pacchetto installa gli assembly necessari e aggiunge le visualizzazioni MVC per le pagine della Guida nel **aree/HelpPage** cartella.

    ![Area HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage Area")

    *Area HelpPage*
3. Per impostazione predefinita, la Guida pagine sono stringhe di segnaposto per la documentazione. È possibile utilizzare i commenti della documentazione XML per creare la documentazione. Per abilitare questa funzionalità, aprire il **HelpPageConfig.cs** file si trova nella **HelpPage/aree/App\_avviare** cartella e rimuovere il commento la riga seguente:

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. In **Esplora**, fare clic sul progetto **MyHybridSite**selezionare **proprietà** e fare clic su di **compilare** scheda.

    ![Scheda Compila](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "compilare sezione")

    *Scheda Compila*
5. In **Output**selezionare **file di documentazione XML**. Nella casella di modifica, digitare **App\_Data/XmlDocument.xml**.

    ![Output sezione nella scheda compilazione](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "sezione nella scheda compilazione di Output")

    *Sezione di output nella scheda compilazione*
6. Premere **CTRL** + **S** per salvare le modifiche.
7. Aprire il **ApiPersonController.cs** file dal **controller** cartella.
8. Immettere una nuova riga tra il *GetPeople* firma del metodo e */ / ottenere api/ApiPerson* impostare come commento e quindi digitare tre barre.

    > [!NOTE]
    > Visual Studio inserisce automaticamente gli elementi XML che definiscono la documentazione relativa al metodo.
9. Aggiungere un testo di riepilogo e il valore restituito per il *GetPeople* metodo. Dovrebbe essere simile al seguente.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. Premere **F5** per eseguire la soluzione.
11. Aggiungere **/Help** per l'URL nella barra degli indirizzi per passare alla pagina della Guida.

    ![Pagina della Guida ASP.NET Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "pagina della Guida ASP.NET Web API")

    *Pagina della Guida ASP.NET Web API*

    > [!NOTE]
    > Il principale contenuto della pagina è una tabella delle API, raggruppate dal controller. Le voci della tabella vengono generate in modo dinamico, utilizzando il **IApiExplorer** interfaccia. Se si aggiunge o aggiorna un controller API, la tabella verrà automaticamente aggiornata la volta successiva che si compila l'applicazione.
    > 
    > Il **API** colonna elenca il metodo HTTP e un URI relativo. Il **descrizione** colonna contiene informazioni che sono stato estratto dalla documentazione del metodo.
12. Si noti che la descrizione che aggiunti sopra la definizione di metodo viene visualizzata nella colonna Descrizione.

    ![Descrizione del metodo API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "descrizione del metodo API")

    *Descrizione del metodo API*
13. Fare clic su uno dei metodi API per passare a una pagina con informazioni più dettagliate, tra cui corpi di risposta di esempio.

    ![Pagina informazioni di dettaglio](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "pagina informazioni di dettaglio")

    *Pagina informazioni dettagliate*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa esercitazione si è appreso come:

- Creare una nuova applicazione Web utilizzando un'esperienza di ASP.NET in Visual Studio 2013
- Integrare più tecnologie ASP.NET in un unico progetto
- Generare visualizzazioni di controller MVC e dalle classi modello mediante Scaffolding di ASP.NET
- Generare controller API Web, utilizzare le funzionalità, ad esempio di programmazione asincrona e l'accesso ai dati tramite Entity Framework
- Generare automaticamente le pagine della Guida di API Web per i controller
