---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: Errori temporanei di gestione (creazione di applicazioni Cloud del mondo reale con Azure) | Documenti Microsoft
author: MikeWasson
description: "Le App per Cloud mondo reale compilazione con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure che è possibile..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/03/2015
ms.topic: article
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: b743b04789c5e5ebf5ab922cf34a516a16a6d356
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>Errori temporanei di gestione (creazione di applicazioni Cloud del mondo reale con Azure)
====================
da [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download correggerlo progetto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [E-book di Download](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **predefiniti reale World Cloud App con Azure** e-book è basato su una presentazione sviluppata da Scott Guthrie. Vengono descritte le 13 modelli e procedure consigliate che consentono di avere esito negativo con lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [primo capitolo](introduction.md).


Quando si progetta un'applicazione cloud reale, una delle operazioni di che è necessario preoccuparsi viene illustrato come gestire le interruzioni di servizio temporaneo. Questo problema è importante in modo univoco in cloud App perché è così dipendono le connessioni di rete e servizi esterni. È possibile ottenere spesso poco anomalie che in genere di riparazione automatica e, se non si è pronti a gestirli in modo intelligente, sarà comportano un utilizzo non valido per i clienti.

## <a name="causes-of-transient-failures"></a>Cause di errori temporanei

Nell'ambiente cloud sono disponibili non riusciti e che le connessioni al database eliminato si verifica periodicamente. Che è in parte perché sarà tramite servizi di bilanciamento del carico più rispetto all'ambiente locale in cui il server web e server di database dispone di una connessione fisica diretta. Inoltre, talvolta, quando si è dipendente da un servizio multi-tenant vedrai chiamate nel servizio get più lento o di un timeout perché qualcuno che utilizza il servizio sta impegnando frequentemente. In altri casi potrebbe essere l'utente che sta impegnando troppo frequente del servizio e il servizio è: Nega connessioni – limita deliberatamente per evitare di influire negativamente sugli altri tenant del servizio.

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>Utilizzare logica di tentativi/Backoff intelligente per ridurre l'effetto di errori temporanei

Anziché generare un'eccezione e la visualizzazione di una pagina di errore o non disponibile per il cliente, è possibile rilevare gli errori che sono in genere temporanei e automaticamente ripetere l'operazione che ha generato il messaggio di errore spera che prima di durata sarà ha esito positivo. La maggior parte dei casi l'operazione avrà esito positivo al secondo tentativo e sarà possibile recuperare l'errore senza il cliente dovesse essere state tenere presente che si è verificato un problema.

Esistono diversi modi, è possibile implementare logica di riesecuzione smart.

- Microsoft Patterns &amp; gruppo di procedure consigliate ha un [Transient Fault Handling Application Block](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx) che non tutti gli elementi per l'utente se si utilizza ADO.NET per l'accesso al Database SQL (non tramite Entity Framework). È possibile impostare un criterio per i tentativi: il numero di volte per ripetere una query o di comando e il tempo di attesa tra tentativi: ed esegue il wrapping del database SQL di codice un *utilizzando* blocco.

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    Supporta inoltre TFH [Cache nel ruolo di Azure](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx) e [Bus di servizio](https://azure.microsoft.com/services/service-bus/).
- Quando si utilizza Entity Framework in genere non sono in uso direttamente con le connessioni SQL, pertanto non è possibile utilizzare questo pacchetto Patterns and Practices, ma Entity Framework 6 si basa questo tipo di logica di riesecuzione direttamente nel framework. In modo analogo è specificare la strategia di tentativi e quindi EF utilizza questa strategia, ogni volta che accede al database.

    Per utilizzare questa funzionalità nell'app Correggi, è sufficiente è aggiungere una classe che deriva da *DbConfiguration* e attivare la logica di tentativi.

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    Per le eccezioni di Database SQL che il framework identifica come errori temporanei in genere, il codice mostrato indica EF per ripetere l'operazione fino a 3 volte, con un ritardo backoff esponenziale tra tentativi e un ritardo massimo di 5 secondi. Backoff esponenziale significa che dopo ogni tentativo non riuscito attenderà per un periodo di tempo prima di riprovare. Se in una riga tre tentativi hanno esito negativo, verrà generata un'eccezione. La sezione seguente relativa circuito spiega motivo per cui si desidera backoff esponenziale e un numero limitato di tentativi.

    Quando si utilizza il servizio di archiviazione di Azure, come l'app Correggi non per i BLOB e l'API client di archiviazione .NET implementa già lo stesso tipo di logica, è possibile avere problemi simili. È sufficiente specificare il criterio di ripetizione, o non è ancora a tale scopo se si riescono con le impostazioni predefinite.

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>Interruttori

Esistono diversi motivi per cui non si desidera ripetere troppe volte un determinato periodo troppo lungo:

- Numero eccessivo di utenti in modo permanente ritentare le richieste non riuscite può influire negativamente sulle prestazioni degli altri utenti. Se milioni di persone sono tutti effettua ripetuti ritentare le richieste è possibile bloccare le code di recapito IIS e impedendo l'app di soddisfare le richieste che altrimenti in grado di gestire correttamente.
- Se tutti gli utenti ritenterà l'esecuzione a causa di un errore del servizio, si potrebbe pertanto un numero di richieste accodate che il servizio ottiene flood all'avvio di ripristinare.
- Se l'errore è dovuto alla limitazione ed è presente un intervallo di tempo il servizio utilizza per la limitazione delle richieste, tentativi continuare Impossibile spostare la finestra e causare la limitazione delle richieste continuare.
- Potrebbe essere un utente in attesa di una pagina web per eseguire il rendering. Apportato persone attesa è troppo lungo potrà essere indesiderate più tale relativamente rapidamente consulenza riprovare in seguito.

Backoff esponenziale risolve alcuni di questi problema limitando la frequenza di tentativi di che un servizio può ottenere dall'applicazione. Ma è necessario disporre anche *circuito*: questo significa che in un determinato ripetere soglia app arresta nuovo tentativo in corso e accetta un'altra azione, ad esempio uno dei seguenti:

- Fallback personalizzato. Se non è possibile ottenere un prezzo azionario da Reuters, forse è possibile ottenerlo dal Bloomberg; o se è Impossibile ottenere dati dal database, forse è possibile ottenerlo dalla cache.
- Esito negativo invisibile all'utente. Se è necessario da un servizio non radicale per l'app, semplicemente restituire null se non è possibile ottenere i dati. Se viene visualizzato un'attività Correggi e il servizio Blob non risponde, è possibile visualizzare i dettagli dell'attività senza l'immagine.
- Esito negativo rapido. L'utente per non sovraccaricare il servizio con un errore ripetere le richieste che potrebbero causare interruzioni del servizio per altri utenti o estendere un intervallo di limitazione. È possibile visualizzare un messaggio descrittivo "e riprovare".

Non è presente alcun criterio di ripetizione giusto. È possibile ripetere più volte e attendere più in un processo di lavoro in background asincrona rispetto in un'app web sincrono in cui un utente è in attesa di una risposta. È possibile attendere più tra i tentativi per un servizio di database relazionale che si farebbe per un servizio cache. Ecco alcuni esempi consigliati criteri per farsi un'idea di come potrebbero variare i numeri di tentativi. (Nessun ritardo prima del primo tentativo significa "Fast First".

![Criteri di tentativi di esempio](transient-fault-handling/_static/image1.png)

Per istruzioni criteri tentativi di Database SQL, vedere [risolvere i problemi relativi a errori temporanei e connessione al Database SQL](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/).

## <a name="summary"></a>Riepilogo

Una strategia di tentativi/Backoff contribuisce a rendere errori temporanei invisibile al cliente la maggior parte dei casi, e Microsoft fornisce il Framework che consente di ridurre al minimo il lavoro di implementazione di una strategia se si utilizza ADO.NET Entity Framework o Azure Servizio di archiviazione.

Nel [capitolo successivo](distributed-caching.md), esamineremo come migliorare le prestazioni e affidabilità con memorizzazione nella cache distribuita.

## <a name="resources"></a>Risorse

Per altre informazioni, vedere le seguenti risorse:

Documentazione

- [Procedure consigliate per la progettazione di servizi su larga scala nei servizi Cloud Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). White paper Mark Simms e Michael Thomassy. Simile alla serie di operatore alternativo ma consente di spostarsi in dettaglio procedure. Vedere la sezione di dati di telemetria e diagnostica.
- [Operatore alternativo: Informazioni aggiuntive per architetture Cloud resilienti](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). White paper Marc Mercuri, Ulrich Homann e Andrew Townhill. Versione della pagina Web della serie di video di operatore alternativo.
- [Microsoft Patterns and Practices - informazioni aggiuntive su Azure](https://msdn.microsoft.com/library/dn568099.aspx). Vedere tentativi criterio di ricerca, il modello dell'utilità di pianificazione agente supervisore.
- [La tolleranza di errore nel Database SQL di Azure](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx). Post di blog di Tony Petrossian.
- [Entity Framework - resilienza di connessione / la logica di riesecuzione](https://msdn.microsoft.com/data/dn456835). Come usare e personalizzare il funzionalità di Entity Framework 6 di gestione di errori temporanei.
- [Resilienza di connessione e comando di intercettazione con Entity Framework in un'applicazione MVC ASP.NET](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Quarto in una serie di esercitazioni nove parti, viene illustrato come impostare la funzionalità di resilienza di connessione Entity Framework 6 per il Database SQL.

Video

- [Operatore alternativo: Compilazione di servizi Cloud resilienti e scalabili](https://channel9.msdn.com/Series/FailSafe). Serie di nove parti Ulrich Homann, Marc Mercuri e Mark Simms. Presenta i concetti generali e i principi architetturali in modo molto accessibile e interessano con storie ricavate dall'esperienza di Microsoft Team (CAT, Customer Advisory) con i clienti effettivi. Vedere la discussione di interruttori in episodio 3 a partire da 40:55.
- [Compilazione Big: Apprese dai clienti di Azure - parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Gestione e la strumentazione di tutti gli elementi dell'errore Mark Simms discussioni sulla progettazione per un errore temporaneo.

Nell'esempio di codice

- [Sviluppo di servizi in Azure cloud](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Esempio di applicazione creato da Microsoft Azure Customer Advisory Team viene illustrato come utilizzare il [Enterprise Library Transient Fault Handling Block](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH). Per ulteriori informazioni, vedere [Cloud servizio nozioni fondamentali su livello accesso dati: gestione degli errori temporanei](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx). TFH è consigliato per l'accesso al database mediante ADO.NET direttamente (senza dover utilizzare Entity Framework).

>[!div class="step-by-step"]
[Precedente](monitoring-and-telemetry.md)
[Successivo](distributed-caching.md)
