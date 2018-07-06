---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-cs
title: Assegnazione di ruoli agli utenti (c#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione creeremo due pagine ASP.NET per agevolare la gestione degli utenti appartengono a quali ruoli. La prima pagina includerà le funzionalità per vedere quali...
ms.author: aspnetcontent
ms.date: 03/24/2008
ms.assetid: d522639a-5aca-421e-9a76-d73f95607f57
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-cs
msc.type: authoredcontent
ms.openlocfilehash: f8573afc2fd5f12611f88f8bdad7e14389017808
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820130"
---
<a name="assigning-roles-to-users-c"></a>Assegnazione di ruoli agli utenti (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.10.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_cs.pdf)

> In questa esercitazione creeremo due pagine ASP.NET per agevolare la gestione degli utenti appartengono a quali ruoli. La prima pagina includerà le funzionalità per vedere quali utenti appartengono a un determinato ruolo, i ruoli a cui appartiene un utente specifico e la possibilità di assegnare o rimuovere un utente specifico da un particolare ruolo. Nella seconda pagina aumenterà il controllo CreateUserWizard in modo che includa un passaggio per specificare i ruoli a cui appartiene l'utente appena creato. Ciò è utile negli scenari in cui è in grado di creare nuovi account utente amministratore.


## <a name="introduction"></a>Introduzione

Il <a id="_msoanchor_1"> </a> [esercitazione precedente](creating-and-managing-roles-cs.md) esaminato il framework di ruoli e le `SqlRoleProvider`; è stato illustrato come utilizzare il `Roles` classe da creare, recuperare ed eliminare ruoli. Oltre alla creazione ed eliminazione di ruoli, è necessario essere in grado di assegnare o rimuovere utenti da un ruolo. Purtroppo, ASP.NET non viene fornito con tutti i controlli Web per la gestione di ciò che gli utenti appartengono a quali ruoli. In alternativa, è necessario creare proprie pagine ASP.NET per la gestione di queste associazioni. La buona notizia è che l'aggiunta e rimozione di utenti ai ruoli è piuttosto semplice. Il `Roles` classe contiene numerosi metodi per l'aggiunta di uno o più utenti a uno o più ruoli.

In questa esercitazione creeremo due pagine ASP.NET per agevolare la gestione degli utenti appartengono a quali ruoli. La prima pagina includerà le funzionalità per vedere quali utenti appartengono a un determinato ruolo, i ruoli a cui appartiene un utente specifico e la possibilità di assegnare o rimuovere un utente specifico da un particolare ruolo. Nella seconda pagina aumenterà il controllo CreateUserWizard in modo che includa un passaggio per specificare i ruoli a cui appartiene l'utente appena creato. Ciò è utile negli scenari in cui è in grado di creare nuovi account utente amministratore.

Iniziamo!

## <a name="listing-what-users-belong-to-what-roles"></a>Elenca gli utenti che appartengono a quali ruoli

Il primo ordine di importanza per questa esercitazione consiste nel creare una pagina web da cui gli utenti possono essere assegnati ai ruoli. Prima che si sono relative a noi con le procedure per assegnare gli utenti a ruoli, è possibile concentrare l'attenzione su come determinare quali utenti appartengono a quali ruoli. Esistono due modi per visualizzare queste informazioni: "dal ruolo" o ""utente. È possibile consentire il visitatore selezionare un ruolo e quindi visualizzarli tutti gli utenti che appartengono al ruolo (visualizzazione "dal ruolo") o è stato possibile richiedere il visitatore per selezionare un utente e quindi visualizzarli i ruoli assegnati all'utente (visualizzazione "dall'utente").

La visualizzazione "dal ruolo" è utile nei casi in cui vuole conoscere il set di utenti che appartengono a un ruolo specifico; il visitatore la visualizzazione "dall'utente" è ideale quando i visitatori devono conoscere i ruoli di un utente specifico. È possibile che la pagina includa sia "role" e "dall'utente" interfacce.

Si inizierà con la creazione dell'interfaccia "dall'utente". Questa interfaccia sarà costituito da un elenco di riepilogo e un elenco di caselle di controllo. L'elenco a discesa viene popolato con il set di utenti nel sistema. le caselle di controllo permette di enumerare i ruoli. Selezionando un utente dall'elenco a discesa controllerà questi ruoli a cui che appartiene l'utente. La persona visitando la pagina può quindi selezionare o deselezionare le caselle di controllo per aggiungere o rimuovere l'utente selezionato dalla ruoli corrispondenti.

> [!NOTE]
> Usando un elenco a discesa elenco all'account utente non è la scelta ideale per siti Web in cui possono essere presenti centinaia di account utente. Un elenco di riepilogo a discesa è progettato per consentire all'utente di selezionare un elemento da un relativamente breve elenco di opzioni. Diventa rapidamente scomodo man mano che aumenta il numero di elementi dell'elenco. Se si sta creando un sito Web che avrà un numero potenzialmente elevato di account utente, è possibile provare a usare un'interfaccia utente alternative, ad esempio un controllo GridView paginabile o un'interfaccia filtrabile che elenca richiede il visitatore per scegliere una lettera e quindi solo Mostra gli utenti il cui nome utente inizi con la lettera selezionata.


## <a name="step-1-building-the-by-user-user-interface"></a>Passaggio 1: Creazione dell'interfaccia utente "Dall'utente"

Aprire il `UsersAndRoles.aspx` pagina. Nella parte superiore della pagina, aggiungere un controllo etichetta Web denominato `ActionStatus` e cancellare il `Text` proprietà. Si userà l'etichetta per fornire commenti e suggerimenti sulle azioni eseguite, la visualizzazione dei messaggi, ad esempio, "Tito utente è stato aggiunto al ruolo Administrators" o "Jisun utente è stato rimosso dal ruolo supervisori." Per rendere questi sono i messaggi, impostare l'etichetta `CssClass` proprietà su "Important".

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample1.aspx)]

Successivamente, aggiungere la seguente definizione di classe CSS per il `Styles.css` foglio di stile:

[!code-css[Main](assigning-roles-to-users-cs/samples/sample2.css)]

Questa definizione CSS indica al browser per visualizzare l'etichetta con un tipo di carattere di grandi dimensioni, di colore rosso. Figura 1 mostra questo effetto tramite la progettazione di Visual Studio.


[![La proprietà dell'etichetta CssClass comporta un tipo di carattere rosso e di grandi dimensioni](assigning-roles-to-users-cs/_static/image2.png)](assigning-roles-to-users-cs/_static/image1.png)

**Figura 1**: l'etichetta `CssClass` proprietà genererà una grande, tipo di carattere rosso ([fare clic per visualizzare l'immagine con dimensioni normali](assigning-roles-to-users-cs/_static/image3.png))


Successivamente, aggiungere un controllo DropDownList alla pagina, impostare relativi `ID` proprietà `UserList`e impostare il `AutoPostBack` proprietà su True. Si userà questo DropDownList per elencare tutti gli utenti nel sistema. Questo controllo DropDownList verrà associata a una raccolta di oggetti MembershipUser. Dal momento che il controllo DropDownList per visualizzare la proprietà UserName dell'oggetto MembershipUser (e usarlo come valore degli elementi dell'elenco), impostare il controllo DropDownList `DataTextField` e `DataValueField` proprietà su "UserName".

Sotto il controllo DropDownList, aggiungere un controllo Repeater denominato `UsersRoleList`. Il Repeater elencherà tutti i ruoli del sistema come una serie di caselle di controllo. Definire il Repeater `ItemTemplate` utilizzando il seguente markup dichiarativo:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample3.aspx)]

Il `ItemTemplate` markup include un singolo controllo casella di controllo Web denominato `RoleCheckBox`. La casella di controllo `AutoPostBack` è impostata su True e il `Text` proprietà è associata a `Container.DataItem`. L'obiettivo è semplicemente la sintassi di associazione dati `Container.DataItem` , infatti, il framework di ruoli restituisce l'elenco di nomi di ruolo come una matrice di stringhe ed è questa matrice di stringhe che eseguiremo l'associazione al controllo Repeater. Una descrizione dettagliata del motivo per cui questa sintassi consente di visualizzare il contenuto della matrice associata a un controllo Web esula dall'ambito di questa esercitazione. Per altre informazioni al riguardo, consultare [associazione di una matrice scalare a un controllo Web](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

Markup dichiarativo dell'interfaccia "dall'utente" a questo punto dovrebbe essere simile al seguente:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample4.aspx)]

A questo punto siamo pronti a scrivere il codice per associare il set di account utente a DropDownList e il set di ruoli a Repeater. Nella classe code-behind della pagina, aggiungere un metodo denominato `BindUsersToUserList` e un altro denominato `BindRolesList`, usando il codice seguente:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample5.cs)]

Il `BindUsersToUserList` metodo consente di recuperare tutti gli account utente nel sistema tramite il [ `Membership.GetAllUsers` metodo](https://msdn.microsoft.com/library/dy8swhya.aspx). Restituisce un [ `MembershipUserCollection` oggetto](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx), che è una raccolta di [ `MembershipUser` istanze](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx). Questa raccolta viene quindi associata ai `UserList` DropDownList. Il `MembershipUser` istanze di tale struttura la raccolta contiene una varietà di proprietà, ad esempio `UserName`, `Email`, `CreationDate`, e `IsOnline`. Per indicare a DropDownList per visualizzare il valore della `UserName` proprietà, assicurarsi che il `UserList` del controllo DropDownList `DataTextField` e `DataValueField` proprietà sono state impostate su "UserName".

> [!NOTE]
> Il `Membership.GetAllUsers` metodo presenta due overload: uno che non accetta alcun parametro di input e restituisce tutti gli utenti e uno che accetta valori interi per l'indice della pagina e dimensioni di pagina e restituisce solo il subset di utenti specificato. Quando sono presenti grandi quantità di account utente visualizzato in un elemento dell'interfaccia utente paginabile, il secondo overload è utilizzabile per pagina in modo più efficiente attraverso gli utenti poiché restituisce solo il subset preciso di account utente anziché a tutti gli elementi.


Il `BindRolesToList` metodo inizia chiamando le `Roles` della classe [ `GetAllRoles` metodo](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx), che restituisce una matrice di stringhe contenente i ruoli del sistema. Questa matrice di stringhe viene quindi associata a Repeater.

Infine, dobbiamo chiamare questi due metodi, quando la pagina viene caricata. Aggiungere il codice seguente al gestore eventi `Page_Load`:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample6.cs)]

Con questo codice, si consiglia di visitare la pagina tramite un browser. la schermata dovrebbe essere simile alla figura 2. Tutti gli account utente vengono popolati nell'elenco a discesa elenco e, di sotto, ogni ruolo viene visualizzato come una casella di controllo. Perché lo abbiamo impostato il `AutoPostBack` proprietà del controllo DropDownList e caselle di controllo su True, modifica utente selezionato o selezionare o deselezionare un ruolo determina un postback. Viene eseguita alcuna azione, tuttavia, poiché è ancora necessario scrivere codice per gestire queste azioni. Che verranno affrontati queste attività in due sezioni successive.


[![La pagina Visualizza gli utenti e ruoli](assigning-roles-to-users-cs/_static/image5.png)](assigning-roles-to-users-cs/_static/image4.png)

**Figura 2**: la pagina Visualizza gli utenti e ruoli ([fare clic per visualizzare l'immagine con dimensioni normali](assigning-roles-to-users-cs/_static/image6.png))


### <a name="checking-the-roles-the-selected-user-belongs-to"></a>Controllare i ruoli selezionato appartiene l'utente

Quando la pagina viene caricata o ogni volta che il visitatore consente di selezionare un nuovo utente nell'elenco a discesa, è necessario aggiornare il `UsersRoleList`di caselle di controllo in modo che un determinato ruolo casella di controllo solo se l'utente selezionato appartiene a tale ruolo. A tale scopo, creare un metodo denominato `CheckRolesForSelectedUser` con il codice seguente:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample7.cs)]

Il codice sopra riportato viene avviato, determinando l'utente selezionato. Quindi utilizza la classe di ruoli [ `GetRolesForUser(userName)` metodo](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx) restituire set di ruoli come una matrice di stringhe dell'utente specificato. Successivamente, vengono enumerati gli elementi del controllo Repeater e ogni elemento `RoleCheckBox` casella di controllo a livello di codice viene fatto riferimento. La casella di controllo è selezionata solo se il ruolo corrisponde a è contenuto all'interno di `selectedUsersRoles` matrice di stringhe.

> [!NOTE]
> Il `selectedUserRoles.Contains<string>(...)` sintassi non verrà compilato se si usa ASP.NET versione 2.0. Il `Contains<string>` metodo fa parte il [libreria LINQ](http://en.wikipedia.org/wiki/Language_Integrated_Query), che è una novità di ASP.NET 3.5. Se si sta ancora usando ASP.NET versione 2.0, usare il [ `Array.IndexOf<string>` metodo](https://msdn.microsoft.com/library/eha9t187.aspx) invece.


Il `CheckRolesForSelectedUser` metodo deve essere chiamata in due casi: quando la pagina viene caricata e ogni volta che il `UserList` indice selezionato del controllo DropDownList è stato modificato. Pertanto, chiamare questo metodo dal `Page_Load` gestore dell'evento (dopo le chiamate a `BindUsersToUserList` e `BindRolesToList`). Inoltre, creare un gestore eventi per il controllo DropDownList `SelectedIndexChanged` eventi e chiamare il metodo da tale posizione.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample8.cs)]

Con questo codice, è possibile testare la pagina tramite il browser. Tuttavia, poiché il `UsersAndRoles.aspx` pagina attualmente non ha la possibilità di assegnare utenti a ruoli, gli utenti non hanno i ruoli. Verrà creata l'interfaccia per l'assegnazione di utenti ai ruoli in qualche secondo, in modo che è possibile sfruttare la parola che questo codice funziona e verificare che esegue l'operazione in un secondo momento, oppure è possibile aggiungere manualmente utenti ai ruoli mediante l'inserimento di record nel `aspnet_UsersInRoles` tabella per testare questo functi a questo punto onality.

### <a name="assigning-and-removing-users-from-roles"></a>Assegnazione e rimuovendo utenti dai ruoli

Quando il visitatore controlla o deseleziona una casella di controllo nel `UsersRoleList` Repeater, dobbiamo aggiungere o rimuovere l'utente selezionato dal ruolo corrispondente. La casella di controllo `AutoPostBack` viene attualmente impostata su True, che determina un postback in qualsiasi momento una casella di controllo nel Repeater è selezionato o deselezionato. In breve, è necessario creare un gestore eventi per la casella di controllo `CheckChanged` evento. Poiché la casella di controllo si trova in un controllo Repeater, è necessario aggiungere manualmente l'impianto di gestore dell'evento. Iniziare aggiungendo il gestore eventi per la classe code-behind come un `protected` metodo, come illustrato di seguito:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample9.cs)]

Verranno restituiti per scrivere il codice per questo gestore eventi a breve. Prima di tutto punto è necessario completare le operazioni di base di gestione degli eventi. Dalla casella di controllo all'interno di Repeater `ItemTemplate`, aggiungere `OnCheckedChanged="RoleCheckBox_CheckChanged"`. Collega questa sintassi il `RoleCheckBox_CheckChanged` gestore dell'evento per il `RoleCheckBox`del `CheckedChanged` evento.

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample10.aspx)]

L'attività finale consiste nel completare il `RoleCheckBox_CheckChanged` gestore dell'evento. È necessario iniziare facendo riferimento il controllo casella di controllo che ha generato l'evento, perché questa istanza casella di controllo per indicare quale ruolo è stato selezionato o deselezionato tramite relativi `Text` e `Checked` proprietà. Queste informazioni e il nome utente dell'utente selezionato è aggiungere o rimuovere l'utente dal ruolo tramite il `Roles` della classe [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx) oppure [ `RemoveUserFromRole` metodo](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx).

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample11.cs)]

Il codice sopra riportato viene avviato a livello di codice facendo riferimento alla casella di controllo che ha generato l'evento, che è disponibile tramite il `sender` parametro di input. Se la casella di controllo è selezionata, l'utente selezionato viene aggiunto al ruolo specificato, in caso contrario, che vengono rimossi dal ruolo. In entrambi i casi il `ActionStatus` etichetta viene visualizzato un messaggio di riepilogo l'azione appena eseguita.

Si consiglia di testare questa pagina tramite un browser. Selezionare utente Tito e quindi aggiungere Tito ai supervisori sia agli amministratori di ruoli.


[![Tito è stato aggiunto per gli amministratori e ruoli supervisori](assigning-roles-to-users-cs/_static/image8.png)](assigning-roles-to-users-cs/_static/image7.png)

**Figura 3**: Tito è stato aggiunto per gli amministratori e ruoli supervisori ([fare clic per visualizzare l'immagine con dimensioni normali](assigning-roles-to-users-cs/_static/image9.png))


Selezionare quindi l'utente Bruce nell'elenco a discesa. È presente un postback e caselle di controllo del controllo Repeater vengono aggiornati tramite il `CheckRolesForSelectedUser`. Poiché Bruce ancora non appartiene ad alcun ruolo, le due caselle di controllo sono deselezionati. Aggiungere quindi Bruce al ruolo supervisori.


[![Bruce è stato aggiunto al ruolo supervisori](assigning-roles-to-users-cs/_static/image11.png)](assigning-roles-to-users-cs/_static/image10.png)

**Figura 4**: Bruce è stato aggiunto al ruolo supervisori ([fare clic per visualizzare l'immagine con dimensioni normali](assigning-roles-to-users-cs/_static/image12.png))


Per verificare ulteriormente le funzionalità del `CheckRolesForSelectedUser` (metodo), selezionare un utente diverso da Tito o Bruce. Si noti come le caselle di controllo sono automaticamente deselezionati, che indica che non appartiene ad alcun ruolo. Tornare a Tito. È necessario verificare i supervisori sia agli amministratori di caselle di controllo.

## <a name="step-2-building-the-by-roles-user-interface"></a>Passaggio 2: Creazione dell'interfaccia utente "Dai ruoli"

A questo punto si have completato l'interfaccia "dagli utenti" e pronti a iniziare affronta l'interfaccia "dai ruoli". L'interfaccia "dai ruoli" richiede all'utente di selezionare un ruolo da un elenco di riepilogo a discesa e quindi Visualizza il set di utenti che appartengono a tale ruolo in un controllo GridView.

Aggiungere un altro controllo DropDownList per il `UsersAndRoles.aspx` pagina. Posizionare il presente sotto il controllo Repeater, denominarlo `RoleList`e impostare il `AutoPostBack` proprietà su True. Di sotto, aggiungere un controllo GridView e denominarlo `RolesUserList`. Questo controllo GridView elencherà gli utenti che appartengono al ruolo selezionato. Impostare il controllo GridView `AutoGenerateColumns` la proprietà su False, aggiungere un TemplateField alla griglia `Columns` raccolta e set relativo `HeaderText` proprietà su "Utenti". Definire il TemplateField `ItemTemplate` in modo che venga visualizzato il valore dell'espressione di Data Binding `Container.DataItem` nel `Text` proprietà di un'etichetta denominata `UserNameLabel`.

Dopo l'aggiunta e configurazione di GridView, markup dichiarativo dell'interfaccia "dal ruolo" dovrebbe essere simile al seguente:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample12.aspx)]

È necessario popolare il `RoleList` DropDownList con il set di ruoli del sistema. A tale scopo, aggiornare il `BindRolesToList` metodo in modo che associa la matrice di stringhe restituito dal `Roles.GetAllRoles` metodo per il `RolesList` DropDownList (così come il `UsersRoleList` Repeater).

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample13.cs)]

Le ultime due righe nel `BindRolesToList` metodo sono state aggiunte ad per associare il set di ruoli per il `RoleList` controllo DropDownList. Figura 5 mostra il risultato finale quando viene visualizzato tramite un browser, un elenco di riepilogo a discesa popolato con i ruoli del sistema.


[![I ruoli vengono visualizzati in RoleList DropDownList](assigning-roles-to-users-cs/_static/image14.png)](assigning-roles-to-users-cs/_static/image13.png)

**Figura 5**: The ruoli vengono visualizzati nei `RoleList` DropDownList ([fare clic per visualizzare l'immagine con dimensioni normali](assigning-roles-to-users-cs/_static/image15.png))


### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>Visualizzare gli utenti che appartengono al ruolo selezionato

Quando la pagina viene caricata oppure quando è selezionato un nuovo ruolo dal `RoleList` DropDownList, è necessario visualizzare l'elenco di utenti che appartengono a tale ruolo in GridView. Creare un metodo denominato `DisplayUsersBelongingToRole` usando il codice seguente:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample14.cs)]

Il metodo inizia recuperando il ruolo selezionato dal `RoleList` DropDownList. Quindi, utilizza il [ `Roles.GetUsersInRole(roleName)` metodo](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx) per recuperare una matrice di stringhe dei nomi utente degli utenti che appartengono a tale ruolo. Questa matrice viene quindi associata ai `RolesUserList` GridView.

Questo metodo deve essere chiamata in due casi: quando la pagina viene caricata inizialmente e quando il ruolo selezionato nel `RoleList` DropDownList modifiche. Di conseguenza, aggiornare il `Page_Load` gestore dell'evento in modo che questo metodo viene richiamato dopo la chiamata a `CheckRolesForSelectedUser`. Successivamente, creare un gestore eventi per il `RoleList`del `SelectedIndexChanged` evento e questo metodo viene chiamato da qui, troppo.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample15.cs)]

Con questo codice attivo, il `RolesUserList` GridView deve visualizzare gli utenti che appartengono al ruolo selezionato. Come illustrato nella figura 6, il ruolo supervisori è costituito da due membri: Bruce e Tito.


[![Il controllo GridView sono elencati gli utenti che appartengono al ruolo selezionato](assigning-roles-to-users-cs/_static/image17.png)](assigning-roles-to-users-cs/_static/image16.png)

**Figura 6**: il GridView sono elencati quelli gli utenti che appartengono al ruolo selezionato ([fare clic per visualizzare l'immagine con dimensioni normali](assigning-roles-to-users-cs/_static/image18.png))


### <a name="removing-users-from-the-selected-role"></a>Rimozione di utenti dal ruolo selezionato

È possibile aumentare il `RolesUserList` GridView in modo che includa una colonna di "Rimuovi" pulsanti. Facendo clic sul pulsante "Rimuovi" per un particolare utente verranno rimossi da tale ruolo.

Iniziare aggiungendo un campo pulsante Delete a GridView. Impostare questo campo vengono visualizzati come più segnalato sinistra e modificare le `DeleteText` proprietà da "Delete" (predefinito) a "Remove".


[![Aggiungere il](assigning-roles-to-users-cs/_static/image20.png)](assigning-roles-to-users-cs/_static/image19.png)

**Figura 7**: aggiungere il pulsante "Rimuovi" per il controllo GridView ([fare clic per visualizzare l'immagine con dimensioni normali](assigning-roles-to-users-cs/_static/image21.png))


Quando si fa clic sul pulsante "Rimuovi" un postback previsioni e del controllo GridView `RowDeleting` viene generato l'evento. È necessario creare un gestore eventi per questo evento e scrivere il codice che consente di rimuovere l'utente dal ruolo selezionato. Creare il gestore dell'evento e quindi aggiungere il codice seguente:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample16.cs)]

Il codice avvia determinando il nome del ruolo selezionato. Viene quindi a livello di programmazione i riferimenti di `UserNameLabel` controllo dalla riga di cui è stato fatto clic sul cui pulsante "Rimuovi" per determinare il nome utente dell'utente da rimuovere. L'utente viene quindi rimosso dal ruolo tramite una chiamata verso la `Roles.RemoveUserFromRole` (metodo). Il `RolesUserList` GridView vengono quindi aggiornati e viene visualizzato un messaggio tramite la `ActionStatus` controllo etichetta.

> [!NOTE]
> Il pulsante "Rimuovi" non richiede una sorta di conferma da parte dell'utente prima di rimuovere l'utente dal ruolo. È consigliabile aggiungere un certo livello di conferma dell'utente. Uno dei modi più semplici per confermare un'azione è tramite una finestra di dialogo di conferma dal lato client. Per altre informazioni su questa tecnica, vedere [aggiunta di conferma dal lato Client quando Elimina](https://asp.net/learn/data-access/tutorial-42-cs.aspx).


Figura 8 mostra la pagina dopo che l'utente Tito è stato rimosso dal gruppo supervisori.


[![Che si verifichi Tito non è più un supervisore](assigning-roles-to-users-cs/_static/image23.png)](assigning-roles-to-users-cs/_static/image22.png)

**Figura 8**: che si verifichi Tito non è più un supervisore ([fare clic per visualizzare l'immagine con dimensioni normali](assigning-roles-to-users-cs/_static/image24.png))


### <a name="adding-new-users-to-the-selected-role"></a>Aggiunta di nuovi utenti al ruolo selezionato

Con la rimozione di utenti dal ruolo selezionato, il visitatore per questa pagina inoltre deve essere in grado di aggiungere un utente al ruolo selezionato. L'interfaccia ottimo per l'aggiunta di un utente al ruolo selezionato dipende dal numero di account utente che previsti. Se il sito Web conterrà solo alcuni account utente dozzine o meno, è possibile usare un controllo DropDownList qui. Se potrebbero essere presenti migliaia di account utente, si desidera includere un'interfaccia utente che consente il visitatore per spostarsi tra gli account, cercare un account specifico, o filtrare gli account utente in qualche altro modo.

Per questa pagina è possibile usare un'interfaccia molto semplice che funziona indipendentemente dal numero di account utente nel sistema. In particolare, si userà una casella di testo, che richiede il visitatore per digitare il nome utente dell'utente che vuole aggiungere al ruolo selezionato. Se non esiste alcun utente con lo stesso nome, o se l'utente è già un membro del ruolo, verrà visualizzato un messaggio in `ActionStatus` etichetta. Ma se l'utente esiste e non è un membro del ruolo, si verrà aggiunti al ruolo e aggiornare la griglia.

Aggiungere una casella di testo e un pulsante sotto il controllo GridView. Impostare la casella di testo `ID` al `UserNameToAddToRole` e impostare il pulsante `ID` e `Text` le proprietà da `AddUserToRoleButton` e "Aggiunta al ruolo utente", rispettivamente.

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample17.aspx)]

A questo punto, creare un `Click` gestore dell'evento per il `AddUserToRoleButton` e aggiungere il codice seguente:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample18.cs)]

La maggior parte del codice di `Click` gestore eventi esegue vari controlli di convalida. Assicura che il visitatore specificato un nome utente nel `UserNameToAddToRole` casella di testo che l'utente esiste nel sistema e che non appartengono già al ruolo selezionato. Se uno di questi controlli ha esito negativo, viene visualizzato un messaggio appropriato nella `ActionStatus` e il gestore dell'evento è terminato. Se tutti i controlli di esito positivo, l'utente viene aggiunto al ruolo tramite il `Roles.AddUserToRole` (metodo). In seguito, la casella di testo del `Text` proprietà è stata rimossa, il controllo GridView, viene aggiornato e `ActionStatus` etichetta viene visualizzato un messaggio che indica che l'utente specificato sia stato aggiunto correttamente al ruolo selezionato.

> [!NOTE]
> Per assicurarsi che l'utente specificato non appartiene già al ruolo selezionato, viene usato il [ `Roles.IsUserInRole(userName, roleName)` metodo](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx), che restituisce un valore booleano che indica se *userName* è un membro del *roleName*. Si userà questo metodo nuovamente nel <a id="_msoanchor_2"> </a> [esercitazione successiva](role-based-authorization-cs.md) quando si esamina l'autorizzazione basata sui ruoli.


Visita la pagina tramite un browser e selezionare il ruolo supervisori dal `RoleList` DropDownList. Provare a immettere un nome utente non valido, verrà visualizzato un messaggio che spiega che l'utente non esiste nel sistema.


[![È possibile aggiungere un utente Non esistente a un ruolo](assigning-roles-to-users-cs/_static/image26.png)](assigning-roles-to-users-cs/_static/image25.png)

**Figura 9**: È possibile aggiungere un utente Non esistente a un ruolo ([fare clic per visualizzare l'immagine con dimensioni normali](assigning-roles-to-users-cs/_static/image27.png))


Ora provare ad aggiungere un utente valido. Proseguire e aggiungere nuovamente Tito al ruolo supervisori.


[![Tito è ancora una volta un supervisore.](assigning-roles-to-users-cs/_static/image29.png)](assigning-roles-to-users-cs/_static/image28.png)

**Figura 10**: Tito è ancora una volta supervisore.  ([Fare clic per visualizzare l'immagine con dimensioni normali](assigning-roles-to-users-cs/_static/image30.png))


## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>Passaggio 3: Cross-l'aggiornamento di "utente" e "Dal ruolo" interfacce

Il `UsersAndRoles.aspx` nella pagina sono disponibili due interfacce distinte per la gestione di utenti e ruoli. Attualmente, queste due interfacce di agiscono indipendentemente uno da altro, pertanto è possibile che una modifica apportata in un'unica interfaccia verrà non propagata immediatamente negli altri. Ad esempio, si supponga che il visitatore per la pagina Seleziona il ruolo supervisori dal `RoleList` DropDownList, in cui sono elencati Bruce e Tito come relativi membri. Successivamente, il visitatore seleziona Tito dal `UserList` DropDownList, che consente di archiviare le caselle di controllo agli amministratori e i supervisori il `UsersRoleList` Repeater. Se il visitor deseleziona quindi il ruolo di Supervisore da Repeater, Tito viene rimosso dal ruolo supervisori, ma questa modifica non viene riflessa nell'interfaccia "dal ruolo". Il controllo GridView viene comunque visualizzata Tito come membro del ruolo supervisori.

Per risolvere il problema è necessario aggiorna il GridView ogni volta che un ruolo è selezionato o deselezionato dal `UsersRoleList` Repeater. Allo stesso modo, è necessario aggiornare il Repeater ogni volta che un utente viene rimosso o aggiunto a un ruolo dall'interfaccia "dal ruolo".

Il controllo Repeater nell'interfaccia "dall'utente" viene aggiornato mediante la chiamata di `CheckRolesForSelectedUser` (metodo). L'interfaccia "dal ruolo" può essere modificato nel `RolesUserList` GridView `RowDeleting` gestore dell'evento e il `AddUserToRoleButton` del pulsante `Click` gestore dell'evento. Di conseguenza, dobbiamo chiamare il `CheckRolesForSelectedUser` metodo da ognuno di questi metodi.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample19.cs)]

Allo stesso modo, il controllo GridView nell'interfaccia "dal ruolo" vengono aggiornati chiamando il `DisplayUsersBelongingToRole` metodo e l'interfaccia "dall'utente" viene modificato tramite la `RoleCheckBox_CheckChanged` gestore dell'evento. Di conseguenza, dobbiamo chiamare il `DisplayUsersBelongingToRole` metodo da questo gestore eventi.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample20.cs)]

Con queste modifiche al codice secondario, il "dall'utente" e "dal ruolo" interfacce ora correttamente cross-aggiornamento. Per verificarlo, visitare la pagina tramite un browser e selezionare Tito e supervisori dal `UserList` e `RoleList` controlli DropDownList, rispettivamente. Si noti che quando si deseleziona il ruolo supervisori per Tito da Repeater nell'interfaccia "dall'utente", Tito viene rimosso automaticamente dalla GridView nell'interfaccia "dal ruolo". Aggiunta di Tito nuovamente al ruolo supervisori dall'interfaccia "dal ruolo" automaticamente controlli nuovamente la casella di controllo supervisori nell'interfaccia "dall'utente".

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>Passaggio 4: Personalizzazione CreateUserWizard per includere un passaggio "Specificare ruoli"

Nel <a id="_msoanchor_3"> </a> [ *creazione degli account utente* ](../membership/creating-user-accounts-cs.md) esercitazione è stato illustrato come usare il controllo CreateUserWizard Web per fornire un'interfaccia per la creazione di un nuovo account utente. Il controllo CreateUserWizard può essere usato in uno dei due modi:

- Per consentire ai visitatori di creare il proprio account utente nel sito, e
- Per consentire agli amministratori di creare nuovi account

Nel primo caso, usare un visitatore visiti il sito e compila CreateUserWizard, immettendo le proprie informazioni per eseguire la registrazione nel sito. Nel secondo caso, un amministratore crea un nuovo account da un altro utente.

Quando viene creato un account da un amministratore di un'altra persona, potrebbe essere utile consentire all'amministratore di specificare i ruoli a cui appartiene il nuovo account utente. Nel <a id="_msoanchor_4"> </a> [ *Storing* *informazioni utente aggiuntive* ](../membership/storing-additional-user-information-cs.md) esercitazione è stato illustrato come personalizzare il CreateUserWizard aggiungendo altri `WizardSteps`. Ecco come aggiungere un passaggio aggiuntivo per CreateUserWizard per specificare i nuovi ruoli dell'utente.

Aprire il `CreateUserWizardWithRoles.aspx` pagina e aggiungere un controllo CreateUserWizard denominato `RegisterUserWithRoles`. Impostare il controllo `ContinueDestinationPageUrl` proprietà su "~ / default. aspx". Poiché l'idea è che un amministratore utilizzerà questo controllo CreateUserWizard per creare nuovi account utente, impostare il controllo `LoginCreatedUser` proprietà su False. Ciò `LoginCreatedUser` proprietà specifica se il visitor automaticamente per l'accesso l'utente appena creato e il valore predefinito è true. È impostato su False poiché quando un amministratore crea un nuovo account si vuole mantenere quest'ultimo accesso eseguito come se stesso.

Selezionare quindi il "Aggiungi/Rimuovi `WizardSteps`..." dallo Smart Tag del CreateUserWizard e aggiungere una nuova `WizardStep`, l'impostazione relativa `ID` a `SpecifyRolesStep`. Spostare il `SpecifyRolesStep WizardStep` in modo che si tratta di dopo il passaggio "Sign Up for Your New Account", ma prima del passaggio "Complete". Impostare il `WizardStep`del `Title` proprietà su "specificare"Roles, relativo `StepType` proprietà `Step`e il relativo `AllowReturn` proprietà su False.


[![Aggiungere il](assigning-roles-to-users-cs/_static/image32.png)](assigning-roles-to-users-cs/_static/image31.png)

**Figura 11**: aggiungere "Specificare ruoli" `WizardStep` a CreateUserWizard ([fare clic per visualizzare l'immagine con dimensioni normali](assigning-roles-to-users-cs/_static/image33.png))


Dopo questa modifica markup dichiarativo di CreateUserWizard dovrebbe essere simile al seguente:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample21.aspx)]

In "Specificare ruoli" `WizardStep`, aggiungere un controllo CheckBoxList denominato `RoleList`. Questo CheckBoxList elencherà i ruoli disponibili, l'abilitazione di persona visitando la pagina controllare i ruoli a cui appartiene l'utente appena creato.

Abbiamo ancora due attività di codifica: prima di tutto è necessario popolare il `RoleList` CheckBoxList con i ruoli del sistema; in secondo luogo, è necessario aggiungere l'utente creato per i ruoli selezionati quando l'utente sposta il passaggio "Specificare ruoli" con il passaggio "Completa". Permette di eseguire la prima attività di `Page_Load` gestore dell'evento. Nell'esempio di codice a livello di programmazione i riferimenti di `RoleList` casella di controllo nella prima visita la pagina e associa i ruoli del sistema ad esso.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample22.cs)]

Il codice precedente dovrebbe risultare familiare. Nel <a id="_msoanchor_5"> </a> [ *Storing* *informazioni utente aggiuntive* ](../membership/storing-additional-user-information-cs.md) esercitazione viene usato due `FindControl` istruzioni per fare riferimento a un controllo Web all'interno di un oggetto personalizzato `WizardStep`. E il codice che associa i ruoli a CheckBoxList è stato ricavato da precedentemente in questa esercitazione.

Per eseguire la seconda attività di programmazione, che è necessario conoscere quando il passaggio "Specificare ruoli" è stato completato. Tenere presente che CreateUserWizard è un `ActiveStepChanged` evento, che viene attivato ogni volta che il visitatore consente di passare da un unico passaggio a un altro. Qui è possibile determinare se l'utente ha raggiunto il passaggio "Completa". In questo caso, è necessario aggiungere l'utente per i ruoli selezionati.

Creare un gestore eventi per il `ActiveStepChanged` eventi e aggiungere il codice seguente:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample23.cs)]

Se l'utente ha appena raggiunto il passaggio "Completed", il gestore dell'evento enumera gli elementi del `RoleList` CheckBoxList e l'utente appena creato viene assegnato ai ruoli selezionati.

Visitare questa pagina tramite un browser. Il primo passaggio nella CreateUserWizard è il passaggio "Sign Up for Your New Account" standard, che viene richiesto per il nuovo nome utente, password, indirizzo di posta elettronica e altre informazioni sulla chiave. Immettere le informazioni per creare un nuovo utente denominato Wanda.


[![Creare un nuovo utente denominato Wanda](assigning-roles-to-users-cs/_static/image35.png)](assigning-roles-to-users-cs/_static/image34.png)

**Figura 12**: creare un nuovo Wanda denominato di utente ([fare clic per visualizzare l'immagine con dimensioni normali](assigning-roles-to-users-cs/_static/image36.png))


Fare clic sul pulsante "Create User". CreateUserWizard chiama internamente il `Membership.CreateUser` metodo, la creazione del nuovo account utente, quindi passa al passaggio successivo, "specifica ruoli". Di seguito sono elencati i ruoli del sistema. Selezionare la casella di controllo supervisori e fare clic su Avanti.


[![Rendere un membro del ruolo supervisori Wanda](assigning-roles-to-users-cs/_static/image38.png)](assigning-roles-to-users-cs/_static/image37.png)

**Figura 13**: impostare come membro del ruolo supervisori Wanda ([fare clic per visualizzare l'immagine con dimensioni normali](assigning-roles-to-users-cs/_static/image39.png))


Selezione di Avanti provoca un postback e gli aggiornamenti di `ActiveStep` al passaggio "Completa". Nel `ActiveStepChanged` gestore eventi, l'account utente creati di recente viene assegnato al ruolo supervisori. Per verificarlo, tornare al `UsersAndRoles.aspx` pagina e selezionare i supervisori dal `RoleList` DropDownList. Come illustrato nella figura 14, Supervisor sono ora costituita da tre utenti: Bruce Tito e Wanda.


[![Bruce Tito e Wanda sono tutti i supervisori](assigning-roles-to-users-cs/_static/image41.png)](assigning-roles-to-users-cs/_static/image40.png)

**Figura 14**: Bruce Tito e Wanda sono tutti i supervisori ([fare clic per visualizzare l'immagine con dimensioni normali](assigning-roles-to-users-cs/_static/image42.png))


## <a name="summary"></a>Riepilogo

Il framework di ruoli offre i metodi per il recupero di informazioni sui ruoli e i metodi per determinare quali utenti appartengono a un ruolo specifico di un utente specifico. Inoltre, esistono diversi metodi per l'aggiunta e rimozione di uno o più utenti a uno o più ruoli. In questa esercitazione ci siamo concentrati su due di questi metodi: `AddUserToRole` e `RemoveUserFromRole`. Esistono altre varianti progettate per aggiungere più utenti a un singolo ruolo e per assegnare più ruoli a un singolo utente.

Questa esercitazione è inoltre incluso uno sguardo di estendere il controllo CreateUserWizard per includere un `WizardStep` per specificare i ruoli dell'utente appena creato. Tale passaggio può essere utile un amministratore di semplificare il processo di creazione di account utente per i nuovi utenti.

A questo punto abbiamo visto come creare ed eliminare i ruoli e su come aggiungere e rimuovere utenti dai ruoli. Ma è ancora vedere autorizzazione basata sui ruoli applicazione. Nel <a id="_msoanchor_6"> </a> [esercitazione seguente](role-based-authorization-cs.md) si esaminerà la definizione di regole di autorizzazione URL del ruolo per ruolo, nonché come illustrato per limitare le funzionalità a livello di pagina basata su ruoli dell'utente attualmente connesso.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Panoramica sullo strumento Amministrazione sito Web ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx)
- [Esame di ASP. Appartenenza, ruoli e profilo di rete](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [In sequenza il proprio strumento Amministrazione sito Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla.com, ha lavorato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola  *[Sams Teach Yourself ASP.NET 2.0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. È possibile contattarlo Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali...

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata Teresa Murphy. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami un messaggio nella [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](creating-and-managing-roles-cs.md)
> [Successivo](role-based-authorization-cs.md)
