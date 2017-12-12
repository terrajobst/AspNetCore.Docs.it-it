---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs
title: Configurazione di un sito Web che utilizza i servizi delle applicazioni (c#) | Documenti Microsoft
author: rick-anderson
description: "ASP.NET versione 2.0 è stato introdotto una serie di servizi delle applicazioni, che fanno parte di .NET Framework e vengono usati come blocco predefinito di un gruppo di servizi,..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 1e33d1c6-3f9f-4c26-81e2-2a8f8907bb05
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs
msc.type: authoredcontent
ms.openlocfilehash: 030b0bb218ca05ec270b8fb0a9321e31d9ab5180
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="configuring-a-website-that-uses-application-services-c"></a>Configurazione di un sito Web che utilizza i servizi delle applicazioni (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_cs.pdf)

> ASP.NET versione 2.0 è stato introdotto una serie di servizi delle applicazioni, che fanno parte di .NET Framework e vengono usati come un gruppo di servizi di blocchi predefiniti che è possibile utilizzare per aggiungere funzionalità avanzate per l'applicazione web. In questa esercitazione viene illustrato come configurare un sito Web nell'ambiente di produzione, utilizzare i servizi delle applicazioni e i problemi comuni con la gestione degli account utente e ruoli nell'ambiente di produzione.


## <a name="introduction"></a>Introduzione

ASP.NET versione 2.0 è stata introdotta una serie di *servizi delle applicazioni*, che fanno parte di .NET Framework e fungono da un gruppo di blocchi predefiniti di servizi che si possa usare per aggiungere funzionalità avanzate per l'applicazione web. I servizi delle applicazioni includono:

- **L'appartenenza** : un'API per la creazione e gestione degli account utente.
- **Ruoli** : un'API per la classificazione degli utenti in gruppi.
- **Profilo** : un'API per la memorizzazione di contenuto personalizzato, specifico dell'utente.
- **Mappa del sito** : un'API per la definizione di una struttura logica del sito s sotto forma di una gerarchia, che può quindi essere visualizzata tramite i controlli di navigazione, ad esempio menu e controlli di navigazione.
- **Personalizzazione** : un'API per la gestione delle preferenze di personalizzazione, generalmente utilizzate con [ *WebParts*](https://msdn.microsoft.com/en-us/library/e0s9t4ck.aspx).
- **Monitoraggio integrità** : un'API per il monitoraggio delle prestazioni, sicurezza, gli errori e altre metriche di integrità di sistema per un'applicazione web in esecuzione.
  

I servizi delle applicazioni API non vengono associati a un'implementazione specifica. In alternativa, è indicare i servizi delle applicazioni di utilizzare un particolare *provider*, e tale provider implementa il servizio utilizzando una particolare tecnologia. Il provider più comunemente usate per le applicazioni web basati su Internet ospitate in una società di hosting web sono i provider che usano un'implementazione di database di SQL Server. Ad esempio, il `SqlMembershipProvider` è un provider per l'API di appartenenze che archivia informazioni sull'account utente in un database di Microsoft SQL Server.

Utilizzando i servizi delle applicazioni e i provider SQL Server consente di aggiungere alcuni problemi quando si distribuisce l'applicazione. Per iniziare, gli oggetti di database di servizi di applicazione devono essere creati in modo corretto nel database di sviluppo e produzione e inizializzati in modo appropriato. Sono anche importanti impostazioni di configurazione che devono essere apportate.

> [!NOTE]
> I servizi delle applicazioni API sono state progettate con la [ *modello provider*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), un modello di progettazione che consente un'API s dettagli di implementazione deve essere fornito in fase di esecuzione. .NET Framework viene fornito con un numero di provider di servizi di applicazione che può essere utilizzato, ad esempio il `SqlMembershipProvider` e `SqlRoleProvider`, quali sono i provider per l'appartenenza e ruoli API che utilizzano un Server SQL database implementazione. È inoltre possibile creare e plug-in un provider personalizzato. In realtà, l'applicazione web di recensioni già contiene un provider personalizzato per l'API di mappa del sito (`ReviewSiteMapProvider`), che costruisce la mappa del sito dai dati di `Genres` e `Books` nelle tabelle del database.


In questa esercitazione inizia con un'occhiata a come estesa l'applicazione web recensioni per utilizzare l'appartenenza e l'API di ruoli. Quindi illustra in dettaglio la distribuzione di un'applicazione web che utilizza i servizi delle applicazioni con un'implementazione di un database di SQL Server e si conclude con la risoluzione di problemi comuni di gestione di account utente e ruoli nell'ambiente di produzione.

## <a name="updates-to-the-book-reviews-application"></a>Aggiornamenti all'applicazione revisioni Rubrica

Negli ultimi due esercitazioni che l'applicazione web revisioni del libro è stato aggiornato da un sito Web statico a un'applicazione web dinamica, basati sui dati completo di un set di pagine di amministrazione per la gestione di generi e commenti. Tuttavia, in questa sezione di amministrazione non è protetto, qualsiasi utente che conosce l'URL della pagina Amministrazione (o tentativi) può walzer e creare, modificare o eliminare le revisioni sono disponibili sul sito. Un modo comune per proteggere determinate parti di un sito Web consiste nell'implementare gli account utente e quindi usare le regole di autorizzazione URL per limitare l'accesso a determinati utenti o ruoli. L'applicazione web recensioni disponibile per il download in questa esercitazione supporta i ruoli e account utente. Dispone di un singolo ruolo definito denominato amministratore e solo gli utenti in questo ruolo possono accedere alle pagine di amministrazione.

> [!NOTE]
> I tre account utente creato nell'applicazione web recensioni: Scott Jisun e Alice. Tutti e tre gli utenti hanno la stessa password: **password!** Scott e Jisun assegnato il ruolo di amministratore, Alice non lo è. Le pagine di amministrazione non s del sito sono ancora accessibili agli utenti anonimi. Ovvero, non è necessaria accedere a visitare il sito, a meno che non si desidera amministrare, nel qual caso è necessario accedere come utente nel ruolo di amministratore.


Pagina master recensioni applicazione s è stata aggiornata per includere un'interfaccia utente differente per gli utenti anonimi e autenticati. Se un utente anonimo visita il sito vede un collegamento di accesso nell'angolo superiore destro. Un utente autenticato vede il messaggio "Bentornato, *username*!" e un collegamento per la disconnessione. Esiste anche una pagina di accesso s (`~/Login.aspx`), che contiene un controllo di accesso Web che fornisce l'interfaccia utente e la logica per l'autenticazione di un visitatore. Solo gli amministratori possono creare nuovi account. (Sono presenti pagine per la creazione e la gestione di account utente di `~/Admin` cartella.)

### <a name="configuring-the-membership-and-roles-apis"></a>La configurazione di appartenenze e ruoli API

L'applicazione web recensioni utilizza l'appartenenza e l'API di ruoli per supportare gli account utente e gruppo di utenti in ruoli (vale a dire, il ruolo di amministratore). Il `SqlMembershipProvider` e `SqlRoleProvider` vengono utilizzate le classi provider perché si desidera archiviare informazioni sui ruoli e account in un database di SQL Server.

> [!NOTE]
> In questa esercitazione non deve essere un esame dettagliato la configurazione di un'applicazione web per supportare l'appartenenza e l'API di ruoli. Per dare un'occhiata approfondita queste API e i passaggi da eseguire per configurare un sito Web a questo scopo, leggere la [ *esercitazioni di sicurezza del sito Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Per utilizzare i servizi delle applicazioni con un database di SQL Server, che è innanzitutto necessario aggiungere le informazioni sui ruoli memorizzati gli oggetti di database utilizzati da questi provider per il database in cui si vuole che l'account utente. Questi oggetti di database richiesti includono un'ampia gamma di tabelle, viste e stored procedure. Se non specificato diversamente, il `SqlMembershipProvider` e `SqlRoleProvider` classi di provider di utilizzano un database di SQL Server Express Edition denominato `ASPNETDB` si trova in s applicazione `App_Data` cartella; se tale database non esiste, viene creato automaticamente con gli oggetti di database necessari da questi provider in fase di esecuzione.

È possibile, e in genere ideale per creare l'applicazione Servizi nello stesso database in cui il sito Web s specifici dell'applicazione vengono archiviati gli oggetti di database. .NET Framework viene fornito con uno strumento denominato `aspnet_regsql.exe` che installa gli oggetti di database in un database specificato. Dichiaro di aver effettuato-ahead e utilizzato questo strumento per aggiungere questi oggetti per il `Reviews.mdf` database il `App_Data` cartella (database di sviluppo). Vedremo come utilizzare questo strumento più avanti in questa esercitazione, quando si aggiungono questi oggetti nel database di produzione.

Se si aggiunge l'applicazione Servizi gli oggetti di database a un database diverso da `ASPNETDB` sarà necessario personalizzare il `SqlMembershipProvider` e `SqlRoleProvider` provider classi configurazioni in modo che utilizzino il database appropriato. Per personalizzare il provider di appartenenze aggiungere un [  *&lt;appartenenza&gt; elemento* ](https://msdn.microsoft.com/en-us/library/1b9hw62f.aspx) all'interno di `<system.web>` sezione `Web.config`; utilizzare il [  *&lt;roleManager&gt; elemento* ](https://msdn.microsoft.com/en-us/library/ms164660.aspx) per configurare il provider di ruoli. Nel frammento seguente è tratto dall'applicazione s recensioni `Web.config` e Mostra le impostazioni di configurazione per l'appartenenza e l'API di ruoli. Si noti che sia registrato un nuovo provider - `ReviewMembership` e `ReviewRole` -che utilizzano il `SqlMembershipProvider` e `SqlRoleProvider` provider, rispettivamente.

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample1.xml)]

Il `Web.config` file s `<authentication>` anche l'elemento è stato configurato per supportare l'autenticazione basata su form.
  

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>Limitare l'accesso alle pagine di amministrazione

ASP.NET consente di concedere o negare l'accesso a un determinato file o una cartella dall'utente o ruolo tramite il relativo *autorizzazione URL* funzionalità. (Illustrata brevemente l'autorizzazione URL nel *principali differenze tra IIS e il Server di sviluppo ASP.NET* esercitazione e la visualizzazione come IIS e il Server di sviluppo ASP.NET applicare le regole di autorizzazione URL diversi per statico rispetto al contenuto dinamico.) Poiché si desidera impedire l'accesso per il `~/Admin` cartella ad eccezione di tali utenti nel ruolo di amministratore, è necessario aggiungere regole di autorizzazione URL in questa cartella. In particolare, le regole di autorizzazione URL necessario consentire agli utenti nel ruolo di amministratore e tutti gli altri utenti di negazione. Questa operazione viene eseguita aggiungendo un `Web.config` file per il `~/Admin` cartella con il seguente contenuto:

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample3.xml)]

Per ulteriori informazioni sulle funzionalità di autorizzazione URL ASP.NET s e come usarlo per scrivere le regole di autorizzazione per gli utenti e per i ruoli, assicurarsi di leggere il [ *autorizzazione basata sull'utente* ](../../older-versions-security/membership/user-based-authorization-cs.md) e [ *Autorizzazione basata sui ruoli* ](../../older-versions-security/roles/role-based-authorization-cs.md) esercitazioni da my [ *esercitazioni di sicurezza del sito Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

## <a name="deploying-a-web-application-that-uses-application-services"></a>Distribuzione di un'applicazione Web che utilizza i servizi delle applicazioni

Quando si distribuisce un sito Web che utilizza i servizi delle applicazioni e un provider che archivia le informazioni sui servizi dell'applicazione in un database, è fondamentale che gli oggetti di database necessari per i servizi delle applicazioni creati nel database di produzione. Inizialmente il database di produzione non contiene tali oggetti, questa operazione quando l'applicazione viene distribuita prima (o quando viene distribuito per la prima volta dopo aver aggiunto i servizi delle applicazioni), è necessario eseguire passaggi aggiuntivi per ottenere questi oggetti di database necessari sul database di produzione.

Un altro problema può verificarsi quando si distribuisce un sito Web che utilizza i servizi delle applicazioni, se si intende replicare gli account utente creati nell'ambiente di sviluppo all'ambiente di produzione. A seconda della configurazione di appartenenze e ruoli, è possibile che anche se si copiano correttamente gli account utente creati in un ambiente di sviluppo di database di produzione, questi utenti non possono effettuare l'accesso all'applicazione web nell'ambiente di produzione. Viene esaminata la causa del problema e viene illustrato come impedire che si verifichi.

ASP.NET viene fornito con un accessorio [ *strumento Amministrazione sito Web (WSAT)* ](https://msdn.microsoft.com/en-us/library/yy40ytx0.aspx) che può essere avviato da Visual Studio e consente all'utente di regole di autorizzazione, ruoli e account da gestire tramite basata sul web interfaccia. Sfortunatamente, il WSAT funziona solo per i siti Web locale, vale a dire che non è possibile utilizzare per la gestione remota degli account utente, ruoli e le regole di autorizzazione per l'applicazione web nell'ambiente di produzione. Esamineremo diversi modi per implementare un comportamento simile a WSAT dal sito Web di produzione.

### <a name="adding-the-database-objects-using-aspnetregsqlexe"></a>Aggiunta di oggetti di Database mediante aspnet\_regsql.exe

Il *distribuzione di un Database* esercitazione viene illustrato come copiare le tabelle e i dati dal database di sviluppo nel database di produzione, e queste tecniche certamente possono essere utilizzate per copiare gli oggetti di database di servizi di applicazione per il database di produzione. Un'altra opzione consiste di `aspnet_regsql.exe` uno strumento che aggiunge o rimuove gli oggetti di database di servizi di applicazione da un database.

> [!NOTE]
> Il `aspnet_regsql.exe` strumento crea gli oggetti di database in un database specificato. Non non eseguirne la migrazione di dati in tali oggetti di database dal database di sviluppo nel database di produzione. Se non si desidera copiare le informazioni di account e il ruolo utente nel database di sviluppo di database di produzione, utilizzare le tecniche illustrate nel *distribuzione di un Database* esercitazione.


Consente di esaminare come aggiungere gli oggetti di database per il database di produzione utilizzando s il `aspnet_regsql.exe` dello strumento. Per iniziare, aprire Esplora risorse e passare alla directory .NET Framework versione 2.0 nel computer in uso, %WINDIR%\ Microsoft.NET\Framework\v2.0.50727. Si noteranno le `aspnet_regsql.exe` strumento. Questo strumento può essere utilizzato dalla riga di comando, ma include anche un'interfaccia utente grafica. Fare doppio clic su di `aspnet_regsql.exe` file per avviare il componente con interfaccia grafica.

Lo strumento si avvia visualizzando una schermata che spiega lo scopo. Fare clic su Avanti per passare alla schermata "Selezionare un'opzione di installazione", come illustrato nella figura 1. Da qui è possibile scegliere di aggiungere i servizi delle applicazioni, gli oggetti di database o rimuoverli da un database. Poiché si desidera aggiungere questi oggetti nel database di produzione, selezionare l'opzione "Configura SQL Server per i servizi applicativi" e fare clic su Avanti.


[![Scegliere di configurare SQL Server per i servizi delle applicazioni](configuring-a-website-that-uses-application-services-cs/_static/image2.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image1.jpg)

**Figura 1**: scegliere di configurare SQL Server per i servizi applicativi ([fare clic per visualizzare l'immagine ingrandita](configuring-a-website-that-uses-application-services-cs/_static/image3.jpg))


In "Selezionare il Server e Database" schermata verranno richieste informazioni per la connessione al database. Immettere il nome del database è fornito dalla società di hosting web, le credenziali di sicurezza e il server di database e fare clic su Avanti.

> [!NOTE]
> Dopo aver immesso le credenziali e il server di database si potrebbe verificarsi un errore quando si espande l'elenco di riepilogo a discesa del database. Il `aspnet_regsql.exe` strumento query il `sysdatabases` tabella di sistema per recuperare un elenco di database nel server, ma alcune web che ospita i server di database di bloccare le società in modo che queste informazioni non sono disponibile pubblicamente. Se si verifica questo errore è possibile digitare il nome del database direttamente nell'elenco a discesa.


[![Specificare lo strumento con le informazioni di connessione del Database s](configuring-a-website-that-uses-application-services-cs/_static/image5.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image4.jpg)

**Figura 2**: fornire informazioni di connessione s strumento con il Database ([fare clic per visualizzare l'immagine ingrandita](configuring-a-website-that-uses-application-services-cs/_static/image6.jpg))


La schermata successiva sono riepilogate le azioni che verranno eseguite, vale a dire che gli oggetti di database di servizi applicazione sta per essere aggiunto al database specificato. Fare clic su Avanti per completare questa azione. Dopo alcuni istanti, viene visualizzata la schermata finale, notare che sono stati aggiunti gli oggetti di database (vedere la figura 3).


[![Operazione completata Gli oggetti di Database di servizi applicazione sono stati aggiunti al Database di produzione](configuring-a-website-that-uses-application-services-cs/_static/image8.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image7.jpg)

**Figura 3**: esito positivo. L'applicazione Servizi di Database sono oggetti aggiunti al Database di produzione ([fare clic per visualizzare l'immagine ingrandita](configuring-a-website-that-uses-application-services-cs/_static/image9.jpg))


Per verificare che gli oggetti di database di servizi applicazione sono stati aggiunti al database di produzione, aprire SQL Server Management Studio e connettersi al database di produzione. Come illustrato nella figura 4, dovrebbe essere tabelle di database dell'applicazione servizi del database, `aspnet_Applications`, `aspnet_Membership`, `aspnet_Users`e così via.


[![Verificare che gli oggetti di Database sono stati aggiunti al Database di produzione](configuring-a-website-that-uses-application-services-cs/_static/image11.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image10.jpg)

**Figura 4**: verificare che gli oggetti di Database sono stati aggiunti al Database di produzione ([fare clic per visualizzare l'immagine ingrandita](configuring-a-website-that-uses-application-services-cs/_static/image12.jpg))


È necessario utilizzare il `aspnet_regsql.exe` strumento quando si distribuisce l'applicazione web per la prima volta o per la prima volta, dopo avere avviato utilizzando i servizi delle applicazioni. Dopo aver installato questi oggetti di database nel database di produzione vengono acquisite t dovranno essere nuovamente aggiunto o modificato.

### <a name="copying-user-accounts-from-development-to-production"></a>Copia gli account utente dallo sviluppo alla produzione

Quando si utilizza il `SqlMembershipProvider` e `SqlRoleProvider` classi provider per archiviare le informazioni sui servizi dell'applicazione in un database di SQL Server, le informazioni di account e il ruolo di utente vengono archiviate in un'ampia gamma di tabelle di database, tra cui `aspnet_Users`, `aspnet_Membership`, `aspnet_Roles`, e `aspnet_UsersInRoles`, tra gli altri. Se durante lo sviluppo si creano gli account utente nell'ambiente di sviluppo è possibile replicare gli account utente nell'ambiente di produzione copiando i record corrispondenti dalle tabelle di database applicabile. Se la pubblicazione guidata Database è stata utilizzata per distribuire gli oggetti di database di servizi di applicazione che si sia scelto anche per copiare i record, con la conseguenza di account utente creato in fase di sviluppo per essere in produzione. Tuttavia, a seconda delle impostazioni di configurazione, è possibile che gli utenti i cui account sono stati creati in fase di sviluppo e copiate nell'ambiente di produzione sono in grado di accedere dal sito Web di produzione. Cosa offre?

Il `SqlMembershipProvider` e `SqlRoleProvider` classi provider sono state progettate in modo che un singolo database può essere utilizzato come archivio utente, per dispongono di più applicazioni, in cui ogni applicazione può, in teoria, gli utenti con sovrapposizione dei nomi utente e i ruoli con lo stesso nome. Per consentire questa flessibilità, il database conserva un elenco di applicazioni nel `aspnet_Applications` tabella e ogni utente è associata a una di queste applicazioni. In particolare, il `aspnet_Users` tabella include un `ApplicationId` colonna che collega ogni utente a un record di `aspnet_Applications` tabella.

Oltre al `ApplicationId` colonna, il `aspnet_Applications` tabella include inoltre un `ApplicationName` colonna che fornisce un nome più adatto al personale per l'applicazione. Quando un sito Web tenta di utilizzare un account utente, ad esempio la convalida delle credenziali un utente s dalla pagina di accesso, è necessario indicare la `SqlMembershipProvider` classe applicazione da utilizzare. In genere avviene questo fornendo il nome dell'applicazione e il valore deriva dalla configurazione del provider s in `Web.config` , in particolare tramite il `applicationName` attributo.

Ma cosa accade se il `applicationName` attributo non viene specificato `Web.config`? In questo caso l'appartenenza al sistema utilizza il percorso radice dell'applicazione come il `applicationName` valore. Se il `applicationName` attributo non è impostato in modo esplicito `Web.config`, quindi, è possibile che l'ambiente di sviluppo e produzione usare una radice dell'applicazione diverso e pertanto verrà associate a una diversa applicazione nomi dei servizi dell'applicazione. Se si verifica una mancata corrispondenza di tali utenti creati nell'ambiente di sviluppo avrà un `ApplicationId` valore che non corrisponde con il `ApplicationId` valore per l'ambiente di produzione. Il risultato è che gli utenti-acquisiti t potranno eseguire l'accesso.

> [!NOTE]
> Se è necessario in questo caso - con account utente copiato nell'ambiente di produzione con una mancata corrispondenza `ApplicationId` valore - è possibile scrivere una query per aggiornare questi corretto `ApplicationId` i valori per il `ApplicationId` utilizzata in produzione. Una volta aggiornato, gli utenti il cui account sono stati creati nell'ambiente di sviluppo ora sarebbe in grado di accedere all'applicazione web in produzione.


Buone notizie sono disponibile un semplice passaggio da eseguire per assicurarsi che entrambi gli ambienti usino gli stessi `ApplicationId` - impostare in modo esplicito il `applicationName` attributo `Web.config` per tutti i provider di servizi di applicazione. È impostato in modo esplicito il `applicationName` attributo "BookReviews" nel `<membership>` e `<roleManager>` elementi come questo frammento di codice da `Web.config` Mostra.

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample4.xml)]

Per ulteriori informazioni sull'impostazione di `applicationName` attributo e la logica alla base, fare riferimento a [ *Scott Guthrie* ](https://weblogs.asp.net/scottgu/) post di blog s [ *sempre imposta applicationName Quando si configura l'appartenenza ASP.NET e altri provider di proprietà*](https://weblogs.asp.net/scottgu/443634).

### <a name="managing-user-accounts-in-the-production-environment"></a>Gestione degli account utente nell'ambiente di produzione

La strumento Amministrazione sito Web di ASP.NET (WSAT) semplifica creare e gestire gli account utente, definire e applicare i ruoli e specificare le regole di autorizzazione basata sui ruoli e utenti. È possibile avviare il WSAT da Visual Studio a Esplora soluzioni e scegliendo l'icona di configurazione di ASP.NET oppure selezionando i menu sito Web o il progetto e selezionando la voce di menu configurazione di ASP.NET. Sfortunatamente, il WSAT funzionano solo con i siti Web locale. Pertanto, è possibile utilizzare il WSAT dalla propria workstation per gestire il sito Web nell'ambiente di produzione.

Buone notizie sono che tutte le funzionalità esposte fornite dal WSAT è disponibile a livello di codice tramite l'appartenenza e ruoli API; Inoltre, molte delle schermate di WSAT utilizzare i controlli standard relative all'accesso di ASP.NET. In breve, è possibile aggiungere le pagine ASP.NET per il sito Web che offrono le funzionalità di gestione necessarie.

Tenere presente che un'esercitazione precedente aggiornato l'applicazione web recensioni per includere un `~/Admin` le cartelle e questo è stato configurato per consentire agli utenti nel ruolo di amministratore. È stata aggiunta una pagina in tale cartella denominata `CreateAccount.aspx` da cui un amministratore può creare un nuovo account utente. Questa pagina utilizza il controllo CreateUserWizard per visualizzare la logica di back-end e l'interfaccia utente per la creazione di un nuovo account utente. Novità più, personalizzate del controllo per includere una casella di controllo che richiede se il nuovo utente deve essere aggiunti anche al ruolo di amministratore (vedere Figura 5). Con un minimo di lavoro è possibile compilare un set personalizzato di pagine che implementa le attività correlate alla gestione utente e il ruolo che sarebbero altrimenti essere fornite dal WSAT.

> [!NOTE]
> Per ulteriori informazioni sull'utilizzo l'appartenenza e l'API di ruoli e i controlli Web di ASP.NET relative all'accesso, assicurarsi di leggere il [ *esercitazioni di sicurezza del sito Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Per altre informazioni sulla personalizzazione del controllo CreateUserWizard, consultare il [ *creazione degli account utente* ](../../older-versions-security/membership/creating-user-accounts-cs.md) e [ *l'archiviazione di informazioni utente aggiuntive* ](../../older-versions-security/membership/storing-additional-user-information-cs.md) esercitazioni o check-out [ *Erich Peterson* ](http://www.erichpeterson.com/) articolo s [ *personalizzazione del controllo CreateUserWizard* ](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx).


[![Gli amministratori possono creare nuovi account utente](configuring-a-website-that-uses-application-services-cs/_static/image14.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image13.jpg)

**Figura 5**: gli amministratori possono creare nuovi account utente ([fare clic per visualizzare l'immagine ingrandita](configuring-a-website-that-uses-application-services-cs/_static/image15.jpg))


Se è necessario che tutte le funzionalità di estrazione WSAT [ *in sequenza il proprio strumento Amministrazione sito Web*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx), in cui l'autore Dan Clem vengono illustrati il processo di compilazione di uno strumento simile WSAT personalizzato. Dan condivide il codice sorgente dell'applicazione s (in c#) e vengono fornite istruzioni dettagliate per l'aggiunta al sito Web ospitato.

## <a name="summary"></a>Riepilogo

Quando si distribuisce un'applicazione web che utilizza l'implementazione di database di applicazione dei servizi è necessario assicurarsi che il database di produzione con gli oggetti di database richiesti. Questi oggetti possono essere aggiunti utilizzando le tecniche descritte nel *distribuzione di un Database* esercitazione; in alternativa, è possibile utilizzare il `aspnet_regsql.exe` strumento, come illustrato in questa esercitazione. Altre problematiche è stata presa in considerazione sono incentrate sui concetti di sincronizzazione utilizzato negli ambienti di sviluppo e produzione (che è importante se si desidera che gli utenti e i ruoli creati nell'ambiente di sviluppo sia valido in produzione) e le tecniche per il nome dell'applicazione gestione di utenti e ruoli nell'ambiente di produzione.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [*Strumento di registrazione di ASP.NET SQL Server (aspnet_regsql.exe)*](https://msdn.microsoft.com/en-us/library/ms229862.aspx)
- [*Creazione del Database di servizi di applicazione per SQL Server*](https://msdn.microsoft.com/en-us/library/x28wfk74.aspx)
- [*Creazione dello Schema di appartenenza in SQL Server*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md)
- [*Esaminare l'appartenenza ASP.NET s, ruoli e profilo*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*Proprio strumento Amministrazione sito Web in sequenza*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*Esercitazioni di sicurezza del sito Web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*Panoramica dello strumento di amministrazione sito Web*](https://msdn.microsoft.com/en-us/library/yy40ytx0.aspx)

>[!div class="step-by-step"]
[Precedente](configuring-the-production-web-application-to-use-the-production-database-cs.md)
[Successivo](strategies-for-database-development-and-deployment-cs.md)
