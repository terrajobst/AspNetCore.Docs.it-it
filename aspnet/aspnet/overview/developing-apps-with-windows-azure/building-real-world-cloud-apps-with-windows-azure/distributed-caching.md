---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: Memorizzazione nella cache (creazione di applicazioni con Azure Cloud del mondo reale) distribuita | Documenti Microsoft
author: MikeWasson
description: "Le App per Cloud mondo reale compilazione con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure che è possibile..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2015
ms.topic: article
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 24ede9cb9289c84140f6e2573f9d526f19cac64b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>Distribuita di memorizzazione nella cache (compilazione Cloud del mondo reale App con Azure)
====================
da [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download correggerlo progetto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [E-book di Download](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **predefiniti reale World Cloud App con Azure** e-book è basato su una presentazione sviluppata da Scott Guthrie. Vengono descritte le 13 modelli e procedure consigliate che consentono di avere esito negativo con lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [primo capitolo](introduction.md).


Capitolo precedente esaminato gestione degli errori temporanei e indicato come interruttore strategia di memorizzazione nella cache. Questo capitolo vengono fornite ulteriori informazioni sulla memorizzazione nella cache, incluso quando utilizzare la funzionalità, i modelli comuni per il relativo utilizzo e la relativa implementazione in Azure.

## <a name="what-is-distributed-caching"></a>Che cos'è distribuita la memorizzazione nella cache

Una cache offre una velocità effettiva elevata, bassa latenza di accesso ai dati dell'applicazione si accede di frequente, archiviando i dati in memoria. Per un'app cloud il tipo di cache più utile è la cache distribuita, ovvero i dati non vengono archiviati in memoria del server web singoli, ma in altre risorse del cloud e i dati memorizzati nella cache diventa disponibili per tutti i server di un'applicazione web (o altri cloud macchine virtuali che ar e utilizzata dall'applicazione).

![diagramma che illustra più server web, accesso ai server di cache stessa](distributed-caching/_static/image1.png)

Quando l'applicazione viene ridimensionata tramite l'aggiunta o rimozione di server o quando vengono sostituiti i server a causa di aggiornamenti o errori, i dati memorizzati nella cache rimangono accessibili a tutti i server che esegue l'applicazione.

Evitando l'accesso ai dati ad alta latenza di un archivio dati persistente, la memorizzazione nella cache possono migliorare notevolmente la velocità di risposta dell'applicazione. Ad esempio, il recupero dei dati dalla cache è molto più veloce rispetto recuperarlo da un database relazionale.

Dei vantaggi della memorizzazione nella cache viene ridotto il traffico verso l'archivio dati persistente, con conseguente riduzione dei costi quando sono presenti dati in uscita gli addebiti per l'archivio dati persistente.

## <a name="when-to-use-distributed-caching"></a>Quando utilizzare la memorizzazione nella cache distribuita

Memorizzazione nella cache più adatta per i carichi di lavoro dell'applicazione che non la lettura di più rispetto alla scrittura di dati e quando il modello di dati supporta l'organizzazione di chiave/valore utilizzate per archiviare e recuperare i dati nella cache. È inoltre più utile quando gli utenti dell'applicazione condividono molti dati comuni; ad esempio, cache non tanti vantaggi se fornisce in genere, ogni utente recupera dati univoci per l'utente. Un esempio in cui potrebbe essere molto utile la memorizzazione nella cache è un catalogo dei prodotti, poiché i dati non vengono modificati spesso e tutti i clienti siano esaminando gli stessi dati.

Il vantaggio della memorizzazione nella cache diventa sempre più misurabile maggiore scalabilità di un'applicazione, come i limiti di velocità effettiva e i ritardi di latenza dell'archivio dati permanente diventano più di un limite per le prestazioni complessive dell'applicazione. Tuttavia, è possibile implementare la memorizzazione nella cache per altri motivi di prestazioni anche. Per i dati che non devono essere perfettamente aggiornati quando viene visualizzata all'utente, accesso alla cache può essere utilizzato come un interruttore per quando l'archivio dati persistente è che non risponde o non disponibile.

## <a name="popular-cache-population-strategies"></a>Strategie di popolamento della cache comuni

Per poter recuperare i dati dalla cache, è necessario archiviarlo esiste prima. Sono disponibili diverse strategie per il recupero di dati necessari in una cache:

- Su richiesta / Cache Aside

    L'applicazione tenta di recuperare i dati dalla cache e quando la cache non dispone di dati (un "mancati"), l'applicazione archivia i dati nella cache in modo che sia disponibile al successivo. Alla successiva, che l'applicazione tenta di ottenere gli stessi dati, Trova ciò che sta cercando nella cache (un "accesso"). Per evitare il recupero di dati memorizzati nella cache sono stata modificata nel database, invalidano la cache quando si apportano modifiche all'archivio dati.
- Push di dati in background

    Servizi in background push dei dati nella cache in base a una pianificazione regolare e l'app sempre esegue il pull dalla cache. Questo approccio funziona bene con le origini dati ad alta latenza che non richiedono sempre di restituire i dati più recenti.
- Interruttore

    L'applicazione è in genere comunica direttamente con l'archivio dati persistente, ma quando l'archivio dati persistente presenta problemi di disponibilità, l'applicazione recupera dati dalla cache. Dati potrebbero essere stati messi in cache utilizzando la cache riservata o strategia di push dei dati in background. Si tratta di handler strategia anziché una strategia di miglioramento delle prestazioni.

Per mantenere i dati nella cache corrente, è possibile eliminare le voci della cache correlati quando l'applicazione consente di creare, gli aggiornamenti, o Elimina i dati. Se è bene per l'applicazione per ottenere in alcuni casi i dati obsoleti leggermente, è possibile basarsi su una data di scadenza per impostare un limite della cache risale dati possono essere configurabili.

È possibile configurare la scadenza assoluta (quantità di tempo dopo che è stato creato l'elemento della cache) o alla scadenza (quantità di tempo trascorso dall'ultimo accesso di un elemento della cache). La scadenza assoluta viene utilizzata quando si è legati al meccanismo di scadenza della cache per impedire che i dati di diventare obsoleti. Nell'app Correggi si saranno rimuovere manualmente gli elementi della cache non aggiornata e verrà utilizzata la scadenza variabile per mantenere i dati più recenti nella cache. Indipendentemente dal criterio di scadenza desiderato, la cache automaticamente rimuoverà gli elementi meno recenti (almeno usati di recente o LRU) quando viene raggiunto il limite di memoria della cache.

## <a name="sample-cache-aside-code-for-fix-it-app"></a>Esempio di codice di cache-aside per Correggi app

Nell'esempio di codice seguente, è innanzitutto nella cache durante il recupero di un'attività di correzione. Se l'attività è stata trovata nella cache, vengono restituiti; Se non viene trovato, scaricarla dal database e memorizzarlo nella cache. Le modifiche verrebbero apportate aggiungere la memorizzazione nella cache per il `FindTaskByIdAsync` metodo vengono evidenziati.

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

Quando si aggiorna o si elimina un'attività di correzione, è necessario invalidare (rimuovere) memorizzato nella cache attività. In caso contrario, futuro tenta di leggere che tale attività continuerà ad avere i vecchi dati dalla cache.

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

Questi sono esempi per illustrare semplice codice di memorizzazione nella cache. la memorizzazione nella cache non è stato implementato nel Correggi progetto scaricabile.

## <a name="azure-caching-services"></a>Servizi di caching di Azure

Azure offre i seguenti servizi di memorizzazione nella cache: [Cache Redis di Azure](https://msdn.microsoft.com/library/dn690523.aspx) e [Cache gestita di Azure](https://msdn.microsoft.com/library/dn386094.aspx). Cache Redis di Azure si basa sulla famosa [Cache Redis open source](http://redis.io/) ed è la prima scelta per la maggior parte di memorizzazione nella cache gli scenari.

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>Stato della sessione ASP.NET utilizza un provider di cache

Come accennato nel [capitolo di web sviluppo best practices](web-development-best-practices.md), una procedura consigliata consiste nell'evitare di utilizzare lo stato della sessione. Se l'applicazione richiede lo stato della sessione, la successiva procedura ottimale consiste nell'evitare il provider in memoria predefinito dal momento che non abilitare la scalabilità orizzontale (più istanze del server web). Il provider di stato sessione ASP.NET SQL Server consente a un sito in esecuzione su più server web per utilizzare lo stato della sessione, ma comporta un costo di latenza elevata rispetto a un provider in memoria. La soluzione migliore se è necessario utilizzare lo stato della sessione consiste nell'utilizzare un provider di cache, ad esempio il [Provider di stato sessione per Cache di Azure](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).

## <a name="summary"></a>Riepilogo

Si è visto come app Correggi Impossibile implementare la memorizzazione nella cache per migliorare la scalabilità e tempo di risposta e per consentire all'app continuare a essere reattiva per operazioni di lettura quando il database è disponibile. Nel [capitolo successivo](queue-centric-work-pattern.md) vi mostreremo come ulteriore miglioramento della scalabilità e rendere l'app continuano a essere reattiva per operazioni di scrittura.

## <a name="resources"></a>Risorse

Per ulteriori informazioni sulla memorizzazione nella cache, vedere le risorse seguenti.

Documentazione

- [Cache di Azure](https://msdn.microsoft.com/library/gg278356.aspx). Ufficiale documentazione MSDN relativa alla memorizzazione nella cache in Azure.
- [Microsoft Patterns and Practices - informazioni aggiuntive su Azure](https://msdn.microsoft.com/library/dn568099.aspx). Visualizzare informazioni aggiuntive di memorizzazione nella cache e modello Cache-Aside.
- [Operatore alternativo: Informazioni aggiuntive per architetture Cloud resilienti](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). White paper Marc Mercuri, Ulrich Homann e Andrew Townhill. Vedere la sezione la memorizzazione nella cache.
- [Procedure consigliate per la progettazione di servizi su larga scala nei servizi Cloud Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). W. White paper Mark Simms e Michael Thomassy. Vedere la sezione di memorizzazione nella cache distribuita.
- [Cache nel percorso scalabilità distribuita](https://msdn.microsoft.com/magazine/dd942840.aspx). Un articolo di MSDN Magazine meno recente (2009), ma un'introduzione chiare per la cache distribuita. vengono inseriti in modo più dettagliato le sezioni di memorizzazione nella cache dei white paper operatore alternativo e procedure consigliate.

Video

- [Operatore alternativo: Compilazione di servizi Cloud resilienti e scalabili](https://channel9.msdn.com/Series/FailSafe). Serie di nove parti Ulrich Homann, Marc Mercuri e Mark Simms. Presenta una visualizzazione a livello di 400 come progettare applicazioni basate su cloud. Questa serie è incentrata sulla teoria e le ragioni; Per ulteriori informazioni sulle procedure, vedere la serie compilazione Big Mark Simms. Vedere la discussione di memorizzazione nella cache in episodio 3 a partire da 1:24:14.
- [Compilazione Big: Apprese dai clienti di Azure - parte I](https://channel9.msdn.com/Events/Build/2012/3-029). Simon Davies viene distribuita la memorizzazione nella cache a partire da 46:00. Simile alla serie di operatore alternativo ma consente di spostarsi in dettaglio procedure. La presentazione è stata specificata il 31 ottobre 2012 in modo non copre il servizio cache di App Web in Azure App Service che è stata introdotta in 2013.

Nell'esempio di codice

- [Sviluppo di servizi in Azure cloud](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Applicazione di esempio che implementa la memorizzazione nella cache distribuita. Vedere il blog relativo post [sviluppo di servizi Cloud: nozioni di base sulla memorizzazione nella cache](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).

>[!div class="step-by-step"]
[Precedente](transient-fault-handling.md)
[Successivo](queue-centric-work-pattern.md)
