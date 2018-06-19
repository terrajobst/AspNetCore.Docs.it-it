---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: 'Lab pratico: Le applicazioni Web in tempo reale con SignalR | Documenti Microsoft'
author: rick-anderson
description: Applicazioni Web in tempo reale offrono la possibilità di effettuare il push del contenuto ai client connessi non appena si verifica in tempo reale sul lato server. Per gli sviluppatori ASP.NET, ASP...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 5a2bc120ded18ad2302fd6c5cde65a5323e86ca8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878048"
---
<a name="hands-on-lab-real-time-web-applications-with-signalr"></a>Lab pratico: Le applicazioni Web in tempo reale con SignalR
====================
Da [categorie Web Team](https://twitter.com/webcamps)

[Download Web categorie Kit di formazione](http://aka.ms/webcamps-training-kit)

> Applicazioni Web in tempo reale offrono la possibilità di effettuare il push del contenuto ai client connessi non appena si verifica in tempo reale sul lato server. Per gli sviluppatori ASP.NET, **ASP.NET SignalR** è una raccolta per aggiungere funzionalità web in tempo reale alle applicazioni. Consente di usufruire dei trasporti diversi, il trasporto disponibile meglio assegnato il client e più disponibile il trasporto del server di selezione automatica. Consente di usufruire del **WebSocket**, un'API di HTML5 che consente la comunicazione bidirezionale tra il browser e server.
> 
> **SignalR** fornisce inoltre un'API semplice di alto livello per l'esecuzione di server per client RPC (chiamata di funzioni JavaScript nei browser dei client dal codice .NET sul lato server) in un'applicazione ASP.NET, nonché l'aggiunta utili hook per la gestione connessione, ad esempio connettere/disconnettere gli eventi, le connessioni di raggruppamento e autorizzazione.
> 
> **SignalR** è un'astrazione su alcuni dei trasporti che sono necessari per svolgere il lavoro in tempo reale tra client e server. Oggetto **SignalR** connessione viene avviato come HTTP e quindi viene promosso a un **WebSocket** connessione, se disponibile. **WebSocket** è il trasporto ideale per **SignalR**, dal momento che semplifica l'utilizzo più efficiente della memoria del server, la latenza più bassa, che ha le funzionalità più sottostante (ad esempio la comunicazione full duplex tra client e Server), ma include anche i requisiti più rigidi: **WebSocket** richiede che il server utilizzino **Windows Server 2012** oppure **Windows 8**, insieme a **.NET framework 4.5**. Se non vengono soddisfatti questi requisiti, **SignalR** tenterà di utilizzare altri trasporti per rendere le connessioni (ad esempio *Ajax polling prolungato*).
> 
> Il **SignalR** API contiene due modelli per la comunicazione tra client e server: **connessioni permanenti** e **hub**. Oggetto **connessione** rappresenta un endpoint di tipo semplice per l'invio singolo destinatario, raggruppati o trasmissione di messaggi. Oggetto **Hub** è una pipeline più alto livello basata sull'API di connessione che consente il client e server chiamare i metodi direttamente tra loro.
> 
> ![Architettura di SignalR](real-time-web-applications-with-signalr/_static/image1.png)
> 
> Tutto il codice di esempio e i frammenti di codice sono inclusi nel Web categorie Training Kit, disponibile all'indirizzo [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Panoramica

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questa esercitazione pratica, si apprenderà come:

- Inviare notifiche dal server al client utilizzando SignalR.
- Scalabilità orizzontale SignalR applicazione utilizzando **SQL Server**.

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Di seguito è necessario per completare questa esercitazione pratica:

- [Visual Studio Express 2013 per Web](https://www.microsoft.com/visualstudio/) o versione successiva

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

Per eseguire gli esercizi di questa esercitazione pratica, è necessario configurare prima di tutto l'ambiente.

1. Aprire una finestra di Esplora risorse e individuare il lab **origine** cartella.
2. Fare doppio clic su **Setup.cmd** e selezionare **Esegui come amministratore** per avviare il processo di installazione che la configurazione dell'ambiente e installare i frammenti di codice di Visual Studio per questo ambiente lab.
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

1. [Utilizzo di dati in tempo reale tramite SignalR](#Exercise1)
2. [Scalabilità orizzontale mediante SQL Server](#Exercise2)

Tempo stimato per completare questa esercitazione: **60 minuti**

> [!NOTE]
> Al primo avvio di Visual Studio, è necessario selezionare una delle raccolte di impostazioni predefinite. Ogni raccolta predefinita è progettato in modo che corrisponda a un particolare stile di sviluppo e determina il layout di finestra, il comportamento dell'editor, frammenti di codice IntelliSense e finestre di dialogo. Le procedure descritte in questa esercitazione vengono descritte le azioni necessarie per eseguire una determinata attività in Visual Studio quando si utilizza il **impostazioni generali per lo sviluppo** insieme. Se si sceglie una raccolta di impostazioni diverse per l'ambiente di sviluppo, possono essere presenti differenze nei passaggi che è necessario prendere in considerazione.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a>Esercizio 1: Utilizzo di dati in tempo reale tramite SignalR

Mentre chat viene spesso utilizzata come esempio, è possibile eseguire un intero molte altre funzionalità Web in tempo reale. Ogni volta che un utente aggiorna una pagina web per visualizzare i nuovi dati o l'oggetto implementa pagina Ajax lungo il polling per recuperare nuovi dati, è possibile utilizzare SignalR.

Supporta SignalR **push server** o **broadcast** funzionalità; gestisce automaticamente la gestione della connessione. In connessioni HTTP classiche per la comunicazione tra client e server, viene ristabilita per ogni richiesta di connessione, ma SignalR fornisce una connessione permanente tra il client e server. In SignalR che il codice server effettua una chiamata a un codice del client nel browser utilizzando le chiamate RPC (Remote Procedure), anziché il modello di richiesta-risposta sappiamo oggi.

In questo esercizio, si configurerà il **appassionato Quiz** applicazione per l'utilizzo di SignalR per visualizzare il dashboard di statistiche e le metriche aggiornate senza la necessità di aggiornare la pagina intera.

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a>Attività 1: la pagina di statistiche Quiz appassionato di esplorazione

In questa attività verrà passare attraverso l'applicazione e verificare come viene visualizzata la pagina statistiche e come è possibile migliorare la modalità di informazioni viene aggiornata.

1. Aprire **Visual Studio Express 2013 per Web** e aprire il **GeekQuiz.sln** soluzione si trova nel **Source\Ex1 WorkingWithRealTimeData\Begin** cartella.
2. Premere **F5** per eseguire la soluzione. Il **Accedi** verrà visualizzata la pagina nel browser.

    ![Esecuzione della soluzione](real-time-web-applications-with-signalr/_static/image2.png "esecuzione della soluzione")

    *Esecuzione della soluzione*
3. Fare clic su **registrare** nell'angolo superiore destro della pagina per creare un nuovo utente nell'applicazione.

    ![Registrare collegamento](real-time-web-applications-with-signalr/_static/image3.png "collegamento registra")

    *Registrare collegamento*
4. Nel **registrare** pagina, immettere un **nome utente** e **Password**, quindi fare clic su **registrare**.

    ![Registrazione di un utente](real-time-web-applications-with-signalr/_static/image4.png "registrazione di un utente")

    *Registrazione di un utente*
5. L'applicazione Registra nuovo account e l'utente è autenticato e reindirizzato alla home page che mostra la prima domanda quiz.
6. Aprire il **statistiche** pagina in una nuova finestra e inserire il **Home** pagina e **statistiche** pagina side-by-side.

    ![Windows side-by-side](real-time-web-applications-with-signalr/_static/image5.png "lato da windows side")

    *Windows side-by-side*
7. Nel **Home** pagina, rispondere alla domanda facendo clic su una delle opzioni.

    ![Rispondendo a una domanda](real-time-web-applications-with-signalr/_static/image6.png "rispondere")

    *Rispondendo a una domanda*
8. Dopo aver fatto clic su uno dei pulsanti, la risposta dovrebbe essere visualizzato.

    ![Risposta corretta](real-time-web-applications-with-signalr/_static/image7.png "risposta corretta")

    *Domanda risposta correttamente*
9. Si noti che le informazioni fornite nella pagina delle statistiche sono obsolete. Aggiornare la pagina per visualizzare i risultati aggiornati.

    ![Pagina statistiche](real-time-web-applications-with-signalr/_static/image8.png "pagina statistiche")

    *Pagina statistiche*
10. Tornare a Visual Studio e arrestare il debug.

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a>Attività 2: aggiunta di SignalR per il Quiz appassionato per visualizzare i grafici in linea

In questa attività verrà aggiunta SignalR alla soluzione e inviare gli aggiornamenti ai client automaticamente quando una nuova risposta viene inviata al server.

1. Dal **strumenti** menu in Visual Studio, selezionare **Gestione pacchetti libreria**, quindi fare clic su **Console di gestione pacchetti**.
2. Nel **Package Manager Console** finestra, eseguire il comando seguente:

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    ![Installazione del pacchetto SignalR](real-time-web-applications-with-signalr/_static/image9.png "installazione del pacchetto SignalR")

    *Installazione del pacchetto SignalR*

   > [!NOTE]
   > Quando si installa **SignalR** versione di pacchetti di NuGet 2.0.2 da una nuova applicazione MVC 5, è necessario aggiornare manualmente **OWIN** dei pacchetti alla versione 2.0.1 (o versione successiva) prima di installare SignalR. A tale scopo, è possibile eseguire lo script seguente nel **Package Manager Console**:
   > 
   > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
   > 
   > In una versione futura di SignalR, dipendenze OWIN verranno aggiornate automaticamente.
3. In **Esplora**, espandere il **script** cartella e notare che SignalR *js* file sono stati aggiunti alla soluzione.

    ![Fa riferimento a SignalR JavaScript](real-time-web-applications-with-signalr/_static/image10.png "fa riferimento a SignalR JavaScript")

    *Riferimenti di SignalR JavaScript*
4. In **Esplora**, fare doppio clic su di **GeekQuiz** progetto, selezionare **Aggiungi** | **nuova cartella**e denominarla  **Gli hub**.
5. Fare doppio clic su di **hub** cartella e selezionare **Aggiungi | Nuovo elemento**.

    ![Aggiungi nuovo elemento](real-time-web-applications-with-signalr/_static/image11.png "Aggiungi nuovo elemento")

    *Aggiungi nuovo elemento*
6. Nel **Aggiungi nuovo elemento** la finestra di dialogo, seleziona il **Visual c# | Web | SignalR** nodo nel riquadro a sinistra, seleziona **classe Hub SignalR (v2)** dal riquadro centrale, denominare il file **StatisticsHub.cs** e fare clic su **Aggiungi**.

    ![Finestra di dialogo Aggiungi nuovo elemento](real-time-web-applications-with-signalr/_static/image12.png "finestra di dialogo Aggiungi nuovo elemento")

    *Aggiungi finestra di dialogo Nuovo elemento*
7. Sostituire il codice di **StatisticsHub** classe con il codice seguente.

    (- Frammento di codice *StatisticsHubClass RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. Aprire **Startup.cs** e aggiungere la riga seguente alla fine del **configurazione** metodo.

    (- Frammento di codice *MapSignalR RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. Aprire il **StatisticsService.cs** pagina all'interno di **servizi** cartella e aggiungere le seguenti direttive using.

    (- Frammento di codice *UsingDirectives RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. Per notificare ai client connessi degli aggiornamenti, è innanzitutto necessario recuperare un **contesto** oggetto per la connessione corrente. Il **Hub** oggetto contiene i metodi per inviare messaggi a un unico client o di trasmissione per tutti i client. Aggiungere il metodo seguente per il **StatisticsService** classe per trasmettere i dati delle statistiche.

    (- Frammento di codice *NotifyUpdatesMethod RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > Nel codice precedente, si utilizza un nome di metodo arbitrario per chiamare una funzione nel client (ad esempio: *updateStatistics*). Il nome del metodo specificato viene interpretato come un oggetto dinamico, ovvero che nessuna IntelliSense o la relativa convalida in fase di compilazione. L'espressione viene valutata in fase di esecuzione. Quando viene eseguita la chiamata al metodo, il nome del metodo e i valori dei parametri SignalR invia al client. Se il client dispone di un metodo che corrisponde al nome, tale metodo viene chiamato e i valori dei parametri vengono passati al metodo. Se viene trovato alcun metodo di corrispondenza nel client, viene generato alcun errore. Per ulteriori informazioni, vedere [ASP.NET SignalR hub API Guida](../guide-to-the-api/hubs-api-guide-server.md).
11. Aprire il **TriviaController.cs** pagina all'interno di **controller** cartella e aggiungere le seguenti direttive using.

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. Aggiungere il codice evidenziato di seguito per il **Post** metodo di azione.

    (- Frammento di codice *NotifyUpdatesCall RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. Aprire il **Statistics.cshtml** pagina all'interno di **viste | Home** cartella. Individuare il **script** sezione e aggiungere i riferimenti a script seguente all'inizio della sezione.

    (- Frammento di codice *SignalRScriptReferences RealTimeSignalR - Ex1 -*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > Quando si aggiungono SignalR e altre librerie di script al progetto di Visual Studio, la gestione dei pacchetti potrebbe installare una versione del file di script SignalR più recenti rispetto a quella illustrata in questo argomento. Assicurarsi che il riferimento allo script nel codice corrisponda alla versione della libreria di script installata nel progetto.
14. Aggiungere il codice evidenziato di seguito per connettere il client per l'hub SignalR e aggiornare i dati delle statistiche quando viene ricevuto un nuovo messaggio dall'hub.

    (- Frammento di codice *SignalRClientCode RealTimeSignalR - Ex1 -*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    In questo codice, la creazione di un Proxy dell'Hub e registrare un gestore eventi per restare in attesa per i messaggi inviati dal server. In questo caso, si è in ascolto dei messaggi inviati mediante il *updateStatistics* metodo.

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Attività 3: esecuzione della soluzione

In questa attività si eseguirà la soluzione per verificare che la visualizzazione delle statistiche viene aggiornata automaticamente mediante SignalR dopo la risposta a una nuova domanda.

1. Premere **F5** per eseguire la soluzione.

    > [!NOTE]
    > Se non è già stato effettuato l'accesso all'applicazione, accedere con l'utente creato nell'attività 1.
2. Aprire il **statistiche** pagina in una nuova finestra e inserire il **Home** pagina e **statistiche** pagina side-by-side, come descritto nell'attività 1.
3. Nel **Home** pagina, rispondere alla domanda facendo clic su una delle opzioni.

    ![Rispondendo a un'altra domanda](real-time-web-applications-with-signalr/_static/image13.png "rispondendo a un'altra domanda")

    *Rispondendo a un'altra domanda*
4. Dopo aver fatto clic su uno dei pulsanti, la risposta dovrebbe essere visualizzato. Si noti che le informazioni sulle statistiche nella pagina viene aggiornate automaticamente dopo la risposta alla domanda con le informazioni aggiornate senza la necessità di aggiornare la pagina intera.

    ![Pagina statistiche aggiornata dopo la risposta](real-time-web-applications-with-signalr/_static/image14.png "pagina statistiche aggiornata dopo la risposta")

    *Pagina statistiche aggiornato dopo la risposta*

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a>Esercizio 2: Scalabilità orizzontale mediante SQL Server

Quando si ridimensiona un'applicazione web, in genere è possibile scegliere tra *scalabilità* e *la scalabilità orizzontale* opzioni. *Applicare la scalabilità verticale* comporta l'utilizzo di un server più grande, con più risorse (CPU, RAM e così via) durante *scalare in orizzontale* comporta l'aggiunta di più server per gestire il carico. Il problema con il secondo è che i client possono ottenere indirizzati al server diversi. Un client connesso a un server non riceverà i messaggi inviati da un altro server.

È possibile risolvere questi problemi usando un componente chiamato *backplane*per inoltrare i messaggi tra server. Con un backplane è abilitato, ogni istanza dell'applicazione invia messaggi al backplane e li inoltra backplane alle altre istanze dell'applicazione.

Esistono attualmente tre tipi dei ripiani posteriori per SignalR:

- **Windows Azure Service Bus**. Bus di servizio è un'infrastruttura di messaggistica che consente ai componenti di inviare messaggi regime di controllo.
- **SQL Server**. SQL Server backplane scrive i messaggi alle tabelle SQL. Backplane utilizza Service Broker per la messaggistica efficiente. Tuttavia, funziona anche se Service Broker non è abilitato.
- **Redis**. Redis è un archivio chiave-valore in memoria. Redis supporta un modello di pubblicazione/sottoscrizione ("pubblicazione/sottoscrizione") per l'invio di messaggi.

Ogni messaggio viene inviato tramite un bus di messaggi. Implementa un bus di messaggi di [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interfaccia, che fornisce un'astrazione di pubblicazione/sottoscrizione. Sostituendo il valore predefinito di lavoro dei ripiani posteriori delle **IMessageBus** con un bus progettato per tale backplane.

Ogni istanza del server si connette al backplane tramite il bus. Quando viene inviato un messaggio, passa al backplane e backplane invia ogni server. Quando un server riceve un messaggio dal backplane, archivia il messaggio nella propria cache locale. Il server recapita i messaggi per i client dalla cache locale.

Per ulteriori informazioni sull'uso di backplane SignalR, leggere questo [articolo](../performance/scaleout-in-signalr.md).

> [!NOTE]
> Esistono alcuni scenari in cui un backplane può diventare un collo di bottiglia. Ecco alcuni scenari tipici di SignalR:
> 
> - [Trasmissione server](tutorial-server-broadcast-with-signalr.md) (ad esempio quotazioni di borsa): i ripiani posteriori delle funziona anche per questo scenario, perché il server controlla la frequenza con cui vengono inviati i messaggi.
> - [Da client](tutorial-getting-started-with-signalr.md) (ad esempio, chat): In questo scenario, backplane potrebbe essere un collo di bottiglia se il numero di messaggi viene ridimensionato con il numero di client; vale a dire, se la velocità dei messaggi aumenta proporzionalmente man mano i client si aggiungono.
> - [In tempo reale ad alta frequenza](tutorial-high-frequency-realtime-with-signalr.md) (ad esempio, giochi in tempo reale): un backplane non è consigliato per questo scenario.


In questo esercizio, si utilizzerà **SQL Server** per distribuire i messaggi tra il **appassionato Quiz** dell'applicazione. Eseguire queste attività in un computer singolo test per informazioni su come impostare la configurazione, ma per ottenere l'effetto completo, è necessario distribuire l'applicazione di SignalR per due o più server. È inoltre necessario installare SQL Server in uno dei server o in un server dedicato.

![Scalabilità orizzontale utilizzando SQL Server diagramma](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a>Attività 1: comprendere lo Scenario

In questa attività si eseguiranno 2 istanze di **appassionato Quiz** simulando IIS più istanze nel computer locale. In questo scenario, quando si risponde alle domande gli elementi semplici su un'applicazione, è non riceverà alcuna notifica aggiornamento nella pagina delle statistiche della seconda istanza. La simulazione è simile a un ambiente in cui l'applicazione viene distribuita in più istanze e l'utilizzo di un servizio di bilanciamento del carico per comunicare con essi.

1. Aprire il **Begin.sln** soluzione si trova nel **origine/Ex2-ScalingOutWithSQLServer/inizio** cartella. Una volta caricato, si noterà nel **Esplora Server** che la soluzione include due progetti con nomi diversi ma le strutture. Ciò consente di simulare in esecuzione due istanze della stessa applicazione sul computer locale.

    ![Iniziare soluzione simulando 2 istanze del Quiz appassionato](real-time-web-applications-with-signalr/_static/image16.png "iniziare soluzione simulando 2 istanze del Quiz appassionato")

    *Iniziare soluzione simulando 2 istanze del Quiz appassionato*
2. Aprire la pagina delle proprietà della soluzione facendo clic sul nodo della soluzione e selezionando **proprietà**. In **progetto di avvio**selezionare **più progetti di avvio** e modificare il **azione** valore per entrambi i progetti per *avviare*.

    ![Avviare più progetti](real-time-web-applications-with-signalr/_static/image17.png "avviare più progetti")

    *Avviare più progetti*
3. Premere **F5** per eseguire la soluzione. L'applicazione verrà avviata due istanze di **appassionato Quiz** in porte diverse, la simulazione di più istanze della stessa applicazione. Aggiungere uno dei browser a sinistra e l'altro a destra dello schermo. Accedere con le credenziali o registrare un nuovo utente. Una volta effettuato l'accesso, mantenere la pagina gli elementi semplici a sinistra e passare al **statistiche** pagina nel browser sulla destra.

    ![Appassionato Quiz Side-by-Side](real-time-web-applications-with-signalr/_static/image18.png)

    *Appassionato Quiz Side-by-Side*

    ![Quiz appassionato in porte diverse](real-time-web-applications-with-signalr/_static/image19.png)

    *Quiz appassionato in porte diverse*
4. Avviare rispondere alle domande nel browser a sinistra e si noterà che il **statistiche** pagina nel browser destra non viene aggiornato. In questo modo **SignalR** memorizzare nella cache locale viene utilizzato per distribuire i messaggi tra i client e questo scenario è la simulazione di più istanze, pertanto la cache non è condivisa tra di essi. È possibile verificare che **SignalR** stia funzionando, gli stessi passaggi di test, ma con una singola app. Nelle attività seguente verrà configurato un backplane per replicare i messaggi tra istanze.
5. Tornare a Visual Studio e arrestare il debug.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a>Attività 2: creazione di SQL Server Backplane

In questa attività si creerà un database che verrà utilizzato come un backplane per il **appassionato Quiz** dell'applicazione. Si utilizzerà **Esplora oggetti di SQL Server** per esplorare il server e inizializzare il database. Inoltre, si abiliterà il **Service Broker**.

1. In **Visual Studio**, Apri menu **vista** e selezionare **Esplora oggetti di SQL Server**.
2. Connettersi all'istanza di LocalDB facendo clic con il **SQL Server** nodo e selezionando **aggiungere SQL Server...**  opzione.

    ![Aggiunta di un'istanza del Server SQL](real-time-web-applications-with-signalr/_static/image20.png "aggiunta di un'istanza di SQL Server")

    *Aggiunta di un'istanza di SQL Server in Esplora oggetti di SQL Server*
3. Impostare il **nome server** a *(localdb) \v11.0* e lasciare **l'autenticazione di Windows** come modalità di autenticazione. Fare clic su **Connetti** per continuare.

    ![Connessione a LocalDB](real-time-web-applications-with-signalr/_static/image21.png "la connessione a LocalDB")

    *Connessione a LocalDB*
4. Ora che si è connessi all'istanza di LocalDB, sarà necessario creare un database che rappresenterà il backplane di SQL Server per SignalR. A tale scopo, fare doppio clic su di **database** nodo e selezionare **aggiungere un nuovo Database**.

    ![Aggiunge un nuovo database](real-time-web-applications-with-signalr/_static/image22.png "aggiunge un nuovo database")

    *Aggiunge un nuovo database*
5. Impostare il nome del database *SignalR* e fare clic su **OK** per la sua creazione.

    ![Creazione del database SignalR](real-time-web-applications-with-signalr/_static/image23.png "creazione del database di SignalR")

    *Creazione del database di SignalR*

    > [!NOTE]
    > È possibile scegliere qualsiasi nome per il database.
6. Per ricevere gli aggiornamenti in modo più efficiente da backplane, è consigliabile abilitare Service Broker per il database. Service Broker fornisce supporto nativo per la messaggistica e Accodamento messaggi in SQL Server. Backplane funziona anche senza Service Broker. Aprire una nuova query facendo clic con il database e selezionare **nuova Query**.

    ![Apertura di una nuova Query](real-time-web-applications-with-signalr/_static/image24.png "aperta una nuova Query")

    *Apertura di una nuova Query*
7. Per controllare se Service Broker è abilitato, eseguire una query di **è\_broker\_abilitato** colonna il **Sys. Databases** vista del catalogo. Eseguire lo script seguente nella finestra di query aperti di recente.

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    ![La ricerca dello stato di Service Broker](real-time-web-applications-with-signalr/_static/image25.png "la ricerca dello stato di Service Broker")

    *La ricerca dello stato di Service Broker*
8. Se il valore della **è\_broker\_abilitato** colonna del database è &quot;0&quot;, usare il comando seguente per abilitarlo. Sostituire **&lt;del DATABASE&gt;** con il nome è impostato quando si crea il database (ad esempio: SignalR).

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    ![Abilitazione di Service Broker](real-time-web-applications-with-signalr/_static/image26.png "abilitare Service Broker")

    *Abilitazione di Service Broker*

    > [!NOTE]
    > Se questa query viene visualizzato per verificare un deadlock, assicurarsi che non sono presenti applicazioni connesse al database.

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a>Attività 3: configurazione dell'applicazione di SignalR

In questa attività si configurerà **appassionato Quiz** per connettersi al backplane SQL Server. Innanzitutto si aggiungerà il **SignalR.SqlServer** pacchetto NuGet e impostare la connessione al database backplane di stringa.

1. Aprire il **Console di gestione pacchetti** da **strumenti** | **Gestione pacchetti libreria**. Assicurarsi che **GeekQuiz** selezionato nel progetto di **progetto predefinito** elenco a discesa. Digitare il comando seguente per installare il **italiano** pacchetto NuGet.

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. Ripetere il passaggio precedente, ma questa volta per progetto **GeekQuiz2**.
3. Per configurare il backplane di SQL Server, aprire il **Startup.cs** file del **GeekQuiz** del progetto e aggiungere il codice seguente per il **configura** metodo. Sostituire **&lt;del DATABASE&gt;** con il nome del database utilizzato per la creazione di backplane di SQL Server. Ripetere questo passaggio per la **GeekQuiz2** progetto.

    (- Frammento di codice *StartupConfiguration RealTimeSignalR - Ex2 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. Ora che entrambi i progetti configurati per utilizzare il backplane di SQL Server, premere **F5** eseguirli contemporaneamente.
5. Nuovamente, **Visual Studio** avvierà due istanze di **appassionato Quiz** in porte diverse. Aggiungere uno dei browser sulla sinistra e l'altro a destra dello schermo e accedere con le credenziali. Mantenere la pagina gli elementi semplici a sinistra e passare a **statistiche** pagein il browser di destra.
6. Avviare la risposta alle domande nel browser a sinistra. Questa volta il **statistiche** ringraziamenti backplane aggiornamento della pagina. Commutatore tra applicazioni (**statistiche** è ora a sinistra e **gli elementi semplici** è a destra) e ripetere il test per convalidare il funzionamento di entrambe le istanze. Funge da backplane un *cache condivisa* dei messaggi per tutti i server collegati e ogni server verranno archiviati i messaggi nella propria cache locale per la distribuzione ai client connessi.
7. Tornare a Visual Studio e arrestare il debug.
8. Il componente backplane di SQL Server genera automaticamente le tabelle necessarie nel database specificato. Nel **Esplora oggetti di SQL Server** pannello, aprire il database creato per backplane (ad esempio: SignalR) ed espandere le tabelle. Verrà visualizzato nelle tabelle seguenti:

    ![Backplane generato tabelle](real-time-web-applications-with-signalr/_static/image27.png)

    *Backplane generato tabelle*
9. Fare doppio clic su di **SignalR.Messages\_0** tabella e selezionare **Visualizza dati**.

    ![Tabella di messaggi SignalR Backplane di visualizzazione](real-time-web-applications-with-signalr/_static/image28.png)

    *Tabella di messaggi SignalR Backplane di visualizzazione*
10. È possibile visualizzare i diversi messaggi inviati al **Hub** per rispondere alle domande gli elementi semplici. Backplane distribuisce i messaggi in qualsiasi istanza connessa.

    ![Tabella di messaggi backplane](real-time-web-applications-with-signalr/_static/image29.png)

    *Tabella di messaggi backplane*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

In questa esercitazione pratica, si è appreso come aggiungere **SignalR** per le notifiche di trasmissione e di applicazione dal server ai client connessi tramite **hub**. Inoltre, si è appreso come scalare orizzontalmente l'applicazione utilizzando un *backplane* componente quando l'applicazione viene distribuita in più istanze IIS.
