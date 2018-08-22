---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: Modello di lavoro incentrato sulle code (creazione di App Cloud funzionanti con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione Real World di App Cloud con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure consigliate che egli può...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: c454023c26d73df174a43b3a8ad6745ef8654fd2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826329"
---
<a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>Modello di lavoro incentrato sulle code (creazione di App Cloud funzionanti con Azure)
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download risolverlo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **creazione Real World di App Cloud con Azure** eBook si basa su una presentazione sviluppata da Scott Guthrie. Viene illustrato 13 modelli e procedure consigliate che consentono di avere esito positivo lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [capitolo prima](introduction.md).


Versioni precedenti, abbiamo visto che con più servizi può comportare un contratto di servizio "composto", in cui contratto di servizio effettivo dell'app è il *prodotto* singoli contratti di servizio. Ad esempio, l'app Fix It Usa siti Web, archiviazione e Database SQL. Se uno di questi servizi non riesce, l'app restituirà un errore all'utente.

La memorizzazione nella cache è un buon metodo per gestire gli errori temporanei per il contenuto di sola lettura. Ma cosa accade se l'applicazione deve eseguire le operazioni? Ad esempio, quando l'utente invia un'attività nuova Fix It, l'app non è sufficiente inserire l'attività nella cache. L'app deve scrivere l'attività Fix It in un archivio dati permanente, in modo che possa essere elaborato.

Questo è il modello di lavoro incentrato sulle code entra in gioco. Questo modello consente a regime di controllo libero tra un livello web e un servizio back-end.

Ecco come funziona il modello. Quando l'applicazione riceve una richiesta, inserisce un elemento di lavoro in una coda e la risposta viene restituito immediatamente. Quindi un processo di back-end separati estrae gli elementi di lavoro dalla coda ed esegue il lavoro.

Il modello di lavoro incentrato sulle code è utile per:

- Lavoro che è molto tempo (una latenza elevata).
- Operazioni che richiedono un servizio esterno che potrebbe non essere sempre disponibile.
- Il lavoro a elevato utilizzo di risorse (CPU elevato).
- Lavoro che possono trarre vantaggio dalla velocità livellamento (soggetta a picchi improvvisi di carico).

## <a name="reduced-latency"></a>Riduzione della latenza

Le code sono utili ogni volta che vengono eseguite operazioni che richiedono molto tempo. Se un'attività richiede pochi secondi o più, anziché bloccare l'utente finale, inserita l'elemento di lavoro in una coda. Informare l'utente "Ci stiamo lavorando" e quindi usare un listener della coda per elaborare l'attività in background.

Ad esempio, quando si acquista un articolo in un punto vendita online, il sito web viene confermato il tuo ordine immediatamente. Ma ciò non significa che le opzioni è già in un autocarro recapitato. Un'attività da inserire in una coda e in background stanno eseguendo la verifica del credito, gli elementi di preparazione per la spedizione e così via.

Per gli scenari con una breve latenza, tempo totale end-to-end potrebbe essere più lungo mediante una coda, rispetto all'esecuzione delle attività in modo sincrono. Ma anche in questo caso gli altri vantaggi possono compensare tale svantaggio.

## <a name="increased-reliability"></a>Una maggiore affidabilità

Nella versione di Fix It che sono state state osservando finora, il front-end web è strettamente con il back-end di Database SQL. Se il servizio database SQL è disponibile, l'utente riceve un errore. Se i tentativi non funzionano (vale a dire, l'errore è temporaneo più di), l'unica cosa è possibile eseguire viene visualizzato un messaggio di errore e chiedere all'utente di riprovare più tardi.

![Diagramma che mostra front-end web non superato se back-end del Database SQL non è riuscita](queue-centric-work-pattern/_static/image1.png)

Uso delle code, quando un utente invia un'attività Fix It, l'app scrive un messaggio alla coda. Il payload del messaggio è un [JSON](http://json.org/) rappresentazione dell'attività. Non appena il messaggio viene scritto nella coda, l'app restituisce e mostra immediatamente un messaggio di conferma all'utente.

Se i servizi back-end, ad esempio il database SQL o il listener della coda, passare alla modalità offline, gli utenti possono comunque inviare nuove attività Fix It. I messaggi verranno accodate solo fino a quando non sono disponibili anche in questo caso i servizi back-end. A questo punto, i servizi back-end verranno aggiornata nel backlog.

![Diagramma che mostra web front-end continua a funzionare quando si verifica un errore di Database SQL](queue-centric-work-pattern/_static/image2.png)

Inoltre, ora è possibile aggiungere più logica di back-end senza doversi preoccupare di resilienza per il front-end. Ad esempio, voler inviare un messaggio di posta elettronica o messaggio SMS al proprietario, ogni volta che viene assegnato un nuovo Fix It. Se il messaggio di posta elettronica o SMS non è più disponibile, è possibile elaborare tutti gli altri elementi e quindi inserire un messaggio in una coda separata per l'invio di messaggi di posta elettronica/SMS.

In precedenza, il contratto di servizio effettivo era Web Apps &times; archiviazione &times; Database SQL = % 99,7. (Vedere [progettazione per sopravvivere a errori](design-to-survive-failures.md).)

Quando si modifica l'app di usare una coda, il front-end web dipende solo le app Web e di archiviazione, per un contratto di servizio del 99,8% composito. Si noti che le code sono parte del servizio di archiviazione di Azure, pertanto vengono inclusi nel contratto di servizio stesso come risorsa di archiviazione blob.

Se è necessario anche meglio 99,8%, è possibile creare due code in due aree diverse. Designare uno come primario e l'altro come secondario. Nell'app, eseguire il failover alla coda secondaria se la coda primaria non è disponibile. La probabilità di entrambi è disponibile contemporaneamente è molto piccola.

## <a name="rate-leveling-and-independent-scaling"></a>Il livellamento delle velocità e scalabilità indipendente

Le code sono utili anche per un elemento denominato *frequenza livellamento* oppure *livellamento del carico*.

Le app Web sono spesso soggette a picchi improvvisi di traffico. Sebbene sia possibile usare la scalabilità automatica per aggiungere automaticamente i server web per gestire il traffico web un aumento, la scalabilità automatica potrebbe non essere in grado di reagire in modo sufficientemente rapido per gestire picchi improvvisi di carico. Se i server web possono trasferire una parte del lavoro che sarà più possibile scrivendo un messaggio a una coda, possono gestire più traffico. Un servizio back-end può quindi leggere i messaggi dalla coda ed elaborarli. La profondità della coda verrà aumentare o ridurre in base alla variazione del carico in ingresso.

Con la maggior parte delle operazioni che richiedono molto tempo viene gestita da un servizio back-end, il livello web più facilmente può rispondere a picchi improvvisi di traffico. E risparmiare denaro in quanto qualsiasi quantità di traffico specificato può essere gestita da un numero inferiore di server web.

È possibile scalare il livello web e il servizio back-end in modo indipendente. Ad esempio, potrebbe essere necessario ma un solo server l'elaborazione della coda dei messaggi e tre i server web. O se si esegue un'attività a elevato utilizzo di calcolo in background, potrebbe essere necessario ulteriori server di back-end.

![](queue-centric-work-pattern/_static/image3.png)

La scalabilità automatica funziona con i servizi back-end nonché con il livello web. È possibile aumentare o ridurre il numero di macchine virtuali che elaborano le attività in coda, in base l'utilizzo della CPU di macchine virtuali di back-end. In alternativa, è possibile una scalabilità automatica in base al numero di elementi presenti in una coda. Ad esempio, è possibile indicare la scalabilità automatica per tentare di mantenere non più di 10 elementi nella coda. Se la coda contiene più di 10 elementi, la scalabilità automatica per aggiungere le macchine virtuali. Quando vengono aggiornati, scalabilità automatica verrà disinstallazione le macchine virtuali aggiuntive.

## <a name="adding-queues-to-the-fix-it-application"></a>Aggiunta di lo mette in coda per la correzione dell'applicazione

Per implementare il modello di accodamento, è necessario apportare due modifiche all'app Fix It.

- Quando un utente invia una nuova Fix It attività, inserire l'attività in coda, anziché la scrittura nel database.
- Creare un servizio back-end che elabora i messaggi nella coda.

Per la coda, si userà il [servizio di archiviazione code di Azure](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/). Un'altra opzione consiste nell'usare [Bus di servizio di Azure](https://docs.microsoft.com/azure/service-bus/).

Per decidere quale servizio di Accodamento da usare, considerare come l'app deve inviare e ricevere i messaggi in coda:

- Se hai che cooperano ai producer e consumer concorrenti, è consigliabile usare servizio di archiviazione code di Azure. "Produttori che hanno collaborato" significa che più processi sono aggiungere messaggi a una coda. "Consumer concorrente" significa che più processi esegue il pull dei messaggi dalla coda per l'elaborazione, ma ogni messaggio può essere elaborato solo da un "utente". Se è necessaria una velocità effettiva maggiore rispetto a cui è possibile ottenere con una singola coda, usare le code aggiuntive e/o altri account di archiviazione.
- Se è necessario un [modello di pubblicazione/sottoscrizione](http://en.wikipedia.org/wiki/Publish/subscribe), è consigliabile usare le code del Bus di servizio di Azure.

L'app Fix It è appropriato per i producer che cooperano e modello di consumer concorrenti.

Un'altra considerazione riguarda la disponibilità dell'applicazione. Il servizio di archiviazione di Accodamento fa parte dello stesso servizio che viene usato per l'archiviazione blob, in modo da usarlo non ha alcun effetto sul contratto di servizio. Azure Service Bus è un servizio separato con il proprio contratto di servizio. Se usassimo le code del Bus di servizio, è necessario tenere in considerazione una percentuale di contratto di servizio aggiuntiva e contratto di servizio composito sarà inferiore. Quando si sceglie un servizio di accodamento, assicurarsi di che comprendere l'impatto di propria scelta sulla disponibilità dell'applicazione. Per altre informazioni, vedere la [risorse](#resources) sezione.

## <a name="creating-queue-messages"></a>Creazione di messaggi in coda

Per inserire un'attività Fix It nella coda, il front-end web vengono eseguiti i passaggi seguenti:

1. Creare un [CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx) istanza. Il `CloudQueueClient` istanza viene usata per eseguire le richieste nel servizio di Accodamento.
2. Creare la coda, se non esiste ancora.
3. Serializzare l'attività Fix It.
4. Chiamare [CloudQueue.AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx) per inserire il messaggio in coda.

Faremo questo lavoro nel costruttore e `SendMessageAsync` di un nuovo metodo `FixItQueueManager` classe.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

Qui viene usato il [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) libreria per serializzare il fixit in formato JSON. È possibile usare qualsiasi approccio di serializzazione si preferisce. JSON presenta il vantaggio di essere leggibile dall'utente, pur essendo meno dettagliati rispetto a XML.

Codice di produzione di qualità sarebbe aggiungere logica di gestione degli errori, sospendere se il database risulta non disponibile, gestione del recupero più pulito, creare la coda all'avvio dell'applicazione e gestire "[dei messaggi non elaborabili" messaggi](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx). (Un messaggio non elaborabile è un messaggio che non può essere elaborato per qualche motivo. Per evitare che i messaggi non elaborabili per entrare nella coda, in cui il ruolo di lavoro continuamente tenterà di elaborarli, esito negativo, ripetere l'operazione, esito negativo e così via.)

Nell'applicazione MVC front-end, è necessario aggiornare il codice che crea una nuova attività. Anziché inserire l'attività nel repository, chiamare il `SendMessageAsync` metodo illustrato in precedenza.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>L'elaborazione dei messaggi di coda

Per elaborare i messaggi nella coda, verrà creato un servizio back-end. Il servizio back-end verrà eseguito un ciclo infinito che esegue i passaggi seguenti:

1. Ottiene il messaggio successivo dalla coda.
2. Deserializzare il messaggio a un'attività Fix It.
3. Scrivere l'attività Fix It nel database.

Per ospitare il servizio back-end, si creerà un servizio Cloud di Azure che contiene un *ruolo di lavoro*. Un ruolo di lavoro è costituito da una o più VM che possono eseguire l'elaborazione back-end. Il codice eseguito in queste macchine virtuali effettuerà il pull dei messaggi dalla coda man mano che diventano disponibili. Per ogni messaggio, verrà deserializzare il payload JSON e scrivere un'istanza dell'entità di correzione, attività per il database, usando lo stesso repository che è stato usato in precedenza nel livello web.

I passaggi seguenti illustrano come aggiungere un ruolo di lavoro progetto di ruolo per una soluzione che include un progetto web standard. Questi passaggi hanno già eseguiti nel Fix It progetto che è possibile scaricare.

Aggiungere innanzitutto un progetto servizio Cloud alla soluzione Visual Studio. La soluzione e scegliere **Add**, quindi **nuovo progetto**. Nel riquadro sinistro, espandere **Visual c#** e selezionare **Cloud**.

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

Nel **nuovo servizio Cloud di Azure** finestra di dialogo espandere il **Visual c#** nodo nel riquadro sinistro. Selezionare **ruolo di lavoro** e fare clic sull'icona freccia destra.

![](queue-centric-work-pattern/_static/image6.png)

(Si noti che è anche possibile aggiungere un *ruolo web*. È stato possibile eseguirlo il correggere front-end nello stesso servizio Cloud anziché eseguirla in un sito Web di Azure. Che presenta alcuni vantaggi in rendendo più semplice coordinare le connessioni tra server front-end e back-end. Tuttavia, per mantenere semplice questa demo, abbiamo stiamo mantenendo il front-end in Azure App Service Web App e solo il back-end in esecuzione in un servizio Cloud.)

Per il ruolo di lavoro viene assegnato un nome predefinito. Per modificare il nome, posizionare il mouse sopra il ruolo di lavoro nel riquadro di destra, quindi fare clic sull'icona a forma di matita.

![](queue-centric-work-pattern/_static/image7.png)

Fare clic su **OK** per completare la finestra di dialogo. Aggiunge due progetti alla soluzione di Visual Studio.

- un progetto Azure che definisce il servizio cloud, incluse le informazioni di configurazione.
- Un progetto di ruolo di lavoro che definisce il ruolo di lavoro.

![](queue-centric-work-pattern/_static/image8.png)

Per altre informazioni, vedere [la creazione di un progetto Azure con Visual Studio.](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

All'interno del ruolo di lavoro, si esegue il polling per i messaggi chiamando il `ProcessMessageAsync` metodo di `FixItQueueManager` classe che abbiamo visto in precedenza.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

Il `ProcessMessagesAsync` metodo verifica la presenza di un messaggio in attesa. Se è presente uno, viene deserializzato il messaggio in un `FixItTask` entità e Salva l'entità nel database. Esegue un ciclo fino a quando la coda è vuota.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

Eseguire il polling per i messaggi in coda comporta una transazione di piccole dimensioni applicato un addebito, pertanto quando non è presente alcun messaggio in attesa di essere elaborati, il ruolo di lavoro `RunAsync` metodo attende un secondo prima di polling nuovamente chiamando `Task.Delay(1000)`.

In un progetto web, aggiunta di codice asincrono di migliorare automaticamente le prestazioni perché IIS gestisce un pool di thread limitato. Ciò non avviene in un progetto di ruolo di lavoro. Per migliorare la scalabilità del ruolo di lavoro, è possibile scrivere il codice a thread multipli o usare codice asincrono per implementare [programmazione parallela](https://msdn.microsoft.com/library/ff963553.aspx). L'esempio non implementa la programmazione parallela, ma illustra come rendere il codice asincrono in modo che è possibile implementare la programmazione parallela.

## <a name="summary"></a>Riepilogo

In questo capitolo è stato spiegato come migliorare la velocità di risposta dell'applicazione, l'affidabilità e scalabilità dall'implementazione del pattern di lavoro incentrato sulle code.

Questa è l'ultima 13 modelli illustrati in questo e-book, ma sono ovviamente disponibili molti altri modelli e procedure consigliate che consentono di compilare App cloud di successo. Il [capitolo finale](more-patterns-and-guidance.md) vengono forniti collegamenti alle risorse per gli argomenti non sono stati inclusi in questi 13 modelli.

<a id="resources"></a>
## <a name="resources"></a>Risorse

Per altre informazioni sulle code, vedere le risorse seguenti.

Documentazione:

- [Microsoft Azure Storage code parte 1: Introduzione a](http://justazure.com/microsoft-azure-storage-queues-part-1-getting-started/). Articolo di Schacherl romano.
- [L'esecuzione di attività in Background](https://msdn.microsoft.com/library/ff803365.aspx), il capitolo 5 del [spostando le applicazioni nel Cloud, 3rd Edition](https://msdn.microsoft.com/library/ff728592.aspx) da Microsoft Patterns and Practices. (In particolare, la sezione ["Usando le code di archiviazione di Azure"](https://msdn.microsoft.com/library/ff803365.aspx#sec7).)
- [Procedure consigliate per ottimizzare la scalabilità e la convenienza delle soluzioni di messaggistica basata su coda in Azure](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx). White paper per Valery Mizonov.
- [Confronto tra le code di Azure e code del Bus di servizio](https://msdn.microsoft.com/magazine/jj159884.aspx). Articolo di MSDN Magazine, fornisce informazioni aggiuntive che consentono di scegliere quale servizio di Accodamento da usare. L'articolo indica che il Bus di servizio sia dipendente da ACS per l'autenticazione, ovvero che le code di Service bus sarebbero disponibili quando ACS è disponibile. Tuttavia, poiché l'articolo è stato scritto, Service bus è stata modificata per consentono di usare [token di firma di accesso condiviso](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx) come alternativa ad ACS.
- [Microsoft Patterns and Practices - informazioni aggiuntive su Azure](https://msdn.microsoft.com/library/dn568099.aspx). Vedere Introduzione alla messaggistica asincrona, modello di pipe e filtri, modello di transazioni di compensazione, modello di consumer concorrenti, modello CQRS.
- [Passaggio a CQRS](https://msdn.microsoft.com/library/jj554200). E-book su CQRS di Microsoft Patterns and Practices.

Video:

- [Operatore alternativo: Creazione di servizi Cloud scalabili, resilienti](https://channel9.msdn.com/Series/FailSafe). Serie video in nove parti Marc Mercuri, Ulrich Homann e Mark Simms. Presenta i concetti generali e principi architetturali in modo estremamente accessibile e interessano, con storie ricavate dall'esperienza di Microsoft Customer Advisory Team (CAT) con i clienti effettivi. Per un'introduzione al servizio di archiviazione di Azure e code, vedere episodio 5 partire 35:13.

> [!div class="step-by-step"]
> [Precedente](distributed-caching.md)
> [Successivo](more-patterns-and-guidance.md)
