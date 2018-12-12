---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: 'Laboratorio pratico: Le applicazioni Web in tempo reale con SignalR | Microsoft Docs'
author: rick-anderson
description: Applicazioni Web in tempo reale offrono la possibilità di eseguire il push del contenuto ai client connessi come accade, in tempo reale lato server. Per gli sviluppatori ASP.NET, ASP...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: de2f2349fc284e167bd8227ae55da79b9f1f4549
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53288002"
---
<a name="hands-on-lab-real-time-web-applications-with-signalr"></a>Laboratorio pratico: Applicazioni Web in tempo reale con SignalR
====================

da [Camp Web Team](https://twitter.com/webcamps)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[Download Web Camp Kit di formazione](http://aka.ms/webcamps-training-kit)

> Applicazioni Web in tempo reale offrono la possibilità di eseguire il push del contenuto ai client connessi come accade, in tempo reale lato server. Per gli sviluppatori ASP.NET **ASP.NET SignalR** è una libreria per aggiungere funzionalità web in tempo reale alle applicazioni. Sfrutta i trasporti diversi, automaticamente selezionando il trasporto disponibile meglio assegnato del client e più disponibile il trasporto del server. Sfrutta **WebSocket**, un'API di HTML5 che consente la comunicazione bidirezionale tra il browser e server.
> 
> **SignalR** fornisce anche un'API semplice e dettagliata per l'esecuzione di server a client RPC (chiamare le funzioni JavaScript nei browser dei client dal codice .NET sul lato server) nell'applicazione ASP.NET, nonché l'aggiunta di hook utili per la gestione connessione, ad esempio connettere/disconnettere gli eventi, le connessioni di raggruppamento e l'autorizzazione.
> 
> **SignalR** è un'astrazione su alcuni dei trasporti che sono necessari per svolgere il lavoro in tempo reale tra client e server. Oggetto **SignalR** viene avviato come HTTP, connessione e viene quindi promosso a un **WebSocket** connessione se disponibile. **WebSocket** è il trasporto ideale per **SignalR**, dal momento che rende l'uso più efficiente della memoria del server, ha la latenza più bassa e le funzionalità più sottostante (ad esempio comunicazione full duplex tra client e Server), ma ha anche i più rigorosi requisiti: **WebSocket** richiede che il server di usare **Windows Server 2012** oppure **Windows 8**, insieme a **.NET Framework 4.5**. Se non vengono soddisfatti questi requisiti, **SignalR** proverà a usare altri tipi di trasporto per rendere le connessioni (ad esempio *Ajax polling prolungato*).
> 
> Il **SignalR** API contiene due modelli per la comunicazione tra client e server: **Le connessioni permanenti** e **hub**. Oggetto **connessione** rappresenta un semplice endpoint per l'invio di singolo destinatario, raggruppati o trasmettere i messaggi. Oggetto **Hub** è una pipeline più alto livello basata sull'API di connessione che consente il client e server di chiamare direttamente i metodi a vicenda.
> 
> ![Architettura di SignalR](real-time-web-applications-with-signalr/_static/image1.png)
> 
> Tutto il codice di esempio e frammenti di codice sono inclusi nel Web Camp Kit di formazione, disponibile all'indirizzo [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Panoramica

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico, si apprenderà come:

- Inviare notifiche dal server al client tramite SignalR.
- Scalabilità orizzontale SignalR applicazione utilizzando **SQL Server**.

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Di seguito è necessario per completare questo laboratorio pratico:

- [Visual Studio Express 2013 per Web](https://www.microsoft.com/visualstudio/) o versione successiva

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

Per eseguire gli esercizi in questo laboratorio pratico, è necessario configurare prima di tutto l'ambiente.

1. Aprire una finestra di Windows Explorer e passare a del lab **origine** cartella.
2. Fare doppio clic su **Setup. cmd** e selezionare **Esegui come amministratore** per avviare il processo di installazione che la configurazione dell'ambiente e installare i frammenti di codice di Visual Studio per questo ambiente lab.
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

1. [Utilizzo di dati in tempo reale tramite SignalR](#Exercise1)
2. [Scalabilità orizzontale mediante SQL Server](#Exercise2)

Tempo stimato per completare questa esercitazione: **60 minuti**

> [!NOTE]
> Al primo avvio di Visual Studio, è necessario selezionare una delle raccolte di impostazioni predefinite. Ogni raccolta predefinita è pensata per associare un particolare stile di sviluppo e determina i layout delle finestre, il comportamento dell'editor, frammenti di codice IntelliSense e opzioni della finestra di dialogo. Le procedure descritte in questa esercitazione vengono descritte le azioni necessarie per eseguire una determinata attività in Visual Studio quando si usa la **delle impostazioni di sviluppo generale** raccolta. Se si sceglie una raccolta di impostazioni diverse per l'ambiente di sviluppo, possono essere presenti differenze nei passaggi che è necessario tenere conto.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a>Esercizio 1: Utilizzo di dati in tempo reale tramite SignalR

Mentre chat viene spesso usata come esempio, è possibile eseguire un intero molte altre funzionalità Web in tempo reale. Ogni volta che un utente consente di aggiornare una pagina web per visualizzare i nuovi dati o implementa la pagina Ajax tempo di polling per recuperare nuovi dati, è possibile usare SignalR.

SignalR supporta **push del server** oppure **broadcasting** funzionalità; gestisce automaticamente la gestione della connessione. In connessioni HTTP classica per la comunicazione tra client e server, viene ristabilita per ogni richiesta di connessione, ma SignalR fornisce una connessione permanente tra il client e server. In SignalR che il codice server effettua una chiamata a un codice client nel browser tramite le chiamate RPC (Remote Procedure), anziché il modello di richiesta-risposta sappiamo oggi stesso.

In questo esercizio, si configurerà il **fanatico Quiz** applicazione per usare SignalR per visualizzare il dashboard di statistiche e le metriche aggiornate senza la necessità di aggiornare l'intera pagina.

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a>Attività 1: esplorare la pagina delle statistiche di Quiz fanatico

In questa attività verrà passano attraverso l'applicazione e verificare come viene visualizzata la pagina delle statistiche e come è possibile migliorare il modo le informazioni vengono aggiornate.

1. Aprire **Visual Studio Express 2013 per il Web** e aprire il **GeekQuiz.sln** soluzione che si trova nel **Source\Ex1 WorkingWithRealTimeData\Begin** cartella.
2. Premere **F5** per eseguire la soluzione. Il **Accedi** schermata dovrebbe essere visualizzata nel browser.

    ![Esecuzione della soluzione](real-time-web-applications-with-signalr/_static/image2.png "esecuzione della soluzione")

    *Esecuzione della soluzione*
3. Fare clic su **registrare** nell'angolo superiore destro della pagina per creare un nuovo utente nell'applicazione.

    ![Registrare collegamento](real-time-web-applications-with-signalr/_static/image3.png "collegamento Register")

    *Collegamento Register*
4. Nel **registrare** pagina, immettere una **nome utente** e **Password**, quindi fare clic su **registrare**.

    ![La registrazione di un utente](real-time-web-applications-with-signalr/_static/image4.png "la registrazione di un utente")

    *La registrazione di un utente*
5. L'applicazione Registra nuovo account e l'utente viene autenticato e reindirizzato alla home page che mostra la prima domanda di quiz.
6. Aprire il **statistiche** pagina in una nuova finestra e inserire le **Home** pagina e **statistiche** pagina side-by-side.

    ![Windows side-by-side](real-time-web-applications-with-signalr/_static/image5.png "Side-by windows lato")

    *Windows side-by-side*
7. Nel **Home** pagina, rispondere alla domanda facendo clic su una delle opzioni.

    ![Rispondere alla domanda](real-time-web-applications-with-signalr/_static/image6.png "rispondere alla domanda")

    *Rispondere alla domanda*
8. Dopo aver fatto clic su uno dei pulsanti, apparirà la risposta.

    ![Rispondere alla domanda corretta](real-time-web-applications-with-signalr/_static/image7.png "rispondere alla domanda corretta")

    *Domande cui rispondere*
9. Si noti che le informazioni fornite nella pagina delle statistiche sono obsolete. Aggiornare la pagina per visualizzare i risultati aggiornati.

    ![Pagina statistiche](real-time-web-applications-with-signalr/_static/image8.png "pagina statistiche")

    *Pagina delle statistiche*
10. Tornare a Visual Studio e arrestare il debug.

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a>Attività 2: aggiunta di SignalR per fanatico Quiz per mostrare grafici Online

In questa attività verrà aggiungere SignalR alla soluzione e inviare gli aggiornamenti ai client automaticamente quando una nuova risposta viene inviata al server.

1. Dal **strumenti** menu in Visual Studio, selezionare **NuGet Gestione pacchetti**, quindi fare clic su **la Console di Gestione pacchetti**.
2. Nel **Console di gestione pacchetti** finestra, eseguire il comando seguente:

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    ![Installazione del pacchetto di SignalR](real-time-web-applications-with-signalr/_static/image9.png "installazione del pacchetto di SignalR")

    *Installazione del pacchetto di SignalR*

   > [!NOTE]
   > Quando si installa **SignalR** NuGet pacchetti versione 2.0.2 da una nuova applicazione MVC 5, sarà necessario aggiornare manualmente **OWIN** pacchetti alla versione 2.0.1 (o versione successiva) prima di installare SignalR. A tale scopo, è possibile eseguire lo script seguente nella **Console di gestione pacchetti**:
   > 
   > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
   > 
   > In una versione futura di SignalR, le dipendenze OWIN verranno aggiornate automaticamente.
3. Nella **Esplora soluzioni**, espandere il **script** cartella e notare che SignalR *js* sono stati aggiunti file alla soluzione.

    ![Riferimenti di SignalR JavaScript](real-time-web-applications-with-signalr/_static/image10.png "SignalR JavaScript fa riferimento")

    *Riferimenti di SignalR JavaScript*
4. In **Esplora soluzioni**, fare doppio clic il **GeekQuiz** progetto, selezionare **Add** | **nuova cartella**e denominarla  **Hub**.
5. Fare doppio clic il **hub** cartella e selezionare **Add | Nuovo elemento**.

    ![Aggiungi nuovo elemento](real-time-web-applications-with-signalr/_static/image11.png "Aggiungi nuovo elemento")

    *Aggiungi nuovo elemento*
6. Nel **Aggiungi nuovo elemento** finestra di dialogo, seleziona il **Visual c# | Web | SignalR** nodo nel riquadro sinistro, seleziona **classe tramite SignalR Hub (v2)** nel riquadro centrale, denominare il file **StatisticsHub.cs** e fare clic su **Aggiungi**.

    ![Finestra di dialogo Aggiungi nuovo elemento](real-time-web-applications-with-signalr/_static/image12.png "finestra di dialogo Aggiungi nuovo elemento")

    *Aggiungi finestra di dialogo Nuovo elemento*
7. Sostituire il codice nel **StatisticsHub** classe con il codice seguente.

    (Code - Snippet *RealTimeSignalR - Ex1 - StatisticsHubClass*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. Aprire **Startup.cs** e aggiungere la riga seguente alla fine delle **configurazione** (metodo).

    (Code - Snippet *RealTimeSignalR - Ex1 - MapSignalR*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. Aprire il **StatisticsService.cs** pagina all'interno di **servizi** cartella e aggiungere quanto segue direttive using.

    (Code - Snippet *RealTimeSignalR - Ex1 - UsingDirectives*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. Per notificare ai client connessi degli aggiornamenti, è innanzitutto recuperare una **contesto** oggetto per la connessione corrente. Il **Hub** oggetto contiene i metodi per inviare messaggi a un unico client o client di trasmettere a connesse tutte le soluzioni. Aggiungere il metodo seguente per il **StatisticsService** classe per trasmettere i dati delle statistiche.

    (Code - Snippet *RealTimeSignalR - Ex1 - NotifyUpdatesMethod*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > Nel codice precedente, si utilizza un nome di metodo arbitrario per chiamare una funzione nel client (ad esempio: *updateStatistics*). Il nome del metodo specificato viene interpretato come un oggetto dinamico, ovvero che non IntelliSense o convalida in fase di compilazione appositamente. L'espressione viene valutata in fase di esecuzione. Quando viene eseguita la chiamata al metodo, il nome del metodo e i valori dei parametri SignalR invia al client. Se il client dispone di un metodo che corrisponde al nome, che viene chiamato il metodo e i valori dei parametri vengono passati al metodo. Se non viene trovato alcun metodo corrispondente nel client, viene generato alcun errore. Per altre informazioni, consultare [Guida all'API di ASP.NET SignalR Hubs](../guide-to-the-api/hubs-api-guide-server.md).
11. Aprire il **TriviaController.cs** pagina all'interno la **controller** cartella e aggiungere quanto segue direttive using.

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. Aggiungere il codice evidenziato seguente per il **Post** metodo di azione.

    (Code - Snippet *RealTimeSignalR - Ex1 - NotifyUpdatesCall*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. Aprire il **Statistics.cshtml** pagina all'interno di **viste | Home** cartella. Individuare il **script** sezione e aggiungere i riferimenti allo script seguente all'inizio della sezione.

    (Code - Snippet *RealTimeSignalR - Ex1 - SignalRScriptReferences*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > Quando si aggiungono SignalR e altre librerie di script al progetto di Visual Studio, la gestione dei pacchetti potrebbe installare una versione del file script SignalR più recenti rispetto a quella illustrata in questo argomento. Assicurarsi che il riferimento allo script nel codice corrisponda alla versione della libreria di script installata nel progetto.
14. Aggiungere il codice evidenziato seguente per connettere il client per l'hub SignalR e aggiornare i dati delle statistiche quando viene ricevuto un nuovo messaggio dall'hub.

    (Code - Snippet *RealTimeSignalR - Ex1 - SignalRClientCode*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    In questo codice, la creazione di un Proxy di Hub e la registrazione di un gestore eventi per restare in attesa per i messaggi inviati dal server. In questo caso, in ascolto dei messaggi inviati tramite il *updateStatistics* (metodo).

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Attività 3: esecuzione della soluzione

In questa attività si eseguirà la soluzione per verificare che la visualizzazione delle statistiche viene aggiornata automaticamente tramite SignalR dopo aver risposto a una nuova domanda.

1. Premere **F5** per eseguire la soluzione.

    > [!NOTE]
    > Se non è già stato effettuato l'accesso all'applicazione, accedere con l'utente che è stato creato nell'attività 1.
2. Aprire il **statistiche** pagina in una nuova finestra e inserire le **Home** pagina e **statistiche** pagina side-by-side come è stato fatto nell'attività 1.
3. Nel **Home** pagina, rispondere alla domanda facendo clic su una delle opzioni.

    ![Rispondere a un'altra domanda](real-time-web-applications-with-signalr/_static/image13.png "rispondendo a un'altra domanda")

    *Rispondere a un'altra domanda*
4. Dopo aver fatto clic su uno dei pulsanti, apparirà la risposta. Si noti che le informazioni statistiche nella pagina viene aggiornate automaticamente dopo aver risposto alla domanda con le informazioni aggiornate senza la necessità di aggiornare l'intera pagina.

    ![Pagina statistiche aggiornata dopo la risposta](real-time-web-applications-with-signalr/_static/image14.png "pagina statistiche aggiornata dopo la risposta")

    *Pagina di statistiche aggiornata dopo la risposta*

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a>Esercizio 2: Scalabilità orizzontale mediante SQL Server

Quando si ridimensiona un'applicazione web, in genere è possibile scegliere tra *aumento* e *la scalabilità orizzontale* opzioni. *Scalabilità verticale* comporta l'utilizzo di un server di dimensioni maggiori, con più risorse (CPU, RAM e così via) durante *scalare in orizzontale* significa aggiungere più server per gestire il carico. Il problema può verificarsi con il secondo è che i client possono ottenere indirizzati a server diversi. Un client connesso a un server non riceverà i messaggi inviati da un altro server.

È possibile risolvere questi problemi usando un componente chiamato *backplane*per inoltrare i messaggi tra i server. Con un backplane abilitato, ogni istanza dell'applicazione invia messaggi al backplane e li inoltra il backplane alle altre istanze dell'applicazione.

Esistono attualmente tre tipi dei ripiani posteriori per SignalR:

- **Windows Azure Service Bus**. Service Bus è un'infrastruttura di messaggistica che consente ai componenti di invio di messaggi regime.
- **SQL Server**. Il backplane di SQL Server scrive i messaggi delle tabelle SQL. Backplane utilizza Service Broker di messaggistica efficiente. Tuttavia funziona anche se Service Broker non è abilitato.
- **Redis**. Redis è un archivio chiave-valore in memoria. Redis supporta un modello publish/subscribe ("pub/sub") per l'invio di messaggi.

Ogni messaggio viene inviato tramite un bus di messaggi. Un bus di messaggi implementa il [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interfaccia, che fornisce un'astrazione di pubblicazione/sottoscrizione. Sostituendo il valore predefinito di lavoro dei ripiani posteriori delle **IMessageBus** con bus progettato per il backplane.

Ogni istanza del server si connette al backplane tramite il bus. Quando viene inviato un messaggio, passa al backplane e backplane lo invia a ogni server. Quando un server riceve un messaggio da un backplane, archivia il messaggio nella propria cache locale. Il server recapita quindi i messaggi al client dalla cache locale.

Per altre informazioni sull'uso di backplane SignalR, leggere questo [articolo](../performance/scaleout-in-signalr.md).

> [!NOTE]
> Esistono alcuni scenari in cui un backplane può diventare un collo di bottiglia. Ecco alcuni scenari tipici di SignalR:
> 
> - [Trasmissione del server](tutorial-server-broadcast-with-signalr.md) (ad esempio, le quotazioni di borsa): I ripiani posteriori delle funziona bene per questo scenario, perché il server controlla la frequenza con cui vengono inviati i messaggi.
> - [Da client a](tutorial-getting-started-with-signalr.md) (ad esempio, avvia una chat): In questo scenario, il backplane potrebbe essere un collo di bottiglia se il numero di messaggi viene ridimensionato con il numero di client. vale a dire, se la frequenza dei messaggi aumenta in modo i client in modo proporzionale man mano join.
> - [In tempo reale ad alta frequenza](tutorial-high-frequency-realtime-with-signalr.md) (ad esempio i giochi in tempo reale): Backplane non è consigliato per questo scenario.


In questo esercizio si userà **SQL Server** per distribuire i messaggi tra le **fanatico Quiz** dell'applicazione. Si eseguirà queste attività in un computer singolo test per informazioni su come impostare la configurazione, ma per ottenere l'effetto completo, è necessario distribuire l'applicazione di SignalR in due o più server. È anche necessario installare SQL Server in uno dei server o in un server dedicato distinto.

![Scalabilità orizzontale mediante SQL Server diagramma](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a>Attività 1: comprendere lo Scenario

In questa attività si eseguiranno 2 istanze di **fanatico Quiz** simulando IIS più istanze nel computer locale. In questo scenario, per rispondere alle domande di elementi semplici su un'applicazione di aggiornamento non riceveranno una notifica nella pagina delle statistiche della seconda istanza. Questa simulazione è simile a un ambiente in cui l'applicazione viene distribuita in più istanze e l'utilizzo di un servizio di bilanciamento del carico per comunicare con essi.

1. Aprire il **Begin.sln** soluzione che si trova nel **inizio/origine/Ex2-ScalingOutWithSQLServer** cartella. Una volta caricata, si noterà nella **Esplora Server** che la soluzione contiene due progetti con identiche di strutture ma diversi nomi. Ciò consente di simulare due istanze della stessa applicazione in esecuzione nel computer locale.

    ![Iniziare a soluzione simulando 2 istanze del Quiz fanatico](real-time-web-applications-with-signalr/_static/image16.png "iniziare soluzione simulando 2 istanze del Quiz fanatico")

    *Iniziare a soluzione simulando 2 istanze del Quiz fanatico*
2. Aprire la pagina delle proprietà della soluzione facendo clic sul nodo della soluzione e selezionando **proprietà**. Sotto **progetto di avvio**, selezionare **progetti di avvio multipli** e modificare i **azione** valore per entrambi i progetti in *avviare*.

    ![Avviare più progetti](real-time-web-applications-with-signalr/_static/image17.png "avviare più progetti")

    *Avviare più progetti*
3. Premere **F5** per eseguire la soluzione. L'applicazione verrà avviata due istanze di **fanatico Quiz** in porte diverse, simulazione di più istanze della stessa applicazione. Aggiungere uno dei browser a sinistra e l'altro a destra dello schermo. Accedere con le credenziali o registrare un nuovo utente. Una volta effettuato l'accesso, mantenere la pagina elementi semplici a sinistra e passare al **statistiche** pagina nel browser sul lato destro.

    ![Quiz fanatico Side-by-Side](real-time-web-applications-with-signalr/_static/image18.png)

    *Quiz fanatico Side-by-Side*

    ![Quiz fanatico in porte diverse](real-time-web-applications-with-signalr/_static/image19.png)

    *Quiz fanatico in porte diverse*
4. Iniziare a rispondere alle domande nel browser a sinistra e si noterà che il **statistiche** pagina del browser a destra non viene aggiornata. Infatti **SignalR** memorizzare nella cache locale viene utilizzato per distribuire i messaggi tra i client e questo scenario simula più istanze, pertanto la cache non è condivisa tra di essi. È possibile verificare che **SignalR** stia funzionando, gli stessi passaggi di test, ma con una singola app. Nelle attività seguenti si configurerà un backplane per replicare i messaggi tra istanze.
5. Tornare a Visual Studio e arrestare il debug.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a>Attività 2: creazione di SQL Server Backplane

In questa attività si creerà un database che verrà utilizzato come un backplane per il **fanatico Quiz** dell'applicazione. Si userà **Esplora oggetti di SQL Server** per esplorare il server e inizializzare il database. Inoltre, si abiliterà il **Service Broker**.

1. Nella **Visual Studio**aprire dal menu **View** e selezionare **Esplora oggetti di SQL Server**.
2. Connettersi all'istanza di Local DB facendo clic con il **SQL Server** nodo e selezionando **Aggiungi SQL Server...**  opzione.

    ![Aggiunta di un'istanza di SQL Server](real-time-web-applications-with-signalr/_static/image20.png "aggiunta di un'istanza di SQL Server")

    *Aggiunta di un'istanza di SQL Server da Esplora oggetti di SQL Server*
3. Impostare il **nome server** al *\v11.0 (localdb)* e lasciare **l'autenticazione di Windows** come modalità di autenticazione. Fare clic su **Connect** per continuare.

    ![La connessione a LocalDB](real-time-web-applications-with-signalr/_static/image21.png "la connessione a LocalDB")

    *La connessione a LocalDB*
4. Ora che si è connessi all'istanza di Local DB, è necessario creare un database che rappresenterà il backplane di SQL Server per SignalR. A tale scopo, fare doppio clic il **database** nodo e selezionare **aggiungere un nuovo Database**.

    ![Aggiunta di un nuovo database](real-time-web-applications-with-signalr/_static/image22.png "aggiungendo un nuovo database")

    *Aggiunta di un nuovo database*
5. Impostare il nome del database su *SignalR* e fare clic su **OK** per crearlo.

    ![Creazione del database di SignalR](real-time-web-applications-with-signalr/_static/image23.png "creazione del database di SignalR")

    *Creazione del database di SignalR*

    > [!NOTE]
    > È possibile scegliere qualsiasi nome per il database.
6. Per ricevere gli aggiornamenti in modo più efficiente da un backplane, è consigliabile abilitare Service Broker per il database. Service Broker fornisce il supporto nativo per la messaggistica e Accodamento in SQL Server. Backplane funziona anche senza Service Broker. Aprire una nuova query facendo clic con il database e quindi scegliere **nuova Query**.

    ![Apertura di una nuova Query](real-time-web-applications-with-signalr/_static/image24.png "aprendo una nuova Query")

    *Apertura di una nuova Query*
7. Per verificare se Service Broker è abilitato, eseguire una query il **viene\_broker\_abilitata** colonna il **Sys. Databases** vista del catalogo. Eseguire lo script seguente nella finestra di query aperti di recente.

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    ![Ricerca dello stato di Service Broker](real-time-web-applications-with-signalr/_static/image25.png "ricerca dello stato di Service Broker")

    *Ricerca dello stato di Service Broker*
8. Se il valore della **viene\_broker\_abilitata** colonna nel database è &quot;0&quot;, usare il comando seguente per abilitarlo. Sostituire **&lt;YOUR-DATABASE&gt;** con il nome è impostato durante la creazione del database (ad esempio: SignalR).

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    ![Abilitare Service Broker](real-time-web-applications-with-signalr/_static/image26.png "abilitare Service Broker")

    *Abilitare Service Broker*

    > [!NOTE]
    > Se questa query viene visualizzato un deadlock, assicurarsi che non sono presenti applicazioni connesse al database.

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a>Attività 3: configurazione dell'applicazione di SignalR

In questa attività si configurerà **fanatico Quiz** per connettersi al backplane SQL Server. Prima di tutto si aggiungerà il **SignalR.SqlServer** stringa pacchetto NuGet e impostare la connessione al database backplane.

1. Aprire il **Console di gestione pacchetti** dalla **Tools** > **Gestione pacchetti NuGet**. Assicurarsi che **GeekQuiz** selezionato nel progetto di **progetto predefinito** elenco a discesa. Digitare il comando seguente per installare il **Microsoft.AspNet.SignalR.SqlServer** pacchetto NuGet.

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. Ripetere il passaggio precedente, ma questa volta per il progetto **GeekQuiz2**.
3. Per configurare il backplane di SQL Server, aprire il **Startup.cs** file del **GeekQuiz** del progetto e aggiungere il codice seguente per il **configura** (metodo). Sostituire **&lt;YOUR-DATABASE&gt;** con il nome del database usata durante la creazione il backplane di SQL Server. Ripetere questo passaggio per la **GeekQuiz2** progetto.

    (Code - Snippet *RealTimeSignalR - Ex2 - StartupConfiguration*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. Ora che entrambi i progetti configurati per utilizzare il backplane di SQL Server, premere **F5** eseguirli contemporaneamente.
5. Anche in questo caso **Visual Studio** avvierà due istanze di **fanatico Quiz** in porte diverse. Aggiungere uno dei browser a sinistra e l'altro a destra dello schermo e accedere con le proprie credenziali. Mantenere la pagina elementi semplici a sinistra e passare a **statistiche** pagein del browser a destra.
6. Iniziare a rispondere alle domande nel browser a sinistra. Questa volta, il **statistiche** pagina viene aggiornata grazie al backplane. Passare tra le applicazioni (**statistiche** ora è a sinistra, e **elementi semplici** è a destra) e ripetere il test per convalidare che è in corso per entrambe le istanze. Backplane viene utilizzato come un *cache condivisa* dei messaggi per tutti i server collegati e ogni server verranno archiviati i messaggi nella propria cache locale per la distribuzione ai client connessi.
7. Tornare a Visual Studio e arrestare il debug.
8. Il componente backplane di SQL Server genera automaticamente le tabelle necessarie nel database specificato. Nel **Esplora oggetti di SQL Server** panel, aprire il database creato per il backplane (ad esempio: SignalR) ed espandere le tabelle. Verrà visualizzata nelle tabelle seguenti:

    ![Backplane generate tabelle](real-time-web-applications-with-signalr/_static/image27.png)

    *Backplane generate tabelle*
9. Fare doppio clic il **SignalR.Messages\_0** tabella e selezionare **visualizzare i dati**.

    ![Tabella della vista SignalR Backplane messaggi](real-time-web-applications-with-signalr/_static/image28.png)

    *Tabella della vista SignalR Backplane messaggi*
10. È possibile visualizzare i diversi messaggi inviati per il **Hub** per rispondere alle domande gli elementi semplici. Backplane distribuisce questi messaggi per qualsiasi istanza connessa.

    ![Tabella di messaggi backplane](real-time-web-applications-with-signalr/_static/image29.png)

    *Tabella di messaggi backplane*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

In questo laboratorio pratico, si è appreso come aggiungere **SignalR** alle applicazione e inviare notifiche dal server ai client connessi mediante **hub**. Inoltre, si è appreso come scalare orizzontalmente l'applicazione usando un *backplane* componente quando l'applicazione viene distribuita in più istanze IIS.
