---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: Distributed la memorizzazione nella cache (creazione di App Cloud funzionanti con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione Real World di App Cloud con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure consigliate che egli può...
ms.author: riande
ms.date: 07/20/2015
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 26866ef9d13a198aab627ccf0f5e1ff3d2304427
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577548"
---
<a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>Memorizzazione nella cache (Building Real-World Cloud App distribuite con Azure)
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Download risolverlo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **creazione Real World di App Cloud con Azure** eBook si basa su una presentazione sviluppata da Scott Guthrie. Viene illustrato 13 modelli e procedure consigliate che consentono di avere esito positivo lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [capitolo prima](introduction.md).


Nel capitolo precedente esaminato gestione degli errori temporanei e menzionato la memorizzazione nella cache come strategia di interruttore di circuito. In questo capitolo vengono fornite maggiori sulla memorizzazione nella cache, tra cui sul relativo utilizzo, i modelli comuni per l'uso e come implementarlo in Azure.

## <a name="what-is-distributed-caching"></a>Che cos'è distribuita la memorizzazione nella cache

Una cache offre elevata velocità effettiva, bassa latenza di accesso ai dati dell'applicazione si accede di frequente, archiviando i dati in memoria. Per un'app cloud il tipo di cache più utile è la cache distribuita, ovvero i dati non vengono archiviati in memoria del server web singoli, ma in altre risorse cloud e i dati memorizzati nella cache sarà disponibili per tutti i server web di un'applicazione (o altro cloud macchine virtuali che ar e utilizzato dall'applicazione).

![diagramma che mostra più server web accedono ai server della cache stessa](distributed-caching/_static/image1.png)

Quando l'applicazione viene scalata aggiungendo o rimuovendo i server o quando vengono sostituiti i server a causa di aggiornamenti o errori, i dati memorizzati nella cache rimangono accessibili a tutti i server che esegue l'applicazione.

Evitando l'accesso ai dati ad alta latenza di un archivio dati permanente, la memorizzazione nella cache può migliorare notevolmente la velocità di risposta dell'applicazione. Ad esempio, il recupero dei dati dalla cache è molto più veloce rispetto a recuperarli da un database relazionale.

Un vantaggio secondario, la memorizzazione nella cache si riduce il traffico verso l'archivio dati permanente, che potrebbe comportare costi più bassi quando sono presenti dati in uscita sarà addebitato l'archivio dati permanente.

## <a name="when-to-use-distributed-caching"></a>Quando usare la memorizzazione nella cache distribuita

Memorizzazione nella cache è più adatta per carichi di lavoro dell'applicazione che eseguire ulteriori informazioni rispetto alla scrittura di dati e quando il modello di dati supporta l'organizzazione di chiave/valore che consente di archiviare e recuperare i dati nella cache. È anche più utile quando gli utenti dell'applicazione condividono una grande quantità di dati comuni; ad esempio, della cache non fornirebbe tanti vantaggi se ogni utente in genere recupera dati univoci per l'utente. Un esempio in cui la memorizzazione nella cache potrebbe essere molto utili è un catalogo dei prodotti, poiché i dati non vengono modificati spesso e tutti i clienti cercano gli stessi dati.

Il vantaggio della memorizzazione nella cache diventa sempre più misurabile maggiore scalabilità dell'applicazione, perché i limiti di velocità effettiva e ritardi di latenza dell'archivio dati permanente diventano più di un limite per le prestazioni complessive dell'applicazione. Tuttavia, è possibile implementare la memorizzazione nella cache per altri motivi di prestazioni anche. Per i dati che non deve necessariamente essere perfettamente aggiornati quando visualizzata all'utente, accesso alla cache può essere utilizzato come un interruttore per quando l'archivio dati persistente non risponde o non disponibile.

## <a name="popular-cache-population-strategies"></a>Strategie di popolamento della cache più diffusi

Per poter essere in grado di recuperare i dati dalla cache, è necessario archiviarlo presenti prima di tutto. Sono disponibili diverse strategie per ottenere i dati necessari in una cache:

- Su richiesta o memorizzare nella Cache Aside

    L'applicazione tenta di recuperare dati dalla cache e quando la cache non dispone dei dati (un "miss"), l'applicazione archivia i dati nella cache in modo che sia disponibile al successivo. La volta successiva che l'applicazione prova a ottenere gli stessi dati, Trova ciò che sta cercando nella cache (un "riscontro"). Per evitare il recupero dei dati memorizzati nella cache che sono stato modificato nel database, invalidare la cache quando si apportano modifiche all'archivio dati.
- Push di dati in background

    Servizi in background push dei dati nella cache in base a una pianificazione regolare e l'app sempre pull dalla cache. Questo approccio funziona bene con le origini dati ad alta latenza che non richiedono sempre di restituire i dati più recenti.
- Interruttore di circuito

    L'applicazione è in genere comunica direttamente con l'archivio dati persistente, ma quando l'archivio dati persistente riscontra problemi di disponibilità, l'applicazione recupera i dati dalla cache. I dati inseriti nella cache utilizzando la cache riservato o strategia di push dei dati in background. Si tratta di handler strategia anziché una strategia di miglioramento delle prestazioni.

Per mantenere aggiornati i dati nella cache, è possibile eliminare le voci della cache correlate quando l'applicazione crea, Aggiorna, o Elimina i dati. Se è bene per l'applicazione ottenere i dati che non sono aggiornati leggermente in alcuni casi, è possibile basarsi su una scadenza configurabili per impostare un limite sulla cache quanto tempo i dati possono essere.

È possibile configurare la scadenza assoluta (quantità di tempo poiché è stato creato l'elemento della cache) o (quantità di tempo trascorso dall'ultimo accesso a un elemento della cache è stata) scadenza variabile. Scadenza assoluta viene usata quando dipendono il meccanismo di scadenza della cache per impedire che i dati diventi obsoleta. Nell'app Fix It, abbiamo manualmente verranno rimossi gli elementi della cache non aggiornata e si userà la scadenza variabile per mantenere i dati più recenti nella cache. Indipendentemente dai criteri di scadenza che scelto, la cache automaticamente rimuoverà gli elementi meno recenti (minimi usati di recente o LRU) quando viene raggiunto il limite di memoria della cache.

## <a name="sample-cache-aside-code-for-fix-it-app"></a>Esempio di codice di cache-aside per l'app Fix It

Nell'esempio di codice seguente, viene innanzitutto nella cache durante il recupero di un'attività Fix It. Se l'attività è disponibile nella cache, viene restituito. Se non viene trovato, ottenerlo dal database e memorizzarlo nella cache. Le modifiche da creare per aggiungere la memorizzazione nella cache per il `FindTaskByIdAsync` metodo vengono evidenziati.

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

Quando si aggiorna o elimina un'attività Fix It, è necessario invalidare attività (rimuovere) memorizzato nella cache. In caso contrario, i successivi tentativi leggere che tale attività continuerà ad avere i vecchi dati dalla cache.

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

Questi sono esempi per illustrare semplice codice di memorizzazione nella cache. la memorizzazione nella cache non è stata implementata nel Fix It progetto scaricabile.

## <a name="azure-caching-services"></a>Servizi di caching di Azure

Azure offre i servizi di memorizzazione nella cache seguenti: [Cache Redis di Azure](https://msdn.microsoft.com/library/dn690523.aspx) e [Cache gestita di Azure](https://msdn.microsoft.com/library/dn386094.aspx). Cache Redis di Azure si basa sul popolare [Cache Redis open source](http://redis.io/) ed è la scelta ideale per la maggior parte di memorizzazione nella cache gli scenari.

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>Stato della sessione ASP.NET usando un provider di cache

Come accennato nella [capitolo di web development best practices](web-development-best-practices.md), una procedura consigliata consiste nell'evitare di usare lo stato della sessione. Se l'applicazione richiede lo stato della sessione, per evitare il provider in memoria predefinito dal momento che non abilitare la scalabilità orizzontale (più istanze del server web) è la procedura consigliata successiva. Il provider di stato sessione SQL Server ASP.NET consente a un sito che viene eseguito su più server web per usare lo stato della sessione, ma comporta un costo di una latenza elevata rispetto a un provider in memoria. La soluzione migliore se è necessario usare lo stato della sessione consiste nell'usare un provider di cache, ad esempio la [Provider di stato della sessione per Cache di Azure](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).

## <a name="summary"></a>Riepilogo

Si è visto come l'app Fix It può implementare la memorizzazione nella cache per migliorare la scalabilità e il tempo di risposta e per consentire all'app di continuare a essere reattive per operazioni di lettura quando il database è disponibile. Nel [capitolo successivo](queue-centric-work-pattern.md) vi mostreremo come un'ulteriore miglioramento della scalabilità e rendere l'app continuano a essere reattivi per le operazioni di scrittura.

## <a name="resources"></a>Risorse

Per altre informazioni sulla memorizzazione nella cache, vedere le risorse seguenti.

Documentazione

- [Cache di Azure](https://msdn.microsoft.com/library/gg278356.aspx). Documentazione ufficiale di MSDN sulla memorizzazione nella cache in Azure.
- [Microsoft Patterns and Practices - informazioni aggiuntive su Azure](https://msdn.microsoft.com/library/dn568099.aspx). Visualizzare informazioni aggiuntive di memorizzazione nella cache e modello Cache-Aside.
- [FailSafe: Informazioni aggiuntive per architetture Cloud resilienti](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). White paper Marc Mercuri, Ulrich Homann e Andrew Townhill. Vedere la sezione sulla memorizzazione nella cache.
- [Procedure consigliate per la progettazione di servizi su larga scala nei servizi Cloud di Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). W. White paper Mark Simms e Michael Thomassy. Vedere la sezione sulla memorizzazione nella cache distribuita.
- [Memorizzazione nella cache nel percorso per la scalabilità distribuita](https://msdn.microsoft.com/magazine/dd942840.aspx). Un articolo di MSDN Magazine (2009) precedente, ma un'introduzione alla memorizzazione nella cache distribuita, in genere; scritta in modo chiaro entra in modo più dettagliato le sezioni di memorizzazione nella cache dei white paper operatore alternativo e procedure consigliate.

Video

- [Operatore alternativo: Creazione di servizi Cloud scalabili, resilienti](https://channel9.msdn.com/Series/FailSafe). Serie in nove parti Marc Mercuri, Ulrich Homann e Mark Simms. Presenta una visualizzazione di livello 400 di come progettare App cloud. Questa serie è incentrato sulla teoria e le ragioni; per altre informazioni sulle procedure, vedere la serie Building big data da Mark Simms. Vedere la discussione su memorizzazione nella cache nell'episodio 3 partire 1: 24:14.
- [Compilazione Big: Lezioni apprese dai clienti di Azure - parte I](https://channel9.msdn.com/Events/Build/2012/3-029). Simon Davies illustra distribuita memorizzazione nella cache, iniziando in corrispondenza di 46:00. Simile alla serie di tipo operatore alternativo, ma passa ad analizzare altri dettagli sulle procedure. La presentazione è stata assegnata il 31 ottobre 2012 in modo da non copre il servizio memorizzazione nella cache di App Web nel servizio App di Azure che è stato introdotto nel 2013.

Esempio di codice

- [Sviluppo di servizi in Azure cloud](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Applicazione di esempio che implementa la memorizzazione nella cache distribuita. Vedere il blog di accompagnamento [nozioni fondamentali su servizi Cloud-concetti di base di memorizzazione nella cache](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).

> [!div class="step-by-step"]
> [Precedente](transient-fault-handling.md)
> [Successivo](queue-centric-work-pattern.md)
