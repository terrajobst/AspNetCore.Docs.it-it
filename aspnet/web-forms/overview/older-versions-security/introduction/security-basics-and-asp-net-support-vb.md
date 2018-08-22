---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
title: Nozioni fondamentali sulla sicurezza e supporto di ASP.NET (VB) | Microsoft Docs
author: rick-anderson
description: Questa è la prima esercitazione di una serie di esercitazioni che verranno illustrate le tecniche per l'autenticazione ai visitatori tramite un web form, autorizzare l'accesso a Pers...
ms.author: riande
ms.date: 01/13/2008
ms.assetid: ab68a92b-fc81-40a4-a7dc-406625d2c5d4
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
msc.type: authoredcontent
ms.openlocfilehash: c293f4946001d0940c7e12b67f4fbc63cf5ca18c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826371"
---
<a name="security-basics-and-aspnet-support-vb"></a>Nozioni fondamentali sulla sicurezza e supporto di ASP.NET (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_vb.pdf)

> Questa è la prima esercitazione di una serie di esercitazioni che verranno illustrate le tecniche per l'autenticazione ai visitatori tramite un web form, autorizzare l'accesso a determinate pagine e le funzionalità e la gestione degli account utente in un'applicazione ASP.NET.


## <a name="introduction"></a>Introduzione

Che cos'è il forum di una cosa, siti di e-commerce, siti Web online tramite posta elettronica, siti Web dei portali e siti di rete basati su social network che tutti hanno in comune? Tutte queste connessioni offrono *gli account utente*. Siti che offrono gli account utente devono fornire un numero di servizi. Come minimo, visitatori nuovi devono essere in grado di creare un account e i visitatori restituiti devono essere in grado di accedere. Tali applicazioni web possono prendere decisioni basate su utente connesso: alcune pagine o azioni potrebbero essere limitate solo l'accesso degli utenti o a un determinato subset di utenti. altre pagine potrebbero mostrare informazioni specifiche per l'utente connesso e potrebbero mostrare più o meno informazioni, a seconda qual è l'utente visualizza la pagina.

Questa è la prima esercitazione di una serie di esercitazioni che verranno illustrate le tecniche per l'autenticazione ai visitatori tramite un web form, autorizzare l'accesso a determinate pagine e le funzionalità e la gestione degli account utente in un'applicazione ASP.NET. Nel corso delle esercitazioni seguenti verranno esaminate come:

- Identificare e accesso degli utenti a un sito Web
- Utilizzare le pagine ASP. Framework di appartenenza del .NET per gestire gli account utente
- Creare, aggiornare ed eliminare gli account utente
- Limitare l'accesso a una pagina web, directory o funzionalità specifiche in base all'utente connesso
- Utilizzare le pagine ASP. Framework di ruoli del .NET da associare gli account utente ai ruoli
- Gestire i ruoli utente
- Limitare l'accesso a una pagina web, directory o funzionalità specifiche in base al ruolo dell'utente connesso
- Personalizza ed Estendi ASP. Controlli Web di sicurezza di rete

Queste esercitazioni sono pensate per essere concise e vengono fornite istruzioni dettagliate con numerose catture di schermata che illustra il processo in modo visivo. Ogni esercitazione è disponibile nelle versioni di Visual Basic e c# e comprende un download del codice completo utilizzato. (Questa prima esercitazione illustra i concetti di sicurezza da un punto di vista di alto livello e pertanto non contiene alcun codice associato).

In questa esercitazione verranno illustrati i concetti di sicurezza importante e quali funzionalità sono disponibili in ASP.NET per assistere nell'implementazione di autenticazione basata su form, autorizzazione, gli account utente e ruoli. Iniziamo!

> [!NOTE]
> La sicurezza è un aspetto importante di qualsiasi applicazione che si estende su fisica, tecnologica e le decisioni e richiedono un elevato livello di pianificazione e il dominio della Knowledge base di criteri. Questa serie di esercitazioni non è concepita come una Guida per lo sviluppo di applicazioni web sicure. Piuttosto, è incentrato specificamente su autenticazione basata su form, autorizzazione, gli account utente e ruoli. Mentre alcuni concetti relativi alla sicurezza vengono disposti intorno a questi problemi sono illustrati in questa serie, gli altri vengono lasciati non esplorati.


## <a name="authentication-authorization-user-accounts-and-roles"></a>L'autenticazione, autorizzazione, gli account utente e ruoli

L'autenticazione, autorizzazione, gli account utente e i ruoli sono quattro i termini che verranno usati molto spesso in tutta questa serie di esercitazioni in modo che si vuole richiedere qualche istante veloce per definire questi termini all'interno del contesto di sicurezza web. In un modello client-server, ad esempio Internet, esistono molti scenari in cui il server deve identificare il client effettua la richiesta. *Autenticazione* è il processo con cui si accerta l'identità del client. Un client che è stato identificato correttamente è detto *autenticato*. Un client non identificato è detto *non autenticata* oppure *anonimo*.

Sistemi di autenticazione sicuro implicano almeno uno dei tre facet seguenti: [un'informazione nota, un oggetto fisico o qualcosa di sei](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html). La maggior parte delle applicazioni web si basano su un elemento che il client SA, ad esempio una password o un PIN. Le informazioni utilizzate per identificare un utente - proprio nome utente e password, ad esempio - sono dette *credenziali*. Questa serie di esercitazioni illustra *autenticazione basata su form*, ovvero un modello di autenticazione in cui gli utenti accedono al sito, fornendo le credenziali in una pagina web form. Servizio tutti offerto questo tipo di autenticazione prima. Passare a qualsiasi sito di e-commerce. Quando si è pronti per l'estrazione viene chiesto accedere immettendo il nome utente e password nelle caselle di testo in una pagina web.

Oltre a identificare i client, un server potrebbe essere necessario limitare le funzionalità o le risorse sono accessibili a seconda del client che effettua la richiesta. *Autorizzazione* è il processo volto a determinare se un utente specifico dispone dell'autorità per accedere a una risorsa specifica o funzionalità.

Oggetto *account utente* è un archivio di persistenza delle informazioni relative a un utente specifico. Gli account utente devono includere almeno le informazioni che identificano in modo univoco l'utente, ad esempio il nome di account di accesso dell'utente e password. Con queste informazioni essenziali, degli account utente possono includere, ad esempio: indirizzo di posta elettronica dell'utente; Data e ora che è stato creato l'account. Data e ora che è stati effettuati l'ultimo. nome e cognome; numero di telefono; e indirizzo di posta elettronica. Quando si usa l'autenticazione basata su form, informazioni sull'account utente è in genere archiviati in un database relazionale, ad esempio Microsoft SQL Server.

Applicazioni Web che supportano gli account utente possono facoltativamente raggruppare gli utenti in *ruoli*. Un ruolo è semplicemente un'etichetta che viene applicata a un utente e fornisce un'astrazione per la definizione delle regole di autorizzazione e funzionalità a livello di pagina. Ad esempio, un sito Web può includere un ruolo di amministratore con le regole di autorizzazione che non consentono a chiunque ma un amministratore di accedere a un particolare set di pagine web. Inoltre, un'ampia gamma di pagine che sono accessibili a tutti gli utenti (inclusi utenti non amministratori) può visualizzare dati aggiuntivi o offrono funzionalità aggiuntive quando visitati dagli utenti nel ruolo Administrators. Utilizzo dei ruoli, è possibile definire queste regole di autorizzazione su ruolo per ruolo base invece dall'utente.

## <a name="authenticating-users-in-an-aspnet-application"></a>L'autenticazione degli utenti in un'applicazione ASP.NET

Quando un utente immette un URL nella finestra indirizzo del proprio browser o clic del mouse su un collegamento, il browser invia una [Hypertext Transfer Protocol (HTTP)](http://en.wikipedia.org/wiki/HTTP) richiedere al server web per il contenuto specificato, che si tratti di un ASP.NET pagina, un'immagine, JavaScript file o qualsiasi altro tipo di contenuto. Il server web è il compito di restituzione del contenuto richiesto. In questo modo, è necessario determinare una serie di operazioni relative alla richiesta, tra cui chi ha effettuato la richiesta e indica se l'identità è autorizzata a recuperare il contenuto richiesto.

Per impostazione predefinita, i browser inviano richieste HTTP che non dispongono di alcun tipo di informazioni di identificazione. Ma se il browser includono le informazioni di autenticazione del server web avvia il flusso di lavoro di autenticazione, che tenta di identificare il client effettua la richiesta. I passaggi del flusso di lavoro autenticazione dipendono dal tipo di autenticazione usato dall'applicazione web. ASP.NET supporta tre tipi di autenticazione: moduli, Passport e Windows. Questa serie di esercitazioni si concentra su autenticazione basata su form, ma è possibile richiedere un minuto per confrontare e contrapporre flusso di lavoro e gli archivi utente di autenticazione di Windows.

### <a name="authentication-via-windows-authentication"></a>Autenticazione tramite l'autenticazione di Windows

Il flusso di lavoro di autenticazione di Windows Usa una delle tecniche di autenticazione seguenti:

- autenticazione di base
- Autenticazione del digest
- Autenticazione integrata di Windows

Tutti e tre le tecniche di funzionare approssimativamente allo stesso modo: quando un non autorizzato, arriva una richiesta anonima, il server web invia una risposta HTTP che indica tale autorizzazione è necessaria per continuare. Quindi, il browser visualizza una finestra di dialogo modale che richiede l'immissione di nome utente e password (vedere la figura 1). Queste informazioni vengono quindi inviate al server web tramite un'intestazione HTTP.


![Finestra di dialogo modale richiede all'utente le credenziali](security-basics-and-asp-net-support-vb/_static/image1.png)

**Figura 1**: finestra di dialogo modale richiede all'utente le credenziali


Le credenziali fornite vengono convalidate rispetto Store utente di Windows del server web. Ciò significa che ogni utente autenticato nell'applicazione web deve avere un account di Windows nell'organizzazione. Ciò è comune negli scenari intranet. Infatti, quando si usa l'autenticazione integrata di Windows in un'impostazione dell'intranet, il browser fornisce automaticamente il server web con le credenziali usate per accedere alla rete, eliminando in tal modo la finestra di dialogo illustrata nella figura 1. L'autenticazione di Windows è ideale per applicazioni intranet, ma risulta in genere fattibile per applicazioni Internet perché si preferisce non creare gli account di Windows per ogni utente che effettua l'iscrizione il sito.

### <a name="authentication-via-forms-authentication"></a>Autenticazione tramite autenticazione basata su form

Autenticazione basata su form, d'altra parte, è ideale per le applicazioni web Internet. È importante ricordare che l'autenticazione Form identifica l'utente da cui si richiede di immettere le credenziali tramite un web form. Di conseguenza, quando un utente tenta di accedere a una risorsa non autorizzata, viene automaticamente reindirizzato alla pagina di accesso in cui è possibile immettere le proprie credenziali. Le credenziali inviate quindi vengono convalidate rispetto a un archivio utente personalizzata - in genere un database.

Dopo aver verificato le credenziali inviate, un *ticket di autenticazione form* viene creato per l'utente. Questo ticket indica che l'utente è stato autenticato e include le informazioni di identificazione, ad esempio il nome utente. Il ticket di autenticazione form (in genere) verrà archiviato come cookie nel computer client. Di conseguenza, le successive visite al sito Web includono il ticket di autenticazione form nella richiesta HTTP, abilitando quindi l'applicazione web identificare l'utente dopo che hanno effettuato l'accesso.

Figura 2 illustra il flusso di lavoro autenticazione form da un punto di vista di alto livello. Si noti come le parti di autenticazione e autorizzazione in ASP.NET fungono da due entità separate. Il sistema di autenticazione Form identifica l'utente (o i report che sono anonime). Il sistema di autorizzazione è ciò che determina se l'utente ha accesso alla risorsa richiesta. Se l'utente non è autorizzato (come avviene nella figura 2 quando si prova a visitare in modo anonimo ProtectedPage.aspx), il sistema di autorizzazione segnala che l'utente viene negato, causando moduli di sistema di autenticazione per reindirizzare automaticamente l'utente alla pagina di accesso.

Una volta che l'utente è connesso correttamente, le successive richieste HTTP includono il ticket di autenticazione form. Il sistema di autenticazione Form identifica semplicemente l'utente: è il sistema di autorizzazione che determina se l'utente può accedere alla risorsa richiesta.


![Il flusso di lavoro di autenticazione form](security-basics-and-asp-net-support-vb/_static/image2.png)

**Figura 2**: il flusso di lavoro di autenticazione form


Entrare nell'autenticazione basata su form molto più dettagliatamente nelle prossime due esercitazioni[una panoramica dell'autenticazione basata su form](an-overview-of-forms-authentication-vb.md) e [configurazione dell'autenticazione form e argomenti avanzati](forms-authentication-configuration-and-advanced-topics-vb.md). Per altre informazioni su ASP. Opzioni di autenticazione di rete, vedere [autenticazione ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx).

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>Limitare l'accesso alle pagine Web, directory e funzionalità della pagina

In ASP.NET sono disponibili due modi per determinare se un utente specifico dispone di autorizzazioni sufficienti per accedere a un file specifico o una directory:

- **Autorizzazione del file** : poiché le pagine ASP.NET e servizi web vengono implementati come i file che si trovano nel file system del server web, accedere a tali file possono essere specificati tramite elenchi di controllo di accesso (ACL). I file di autorizzazione più comunemente utilizzato con autenticazione di Windows perché gli ACL sono le autorizzazioni valide per gli account di Windows. Quando si usa l'autenticazione basata su form, vengono eseguite tutte le richieste a livello di sistema del sistema operativo e file per lo stesso account di Windows, indipendentemente dall'utente che visitano il sito.
- **Autorizzazione URL**-autorizzazione URL, lo sviluppatore della pagina specifica le regole di autorizzazione in Web. config. Queste regole di autorizzazione specificano quali utenti o ruoli sono autorizzati ad accedere o non è consentiti l'accesso a determinate pagine o le directory nell'applicazione.

I file di autorizzazione e l'autorizzazione URL definiscono le regole di autorizzazione per l'accesso a una particolare pagina ASP.NET o per tutte le pagine ASP.NET in una directory specifica. Usando queste tecniche è possibile indicare a negare le richieste in una pagina specifica per un determinato utente o consentire l'accesso a un set di utenti e negare l'accesso a tutti gli altri utenti ASP.NET. Per quanto riguarda gli scenari in cui tutti gli utenti possono accedere alla pagina, ma la funzionalità della pagina dipende dall'utente? Ad esempio, molti siti che supportano gli account utente dispongono di pagine che consentono di visualizzare i dati per gli utenti autenticati e agli utenti anonimi o un contenuto diverso. Un utente anonimo potrebbe essere visualizzato un collegamento per accedere al sito, mentre un utente autenticato verrà invece visualizzato un messaggio, ad esempio, Bentornato, *Username* insieme a un collegamento per effettuare la disconnessione. Un altro esempio: quando si visualizzano un elemento in corrispondenza di un sito di aste visualizzare informazioni diverse a seconda che si tratti di una procedura di asta l'elemento o uno di essi.

Tali modifiche a livello di pagina possono essere eseguite in modo dichiarativo o a livello di codice. Per visualizzare un contenuto diverso per anonimi rispetto agli utenti autenticati, basta trascinare un [controllo LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) in una pagina e immettere il contenuto appropriato nel relativo AnonymousTemplate e LoggedInTemplate i modelli. In alternativa, è possibile a livello di programmazione determinare se la richiesta corrente è autenticata, l'utente e i ruoli appartengono a (se presente). È possibile usare queste informazioni quindi visualizzare o nascondere le colonne nei pannelli o una griglia nella pagina.

Questa serie include tre esercitazioni incentrate su autorizzazione. ***Autorizzazione basata su utente***prende in esame come limitare l'accesso a una o più pagine in una directory per gli account utente specifico; ***Basato su ruolo autorizzazione*** esamina specificando regole di autorizzazione presso il ruolo livello; infine, il ***visualizzazione contenuto in base all'attualmente registrato nell'utente*** questa esercitazione illustra la modifica di un particolare il contenuto e funzionalità in base all'utente visitando la pagina della pagina. Per altre informazioni su ASP. Opzioni di autorizzazione di NET, vedere [autorizzazione ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx).

## <a name="user-accounts-and-roles"></a>Account utente e ruoli

ASP. Autenticazione basata su form di NET fornisce un'infrastruttura per gli utenti accedere a un sito e hanno lo stato autenticato memorizzati nella navigazione tra le visite di pagina. E l'autorizzazione URL offre un framework per la limitazione dell'accesso a determinati file o cartelle in un'applicazione ASP.NET. Nessuna delle due funzionalità, tuttavia, fornisce un mezzo per archiviare informazioni sull'account utente o la gestione dei ruoli.

Prima di ASP.NET 2.0, gli sviluppatori erano responsabili per la creazione di propri archivi utente e il ruolo. Erano anche l'hook per progettare interfacce utente e scrivere il codice utente essenziali le pagine relative all'account, come la pagina di accesso e la pagina per creare un nuovo account, tra gli altri. Senza alcun framework di account utente predefinito in ASP.NET, ogni sviluppatore ha implementazione degli account utente per poi arrivare alla propria decisioni di progettazione su domande, ad esempio, come si archiviano le password o altre informazioni riservate? e le linee guida è consigliabile imporre relative alla complessità e la lunghezza della password?

Attualmente, l'implementazione di account utente in un'applicazione ASP.NET è molto più semplice grazie alla *framework di appartenenza* e integrati i controlli Web di accesso. Il framework di appartenenza è un numero limitato di classi nel [dello spazio dei nomi di dotare](https://msdn.microsoft.com/library/system.web.security.aspx) che forniscono funzionalità per eseguire attività relative all'account utente essenziali. La classe principale nel framework di appartenenza è il [classe di appartenenza](https://msdn.microsoft.com/library/system.web.security.membership.aspx), che dispone di metodi, ad esempio:

- CreateUser
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- ValidateUser

Usa il framework di appartenenza i [modello di provider](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), quale separare l'API del framework di appartenenza dalla relativa implementazione. Ciò consente agli sviluppatori di usare un'API comune, ma permette loro di usare un'implementazione che soddisfi esigenze della propria applicazione. In breve, la classe di appartenenza definisce la funzionalità essenziale di framework (metodi, proprietà ed eventi), ma non fornisce effettivamente tutti i dettagli di implementazione. Al contrario, i metodi della classe di appartenenza richiamano provider configurato, ovvero il componente che esegue il lavoro effettivo. Ad esempio, quando viene richiamato l'appartenenza al metodo della classe CreateUser, la classe di appartenenza non conoscenza i dettagli dell'archivio dell'utente. Non è chiaro se gli utenti sono mantenuti in un database o in un file XML o in un altro archivio. La classe di appartenenza esamina la configurazione dell'applicazione web per determinare quale provider di delegare la chiamata a e la classe del provider è responsabile per creare effettivamente il nuovo account utente nell'archivio utente appropriato. Questa interazione è illustrata nella figura 3.

Microsoft offre due classi di provider di appartenenza in .NET Framework:

- [ActiveDirectoryMembershipProvider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) -implementa l'API di appartenenza nei server di Active Directory e Active Directory Application Mode (ADAM).
- [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) -implementa l'API di appartenenza in un database di SQL Server.

Questa serie di esercitazioni illustra esclusivamente il provider SqlMembershipProvider.


[![Il modello consente a diverse implementazioni del Provider per essere facilmente collegato nel Framework di](security-basics-and-asp-net-support-vb/_static/image4.png)](security-basics-and-asp-net-support-vb/_static/image3.png)

**Figura 03**: il modello consente a diverse implementazioni del Provider per essere facilmente collegato nel Framework di ([fare clic per visualizzare l'immagine con dimensioni normali](security-basics-and-asp-net-support-vb/_static/image5.png))


Il vantaggio del modello di provider è che implementazioni alternative possono essere sviluppate da Microsoft, fornitori di terze parti o singoli sviluppatori e collegate senza problemi il framework di appartenenza. Ad esempio, Microsoft ha rilasciato [un provider di appartenenza per database di Microsoft Access](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi). Per altre informazioni sul provider di appartenenze, consultare il [Toolkit del Provider](https://msdn.microsoft.com/asp.net/aa336558.aspx), che include una procedura dettagliata di provider di appartenenze, i provider personalizzato di esempio, più di 100 pagine della documentazione sul modello di provider e il completare il codice sorgente per i provider di appartenenze predefiniti (vale a dire, ActiveDirectoryMembershipProvider e SqlMembershipProvider).

ASP.NET 2.0 ha introdotto anche il framework di ruoli. Ad esempio il framework di appartenenza, il framework di ruoli viene compilato sopra il modello di provider. L'API viene esposta tramite il [classe Roles](https://msdn.microsoft.com/library/system.web.security.roles.aspx) e .NET Framework viene fornito con tre classi del provider:

- [AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) -gestisce le informazioni sui ruoli in un archivio di criteri di gestione autorizzazioni, ad esempio Active Directory o ADAM.
- [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) -implementa i ruoli in un database di SQL Server.
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) -associa le informazioni sui ruoli in base a gruppo di Windows del visitatore. Questo metodo viene in genere utilizzato con l'autenticazione di Windows.

Questa serie di esercitazioni è incentrato esclusivamente sul SqlRoleProvider.

Poiché il modello di provider include una singola API frontale (le classi dell'appartenenza e ruoli), è possibile compilare funzionalità intorno a tale API senza doversi preoccupare dei dettagli di implementazione: quelle vengono gestite dai provider selezionato per la pagina per gli sviluppatori. Questa API unificata consente Microsoft e fornitori di terze parti compilare i controlli Web che si interfacciano con i framework di appartenenza e ruoli. ASP.NET viene fornito con numerosi [controlla l'account di accesso Web](https://msdn.microsoft.com/library/ms178329.aspx) per l'implementazione di interfacce utente più account utente comuni. Ad esempio, il [controllo Login](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) richieda all'utente le credenziali, li convalida e quindi li registra in tramite autenticazione basata su form. Il [controllo LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) offre modelli per la visualizzazione di diversi markup agli utenti anonimi rispetto agli utenti autenticati o markup diversi in base al ruolo dell'utente. E il [controllo CreateUserWizard](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx) fornisce un'interfaccia utente dettagliata per la creazione di un nuovo account utente.

Sotto le quinte i vari controlli di accesso di interagire con i framework di appartenenza e ruoli. La maggior parte dei controlli di accesso possono essere implementati senza dover scrivere una singola riga di codice. Esamineremo questi controlli più dettagliatamente in esercitazioni future, incluse le tecniche per estendere e personalizzare le relative funzionalità.

## <a name="summary"></a>Riepilogo

Tutte le applicazioni web che supportano gli account utente richiedono funzionalità simili, ad esempio: la possibilità per gli utenti accedere e avere i log in stato memorizzati nella navigazione tra le visite di pagina; una pagina web per i visitatori di nuovo creare un account. e la possibilità di sviluppatore della pagina per specificare quali risorse, dati e funzionalità sono disponibili a quali utenti o ruoli. Le attività di autenticazione e autorizzazione degli utenti e di gestione dei ruoli e account utente è molto più semplice eseguire nelle applicazioni ASP.NET grazie all'autenticazione basata su form, l'autorizzazione URL e i framework di appartenenza e ruoli.

Nel corso delle esercitazioni più avanti verranno esaminate questi aspetti tramite la compilazione di un'applicazione web funzionante sin dall'inizio di una Guida dettagliata. Nell'esercitazione due verranno analizzati autenticazione basata su form in modo dettagliato. Si vedrà il flusso di lavoro autenticazione form in azione, analizzare il ticket di autenticazione form, discutere problemi di sicurezza e informazioni su come configurare il sistema di autenticazione form - tutto durante la compilazione di un'applicazione web che consente ai visitatori di accedere e disconnettersi.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [ASP.NET 2.0 l'appartenenza, ruoli, autenticazione basata su form e le risorse di sicurezza](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [ASP.NET 2.0 Security Guidelines](https://msdn.microsoft.com/library/ms998258.aspx)
- [Autenticazione ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [Autorizzazione ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Cenni preliminari sui controlli Login di ASP.NET](https://msdn.microsoft.com/library/ms178329.aspx)
- [Analisi di ASP.NET 2.0 appartenenza, ruoli e profilo](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Come ricerca per categorie: proteggere il sito tramite l'appartenenza e ruoli?](https://asp.net/learn/videos/video-45.aspx) (Video)
- [Introduzione all'appartenenza](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [MSDN Security Developer Center](https://msdn.microsoft.com/security/default.aspx)
- [Professionisti ASP.NET 2.0 Security, l'appartenenza e la gestione dei ruoli](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Toolkit del provider](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stato in questa esercitazione, serie è stata esaminata da diversi validi revisori. I revisori per questa esercitazione includono Alicja Maziarz, John Suru e Teresa Murphy. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](forms-authentication-configuration-and-advanced-topics-cs.md)
> [Successivo](an-overview-of-forms-authentication-vb.md)
