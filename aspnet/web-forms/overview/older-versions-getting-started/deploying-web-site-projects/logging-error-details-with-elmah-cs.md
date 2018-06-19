---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-cs
title: I dettagli dell'errore di registrazione con ELMAH (c#) | Documenti Microsoft
author: rick-anderson
description: Errore di registrazione moduli e gestori (ELMAH) offre un altro approccio per la registrazione degli errori di runtime in un ambiente di produzione. ELMAH è verificato un errore di open source gratuito...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 11f6fe44-64ef-4a38-a3b4-35c7bb992352
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-cs
msc.type: authoredcontent
ms.openlocfilehash: cd91c745624f09d01a326a445bea2bb756576688
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2018
ms.locfileid: "30890619"
---
<a name="logging-error-details-with-elmah-c"></a>Dettagli di errore di registrazione con ELMAH (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_cs.pdf)

> Errore di registrazione moduli e gestori (ELMAH) offre un altro approccio per la registrazione degli errori di runtime in un ambiente di produzione. ELMAH è una raccolta di registrazione errore open source gratuito che include funzionalità quali l'applicazione di filtri di errore e la possibilità di visualizzare il log degli errori da una pagina web, come un feed RSS, o per il download come file delimitato da virgole. Questa esercitazione viene descritto il download e la configurazione ELMAH.


## <a name="introduction"></a>Introduzione

Il [esercitazione precedente](logging-error-details-with-asp-net-health-monitoring-cs.md) esaminate ASP. Monitoraggio del sistema, che offre un timeout della libreria di casella per la registrazione di un'ampia gamma di eventi Web dello stato della rete. Molti sviluppatori di utilizzano per accedere e inviare tramite posta elettronica i dettagli dell'eccezione non gestita di monitoraggio dello stato. Tuttavia, esistono alcuni punti deboli con questo sistema. Prima di tutto è la mancanza di qualsiasi tipo di interfaccia utente per visualizzare informazioni sugli eventi registrati. Se si desidera visualizzare un riepilogo degli ultimi 10 errori o visualizzare i dettagli di un errore che si è verificato l'ultima settimana, è necessario che entrambi poke tramite il database, analizzare il messaggio di posta elettronica in arrivo o creare una pagina web che visualizza informazioni dal `aspnet_WebEvent_Events` tabella.

Un altro punto debole coinvolge dalla complessità del monitoraggio dell'integrità. Poiché il monitoraggio dello stato può essere utilizzato per registrare eventi diversi numerosissime e perché esistono numerose opzioni per indicare come e quando vengono registrati gli eventi, la corretta configurazione di sistema di monitoraggio dello stato può essere un'attività onerosa. Infine, sono presenti problemi di compatibilità. Poiché il monitoraggio dello stato è stata prima aggiunta a .NET Framework versione 2.0, non è disponibile per le applicazioni web meno recenti create con la versione ASP.NET 1. x. Inoltre, il `SqlWebEventProvider` (classe), che è utilizzati nell'esercitazione precedente per i dettagli di errore registri per un database, funziona solo con i database di Microsoft SQL Server. È necessario creare una classe di provider di log personalizzato, è necessario registrare gli errori in un archivio di dati alternativi, ad esempio un file XML o di un database Oracle.

Un'alternativa per il sistema di monitoraggio dello stato è errore registrazione moduli e gestori (ELMAH), creato da un sistema di registrazione degli errori gratuita e open source [Atif Aziz](http://www.raboof.com/). La differenza più evidente tra i due sistemi è la possibilità di ELAMH per visualizzare un elenco di errori e i dettagli di un errore specifico da una pagina web e come un feed RSS. ELMAH è più facile da configurare rispetto perché registra solo gli errori di monitoraggio dello stato. Inoltre, ELMAH include il supporto per ASP.NET 1. x, le applicazioni ASP.NET 2.0 e ASP.NET 3.5 e viene fornito con diversi provider di origine di log.

In questa esercitazione vengono illustrati i passaggi per aggiungere ELMAH a un'applicazione ASP.NET. Iniziamo!

> [!NOTE]
> Monitoraggio di sistema ed ELMAH hanno i propri set di vantaggi e svantaggi. Ti suggeriamo di provare a entrambi i sistemi e decidere quali uno preferito le proprie esigenze.


## <a name="adding-elmah-to-an-aspnet-web-application"></a>Aggiunta di ELMAH a un'applicazione Web ASP.NET

Integrazione ELMAH in un'applicazione ASP.NET di nuovo o esistente è un processo semplice e lineare che richiede meno di cinque minuti. In pratica, e prevede quattro semplici passaggi:

1. Scaricare ELMAH e aggiungere il `Elmah.dll` assembly all'applicazione web
2. Registrare i moduli HTTP del ELMAH e gestore in `Web.config`,
3. Specificare le opzioni di configurazione del ELMAH, e
4. Creare l'infrastruttura di origine del log di errore, se necessario.

Si analizzerà ognuno di questi quattro passaggi, uno alla volta.

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>Passaggio 1: Scaricare i file di progetto ELMAH e l'aggiunta di`Elmah.dll`all'applicazione Web

ELMAH 1.0 BETA 3 (compilare 10617), la versione più recente al momento della scrittura, è incluso nel download disponibile in questa esercitazione. In alternativa, è possibile visitare il [sito Web ELMAH](https://code.google.com/p/elmah/) per ottenere la versione più recente o per scaricare il codice sorgente. Estrarre il download ELMAH a una cartella sul desktop e individuare il file di assembly ELMAH (`Elmah.dll`).

> [!NOTE]
> Il `Elmah.dll` file si trova il download `Bin` cartella che include le sottocartelle per le diverse versioni di .NET Framework e per le compilazioni di Debug e rilascio. Utilizzare la build di rilascio per la versione di framework appropriato. Ad esempio, se si compila un'applicazione web ASP.NET 3.5, copiare il `Elmah.dll` file dal `Bin\net-3.5\Release` cartella.


Successivamente, aprire Visual Studio e aggiungere l'assembly al progetto facendo clic sul nome del sito Web in Esplora soluzioni e scegliendo Aggiungi riferimento dal menu di scelta rapida. Verrà visualizzata la finestra di dialogo Aggiungi riferimento. Passare alla scheda Sfoglia, quindi scegliere il `Elmah.dll` file. Questa azione aggiunge il `Elmah.dll` file dell'applicazione web `Bin` cartella.

> [!NOTE]
> Il tipo di progetto applicazione Web (WAP) non viene visualizzato il `Bin` cartella in Esplora soluzioni. Elenca, invece, questi elementi nella cartella riferimenti.


Il `Elmah.dll` assembly include le classi utilizzate dal sistema ELMAH. Queste classi rientrano in una delle tre categorie:

- **I moduli HTTP** -un modulo HTTP è una classe che definisca i gestori eventi per `HttpApplication` degli eventi, ad esempio il `Error` evento. ELMAH include più moduli HTTP, quelli più pertinenti tre da: 

    - `ErrorLogModule` -Registra le eccezioni non gestite in un'origine di log.
    - `ErrorMailModule` -Invia i dettagli dell'eccezione non gestita in un messaggio di posta elettronica.
    - `ErrorFilterModule` -Applica i filtri specificati dallo sviluppatore per determinare quali eccezioni vengono registrate e cosa quelle vengono ignorati.
- **I gestori HTTP** -un gestore HTTP è una classe che è responsabile della generazione il markup per un particolare tipo di richiesta. ELMAH include i gestori HTTP che eseguono il rendering i dettagli dell'errore, come una pagina web, come un feed RSS o come file delimitato da virgole (CSV).
- **Le origini di Log degli errori** - quella ELMAH può registrare errori di memoria, in un database di Microsoft SQL Server, a un database Microsoft Access, per un database Oracle, in un file XML, a un database SQLite o a un database DB Vista. Ad esempio l'integrità di sistema di monitoraggio, architettura del ELMAH è stato compilato utilizzando il modello di provider, vale a dire che è possibile creare e integrare facilmente i propri provider di origine di log personalizzato, se necessario.

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>Passaggio 2: Registrazione del ELMAH modulo HTTP e gestore

Mentre il `Elmah.dll` file contiene i moduli HTTP e gestore necessario per registrare automaticamente le eccezioni non gestite e di visualizzare i dettagli dell'errore da una pagina web, questi dati devono essere registrati in modo esplicito nella configurazione dell'applicazione web. Il `ErrorLogModule` modulo HTTP, una volta registrato, sottoscrive i `HttpApplication`del `Error` evento. Ogni volta che questo evento viene generato il `ErrorLogModule` registra i dettagli dell'eccezione di un'origine di log specificato. Vedremo come definire il provider di origine di log nella sezione successiva, "Configurazione ELMAH". Il `ErrorLogPageFactory` factory del gestore HTTP è responsabile della generazione di codice quando si visualizza il log degli errori da una pagina web.

La sintassi specifica per la registrazione di gestori e i moduli HTTP dipende dal server web che è il sito di accensione. Per il Server di sviluppo ASP.NET e di Microsoft IIS 6.0 e versioni precedenti, i moduli e i gestori registrati nel `<httpModules>` e `<httpHandlers>` sezioni, vengono visualizzati all'interno di `<system.web>` elemento. Se si utilizza IIS 7.0, devono essere registrati nel `<system.webServer>` dell'elemento `<modules>` e `<handlers>` sezioni. Fortunatamente, è possibile definire i gestori in e moduli HTTP *entrambi* inserisce indipendentemente dal server web in uso. Questa opzione è quella più portabile, in quanto consente la stessa configurazione da utilizzare negli ambienti di sviluppo e produzione indipendentemente dal server web in uso.

Avviare la registrazione di `ErrorLogModule` modulo HTTP e `ErrorLogPageFactory` gestore HTTP nel `<httpModules>` e `<httpHandlers>` sezione `<system.web>`. Se la configurazione già definisce questi due elementi semplicemente quindi includere la `<add>` elemento per il modulo HTTP del ELMAH e gestore.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample1.xml)]

Successivamente, registrare del ELMAH modulo HTTP e gestore di `<system.webServer>` elemento. Come in precedenza, se questo elemento non è già presente nella configurazione di aggiungerlo.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample2.xml)]

Per impostazione predefinita, tuttavia se i moduli HTTP e gestori eventi vengono registrati in IIS 7 di `<system.web>` sezione. Il `validateIntegratedModeConfiguration` attributo il `<validation>` elemento indica IIS 7 per eliminare tali messaggi di errore.

Si noti che la sintassi per la registrazione di `ErrorLogPageFactory` gestore HTTP include un `path` attributo, che è impostato su `elmah.axd`. Questo attributo indica all'applicazione web che, se arriva una richiesta per una pagina denominata `elmah.axd` quindi deve essere elaborata la richiesta di `ErrorLogPageFactory` gestore HTTP. Verranno esaminati i `ErrorLogPageFactory` gestore HTTP nell'azione più avanti in questa esercitazione.

### <a name="step-3-configuring-elmah"></a>Passaggio 3: Configurazione ELMAH

ELMAH cerca le opzioni di configurazione del sito Web `Web.config` file in una sezione di configurazione personalizzata denominata `<elmah>`. Per utilizzare una sezione personalizzata in `Web.config` deve innanzitutto essere definita nel `<configSections>` elemento. Aprire il `Web.config` file e aggiungere il markup seguente per il `<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample3.xml)]

> [!NOTE]
> Se si sta configurando ELMAH per un'applicazione di 1. x ASP.NET quindi rimuovere il `requirePermission="false"` dall'attributo di `<section>` elementi sopra riportato.


La sintassi precedente registra personalizzata `<elmah>` sezione e le relative sottosezioni: `<security>`, `<errorLog>`, `<errorMail>`, e `<errorFilter>`.

Successivamente, aggiungere il `<elmah>` sezione a `Web.config`. In questa sezione deve essere visualizzato allo stesso livello di `<system.web>` elemento. All'interno di `<elmah>` sezione aggiungere la `<security>` e `<errorLog>` sezioni come illustrato di seguito:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample4.xml)]

Il `<security>` della sezione `allowRemoteAccess` attributo indica se è consentito l'accesso remoto. Se questo valore è impostato su 0, la pagina web log di errore può essere visualizzato solo in locale. Se questo attributo è impostato su 1 la pagina di errore log web è abilitata per i visitatori locali e remoti. Per il momento, si disattiva l'errore log web per i visitatori remoti. Faremo in modo accesso remoto in un secondo momento, dopo aver ottenuto l'opportunità di discutere i problemi di protezione di questa operazione.

Il `<errorLog>` sezione definisce l'origine del registro errori, che stabilisce in cui vengono registrati i dettagli dell'errore, è simile al `<providers>` sezione dello stato di integrità sistema di monitoraggio. Specifica la sintassi precedente il `SqlErrorLog` classe come origine di log dell'errore, che registra gli errori a un database di Microsoft SQL Server specificato per il `connectionStringName` valore dell'attributo.

> [!NOTE]
> ELMAH viene fornito con i provider di log di errore che possono essere usati per registrare gli errori in un file XML, un database Microsoft Access, database Oracle e altri archivi dati. Fare riferimento all'esempio `Web.config` file incluso con il download ELMAH per informazioni su come utilizzare questi provider di log di errore alternativa.


### <a name="step-4-creating-the-error-log-source-infrastructure"></a>Passaggio 4: Creazione dell'infrastruttura di origine di Log di errore

Del ELMAH `SqlErrorLog` provider registra i dettagli dell'errore a un database di Microsoft SQL Server specificato. Il `SqlErrorLog` provider prevede che il database per una tabella denominata `ELMAH_Error` e tre stored procedure: `ELMAH_GetErrorsXml`, `ELMAH_GetErrorXml`, e `ELMAH_LogError`. Il download ELMAH include un file denominato `SQLServer.sql` nel `db` cartella che contiene l'oggetto T-SQL per la creazione di questa tabella e queste stored procedure. È necessario eseguire queste istruzioni sul database da utilizzare il `SqlErrorLog` provider.

**Nelle figure 1** e **2** Mostra Esplora Database in Visual Studio dopo gli oggetti di database necessari per il `SqlErrorLog` provider sono stati aggiunti.

[![](logging-error-details-with-elmah-cs/_static/image2.png)](logging-error-details-with-elmah-cs/_static/image1.png)

**Figura 1**: il `SqlErrorLog` Provider registra gli errori per il `ELMAH_Error` tabella

[![](logging-error-details-with-elmah-cs/_static/image4.png)](logging-error-details-with-elmah-cs/_static/image3.png)

**Figura 2**: il `SqlErrorLog` Provider utilizza tre Stored procedure

## <a name="elmah-in-action"></a>ELMAH In azione

A questo punto sono state aggiunte ELMAH all'applicazione web, registrato il `ErrorLogModule` modulo HTTP e `ErrorLogPageFactory` gestore HTTP, specificare le opzioni di configurazione del ELMAH in `Web.config`e aggiunti gli oggetti di database necessari per il `SqlErrorLog` provider di log di errore. A questo punto si è pronti vedere ELMAH nell'azione! Visitare il sito Web recensioni e visitare una pagina che genera un errore di runtime, ad esempio `Genre.aspx?ID=foo`, o una pagina non esistente, ad esempio `NoSuchPage.aspx`. Ciò che viene visualizzato durante la visita di queste pagine dipende il `<customErrors>` configurazione e se si sta visitando localmente o in remoto. (Fare riferimento al [ *la visualizzazione di una pagina di errore personalizzato* esercitazione](displaying-a-custom-error-page-cs.md) per un aggiornamento in questo argomento.)

ELMAH non influisce sul contenuto viene visualizzato all'utente quando si verifica un'eccezione non gestita; Registra solo i dettagli. Il log degli errori è accessibile dalla pagina web `elmah.axd` dalla radice del sito Web, ad esempio `http://localhost/BookReviews/elmah.axd`. (Questo file non è fisicamente esistente nel progetto, ma quando arriva una richiesta per `elmah.axd` il runtime lo invia al `ErrorLogPageFactory` gestore HTTP che genera il markup inviato al browser.)

> [!NOTE]
> È inoltre possibile utilizzare il `elmah.axd` pagina per indicare ELMAH per generare un errore di test. Visita `elmah.axd/test` (come in `http://localhost/BookReviews/elmah.axd/test`) fa sì che ELMAH generare un'eccezione di tipo `Elmah.TestException`, che contiene il messaggio di errore: "This is a eccezione di prova che può essere tranquillamente ignorata."


**Figura 3** Mostra il log degli errori durante la visita `elmah.axd` dall'ambiente di sviluppo.

[![](logging-error-details-with-elmah-cs/_static/image6.png)](logging-error-details-with-elmah-cs/_static/image5.png)

**Figura 3**: `Elmah.axd` consente di visualizzare il Log degli errori da una pagina Web  
([Fare clic per visualizzare l'immagine ingrandita](logging-error-details-with-elmah-cs/_static/image7.png))

I log degli errori **figura 3** contiene sei voci di errore. Ogni voce include il codice di stato HTTP (404 o 500, questi errori), il tipo, la descrizione, il nome dell'utente connesso quando si è verificato l'errore e la data e ora. Il collegamento dei dettagli visualizza una pagina che include lo stesso messaggio di errore nell'errore dettagli giallo schermata di morte (vedere **figura 4**) insieme ai valori di variabili server al momento dell'errore (vedere  **Figura 5**). È inoltre possibile visualizzare il XML non elaborato in cui vengono salvati i dettagli dell'errore, che include informazioni aggiuntive, ad esempio i valori dell'intestazione HTTP POST.

[![](logging-error-details-with-elmah-cs/_static/image9.png)](logging-error-details-with-elmah-cs/_static/image8.png)

**Figura 4**: visualizzare i dettagli dell'errore YSOD  
([Fare clic per visualizzare l'immagine ingrandita](logging-error-details-with-elmah-cs/_static/image10.png))

[![](logging-error-details-with-elmah-cs/_static/image12.png)](logging-error-details-with-elmah-cs/_static/image11.png)

**Figura 5**: esplorare i valori della raccolta di variabili Server al momento dell'errore  
([Fare clic per visualizzare l'immagine ingrandita](logging-error-details-with-elmah-cs/_static/image13.png))

Distribuzione ELMAH al sito Web di produzione comporta:

- Copia il `Elmah.dll` file per il `Bin` cartella alla produzione,
- Copia le impostazioni di configurazione ELMAH specifiche per la `Web.config` file utilizzata in produzione, e
- Aggiunta di infrastruttura di origine dell'errore del log al database di produzione.

Abbiamo esplorato tecniche per la copia dei file dallo sviluppo alla produzione nelle esercitazioni precedenti. Ad esempio il modo più semplice per ottenere l'infrastruttura di origine di log di errore nel database di produzione è di utilizzare SQL Server Management Studio per connettersi al database di produzione e quindi eseguire il `SqlServer.sql` file di script, che verrà creata la tabella desiderata e archiviati procedure.

### <a name="viewing-the-error-details-page-on-production"></a>Visualizzazione di pagina dei dettagli dell'errore in produzione

Dopo aver distribuito il sito di produzione, visitare il sito Web di produzione e generare un'eccezione non gestita. Come ambiente di sviluppo, ELMAH non ha alcun effetto nella pagina di errore visualizzata quando si verifica un'eccezione non gestita; al contrario, semplicemente registra l'errore. Se si tenta di visitare la pagina di log degli errori (`elmah.axd`) dall'ambiente di produzione, verrà visualizzata con la pagina di accesso negato nel **figura 6**.

[![](logging-error-details-with-elmah-cs/_static/image15.png)](logging-error-details-with-elmah-cs/_static/image14.png)

**Figura 6**: per impostazione predefinita, i visitatori remoti non è possibile visualizzare la pagina di Web Log Errore  
([Fare clic per visualizzare l'immagine ingrandita](logging-error-details-with-elmah-cs/_static/image16.png))

Tenere presente che la configurazione di ELMAH `<security>` sezione configuriamo il `allowRemoteAccess` attributo 0, che impedisce agli utenti remoti di visualizzare il log degli errori. È importante impedire ai visitatori anonimi di visualizzare il log degli errori, come i dettagli dell'errore potrebbero rivelare vulnerabilità della sicurezza o altre informazioni riservate. Se si decide di impostare questo attributo su 1 e abilitare l'accesso remoto per il log degli errori, è importante per bloccare il `elmah.axd` percorso in modo che solo autorizzati visitatori può accedervi. Questo può essere ottenuto mediante l'aggiunta di un `<location>` elemento per il `Web.config` file.

La configurazione seguente consente solo agli utenti nel ruolo di amministratore per accedere alla pagina web log di errore:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample5.xml)]

> [!NOTE]
> Il ruolo di amministratore e tre utenti nel sistema - Scott Jisun e Alice - sono stati aggiunti nel [ *la configurazione di un sito Web che utilizza i servizi delle applicazioni* esercitazione](configuring-a-website-that-uses-application-services-cs.md). Gli utenti Scott e Jisun sono i membri del ruolo di amministratore. Per ulteriori informazioni sull'autenticazione e autorizzazione, consultare il [esercitazioni di sicurezza del sito Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Il log degli errori nell'ambiente di produzione può ora essere visualizzato dagli utenti remoti; Fare riferimento al **3 cifre**, **4**, e **5** per catture di schermata della pagina web di log di errore. Tuttavia, se un utente anonimo o l'utente non amministratore tenta di visualizzare la pagina di log degli errori viene automaticamente reindirizzato alla pagina di accesso (`Login.aspx`), come **figura 7** Mostra.

[![](logging-error-details-with-elmah-cs/_static/image18.png)](logging-error-details-with-elmah-cs/_static/image17.png)

**Figura 7**: gli utenti non autorizzati vengono automaticamente reindirizzati alla pagina di accesso  
([Fare clic per visualizzare l'immagine ingrandita](logging-error-details-with-elmah-cs/_static/image19.png))

### <a name="programmatically-logging-errors"></a>A livello di codice di registrazione degli errori

Del ELMAH `ErrorLogModule` modulo HTTP registra automaticamente le eccezioni non gestite per l'origine di log specificato. In alternativa, è possibile registrare un errore senza dover generare un'eccezione non gestita utilizzando il `ErrorSignal` classe e il relativo `Raise` metodo. Il `Raise` viene passato un `Exception` dell'oggetto e lo registra come se l'eccezione è stata generata e ha raggiunto il runtime di ASP.NET senza essere stato gestito. La differenza, tuttavia, è che la richiesta continua l'esecuzione in genere dopo la `Raise` metodo è stato chiamato, mentre viene generata un'eccezione non gestita interrompe l'esecuzione normale della richiesta, il runtime ASP.NET visualizzare l'applicazione configurata pagina di errore.

La `ErrorSignal` classe è utile nelle situazioni in cui è presente un'azione che potrebbe non riuscire, ma il relativo tipo di errore non irreversibile per l'intera operazione viene eseguita. Ad esempio, un sito Web può contenere un form che accetta l'input dell'utente, viene memorizzato in un database e quindi invia un messaggio di posta elettronica informa l'utente che è state elaborate informazioni. L'azione da eseguire se le informazioni vengono salvate correttamente al database, ma si verifica un errore durante l'invio del messaggio di posta elettronica? Un'opzione, è possibile generare un'eccezione e invia l'utente alla pagina di errore. Tuttavia, ciò potrebbe confondere l'utente in modo che le informazioni che utente ha immesso non è state salvate. Un altro approccio sarebbe per registrare l'errore relativo alla posta elettronica, ma non modificare l'esperienza dell'utente in alcun modo. Questa opzione è quando la `ErrorSignal` classe è utile.

[!code-csharp[Main](logging-error-details-with-elmah-cs/samples/sample6.cs)]

## <a name="error-notification-via-email"></a>Errore di notifica tramite posta elettronica

Insieme agli errori di registrazione a un database, ELMAH può essere configurato anche per i dettagli dell'errore di posta elettronica a un destinatario specificato. Questa funzionalità viene fornita per il `ErrorMailModule` modulo HTTP; pertanto, è necessario registrare il modulo HTTP in `Web.config` per inviare i dettagli dell'errore tramite posta elettronica.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample7.xml)]

Successivamente, specificare le informazioni sul messaggio di posta elettronica errore nel `<elmah>` dell'elemento `<errorMail>` sezione, che indica il messaggio di posta elettronica mittente e destinatario, oggetto, e se il messaggio di posta elettronica viene inviato in modo asincrono.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample8.xml)]

Con le impostazioni sopra riportate sul posto, ogni volta che un errore di runtime si verifica ELMAH invia un messaggio di posta support@example.com con i dettagli dell'errore. Messaggio di posta elettronica dell'ELMAH errore include le stesse informazioni mostrate in errore dettagli pagina web, vale a dire il messaggio di errore, la traccia dello stack e le variabili del server (fare riferimento al **4 cifre** e **5**). Il messaggio di errore include anche il contenuto di eccezione dettagli giallo schermata di morte come un allegato (`YSOD.html`).

**Figura 8** Mostra errore messaggio di posta elettronica dell'ELMAH generato visitando `Genre.aspx?ID=foo`. Mentre **figura 8** Mostra solo l'errore messaggio e traccia dello stack, variabili server sono incluse ulteriormente verso il basso nel corpo del messaggio di posta elettronica.

[![](logging-error-details-with-elmah-cs/_static/image21.png)](logging-error-details-with-elmah-cs/_static/image20.png)

**Figura 8**: È possibile configurare ELMAH per inviare i dettagli dell'errore tramite posta elettronica  
([Fare clic per visualizzare l'immagine ingrandita](logging-error-details-with-elmah-cs/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>Solo registrazione degli errori di interesse

Per impostazione predefinita, ELMAH registra i dettagli di ogni eccezione non gestita, nonché altri errori HTTP 404. È possibile indicare ELMAH per ignorare tali o altri tipi di errori tramite il filtro di errore. La logica di filtro viene eseguita da dell'ELMAH `ErrorFilterModule` modulo HTTP, che è necessario registrare in `Web.config` per utilizzare la logica di filtro. Le regole per il filtro vengono specificate nel `<errorFilter>` sezione.

Il markup seguente indica ELMAH non registrare gli 404 errori.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample9.xml)]

> [!NOTE]
> Non dimenticare, per poter utilizzare i filtri di errore è necessario registrare il `ErrorFilterModule` modulo HTTP.


Il `<equal>` elemento all'interno di `<test>` sezione viene definita un'asserzione. Se l'asserzione viene valutata true l'errore viene filtrato dal registro del ELMAH. Sono disponibili, tra cui le altre asserzioni: `<greater>`, `<greater-or-equal>`, `<not-equal>`, `<lesser>`, `<lesser-or-equal>`e così via. È anche possibile combinare le asserzioni usando il `<and>` e `<or>` operatori booleani. Inoltre, è possibile anche includere un'espressione semplice JavaScript come un'asserzione, o scrivere asserzioni personalizzate in c# o Visual Basic.

Per ulteriori informazioni sull'errore della ELMAH funzionalità di filtro, vedere il [sezione filtrare errori](https://code.google.com/p/elmah/wiki/ErrorFiltering) nel [ELMAH wiki](https://code.google.com/p/elmah/w/list).

## <a name="summary"></a>Riepilogo

ELMAH fornisce un meccanismo semplice ma potente per la registrazione degli errori in un'applicazione web ASP.NET. Come sistema di monitoraggio di integrità di Microsoft ELMAH può registrare errori in un database e può inviare i dettagli dell'errore per gli sviluppatori tramite posta elettronica. A differenza di integrità di sistema di monitoraggio, ELMAH include fuori il supporto per una vasta gamma di archivi di dati di log errore, tra cui: Microsoft SQL Server, Microsoft Access, Oracle, file XML e molti altri. Inoltre, ELMAH offre un meccanismo predefinito per la visualizzazione dei log degli errori e informazioni dettagliate su un errore specifico da una pagina web, `elmah.axd`. Il `elmah.axd` pagina può anche visualizzare informazioni sull'errore come un feed RSS o un file con valori delimitati da virgole (CSV), che consente di leggere utilizzando Microsoft Excel. È anche possibile indicare ELMAH per filtrare errori dal log di utilizzo di asserzioni dichiarative o a livello di codice. E può essere utilizzato con le applicazioni ASP.NET versione 1. x ELMAH.

Ogni applicazione distribuita deve avere un meccanismo per la registrazione automaticamente le eccezioni non gestite e l'invio di notifiche al team di sviluppo. Se questa operazione viene eseguita tramite il monitoraggio dello stato o ELMAH è secondaria. In altre parole, non è importante molto se si utilizza il monitoraggio dello stato o ELMAH; entrambi i sistemi di valutare e quindi scegliere quello più adatto alle proprie esigenze. Che cos'è fondamentalmente importante è che è possibile inserire un meccanismo in grado di registrare le eccezioni non gestite nell'ambiente di produzione.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [ELMAH - gestori e i moduli di registrazione errore](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [Pagina progetto ELMAH](https://code.google.com/p/elmah/) (origine codice, gli esempi, wiki)
- [Collegare ELMAH in un Web dell'applicazione per intercettare le eccezioni non gestite](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx) (video)
- [Pagine di Log di errore di sicurezza](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [Utilizzo di gestori e i moduli HTTP per creare componenti ASP.NET collegabili](https://msdn.microsoft.com/library/aa479332.aspx)
- [Esercitazioni di sicurezza sito Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Precedente](logging-error-details-with-asp-net-health-monitoring-cs.md)
> [Successivo](precompiling-your-website-cs.md)
