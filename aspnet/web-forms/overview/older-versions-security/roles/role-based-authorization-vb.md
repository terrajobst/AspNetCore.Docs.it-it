---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-vb
title: Autorizzazione basata sui ruoli (VB) | Microsoft Docs
author: rick-anderson
description: Questa esercitazione inizia con uno sguardo al modo in cui il framework di ruoli associa ruoli di un utente con il suo contesto di sicurezza. Esamina quindi come applicare URL basato su ruolo...
ms.author: aspnetcontent
ms.date: 03/24/2008
ms.assetid: 83b4f5a4-4f5a-4380-ba33-f0b5c5ac6a75
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: 6bf9e1f0832b811ce8e2033dd45f94dc3baaeea6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829645"
---
<a name="role-based-authorization-vb"></a>Autorizzazione basata sui ruoli (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.11.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_vb.pdf)

> Questa esercitazione inizia con uno sguardo al modo in cui il framework di ruoli associa ruoli di un utente con il suo contesto di sicurezza. Esamina quindi come applicare le regole di autorizzazione URL basata sui ruoli. In seguito, verranno esaminati usando strumenti dichiarativi e programmatici per la modifica di dati visualizzati e le funzionalità offerte da una pagina ASP.NET.


## <a name="introduction"></a>Introduzione

Nel <a id="_msoanchor_1"> </a> [ *basata sull'utente Authorization* ](../membership/user-based-authorization-vb.md) esercitazione è stato illustrato come utilizzare l'autorizzazione URL per specificare quali utenti è stato possibile visitare un particolare set di pagine. Con solo un po' di markup in `Web.config`, è possibile indicare ad ASP.NET per consentire solo agli utenti autenticati visitare una pagina. O è stato possibile indicano che solo gli utenti Tito e Bob sono stati autorizzati o indicano che sono stati consentiti tutti gli utenti autenticati ad eccezione di Sam.

Oltre all'autorizzazione URL, è anche stato tecniche disponibili strumenti dichiarative e programmatiche per controllare i dati visualizzati e le funzionalità offerte da una pagina in base all'utente di visitare. In particolare, viene creata una pagina in cui è elencato il contenuto della directory corrente. Qualsiasi utente potrebbe visitare questa pagina, ma solo gli utenti autenticati è stato possibile visualizzare il contenuto dei file e solo Tito è stato possibile eliminare i file.

Applicazione delle regole di autorizzazione in base a utente può aumentare in un incubo di attività di manutenzione. Un approccio migliore è utilizzare l'autorizzazione basata sui ruoli. La buona notizia è che gli strumenti a disposizione per l'applicazione delle regole di autorizzazione funzionano altrettanto bene con i ruoli, come avviene per gli account utente. Regole di autorizzazione URL possono specificare ruoli anziché agli utenti. Il controllo LoginView, che esegue il rendering di output diversa per gli utenti autenticati e anonimi, può essere configurato per visualizzare un contenuto diverso in base ai ruoli dell'utente connesso. E l'API di ruoli include metodi per determinare i ruoli dell'utente connesso.

Questa esercitazione inizia con uno sguardo al modo in cui il framework di ruoli associa ruoli di un utente con il suo contesto di sicurezza. Esamina quindi come applicare le regole di autorizzazione URL basata sui ruoli. In seguito, verranno esaminati usando strumenti dichiarativi e programmatici per la modifica di dati visualizzati e le funzionalità offerte da una pagina ASP.NET. Iniziamo!

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>Informazioni su come ruoli sono associati il contesto di sicurezza dell'utente

Ogni volta che una richiesta attiva la pipeline ASP.NET è associato a un contesto di sicurezza, che include informazioni che identificano il richiedente. Quando si usa l'autenticazione basata su form, viene usato un ticket di autenticazione come un token di identità. Come accennato nel <a id="_msoanchor_2"> </a> [ *una panoramica dell'autenticazione basata su form* ](../introduction/an-overview-of-forms-authentication-vb.md) e <a id="_msoanchor_3"> </a> [ *form Configurazione dell'autenticazione e gli argomenti avanzati* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) esercitazioni e i `FormsAuthenticationModule` è responsabile di determinare l'identità del richiedente, che si verifica durante la [ `AuthenticateRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Se viene trovato un ticket di autenticazione valido non scaduto, il `FormsAuthenticationModule` decodifica in modo da verificare l'identità del richiedente. Crea una nuova `GenericPrincipal` dell'oggetto e la assegna a questa opzione per il `HttpContext.User` oggetto. Lo scopo di un'entità, ad esempio `GenericPrincipal`, consiste nell'identificare il nome dell'utente autenticato e gli elementi che appartengono a ruoli. A questo scopo è evidente dal fatto che tutti gli oggetti principali hanno un' `Identity` proprietà e un `IsInRole(roleName)` (metodo). Il `FormsAuthenticationModule`, tuttavia, non è interessato nella registrazione delle informazioni sui ruoli e le `GenericPrincipal` crea l'oggetto non specifica alcun ruolo.

Se il framework di ruoli è abilitato, il [ `RoleManagerModule` ](https://msdn.microsoft.com/library/system.web.security.rolemanagermodule.aspx) modulo HTTP passaggi dopo il `FormsAuthenticationModule` e identifica i ruoli dell'utente autenticato durante la [ `PostAuthenticateRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), che viene attivato dopo il `AuthenticateRequest` evento. Se la richiesta proviene da un utente autenticato, il `RoleManagerModule` sovrascrive il `GenericPrincipal` oggetto creato dalle `FormsAuthenticationModule` e la sostituisce con un [ `RolePrincipal` oggetto](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx). Il `RolePrincipal` classe Usa l'API di ruoli per determinare quali ruoli a cui appartiene l'utente.

Figura 1 illustra il flusso di lavoro pipeline ASP.NET quando si usa l'autenticazione basata su form e il framework di ruoli. Il `FormsAuthenticationModule` venga eseguito per primo, identifica l'utente attraverso il ticket di autenticazione e crea un nuovo `GenericPrincipal` oggetto. Successivamente, il `RoleManagerModule` passaggi e sovrascrive la `GenericPrincipal` dell'oggetto con un `RolePrincipal` oggetto.

Se un utente anonimo visita il sito, né il `FormsAuthenticationModule` né il `RoleManagerModule` crea un oggetto entità.


[![Gli eventi della Pipeline ASP.NET per un utente autenticato quando si usa l'autenticazione basata su form e il Framework di ruoli](role-based-authorization-vb/_static/image2.png)](role-based-authorization-vb/_static/image1.png)

**Figura 1**: di eventi della Pipeline ASP.NET per un'autenticazione utente quando con autenticazione basata su form e il Framework di ruoli ([fare clic per visualizzare l'immagine con dimensioni normali](role-based-authorization-vb/_static/image3.png))


### <a name="caching-role-information-in-a-cookie"></a>La memorizzazione nella cache le informazioni sui ruoli in un Cookie

Il `RolePrincipal` dell'oggetto `IsInRole(roleName)` chiamate al metodo `Roles`.`GetRolesForUser` Per ottenere i ruoli per l'utente per determinare se l'utente è membro del *roleName*. Quando si usa il `SqlRoleProvider`, ciò comporta una query al database dell'archivio di ruolo. Quando si usano regole di autorizzazione URL basata sui ruoli di `RolePrincipal`del `IsInRole` metodo verrà chiamato a ogni richiesta a una pagina che è protetto dalle regole di autorizzazione URL basata sui ruoli. Anziché dover cercare le informazioni sui ruoli nel database in ogni richiesta, il `Roles` framework include un'opzione per memorizzare nella cache i ruoli dell'utente in un cookie.

Se il framework di ruoli è configurato per memorizzare nella cache i ruoli dell'utente in un cookie, il `RoleManagerModule` consente di creare il cookie durante la pipeline di ASP.NET [ `EndRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.endrequest.aspx). Questo cookie viene usato nelle richieste successive nella `PostAuthenticateRequest`, ovvero quando la `RolePrincipal` oggetto viene creato. Se il cookie è valido e non sia scaduto, i dati nel cookie vengano analizzati e utilizzati per popolare i ruoli dell'utente, evitando così il `RolePrincipal` dalla necessità di effettuare una chiamata al `Roles` classe per determinare i ruoli dell'utente. La figura 2 illustra questo flusso di lavoro.


[![Informazioni sui ruoli dell'utente possono essere archiviati in un Cookie per migliorare le prestazioni](role-based-authorization-vb/_static/image5.png)](role-based-authorization-vb/_static/image4.png)

**Figura 2**: ruolo informazioni possono essere archiviati l'utente in un Cookie per migliorare le prestazioni ([fare clic per visualizzare l'immagine con dimensioni normali](role-based-authorization-vb/_static/image6.png))


Per impostazione predefinita, il meccanismo dei cookie ruolo della cache è disabilitato. Può essere abilitata tramite il `<roleManager>`; il markup di configurazione in `Web.config`. Stato illustrato l'uso di [ `<roleManager>` elemento](https://msdn.microsoft.com/library/ms164660.aspx) per specificare i provider di ruoli nel <a id="_msoanchor_4"> </a> [ *creazione e gestione dei ruoli* ](creating-and-managing-roles-vb.md) esercitazione pertanto è necessario disporre già questo elemento all'interno dell'applicazione `Web.config` file. Il ruolo della cache cookie sono specificate come attributi del `<roleManager>`; elemento e sono riepilogati nella tabella 1.

> [!NOTE]
> Le impostazioni di configurazione elencate nella tabella 1 specificano le proprietà del cookie cache ruolo risultante. Per altre informazioni sui cookie, sul relativo funzionamento e le relative proprietà diverse, leggere [in questa esercitazione i cookie](http://www.quirksmode.org/js/cookies.html).


| <strong>Property</strong> |                                                                                                                                                                                                                                                                                                                                                         <strong>Descrizione</strong>                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   `cacheRolesInCookie`    |                                                                                                                                                                                                                                                                                                                              Valore booleano che indica se la memorizzazione nella cache di cookie viene utilizzato. Il valore predefinito è `false`.                                                                                                                                                                                                                                                                                                                              |
|       `cookieName`        |                                                                                                                                                                                                                                                                                                                                     Il nome del cookie di cache di ruoli. Il valore predefinito è ". ASPXROLES".                                                                                                                                                                                                                                                                                                                                     |
|       `cookiePath`        |                                                                                                                                                                                                                                Il percorso del cookie di nome dei ruoli. L'attributo path consente agli sviluppatori di limitare l'ambito di un cookie da una gerarchia di directory specifico. Il valore predefinito è "/", che informa il browser per inviare il cookie del ticket di autenticazione a qualsiasi richiesta effettuata al dominio.                                                                                                                                                                                                                                 |
|    `cookieProtection`     |                                                                                                                                                               Indica quali tecniche usate per proteggere il cookie di cache di ruolo. I valori consentiti sono: `All` (predefinito); `Encryption`; `None`; e `Validation`. Fare riferimento al passaggio 3 della <a id="_anchor_5"> </a> [ *configurazione dell'autenticazione form e argomenti avanzati* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) esercitazione per altre informazioni su questi livelli di protezione.                                                                                                                                                                |
|    `cookieRequireSSL`     |                                                                                                                                                                                                                                                                                                   Valore booleano che indica se trasmettere il cookie di autenticazione è necessaria una connessione SSL. Il valore predefinito è `false`.                                                                                                                                                                                                                                                                                                   |
| `cookieSlidingExpiration` |                                                                                                                                                                                                                                                  Valore booleano che indica che se il timeout del cookie viene reimpostato ogni volta che l'utente visita il sito durante una singola sessione. Il valore predefinito è `false`. Questo valore è pertinente solo quando `createPersistentCookie` è impostata su `true`.                                                                                                                                                                                                                                                  |
|      `cookieTimeout`      |                                                                                                                                                                                                                                                                         Specifica l'ora, minuti, dopo il quale il cookie del ticket di autenticazione prima della scadenza. Il valore predefinito è `30`. Questo valore è pertinente solo quando `createPersistentCookie` è impostata su `true`.                                                                                                                                                                                                                                                                         |
| `createPersistentCookie`  |                                                                                                                                                                   Valore booleano che specifica se il ruolo di memorizzare nella cache di cookie è un cookie di sessione o un cookie persistente. Se `false` (impostazione predefinita), viene usato un cookie di sessione, che viene eliminato quando il browser viene chiuso. Se `true`, viene utilizzato un cookie permanente, scade `cookieTimeout` numero di minuti dopo che è stato creato o dopo la visita precedente, a seconda del valore di `cookieSlidingExpiration`.                                                                                                                                                                    |
|         `domain`          |                                                                                                                                                 Specifica il valore di dominio del cookie. Il valore predefinito è una stringa vuota, che consente al browser di usare il dominio da cui è stato rilasciato (ad esempio www.yourdomain.com). In questo caso, il cookie verrà <strong>non</strong> da inviare quando sottodomini, ad esempio admin.yourdomain.com richiede la creazione. Se si desidera che il cookie deve essere passato a tutti i sottodomini è necessario personalizzare il `domain` attributo, impostandola su "dominio.com".                                                                                                                                                 |
|    `maxCachedResults`     | Specifica il numero massimo di nomi di ruolo che vengono memorizzati nella cache nel cookie. Il valore predefinito è 25. Il `RoleManagerModule` non crea un cookie per gli utenti che appartengono a più di `maxCachedResults` ruoli. Di conseguenza, il `RolePrincipal` dell'oggetto `IsInRole` metodo verrà utilizzato il `Roles` classe per determinare i ruoli dell'utente. Il motivo `maxCachedResults` esiste perché molti agenti utente non consentono i cookie di dimensioni superiori a 4.096 byte. Pertanto, questo limite di utilizzo è progettato per ridurre la probabilità di superare questa limitazione delle dimensioni. Se si dispone di nomi di ruolo estremamente lunghi, è possibile provare a specificare un minore `maxCachedResults` valore; contrariwise, se si hanno i nomi dei ruoli molto breve, è possibile aumentare probabilmente questo valore. |

**Tabella 1**: le opzioni di configurazione di ruolo della Cache Cookie

È possibile configurare l'applicazione per usare i cookie di ruolo non permanente della cache. A tale scopo, aggiornare il `<roleManager>` elemento `Web.config` per includere gli attributi correlati al cookie seguenti:

[!code-xml[Main](role-based-authorization-vb/samples/sample1.xml)]

Ho aggiornato il `<roleManager>`; elemento mediante l'aggiunta di tre attributi: `cacheRolesInCookie`, `createPersistentCookie`, e `cookieProtection`. Impostando `cacheRolesInCookie` al `true`, il `RoleManagerModule` ora automaticamente nella cache i ruoli dell'utente in un cookie, anziché dover cercare le informazioni sui ruoli dell'utente per ogni richiesta. Impostato in modo esplicito il `createPersistentCookie` e `cookieProtection` attributi agli `false` e `All`, rispettivamente. Tecnicamente, non devo specificare valori per questi attributi, poiché li semplicemente assegnato sui rispettivi valori predefiniti, ma inserire qui per renderla in modo esplicito cancellare che non si usa i cookie permanenti e che il cookie è crittografato e convalidato.

Questo è tutto qui! Da questo momento, il framework di ruoli nella cache dei ruoli degli utenti nei cookie. Se il browser dell'utente non supporta i cookie, o se i cookie vengono cancellati o perso, in qualche modo, non è banale: il `RolePrincipal` oggetto utilizzerà semplicemente il `Roles` classe nel caso che nessun cookie (o uno valido o è scaduto) è disponibile.

> [!NOTE]
> I modelli di Microsoft &amp; gruppo di procedure consigliate, scoraggia l'utilizzo dei cookie cache ruolo persistente. Poiché il possesso del cookie cache ruolo sia sufficiente per dimostrare l'appartenenza al ruolo, se un pirata informatico possibile in qualche modo accedere ai cookie dell'utente valido può rappresentare tale utente. Aumenta la probabilità che ciò si verifichi se il cookie è persistente nel browser dell'utente. Per altre informazioni su questa raccomandazione di sicurezza, nonché altri problemi di sicurezza, vedere la [elenco di domande di sicurezza per ASP.NET 2.0](https://msdn.microsoft.com/library/ms998375.aspx).


## <a name="step-1-defining-role-based-url-authorization-rules"></a>Passaggio 1: Definizione di regole di autorizzazione URL basata sui ruoli

Come descritto nel <a id="_msoanchor_6"> </a> [ *basata sull'utente Authorization* ](../membership/user-based-authorization-vb.md) esercitazione, l'autorizzazione URL offre un modo per limitare l'accesso a un set di pagine in dall'utente o ruolo per ruolo base. Le regole di autorizzazione URL siano state digitate nel `Web.config` usando il [ `<authorization>` elemento](https://msdn.microsoft.com/library/8d82143t.aspx) con `<allow>` e `<deny>` elementi figlio. Oltre alle regole relative agli utenti autorizzazioni illustrate nelle esercitazioni precedenti, ogni `<allow>` e `<deny>` elemento figlio può includere anche:

- Un particolare ruolo
- Un elenco delimitato da virgole dei ruoli

Le regole di autorizzazione URL, ad esempio, concedono l'accesso agli utenti nei ruoli amministratori e i supervisori, ma negano l'accesso a tutti gli altri:

[!code-xml[Main](role-based-authorization-vb/samples/sample2.xml)]

Il `<allow>` elemento nel markup riportato sopra indica che sono consentiti i ruoli agli amministratori e i supervisori; la `<deny>`; elemento indica che *tutte* agli utenti viene negato.

È possibile configurare l'applicazione in modo che il `ManageRoles.aspx`, `UsersAndRoles.aspx`, e `CreateUserWizardWithRoles.aspx` pagine accessibili solo agli utenti nel ruolo di amministratori, mentre il `RoleBasedAuthorization.aspx` pagina rimane accessibile a tutti i visitatori.

A tale scopo, iniziare aggiungendo un `Web.config` file per il `Roles` cartella.


[![Aggiungere un File Web. config nella directory di ruoli](role-based-authorization-vb/_static/image8.png)](role-based-authorization-vb/_static/image7.png)

**Figura 3**: aggiungere un `Web.config` del File per il `Roles` directory ([fare clic per visualizzare l'immagine con dimensioni normali](role-based-authorization-vb/_static/image9.png))


Successivamente, aggiungere il markup di configurazione seguente alla `Web.config`:

[!code-xml[Main](role-based-authorization-vb/samples/sample3.xml)]

Il `<authorization>` elemento il `<system.web>` sezione indica che solo gli utenti nel ruolo Administrators possono accedere alle risorse ASP.NET nel `Roles` directory. Il `<location>` elemento definisce un set alternativo di regole di autorizzazione URL per il `RoleBasedAuthorization.aspx` pagina, consentendo a tutti gli utenti di visitare la pagina.

Dopo aver salvato le modifiche apportate alla `Web.config`, accedere come un utente che non si trova il ruolo di amministratore e quindi provare a visitare una delle pagine protette. Il `UrlAuthorizationModule` rileverà che si dispone dell'autorizzazione per visitare la risorsa richiesta; di conseguenza, il `FormsAuthenticationModule` si verrà reindirizzati alla pagina di accesso. La pagina di accesso quindi passerà al `UnauthorizedAccess.aspx` pagina (vedere la figura 4). Questo reindirizzamento finale dalla pagina di accesso al `UnauthorizedAccess.aspx` si verifica a causa di codice aggiunto alla pagina di accesso nel passaggio 2 della <a id="_msoanchor_7"> </a> [ *autorizzazione basata su utente* ](../membership/user-based-authorization-vb.md) esercitazione. In particolare, la pagina di accesso reindirizza automaticamente a qualunque utente autenticato `UnauthorizedAccess.aspx` se la stringa di query contiene un `ReturnUrl` parametro, come questo parametro indica che l'utente è arrivato alla pagina di accesso dopo il tentativo di visualizzare una pagina non è stato è autorizzato a visualizzare.


[![Solo gli utenti nel ruolo Administrators possono visualizzare le pagine protette](role-based-authorization-vb/_static/image11.png)](role-based-authorization-vb/_static/image10.png)

**Figura 4**: solo gli utenti nel ruolo Administrators possono visualizzare le pagine protette ([fare clic per visualizzare l'immagine con dimensioni normali](role-based-authorization-vb/_static/image12.png))


Disconnettersi e quindi accedere come un utente che appartiene al ruolo Administrators. A questo punto sarà possibile visualizzare tre pagine protette.


[![Tito possono visitare che l'UsersAndRoles.aspx pagina perché si trova nel ruolo di amministratore](role-based-authorization-vb/_static/image14.png)](role-based-authorization-vb/_static/image13.png)

**Figura 5**: Tito possono visitare la `UsersAndRoles.aspx` pagina perché è nel ruolo Administrators ([fare clic per visualizzare l'immagine con dimensioni normali](role-based-authorization-vb/_static/image15.png))


> [!NOTE]
> Quando si specificano regole di autorizzazione URL, per i ruoli o utenti, è importante tenere presente che le regole sono analizzati uno alla volta, dall'alto verso il basso. Non appena viene rilevata una corrispondenza, l'utente viene concesso o negato l'accesso, a seconda se è stata trovata la corrispondenza un `<allow>` o `<deny>` elemento. **Se viene trovata alcuna corrispondenza, l'utente viene concesso l'accesso.** Di conseguenza, se si desidera limitare l'accesso a uno o più account utente, è essenziale usare un `<deny>` l'ultimo elemento nella configurazione di autorizzazione URL. **Se le regole di autorizzazione URL non includono una**`<deny>`**elemento, tutti gli utenti verranno concesso l'accesso.** Per una discussione più approfondita sul modo in cui vengono analizzate le regole di autorizzazione URL, fare riferimento al "un'occhiata a come le `UrlAuthorizationModule` utilizza le regole di autorizzazione da concedere o negare l'accesso" sezione del <a id="_msoanchor_8"> </a> [  *Autorizzazione basata sull'utente* ](../membership/user-based-authorization-vb.md) esercitazione.


## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>Passaggio 2: Limitazione della funzionalità di base ai ruoli dell'utente attualmente connesso

Rende di autorizzazione URL è più facile specificare l'autorizzazione grossolani che lo stato delle regole quali identità sono consentite e quelle che non è consentiti visualizzare una pagina particolare (o tutte le pagine in una cartella e nelle relative sottocartelle). In alcuni casi, tuttavia, si potrebbe essere necessario consentire tutti gli utenti visitano una pagina, ma limitare la funzionalità della pagina in base ai ruoli dell'utente. Ciò può comportare nasconda o visualizzando i dati in base al ruolo dell'utente o che offrono funzionalità aggiuntive agli utenti che appartengono a un particolare ruolo.

Regole di autorizzazione in base al ruolo granulare possono essere implementate in modo dichiarativo o a livello di codice (o tramite una combinazione dei due). Nella sezione successiva si vedrà come implementare l'autorizzazione dichiarativa granulare tramite il controllo LoginView. In seguito, verranno analizzati tecniche a livello di codice. Prima di poter esaminare applicando le regole di autorizzazione granulare, tuttavia, prima di tutto dobbiamo creare una pagina di cui funzionalità dipende dal ruolo dell'utente visita.

È possibile creare una pagina che elenca tutti gli account utente nel sistema in un controllo GridView. Il controllo GridView includeranno nome utente, indirizzo di posta elettronica, data dell'ultimo accesso e i commenti relativi all'utente di ogni utente. Oltre a visualizzare le informazioni di ogni utente, il controllo GridView verrà includono modifica ed eliminare le funzionalità. Inizialmente verrà creato in questa pagina con la modifica e funzionalità disponibili a tutti gli utenti di eliminazione. Nelle sezioni "Usando il controllo LoginView" e "Limitazione a livello di codice la funzionalità" si vedrà come abilitare o disabilitare queste funzionalità in base al ruolo dell'utente.

> [!NOTE]
> La pagina ASP.NET che si sta tentando di compilazione Usa un controllo GridView per visualizzare gli account utente. Poiché questa esercitazione, serie è incentrato sull'autenticazione basata su form, autorizzazione, gli account utente e ruoli, non desidera dedicare troppo tempo a descrivere i meccanismi interni del controllo GridView. Anche se in questa esercitazione offre istruzioni dettagliate specifiche per la configurazione di questa pagina, non approfondire i dettagli del motivo per cui sono state apportate alcune scelte, o ciò che hanno proprietà particolare effetto sull'output sottoposto a rendering. Per un esame approfondito del controllo GridView, consultare il mio *[usare i dati in ASP.NET 2.0](../../data-access/index.md)* serie di esercitazioni.


Iniziare aprendo il `RoleBasedAuthorization.aspx` nella pagina di `Roles` cartella. Trascinare un controllo GridView dalla pagina nella finestra di progettazione e set relativo `ID` a `UserGrid`. In un momento in cui si scriverà codice che chiama il `Membership`.`GetAllUsers` metodo e associa l'oggetto risultante `MembershipUserCollection` oggetto a GridView. Il `MembershipUserCollection` contiene un `MembershipUser` oggetto per ogni account utente nel sistema. `MembershipUser` oggetti dispongono di proprietà, ad esempio `UserName`,`Email`,`LastLoginDate` e così via.

Prima si scrive il codice che associa gli account utente alla griglia, è possibile definire i campi del controllo GridView. Dallo Smart Tag del controllo GridView, fare clic sul collegamento "Modifica colonne" per avviare la finestra di dialogo campi (vedere la figura 6). A questo punto, deselezionare la casella di controllo "Genera campi automaticamente" nell'angolo inferiore sinistro. Poiché si desidera che questo controllo GridView per includere la modifica ed eliminazione di funzionalità, aggiungere un CommandField e impostare relativi `ShowEditButton` e `ShowDeleteButton` proprietà su True. Successivamente, aggiungere quattro campi per visualizzare il `UserName`, `Email`, `LastLoginDate`, e `Comment` proprietà. Usare un BoundField per le due proprietà di sola lettura (`UserName` e `LastLoginDate`) e TemplateFields per i due campi modificabili (`Email` e `Comment`).

La visualizzazione BoundField prima la `UserName` proprietà; set relativo `HeaderText` e `DataField` proprietà su "UserName". Questo campo non sono modificabile, in modo da impostare relativo `ReadOnly` proprietà su True. Configurare il `LastLoginDate` BoundField impostando relativi `HeaderText` ad "Accesso ultimo" e il relativo `DataField` a "LastLoginDate". È possibile formattare l'output di questo BoundField in modo che solo la data viene visualizzata (anziché la data e ora). A tale scopo, impostare questo BoundField `HtmlEncode` la proprietà su False e la relativa `DataFormatString` proprietà su "{0:d}". Impostare anche il `ReadOnly` proprietà su True.

Impostare il `HeaderText` le proprietà delle due TemplateFields "Email" e "Comment".


[![Campi del controllo GridView possono essere configurati tramite la finestra di dialogo campi](role-based-authorization-vb/_static/image17.png)](role-based-authorization-vb/_static/image16.png)

**Figura 6**: campi può essere configurato tramite campi di finestra di GridView The di dialogo ([fare clic per visualizzare l'immagine con dimensioni normali](role-based-authorization-vb/_static/image18.png))


È ora necessario definire il `ItemTemplate` e `EditItemTemplate` per il "Email" e "Comment" TemplateFields. Aggiungere un controllo Web etichetta a ogni il `ItemTemplates` ed eseguire l'associazione loro `Text` delle proprietà per il `Email` e `Comment` proprietà, rispettivamente.

Per TemplateField di "Email", aggiungere una casella di testo denominato `Email` al relativo `EditItemTemplate` ed eseguire l'associazione relativo `Text` proprietà per il `Email` proprietà tramite associazione dati bidirezionale. Aggiungere un RequiredFieldValidator e RegularExpressionValidator al `EditItemTemplate` per garantire che un visitatore di modifica proprietà indirizzo di posta elettronica ha immesso un indirizzo di posta elettronica valido. Per il TemplateField "Comment", aggiungere una casella di testo multilinea denominato `Comment` al relativo `EditItemTemplate`. Impostare la casella di testo `Columns` e `Rows` le proprietà da 40 e 4, rispettivamente e quindi associare relativo `Text` proprietà per il `Comment` proprietà tramite associazione dati bidirezionale.

Dopo aver configurato queste TemplateFields, il relativo markup dichiarativo dovrebbe essere simile al seguente:

[!code-aspx[Main](role-based-authorization-vb/samples/sample4.aspx)]

Durante la modifica o eliminazione di un account utente è necessario conoscere l'utente `UserName` valore della proprietà. Impostare il controllo GridView `DataKeyNames` proprietà su "UserName" in modo che queste informazioni sono disponibili tramite il controllo GridView `DataKeys` raccolta.

Infine, aggiungere un controllo di controllo ValidationSummary alla pagina e impostare relativi `ShowMessageBox` su True e il relativo `ShowSummary` proprietà su False. Con queste impostazioni, di controllo ValidationSummary visualizzerà un avviso sul lato client, se l'utente tenta di modificare un account utente con un indirizzo di posta elettronica non valido o mancante.

[!code-aspx[Main](role-based-authorization-vb/samples/sample5.aspx)]

A questo punto è stata completata markup dichiarativo di questa pagina. L'attività successiva consiste nell'associare il set di account utente a GridView. Aggiungere un metodo denominato `BindUserGrid` per il `RoleBasedAuthorization.aspx` classe code-behind della pagina che associa le `MembershipUserCollection` restituito da `Membership.GetAllUsers` per il `UserGrid` GridView. Chiamare questo metodo dalla `Page_Load` gestore dell'evento nella prima visita pagina.

[!code-vb[Main](role-based-authorization-vb/samples/sample6.vb)]

Con questo codice, visitare la pagina tramite un browser. Come illustrato nella figura 7, verrà visualizzato un controllo GridView Elenca informazioni su ogni account utente nel sistema.


[![Il controllo UserGrid GridView Elenca le informazioni relative a ciascun utente nel sistema](role-based-authorization-vb/_static/image20.png)](role-based-authorization-vb/_static/image19.png)

**Figura 7**: il `UserGrid` GridView Elenca informazioni su ogni utente nel sistema ([fare clic per visualizzare l'immagine con dimensioni normali](role-based-authorization-vb/_static/image21.png))


> [!NOTE]
> Il `UserGrid` GridView sono elencati tutti gli utenti in un'interfaccia non di paging. Questa interfaccia come griglia semplice non è adatta per scenari in cui sono presenti diversi dozzina o più utenti. Una possibilità consiste nel configurare il controllo GridView per attivare il paging. Il `Membership.GetAllUsers` metodo presenta due overload: uno che non accetta alcun parametro di input e restituisce tutti gli utenti e uno che accetta valori interi per l'indice della pagina e dimensioni di pagina e restituisce solo il subset di utenti specificato. Il secondo overload può essere utilizzato per pagina in modo più efficiente attraverso gli utenti poiché restituisce solo il subset preciso degli account utente invece *tutti* di essi. Se si dispone di migliaia di account utente, è possibile prendere in considerazione un'interfaccia basata su filtro, che mostra solo gli utenti il cui nome utente inizi con un carattere selezionato, ad esempio. Il [ `Membership.FindUsersByName` metodo](https://msdn.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) è ideale per la creazione di un'interfaccia utente basata su filtro. Si esaminerà la creazione di tale interfaccia in un'esercitazione futura.


Il controllo GridView offre incorporati di modifica ed eliminazione supporto quando il controllo è associato a un controllo origine dati configurata in modo corretto, ad esempio il SqlDataSource o ObjectDataSource. Il `UserGrid` GridView, tuttavia, presenta i dati a livello di codice associati; pertanto, è necessario scrivere codice per eseguire queste due attività. In particolare, è necessario creare i gestori eventi per il controllo GridView `RowEditing`, `RowCancelingEdit`, `RowUpdating`, e `RowDeleting` eventi che vengono attivati quando un visitatore seleziona di GridView modifica, Cancel, aggiornamento, o eliminare i pulsanti.

Per iniziare, creare i gestori eventi per il controllo GridView `RowEditing`, `RowCancelingEdit`, e `RowUpdating` eventi e quindi aggiungere il codice seguente:

[!code-vb[Main](role-based-authorization-vb/samples/sample7.vb)]

Il `RowEditing` e `RowCancelingEdit` gestori eventi è sufficiente impostano il controllo GridView `EditIndex` proprietà e poi rieseguiremo l'associazione di elenco dell'utente account alla griglia. Le cose interessanti avviene nella `RowUpdating` gestore dell'evento. Questo gestore dell'evento inizia assicurandosi che i dati sia validi e quindi estrae il `UserName` dell'account utente modificato dal valore di `DataKeys` raccolta. Il `Email` e `Comment` caselle di testo in 'TemplateFields due `EditItemTemplate` s quindi fanno riferimento a livello di codice. Loro `Text` proprietà contengono l'indirizzo di posta elettronica modificato e il commento.

Per aggiornare un account utente tramite l'API di appartenenza è necessario prima ottenere le informazioni dell'utente, come in questo caso tramite una chiamata a `Membership.GetUser(userName)`. L'oggetto restituito `MembershipUser` dell'oggetto `Email` e `Comment` proprietà vengono quindi aggiornate con i valori immessi nelle due caselle di testo dall'interfaccia di modifica. Infine, queste modifiche vengono salvate con una chiamata a [ `Membership.UpdateUser` ](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx). Il `RowUpdating` gestore eventi viene completato, è necessario ripristinare il controllo GridView alla relativa interfaccia pre-editing.

A questo punto, creare il `RowDeleting` RowDeleting gestore dell'evento e quindi aggiungere il codice seguente:

[!code-vb[Main](role-based-authorization-vb/samples/sample8.vb)]

Il gestore dell'evento precedente inizia utilizzando il `UserName` valore del controllo GridView `DataKeys` raccolta; in questo `UserName` valore viene quindi passato alla classe di appartenenza [ `DeleteUser` metodo](https://msdn.microsoft.com/library/system.web.security.membership.deleteuser.aspx). Il `DeleteUser` metodo elimina l'account utente dal sistema, inclusi i dati di appartenenza correlati (ad esempio i ruoli di questo utente appartiene a). Dopo l'eliminazione, la griglia dell'utente `EditIndex` è impostato su -1 (nel caso in cui l'utente fa clic su Elimina mentre un'altra riga era in modalità di modifica) e il `BindUserGrid` viene chiamato il metodo.

> [!NOTE]
> Il pulsante di eliminazione non richiede una sorta di conferma da parte dell'utente prima di eliminare l'account utente. Consiglia di aggiungere una forma di conferma dell'utente per ridurre la probabilità di un account di eliminazione accidentale. Uno dei modi più semplici per confermare un'azione è tramite una finestra di dialogo di conferma dal lato client. Per altre informazioni su questa tecnica, vedere [aggiunta di conferma dal lato Client quando Elimina](https://asp.net/learn/data-access/tutorial-42-vb.aspx).


Verificare che questa pagina funziona come previsto. È necessario essere in grado di modificare qualsiasi indirizzo e-mail di utente e il commento, nonché eliminare un account utente. Poiché il `RoleBasedAuthorization.aspx` pagina è accessibile a tutti gli utenti, tutti – visitatori anonimi anche: gli utenti possono visitare questa pagina e modificare ed eliminare gli account utente. È possibile aggiornare questa pagina in modo che solo gli utenti nei ruoli supervisori e gli amministratori possono modificare indirizzo di posta elettronica dell'utente e il commento e solo gli amministratori possono eliminare un account utente.

La sezione "Uso del controllo LoginView" vengono esaminati tramite il controllo LoginView per visualizzare istruzioni specifiche per il ruolo dell'utente. Se una persona al ruolo Administrators visita questa pagina, si visualizzerà le istruzioni su come modificare ed eliminare gli utenti. Se un utente nel ruolo supervisori raggiunge questa pagina, illustreremo le istruzioni su come modificare gli utenti. E se il visitor è anonima o non è nel ruolo di amministratori o i supervisori, verrà visualizzato un messaggio che indica che essi non è possibile modificare o eliminare informazioni sull'account utente. Nella sezione "Limitazione a livello di codice la funzionalità" si scriverà codice che a livello di codice Mostra o nasconde i pulsanti di modifica ed eliminazione in base al ruolo dell'utente.

### <a name="using-the-loginview-control"></a>Utilizzare il controllo LoginView

Come abbiamo visto nelle esercitazioni precedenti, il controllo LoginView è utile per visualizzare le interfacce diverse per gli utenti autenticati e anonimi, ma il controllo LoginView è anche utilizzabile per visualizzare il markup diversi in base ai ruoli dell'utente. È possibile usare un controllo LoginView per visualizzare istruzioni diverse in base al ruolo dell'utente.

Inizio aggiungendo un LoginView sopra il `UserGrid` GridView. Come illustrato in precedenza, il controllo LoginView dispone di due modelli predefiniti: `AnonymousTemplate` e `LoggedInTemplate`. Immettere un messaggio breve in entrambi i modelli che informi l'utente che è possibile modificare o eliminare le informazioni sull'utente.

[!code-aspx[Main](role-based-authorization-vb/samples/sample9.aspx)]

Oltre al `AnonymousTemplate` e `LoggedInTemplate`, può includere il controllo LoginView *RoleGroup*, quali sono i modelli di ruolo specifiche. Ogni RoleGroup contiene una singola proprietà, `Roles`, che consente di specificare i ruoli di RoleGroup si applica a. Il `Roles` proprietà può essere impostata per un singolo ruolo (ad esempio "Administrators") o per un elenco delimitato da virgole dei ruoli (ad esempio "Administrators", Supervisor).

Per gestire il RoleGroup, fare clic sul collegamento "Modifica RoleGroup" da Smart Tag del controllo per visualizzare Editor raccolta di RoleGroup. Aggiungere due nuove RoleGroup. Impostare il primo RoleGroup `Roles` proprietà su "Administrators" e il secondo per "Supervisori".


[![Gestire i modelli specifici per il ruolo di LoginView tramite l'Editor della raccolta RoleGroup](role-based-authorization-vb/_static/image23.png)](role-based-authorization-vb/_static/image22.png)

**Figura 8**: la gestione specifiche del ruolo modelli tramite l'insieme RoleGroup di LoginView ([fare clic per visualizzare l'immagine con dimensioni normali](role-based-authorization-vb/_static/image24.png))


Fare clic su OK per chiudere l'Editor della raccolta RoleGroup; Aggiorna markup dichiarativo di LoginView per includere un `<RoleGroups>` sezione con un `<asp:RoleGroup>` elemento figlio di ogni RoleGroup definito RoleGroup Nell'Editor della raccolta. Inoltre, l'elenco a discesa "Visualizzazioni" elenco Smart Tag di LoginView - inizialmente incluso nell'elenco solo il `AnonymousTemplate` e `LoggedInTemplate` – include ora anche la RoleGroup aggiunto.

Modificare il RoleGroup in modo che gli utenti nel ruolo supervisori sono visualizzate le istruzioni su come modificare gli account utente, mentre gli utenti nel ruolo Administrators vengono visualizzati le istruzioni per la modifica e l'eliminazione. Dopo aver apportato queste modifiche, markup dichiarativo di LoginView dovrebbe essere simile al seguente.

[!code-aspx[Main](role-based-authorization-vb/samples/sample10.aspx)]

Dopo aver apportato queste modifiche, salvare la pagina e quindi lo si accede tramite un browser. Prima di tutto, visitare la pagina come utente anonimo. Si deve essere visualizzato il messaggio, "non si è connessi al sistema. Di conseguenza è possibile modificare o eliminare tutte le informazioni utente." Accedere quindi come un utente autenticato, ma che non è nel ruolo supervisori né gli amministratori. Questa volta verrà visualizzato il messaggio, "non si è un membro dei ruoli supervisori e agli amministratori. Di conseguenza è possibile modificare o eliminare tutte le informazioni utente."

Successivamente, accedere come utente membro del ruolo supervisori. Questa volta si noterà i supervisori specifiche del ruolo del messaggio (vedere la figura 9). E se si accede come utente negli amministratori del ruolo dovrebbe essere gli amministratori di ruolo specifiche del messaggio (vedere la figura 10).


[![Bruce viene visualizzato il messaggio specifiche del ruolo supervisori](role-based-authorization-vb/_static/image26.png)](role-based-authorization-vb/_static/image25.png)

**Figura 9**: Bruce viene visualizzato il messaggio specifiche del ruolo supervisori ([fare clic per visualizzare l'immagine con dimensioni normali](role-based-authorization-vb/_static/image27.png))


[![Tito viene visualizzato il messaggio specifiche del ruolo di amministratori](role-based-authorization-vb/_static/image29.png)](role-based-authorization-vb/_static/image28.png)

**Figura 10**: Tito viene visualizzato il messaggio specifiche del ruolo di amministratori ([fare clic per visualizzare l'immagine con dimensioni normali](role-based-authorization-vb/_static/image30.png))


Come le schermate di figure 9 e 10 show, di LoginView solo esegue il rendering di un modello, anche se applicano più modelli. Bruce e Tito sono entrambi gli utenti connessi, ma il LoginView esegue il rendering solo il RoleGroup corrispondente e non il `LoggedInTemplate`. Inoltre, Tito appartiene ai supervisori sia agli amministratori di ruoli, anche se il controllo LoginView viene eseguito il rendering del modello, specifiche del ruolo amministratori invece Supervisor uno.

Figura 11 viene illustrato il flusso di lavoro utilizzato dal controllo LoginView per determinare quale modello per eseguire il rendering. Si noti che se è presente più di uno RoleGroup specificato, il modello di LoginView esegue il rendering di *primo* RoleGroup corrispondente. In altre parole, se abbiamo avessimo posizionate RoleGroup i supervisori come il primo RoleGroup e gli amministratori come il secondo, quando Tito visita questa pagina egli verrebbe visualizzato il messaggio supervisori.


[![Flusso di lavoro del controllo LoginView per determinare quale modello per il rendering](role-based-authorization-vb/_static/image32.png)](role-based-authorization-vb/_static/image31.png)

**Figura 11**: flusso di lavoro del controllo LoginView The per determinare quali modello per il rendering ([fare clic per visualizzare l'immagine con dimensioni normali](role-based-authorization-vb/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Limitazione a livello di codice della funzionalità

Mentre il controllo LoginView consente di visualizzare istruzioni diverse in base al ruolo dell'utente visitando la pagina, restano visibili a tutti i pulsanti di modifica e Annulla. È necessario nascondere a livello di codice i pulsanti di modifica ed eliminazione per i visitatori anonimi e gli utenti nel ruolo supervisori né gli amministratori. È necessario nascondere il pulsante di eliminazione per tutti gli utenti che non sia un amministratore. A tal fine si scriverà un po' di codice che fa riferimento a livello di codice il CommandField modifica ed Elimina LinkButton e set di loro `Visible` proprietà `False`, se necessario.

Il modo più semplice per fare riferimento a livello di programmazione di controlli in un CommandField consiste innanzitutto convertirlo in un modello. A tale scopo, fare clic sul collegamento "Modifica colonne" dallo Smart Tag del controllo GridView, selezionare il CommandField dall'elenco di campi correnti e fare clic sul collegamento "Converti il campo in un TemplateField". Il CommandField si trasforma in un TemplateField con un `ItemTemplate` e `EditItemTemplate`. Il `ItemTemplate` contiene la modifica ed Elimina LinkButton durante il `EditItemTemplate` ospita l'aggiornamento e annullamento LinkButton.


[![Convertire il CommandField in un TemplateField](role-based-authorization-vb/_static/image35.png)](role-based-authorization-vb/_static/image34.png)

**Figura 12**: converte il CommandField in un TemplateField ([fare clic per visualizzare l'immagine con dimensioni normali](role-based-authorization-vb/_static/image36.png))


Aggiornare la modifica ed Elimina LinkButton nel `ItemTemplate`, l'impostazione loro `ID` vengono impostate su valori di `EditButton` e `DeleteButton`, rispettivamente.

[!code-aspx[Main](role-based-authorization-vb/samples/sample11.aspx)]

Ogni volta che i dati sono associati a GridView, il controllo GridView enumera i record nel relativo `DataSource` proprietà e genera un oggetto corrispondente `GridViewRow` oggetto. Dato che ogni `GridViewRow` viene creato l'oggetto, il `RowCreated` evento. Per nascondere i pulsanti di modifica ed eliminazione per gli utenti non autorizzati, è necessario creare un gestore eventi per questo evento e fa riferimento a livello di codice modifica ed Elimina LinkButton, impostando i `Visible` le proprietà di conseguenza.

Creare un gestore eventi di `RowCreated` evento e quindi aggiungere il codice seguente:

[!code-vb[Main](role-based-authorization-vb/samples/sample12.vb)]

Tenere presente che il `RowCreated` evento viene generato per *tutto* righe GridView, inclusi l'intestazione, piè di pagina, l'interfaccia cercapersone e così via. Si vuole solo a livello di codice fa riferimento a Edit e Delete LinkButton se sono legati esclusivamente una riga di dati non in modalità di modifica (poiché la riga in modalità di modifica è disponibili pulsanti di aggiornamento e annullamento anziché modifica ed eliminazione). Questo controllo viene gestito dal `If` istruzione.

Se sono legati esclusivamente una riga di dati che non è in modalità di modifica, la modifica ed Elimina LinkButton si fa riferimento e i relativi `Visible` delle proprietà vengono impostate in base i valori booleani restituiti dal `User` dell'oggetto `IsInRole(roleName)` (metodo). Il `User` oggetto vi fa riferimento l'entità è stata creata per il `RoleManagerModule`; di conseguenza, il `IsInRole(roleName)` metodo Usa l'API di ruoli per determinare se il visitatore corrente appartiene al *roleName*.

> [!NOTE]
> Si sarebbe potuto usare la classe di ruoli direttamente, sostituendo la chiamata a `User.IsInRole(roleName)` con una chiamata per il [ `Roles.IsUserInRole(roleName)` metodo](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx). Ho deciso di utilizzare l'oggetto principal `IsInRole(roleName)` metodo in questo esempio perché è più efficiente rispetto all'utilizzo direttamente l'API di ruoli. In precedenza in questa esercitazione è stato configurato la gestione dei ruoli per memorizzare nella cache i ruoli dell'utente in un cookie. Questo cookie dati memorizzati nella cache viene utilizzata soltanto quando l'entità `IsInRole(roleName)` viene chiamato il metodo; le chiamate dirette all'API di ruoli comportano sempre una corsa all'archivio ruoli. Anche se i ruoli non sono memorizzate nella cache in un cookie, la chiamata a un oggetto entità `IsInRole(roleName)` metodo è in genere più efficiente perché quando viene chiamato per la prima volta durante una richiesta viene memorizzato nella cache i risultati. L'API di ruoli, d'altra parte, non esegue qualsiasi la memorizzazione nella cache. Poiché il `RowCreated` evento viene generato una volta per ogni riga in GridView, usando `User.IsInRole(roleName)` implica semplicemente un unico round trip all'archivio di ruolo, mentre `Roles.IsUserInRole(roleName)` richiede *N* trip, dove *N* è il numero di account utente visualizzati nella griglia.


Il pulsante di modifica `Visible` è impostata su `True` se l'utente visita questa pagina è nel ruolo Administrators o supervisori; in caso contrario, impostarlo su `False`. Il pulsante di eliminazione `Visible` è impostata su `True` solo se l'utente è nel ruolo Administrators.

Testare questa pagina tramite un browser. Se si visita la pagina come un visitatore anonimo o un utente che non è né un supervisore né un amministratore, il CommandField è vuoto. è ancora presente, ma come un frammento thin senza la modifica o Elimina i pulsanti.

> [!NOTE]
> È possibile nascondere la CommandField completamente quando un non-supervisore e senza privilegi di amministratore è sinonimo di visitare la pagina. Lasciare come esercizio per il lettore.


[![La modifica ed elimina i pulsanti sono nascoste per Non-supervisori e i Non amministratori](role-based-authorization-vb/_static/image38.png)](role-based-authorization-vb/_static/image37.png)

**Figura 13**: la modifica ed elimina i pulsanti sono nascoste per Non-supervisori e i Non amministratori ([fare clic per visualizzare l'immagine con dimensioni normali](role-based-authorization-vb/_static/image39.png))


Se un utente appartenente al ruolo supervisori (ma non per il ruolo di amministratore) visita, vede solo il pulsante di modifica.


[![Anche se il pulsante Modifica è disponibile per i supervisori, il pulsante Elimina è nascosta](role-based-authorization-vb/_static/image41.png)](role-based-authorization-vb/_static/image40.png)

**Figura 14**: anche se il pulsante Modifica è disponibile per i supervisori, pulsante Elimina viene nascosta ([fare clic per visualizzare l'immagine con dimensioni normali](role-based-authorization-vb/_static/image42.png))


E se un amministratore di visita, susie ha accesso a entrambi i pulsanti di modifica ed eliminazione.


[![La modifica ed elimina i pulsanti sono disponibili solo per gli amministratori](role-based-authorization-vb/_static/image44.png)](role-based-authorization-vb/_static/image43.png)

**Figura 15**: eliminare pulsanti e la modifica sono disponibili solo per gli amministratori ([fare clic per visualizzare l'immagine con dimensioni normali](role-based-authorization-vb/_static/image45.png))


## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>Passaggio 3: Applicazione delle regole di autorizzazione basata sui ruoli alle classi e metodi

Nel passaggio 2 che è stato ridotto modificare funzionalità agli utenti nei ruoli supervisori e agli amministratori ed eliminare solo le funzionalità agli amministratori. Questa operazione è stata eseguita, nascondendo gli elementi dell'interfaccia utente associato per gli utenti non autorizzati tramite tecniche a livello di codice. Queste misure non garantisce che un utente non autorizzato sarà Impossibile eseguire un'azione con privilegi. Possono esistere elementi dell'interfaccia utente che vengono aggiunti in un secondo momento o che è stato dimenticato di nascondere per gli utenti non autorizzati. O un pirata informatico potrebbe individuare un altro modo per ottenere la pagina ASP.NET per eseguire il metodo desiderato.

È un modo semplice per garantire che una particolare parte della funzionalità non è accessibile da un utente non autorizzato per decorare quella classe o un metodo con il [ `PrincipalPermission` attributo](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Quando il runtime di .NET viene utilizzata una classe o esegue uno dei relativi metodi, verifica per assicurarsi che il contesto di sicurezza corrente dispone dell'autorizzazione. Il `PrincipalPermission` attributo fornisce un meccanismo attraverso il quale è possibile definire queste regole.

È stata esaminata tramite il `PrincipalPermission` dell'attributo nel <a id="_msoanchor_9"> </a> [ *autorizzazione basata su utente* ](../membership/user-based-authorization-vb.md) esercitazione. In particolare, abbiamo visto come decorare il controllo GridView `SelectedIndexChanged` e `RowDeleting` gestore dell'evento in modo che si può essere eseguiti solo da utenti autenticati e Tito, rispettivamente. Il `PrincipalPermission` attributo funziona altrettanto bene con i ruoli.

Di seguito viene illustrato l'utilizzo di `PrincipalPermission` attributo del controllo GridView `RowUpdating` e `RowDeleting` gestori eventi per non consentire l'esecuzione per gli utenti non autorizzati. Tutto dobbiamo fare è aggiungere l'attributo appropriato nella parte superiore di ogni definizione di funzione:

[!code-vb[Main](role-based-authorization-vb/samples/sample13.vb)]

L'attributo per il `RowUpdating` gestore dell'evento indica che solo gli utenti nei ruoli amministratori o i supervisori possono eseguire il gestore eventi, mentre l'attributo nel `RowDeleting` gestore eventi consente di limitare l'esecuzione agli utenti negli amministratori ruolo.


> [!NOTE]
> Il `PrincipalPermission` attributo è rappresentato come una classe nel `System.Security.Permissions` dello spazio dei nomi. Assicurarsi di aggiungere un `Imports System.Security.Permissions` istruzione all'inizio del file di classe code-behind per importare questo spazio dei nomi.


Se, in qualche modo, un utente non amministratore tenta di eseguire la `RowDeleting` gestore dell'evento o se un non-Supervisor o senza privilegi di amministratore tenta di eseguire il `RowUpdating` gestore eventi, il runtime di .NET verrà generato un `SecurityException`.


[![Se il contesto di sicurezza non è autorizzato a eseguire il metodo, viene generata un'eccezione SecurityException](role-based-authorization-vb/_static/image47.png)](role-based-authorization-vb/_static/image46.png)

**Figura 16**: se il contesto di sicurezza non è autorizzato a eseguire il metodo, una `SecurityException` generata ([fare clic per visualizzare l'immagine con dimensioni normali](role-based-authorization-vb/_static/image48.png))


Oltre alle pagine ASP.NET, molte applicazioni hanno anche un'architettura che include vari livelli, ad esempio la logica di Business e livelli di accesso ai dati. Questi livelli vengono in genere implementati come librerie di classi e classi e metodi per l'esecuzione di funzionalità relative ai dati e per la logica di business dell'offerta. Il `PrincipalPermission` attributo è utile per l'applicazione delle regole di autorizzazione a questi livelli anche.

Per altre informazioni sull'uso di `PrincipalPermission` attributo per definire le regole di autorizzazione sulle classi e metodi, fare riferimento a [Scott Guthrie](https://weblogs.asp.net/scottgu/)del post di blog [aggiunta di regole di autorizzazione per Business e l'utilizzo di livelli di dati `PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come specificare grossolano e le regole di autorizzazione granulare in base ai ruoli dell'utente. ASP. Funzionalità di autorizzazione URL della rete consente a uno sviluppatore di pagina specificare le identità sono consentito o negato l'accesso a quali pagine. Come abbiamo visto nella <a id="_msoanchor_10"> </a> [ *autorizzazione basata su utente* ](../membership/user-based-authorization-vb.md) autorizzazione URL dell'esercitazione, le regole possono essere applicate in base a utente. Possono anche essere applicati a intervalli di ruolo per ruolo, come mostrato nel passaggio 1 di questa esercitazione.

È possibile applicare le regole di autorizzazione granulare in modo dichiarativo o a livello di codice. Nel passaggio 2 è stato con funzionalità RoleGroup del controllo LoginView per eseguire il rendering in base ai ruoli dell'utente visita un output diverso. È stato anche modi per determinare a livello di programmazione se un utente appartiene a un ruolo specifico e su come modificare di conseguenza la funzionalità della pagina.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Aggiunta di regole di autorizzazione al Business e i livelli di dati utilizzando `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Analisi di ASP.NET 2.0 appartenenza, ruoli e profilo: utilizzo dei ruoli](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [Elenco di domande di sicurezza per ASP.NET 2.0](https://msdn.microsoft.com/library/ms998375.aspx)
- [Documentazione tecnica per il `<roleManager>` elemento](https://msdn.microsoft.com/library/ms164660.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla.com, ha lavorato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola  *[Sams Teach Yourself ASP.NET 2.0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. È possibile contattarlo Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali...

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione includono Suchi Banerjee e Teresa Murphy. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami un messaggio nella [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](assigning-roles-to-users-vb.md)
