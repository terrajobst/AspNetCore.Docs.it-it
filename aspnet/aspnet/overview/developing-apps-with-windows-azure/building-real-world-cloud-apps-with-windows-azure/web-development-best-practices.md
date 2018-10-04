---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Web consigliate per lo sviluppo (creazione di App Cloud funzionanti con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione Real World di App Cloud con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure consigliate che egli può...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: 930b9be35ef2e0dd85cee8f6584b9e90c80933b9
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577899"
---
<a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Procedure guidate di sviluppo Web (creazione di App Cloud funzionanti con Azure)
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Download risolverlo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **creazione Real World di App Cloud con Azure** eBook si basa su una presentazione sviluppata da Scott Guthrie. Viene illustrato 13 modelli e procedure consigliate che consentono di avere esito positivo lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [capitolo prima](introduction.md).


I primi tre modelli sono stati sulla configurazione di un processo di sviluppo agile. gli altri sono sull'architettura e codice. Questa è una raccolta di procedure consigliate per lo sviluppo web:

- [Server web senza stato](#stateless) dietro un bilanciamento del carico intelligente.
- [Evitare lo stato della sessione](#sessionstate) (o se non strettamente necessario, utilizzare cache distribuita, anziché un database).
- [Usare una rete CDN](#cdn) agli asset di file statici di cache perimetrale (immagini, script).
- [Usare il supporto asincrono di .NET 4.5](#async) per evitare di bloccare le chiamate.

Queste procedure sono valide per tutto lo sviluppo web, non solo per le app cloud, ma sono particolarmente importante per le app cloud. Interagiscono per garantire un uso ottimale della scalabilità altamente flessibile offerto da parte dell'ambiente cloud. Se non si seguono queste procedure consigliate, verranno generati limitazioni quando si prova a ridimensionare l'applicazione.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Livello web senza stato dietro un bilanciamento del carico intelligente

*Livello web senza stato* significa che è non archiviare i dati dell'applicazione nel sistema web server file o della memoria. Mantenere il livello web senza stato consente di offrire un'esperienza cliente migliore sia risparmiare denaro:

- Se il livello web è senza stato e si trova dietro un bilanciamento del carico, è possibile rispondere rapidamente ai cambiamenti del traffico delle applicazioni in modo dinamico aggiungendo o rimuovendo i server. Nell'ambiente cloud in cui si paga solo per le risorse del server, purché è effettivamente utilizzare, la possibilità di rispondere alle modifiche della domanda può tradurre in risparmio enorme.
- Un livello web senza stato è a livello di architettura molto più semplice scalare orizzontalmente l'applicazione. Troppo, che consente di rispondere alle esigenze di ridimensionamento più rapidamente e risparmiare sui sviluppo e test nel processo.
- Server del cloud, come i server in locale, è necessario installare patch e riavviati occasionalmente; e se il livello web è senza stato, il reindirizzamento del traffico quando si arresta temporaneamente un server non provocheranno errori o comportamenti imprevisti.

La maggior parte delle applicazioni reali è necessario archiviare lo stato per una sessione web. Ecco l'aspetto principale consiste nel non archiviarli nel server web. È possibile archiviare lo stato in altri modi, ad esempio nel client nei cookie o all'esterno di processo server-side nello stato della sessione ASP.NET usando un provider di cache. È possibile archiviare i file in [archiviazione Blob di Windows Azure](unstructured-blob-storage.md) anziché nel file system locale.

Un esempio di come è facile ridimensionare un'applicazione in siti Web di Microsoft Azure, se il livello web è senza stato, vedere la **scalabilità** scheda per un sito Microsoft Azure Web nel portale di gestione:

![Scheda scalabilità](web-development-best-practices/_static/image1.png)

Se si desidera aggiungere i server web, è possibile semplicemente trascinare il dispositivo di scorrimento di conteggio di istanza a destra. Impostarla su 5 e fare clic su **salvare**, e in pochi secondi si dispone di 5 server web in Windows Azure la gestione del traffico del sito web.

![Cinque istanze](web-development-best-practices/_static/image2.png)

È possibile impostare semplicemente il numero di istanze fino a 3 o torni a 1. Quando si passa indietro, il risparmio inizia immediatamente perché Windows Azure addebita al minuto, non per l'ora.

È anche possibile invitare Windows Azure per aumentare o ridurre il numero di server web in base all'utilizzo della CPU automaticamente. Nell'esempio seguente, quando l'utilizzo della CPU scende sotto il 60%, il numero di server web verrà ridotti al minimo di 2 e in caso di utilizzo della CPU superiore all'80%, aumenterà il numero di server web con un massimo di 4.

![Ridimensionare mediante l'utilizzo della CPU](web-development-best-practices/_static/image3.png)

Oppure cosa accade se si sa che il sito solo sarà occupato durante l'orario di lavoro? È possibile indicare a Windows Azure per eseguire più server durante le ore del giorno e diminuire a un singolo server validità, lunghe notti e fine settimana. La serie di catture di schermata seguenti viene illustrato come configurare il sito web per eseguire un server in ore non lavorative e 4 i server durante le ore lavorative dalle 8 alle 17.

![Ridimensiona di pianificazione](web-development-best-practices/_static/image4.png)

![Impostare i tempi di pianificazione](web-development-best-practices/_static/image5.png)

![Pianificazione diurno](web-development-best-practices/_static/image6.png)

![Pianificazione weeknight](web-development-best-practices/_static/image7.png)

![Pianificazione del fine settimana](web-development-best-practices/_static/image8.png)

E naturalmente tutto questo può essere eseguita negli script anche come nel portale.

La capacità dell'applicazione per la scalabilità orizzontale è quasi illimitata come in Windows Azure, purché si evitare le difficoltà a in modo dinamico aggiungendo o rimuovendo le macchine virtuali server, mantenendo il livello web senza stato.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Evitare lo stato della sessione

È spesso poco pratico in un'app cloud reale per evitare di archiviare qualche forma di stato per una sessione utente, ma alcuni approcci influire sulle prestazioni e scalabilità rispetto ad altri. Se è necessario archiviare lo stato, la soluzione migliore è mantenere piccola la quantità di stato e archiviarla nei cookie. Se non è fattibile, la seconda miglior soluzione consiste nell'utilizzare lo stato della sessione ASP.NET con un provider per [cache in memoria distribuita](distributed-caching.md#sessionstate). La soluzione peggiore da un punto di vista della scalabilità e prestazioni è utilizzare un database eseguito il provider di stato della sessione.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Usare una rete CDN per memorizzare nella cache gli asset di file statici

Rete CDN è l'acronimo di rete CDN. Vengono fornite risorse file statiche quali immagini e i file di script a un provider della rete CDN e il provider memorizza nella cache di questi file nei data center in tutto il mondo, in modo che ogni volta che gli utenti accedono all'applicazione, ricevono risposta relativamente rapido e a bassa latenza per memorizzato nella cache Asset. Ciò consente di velocizzare il tempo di caricamento complessiva del sito e riduce il carico sui server web. Le reti CDN sono importanti specialmente se sono destinati a un gruppo di destinatari ampiamente distribuita geograficamente.

Windows Azure ha una rete CDN, è possibile usare altre reti CDN in un'applicazione che viene eseguito in Windows Azure o in qualsiasi ambiente di hosting web.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>Usare il supporto asincrono di .NET 4.5 per evitare di bloccare le chiamate

.NET 4.5 migliorati i linguaggi di programmazione c# e VB per poter rendere molto più semplice gestire le attività in modo asincrono. Il vantaggio della programmazione asincrona non è disponibile solo per le situazioni di elaborazione parallela, ad esempio quando si desidera avviare contemporaneamente più chiamate al servizio web. Consente inoltre al server web di eseguire in modo più efficiente e affidabile in condizioni di carico elevato. Un server web ha solo un numero limitato di thread disponibili e condizioni di carico elevato quando tutti i thread sono in uso, le richieste in ingresso sono necessario attendere che i thread vengono liberati. Se il codice dell'applicazione non gestisce attività, ad esempio le query e chiamate al servizio web di database in modo asincrono, numero di thread venga inutilmente vincolati mentre il server è in attesa di una risposta dei / o. Questo limita la quantità di traffico che in condizioni di carico elevato grado di gestire il server. Con la programmazione asincrona, i thread in attesa di un servizio web o un database per restituire i dati vengono liberate fino a soddisfare nuove richieste finché i dati di ricezione. In un server web occupato, centinaia o migliaia di richieste può quindi essere elaborata immediatamente che verrebbe in attesa del thread da liberare.

Come illustrato in precedenza, è sufficiente per ridurre il numero di server web che gestiscono il sito web come succede per aumentarle. Pertanto, se un server può ottenere maggiore velocità effettiva, non è necessario perché molti di essi e non è possibile ridurre i costi perché è necessario meno server per un volume di traffico dato quanto accadrebbe in caso contrario.

Supporto per il modello di programmazione asincrono .NET 4.5 è incluso in ASP.NET 4.5 per Web Form, MVC e API Web. in Entity Framework 6 e nel [API di archiviazione di Azure Windows](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>Supporto asincrono in ASP.NET 4.5

In ASP.NET 4.5, il supporto per la programmazione asincrona è stato aggiunto non solo per la lingua, ma anche per i framework MVC, Web Form e Web API. Ad esempio, un metodo di azione del controller ASP.NET MVC riceve dati da una richiesta web e passa i dati a una visualizzazione che crea quindi il codice HTML da inviare al browser. Il metodo di azione deve spesso ottenere dati da un database o un servizio web per visualizzarla in una pagina web o per salvare i dati immessi in una pagina web. In questi scenari è facile rendere asincrono il metodo di azione: invece di restituire un *ActionResult* dell'oggetto, restituito *Task&lt;ActionResult&gt;*  e contrassegnare il metodo con il *async* (parola chiave). All'interno del metodo, quando una riga di codice avvia un'operazione che comporta il tempo di attesa, viene contrassegnato con la parola chiave await.

Ecco un metodo di azione semplice che chiama un metodo di repository per una query sul database:

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

Ed ecco lo stesso metodo che gestisce in modo asincrono la chiamata del database:

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

Dietro le quinte il compilatore genera il codice asincrono appropriato. Quando l'applicazione effettua la chiamata a `FindTaskByIdAsync`, ASP.NET rende la `FindTask` richiesta e quindi viene rimosso il thread di lavoro e rende disponibili per l'elaborazione di un'altra richiesta. Quando il `FindTask` richiesta viene eseguita, per continuare a elaborare il codice che segue tale chiamata viene riavviato un thread. Nel frattempo tra quando i `FindTask` richiesta viene avviata e quando vengono restituiti i dati, si dispone di un thread disponibile per eseguire operazioni utili che in caso contrario potrebbero essere associato in attesa di risposta.

Comporta un sovraccarico per il codice asincrono, ma in condizioni di carico basso, il sovraccarico non è irrilevante, mentre in condizioni di carico elevato in grado di elaborare le richieste in caso contrario, saranno contenute in attesa di thread disponibili.

È stato possibile eseguire questo tipo di programmazione asincrona a partire da ASP.NET 1.1, ma è difficile eseguire il debug, difficili da scrivere e soggetta a errori. Ora che è stata semplificata la scrittura di codice per tale in ASP.NET 4.5, non c'è nessun motivo per non più.

### <a name="async-support-in-entity-framework-6"></a>Supporto asincrono in Entity Framework 6

Come parte del supporto di async in 4.5 abbiamo fornito supporto asincrono per chiamate al servizio web socket e il sistema di file i/o, ma il modello più comune per le applicazioni web consiste nel raggiungere un database e le librerie di dati non supportava asincrono. Entity Framework 6 ora aggiunge il supporto di async per l'accesso al database.

In Entity Framework 6 tutti i metodi che causano una query o comandi da inviare al database hanno versioni asincrone. In questo esempio mostra la versione asincrona del *trovare* (metodo).

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

E questo supporto di async funziona non solo per gli inserimenti, eliminazioni, gli aggiornamenti e trova semplice, funziona anche con le query LINQ:

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

È presente un' `Async` versione del `ToList` metodo perché in questo codice è il metodo che genera una query da inviare al database. Il `Where` e `OrderByDescending` metodi solo configurare la query, mentre le `ToListAsync` metodo esegue la query e archivia la risposta nel `result` variabile.

## <a name="summary"></a>Riepilogo

È possibile implementare le procedure guidate di sviluppo web illustrate nell'articolo in tutte qualsiasi ambiente cloud e framework di programmazione web, ma sono disponibili strumenti di ASP.NET e Windows Azure per semplificare. Se si seguono questi modelli, è possibile scalare orizzontalmente il livello web e si saranno ridurre al minimo le spese, poiché ogni server sarà in grado di gestire più traffico.

Il [capitolo successivo](single-sign-on.md) esamina come il cloud consente scenari single sign-on.

## <a name="resources"></a>Risorse

Per altre informazioni vedere le risorse seguenti.

Server web senza stato:

- [Microsoft Patterns and Practices - indicazioni sulla scalabilità automatica](https://msdn.microsoft.com/library/dn589774.aspx).
- [La disabilitazione della ARR affinità nei siti Web di Azure di Windows dell'istanza](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/). Post di blog di Erez Benari, spiega l'affinità di sessione in siti Web di Microsoft Azure.

CDN:

- [Operatore alternativo: Creazione di servizi Cloud scalabili, resilienti](https://channel9.msdn.com/Series/FailSafe). Serie video in nove parti Marc Mercuri, Ulrich Homann e Mark Simms. Vedere la discussione della rete CDN nell'episodio 3 iniziando in corrispondenza di 1:34:00.
- [Modello di Microsoft Patterns and Practices Hosting di contenuto statico](https://msdn.microsoft.com/library/dn589776.aspx)
- [Le verifiche della rete CDN](http://www.cdnreviews.com/). Panoramica di molte reti CDN.

Programmazione asincrona:

- [Uso di metodi asincroni in ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Esercitazione di Rick Anderson.
- [Programmazione asincrona con Async e Await (c# e Visual Basic)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx). White paper MSDN che spiega i motivi per la programmazione asincrona, come funziona in ASP.NET 4.5 e come scrivere codice per la sua implementazione.
- [Query asincrone di Entity Framework e salvataggio](https://msdn.microsoft.com/data/jj819165)
- [Come creare applicazioni Web ASP.NET usando Async](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Presentazione video di Rowan Miller. Include una dimostrazione di grafici di programmazione asincrona può facilitare un notevole aumento della velocità effettiva del server web in condizioni di carico elevato.
- [Operatore alternativo: Creazione di servizi Cloud scalabili, resilienti](https://channel9.msdn.com/Series/FailSafe). Serie video in nove parti Marc Mercuri, Ulrich Homann e Mark Simms. Per le discussioni relative all'impatto della programmazione asincrona sulla scalabilità, vedere episodio 4 e 8 episodio.
- [La magia di uso di metodi asincroni in ASP.NET 4.5 oltre un importante trabocchetto](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Post di blog di Scott Hanselman, principalmente sull'uso della funzionalità async in applicazioni Web Form ASP.NET.

Per procedure guidate di sviluppo web aggiuntivi, vedere le risorse seguenti:

- [La correzione applicazione - procedure consigliate di esempio](the-fix-it-sample-application.md#bestpractices). Appendice di questo e-book elencata una serie di procedure consigliate che sono state implementate nell'applicazione Fix It.
- [Elenco di controllo per gli sviluppatori Web](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [Precedente](continuous-integration-and-continuous-delivery.md)
> [Successivo](single-sign-on.md)
