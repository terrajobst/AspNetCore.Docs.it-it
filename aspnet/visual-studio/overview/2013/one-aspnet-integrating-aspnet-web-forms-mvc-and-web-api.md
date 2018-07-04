---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Laboratorio pratico: One ASP.NET: integrazione di Web Form ASP.NET, MVC e API Web | Microsoft Docs'
author: rick-anderson
description: ASP.NET è un framework per la creazione di siti Web, App e servizi mediante tecnologie specializzate, ad esempio MVC, API Web e ad altri utenti. A causa dell'espansione h ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7dec4daffa66621acaee1c76fda7b2e7550ad925
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382952"
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a>Laboratorio pratico: One ASP.NET: integrazione di Web Form ASP.NET, MVC e API Web
====================
da [Camp Web Team](https://twitter.com/webcamps)

[Download Web Camp Kit di formazione](http://aka.ms/webcamps-training-kit)

> ASP.NET è un framework per la creazione di siti Web, App e servizi mediante tecnologie specializzate, ad esempio MVC, API Web e ad altri utenti. A causa dell'espansione ASP.NET ha rilevato dopo la creazione e l'espresso necessario disporre di queste tecnologie integrate, a favore di sono state apportate le attività recenti **One ASP.NET**.
> 
> Visual Studio 2013 introduce un nuovo sistema di progetto unificato che consente di creare un'applicazione e usare tutte le tecnologie ASP.NET in un unico progetto. Questa funzionalità Elimina la necessità di scegliere una tecnologia all'inizio di un progetto e stilizzata con esso e invece consiglia l'utilizzo di più framework ASP.NET all'interno di un progetto.
> 
> Tutto il codice di esempio e frammenti di codice sono inclusi nel Web Camp Kit di formazione, disponibile all'indirizzo [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Panoramica

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico, si apprenderà come:

- Creare un sito Web basato sul **One ASP.NET** tipo di progetto
- Utilizzare diverse **ASP.NET** Framework come **MVC** e **API Web** nello stesso progetto
- Identificare i componenti principali di un' **ASP.NET** applicazione
- Sfruttare il **Scaffolding di ASP.NET** framework per creare automaticamente i controller e viste per eseguire operazioni CRUD di base le classi del modello
- Esporre lo stesso set di informazioni in formati macchina e leggibili utilizzando lo strumento appropriato per ogni processo

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Di seguito è necessario per completare questo laboratorio pratico:

- [Visual Studio Express 2013 per Web](https://www.microsoft.com/visualstudio/) o versione successiva
- [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

Per eseguire gli esercizi in questo laboratorio pratico, è necessario configurare prima di tutto l'ambiente.

1. Aprire Windows Explorer e passare del lab **origine** cartella.
2. Fare clic su **Setup. cmd** e selezionare **Esegui come amministratore** per avviare il processo di installazione che la configurazione dell'ambiente e installare i frammenti di codice di Visual Studio per questo ambiente lab.
3. Se viene visualizzata la finestra di dialogo controllo dell'Account utente, confermare l'azione per continuare.

> [!NOTE]
> Assicurarsi che tutte le dipendenze per questo ambiente lab che sono stati verificati prima di eseguire il programma di installazione.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Usando i frammenti di codice

In tutto il documento di laboratorio, verrà invitati a inserire blocchi di codice. Per praticità, la maggior parte di questo codice viene fornito come Visual Studio frammenti di codice che è possibile accedere da all'interno di Visual Studio 2013, per evitare che sia necessario aggiungerla manualmente.

> [!NOTE]
> Ogni esercizio è accompagnata da una soluzione inizia che si trova nel **iniziare** cartella dell'esercizio che consente di seguire ogni esercizio indipendentemente dagli altri. Tenere presente che i frammenti di codice aggiunti durante un esercizio non sono presenti queste soluzioni di avvio e potrebbero non funzionare fino a quando non si have completato l'esercizio. All'interno del codice sorgente per un esercizio, si noterà anche un **End** cartella che contiene una soluzione di Visual Studio con il codice che scaturisce da completare i passaggi nell'esercizio corrispondente. È possibile usare queste soluzioni come prassi consigliata se ti serve assistenza aggiuntiva durante l'esecuzione in questo laboratorio pratico.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questo laboratorio pratico include gli esercizi seguenti:

1. [Creazione di un nuovo progetto di Web Form](#Exercise1)
2. [Creazione di un Controller MVC con Scaffolding](#Exercise2)
3. [Creazione di un Controller API Web usando lo Scaffolding](#Exercise3)

Tempo stimato per completare questa esercitazione: **60 minuti**

> [!NOTE]
> Al primo avvio di Visual Studio, è necessario selezionare una delle raccolte di impostazioni predefinite. Ogni raccolta predefinita è pensata per associare un particolare stile di sviluppo e determina i layout delle finestre, il comportamento dell'editor, frammenti di codice IntelliSense e opzioni della finestra di dialogo. Le procedure descritte in questa esercitazione vengono descritte le azioni necessarie per eseguire una determinata attività in Visual Studio quando si usa la **delle impostazioni di sviluppo generale** raccolta. Se si sceglie una raccolta di impostazioni diverse per l'ambiente di sviluppo, possono essere presenti differenze nei passaggi che è necessario tenere conto.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a>Esercizio 1: Creazione di un nuovo progetto di Web Form

In questo esercizio si creerà un nuovo sito Web Form in Visual Studio 2013 usando il **One ASP.NET** unificata esperienza di progetto, che è possibile integrare facilmente i componenti di Web Form, MVC e API Web nella stessa applicazione. Si verrà quindi esplorare la soluzione generata e identificare le parti e infine noterete il sito Web in azione.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a>Attività 1: creazione di un nuovo sito usando un'esperienza di ASP.NET

In questa attività si avvierà la creazione di un nuovo sito Web in Visual Studio basato sul **One ASP.NET** tipo di progetto. **One ASP.NET** unifica tutte le tecnologie di ASP.NET e offre la possibilità di combinare e abbinare in base alle esigenze. Potrà quindi riconoscere i diversi componenti di Web Form, MVC e API Web che si trovano side-by-side all'interno dell'applicazione.

1. Aprire **Visual Studio Express 2013 per il Web** e selezionare **File | Nuovo progetto...**  per avviare una nuova soluzione.

    ![Creazione di un nuovo progetto](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    *Crea un nuovo progetto*
2. Nel **nuovo progetto** finestra di dialogo **applicazione Web ASP.NET** sotto il **Visual c# | Web** scheda e assicurarsi che **.NET Framework 4.5** sia selezionata. Denominare il progetto *MyHybridSite*, scegliere una **posizione** e fare clic su **OK**.

    ![Nuovo progetto di applicazione Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    *Crea un nuovo progetto di applicazione Web ASP.NET*
3. Nel **nuovo progetto ASP.NET** finestra di dialogo, seleziona la **Web Form** modello e selezionare il **MVC** e **API Web** opzioni. Inoltre, assicurarsi che il **Authentication** opzione è impostata su **singoli account utente di**. Fare clic su **OK** per continuare.

    ![Crea un nuovo progetto con il modello Web Form, inclusi i componenti MVC e API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    *Crea un nuovo progetto con il modello Web Form, inclusi i componenti MVC e API Web*
4. È ora possibile esplorare la struttura della soluzione generata.

    ![Esplorazione della soluzione generata](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    *Esplorazione della soluzione generata*

    1. **Account:** questa cartella contiene le pagine Web Form per registrare, accedere a e gestire gli account utente dell'applicazione. Questa cartella viene aggiunto quando la **singoli account utente di** autenticazione scelto durante la configurazione del modello di progetto Web Form.
    2. **I modelli:** questa cartella contiene le classi che rappresentano i dati dell'applicazione.
    3. **I controller** e **viste**: queste cartelle sono necessari per il **ASP.NET MVC** e **ASP.NET Web API** componenti. Si esaminerà le tecnologie MVC e API Web negli esercizi successivi.
    4. Il **default. aspx**, **aspx** e **About** file sono pagine Web Form predefinite che è possibile usare come punto di partenza per compilare le pagine specifiche per il applicazione. La logica di programmazione di tali file si trova in un file separato, detto il &quot;code-behind&quot; file, che ha un' &quot;. aspx&quot; o &quot;. aspx.cs&quot; estensione (a seconda di linguaggio utilizzato). La logica code-behind viene eseguito nel server e in modo dinamico produce l'output HTML della pagina.
    5. Il **Site. master** e **Site.Mobile.Master** pagine definiscono l'aspetto e il comportamento standard di tutte le pagine nell'applicazione.
5. Fare doppio clic il **default. aspx** file per esaminarne il contenuto della pagina.

    ![Esplorare la pagina default. aspx](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    *Esplorare la pagina default. aspx*

    > [!NOTE]
    > Il **pagina** direttiva all'inizio del file definisce gli attributi della pagina Web Form. Ad esempio, il **MasterPageFile** attributo specifica il percorso al master pagina - in questo caso, il *Site. master* pagina - e il **Inherits** attributo definisce il classe code-behind per la pagina in modo che erediti. Questa classe si trova nel file di base di **CodeBehind** attributo.
    > 
    > Il **asp: Content** controllo mantiene il contenuto effettivo della pagina (testo, markup e controlli) e viene eseguito il mapping a un **asp: ContentPlaceHolder** controllo nella pagina master. In questo caso, il contenuto della pagina rendering verrà eseguito all'interno di *MainContent* controllo definito nel *Site. master* pagina.
6. Espandere la **App\_avviare** cartella e notare che il **WebApiConfig.cs** file. Visual Studio incluso tale file nella soluzione generata perché l'API Web incluso quando si configura il progetto con il modello di One ASP.NET.
7. Aprire il **WebApiConfig.cs** file. Nel *WebApiConfig* troverai la configurazione associata all'API Web, che esegue il mapping HTTP classe indirizza al **controller API Web**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. Aprire il **RouteConfig.cs** file. All'interno di *RegisterRoutes* troverai la configurazione associata MVC, che esegue il mapping di route HTTP al metodo **controller MVC**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a>Attività 2: esecuzione della soluzione

In questa attività verranno di eseguire la soluzione generata, esplorare l'app e alcune delle funzionalità, ad esempio la riscrittura degli URL e autenticazione incorporata.

1. Per eseguire la soluzione, premere **F5** oppure fare clic sui **avviare** pulsante Trova sulla barra degli strumenti. Home page dell'applicazione deve aprire nel browser.

    ![Esecuzione della soluzione](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. Verificare che le pagine Web Form vengono richiamate. A tale scopo, aggiungere **/contact.aspx** per l'URL nella barra degli indirizzi e premere **invio**.

    ![URL brevi](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    *URL brevi*

    > [!NOTE]
    > Come può notare, l'URL viene modificato da **/contattare**. A partire da **ASP.NET 4**, funzionalità di routing di URL sono stati aggiunti a Web Form, pertanto è possibile scrivere gli URL, ad esempio *[ http://www.mysite.com/products/software ](http://www.mysite.com/products/software)* anziché  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*. Per altre informazioni fare riferimento a [Routing degli URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).
3. Verrà ora illustrato il flusso di autenticazione integrato nell'applicazione. A tale scopo, fare clic su **registrare** nell'angolo superiore destro della pagina.

    ![La registrazione di un nuovo utente](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    *La registrazione di un nuovo utente*
4. Nel **registrare** pagina, immettere una **nome utente** e **Password**, quindi fare clic su **registrare**.

    ![Pagina di registrazione](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    *Pagina di registrazione*
5. L'applicazione Registra nuovo account e l'utente viene autenticato.

    ![Autenticata utente](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    *Autenticata utente*
6. Tornare a Visual Studio e premere **MAIUSC+F5** per arrestare il debug.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a>Esercizio 2: Creazione di un Controller MVC con Scaffolding

In questo esercizio si sfrutterà il framework di Scaffolding di ASP.NET fornito da Visual Studio per creare un controller ASP.NET MVC 5 con visualizzazioni Razor per eseguire operazioni CRUD, senza scrivere una singola riga di codice e azioni. Il processo di scaffolding utilizzerà Code First di Entity Framework per generare il contesto dei dati e lo schema del database nel database SQL.

**Informazioni su Entity Framework Code First**

Entity Framework (EF) è un object relational mapper (ORM) che consente di creare applicazioni di accesso ai dati tramite programmazione con un modello di applicazione concettuale anziché programmando direttamente con uno schema di archiviazione relazionale.

Il flusso di lavoro di modellazione Code First di Entity Framework consente di usare le proprie classi di dominio per rappresentare il modello di Entity Framework si basa su durante l'esecuzione di query, le funzioni di rilevamento delle modifiche e l'aggiornamento. Usa il flusso di lavoro di sviluppo Code First, è necessario avviare l'applicazione mediante la creazione di un database o specificare uno schema. In alternativa, è possibile scrivere classi .NET standard che definiscono gli oggetti del modello di dominio più appropriati per l'applicazione ed Entity Framework creerà il database per l'utente.

> [!NOTE]
> Altre informazioni su Entity Framework [qui](../../../entity-framework.md).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a>Attività 1: creazione di un nuovo modello

Verrà ora definita una **persona** (classe), che sarà il modello usato dal processo di scaffolding per creare le visualizzazioni e controller MVC. Inizierà creando un **persona** classe di modello e le operazioni CRUD nel controller verranno automaticamente creata utilizzando le funzionalità di scaffolding.

1. Aprire **Visual Studio Express 2013 per il Web** e il **MyHybridSite.sln** soluzione che si trova nel **inizio/origine/Ex2-MvcScaffolding** cartella. In alternativa, è possibile continuare con la soluzione ottenuta nell'esercizio precedente.
2. In **Esplora soluzioni**, fare doppio clic sui **modelli** cartella della **MyHybridSite** del progetto e selezionare **Add | Classe...** .

    ![Aggiunta di una classe modello persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    *Aggiunta di una classe modello persona*
3. Nel **Aggiungi nuovo elemento** finestra di dialogo, denominare il file *Person.cs* e fare clic su **Add**.

    ![Creazione della classe di modello di persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    *Creazione della classe di modello di persona*
4. Sostituire il contenuto di **Person.cs** file con il codice seguente. Premere **CTRL + S** per salvare le modifiche.

    (Code - Snippet *BringingTogetherOneAspNet - Ex2 - PersonClass*)

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. In **Esplora soluzioni**, fare doppio clic il **MyHybridSite** del progetto e selezionare **compilare**, oppure premere **CTRL + MAIUSC + B** per compilare il progetto.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a>Attività 2: creazione di un Controller MVC

Ora che il **Person** creazione del modello, si utilizzerà lo scaffolding di ASP.NET MVC con Entity Framework per creare le azioni del controller CRUD e le viste per **persona**.

1. In **Esplora soluzioni**, fare doppio clic sul **controller** cartella della **MyHybridSite** del progetto e selezionare **Add | Nuovo elemento di scaffolding...** .

    ![Creare un nuovo Controller sottoposto a scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    *Creazione di un nuovo Controller sottoposto a scaffolding*
2. Nel **Add Scaffold** finestra di dialogo **Controller MVC 5 con visualizzazioni, mediante Entity Framework** e quindi fare clic su **Add.**

    ![Selezione di Controller MVC 5 con visualizzazioni ed Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    *Selezione di Controller MVC 5 con visualizzazioni ed Entity Framework*
3. Impostare *MvcPersonController* come la **nome del Controller**, selezionare il **usano le azioni del controller asincrono** opzione e selezionare **persona (MyHybridSite.Models)**  come il **classe del modello**.

    ![Aggiunta di un controller MVC con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    *Aggiunta di un controller MVC con scaffolding*
4. Sotto **alla classe contesto dati**, fare clic su **nuovo contesto dati...** .

    ![Creazione di un nuovo contesto dati](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    *Creazione di un nuovo contesto dati*
5. Nel **nuovo contesto dati** della finestra di dialogo Nome nuovo contesto dati *PersonContext* e fare clic su **Add**.

    ![Creare il nuovo PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    *Creazione del nuovo tipo PersonContext*
6. Fare clic su **Add** per creare il nuovo controller per **persona** con scaffolding. Visual Studio genera quindi le azioni del controller, il contesto dei dati utente e le visualizzazioni Razor.

    ![Dopo aver creato il controller MVC con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    *Dopo aver creato il controller MVC con scaffolding*
7. Aprire il **MvcPersonController.cs** del file nei **controller** cartella. Si noti che i metodi di azione CRUD siano stati generati automaticamente.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > Selezionando il **usare le azioni del controller asincrono** casella di controllo dallo scaffolding delle opzioni nei passaggi precedenti, Visual Studio genera metodi di azione asincroni per tutte le azioni che comportano l'accesso al contesto dati Person. È consigliabile che si usano metodi di azione asincroni per a esecuzione prolungata, non associate alla CPU le richieste per evitare di bloccare il server Web di esecuzione del lavoro mentre viene elaborata la richiesta.

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a>Attività 3: esecuzione della soluzione

In questa attività si eseguiranno la soluzione per verificare che le viste per **persona** funzionino come previsto. Si aggiungerà una nuova persona per verificare che venga salvato correttamente nel database.

1. Premere **F5** per eseguire la soluzione.
2. Passare a **/MvcPerson**. La visualizzazione sottoposto a scaffolding che mostra l'elenco degli utenti dovrebbe essere visualizzato.
3. Fare clic su **Crea nuovo** per aggiungere una nuova persona.

    ![Passare alle visualizzazioni MVC con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    *Passare alle visualizzazioni MVC con scaffolding*
4. Nel **Create** consente di visualizzare, fornire un **nome** e un **Age** per la persona e fare clic su **crea**.

    ![Aggiunta di una nuova persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    *Aggiunta di una nuova persona*
5. Il nuovo utente viene aggiunto all'elenco. Nell'elenco di elementi, fare clic su **dettagli** per visualizzare i dettagli di una persona. Quindi, nella **informazioni dettagliate** consente di visualizzare, fare clic su **torna all'elenco** per tornare alla visualizzazione elenco.

    ![Visualizzazione dei dettagli della persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    *Visualizzazione dei dettagli della persona*
6. Fare clic sui **eliminare** collegamento per eliminare l'utente. Nel **eliminare** consente di visualizzare, fare clic su **eliminare** per confermare l'operazione.

    ![L'eliminazione di un utente](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    *L'eliminazione di un utente*
7. Tornare a Visual Studio e premere **MAIUSC+F5** per arrestare il debug.

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a>Esercizio 3: Creazione di un Controller API Web usando lo Scaffolding

Il framework API Web fa parte dello Stack di ASP.NET e progettato per semplificare l'implementazione di servizi HTTP, in genere l'invio e ricezione di dati in formato JSON o XML tramite un'API RESTful.

In questo esercizio, si utilizzerà lo Scaffolding di ASP.NET nuovamente per generare un controller API Web. Si userà lo stesso **Person** e **PersonContext** classi nell'esercizio precedente per fornire gli stessi dati di persona in formato JSON. Verrà visualizzato come esporre le stesse risorse in modi diversi della stessa applicazione ASP.NET.

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a>Attività 1: creazione di un Controller API Web

In questa attività si creerà una nuova **Controller Web API** che espone i dati di persona in un formato di consumo macchina come JSON.

1. Se non è già aperto, aprire **Visual Studio Express 2013 per il Web** e aprire il **MyHybridSite.sln** soluzione che si trova nel **inizio/origine/Ex3-WebAPI** cartella. In alternativa, è possibile continuare con la soluzione ottenuta nell'esercizio precedente.

    > [!NOTE]
    > Se si inizia con la soluzione di inizio compresa tra 3 esercizio, premere **CTRL + MAIUSC + B** per compilare la soluzione.
2. In **Esplora soluzioni**, fare doppio clic sul **controller** cartella della **MyHybridSite** del progetto e selezionare **Add | Nuovo elemento di scaffolding...** .

    ![Creare un nuovo Controller sottoposto a scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    *Creare un nuovo Controller sottoposto a scaffolding*
3. Nel **Add Scaffold** finestra di dialogo **API Web** nel riquadro sinistro, quindi **Controller Web API 2 con azioni, che usa Entity Framework** nel riquadro centrale e quindi fare clic su  **Aggiungere.**

    ![Selezionare Controller Web API 2 con azioni ed Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "selezionare Controller Web API 2 con azioni ed Entity Framework")

    *Selezionare Controller Web API 2 con azioni ed Entity Framework*
4. Impostare *ApiPersonController* come la **nome del Controller**, selezionare il **usano le azioni del controller asincrono** opzione e selezionare **persona (MyHybridSite.Models)**  e **PersonContext (MyHybridSite.Models)** come il **Model** e **contesto dati** rispettivamente le classi. Fare quindi clic su **Aggiungi**.

    ![Aggiunta di un Controller API Web con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "aggiunta di un controller API Web con scaffolding")

    *Aggiunta di un controller API Web con scaffolding*
5. Visual Studio genera quindi il **ApiPersonController** classe con le quattro azioni CRUD per lavorare con i dati.

    ![Dopo aver creato il controller Web API con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "dopo aver creato il controller Web API con scaffolding")

    *Dopo aver creato il controller Web API con scaffolding*
6. Aprire il **ApiPersonController.cs** del file ed esaminare le *GetPeople* metodo di azione. Questo metodo esegue una query il campo di database di **PersonContext** tipo per ottenere le persone i dati.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. A questo punto si noti che il commento sopra la definizione del metodo. Fornisce l'URI che espone questa azione che verrà usato nell'attività successiva.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > Per impostazione predefinita, l'API Web è configurata per intercettare le query per il */api* path per evitare conflitti con i controller MVC. Se è necessario modificare questa impostazione, vedere [Routing in API Web ASP.NET](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a>Attività 2: esecuzione della soluzione

In questa attività si userà di Internet Explorer **strumenti di sviluppo F12** per controllare la risposta completa da controller API Web. Verrà visualizzato come è possibile acquisire il traffico di rete per ottenere informazioni più dettagliate sui dati dell'applicazione.

> [!NOTE]
> Verificare che l'opzione **Internet Explorer** sia selezionato nel **avviare** pulsante Trova sulla barra degli strumenti di Visual Studio.
> 
> ![Opzione di Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> Il **strumenti di sviluppo F12** hanno un ampio set di funzionalità non trattate in questo-pratica. Se si vuole ottenere altre informazioni, vedere [usando gli strumenti di sviluppo F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).


1. Premere **F5** per eseguire la soluzione.

    > [!NOTE]
    > Per seguire correttamente questa attività, l'applicazione deve disporre dei dati. Se il database è vuoto, è possibile tornare all'attività 3 nell'esercizio 2 e seguire i passaggi su come creare una nuova persona usando le visualizzazioni MVC.
2. Nel browser, premere **F12** per aprire il **strumenti di sviluppo** pannello. Premere **CTRL** + **4** oppure fare clic sui **rete** icona e quindi scegliere la freccia verde sul pulsante per avviare l'acquisizione del traffico di rete.

    ![Avvio di acquisizione di rete di API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "acquisizione di rete di avvio di API Web")

    *Avvio di acquisizione di rete di API Web*
3. Accodare **api/ApiPerson** all'URL nella barra degli indirizzi del browser. A questo punto si analizzeranno i dettagli della risposta dei **ApiPersonController**.

    ![Il recupero dei dati utente tramite l'API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "il recupero dei dati utente tramite l'API Web")

    *Il recupero dei dati utente tramite l'API Web*

    > [!NOTE]
    > Una volta completato il download, verrà richiesto di eseguire un'azione con il file scaricato. Lasciare la finestra di dialogo aperta per poter visualizzare il contenuto della risposta tramite la finestra degli strumenti di sviluppatori.
4. A questo punto si analizzeranno il corpo della risposta. A questo scopo, scegliere il **informazioni dettagliate** scheda e quindi fare clic su **corpo della risposta**. È possibile controllare che i dati scaricati sono un elenco di oggetti con la proprietà **Id**, **Name** e **Age** che corrispondono al **persona** classe.

    ![Visualizzazione Web corpo della risposta dell'API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "visualizzazione Web corpo della risposta dell'API")

    *Corpo della risposta dell'API Web di visualizzazione*

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a>Attività 3: aggiunta di pagine della Guida di API Web

Quando si crea un'API Web, è utile creare una pagina della Guida in modo che altri sviluppatori saranno in grado di chiamare l'API. È possibile creare e aggiornare manualmente le pagine di documentazione, ma è preferibile generare automaticamente in modo da evitare di dover eseguire operazioni di manutenzione. In questa attività si userà un pacchetto Nuget per generare automaticamente le pagine della Guida di API Web alla soluzione.

1. Dal **degli strumenti** dal menu di Visual Studio, selezionare **Library Package Manager**e quindi fare clic su **Package Manager Console**.
2. Nel **Console di gestione pacchetti** finestra, eseguire il comando seguente:

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > Il **Microsoft.AspNet.WebApi.HelpPage** pacchetto installa gli assembly necessari e aggiunge le visualizzazioni MVC per le pagine della Guida sotto il **aree/HelpPage** cartella.

    ![Area HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage Area")

    *Area HelpPage*
3. Per impostazione predefinita, la Guida in linea nelle pagine siano stringhe di segnaposto per la documentazione. È possibile utilizzare i commenti della documentazione XML per creare la documentazione. Per abilitare questa funzionalità, aprire il **HelpPageConfig.cs** file che si trova nel **aree/HelpPage/App\_avviare** cartella e rimuovere il commento la riga seguente:

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. Nelle **Esplora soluzioni**, fare clic sul progetto **MyHybridSite**, selezionare **proprietà** e fare clic sul **compilare** scheda.

    ![Scheda compilazione](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Build sezione")

    *Scheda compilazione*
5. Sotto **Output**, selezionare **file di documentazione XML**. Nella casella di modifica, digitare **App\_Data/XmlDocument.xml**.

    ![Nella scheda Build sezione output](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "sezione scheda compilazione nella finestra di Output")

    *Sezione di output nella scheda compilazione*
6. Premere **CTRL** + **S** per salvare le modifiche.
7. Aprire il **ApiPersonController.cs** del file dal **controller** cartella.
8. Immettere una nuova riga tra il *GetPeople* firma del metodo e il */ / ottenere api/ApiPerson* come commento e quindi digitare tre barre.

    > [!NOTE]
    > Visual Studio inserisce automaticamente gli elementi XML che definiscono la documentazione del metodo.
9. Aggiungere un testo di riepilogo e il valore restituito per il *GetPeople* (metodo). Dovrebbe essere simile al seguente.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. Premere **F5** per eseguire la soluzione.
11. Accodare **/Help** per l'URL nella barra degli indirizzi per passare alla pagina della Guida.

    ![Pagina della Guida ASP.NET Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "pagina della Guida ASP.NET Web API")

    *Pagina della Guida ASP.NET Web API*

    > [!NOTE]
    > Il principale contenuto della pagina è una tabella delle API, raggruppate dal controller. Le voci della tabella vengono generate in modo dinamico, usando il **IApiExplorer** interfaccia. Se si aggiunge o aggiorna un controller API, la tabella verrà automaticamente aggiornata la volta successiva che si compila l'applicazione.
    > 
    > Il **API** colonna elenca l'URI relativo e il metodo HTTP. Il **descrizione** colonna contiene informazioni che sono stato estratto dalla documentazione relativa al metodo.
12. Si noti che la descrizione che è stato aggiunto di sopra la definizione del metodo viene visualizzata nella colonna Descrizione.

    ![Descrizione del metodo API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "descrizione del metodo API")

    *Descrizione del metodo API*
13. Fare clic su uno dei metodi API per passare a una pagina con informazioni più dettagliate, tra cui corpi di risposta di esempio.

    ![Pagina informazioni dettagliate](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "pagina informazioni dettagliate")

    *Pagina informazioni dettagliate*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa esercitazione pratica si è appreso come:

- Creare una nuova applicazione Web tramite un'esperienza di ASP.NET in Visual Studio 2013
- Integrare più tecnologie di ASP.NET in un singolo progetto
- Generare controlli e visualizzazioni MVC da classi di modelli usando lo Scaffolding di ASP.NET
- Generare i controller API Web, che usano funzionalità, ad esempio di programmazione asincrona e accesso ai dati tramite Entity Framework
- Generare automaticamente pagine della Guida dell'API Web per i controller
