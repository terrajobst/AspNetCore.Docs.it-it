---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: Modello basate su coda di lavoro (creazione di applicazioni con Azure Cloud del mondo reale) | Documenti Microsoft
author: MikeWasson
description: "Le App per Cloud mondo reale compilazione con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure che è possibile..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: ccfbaa26cbf610f847811e6f3c612458277046ed
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>Modello basate su coda di lavoro (creazione di applicazioni Cloud reale in Azure)
====================
da [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download correggerlo progetto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [E-book di Download](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **predefiniti reale World Cloud App con Azure** e-book è basato su una presentazione sviluppata da Scott Guthrie. Vengono descritte le 13 modelli e procedure consigliate che consentono di avere esito negativo con lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [primo capitolo](introduction.md).


In precedenza, è stato illustrato che con più servizi può comportare un contratto di servizio "composito", in cui contratto di servizio effettivo dell'app è il *prodotto* di contratti di servizio singoli. Ad esempio, l'app Correggi Usa siti Web, archiviazione e Database SQL. Se uno di questi servizi non riesce, l'app restituirà un errore all'utente.

La memorizzazione nella cache è un buon metodo per gestire gli errori temporanei per il contenuto di sola lettura. Ma cosa accade se l'applicazione deve eseguire il lavoro? Ad esempio, quando l'utente invia una nuova Correggi attività, l'app non è possibile inserire l'attività nella cache. L'applicazione deve scrivere l'attività Correggi in un archivio dati persistente, in modo da elaborare.

Ovvero in cui è disponibile il modello di lavoro basate su coda in. Questo modello consente l'accoppiamento debole tra un livello web e un servizio back-end.

Ecco il funzionamento di modello. Quando l'applicazione riceve una richiesta, inserisce un elemento di lavoro in una coda e la risposta viene restituito immediatamente. Quindi un processo separato di back-end estrae gli elementi di lavoro dalla coda ed esegue il lavoro.

È utile per il modello di lavoro basate su coda:

- Lavoro in tempo (latenza elevata).
- Operazioni che richiedono un servizio esterno che potrebbe non essere sempre disponibile.
- Il lavoro a elevato utilizzo di risorse (CPU elevata).
- Lavoro che può costituire un vantaggio dalla frequenza livellamento (soggetta a picchi di carico improvviso).

## <a name="reduced-latency"></a>Riduzione della latenza

Le code sono utili ogni volta che si sta eseguendo il lavoro richiesto molto tempo. Se un'attività richiede pochi secondi o più, anziché bloccare l'utente finale, inserire l'elemento di lavoro in una coda. Informare l'utente "Stiamo lavorando su di esso" e quindi utilizzare un listener della coda per elaborare l'attività in background.

Ad esempio, quando si acquista un elemento in un punto vendita online, il sito web viene confermata l'ordine immediatamente. Ma ciò non significa che tutti i contenuti sono già in un autocarro recapitato. Un'attività e inseriti in una coda, e in background stia eseguendo la verifica del credito, gli elementi di preparazione per la spedizione e così via.

Per gli scenari con latenza brevi, il tempo totale di end-to-end potrebbe essere più lungo mediante una coda, rispetto all'esecuzione delle attività in modo sincrono. Ma anche in questo caso, gli altri vantaggi possono annullare tale svantaggio.

## <a name="increased-reliability"></a>Aumentare l'affidabilità

Nella versione di Correggi che si stata stato esaminando finora, web front-end è strettamente con il back-end di Database SQL. Se il servizio database SQL è disponibile, l'utente riceve un errore. Se non funzionano tentativi (ovvero, l'errore è temporaneo più), l'unico elemento che è possibile eseguire è visualizzato un errore e chiedere all'utente e riprovare.

![Diagramma che mostra front-end web con esito negativo quando il Database SQL back-end ha esito negativo](queue-centric-work-pattern/_static/image1.png)

Utilizzo delle code, quando un utente invia un'attività di correzione, l'applicazione scrive un messaggio alla coda. Il payload del messaggio è un [JSON](http://json.org/) rappresentazione dell'attività. Non appena il messaggio viene scritto nella coda, l'applicazione restituisce e viene immediatamente un messaggio di conferma all'utente.

Se uno dei servizi back-end: ad esempio il database SQL o il listener della coda, passare alla modalità offline, agli utenti di inviare ancora nuove attività Correggi. I messaggi solo verranno inseriti nella coda fino a quando non sono disponibili i servizi back-end nuovamente. A questo punto, i servizi back-end verranno aggiornarsi nel backlog.

![Diagramma che illustra web front-end continuare a funzionare quando si verifica un errore di Database SQL](queue-centric-work-pattern/_static/image2.png)

Inoltre, ora è possibile aggiungere più logica di back-end senza preoccuparsi di adattabilità di front-end. Ad esempio, si può valutare di inviare un messaggio di posta elettronica o messaggio SMS al proprietario ogni volta che un nuovo correggere è assegnato. Se il messaggio di posta elettronica o il servizio SMS è disponibile, è possibile elaborare tutti gli altri elementi e quindi inserire un messaggio in una coda separata per l'invio di messaggi di posta elettronica o SMS.

Il contratto di servizio effettivo è stato in precedenza, le applicazioni Web &times; archiviazione &times; SQL Database = 99,7%. (Vedere [progettazione sopravvivenza errori](design-to-survive-failures.md).)

Quando si modifica l'applicazione di utilizzare una coda, il front-end web dipende solo le applicazioni Web e di archiviazione, per un contratto di servizio % 99,8 composito. Si noti che le code fanno parte del servizio di archiviazione di Azure, sono inclusi nel contratto di servizio stesso come spazio di archiviazione blob.

Se è necessario anche prestazioni migliori rispetto 99,8%, è possibile creare due code in due aree diverse. Designare uno come primario, mentre l'altro come secondario. Nell'app, eseguire il failover alla coda secondaria se la coda primaria non è disponibile. La possibilità di entrambi non è disponibile allo stesso tempo è molto piccola.

## <a name="rate-leveling-and-independent-scaling"></a>Livellamento frequenza e la scalabilità indipendente

Le code sono utili anche per un elemento denominato *frequenza livellamento* o *livellamento del carico*.

Le app Web sono spesso soggette a improvvisi del traffico. Sebbene sia possibile utilizzare la scalabilità automatica per aggiungere automaticamente i server web per gestire il traffico web maggiore, il ridimensionamento automatico non potrebbe essere in grado di rispondere in modo sufficientemente rapido per gestire picchi improvvisi di carico. Se i server web possono trasferire una parte del lavoro che sarà più possibile scrivendo un messaggio a una coda, possono gestire più traffico. Un servizio back-end può quindi leggere i messaggi dalla coda ed elaborarle. La profondità della coda verrà espansione o riduzione in base alla variazione del carico in ingresso.

Con la maggior parte delle operazioni richiede molto tempo viene gestita da un servizio back-end, livello web più facilmente può rispondere a picchi improvvisi di traffico. E risparmiare perché qualsiasi determinata quantità di traffico può essere gestito da un numero inferiore di server web.

È possibile aumentare il livello web e il servizio back-end in modo indipendente. Ad esempio, potrebbe essere necessario ma coda messaggi l'elaborazione di un solo server e tre i server web. O se si esegue un'attività a elevato utilizzo di calcolo in background, potrebbe essere necessario ulteriori server back-end.

![](queue-centric-work-pattern/_static/image3.png)

Il ridimensionamento automatico funziona con i servizi back-end e con il livello di web. È possibile applicare la scalabilità verticale o ridurre il numero di macchine virtuali che le attività in coda, in base all'utilizzo della CPU di back-end le macchine virtuali in esecuzione. In alternativa, è possibile in base a quanti elementi sono in una coda di scalabilità automatica. Ad esempio, si possono indicare di scalabilità automatica per tentare di mantenere non più di 10 elementi nella coda. Se la coda contiene più di 10 elementi, scalabilità automatica per aggiungere le macchine virtuali. Quando si aggiornarsi, scalabilità automatica interrompe le macchine virtuali aggiuntive.

## <a name="adding-queues-to-the-fix-it-application"></a>Aggiunta in coda per la correzione dell'applicazione

Per implementare il modello di coda, è necessario apportare due modifiche all'app Correggi.

- Quando un utente invia una nuova Correggi attività, inserire l'attività in coda, anziché la scrittura nel database.
- Creare un servizio back-end che elabora i messaggi nella coda.

Per la coda, verrà usato il [servizio di archiviazione di Azure coda](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/). Un'altra opzione consiste nell'utilizzare [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).

Per decidere quale servizio di Accodamento da usare, considerare come l'app deve inviare e ricevere i messaggi in coda:

- Se si hanno collaborato producer e consumer concorrenti, considerare l'utilizzo di servizio di archiviazione di Accodamento di Azure. "Produttori che hanno collaborato" significa che più processi sono aggiunta a una coda di messaggi. "Consumer concorrenti" significa più processi sono pull dei messaggi dalla coda per l'elaborazione, ma un determinato messaggio può essere elaborato solo da un "utente". Se è necessaria una velocità effettiva maggiore rispetto a cui è possibile ottenere con una singola coda, utilizzare code aggiuntive e/o account di archiviazione aggiuntivi.
- Se è necessario un [modello di pubblicazione/sottoscrizione](http://en.wikipedia.org/wiki/Publish/subscribe), è consigliabile utilizzare le code di Azure Service Bus.

L'app Correggi adatta il produttori che hanno collaborato e il modello di consumer concorrenti.

Un'altra considerazione riguarda la disponibilità dell'applicazione. Il servizio di archiviazione della coda fa parte dello stesso servizio che viene usata per l'archiviazione blob, in modo da utilizzarlo non ha alcun effetto sul nostro contratto di servizio. Service Bus di Azure è un servizio separato con il proprio contratto di servizio. Se utilizzassimo code del Bus di servizio, è necessario tenere in considerazione una percentuale di contratto di servizio aggiuntiva, e il contratto di servizio composito sarà inferiore. Se si sceglie un servizio di accodamento, assicurarsi che comprendere l'impatto di propria scelta sulla disponibilità dell'applicazione. Per ulteriori informazioni, vedere il [risorse](#resources) sezione.

## <a name="creating-queue-messages"></a>Creazione di messaggi in coda

Per inserire un'attività di correzione nella coda, il front-end web esegue i passaggi seguenti:

1. Creare un [CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx) istanza. Il `CloudQueueClient` istanza viene utilizzata per eseguire le richieste nel servizio di Accodamento.
2. Creare la coda, se non esiste ancora.
3. L'attività di correzione della serializzazione.
4. Chiamare [CloudQueue.AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx) per inserire il messaggio nella coda.

È possibile eseguire questa operazione nel costruttore e `SendMessageAsync` metodo di un nuovo `FixItQueueManager` classe.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

Di seguito viene usato il [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) libreria per serializzare il fixit in formato JSON. È possibile utilizzare qualsiasi approccio di serializzazione desiderato. JSON ha il vantaggio di essere leggibile, pur essendo meno dettagliati rispetto ai dati XML.

Codice di produzione potrebbe aggiungere logica di gestione degli errori, sospendere se il database è diventato non disponibile, gestire più pulite ripristino, creare la coda all'avvio dell'applicazione e gestire "[non elaborabile" messaggi](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx). (Un messaggio non elaborabile è un messaggio che non può essere elaborato per qualche motivo. Per evitare che i messaggi non elaborabili per cui si trovano nella coda, in cui il ruolo di lavoro continuamente tenta di elaborarle, esito negativo, ripetere l'operazione, esito negativo e così via.)

Nell'applicazione MVC front-end, è necessario aggiornare il codice che crea una nuova attività. Anziché inserire l'attività nel repository, chiamare il `SendMessageAsync` metodo illustrato in precedenza.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>L'elaborazione di messaggi in coda

Per elaborare i messaggi nella coda, verrà creato un servizio back-end. Il servizio back-end verrà eseguito un ciclo infinito che effettua le operazioni seguenti:

1. Ottenere il messaggio successivo dalla coda.
2. Deserializzare il messaggio a un'attività di correzione.
3. Scrivere l'attività di correzione nel database.

Per ospitare il servizio back-end, si creerà un servizio Cloud di Azure che contiene un *ruolo di lavoro*. Un ruolo di lavoro è costituito da uno o più macchine virtuali che possono eseguire operazioni di elaborazione back-end. Il codice eseguito in queste macchine virtuali verrà il pull dei messaggi dalla coda appena diventano disponibili. Per ogni messaggio, viene deserializzare il payload JSON e scrivere un'istanza dell'entità di correzione, attività per il database, utilizzando lo stesso repository utilizzata in precedenza nel livello web.

La procedura seguente mostra come aggiungere un processo di lavoro di progetto di ruolo a una soluzione che include un progetto web standard. Questi passaggi sono già nel Correggi progetto che è possibile scaricare.

Aggiungere innanzitutto un progetto servizio Cloud per la soluzione di Visual Studio. La soluzione e scegliere **Aggiungi**, quindi **nuovo progetto**. Nel riquadro sinistro, espandere **Visual c#** e selezionare **Cloud**.

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

Nel **nuovo servizio Cloud Azure** finestra di dialogo, espandere il **Visual c#** nodo nel riquadro sinistro. Selezionare **ruolo di lavoro** e fare clic sull'icona freccia destra.

![](queue-centric-work-pattern/_static/image6.png)

(Si noti che è anche possibile aggiungere un *ruolo web*. È possibile eseguire il correggere front-end nello stesso servizio Cloud anziché eseguirla in un sito Web di Azure. Che presenta alcuni vantaggi di rendere più semplice coordinare le connessioni tra server front-end e back-end. Tuttavia, per mantenere semplice questa demo, è sta mantenendo il front-end in un'applicazione Web di Azure App Service e solo back-end in esecuzione in un servizio Cloud.)

Viene assegnato un nome predefinito per il ruolo di lavoro. Per modificare il nome, posizionare il mouse sopra il ruolo di lavoro nel riquadro di destra, quindi fare clic sull'icona della matita.

![](queue-centric-work-pattern/_static/image7.png)

Fare clic su **OK** per completare la finestra di dialogo. Aggiunge due progetti alla soluzione Visual Studio.

- un progetto Azure che definisce il servizio cloud, incluse le informazioni di configurazione.
- Un progetto di ruolo di lavoro che definisce il ruolo di lavoro.

![](queue-centric-work-pattern/_static/image8.png)

Per ulteriori informazioni, vedere [creazione di un progetto Azure con Visual Studio.](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

All'interno del ruolo di lavoro, si esegue il polling per i messaggi chiamando il `ProcessMessageAsync` metodo la `FixItQueueManager` classe che è stato illustrato in precedenza.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

Il `ProcessMessagesAsync` metodo controlla se è in attesa di un messaggio. Se è presente, viene deserializzato il messaggio in un `FixItTask` entità e Salva l'entità nel database. Esegue un ciclo fino a quando la coda è vuota.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

Il polling per i messaggi in coda comporta una transazione di piccole dimensioni addebito, pertanto quando sono presenti messaggi in attesa di essere elaborato, il ruolo di lavoro `RunAsync` metodo attende un secondo prima di polling nuovamente chiamando `Task.Delay(1000)`.

In un progetto web, aggiunta di codice asincrono automaticamente migliorare le prestazioni poiché IIS gestisce un pool di thread limitato. Non è che nel caso di un progetto di ruolo di lavoro. Per migliorare la scalabilità del ruolo di lavoro, è possibile scrivere il codice a thread multipli o utilizzare codice asincrono per implementare [programmazione parallela](https://msdn.microsoft.com/library/ff963553.aspx). L'esempio viene illustrato come eseguire il codice asincrono, è possibile implementare la programmazione parallela ma che non implementa la programmazione parallela.

## <a name="summary"></a>Riepilogo

In questo capitolo è stato spiegato come migliorare la scalabilità, affidabilità e velocità di risposta dell'applicazione dall'implementazione del pattern di lavoro basate su coda.

Questa è l'ultima dei modelli di 13 trattati in questa e-book, ma esistono naturalmente molti altri modelli e procedure consigliate che consentono di compilare applicazioni basate su cloud ha esito positivo. Il [capitolo finale](more-patterns-and-guidance.md) vengono forniti collegamenti alle risorse per gli argomenti non sono stati inclusi in questi 13 modelli.

<a id="resources"></a>
## <a name="resources"></a>Risorse

Per ulteriori informazioni sulle code, vedere le risorse seguenti.

Documentazione:

- [Microsoft Azure Storage code parte 1: Introduzione](http://justazure.com/microsoft-azure-storage-queues-part-1-getting-started/). Articolo di Schacherl romano.
- [L'esecuzione di attività in Background](https://msdn.microsoft.com/library/ff803365.aspx), capitolo 5 di [spostamento di applicazioni Cloud, 3rd Edition](https://msdn.microsoft.com/library/ff728592.aspx) da Microsoft Patterns and Practices. (In particolare, la sezione ["Utilizzo delle code di archiviazione Azure"](https://msdn.microsoft.com/library/ff803365.aspx#sec7).)
- [Procedure consigliate per ottimizzare la scalabilità e la convenienza delle soluzioni di messaggistica basata su coda in Azure](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx). White paper per Valery Mizonov.
- [Confronto tra le code di Azure e code del Bus di servizio](https://msdn.microsoft.com/magazine/jj159884.aspx). Articolo di MSDN Magazine, fornisce informazioni aggiuntive che consentono di scegliere quale servizio di Accodamento da utilizzare. L'articolo viene indicato che il Bus di servizio sia dipendente da ACS per l'autenticazione, che indica che le code di Service bus sarebbe disponibile quando ACS non è disponibile. Tuttavia, poiché l'articolo è stato scritto, Service bus è stato modificato per consentono di utilizzare [token SAS](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx) un'alternativa ad ACS.
- [Microsoft Patterns and Practices - informazioni aggiuntive su Azure](https://msdn.microsoft.com/library/dn568099.aspx). Introduzione alla messaggistica asincrona, modello di pipe e filtri, il modello di compensazione delle transazioni, modello consumer concorrenti, modello CQRS, vedere.
- [CQRS viaggio](https://msdn.microsoft.com/library/jj554200). E-book su CQRS Microsoft Patterns and Practices.

Video:

- [Operatore alternativo: Compilazione di servizi Cloud resilienti e scalabili](https://channel9.msdn.com/Series/FailSafe). Serie video in nove parti Ulrich Homann, Marc Mercuri e Mark Simms. Presenta i concetti generali e i principi architetturali in modo molto accessibile e interessano con storie ricavate dall'esperienza di Microsoft Team (CAT, Customer Advisory) con i clienti effettivi. Per un'introduzione al servizio di archiviazione di Azure e le code, vedere episodio 5 partendo da 35:13.

>[!div class="step-by-step"]
[Precedente](distributed-caching.md)
[Successivo](more-patterns-and-guidance.md)
