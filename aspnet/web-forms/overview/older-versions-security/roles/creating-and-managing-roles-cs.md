---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
title: Creazione e gestione dei ruoli (c#) | Microsoft Docs
author: rick-anderson
description: Questa esercitazione vengono esaminate le procedure necessarie per configurare il framework di ruoli. In seguito, verrà compilata pagine web per creare ed eliminare ruoli.
ms.author: aspnetcontent
ms.date: 03/24/2008
ms.assetid: 113f10b3-a19a-471b-8ff6-db3c79ce8a91
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
msc.type: authoredcontent
ms.openlocfilehash: 84b9624aaae0cc98f1908b4521cee43bce6e856d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806872"
---
<a name="creating-and-managing-roles-c"></a>Creazione e gestione dei ruoli (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.09.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_cs.pdf)

> Questa esercitazione vengono esaminate le procedure necessarie per configurare il framework di ruoli. In seguito, verrà compilata pagine web per creare ed eliminare ruoli.


## <a name="introduction"></a>Introduzione

Nel <a id="_msoanchor_1"> </a> [ *basata sull'utente Authorization* ](../membership/user-based-authorization-cs.md) esercitazione abbiamo esaminato con l'autorizzazione URL per limitare a determinati utenti da un set di pagine ed è stata esaminata dichiarativa e tecniche a livello di codice per la configurazione delle funzionalità di una pagina ASP.NET basate su dell'utente. Concessione dell'autorizzazione per l'accesso alla pagina o la funzionalità in base a utente, tuttavia, può diventare un incubo negli scenari in cui sono presenti molti account utente o quando i privilegi degli utenti cambiano spesso. Ogni volta che un utente acquisisce o perde l'autorizzazione per eseguire una determinata attività, l'amministratore dovrà aggiornare le regole di autorizzazione URL appropriate, markup dichiarativo e codice.

In genere consente di classificare gli utenti in gruppi o *ruoli* e quindi di applicare le autorizzazioni in base a ruolo per ruolo. Ad esempio, la maggior parte delle applicazioni web hanno uno specifico set di pagine o attività che sono riservate solo per gli utenti amministratori. Utilizzando le tecniche apprese nel *autorizzazione basata su utente* esercitazione, verrà aggiunto le regole di autorizzazione URL appropriate, markup dichiarativo e codice per consentire gli account utente specificato eseguire attività amministrative. Ma se è stato aggiunto un nuovo amministratore o se un amministratore esistente è necessario avere diritti di amministrazione del proprio revocati, è necessario restituire e aggiornare il file di configurazione e le pagine web. Con i ruoli, tuttavia, è stato possibile creare un ruolo, denominato Administrators e assegnare tali utenti considerati attendibili per il ruolo di amministratore. Successivamente, si potrebbe aggiungere le regole di autorizzazione URL appropriate, markup dichiarativo e codice per consentire al ruolo di amministratori per eseguire le diverse attività amministrative. Con questa infrastruttura, è semplice come includendo o la rimozione dell'utente dal ruolo degli amministratori di aggiunta di nuovi amministratori del sito o la rimozione di quelli esistenti. Nessuna configurazione, markup dichiarativo o modifiche al codice sono necessari.

ASP.NET offre un framework di ruoli per la definizione dei ruoli e associandoli agli account utente. Con il framework di ruoli, è possibile creare ed eliminare ruoli, aggiungere utenti o rimuovere utenti da un ruolo, determinare il set di utenti che appartengono a un ruolo specifico e indicare se un utente appartiene a un particolare ruolo. Dopo aver configurato il framework di ruoli, è possibile limitare l'accesso alle pagine in un singolo ruolo-da-ruolo tramite regole di autorizzazione URL e mostrare o nascondere informazioni aggiuntive o funzionalità in una pagina in base ai ruoli dell'utente attualmente connesso.

Questa esercitazione vengono esaminate le procedure necessarie per configurare il framework di ruoli. In seguito, verrà compilata pagine web per creare ed eliminare ruoli. Nel <a id="_msoanchor_2"> </a> [ *assegnando ruoli agli utenti* ](assigning-roles-to-users-cs.md) dell'esercitazione verrà illustrato come aggiungere e rimuovere utenti dai ruoli. E il <a id="_msoanchor_3"> </a> [ *autorizzazione basata sui ruoli* ](role-based-authorization-cs.md) esercitazione verrà spiegato come limitare l'accesso alle pagine in base a ruolo per ruolo insieme come regolare a seconda della funzionalità di pagina ruolo dell'utente. Iniziamo!

## <a name="step-1-adding-new-aspnet-pages"></a>Passaggio 1: Aggiunta di nuove pagine ASP.NET

In questa esercitazione e i successivi due è verrà esaminare varie funzioni correlate a ruoli e funzionalità. È necessario una serie di pagine ASP.NET per implementare gli argomenti esaminati in queste esercitazioni. È possibile creare queste pagine e aggiornare la mappa del sito.

Iniziare creando una nuova cartella nel progetto denominato `Roles`. Successivamente, aggiungere quattro nuove pagine ASP.NET per la `Roles` cartella, ogni pagina con il collegamento di `Site.master` pagina master. Denominare le pagine:

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

Esplora soluzioni del progetto a questo punto dovrebbe essere simile allo screenshot illustrato nella figura 1.


[![Quattro nuove pagine sono stati aggiunti alla cartella dei ruoli](creating-and-managing-roles-cs/_static/image2.png)](creating-and-managing-roles-cs/_static/image1.png)

**Figura 1**: quattro nuove pagine sono stati aggiunti per il `Roles` cartella ([fare clic per visualizzare l'immagine con dimensioni normali](creating-and-managing-roles-cs/_static/image3.png))


Ogni pagina, a questo punto, avranno i controlli contenuto di due, uno per ognuno degli elementi ContentPlaceHolder della pagina master: `MainContent` e `LoginContent`.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample1.aspx)]

Si tenga presente che il `LoginContent` markup predefinite del controllo ContentPlaceHolder viene visualizzato un collegamento per effettuare l'accesso o disconnessione dal sito, a seconda che l'utente è autenticato. La presenza del `Content2` contenuto controllo nella pagina ASP.NET, tuttavia, esegue l'override di markup predefinito della pagina master. Come descritto <a id="_msoanchor_4"> </a> [ *una panoramica dell'autenticazione basata su form* ](../introduction/an-overview-of-forms-authentication-cs.md) esercitazione, viene sottoposto a override il markup predefinito è utile nelle pagine in cui è non si desidera visualizzare correlate all'accesso opzioni nella colonna sinistra.

Per questi quattro pagine, tuttavia, si vuole visualizzare il markup predefinito della pagina master per il `LoginContent` ContentPlaceHolder. Pertanto, rimuovere il markup dichiarativo per la `Content2` controllo contenuto. Al termine dell'operazione, ognuno dei markup della pagina quattro deve contenere solo un controllo contenuto.

Infine, è possibile aggiornare la mappa del sito (`Web.sitemap`) per includere le nuove pagine web. Aggiungere il codice XML seguente dopo il `<siteMapNode>` è stato aggiunto per le esercitazioni di appartenenza.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample2.xml)]

Con la mappa del sito aggiornata, visitare il sito tramite un browser. Come illustrato nella figura 2, la navigazione a sinistra ora include elementi per le esercitazioni di ruoli.


[![Quattro nuove pagine sono stati aggiunti alla cartella dei ruoli](creating-and-managing-roles-cs/_static/image5.png)](creating-and-managing-roles-cs/_static/image4.png)

**Figura 2**: quattro nuove pagine sono stati aggiunti per il `Roles` cartella ([fare clic per visualizzare l'immagine con dimensioni normali](creating-and-managing-roles-cs/_static/image6.png))


## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>Passaggio 2: Specificando e configurando il Provider di Framework di ruoli

Ad esempio il framework di appartenenza, il framework di ruoli viene compilato sopra il modello di provider. Come descritto nel <a id="_msoanchor_5"> </a> [ *nozioni fondamentali sulla sicurezza e supporto di ASP.NET* ](../introduction/security-basics-and-asp-net-support-cs.md) esercitazione .NET Framework viene fornito con tre provider di ruoli predefiniti: [ `AuthorizationStoreRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) , [ `WindowsTokenRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx), e [ `SqlRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx). Questa serie di esercitazioni illustra il `SqlRoleProvider`, che usa un database Microsoft SQL Server come archivio ruoli.

Sotto le quinte il framework di ruoli e `SqlRoleProvider` funzionano esattamente come il framework di appartenenza e `SqlMembershipProvider`. .NET Framework contiene un `Roles` classe utilizzata come l'API per il framework di ruoli. Il `Roles` classe dispone di metodi statici, ad esempio `CreateRole`, `DeleteRole`, `GetAllRoles`, `AddUserToRole`, `IsUserInRole`e così via. Quando uno di questi metodi viene richiamato, il `Roles` classe delega la chiamata al provider configurato. Il `SqlRoleProvider` funziona con le tabelle specifiche del ruolo (`aspnet_Roles` e `aspnet_UsersInRoles`) nella risposta.

Per poter utilizzare il `SqlRoleProvider` provider nella nostra applicazione, è necessario specificare quali database da utilizzare come archivio. Il `SqlRoleProvider` prevede che l'archivio di ruolo specificato dispone di alcune tabelle di database, viste e stored procedure. Questi oggetti di database necessari possono essere aggiunti utilizzando la [ `aspnet_regsql.exe` strumento](https://msdn.microsoft.com/library/ms229862.aspx). A questo punto si dispone già di un database con lo schema necessario per il `SqlRoleProvider`. Nel <a id="_msoanchor_6"> </a> [ *creazione dello Schema di appartenenza in SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) esercitazione è stato creato un database denominato `SecurityTutorials.mdf` e usato `aspnet_regsql.exe` per aggiungere l'applicazione servizi, inclusi gli oggetti di database necessari per il `SqlRoleProvider`. Pertanto è sufficiente indicare al framework di ruoli per abilitare il supporto di ruolo e usare la `SqlRoleProvider` con il `SecurityTutorials.mdf` database come archivio ruoli.

Il framework di ruoli viene configurato tramite il &lt; `roleManager` &gt; elemento dell'applicazione `Web.config` file. Per impostazione predefinita, supporto del ruolo è disabilitato. Per abilitarla, è necessario impostare il [ &lt; `roleManager` &gt; ](https://msdn.microsoft.com/library/ms164660.aspx) dell'elemento `enabled` attributo `true` come segue:

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample3.xml)]

Per impostazione predefinita, tutte le applicazioni web dispongono di un provider di ruoli denominato `AspNetSqlRoleProvider` di tipo `SqlRoleProvider`. Questo provider predefinito sia registrato nella `machine.config` (disponibile all'indirizzo `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample4.xml)]

Il provider `connectionStringName` attributo specifica l'archivio di ruolo che viene usato. Il `AspNetSqlRoleProvider` provider imposta questo attributo `LocalSqlServer`, che è definito anche nel `machine.config` e i punti, per impostazione predefinita, a un database di SQL Server 2005 Express Edition nel `App_Data` cartella denominata `aspnet.mdf`.

Di conseguenza, se è sufficiente abilitare il framework di ruoli senza specificare le informazioni del provider nella nostra applicazione `Web.config` file, l'applicazione usa il provider di ruoli predefinito registrato, `AspNetSqlRoleProvider`. Se il `~/App_Data/aspnet.mdf` database non esiste, il runtime ASP.NET creerà automaticamente e aggiungere lo schema di servizi dell'applicazione. Tuttavia, non si vuole usare il `aspnet.mdf` database; piuttosto, è possibile usare il `SecurityTutorials.mdf` database che abbiamo già creato e aggiunto lo schema di servizi dell'applicazione per. Questa modifica può essere eseguita in uno dei due modi:

- <strong>Specificare un valore per il</strong><strong>`LocalSqlServer`</strong><strong>nome stringa di connessione in</strong><strong>`Web.config`</strong><strong>.</strong> Sovrascrivendo il `LocalSqlServer` valore di nome di stringa di connessione nel `Web.config`, è possibile usare il provider di ruoli predefinito registrato (`AspNetSqlRoleProvider`) e assicurarsi che funzioni correttamente con il `SecurityTutorials.mdf` database. Per altre informazioni su questa tecnica, vedere [Scott Guthrie](https://weblogs.asp.net/scottgu/)del post di blog [configurazione di servizi dell'applicazione di ASP.NET 2.0 per usare SQL Server 2000 o SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Aggiungere un nuovo provider registrati typu</strong><strong>`SqlRoleProvider`</strong><strong>e configurare relativo</strong><strong>`connectionStringName`</strong><strong>impostazione in modo che punti la</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>database.</strong> Questo è l'approccio è consigliato e utilizzati nel <a id="_msoanchor_7"> </a> [ *creazione dello Schema di appartenenza in SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) esercitazione ed è l'approccio userà anche questa esercitazione.

Aggiungere il markup di configurazione dei ruoli seguente per il `Web.config` file. Questo markup registra un nuovo provider denominato `SecurityTutorialsSqlRoleProvider`.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample5.xml)]

Il markup precedente definisce il `SecurityTutorialsSqlRoleProvider` come provider predefinito (tramite il `defaultProvider` attributo il `<roleManager>` elemento). Imposta anche il `SecurityTutorialsSqlRoleProvider`del `applicationName` se si imposta su `SecurityTutorials`, che è la stessa `applicationName` impostazione utilizzata dal provider di appartenenze (`SecurityTutorialsSqlMembershipProvider`). Sebbene non illustrato in questo caso, il [ `<add>` elemento](https://msdn.microsoft.com/library/ms164662.aspx) per il `SqlRoleProvider` può inoltre contenere una `commandTimeout` attributo per specificare la durata del timeout del database, in secondi. Il valore predefinito è 30.

Con questo codice di configurazione posto, siamo pronti iniziare a usare funzionalità di ruolo all'interno dell'applicazione.

> [!NOTE]
> Il markup di configurazione precedente viene illustrato l'utilizzo di &lt; `roleManager` &gt; dell'elemento `enabled` e `defaultProvider` attributi. Esistono numerosi altri attributi che influiscono sul modo in cui il framework di ruoli associa le informazioni sui ruoli in base a utente. Verranno esaminate queste impostazioni nella finestra di <a id="_msoanchor_8"> </a> [ *autorizzazione basata sui ruoli* ](role-based-authorization-cs.md) esercitazione.


## <a name="step-3-examining-the-roles-api"></a>Passaggio 3: Esaminare i ruoli API

Funzionalità del framework ruoli sono esposte tramite il [ `Roles` classe](https://msdn.microsoft.com/library/system.web.security.roles.aspx), che contiene metodi statici tredici per l'esecuzione di operazioni basato sui ruoli. Quando si esamina la creazione e l'eliminazione di ruoli in 4 passaggi e 6 si userà il [ `CreateRole` ](https://msdn.microsoft.com/library/system.web.security.roles.createrole.aspx) e [ `DeleteRole` ](https://msdn.microsoft.com/library/system.web.security.roles.deleterole.aspx) metodi, aggiungere o rimuovere un ruolo dal sistema.

Per ottenere un elenco di tutti i ruoli del sistema, usare il [ `GetAllRoles` metodo](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx) (vedere il passaggio 5). Il [ `RoleExists` metodo](https://msdn.microsoft.com/library/system.web.security.roles.roleexists.aspx) restituisce un valore booleano che indica se un ruolo specificato esiste.

Nella prossima esercitazione verrà esaminato come associare gli utenti a ruoli. Il `Roles` della classe [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx), [ `AddUserToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertoroles.aspx), [ `AddUsersToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstorole.aspx), e [ `AddUsersToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstoroles.aspx) metodi consentono di aggiungere uno o più utenti a uno o più ruoli. Per rimuovere utenti dai ruoli, usare il [ `RemoveUserFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx), [ `RemoveUserFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromroles.aspx), [ `RemoveUsersFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromrole.aspx), o [ `RemoveUsersFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromroles.aspx) metodi.

Nel <a id="_msoanchor_9"> </a> [ *autorizzazione basata sui ruoli* ](role-based-authorization-cs.md) esercitazione verranno esaminati modi a livello di programmazione mostrare o nascondere la funzionalità in base al ruolo dell'utente attualmente connesso. A tale scopo, è possibile usare la `Role` della classe [ `FindUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.findusersinrole.aspx), [ `GetRolesForUser` ](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx), [ `GetUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx), o [ `IsUserInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx) metodi.

> [!NOTE]
> Tenere presente che qualsiasi momento uno di questi metodi viene richiamato, il `Roles` classe delega la chiamata al provider configurato. In questo caso, ciò significa che la chiamata viene inviata al `SqlRoleProvider`. Il `SqlRoleProvider` quindi esegue l'operazione di database appropriato in base al metodo richiamato. Ad esempio, il codice `Roles.CreateRole("Administrators")` comporta la `SqlRoleProvider` l'esecuzione il `aspnet_Roles_CreateRole` stored procedure, che inserisce un nuovo record nel `aspnet_Roles` tabella denominata gli amministratori.


Nella parte restante di questa esercitazione vengono esaminati tramite il `Roles` della classe `CreateRole`, `GetAllRoles`, e `DeleteRole` metodi per gestire i ruoli del sistema.

## <a name="step-4-creating-new-roles"></a>Passaggio 4: Creazione di nuovi ruoli

Ruoli offrono un modo per utenti del gruppo in modo arbitrario e questo raggruppamento viene solitamente usato per applicare le regole di autorizzazione in modo più pratico. Ma per poter utilizzare i ruoli come un meccanismo di autorizzazione è innanzitutto necessario definire i ruoli presenti nell'applicazione. Purtroppo, ASP.NET non include un controllo CreateRoleWizard. Per aggiungere nuovi ruoli è necessario creare un'interfaccia utente appropriata e richiamare l'API di ruoli di noi stessi. La buona notizia è che questo è molto facile da realizzare.

> [!NOTE]
> Se non è presente alcun controllo CreateRoleWizard Web, è il [strumento Amministrazione sito Web ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx), ovvero un'applicazione ASP.NET locale progettata per facilitare la visualizzazione e gestione della configurazione dell'applicazione web. Tuttavia, non sono un grande fan dello strumento di amministrazione sito Web ASP.NET per due motivi. In primo luogo, è un po' anomalo e l'esperienza utente lascia un possono essere notevolmente. In secondo luogo, lo strumento Amministrazione sito Web di ASP.NET è progettato per funzionare solo in locale, vale a dire che è necessario creare il proprio ruolo le pagine web di gestione se si desidera gestire in remoto ruoli in un sito live. Per questi due motivi, in questa esercitazione e i successivi sarà incentrata sugli Creazione ruolo necessario strumenti di gestione in una pagina web anziché basarsi sullo strumento Amministrazione sito Web di ASP.NET.


Aprire il `ManageRoles.aspx` nella pagina di `Roles` cartella e aggiungere una casella di testo e un controllo pulsante Web alla pagina. Impostare il controllo TextBox `ID` proprietà `RoleName` e il pulsante `ID` e `Text` le proprietà da `CreateRoleButton` e Create Role, rispettivamente. A questo punto, markup dichiarativo della pagina dovrebbe essere simile al seguente:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample6.aspx)]

Successivamente, fare doppio clic il `CreateRoleButton` pulsante di controllo nella finestra di progettazione per creare un `Click` gestore dell'evento e quindi aggiungere il codice seguente:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample7.cs)]

Il codice sopra riportato inizia assegnando il nome del ruolo tagliati immesso nel `RoleName` casella di testo per il `newRoleName` variabile. Successivamente, il `Roles` della classe `RoleExists` viene chiamato per determinare se il ruolo `newRoleName` esiste già nel sistema. Se il ruolo non esiste, viene creato tramite una chiamata verso la `CreateRole` (metodo). Se il `CreateRole` viene passato un nome di ruolo che esiste già nel sistema, un `ProviderException` viene generata un'eccezione. Ecco perché il codice controlla per verificare che il ruolo non esiste già nel sistema prima di chiamare `CreateRole`. Il `Click` gestore dell'evento conclude cancellando il `RoleName` della casella di testo `Text` proprietà.

> [!NOTE]
> Si potrebbe chiedere cosa succede se l'utente non immette qualsiasi valore nel `RoleName` nella casella di testo. Se il valore passato nel `CreateRole` metodo `null` o una stringa vuota, un'eccezione viene generata. Analogamente, se il nome del ruolo contiene una virgola viene generata un'eccezione. Di conseguenza, la pagina deve contenere i controlli di convalida per assicurare che l'utente immette un ruolo e che non contiene virgole. Lascia come esercizio per il lettore.


È possibile creare un ruolo denominato Administrators. Visita il `ManageRoles.aspx` tramite un browser, digitare nella casella di testo negli amministratori (vedere la figura 3), quindi fare clic sul pulsante Crea ruolo.


[![Creare un ruolo di amministratori](creating-and-managing-roles-cs/_static/image8.png)](creating-and-managing-roles-cs/_static/image7.png)

**Figura 3**: creare un ruolo di amministratori ([fare clic per visualizzare l'immagine con dimensioni normali](creating-and-managing-roles-cs/_static/image9.png))


Che succede? Si verifica un postback, ma non esiste alcun segnale visivo che il ruolo è stata effettivamente aggiunta al sistema. Microsoft potrebbe aggiornare questa pagina nel passaggio 5 per includere indicazioni visive. Per il momento, tuttavia, è possibile verificare che sia stato creato il ruolo selezionando la `SecurityTutorials.mdf` database e la visualizzazione dei dati il `aspnet_Roles` tabella. Come illustrato nella figura 4, il `aspnet_Roles` tabella contiene un record per i ruoli di amministratori appena aggiunti.


[![La tabella aspnet_Roles dispone di una riga per gli amministratori](creating-and-managing-roles-cs/_static/image11.png)](creating-and-managing-roles-cs/_static/image10.png)

**Figura 4**: il `aspnet_Roles` la tabella contiene una riga per gli amministratori ([fare clic per visualizzare l'immagine con dimensioni normali](creating-and-managing-roles-cs/_static/image12.png))


## <a name="step-5-displaying-the-roles-in-the-system"></a>Passaggio 5: Visualizzare i ruoli del sistema

È possibile aumentare il `ManageRoles.aspx` pagina per includere un elenco dei ruoli correnti nel sistema. A tale scopo, aggiungere un controllo GridView alla pagina e impostare relativi `ID` proprietà `RoleList`. Successivamente, aggiungere un metodo alla classe code-behind della pagina denominato `DisplayRolesInGrid` usando il codice seguente:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample8.cs)]

Il `Roles` della classe `GetAllRoles` metodo restituisce tutti i ruoli del sistema come una matrice di stringhe. Questa matrice di stringhe viene quindi associata a GridView. Per associare l'elenco dei ruoli predefiniti a GridView quando la pagina viene caricata, è necessario chiamare il `DisplayRolesInGrid` metodo di pagina `Page_Load` gestore dell'evento. Il codice seguente chiama questo metodo quando la pagina viene prima visitata, ma non nei postback successivi.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample9.cs)]

Con questo codice, visitare la pagina tramite un browser. Come illustrato nella figura 5, verrà visualizzata una griglia con una singola colonna con etichetta elemento. La griglia include una riga per il ruolo di amministratore che è stato aggiunto nel passaggio 4.


[![GridView Visualizza i ruoli in una singola colonna](creating-and-managing-roles-cs/_static/image14.png)](creating-and-managing-roles-cs/_static/image13.png)

**Figura 5**: GridView Visualizza i ruoli in una sola colonna ([fare clic per visualizzare l'immagine con dimensioni normali](creating-and-managing-roles-cs/_static/image15.png))


GridView Visualizza una colonna singola con etichettata elemento perché il controllo GridView `AutoGenerateColumns` viene impostata su True (impostazione predefinita), che fa sì che il controllo GridView creare automaticamente una colonna per ogni proprietà nel relativo `DataSource`. Una matrice ha una singola proprietà che rappresenta gli elementi nella matrice, di conseguenza la singola colonna in GridView.

Quando si visualizzano i dati con un controllo GridView, preferisco in modo esplicito definire articoli anziché generarli in modo implicito per il controllo GridView. Definendo in modo esplicito le colonne è molto più semplice formattare i dati, riorganizzare le colonne ed eseguire altre attività comuni. Pertanto, aggiorniamo markup dichiarativo del controllo GridView in modo che le relative colonne sono definite esplicitamente.

Per iniziare, l'impostazione del controllo GridView `AutoGenerateColumns` proprietà su False. Successivamente, aggiungere un TemplateField alla griglia, impostare relativi `HeaderText` proprietà ai ruoli e configurare relativo `ItemTemplate` in modo che venga visualizzato il contenuto della matrice. A tale scopo, aggiungere un controllo etichetta Web denominato `RoleNameLabel` per il `ItemTemplate` ed eseguire l'associazione relativa `Text` proprietà `Container.DataItem`.

Queste proprietà e il `ItemTemplate`del contenuto può essere impostato in modo dichiarativo o tramite la finestra di dialogo campi del controllo GridView e l'interfaccia di modifica modelli. Per raggiungere i campi nella finestra di dialogo, fare clic sul collegamento Modifica colonne in GridView Smart Tag. Successivamente, deselezionare l'opzione genera automaticamente campi sulla casella di controllo per impostare il `AutoGenerateColumns` la proprietà su False e aggiungere un TemplateField a GridView, impostando il `HeaderText` proprietà al ruolo. Per definire il `ItemTemplate`del contenuto, scegliere l'opzione di modifica modelli dallo Smart Tag del controllo GridView. Trascinare un controllo etichetta Web il `ItemTemplate`, impostare relativi `ID` proprietà `RoleNameLabel`e configurarne le impostazioni di Data Binding in modo che relativi `Text` proprietà è associata a `Container.DataItem`.

Indipendentemente dal fatto che l'approccio scelto, dopo aver dovrebbe essere simile al seguente markup dichiarativo risultante di GridView.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample10.aspx)]

> [!NOTE]
> Il contenuto della matrice viene visualizzato utilizzando la sintassi di associazione dati `<%# Container.DataItem %>`. Una descrizione dettagliata del motivo per cui viene usata questa sintassi quando la visualizzazione del contenuto di una matrice associato a GridView esula dall'ambito di questa esercitazione. Per altre informazioni al riguardo, consultare [associazione di una matrice scalare a un controllo Web](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).


Attualmente, il `RoleList` GridView è associato solo all'elenco dei ruoli quando viene prima visitata la pagina. È necessario aggiornare la griglia ogni volta che viene aggiunto un nuovo ruolo. A tale scopo, aggiornare il `CreateRoleButton` del pulsante `Click` gestore dell'evento in modo che venga chiamato il `DisplayRolesInGrid` metodo se viene creato un nuovo ruolo.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample11.cs)]

A questo punto quando l'utente aggiunge un nuovo ruolo di `RoleList` GridView viene illustrato il ruolo appena aggiunti durante il postback, che fornisce feedback visivo che è stato creato il ruolo. Per illustrare questo scenario, visitare il `ManageRoles.aspx` pagina tramite un browser e aggiungere un ruolo denominato supervisori. Facendo clic sul pulsante Create Role, negativi a un postback e la griglia verrà aggiornato per includere gli amministratori, nonché il nuovo ruolo, supervisori.


[![Il ruolo supervisori è stato aggiunto](creating-and-managing-roles-cs/_static/image17.png)](creating-and-managing-roles-cs/_static/image16.png)

**Figura 6**: il ruolo supervisori è stato aggiunto ([fare clic per visualizzare l'immagine con dimensioni normali](creating-and-managing-roles-cs/_static/image18.png))


## <a name="step-6-deleting-roles"></a>Passaggio 6: Eliminazione di ruoli

A questo punto un utente può creare un nuovo ruolo e visualizzare tutti i ruoli esistenti dal `ManageRoles.aspx` pagina. È possibile consentire agli utenti di eliminare anche i ruoli. Il `Roles.DeleteRole` metodo presenta due overload:

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/library/ek4sywc0.aspx) -Consente di eliminare il ruolo *roleName*. Se il ruolo contiene uno o più membri, viene generata un'eccezione.
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/library/38h6wf59.aspx) -Consente di eliminare il ruolo *roleName*. Se *throwOnPopulateRole* è `true`, viene generata un'eccezione se il ruolo contiene uno o più membri. Se *throwOnPopulateRole* è `false`, quindi viene eliminato il ruolo se o non contiene alcun membro. Internamente, il `DeleteRole(roleName)` chiamate al metodo `DeleteRole(roleName, true)`.

Il `DeleteRole` metodo genererà anche un'eccezione se *roleName* viene `null` o una stringa vuota o se *roleName* contiene una virgola. Se *roleName* non esiste nel sistema, `DeleteRole` ha esito negativo in modalità invisibile, senza generare un'eccezione.

È possibile aumentare il controllo GridView in `ManageRoles.aspx` da includere un'operazione di eliminazione pulsante che, quando si fa clic, consente di eliminare il ruolo selezionato. Iniziare aggiungendo un pulsante di eliminazione a GridView passando alla finestra di dialogo campi e aggiunta di un pulsante di eliminazione, che si trova sotto l'opzione CommandField. Rendere l'eliminazione colonna all'estrema sinistra pulsante e impostarne il `DeleteText` proprietà da eliminare ruoli.


[![Aggiungere un pulsante di eliminazione a RoleList GridView](creating-and-managing-roles-cs/_static/image20.png)](creating-and-managing-roles-cs/_static/image19.png)

**Figura 7**: aggiungere un pulsante Elimina per il `RoleList` GridView ([fare clic per visualizzare l'immagine con dimensioni normali](creating-and-managing-roles-cs/_static/image21.png))


Dopo aver aggiunto il pulsante di eliminazione, markup dichiarativo di GridView dovrebbe essere simile al seguente:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample12.aspx)]

Successivamente, creare un gestore eventi per il controllo GridView `RowDeleting` evento. Questo è l'evento generato durante il postback quando viene scelto il pulsante Elimina ruolo. Aggiungere il codice seguente al gestore eventi.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample13.cs)]

Il codice inizia facendo riferimento a livello di codice il `RoleNameLabel` controllo nella riga è stato fatto clic sul pulsante la cui eliminazione ruolo Web. Il `Roles.DeleteRole` metodo viene quindi richiamato, passando il `Text` del `RoleNameLabel` e `false`, in tal modo eliminare il ruolo indipendentemente dal fatto che sono presenti utenti associati al ruolo. Infine, il `RoleList` GridView viene aggiornato in modo che il ruolo appena è stato eliminato non viene più visualizzata nella griglia.

> [!NOTE]
> Il pulsante Elimina ruolo non richiede una sorta di conferma da parte dell'utente prima di eliminare il ruolo. Uno dei modi più semplici per confermare un'azione è tramite una finestra di dialogo di conferma dal lato client. Per altre informazioni su questa tecnica, vedere [aggiunta di conferma dal lato Client quando Elimina](https://asp.net/learn/data-access/tutorial-42-cs.aspx).


## <a name="summary"></a>Riepilogo

Molte applicazioni web sono determinate regole di autorizzazione o una funzionalità a livello di pagina è disponibile solo per alcune classi di utenti. Ad esempio, potrebbe esserci un set di pagine web che solo gli amministratori possono accedere. Invece di definire queste regole di autorizzazione in base a utente, spesso è più utile definire le regole in base a un ruolo. Anziché consentire esplicitamente agli utenti in cui Scott e Jisun per accedere alle pagine web di amministrazione, un approccio più facilmente gestibile è per consentire i membri del ruolo amministratori di accesso alle singole pagine e quindi per indicare come gli utenti appartenenti a Scott e Jisun il Ruolo di amministratore.

Il framework di ruoli semplifica creare e gestire i ruoli. In questa esercitazione sono stati esaminati jak nakonfigurovat al framework di ruoli di usare il `SqlRoleProvider`, che usa un database Microsoft SQL Server come archivio ruoli. Abbiamo creato anche una pagina web che elenca i ruoli esistenti nel sistema e consente di nuovi ruoli deve essere creato e quelle esistenti da eliminare. Nelle esercitazioni successive si vedrà come assegnare gli utenti ai ruoli e su come applicare l'autorizzazione basata sui ruoli.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Analisi di ASP.NET 2.0 appartenenza, ruoli e profilo](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Procedura: Utilizzare Gestione ruoli in ASP.NET 2.0](https://msdn.microsoft.com/library/ms998314.aspx)
- [Provider di ruoli](https://msdn.microsoft.com/library/aa478950.aspx)
- [In sequenza il proprio strumento Amministrazione sito Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Documentazione tecnica per il `<roleManager>` elemento](https://msdn.microsoft.com/library/ms164660.aspx)
- [Tramite l'appartenenza e l'API di gestione di ruolo](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla.com, ha lavorato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola  *[Sams Teach Yourself ASP.NET 2.0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. È possibile contattarlo Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione includono Alicja Maziarz Suchi Banerjee e Teresa Murphy. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami un messaggio nella [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [avanti](assigning-roles-to-users-cs.md)
