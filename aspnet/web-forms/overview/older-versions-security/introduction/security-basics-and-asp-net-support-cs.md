---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
title: Nozioni fondamentali sulla sicurezza e il supporto di ASP.NET (c#) | Documenti Microsoft
author: rick-anderson
description: Questa è la prima esercitazione di una serie di esercitazioni che esaminerà le tecniche per l'autenticazione degli utenti tramite un web form, autorizzare l'accesso alle partic...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/13/2008
ms.topic: article
ms.assetid: 07e15538-2f29-40c6-b2e7-e6115075ac83
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
msc.type: authoredcontent
ms.openlocfilehash: 2b89a8b0dd88505c1d63054db508590c26684158
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889791"
---
<a name="security-basics-and-aspnet-support-c"></a>Nozioni fondamentali sulla sicurezza e il supporto di ASP.NET (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica il PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_cs.pdf)

> Questa è la prima esercitazione di una serie di esercitazioni che esaminerà le tecniche per l'autenticazione degli utenti tramite un web form, autorizzare l'accesso alle pagine specifiche e le funzionalità e la gestione degli account utente in un'applicazione ASP.NET.


## <a name="introduction"></a>Introduzione

Che cos'è il forum una cosa, siti e-commerce, siti Web di posta elettronica online, siti Web dei portali e siti di social network che tutti hanno in comune? Tutte le offerte *gli account utente*. Siti che offrono gli account utente devono fornire un numero di servizi. Come minimo, nuovo visitatori devono essere in grado di creare un account e la restituzione visitatori devono essere in grado di accedere. Tali applicazioni web possano prendere decisioni in base all'utente connesso: alcune pagine o azioni potrebbero essere limitate solo l'accesso degli utenti o a un determinato sottoinsieme di utenti. altre pagine potrebbero mostrare informazioni specifiche per l'utente connesso o potrebbero essere più o meno informazioni a seconda di quale utente visualizza la pagina.

Questa è la prima esercitazione di una serie di esercitazioni che esaminerà le tecniche per l'autenticazione degli utenti tramite un web form, autorizzare l'accesso alle pagine specifiche e le funzionalità e la gestione degli account utente in un'applicazione ASP.NET. Nel corso di queste esercitazioni verranno presi in esame come:

- Identificare e accesso degli utenti a un sito Web
- Utilizzare le pagine ASP. Dell'appartenenza a .NET framework per gestire gli account utente
- Creare, aggiornare ed eliminare gli account utente
- Limitare l'accesso a una pagina web, directory o funzionalità specifiche in base all'utente connesso
- Utilizzare le pagine ASP. Del ruoli .NET framework per associare gli account utente ai ruoli
- Gestire i ruoli utente
- Limitare l'accesso a una pagina web, directory o funzionalità specifiche in base al ruolo dell'utente connesso
- Personalizzare ed estendere ASP. Controlli Web di sicurezza di rete

Queste esercitazioni sono pensate per essere conciso e vengono fornite istruzioni dettagliate con una notevole quantità di schermate guidare il processo in modo visivo. Ogni esercitazione è disponibile in c# e Visual Basic versioni e include il download di codice completo utilizzato. (Prima di questa esercitazione illustra i concetti di sicurezza da un punto di vista di alto livello e pertanto non contiene alcun codice associato).

In questa esercitazione verranno illustrate importanti criteri di protezione e quali funzionalità sono disponibili in ASP.NET per facilitare l'implementazione di autenticazione basata su form, autorizzazione, gli account utente e ruoli. Iniziamo!

> [!NOTE]
> La sicurezza è un aspetto importante di qualsiasi applicazione che si estende su fisico, tecnologico e criteri decisioni e richiedono un elevato livello di informazioni di pianificazione e di dominio. Questa serie di esercitazioni non è come guida per lo sviluppo di applicazioni web in modo sicuro. Invece, si concentra in modo specifico nell'autenticazione basata su form, autorizzazione, gli account utente e ruoli. Mentre alcuni concetti di sicurezza vengono disposti intorno a questi problemi sono discussi in questa serie, altri vengono lasciati intatta.


## <a name="authentication-authorization-user-accounts-and-roles"></a>L'autenticazione, autorizzazione, gli account utente e ruoli

L'autenticazione, autorizzazione, gli account utente e ruoli sono quattro i termini che verranno usati molto spesso in tutta questa serie di esercitazioni, quindi come è possibile richiedere qualche istante rapido per definire questi termini all'interno del contesto di sicurezza di web. In un modello client-server, ad esempio Internet, sono presenti molti scenari in cui il server deve identificare il client effettua la richiesta. *Autenticazione* è il processo di individuazione delle identità del client. Viene definito un client che è stato identificato correttamente *autenticato*. Un client sconosciuto è detto *non autenticate* o *anonimo*.

Sistemi di autenticazione sicuri comportano almeno uno dei tre facet seguenti: [un elemento noto, un oggetto fisico o un elemento sono](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html). La maggior parte delle applicazioni web si basano su un elemento che il client conosce, ad esempio una password o un PIN. Le informazioni utilizzate per identificare un utente - proprio nome utente e password, ad esempio, sono detti *credenziali*. Questa serie di esercitazioni si concentra sulla *autenticazione basata su form*, ovvero un modello di autenticazione in cui gli utenti accedono al sito, fornendo le credenziali in un formato pagina web. Servizio tutti offerto questo tipo di autenticazione prima. Passare a qualsiasi sito di e-commerce. Quando si è pronti per l'estrazione viene chiesto di accedere immettendo il nome utente e password nelle caselle di testo in una pagina web.

Oltre a identificare i client, potrebbe essere necessario un server limitare le risorse o le funzionalità sono accessibili a seconda del client che effettua la richiesta. *Autorizzazione* è il processo volto a determinare se un utente specifico dispone di autorizzazioni sufficienti per accedere a una risorsa specifica o funzionalità.

Oggetto *account utente* è un archivio di mantenimento delle informazioni relative a un utente specifico. Gli account utente minima devono includere informazioni che identificano l'utente, ad esempio il nome di account di accesso dell'utente e password. Con queste informazioni essenziali, gli account utente possono includere, ad esempio: indirizzo di posta elettronica dell'utente. Data e ora di che creazione dell'account; Data e ora che è effettuato l'ultimo accesso. nome e cognome; numero di telefono. e indirizzo di posta elettronica. Quando si utilizza l'autenticazione basata su form, informazioni sull'account utente è in genere archiviati in un database relazionale, ad esempio Microsoft SQL Server.

Applicazioni Web che supportano gli account utente possono facoltativamente gruppo utenti in *ruoli*. Un ruolo è semplicemente un'etichetta che viene applicata a un utente e fornisce un'astrazione per la definizione di regole di autorizzazione e funzionalità a livello di pagina. Ad esempio, un sito Web potrebbe includere un ruolo di amministratore con le regole di autorizzazione che impediscono l'amministratore di accedere a un particolare set di pagine web. Inoltre, un'ampia gamma di pagine che sono accessibili a tutti gli utenti (inclusi utenti non amministratori) potrebbe visualizzare dati aggiuntivi o offrono funzionalità aggiuntive quando visitati dagli utenti nel ruolo Administrators. Utilizzo dei ruoli, è possibile definire le regole di autorizzazione in un ruolo per ruolo anziché utente.

## <a name="authenticating-users-in-an-aspnet-application"></a>L'autenticazione degli utenti in un'applicazione ASP.NET

Quando un utente immette un URL in finestra indirizzo del browser o fa clic su un collegamento, il browser invia una [protocollo HTTP (Hypertext Transfer)](http://en.wikipedia.org/wiki/HTTP) richiesta al server web per il contenuto specificato, ovvero un ASP.NET pagina, un'immagine, JavaScript file o qualsiasi altro tipo di contenuto. Il server web è il compito di restituire il contenuto richiesto. In questo modo, è necessario determinare un numero di operazioni relative alla richiesta, incluso chi ha effettuato la richiesta e se l'identità disponga delle autorizzazioni per recuperare il contenuto richiesto.

Per impostazione predefinita, i browser inviano richieste HTTP che non dispongono di alcun tipo di informazioni di identificazione. Tuttavia, se il browser include informazioni di autenticazione del server web avvia il flusso di lavoro di autenticazione, che tenta di identificare il client effettua la richiesta. I passaggi del flusso di lavoro autenticazione dipendono dal tipo di autenticazione utilizzato dall'applicazione web. ASP.NET supporta tre tipi di autenticazione: Windows, Passport e form. Questa serie di esercitazioni riguarda l'autenticazione basata su form, tuttavia è possibile richiedere un minuto, confrontare e contrapporre flusso di lavoro e gli archivi utente di autenticazione di Windows.

### <a name="authentication-via-windows-authentication"></a>Autenticazione tramite l'autenticazione di Windows

Il flusso di lavoro di autenticazione di Windows Usa una delle tecniche di autenticazione seguenti:

- autenticazione di base
- Autenticazione del digest
- Autenticazione integrata di Windows

Tutti e tre le tecniche di lavoro approssimativamente nello stesso modo: quando un non autorizzati, arriva una richiesta anonima, il server web invia una risposta HTTP che indica l'autorizzazione per continuare è necessaria. Quindi, il browser visualizza una finestra di dialogo modale che richiede l'immissione di nome utente e password (vedere la figura 1). Queste informazioni vengono quindi inviate al server web tramite un'intestazione HTTP.


![Finestra di dialogo modale richiede all'utente le credenziali](security-basics-and-asp-net-support-cs/_static/image1.png)

**Figura 1**: finestra di dialogo modale richiede all'utente le credenziali


Le credenziali fornite vengono convalidate rispetto archivio utente di Windows del server web. Ciò significa che ogni utente autenticato nell'applicazione web deve avere un account di Windows nell'organizzazione. Si tratta di comuni negli scenari intranet. Infatti, quando si utilizza l'autenticazione integrata di Windows in un'impostazione intranet, il browser fornisce automaticamente il server web con le credenziali utilizzate per accedere alla rete, quindi eliminare la finestra di dialogo illustrata nella figura 1. L'autenticazione di Windows è ideale per applicazioni intranet, ma è impraticabile in genere per le applicazioni Internet poiché non si desidera creare gli account di Windows per ogni utente che effettua l'iscrizione al sito.

### <a name="authentication-via-forms-authentication"></a>Autenticazione tramite l'autenticazione basata su form

Autenticazione basata su form, d'altra parte, è ideale per applicazioni web di Internet. Tenere presente che l'autenticazione basata su form identifica l'utente mediante la richiesta di immettere le credenziali tramite un web form. Di conseguenza, quando un utente tenta di accedere a una risorsa non autorizzata, vengono automaticamente reindirizzati alla pagina di accesso in cui possono inserire le proprie credenziali. Le credenziali presentate quindi vengono convalidati rispetto a un archivio utente personalizzato, in genere un database.

Dopo aver verificato le credenziali inviate, un *form il ticket di autenticazione* viene creato per l'utente. Questo ticket indica che l'utente è stato autenticato e include informazioni di identificazione, ad esempio il nome utente. Il ticket di autenticazione form (in genere) viene archiviato come un cookie sul computer client. Pertanto, le successive visite al sito Web includono il ticket di autenticazione form nella richiesta HTTP, consentendo l'applicazione web identificare l'utente quando sono connessi.

La figura 2 mostra il flusso di lavoro autenticazione form da un punto di vista di alto livello. Si noti come le parti di autenticazione e autorizzazione in ASP.NET fungono da due entità separate. Il sistema di autenticazione Form identifica l'utente (o i report che sono anonime). Il sistema di autorizzazione è ciò che determina se l'utente ha accesso alla risorsa richiesta. Se l'utente non autorizzato (come se fossero nella figura 2 quando si tenta di visitare in modo anonimo ProtectedPage.aspx), il sistema di autorizzazione segnala che l'utente viene negata, causando moduli di sistema di autenticazione per reindirizzare automaticamente l'utente alla pagina di accesso.

Dopo che l'utente è connesso correttamente, le richieste HTTP successive includono il ticket di autenticazione form. Il sistema di autenticazione Form identifica semplicemente l'utente, è il sistema di autorizzazione che determina se l'utente può accedere alla risorsa richiesta.


![Il flusso di lavoro autenticazione form](security-basics-and-asp-net-support-cs/_static/image2.png)

**Figura 2**: il flusso di lavoro autenticazione form


Si verrà approfondita l'autenticazione basata su form in modo molto più dettagliato nelle prossime due esercitazioni,[una panoramica dell'autenticazione basata su form](an-overview-of-forms-authentication-cs.md) e [configurazione dell'autenticazione form e argomenti avanzati](forms-authentication-configuration-and-advanced-topics-cs.md). Per ulteriori informazioni su ASP. Opzioni di autenticazione della rete, vedere [autenticazione ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx).

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>Limitare l'accesso alle pagine Web, directory e funzionalità della pagina

In ASP.NET sono disponibili due modalità per determinare se un utente specifico dispone di autorizzazioni sufficienti per accedere a un file specifico o una directory:

- **Autorizzazione del file** - poiché le pagine ASP.NET e servizi web vengono implementati come file che risiedono nel file system del server web, l'accesso a questi file possono essere specificati tramite elenchi di controllo di accesso (ACL). Autorizzazione del file viene utilizzato più di frequente con l'autenticazione di Windows perché gli ACL sono le autorizzazioni valide per gli account di Windows. Quando si utilizza l'autenticazione basata su form, vengono eseguite tutte le richieste a livello di sistema di file e sistema operativo dello stesso account di Windows, indipendentemente dall'utente visita il sito.
- **Autorizzazione URL**-con l'autorizzazione URL, lo sviluppatore della pagina specifica le regole di autorizzazione in Web. config. Queste regole di autorizzazione specificano quali utenti o ruoli sono autorizzati ad accedere o non autorizzati l'accesso a determinate pagine o la directory dell'applicazione.

Autorizzazione del file e l'autorizzazione URL definire le regole di autorizzazione per l'accesso a una particolare pagina ASP.NET o per tutte le pagine ASP.NET in una directory specifica. L'utilizzo di queste tecniche è possibile indicare ASP.NET per negare le richieste a una pagina specifica per un particolare utente o consentire l'accesso a un set di utenti e negare l'accesso a tutti gli altri. Per quanto riguarda gli scenari in cui tutti gli utenti possono accedere alla pagina, ma la funzionalità della pagina dipende dall'utente? Ad esempio, molti siti che supportano gli account utente dispongono di pagine che consentono di visualizzare dati per gli utenti autenticati e gli utenti anonimi o contenuti diversi. Un utente anonimo potrebbe essere visualizzato un collegamento per accedere al sito, mentre un utente autenticato verrà invece visualizzato un messaggio, quali, Bentornato, *Username* insieme a un collegamento per la disconnessione. Un altro esempio: quando si visualizza un elemento in corrispondenza di un sito di vendita all'asta vedrai informazioni diverse a seconda che si tratti di una procedura di asta l'elemento o di uno di essi.

Tali modifiche a livello di pagina possono essere eseguite in modo dichiarativo o a livello di codice. Per visualizzare contenuti diversi per anonimo rispetto agli utenti autenticati, basta trascinare un [controllo LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) nella pagina e immettere il contenuto appropriato nel relativo modelli AnonymousTemplate e LoggedInTemplate. In alternativa, è possibile a livello di programmazione determinare se la richiesta corrente viene autenticata, identità dell'utente e i ruoli appartengono a (se presente). Quindi visualizzare o nascondere colonne nei pannelli o una griglia nella pagina, è possibile utilizzare queste informazioni.

Questa serie include tre esercitazioni che concentrano sulle autorizzazioni. ***Autorizzazione basata sull'utente***esamina come limitare l'accesso a una o più pagine in una directory per gli account utente specifico; ***Autorizzazione basata su ruolo*** fornendo le regole di autorizzazione il ruolo a livello; infine, vengono esaminati i ***la visualizzazione contenuto basato su utente l'accesso attualmente connesso*** esercitazione viene esaminata la modifica di un particolare pagina contenuto e alle funzionalità basate sull'utente visita la pagina. Per ulteriori informazioni su ASP. Opzioni di autorizzazione di rete, vedere [autorizzazione ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx).


## <a name="user-accounts-and-roles"></a>Account utente e ruoli

ASP. Autenticazione basata su form di rete offre un'infrastruttura per gli utenti di accedere a un sito e lo stato autenticato memorizzate in occasione di pagina. E l'autorizzazione URL offre un framework per la limitazione dell'accesso a specifici file o cartelle in un'applicazione ASP.NET. Nessuna delle due funzionalità, tuttavia, fornisce un mezzo per l'archiviazione delle informazioni sull'account utente o la gestione dei ruoli.

Prima di ASP.NET 2.0, gli sviluppatori sono responsabili della creazione di un utente e il ruolo Negozio. Fossero anche l'hook per la progettazione di interfacce utente e scrivere il codice utente essenziali relative all'account di pagine come pagina di accesso e la pagina per creare un nuovo account, tra gli altri. Senza un framework di account utente predefiniti in ASP.NET, ogni sviluppatore dovevano arrivano al proprio decisioni di progettazione su domande, ad esempio, gli account utente di implementazione come memorizzare le password o altre informazioni riservate? e le linee guida è necessario imporre relative a livello di attendibilità e la lunghezza della password?

Oggi, l'implementazione di account utente in un'applicazione ASP.NET è molto più semplice grazie al *framework appartenenza* e i controlli Web di accesso predefinito. Il framework di appartenenza è un numero limitato di classi di [dello spazio dei nomi System.Web.Security](https://msdn.microsoft.com/library/system.web.security.aspx) che forniscono funzionalità per l'esecuzione di attività relative all'account utente essenziali. La classe principale il framework di appartenenza è il [la classe di appartenenza](https://msdn.microsoft.com/library/system.web.security.membership.aspx), che dispone di metodi, ad esempio:

- CreateUser
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- ValidateUser

Il framework di appartenenza utilizza il [modello provider](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), che separa in modo netto l'API del framework di appartenenza dall'implementazione. Questo consente agli sviluppatori di utilizzare un'API comune, ma consente di utilizzare un'implementazione soddisfa le esigenze personalizzato della propria applicazione. In breve, la classe di appartenenza definisce la funzionalità essenziale di framework (metodi, proprietà ed eventi), ma non fornisce effettivamente i dettagli di implementazione. I metodi della classe di appartenenza richiamano invece il provider configurato, che è il componente che esegue il lavoro effettivo. Ad esempio, quando viene richiamato il metodo di CreateUser della classe di appartenenza, la classe di appartenenza non riconosce i dettagli dell'archivio dell'utente. Non è chiaro se gli utenti sono mantenuti in un database o in un file XML o in un altro archivio. La classe di appartenenza esamina la configurazione dell'applicazione web per determinare quale il provider per delegare la chiamata a, e tale classe provider è responsabile della creazione effettivamente il nuovo account utente nell'archivio utente appropriato. Questa interazione come illustrata nella figura 3.

Microsoft sono disponibili due classi di provider di appartenenze in .NET Framework:

- [ActiveDirectoryMembershipProvider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) -implementa l'API di appartenenza nei server Active Directory e Active Directory ADAM Application Mode ().
- [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) -implementa l'API di appartenenza in un database di SQL Server.

Questa serie di esercitazioni incentrato esclusivamente sul SqlMembershipProvider.


[![Il modello consente diverse implementazioni del Provider per essere facilmente collegato nel Framework di&lt;/ strong&gt;](security-basics-and-asp-net-support-cs/_static/image4.png)](security-basics-and-asp-net-support-cs/_static/image3.png)

**Figura 03**: il modello consente diverse implementazioni del Provider per essere facilmente collegato nel Framework di ([fare clic per visualizzare l'immagine ingrandita](security-basics-and-asp-net-support-cs/_static/image5.png))


Il vantaggio del modello di provider è che implementazioni alternative possono essere sviluppate da Microsoft, i fornitori di terze parti o singoli sviluppatori e collegate senza problemi nel framework di appartenenza. Ad esempio, Microsoft ha rilasciato [un provider di appartenenze per i database Microsoft Access](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi). Per ulteriori informazioni sui provider di appartenenze, consultare il [Toolkit del Provider](https://msdn.microsoft.com/asp.net/aa336558.aspx), che include una procedura dettagliata di provider di appartenenze, il provider personalizzato di esempio, più di 100 pagine della documentazione sul modello di provider e eseguire il codice sorgente per i provider di appartenenze predefiniti (vale a dire, ActiveDirectoryMembershipProvider e SqlMembershipProvider).

ASP.NET 2.0 ha introdotto anche il framework di ruoli. Ad esempio il framework di appartenenza, il framework di ruoli viene compilato nella parte superiore del modello di provider. L'API viene esposta tramite il [classe ruoli](https://msdn.microsoft.com/library/system.web.security.roles.aspx) e .NET Framework sono disponibili tre classi di provider:

- [AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) -gestisce le informazioni sui ruoli in un archivio criteri di gestione autorizzazioni, ad esempio Active Directory o ADAM.
- [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) -implementa i ruoli in un database di SQL Server.
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) -associa le informazioni sui ruoli in base a gruppo di Windows del visitatore. Questo metodo viene generalmente utilizzato con l'autenticazione di Windows.

Questa serie di esercitazioni incentrato esclusivamente sul SqlRoleProvider.

Poiché il modello di provider include una singola API frontale (classi di appartenenza e ruoli), è possibile compilare funzionalità API senza doversi preoccupare dei dettagli di implementazione, quelli gestiti dai provider selezionato nella pagina Developer. Questa API unificata consente di Microsoft e fornitori di terze parti compilare i controlli Web che si interfacciano con il framework di appartenenza e ruoli. ASP.NET viene fornito con un numero di [i controlli Web di accesso](https://msdn.microsoft.com/library/ms178329.aspx) per l'implementazione di interfacce utente più account utente comuni. Ad esempio, il [controllo di accesso](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) richieda all'utente le credenziali, li convalida e quindi li registra in tramite l'autenticazione basata su form. Il [controllo LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) sono disponibili modelli per la visualizzazione di diversi markup agli utenti anonimi e autenticati o markup diverso in base al ruolo dell'utente. E [controllo CreateUserWizard](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx) fornisce un'interfaccia utente dettagliate per la creazione di un nuovo account utente.

Nel sistema il i vari controlli di accesso interagiscono con il framework di appartenenza e ruoli. La maggior parte dei controlli di accesso possono essere implementati senza dover scrivere una singola riga di codice. Questi controlli in modo più dettagliato verrà esaminato in esercitazioni future, comprese le tecniche per estendere e personalizzare le relative funzionalità.

## <a name="summary"></a>Riepilogo

Tutte le applicazioni web che supportano gli account utente richiedono funzionalità simili, ad esempio: la possibilità agli utenti di accedere e i relativi log nello stato memorizzato in una pagina visite; una pagina web per poter creare un account. e la possibilità per lo sviluppatore della pagina per specificare quali risorse, i dati e funzionalità sono disponibili per gli utenti o ruoli. Le attività di autenticare e autorizzare gli utenti e di gestione dei ruoli e account utente è molto più semplice eseguire nelle applicazioni ASP.NET grazie all'autenticazione basata su form, l'autorizzazione URL e il framework di appartenenza e ruoli.

Nel corso delle diverse esercitazioni successiva verrà esaminato questi aspetti creando un'applicazione web da zero in modo sequenziale. Nell'esercitazione due verranno analizzati in dettaglio autenticazione basata su form. Verrà spiegato il flusso di lavoro autenticazione form in azione, analizzare il ticket di autenticazione form, discutere problemi di sicurezza e informazioni su come configurare il sistema di autenticazione form - tutti durante la creazione di un'applicazione web che consente ai visitatori di accedere e disconnettersi.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [ASP.NET 2.0 appartenenza, ruoli, autenticazione basata su form e le risorse di sicurezza](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [Linee guida ASP.NET 2.0 sicurezza](https://msdn.microsoft.com/library/ms998258.aspx)
- [Autenticazione ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [Autorizzazione ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Cenni preliminari sui controlli di accesso ASP.NET](https://msdn.microsoft.com/library/ms178329.aspx)
- [Analisi di ASP.NET 2.0 appartenenza, ruoli e profilo](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [In che modo ricerca per categorie: protette del sito mediante l'appartenenza e ruoli?](https://asp.net/learn/videos/video-45.aspx) (Video)
- [Introduzione all'appartenenza](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [MSDN Security Developer Center](https://msdn.microsoft.com/security/default.aspx)
- [Professional ASP.NET 2.0 sicurezza, l'appartenenza e gestione dei ruoli](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (codice ISBN: 978-0-7645-9698-8)
- [Toolkit del provider](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stato in questa esercitazione serie è stato esaminato da diversi validi revisori. Lead revisori per questa esercitazione includono Alicja Maziarz, John Suru e Teresa Murphy. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [avanti](an-overview-of-forms-authentication-cs.md)
