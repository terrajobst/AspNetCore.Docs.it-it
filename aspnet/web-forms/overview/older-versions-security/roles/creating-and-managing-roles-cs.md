---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
title: Creazione e gestione dei ruoli (c#) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione vengono esaminati i passaggi necessari per configurare il framework di ruoli. Successivamente, verrà compilata pagine web per creare ed eliminare ruoli.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 113f10b3-a19a-471b-8ff6-db3c79ce8a91
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
msc.type: authoredcontent
ms.openlocfilehash: a4ea7e76e023cd436d1d8ac52307a3ac17267fef
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2018
ms.locfileid: "30891679"
---
<a name="creating-and-managing-roles-c"></a>Creazione e gestione dei ruoli (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.09.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_cs.pdf)

> In questa esercitazione vengono esaminati i passaggi necessari per configurare il framework di ruoli. Successivamente, verrà compilata pagine web per creare ed eliminare ruoli.


## <a name="introduction"></a>Introduzione

Nel <a id="_msoanchor_1"> </a> [ *autorizzazione basata sull'utente* ](../membership/user-based-authorization-cs.md) esercitazione viene descritto l'utilizzo di autorizzazione URL per impedire un set di pagine di determinati utenti ed esplorato dichiarativa e tecniche a livello di codice per la regolazione delle funzionalità della pagina ASP.NET in base all'utente di accesso. Concessione dell'autorizzazione per l'accesso alla pagina o la funzionalità su base utente dall'utente, tuttavia, può diventare un incubo negli scenari in cui sono presenti più account utente o quando vengono modificati spesso privilegi degli utenti. Ogni volta che un utente acquisisce o perde l'autorizzazione per eseguire una determinata attività, l'amministratore deve aggiornare le regole di autorizzazione URL appropriate, markup dichiarativo e codice.

In genere consente di classificare gli utenti in gruppi o *ruoli* e quindi applicare le autorizzazioni in base al ruolo per ruolo. Ad esempio, la maggior parte delle applicazioni web dispongono di un determinato set di pagine o attività che sono riservate solo per gli utenti amministratori. Utilizzando le tecniche descritte nel *autorizzazione basata sull'utente* esercitazione, verrà aggiunto codice per consentire gli account utente specificato eseguire attività amministrative, regole di autorizzazione URL appropriate e markup dichiarativo. Ma se è stato aggiunto un nuovo amministratore o un amministratore esistente è necessario disporre di diritti di amministrazione del proprio revocati, è necessario restituire e aggiornare il file di configurazione e le pagine web. Con i ruoli, tuttavia, è possibile creare un ruolo di amministratori e assegnare gli utenti attendibili per il ruolo di amministratore. Successivamente, verrà aggiunto codice per consentire al ruolo di amministratori per eseguire le varie attività amministrative, regole di autorizzazione URL appropriate e markup dichiarativo. Con questa infrastruttura, è semplice come includendo o la rimozione dell'utente dal ruolo degli amministratori di aggiungere nuovi amministratori al sito o la rimozione di quelli esistenti. Nessuna configurazione, markup dichiarativo o modifiche al codice sono necessari.

ASP.NET offre un framework di ruoli per definire i ruoli e associarli agli account utente. Con il framework di ruoli, è possibile creare ed eliminare ruoli, aggiungere o rimuovere utenti da un ruolo, determinare il set di utenti che appartengono a un ruolo specifico e indicare se un utente appartiene a un ruolo specifico. Una volta configurato il framework di ruoli, è possibile limitare l'accesso alle pagine in base ruolo per ruolo tramite regole di autorizzazione URL e mostrare o nascondere informazioni aggiuntive o funzionalità in una pagina in base ai ruoli dell'utente attualmente connesso.

In questa esercitazione vengono esaminati i passaggi necessari per configurare il framework di ruoli. Successivamente, verrà compilata pagine web per creare ed eliminare ruoli. Nel <a id="_msoanchor_2"> </a> [ *l'assegnazione di ruoli agli utenti* ](assigning-roles-to-users-cs.md) esercitazione verranno esaminati come aggiungere e rimuovere utenti dai ruoli. E il <a id="_msoanchor_3"> </a> [ *autorizzazione basata sui ruoli* ](role-based-authorization-cs.md) esercitazione verrà spiegato come limitare l'accesso alle pagine con cadenza ruolo per ruolo insieme come regolare a seconda delle funzionalità pagina ruolo dell'utente. Iniziamo!

## <a name="step-1-adding-new-aspnet-pages"></a>Passaggio 1: Aggiunta di nuove pagine ASP.NET

In questa esercitazione e i due successivi è verrà esaminando le funzioni correlate a ruoli e le funzionalità. È necessario una serie di pagine ASP.NET per implementare gli argomenti esaminati in queste esercitazioni. Di seguito creare queste pagine e aggiornare la mappa del sito.

Iniziare creando una nuova cartella nel progetto denominato `Roles`. Successivamente, aggiungere quattro nuove pagine ASP.NET per il `Roles` cartella, il collegamento di ogni pagina con il `Site.master` pagina master. Le pagine dei nomi:

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

Esplora soluzioni del progetto a questo punto dovrebbe essere simile alla schermata illustrata nella figura 1.


[![Quattro nuove pagine sono stati aggiunti alla cartella dei ruoli](creating-and-managing-roles-cs/_static/image2.png)](creating-and-managing-roles-cs/_static/image1.png)

**Figura 1**: quattro nuove pagine sono stati aggiunti per il `Roles` cartella ([fare clic per visualizzare l'immagine ingrandita](creating-and-managing-roles-cs/_static/image3.png))


Ogni pagina, dovrebbe rispondere a questo punto, includere i due controlli contenuti, uno per ognuno dei ContentPlaceHolder della pagina master: `MainContent` e `LoginContent`.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample1.aspx)]

Tenere presente che il `LoginContent` markup predefinito del ContentPlaceHolder viene visualizzato un collegamento per connettersi o disconnettersi del sito, a seconda che l'utente è autenticato. La presenza del `Content2` contenuto controllo nella pagina ASP.NET, tuttavia, esegue l'override di markup predefinito della pagina master. Come accennato <a id="_msoanchor_4"> </a> [ *una panoramica dell'autenticazione basata su form* ](../introduction/an-overview-of-forms-authentication-cs.md) esercitazione, si esegue l'override il markup predefinito è utile nelle pagine in cui si preferisce non visualizzare relative all'accesso opzioni nella colonna sinistra.

Per questi quattro pagine, tuttavia, si desidera mostrare il markup della pagina master predefinita per il `LoginContent` ContentPlaceHolder. Pertanto, rimuovere il markup dichiarativo per la `Content2` controllo contenuto. Al termine dell'operazione, ognuno dei markup della pagina quattro deve contenere solo un controllo contenuto.

Infine, si aggiorna la mappa del sito (`Web.sitemap`) per includere le nuove pagine web. Aggiungere il seguente codice XML dopo la `<siteMapNode>` abbiamo aggiunto per le esercitazioni di appartenenza.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample2.xml)]

Con la mappa del sito è stata aggiornata, visitare il sito tramite un browser. Come illustrato nella figura 2, la navigazione a sinistra ora include gli elementi per le esercitazioni di ruoli.


[![Quattro nuove pagine sono stati aggiunti alla cartella dei ruoli](creating-and-managing-roles-cs/_static/image5.png)](creating-and-managing-roles-cs/_static/image4.png)

**Figura 2**: quattro nuove pagine sono stati aggiunti per il `Roles` cartella ([fare clic per visualizzare l'immagine ingrandita](creating-and-managing-roles-cs/_static/image6.png))


## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>Passaggio 2: Impostazione e sulla configurazione del Provider di ruoli Framework

Ad esempio il framework di appartenenza, il framework di ruoli viene compilato nella parte superiore del modello di provider. Come descritto nel <a id="_msoanchor_5"> </a> [ *nozioni fondamentali sulla sicurezza e il supporto ASP.NET* ](../introduction/security-basics-and-asp-net-support-cs.md) esercitazione, .NET Framework include tre provider di ruoli predefiniti: [ `AuthorizationStoreRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) , [ `WindowsTokenRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx), e [ `SqlRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx). Questa serie di esercitazioni si concentra sul `SqlRoleProvider`, che utilizza un database di Microsoft SQL Server come archivio ruoli.

Nel sistema il framework di ruoli e `SqlRoleProvider` funzionano esattamente come il framework di appartenenza e `SqlMembershipProvider`. .NET Framework contiene un `Roles` classe utilizzata come l'API del Framework di ruoli. Il `Roles` classe dispone di metodi statici, ad esempio `CreateRole`, `DeleteRole`, `GetAllRoles`, `AddUserToRole`, `IsUserInRole`e così via. Quando uno di questi metodi viene richiamato, il `Roles` classe delega la chiamata al provider configurato. Il `SqlRoleProvider` funziona con le tabelle specifiche del ruolo (`aspnet_Roles` e `aspnet_UsersInRoles`) nella risposta.

Per utilizzare il `SqlRoleProvider` provider nell'applicazione, è necessario specificare quali database utilizzare come archivio. Il `SqlRoleProvider` prevede l'archivio di ruolo specificato per alcune tabelle di database, viste e stored procedure. Questi oggetti di database necessari possono essere aggiunti mediante la [ `aspnet_regsql.exe` strumento](https://msdn.microsoft.com/library/ms229862.aspx). A questo punto si dispone già di un database con lo schema necessario per il `SqlRoleProvider`. Nel <a id="_msoanchor_6"> </a> [ *creazione dello Schema di appartenenza in SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) esercitazione è stato creato un database denominato `SecurityTutorials.mdf` e utilizzato `aspnet_regsql.exe` per aggiungere l'applicazione servizi, inclusi gli oggetti di database necessari per il `SqlRoleProvider`. Pertanto è necessario indicare al framework di ruoli per abilitare il supporto del ruolo e utilizzare il `SqlRoleProvider` con il `SecurityTutorials.mdf` database come archivio di ruolo.

Il framework di ruoli viene configurato tramite il &lt; `roleManager` &gt; elemento dell'applicazione `Web.config` file. Per impostazione predefinita, il supporto di ruolo è disabilitato. Per abilitarlo, è necessario impostare il [ &lt; `roleManager` &gt; ](https://msdn.microsoft.com/library/ms164660.aspx) dell'elemento `enabled` attributo `true` come illustrato di seguito:

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample3.xml)]

Per impostazione predefinita, tutte le applicazioni web dispongono di un provider di ruoli denominato `AspNetSqlRoleProvider` di tipo `SqlRoleProvider`. Il provider predefinito è registrato `machine.config` (disponibile all'indirizzo `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample4.xml)]

Il provider `connectionStringName` attributo specifica l'archivio di ruolo che viene utilizzato. Il `AspNetSqlRoleProvider` provider imposta questo attributo su `LocalSqlServer`, anch ' esso definito `machine.config` punti e, per impostazione predefinita, a un database di SQL Server 2005 Express Edition nel `App_Data` cartella denominata `aspnet.mdf`.

Di conseguenza, se si abilita semplicemente il framework di ruoli senza specificare le informazioni del provider dell'applicazione `Web.config` file, l'applicazione utilizza il provider di ruoli predefinito registrato, `AspNetSqlRoleProvider`. Se il `~/App_Data/aspnet.mdf` database non esiste, il runtime di ASP.NET verrà automaticamente creato e aggiungere lo schema di servizi di applicazione. Tuttavia, non si desidera utilizzare il `aspnet.mdf` database; invece, si desidera utilizzare il `SecurityTutorials.mdf` database che abbiamo già creato e aggiunto lo schema di servizi delle applicazioni per. Questa modifica può essere eseguita in uno dei due modi:

- <strong>Specificare un valore per il</strong><strong>`LocalSqlServer`</strong><strong>nome di stringa di connessione in</strong><strong>`Web.config`</strong><strong>.</strong> Sovrascrivendo il `LocalSqlServer` valore di nome di stringa di connessione in `Web.config`, è possibile utilizzare il provider di ruoli predefinito registrato (`AspNetSqlRoleProvider`) e fare in modo funzionare correttamente con il `SecurityTutorials.mdf` database. Per ulteriori informazioni su questa tecnica, vedere [Scott Guthrie](https://weblogs.asp.net/scottgu/)del post di blog, [la configurazione di servizi applicativi di ASP.NET 2.0 per utilizzare SQL Server 2000 o SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Aggiungere un nuovo provider di tipo registrato</strong><strong>`SqlRoleProvider`</strong><strong>e configurare il relativo</strong><strong>`connectionStringName`</strong><strong>impostazione in modo che punti al</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>database.</strong> Si tratta dell'approccio è consigliato e utilizzata per la <a id="_msoanchor_7"> </a> [ *creazione dello Schema di appartenenza in SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) esercitazione ed è l'approccio verrà utilizzato in questa esercitazione anche.

Aggiungere il seguente markup per la configurazione dei ruoli per il `Web.config` file. In questo markup registra un nuovo provider denominato `SecurityTutorialsSqlRoleProvider`.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample5.xml)]

Definisce il codice precedente il `SecurityTutorialsSqlRoleProvider` come provider predefinito (tramite il `defaultProvider` attributo il `<roleManager>` elemento). Imposta inoltre il `SecurityTutorialsSqlRoleProvider`del `applicationName` impostando su `SecurityTutorials`, ovvero lo stesso `applicationName` impostazione utilizzata dal provider di appartenenze (`SecurityTutorialsSqlMembershipProvider`). Mentre non riportati di seguito, il [ `<add>` elemento](https://msdn.microsoft.com/library/ms164662.aspx) per il `SqlRoleProvider` può inoltre contenere un `commandTimeout` attributo per specificare la durata del timeout del database, in secondi. Il valore predefinito è 30.

Con questo codice di configurazione sul posto, ci sono pronti per iniziare a usare la funzionalità di ruolo all'interno dell'applicazione.

> [!NOTE]
> Il tag di configurazione precedente viene illustrato l'utilizzo di &lt; `roleManager` &gt; dell'elemento `enabled` e `defaultProvider` gli attributi. Esistono numerosi altri attributi che influiscono sul modo in cui il framework di ruoli associa le informazioni sui ruoli su base utente dall'utente. Verranno esaminate queste impostazioni nella finestra di <a id="_msoanchor_8"> </a> [ *autorizzazione basata sui ruoli* ](role-based-authorization-cs.md) esercitazione.


## <a name="step-3-examining-the-roles-api"></a>Passaggio 3: Esaminare i ruoli API

Funzionalità del framework ruoli viene esposta tramite il [ `Roles` classe](https://msdn.microsoft.com/library/system.web.security.roles.aspx), che contiene metodi statici tredici per l'esecuzione di operazioni basato sui ruoli. Quando si esamina creazione ed eliminazione di ruoli in passaggi 4 e 6 verrà usata la [ `CreateRole` ](https://msdn.microsoft.com/library/system.web.security.roles.createrole.aspx) e [ `DeleteRole` ](https://msdn.microsoft.com/library/system.web.security.roles.deleterole.aspx) i metodi, aggiungere o rimuovere un ruolo dal sistema.

Per ottenere un elenco di tutti i ruoli del sistema, utilizzare il [ `GetAllRoles` metodo](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx) (vedere il passaggio 5). Il [ `RoleExists` metodo](https://msdn.microsoft.com/library/system.web.security.roles.roleexists.aspx) restituisce un valore booleano che indica se un ruolo specificato esiste.

Nella prossima esercitazione verrà esaminato associare gli utenti a ruoli. Il `Roles` della classe [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx), [ `AddUserToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertoroles.aspx), [ `AddUsersToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstorole.aspx), e [ `AddUsersToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstoroles.aspx) metodi consentono di aggiungere uno o più utenti a uno o più ruoli. Per rimuovere utenti dai ruoli, utilizzare il [ `RemoveUserFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx), [ `RemoveUserFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromroles.aspx), [ `RemoveUsersFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromrole.aspx), o [ `RemoveUsersFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromroles.aspx) metodi.

Nel <a id="_msoanchor_9"> </a> [ *autorizzazione basata sui ruoli* ](role-based-authorization-cs.md) esercitazione verranno esaminati modi per livello di codice Mostra o nasconde la funzionalità in base al ruolo dell'utente attualmente connesso. A tale scopo, è possibile utilizzare il `Role` della classe [ `FindUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.findusersinrole.aspx), [ `GetRolesForUser` ](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx), [ `GetUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx), o [ `IsUserInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx) metodi.

> [!NOTE]
> Tenere presente che viene richiamato ogni volta che uno di questi metodi, il `Roles` classe delega la chiamata al provider configurato. In questo caso, ciò significa che la chiamata viene inviata al `SqlRoleProvider`. Il `SqlRoleProvider` quindi esegue l'operazione di database appropriato in base al metodo richiamato. Ad esempio, il codice `Roles.CreateRole("Administrators")` comporta il `SqlRoleProvider` l'esecuzione il `aspnet_Roles_CreateRole` stored procedure che inserisce un nuovo record nel `aspnet_Roles` tabella denominata Administrators.


Il resto di questa esercitazione vengono esaminati mediante il `Roles` della classe `CreateRole`, `GetAllRoles`, e `DeleteRole` metodi per gestire i ruoli del sistema.

## <a name="step-4-creating-new-roles"></a>Passaggio 4: Creazione di nuovi ruoli

Ruoli offrono un modo per raggruppare arbitrariamente gli utenti, e in genere questo raggruppamento viene utilizzato per applicare le regole di autorizzazione in modo più semplice. Tuttavia, per utilizzare i ruoli come un meccanismo di autorizzazione è innanzitutto necessario definire i ruoli presenti nell'applicazione. Purtroppo, ASP.NET non include un controllo CreateRoleWizard. Per aggiungere nuovi ruoli, che è necessario creare un'interfaccia utente appropriata e richiamare l'API di ruoli effettuata. Buone notizie sono che questo è molto facile da eseguire.

> [!NOTE]
> Benché sia presente alcun controllo CreateRoleWizard Web, è il [strumento Amministrazione sito Web ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx), che è un'applicazione ASP.NET locale progettata per facilitare la visualizzazione e la gestione della configurazione dell'applicazione web. Tuttavia, non si è un grande fan dello strumento Amministrazione sito Web ASP.NET per due motivi. Innanzitutto, è un po' anomalo e l'esperienza utente migliorate per essere desiderato. In secondo luogo, lo strumento Amministrazione sito Web di ASP.NET è progettato per funzionare solo in locale, vale a dire che è necessario compilare il proprio ruolo pagine web di gestione, se si desidera gestire in remoto i ruoli in un sito in tempo reale. Per questi motivi, in questa esercitazione e la successiva esaminerà in compilazione il ruolo necessario strumenti di gestione in una pagina web, anziché utilizzare lo strumento Amministrazione sito Web di ASP.NET.


Aprire il `ManageRoles.aspx` nella pagina di `Roles` cartella e aggiungere una casella di testo e un controllo pulsante Web alla pagina. Impostare il controllo TextBox `ID` proprietà `RoleName` e il pulsante `ID` e `Text` proprietà `CreateRoleButton` e Create Role, rispettivamente. A questo punto, markup dichiarativo della pagina dovrebbe essere simile al seguente:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample6.aspx)]

Successivamente, fare doppio clic su di `CreateRoleButton` pulsante di controllo nella finestra di progettazione per creare un `Click` gestore dell'evento e quindi aggiungere il codice seguente:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample7.cs)]

Il codice sopra riportato avvia assegnando il nome del ruolo tagliati immesso nel `RoleName` casella di testo per il `newRoleName` variabile. Successivamente, il `Roles` della classe `RoleExists` metodo viene chiamato per determinare se il ruolo `newRoleName` esiste già nel sistema. Se il ruolo non esiste, viene creato mediante una chiamata al `CreateRole` metodo. Se il `CreateRole` viene passato un nome di ruolo esiste già nel sistema, un `ProviderException` viene generata un'eccezione. Ecco perché il codice controlla per verificare che il ruolo non esiste già nel sistema prima di chiamare `CreateRole`. Il `Click` dell'operazione del gestore di evento cancellando la `RoleName` della casella di testo `Text` proprietà.

> [!NOTE]
> Per comprendere che cosa accade se l'utente non immette un valore nel `RoleName` casella di testo. Se il valore passato al `CreateRole` metodo `null` o una stringa vuota, un'eccezione viene generata. Analogamente, se il nome del ruolo contiene una virgola viene generata un'eccezione. Di conseguenza, la pagina deve contenere i controlli di convalida per verificare che l'utente immette un ruolo e che non contiene virgole. Lasciare un esercizio per il lettore.


Creare un ruolo denominato Administrators. Visitare il `ManageRoles.aspx` tramite un browser, digitare negli amministratori nella casella di testo (vedere Figura 3), quindi fare clic sul pulsante Crea ruolo.


[![Creare un ruolo di amministratori](creating-and-managing-roles-cs/_static/image8.png)](creating-and-managing-roles-cs/_static/image7.png)

**Figura 3**: creare un ruolo di amministratori ([fare clic per visualizzare l'immagine ingrandita](creating-and-managing-roles-cs/_static/image9.png))


Che succede? Si verifica un postback, ma non esiste alcun segnale visivo che il ruolo sia stato effettivamente aggiunti al sistema. Questa pagina nel passaggio 5 per includere indicazioni visive verrà aggiornato. Per il momento, tuttavia, è possibile verificare che il ruolo è stato creato, passare al `SecurityTutorials.mdf` database e la visualizzazione dei dati di `aspnet_Roles` tabella. Come illustrato nella figura 4, il `aspnet_Roles` tabella include un record per i ruoli di amministratori appena aggiunti.


[![Aspnet_Roles tabella dispone di una riga per gli amministratori](creating-and-managing-roles-cs/_static/image11.png)](creating-and-managing-roles-cs/_static/image10.png)

**Figura 4**: il `aspnet_Roles` la tabella contiene una riga per gli amministratori ([fare clic per visualizzare l'immagine ingrandita](creating-and-managing-roles-cs/_static/image12.png))


## <a name="step-5-displaying-the-roles-in-the-system"></a>Passaggio 5: Visualizzare i ruoli del sistema

Consente di aumentare la `ManageRoles.aspx` pagina per includere un elenco di ruoli correnti nel sistema. A tale scopo, aggiungere un controllo GridView alla pagina e impostare il relativo `ID` proprietà `RoleList`. Successivamente, aggiungere un metodo alla classe code-behind della pagina denominata `DisplayRolesInGrid` utilizzando il codice seguente:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample8.cs)]

Il `Roles` della classe `GetAllRoles` metodo restituisce tutti i ruoli del sistema come una matrice di stringhe. Questa matrice di stringhe viene quindi associata a GridView. Per associare l'elenco di ruoli a GridView, al primo caricamento della pagina, è necessario chiamare il `DisplayRolesInGrid` metodo della pagina `Page_Load` gestore dell'evento. Il codice seguente chiama questo metodo quando viene innanzitutto visitata, ma non nei postback successivi.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample9.cs)]

Con questo codice, visitare la pagina tramite un browser. Come illustrato nella figura 5, verrà visualizzata una griglia con una singola colonna con etichetta di elemento. La griglia include una riga per il ruolo di amministratore che è aggiunto nel passaggio 4.


[![Controllo GridView. consente di visualizzare i ruoli in un'unica colonna](creating-and-managing-roles-cs/_static/image14.png)](creating-and-managing-roles-cs/_static/image13.png)

**Figura 5**: GridView consente di visualizzare i ruoli in un'unica colonna ([fare clic per visualizzare l'immagine ingrandita](creating-and-managing-roles-cs/_static/image15.png))


GridView Visualizza una colonna singola con l'etichetta di elemento perché il controllo GridView `AutoGenerateColumns` è impostata su True (impostazione predefinita), provocando GridView creare automaticamente una colonna per ogni proprietà nel relativo `DataSource`. Matrice con un'unica proprietà che rappresenta gli elementi nella matrice, pertanto la singola colonna in GridView.

Quando si visualizzano i dati con un controllo GridView, è preferibile in modo esplicito definire colonne anziché generarli in modo implicito dal controllo GridView. Definendo in modo esplicito le colonne è molto più semplice formattare i dati, ridisporre le colonne ed eseguire altre attività comuni. Di conseguenza, si aggiornare markup dichiarativo del controllo GridView in modo che le colonne definite in modo esplicito.

Iniziare con la configurazione del controllo GridView `AutoGenerateColumns` la proprietà su False. Successivamente, aggiungere un TemplateField alla griglia, impostare il relativo `HeaderText` proprietà ai ruoli e configurare il `ItemTemplate` in modo che venga visualizzato il contenuto della matrice. A tale scopo, aggiungere un controllo etichetta Web denominato `RoleNameLabel` per il `ItemTemplate` e associare il `Text` proprietà `Container.DataItem`.

Queste proprietà e `ItemTemplate`del contenuto può essere impostato in modo dichiarativo o tramite la finestra di dialogo di campi del controllo GridView e interfaccia di modifica modelli. Per raggiungere i campi la finestra di dialogo, fare clic sul collegamento di modifica colonne nello Smart Tag del controllo GridView. Successivamente, deselezionare l'opzione genera automaticamente i campi casella di controllo per impostare il `AutoGenerateColumns` la proprietà su False e aggiungervi un TemplateField GridView, impostando il relativo `HeaderText` proprietà al ruolo. Per definire il `ItemTemplate`del contenuto, scegliere l'opzione di modifica modelli dal Smart Tag del controllo GridView. Trascinare un controllo etichetta Web il `ItemTemplate`, impostare il `ID` proprietà `RoleNameLabel`e configurare le impostazioni di associazione dati in modo che il `Text` proprietà è associata a `Container.DataItem`.

Indipendentemente dal quale approccio adottato, dichiarativo del controllo GridView risulta dovrebbe essere simile al seguente al termine.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample10.aspx)]

> [!NOTE]
> Contenuto della matrice viene visualizzato utilizzando la sintassi di associazione dati `<%# Container.DataItem %>`. Una descrizione completa dei motivi per cui questa sintassi è utilizzata quando il contenuto di una matrice di visualizzazione associato a GridView non rientra nell'ambito di questa esercitazione. Per ulteriori informazioni al riguardo, vedere [l'associazione di una matrice scalare per un controllo Web](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).


Attualmente, il `RoleList` GridView è associato solo all'elenco dei ruoli prima visita la pagina. È necessario aggiornare la griglia ogni volta che viene aggiunto un nuovo ruolo. A tale scopo, aggiornare il `CreateRoleButton` del pulsante `Click` gestore dell'evento in modo che venga chiamato il `DisplayRolesInGrid` metodo se viene creato un nuovo ruolo.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample11.cs)]

A questo punto quando l'utente aggiunge un nuovo ruolo di `RoleList` GridView viene illustrato il ruolo appena aggiunti durante il postback, fornire feedback visivo che il ruolo è stato creato. A questo scopo, visitare il `ManageRoles.aspx` pagina tramite un browser e aggiungere un ruolo denominato supervisori. Fare clic sul pulsante Create Role, su un postback verrà insorgere e la griglia verrà aggiornata per includere gli amministratori, nonché il nuovo ruolo, mentre per i supervisori.


[![Il ruolo supervisori è stato aggiunto](creating-and-managing-roles-cs/_static/image17.png)](creating-and-managing-roles-cs/_static/image16.png)

**Figura 6**: ruolo supervisori è stata aggiunta ([fare clic per visualizzare l'immagine ingrandita](creating-and-managing-roles-cs/_static/image18.png))


## <a name="step-6-deleting-roles"></a>Passaggio 6: Eliminazione di ruoli

A questo punto un utente può creare un nuovo ruolo e visualizzare tutti i ruoli esistenti dal `ManageRoles.aspx` pagina. Si consente agli utenti di eliminare anche i ruoli. Il `Roles.DeleteRole` metodo dispone di due overload:

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/library/ek4sywc0.aspx) -Elimina il ruolo *roleName*. Se il ruolo contiene uno o più membri, viene generata un'eccezione.
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/library/38h6wf59.aspx) -Elimina il ruolo *roleName*. Se *throwOnPopulateRole* è `true`, viene generata un'eccezione se il ruolo contiene uno o più membri. Se *throwOnPopulateRole* è `false`, quindi viene eliminato il ruolo se o non contiene membri. Internamente, il `DeleteRole(roleName)` chiamate al metodo `DeleteRole(roleName, true)`.

Il `DeleteRole` metodo genererà un'eccezione anche se *roleName* è `null` o una stringa vuota o se *roleName* contiene una virgola. Se *roleName* non esiste nel sistema, `DeleteRole` non riesce senza avvisare, senza generare un'eccezione.

Consente di aumentare GridView in `ManageRoles.aspx` per includere un pulsante Elimina che, quando si fa clic, Elimina il ruolo selezionato. Per iniziare, aggiungere un pulsante Elimina a GridView, riaprire la finestra di dialogo campi e aggiunta di un pulsante Elimina, che si trova sotto l'opzione CommandField. Verificare l'eliminazione pulsante colonna all'estrema sinistra e impostare il relativo `DeleteText` proprietà eliminare ruoli.


[![Aggiungere un pulsante Elimina a RoleList GridView](creating-and-managing-roles-cs/_static/image20.png)](creating-and-managing-roles-cs/_static/image19.png)

**Figura 7**: aggiungere un pulsante Elimina per il `RoleList` GridView ([fare clic per visualizzare l'immagine ingrandita](creating-and-managing-roles-cs/_static/image21.png))


Dopo aver aggiunto il pulsante Elimina, markup dichiarativo di GridView dovrebbe essere simile al seguente:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample12.aspx)]

Successivamente, creare un gestore eventi per il controllo GridView `RowDeleting` evento. Questo è l'evento generato durante il postback quando si fa clic sul pulsante Elimina ruolo. Aggiungere il codice seguente al gestore eventi.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample13.cs)]

Il codice di avvio facendo riferimento a livello di codice il `RoleNameLabel` controllo nella riga il cui pulsante Elimina ruolo Web. Il `Roles.DeleteRole` viene quindi richiamato metodo, passando il `Text` del `RoleNameLabel` e `false`, quindi eliminare il ruolo, indipendentemente dal fatto che vi sono utenti associati al ruolo. Infine, il `RoleList` GridView viene aggiornato in modo che il ruolo appena eliminato non viene più visualizzata nella griglia.

> [!NOTE]
> Pulsante Elimina ruolo non è necessaria una sorta di conferma da parte dell'utente prima di eliminare il ruolo. Uno dei modi più semplici per confermare un'azione avviene tramite una finestra di dialogo di conferma sul lato client. Per ulteriori informazioni su questa tecnica, vedere [aggiunta sul lato Client conferma quando l'eliminazione di](https://asp.net/learn/data-access/tutorial-42-cs.aspx).


## <a name="summary"></a>Riepilogo

Molte applicazioni web sono determinate regole di autorizzazione o una funzionalità a livello di pagina è disponibile solo per alcune classi di utenti. Ad esempio, potrebbe essere un set di pagine web che solo gli amministratori possono accedere. Invece di definire le regole di autorizzazione su base utente dall'utente, spesso è più utile definire le regole in base a un ruolo. Anziché consentire esplicitamente agli utenti Scott e Jisun per accedere alle pagine web di amministrazione, un approccio migliore è per consentire l'accesso alle singole pagine, i membri del ruolo amministratori e quindi indicare Scott e Jisun come appartenenti a utenti di Ruolo di amministratore.

Il framework di ruoli è semplice creare e gestire i ruoli. In questa esercitazione viene esaminato come configurare il framework di ruoli da utilizzare il `SqlRoleProvider`, che utilizza un database di Microsoft SQL Server come archivio ruoli. È stato anche creato una pagina web che elenca i ruoli esistenti nel sistema e consente di creare nuovi ruoli del e quelli esistenti da eliminare. Nelle esercitazioni successive verrà spiegato come assegnare utenti a ruoli e come applicare l'autorizzazione basata sui ruoli.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Analisi di ASP.NET 2.0 appartenenza, ruoli e profilo](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Procedura: Utilizzare Gestione ruoli in ASP.NET 2.0](https://msdn.microsoft.com/library/ms998314.aspx)
- [Provider di ruoli](https://msdn.microsoft.com/library/aa478950.aspx)
- [In sequenza il proprio strumento Amministrazione sito Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Documentazione tecnica per il `<roleManager>` elemento](https://msdn.microsoft.com/library/ms164660.aspx)
- [Tramite l'appartenenza e l'API di gestione di ruolo](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è  *[SAM insegna manualmente ASP.NET 2.0 nelle 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott può essere raggiunto al [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione includono Alicja Maziarz Suchi Banerjee e Teresa Murphy. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [avanti](assigning-roles-to-users-cs.md)
