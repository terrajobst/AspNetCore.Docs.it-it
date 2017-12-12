---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: Progettazione di sopravvivenza errori (creazione di applicazioni con Azure Cloud del mondo reale) | Documenti Microsoft
author: MikeWasson
description: "Le App per Cloud mondo reale compilazione con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure che è possibile..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: a0ee790da07c99cdb1279a6bca637a4ce8076e84
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>Progettazione di sopravvivenza errori (creazione di applicazioni Cloud reale in Azure)
====================
da [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download correggerlo progetto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [E-book di Download](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **predefiniti reale World Cloud App con Azure** e-book è basato su una presentazione sviluppata da Scott Guthrie. Vengono descritte le 13 modelli e procedure consigliate che consentono di avere esito negativo con lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [primo capitolo](introduction.md).


Una delle operazioni è necessario considerare quando si compila qualunque tipo di applicazione, ma soprattutto che verrà eseguito nel cloud in un numero elevato di utenti verrà utilizzata, viene illustrato come progettare l'app in modo che può gestire gli errori e continuare a offrire valore normalmente quanto possibili. Dato un tempo sufficiente, operazioni intende in caso di errori in qualsiasi ambiente o in qualsiasi sistema software. Come l'applicazione gestisce situazioni determina come quell ai clienti verranno visualizzato e il tempo è necessario investire l'analisi e risoluzione dei problemi.

## <a name="types-of-failures"></a>Tipi di errori

Esistono due categorie principali di errori che si desideri gestire in modo diverso:

- Temporanei, errori, ad esempio problemi di connettività di rete intermittenti di riparazione automatica.
- Tollerare errori che richiedono un intervento.

Per errori temporanei, è possibile implementare un criterio di ripetizione per assicurarsi che la maggior parte dei casi l'app recupera in modo rapido e automatico. I clienti potrebbero notare leggermente più tempo di risposta, ma in caso contrario non saranno interessate. Ecco alcuni modi per gestire questi errori nel [gestione degli errori temporanei capitolo](transient-fault-handling.md).

Per tollerare errori, è possibile implementare il monitoraggio e la funzionalità che avvisa tempestivamente quando si verificano problemi e che semplifica l'analisi delle cause principali di registrazione. Vi mostreremo alcuni metodi che consentono di controllare questi tipi di errori di [capitolo di monitoraggio e telemetria](monitoring-and-telemetry.md).

## <a name="failure-scope"></a>Ambito di un errore

È inoltre necessario considerare l'ambito di un errore: se è interessato un singolo computer, un intero servizio, ad esempio Database SQL o archiviazione o un'intera area.

![Ambito di un errore](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>Errori di computer

In Azure, un server non riuscito verrà automaticamente sostituito con un nuovo e il recupera di un'applicazione ben progettata cloud da questo tipo di errore in modo rapido e automaticamente. In precedenza è sottolineato i vantaggi di scalabilità di un livello web senza stato e la facilità di ripristino da un server è un altro vantaggio della senza stato. Facilità di recupero è uno dei vantaggi delle funzionalità di piattaforma come servizio (PaaS), ad esempio Database SQL e App Web di servizio App di Azure. Errori hardware sono rari, ma quando si verificano che questi servizi di gestirle automaticamente. non è anche necessario scrivere codice per gestire gli errori di computer quando si utilizza uno di questi servizi.

### <a name="service-failures"></a>Errori del servizio

Applicazioni basate su cloud usano in genere più servizi. Ad esempio, l'app Correggi utilizza il servizio di Database SQL, il servizio di archiviazione e l'app web viene distribuito in Azure App Service. Quali app eseguirà se uno dei servizi che si dipende ha esito negativo? Per alcuni errori del servizio un nome "Impossibile, riprovare più tardi" messaggio potrebbe essere la migliore, è possibile eseguire. Ma in molti scenari è possibile eseguire meglio. Ad esempio, quando l'archivio dati back-end è attivo, è possibile accettare l'input dell'utente, visualizzare "è stata ricevuta la richiesta" e archiviare l'input di qualche punto else temporaneamente. quindi, quando il servizio è necessario non sarà operativo, è possibile recuperare l'input ed elaborarlo.

Il [basate su coda lavoro modello](queue-centric-work-pattern.md) capitolo viene illustrato un modo per gestire questo scenario. L'app Correggi consente di archiviare le attività nel Database SQL, ma non è necessario chiudere quando il Database SQL non è attivo. In questo capitolo vedremo come archiviare l'input dell'utente per un'attività in una coda e utilizzare un processo di lavoro di lettura coda e aggiornare l'attività. Se SQL è attivo, la possibilità di creare attività di correzione non viene influenzata; il processo di lavoro è possibile attendere ed elaborare nuove attività, quando sono disponibili Database SQL.

### <a name="region-failures"></a>Errori di area

Intere aree potrebbero non riuscire. Una calamità naturale potrebbe eliminare definitivamente un data center, si potrebbe ottenere bidimensionale da un meteor, la riga trunk nel centro dati può essere isolata da un farmer incenerimento un cow con una retroescavatore e così via. Se l'app è ospitato nel Data Center stricken operazioni? È possibile configurare l'app in Azure per eseguire contemporaneamente in più aree in modo che se è una situazione di emergenza in uno, continuare l'esecuzione in un'altra area. Tali errori sono molto rare occorrenze e la maggior parte delle applicazioni non passare attraverso i cerchi necessarie per garantire la continuità del servizio tramite gli errori di questo tipo. Vedere la sezione risorse alla fine del capitolo per informazioni su come mantenere l'app disponibile anche in caso di un errore di area.

È un obiettivo di Azure per rendere la gestione di tutti i tipi di errori molto più semplici e si noterà che alcuni esempi di come facciamo che nei capitoli seguenti.

## <a name="slas"></a>Contratti di servizio

Le persone percepiscono spesso sui contratti di servizio (SLA) nell'ambiente cloud. In pratica, questi sono promesse società sulla come affidabile il servizio. Un 99,9% SLA significa che è necessario prevedere che il servizio del corretto funzionamento del 99,9% del tempo. Che è un valore tipico per un contratto di servizio e può sembrare un numero molto elevato, ma potrebbero non essere consapevoli quanto tempo verso il basso. % 1 effettivamente gli importi di. Di seguito è riportata una tabella che mostra la quantità dei tempi di inattività diverse percentuali di contratto di servizio equivalgono a un anno, mese e una settimana.

![Tabella di contratto di servizio](design-to-survive-failures/_static/image2.png)

Pertanto, un contratto di servizio indica che il servizio di 99,9% potrebbero non essere attivi 8,76 ore 43,2 minuti un mese o un anno. Che è più il tempo di inattività di realizzare la maggior parte delle persone. In modo da uno sviluppatore desidera tenere presente che è possibile una certa quantità di tempo di inattività e gestirlo in modo normale. A un certo punto un utente sta per essere usando l'app, un servizio sta per essere verso il basso e si desidera ridurre al minimo l'impatto negativo di tale sul cliente.

Una cosa è necessario conoscere un contratto di servizio è il periodo di tempo fa riferimento a: l'ottenere reimpostata ogni settimana, mese o ogni anno? In Azure è possibile reimpostare l'orologio di ogni mese, che è migliore per l'utente di un contratto di servizio annua, poiché un contratto di servizio annua possibile nascondere mesi errati mediante compensazione con una serie di mesi validi.

Naturalmente è sempre mirare a tale migliori di quelle del contratto di servizio; in genere sarà molto meno rispetto a quello verso il basso. La promessa è che se si è mai verso il basso per un tempo superiore al massimo il tempo di inattività è possibile richiedere back money. La quantità di denaro che viene nuovamente visualizzato probabilmente non sarebbe compensa completamente l'impatto di quest ' ultimo tempo di inattività, ma l'aspetto del contratto di servizio funge da un criterio di applicazione e consente di sapere che si accettano molto seri.

### <a name="composite-slas"></a>Contratti di servizio compositi

Un aspetto importante da considerare quando si osservano i contratti di servizio è l'impatto dell'utilizzo di più servizi in un'applicazione, con ogni servizio che dispone di un contratto di servizio separato. Ad esempio, l'app Correggi utilizza App Web di servizio App di Azure, archiviazione di Azure e Database SQL. Ecco i numeri di contratto di servizio alla data di che questo manuale e viene scritto nel dicembre 2013:

![Database SQL di sito Web, archiviazione, contratto di servizio](design-to-survive-failures/_static/image3.png)

Che cos'è il numero massimo di tempo che previsto per l'app in base a questi contratti di servizio? Si potrebbe pensare che i tempi di inattività deve essere uguali alla percentuale di SLA peggiore o 99,9% in questo caso. Che sarà true se tutti e tre i servizi non è stato possibile sempre allo stesso tempo, ma che non necessariamente che si verifica effettivamente. Ogni servizio potrebbe non riuscire in modo indipendente in momenti diversi, è necessario calcolare il contratto di servizio composito moltiplicando i numeri di contratto di servizio singoli.

![Contratto di servizio composito](design-to-survive-failures/_static/image4.png)

Allo stesso modo l'app potrebbe verso il basso non solo 43,2 minuti 3 volte la quantità, ma un mese 108 minuti un mese e comunque essere entro i limiti del contratto di servizio di Azure.

Questo problema non è univoco in Azure. Si forniscono cloud migliore SLA di qualsiasi servizio cloud disponibile e sarà necessario simili problemi se si utilizzano servizi cloud di un fornitore. Per questo motivo è che è importante considerare come è possibile progettare l'app per gestire gli errori del servizio inevitabile risolvibili, poiché può verificarsi con una frequenza tale da influire sui clienti e utenti.

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>Contratti di servizio cloud rispetto alle esperienze tempi di inattività

Persone talvolta ad esempio, "Nell'applicazione enterprise non è mai possibile queste." Se si richiede la quantità tempi di inattività mese effettivamente hanno, in genere si dice, ", si verifica occasionalmente." E se si richiede la frequenza, ammettono che "Talvolta è necessario eseguire il backup o installare un nuovo server o aggiornamento software". Naturalmente, che conta come tempo di inattività. La maggior parte delle applicazioni aziendali a meno che non sono particolarmente critici sono effettivamente verso il basso per più di una quantità di tempo consentito per i contratti di servizio. Ma quando è il server e l'infrastruttura e si è responsabili relativo e il controllo, si tende a ritiene meno angst tempi di inattività. In un ambiente cloud si è dipendente da un altro utente e non si conosce cosa sta accadendo, pertanto è possibile che tendono a ottenere più preoccupazione.

Quando un'organizzazione consente di ottenere una percentuale di tempo maggiore di quella che si ottiene da un contratto di servizio cloud, non da spesa molto su hardware. Un servizio cloud è possibile farlo ma avrebbe ad addebitare i costi molto altro ancora per i servizi. Invece sfruttare i vantaggi di un servizio economico e progettare il software in modo che gli errori inevitabili causano interruzioni minime ai clienti. Il compito di una finestra di progettazione di app cloud non è così per evitare l'errore per evitare inoltre della situazione di emergenza e a tale scopo, porre l'attenzione sul software, non su hardware. Considerando che le app aziendali cercano di ottimizzare il tempo medio tra gli errori, le app cloud cercano di ridurre al minimo il tempo medio per il recupero.

### <a name="not-all-cloud-services-have-slas"></a>Non tutti i servizi cloud sono contratti di servizio

Tenere presente inoltre che non tutti i servizi cloud ha anche un contratto di servizio. Se l'app è dipendente da un servizio con alcuna garanzia di tempo, potrebbero non essere attivi molto più lungo di quando si possa immaginare. Ad esempio, se si abilita l'accesso al sito utilizzando un provider di social networking, ad esempio Facebook o Twitter, verificare con il provider del servizio per scoprire se è presente un contratto di servizio e si noterà che non è disponibile. Tuttavia, se il servizio di autenticazione si arresta o non è in grado di supportare il volume di richieste che è generare questo, i clienti vengono bloccati l'app. Potrebbero non essere attivi per giorni o più. Gli autori di una nuova app previsto centinaia di milioni di download e ha acquisito una dipendenza sull'autenticazione di Facebook, ma non parlare con Facebook prima di passare in tempo reale e individuati troppo tardi che si è verificato alcun contratto di servizio per il servizio.

### <a name="not-all-downtime-counts-toward-slas"></a>Non tutti i conteggi di tempi di inattività per i contratti di servizio

Alcuni servizi cloud deliberatamente potrebbero negare i servizi se eccessiva Usa l'app. Si tratta di *limitazione*. Se un servizio dispone di un contratto di servizio, devono indicare le condizioni in cui potrebbe essere limitate, e la progettazione di app deve evitare tali condizioni e rispondere nel modo appropriato per la limitazione delle richieste se si verifica. Ad esempio, se le richieste a un servizio se si supera un determinato numero al secondo, si desidera assicurarsi che i tentativi automatici non verificarsi veloce che provocano la limitazione delle richieste continuare. Sono più sulla limitazione delle richieste nel [gestione degli errori temporanei capitolo](transient-fault-handling.md).

## <a name="summary"></a>Riepilogo

In questo capitolo ha tentato di comprendere perché un'applicazione cloud reale è necessario progettare sopravvivenza normalmente gli errori. A partire dal [capitolo successivo](monitoring-and-telemetry.md), i modelli rimanenti di questa serie passare informazioni più dettagliate su alcune strategie che è possibile utilizzare a tale scopo:

- Avere una buona [monitoraggio e telemetria](monitoring-and-telemetry.md), in modo che scoprire rapidamente gli errori che richiedono un intervento e si dispone di informazioni sufficienti per risolverli.
- [Gestire gli errori temporanei](transient-fault-handling.md) implementando la logica di retry intelligenti, in modo che l'app consente di recuperare automaticamente quando possibile e tenti di [interruttore](transient-fault-handling.md#circuitbreakers) logica quando non è possibile.
- Utilizzare [cache distribuita](distributed-caching.md) per ridurre al minimo i problemi di velocità effettiva, latenza e la connessione con accesso al database.
- Implementare separati accoppiamento tramite il [basate su coda di lavoro modello](queue-centric-work-pattern.md), in modo che il front-end app è possibile continuare a lavorare quando back-end è inattivo.

## <a name="resources"></a>Risorse

Per ulteriori informazioni, vedere capitoli più avanti in questo e-book e le risorse seguenti.

Documentazione:

- [Operatore alternativo: Informazioni aggiuntive per architetture Cloud resilienti](https://msdn.microsoft.com/en-us/library/windowsazure/jj853352.aspx). White paper Marc Mercuri, Ulrich Homann e Andrew Townhill. Versione della pagina Web della serie di video di operatore alternativo.
- [Procedure consigliate per la progettazione di servizi su larga scala nei servizi Cloud Azure](https://msdn.microsoft.com/en-us/library/windowsazure/jj717232.aspx). White paper Mark Simms e Michael Thomassy.
- [Informazioni tecniche sulla continuità aziendale di Azure](https://msdn.microsoft.com/en-us/library/windowsazure/hh873027.aspx). White paper Patrick Wickline, Jason Roth.
- [Ripristino di emergenza e disponibilità elevata per applicazioni Azure](https://msdn.microsoft.com/en-us/library/windowsazure/dn251004.aspx). White paper Michael McKeown Hanu Kommalapati, e Jason Roth.
- [Microsoft Patterns and Practices - informazioni aggiuntive su Azure](https://msdn.microsoft.com/en-us/library/dn568099.aspx). Vedere la Guida di distribuzione con più Data Center, modello di interruttore.
- [Supporto tecnico di Azure - contratti di servizio](https://azure.microsoft.com/support/legal/sla/).
- [Continuità aziendale nel Database SQL di Azure](https://msdn.microsoft.com/en-us/library/windowsazure/hh852669.aspx). Documentazione sulle funzionalità Ripristino emergenza e disponibilità elevata del Database SQL.
- [Disponibilità elevata e ripristino di emergenza per SQL Server in macchine virtuali di Azure](https://msdn.microsoft.com/en-us/library/windowsazure/jj870962.aspx).

Video

- [Operatore alternativo: Compilazione di servizi Cloud resilienti e scalabili](https://channel9.msdn.com/Series/FailSafe). Serie di nove parti Ulrich Homann, Marc Mercuri e Mark Simms. Presenta i concetti generali e i principi architetturali in modo molto accessibile e interessano con storie ricavate dall'esperienza di Microsoft Team (CAT, Customer Advisory) con i clienti effettivi. Episodi 1 e 8 entrano in modo approfondito nei motivi per la progettazione di applicazioni basate su cloud vengano conservate con errori. Vedere anche la discussione della limitazione delle richieste in episodio 2 iniziando 49:57, la discussione sulle modalità di errore in episodio 2 iniziando 56:05 e punti di errore e la descrizione del circuito breaker nell'episodio 3 iniziando 40:55 follow-up.
- [Compilazione Big: Apprese dai clienti di Azure - parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms comunica sulla progettazione per errore e la strumentazione di tutti gli elementi. Simile alla serie di operatore alternativo ma consente di spostarsi in dettaglio procedure.

>[!div class="step-by-step"]
[Precedente](unstructured-blob-storage.md)
[Successivo](monitoring-and-telemetry.md)
