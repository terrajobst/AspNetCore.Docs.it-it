---
title: ASP.NET Core Blazor modelli di hosting
author: guardrex
description: Informazioni sui modelli di hosting Blazor webassembly e Blazor server.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/18/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-models
ms.openlocfilehash: e6ce2be53c35268854e0e8d408b649a8c6ef497e
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658319"
---
# <a name="aspnet-core-opno-locblazor-hosting-models"></a>ASP.NET Core Blazor modelli di hosting

Di [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor è un framework web progettato per l'esecuzione sul lato client nel browser in un Runtime .NET basato su [webassembly](https://webassembly.org/)( *Blazor webassembly*) o sul lato server in ASP.NET Core ( *Blazor server*). Indipendentemente dal modello di hosting, i modelli di app e componenti *sono gli stessi*.

Per creare un progetto per i modelli di hosting descritti in questo articolo, vedere <xref:blazor/get-started>.

Per la configurazione avanzata, vedere <xref:blazor/hosting-model-configuration>.

## <a name="opno-locblazor-webassembly"></a>Blazor webassembly

Il modello di hosting principale per Blazor è in esecuzione sul lato client nel browser del webassembly. L'app Blazor, le relative dipendenze e il Runtime .NET vengono scaricati nel browser. L'app viene eseguita direttamente nel thread dell'interfaccia utente del browser. Gli aggiornamenti dell'interfaccia utente e la gestione degli eventi si verificano nello stesso processo. Le risorse dell'app vengono distribuite come file statici in un server Web o in un servizio in grado di servire contenuto statico ai client.

![Blazor webassembly: l'app Blazor viene eseguita su un thread dell'interfaccia utente all'interno del browser.](hosting-models/_static/blazor-webassembly.png)

Per creare un'app Blazor usando il modello di hosting lato client, usare il modello di **app webassemblyBlazor** ([DotNet New blazorwasm](/dotnet/core/tools/dotnet-new)).

Dopo aver selezionato il modello di **app webassemblyBlazor** , è possibile configurare l'app per l'uso di un back-end ASP.NET Core selezionando la casella di controllo **ASP.NET Core Hosted** ([DotNet New blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)). L'app ASP.NET Core serve l'app Blazor ai client. L'app webassembly Blazor può interagire con il server tramite la rete usando chiamate API Web o [SignalR](xref:signalr/introduction) (<xref:tutorials/signalr-blazor-webassembly>).

I modelli includono lo script `blazor.webassembly.js` che gestisce:

* Download del runtime .NET, dell'app e delle dipendenze dell'app.
* Inizializzazione del runtime per l'esecuzione dell'app.

Il modello di hosting webassembly Blazor offre diversi vantaggi:

* Non esiste alcuna dipendenza lato server .NET. L'app funziona completamente dopo il download nel client.
* Le risorse e le funzionalità client sono completamente sfruttate.
* Il lavoro viene scaricato dal server al client.
* Non è necessario un server Web ASP.NET Core per ospitare l'app. Gli scenari di distribuzione senza server sono possibili, ad esempio per servire l'app da una rete CDN.

Ci sono svantaggi per Blazor l'hosting di webassembly:

* L'app è limitata alle funzionalità del browser.
* È necessario disporre di hardware e software client idonei (ad esempio, il supporto di webassembly).
* Le dimensioni del download sono maggiori e le app importano più tempo per il caricamento.
* Il supporto di runtime e strumenti .NET è meno maturo. Ad esempio, esistono limitazioni in [.NET standard](/dotnet/standard/net-standard) supporto e debug.

## <a name="opno-locblazor-server"></a>Server Blazor

Con il modello di hosting del server di Blazor, l'app viene eseguita nel server dall'interno di un'app ASP.NET Core. Gli aggiornamenti dell'interfaccia utente, la gestione degli eventi e le chiamate JavaScript vengono gestiti tramite una connessione [SignalR](xref:signalr/introduction) .

![il browser interagisce con l'app (ospitata all'interno di un'app ASP.NET Core) nel server tramite una connessione SignalR.](hosting-models/_static/blazor-server.png)

Per creare un'app Blazor usando il modello di hosting del server Blazor, usare il modello di **app ASP.NET CoreBlazor server** ([DotNet New blazorserver](/dotnet/core/tools/dotnet-new)). L'app ASP.NET Core ospita l'app Blazor server e crea l'endpoint SignalR a cui si connettono i client.

L'app ASP.NET Core fa riferimento alla classe `Startup` dell'app per aggiungere:

* Servizi lato server.
* App per la pipeline di gestione delle richieste.

Lo script `blazor.server.js`&dagger; stabilisce la connessione client. È responsabilità dell'app salvare in modo permanente e ripristinare lo stato dell'app come richiesto, ad esempio in caso di perdita di una connessione di rete.

Il modello di hosting del server Blazor offre diversi vantaggi:

* Le dimensioni del download sono notevolmente inferiori rispetto a un'app webassembly Blazor e l'app viene caricata molto più velocemente.
* L'app sfrutta appieno le funzionalità del server, incluso l'uso di tutte le API compatibili con .NET Core.
* .NET Core nel server viene usato per eseguire l'app, in modo che gli strumenti .NET esistenti, ad esempio il debug, funzionino come previsto.
* Sono supportati i thin client. Ad esempio, le app Server Blazor funzionano con i browser che non supportano webassembly e nei dispositivi con vincoli di risorse.
* La codebase .NET/C# code dell'app, incluso il codice componente dell'app, non viene servita ai client.

Ci sono svantaggi per l'hosting di Blazor server:

* In genere esiste una latenza più elevata. Ogni interazione dell'utente comporta un hop di rete.
* Non è disponibile alcun supporto offline. Se la connessione client non riesce, l'app smette di funzionare.
* La scalabilità è complessa per le app con molti utenti. Il server deve gestire più connessioni client e gestire lo stato del client.
* Per gestire l'app, è necessario un server ASP.NET Core. Gli scenari di distribuzione senza server non sono possibili, ad esempio per servire l'app da una rete CDN.

&dagger;lo script di `blazor.server.js` viene servito da una risorsa incorporata nel framework ASP.NET Core Shared.

### <a name="comparison-to-server-rendered-ui"></a>Confronto con interfaccia utente sottoposta a rendering server

Un modo per comprendere Blazor app Server consiste nel comprendere come si differenzia dai modelli tradizionali per il rendering dell'interfaccia utente nelle app ASP.NET Core con visualizzazioni Razor o Razor Pages. Entrambi i modelli utilizzano il linguaggio Razor per descrivere il contenuto HTML, ma presentano differenze significative per il rendering del markup.

Quando viene eseguito il rendering di una pagina o di una visualizzazione Razor, ogni riga di codice Razor emette HTML in formato testo. Dopo il rendering, il server Elimina l'istanza della pagina o della vista, incluso qualsiasi stato prodotto. Quando si verifica un'altra richiesta della pagina, ad esempio quando la convalida del server non riesce e viene visualizzato il riepilogo di convalida:

* Viene nuovamente eseguito il rendering dell'intera pagina in testo HTML.
* La pagina viene inviata al client.

Un'app Blazor è costituita da elementi riutilizzabili di un'interfaccia utente denominata *Components*. Un componente contiene C# codice, markup e altri componenti. Quando viene eseguito il rendering di un componente, Blazor produce un grafico dei componenti inclusi in modo analogo a un HTML o XML Document Object Model (DOM). Questo grafico include lo stato del componente contenuto in proprietà e campi. Blazor valuta il grafico dei componenti per produrre una rappresentazione binaria del markup. Il formato binario può essere:

* Trasformato in testo HTML (durante il prerendering&dagger;).
* Utilizzato per aggiornare in modo efficiente il markup durante il normale rendering.

&dagger;*prerendering* &ndash; il componente Razor richiesto viene compilato nel server in HTML statico e inviato al client, dove viene sottoposto a rendering all'utente. Una volta stattiva la connessione tra il client e il server, gli elementi statici di cui è stato eseguito il rendering sono sostituiti da elementi interattivi. Il prerendering rende l'app più rispondente all'utente.

Un aggiornamento dell'interfaccia utente in Blazor viene attivato da:

* Interazione dell'utente, ad esempio la selezione di un pulsante.
* Trigger dell'app, ad esempio un timer.

Viene eseguito il rendering del grafico e viene calcolata *una differenza dell'interfaccia utente* (differenza). Questa differenza è il set più piccolo di modifiche DOM necessarie per aggiornare l'interfaccia utente nel client. Il diff viene inviato al client in un formato binario e applicato dal browser.

Un componente viene eliminato dopo che l'utente si è spostato dal client. Mentre un utente interagisce con un componente, lo stato del componente (servizi, risorse) deve essere mantenuto nella memoria del server. Poiché lo stato di molti componenti può essere gestito simultaneamente dal server, l'esaurimento della memoria è un problema che deve essere risolto. Per istruzioni su come creare un'app Server Blazor per garantire l'utilizzo ottimale della memoria del server, vedere <xref:security/blazor/server>.

### <a name="circuits"></a>Circuiti

Un'app Server Blazor è basata sull' [SignalRASP.NET Core ](xref:signalr/introduction). Ogni client comunica al server tramite una o più connessioni di SignalR chiamate *circuito*. Un circuito viene Blazorastrazione su SignalR connessioni che possono tollerare interruzioni di rete temporanee. Quando un client Blazor rileva che la connessione SignalR è disconnessa, tenta di riconnettersi al server utilizzando una nuova connessione SignalR.

Ogni schermata del browser (scheda del browser o iframe) connessa a un'app del server Blazor usa una connessione SignalR. Si tratta ancora di un'altra distinzione importante rispetto alle app tipiche sottoposte a rendering server. In un'app sottoposta a rendering server, l'apertura della stessa app in più schermate del browser in genere non viene convertita in richieste di risorse aggiuntive sul server. In un'app Server Blazor, ogni schermata del browser richiede un circuito separato e istanze separate dello stato dei componenti che devono essere gestite dal server.

Blazor considera la chiusura di una scheda del browser o l'esplorazione di un URL esterno a una chiusura *normale* . In caso di chiusura normale, il circuito e le risorse associate vengono immediatamente rilasciati. Un client può anche disconnettersi in modo non corretto, ad esempio a causa di un'interruzione della rete. Blazor Server archivia circuiti disconnessi per un intervallo configurabile in modo da consentire la riconnessione del client.

Blazor Server consente al codice di definire un *gestore di circuito*, che consente l'esecuzione di codice in base alle modifiche apportate allo stato del circuito di un utente. Per altre informazioni, vedere <xref:blazor/advanced-scenarios#blazor-server-circuit-handler>.

### <a name="ui-latency"></a>Latenza interfaccia utente

La latenza dell'interfaccia utente è il tempo necessario da un'azione iniziata al momento dell'aggiornamento dell'interfaccia utente. I valori più bassi per la latenza dell'interfaccia utente sono imperativi perché un'app risponda a un utente. In un'app Server Blazor, ogni azione viene inviata al server, elaborata e viene restituita una diff dell'interfaccia utente. Di conseguenza, la latenza dell'interfaccia utente è la somma della latenza di rete e la latenza del server nell'elaborazione dell'azione.

Per un'app line-of-business limitata a una rete aziendale privata, l'effetto sulle percezioni utente della latenza a causa della latenza di rete è in genere impercettibile. Per un'app distribuita su Internet, la latenza può diventare evidente per gli utenti, in particolare se gli utenti sono ampiamente distribuiti geograficamente.

L'utilizzo della memoria può anche contribuire alla latenza dell'app. Un aumento dell'utilizzo della memoria comporta un frequente Garbage Collection o il paging della memoria su disco, che comportano una riduzione delle prestazioni dell'app e quindi aumentano la latenza dell'interfaccia Per altre informazioni, vedere <xref:security/blazor/server>.

Blazor le app Server devono essere ottimizzate per ridurre al minimo la latenza dell'interfaccia utente riducendo la latenza di rete e l'utilizzo Per un approccio alla misurazione della latenza di rete, vedere <xref:host-and-deploy/blazor/server#measure-network-latency>. Per ulteriori informazioni su SignalR e Blazor, vedere:

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="connection-to-the-server"></a>Connessione al server

Blazor app Server richiedono una connessione SignalR attiva al server. Se la connessione viene persa, l'app tenta di riconnettersi al server. Fino a quando lo stato del client è ancora in memoria, la sessione client riprende senza perdere lo stato.

Viene eseguito il prerendering di un'app Blazor server in risposta alla prima richiesta client, che imposta lo stato dell'interfaccia utente sul server. Quando il client tenta di creare una connessione SignalR, il client deve riconnettersi allo stesso server. Blazor app server che usano più di un server back-end devono implementare *sessioni permanenti* per le connessioni SignalR.

È consigliabile usare il [servizio SignalR di Azure](/azure/azure-signalr) per le app Blazor server. Il servizio consente di aumentare le prestazioni di un'app Server Blazor per un numero elevato di connessioni SignalR simultanee. Le sessioni permanenti sono abilitate per il servizio Azure SignalR impostando l'opzione di `ServerStickyMode` del servizio o il valore di configurazione su `Required`. Per altre informazioni, vedere <xref:host-and-deploy/blazor/server#signalr-configuration>.

Quando si usa IIS, le sessioni permanenti sono abilitate con il routing delle richieste di applicazioni. Per altre informazioni, vedere [bilanciamento del carico http usando il routing delle richieste di applicazioni](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:blazor/get-started>
* <xref:signalr/introduction>
* <xref:blazor/hosting-model-configuration>
* <xref:tutorials/signalr-blazor-webassembly>
