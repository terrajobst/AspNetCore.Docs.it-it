---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs
title: Registrazione dei dettagli di errore con ASP.NET di monitoraggio (c#) | Microsoft Docs
author: rick-anderson
description: Sistema di monitoraggio dell'integrità di Microsoft fornisce un modo semplice e personalizzabile per registrare eventi web diversi, tra cui le eccezioni non gestite. Questa esercitazione illustra in modo through...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: b1abb452-642a-4ff3-8504-37b85590ff79
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs
msc.type: authoredcontent
ms.openlocfilehash: 73b78b99f68dc32f37bcd16865091deebdb1399a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363481"
---
<a name="logging-error-details-with-aspnet-health-monitoring-c"></a>Registrazione dei dettagli di errore con ASP.NET di monitoraggio (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_cs.pdf)

> Sistema di monitoraggio dell'integrità di Microsoft fornisce un modo semplice e personalizzabile per registrare eventi web diversi, tra cui le eccezioni non gestite. Questa esercitazione spiega come impostare il sistema di monitoraggio dello stato per registrare le eccezioni non gestite in un database e per notificare agli sviluppatori tramite un messaggio di posta elettronica.


## <a name="introduction"></a>Introduzione

La registrazione è uno strumento utile per il monitoraggio dell'integrità di un'applicazione distribuita e per la diagnosi di eventuali problemi che potrebbero verificarsi. È particolarmente importante registrare gli errori che si verificano in un'applicazione distribuita in modo che si può essere risolta. Il `Error` evento viene generato ogni volta che si verifica un'eccezione non gestita in un'applicazione ASP.NET; la [esercitazione precedente](processing-unhandled-exceptions-cs.md) è stato illustrato come inviare una notifica di uno sviluppatore di un errore e i dettagli di log mediante la creazione di un gestore eventi per il `Error` evento. Tuttavia, la creazione di un `Error` gestore dell'evento per registrare i dettagli dell'errore e inviare una notifica di uno sviluppatore non è necessaria, poiché questa attività può essere eseguita da ASP. Di NET *integrità sistema di monitoraggio*.

Sistema di monitoraggio dell'integrità è stato introdotto in ASP.NET 2.0 ed è progettato per monitorare l'integrità di un'applicazione ASP.NET distribuita per la registrazione eventi che si verificano durante l'applicazione o la durata della richiesta. Gli eventi registrati dal sistema di monitoraggio dell'integrità sono dette *gli eventi di monitoraggio dell'integrità* oppure *eventi Web*e includono:

- Eventi di durata dell'applicazione, ad esempio quando un'applicazione avvia o arresta
- Gli eventi di sicurezza, inclusi i tentativi di accesso non riusciti e URL autorizzazione richieste non riuscite
- Errori dell'applicazione, tra cui eccezioni non gestite, lo stato di visualizzazione delle eccezioni, le eccezioni della convalida richiesta e gli errori di compilazione, tra gli altri tipi di errori di analisi.

Quando un evento monitoraggio dell'integrità possono essere registrato in un numero qualsiasi di specificata *registrare origini*. Sistema di monitoraggio dell'integrità viene fornito con le origini di log che registrano eventi Web in un database di Microsoft SQL Server, nel registro eventi di Windows, o tramite un messaggio di posta elettronica, tra gli altri. È anche possibile creare le proprie origini di log.

Gli eventi registra l'integrità di sistema di monitoraggio, con le origini di log usate, sono definiti `Web.config`. Con poche righe di markup di configurazione è possibile usare per registrare tutte le eccezioni non gestite in un database e per ricevere una notifica dell'eccezione tramite posta elettronica di monitoraggio dell'integrità.

## <a name="exploring-the-health-monitoring-systems-configuration"></a>Esplorazione di configurazione del sistema di monitoraggio dell'integrità

Comportamento del sistema di monitoraggio dell'integrità è definito per le informazioni di configurazione, che si trovano nella [ `<healthMonitoring>` elemento](https://msdn.microsoft.com/library/2fwh2ss9.aspx) in `Web.config`. Questa sezione di configurazione definisce i seguenti tre importanti tipi di informazioni:

1. Gli eventi di monitoraggio dell'integrità che, quando generati, devono essere registrate,
2. Le origini di log, e
3. Modalità di mapping ciascun monitoraggio evento definito in (1) dell'integrità per le origini di log definito (2).

Queste informazioni vengono specificate tramite gli elementi di configurazione di tre membri figlio: [ `<eventMappings>` ](https://msdn.microsoft.com/library/yc5yk01w.aspx), [ `<providers>` ](https://msdn.microsoft.com/library/zaa41kz1.aspx), e [ `<rules>` ](https://msdn.microsoft.com/library/fe5wyxa0.aspx), rispettivamente.

Le informazioni di configurazione di sistema di monitoraggio dell'integrità predefinita è reperibile nella `Web.config` del file in `%WINDIR%\Microsoft.NET\Framework\version\CONFIG` cartella. Queste informazioni di configurazione predefinito, con alcuni tag rimossi per brevità, sono illustrate di seguito:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample1.xml)]

Lo stato di integrità monitoraggio eventi di interesse sono definiti nel `<eventMappings>` elemento, che assegna un nome Converto in una classe di eventi di monitoraggio dell'integrità. Nel markup riportato sopra, il `<eventMappings>` elemento assegna il nome Converto "Tutti gli errori" per gli eventi del tipo di monitoraggio dell'integrità `WebBaseErrorEvent` e il nome "operazioni non riuscite" per gli eventi del tipo di monitoraggio dell'integrità `WebFailureAuditEvent`.

Il `<providers>` elemento definisce le origini di log, concedere loro un nome Converto e specificando le informazioni di configurazione specifiche dell'origine di log. Il primo `<add>` elemento definisce il provider "EventLogProvider", che registra gli eventi di utilizzo di monitoraggio dell'integrità specificato il `EventLogWebEventProvider` classe. Il `EventLogWebEventProvider` classe registra l'evento nel registro eventi di Windows. La seconda `<add>` elemento definisce il provider "SqlWebEventProvider", che registra gli eventi di un database di Microsoft SQL Server tramite il `SqlWebEventProvider` classe. La configurazione "SqlWebEventProvider" Specifica la stringa di connessione del database (`connectionStringName`) tra le altre opzioni di configurazione.

Il `<rules>` gli eventi specificati nel mapping dell'elemento il `<eventMappings>` elemento per l'accesso origini la `<providers>` elemento. Per impostazione predefinita, le applicazioni web ASP.NET registrare tutte le eccezioni non gestite e controllare gli errori nel registro eventi di Windows.

## <a name="logging-events-to-a-database"></a>Eventi di registrazione per un Database

Configurazione predefinita del sistema di monitoraggio dell'integrità possono essere personalizzati in base dell'applicazione web dall'applicazione web mediante l'aggiunta di un `<healthMonitoring>` sezione per l'applicazione `Web.config` file. È possibile includere elementi aggiuntivi il `<eventMappings>`, `<providers>`, e `<rules>` sezioni utilizzando la `<add>` elemento. Per rimuovere un'impostazione dalla configurazione predefinita, usare il `<remove>` elemento oppure utilizzare `<clear />` per rimuovere tutti i valori predefiniti da una di queste sezioni. È possibile configurare l'applicazione web le recensioni dei libri per registrare le eccezioni non gestite tutte in un database di Microsoft SQL Server usando il `SqlWebEventProvider` classe.

Il `SqlWebEventProvider` classe fa parte del sistema di monitoraggio dell'integrità e registra un evento a un database di SQL Server specificato di monitoraggio dello stato. Il `SqlWebEventProvider` classe si aspetta che il database specificato include una stored procedure denominata `aspnet_WebEvent_LogEvent`. Questa stored procedure verrà passata i dettagli dell'evento e viene assegnato il compito con l'archiviazione dei dettagli dell'evento. La buona notizia è che non è necessaria creare questa stored procedure né la tabella per archiviare i dettagli dell'evento. È possibile aggiungere questi oggetti al database tramite il `aspnet_regsql.exe` dello strumento.

> [!NOTE]
> Il `aspnet_regsql.exe` strumento è stato esaminato nel [ *configurazione di un sito Web che usa servizi delle applicazioni* esercitazione](configuring-a-website-that-uses-application-services-cs.md) quando è stato aggiunto il supporto per ASP. Servizi delle applicazioni di rete. Di conseguenza, il database del sito Web le recensioni dei libri già contiene il `aspnet_WebEvent_LogEvent` stored procedure, che archivia le informazioni sull'evento in una tabella denominata `aspnet_WebEvent_Events`.


Dopo aver creato la stored procedure necessaria e la tabella aggiunta al database, è ora indicare per registrare tutte le eccezioni non gestite nel database di monitoraggio dell'integrità. Eseguire questa operazione aggiungendo il markup seguente nel sito Web `Web.config` file:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample2.xml)]

Markup di configurazione precedente Usa di monitoraggio dell'integrità `<clear />` elementi da cancellare le informazioni di configurazione di monitoraggio dell'integrità predefinite di `<eventMappings>`, `<providers>`, e `<rules>` sezioni. Aggiunge quindi una singola voce per ognuna di queste sezioni.

- Il `<eventMappings>` elemento definisce un singolo evento di interesse denominato "Tutti gli errori", che viene generato ogni volta che si verifica un'eccezione non gestita di monitoraggio dello stato.
- Il `<providers>` elemento definisce un'origine singola log denominata "SqlWebEventProvider" che usa il `SqlWebEventProvider` classe. Il `connectionStringName` attributo è stato impostato su "ReviewsConnectionString", ovvero il nome del nostro connessione definito nella stringa di `<connectionStrings>` sezione.
- Infine, il &lt;regole&gt; elemento indica che quando un evento di "Tutti gli errori" accade realmente che si devono essere registrato con provider "SqlWebEventProvider".

Le informazioni di configurazione indica l'integrità di sistema per registrare tutte le eccezioni non gestite nel database le recensioni dei libri di monitoraggio.

> [!NOTE]
> Il `WebBaseErrorEvent` evento viene generato solo per gli errori del server; non viene generato per gli errori HTTP, ad esempio una richiesta per una risorsa di ASP.NET che non viene trovata. Questo comportamento è diverso dal comportamento dei `HttpApplication` della classe `Error` evento, che viene generato per il server e gli errori HTTP.


Per visualizzare l'integrità di sistema nell'azione di monitoraggio, visitare il sito Web e generare un errore di runtime visitando `Genre.aspx?ID=foo`. È consigliabile vedere la pagina di errore appropriato: eccezione dettagli giallo schermata di morte (quando si visita in locale) o la pagina di errore personalizzata (quando si visita il sito nell'ambiente di produzione). Dietro le quinte, sistema di monitoraggio dell'integrità registrate le informazioni sull'errore nel database. Deve essere presente un record di `aspnet_WebEvent_Events` tabella (vedere **figura 1**); questo record contiene informazioni sull'errore di runtime che si è verificato.

[![](logging-error-details-with-asp-net-health-monitoring-cs/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-cs/_static/image1.png)

**Figura 1**: sono stati registrati i dettagli dell'errore di `aspnet_WebEvent_Events` tabella  
([Fare clic per visualizzare l'immagine con dimensioni normali](logging-error-details-with-asp-net-health-monitoring-cs/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>Visualizzare il Log degli errori In una pagina Web

Con la configurazione corrente del sito Web, l'integrità di sistema di monitoraggio registra tutte le eccezioni non gestite nel database. Tuttavia, il monitoraggio dello stato non fornisce alcun meccanismo per visualizzare il log degli errori tramite una pagina web. Tuttavia, è possibile creare una pagina ASP.NET che consente di visualizzare queste informazioni dal database. (Come si vedrà momentaneamente, è possibile scegliere di memorizzare i dettagli dell'errore inviati all'utente un messaggio di posta elettronica.)

Se si crea tale pagina, accertarsi di eseguire questa procedura per consentire solo agli utenti autorizzati a visualizzare i dettagli dell'errore. Se il sito Usa già gli account utente è possibile usare regole di autorizzazione URL per limitare l'accesso alla pagina per determinati utenti o ruoli. Per altre informazioni su come concedere o limitare l'accesso alle pagine web basate sull'utente connesso, fare riferimento a mio [sito Web di sicurezza esercitazioni](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

> [!NOTE]
> L'esercitazione successiva illustra un sistema di registrazione e la notifica di errore alternativo denominato ELMAH. ELMAH include un meccanismo predefinito per visualizzare il log degli errori da entrambi una pagina web e come un feed RSS.


## <a name="logging-events-to-email"></a>La registrazione di eventi messaggio di posta elettronica

L'integrità di sistema di monitoraggio include un provider di origine di log che "Registra" un evento di un messaggio di posta elettronica. L'origine di log include le stesse informazioni che vengano eseguito l'accesso al database nel corpo del messaggio di posta elettronica. È possibile usare questa origine di log per notificare a uno sviluppatore quando si verifica un determinato evento di monitoraggio dell'integrità.

È possibile aggiornare la configurazione del sito Web in modo che si riceva un messaggio di posta elettronica ogni volta che un'eccezione si verifica le recensioni dei libri. A tale scopo è necessario eseguire tre attività:

1. Configurare l'applicazione web ASP.NET per l'invio di posta elettronica. Ciò avviene specificando la modalità di invio messaggi di posta elettronica tramite il `<system.net>` elemento di configurazione. Per altre informazioni sull'invio di posta elettronica dei messaggi in un'applicazione ASP.NET si intende [l'invio di posta elettronica in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx) e il [domande frequenti su System.NET. Mail](http://systemnetmail.com/).
2. Registrare il provider di origine di log tramite posta elettronica nel `<providers>` elemento, e
3. Aggiungere una voce per il `<rules>` elemento che viene eseguito il mapping dell'evento di "Tutti gli errori" per il provider di origine di log aggiunto nel passaggio (2).

Sistema di monitoraggio dell'integrità include due classi di provider dell'origine di log tramite posta elettronica: `SimpleMailWebEventProvider` e `TemplatedMailWebEventProvider`. Il [ `SimpleMailWebEventProvider` classe](https://msdn.microsoft.com/library/system.web.management.simplemailwebeventprovider.aspx) invia un messaggio di posta elettronica di testo che include l'evento illustra in dettaglio e offre poche operazioni di personalizzazione del corpo del messaggio di posta elettronica. Con il [ `TemplatedMailWebEventProvider` classe](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) si specifica una pagina ASP.NET cui markup sottoposto a rendering viene utilizzato come il corpo del messaggio di posta elettronica. Il [ `TemplatedMailWebEventProvider` classe](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) offre maggiore controllo sui contenuti e formato di messaggio di posta elettronica, ma richiede maggiore impegno iniziale perché è necessario creare la pagina ASP.NET che genera il corpo del messaggio di posta elettronica. Questa esercitazione è incentrata sull'uso di `SimpleMailWebEventProvider` classe.

Aggiornamento del sistema di monitoraggio dell'integrità `<providers>` elemento il `Web.config` file da includere un'origine di log per il `SimpleMailWebEventProvider` classe:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample3.xml)]

Il markup riportato sopra Usa la `SimpleMailWebEventProvider` classe come provider di origine di log e le assegna il nome descrittivo "EmailWebEventProvider". Inoltre, il `<add>` attributo include opzioni di configurazione aggiuntive, ad esempio a e dagli indirizzi del messaggio di posta elettronica.

Con l'origine di log di posta elettronica definito, resta che per indicare l'integrità di sistema per usare questa origine per "registrare" eccezioni non gestite di monitoraggio. Questa operazione viene eseguita mediante l'aggiunta di una nuova regola nel `<rules>` sezione:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample4.xml)]

Il `<rules>` sezione include ora due regole. Il primo webhook denominato "Tutti gli errori per posta elettronica", invia tutte le eccezioni non gestite per l'origine di log "EmailWebEventProvider". Questa regola ha l'effetto di inviare i dettagli sugli errori sul sito Web specificato all'indirizzo. La regola di "Tutti gli errori al Database" Registra i dettagli dell'errore per il database del sito. Di conseguenza, ogni volta che si verifica un'eccezione non gestita nel sito i dettagli sono entrambi connessi al database e inviati all'indirizzo di posta elettronica specificato.

**Figura 2** Mostra il messaggio di posta elettronica generato dal `SimpleMailWebEventProvider` classe quando si visita `Genre.aspx?ID=foo`.

[![](logging-error-details-with-asp-net-health-monitoring-cs/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-cs/_static/image4.png)

**Figura 2**: The i dettagli dell'errore vengono inviati in un messaggio di posta elettronica  
([Fare clic per visualizzare l'immagine con dimensioni normali](logging-error-details-with-asp-net-health-monitoring-cs/_static/image6.png))

## <a name="summary"></a>Riepilogo

Il sistema di monitoraggio dell'integrità ASP.NET è progettato per consentire agli amministratori di monitorare l'integrità di un'applicazione web distribuita. Espandi determinate azioni, ad esempio quando si arresta l'applicazione, quando un utente accede correttamente al sito, oppure se si verifica un'eccezione non gestita, vengono generati eventi di monitoraggio dell'integrità. Questi eventi possono essere registrati in un numero qualsiasi di origini di log. Questa esercitazione ha illustrato come registrare i dettagli delle eccezioni non gestite in un database e tramite un messaggio di posta elettronica.

Questa esercitazione è incentrato sull'uso per registrare le eccezioni non gestite, ma tenere presente che il monitoraggio dell'integrità è quello di misurare l'integrità complessiva di un'applicazione ASP.NET distribuita e include un'ampia gamma di eventi di monitoraggio dell'integrità e del log non origini di monitoraggio dell'integrità sono stati illustrati di seguito. Inoltre, è possibile creare il proprio monitoraggio origini log ed eventi dello stato in caso di necessità si verificano. Se è interessati a ottenere ulteriori informazioni su monitoraggio dell'integrità, un buon primo passo è leggere con attenzione [Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)del [domande frequenti sul monitoraggio dell'integrità](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx). In seguito, consultare [How To: il monitoraggio dell'integrità di utilizzo in ASP.NET 2.0](https://msdn.microsoft.com/library/ms998306.aspx).

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Panoramica di monitoraggio dell'integrità di ASP.NET](https://msdn.microsoft.com/library/bb398933.aspx)
- [Configurazione e la personalizzazione del sistema di ASP.NET di monitoraggio dell'integrità](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [Domande frequenti - monitoraggio dell'integrità in ASP.NET 2.0](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [Procedura: Inviare posta elettronica per le notifiche di monitoraggio dell'integrità](https://msdn.microsoft.com/library/ms227553.aspx)
- [Procedura: Usare il monitoraggio dello stato in ASP.NET](https://msdn.microsoft.com/library/ms998306.aspx)
- [Monitoraggio dell'integrità in ASP.NET](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

> [!div class="step-by-step"]
> [Precedente](processing-unhandled-exceptions-cs.md)
> [Successivo](logging-error-details-with-elmah-cs.md)
