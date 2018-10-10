---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: "Laboratorio pratico: Creare un'applicazione a pagina singola (SPA) con l'API Web ASP.NET e Angular. js | Microsoft Docs"
author: rick-anderson
description: Nelle applicazioni web tradizionali, il client (browser) avvia la comunicazione con il server richiede una pagina. Il server elabora quindi la richiesta...
ms.author: riande
ms.date: 09/30/2015
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 3c3557bb2be2807b11874937fcc629b5b773e463
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912254"
---
<a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>Laboratorio pratico: Creare un'applicazione a pagina singola (SPA) con l'API Web ASP.NET e Angular. js
====================
da [Camp Web Team](https://twitter.com/webcamps)

[Download Web Camp Kit di formazione](http://aka.ms/webcamps-training-kit)

> Nelle applicazioni web tradizionali, il client (browser) avvia la comunicazione con il server richiede una pagina. Quindi, il server elabora la richiesta e invia il codice HTML della pagina al client. Nelle successive interazioni con la pagina, ad esempio, l'utente passa a un collegamento o invia un modulo con i dati, viene inviata una richiesta di nuovo al server e il flusso viene riavviata: il server elabora la richiesta e invia una nuova pagina nel browser in risposta a nuova richiesta di azione ed dal client.
> 
> Nelle applicazioni a pagina singola (SPAs) l'intera pagina viene caricata nel browser dopo la richiesta iniziale, ma le interazioni successive hanno luogo tramite le richieste Ajax. Questo significa che il browser aggiornare solo la parte della pagina a cui è stato modificato; non è necessario ricaricare la pagina intera. L'approccio SPA consente di ridurre il tempo impiegato dall'applicazione per rispondere alle azioni dell'utente, un'esperienza più fluida.
> 
> L'architettura di un'applicazione a singola pagina comporta alcuni problemi che non sono presenti nelle applicazioni web tradizionali. Tuttavia, nuove tecnologie, ad esempio API Web ASP.NET, come i framework JavaScript AngularJS e nuove funzionalità di applicazione degli stili fornite da CSS3 rendono molto semplice progettare e compilare SPAs.
> 
> In questo lab pratici, si sarà possibile avvalersi di queste tecnologie per implementare fanatico Quiz, un sito Web gli elementi semplici basato sul concetto di applicazione a singola pagina. Prima di tutto si implementerà il livello di servizio con l'API Web ASP.NET per esporre gli endpoint necessari per recuperare le domande di quiz e archiviare le risposte. Quindi, si compilerà un'interfaccia utente avanzata e reattiva usando AngularJS e CSS3 effetti di trasformazione.
> 
> Tutto il codice di esempio e frammenti di codice sono inclusi nel Web Camp Kit di formazione, disponibile all'indirizzo [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


## <a name="overview"></a>Panoramica

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico, si apprenderà come:

- Creare un servizio API Web ASP.NET per inviare e ricevere i dati JSON
- Creare un'interfaccia utente reattiva usando AngularJS
- Migliorare l'esperienza dell'interfaccia utente con le trasformazioni di CSS3

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Di seguito è necessario per completare questo laboratorio pratico:

- [Visual Studio Express 2013 per Web](https://www.microsoft.com/visualstudio/) o versione successiva

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

1. [Creazione di un'API Web](#Exercise1)
2. [Creazione di un'interfaccia SPA](#Exercise2)

Tempo stimato per completare questa esercitazione: **60 minuti**

> [!NOTE]
> Al primo avvio di Visual Studio, è necessario selezionare una delle raccolte di impostazioni predefinite. Ogni raccolta predefinita è pensata per associare un particolare stile di sviluppo e determina i layout delle finestre, il comportamento dell'editor, frammenti di codice IntelliSense e opzioni della finestra di dialogo. Le procedure descritte in questa esercitazione vengono descritte le azioni necessarie per eseguire una determinata attività in Visual Studio quando si usa la **delle impostazioni di sviluppo generale** raccolta. Se si sceglie una raccolta di impostazioni diverse per l'ambiente di sviluppo, possono essere presenti differenze nei passaggi che è necessario tenere conto.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>Esercizio 1: Creazione di un'API Web

Una delle parti principali di un'applicazione a singola pagina è il livello di servizio. È responsabile dell'elaborazione di chiamate Ajax inviate dell'interfaccia utente e i dati restituiti in risposta a questa chiamata. I dati recuperati dovrebbero essere visualizzati in un formato leggibile al computer per poter essere analizzato e utilizzato dal client.

Il framework API Web fa parte dello Stack di ASP.NET ed è progettato per renderlo più facile da implementare servizi HTTP, in genere l'invio e ricezione di dati in formato JSON o XML tramite un'API RESTful. In questo esercizio si creerà il sito Web per ospitare l'applicazione fanatico Quiz e quindi implementare il servizio back-end per esporre e rendere persistenti i dati di test usando l'API Web ASP.NET.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>Attività 1: creazione del progetto iniziale per fanatico Quiz

In questa attività si avvierà la creazione di un nuovo progetto MVC ASP.NET con supporto per API Web ASP.NET in base il **One ASP.NET** tipo fornito con Visual Studio di progetto. **One ASP.NET** unifica tutte le tecnologie di ASP.NET e offre la possibilità di combinare e abbinare in base alle esigenze. Si aggiungerà quindi initializator il database per inserire le domande di quiz e classi del modello di Entity Framework.

1. Aprire **Visual Studio Express 2013 per il Web** e selezionare **File | Nuovo progetto...**  per avviare una nuova soluzione.

    ![Crea un nuovo progetto](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "creando un nuovo progetto")

    *Crea un nuovo progetto*
2. Nel **nuovo progetto** finestra di dialogo **applicazione Web ASP.NET** sotto il **Visual c# | Web** scheda. Assicurarsi che **.NET Framework 4.5** è selezionata, denominarla *GeekQuiz*, scegliere un **posizione** e fare clic su **OK**.

    ![Crea un nuovo progetto di applicazione Web ASP.NET](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "creando un nuovo progetto di applicazione Web ASP.NET")

    *Crea un nuovo progetto di applicazione Web ASP.NET*
3. Nel **nuovo progetto ASP.NET** finestra di dialogo, seleziona la **MVC** modello e selezionare il **API Web** opzione. Inoltre, assicurarsi che il **Authentication** opzione è impostata su **singoli account utente di**. Fare clic su **OK** per continuare.

    ![Crea un nuovo progetto con il modello MVC, inclusi i componenti di API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *Crea un nuovo progetto con il modello MVC, inclusi i componenti di API Web*
4. In **Esplora soluzioni**, fare doppio clic sui **modelli** cartella della **GeekQuiz** del progetto e selezionare **Add | Elemento esistente...** .

    ![Aggiunta di un elemento esistente](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "aggiungendo un elemento esistente")

    *Aggiunta di un elemento esistente*
5. Nel **Aggiungi elemento esistente** finestra di dialogo passare alle **origine/risorse/modelli** cartella e selezionare tutti i file. Fare clic su **Aggiungi**.

    ![Aggiungere le risorse di modello](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "aggiungendo le risorse modello")

    *Aggiungere le risorse modello*

    > [!NOTE]
    > Aggiungendo questi file, si aggiunge il modello di dati, il contesto di database di Entity Framework e l'inizializzatore del database per l'applicazione fanatico Quiz.
    > 
    > **Entity Framework (EF)** è un object relational mapper (ORM) che consente di creare applicazioni di accesso ai dati tramite programmazione con un modello di applicazione concettuale anziché programmando direttamente con uno schema di archiviazione relazionale. Altre informazioni su Entity Framework [qui](../../../entity-framework.md).
    > 
    > Di seguito è una descrizione delle classi che appena aggiunto:
    > 
    > - **TriviaOption:** rappresenta una singola opzione associata a una domanda quiz
    > - **: TriviaQuestion** rappresenta una domanda quiz ed espone le opzioni associate tramite i **opzioni** proprietà
    > - **TriviaAnswer:** rappresenta l'opzione selezionata dall'utente in risposta a una domanda quiz
    > - **TriviaContext:** rappresenta il contesto di database di Entity Framework dell'applicazione fanatico Quiz. Questa classe deriva da **DContext** ed espone **DbSet** proprietà che rappresentano raccolte di entità descritta in precedenza.
    > - **: TriviaDatabaseInitializer** l'implementazione dell'inizializzatore di Entity Framework per il **TriviaContext** classe che eredita da **CreateDatabaseIfNotExists**. Il comportamento predefinito di questa classe per creare il database solo se non esiste, l'inserimento di entità specificato nella **Seed** (metodo).
6. Aprire il **Global.asax.cs** file e aggiungere la seguente istruzione using.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. Aggiungere il codice seguente all'inizio del **Application\_avviare** per impostare il **TriviaDatabaseInitializer** come l'inizializzatore del database.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. Modificare il **Home** controller per limitare l'accesso per gli utenti autenticati. A tale scopo, aprire il **HomeController.cs** all'interno del file il **controller** cartella e aggiungere il **Authorize** dell'attributo il **HomeController**definizione di classe.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > Il **Authorize** filtrare controlla se l'utente viene autenticato. Se l'utente non è autenticato, restituisce il codice di stato HTTP 401 (non autorizzato) senza richiamare l'azione. È possibile applicare il filtro a livello globale, a livello di controller o a livello di singole azioni.
9. A questo punto si personalizzerà il layout delle pagine web e le informazioni di personalizzazione. A tale scopo, aprire il  **\_layout. cshtml** file all'interno di **viste | Shared** cartella e aggiornare il contenuto del **&lt;titolo&gt;** elemento sostituendo *My ASP.NET Application* con *fanatico Quiz* .

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. Nello stesso file, aggiornare la barra di spostamento tramite la rimozione il *sulle* e *Contact* collegamenti e ridenominazione il *Home* collegare a *riprodurre*. Inoltre, rinominare il *nome dell'applicazione* collegare *fanatico Quiz*. Il codice HTML per la barra di spostamento dovrebbe essere simile al codice seguente.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. Aggiornare il piè di pagina della pagina di layout, sostituendo *My ASP.NET Application* con *fanatico Quiz*. A tale scopo, sostituire il contenuto di **&lt;piè di pagina&gt;** elemento con il codice evidenziato seguente.

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>Attività 2: creazione di API Web TriviaController

Nell'attività precedente è creata la struttura iniziale dell'applicazione web Quiz fanatico. A questo punto si creerà un semplice servizio API Web che interagisce con il modello di dati del quiz ed espone le azioni seguenti:

- **GET/api/gli elementi semplici**: recupera la domanda successiva nell'elenco del quiz la risposta da parte dell'utente autenticato.
- **POST/api/gli elementi semplici**: archivia le risposte di quiz specificata dall'utente autenticato.

Si userà gli strumenti di Scaffolding di ASP.NET forniti da Visual Studio per creare la linea di base per la classe controller API Web.

1. Aprire il **WebApiConfig.cs** file all'interno di **App\_avviare** cartella. Questo file definisce la configurazione del servizio API Web, ad esempio come le route vengono mappate alle azioni del controller API Web.
2. Aggiungere la seguente istruzione using all'inizio del file.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. Aggiungere il codice evidenziato seguente per il **registrare** metodo per configurare a livello globale il formattatore per i dati JSON recuperati dai metodi di azione API Web.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > Il **CamelCasePropertyNamesContractResolver** converte automaticamente i nomi delle proprietà per *camel* case, che rappresenta le convenzioni generali per i nomi delle proprietà in JavaScript.
4. In **Esplora soluzioni**, fare doppio clic sul **controller** cartella della **GeekQuiz** del progetto e selezionare **Add | Nuovo elemento di scaffolding...** .

    ![Creazione di un nuovo elemento di scaffolding](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "creando un nuovo elemento di scaffolding")

    *Creazione di un nuovo elemento di scaffolding*
5. Nel **Add Scaffold** finestra di dialogo, assicurarsi che le **comuni** nodo è selezionato nel riquadro sinistro. Selezionare quindi il **Web API 2 Controller - Empty** modello nel riquadro centrale e fare clic su **Add**.

    ![Selezione del modello di Web API 2 Controller vuoto](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "selezionando il modello Web API 2 Controller vuoto")

    *Selezione del modello di Web API 2 Controller vuoto*

    > [!NOTE]
    > **Scaffolding di ASP.NET** è un framework di generazione di codice per applicazioni Web ASP.NET. Visual Studio 2013 include generatori di codice pre-installato per i progetti MVC e API Web. Quando si desidera aggiungere rapidamente codice che interagisce con i modelli di dati per ridurre la quantità di tempo richiesto per sviluppare le operazioni sui dati standard, è necessario utilizzare lo scaffolding nel progetto.
    > 
    > Il processo di scaffolding assicura anche che tutte le dipendenze necessarie siano installate nel progetto. Ad esempio, se si avvia con un progetto ASP.NET vuoto e quindi Usa lo scaffolding per aggiungere un controller API Web, i pacchetti NuGet di API Web e i riferimenti vengono aggiunti al progetto automaticamente.
6. Nel **Aggiungi Controller** finestra di dialogo, digitare *TriviaController* nel **nome Controller** casella di testo e fare clic su **Aggiungi**.

    ![Aggiunta di Controller di elementi semplici](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "aggiungendo il Controller di elementi semplici")

    *Aggiunta di Controller di elementi semplici*
7. Il **TriviaController.cs** file viene quindi aggiunto al **controller** cartella della **GeekQuiz** progetto, contenente un oggetto vuoto **TriviaController** classe. Aggiungere le seguenti istruzioni using all'inizio del file.

    (Code - Snippet *AspNetWebApiSpa - Ex1 - TriviaControllerUsings*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. Aggiungere il codice seguente all'inizio del **TriviaController** classe per definire, inizializzare e dispose il **TriviaContext** istanza nel controller.

    (Code - Snippet *AspNetWebApiSpa - Ex1 - TriviaControllerContext*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > Il **Dispose** metodo **TriviaController** richiama il **Dispose** metodo del **TriviaContext** istanza, che assicura che tutti i le risorse usate dall'oggetto di contesto vengono rilasciate quando la **TriviaContext** istanza viene eliminata o sottoposto a garbage collection. Ciò include la chiusura di tutte le connessioni di database aperte da Entity Framework.
9. Aggiungere il metodo helper seguente alla fine del **TriviaController** classe. Questo metodo recupera i seguenti aspetti di quiz dal database la risposta da parte dell'utente specificato.

    (Code - Snippet *AspNetWebApiSpa - Ex1 - TriviaControllerNextQuestion*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. Aggiungere il codice seguente **ottenere** metodo di azione per il **TriviaController** classe. Questo metodo di azione chiama il **NextQuestionAsync** metodo helper definita nel passaggio precedente per recuperare la domanda successiva per l'utente autenticato.

    (Code - Snippet *AspNetWebApiSpa - Ex1 - TriviaControllerGetAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. Aggiungere il metodo helper seguente alla fine del **TriviaController** classe. Questo metodo archivia la risposta specificata nel database e restituisce un valore booleano che indica se la risposta è corretta.

    (Code - Snippet *AspNetWebApiSpa - Ex1 - TriviaControllerStoreAsync*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. Aggiungere il codice seguente **Post** metodo di azione per il **TriviaController** classe. Questo metodo di azione associa la risposta per l'utente autenticato e chiama il **StoreAsync** metodo helper. Quindi, invia una risposta con il valore booleano restituito dal metodo di supporto.

    (Code - Snippet *AspNetWebApiSpa - Ex1 - TriviaControllerPostAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. Modificare il controller Web API per limitare l'accesso agli utenti autenticati mediante l'aggiunta di **Authorize** attributo il **TriviaController** definizione della classe.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Attività 3: esecuzione della soluzione

In questa attività si verificherà che il servizio API Web che è stata compilata nell'attività precedente funziona come previsto. Si userà di Internet Explorer **strumenti di sviluppo F12** per acquisire il traffico di rete ed esaminare l'intera risposta dal servizio API Web.

> [!NOTE]
> Verificare che l'opzione **Internet Explorer** sia selezionato nel **avviare** pulsante Trova sulla barra degli strumenti di Visual Studio.
> 
> ![Opzione di Internet Explorer](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)


1. Premere **F5** per eseguire la soluzione. Il **Accedi** schermata dovrebbe essere visualizzata nel browser.

    > [!NOTE]
    > All'avvio dell'applicazione, viene attivata la route MVC predefinita, che per impostazione predefinita viene eseguito il mapping per il **indice** azione delle **HomeController** classe. Poiché **HomeController** è limitato agli utenti autenticati (tenere presente che è decorato di tale classe con il **autorizza** attributo nell'esercizio 1) ed è presente alcun utente autenticato ancora, l'applicazione Reindirizza la richiesta originale nella pagina di accesso.

    ![Esecuzione della soluzione](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "esecuzione della soluzione")

    *Esecuzione della soluzione*
2. Fare clic su **registrare** per creare un nuovo utente.

    ![La registrazione di un nuovo utente](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "la registrazione di un nuovo utente")

    *La registrazione di un nuovo utente*
3. Nel **registrare** pagina, immettere una **nome utente** e **Password**, quindi fare clic su **registrare**.

    ![Pagina Registra](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "pagina registrazione")

    *Pagina di registrazione*
4. L'applicazione Registra nuovo account e l'utente viene autenticato e reindirizzato alla home page.

    ![Utente autenticato](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "utente autenticato")

    *Autenticazione dell'utente*
5. Nel browser, premere **F12** per aprire il **strumenti di sviluppo** pannello. Premere **CTRL + 4** oppure fare clic sui **rete** icona e quindi scegliere la freccia verde sul pulsante per avviare l'acquisizione del traffico di rete.

    ![Avvio di acquisizione di rete di API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "acquisizione di rete di avvio di API Web")

    *Avvio di acquisizione di rete di API Web*
6. Accodare **api/gli elementi semplici** all'URL nella barra degli indirizzi del browser. A questo punto si analizzeranno i dettagli della risposta dei **ottenere** metodo di azione **TriviaController**.

    ![Recupero dei dati tramite l'API Web domanda successivi](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "recuperando i dati di domanda successivi tramite API Web")

    *Recupero dei dati domanda successivi tramite API Web*

    > [!NOTE]
    > Una volta completato il download, verrà richiesto di eseguire un'azione con il file scaricato. Lasciare la finestra di dialogo aperta per poter visualizzare il contenuto della risposta tramite la finestra degli strumenti di sviluppatori.
7. A questo punto si analizzeranno il corpo della risposta. A questo scopo, scegliere il **informazioni dettagliate** scheda e quindi fare clic su **corpo della risposta**. È possibile verificare che i dati scaricati siano un oggetto con la proprietà **le opzioni** (ovvero un elenco di **TriviaOption** oggetti), **id** e **title** che corrispondono ai **TriviaQuestion** classe.

    ![Visualizzare il corpo della risposta dell'API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "visualizzando il corpo della risposta dell'API Web")

    *Corpo della risposta dell'API Web di visualizzazione*
8. Tornare a Visual Studio e premere **MAIUSC+F5** per arrestare il debug.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>Esercizio 2: Creazione dell'interfaccia SPA

In questo esercizio verranno compilate la parte di front-end web di fanatico Quiz, concentrandosi sull'uso di interazione tra applicazione a singola pagina **AngularJS**. Quindi si miglioreranno l'esperienza utente con CSS3 di eseguire quali animazioni sofisticate e fornire un effetto visivo di contesto nel passaggio durante la transizione da una domanda a quella successiva.

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>Attività 1: creazione dell'interfaccia di applicazione a singola pagina con AngularJS

In questa attività si userà **AngularJS** per implementare il lato client dell'applicazione fanatico Quiz. **AngularJS** è un framework JavaScript open source che integra le applicazioni basate su browser con *Model-View-Controller* funzionalità (MVC), facilitando operazioni sia di sviluppo e test.

Iniziare installando AngularJS dalla Console di gestione pacchetti di Visual Studio. Successivamente, si creerà il controller per fornire il comportamento dell'app fanatico Quiz e la visualizzazione per il rendering del quiz domande e risposte con il motore del modello AngularJS.

> [!NOTE]
> Per ulteriori informazioni su AngularJS, vedere [ [ http://angularjs.org/ ](http://angularjs.org/) ](http://angularjs.org/).


1. Aprire **Visual Studio Express 2013 per il Web** e aprire il **GeekQuiz.sln** soluzione che si trova nel **inizio/origine/Ex2-CreatingASPAInterface** cartella. In alternativa, è possibile continuare con la soluzione ottenuta nell'esercizio precedente.
2. Aprire il **Console di gestione pacchetti** dalla **Tools** > **Gestione pacchetti NuGet**. Digitare il comando seguente per installare il **AngularJS.Core** pacchetto NuGet.

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. In **Esplora soluzioni**, fare doppio clic sul **script** cartella della **GeekQuiz** del progetto e selezionare **Add | Nuova cartella**. Denominare la cartella **app** , quindi premere **invio**.
4. Fare doppio clic il **app** cartella appena creato e selezionare **Add | File JavaScript**.

    ![Creazione di un nuovo file JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *Creazione di un nuovo file JavaScript*
5. Nel **Specifica nome per l'elemento** finestra di dialogo, digitare *quiz-controller* nel **nome elemento** casella di testo e fare clic su **OK**.

    ![Il nuovo file JavaScript di denominazione](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *Il nuovo file JavaScript di denominazione*
6. Nel **quiz controller.js** , aggiungere il codice seguente per dichiarare e inizializzare AngularJS **QuizCtrl** controller.

    (Code - Snippet *AspNetWebApiSpa - Ex2 - AngularQuizController*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > La funzione del costruttore di **QuizCtrl** controller prevede un parametro da Iniettare denominato **$scope**. Lo stato iniziale dell'ambito deve essere configurato in funzione del costruttore collegando le proprietà per il **$scope** oggetto. Le proprietà contengono il **modello di visualizzazione**e sarà possibile accedere al modello quando il controller è registrato.
    > 
    > Il **QuizCtrl** controller è definito all'interno di un modulo denominato **QuizApp**. I moduli sono unità di lavoro che consentono di suddividere l'applicazione in componenti separati. I vantaggi principali dell'uso dei moduli è che il codice più facile da comprendere e facilita l'esecuzione di unit test, riusabilità e della manutenibilità.
7. Si aggiungerà ora comportamento all'ambito per reagire agli eventi generati dalla vista. Aggiungere il codice seguente alla fine del **QuizCtrl** controller per definire le **nextQuestion** funzionare nel **$scope** oggetto.

    (Code - Snippet *AspNetWebApiSpa - Ex2 - AngularQuizControllerNextQuestion*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > Questa funzione recupera il passo successivo dal **gli elementi semplici** API Web creato nell'esercizio precedente e associa i dati di domanda per il **$scope** oggetto.
8. Inserire il codice seguente alla fine del **QuizCtrl** controller per definire le **sendAnswer** funzionare nel **$scope** oggetto.

    (Code - Snippet *AspNetWebApiSpa - Ex2 - AngularQuizControllerSendAnswer*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > Questa funzione Invia risposta selezionata dall'utente per il **gli elementi semplici** API Web e archivia il risultato: ad esempio se la risposta è corretta o non-nelle **$scope** oggetto.
    > 
    > Il **nextQuestion** e **sendAnswer** funzioni sopra usano AngularJS **$http** oggetto astrarre le comunicazioni con l'API Web tramite l'oggetto XMLHttpRequest Oggetto JavaScript dal browser. AngularJS supporta un altro servizio che offre un livello superiore di astrazione per eseguire operazioni CRUD su una risorsa tramite le API RESTful. AngularJS **$resource** oggetto dispone di metodi di azione che forniscono i comportamenti di alto livello senza la necessità di interagire con i **$http** oggetto. È consigliabile usare la **$resource** oggetto negli scenari che richiede il modello CRUD (per altre informazioni, vedere la [$resource documentazione](https://docs.angularjs.org/api/ngResource/service/$resource)).
9. Il passaggio successivo consiste nel creare il modello AngularJS che definisce la visualizzazione per il quiz. A tale scopo, aprire il **index. cshtml** all'interno del file di **viste | Home** cartella e sostituire il contenuto con il codice seguente.

    (Code - Snippet *AspNetWebApiSpa - Ex2 - GeekQuizView*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > Il modello AngularJS è una specifica dichiarativa che utilizza le informazioni del modello e il controller per trasformare i markup statici in visualizzazione dinamica che l'utente vede nel browser. Di seguito è riportati esempi di AngularJS elementi e attributi dell'elemento che possono essere usati in un modello:
    > 
    > - Il **ng-app** direttiva richiede AngularJS all'elemento DOM che rappresenta l'elemento radice dell'applicazione.
    > - Il **ng-controller** direttiva allega un controller al DOM in corrispondenza del punto in cui viene dichiarata la direttiva.
    > - La notazione di parentesi graffa **{{}}** denota le associazioni per le proprietà dell'ambito definite nel controller.
    > - Il **ng-click** direttiva viene usata per richiamare le funzioni definite nell'ambito in risposta all'interazione dell'utente.
10. Aprire il **CSS** all'interno del file il **contenuto** cartella e aggiungere i seguenti stili evidenziati alla fine del file per fornire un aspetto per la visualizzazione del quiz.

    (Code - Snippet *AspNetWebApiSpa - Ex2 - GeekQuizStyles*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>Attività 2: esecuzione della soluzione

In questa attività verrà eseguita la soluzione usando il nuovo utente dell'interfaccia è compilata con AngularJS per rispondere ad alcune delle domande quiz.

1. Premere **F5** per eseguire la soluzione.
2. Registrare un nuovo account utente. A questo scopo, seguire la procedura di registrazione descritta nell'esercizio 1, attività 3.

    > [!NOTE]
    > Se si usa la soluzione nell'esercizio precedente, è possibile accedere con l'account utente che è stato creato prima.
3. Il **Home** schermata dovrebbe essere visualizzata, che mostra la prima domanda del quiz. Rispondere alla domanda facendo clic su una delle opzioni. Verrà attivata la **sendAnswer** funzione definita in precedenza, che invia l'opzione selezionata per il **elementi semplici** API Web.

    ![Rispondere alla domanda](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "rispondere alla domanda")

    *Rispondere alla domanda*
4. Dopo aver fatto clic su uno dei pulsanti, apparirà la risposta. Fare clic su **domanda successiva** per mostrare la domanda seguente. Verrà attivata la **nextQuestion** funzione definite nel controller.

    ![Richiede la domanda successiva](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "che richiede la domanda successiva")

    *Richiede la domanda successiva*
5. La domanda successiva dovrebbe essere visualizzato. Continuare a rispondere alle domande come numero di volte desiderato. Dopo aver completato tutte le domande è necessario restituire alla prima domanda.

    ![Un'altra domanda](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "un'altra domanda")

    *Prossima domanda*
6. Tornare a Visual Studio e premere **MAIUSC+F5** per arrestare il debug.

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>Attività 3: creazione di un'animazione Flip tramite CSS3

In questa attività si userà le proprietà di CSS3 eseguire quali animazioni sofisticate aggiungendo un effetto di scorrimento quando è stato risposto a una domanda e quando viene recuperata la domanda successiva.

1. Nel **Esplora soluzioni**, fare doppio clic sul **contenuto** cartella della **GeekQuiz** del progetto e selezionare **Add | Elemento esistente...** .

    ![Aggiunta di un elemento esistente nella cartella Content](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "aggiungendo un elemento esistente nella cartella Content")

    *Aggiunta di un elemento esistente nella cartella Content*
2. Nel **Aggiungi elemento esistente** finestra di dialogo passare alle **origine/asset** cartella e selezionare **Flip.css**. Fare clic su **Aggiungi**.

    ![Aggiunta del file Flip.css dagli asset](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "aggiunta del file Flip.css dagli Asset")

    *Aggiunta del file Flip.css dagli Asset*
3. Aprire il **Flip.css** file appena aggiunto in precedenza ed esaminare il relativo contenuto.
4. Individuare il **Capovolgi trasformazione** commento. Gli stili di sotto di quel commento utilizzano CSS **prospettiva** e **rotateY** trasformazioni per generare un &quot;scheda capovolgimento&quot; effetto.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. Individuare il **parte posteriore del riquadro nascondere durante lo scorrimento** commento. Lo stile di sotto di quel commento nasconde il lato posteriore delle facce quando si verificano lontano dall'osservatore impostando il **faccia posteriore-visibility** proprietà CSS *nascosto*.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. Aprire il **BundleConfig.cs** all'interno del file il **App\_avviare** cartella e aggiungere il riferimento al **Flip.css** del file nel **&quot;~/Content/css&quot;** bundle di stile

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. Premere **F5** per eseguire la soluzione e accedere con le proprie credenziali.
8. Rispondere a una domanda facendo clic su una delle opzioni. Notare l'effetto scorrimento durante la transizione tra le visualizzazioni.

    ![Rispondere alla domanda con l'effetto di scorrimento](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "rispondere alla domanda con l'effetto di scorrimento")

    *Rispondere alla domanda con l'effetto di scorrimento*
9. Fare clic su **domanda successiva** per recuperare la domanda seguente. L'effetto scorrimento dovrebbe essere visualizzata nuovamente.

    ![Il recupero alla domanda seguente con l'effetto di scorrimento](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "recupero alla domanda seguente con l'effetto di scorrimento")

    *Il recupero alla domanda seguente con l'effetto di scorrimento*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa esercitazione pratica si è appreso come:

- Creare un controller API Web ASP.NET usando lo Scaffolding di ASP.NET
- Implementare un'azione di Web API Get per recuperare la domanda successiva quiz
- Implementare un'azione di Post di API Web per archiviare le risposte
- Installare AngularJS dalla Console di gestione pacchetti Visual Studio
- Implementare AngularJS modelli e i controller
- Usare le transizioni CSS3 per eseguire gli effetti di animazione
