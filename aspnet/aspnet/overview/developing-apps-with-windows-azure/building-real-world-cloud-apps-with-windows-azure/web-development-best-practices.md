---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Web consigliate per lo sviluppo (creazione di applicazioni con Azure Cloud del mondo reale) | Documenti Microsoft
author: MikeWasson
description: Le App per Cloud mondo reale compilazione con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure che è possibile...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: 4c43b256018d91e89b3427f90fc5c6cd018641f9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Consigliate per lo sviluppo Web (creazione di applicazioni Cloud reale in Azure)
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download correggerlo progetto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [scaricare E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **predefiniti reale World Cloud App con Azure** e-book è basato su una presentazione sviluppata da Scott Guthrie. Vengono descritte le 13 modelli e procedure consigliate che consentono di avere esito negativo con lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [primo capitolo](introduction.md).


I primi tre modelli sono stati sulla configurazione di un processo di sviluppo agile. gli altri sono sull'architettura e codice. Questa è una raccolta di procedure consigliate di sviluppo web:

- [Server web senza stato](#stateless) dietro il bilanciamento del carico intelligente.
- [Evitare lo stato della sessione](#sessionstate) (o se non è possibile evitare tale situazione, usare cache distribuita, invece di un database).
- [Usa una rete CDN](#cdn) agli asset di file statici cache perimetrale (immagini, gli script).
- [Utilizzare il supporto asincrono del .NET 4.5](#async) per evitare di bloccare le chiamate.

Queste procedure sono valide per tutti lo sviluppo web, non solo per le app cloud, ma sono particolarmente importanti per le applicazioni basate su cloud. Interagiscono per garantire un utilizzo ottimale di scalabilità estremamente flessibile offerto da parte dell'ambiente cloud. Se non si seguono queste procedure, viene eseguita in limitazioni quando si tenta di applicare la scalabilità dell'applicazione.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Livello web senza stato dietro il bilanciamento del carico intelligente

*Livello web senza stato* significa non memorizzare i dati dell'applicazione nel sistema di memoria o su file server di web. Mantenere il livello web senza stato consente di fornire una migliore esperienza del cliente e risparmiare denaro:

- Se il livello web è senza stato e si trova dietro un bilanciamento del carico, è possibile rispondere rapidamente alle modifiche del traffico dell'applicazione in modo dinamico aggiungendo o rimuovendo i server. Nell'ambiente cloud in cui si paga solo per le risorse del server fino a quando è in realtà utilizzare, la possibilità di rispondere alle modifiche nella richiesta si traduce in un notevole risparmio.
- Un livello web senza stato è strutturalmente molto più semplice di scalabilità orizzontale dell'applicazione. Troppo, che consente di rispondere alle esigenze più rapidamente la scalabilità e spesa nello sviluppo e test nel processo.
- I server del cloud, come i server locali, è necessario installare patch e riavviato occasionalmente; e se il livello web è senza stato, reindirizzamento del traffico quando si arresta temporaneamente un server non provocheranno errori o comportamenti imprevisti.

La maggior parte delle applicazioni reali necessario archiviare lo stato per una sessione web. il principale punto non viene archiviata nel server web. È possibile archiviare lo stato in altri modi, ad esempio nel client di cookie o all'esterno di processo sul lato server dello stato sessione ASP.NET utilizza un provider di cache. È possibile archiviare i file in [archiviazione Blob di Windows Azure](unstructured-blob-storage.md) anziché nel file system locale.

Ad esempio di come è facile ridimensionare un'applicazione in siti Web di Windows Azure, se il livello web senza stato, vedere il **scala** scheda per un sito Microsoft Azure Web nel portale di gestione:

![Scheda scala](web-development-best-practices/_static/image1.png)

Se si desidera aggiungere i server web, è solo possibile trascinare il dispositivo di scorrimento di numero di istanza a destra. Impostarla su 5 e fare clic su **salvare**, e in pochi secondi si dispone di 5 server web in Windows Azure, la gestione del traffico del sito web.

![Cinque istanze](web-development-best-practices/_static/image2.png)

È possibile impostare facilmente il numero di istanze fino a 3 o indietro fino a 1. Quando si scala back, iniziare risparmiando i costi immediatamente perché Windows Azure addebita al minuto, non in base all'ora.

È anche possibile impostare Windows Azure per aumentare o ridurre il numero di server web in base all'utilizzo della CPU automaticamente. Nell'esempio seguente, quando l'utilizzo della CPU scende sotto il 60%, il numero di server web verrà ridotti al minimo di 2 e se l'utilizzo della CPU è superiore all'80%, aumenterà il numero di server web con un massimo di 4.

![Scala per l'utilizzo della CPU](web-development-best-practices/_static/image3.png)

O cosa accade se si è certi che il sito verrà solo occupato durante le ore lavorative? È possibile indicare a Windows Azure per eseguire più server durante il giorno e diminuire in un singolo server di validità, nights e i fine settimana. La serie di catture di schermata seguente viene illustrato come configurare il sito web per eseguire un server in orari e 4 server durante le ore lavorative dalle 8.00 alle 17.00.

![Scalabilità, pianificazione](web-development-best-practices/_static/image4.png)

![Impostare i tempi di pianificazione](web-development-best-practices/_static/image5.png)

![Pianificazione diurno](web-development-best-practices/_static/image6.png)

![Pianificazione weeknight](web-development-best-practices/_static/image7.png)

![Pianificazione del fine settimana](web-development-best-practices/_static/image8.png)

E, naturalmente, tutto ciò può essere eseguita negli script nonché come nel portale.

La possibilità di scalare orizzontalmente l'applicazione è quasi illimitata in Windows Azure, purché si evita gli impedimenti in modo dinamico aggiungendo o rimuovendo le macchine virtuali di server, mantenendo il livello web senza stato.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Evitare lo stato della sessione

È spesso poco pratico in un'applicazione cloud reale per evitare di archiviare un tipo di stato per una sessione utente, ma alcuni approcci influire negativamente sulle prestazioni e scalabilità più rispetto ad altri. Se è necessario archiviare lo stato, la soluzione migliore è di ridurre la quantità di stato e archiviarla nel cookie. Se ciò non è possibile, la soluzione migliore successiva consiste nell'utilizzare lo stato della sessione ASP.NET con un provider per [cache distribuita e in memoria](distributed-caching.md#sessionstate). La soluzione peggiore da un punto di vista della scalabilità e prestazioni è stato eseguito il provider di stato della sessione per utilizzare un database.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Utilizzare una rete CDN per memorizzare nella cache gli asset di file statici

Rete CDN è l'acronimo di rete per la distribuzione del contenuto. Fornire risorse di file statici, ad esempio immagini e i file di script a un provider di rete CDN e il provider memorizza nella cache di questi file nei data Center nel mondo, in modo che ogni volta che gli utenti accedono all'applicazione, ricevono risposta relativamente rapido e a bassa latenza per memorizzato nella cache Asset. Ciò consente di velocizzare il tempo di carico complessivo del sito e si riduce il carico sul server web. Le reti CDN sono importanti specialmente se si desidera connettersi a un gruppo di destinatari che è ampiamente distribuita geograficamente.

Windows Azure ha una rete CDN, è possibile utilizzare altri CDN in un'applicazione che viene eseguito in Windows Azure o in qualsiasi ambiente di hosting web.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>Utilizzare il supporto asincrono del .NET 4.5 per evitare il blocco delle chiamate

.NET 4.5 migliorato linguaggi di programmazione c# e VB in modo da rendere molto più semplice gestire le attività in modo asincrono. Il vantaggio della programmazione asincrona non è disponibile solo per l'elaborazione parallela di situazioni, ad esempio quando si desidera avviare contemporaneamente più chiamate al servizio web. Consente inoltre al server web di eseguire in modo più efficiente e affidabile in condizioni di carico elevato. Un server web contiene solo un numero limitato di thread disponibili, e in condizioni di carico elevato quando tutti i thread sono in uso, le richieste in ingresso necessario attendere fino a quando non verranno liberati thread. Se il codice dell'applicazione non gestisce le attività come chiamate al servizio web e query di database in modo asincrono, numero di thread viene inutilmente vincolate mentre il server è in attesa di una risposta dei / o. Questo riduce la quantità di traffico che in condizioni di carico elevato grado di gestire il server. Con la programmazione asincrona, i thread in attesa di un servizio web o un database per restituire i dati vengono liberate fino a soddisfare nuove richieste fino a quando i dati di ricezione. In un server web occupato, centinaia o migliaia di richieste può quindi essere elaborata immediatamente che in caso contrario potrebbe essere in attesa per liberare i thread.

Come illustrato in precedenza, è facile per ridurre il numero di server web gestisce il sito web perché è di aumentare. Pertanto, se un server può ottenere maggiore velocità effettiva, non è necessario poiché molti di essi si può ridurre i costi in quanto è necessario per un determinato volume meno server di quanto sarebbe altrimenti.

Supporto per il modello di programmazione asincrona .NET 4.5 è incluso in ASP.NET 4.5 per Web Form, MVC e Web API. in Entity Framework 6 e nel [API di archiviazione di Azure Windows](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>Supporto asincrono in ASP.NET 4.5

In ASP.NET 4.5, il supporto per la programmazione asincrona è stata aggiunta non solo per la lingua ma anche per i framework MVC, Web Form e Web API. Ad esempio, un metodo di azione del controller MVC ASP.NET riceve dati da una richiesta web e i dati vengono passati a una visualizzazione che crea quindi il codice HTML da inviare al browser. Spesso è necessario il metodo di azione ottenere dati da un database o servizio web per visualizzarla in una pagina web o per salvare i dati immessi in una pagina web. In questi scenari è facile rendere il metodo di azione asincrono: invece di restituire un *ActionResult* dell'oggetto, restituire *attività&lt;ActionResult&gt;*  e contrassegnare il metodo con il *async* (parola chiave). All'interno del metodo, quando una riga di codice avvia un'operazione che implica il tempo di attesa, viene contrassegnato con la parola chiave await.

Ecco un metodo di azione semplice che chiama un metodo di repository per una query sul database:

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

Di seguito è lo stesso metodo che gestisce la chiamata al database in modo asincrono:

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

Dietro le quinte il compilatore genera il codice asincrono appropriato. Quando l'applicazione effettua la chiamata a `FindTaskByIdAsync`, ASP.NET esegue il `FindTask` e quindi rimuove il thread di lavoro e rende disponibili per l'elaborazione di un'altra richiesta. Quando il `FindTask` viene eseguita l'operazione richiesta, un thread viene riavviato per continuare l'elaborazione del codice che segue la chiamata. Nel frattempo tra il momento di `FindTask` richiesta viene avviata e quando vengono restituiti i dati, si dispone di un thread disponibile per eseguire operazioni utili che diversamente sarebbero impegnato in attesa della risposta.

Per il codice asincrono comporta un certo sovraccarico, ma in condizioni di carico basso, l'overhead è irrilevante, mentre in condizioni di carico elevato in grado di elaborare le richieste che in caso contrario, potrebbero essere contenute in attesa di thread disponibili.

È stato possibile eseguire questo tipo di programmazione asincrona, a partire da ASP.NET 1.1, ma è difficile la scrittura, errori e difficile eseguire il debug. Ora che è stata semplificata la codifica, in ASP.NET 4.5, non vi è alcun motivo per non visualizzare più.

### <a name="async-support-in-entity-framework-6"></a>Supporto asincrono in Entity Framework 6

Come parte del supporto asincrono in 4.5 è stato fornito il supporto asincrono per chiamate al servizio web, socket e file system, i/o, ma il modello più comune per le applicazioni web è venga raggiunto un database e il nostro raccolte di dati non supportano asincrono. Ora Entity Framework 6 aggiunge il supporto asincrono per l'accesso al database.

In Entity Framework 6 tutti i metodi che causano una query o un comando da inviare al database hanno versioni asincrone. Questo esempio mostra la versione asincrona di *trovare* metodo.

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

Questo supporto asincrono funziona non solo per gli inserimenti, eliminazioni, aggiornamenti e trova semplice e funziona anche con le query LINQ:

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

È presente un `Async` versione il `ToList` metodo perché in questo codice è il metodo che genera una query da inviare al database. Il `Where` e `OrderByDescending` metodi solo configurare la query, mentre il `ToListAsync` metodo esegue la query e archivia la risposta nel `result` variabile.

## <a name="summary"></a>Riepilogo

È possibile implementare le web sviluppo consigliate descritte di seguito in qualsiasi framework e tutti gli ambienti di cloud di programmazione web, ma sono disponibili strumenti di ASP.NET e Windows Azure per semplificarne l'inserimento. Se si seguono questi modelli, è possibile ridimensionare orizzontalmente il livello web e si saranno ridurre al minimo le spese, perché ogni server sarà in grado di gestire più traffico.

Il [capitolo successivo](single-sign-on.md) esamina come il cloud consente scenari single sign-on.

## <a name="resources"></a>Risorse

Per ulteriori informazioni, vedere le risorse seguenti.

Server web senza stato:

- [Microsoft Patterns and Practices - materiale sussidiario per la scalabilità automatica](https://msdn.microsoft.com/library/dn589774.aspx).
- [La disattivazione ARR istanza affinità in siti Web di Azure Windows](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/). Post di blog di Erez Benari, illustra l'affinità di sessione in siti Web di Windows Azure.

CDN:

- [Operatore alternativo: Compilazione di servizi Cloud scalabili e resilienti](https://channel9.msdn.com/Series/FailSafe). Serie video in nove parti Ulrich Homann, Marc Mercuri e Mark Simms. Vedere la discussione della rete CDN nell'episodio 3 a partire da 1:34:00.
- [Modello di Microsoft Patterns e procedure consigliate statico Hosting del contenuto](https://msdn.microsoft.com/library/dn589776.aspx)
- [Le revisioni della rete CDN](http://www.cdnreviews.com/). Panoramica di molte reti CDN.

Programmazione asincrona:

- [Uso di metodi asincroni in ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Esercitazione di Rick Anderson.
- [Programmazione asincrona con Async e Await (c# e Visual Basic)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx). White paper di MSDN che illustra come scrivere codice per la relativa implementazione, il funzionamento in ASP.NET 4.5 e spiegazione logica per la programmazione asincrona.
- [Query asincrone di Entity Framework e al salvataggio](https://msdn.microsoft.com/data/jj819165)
- [Come creare applicazioni Web ASP.NET Usa Async](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Presentazione video di Miller Rowan. È inclusa una dimostrazione di grafici di programmazione asincrona può facilitare l'aumento della velocità effettiva del server web in condizioni di carico elevato.
- [Operatore alternativo: Compilazione di servizi Cloud scalabili e resilienti](https://channel9.msdn.com/Series/FailSafe). Serie video in nove parti Ulrich Homann, Marc Mercuri e Mark Simms. Per informazioni sull'impatto della programmazione asincrona sulla scalabilità, vedere episodio 4 e 8 episodio.
- [Il trucco dell'utilizzo di metodi asincroni in ASP.NET 4.5 oltre a un problema importante](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Post di blog di Scott Hanselman, principalmente sull'utilizzo di async in applicazioni Web Form ASP.NET.

Per procedure guidate di sviluppo web aggiuntivi, vedere le risorse seguenti:

- [Per risolvere il problema applicazione - procedure consigliate di esempio](the-fix-it-sample-application.md#bestpractices). Appendice di questo e-book viene elencata una serie di procedure consigliate che sono state implementate nell'applicazione Correggi.
- [Elenco di controllo di Web Developer](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [Precedente](continuous-integration-and-continuous-delivery.md)
> [Successivo](single-sign-on.md)
