---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-vb
title: Autorizzazione basata sui ruoli (VB) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione inizia con uno sguardo al modo in cui il framework di ruoli associa ruoli di un utente con il suo contesto di sicurezza. Viene quindi esaminato come applicare l'URL basato su ruoli...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 83b4f5a4-4f5a-4380-ba33-f0b5c5ac6a75
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: 1ca92fd194ed36f55c58666145efe445fd92823b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892153"
---
<a name="role-based-authorization-vb"></a>Autorizzazione basata sui ruoli (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.11.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_vb.pdf)

> In questa esercitazione inizia con uno sguardo al modo in cui il framework di ruoli associa ruoli di un utente con il suo contesto di sicurezza. Viene quindi esaminato come applicare le regole di autorizzazione URL basata su ruoli. In seguito, che verranno esaminati utilizzando dichiarativi e programmatici per la modifica dei dati visualizzati e le funzionalità offerte da una pagina ASP.NET.


## <a name="introduction"></a>Introduzione

Nel <a id="_msoanchor_1"> </a> [ *autorizzazione basata sull'utente* ](../membership/user-based-authorization-vb.md) esercitazione è stato illustrato come utilizzare l'autorizzazione di URL per specificare quali utenti potrebbero visitare un particolare set di pagine. Con un minimo di markup in `Web.config`, è possibile indicare a consentire solo agli utenti autenticati di visitare una pagina ASP.NET. È stato possibile prevedono che sia stato ammesso solo gli utenti Tito e Bob, o indicare che tutti gli utenti autenticati, ad eccezione di Sam fosse consentiti.

Oltre ai autorizzazione URL, abbiamo esaminato anche tecniche dichiarative e programmatici per controllare i dati visualizzati e le funzionalità offerte da una pagina in base all'utente visita. In particolare, è creata una pagina in cui è elencato il contenuto della directory corrente. Chiunque può visitare questa pagina, ma solo gli utenti autenticati Impossibile visualizzare il contenuto dei file e solo Tito è stato possibile eliminare i file.

Applicazione delle regole di autorizzazione su base utente per l'utente può assumere un incubo contabilità. Un approccio migliore consiste nell'utilizzare autorizzazione basata sui ruoli. Buone notizie sono che gli strumenti di disposizione per l'applicazione delle regole di autorizzazione funzionano altrettanto bene con i ruoli, come avviene per gli account utente. Regole di autorizzazione URL è possono specificare ruoli anziché agli utenti. Il controllo LoginView, che esegue il rendering di un output diverso per gli utenti autenticati e anonimi, può essere configurato per visualizzare contenuto diverso in base ai ruoli dell'utente connesso. E l'API di ruoli include metodi per determinare i ruoli dell'utente connesso.

In questa esercitazione inizia con uno sguardo al modo in cui il framework di ruoli associa ruoli di un utente con il suo contesto di sicurezza. Viene quindi esaminato come applicare le regole di autorizzazione URL basata su ruoli. In seguito, che verranno esaminati utilizzando dichiarativi e programmatici per la modifica dei dati visualizzati e le funzionalità offerte da una pagina ASP.NET. Iniziamo!

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>Informazioni sulle modalità ruoli sono associati un contesto di sicurezza

Ogni volta che una richiesta attiva della pipeline ASP.NET è associata a un contesto di sicurezza, che include informazioni che identificano il richiedente. Quando si utilizza l'autenticazione basata su form, un ticket di autenticazione viene utilizzato come un token di identità. Come accennato nel <a id="_msoanchor_2"> </a> [ *una panoramica dell'autenticazione basata su form* ](../introduction/an-overview-of-forms-authentication-vb.md) e <a id="_msoanchor_3"> </a> [ *form Configurazione dell'autenticazione e argomenti avanzati* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) esercitazioni, la `FormsAuthenticationModule` è responsabile della determinazione dell'identità del richiedente, in cui viene fatto durante la [ `AuthenticateRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Se viene trovato un ticket di autenticazione valido non scaduto, il `FormsAuthenticationModule` decodifica per verificare l'identità del richiedente. Viene creato un nuovo `GenericPrincipal` dell'oggetto e la assegna a questa opzione per il `HttpContext.User` oggetto. Lo scopo di un'entità, ad esempio `GenericPrincipal`, consiste nell'identificare il nome dell'utente autenticato e gli elementi che appartengono a ruoli. Lo scopo è possibile dedurre dal fatto che tutti gli oggetti principali hanno un `Identity` proprietà e un `IsInRole(roleName)` metodo. Il `FormsAuthenticationModule`, tuttavia, non è interessato nella registrazione delle informazioni sul ruolo e `GenericPrincipal` crea l'oggetto non specifica alcun ruolo.

Se è abilitato il framework di ruoli, la [ `RoleManagerModule` ](https://msdn.microsoft.com/library/system.web.security.rolemanagermodule.aspx) modulo HTTP passaggi dopo il `FormsAuthenticationModule` e identifica i ruoli dell'utente autenticato durante il [ `PostAuthenticateRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), che viene generato dopo il `AuthenticateRequest` evento. Se la richiesta proviene da un utente autenticato, il `RoleManagerModule` sovrascrive il `GenericPrincipal` oggetto creato tramite il `FormsAuthenticationModule` e lo sostituisce con un [ `RolePrincipal` oggetto](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx). La `RolePrincipal` classe Usa l'API di ruoli per determinare quali ruoli a cui appartiene l'utente.

Figura 1 illustra il flusso di lavoro della pipeline ASP.NET quando si utilizza l'autenticazione basata su form e il framework di ruoli. Il `FormsAuthenticationModule` viene eseguito per primo, identifica l'utente tramite il ticket di autenticazione e crea un nuovo `GenericPrincipal` oggetto. Successivamente, il `RoleManagerModule` di passaggi e sovrascrive la `GenericPrincipal` dell'oggetto con un `RolePrincipal` oggetto.

Se un utente anonimo visita il sito, né il `FormsAuthenticationModule` né la `RoleManagerModule` crea un oggetto entità.


[![Gli eventi Pipeline ASP.NET per un utente autenticato quando si utilizza l'autenticazione basata su form e il Framework di ruoli](role-based-authorization-vb/_static/image2.png)](role-based-authorization-vb/_static/image1.png)

**Figura 1**: il eventi della Pipeline ASP.NET per un'autenticazione utente quando tramite autenticazione basata su form e il Framework di ruoli ([fare clic per visualizzare l'immagine ingrandita](role-based-authorization-vb/_static/image3.png))


### <a name="caching-role-information-in-a-cookie"></a>La memorizzazione nella cache le informazioni sui ruoli in un Cookie

Il `RolePrincipal` dell'oggetto `IsInRole(roleName)` chiamate al metodo `Roles`.`GetRolesForUser` Per ottenere i ruoli per l'utente per determinare se l'utente è un membro di *roleName*. Quando si utilizza il `SqlRoleProvider`, ciò comporta una query per il database dell'archivio ruoli. Quando si utilizzano regole di autorizzazione URL basata sui ruoli di `RolePrincipal`del `IsInRole` metodo verrà chiamato a ogni richiesta a una pagina che è protetto dalle regole di autorizzazione URL basata su ruoli. Anziché dover cercare le informazioni sui ruoli nel database in ogni richiesta, il `Roles` framework include un'opzione per memorizzare nella cache i ruoli dell'utente in un cookie.

Se il framework di ruoli è configurato per memorizzare nella cache i ruoli dell'utente in un cookie, il `RoleManagerModule` crea il cookie durante della pipeline ASP.NET [ `EndRequest` evento](https://msdn.microsoft.com/library/system.web.httpapplication.endrequest.aspx). Questo cookie viene usato nelle richieste successive nella `PostAuthenticateRequest`, ovvero quando la `RolePrincipal` viene creato l'oggetto. Se il cookie è valido e non sia scaduto, i dati nel cookie vengano analizzati e utilizzati per popolare i ruoli dell'utente, evitando così la `RolePrincipal` di dover eseguire una chiamata alla `Roles` classe per determinare i ruoli dell'utente. Figura 2 illustra il flusso di lavoro.


[![Le informazioni sui ruoli dell'utente possono essere archiviati in un Cookie per migliorare le prestazioni](role-based-authorization-vb/_static/image5.png)](role-based-authorization-vb/_static/image4.png)

**Figura 2**: ruolo informazioni possono essere archiviati l'utente in un Cookie per migliorare le prestazioni ([fare clic per visualizzare l'immagine ingrandita](role-based-authorization-vb/_static/image6.png))


Per impostazione predefinita, il meccanismo dei cookie ruolo della cache è disabilitato. Può essere abilitata tramite il `<roleManager>`; markup di configurazione in `Web.config`. È stato illustrato l'utilizzo di [ `<roleManager>` elemento](https://msdn.microsoft.com/library/ms164660.aspx) per specificare i provider di ruoli nel <a id="_msoanchor_4"> </a> [ *creazione e gestione dei ruoli* ](creating-and-managing-roles-vb.md) esercitazione in modo da questo elemento si dovrebbe già disporre dell'applicazione `Web.config` file. Le impostazioni di cookie nella cache di ruolo vengono specificate come attributi del `<roleManager>`; elemento e sono riepilogati nella tabella 1.

> [!NOTE]
> Le impostazioni di configurazione elencate nella tabella 1 specificano le proprietà del cookie di cache ruoli risultante. Per altre informazioni su cookie, il funzionamento e le varie proprietà, vedere [questa esercitazione i cookie](http://www.quirksmode.org/js/cookies.html).


| <strong>Property</strong> |                                                                                                                                                                                                                                                                                                                                                         <strong>Descrizione</strong>                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   `cacheRolesInCookie`    |                                                                                                                                                                                                                                                                                                                              Valore booleano che indica se viene utilizzata la memorizzazione nella cache di cookie. Il valore predefinito è `false`.                                                                                                                                                                                                                                                                                                                              |
|       `cookieName`        |                                                                                                                                                                                                                                                                                                                                     Il nome del cookie di cache di ruoli. Il valore predefinito è ". ASPXROLES".                                                                                                                                                                                                                                                                                                                                     |
|       `cookiePath`        |                                                                                                                                                                                                                                Il percorso del cookie del nome dei ruoli. L'attributo path consente a uno sviluppatore limitare l'ambito di un cookie a una gerarchia di directory specifico. Il valore predefinito è "/", che informa il browser invii il cookie di ticket di autenticazione a qualsiasi richiesta eseguita al dominio.                                                                                                                                                                                                                                 |
|    `cookieProtection`     |                                                                                                                                                               Indica le tecniche utilizzate per proteggere il cookie di cache di ruolo. I valori consentiti sono: `All` (impostazione predefinita). `Encryption`; `None`; e `Validation`. Fare riferimento al passaggio 3 di <a id="_anchor_5"> </a> [ *configurazione dell'autenticazione form e argomenti avanzati* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) esercitazione per ulteriori informazioni su questi livelli di protezione.                                                                                                                                                                |
|    `cookieRequireSSL`     |                                                                                                                                                                                                                                                                                                   Valore booleano che indica se è necessaria una connessione SSL per trasmettere il cookie di autenticazione. Il valore predefinito è `false`.                                                                                                                                                                                                                                                                                                   |
| `cookieSlidingExpiration` |                                                                                                                                                                                                                                                  Valore booleano che indica che se il timeout del cookie viene reimpostato ogni volta che l'utente visita il sito durante una singola sessione. Il valore predefinito è `false`. Questo valore è pertinente solo quando `createPersistentCookie` è impostato su `true`.                                                                                                                                                                                                                                                  |
|      `cookieTimeout`      |                                                                                                                                                                                                                                                                         Specifica il tempo, espresso in minuti, trascorso il quale il cookie di ticket di autenticazione scade. Il valore predefinito è `30`. Questo valore è pertinente solo quando `createPersistentCookie` è impostato su `true`.                                                                                                                                                                                                                                                                         |
| `createPersistentCookie`  |                                                                                                                                                                   Valore booleano che specifica se il cookie di cache di ruolo è un cookie di sessione o persistente. Se `false` (impostazione predefinita), viene utilizzato un cookie di sessione, che viene eliminato quando il browser viene chiuso. Se `true`, viene utilizzato un cookie permanente, data di scadenza `cookieTimeout` numero di minuti dopo che è stato creato o dopo la visita precedente, a seconda del valore di `cookieSlidingExpiration`.                                                                                                                                                                    |
|         `domain`          |                                                                                                                                                 Specifica il valore di dominio del cookie. Il valore predefinito è una stringa vuota, provocando il browser da usare il dominio da cui è stato rilasciato (ad esempio www.yourdomain.com). In questo caso, il cookie verrà <strong>non</strong> da inviare quando apportato richieste per i sottodomini, ad esempio admin.yourdomain.com. Se si desidera che il cookie deve essere passato a tutti i sottodomini è necessario personalizzare il `domain` attributo, se è impostato su "<sottodominio>.nomedominio.com".                                                                                                                                                 |
|    `maxCachedResults`     | Specifica il numero massimo di nomi di ruoli memorizzati nella cache nel cookie. Il valore predefinito è 25. Il `RoleManagerModule` non crea un cookie per gli utenti che appartengono a più di `maxCachedResults` ruoli. Di conseguenza, il `RolePrincipal` dell'oggetto `IsInRole` metodo utilizzerà la `Roles` classe per determinare i ruoli dell'utente. Il motivo `maxCachedResults` esiste perché molti agenti utente non consente di limitare i cookie di dimensioni maggiori di 4.096 byte. Pertanto questa estremità è progettato per ridurre la probabilità di superare questa limitazione delle dimensioni. Se si dispone di nomi di ruolo estremamente lunghi, è consigliabile provare a specificare un minore `maxCachedResults` valore; contrariwise, se si dispone di nomi di ruolo molto breve, è possibile aumentare probabilmente questo valore. |

**Tabella 1**: le opzioni di configurazione di ruolo della Cache Cookie

Si configura l'applicazione di utilizzare i cookie di cache di ruolo non persistente. A tale scopo, aggiornare il `<roleManager>` elemento `Web.config` per includere i seguenti attributi cookie:

[!code-xml[Main](role-based-authorization-vb/samples/sample1.xml)]

Aggiornato il `<roleManager>`; elemento tramite l'aggiunta di tre attributi: `cacheRolesInCookie`, `createPersistentCookie`, e `cookieProtection`. Impostando `cacheRolesInCookie` a `true`, `RoleManagerModule` vengono ora automaticamente memorizzati nella cache i ruoli dell'utente in un cookie, anziché dover cercare le informazioni sui ruoli dell'utente a ogni richiesta. È impostato in modo esplicito il `createPersistentCookie` e `cookieProtection` attributi `false` e `All`, rispettivamente. Tecnicamente, non è necessario specificare i valori per questi attributi poiché semplicemente assegnati ai valori predefiniti, ma è inserire qui per renderlo in modo esplicito deselezionare che non si utilizza i cookie permanenti e che il cookie non è crittografata e convalidato.

Questo è tutto qui! Da questo momento, il framework di ruoli verrà memorizzati nella cache i ruoli degli utenti nei cookie. Se il browser dell'utente non supporta i cookie, o se i cookie vengono eliminati o persi, in qualche modo, è semplice: il `RolePrincipal` oggetto utilizzerà semplicemente il `Roles` classe nel caso che sia disponibile alcun cookie (o uno non valido o scaduto).

> [!NOTE]
> Modelli di Microsoft &amp; gruppo di procedure consigliate sconsiglia l'utilizzo dei cookie cache ruolo persistente. Poiché il possesso del cookie di cache di ruoli è sufficiente per dimostrare l'appartenenza al ruolo, se un pirata informatico in qualche modo riescono ad accedere al cookie di un utente valido può rappresentare tale utente. La probabilità che questo accada aumenta se il cookie è persistente nel browser dell'utente. Per ulteriori informazioni su questa raccomandazione di sicurezza, nonché altri problemi di sicurezza, vedere il [elenco domande di sicurezza per ASP.NET 2.0](https://msdn.microsoft.com/library/ms998375.aspx).


## <a name="step-1-defining-role-based-url-authorization-rules"></a>Passaggio 1: Definizione di regole di autorizzazione URL basata sui ruoli

Come descritto nel <a id="_msoanchor_6"> </a> [ *autorizzazione basata sull'utente* ](../membership/user-based-authorization-vb.md) esercitazione, l'autorizzazione URL offre un mezzo per limitare l'accesso a un set di pagine in un utente dall'utente o ruolo per ruolo base. Le regole di autorizzazione URL siano state digitate `Web.config` utilizzando il [ `<authorization>` elemento](https://msdn.microsoft.com/library/8d82143t.aspx) con `<allow>` e `<deny>` gli elementi figlio. Oltre alle regole di autorizzazione relativi agli utenti illustrate nelle esercitazioni precedenti, ogni `<allow>` e `<deny>` possa includere anche l'elemento figlio:

- Un particolare ruolo
- Un elenco delimitato da virgole dei ruoli

Le regole di autorizzazione URL, ad esempio, concedono l'accesso agli utenti nei ruoli amministratori e i supervisori ma negano l'accesso a tutti gli altri:

[!code-xml[Main](role-based-authorization-vb/samples/sample2.xml)]

Il `<allow>` elemento nel markup precedente indica che sono consentiti i ruoli agli amministratori e i supervisori; il `<deny>`; elemento indica che *tutti* viene negato agli utenti.

Consente di configurare l'applicazione in modo che il `ManageRoles.aspx`, `UsersAndRoles.aspx`, e `CreateUserWizardWithRoles.aspx` pagine accessibili solo agli utenti nel ruolo di amministratori, mentre il `RoleBasedAuthorization.aspx` pagina rimane accessibile a tutti i visitatori.

A tale scopo, aggiungere innanzitutto un `Web.config` file per il `Roles` cartella.


[![Aggiungere un File Web. config nella directory di ruoli](role-based-authorization-vb/_static/image8.png)](role-based-authorization-vb/_static/image7.png)

**Figura 3**: aggiungere un `Web.config` del File per il `Roles` directory ([fare clic per visualizzare l'immagine ingrandita](role-based-authorization-vb/_static/image9.png))


Successivamente, aggiungere il markup seguente di configurazione per `Web.config`:

[!code-xml[Main](role-based-authorization-vb/samples/sample3.xml)]

Il `<authorization>` elemento il `<system.web>` sezione indica che solo gli utenti al ruolo Administrators possono accedere alle risorse ASP.NET il `Roles` directory. Il `<location>` elemento definisce un set alternativo di regole di autorizzazione URL per il `RoleBasedAuthorization.aspx` pagina per consentire tutti gli utenti visitare la pagina.

Dopo aver salvato le modifiche apportate alla `Web.config`, accedere come un utente che non si trova il ruolo di amministratore e quindi tenta di visitare una delle pagine protette. Il `UrlAuthorizationModule` rileverà che non si dispone dell'autorizzazione per visitare la risorsa richiesta, di conseguenza, il `FormsAuthenticationModule` verrà reindirizzati alla pagina di accesso. Quindi, la pagina di accesso reindirizzerà l'utente per il `UnauthorizedAccess.aspx` pagina (vedere la figura 4). Questo reindirizzamento finale dalla pagina di accesso per `UnauthorizedAccess.aspx` si verifica a causa di codice viene aggiunto alla pagina di accesso nel passaggio 2 del <a id="_msoanchor_7"> </a> [ *autorizzazione basata sull'utente* ](../membership/user-based-authorization-vb.md) esercitazione. In particolare, la pagina di accesso reindirizza automaticamente qualsiasi utente autenticato a `UnauthorizedAccess.aspx` se la stringa di query contiene un `ReturnUrl` parametro, come questo parametro indica che l'utente è arrivato alla pagina di accesso dopo un tentativo di visualizzare una pagina che non è stato autorizzati a visualizzare.


[![Solo gli utenti nel ruolo gli amministratori possono visualizzare le pagine protette](role-based-authorization-vb/_static/image11.png)](role-based-authorization-vb/_static/image10.png)

**Figura 4**: solo gli utenti nel ruolo gli amministratori possono visualizzare le pagine protetto ([fare clic per visualizzare l'immagine ingrandita](role-based-authorization-vb/_static/image12.png))


Disconnettersi e quindi accedere come utente al ruolo Administrators. Ora deve essere in grado di visualizzare tre pagine protette.


[![Tito visitando che il UsersAndRoles.aspx pagina perché è nel ruolo Administrators](role-based-authorization-vb/_static/image14.png)](role-based-authorization-vb/_static/image13.png)

**Figura 5**: Tito possibile visitare il `UsersAndRoles.aspx` pagina perché è nel ruolo Administrators ([fare clic per visualizzare l'immagine ingrandita](role-based-authorization-vb/_static/image15.png))


> [!NOTE]
> Quando si specificano regole di autorizzazione URL: per i ruoli o utenti, è importante tenere presente che le regole sono analizzati uno alla volta, dall'alto verso il basso. Non appena viene rilevata una corrispondenza, l'utente viene concesso o negato l'accesso, a seconda se è stata trovata la corrispondenza un `<allow>` o `<deny>` elemento. **Se viene trovata alcuna corrispondenza, l'utente viene concesso l'accesso.** Di conseguenza, se si desidera limitare l'accesso a uno o più account utente, è essenziale usare un `<deny>` elemento come ultimo elemento nella configurazione di autorizzazione URL. **Se le regole di autorizzazione URL non includono un**`<deny>`**elemento, tutti gli utenti verranno concesso l'accesso.** Per una discussione più accurata sul modo in cui vengono analizzate le regole di autorizzazione URL, fare riferimento al "sul funzionamento di `UrlAuthorizationModule` utilizza le regole di autorizzazione per concedere o negare l'accesso" sezione del <a id="_msoanchor_8"> </a> [  *Autorizzazione basata sull'utente* ](../membership/user-based-authorization-vb.md) esercitazione.


## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>Passaggio 2: Limitare la funzionalità in base ai ruoli dell'utente attualmente connesso

Rende di autorizzazione URL è più facile specificare l'autorizzazione grossolana regole di tale stato quali identità sono consentiti e quelli non autorizzati alla visualizzazione di un particolare (o tutte le pagine in una cartella e nelle relative sottocartelle). Tuttavia, in alcuni casi potrebbe voler consentire tutti gli utenti di visitare una pagina, ma limitarne la funzionalità della pagina in base ai ruoli dell'utente. Questo può comportare nasconda o visualizzazione di dati in base al ruolo dell'utente o che offrono funzionalità aggiuntive per gli utenti che appartengono a un ruolo specifico.

Tali regole di autorizzazione basata sui ruoli di singole possono essere implementate in modo dichiarativo o a livello di codice (o tramite una combinazione dei due). Nella sezione successiva viene verrà illustrato come implementare l'autorizzazione dichiarativa grana tramite il controllo LoginView. Successivamente, verranno analizzati tecniche a livello di codice. Prima di poter esaminare l'applicazione delle regole di autorizzazione grana, tuttavia, è necessario creare una pagina di cui funzionalità dipende dal ruolo dell'utente visita il.

Creare una pagina che elenca tutti gli account utente nel sistema in un controllo GridView. GridView includerà username, indirizzo di posta elettronica, data di ultimo accesso e i commenti sull'utente di ogni utente. Oltre a visualizzare informazioni di ogni utente, GridView verranno includono modifica ed eliminare le funzionalità. Inizialmente verrà creato in questa pagina con la modifica e funzionalità disponibili per tutti gli utenti di eliminare. Nella sezione "Utilizzo del controllo LoginView" e "Limitazione a livello di codice la funzionalità" vedremo come abilitare o disabilitare queste funzionalità in base al ruolo dell'utente.

> [!NOTE]
> Per visualizzare gli account utente, la pagina ASP.NET che si sta tentando di compilare utilizza un controllo GridView. Poiché in questa esercitazione serie è incentrata sulla autenticazione basata su form, autorizzazione, gli account utente e ruoli, non si desidera dedicare troppo tempo che illustrano il funzionamento interno del controllo GridView. Durante questa esercitazione vengono fornite istruzioni dettagliate specifiche per la configurazione di questa pagina, non approfondire i dettagli di perché in cui sono state apportate alcune scelte, o quali dispone di proprietà particolare effetto sull'output sottoposto a rendering. Per un esame esauriente del controllo GridView, consultare il *[utilizzo dei dati in ASP.NET 2.0](../../data-access/index.md)* serie di esercitazioni.


Aprire il `RoleBasedAuthorization.aspx` nella pagina di `Roles` cartella. Trascinare un controllo GridView dalla pagina nella finestra di progettazione e set relativo `ID` a `UserGrid`. In un momento in cui si scriverà il codice che chiama il `Membership`.`GetAllUsers` metodo e associa il valore risultante `MembershipUserCollection` oggetto GridView. Il `MembershipUserCollection` contiene un `MembershipUser` oggetto per ogni account utente nel sistema. `MembershipUser` oggetti dispongono di proprietà come `UserName`,`Email`,`LastLoginDate` e così via.

Prima che si scrive il codice che associa gli account utente alla griglia, è pertanto possibile definire innanzitutto i campi del controllo GridView. Smart tag del controllo GridView, fare clic sul collegamento "Modifica colonne" per avviare la finestra di dialogo campi (vedere Figura 6). Da qui, deselezionare la casella di controllo "genera automaticamente i campi" nell'angolo inferiore sinistro. Poiché si desidera che questa GridView includono la modifica ed eliminazione di funzionalità, aggiungere un CommandField e impostare il relativo `ShowEditButton` e `ShowDeleteButton` proprietà su True. Successivamente, aggiungere quattro campi per la visualizzazione di `UserName`, `Email`, `LastLoginDate`, e `Comment` proprietà. Utilizzare un BoundField per le due proprietà di sola lettura (`UserName` e `LastLoginDate`) e TemplateFields per i due campi modificabili (`Email` e `Comment`).

La prima visualizzazione BoundField il `UserName` proprietà; impostare il relativo `HeaderText` e `DataField` proprietà su "UserName". Questo campo non sono modificabile, quindi impostare il relativo `ReadOnly` proprietà su True. Configurare il `LastLoginDate` BoundField impostando il relativo `HeaderText` ad "Accesso ultimo" e il relativo `DataField` per "LastLoginDate". Di seguito formattare l'output di questo BoundField in modo che solo la data viene visualizzata (anziché la data e ora). A tale scopo, impostare questo BoundField `HtmlEncode` la proprietà su False e il relativo `DataFormatString` proprietà su "{0: d}". Impostare inoltre la `ReadOnly` proprietà su True.

Impostare il `HeaderText` proprietà le due TemplateFields "Email" e "Comment".


[![I campi del controllo GridView possono essere configurati tramite la finestra di dialogo campi](role-based-authorization-vb/_static/image17.png)](role-based-authorization-vb/_static/image16.png)

**Figura 6**: campi possono essere configurate tramite campi finestra il GridView di dialogo ([fare clic per visualizzare l'immagine ingrandita](role-based-authorization-vb/_static/image18.png))


È ora necessario definire il `ItemTemplate` e `EditItemTemplate` per il "Email" e "Comment" TemplateFields. Aggiungere un controllo Web etichetta a ogni il `ItemTemplates` e associare i `Text` proprietà per il `Email` e `Comment` proprietà, rispettivamente.

Per il TemplateField "Email", aggiungere una casella di testo denominato `Email` per relativo `EditItemTemplate` e associare il `Text` proprietà per il `Email` proprietà tramite associazione dati bidirezionale. Aggiungere un RequiredFieldValidator e RegularExpressionValidator per il `EditItemTemplate` per garantire che un utente modifica la proprietà del messaggio di posta elettronica ha immesso un indirizzo di posta elettronica valido. Per il TemplateField "Comment", aggiungere una casella di testo multilinea denominato `Comment` al relativo `EditItemTemplate`. Impostare la casella di testo `Columns` e `Rows` proprietà 40 e 4, rispettivamente e quindi associare relativo `Text` proprietà per il `Comment` proprietà tramite associazione dati bidirezionale.

Dopo aver configurato questi TemplateFields, il markup dichiarativo dovrebbe essere simile al seguente:

[!code-aspx[Main](role-based-authorization-vb/samples/sample4.aspx)]

Durante la modifica o eliminazione di un account utente, è necessario conoscere tale utente `UserName` valore della proprietà. Impostare il controllo GridView `DataKeyNames` proprietà su "UserName" in modo che queste informazioni sono disponibili tramite il controllo GridView `DataKeys` insieme.

Infine, aggiungere un controllo ValidationSummary alla pagina e impostare il relativo `ShowMessageBox` proprietà su True e il relativo `ShowSummary` la proprietà su False. Con queste impostazioni, il controllo ValidationSummary visualizzerà un avviso sul lato client se l'utente tenta di modificare un account utente con un indirizzo di posta elettronica non valido o mancante.

[!code-aspx[Main](role-based-authorization-vb/samples/sample5.aspx)]

Markup dichiarativo della pagina è stata completata. L'attività successiva consiste nell'associare il set di account utente a GridView. Aggiungere un metodo denominato `BindUserGrid` per il `RoleBasedAuthorization.aspx` classe code-behind della pagina che associa il `MembershipUserCollection` restituito da `Membership.GetAllUsers` per il `UserGrid` GridView. Chiamare questo metodo dal `Page_Load` gestore dell'evento nella prima visita pagina.

[!code-vb[Main](role-based-authorization-vb/samples/sample6.vb)]

Con questo codice, visitare la pagina tramite un browser. Come illustrato nella figura 7, verrà visualizzato un controllo GridView Elenca informazioni su ogni account utente nel sistema.


[![UserGrid GridView Elenca le informazioni relative a ciascun utente nel sistema](role-based-authorization-vb/_static/image20.png)](role-based-authorization-vb/_static/image19.png)

**Figura 7**: il `UserGrid` GridView Elenca informazioni su ogni utente nel sistema ([fare clic per visualizzare l'immagine ingrandita](role-based-authorization-vb/_static/image21.png))


> [!NOTE]
> Il `UserGrid` GridView Elenca tutti gli utenti in un'interfaccia non di paging. Questa interfaccia griglia semplice non è adatta agli scenari in cui sono presenti diverse decine o più utenti. Un'opzione consiste nel configurare GridView per attivare il paging. Il `Membership.GetAllUsers` metodo dispone di due overload: uno che non accetta alcun parametro di input e restituisce tutti gli utenti e uno che accetta i valori integer per l'indice della pagina e le dimensioni di pagina e restituisce solo il sottoinsieme di utenti specificato. Il secondo overload può essere utilizzato per gli utenti in modo più efficiente paging perché restituisce solo il subset preciso di account utente anziché *tutti* di essi. Se si dispone di migliaia di account utente, è possibile considerare un'interfaccia basata su filtro, che vengono visualizzati solo gli utenti il cui nome utente inizia con un carattere selezionato, ad esempio. Il [ `Membership.FindUsersByName` metodo](https://msdn.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) è ideale per la creazione di un'interfaccia utente basata su filtro. Verranno esaminati la creazione di tale interfaccia in un'esercitazione future.


Il controllo GridView offre incorporati modifica e l'eliminazione di supporto quando il controllo è associato a un controllo origine dati configurato correttamente, ad esempio la SqlDataSource o ObjectDataSource. Il `UserGrid` GridView, tuttavia, presenta i dati associati a livello di codice; pertanto, è necessario scrivere codice per eseguire queste due attività. In particolare, è necessario creare gestori eventi per il controllo GridView `RowEditing`, `RowCancelingEdit`, `RowUpdating`, e `RowDeleting` eventi che vengono generati quando un visitatore fa clic su di GridView modifica, Annulla, aggiornamento, o eliminare i pulsanti.

Creare innanzitutto i gestori eventi per il controllo GridView `RowEditing`, `RowCancelingEdit`, e `RowUpdating` gli eventi e quindi aggiungere il codice seguente:

[!code-vb[Main](role-based-authorization-vb/samples/sample7.vb)]

Il `RowEditing` e `RowCancelingEdit` è sufficiente impostano gestori eventi del controllo GridView `EditIndex` proprietà e quindi rieseguire il binding l'elenco di utenti account alla griglia. Informazioni interessanti avviene nella `RowUpdating` gestore dell'evento. Questo gestore eventi viene avviato, verificare che i dati sia validi e quindi acquisisce il `UserName` valore dell'account utente modificato dal `DataKeys` insieme. Il `Email` e `Comment` nelle caselle di testo in 'TemplateFields due `EditItemTemplate` s vengono quindi a livello di codice a cui fa riferimento. I relativi `Text` proprietà contengono l'indirizzo di posta elettronica modificato e il commento.

Per aggiornare un account utente tramite l'API di appartenenza è necessario ottenere prima le informazioni dell'utente, come in questo caso tramite una chiamata a `Membership.GetUser(userName)`. L'oggetto restituito `MembershipUser` dell'oggetto `Email` e `Comment` proprietà vengono quindi aggiornate con i valori immessi nelle due caselle di testo dall'interfaccia di modifica. Infine, queste modifiche vengono salvate con una chiamata a [ `Membership.UpdateUser` ](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx). Il `RowUpdating` completa ripristino GridView alla relativa interfaccia pre-Modifica gestore dell'evento.

Successivamente, creare il `RowDeleting` RowDeleting gestore dell'evento e quindi aggiungere il codice seguente:

[!code-vb[Main](role-based-authorization-vb/samples/sample8.vb)]

Il gestore dell'evento precedente inizia con la cattura il `UserName` compreso il GridView `DataKeys` insieme; questo `UserName` valore viene quindi passato alla classe di appartenenza [ `DeleteUser` metodo](https://msdn.microsoft.com/library/system.web.security.membership.deleteuser.aspx). Il `DeleteUser` metodo elimina l'account utente dal sistema, inclusi i dati di appartenenza correlati (ad esempio quali ruoli appartiene l'utente). Dopo l'eliminazione, la griglia dell'utente `EditIndex` è impostato su -1 (nel caso in cui l'utente fa clic su Elimina, mentre un'altra riga era in modalità di modifica) e `BindUserGrid` metodo viene chiamato.

> [!NOTE]
> Pulsante Elimina non richiede una sorta di conferma da parte dell'utente prima di eliminare l'account utente. Ti suggeriamo di aggiungere una forma di conferma dell'utente per diminuire le probabilità che un account viene eliminato per errore. Uno dei modi più semplici per confermare un'azione avviene tramite una finestra di dialogo di conferma sul lato client. Per ulteriori informazioni su questa tecnica, vedere [aggiunta sul lato Client conferma quando l'eliminazione di](https://asp.net/learn/data-access/tutorial-42-vb.aspx).


Verificare che questa pagina funzioni come previsto. Dovrebbe essere possibile modificare l'indirizzo di posta elettronica di qualsiasi utente e un commento, nonché eliminare qualsiasi account utente. Poiché il `RoleBasedAuthorization.aspx` pagina è accessibile a tutti gli utenti, tutti gli utenti: anonimi anche visitatori – possono visitare questa pagina e modificare ed eliminare account utente. Questa pagina si aggiorna in modo che solo gli utenti nei ruoli supervisori e gli amministratori possono modificare indirizzo di posta elettronica dell'utente e un commento e solo gli amministratori possono eliminare un account utente.

La sezione "Utilizzo del controllo LoginView" esamina l'utilizzo del controllo LoginView per visualizzare istruzioni specifiche per il ruolo dell'utente. Se una persona al ruolo Administrators visita questa pagina, verranno illustrati le istruzioni su come modificare ed eliminare gli utenti. Se un utente nel ruolo supervisori raggiunge questa pagina, sono disponibili istruzioni sulla modifica degli utenti. E se il visitatore è anonimo o non è nel ruolo di amministratori o i supervisori, verrà visualizzato un messaggio che indica che è possibile modificare o eliminare informazioni sull'account utente. Nella sezione "Limitazione a livello di codice la funzionalità" si scriverà il codice che a livello di codice Mostra o nasconde i pulsanti Modifica e l'eliminazione in base al ruolo dell'utente.

### <a name="using-the-loginview-control"></a>Utilizzare il controllo LoginView

Come abbiamo visto nelle esercitazioni precedenti, il controllo LoginView è utile per la visualizzazione di interfacce diverse per gli utenti autenticati e anonimi, ma il controllo LoginView può anche essere consentono di visualizzare codice diverso in base ai ruoli dell'utente. Utilizziamo un controllo LoginView per visualizzare istruzioni diverse in base al ruolo dell'utente.

Inizio aggiungendo un LoginView sopra il `UserGrid` GridView. Come descritto in precedenza, il controllo LoginView dispone di due modelli predefiniti: `AnonymousTemplate` e `LoggedInTemplate`. Immettere un messaggio breve in entrambi i modelli che informa l'utente che è possibile modificare o eliminare le informazioni sull'utente.

[!code-aspx[Main](role-based-authorization-vb/samples/sample9.aspx)]

Oltre al `AnonymousTemplate` e `LoggedInTemplate`, può includere il controllo LoginView *RoleGroups*, che sono modelli specifici del ruolo. Ogni RoleGroup contiene una singola proprietà, `Roles`, che consente di specificare i ruoli di RoleGroup si applica a. Il `Roles` proprietà può essere impostata per un singolo ruolo (ad esempio "Administrators") o per un elenco delimitato da virgole dei ruoli (ad esempio "Administrators", mentre per i supervisori).

Per gestire il RoleGroups, fare clic sul collegamento "Modifica RoleGroups" del controllo Smart tag per visualizzare Editor raccolta di RoleGroup. Aggiungere due RoleGroups di nuovo. Impostare il primo RoleGroup `Roles` proprietà su "Administrators" e la seconda ai "Supervisori".


[![Gestire i modelli specifici per il ruolo di LoginView tramite l'Editor della raccolta RoleGroup](role-based-authorization-vb/_static/image23.png)](role-based-authorization-vb/_static/image22.png)

**Figura 8**: la gestione specifiche del ruolo modelli tramite l'Editor del LoginView della raccolta RoleGroup ([fare clic per visualizzare l'immagine ingrandita](role-based-authorization-vb/_static/image24.png))


Fare clic su OK per chiudere l'Editor della raccolta RoleGroup; Aggiorna dichiarativo di LoginView per includere un `<RoleGroups>` sezione con un `<asp:RoleGroup>` nell'Editor della raccolta RoleGroup definito l'elemento figlio per ogni RoleGroup. Inoltre, l'elenco a discesa "Visualizzazioni" elenco Smart Tag di LoginView - elencate inizialmente solo il `AnonymousTemplate` e `LoggedInTemplate` – include ora anche la RoleGroups aggiunto.

Modifica il RoleGroups in modo che gli utenti del ruolo supervisori vengono visualizzate le istruzioni su come modificare gli account utente, mentre gli utenti del ruolo amministratori vengono visualizzati le istruzioni per la modifica ed eliminazione. Dopo aver apportato queste modifiche, markup dichiarativo di LoginView dovrebbe essere simile al seguente.

[!code-aspx[Main](role-based-authorization-vb/samples/sample10.aspx)]

Dopo aver apportato queste modifiche, salvare la pagina e quindi vi accede tramite un browser. Innanzitutto, visitare la pagina come utente anonimo. Si deve essere visualizzato il messaggio, "non si è connessi al sistema. Pertanto è possibile modificare o eliminare le informazioni sull'utente." Accedere quindi come un utente autenticato, ma che non è nel ruolo supervisori né gli amministratori. Questa volta verrà visualizzato il messaggio "non è un membro dei ruoli i supervisori o gli amministratori. Pertanto è possibile modificare o eliminare le informazioni sull'utente."

Successivamente, accedere come utente membro del ruolo supervisori. Questo tempo dovrebbe essere supervisori specifiche del ruolo del messaggio (vedere Figura 9). E se si accede come utente nel ruolo dovrebbe essere specifici del ruolo amministratori di messaggio (vedere la figura 10) amministratori.


[![Bruce viene visualizzato il messaggio di specifiche del ruolo mentre per i supervisori](role-based-authorization-vb/_static/image26.png)](role-based-authorization-vb/_static/image25.png)

**Figura 9**: Bruce viene visualizzato il messaggio di specifiche del ruolo mentre per i supervisori ([fare clic per visualizzare l'immagine ingrandita](role-based-authorization-vb/_static/image27.png))


[![Tito viene visualizzato il messaggio specifici per il ruolo di amministratori](role-based-authorization-vb/_static/image29.png)](role-based-authorization-vb/_static/image28.png)

**Figura 10**: Tito viene visualizzato il messaggio specifici per il ruolo di amministratori ([fare clic per visualizzare l'immagine ingrandita](role-based-authorization-vb/_static/image30.png))


Come nelle schermate figure 9 e 10 Mostra, di LoginView solo esegue il rendering di un modello, anche se si applicano più modelli. Bruce e Tito sono entrambi gli utenti connessi, ma il LoginView esegue il rendering solo il RoleGroup corrispondente e non il `LoggedInTemplate`. Inoltre, Tito appartiene ai ruoli di amministratori e i supervisori, ma il controllo LoginView visualizza il modello di specifiche del ruolo amministratori anziché i supervisori uno.

La figura 11 illustra il flusso di lavoro utilizzata dal controllo LoginView per determinare quale modello per eseguire il rendering. Si noti che se è presente più di uno RoleGroup specificato, il modello di LoginView esegue il rendering di *prima* RoleGroup corrispondente. In altre parole, se è stato inserito RoleGroup i supervisori come il primo RoleGroup e gli amministratori di come il secondo, quando Tito visita questa pagina ha verrà visualizzati il messaggio supervisori.


[![Flusso di lavoro del controllo LoginView per determinare quale modello per il rendering](role-based-authorization-vb/_static/image32.png)](role-based-authorization-vb/_static/image31.png)

**Figura 11**: flusso di lavoro del controllo LoginView il per determinare cosa modello per il rendering ([fare clic per visualizzare l'immagine ingrandita](role-based-authorization-vb/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Funzionalità di limitazione a livello di codice

Mentre il controllo LoginView Visualizza istruzioni diverse in base al ruolo dell'utente visita la pagina, restano visibili a tutti i pulsanti Modifica e Annulla. È necessario nascondere a livello di codice i pulsanti Modifica e l'eliminazione per i visitatori anonimi e utenti inclusi nel ruolo supervisori né gli amministratori. È necessario nascondere il pulsante Elimina per tutti gli utenti che non è un amministratore. Per questo scopo viene illustrato come scrivere codice che fa riferimento a livello di codice modifica la CommandField e LinkButton eliminare e imposta i `Visible` proprietà `False`, se necessario.

Il modo più semplice per fare riferimento a livello di programmazione di controlli in un CommandField consiste innanzitutto convertirlo in un modello. A tale scopo, fare clic sul collegamento "Modifica colonne" Smart tag del controllo GridView, selezionare il CommandField dall'elenco di campi correnti e fare clic sul collegamento "Converti il campo in un TemplateField". Questo consente di trasformare il CommandField in un TemplateField con un `ItemTemplate` e `EditItemTemplate`. Il `ItemTemplate` contiene la modifica e LinkButton eliminare durante la `EditItemTemplate` ospita l'aggiornamento e annullare LinkButton.


[![Convertire il CommandField in un TemplateField](role-based-authorization-vb/_static/image35.png)](role-based-authorization-vb/_static/image34.png)

**Figura 12**: convertire CommandField in un TemplateField ([fare clic per visualizzare l'immagine ingrandita](role-based-authorization-vb/_static/image36.png))


Aggiornare la modifica ed eliminare LinkButton nel `ItemTemplate`, impostando i relativi `ID` proprietà con valori di `EditButton` e `DeleteButton`, rispettivamente.

[!code-aspx[Main](role-based-authorization-vb/samples/sample11.aspx)]

Ogni volta che i dati sono associati a GridView, GridView enumera i record relativi `DataSource` proprietà e genera un corrispondente `GridViewRow` oggetto. Come ogni `GridViewRow` viene creato l'oggetto, il `RowCreated` viene generato l'evento. Per nascondere i pulsanti Modifica e l'eliminazione di utenti non autorizzati, è necessario creare un gestore eventi per questo evento e a livello di codice fa riferimento a eliminare LinkButton, impostazione e modifica i `Visible` proprietà conseguenza.

Creare un gestore eventi di `RowCreated` evento e quindi aggiungere il codice seguente:

[!code-vb[Main](role-based-authorization-vb/samples/sample12.vb)]

Tenere presente che il `RowCreated` evento viene generato per *tutti* righe GridView, incluse l'intestazione, il piè di pagina, l'interfaccia di cercapersone e così via. Vogliamo solo a livello di codice fa riferimento a modifica e LinkButton eliminare se ci stiamo occupa una riga di dati non in modalità di modifica (poiché la riga in modalità di modifica contiene pulsanti Aggiorna e Annulla anziché Edit e Delete). Questo controllo viene gestito dal `If` istruzione.

Se si sta occupa una riga di dati che non è in modalità di modifica, la modifica e l'eliminazione LinkButton viene fatto riferimento e i relativi `Visible` proprietà vengono impostate in base ai valori Boolean restituito dal `User` dell'oggetto `IsInRole(roleName)` metodo. Il `User` oggetto fa riferimento l'entità creata per il `RoleManagerModule`; di conseguenza, il `IsInRole(roleName)` metodo Usa l'API di ruoli per determinare se l'utente corrente appartiene a *roleName*.

> [!NOTE]
> Avremmo potuto utilizzare la classe di ruoli direttamente, sostituendo la chiamata a `User.IsInRole(roleName)` con una chiamata al [ `Roles.IsUserInRole(roleName)` metodo](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx). Ho deciso di utilizzare l'oggetto principal `IsInRole(roleName)` metodo in questo esempio perché è più efficiente rispetto all'utilizzo direttamente l'API di ruoli. In precedenza in questa esercitazione è stato configurato la gestione dei ruoli per memorizzare nella cache i ruoli dell'utente in un cookie. Questo cookie dati memorizzati nella cache viene utilizzata soltanto quando l'entità `IsInRole(roleName)` metodo viene chiamato; chiamate dirette all'API di ruoli comportano sempre un trip all'archivio di ruolo. Anche se in un cookie non vengono memorizzati i ruoli, chiamare l'oggetto principal `IsInRole(roleName)` metodo è in genere più efficiente perché quando viene chiamato per la prima volta durante una richiesta viene memorizzato nella cache i risultati. L'API di ruoli, d'altra parte, non esegue qualsiasi la memorizzazione nella cache. Poiché il `RowCreated` evento viene generato una volta per ogni riga in GridView, utilizzando `User.IsInRole(roleName)` comporta solo un unico round trip all'archivio di ruolo, mentre `Roles.IsUserInRole(roleName)` richiede *N* trip, in cui *N* è il numero di account utente visualizzati nella griglia.


Pulsante Modifica `Visible` è impostata su `True` se l'utente visita questa pagina è nel ruolo amministratori o i supervisori; in caso contrario è impostata su `False`. Pulsante Elimina `Visible` è impostata su `True` solo se l'utente al ruolo Administrators.

Questa pagina tramite un browser di prova. Se si visita la pagina come un visitatore anonimo o come un utente che non è un supervisore né un amministratore, il CommandField è vuoto. ancora presente, ma come un frammento thin senza modifica o Elimina i pulsanti.

> [!NOTE]
> È possibile nascondere il CommandField completamente quando un non-supervisore e senza privilegi di amministratore visita la pagina. Lasciare questo campo come esercizio per il lettore.


[![La modifica e i pulsanti Elimina sono nascosti per i supervisori Non e di utenti Non amministratori](role-based-authorization-vb/_static/image38.png)](role-based-authorization-vb/_static/image37.png)

**Figura 13**: pulsanti Elimina e modifica sono nascosti per i supervisori Non e gli altri utenti ([fare clic per visualizzare l'immagine ingrandita](role-based-authorization-vb/_static/image39.png))


Se un utente che appartiene al ruolo supervisori (ma non per il ruolo di amministratore) visita, vede solo il pulsante Modifica.


[![Anche se il pulsante Modifica è disponibile per i supervisori, il pulsante Elimina è nascosto](role-based-authorization-vb/_static/image41.png)](role-based-authorization-vb/_static/image40.png)

**Figura 14**: mentre il pulsante Modifica è disponibile per i supervisori, il pulsante Elimina è nascosto ([fare clic per visualizzare l'immagine ingrandita](role-based-authorization-vb/_static/image42.png))


E, se un amministratore visita, chiara ha accesso a entrambi i pulsanti Modifica e l'eliminazione.


[![La modifica e i pulsanti Elimina sono disponibili solo per gli amministratori](role-based-authorization-vb/_static/image44.png)](role-based-authorization-vb/_static/image43.png)

**Figura 15**: pulsanti Elimina e modifica sono disponibili solo per gli amministratori ([fare clic per visualizzare l'immagine ingrandita](role-based-authorization-vb/_static/image45.png))


## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>Passaggio 3: Applicazione di regole di autorizzazione basata sui ruoli a classi e metodi

Nel passaggio 2 che è stato ridotto modificare funzionalità agli utenti nei ruoli supervisori e gli amministratori ed eliminare solo le funzionalità agli amministratori. A tale scopo come nascondere gli elementi dell'interfaccia utente associato per gli utenti non autorizzati tramite tecniche a livello di codice. Tali misure non garantiscono che un utente non autorizzato in grado di eseguire un'azione con privilegi. Possono esistere elementi dell'interfaccia utente che vengono aggiunti in un secondo momento o che è stato dimenticato di nascondere per gli utenti non autorizzati. O un pirata informatico potrebbe individuare un altro modo per ottenere la pagina ASP.NET per eseguire il metodo desiderato.

È un modo semplice per garantire che una particolare parte della funzionalità non è possibile accedere da un utente non autorizzato per decorare quella classe o metodo con il [ `PrincipalPermission` attributo](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Quando il runtime .NET viene utilizzata una classe o esegue uno dei relativi metodi, verifica per assicurarsi che il contesto di sicurezza corrente dispone dell'autorizzazione. Il `PrincipalPermission` attributo fornisce un meccanismo tramite il quale è possibile definire queste regole.

Abbiamo esaminato utilizzando il `PrincipalPermission` attributo nuovamente il <a id="_msoanchor_9"> </a> [ *autorizzazione basata sull'utente* ](../membership/user-based-authorization-vb.md) esercitazione. In particolare, è stato illustrato come decorare il GridView `SelectedIndexChanged` e `RowDeleting` gestore dell'evento in modo che si può essere eseguiti solo da utenti autenticati e Tito, rispettivamente. Il `PrincipalPermission` attributo funziona anche con i ruoli.

Di seguito viene illustrato l'utilizzo di `PrincipalPermission` attributo del controllo GridView `RowUpdating` e `RowDeleting` gestori eventi per impedire l'esecuzione per gli utenti non autorizzati. Tutto è necessario fare è aggiungere l'attributo appropriato nella parte superiore di ogni definizione di funzione:

[!code-vb[Main](role-based-authorization-vb/samples/sample13.vb)]

L'attributo per il `RowUpdating` gestore dell'evento indica che solo gli utenti nei ruoli amministratori o i supervisori possono eseguire il gestore dell'evento, mentre l'attributo di `RowDeleting` gestore eventi consente di limitare l'esecuzione per gli utenti amministratori ruolo.


> [!NOTE]
> Il `PrincipalPermission` attributo è rappresentato come una classe di `System.Security.Permissions` dello spazio dei nomi. Assicurarsi di aggiungere un `Imports System.Security.Permissions` istruzione all'inizio del file di classe di codice per importare questo spazio dei nomi.


Se, in qualche modo, un utente non amministratore tenta di eseguire il `RowDeleting` gestore eventi o se un tentativi non supervisore o senza privilegi di amministratore per eseguire il `RowUpdating` gestore dell'evento, il runtime .NET genererà un `SecurityException`.


[![Se il contesto di sicurezza non è autorizzato a eseguire il metodo, viene generata un'eccezione SecurityException](role-based-authorization-vb/_static/image47.png)](role-based-authorization-vb/_static/image46.png)

**Figura 16**: se il contesto di sicurezza non è autorizzato a eseguire il metodo, una `SecurityException` viene generata un'eccezione ([fare clic per visualizzare l'immagine ingrandita](role-based-authorization-vb/_static/image48.png))


Oltre alle pagine ASP.NET, molte applicazioni hanno inoltre un'architettura che include vari livelli, ad esempio la logica di Business e i livelli di accesso ai dati. Questi livelli vengono in genere implementati come librerie di classi e offrono le classi e metodi per l'esecuzione delle funzionalità relative ai dati e logica di business. Il `PrincipalPermission` attributo è utile per applicare le regole di autorizzazione anche questi livelli.

Per ulteriori informazioni sull'utilizzo di `PrincipalPermission` attributo per definire le regole di autorizzazione per le classi e metodi, fare riferimento a [Scott Guthrie](https://weblogs.asp.net/scottgu/)del post di blog [aggiungendo le regole di autorizzazione Business e i livelli di dati mediante `PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Riepilogo

In questa esercitazione illustra come specificare grossolana e le regole di autorizzazione grana basano sui ruoli dell'utente. ASP. La funzionalità di autorizzazione URL di NET consente a uno sviluppatore di pagina specificare quali identità consentito o negato l'accesso alle pagine. Come abbiamo visto nuovamente il <a id="_msoanchor_10"> </a> [ *autorizzazione basata sull'utente* ](../membership/user-based-authorization-vb.md) tutorial, autorizzazione URL regole possono essere applicate su base utente dall'utente. Sono inoltre applicabili in base a ruolo per ruolo, come illustrato nel passaggio 1 di questa esercitazione.

Le regole di autorizzazione grana possono essere applicate in modo dichiarativo o a livello di codice. Nel passaggio 2 è stato esaminato utilizzando funzionalità RoleGroups del controllo LoginView per il rendering dell'output diversi in base ai ruoli dell'utente. È stato esaminato anche metodi per determinare a livello di programmazione se un utente appartiene a un ruolo specifico e come modificare la funzionalità della pagina in modo appropriato.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Aggiunta di regole di autorizzazione di Business e i livelli di dati tramite `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Analisi di ASP.NET 2.0 appartenenza, ruoli e profilo: utilizzo dei ruoli](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [Elenco di domande di sicurezza per ASP.NET 2.0](https://msdn.microsoft.com/library/ms998375.aspx)
- [Documentazione tecnica per il `<roleManager>` elemento](https://msdn.microsoft.com/library/ms164660.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è  *[SAM insegna manualmente ASP.NET 2.0 nelle 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott può essere raggiunto al [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali...

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione includono Suchi Banerjee e Teresa Murphy. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](assigning-roles-to-users-vb.md)
