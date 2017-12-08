---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: 'Lab pratico: Compilare un''applicazione a pagina singola (SPA) con ASP.NET Web API e Angular.js | Documenti Microsoft'
author: rick-anderson
description: Nelle applicazioni web tradizionali, il client (browser) avvia la comunicazione con il server richiedendo una pagina. Il server elabora quindi la richiesta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2015
ms.topic: article
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 9a748628d53878be380869ac5327de0111d2284d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>Lab pratico: Compilare un'applicazione a pagina singola (SPA) con ASP.NET Web API e Angular.js
====================
da [categorie Web Team](https://twitter.com/webcamps)

[Download Web categorie Kit di formazione](http://aka.ms/webcamps-training-kit)

> Nelle applicazioni web tradizionali, il client (browser) avvia la comunicazione con il server richiedendo una pagina. Quindi, il server elabora la richiesta e invia al client il codice HTML della pagina. Nelle successive interazioni con la pagina, ad esempio, l'utente passa a un collegamento o invia un modulo con i dati: una nuova richiesta viene inviata al server e il flusso viene riavviata: il server elabora la richiesta e invia una nuova pagina nel browser in risposta a una nuova richiesta di azione ed dal client.
> 
> Nelle applicazioni a pagina singola (stabilimenti termali) la pagina intera viene caricata nel browser dopo la richiesta iniziale, ma le interazioni successive vengono eseguite utilizzando le richieste Ajax. Ciò significa che il browser ha per aggiornare solo la parte della pagina che è stato modificato. non è necessario ricaricare la pagina intera. L'approccio SPA riduce il tempo impiegato dall'applicazione per rispondere alle azioni utente, risultante in un'esperienza più semplice.
> 
> L'architettura di un SPA implica alcuni problemi che non sono presenti nelle applicazioni web tradizionali. Tuttavia, le ultimissime tecnologie quali ASP.NET Web API, JavaScript Framework come AngularJS e nuove funzionalità di applicazione di stili fornite da CSS3 rendono molto semplice progettare e compilare stabilimenti termali.
> 
> In questo laboratorio mano-on, si sarà possibile avvalersi di tali tecnologie per implementare appassionato Quiz, un sito Web gli elementi semplici in base al concetto SPA. Innanzitutto verrà implementato il livello di servizio con l'API Web ASP.NET per esporre gli endpoint necessari per recuperare le domande del quiz e archiviare le risposte. Si compilerà quindi un'interfaccia avanzata e reattiva utente usando AngularJS e CSS3 effetti di trasformazione.
> 
> Tutto il codice di esempio e i frammenti di codice sono inclusi nel Web categorie Training Kit, disponibile all'indirizzo [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).


## <a name="overview"></a>Panoramica

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questa esercitazione pratica, si apprenderà come:

- Creare un servizio ASP.NET Web API per inviare e ricevere dati JSON
- Creare un'interfaccia utente reattiva con AngularJS
- Migliorare l'esperienza dell'interfaccia utente con le trasformazioni CSS3

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Di seguito è necessario per completare questa esercitazione pratica:

- [Visual Studio Express 2013 per Web](https://www.microsoft.com/visualstudio/) o versione successiva

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

1. [Creazione di un'API Web](#Exercise1)
2. [Creazione di un'interfaccia SPA](#Exercise2)

Tempo stimato per completare questa esercitazione: **60 minuti**

> [!NOTE]
> Al primo avvio di Visual Studio, è necessario selezionare una delle raccolte di impostazioni predefinite. Ogni raccolta predefinita è progettato in modo che corrisponda a un particolare stile di sviluppo e determina il layout di finestra, il comportamento dell'editor, frammenti di codice IntelliSense e finestre di dialogo. Le procedure descritte in questa esercitazione vengono descritte le azioni necessarie per eseguire una determinata attività in Visual Studio quando si utilizza il **impostazioni generali per lo sviluppo** insieme. Se si sceglie una raccolta di impostazioni diverse per l'ambiente di sviluppo, possono essere presenti differenze nei passaggi che è necessario prendere in considerazione.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>Esercizio 1: Creazione di un'API Web

Una delle parti principali di un SPA è il livello di servizio. È responsabile dell'elaborazione le chiamate Ajax inviate da interfaccia utente e la restituzione di dati in risposta a tale chiamata. I dati recuperati devono essere presentati in un formato leggibile al computer per poter essere analizzato e utilizzato dal client.

Framework Web API fa parte dello Stack di ASP.NET ed è progettato per semplificare l'implementazione di servizi HTTP, in genere l'invio e ricezione di dati in formato JSON o XML tramite un'API RESTful. In questo esercizio si creerà il sito Web per ospitare l'applicazione appassionato Quiz e quindi implementare il servizio back-end per esporre e rendere persistenti i dati del quiz mediante ASP.NET Web API.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>Attività 1: creazione del progetto iniziale per il Quiz appassionato

In questa attività si inizierà creando un nuovo progetto MVC ASP.NET con supporto per ASP.NET Web API basata sul **ASP.NET uno** tipo che viene fornito con Visual Studio di progetto. **Uno ASP.NET** unifica tutte le tecnologie di ASP.NET e offre la possibilità di combinare e associarli in base alle esigenze. Si aggiungeranno quindi classi del modello di Entity Framework e initializator il database per inserire le domande del quiz.

1. Aprire **Visual Studio Express 2013 per Web** e selezionare **File | Nuovo progetto...**  per avviare una nuova soluzione.

    ![Crea un nuovo progetto](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "creando un nuovo progetto")

    *Crea un nuovo progetto*
2. Nel **nuovo progetto** nella finestra di dialogo **applicazione Web ASP.NET** sotto il **Visual c# | Web** scheda. Assicurarsi che **.NET Framework 4.5** è selezionata, il nome *GeekQuiz*, scegliere un **percorso** e fare clic su **OK**.

    ![Crea un nuovo progetto applicazione Web ASP.NET](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "creando un nuovo progetto applicazione Web ASP.NET")

    *Crea un nuovo progetto applicazione Web ASP.NET*
3. Nel **nuovo progetto ASP.NET** la finestra di dialogo, seleziona il **MVC** modello e selezionare il **API Web** opzione. Inoltre, assicurarsi che il **autenticazione** opzione è impostata su **singoli account utente di**. Fare clic su **OK** per continuare.

    ![Crea un nuovo progetto con il modello MVC, inclusi i componenti di Web API](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *Crea un nuovo progetto con il modello MVC, inclusi i componenti di Web API*
4. In **Esplora**, fare doppio clic su di **modelli** cartella del **GeekQuiz** del progetto e selezionare **Aggiungi | Elemento esistente...** .

    ![Aggiunta di un elemento esistente](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "aggiunta di un elemento esistente")

    *Aggiunta di un elemento esistente*
5. Nel **Aggiungi elemento esistente** finestra di dialogo passare al **/modelli di attività diorigine/** cartella e selezionare tutti i file. Fare clic su **Aggiungi**.

    ![Aggiunta di risorse modello](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "aggiungendo gli asset di modello")

    *Aggiunta di risorse modello*

    > [!NOTE]
    > Aggiungendo questi file, si aggiunge il modello di dati, il contesto di database di Entity Framework e l'inizializzatore del database per l'applicazione appassionato Quiz.
    > 
    > **Entity Framework (EF)** è un mapping relazionale a oggetti (ORM) che consente di creare applicazioni di accesso ai dati tramite programmazione con un modello di applicazione concettuale anziché programmando direttamente con uno schema di archiviazione relazionale. Maggiori informazioni su Entity Framework [qui](../../../entity-framework.md).
    > 
    > Di seguito è una descrizione delle classi che appena aggiunto:
    > 
    > - **TriviaOption:** rappresenta una singola opzione associata a una domanda quiz
    > - **TriviaQuestion:** rappresenta una domanda quiz ed espone le relative opzioni tramite il **opzioni** proprietà
    > - **TriviaAnswer:** rappresenta l'opzione selezionata dall'utente in risposta a una domanda quiz
    > - **TriviaContext:** rappresenta il contesto di database di Entity Framework dell'applicazione appassionato Quiz. Questa classe deriva da **DContext** ed espone **DbSet** le proprietà che rappresentano raccolte di entità descritta in precedenza.
    > - **TriviaDatabaseInitializer:** l'implementazione dell'inizializzatore di Entity Framework per la **TriviaContext** classe che eredita da **CreateDatabaseIfNotExists**. Il comportamento predefinito di questa classe per creare il database solo se non esiste, l'inserimento di entità specificato nella **valore di inizializzazione** metodo.
6. Aprire il **Global.asax.cs** file e aggiungere la seguente istruzione using.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. Aggiungere il codice seguente all'inizio del **applicazione\_avviare** per impostare il **TriviaDatabaseInitializer** come l'inizializzatore del database.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. Modificare il **Home** controller per limitare l'accesso per gli utenti autenticati. A tale scopo, aprire il **HomeController. cs** file all'interno di **controller** cartella e aggiungere il **Authorize** attributo il **HomeController**definizione di classe.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > Il **Authorize** filtro controlla se l'utente viene autenticato. Se l'utente non è autenticato, viene restituito il codice di stato HTTP 401 (Unauthorized) senza richiamare l'azione. È possibile applicare il filtro a livello globale, a livello di controller o al livello di singole azioni.
9. A questo punto si personalizzerà il layout delle pagine web e la personalizzazione. A tale scopo, aprire il  **\_cshtml** file all'interno di **viste | Condiviso** cartella e aggiornare il contenuto del  **&lt;titolo&gt;**  elemento sostituendo *applicazione ASP.NET* con *appassionato Quiz* .

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. Nello stesso file, aggiornare la barra di navigazione rimuovendo il *su* e *contatto* collegamenti e ridenominazione il *Home* collegare *riprodurre*. Inoltre, rinominare il *nome applicazione* collegare *appassionato Quiz*. Il codice HTML per la barra di spostamento dovrebbe essere simile al codice seguente.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. Aggiornare il piè di pagina della pagina di layout sostituendo *applicazione ASP.NET* con *appassionato Quiz*. A tale scopo, sostituire il contenuto del  **&lt;piè di pagina&gt;**  elemento con il codice evidenziato di seguito.

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>Attività 2: creazione di API Web TriviaController

Nell'attività precedente, la creazione della struttura iniziale dell'applicazione web appassionato Quiz. Ora è necessario compilare un semplice servizio API Web che interagisce con il modello di dati quiz ed espone le seguenti azioni:

- **GET/api/gli elementi semplici**: recupera la prossima domanda dall'elenco quiz la risposta da parte dell'utente autenticato.
- **POST/api/gli elementi semplici**: archivia la risposta quiz specificata dall'utente autenticato.

Utilizzare gli strumenti di ASP.NET Scaffolding forniti da Visual Studio per creare la linea di base per la classe controller API Web.

1. Aprire il **WebApiConfig.cs** file all'interno di **App\_avviare** cartella. Questo file definisce la configurazione del servizio Web API, ad esempio la modalità di mapping delle route per le azioni del controller API Web.
2. Aggiungere la seguente istruzione using all'inizio del file.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. Aggiungere il codice evidenziato di seguito per il **registrare** metodo per configurare a livello globale del formattatore per i dati JSON recuperati dai metodi di azione di API Web.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > Il **CamelCasePropertyNamesContractResolver** converte automaticamente i nomi delle proprietà da *camel* caso, è la convenzione generale per i nomi delle proprietà in JavaScript.
4. In **Esplora**, fare doppio clic sul **controller** cartella del **GeekQuiz** del progetto e selezionare **Aggiungi | Nuovo elemento di scaffolding...** .

    ![Creazione di un nuovo elemento di scaffolding](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "la creazione di un nuovo elemento di scaffolding")

    *Creazione di un nuovo elemento di scaffolding*
5. Nel **aggiungere lo scaffolding** finestra di dialogo, assicurarsi che il **comuni** nodo viene selezionato nel riquadro a sinistra. Selezionare quindi il **Web API 2 Controller - vuoto** modello nel riquadro centrale e fare clic su **Aggiungi**.

    ![Selezionare il modello vuoto di Controller Web API 2](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "selezionando il modello vuoto di Controller Web API 2")

    *Selezionare il modello vuoto di Controller Web API 2*

    > [!NOTE]
    > **Lo Scaffolding di ASP.NET** è un framework di generazione di codice per applicazioni Web ASP.NET. Visual Studio 2013 sono inclusi generatori di codice pre-installato per i progetti MVC e Web API. Utilizzare lo scaffolding nel progetto quando si desidera aggiungere rapidamente il codice che interagisce con i modelli di dati per ridurre la quantità di tempo necessari per sviluppare le operazioni di dati standard.
    > 
    > Il processo di scaffolding assicura inoltre che tutte le dipendenze necessarie siano installate nel progetto. Ad esempio, se si avvia un progetto ASP.NET vuoto e quindi Usa lo scaffolding per aggiungere un controller API Web, i pacchetti NuGet dell'API Web richiesti e i riferimenti vengono aggiunti al progetto automaticamente.
6. Nel **Aggiungi Controller** della finestra di dialogo tipo *TriviaController* nel **nome Controller** casella di testo e fare clic su **Aggiungi**.

    ![Aggiunta del Controller gli elementi semplici](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "aggiunta del Controller gli elementi semplici")

    *Aggiunta del Controller gli elementi semplici*
7. Il **TriviaController.cs** file viene quindi aggiunto al **controller** cartella del **GeekQuiz** progetto, contenente un oggetto vuoto **TriviaController** classe. Aggiungere le seguenti istruzioni using all'inizio del file.

    (- Frammento di codice *TriviaControllerUsings AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. Aggiungere il codice seguente all'inizio del **TriviaController** classe per definire, inizializzare ed eliminare il **TriviaContext** istanza nel controller.

    (- Frammento di codice *TriviaControllerContext AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > Il **Dispose** metodo **TriviaController** richiama il **Dispose** metodo il **TriviaContext** istanza, che assicura che tutti i le risorse usate dall'oggetto di contesto vengono rilasciate quando il **TriviaContext** istanza viene eliminata o sottoposto a garbage collection. Ciò include la chiusura di tutte le connessioni di database aperte da Entity Framework.
9. Aggiungere il seguente metodo helper alla fine del **TriviaController** classe. Questo metodo recupera i seguenti aspetti quiz dal database la risposta da parte dell'utente specificato.

    (- Frammento di codice *TriviaControllerNextQuestion AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. Aggiungere il seguente **ottenere** il metodo di azione per il **TriviaController** classe. Questo metodo di azione chiama il **NextQuestionAsync** metodo helper definito nel passaggio precedente per recuperare la domanda successiva per l'utente autenticato.

    (- Frammento di codice *TriviaControllerGetAction AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. Aggiungere il seguente metodo helper alla fine del **TriviaController** classe. Questo metodo archivia risposte specificato nel database e restituisce un valore booleano che indica se la risposta è corretta.

    (- Frammento di codice *TriviaControllerStoreAsync AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. Aggiungere il seguente **Post** il metodo di azione per il **TriviaController** classe. Questo metodo di azione associa la risposta per l'utente autenticato e chiama il **StoreAsync** metodo di supporto. Quindi, invia una risposta con il valore booleano restituito dal metodo di supporto.

    (- Frammento di codice *TriviaControllerPostAction AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. Modificare il controller API Web per limitare l'accesso agli utenti autenticati mediante l'aggiunta di **Authorize** attributo la **TriviaController** definizione di classe.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Attività 3: esecuzione della soluzione

In questa attività si verificherà che il servizio Web API che incluso nell'attività precedente è funzioni come previsto. Utilizzare Internet Explorer **strumenti di sviluppo F12** per acquisire il traffico di rete e controllare l'intera risposta dal servizio Web API.

> [!NOTE]
> Assicurarsi che **Internet Explorer** è selezionata nel **avviare** pulsante Trova sulla barra degli strumenti di Visual Studio.
> 
> ![Opzione di Internet Explorer](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)


1. Premere **F5** per eseguire la soluzione. Il **Accedi** verrà visualizzata la pagina nel browser.

    > [!NOTE]
    > All'avvio dell'applicazione, viene attivata la route MVC predefinita, che per impostazione predefinita viene eseguito il mapping per il **indice** azione del **HomeController** classe. Poiché **HomeController** è limitato agli utenti autenticati (tenere presente che è decorato di tale classe con il **Authorize** attributo nell'esercizio 1) e non esiste alcun utente autenticato ancora, l'applicazione Reindirizza la richiesta originale a pagina di accesso.

    ![Esecuzione della soluzione](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "esecuzione della soluzione")

    *Esecuzione della soluzione*
2. Fare clic su **registrare** per creare un nuovo utente.

    ![Registrazione di un nuovo utente](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "la registrazione di un nuovo utente")

    *Registrazione di un nuovo utente*
3. Nel **registrare** pagina, immettere un **nome utente** e **Password**, quindi fare clic su **registrare**.

    ![Pagina Registra](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "pagina di registrazione")

    *Pagina di registrazione*
4. L'applicazione Registra nuovo account e l'utente è autenticato e reindirizzato alla home page.

    ![Autenticazione dell'utente](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "utente autenticato")

    *Autenticazione dell'utente*
5. Nel browser, premere **F12** per aprire la **gli strumenti di sviluppo** pannello. Premere **CTRL + 4** oppure fare clic su di **rete** icona e quindi fare clic su pulsante freccia verde per avviare l'acquisizione del traffico di rete.

    ![Avvio di acquisizione di rete di API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "acquisizione di rete di avvio di Web API")

    *Avvio di acquisizione di rete di API Web*
6. Aggiungere **api o gli elementi semplici** per l'URL nella barra degli indirizzi del browser. È ora esamina i dettagli della risposta di **ottenere** metodo di azione **TriviaController**.

    ![Il recupero dei dati di domanda successivi tramite l'API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "il recupero dei dati di domanda successivi tramite l'API Web")

    *Il recupero dei dati di domanda successivi tramite l'API Web*

    > [!NOTE]
    > Una volta completato il download, verrà richiesto di effettuare un'azione con il file scaricato. Lasciare la finestra di dialogo aperta per poter visualizzare il contenuto della risposta tramite la finestra degli strumenti sviluppatori.
7. Ora si esamina il corpo della risposta. A tale scopo, fare clic su di **dettagli** scheda e quindi fare clic su **corpo della risposta**. È possibile verificare che i dati scaricati sono un oggetto con le proprietà **opzioni** (ovvero un elenco di **TriviaOption** oggetti), **id** e **titolo** che corrispondono al **TriviaQuestion** classe.

    ![Visualizzare il corpo della risposta di API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "visualizzando il corpo della risposta di API Web")

    *Corpo della risposta di API Web visualizzazione*
8. Tornare a Visual Studio e premere **MAIUSC + F5** per arrestare il debug.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>Esercizio 2: Creazione dell'interfaccia SPA

In questo esercizio verranno compilate parte web front-end del Quiz appassionato, porre l'attenzione sull'interazione dell'applicazione a pagina singola utilizzando **AngularJS**. È quindi migliorare l'esperienza dell'utente con CSS3 di eseguire le animazioni avanzate e fornire un effetto visivo di durante la transizione da una domanda a quella successiva il cambio di contesto.

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>Attività 1: creazione dell'interfaccia SPA con AngularJS

In questa attività si utilizzerà **AngularJS** per implementare il lato client dell'applicazione appassionato Quiz. **AngularJS** è un framework JavaScript open source che aumenta le applicazioni basate su browser con *Model-View-Controller* funzionalità (MVC), semplificando sia lo sviluppo e test.

Si inizierà installando AngularJS dalla Console di gestione pacchetti di Visual Studio. Quindi, si creerà il controller per fornire il comportamento dell'app appassionato Quiz e la visualizzazione per il rendering il quiz domande frequenti utilizzando il motore del modello di AngularJS.

> [!NOTE]
> Per ulteriori informazioni su AngularJS, vedere [ [http://angularjs.org/](http://angularjs.org/)](http://angularjs.org/).


1. Aprire **Visual Studio Express 2013 per Web** e aprire il **GeekQuiz.sln** soluzione si trova nel **origine/Ex2-CreatingASPAInterface/inizio** cartella. In alternativa, è possibile continuare con la soluzione ottenuta nell'esercizio precedente.
2. Aprire il **Console di gestione pacchetti** da **strumenti** | **Gestione pacchetti libreria**. Digitare il comando seguente per installare il **AngularJS.Core** pacchetto NuGet.

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. In **Esplora**, fare doppio clic su di **script** cartella del **GeekQuiz** del progetto e selezionare **Aggiungi | Nuova cartella**. Denominare la cartella **app** e premere **invio**.
4. Fare doppio clic su di **app** cartella appena creata e selezionare **Aggiungi | File JavaScript**.

    ![Creazione di un nuovo file di JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *Creazione di un nuovo file di JavaScript*
5. Nel **Specifica nome per l'elemento** della finestra di dialogo tipo *quiz controller* nel **nome elemento** casella di testo e fare clic su **OK**.

    ![Il nuovo file JavaScript di denominazione](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *Il nuovo file JavaScript di denominazione*
6. Nel **quiz controller.js** file, aggiungere il codice seguente per dichiarare e inizializzare AngularJS **QuizCtrl** controller.

    (- Frammento di codice *AngularQuizController AspNetWebApiSpa - Ex2 -*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > La funzione del costruttore di **QuizCtrl** controller prevede un parametro da Iniettare denominato **$scope**. Lo stato iniziale dell'ambito deve essere configurato nella funzione del costruttore mediante l'aggiunta di proprietà per il **$scope** oggetto. Le proprietà contengono il **modello di visualizzazione**e sarà possibile accedere al modello quando il controller è registrato.
    > 
    > Il **QuizCtrl** controller è definito all'interno di un modulo denominato **QuizApp**. I moduli sono unità di lavoro che consentono di suddividere l'applicazione in componenti separati. I principali vantaggi dell'utilizzo di moduli è che il codice è più facile da comprendere e facilita l'esecuzione di unit test, riusabilità e la gestibilità.
7. Si aggiungerà il comportamento all'ambito per rispondere agli eventi generati dalla vista. Aggiungere il codice seguente alla fine del **QuizCtrl** controller per definire il **nextQuestion** funzionare nel **$scope** oggetto.

    (- Frammento di codice *AngularQuizControllerNextQuestion AspNetWebApiSpa - Ex2 -*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > Questa funzione recupera la domanda successiva dal **gli elementi semplici** API Web creato nell'esercizio precedente e associa i dati di domanda per la **$scope** oggetto.
8. Inserire il codice seguente alla fine del **QuizCtrl** controller per definire il **sendAnswer** funzionare nel **$scope** oggetto.

    (- Frammento di codice *AngularQuizControllerSendAnswer AspNetWebApiSpa - Ex2 -*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > Questa funzione Invia risposta selezionata dall'utente per il **gli elementi semplici** API Web e archivia il risultato: ad esempio se la risposta è corretta o non-in di **$scope** oggetto.
    > 
    > Il **nextQuestion** e **sendAnswer** funzioni sopra usano AngularJS **$http** oggetto per la comunicazione con l'API Web tramite il XMLHttpRequest astratta Oggetto di JavaScript nel browser. AngularJS supporta un altro servizio che offre un livello superiore di astrazione per eseguire operazioni CRUD su una risorsa tramite le API REST. AngularJS **$resource** oggetto dispone di metodi di azione che forniscono i comportamenti di alto livello senza la necessità di interagire con il **$http** oggetto. È consigliabile utilizzare il **$resource** oggetto negli scenari che richiede il modello CRUD (fore informazioni, vedere il [$resource documentazione](https://docs.angularjs.org/api/ngResource/service/$resource)).
9. Il passaggio successivo consiste nel creare il modello AngularJS che definisce la vista per il quiz. A tale scopo, aprire il **cshtml** file all'interno di **viste | Home** cartella e sostituire il contenuto con il codice seguente.

    (- Frammento di codice *GeekQuizView AspNetWebApiSpa - Ex2 -*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > Il modello di AngularJS è una specifica dichiarativa che utilizza le informazioni dal modello e il controller per trasformare markup statico in visualizzazione dinamica che l'utente visualizza nel browser. Di seguito è riportati esempi di AngularJS elementi e attributi di elemento che possono essere usati in un modello:
    > 
    > - Il **ng-app** direttiva richiede AngularJS all'elemento DOM che rappresenta l'elemento radice dell'applicazione.
    > - Il **ng-controller** direttiva collega un controller al DOM in corrispondenza del punto in cui viene dichiarata la direttiva.
    > - La notazione con parentesi graffa **{{}}** indica le associazioni per le proprietà dell'ambito definite nel controller.
    > - Il **ng-click** direttiva viene utilizzata per richiamare le funzioni definite nell'ambito in risposta ai clic dell'utente.
10. Aprire il **Site** file all'interno di **contenuto** cartella e aggiungere i seguenti stili evidenziati alla fine del file per fornire un aspetto coerente per la visualizzazione del quiz.

    (- Frammento di codice *GeekQuizStyles AspNetWebApiSpa - Ex2 -*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>Attività 2: esecuzione della soluzione

In questa attività verrà eseguita la soluzione utilizzando il nuovo utente interfaccia è compilata con AngularJS per rispondere ad alcune delle domande quiz.

1. Premere **F5** per eseguire la soluzione.
2. Registrare un nuovo account utente. A tale scopo, seguire i passaggi di registrazione descritti nell'esercizio 1, l'attività 3.

    > [!NOTE]
    > Se si utilizza la soluzione nell'esercizio precedente, è possibile accedere con l'account utente che è stato creato prima.
3. Il **Home** verrà visualizzata la pagina, che mostra la prima domanda del quiz. Rispondere alla domanda facendo clic su una delle opzioni. In questo modo verrà attivata la **sendAnswer** funzione definita in precedenza, che invia l'opzione selezionata per il **gli elementi semplici** API Web.

    ![Per rispondere a una domanda](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "rispondere")

    *Per rispondere a una domanda*
4. Dopo aver fatto clic su uno dei pulsanti, la risposta dovrebbe essere visualizzato. Fare clic su **domanda successiva** per visualizzare i seguenti aspetti. In questo modo verrà attivata la **nextQuestion** funzione definite nel controller.

    ![La richiesta di domanda successiva](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "richiesta domanda successiva")

    *La richiesta di domanda successiva*
5. Domanda successiva dovrebbe essere visualizzato. Continuare a rispondere alle domande più volte. Dopo aver completato tutte le domande è necessario restituire alla prima domanda.

    ![Un'altra domanda](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "un'altra domanda")

    *Prossima domanda*
6. Tornare a Visual Studio e premere **MAIUSC + F5** per arrestare il debug.

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>Attività 3: creazione di un'animazione capovolto tramite CSS3

In questa attività si utilizzerà CSS3 proprietà per eseguire le animazioni RTF aggiungendo un effetto di scorrimento quando un messaggio di risposta e quando viene recuperata la domanda successiva.

1. In **Esplora**, fare doppio clic su di **contenuto** cartella del **GeekQuiz** del progetto e selezionare **Aggiungi | Elemento esistente...** .

    ![Aggiunta di un elemento esistente nella cartella dati](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "aggiunta di un elemento esistente per la cartella del contenuto")

    *Aggiunta di un elemento esistente per la cartella del contenuto*
2. Nel **Aggiungi elemento esistente** finestra di dialogo passare al **origine/asset** cartella e selezionare **Flip.css**. Fare clic su **Aggiungi**.

    ![Aggiunta del file Flip.css da asset](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "aggiunta del file Flip.css da Asset")

    *Aggiunta del file Flip.css da Asset*
3. Aprire il **Flip.css** file è stato aggiunto ed esaminare il relativo contenuto.
4. Individuare il **capovolgere trasformazione** commento. Foglio di stile CSS di utilizzare gli stili di sotto di tale commento **prospettiva** e **rotateY** trasformazioni per generare un &quot;scheda capovolto&quot; effetto.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. Individuare il **nascondere parte posteriore riquadro durante capovolto** commento. Lo stile sotto il commento nasconde il lato posteriore delle facce quando essi sono lontani Visualizzatore impostando il **faccia posteriore visibilità** proprietà CSS *nascosto*.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. Aprire il **BundleConfig.cs** file all'interno di **App\_avviare** cartella e aggiungere il riferimento al **Flip.css** file nel  **&quot;~/Content/css&quot;**  bundle di stile

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. Premere **F5** per eseguire la soluzione e accedere con le credenziali.
8. Rispondere a una domanda facendo clic su una delle opzioni. Si noti l'effetto scorrimento durante la transizione tra le visualizzazioni.

    ![Per rispondere a una domanda con l'effetto scorrimento](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "rispondere con l'effetto di scorrimento")

    *Per rispondere a una domanda con l'effetto di scorrimento*
9. Fare clic su **domanda successiva** per recuperare la domanda seguente. L'effetto scorrimento dovrebbe essere visualizzato nuovamente.

    ![Il recupero alla domanda seguente con l'effetto scorrimento](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "recupero alla domanda seguente con l'effetto di scorrimento")

    *Il recupero alla domanda seguente con l'effetto di scorrimento*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa esercitazione si è appreso come:

- Creare un controller API Web ASP.NET utilizzando lo Scaffolding di ASP.NET
- Implementare un'azione di Web API Get per recuperare la domanda del quiz successiva
- Implementare un'azione di Post di API Web per archiviare le risposte
- Installare AngularJS dalla Console di gestione di pacchetti di Visual Studio
- Implementare AngularJS modelli e i controller
- Utilizzare le transizioni CSS3 esegua gli effetti di animazione
