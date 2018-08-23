---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: Progettazione per sopravvivere a errori (creazione di App Cloud funzionanti con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione Real World di App Cloud con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure consigliate che egli può...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: 1098892c621b8add624b18788076be2fac1cc158
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832358"
---
<a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>Progettare per sopravvivere a errori (creazione di App Cloud funzionanti con Azure)
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download risolverlo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **creazione Real World di App Cloud con Azure** eBook si basa su una presentazione sviluppata da Scott Guthrie. Viene illustrato 13 modelli e procedure consigliate che consentono di avere esito positivo lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [capitolo prima](introduction.md).


Una delle operazioni è necessario preoccuparsi quando si compila qualsiasi tipo di applicazione, ma specialmente uno che verrà eseguito nel cloud in cui un numero elevato di persone useranno, viene illustrato come progettare l'app in modo che possa gestire gli errori e continuare a offrire valore normalmente quanto più possibili. Dato un tempo sufficiente, procede per andare storto in qualsiasi ambiente o da qualsiasi sistema software. Come l'app gestisce tali situazioni determina come contrariati ai clienti verranno visualizzato e la quantità di tempo è necessario dedicare l'analisi e correzione dei problemi.

## <a name="types-of-failures"></a>Tipi di errori

Esistono due categorie principali di errori che si voglia gestiscono in modo diverso:

- Temporaneo, correzione automatica degli errori, ad esempio problemi di connettività di rete intermittente.
- Tollerare gli errori che richiedono un intervento.

Per gli errori temporanei, è possibile implementare un criterio di ripetizione dei tentativi per assicurarsi che la maggior parte dei casi il ripristino dell'app rapidamente e automaticamente. I clienti potrebbero notare leggermente più lunghi tempi di risposta, ma in caso contrario non saranno interessate. Ecco alcuni modi per gestire questi errori nel [gestione degli errori temporanei capitolo](transient-fault-handling.md).

Per tollerare gli errori, è possibile implementare il monitoraggio e la funzionalità che avvisa tempestivamente quando si verificano problemi e che semplifica l'analisi della causa radice di registrazione. Ecco alcuni modi per semplificare la connessione nella parte superiore di questi tipi di errori nel [capitolo di monitoraggio e telemetria](monitoring-and-telemetry.md).

## <a name="failure-scope"></a>Ambito di un errore

È necessario anche considerare l'ambito di un errore: se è interessato un solo computer, un intero servizio, ad esempio archiviazione o Database SQL o un'intera area.

![Ambito di un errore](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>Errori di computer

In Azure, un server in errore verrà automaticamente sostituito con un nuovo e un'app cloud ben progettata consente di recuperare da questo tipo di errore in modo rapido e automaticamente. In precedenza è sottolineato i vantaggi di scalabilità di un livello web senza stato e facilità di ripristino da un server non riuscito è un altro vantaggio del concetto. Facilità di ripristino è anche uno dei vantaggi delle funzionalità di platform-as-a-service (PaaS), ad esempio Database SQL e App Web di servizio App di Azure. Errori hardware sono rari, ma quando si verificano che questi servizi di gestirle automaticamente. ancora non è necessario scrivere codice per gestire gli errori di computer quando si usa uno di questi servizi.

### <a name="service-failures"></a>Errori del servizio

Le app cloud usano in genere più servizi. Ad esempio, l'app Fix It Usa il servizio di Database SQL, il servizio di archiviazione e viene distribuita l'app web in servizio App di Azure. Ciò che l'app eseguirà se uno dei servizi di cui che si dipende ha esito negativo? Per alcuni errori del servizio descrittivo "Riprova, sarai più tardi" messaggio potrebbe costituire la scelta migliore è possibile eseguire. Ma in molti scenari è possibile eseguire meglio. Ad esempio, quando l'archivio dati back-end è inattivo, è possibile accettare l'input dell'utente, visualizzare "è stata ricevuta la richiesta" e archiviare temporaneamente; l'input da qualche parte else quindi, quando il servizio è necessario sia operativo anche in questo caso, è possibile recuperare l'input ed elaborarlo.

Il [modello di lavoro incentrato sulle code](queue-centric-work-pattern.md) capitolo viene illustrato un modo per gestire questo scenario. L'app Fix It archivia le attività nel Database SQL, ma non è necessario uscire da quando il Database SQL è verso il basso. In questo capitolo vedremo come archiviare l'input dell'utente per un'attività in una coda e usare un processo di lavoro per la coda di lettura e aggiornare l'attività. Se SQL è attivo, la possibilità di creare attività Fix It è rimangono invariata. il processo di lavoro può attendere ed elaborare le nuove attività quando il Database SQL è disponibile.

### <a name="region-failures"></a>Errori di area

Intere aree potrebbero non riuscire. Una calamità naturale potrebbe eliminare definitivamente un data center, si potrebbe ottenere bidimensionale da un meteor, la riga trunk nel Data Center è stato possibile tagliare un imprenditore incenerimento un cow con un retroescavatore e così via. Se l'app è ospitata nel Data Center stricken cosa fare? È possibile configurare l'app in Azure per eseguire contemporaneamente in più aree in modo che un eventuale in uno è presente una situazione di emergenza, in esecuzione in un'altra area. Questi errori sono molto rari e la maggior parte delle App non passa attraverso i necessario per assicurare la continuità del servizio tramite gli errori di questo tipo di problemi. Vedere la sezione risorse alla fine del capitolo per informazioni su come mantenere l'applicazione disponibile anche tramite un errore di area.

È un obiettivo di Azure per semplificare la gestione di tutti questi tipi di errori molto più semplici e sono riportati alcuni esempi di opinione che nei capitoli seguenti.

## <a name="slas"></a>Contratti di servizio

Le persone spesso percepiscono sui contratti di servizio (SLA) nell'ambiente cloud. Questi sono fondamentalmente promesse che aziende sull'affidabilità di servizio. Un 99,9% contratto di servizio indica che è necessario prevedere che il servizio del corretto funzionamento del 99,9% del tempo. Questo è un valore tipico per un contratto di servizio e ha l'aspetto un numero molto elevato, ma potrebbe non essere consapevoli quanti tempi di inattività. % 1 equivale effettivamente a. Ecco una tabella che mostra i tempi di inattività per l'importo diverse percentuali di contratto di servizio su un anno, mese e una settimana.

![Tabella di contratto di servizio](design-to-survive-failures/_static/image2.png)

Un contratto di servizio indica che il servizio del 99,9% pertanto potrebbero non essere attivi 8,76 ore all'anno o 43,2 minuti un mese. Che è più il tempo di inattività rispetto a realizzare la maggior parte delle persone. Gli sviluppatori opportuno tenere presente che è possibile una certa quantità di tempo di inattività e gestirla in modo normale. A un certo punto qualcuno si prevede di usare l'app, un servizio dovrà essere verso il basso e si vuole ridurre al minimo l'impatto negativo di tale funzione del cliente.

Una cosa da sapere su un contratto di servizio è quale tempistica fa riferimento a: l'orologio venga azzerato ogni settimana, ogni mese o ogni anno? In Azure è possibile reimpostare l'orologio di ogni mese, è preferibile per l'utente rispetto a un contratto di servizio annua, poiché un contratto di servizio annua è stato possibile nascondere mesi errate mediante compensazione con una serie di mesi buona.

Naturalmente è sempre mirare a migliorare il contratto di servizio; in genere sarà molto inferiore rispetto a quello verso il basso. La promessa è che se si è mai verso il basso per un tempo superiore al massimo il tempo di inattività è possibile richiedere back denaro. La quantità di denaro che viene nuovamente visualizzato probabilmente non sarebbe compensa completamente di impatto aziendale di quest ' ultimo tempo di inattività, ma l'aspetto del contratto di servizio funge da criterio di imposizione e informa l'utente che abbiamo portarlo molto seriamente.

### <a name="composite-slas"></a>Contratti di servizio compositi

Un aspetto importante da considerare quando si osservano i contratti di servizio è l'impatto dell'uso di più servizi in un'app, con ogni servizio con un contratto di servizio separato. Ad esempio, l'app Fix It Usa App Web di servizio App di Azure, archiviazione di Azure e Database SQL. Ecco i relativi numeri di contratto di servizio alla data che viene scritto questo e-book nel dicembre 2013:

![Database SQL del sito Web, archiviazione, contratto di servizio](design-to-survive-failures/_static/image3.png)

Che cos'è il numero massimo di inattività che previsto per l'app in base a questi contratti di servizio? Si potrebbe pensare che i tempi di inattività potrebbero essere uguali alla percentuale di contratto di servizio peggiore o pari al 99,9% in questo caso. Che sarà true se tutte e tre i servizi sempre eseguito allo stesso tempo, ma che non è necessariamente che cosa succede effettivamente. Ogni servizio può non riuscire in modo indipendente in momenti diversi, pertanto è necessario calcolare il contratto di servizio composito moltiplicando i numeri di contratto di servizio singoli.

![Contratto di servizio composito](design-to-survive-failures/_static/image4.png)

In modo che l'app è stato possibile targettizzati non solo 43,2 minuti al mese ma 3 volte tale quantità, 108 minuti mese – e rimanere comunque all'interno dei limiti del contratto di servizio di Azure.

Questo problema non è univoco in Azure. Viene effettivamente fornito il miglior cloud contratti di servizio di qualsiasi servizio cloud disponibile e sarà necessario problemi analoghi considerare se si usano servizi cloud di qualsiasi fornitore. Per questo motivo è l'importanza di pensare a come è possibile progettare l'app per gestire gli errori di servizio inevitabile correttamente, poiché potrebbero verificarsi abbastanza spesso un impatto sui clienti o gli utenti.

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>Contratti di servizio cloud rispetto all'esperienza di tempi di inattività enterprise

In alcuni casi persone sostengono, "Nelle app aziendali non ho bisogno sono questi problemi." Se si chiede di quanti tempi di inattività mese, in realtà hanno, in genere si dice, "Beh, si verifica occasionalmente." E se si richiede la frequenza, essi ammettere che "Talvolta è necessario eseguire il backup o installare un nuovo server o aggiornamento software". Naturalmente, che conta come tempo di inattività. La maggior parte delle App aziendali a meno che non sono particolarmente cruciali sono effettivamente verso il basso oltre la quantità di tempo consentito da contratti di servizio. Ma quando è il server e l'infrastruttura e ha la responsabilità per tale e nel controllo, sentano meno angst tempi di inattività. In un ambiente cloud avete dipende da un altro utente e non si conosce ciò che sta accadendo, pertanto è possibile che tendono a ottenere più preoccupati.

Quando un'organizzazione consente di ottenere una percentuale di tempo maggiore rispetto a ottenuti da un contratto di servizio cloud, lo scopo, spesa denaro molto di più su hardware. Un servizio cloud è stato possibile eseguire questa operazione, ma avrebbe ad addebitare i costi per i propri servizi molto altro ancora. Invece sfruttare un servizio conveniente e progettazione del software in modo che gli inevitabili errori causano un'interruzione minima per i clienti. Il processo come una finestra di progettazione di app cloud non è così tanto per evitare errori per evitare inoltre catastrofe, e a tale scopo, concentrandosi su software, non su hardware. Mentre le app enterprise si impegnano per ottimizzare il tempo medio tra gli errori, cercano le app cloud per ridurre al minimo il tempo medio per il ripristino.

### <a name="not-all-cloud-services-have-slas"></a>Non tutti i servizi cloud hanno contratti di servizio

Tenere presente inoltre che non tutti i servizi cloud ha anche un contratto di servizio. Se l'app è dipendente da un servizio con alcuna garanzia di tempo, potrebbe essere inattivo molto più lungo di quanto si possa immaginare. Ad esempio, se si abilita l'accesso al sito usando un provider di social networking come Facebook o Twitter, verificare con il provider di servizi per scoprire se è presente un contratto di servizio, e si noterà che non corrisponde a uno. Ma se il servizio di autenticazione non risponde o è in grado di supportare il volume di richieste che è generare analizzarlo, i clienti vengono bloccati all'app. È possibile verso il basso per giorni o più. Gli autori di una nuova app previsto centinaia di milioni di download e ha acquisito una dipendenza dall'autenticazione di Facebook, ma non è stata illustrata a Facebook prima di passare in tempo reale e individuati troppo tardi che si è verificato alcun contratto di servizio per il servizio.

### <a name="not-all-downtime-counts-toward-slas"></a>Non tutti i tempi di inattività rientrano i contratti di servizio

Alcuni servizi cloud possono deliberatamente negazione del servizio se l'app li Usa risorse in eccesso. Questa operazione viene definita *limitazione*. Se un servizio ha un contratto di servizio, dovrebbe indicare le condizioni in base alle quali potrebbe essere limitato e la progettazione di app deve evitare tali condizioni e rispondere in modo appropriato per la limitazione delle richieste se si verifica. Ad esempio, se le richieste a un servizio iniziano ad avere esito negativo quando si supera un determinato numero al secondo, si desidera assicurarsi che i tentativi automatici non si verifichino così rapida che provocano la limitazione continuare. Avremo più da dire sulla limitazione delle richieste nel [gestione degli errori temporanei capitolo](transient-fault-handling.md).

## <a name="summary"></a>Riepilogo

In questo capitolo ha cercato di permettere di sfruttare il motivo per cui è necessario progettare per sopravvivere a errori correttamente un'app per cloud reali. Inizia con la [capitolo successivo](monitoring-and-telemetry.md), i criteri rimanenti in questa serie di passare più dettagliate su alcune strategie che è possibile usare per eseguire questa operazione:

- Sono buoni [monitoraggio e telemetria](monitoring-and-telemetry.md), in modo che scoprire rapidamente gli errori che richiedono un intervento, e si avranno informazioni sufficienti per risolverli.
- [Gestire gli errori temporanei](transient-fault-handling.md) implementando la logica di ripetizione dei tentativi intelligente, in modo che l'app viene ripristinato automaticamente quando possibile e il fallback alla [interruttore](transient-fault-handling.md#circuitbreakers) per la logica quando non è possibile.
- Uso [memorizzazione nella cache distribuita](distributed-caching.md) per ridurre al minimo i problemi di velocità effettiva, latenza e la connessione con accesso al database.
- Implementare loose accoppiamento tramite il [modello di lavoro incentrato sulle code](queue-centric-work-pattern.md), in modo che il front-end dell'app è possibile continuare a lavorare quando il back-end è inattivo.

## <a name="resources"></a>Risorse

Per altre informazioni, vedere i capitoli più avanti in questo e-book e le risorse seguenti.

Documentazione:

- [FailSafe: Informazioni aggiuntive per architetture Cloud resilienti](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). White paper Marc Mercuri, Ulrich Homann e Andrew Townhill. Versione della pagina Web della serie di video di operatore alternativo.
- [Procedure consigliate per la progettazione di servizi su larga scala nei servizi Cloud di Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). White paper Mark Simms e Michael Thomassy.
- [Informazioni tecniche sulla continuità aziendale di Azure](https://msdn.microsoft.com/library/windowsazure/hh873027.aspx). White paper Patrick Wickline, Jason Roth.
- [Ripristino di emergenza e disponibilità elevata per le applicazioni Azure](https://msdn.microsoft.com/library/windowsazure/dn251004.aspx). White paper da Michael McKeown Hanu Kommalapati e Jason Roth.
- [Microsoft Patterns and Practices - informazioni aggiuntive su Azure](https://msdn.microsoft.com/library/dn568099.aspx). Vedere le indicazioni di distribuzione in più Data Center, schema Circuit breaker.
- [Supporto di Azure - contratti di servizio](https://azure.microsoft.com/support/legal/sla/).
- [Continuità aziendale nel Database SQL di Azure](https://msdn.microsoft.com/library/windowsazure/hh852669.aspx). Documentazione sulle funzionalità Ripristino emergenza e disponibilità elevata del Database SQL.
- [Disponibilità elevata e ripristino di emergenza per SQL Server in macchine virtuali di Azure](https://msdn.microsoft.com/library/windowsazure/jj870962.aspx).

Video

- [Operatore alternativo: Creazione di servizi Cloud scalabili, resilienti](https://channel9.msdn.com/Series/FailSafe). Serie in nove parti Marc Mercuri, Ulrich Homann e Mark Simms. Presenta i concetti generali e principi architetturali in modo estremamente accessibile e interessano, con storie ricavate dall'esperienza di Microsoft Customer Advisory Team (CAT) con i clienti effettivi. Episodi 1 e 8 analizzare in dettaglio i motivi per la progettazione di App cloud per sopravvivere a errori. Vedere anche la discussione della limitazione delle richieste nell'episodio 2 partire 49:57, la discussione sulle modalità di errore nell'episodio 2 partire 56:05 e i punti di errore e la discussione degli interruttori nell'episodio 3 partire 40:55 follow-up.
- [Compilazione Big: Lezioni apprese dai clienti di Azure - parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms illustra la progettazione di errore e strumentazione di tutti gli elementi. Simile alla serie di tipo operatore alternativo, ma passa ad analizzare altri dettagli sulle procedure.

> [!div class="step-by-step"]
> [Precedente](unstructured-blob-storage.md)
> [Successivo](monitoring-and-telemetry.md)
