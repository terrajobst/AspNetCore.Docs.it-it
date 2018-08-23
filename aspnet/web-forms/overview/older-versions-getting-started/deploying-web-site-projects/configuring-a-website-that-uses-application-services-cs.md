---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs
title: Configurazione di un sito Web che utilizza i servizi dell'applicazione (c#) | Microsoft Docs
author: rick-anderson
description: Verze Technologie ASP.NET 2.0 introdotto una serie di servizi delle applicazioni, che fanno parte di .NET Framework e vengono usati come una suite di blocchi predefiniti di servizi che yo...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 1e33d1c6-3f9f-4c26-81e2-2a8f8907bb05
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs
msc.type: authoredcontent
ms.openlocfilehash: 4cec939795c2b3abfd51c894f985dfd2eb7bc361
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823919"
---
<a name="configuring-a-website-that-uses-application-services-c"></a>Configurazione di un sito Web che utilizza i servizi dell'applicazione (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_cs.pdf)

> Verze Technologie ASP.NET 2.0 introdotto una serie di servizi delle applicazioni, che fanno parte di .NET Framework e vengono usati come una suite di servizi di blocchi predefiniti che è possibile usare per aggiungere funzionalità avanzate per l'applicazione web. Questa esercitazione illustra come configurare un sito Web nell'ambiente di produzione usare i servizi dell'applicazione e risolve i problemi comuni con la gestione degli account utente e i ruoli nell'ambiente di produzione.


## <a name="introduction"></a>Introduzione

Verze Technologie ASP.NET 2.0 introdotto una serie di *servizi delle applicazioni*, che fanno parte di .NET Framework e servire come una suite di blocchi predefiniti di servizi che si possono usare per aggiungere funzionalità avanzate per l'applicazione web. I servizi delle applicazioni includono:

- **Appartenenza** : un'API per la creazione e gestione degli account utente.
- **Ruoli** : un'API per la classificazione degli utenti in gruppi.
- **Profilo** : un'API per l'archiviazione dei contenuti personalizzato, specifico dell'utente.
- **Mappa del sito** : un'API per la definizione di una struttura logica sito s sotto forma di una gerarchia, che può quindi essere visualizzata tramite i controlli di navigazione, ad esempio menu e percorsi di navigazione.
- **Personalizzazione** : un'API per la gestione delle preferenze di personalizzazione, generalmente utilizzate con [ *WebParts*](https://msdn.microsoft.com/library/e0s9t4ck.aspx).
- **Monitoraggio dell'integrità** : un'API per il monitoraggio delle prestazioni, sicurezza, gli errori e altre metriche di integrità di sistema per un'applicazione web in esecuzione.
  

I servizi delle applicazioni API non sono associati a un'implementazione specifica. In alternativa, è indicare i servizi delle applicazioni di utilizzare un determinato *provider*, e che il provider implementa il servizio utilizzando una tecnologia specifica. I provider di usati più comune per le applicazioni web basati su Internet ospitate in una società di hosting web sono i provider che usano un'implementazione di database di SQL Server. Ad esempio, il `SqlMembershipProvider` è un provider per l'API di appartenenza che archivia informazioni sull'account utente in un database Microsoft SQL Server.

Usando i servizi delle applicazioni e i provider di SQL Server rende piuttosto complicato quando si distribuisce l'applicazione. Per i principianti, gli oggetti di database di servizi dell'applicazione devono essere stati creati correttamente nel database di sviluppo e produzione e inizializzati in modo appropriato. Sono inoltre presenti importanti impostazioni di configurazione che è necessario apportare.

> [!NOTE]
> I servizi delle applicazioni API sono state progettate con il [ *modello di provider*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), uno schema progettuale che consente un'API s dettagli di implementazione essere fornito in fase di esecuzione. .NET Framework viene fornito con un numero di provider di servizi di applicazione che può essere utilizzato, ad esempio la `SqlMembershipProvider` e `SqlRoleProvider`, quali sono i provider per l'appartenenza e ruoli API che usano un Server SQL database implementazione. È anche possibile creare e plug-in un provider personalizzato. In effetti, l'applicazione web le recensioni dei libri già contiene un provider personalizzato per l'API di mappa del sito (`ReviewSiteMapProvider`), che costruisce la mappa del sito dai dati di `Genres` e `Books` nelle tabelle del database.


Questa esercitazione inizia con uno sguardo al modo in cui l'applicazione web le recensioni dei libri per usare le API di ruoli e appartenenza esteso. Quindi illustra in modo dettagliato la distribuzione di un'applicazione web che usa i servizi dell'applicazione con un'implementazione di database di SQL Server e si conclude con una risoluzione dei problemi comuni con la gestione degli account utente e i ruoli nell'ambiente di produzione.

## <a name="updates-to-the-book-reviews-application"></a>Aggiornamenti all'applicazione le verifiche della Rubrica

Ultimi esercitazioni che l'applicazione web di verifiche di libro è stato aggiornato da un sito Web statico per un'applicazione web dinamiche guidate dai dati completata con un set di pagine di amministrazione per la gestione di generi e recensioni. Tuttavia, in questa sezione di amministrazione non è protetto, qualsiasi utente che conosce l'URL della pagina Amministrazione (o verrà automaticamente dedotto) può walzer e creare, modificare o eliminare le revisioni nel nostro sito. Un modo comune per proteggere determinate parti di un sito Web consiste nell'implementare gli account utente e quindi usare le regole di autorizzazione URL per limitare l'accesso a determinati utenti o ruoli. L'applicazione web le recensioni dei libri disponibile per il download in questa esercitazione supporta i ruoli e account utente. Ha un unico ruolo definito denominato amministratore e solo gli utenti di questo ruolo possono accedere alle pagine di amministrazione.

> [!NOTE]
> Ho creato tre account utente nell'applicazione web le recensioni dei libri: Scott Jisun e Alice. Tutti e tre gli utenti hanno la stessa password: **password!** In cui Scott e Jisun fanno parte del ruolo di amministratore, Alice non è presente. Le pagine di amministrazione non s sito sono ancora accessibili agli utenti anonimi. Vale a dire, non è necessario accedere a visitare il sito, a meno che non si desidera amministrare, nel qual caso è necessario accedere come utente nel ruolo di amministratore.


Pagina master le recensioni dei libri applicazione s è stata aggiornata per includere un'interfaccia utente diversa per gli utenti autenticati e anonimi. Se un utente anonimo visita il sito vede un collegamento di accesso nell'angolo superiore destro. Un utente autenticato vede il messaggio ", Bentornato *username*!" e un collegamento per effettuare la disconnessione. Esiste anche una pagina di accesso s (`~/Login.aspx`), che contiene un controllo di accesso Web che fornisce l'interfaccia utente e per la logica per l'autenticazione di un visitatore. Solo gli amministratori possono creare nuovi account. (Sono presenti pagine per la creazione e la gestione degli account utente di `~/Admin` cartella.)

### <a name="configuring-the-membership-and-roles-apis"></a>La configurazione di appartenenza e ruoli API

L'applicazione web le recensioni dei libri Usa le API di ruoli e appartenenza per supportare gli account utente e raggruppare gli utenti in ruoli (vale a dire, il ruolo di amministratore). Il `SqlMembershipProvider` e `SqlRoleProvider` vengono utilizzate le classi di provider perché si desidera archiviare informazioni sui ruoli e account in un database di SQL Server.

> [!NOTE]
> Questa esercitazione non intende essere un esame approfondito la configurazione di un'applicazione web per supportare le API di ruoli e appartenenza. Per un'analisi approfondita queste API e i passaggi da eseguire per configurare un sito Web per il loro uso, consultare il [ *sito Web di sicurezza esercitazioni*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Usare i servizi delle applicazioni con un database di SQL Server, che è innanzitutto necessario aggiungere gli oggetti di database usati da questi provider per il database in cui si desidera che l'account utente e le informazioni sui ruoli archiviate. Questi oggetti di database necessarie includono un'ampia gamma di tabelle, viste e stored procedure. Se non diversamente specificato, il `SqlMembershipProvider` e `SqlRoleProvider` classi provider usano un database di SQL Server Express Edition denominato `ASPNETDB` che si trova in s applicazione `App_Data` cartella; se tale database non esiste, viene creato automaticamente con gli oggetti di database necessari da questi provider in fase di esecuzione.

È possibile, e in genere ideale, per creare l'applicazione Servizi gli oggetti di database nello stesso database dove sono archiviati i dati specifici dell'applicazione s sito Web. .NET Framework viene fornito con uno strumento denominato `aspnet_regsql.exe` che installa gli oggetti di database in un database specificato. Ho voluto e consente di aggiungere questi oggetti a questo strumento il `Reviews.mdf` del database nel `App_Data` cartella (database di sviluppo). Si vedrà come utilizzare questo strumento più avanti in questa esercitazione quando si aggiungono questi oggetti nel database di produzione.

Se si aggiunge l'applicazione di servizi gli oggetti di database a un database diverso da `ASPNETDB` sarà necessario personalizzare il `SqlMembershipProvider` e `SqlRoleProvider` provider classi delle configurazioni in modo che utilizzino il database appropriato. Per personalizzare il provider di appartenenze aggiungere un [  *&lt;appartenenza&gt; elemento* ](https://msdn.microsoft.com/library/1b9hw62f.aspx) all'interno di `<system.web>` sezione `Web.config`; usare il [  *&lt;roleManager&gt; elemento* ](https://msdn.microsoft.com/library/ms164660.aspx) per configurare il provider di ruoli. Il frammento di codice seguente è tratto dall'applicazione s le recensioni dei libri `Web.config` e Mostra le impostazioni di configurazione per le API di ruoli e appartenenza. Si noti che sia registrato un nuovo provider - `ReviewMembership` e `ReviewRole` -che usano il `SqlMembershipProvider` e `SqlRoleProvider` provider, rispettivamente.

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample1.xml)]

Il `Web.config` file s `<authentication>` elemento è stato configurato anche per supportare l'autenticazione basata su form.
  

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>Limitare l'accesso alle pagine di amministrazione

ASP.NET semplifica concedere o negare l'accesso a un determinato file o una cartella dall'utente o al ruolo tramite il *autorizzazione URL* funzionalità. (Illustrata brevemente autorizzazione URL nel *differenze principali tra IIS e ASP.NET Development Server* esercitazione ed è stato illustrato come il Server di sviluppo ASP.NET e IIS si applicano le regole di autorizzazione URL in modo diverso per statico rispetto a contenuto dinamico.) Poiché si vuole proibire l'accesso al `~/Admin` cartella ad eccezione di tali utenti il ruolo di amministratore, è necessario aggiungere regole di autorizzazione URL per questa cartella. In particolare, le regole di autorizzazione URL necessario consentire agli utenti il ruolo di amministratore e negare tutti gli altri utenti. Questa operazione viene eseguita mediante l'aggiunta di un `Web.config` file per il `~/Admin` cartella con il contenuto seguente:

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample3.xml)]

Per altre informazioni sulle funzionalità di autorizzazione URL ASP.NET s e su come usarlo per spiegare chiaramente le regole di autorizzazione per gli utenti e per i ruoli, assicurarsi di leggere il [ *basata sull'utente Authorization* ](../../older-versions-security/membership/user-based-authorization-cs.md) e [ *Autorizzazione basata sui ruoli* ](../../older-versions-security/roles/role-based-authorization-cs.md) esercitazioni dalla mia [ *sito Web di sicurezza esercitazioni*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

## <a name="deploying-a-web-application-that-uses-application-services"></a>Distribuzione di un'applicazione Web che utilizza i servizi dell'applicazione

Quando si distribuisce un sito Web che usa un provider che archivia le informazioni di servizi dell'applicazione in un database e servizi dell'applicazione, è fondamentale che gli oggetti di database necessari per i servizi delle applicazioni creati nel database di produzione. Inizialmente non contiene il database di produzione questi oggetti, questa operazione quando l'applicazione viene distribuita prima di tutto (o quando viene distribuito per la prima volta dopo l'aggiunta di servizi delle applicazioni), è necessario eseguire passaggi aggiuntivi per ottenere questi oggetti di database richiesti di database di produzione.

Un altro problema può verificarsi quando si distribuisce un sito Web che utilizza i servizi dell'applicazione se si intende eseguire la replica degli account utente creati nell'ambiente di sviluppo all'ambiente di produzione. A seconda della configurazione di appartenenza e ruoli, è possibile che anche se si copiano correttamente gli account utente che sono stati creati nell'ambiente di sviluppo per il database di produzione, questi utenti non possono accedere all'applicazione web nell'ambiente di produzione. Verranno esaminare la causa del problema e illustra come impedire che si verifichi.

ASP.NET viene fornito con un interessante [ *Web Site Administration Tool (WSAT)* ](https://msdn.microsoft.com/library/yy40ytx0.aspx) che può essere avviato da Visual Studio e consente all'utente le regole di account, ruoli e autorizzazione a essere gestiti attraverso una basata sul web interfaccia. Sfortunatamente, il WSAT funziona solo per i siti Web locale, vale a dire che non può essere usato per gestire in remoto gli account utente, ruoli e le regole di autorizzazione per l'applicazione web nell'ambiente di produzione. Esamineremo diversi modi per implementare un comportamento simile a WSAT dal sito Web di produzione.

### <a name="adding-the-database-objects-using-aspnetregsqlexe"></a>Aggiunta di oggetti di Database mediante aspnet\_regsql.exe

Il *distribuzione di un Database* esercitazione ha illustrato come copiare le tabelle e i dati dal database di sviluppo nel database di produzione e queste tecniche sono certamente utilizzabile per copiare gli oggetti di database di servizi di applicazione per il database di produzione. Un'altra opzione consiste di `aspnet_regsql.exe` strumento che aggiunge o rimuove gli oggetti di database di servizi di applicazione da un database.

> [!NOTE]
> Il `aspnet_regsql.exe` strumento crea gli oggetti di database in un database specificato. Non eseguire la migrazione di dati in tali oggetti di database dal database di sviluppo per il database di produzione. Se si intende copiare le informazioni di account e il ruolo utente nel database di sviluppo per il database di produzione usare le tecniche illustrate nel *distribuzione di un Database* esercitazione.


Let s esaminare come aggiungere gli oggetti di database al database di produzione usando il `aspnet_regsql.exe` dello strumento. Iniziare aprendo Esplora Windows e passare alla directory .NET Framework versione 2.0 nel computer, %WINDIR%\ Microsoft.NET\Framework\v2.0.50727. Vi deve trovare la `aspnet_regsql.exe` dello strumento. Questo strumento può essere usato dalla riga di comando, ma include anche un'interfaccia utente grafica. Fare doppio clic il `aspnet_regsql.exe` file per avviare il componente con interfaccia grafica.

Lo strumento si avvia visualizzando una schermata iniziale che spiega lo scopo. Fare clic su Avanti per passare alla schermata di "Selezionare un'opzione di installazione", come illustrata nella figura 1. Da qui è possibile scegliere di aggiungere i servizi delle applicazioni di oggetti di database o rimuoverli da un database. Poiché si vuole aggiungere questi oggetti nel database di produzione, selezionare l'opzione "Configura SQL Server per i servizi dell'applicazione" e fare clic su Avanti.


[![Scegliere di configurare SQL Server per i servizi dell'applicazione](configuring-a-website-that-uses-application-services-cs/_static/image2.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image1.jpg)

**Figura 1**: scegliere di configurare SQL Server per i servizi dell'applicazione ([fare clic per visualizzare l'immagine con dimensioni normali](configuring-a-website-that-uses-application-services-cs/_static/image3.jpg))


Nella finestra "Selezionare il Server e Database" schermata richiede per le informazioni per connettersi al database. Immettere il server di database, le credenziali di sicurezza e il nome del database fornito all'utente da società di hosting web e fare clic su Avanti.

> [!NOTE]
> Dopo aver immesso il server di database e credenziali è potrebbe verificarsi un errore quando si espande l'elenco di riepilogo a discesa di database. Il `aspnet_regsql.exe` dello strumento di query di `sysdatabases` tabella di sistema per recuperare un elenco di database nel server, ma alcuni web hosting bloccare le aziende i server di database in modo che queste informazioni non sono disponibili pubblicamente. Se si verifica questo errore è possibile digitare il nome del database direttamente nell'elenco a discesa.


[![Fornire allo strumento con le informazioni di connessione Database s](configuring-a-website-that-uses-application-services-cs/_static/image5.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image4.jpg)

**Figura 2**: specificare gli oggetti degli strumenti con il Database le informazioni di connessione ([fare clic per visualizzare l'immagine con dimensioni normali](configuring-a-website-that-uses-application-services-cs/_static/image6.jpg))


La schermata successiva sono riepilogate le azioni che verranno eseguite, vale a dire che gli oggetti di database dell'applicazione Servizi stanno per essere aggiunto al database specificato. Fare clic su Avanti per completare questa azione. Dopo alcuni istanti, viene visualizzata la schermata finale, notare che sono stati aggiunti gli oggetti di database (vedere la figura 3).


[![Vero successo! Gli oggetti di Database di servizi di applicazione sono stati aggiunti al Database di produzione](configuring-a-website-that-uses-application-services-cs/_static/image8.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image7.jpg)

**Figura 3**: esito positivo. L'applicazione Servizi di Database sono oggetti aggiunti al Database di produzione ([fare clic per visualizzare l'immagine con dimensioni normali](configuring-a-website-that-uses-application-services-cs/_static/image9.jpg))


Per verificare che gli oggetti di database di servizi di applicazione sono stati aggiunti al database di produzione, aprire SQL Server Management Studio e connettersi al database di produzione. Come illustrato nella figura 4, si noterà ora le tabelle di database di servizi di applicazione nel database `aspnet_Applications`, `aspnet_Membership`, `aspnet_Users`e così via.


[![Verificare che gli oggetti di Database sono stati aggiunti al Database di produzione](configuring-a-website-that-uses-application-services-cs/_static/image11.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image10.jpg)

**Figura 4**: confermare che gli oggetti di Database sono stati aggiunti al Database di produzione ([fare clic per visualizzare l'immagine con dimensioni normali](configuring-a-website-that-uses-application-services-cs/_static/image12.jpg))


È necessario usare il `aspnet_regsql.exe` strumento quando si distribuisce l'applicazione web per la prima volta o per la prima volta dopo avere avviato usando i servizi delle applicazioni. Dopo aver installato questi oggetti di database nel database di produzione si ha vinto t dover essere nuovamente aggiunto o modificato.

### <a name="copying-user-accounts-from-development-to-production"></a>Copia gli account utente, dallo sviluppo alla produzione

Quando si usa la `SqlMembershipProvider` e `SqlRoleProvider` classi del provider per archiviare le informazioni di servizi dell'applicazione in un database di SQL Server, le informazioni di account e il ruolo utente vengono archiviate in un'ampia gamma di tabelle di database, tra cui `aspnet_Users`, `aspnet_Membership`, `aspnet_Roles`, e `aspnet_UsersInRoles`, tra gli altri. Se durante lo sviluppo si creano gli account utente nell'ambiente di sviluppo è possibile replicare gli account utente nell'ambiente di produzione copiando i record corrispondenti da tabelle di database applicabile. Se si usa la pubblicazione guidata Database per distribuire gli oggetti di database di servizi di applicazione che si sia scelto anche per copiare i record, che genera gli account utente creati in fase di sviluppo per essere anche in produzione. Tuttavia, a seconda delle impostazioni di configurazione, è probabile che gli utenti con account creati in fase di sviluppo e copiati nell'ambiente di produzione sono in grado di accedere dal sito Web di produzione. Quali sono i vantaggi?

Il `SqlMembershipProvider` e `SqlRoleProvider` classi del provider sono state progettate in modo che un singolo database può essere utilizzato come un archivio utente, per dispongono di più applicazioni, in cui ogni applicazione può, in teoria, gli utenti con sovrapposizione dei nomi utente e i ruoli con lo stesso nome. Per consentire questa flessibilità, il database conserva un elenco delle applicazioni nel `aspnet_Applications` tabella e ogni utente è associato a una di queste applicazioni. In particolare, il `aspnet_Users` tabella ha un `ApplicationId` colonna che associa ogni utente a un record nel `aspnet_Applications` tabella.

Oltre al `ApplicationId` colonna, il `aspnet_Applications` tabella include inoltre un `ApplicationName` colonna che fornisce un nome più Converto per l'applicazione. Quando un sito Web tenta di usare un account utente, ad esempio la convalida di credenziali di un utente s dalla pagina di accesso, è necessario indicare il `SqlMembershipProvider` classe dell'applicazione da usare. Avviene in genere ciò specificando il nome dell'applicazione e questo valore deriva dalla configurazione del provider s nel `Web.config` : in particolare tramite il `applicationName` attributo.

Ma cosa succede se il `applicationName` attributo viene omesso in `Web.config`? In questo caso l'appartenenza al sistema utilizza il percorso radice dell'applicazione come il `applicationName` valore. Se il `applicationName` attributo non è impostato in modo esplicito `Web.config`, quindi, è possibile che l'ambiente di sviluppo e l'ambiente di produzione usare una radice dell'applicazione diverso e pertanto verrà associati a diversi dell'applicazione nomi dei servizi dell'applicazione. Se si verifica una mancata corrispondenza di questo tipo, quindi gli utenti creati nell'ambiente di sviluppo avrà un `ApplicationId` che non corrisponde al valore di `ApplicationId` valore per l'ambiente di produzione. Il risultato finale è che gli utenti ha vinto t potranno eseguire l'accesso.

> [!NOTE]
> Se ci si ritrova in questo caso - con gli account utente copiati nell'ambiente di produzione con una mancata corrispondenza `ApplicationId` valore, è possibile scrivere una query per aggiornare questi errato `ApplicationId` i valori per il `ApplicationId` utilizzata in produzione. Al termine dell'aggiornamento, gli utenti con account creati nell'ambiente di sviluppo a questo punto sarebbe in grado di accedere all'applicazione web in produzione.


La buona notizia è che vi sia un semplice passaggio da eseguire per assicurarsi che i due ambienti usino gli stessi `ApplicationId` - impostare in modo esplicito il `applicationName` attributo `Web.config` per tutti i provider di servizi di applicazione. Impostato in modo esplicito il `applicationName` attributo su "BookReviews" il `<membership>` e `<roleManager>` elementi come questo frammento di codice da `Web.config` Mostra.

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample4.xml)]

Per altre informazioni sull'impostazione di `applicationName` attributo e i motivi per cui, fare riferimento a [ *Scott Guthrie* ](https://weblogs.asp.net/scottgu/) , il post di blog [ *sempre impostato applicationName proprietà durante la configurazione di appartenenza ASP.NET e altri provider*](https://weblogs.asp.net/scottgu/443634).

### <a name="managing-user-accounts-in-the-production-environment"></a>Gestione degli account utente nell'ambiente di produzione

ASP.NET Web Site Administration Tool (WSAT) semplifica creare e gestire gli account utente, definire e applicare i ruoli e spiegare chiaramente le regole di autorizzazione basata su utenti e ruoli. È possibile avviare il WSAT da Visual Studio passare a Esplora soluzioni e scegliendo l'icona di configurazione di ASP.NET o per i menu sito Web o un progetto e selezionando la voce di menu configurazione ASP.NET. Sfortunatamente, il WSAT funzionano solo con i siti Web locali. Pertanto, è possibile utilizzare il WSAT dalla workstation per gestire il sito Web nell'ambiente di produzione.

La buona notizia è che tutte le funzionalità esposte fornito dal WSAT è disponibile a livello di codice tramite l'appartenenza e ruoli API; Inoltre, molte delle schermate di WSAT usare i controlli correlati all'accesso ASP.NET standard. In breve, è possibile aggiungere le pagine ASP.NET nel sito Web che offrono le funzionalità di gestione necessarie.

È importante ricordare che un'esercitazione precedente aggiornato l'applicazione web le recensioni dei libri per includere un `~/Admin` e questa cartella è stato configurato per consentire agli utenti il ruolo di amministratore. Ho aggiunto una pagina in tale cartella denominata `CreateAccount.aspx` da cui un amministratore può creare un nuovo account utente. Questa pagina Usa il controllo CreateUserWizard per visualizzare la logica di back-end e interfaccia utente per la creazione di un nuovo account utente. Novità più, personalizzate del controllo per includere una casella di controllo in cui viene richiesto se il nuovo utente deve anche essere aggiunto al ruolo di amministratore (vedere la figura 5). Con un po' di lavoro è possibile creare un set personalizzato di pagine che implementa le attività correlate alla gestione utente e il ruolo che in caso contrario a fornire il WSAT.

> [!NOTE]
> Per altre informazioni sull'uso di appartenenza e ruoli API con i controlli correlati all'accesso Web di ASP.NET, assicurarsi di leggere mio [ *sito Web di sicurezza esercitazioni*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Per altre informazioni sulla personalizzazione del controllo CreateUserWizard, vedere la [ *creazione degli account utente* ](../../older-versions-security/membership/creating-user-accounts-cs.md) e [ *archiviare informazioni utente aggiuntive* ](../../older-versions-security/membership/storing-additional-user-information-cs.md) esercitazioni o guarda [ *Erich Peterson* ](http://www.erichpeterson.com/) articolo s [ *personalizzare il controllo CreateUserWizard* ](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx).


[![Gli amministratori possono creare nuovi account utente](configuring-a-website-that-uses-application-services-cs/_static/image14.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image13.jpg)

**Figura 5**: gli amministratori possono creare nuovi account utente ([fare clic per visualizzare l'immagine con dimensioni normali](configuring-a-website-that-uses-application-services-cs/_static/image15.jpg))


Se necessaria la funzionalità completa l'estrazione WSAT [ *Rolling Your proprio strumento Amministrazione sito Web*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx), in cui l'autore Dan Clem descrive il processo di compilazione di uno strumento simile a WSAT personalizzato. Dan condivide il suo codice sorgente s dell'applicazione (in c#) e vengono fornite istruzioni dettagliate per aggiungerla al sito Web ospitato.

## <a name="summary"></a>Riepilogo

Quando si distribuisce un'applicazione web che usa l'implementazione di database di servizi di applicazione è necessario assicurarsi che il database di produzione contiene gli oggetti di database necessari. È possibile aggiungere questi oggetti usando le tecniche descritte nel *distribuzione di un Database* esercitazione; in alternativa, è possibile usare il `aspnet_regsql.exe` dello strumento, come abbiamo visto in questa esercitazione. Altri problemi illustrate nel sono incentrate sulla sincronizzazione utilizzato negli ambienti di sviluppo e produzione (che è importante se si desidera che gli utenti e ruoli creati nell'ambiente di sviluppo per essere valido in produzione) e le tecniche per il nome di applicazione gestione di utenti e ai ruoli nell'ambiente di produzione.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [*Strumento di registrazione ASP.NET SQL Server (aspnet_regsql.exe)*](https://msdn.microsoft.com/library/ms229862.aspx)
- [*Creare il Database di servizi dell'applicazione per SQL Server*](https://msdn.microsoft.com/library/x28wfk74.aspx)
- [*Creazione dello Schema di appartenenza in SQL Server*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md)
- [*Analisi ASP.NET s appartenenza, ruoli e profilo*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*In sequenza il proprio strumento Amministrazione sito Web*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*Sito Web sicurezza esercitazioni*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*Panoramica sullo strumento Amministrazione sito Web*](https://msdn.microsoft.com/library/yy40ytx0.aspx)

> [!div class="step-by-step"]
> [Precedente](configuring-the-production-web-application-to-use-the-production-database-cs.md)
> [Successivo](strategies-for-database-development-and-deployment-cs.md)
