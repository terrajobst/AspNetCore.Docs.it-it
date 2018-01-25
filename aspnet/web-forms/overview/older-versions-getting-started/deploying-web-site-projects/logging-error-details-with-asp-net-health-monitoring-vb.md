---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
title: I dettagli dell'errore di registrazione con monitoraggio ASP.NET (VB) | Documenti Microsoft
author: rick-anderson
description: "Sistema di monitoraggio di integrità di Microsoft fornisce un modo semplice e personalizzabile per accedere a vari eventi web, incluse le eccezioni non gestite. In questa esercitazione scheda..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 09a6c74e-936a-4c04-8547-5bb313a4e4a3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
msc.type: authoredcontent
ms.openlocfilehash: 95c0b72e3811dc23f8bdea180be5b20800ab3bd8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="logging-error-details-with-aspnet-health-monitoring-vb"></a>Dettagli di errore di registrazione con monitoraggio ASP.NET (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_vb.pdf)

> Sistema di monitoraggio di integrità di Microsoft fornisce un modo semplice e personalizzabile per accedere a vari eventi web, incluse le eccezioni non gestite. Questa esercitazione viene descritto come configurare il sistema di monitoraggio dello stato per registrare le eccezioni non gestite in un database e per notificare agli sviluppatori tramite un messaggio di posta elettronica.


## <a name="introduction"></a>Introduzione

La registrazione è uno strumento utile per il monitoraggio dello stato di un'applicazione distribuita e per la diagnosi dei problemi che possono verificarsi. È particolarmente importante registrare gli errori che si verificano in un'applicazione distribuita in modo che si può essere risolta. Il `Error` evento viene generato ogni volta che si verifica un'eccezione non gestita in un'applicazione ASP.NET; il [esercitazione precedente](processing-unhandled-exceptions-vb.md) è stato illustrato come inviare una notifica di uno sviluppatore di un errore e i dettagli di log mediante la creazione di un gestore eventi per il `Error` evento. Tuttavia, la creazione di un `Error` gestore eventi per registrare i dettagli dell'errore e inviare una notifica a uno sviluppatore non è necessaria, questa attività può essere eseguita da ASP. Del NET *integrità sistema di monitoraggio*.

L'integrità di sistema di monitoraggio è stato introdotto in ASP.NET 2.0 ed è progettato per monitorare l'integrità di un'applicazione ASP.NET distribuita tramite la registrazione eventi che si verificano durante l'applicazione o la durata della richiesta. Gli eventi registrati dal sistema di monitoraggio dello stato sono detti *gli eventi di monitoraggio dello stato* o *eventi Web*e includono:

- Eventi di durata dell'applicazione, ad esempio quando un'applicazione avvia o arresta
- Eventi di sicurezza, inclusi i tentativi di accesso e non riusciti di richieste di autorizzazione URL
- Errori dell'applicazione, incluse eccezioni non gestite, eccezioni, le eccezioni di convalida richiesta e gli errori di compilazione, tra gli altri tipi di errori di analisi dello stato di visualizzazione.

Quando un evento monitoraggio di integrità possono essere registrato in un numero qualsiasi di specificato *log origini*. Il sistema di monitoraggio dello stato viene fornito con origini di log che vengono registrati eventi Web a un database di Microsoft SQL Server, nel registro eventi di Windows, o tramite un messaggio di posta elettronica, tra gli altri. È anche possibile creare le proprie origini di log.

Gli eventi di log di sistema di monitoraggio dello stato, con le origini di log utilizzate, sono definiti `Web.config`. Con poche righe di codice di configurazione è possibile utilizzare per registrare tutte le eccezioni non gestite in un database e per ricevere una notifica dell'eccezione tramite posta elettronica di monitoraggio dello stato.

## <a name="exploring-the-health-monitoring-systems-configuration"></a>Esplorazione di configurazione del sistema di monitoraggio dello stato

Il comportamento del sistema di monitoraggio dello stato è definito per le informazioni di configurazione, che si trovano nella [ `<healthMonitoring>` elemento](https://msdn.microsoft.com/library/2fwh2ss9.aspx) in `Web.config`. Questa sezione di configurazione definisce i seguenti tre importanti tipi di informazioni:

1. Il monitoraggio di eventi dello stato che, quando generato, devono essere registrate,
2. Le origini di log, e
3. Modalità di mapping di ogni evento definito in (1) sul monitoraggio dello stato per le origini di log definiti nel (2).

Queste informazioni vengono specificate tramite gli elementi di configurazione di tre elementi figlio: [ `<eventMappings>` ](https://msdn.microsoft.com/library/yc5yk01w.aspx), [ `<providers>` ](https://msdn.microsoft.com/library/zaa41kz1.aspx), e [ `<rules>` ](https://msdn.microsoft.com/library/fe5wyxa0.aspx), rispettivamente.

Informazioni di configurazione di sistema di monitoraggio dello stato predefinito, vedere il `Web.config` file `%WINDIR%\Microsoft.NET\Framework\version\CONFIG` cartella. Queste informazioni di configurazione predefinite, con alcuni tag rimosso per ragioni di brevità, sono illustrate di seguito:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample1.xml)]

Lo stato di monitoraggio eventi di interesse vengono definiti nel `<eventMappings>` elemento, che fornisce un nome descrittivo umane in una classe di eventi di monitoraggio dello stato. Nel codice precedente, il `<eventMappings>` elemento assegna il nome descrittivo umane "Tutti gli errori" allo stato di monitoraggio degli eventi del tipo di `WebBaseErrorEvent` e il nome "Failure Audits" per gli eventi del tipo di monitoraggio dello stato `WebFailureAuditEvent`.

Il `<providers>` elemento definisce le origini di log, assegnare loro un nome descrittivo umane e specificando le informazioni di configurazione specifiche dell'origine di log. Il primo `<add>` elemento definisce il provider "EventLogProvider", che esegue il monitoraggio degli eventi di utilizzo dello stato specificato il `EventLogWebEventProvider` classe. La `EventLogWebEventProvider` classe registra l'evento nel registro eventi di Windows. Il secondo `<add>` elemento definisce il provider "SqlWebEventProvider", che registra gli eventi di un database di Microsoft SQL Server tramite la `SqlWebEventProvider` classe. La configurazione "SqlWebEventProvider" Specifica la stringa di connessione del database (`connectionStringName`) tra le altre opzioni di configurazione.

Il `<rules>` gli eventi specificati in mapping dell'elemento di `<eventMappings>` elemento per l'accesso origini il `<providers>` elemento. Per impostazione predefinita, le applicazioni web ASP.NET registrare tutte le eccezioni non gestite e controllare gli errori nel registro eventi di Windows.

## <a name="logging-events-to-a-database"></a>La registrazione eventi a un Database

Configurazione predefinita del sistema di monitoraggio dello stato può essere personalizzato per applicazione web dall'applicazione web tramite l'aggiunta di un `<healthMonitoring>` sezione per l'applicazione `Web.config` file. È possibile includere elementi aggiuntivi di `<eventMappings>`, `<providers>`, e `<rules>` sezioni utilizzando la `<add>` elemento. Per rimuovere un'impostazione dell'utilizzo della configurazione predefinita di `<remove>` elemento oppure utilizzare `<clear />` per rimuovere tutti i valori predefiniti da una di queste sezioni. Si configura l'applicazione web recensioni per registrare le eccezioni di tutti non gestite a un database di Microsoft SQL Server utilizzando il `SqlWebEventProvider` classe.

La `SqlWebEventProvider` classe fa parte del sistema di monitoraggio dello stato e registra un evento a un database di SQL Server specificato di monitoraggio dello stato. Il `SqlWebEventProvider` classe prevede che il database specificato include una stored procedure denominata `aspnet_WebEvent_LogEvent`. Questa stored procedure viene passata i dettagli dell'evento ed è il compito di archiviare i dettagli dell'evento. Buone notizie sono che non è necessario creare la stored procedure né la tabella per archiviare i dettagli dell'evento. È possibile aggiungere questi oggetti nel database mediante il `aspnet_regsql.exe` dello strumento.

> [!NOTE]
> Il `aspnet_regsql.exe` strumento è stato esaminato nel [ *la configurazione di un sito Web che utilizza i servizi delle applicazioni* esercitazione](configuring-a-website-that-uses-application-services-vb.md) quando è stato aggiunto il supporto per ASP. Servizi delle applicazioni di rete. Di conseguenza, il database del sito Web recensioni contiene già il `aspnet_WebEvent_LogEvent` stored procedure, che archivia le informazioni sull'evento in una tabella denominata `aspnet_WebEvent_Events`.


Dopo aver creato la stored procedure necessaria e la tabella aggiunta al database, è comunque per indicare di monitoraggio per registrare tutte le eccezioni non gestite al database. Eseguire questa operazione aggiungendo il seguente markup al sito Web `Web.config` file:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample2.xml)]

Il tag di configurazione precedente viene monitoraggio dello stato `<clear />` elementi da cancellare le informazioni di configurazione di monitoraggio dello stato predefinito di `<eventMappings>`, `<providers>`, e `<rules>` sezioni. Aggiunge quindi una singola voce per ognuna di queste sezioni.

- Il `<eventMappings>` elemento definisce un evento di interesse denominato "Tutti gli errori", che viene generato ogni volta che si verifica un'eccezione non gestita di monitoraggio dello stato singolo.
- Il `<providers>` elemento definisce un'origine singolo registro denominata "SqlWebEventProvider" che usa la `SqlWebEventProvider` classe. Il `connectionStringName` attributo è stato impostato su "ReviewsConnectionString", ovvero il nome di questo tipo di connessione definito nella stringa di `<connectionStrings>` sezione.
- Infine, il &lt;regole&gt; elemento indica che quando un evento di "Tutti gli errori" di seguito che si devono essere registrato utilizzando il provider "SqlWebEventProvider".

Le informazioni di configurazione indica l'integrità di sistema per registrare tutte le eccezioni non gestite al database recensioni di monitoraggio.

> [!NOTE]
> Il `WebBaseErrorEvent` evento viene generato solo per gli errori del server; non viene generato per gli errori HTTP, ad esempio una richiesta per una risorsa ASP.NET che non viene trovata. Questo comportamento è diverso dal comportamento del `HttpApplication` della classe `Error` evento, viene generato per il server e gli errori HTTP.


Per visualizzare l'integrità di sistema nell'azione di monitoraggio, visitare il sito Web e generare un errore di runtime, visitare il sito `Genre.aspx?ID=foo`. È consigliabile vedere la pagina di errore appropriato - eccezione dettagli giallo schermata di morte (durante la visita in locale) o la pagina di errore personalizzato (durante la visita il sito nell'ambiente di produzione). Dietro le quinte, il sistema di monitoraggio dello stato ha registrato le informazioni di errore al database. Dovrebbe esserci un record di `aspnet_WebEvent_Events` tabella (vedere **figura 1**); questo record contiene informazioni che si è verificato l'errore di runtime.

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image1.png)

**Figura 1**: sono stati registrati i dettagli dell'errore per il `aspnet_WebEvent_Events` tabella  
([Fare clic per visualizzare l'immagine ingrandita](logging-error-details-with-asp-net-health-monitoring-vb/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>Visualizzare il Log degli errori In una pagina Web

Con la configurazione corrente del sito Web, l'integrità di sistema di monitoraggio registra tutte le eccezioni non gestite al database. Tuttavia, il monitoraggio dello stato non fornisce alcun meccanismo per visualizzare il log degli errori tramite una pagina web. Tuttavia, è possibile creare una pagina ASP.NET che consente di visualizzare le informazioni dal database. (Come vedremo temporaneamente, è possibile scegliere di disporre dei dettagli sull'errore inviati in un messaggio di posta elettronica.)

Se si crea una pagina, accertarsi di eseguire i passaggi per consentire solo agli utenti autorizzati a visualizzare i dettagli dell'errore. Se il sito utilizza già account utente è possibile utilizzare regole di autorizzazione URL per limitare l'accesso alla pagina per determinati utenti o ruoli. Per ulteriori informazioni su come consentire o limitare l'accesso alle pagine web in base all'utente connesso, fare riferimento a my [esercitazioni di sicurezza del sito Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

> [!NOTE]
> L'esercitazione successiva viene illustrato un sistema di registrazione e la notifica di errore alternativo denominato ELMAH. ELMAH include un meccanismo predefinito per visualizzare il log degli errori da entrambi una pagina web e come un feed RSS.


## <a name="logging-events-to-e-mail"></a>Registrazione di eventi di posta elettronica

L'integrità di sistema di monitoraggio include un provider di origine di log che "Registra" un evento di un messaggio di posta elettronica. L'origine di log include le stesse informazioni che sono connesso al database nel corpo del messaggio di posta elettronica. È possibile utilizzare questa origine di log per segnalare un sviluppatore quando si verifica un determinato evento di monitoraggio di integrità.

Consente di aggiornare le recensioni di configurazione del sito Web in modo che si riceverà un messaggio di posta elettronica ogni volta che un'eccezione si verifica. A tale scopo è necessario eseguire tre operazioni:

1. Configurare l'applicazione web ASP.NET per l'invio di posta elettronica. Questa operazione viene eseguita specificando la modalità di invio di messaggi di posta elettronica tramite il `<system.net>` elemento di configurazione. Per ulteriori informazioni sull'invio di posta elettronica messaggi in un'applicazione ASP.NET si riferiscono a [l'invio di posta elettronica in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx) e [domande frequenti su System.Net.Mail](http://systemnetmail.com/).
2. Registrare il provider di origine di log di posta elettronica nel `<providers>` elemento, e
3. Aggiungere una voce per il `<rules>` elemento che esegue il mapping al provider di origine di log aggiunto nel passaggio (2) l'evento di "Tutti gli errori".

Il sistema di monitoraggio dello stato include due classi di provider dell'origine di log tramite posta elettronica: `SimpleMailWebEventProvider` e `TemplatedMailWebEventProvider`. Il [ `SimpleMailWebEventProvider` classe](https://msdn.microsoft.com/library/system.web.management.simplemailwebeventprovider.aspx) invia un messaggio di posta elettronica con testo normale che l'evento include i dettagli e fornisce poche operazioni di personalizzazione del corpo del messaggio di posta elettronica. Con il [ `TemplatedMailWebEventProvider` classe](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) è specificare una pagina ASP.NET con markup sottoposto a rendering viene utilizzato come corpo del messaggio di posta elettronica. Il [ `TemplatedMailWebEventProvider` classe](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) offre maggiore controllo sul contenuto e formato del messaggio di posta elettronica, ma richiede un po' più lavoro iniziale, è necessario creare la pagina ASP.NET che genera il corpo del messaggio di posta elettronica. In questa esercitazione viene illustrato l'utilizzo di `SimpleMailWebEventProvider` classe.

Aggiornare lo stato sistema di monitoraggio `<providers>` elemento il `Web.config` file da includere un'origine di log per il `SimpleMailWebEventProvider` classe:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample3.xml)]

Il codice precedente utilizza la `SimpleMailWebEventProvider` classe come provider di origine di log e assegna il nome descrittivo "EmailWebEventProvider". Inoltre, il `<add>` attributo include opzioni di configurazione aggiuntive, ad esempio a e da indirizzi di messaggio di posta elettronica.

Con l'origine di log di posta elettronica definito, è comunque per indicare l'integrità di sistema per l'utilizzo di questa origine di "accesso" eccezione non gestita di monitoraggio. Questa operazione viene eseguita aggiungendo una nuova regola di `<rules>` sezione:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample4.xml)]

Il `<rules>` sezione include ora due regole. Il primo, denominato "Tutti gli errori di posta elettronica", l'origine del registro "EmailWebEventProvider" Invia tutte le eccezioni non gestite. Questa regola ha l'effetto dell'invio di dettagli sugli errori nel sito Web all'oggetto specificato all'indirizzo. La regola "Tutti gli errori al Database" Registra i dettagli dell'errore per il database del sito. Di conseguenza, ogni volta che si verifica un'eccezione non gestita nel sito i dettagli sono entrambi connessi al database e inviati all'indirizzo di posta elettronica specificato.

**Figura 2** Mostra messaggio di posta elettronica generato dal `SimpleMailWebEventProvider` classe durante la visita `Genre.aspx?ID=foo`.

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image4.png)

**Figura 2**: il dettagli dell'errore vengono inviati in un messaggio di posta elettronica  
([Fare clic per visualizzare l'immagine ingrandita](logging-error-details-with-asp-net-health-monitoring-vb/_static/image6.png))

## <a name="summary"></a>Riepilogo

Il sistema di monitoraggio dello stato ASP.NET è progettato per consentire agli amministratori di monitorare l'integrità di un'applicazione web distribuita. Gli eventi di monitoraggio di stato vengono generati quando si svolgano determinate azioni, ad esempio quando si arresta l'applicazione, quando un utente accede al sito o quando si verifica un'eccezione non gestita. Questi eventi possono essere registrati in qualsiasi numero di origini di log. In questa esercitazione viene illustrato come registrare i dettagli dell'eccezione non gestita a un database e tramite un messaggio di posta elettronica.

In questa esercitazione è incentrata sull'utilizzo di monitoraggio per registrare le eccezioni non gestite, ma tenere presente che il monitoraggio dello stato è progettato per misurare l'integrità complessiva di un'applicazione ASP.NET distribuita e include una vasta gamma di eventi di monitoraggio dello stato e le origini del log non sono stati illustrati qui. In particolare, è possibile creare la propria integrità monitoraggio gli eventi e origini di log, in caso di necessità si verificano. Se si desidera ottenere ulteriori informazioni sul monitoraggio dello stato, un buon primo passaggio consiste nel leggere [Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)del [domande frequenti sul monitoraggio dello stato](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx). Successivamente, consultare [How To: il monitoraggio dell'integrità di utilizzo di ASP.NET 2.0](https://msdn.microsoft.com/library/ms998306.aspx).

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Cenni preliminari sul monitoraggio dello stato di ASP.NET](https://msdn.microsoft.com/library/bb398933.aspx)
- [Configurare e personalizzare il sistema di ASP.NET di monitoraggio dello stato](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [Domande frequenti - monitoraggio dell'integrità in ASP.NET 2.0](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [Procedura: Inviare posta elettronica per le notifiche di monitoraggio dello stato](https://msdn.microsoft.com/library/ms227553.aspx)
- [Procedura: Utilizzare il monitoraggio dello stato di ASP.NET](https://msdn.microsoft.com/library/ms998306.aspx)
- [Monitoraggio dell'integrità in ASP.NET](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

>[!div class="step-by-step"]
[Precedente](processing-unhandled-exceptions-vb.md)
[Successivo](logging-error-details-with-elmah-vb.md)
