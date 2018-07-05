---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: Errori temporanei, la gestione (creazione di App per Cloud funzionanti con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione Real World di App Cloud con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure consigliate che egli può...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/03/2015
ms.topic: article
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: 13ed8c2373c22070d21519bc495161e956b0ac4d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398414"
---
<a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>Errori temporanei, la gestione (creazione di App per Cloud funzionanti con Azure)
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download risolverlo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **creazione Real World di App Cloud con Azure** eBook si basa su una presentazione sviluppata da Scott Guthrie. Viene illustrato 13 modelli e procedure consigliate che consentono di avere esito positivo lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [capitolo prima](introduction.md).


Quando si progetta un'applicazione cloud reale, una delle operazioni che è necessario preoccuparsi viene illustrato come gestire le interruzioni di servizio temporaneo. Questo problema è importante in modo univoco nelle App cloud perché si è così dipende da servizi esterni e le connessioni di rete. È possibile ottenere spesso poco anomalie che sono in genere ripara automaticamente e se non si è pronti a gestirli in modo intelligente, sono sarà il risultato in un'esperienza negativa per i clienti.

## <a name="causes-of-transient-failures"></a>Cause di errori temporanei

Nell'ambiente cloud si scoprirà che non è riuscita ed eliminata le connessioni di database eseguito periodicamente. Che è in parte perché si sta attraverso i bilanciamenti del carico di più rispetto all'ambiente locale in cui il server web e il server di database dispone di una connessione fisica diretta. Inoltre, in alcuni casi quando si è dipendente da un servizio multi-tenant scoprirai le chiamate al servizio get più lento o timeout perché qualcuno che utilizza il servizio è raggiunge questa porta in modo rilevante. In altri casi potrebbe essere l'utente che raggiunge il servizio una frequenza eccessiva e il servizio è: Nega le connessioni – limita intenzionalmente per evitare di influire negativamente sugli altri tenant del servizio.

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>Utilizzare logica di ripetizione dei tentativi/Backoff intelligente per ridurre l'effetto di errori temporanei

Invece di generare un'eccezione e visualizzazione di una pagina di errore o non è disponibile per il cliente, è in grado di riconoscere gli errori che sono in genere temporanei e automaticamente ripetere l'operazione che ha generato l'errore, si augura che tempo sarà necessario ha esito positivo. La maggior parte dei casi l'operazione avrà esito positivo al secondo tentativo e sarà possibile recuperare l'errore senza il cliente dovesse essere stata tenere presente che si è verificato un problema.

Esistono diversi modi, è possibile implementare la logica di ripetizione dei tentativi intelligente.

- Microsoft Patterns &amp; gruppo di procedure consigliate ha un [Transient Fault Handling Application Block](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx) che effettua le operazioni necessarie per l'utente se si usa ADO.NET per l'accesso al Database SQL (non tramite Entity Framework). Sufficiente impostare un criterio di ripetizione dei tentativi, quante volte per ripetere una query o comando e il tempo di attesa tra tentativi: e incapsulamento di SQL code in un *usando* blocco.

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    Supporta inoltre TFH [Cache nel ruolo di Azure](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx) e [Bus di servizio](https://azure.microsoft.com/services/service-bus/).
- Quando si usa Entity Framework in genere non si usano direttamente le connessioni SQL, in modo che non è possibile usare questo pacchetto di Microsoft Patterns and Practices, ma Entity Framework 6 si basa questo tipo di logica di ripetizione dei tentativi direttamente nel framework. In modo analogo specificare la strategia di ripetizione dei tentativi ed Entity Framework Usa quindi questa strategia ogni volta che accede al database.

    Per usare questa funzionalità nell'app Fix It, tutti noi dobbiamo fare è aggiungere una classe che deriva da *DbConfiguration* e attivare la logica di ripetizione dei tentativi.

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    Per le eccezioni di Database SQL che consente di identificare il framework come gli errori temporanei in genere, il codice illustrato indica a Entity Framework per ripetere l'operazione fino a 3 volte, con un ritardo esponenziale Backoff tra i tentativi e un ritardo massimo di 5 secondi. Backoff esponenziale significa che dopo ogni tentativo non riuscito attenderà per un periodo più lungo del tempo prima di riprovare. Se tre tentativi in una riga non riescono, verrà generata un'eccezione. La sezione seguente sugli interruttori di circuito spiega il motivo per cui si desidera backoff esponenziale e un numero limitato di ripetizione dei tentativi.

    Quando si usa il servizio di archiviazione di Azure, come l'app Fix It esegue per i BLOB e l'API client di archiviazione .NET implementa già lo stesso tipo di logica, è possibile avere problemi simili. È sufficiente specificare i criteri di ripetizione oppure non è ancora necessario eseguire questa operazione se si è soddisfatti con le impostazioni predefinite.

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>Interruttori di circuito

Esistono diversi motivi per cui non si vuole ripetere troppe volte un determinato periodo troppo lungo:

- Troppi utenti in modo permanente ritentare le richieste non riuscite potrebbero compromettere l'esperienza degli altri utenti. Se milioni di persone sono tutti rendendo ripetuti tentativi delle richieste è possibile bloccare le code di invio IIS e impedendo che l'app per gestire le richieste in caso contrario, grado di gestire correttamente.
- Se tutti gli utenti ritenterà l'esecuzione a causa di un errore del servizio, si potrebbero così tante richieste accodate che il servizio ottiene sovraccarico all'avvio per il ripristino.
- Se l'errore è dovuto alla limitazione ed è presente un intervallo di tempo il servizio Usa per la limitazione delle richieste, vengano fatti tentativi è stato possibile spostare la finestra e causare la limitazione continuare.
- Potrebbe essere un utente in attesa di una pagina web per eseguire il rendering. Making persone attesa troppo lungo potrà essere indesiderate più tale relativamente rapida consulenza per riprovare più tardi.

Backoff esponenziale consente di risolvere alcuni dei problemi, limitando la frequenza dei tentativi di che un servizio può ottenere dall'applicazione. Ma è anche necessario avere *interruttori*: ciò significa che in una determinata ripetere soglia l'app si arresta nuovo tentativo e richiede un'altra azione, ad esempio uno dei seguenti:

- Fallback personalizzato. Se non è possibile ottenere un prezzo azionario da Reuters, forse è possibile ottenerlo dal Bloomberg; o se è Impossibile ottenere dati dal database, forse è possibile ottenerlo dalla cache.
- Esito negativo invisibile all'utente. Se le informazioni necessarie da un servizio non è esclusiva per l'app, semplicemente restituire null se non è possibile ottenere i dati. Se viene visualizzato un'attività Fix It e il servizio Blob non risponde, è possibile visualizzare i dettagli dell'attività senza l'immagine.
- Esito negativo rapido. Errore di disconnessione dell'utente per evitare di sovraccaricare il servizio con ripetere le richieste che potrebbero causare interruzioni del servizio per gli altri utenti o estendere un intervallo di limitazione delle richieste. È possibile visualizzare un messaggio descrittivo "riprovare più tardi".

Non sono presenti criteri di ripetizione dei tentativi universale. È possibile ripetere più volte e il periodo di attesa in un processo di lavoro in background asincrono quanto sarebbe in un'app web sincrono in cui un utente è in attesa di una risposta. È possibile attendere più tra i tentativi per un servizio di database relazionale che si farebbe per un servizio cache. Ecco alcuni esempi consigliati criteri per farsi un'idea di come potrebbero variare i numeri di ripetizione. (Nessun ritardo prima del primo tentativo significa "Fast First".

![Esempi di criteri di ripetizione dei tentativi](transient-fault-handling/_static/image1.png)

Per linee guida dei criteri di ripetizione dei tentativi del Database SQL, vedere [risolvere i problemi di errori di connessione al Database SQL e gli errori temporanei](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/).

## <a name="summary"></a>Riepilogo

Una strategia di ripetizione dei tentativi/Backoff consentono di rendere gli errori temporanei invisibile al cliente la maggior parte dei casi, e Microsoft fornisce un framework che è possibile usare per ridurre al minimo il lavoro che implementa una strategia indipendentemente dall'uso di ADO.NET, Entity Framework o Azure Servizio di archiviazione.

Nel [capitolo successivo](distributed-caching.md), esamineremo come migliorare le prestazioni e affidabilità tramite memorizzazione nella cache distribuita.

## <a name="resources"></a>Risorse

Per altre informazioni, vedere le seguenti risorse:

Documentazione

- [Procedure consigliate per la progettazione di servizi su larga scala nei servizi Cloud di Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). White paper Mark Simms e Michael Thomassy. Simile alla serie di tipo operatore alternativo, ma passa ad analizzare altri dettagli sulle procedure. Vedere la sezione di dati di telemetria e diagnostica.
- [FailSafe: Informazioni aggiuntive per architetture Cloud resilienti](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). White paper Marc Mercuri, Ulrich Homann e Andrew Townhill. Versione della pagina Web della serie di video di operatore alternativo.
- [Microsoft Patterns and Practices - informazioni aggiuntive su Azure](https://msdn.microsoft.com/library/dn568099.aspx). Vedere di ripetizione dei tentativi pattern, il modello di supervisione agente di pianificazione.
- [Tolleranza di errore nel Database SQL di Azure](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx). Post di blog di Tony Petrossian.
- [Entity Framework - resilienza delle connessioni / logica di ripetizione tentativi](https://msdn.microsoft.com/data/dn456835). Come usare e personalizzare la funzionalità di Entity Framework 6 di gestione di errori temporanei.
- [Resilienza della connessione e intercettazione dei comandi con Entity Framework in un'applicazione ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Quarto di una serie di esercitazioni nove parti, viene illustrato come configurare la funzionalità di resilienza di connessione Entity Framework 6 per il Database SQL.

Video

- [Operatore alternativo: Creazione di servizi Cloud scalabili, resilienti](https://channel9.msdn.com/Series/FailSafe). Serie in nove parti Marc Mercuri, Ulrich Homann e Mark Simms. Presenta i concetti generali e principi architetturali in modo estremamente accessibile e interessano, con storie ricavate dall'esperienza di Microsoft Customer Advisory Team (CAT) con i clienti effettivi. Vedere la discussione degli interruttori nell'episodio 3 partire 40:55.
- [Compilazione Big: Lezioni apprese dai clienti di Azure - parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms illustra la progettazione per gli errori, temporanei fault handling e strumentazione di tutti gli elementi.

Esempio di codice

- [Sviluppo di servizi in Azure cloud](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Esempio di applicazione creata in di Microsoft Azure Customer Advisory Team che illustra come usare il [Enterprise Library Transient Fault Handling Block](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH). Per altre informazioni, vedere [livello Cloud Service Fundamentals Data Access: gestione degli errori temporanei](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx). TFH è consigliato per l'accesso al database usando ADO.NET direttamente (senza usare Entity Framework).

> [!div class="step-by-step"]
> [Precedente](monitoring-and-telemetry.md)
> [Successivo](distributed-caching.md)
