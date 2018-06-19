---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
title: L'assegnazione di ruoli agli utenti (VB) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione che verranno create due pagine ASP.NET per agevolare la gestione degli utenti a cui appartengono a quali ruoli. La prima pagina includerà strutture per vedere quali...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: fd208ee9-69cc-4467-9783-b4e039bdd1d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
msc.type: authoredcontent
ms.openlocfilehash: 959a73f53d4fdb114f222fe8bc830876b76c9d9e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892010"
---
<a name="assigning-roles-to-users-vb"></a>L'assegnazione di ruoli agli utenti (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.10.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_vb.pdf)

> In questa esercitazione che verranno create due pagine ASP.NET per agevolare la gestione degli utenti a cui appartengono a quali ruoli. La prima pagina include funzionalità per visualizzare gli utenti che appartengono a un determinato ruolo, i ruoli a cui appartiene un utente specifico e la possibilità di assegnare o rimuovere un utente specifico da un particolare ruolo. Nella seconda pagina aumenterà il controllo CreateUserWizard in modo che includa un passaggio per specificare i ruoli a cui appartiene l'utente appena creato. Ciò è utile negli scenari in cui è in grado di creare nuovi account utente amministratore.


## <a name="introduction"></a>Introduzione

Il <a id="_msoanchor_1"> </a> [esercitazione precedente](creating-and-managing-roles-vb.md) esaminato il framework di ruoli e `SqlRoleProvider`; è stato illustrato come utilizzare la `Roles` classe per creare, recuperare ed eliminare ruoli. Oltre alla creazione ed eliminazione di ruoli, è necessario essere in grado di assegnare o rimuovere utenti da un ruolo. Purtroppo, ASP.NET non viene fornito con tutti i controlli Web per la gestione degli utenti a cui appartengono a quali ruoli. In alternativa, è necessario creare proprie pagine ASP.NET per gestire queste associazioni. Buone notizie sono che l'aggiunta e rimozione di utenti ai ruoli è molto semplice. La `Roles` classe contiene un numero di metodi per l'aggiunta di uno o più utenti a uno o più ruoli.

In questa esercitazione che verranno create due pagine ASP.NET per agevolare la gestione degli utenti a cui appartengono a quali ruoli. La prima pagina include funzionalità per visualizzare gli utenti che appartengono a un determinato ruolo, i ruoli a cui appartiene un utente specifico e la possibilità di assegnare o rimuovere un utente specifico da un particolare ruolo. Nella seconda pagina aumenterà il controllo CreateUserWizard in modo che includa un passaggio per specificare i ruoli a cui appartiene l'utente appena creato. Ciò è utile negli scenari in cui è in grado di creare nuovi account utente amministratore.

Iniziamo!

## <a name="listing-what-users-belong-to-what-roles"></a>Elenca gli utenti che appartengono a quali ruoli

Il primo ordine di importanza per questa esercitazione consiste nel creare una pagina web da cui gli utenti possono essere assegnati ai ruoli. Prima abbiamo riguardano effettuata con le procedure per assegnare utenti a ruoli, si innanzitutto concentrarsi su come determinare gli utenti che appartengono a quali ruoli. Esistono due modi per visualizzare queste informazioni: "ruolo" o "dall'utente." È possibile consentire il visitatore selezionare un ruolo e quindi visualizzarli tutti gli utenti che appartengono al ruolo (visualizzazione "dal ruolo") oppure è possibile richiedere il visitatore per selezionare un utente e quindi visualizzarli i ruoli assegnati all'utente (visualizzazione "dall'utente").

La visualizzazione "dal ruolo" è utile nei casi in cui il visitatore desidera conoscere il set di utenti che appartengono a un determinato ruolo; la visualizzazione "dall'utente" è ideale quando il visitatore deve conoscere i ruoli di un determinato utente. Di seguito sono page includono "dal ruolo" e "dall'utente" interfacce.

Si inizierà con la creazione dell'interfaccia "dall'utente". Questa interfaccia sarà costituito da un elenco a discesa e un elenco di caselle di controllo. L'elenco a discesa verrà popolato con il set di utenti nel sistema. le caselle di controllo permette di enumerare i ruoli. Selezionando un utente dall'elenco a discesa controllerà i ruoli a cui che appartiene l'utente. L'utente visita la pagina può quindi selezionare o deselezionare le caselle di controllo per aggiungere o rimuovere l'utente selezionato da ruoli corrispondenti.

> [!NOTE]
> Utilizzando un elenco a discesa elenco all'account utente non è la scelta ideale per i siti Web in cui possono essere presenti centinaia di account utente. Un elenco a discesa è progettato per consentire a un utente di selezionare un elemento da un elenco relativamente breve delle opzioni. Rapidamente diventa difficile da gestire man mano che aumenta il numero di elementi dell'elenco. Se si sta compilando un sito Web che avrà un numero potenzialmente elevato di account utente, si consiglia di considerare utilizzando un'interfaccia utente alternativa, ad esempio un GridView paginabile o un'interfaccia filtrabile che elenca richiede il visitatore per scegliere una lettera e quindi solo Mostra gli utenti il cui nome utente inizia con la lettera selezionata.


## <a name="step-1-building-the-by-user-user-interface"></a>Passaggio 1: Creazione dell'interfaccia utente "Dall'utente"

Aprire il `UsersAndRoles.aspx` pagina. Nella parte superiore della pagina, aggiungere un controllo etichetta Web denominato `ActionStatus` e cancellare la `Text` proprietà. Si utilizzerà questa etichetta per fornire commenti e suggerimenti sulle azioni eseguite, la visualizzazione dei messaggi, ad esempio, "Tito utente è stato aggiunto al ruolo Administrators" o "Jisun utente è stato rimosso dal ruolo supervisori". Per apportare queste risalto i messaggi, impostare l'etichetta `CssClass` proprietà su "Importante".

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample1.aspx)]

Successivamente, aggiungere la seguente definizione di classe CSS per il `Styles.css` foglio di stile:

[!code-css[Main](assigning-roles-to-users-vb/samples/sample2.css)]

Questa definizione CSS indica al browser per visualizzare l'etichetta di un carattere di grandi dimensioni, di colore rosso. Figura 1 mostra questo effetto tramite Progettazione di Visual Studio.


[![La proprietà dell'etichetta CssClass comporta un tipo di carattere di grandi dimensioni, di colore rosso](assigning-roles-to-users-vb/_static/image2.png)](assigning-roles-to-users-vb/_static/image1.png)

**Figura 1**: l'etichetta `CssClass` risultati della proprietà in un grande, tipo di carattere rosso ([fare clic per visualizzare l'immagine ingrandita](assigning-roles-to-users-vb/_static/image3.png))


Successivamente, aggiungere un controllo DropDownList alla pagina, impostare il relativo `ID` proprietà `UserList`e impostare il relativo `AutoPostBack` proprietà su True. Si utilizzerà questo DropDownList per elencare tutti gli utenti nel sistema. Questo DropDownList verrà associata a una raccolta di oggetti MembershipUser. Dal momento che DropDownList per visualizzare le proprietà UserName dell'oggetto MembershipUser (e utilizzarlo come valore degli elementi dell'elenco), impostare il DropDownList `DataTextField` e `DataValueField` proprietà su "UserName".

Di sotto di DropDownList, aggiungere un ripetitore denominato `UsersRoleList`. Il controllo Repeater elencherà tutti i ruoli del sistema come una serie di caselle di controllo. Definire il controllo Repeater `ItemTemplate` utilizzando il seguente markup dichiarativo:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample3.aspx)]

Il `ItemTemplate` markup include un singolo controllo casella di controllo Web denominato `RoleCheckBox`. La casella di controllo `AutoPostBack` è impostata su True e `Text` proprietà è associata a `Container.DataItem`. Il motivo è semplicemente la sintassi di associazione dati `Container.DataItem` perché il framework di ruoli restituisce l'elenco di nomi di ruoli come una matrice di stringhe ed è la matrice di stringhe che è l'associazione al controllo Repeater. Una descrizione completa dei motivi per cui questa sintassi è utilizzata per visualizzare il contenuto di una matrice associato a un controllo Web di dati non rientra nell'ambito di questa esercitazione. Per ulteriori informazioni al riguardo, vedere [l'associazione di una matrice scalare per un controllo Web](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

Markup dichiarativo dell'interfaccia "dall'utente" a questo punto dovrebbe essere simile al seguente:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample4.aspx)]

Ci sono ora pronti per la scrittura di codice per associare il set di account utente da DropDownList e il set di ruoli per il controllo Repeater. Nella classe code-behind della pagina, aggiungere un metodo denominato `BindUsersToUserList` e un altro denominato `BindRolesList`, utilizzando il codice seguente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample5.vb)]

Il `BindUsersToUserList` che consente di recuperare tutti gli account utente nel sistema tramite il [ `Membership.GetAllUsers` metodo](https://msdn.microsoft.com/library/dy8swhya.aspx). Restituisce un [ `MembershipUserCollection` oggetto](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx), che è una raccolta di [ `MembershipUser` istanze](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx). Questa raccolta viene quindi associata al `UserList` DropDownList. Il `MembershipUser` istanze di tale struttura, la raccolta contiene una serie di proprietà, ad esempio `UserName`, `Email`, `CreationDate`, e `IsOnline`. Per indicare DropDownList per visualizzare il valore del `UserName` proprietà, assicurarsi che il `UserList` del DropDownList `DataTextField` e `DataValueField` sono state impostate proprietà su "UserName".

> [!NOTE]
> Il `Membership.GetAllUsers` metodo dispone di due overload: uno che non accetta alcun parametro di input e restituisce tutti gli utenti e uno che accetta i valori integer per l'indice della pagina e le dimensioni di pagina e restituisce solo il sottoinsieme di utenti specificato. Quando sono presenti grandi quantità di account utente visualizzato in un elemento dell'interfaccia utente di paging, il secondo overload è utilizzabile per pagina in modo più efficiente di utenti perché restituisce solo il subset preciso di account utente anziché a tutti gli elementi.


Il `BindRolesToList` metodo inizia chiamando il `Roles` della classe [ `GetAllRoles` metodo](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx), che restituisce una matrice di stringhe contenente i ruoli del sistema. Questa matrice di stringhe viene quindi associata al controllo Repeater.

Infine, è necessario chiamare questi due metodi quando viene caricata la pagina. Aggiungere il codice seguente al gestore eventi `Page_Load`:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample6.vb)]

Con questo codice, dedicare alcuni minuti per visitare la pagina tramite un browser. la schermata dovrebbe essere simile alla figura 2. Tutti gli account utente vengono popolati nell'elenco a discesa e, di sotto, ogni ruolo viene visualizzato come una casella di controllo. Poiché è impostato il `AutoPostBack` proprietà DropDownList delle caselle di controllo su True, la modifica dell'utente selezionato o selezionando o deselezionando di un ruolo provoca un postback. Viene eseguita alcuna azione, tuttavia, poiché è ancora necessario scrivere codice per gestire queste azioni. Che verranno affrontati queste attività in due sezioni successive.


[![Nella pagina vengono visualizzati gli utenti e ruoli](assigning-roles-to-users-vb/_static/image5.png)](assigning-roles-to-users-vb/_static/image4.png)

**Figura 2**: nella pagina vengono visualizzati gli utenti e ruoli ([fare clic per visualizzare l'immagine ingrandita](assigning-roles-to-users-vb/_static/image6.png))


### <a name="checking-the-roles-the-selected-user-belongs-to"></a>Controllare i ruoli utente selezionato appartiene a

Quando viene caricata la pagina o ogni volta che il visitatore consente di selezionare un nuovo utente nell'elenco a discesa, è necessario aggiornare il `UsersRoleList`di caselle di controllo in modo che un determinato ruolo casella solo se l'utente selezionato appartiene a tale ruolo. A tale scopo, creare un metodo denominato `CheckRolesForSelectedUser` con il codice seguente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample7.vb)]

Il codice sopra riportato innanzitutto determinare chi è l'utente selezionato. Quindi utilizza la classe di ruoli [ `GetRolesForUser(userName)` metodo](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx) per restituire il set dell'utente specificato dei ruoli come una matrice di stringhe. Successivamente, vengono enumerati gli elementi del ripetitore e ogni elemento `RoleCheckBox` a livello di codice a cui fa riferimento nella casella di controllo. La casella di controllo è selezionata solo se il ruolo corrisponde al contenuto all'interno di `selectedUsersRoles` matrice di stringhe.

> [!NOTE]
> Il `Linq.Enumerable.Contains(Of String)(...)` sintassi non verrà compilato in ASP.NET versione 2.0. Il `Contains(Of String)` metodo fa parte di [libreria LINQ](http://en.wikipedia.org/wiki/Language_Integrated_Query), che è una novità di ASP.NET 3.5. Se si utilizza ancora ASP.NET versione 2.0, usare il [ `Array.IndexOf(Of String)` metodo](https://msdn.microsoft.com/library/eha9t187.aspx) invece.


Il `CheckRolesForSelectedUser` (metodo) deve essere chiamato in due casi: quando la pagina viene caricata e ogni volta che il `UserList` indice selezionato del DropDownList viene modificato. Pertanto, chiamare questo metodo il `Page_Load` gestore dell'evento (dopo le chiamate a `BindUsersToUserList` e `BindRolesToList`). Inoltre, creare un gestore eventi per il controllo DropDownList `SelectedIndexChanged` evento e il metodo viene chiamato da tale posizione.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample8.vb)]

Con questo codice, è possibile testare la pagina tramite il browser. Tuttavia, poiché il `UsersAndRoles.aspx` pagina manca attualmente la possibilità di assegnare utenti a ruoli, nessun utente dispone dei ruoli. Verrà creata l'interfaccia per l'assegnazione di utenti ai ruoli in proposito, pertanto è possibile richiedere la parola che questo codice funziona e verificare che esegue l'operazione in un secondo momento oppure è possibile aggiungere manualmente utenti ai ruoli per l'inserimento di record di `aspnet_UsersInRoles` tabella per testare questo functi onality ora.

### <a name="assigning-and-removing-users-from-roles"></a>L'assegnazione e rimuovendo utenti dai ruoli

Quando il visitatore seleziona o deseleziona una casella di controllo nel `UsersRoleList` Ripetitore è necessario aggiungere o rimuovere l'utente selezionato dal ruolo corrispondente. La casella di controllo `AutoPostBack` proprietà è impostata su True, che provoca un postback in qualsiasi momento nel Repeater una casella di controllo è selezionata o deselezionata. In breve, è necessario creare un gestore eventi per la casella di controllo `CheckChanged` evento. Poiché la casella di controllo si trova in un controllo Repeater, è necessario aggiungere manualmente l'impianto di gestore di evento. Per iniziare, aggiungere il gestore dell'evento per la classe code-behind come un `Protected` (metodo), come illustrato di seguito:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample9.vb)]

Restituiamo per scrivere il codice per questo gestore eventi in proposito. Prima ora è necessario completare l'infrastruttura di gestione degli eventi. Dalla casella di controllo all'interno del ripetitore `ItemTemplate`, aggiungere `OnCheckedChanged="RoleCheckBox_CheckChanged"`. Questa sintassi collega il `RoleCheckBox_CheckChanged` gestore eventi per il `RoleCheckBox`del `CheckedChanged` evento.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample10.aspx)]

L'attività finale consiste nell'eseguire il `RoleCheckBox_CheckChanged` gestore dell'evento. È necessario avviare facendo riferimento il controllo casella di controllo che ha generato l'evento perché questa istanza casella di controllo indica il ruolo è stata selezionata o deselezionata tramite il relativo `Text` e `Checked` proprietà. Queste informazioni con il nome utente dell'utente selezionato è aggiungere o rimuovere l'utente dal ruolo tramite il `Roles` della classe [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx) o [ `RemoveUserFromRole` metodo](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx).

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample11.vb)]

Il codice sopra riportato viene avviato facendo riferimento a livello di codice la casella di controllo che ha generato l'evento, che è disponibile tramite il `sender` parametro di input. Se la casella di controllo è selezionata, l'utente selezionato viene aggiunto al ruolo specificato, in caso contrario che vengono rimossi dal ruolo. In entrambi i casi il `ActionStatus` etichetta verrà visualizzato un riepilogo l'azione eseguita.

È opportuno testare questa pagina tramite un browser. Selezionare utente Tito e quindi aggiungervi Tito supervisori sia agli amministratori di ruoli.


[![Tito è stato aggiunto per gli amministratori e ruoli supervisori](assigning-roles-to-users-vb/_static/image8.png)](assigning-roles-to-users-vb/_static/image7.png)

**Figura 3**: È stato aggiunto Tito agli amministratori e ai ruoli supervisori ([fare clic per visualizzare l'immagine ingrandita](assigning-roles-to-users-vb/_static/image9.png))


Successivamente, selezionare utente Bruce dall'elenco a discesa. È un postback e caselle di controllo del ripetitore vengono aggiornati tramite il `CheckRolesForSelectedUser`. Poiché Bruce non ancora appartiene ad alcun ruolo, le due caselle di controllo siano deselezionate. Successivamente, aggiungere Bruce al ruolo supervisori.


[![Bruce è stato aggiunto al ruolo supervisori](assigning-roles-to-users-vb/_static/image11.png)](assigning-roles-to-users-vb/_static/image10.png)

**Figura 4**: Bruce è stato aggiunto al ruolo supervisori ([fare clic per visualizzare l'immagine ingrandita](assigning-roles-to-users-vb/_static/image12.png))


Per verificare ulteriormente le funzionalità del `CheckRolesForSelectedUser` (metodo), selezionare un utente diverso Tito o Bruce. Si noti come le caselle di controllo sono deselezionate automaticamente, che indica che non appartiene ad alcun ruolo. Tornare a Tito. È necessario verificare i supervisori sia agli amministratori di caselle di controllo.

## <a name="step-2-building-the-by-roles-user-interface"></a>Passaggio 2: Creazione dell'interfaccia utente "da"ruoli

A questo punto si conclude l'interfaccia "utenti" e sono pronti per avviare l'interfaccia "da"ruoli di affrontare. L'interfaccia "da"ruoli richiesto all'utente di selezionare un ruolo da un elenco a discesa e quindi Visualizza il set di utenti che appartengono a tale ruolo in un controllo GridView.

Aggiungere un altro controllo DropDownList il `UsersAndRoles.aspx page`. Inserire questo uno sotto il controllo Repeater, denominarlo `RoleList`e impostare il relativo `AutoPostBack` proprietà su True. Sotto, aggiungere un controllo GridView e denominarla `RolesUserList`. Questo controllo GridView verranno elencati gli utenti che appartengono al ruolo selezionato. Impostare il controllo GridView `AutoGenerateColumns` la proprietà su False, aggiungere un TemplateField alla griglia `Columns` insieme e impostare il relativo `HeaderText` proprietà per "Utenti". Definire il TemplateField `ItemTemplate` in modo che venga visualizzato il valore dell'espressione di associazione dati `Container.DataItem` nel `Text` proprietà di un'etichetta denominata `UserNameLabel`.

Dopo l'aggiunta e configurazione di GridView, markup dichiarativo dell'interfaccia "dal ruolo" dovrebbe essere simile al seguente:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample12.aspx)]

È necessario popolare il `RoleList` DropDownList con il set di ruoli del sistema. A tale scopo, aggiornare il `BindRolesToList` metodo in modo che associa la matrice di stringhe restituito dal `Roles.GetAllRoles` metodo il `RolesList` DropDownList (così come il `UsersRoleList` Ripetitore).

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample13.vb)]

Le ultime due righe nel `BindRolesToList` metodo sono stati aggiunti al set di ruoli per associare il `RoleList` controllo DropDownList. Figura 5 mostra il risultato finale, quando viene visualizzato tramite un browser: un elenco a discesa popolato con i ruoli del sistema.


[![I ruoli vengono visualizzati in RoleList DropDownList](assigning-roles-to-users-vb/_static/image14.png)](assigning-roles-to-users-vb/_static/image13.png)

**Figura 5**: ruoli di vengono visualizzati nel `RoleList` DropDownList ([fare clic per visualizzare l'immagine ingrandita](assigning-roles-to-users-vb/_static/image15.png))


### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>Visualizzare gli utenti che appartengono al ruolo selezionato

Quando la pagina viene caricata, oppure quando è selezionato un nuovo ruolo di `RoleList` DropDownList, è necessario visualizzare l'elenco di utenti che appartengono a tale ruolo in GridView. Creare un metodo denominato `DisplayUsersBelongingToRole` utilizzando il codice seguente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample14.vb)]

Questo metodo avvia ottenendo il ruolo selezionato dal `RoleList` DropDownList. Viene quindi utilizzato il [ `Roles.GetUsersInRole(roleName)` metodo](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx) per recuperare una matrice di stringhe di nomi utente degli utenti che appartengono a tale ruolo. Questa matrice viene quindi associata al `RolesUserList` GridView.

Questo metodo deve essere chiamato in due casi: quando la pagina viene caricata inizialmente e quando il ruolo selezionato nel `RoleList` DropDownList modifiche. Di conseguenza, aggiornare il `Page_Load` gestore dell'evento in modo che questo metodo viene richiamato dopo la chiamata a `CheckRolesForSelectedUser`. Successivamente, creare un gestore eventi per il `RoleList`del `SelectedIndexChanged` evento e il metodo viene chiamato da qui, troppo.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample15.vb)]

Con questo codice nella posizione, la `RolesUserList` GridView deve visualizzare gli utenti che appartengono al ruolo selezionato. Come illustrato nella figura 6, il ruolo di supervisori è costituito da due membri: Bruce e Tito.


[![GridView Elenca gli utenti che appartengono al ruolo selezionato](assigning-roles-to-users-vb/_static/image17.png)](assigning-roles-to-users-vb/_static/image16.png)

**Figura 6**: il GridView sono elencati quelli gli utenti che appartengono al ruolo selezionato ([fare clic per visualizzare l'immagine ingrandita](assigning-roles-to-users-vb/_static/image18.png))


### <a name="removing-users-from-the-selected-role"></a>Rimozione di utenti dal ruolo selezionato

Consente di aumentare la `RolesUserList` GridView in modo che includa una colonna di "Rimuovi" pulsanti. Fare clic sul pulsante "Rimuovi" per un determinato utente verranno rimossi da tale ruolo.

Per iniziare, aggiungere un campo pulsante Elimina a GridView. Rendere questo campo viene visualizzato come archiviato più a sinistra e modificare il relativo `DeleteText` proprietà "Rimuovere" da "Delete" (predefinito).


[![Aggiungere il](assigning-roles-to-users-vb/_static/image20.png)](assigning-roles-to-users-vb/_static/image19.png)

**Figura 7**: aggiungere il pulsante "Rimuovi" a GridView ([fare clic per visualizzare l'immagine ingrandita](assigning-roles-to-users-vb/_static/image21.png))


Quando si fa clic sul pulsante "Rimuovi" viene utilizzata un postback e il controllo GridView `RowDeleting` viene generato l'evento. È necessario creare un gestore eventi per questo evento e scrivere codice che l'utente è stato rimosso dal ruolo selezionato. Creare il gestore dell'evento e quindi aggiungere il codice seguente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample16.vb)]

Il codice avvia determinando il nome del ruolo selezionato. Viene quindi a livello di codice i riferimenti di `UserNameLabel` controllo dalla riga di cui è stato scelto il cui pulsante "Rimuovi" per determinare il nome utente dell'utente da rimuovere. L'utente viene quindi rimosso dal ruolo tramite una chiamata al `Roles.RemoveUserFromRole` metodo. Il `RolesUserList` GridView viene aggiornata e viene visualizzato un messaggio tramite il `ActionStatus` controllo etichetta.

> [!NOTE]
> Il pulsante "Rimuovi" non è necessaria una sorta di conferma da parte dell'utente prima di rimuovere l'utente dal ruolo. Invito di aggiungere un livello di conferma dell'utente. Uno dei modi più semplici per confermare un'azione avviene tramite una finestra di dialogo di conferma sul lato client. Per ulteriori informazioni su questa tecnica, vedere [aggiunta sul lato Client conferma quando l'eliminazione di](https://asp.net/learn/data-access/tutorial-42-vb.aspx).


Figura 8 mostra la pagina dopo Tito utente è stato rimosso dal gruppo supervisori.


[![Purtroppo Tito non è più un supervisore](assigning-roles-to-users-vb/_static/image23.png)](assigning-roles-to-users-vb/_static/image22.png)

**Figura 8**: Purtroppo Tito non è più un supervisore ([fare clic per visualizzare l'immagine ingrandita](assigning-roles-to-users-vb/_static/image24.png))


### <a name="adding-new-users-to-the-selected-role"></a>Aggiunta di nuovi utenti al ruolo selezionato

Con la rimozione di utenti dal ruolo selezionato, il visitatore in questa pagina deve essere inoltre in grado di aggiungere un utente al ruolo selezionato. L'interfaccia migliore per l'aggiunta di un utente al ruolo selezionato dipende dal numero di account utente, che si prevede di avere. Se il sito Web conterrà solo alcuni account utente di decine o meno, è possibile utilizzare un DropDownList qui. Se potrebbero essere presenti migliaia di account utente, si desidera includere un'interfaccia utente che consente il visitatore per spostarsi tra gli account, cercare un account specifico, o filtrare gli account utente in qualche altro modo.

Per questa pagina consente di usare un'interfaccia molto semplice che funziona indipendentemente dal numero di account utente nel sistema. In particolare, si utilizzerà una casella di testo, che richiede il visitatore di digitare il nome utente dell'utente che desidera aggiungere al ruolo selezionato. Se non esiste alcun utente con lo stesso nome, o se l'utente è già un membro del ruolo, si verrà visualizzato un messaggio in `ActionStatus` etichetta. Tuttavia, se l'utente esiste e non è un membro del ruolo, si verrà aggiunti al ruolo e aggiornare la griglia.

Aggiungere una casella di testo e un pulsante sotto il controllo GridView. Impostare la casella di testo `ID` a `UserNameToAddToRole` e impostare il pulsante `ID` e `Text` proprietà `AddUserToRoleButton` e "Aggiunta al ruolo utente", rispettivamente.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample17.aspx)]

Creare quindi un `Click` gestore eventi per il `AddUserToRoleButton` e aggiungere il codice seguente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample18.vb)]

La maggior parte del codice di `Click` gestore esegue vari controlli di convalida. Assicura che il visitatore specificato un nome utente nel `UserNameToAddToRole` casella di testo, che l'utente esiste nel sistema e che non appartengono già al ruolo selezionato. Se uno di questi controlli ha esito negativo, viene visualizzato un messaggio appropriato `ActionStatus` e il gestore dell'evento è stato terminato. Se tutti i controlli di esito positivo, l'utente viene aggiunto al ruolo tramite il `Roles.AddUserToRole` metodo. In seguito, la casella di testo del `Text` proprietà è stata rimossa, GridView viene aggiornato e `ActionStatus` etichetta viene visualizzato un messaggio che indica che l'utente specificato è stato aggiunto al ruolo selezionato.

> [!NOTE]
> Per garantire che l'utente specificato non appartiene già al ruolo selezionato, viene usato il [ `Roles.IsUserInRole(userName, roleName)` metodo](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx), che restituisce un valore booleano che indica se *userName* è un membro di *roleName*. Si utilizzerà questo metodo nuovamente nel <a id="_msoanchor_2"> </a> [esercitazione successiva](role-based-authorization-vb.md) quando si esamina l'autorizzazione basata sui ruoli.


Visitare la pagina tramite un browser e selezionare il ruolo supervisori dal `RoleList` DropDownList. Provare a immettere un nome utente non valido – verrà visualizzato un messaggio che indica che l'utente non esiste nel sistema.


[![È possibile aggiungere un utente Non esistente a un ruolo](assigning-roles-to-users-vb/_static/image26.png)](assigning-roles-to-users-vb/_static/image25.png)

**Figura 9**: È possibile aggiungere un utente Non esistente a un ruolo ([fare clic per visualizzare l'immagine ingrandita](assigning-roles-to-users-vb/_static/image27.png))


Ora provare ad aggiungere un utente valido. Vado avanti e aggiungere nuovamente Tito al ruolo supervisori.


[![Tito è ancora una volta un supervisore!](assigning-roles-to-users-vb/_static/image29.png)](assigning-roles-to-users-vb/_static/image28.png)

**Figura 10**: Tito è ancora una volta un supervisore!  ([Fare clic per visualizzare l'immagine ingrandita](assigning-roles-to-users-vb/_static/image30.png))


## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>Passaggio 3: Cross-l'aggiornamento di "User" e "Dal ruolo" interfacce

Il `UsersAndRoles.aspx` nella pagina sono disponibili due interfacce distinte per la gestione di utenti e ruoli. Attualmente, queste due interfacce agiscono indipendentemente uno da altro, pertanto è possibile che una modifica apportata in un'unica interfaccia non si rifletteranno immediatamente negli altri. Ad esempio, si supponga che il visitatore alla pagina Seleziona il ruolo di supervisori dal `RoleList` DropDownList, in cui sono elencati Bruce e Tito come relativi membri. Successivamente, il visitatore seleziona Tito dal `UserList` DropDownList, che archivia le caselle di controllo di amministratori e i supervisori il `UsersRoleList` Repeater. Se il visitatore deseleziona quindi il ruolo di Supervisore da Ripetitore, Tito viene rimosso dal ruolo supervisori, ma questa modifica non viene riflessa nell'interfaccia "dal ruolo". GridView risulterà comunque Tito come un membro del ruolo supervisori.

Per risolvere il problema è necessario aggiornare GridView ogni volta che un ruolo viene selezionato o deselezionato dal `UsersRoleList` Repeater. Analogamente, è necessario aggiornare Ripetitore ogni volta che un utente viene rimosso o aggiunto a un ruolo dall'interfaccia "dal ruolo".

Ripetitore nell'interfaccia "dall'utente" viene aggiornato mediante la chiamata di `CheckRolesForSelectedUser` metodo. L'interfaccia "dal ruolo" può essere modificato nel `RolesUserList` GridView `RowDeleting` gestore dell'evento e `AddUserToRoleButton` del pulsante `Click` gestore dell'evento. Pertanto, è necessario chiamare il `CheckRolesForSelectedUser` metodo da ognuno di questi metodi.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample19.vb)]

Analogamente, nell'interfaccia "dal ruolo" GridView viene aggiornato chiamando il `DisplayUsersBelongingToRole` (metodo) e l'interfaccia "dall'utente" viene modificata tramite il `RoleCheckBox_CheckChanged` gestore dell'evento. Pertanto, è necessario chiamare il `DisplayUsersBelongingToRole` metodo da questo gestore eventi.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample20.vb)]

Con queste modifiche, codice secondario di "user" e "dal ruolo" interfacce ora correttamente tra l'aggiornamento. Per verificarlo, visitare la pagina tramite un browser e selezionare Tito e supervisori dal `UserList` e `RoleList` controlli DropDownList, rispettivamente. Si noti che quando si deseleziona il ruolo supervisori per Tito da Ripetitore nell'interfaccia "dall'utente", Tito viene rimosso automaticamente dal controllo GridView nell'interfaccia "dal ruolo". Aggiunta di Tito nuovamente al ruolo supervisori dall'interfaccia "dal ruolo" automaticamente controlla nuovamente la casella di controllo supervisori nell'interfaccia "dall'utente".

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>Passaggio 4: Personalizzazione CreateUserWizard per includere un passaggio "Specificare ruoli"

Nel <a id="_msoanchor_3"> </a> [ *creazione degli account utente* ](../membership/creating-user-accounts-vb.md) esercitazione è stato illustrato come utilizzare il controllo CreateUserWizard Web per fornire un'interfaccia per la creazione di un nuovo account utente. Il controllo CreateUserWizard può essere utilizzato in uno dei due modi:

- Come mezzo per i visitatori creare i propri account utente del sito, e
- Come gli amministratori di creare nuovi account

Nel primo caso, usare un visitatore viene fornito per il sito e compila CreateUserWizard, immettere le informazioni per registrare nel sito. Nel secondo caso, un amministratore crea un nuovo account per un altro utente.

Quando viene creato un account da un amministratore di un'altra persona, potrebbe essere utile consentire all'amministratore di specificare i ruoli a cui appartiene il nuovo account utente. Nel <a id="_msoanchor_4"> </a> [ *archiviazione* *informazioni utente aggiuntive* ](../membership/storing-additional-user-information-vb.md) esercitazione è stato illustrato come personalizzare CreateUserWizard aggiungendo ulteriori `WizardSteps`. Vengono descritte le modalità aggiungere un passaggio aggiuntivo a CreateUserWizard per specificare i nuovi ruoli dell'utente.

Aprire il `CreateUserWizardWithRoles.aspx` pagina e aggiungere un controllo CreateUserWizard denominato `RegisterUserWithRoles`. Impostare il controllo `ContinueDestinationPageUrl` proprietà su "~ / Default.aspx". Poiché l'idea è che un amministratore utilizzerà questo controllo CreateUserWizard per creare nuovi account utente, impostare il controllo `LoginCreatedUser` la proprietà su False. Questo `LoginCreatedUser` proprietà specifica se il visitatore viene automaticamente connesso come l'utente appena creato e il valore predefinito è True. È impostato su False poiché quando un amministratore crea un nuovo account a cui si desidera mantenere quest'ultimo accesso eseguito come se stesso.

Selezionare quindi la "Aggiungi/Rimuovi `WizardSteps`..." opzione Smart Tag del CreateUserWizard e aggiungere un nuovo `WizardStep`, impostando il relativo `ID` a `SpecifyRolesStep`. Spostare il `SpecifyRolesStep WizardStep` in modo da posizionarla dopo il passaggio "Sign Up for Your New Account", ma prima del passaggio "Completa". Impostare il `WizardStep`del `Title` proprietà su "Specificare ruoli" relativo `StepType` proprietà `Step`e il relativo `AllowReturn` la proprietà su False.


[![Aggiungere il](assigning-roles-to-users-vb/_static/image32.png)](assigning-roles-to-users-vb/_static/image31.png)

**Figura 11**: aggiungere "Specificare ruoli" `WizardStep` a CreateUserWizard ([fare clic per visualizzare l'immagine ingrandita](assigning-roles-to-users-vb/_static/image33.png))


Dopo questa modifica markup dichiarativo del CreateUserWizard dovrebbe essere simile al seguente:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample21.aspx)]

"Specificare ruoli" `WizardStep`, aggiungere un controllo CheckBoxList denominato `RoleList.` CheckBoxList questo verranno elencati i ruoli disponibili, abilitare l'utente visita la pagina verificare quali ruoli utente appena creato appartiene.

Ancora due attività di codifica: è necessario innanzitutto popolare il `RoleList` CheckBoxList con i ruoli del sistema; in secondo luogo, è necessario aggiungere l'utente creato per i ruoli selezionati quando l'utente sposta il passaggio "Specificare ruoli" con il passaggio "Completa". È possibile eseguire la prima attività di `Page_Load` gestore dell'evento. I seguente codice a livello di codice fa riferimento il `RoleList` casella di controllo nella prima visita la pagina e i ruoli del sistema esegue l'associazione.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample22.vb)]

Il codice sopra riportato dovrebbe essere noti. Nel <a id="_msoanchor_5"> </a> [ *archiviazione* *informazioni utente aggiuntive* ](../membership/storing-additional-user-information-vb.md) esercitazione è stata utilizzata due `FindControl` istruzioni per fare riferimento a un controllo Web all'interno di un oggetto personalizzato `WizardStep`. E il codice che associa i ruoli CheckBoxList è tratto dal precedentemente in questa esercitazione.

Per eseguire la seconda attività di programmazione, che è necessario sapere quando il passaggio "Specificare ruoli" è stato completato. Tenere presente che CreateUserWizard ha un `ActiveStepChanged` evento, che viene generato ogni volta che la navigazione da un passaggio a un altro. Qui è possibile determinare se l'utente ha raggiunto il passaggio "Completa". In questo caso, è necessario aggiungere l'utente ai ruoli selezionati.

Creare un gestore eventi per il `ActiveStepChanged` eventi e aggiungere il codice seguente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample23.vb)]

Se l'utente ha raggiunto solo il passaggio "Completed", il gestore dell'evento enumera gli elementi del `RoleList` CheckBoxList e l'utente appena creato viene assegnato ai ruoli selezionati.

Visitare questa pagina tramite un browser. Il primo passaggio CreateUserWizard è il passaggio "Sign Up for Your New Account" standard, quale richiesto per il nuovo nome dell'utente, password, messaggio di posta elettronica e altre informazioni sulla chiave. Immettere le informazioni per creare un nuovo utente denominato Wanda.


[![Creare un nuovo utente denominato Wanda](assigning-roles-to-users-vb/_static/image35.png)](assigning-roles-to-users-vb/_static/image34.png)

**Figura 12**: creare un nuovo Wanda denominato di utente ([fare clic per visualizzare l'immagine ingrandita](assigning-roles-to-users-vb/_static/image36.png))


Fare clic sul pulsante "Create User". CreateUserWizard chiama internamente il `Membership.CreateUser` (metodo), la creazione del nuovo account utente, quindi avanza al passaggio successivo, "specificare ruoli". Di seguito sono elencati i ruoli del sistema. Selezionare la casella di controllo supervisori e fare clic su Avanti.


[![Rendere membro del ruolo mentre per i supervisori Wanda](assigning-roles-to-users-vb/_static/image38.png)](assigning-roles-to-users-vb/_static/image37.png)

**Figura 13**: impostare Wanda come membro del ruolo mentre per i supervisori ([fare clic per visualizzare l'immagine ingrandita](assigning-roles-to-users-vb/_static/image39.png))


Fare clic su Avanti provoca un postback e gli aggiornamenti di `ActiveStep` al passaggio "Completa". Nel `ActiveStepChanged` gestore dell'evento, a cui è assegnato al ruolo supervisori l'account utente creato di recente. Per verificarlo, tornare al `UsersAndRoles.aspx` pagina e selezionare i supervisori dal `RoleList` DropDownList. Come mostrato nella figura 14, i supervisori sono ora costituiti da tre utenti: Bruce Tito e Wanda.


[![Bruce Tito e Wanda sono tutti i supervisori](assigning-roles-to-users-vb/_static/image41.png)](assigning-roles-to-users-vb/_static/image40.png)

**Figura 14**: Bruce Tito e Wanda sono tutti i supervisori ([fare clic per visualizzare l'immagine ingrandita](assigning-roles-to-users-vb/_static/image42.png))


## <a name="summary"></a>Riepilogo

Il framework di ruoli offre i metodi per recuperare informazioni sui ruoli e i metodi per determinare gli utenti che appartengono a un ruolo specificato di un determinato utente. Inoltre, esistono diversi metodi per l'aggiunta e rimozione di uno o più utenti a uno o più ruoli. In questa esercitazione è incentrata su solo due di questi metodi: `AddUserToRole` e `RemoveUserFromRole`. Sono disponibili ulteriori scenari progettati per aggiungere più utenti di un singolo ruolo e assegnare più ruoli a un singolo utente.

Questa esercitazione è inclusa anche un'occhiata a si estende il controllo CreateUserWizard per includere un `WizardStep` per specificare i ruoli dell'utente appena creato. Un passaggio di questo tipo può contribuire a semplificare il processo di creazione di account utente per i nuovi utenti amministratore.

A questo punto abbiamo visto come creare ed eliminare i ruoli e su come aggiungere e rimuovere utenti dai ruoli. Ma non è stato ancora esaminare l'applicazione di autorizzazione basata sui ruoli. Nel <a id="_msoanchor_6"> </a> [seguenti esercitazione](role-based-authorization-vb.md) verranno esaminati la definizione di regole di autorizzazione URL del ruolo per ruolo, nonché come e limitarne la funzionalità a livello di pagina in base ai ruoli dell'utente attualmente connesso.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Panoramica dello strumento Amministrazione sito Web ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx)
- [Esame ASP. Appartenenza, ruoli e profilo di rete](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [In sequenza il proprio strumento Amministrazione sito Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è  *[SAM insegna manualmente ASP.NET 2.0 nelle 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott può essere raggiunto al [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali...

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata Teresa Murphy. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](creating-and-managing-roles-vb.md)
> [Successivo](role-based-authorization-vb.md)
