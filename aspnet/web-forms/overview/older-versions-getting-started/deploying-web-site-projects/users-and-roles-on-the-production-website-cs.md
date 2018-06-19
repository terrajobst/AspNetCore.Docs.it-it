---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
title: Utenti e ruoli del sito Web di produzione (c#) | Documenti Microsoft
author: rick-anderson
description: La strumento Amministrazione sito Web di ASP.NET (WSAT) fornisce un'interfaccia utente basata sul web per la configurazione delle impostazioni di appartenenza e ruoli e per la creazione, modifica, un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: dbc54313-5d05-4285-98b3-726edea6d0c9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
msc.type: authoredcontent
ms.openlocfilehash: e3e1165959ae47715e0037db7a3bc6ac58807653
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888292"
---
<a name="users-and-roles-on-the-production-website-c"></a>Utenti e ruoli del sito Web di produzione (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica il PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_cs.pdf)

> La strumento Amministrazione sito Web di ASP.NET (WSAT) fornisce un'interfaccia utente basata sul web per la configurazione delle impostazioni di appartenenza e ruoli e per la creazione, modifica ed eliminazione di utenti e ruoli. Sfortunatamente, il WSAT funziona solo se viene visitato da localhost, vale a dire che non è possibile raggiungere lo strumento di amministrazione del sito Web di produzione tramite il browser. Buone notizie sono che non esistono soluzioni alternative che consentono di gestire utenti e ruoli in produzione. In questa esercitazione vengono esaminati queste soluzioni alternative e altri.


## <a name="introduction"></a>Introduzione

ASP.NET 2.0 ha introdotto un numero di *servizi delle applicazioni*, che sono una suite di servizi di blocchi predefiniti che è possibile aggiungere all'applicazione web. Abbiamo aggiunto nuovamente i servizi di appartenenza e ruoli per il sito Web recensioni il [ *la configurazione di un sito Web che utilizza i servizi delle applicazioni* esercitazione](configuring-a-website-that-uses-application-services-cs.md). Il servizio di appartenenza facilita la creazione e gestione degli account utente. il servizio dei ruoli offre un'API per la classificazione degli utenti in gruppi. Il sito recensioni dispone di tre account utente - Scott, Jisun e: Alice e un singolo ruolo, amministrazione, Scott e Jisun nel ruolo di amministratore.

ASP. I servizi delle applicazioni di rete non vengono associati a un'implementazione specifica. In alternativa, è indicare i servizi delle applicazioni di utilizzare un particolare *provider*, e tale provider implementa il servizio utilizzando una particolare tecnologia. È stato configurato l'applicazione web recensioni per utilizzare il `SqlMembershipProvider` e `SqlRoleProvider` provider per i servizi di appartenenza e ruoli. Questi due provider di archiviare informazioni sull'account e il ruolo utente in un database di SQL Server e il provider più comunemente usate per le applicazioni web basati su Internet ospitate in una società di hosting web.

Un problema comune per gli sviluppatori che utilizzano i servizi di appartenenza e ruoli, gestisce gli utenti e ruoli nell'ambiente di produzione. Come si elimina un account utente dal sito Web di produzione, aggiungere un nuovo ruolo o aggiungere un utente esistente a un ruolo esistente? In questa esercitazione illustra diverse tecniche per la gestione di utenti e ruoli del sito Web di produzione.

## <a name="using-the-aspnet-web-site-administration-tool"></a>Utilizzando lo strumento di amministrazione sito Web ASP.NET

ASP.NET include un [strumento Amministrazione sito Web](https://msdn.microsoft.com/library/yy40ytx0.aspx) (WSAT) che consente di creare e gestire account utente e ruoli e per specificare le regole di autorizzazione basata sui ruoli e utenti. Per utilizzare il WSAT, fare clic sull'icona di configurazione di ASP.NET in Esplora soluzioni, o dal menu sito Web o il progetto e scegliere l'opzione di configurazione di ASP.NET. Entrambi gli approcci consente di avviare un web browser e fanno riferimento a WSAT in un indirizzo, ad esempio: `http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

Il WSAT è suddivisa in tre sezioni:

- **Sicurezza** -gestire utenti, ruoli e le regole di autorizzazione.
- **ApplicationConfiguration** -gestire il &lt;appSettings&gt; e le impostazioni SMTP da qui. È possibile anche disconnettere l'applicazione e la gestione di debug e impostazioni in questa finestra di traccia, nonché specificare la pagina di errore personalizzata predefinita.
- **ProviderConfiguration** -configurare i provider utilizzati per i servizi delle applicazioni.

La sezione sicurezza (illustrato **figura 1**) include i collegamenti per la creazione di nuovi utenti, sulla gestione degli utenti, creare e gestire ruoli e la creazione e la gestione delle regole di accesso. Da qui è possibile aggiungere un nuovo ruolo del sistema, un utente esistente, eliminare o aggiungere o rimuovere ruoli da un particolare account utente.

[![](users-and-roles-on-the-production-website-cs/_static/image2.png)](users-and-roles-on-the-production-website-cs/_static/image1.png)

**Figura 1**: la sezione sicurezza WSAT include opzioni per la gestione di utenti e ruoli  
([Fare clic per visualizzare l'immagine ingrandita](users-and-roles-on-the-production-website-cs/_static/image3.png))

Sfortunatamente, il WSAT è accessibile solo in locale. È non è possibile visitare il WSAT nel sito Web di produzione remoto; Se si visita `www.yoursite.com/asp.netwebadminfiles/default.aspx` si ottiene una risposta 404 non trovato. Il codice che consente di creare l'oggetto utilizza WSAT il `Membership` e `Roles` classi in .NET Framework per creare, modificare ed eliminare utenti e ruoli. Queste classi, consultare le informazioni di configurazione dell'applicazione web per determinare quale il provider da utilizzare; nel [ *la configurazione di un sito Web che utilizza i servizi delle applicazioni* esercitazione](configuring-a-website-that-uses-application-services-cs.md) è il sito Web recensioni all'uso di installazione di `SqlMembershipProvider` e `SqlRoleProvider` provider. Questo significava aggiunta `<membership>` e `<roleManager>` sezioni per `Web.config`.

[!code-xml[Main](users-and-roles-on-the-production-website-cs/samples/sample1.xml)]

Si noti che il `<membership>` e `<roleManager>` sezioni di riferimento di `SqlMembershipProvider` e `SqlRoleProvider` provider nel loro `type` attributo, rispettivamente. Questi provider di archiviano le informazioni sui ruoli utente in un database di SQL Server specificato. Il database utilizzato da questi provider viene specificato per il `connectionStringName` attributo, `ReviewsConnectionString`, definito nel `~/ConfigSections/databaseConnectionStrings.config` file. Tenere presente che il `databaseConnectionStrings.config` file nell'ambiente di sviluppo contiene la stringa di connessione al database di sviluppo, mentre il `databaseConnectionStrings.config` file in produzione contiene la stringa di connessione al database di produzione.

In breve, il WSAT deve essere accessibile in locale dall'ambiente di sviluppo e funziona con le informazioni utente e il ruolo del database specificato nella `databaseConnectionStrings.config` file. Di conseguenza, se si modificano le informazioni sulla stringa di connessione nel `databaseConnectionStrings.config` file nell'ambiente di sviluppo è possibile utilizzare il WSAT in locale per gestire utenti e ruoli nell'ambiente di produzione.

Per illustrare questa funzionalità, aprire il `databaseConnectionStrings.config` file in Visual Studio nell'ambiente di sviluppo e sostituire la stringa di connessione di database di sviluppo con la stringa di connessione di database di produzione. Avviare quindi il WSAT, andare alla scheda sicurezza e aggiungere un nuovo utente denominato Sam con la password "password". (minore le virgolette). **Figura 2** viene visualizzata la schermata di WSAT quando si crea questo account.

[![](users-and-roles-on-the-production-website-cs/_static/image5.png)](users-and-roles-on-the-production-website-cs/_static/image4.png)

**Figura 2**: creare un nuovo utente denominato Sam nell'ambiente di produzione  
([Fare clic per visualizzare l'immagine ingrandita](users-and-roles-on-the-production-website-cs/_static/image6.png))

Poiché è stato modificato la stringa di connessione in `databaseConnectionStrings.config` per puntare al server di database di produzione, Giorgio è stato aggiunto come utente nell'ambiente di produzione. Per verificare questa condizione, modificare la stringa di connessione nel `databaseConnectionStrings.config` file al database di sviluppo e quindi visitare il `Login.aspx` pagina nell'ambiente di sviluppo. Tenta di accedere come Sam (vedere **figura 3**).

[![](users-and-roles-on-the-production-website-cs/_static/image8.png)](users-and-roles-on-the-production-website-cs/_static/image7.png)

**Figura 3**: non è possibile accedere come Sam nell'ambiente di sviluppo  
([Fare clic per visualizzare l'immagine ingrandita](users-and-roles-on-the-production-website-cs/_static/image9.png))

Impossibile accedere come Sam nell'ambiente di sviluppo perché le informazioni sull'account utente non esiste nel database locale. Piuttosto, è stato aggiunto al database di produzione. Per verificarlo, visualizzare il contenuto del `aspnet_Users` tabella nel database di sviluppo e produzione. Nell'ambiente di sviluppo dovrebbero essere presenti solo tre record per gli utenti Scott Jisun e Alice. Tuttavia, il `aspnet_Users` tabella nel database di produzione include quattro record: Scott, Jisun, Alice e Sam. Di conseguenza, Sam possibile accedere tramite il sito Web nell'ambiente di produzione, ma non tramite l'ambiente di sviluppo.

[![](users-and-roles-on-the-production-website-cs/_static/image11.png)](users-and-roles-on-the-production-website-cs/_static/image10.png)

**Figura 4**: Sam può accedere sito Web di produzione  
([Fare clic per visualizzare l'immagine ingrandita](users-and-roles-on-the-production-website-cs/_static/image12.png))

> [!NOTE]
> Non dimenticare di modificare la stringa di connessione nel `databaseConnectionStrings.config` file al database di sviluppo della stringa di connessione al termine utilizzano il WSAT in caso contrario verranno usate con dati di produzione durante il test del sito tramite lo sviluppo ambiente. Tenere presente che, mentre la tecnica che fin qui illustrati consente di utilizzare il WSAT per gestire utenti e ruoli, le modifiche alle altre opzioni di configurazione WSAT (regole di accesso, SMTP impostazioni, debug e traccia impostazioni e così via) modifichino il anche`Web.config` file. Di conseguenza, tutte le modifiche apportate alle impostazioni si applicano all'ambiente di sviluppo e non nell'ambiente di produzione.


## <a name="creating-custom-user-and-role-management-web-pages"></a>Creazione utente personalizzata e le pagine Web di gestione di ruolo

Il WSAT fornisce un timeout del sistema casella per la gestione di utenti e ruoli, ma può essere avviata solo localmente e richiede modifiche per le informazioni sulla stringa di connessione per gestire utenti e ruoli in produzione. La maggior parte dei siti Web che supportano gli account utente includono anche un numero di utenti e pagine web di amministrazione al ruolo che consentono agli amministratori di gestire utenti e ruoli dalle pagine all'interno del sito. Tali pagine di amministrazione basata su web rendono molto più semplice gestire utenti e ruoli e sono essenziali per i siti in cui possono essere presenti molti amministratori o gli amministratori che non hanno accesso a o background tecniche per utilizzare Visual Studio per avviare il WSAT.

ASP.NET include un numero di controlli relative all'accesso Web incorporati che consentono l'implementazione di molte di queste pagine web amministrativi semplice come trascinamento della selezione. Ad esempio, è possibile creare una pagina per gli amministratori di creare un nuovo account utente trascinare il controllo CreateUserWizard la pagina e impostando alcune proprietà. Infatti, la pagina per la creazione di utenti di WSAT illustrato **figura 2** utilizza lo stesso controllo CreateUserWizard che è possibile aggiungere alle pagine. Inoltre, le funzionalità di appartenenza e i ruoli dei servizi sono disponibili a livello di programmazione tramite le `Membership` e `Roles` classi in .NET Framework. Con queste classi è possibile scrivere codice per creare, modificare ed eliminare utenti e ruoli, nonché aggiungere o rimuovere utenti da ruoli, per determinare gli utenti quali ruoli ed eseguire altre attività correlate a utenti e ruoli.

Nel [ *la configurazione di un sito Web che utilizza i servizi delle applicazioni* esercitazione](configuring-a-website-that-uses-application-services-cs.md) è stata aggiunta una pagina per il `Admin` cartella denominata `CreateAccount.aspx`. Questa pagina consente agli amministratori per aggiungere un nuovo account utente al sito e specificare se l'utente appena creato è nel ruolo di amministratore (vedere **figura 5**).

[![](users-and-roles-on-the-production-website-cs/_static/image14.png)](users-and-roles-on-the-production-website-cs/_static/image13.png)

**Figura 5**: gli amministratori possono creare nuovi account utente  
([Fare clic per visualizzare l'immagine ingrandita](users-and-roles-on-the-production-website-cs/_static/image15.png))

Per informazioni più dettagliate alla creazione di pagine di amministrazione utente e il ruolo, insieme a istruzioni dettagliate sull'utilizzo di `Membership` e `Roles` classi e i controlli Web di ASP.NET relative all'accesso, assicurarsi di leggere il [sicurezza del sito Web Esercitazioni](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Vi sono disponibili informazioni su come creare pagine web per la creazione di nuovi account, creazione e gestione dei ruoli, assegnare gli utenti ai ruoli e altre comuni attività amministrative.

Per implementare la funzionalità di WSAT sul sito Web di produzione è sempre possibile compilare la propria serie di pagine web che implementano le funzionalità del WSAT. Per iniziare, estrarre il codice sorgente WSAT, che si trova nella cartella `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`. Un'altra opzione consiste nell'utilizzare in alternativa, WSAT di Dan Clem cui condivide nell'articolo, [in sequenza il proprio strumento Amministrazione sito Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx). Dan illustra lettori attraverso il processo di compilazione di uno strumento simile WSAT personalizzato, contiene il codice sorgente dell'applicazione per il download (in c#) e vengono fornite istruzioni dettagliate per l'aggiunta del suo WSAT personalizzato per un sito Web ospitato.

## <a name="summary"></a>Riepilogo

La strumento Amministrazione sito Web di ASP.NET (WSAT) utilizzabile in combinazione con i servizi di appartenenza e i ruoli applicazione per gestire le informazioni utente e il ruolo per il sito Web. Sfortunatamente, il WSAT è accessibile solo localmente e non può essere visitato dal sito Web di produzione. Tuttavia, modificando la stringa di connessione per lo sviluppo ambiente in modo che punti al database di produzione, è possibile utilizzare il WSAT per gestire utenti e ruoli del sito Web di produzione.

Mentre l'approccio WSAT mette a disposizione un modo rapido e semplice per gestire utenti e ruoli, richiede l'avvio di WSAT da Visual Studio, nonché le modifiche temporanee per le informazioni sulla stringa di connessione. Il WSAT offre un modo rapido per gestire utenti e ruoli in produzione, ma è un'operazione complessa e non funziona per i siti Web con più amministratori o agli amministratori che non dispone o non si ha familiarità con Visual Studio e il WSAT. Per questi motivi, la maggior parte dei siti Web che supportano gli account utente includono un set di pagine di amministrazione web. Questo tipo di un set di pagine web Elimina la necessità per il WSAT e utilizzati dai diversi utenti amministrativi da qualsiasi computer.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Esame ASP. Appartenenza, ruoli e profilo di rete](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [In sequenza il propria strumento Amministrazione sito Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Panoramica dello strumento Amministrazione sito Web](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [Esercitazioni di sicurezza sito Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Precedente](precompiling-your-website-cs.md)
> [Successivo](asp-net-hosting-options-vb.md)
