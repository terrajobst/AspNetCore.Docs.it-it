---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
title: Registrazione dei dettagli di errori con ELMAH (VB) | Microsoft Docs
author: rick-anderson
description: Errore di registrazione moduli e gestori (ELMAH) offre un altro approccio alla registrazione degli errori di runtime in un ambiente di produzione. ELMAH viene generato un errore gratuito, open source...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: a5f0439f-18b2-4c89-96ab-75b02c616f46
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
msc.type: authoredcontent
ms.openlocfilehash: ed59c6099925a2046d201e0eab0a9afdd620de28
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389202"
---
<a name="logging-error-details-with-elmah-vb"></a>Registrazione dei dettagli di errori con ELMAH (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_vb.pdf)

> Errore di registrazione moduli e gestori (ELMAH) offre un altro approccio alla registrazione degli errori di runtime in un ambiente di produzione. ELMAH è una libreria di registrazione errore open source gratuito che include funzionalità quali funzioni di filtro e la possibilità di visualizzare il log degli errori da una pagina web, come un feed RSS o scaricarlo come file delimitato da virgole. Questa esercitazione viene illustrato il download e la configurazione ELMAH.


## <a name="introduction"></a>Introduzione

Il [esercitazione precedente](logging-error-details-with-asp-net-health-monitoring-vb.md) esaminato ASP. Monitoraggio di sistema, che offre un insufficiente della libreria di finestra per la registrazione di un'ampia gamma di eventi Web dello stato di NET. Molti sviluppatori di usano per accedere e inviare tramite posta elettronica i dettagli delle eccezioni non gestite di monitoraggio dell'integrità. Tuttavia, esistono alcuni punti deboli con questo sistema. Prima di tutto è la mancanza di una sorta di interfaccia utente per la visualizzazione di informazioni sugli eventi registrati. Se si desidera visualizzare un riepilogo degli ultimi 10 errori o visualizzare i dettagli di un errore che si sono verificati ultima settimana, è necessario uno poke attraverso il database, analizzare il messaggio di posta elettronica nella posta in arrivo o creare una pagina web che consente di visualizzare le informazioni dal `aspnet_WebEvent_Events` tabella.

Un altro punto debole coinvolge complessità del monitoraggio dell'integrità. Poiché il monitoraggio dello stato può essere usato per registrare una vasta gamma di eventi diversi, e poiché sono presenti numerose opzioni per indicare come e quando vengono registrati gli eventi, la corretta configurazione del sistema di monitoraggio dell'integrità può essere un'attività onerosa. Infine, esistono problemi di compatibilità. Poiché il monitoraggio dell'integrità prima di tutto è stata aggiunta a .NET Framework versione 2.0, non è disponibile per applicazioni web meno recenti create con la versione ASP.NET 1.x. Inoltre, il `SqlWebEventProvider` (classe), che viene usati nell'esercitazione precedente per i dettagli dell'errore dei log a un database, funziona solo con i database di Microsoft SQL Server. È necessario creare una classe di provider di log personalizzato, se necessario registrare gli errori in un archivio dati alternativo, ad esempio un file XML o un database Oracle.

In alternativa al sistema di monitoraggio dell'integrità è errore di registrazione moduli e gestori (ELMAH), un sistema di registrazione degli errori gratuito e open source creato da [Atif Aziz](http://www.raboof.com/). La differenza più evidente tra i due sistemi è la capacità del ELAMH per visualizzare un elenco di errori e i dettagli di un errore specifico da una pagina web e come un feed RSS. ELMAH è più facile da configurare rispetto perché registra solo gli errori di monitoraggio dell'integrità. Inoltre, ELMAH include il supporto per ASP.NET 1.x, le applicazioni ASP.NET 2.0 e ASP.NET 3.5 e viene fornito con un'ampia gamma di provider di origine di log.

Questa esercitazione illustra i passaggi relativi all'aggiunta ELMAH a un'applicazione ASP.NET. Iniziamo!

> [!NOTE]
> Il monitoraggio di sistema ed ELMAH dello stato di entrambi hanno i propri set di vantaggi e svantaggi. Ti invitiamo a provare entrambi i sistemi e decidere quali uno preferito le proprie esigenze.


## <a name="adding-elmah-to-an-aspnet-web-application"></a>Aggiunta di ELMAH a un'applicazione Web ASP.NET

L'integrazione di ELMAH in un'applicazione nuova o esistente di ASP.NET è un processo semplice e lineare che richiede meno di cinque minuti. In breve, prevede quattro semplici passaggi:

1. Scaricare ELMAH e aggiungere il `Elmah.dll` assembly all'applicazione web,
2. Registrare i moduli HTTP e il gestore in ELMAH `Web.config`,
3. Specificare le opzioni di configurazione del ELMAH, e
4. Creare l'infrastruttura di origine del log di errore, se necessario.

Di seguito viene illustrato ognuno di questi quattro passaggi, una alla volta.

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>Passaggio 1: Scaricare i file di progetto ELMAH e aggiunta`Elmah.dll`all'applicazione Web

ELMAH 1.0 BETA 3 (compilare 10617), la versione più recente al momento della scrittura, è incluso nel download disponibile in questa esercitazione. In alternativa, è possibile visitare il [sito Web ELMAH](https://code.google.com/p/elmah/) per ottenere la versione più recente o per scaricare il codice sorgente. Estrarre il download ELMAH in una cartella sul desktop e individuare il file di assembly ELMAH (`Elmah.dll`).

> [!NOTE]
> Il `Elmah.dll` file si trova il download `Bin` cartella che include le sottocartelle per le diverse versioni di .NET Framework e per le compilazioni di rilascio e di Debug. Usare la build di rilascio per la versione di framework più appropriato. Ad esempio, se si compila un'applicazione web ASP.NET 3.5, copiare il `Elmah.dll` del file dal `Bin\net-3.5\Release` cartella.


Successivamente, aprire Visual Studio e aggiungere l'assembly al progetto facendo clic sul nome del sito Web in Esplora soluzioni e scegliendo Aggiungi riferimento dal menu di scelta rapida. Verrà visualizzata la finestra di dialogo Aggiungi riferimento. Passare alla scheda Sfoglia, quindi scegliere il `Elmah.dll` file. Questa azione aggiunge il `Elmah.dll` file dell'applicazione web `Bin` cartella.

> [!NOTE]
> Il tipo di progetto applicazione Web (WAP) non viene visualizzato il `Bin` cartella in Esplora soluzioni. Elenca invece, questi elementi nella cartella riferimenti.


Il `Elmah.dll` assembly include le classi usate dal sistema ELMAH. Queste classi rientrano in una delle tre categorie:

- **Moduli HTTP** -un modulo HTTP è una classe che definisce i gestori eventi per `HttpApplication` eventi, ad esempio il `Error` evento. ELMAH include più moduli HTTP, quelli più pertinenti tre in corso: 

    - `ErrorLogModule` -vengono registrate le eccezioni non gestite da un'origine di log.
    - `ErrorMailModule` -Invia un messaggio di posta elettronica i dettagli dell'eccezione non gestita.
    - `ErrorFilterModule` -vengono applicati i filtri specificati dallo sviluppatore per determinare i tipi di eccezioni vengono registrate e ciò che quelli vengono ignorati.
- **I gestori HTTP** -un gestore HTTP è una classe che è responsabile della generazione di markup per un particolare tipo di richiesta. ELMAH include i gestori HTTP che i dettagli dell'errore di rendering come pagina web, come un feed RSS o come file delimitato da virgole (CSV).
- **Origini di Log degli errori** - impostazione predefinita ELMAH possono registrare gli errori di memoria, a un database di Microsoft SQL Server, a un database Microsoft Access, a un database Oracle, in un file XML, in un database SQLite, o a un database DB Vista. Ad esempio l'integrità di sistema di monitoraggio, architettura dell'ELMAH è stata creata usando il modello di provider, vale a dire che è possibile creare e integrare facilmente i propri provider di origine di log personalizzato, se necessario.

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>Passaggio 2: Registrazione del ELMAH modulo HTTP e gestore

Mentre il `Elmah.dll` file contiene i moduli HTTP e gestore necessario per registrare automaticamente le eccezioni non gestite e visualizzare i dettagli dell'errore da una pagina web, questi dati devono essere registrati in modo esplicito nella configurazione dell'applicazione web. Il `ErrorLogModule` sottoscrive modulo HTTP, una volta registrato, il `HttpApplication`del `Error` evento. Ogni volta che questo evento viene generato il `ErrorLogModule` registra i dettagli dell'eccezione in un'origine di log specificato. Si vedrà come definire il provider di origine di log nella sezione successiva, "Configurazione ELMAH". Il `ErrorLogPageFactory` factory di gestori HTTP è responsabile della generazione di markup quando si visualizza il log degli errori da una pagina web.

La sintassi specifica per la registrazione dei moduli e gestori HTTP dipende dal server web che alimenta il sito. Il Server di sviluppo ASP.NET e di Microsoft IIS versione 6.0 e versioni precedenti, moduli e gestori HTTP sono registrati nel `<httpModules>` e `<httpHandlers>` sezioni, vengono visualizzati all'interno di `<system.web>` elemento. Se si usa IIS 7.0, devono essere registrati nel `<system.webServer>` dell'elemento `<modules>` e `<handlers>` sezioni. Fortunatamente, è possibile definire i moduli e gestori HTTP nel *entrambi* colloca indipendentemente dal server web in uso. Questa opzione è quella più portabile in quanto consente la stessa configurazione essere usate negli ambienti di sviluppo e produzione indipendentemente dal server web in uso.

Iniziare registrando le `ErrorLogModule` modulo HTTP e la `ErrorLogPageFactory` gestore HTTP nel `<httpModules>` e `<httpHandlers>` sezione `<system.web>`. Se la configurazione già definisce questi due elementi quindi è sufficiente includere il `<add>` (elemento) per modulo HTTP del ELMAH e il gestore.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample1.xml)]

Successivamente, registrare del ELMAH modulo HTTP e il gestore nel `<system.webServer>` elemento. Come in precedenza, se questo elemento non è già presente nella configurazione quindi aggiungerlo.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample2.xml)]

Per impostazione predefinita, IIS 7, visualizza un errore se i moduli e gestori HTTP sono registrati nel `<system.web>` sezione. Il `validateIntegratedModeConfiguration` attributo la `<validation>` elemento indica a IIS 7 per non visualizzare tali messaggi di errore.

Si noti che la sintassi per la registrazione di `ErrorLogPageFactory` gestore HTTP include un `path` attributo, che è impostato su `elmah.axd`. Questo attributo informa l'applicazione web che, se arriva una richiesta per una pagina denominata `elmah.axd` quindi la richiesta deve essere elaborata dal `ErrorLogPageFactory` gestore HTTP. Si vedrà il `ErrorLogPageFactory` gestore HTTP in azione più avanti in questa esercitazione.

### <a name="step-3-configuring-elmah"></a>Passaggio 3: Configurazione ELMAH

ELMAH cerca le opzioni di configurazione del sito Web `Web.config` file in una sezione di configurazione personalizzato denominata `<elmah>`. Per usare una sezione personalizzata in `Web.config` deve essere definita prima nel `<configSections>` elemento. Aprire il `Web.config` file e aggiungere il markup seguente per il `<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample3.xml)]

> [!NOTE]
> Se si sta configurando ELMAH per un'applicazione di ASP.NET 1.x quindi rimuovere il `requirePermission="false"` dell'attributo dal `<section>` elementi precedenti.


La sintassi precedente registra l'oggetto personalizzato `<elmah>` sezione e relativo sottosezioni: `<security>`, `<errorLog>`, `<errorMail>`, e `<errorFilter>`.

Successivamente, aggiungere il `<elmah>` sezione `Web.config`. In questa sezione deve essere visualizzato allo stesso livello di `<system.web>` elemento. All'interno di `<elmah>` sezione aggiungere i `<security>` e `<errorLog>` sezioni in questo modo:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample4.xml)]

Il `<security>` della sezione `allowRemoteAccess` attributo indica se è consentito l'accesso remoto. Se questo valore è impostato su 0, quindi la pagina web log di errore possono essere visualizzati solo in locale. Se questo attributo è impostato su 1 per i visitatori remoti e locali è abilitata la pagina web log di errore. Per il momento, è possibile disabilitare la pagina web log di errore per i visitatori remoti. Scegliamo l'accesso remoto in un secondo momento dopo aver ottenuto l'opportunità di discutere le problematiche di sicurezza di questa operazione.

Il `<errorLog>` sezione definisce l'origine di log di errore, che stabilisce in cui vengono registrati i dettagli dell'errore; è simile al `<providers>` sezione nel sistema di monitoraggio dell'integrità. La sintassi precedente specifica il `SqlErrorLog` come origine del log di errore, che registra gli errori a un database di Microsoft SQL Server specificato dalla classe di `connectionStringName` valore dell'attributo.

> [!NOTE]
> ELMAH viene fornito con provider di log di errore aggiuntive che può essere utilizzato per registrare gli errori in un file XML, un database Microsoft Access, un database Oracle e altri archivi dati. Fare riferimento all'esempio `Web.config` file incluso nel download ELMAH per informazioni su come usare questi provider di log errore alternativo.


### <a name="step-4-creating-the-error-log-source-infrastructure"></a>Passaggio 4: Creare l'infrastruttura di origine di Log di errore

Del ELMAH `SqlErrorLog` provider registra i dettagli dell'errore di un database Microsoft SQL Server specificato. Il `SqlErrorLog` provider prevede che questo database per avere una tabella denominata `ELMAH_Error` e tre stored procedure: `ELMAH_GetErrorsXml`, `ELMAH_GetErrorXml`, e `ELMAH_LogError`. Il download ELMAH include un file denominato `SQLServer.sql` nella `db` cartella che contiene il T-SQL per la creazione di questa tabella e le stored procedure. È necessario eseguire queste istruzioni nel database da utilizzare il `SqlErrorLog` provider.

**Nelle figure 1** e **2** Mostra Esplora Database in Visual Studio dopo gli oggetti di database necessiti per il `SqlErrorLog` provider sono stati aggiunti.

[![](logging-error-details-with-elmah-vb/_static/image2.png)](logging-error-details-with-elmah-vb/_static/image1.png)

**Figura 1**: il `SqlErrorLog` Provider di log degli errori per il `ELMAH_Error` tabella

[![](logging-error-details-with-elmah-vb/_static/image4.png)](logging-error-details-with-elmah-vb/_static/image3.png)

**Figura 2**: il `SqlErrorLog` Provider Usa tre Stored procedure

## <a name="elmah-in-action"></a>ELMAH In azione

A questo punto è stata aggiunta ELMAH all'applicazione web, registrato il `ErrorLogModule` modulo HTTP e la `ErrorLogPageFactory` gestore HTTP, specificare le opzioni di configurazione del ELMAH nel `Web.config`e aggiunti gli oggetti di database necessari per il `SqlErrorLog` provider di log di errore. A questo punto siamo pronti vedere ELMAH in azione. Visitare il sito Web le recensioni dei libri e visitare una pagina che genera un errore di runtime, ad esempio `Genre.aspx?ID=foo`, o una pagina inesistente, ad esempio `NoSuchPage.aspx`. Ciò che viene visualizzato quando si visita le pagine dipende il `<customErrors>` configuration e se si sta visitando localmente o in remoto. (Fare riferimento al [ *visualizzazione di una pagina di errore personalizzata* esercitazione](displaying-a-custom-error-page-vb.md) per un aggiornamento su questo argomento.)

ELMAH non ha effetto sul contenuto che viene visualizzato all'utente quando si verifica un'eccezione non gestita; solo log relativi dettagli. Il log degli errori è accessibile dalla pagina web `elmah.axd` dalla radice del sito Web, ad esempio `http://localhost/BookReviews/elmah.axd`. (Questo file non esiste fisicamente nel progetto, ma quando viene ricevuta una richiesta per `elmah.axd` il runtime lo invia al `ErrorLogPageFactory` gestore HTTP che genera il markup inviato al browser.)

> [!NOTE]
> È anche possibile usare il `elmah.axd` pagina per indicare a ELMAH per generare un errore di test. Visita `elmah.axd/test` (come in `http://localhost/BookReviews/elmah.axd/test`) fa sì che ELMAH generare un'eccezione di tipo `Elmah.TestException`, che include il messaggio di errore: "Questa è un'eccezione di test che può essere tranquillamente ignorata".


**Figura 3** Mostra il log degli errori quando si visita `elmah.axd` dall'ambiente di sviluppo.

[![](logging-error-details-with-elmah-vb/_static/image6.png)](logging-error-details-with-elmah-vb/_static/image5.png)

**Figura 3**: `Elmah.axd` consente di visualizzare il Log degli errori da una pagina Web  
([Fare clic per visualizzare l'immagine con dimensioni normali](logging-error-details-with-elmah-vb/_static/image7.png))

I log degli errori **figura 3** contiene sei voci di errore. Ogni voce include il codice di stato HTTP (404 o 500, per risolvere questi errori), il tipo, la descrizione, il nome dell'utente attualmente connesso al computer quando si è verificato l'errore e la data e ora. Selezionando il collegamento dei dettagli viene visualizzata una pagina che include lo stesso messaggio di errore visualizzato l'errore dettagli giallo schermata di morte (vedere **figura 4**) con i valori delle variabili server al momento dell'errore (vedere  **Figura 5**). È anche possibile visualizzare il XML non elaborato in cui vengono salvati i dettagli dell'errore, che include informazioni aggiuntive, ad esempio i valori nell'intestazione della richiesta HTTP POST.

[![](logging-error-details-with-elmah-vb/_static/image9.png)](logging-error-details-with-elmah-vb/_static/image8.png)

**Figura 4**: visualizzare i dettagli dell'errore YSOD  
([Fare clic per visualizzare l'immagine con dimensioni normali](logging-error-details-with-elmah-vb/_static/image10.png))

[![](logging-error-details-with-elmah-vb/_static/image12.png)](logging-error-details-with-elmah-vb/_static/image11.png)

**Figura 5**: esplorare i valori della raccolta di variabili Server al momento dell'errore  
([Fare clic per visualizzare l'immagine con dimensioni normali](logging-error-details-with-elmah-vb/_static/image13.png))

Distribuzione ELMAH al sito Web di produzione comporta:

- Copia il `Elmah.dll` file per il `Bin` cartella alla produzione,
- Copia le impostazioni di configurazione ELMAH specifiche per il `Web.config` file utilizzato in fase di produzione e
- Aggiunta di infrastruttura di origine dell'errore del log al database di produzione.

Abbiamo esplorato le tecniche per la copia dei file dallo sviluppo alla produzione nelle esercitazioni precedenti. Probabilmente il modo più semplice per ottenere l'infrastruttura di origine di log di errore nel database di produzione è usare SQL Server Management Studio per connettersi al database di produzione e quindi eseguire il `SqlServer.sql` file di script che verrà creata la tabella desiderata e archiviati procedure.

### <a name="viewing-the-error-details-page-on-production"></a>Visualizzazione di pagina dei dettagli di errore in produzione

Dopo aver distribuito il sito alla produzione, visitare il sito Web di produzione e generare un'eccezione non gestita. Come nell'ambiente di sviluppo, ELMAH non ha alcun effetto nella pagina di errore visualizzata quando si verifica un'eccezione non gestita; al contrario, semplicemente registrato l'errore. Se si prova a visitare la pagina log di errore (`elmah.axd`) dall'ambiente di produzione, verrà visualizzata la pagina di accesso negato illustrato nella **figura 6**.

[![](logging-error-details-with-elmah-vb/_static/image15.png)](logging-error-details-with-elmah-vb/_static/image14.png)

**Figura 6**: per impostazione predefinita, i visitatori remoti non è possibile visualizzare la pagina Web Log di errore  
([Fare clic per visualizzare l'immagine con dimensioni normali](logging-error-details-with-elmah-vb/_static/image16.png))

Si tenga presente che la configurazione di ELMAH `<security>` sezione impostiamo il `allowRemoteAccess` 0, che impedisce agli utenti remoti che il log degli errori di visualizzazione dell'attributo. È importante per non consentire i visitatori anonimi da visualizzare il log degli errori, come i dettagli dell'errore potrebbero rivelare le vulnerabilità di sicurezza o altre informazioni riservate. Se si decide di impostare questo attributo su 1 e abilitare l'accesso remoto per il log degli errori, è importante per bloccare il `elmah.axd` path in modo che solo autorizzati i visitatori possono accedervi. Ciò può essere ottenuta mediante l'aggiunta di un `<location>` elemento per il `Web.config` file.

La configurazione seguente consente la trasmissione solo gli utenti del ruolo di amministratore per accedere alla pagina web log di errore:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample5.xml)]

> [!NOTE]
> Sono stati aggiunti il ruolo di amministratore e i tre utenti nel sistema - Scott Jisun e Alice - i [ *configurazione di un sito Web che usa servizi delle applicazioni* esercitazione](configuring-a-website-that-uses-application-services-vb.md). Gli utenti Scott e Jisun sono membri del ruolo di amministratore. Per altre informazioni sull'autenticazione e autorizzazione, fare riferimento a mio [sito Web di sicurezza esercitazioni](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Il log degli errori nell'ambiente di produzione può ora essere visualizzato dagli utenti remoti; Fare riferimento a **figure 3**, **4**, e **5** per gli screenshot della pagina web di log di errore. Tuttavia, se un utente anonimo o senza privilegi di amministratore tenta di visualizzare la pagina di errore del log viene automaticamente reindirizzato alla pagina di accesso (`Login.aspx`), come **figura 7** Mostra.

[![](logging-error-details-with-elmah-vb/_static/image18.png)](logging-error-details-with-elmah-vb/_static/image17.png)

**Figura 7**: gli utenti non autorizzati vengono automaticamente reindirizzati alla pagina di accesso  
([Fare clic per visualizzare l'immagine con dimensioni normali](logging-error-details-with-elmah-vb/_static/image19.png))

### <a name="programmatically-logging-errors"></a>A livello di codice la registrazione degli errori

Del ELMAH `ErrorLogModule` modulo HTTP registra automaticamente le eccezioni non gestite per l'origine di log specificato. In alternativa, è possibile registrare un errore senza la necessità di generare un'eccezione non gestita tramite il `ErrorSignal` classi e i relativi `Raise` (metodo). Il `Raise` viene passato un `Exception` dell'oggetto e lo registra come se tale eccezione è stata generata l'eccezione e ha raggiunto il runtime ASP.NET senza essere stato gestito. La differenza, tuttavia, è che la richiesta continua l'esecuzione in genere dopo la `Raise` metodo è stato chiamato, mentre viene generata un'eccezione non gestita interrompe l'esecuzione normale della richiesta e fa sì che il runtime ASP.NET visualizzare l'applicazione configurata pagina di errore.

Il `ErrorSignal` classe è utile nelle situazioni in cui è presente un'azione che può avere esito negativo, ma esito negativo non è irreversibile per l'operazione globale viene eseguita. Ad esempio, un sito Web può contenere un modulo che accetta l'input dell'utente, archiviarlo in un database e quindi invia un messaggio di posta elettronica che li informa l'utente utilizzato per le informazioni è state elaborate. Cosa dovrebbe accadere se le informazioni vengono salvate correttamente nel database, ma si verifica un errore durante l'invio del messaggio di posta elettronica? Sarebbe un'opzione per generare un'eccezione e l'invio l'utente alla pagina di errore. Tuttavia, ciò potrebbe confondere l'utente a pensare che le informazioni che utente ha immesso non è state salvate. Un altro approccio, è possibile registrare l'errore relativo alla posta elettronica, ma non modificare l'esperienza dell'utente in alcun modo. Si tratta di dove il `ErrorSignal` classe è utile.

[!code-vb[Main](logging-error-details-with-elmah-vb/samples/sample6.vb)]

## <a name="error-notification-via-email"></a>Notifica di errore tramite posta elettronica

Insieme agli errori di registrazione a un database, ELMAH può essere configurato anche per i dettagli dell'errore di posta elettronica a un destinatario specificato. Questa funzionalità viene fornita per il `ErrorMailModule` modulo HTTP; pertanto, è necessario registrare questo modulo HTTP in `Web.config` per inviare i dettagli dell'errore tramite posta elettronica.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample7.xml)]

Successivamente, specificare le informazioni sul messaggio di posta elettronica errore nel `<elmah>` dell'elemento `<errorMail>` sezione, che indica il messaggio di posta elettronica mittente e destinatario, oggetto, e se il messaggio di posta elettronica viene inviato in modo asincrono.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample8.xml)]

Le impostazioni precedenti posto, ogni volta che un errore di runtime si verifica ELMAH invia un messaggio di posta elettronica support@example.com con i dettagli dell'errore. Messaggio di errore dell'ELMAH include le stesse informazioni mostrate in errore dettagli pagina web, vale a dire il messaggio di errore, l'analisi dello stack e le variabili del server (fare riferimento a **figure 4** e **5**). Il messaggio di errore include anche il contenuto dell'eccezione dettagli giallo schermata di morte come un allegato (`YSOD.html`).

**Figura 8** Mostra errore messaggio di posta elettronica dell'ELMAH generato visitando `Genre.aspx?ID=foo`. Sebbene **figura 8** Mostra solo l'errore messaggio e traccia dello stack, le variabili del server sono incluse più avanti nel corpo del messaggio di posta elettronica.

[![](logging-error-details-with-elmah-vb/_static/image21.png)](logging-error-details-with-elmah-vb/_static/image20.png)

**Figura 8**: È possibile configurare ELMAH per inviare i dettagli dell'errore tramite posta elettronica  
([Fare clic per visualizzare l'immagine con dimensioni normali](logging-error-details-with-elmah-vb/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>Solo registrazione degli errori di interesse

Per impostazione predefinita, ELMAH registra i dettagli di ogni eccezione non gestita, nonché altri errori HTTP 404. È possibile indicare ELMAH ignorare questi o altri tipi di errori durante l'uso di funzioni di filtro. La logica di filtro viene eseguita da dell'ELMAH `ErrorFilterModule` modulo HTTP, che è necessario registrare in `Web.config` per poter usare la logica di filtro. Le regole per il filtro vengono specificate nel `<errorFilter>` sezione.

Il markup seguente indica a ELMAH non registrare gli 404 errori.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample9.xml)]

> [!NOTE]
> Non dimenticare, per poter usare funzioni di filtro, è necessario registrare il `ErrorFilterModule` modulo HTTP.


Il `<equal>` elemento all'interno di `<test>` sezione viene definita un'asserzione. Se l'asserzione viene valutata come true l'errore viene filtrato dal log del ELMAH. Sono disponibili altre asserzioni, tra cui: `<greater>`, `<greater-or-equal>`, `<not-equal>`, `<lesser>`, `<lesser-or-equal>`e così via. È anche possibile combinare le asserzioni usando le `<and>` e `<or>` operatori booleani. Inoltre, è possibile anche includere una semplice espressione JavaScript come un'asserzione o scrivere il proprio asserzioni in c# o Visual Basic.

Per altre informazioni sull'errore della ELMAH funzionalità di filtro, vedere la [sezione funzioni di filtro](https://code.google.com/p/elmah/wiki/ErrorFiltering) nel [wiki ELMAH](https://code.google.com/p/elmah/w/list).

## <a name="summary"></a>Riepilogo

ELMAH fornisce un meccanismo semplice ma potente per la registrazione degli errori in un'applicazione web ASP.NET. Come sistema di monitoraggio dell'integrità di Microsoft, ELMAH può registrare errori in un database e può inviare i dettagli dell'errore per gli sviluppatori tramite posta elettronica. A differenza di monitoraggio dell'integrità del sistema, ELMAH include il supporto di finestra per una vasta gamma di archivi di dati del log di errore, inclusi: Microsoft SQL Server, Microsoft Access, Oracle, file XML e altri ancora. Inoltre, ELMAH offre un meccanismo integrato per la visualizzazione dei log degli errori e informazioni dettagliate su un errore specifico da una pagina web, `elmah.axd`. Il `elmah.axd` pagina può anche visualizzare le informazioni sull'errore come un feed RSS o come file delimitati da virgole (CSV), che è possibile leggere usando Microsoft Excel. È anche possibile indicare ELMAH per filtrare gli errori dal log di utilizzo di asserzioni dichiarative o a livello di codice. Ed ELMAH può essere usato con le applicazioni ASP.NET versione 1.x.

Ogni applicazione distribuita deve avere un meccanismo per l'invio di notifiche al team di sviluppo e automaticamente la registrazione delle eccezioni non gestite. Indica se questa operazione viene eseguita tramite il monitoraggio dell'integrità o ELMAH è secondaria. In altre parole, non è rilevante molto che si usi il monitoraggio dell'integrità o ELMAH; valutazione di entrambi i sistemi e quindi scegliere quello più adatto alle proprie esigenze. Che cos'è assolutamente fondamentale è che un meccanismo possibile mettere in atto per registrare le eccezioni non gestite nell'ambiente di produzione.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [ELMAH - errore registrazione moduli e gestori](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [Pagina del progetto ELMAH](https://code.google.com/p/elmah/) (di origine il codice, esempi e wiki)
- [Inserimento ELMAH in un Web dell'applicazione per intercettare le eccezioni non gestite](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx) (video)
- [Pagine di Log di errore di sicurezza](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [Uso di moduli e gestori HTTP per creare componenti ASP.NET collegabili](https://msdn.microsoft.com/library/aa479332.aspx)
- [Sito Web sicurezza esercitazioni](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Precedente](logging-error-details-with-asp-net-health-monitoring-vb.md)
> [Successivo](precompiling-your-website-vb.md)
