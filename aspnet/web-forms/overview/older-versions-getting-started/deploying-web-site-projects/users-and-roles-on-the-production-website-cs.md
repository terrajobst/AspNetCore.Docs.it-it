---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
title: Utenti e ruoli nel sito Web di produzione (c#) | Microsoft Docs
author: rick-anderson
description: Il sito Web Administration Tool (WSAT) di ASP.NET fornisce un'interfaccia utente basata sul web per la configurazione delle impostazioni di appartenenza e ruoli e per la creazione, modifica, un...
ms.author: aspnetcontent
ms.date: 06/09/2009
ms.assetid: dbc54313-5d05-4285-98b3-726edea6d0c9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
msc.type: authoredcontent
ms.openlocfilehash: f8e714aea2839cc654dac0da310db779f632a26c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802458"
---
<a name="users-and-roles-on-the-production-website-c"></a>Utenti e ruoli nel sito Web di produzione (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_cs.pdf)

> Il sito Web Administration Tool (WSAT) di ASP.NET fornisce un'interfaccia utente basata sul web per la configurazione delle impostazioni di appartenenza e ruoli e per la creazione, modifica ed eliminazione di utenti e ruoli. Sfortunatamente, il WSAT funziona solo quando visitati da localhost, il che significa che non è possibile raggiungere lo strumento di amministrazione del sito Web di produzione tramite il browser. La buona notizia è che non esistono soluzioni alternative che consentono di gestire utenti e ruoli in produzione. Questa esercitazione illustra queste soluzioni alternative e ad altri utenti.


## <a name="introduction"></a>Introduzione

ASP.NET 2.0 ha introdotto numerosi *servizi delle applicazioni*, che sono una suite di servizi di blocchi predefiniti che è possibile aggiungere all'applicazione web. È stato aggiunto di nuovo i servizi di appartenenza e ruoli nel sito Web le recensioni dei libri il [ *configurazione di un sito Web che usa servizi delle applicazioni* esercitazione](configuring-a-website-that-uses-application-services-cs.md). Il servizio di appartenenza facilita la creazione e alla gestione degli account utente. il servizio dei ruoli offre un'API per la classificazione degli utenti in gruppi. Il sito le recensioni dei libri include tre account utente - Scott, Jisun e Alice - e un singolo ruolo, amministratore, con Scott e Jisun il ruolo di amministratore.

ASP. Servizi delle applicazioni di rete non sono associati a un'implementazione specifica. In alternativa, è indicare i servizi delle applicazioni di utilizzare un determinato *provider*, e che il provider implementa il servizio utilizzando una tecnologia specifica. È stata configurata l'applicazione web le recensioni dei libri da utilizzare il `SqlMembershipProvider` e `SqlRoleProvider` provider per i servizi di appartenenza e ruoli. Questi due provider di archiviazione di informazioni sull'account e il ruolo utente in un database di SQL Server e sono i provider di usati più comune per le applicazioni web basati su Internet ospitate in una società di hosting web.

Un problema comune per gli sviluppatori che usano i servizi di appartenenza e ruoli è la gestione utenti e ai ruoli nell'ambiente di produzione. Come si elimina un account utente dal sito Web di produzione, aggiungere un nuovo ruolo o aggiungere un utente esistente a un ruolo esistente? Questa esercitazione illustra le diverse tecniche per la gestione di utenti e ruoli nel sito Web di produzione.

## <a name="using-the-aspnet-web-site-administration-tool"></a>Utilizzando lo strumento di amministrazione sito Web ASP.NET

ASP.NET include un [strumento Amministrazione sito Web](https://msdn.microsoft.com/library/yy40ytx0.aspx) (WSAT) che rende più semplice per creare e gestire gli account utente e i ruoli e per specificare le regole di autorizzazione basata su utenti e ruoli. Per usare il WSAT, fare clic sull'icona di configurazione di ASP.NET in Esplora soluzioni o passare al menu sito Web o un progetto e scegliere l'opzione di configurazione di ASP.NET. Entrambi gli approcci avvia un browser web e lo indirizza al WSAT in un indirizzo, ad esempio: `http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

Il WSAT è suddivisa in tre sezioni:

- **Sicurezza** -gestire utenti, ruoli e le regole di autorizzazione.
- **ApplicationConfiguration** -gestire i &lt;appSettings&gt; e le impostazioni SMTP da qui. Si può inoltre portare offline l'applicazione e gestire di debug e analisi delle impostazioni da qui, nonché specificare la pagina di errore personalizzato predefinito.
- **ProviderConfiguration** -configurare i provider usati dai servizi dell'applicazione.

La sezione Security (illustrato nella **figura 1**) include i collegamenti per la creazione di nuovi utenti, la gestione degli utenti, creazione e gestione dei ruoli e creazione e gestione delle regole di accesso. Da qui è possibile aggiungere un nuovo ruolo al sistema, eliminare un utente esistente, oppure aggiungere o rimuovere i ruoli da un account utente specifico.

[![](users-and-roles-on-the-production-website-cs/_static/image2.png)](users-and-roles-on-the-production-website-cs/_static/image1.png)

**Figura 1**: la sezione sicurezza WSAT include opzioni per la gestione di utenti e ruoli  
([Fare clic per visualizzare l'immagine con dimensioni normali](users-and-roles-on-the-production-website-cs/_static/image3.png))

Sfortunatamente, il WSAT è accessibile solo in locale. È non è possibile visitare il WSAT nel tuo sito Web di produzione remoto; Se visitano `www.yoursite.com/asp.netwebadminfiles/default.aspx` si ottiene una risposta 404 non trovato. Il codice che attiva il WSAT Usa la `Membership` e `Roles` classi in .NET Framework per creare, modificare ed eliminare utenti e ruoli. Queste classi, consultare le informazioni di configurazione dell'applicazione web per determinare quale provider da usare, nel [ *configurazione di un sito Web che usa servizi delle applicazioni* esercitazione](configuring-a-website-that-uses-application-services-cs.md) si configurare il sito Web le recensioni dei libri da utilizzare il `SqlMembershipProvider` e `SqlRoleProvider` provider. Ciò ha comportato svolgimento aggiunta `<membership>` e `<roleManager>` sezioni a `Web.config`.

[!code-xml[Main](users-and-roles-on-the-production-website-cs/samples/sample1.xml)]

Si noti che il `<membership>` e `<roleManager>` sezioni riferimento le `SqlMembershipProvider` e `SqlRoleProvider` provider in loro `type` attributo, rispettivamente. Questi provider di archiviano l'utente e le informazioni sui ruoli in un database di SQL Server specificato. Il database utilizzato dal provider è specificato dal `connectionStringName` attributo, `ReviewsConnectionString`, definito nel `~/ConfigSections/databaseConnectionStrings.config` file. Si tenga presente che il `databaseConnectionStrings.config` file nell'ambiente di sviluppo contiene la stringa di connessione al database di sviluppo, mentre il `databaseConnectionStrings.config` file sulla produzione contiene la stringa di connessione al database di produzione.

In breve, il WSAT deve essere accessibile in locale nell'ambiente di sviluppo e funziona con le informazioni utente e il ruolo del database specificato nella `databaseConnectionStrings.config` file. Di conseguenza, se si modificano le informazioni sulla stringa di connessione nel `databaseConnectionStrings.config` file nell'ambiente di sviluppo è possibile usare il WSAT in locale per gestire utenti e i ruoli nell'ambiente di produzione.

Per illustrare questa funzionalità, aprire il `databaseConnectionStrings.config` file in Visual Studio nell'ambiente di sviluppo e sostituire la stringa di connessione di database di sviluppo con la stringa di connessione di database di produzione. Quindi avviare il WSAT, andare nella scheda sicurezza e aggiungere un nuovo utente denominato Sam con password "password". (minore di punti esclamativi). **Figura 2** Mostra la schermata WSAT quando si crea questo account.

[![](users-and-roles-on-the-production-website-cs/_static/image5.png)](users-and-roles-on-the-production-website-cs/_static/image4.png)

**Figura 2**: creare un nuovo utente denominato Sam nell'ambiente di produzione  
([Fare clic per visualizzare l'immagine con dimensioni normali](users-and-roles-on-the-production-website-cs/_static/image6.png))

Poiché è stato modificato la stringa di connessione in `databaseConnectionStrings.config` per puntare al server di database di produzione, Sam è stato aggiunto come utente nell'ambiente di produzione. Per verificarlo, modificare la stringa di connessione nel `databaseConnectionStrings.config` file nel database di sviluppo e quindi visitare la `Login.aspx` pagina nell'ambiente di sviluppo. Tenta di accedere come Sam (vedere **figura 3**).

[![](users-and-roles-on-the-production-website-cs/_static/image8.png)](users-and-roles-on-the-production-website-cs/_static/image7.png)

**Figura 3**: non è possibile accedere come Sam nell'ambiente di sviluppo  
([Fare clic per visualizzare l'immagine con dimensioni normali](users-and-roles-on-the-production-website-cs/_static/image9.png))

È possibile accedere come Sam nell'ambiente di sviluppo perché le informazioni dell'account utente non esiste nel database locale. Piuttosto, è stato aggiunto al database di produzione. Per verificarlo, visualizzare il contenuto del `aspnet_Users` tabella nel database di sviluppo e produzione. Nell'ambiente di sviluppo deve essere solo tre record per gli utenti a Scott, Jisun e Alice. Tuttavia, il `aspnet_Users` tabella nel database di produzione ha quattro record: Scott, Jisun, Alice e Guido. Di conseguenza, Sam può accedere tramite il sito Web nell'ambiente di produzione, ma non tramite l'ambiente di sviluppo.

[![](users-and-roles-on-the-production-website-cs/_static/image11.png)](users-and-roles-on-the-production-website-cs/_static/image10.png)

**Figura 4**: Sam può accedere nel sito Web di produzione  
([Fare clic per visualizzare l'immagine con dimensioni normali](users-and-roles-on-the-production-website-cs/_static/image12.png))

> [!NOTE]
> Non dimenticare di modificare la stringa di connessione nel `databaseConnectionStrings.config` file nel database di sviluppo della stringa di connessione al termine funziona con il WSAT in caso contrario, è possibile lavorare con i dati di produzione durante il test del sito attraverso lo sviluppo ambiente. Tenere presente che mentre la tecnica che fin qui illustrati consente di utilizzare il WSAT per gestire in remoto gli utenti e ruoli, le modifiche apportate a una delle altre opzioni di configurazione WSAT (regole di accesso, SMTP impostazioni, debug e tracciatura impostazioni e così via) modifichino il anche`Web.config` file. Di conseguenza, tutte le modifiche apportate alle impostazioni si applicano all'ambiente di sviluppo e non nell'ambiente di produzione.


## <a name="creating-custom-user-and-role-management-web-pages"></a>Creazione utente personalizzata e le pagine Web di gestione di ruolo

Il WSAT fornisce un fuori il sistema di finestra per la gestione di utenti e ruoli, ma può essere avviata unicamente in locale ed è necessario apportare modifiche per le informazioni sulla stringa di connessione per gestire gli utenti e ruoli in produzione. La maggior parte dei siti Web che supportano gli account utente anche includere un numero di utenti e pagine web di amministrazione al ruolo che consentono agli amministratori di gestire utenti e ruoli da pagine all'interno del sito. Tali pagine di amministrazione basata su web rendono molto più semplice gestire utenti e ruoli e sono essenziali per i siti in cui possono essere presenti molti amministratori o gli amministratori che non hanno accesso a o il background tecnico per usare Visual Studio per avviare il WSAT.

ASP.NET include un numero di controlli correlati all'accesso Web incorporati che consentono l'implementazione di molte di queste pagine web amministrativi tanto semplice quanto trascinare. Ad esempio, è possibile creare una pagina per gli amministratori di creare un nuovo account utente trascinare il controllo CreateUserWizard sulla pagina e impostando alcune proprietà. In effetti, la pagina per la creazione di utenti nel WSAT illustrato nella **figura 2** Usa lo stesso controllo CreateUserWizard che è possibile aggiungere alle pagine. Inoltre, le funzionalità di appartenenza e ruoli dei servizi sono disponibili a livello di programmazione tramite le `Membership` e `Roles` classi in .NET Framework. Con queste classi è possibile scrivere codice per creare, modificare ed eliminare utenti e ruoli, nonché aggiungere o rimuovere gli utenti a ruoli, per determinare quali utenti sono in quali ruoli e per eseguire altre attività relative a utenti e ruoli.

Nel [ *configurazione di un sito Web che usa servizi delle applicazioni* esercitazione](configuring-a-website-that-uses-application-services-cs.md) ho aggiunto una pagina per il `Admin` cartella denominata `CreateAccount.aspx`. Questa pagina consente a un amministratore di aggiungere un nuovo account utente al sito e di specificare se l'utente appena creato è il ruolo di amministratore (vedere **figura 5**).

[![](users-and-roles-on-the-production-website-cs/_static/image14.png)](users-and-roles-on-the-production-website-cs/_static/image13.png)

**Figura 5**: gli amministratori possono creare nuovi account utente  
([Fare clic per visualizzare l'immagine con dimensioni normali](users-and-roles-on-the-production-website-cs/_static/image15.png))

Per la creazione di pagine di amministrazione utente e il ruolo, oltre a istruzioni dettagliate sull'uso di un'analisi più approfondita i `Membership` e `Roles` classi e i controlli correlati all'accesso Web di ASP.NET, assicurarsi di leggere il [sicurezza del sito Web Esercitazioni](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Vi sono disponibili informazioni su come creare pagine web per la creazione di nuovi account, la creazione e gestione dei ruoli, assegnare utenti a ruoli e altre comuni attività amministrative.

Per implementare la funzionalità di WSAT sul sito Web di produzione è sempre possibile compilare il proprio serie di pagine web che implementano le funzionalità del WSAT. Per iniziare, consultare il codice sorgente WSAT, che si trova nella cartella `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`. Un'altra opzione consiste nell'usare in alternativa, WSAT di Dan Clem che condivide nel suo articolo, [in sequenza del proprio strumento Amministrazione sito Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx). Dan descrive i lettori attraverso il processo di compilazione di uno strumento simile a WSAT personalizzato, include il codice sorgente dell'applicazione per il download (in c#) e vengono fornite istruzioni dettagliate per l'aggiunta di suo WSAT personalizzati a un sito Web ospitato.

## <a name="summary"></a>Riepilogo

ASP.NET Web Site Administration Tool (WSAT) è utilizzabile in parallelo con i servizi dell'applicazione di appartenenza e ruoli per gestire le informazioni utente e il ruolo per il sito Web. Sfortunatamente, il WSAT è accessibile solo in locale e non può essere visualizzato dal sito Web di produzione. Tuttavia, modificando la stringa di connessione per lo sviluppo ambiente in modo che punti al database di produzione, è possibile usare il WSAT per gestire gli utenti e ruoli nel sito Web di produzione.

Mentre l'approccio WSAT mette a disposizione un modo rapido e semplice per gestire utenti e ruoli, è necessario avviare il WSAT da Visual Studio, nonché le modifiche temporanee per le informazioni sulla stringa di connessione. Il WSAT offre un modo rapido per gestire utenti e ruoli in ambiente di produzione, ma è eccessivamente complesso e non funziona bene per i siti Web con più amministratori o agli amministratori che non dispongono o non si ha familiarità con Visual Studio e il WSAT. Per questi motivi, la maggior parte dei siti Web che supportano gli account utente includono un set di pagine web amministrative. Tale set di pagine web Elimina la necessità per il WSAT e usato dagli utenti amministratori diversi da qualsiasi computer.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Esame di ASP. Appartenenza, ruoli e profilo di rete](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [In sequenza il proprio strumento Amministrazione sito Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Panoramica sullo strumento Amministrazione sito Web](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [Sito Web sicurezza esercitazioni](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Precedente](precompiling-your-website-cs.md)
> [Successivo](asp-net-hosting-options-vb.md)
