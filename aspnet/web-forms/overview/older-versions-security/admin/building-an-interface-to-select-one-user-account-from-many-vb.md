---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
title: Creazione di un'interfaccia per selezionare un Account utente da molti (VB) | Documenti Microsoft
author: rick-anderson
description: "In questa esercitazione verrà compilata un'interfaccia utente con una griglia di paging, filtrabile. In particolare, l'interfaccia utente sarà costituito da una serie di LinkButton per..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: da53380c-a16b-41c7-a20d-24343c735c52
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
msc.type: authoredcontent
ms.openlocfilehash: 6561fec9ccffbc421dd6a542dcd421acd0af6052
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="building-an-interface-to-select-one-user-account-from-many-vb"></a>Creazione di un'interfaccia per selezionare un Account utente da molti (Visual Basic)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.12.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_vb.pdf)

> In questa esercitazione verrà compilata un'interfaccia utente con una griglia di paging, filtrabile. In particolare, l'interfaccia utente sarà costituito da una serie di LinkButton per filtrare i risultati in base alla lettera inizia del nome utente e un controllo GridView per visualizzare gli utenti corrispondenti. Si inizierà elencando tutti gli account utente in un controllo GridView. Quindi, nel passaggio 3, si aggiungerà il filtro LinkButton. Passaggio 4 esamina i risultati filtrati di paging. L'interfaccia creato nei passaggi da 2 a 4 verrà considerato nelle esercitazioni successive per eseguire attività amministrative per un particolare account utente.


## <a name="introduction"></a>Introduzione

Nel <a id="_msoanchor_1"> </a> [ *l'assegnazione di ruoli agli utenti* ](../roles/assigning-roles-to-users-vb.md) esercitazione creata un'interfaccia rudimentale all'amministratore di selezionare un utente e gestire i propri ruoli. In particolare, l'interfaccia visualizzata all'amministratore un elenco a discesa di tutti gli utenti. Tale interfaccia è idoneo quando sono presenti, ma una dozzina utente account, ma è difficile da gestire per i siti con centinaia o migliaia di account. Una griglia di paging, filtrabile è più adatto dell'interfaccia utente per i siti Web con basi utente di grandi dimensioni.

In questa esercitazione verrà compilata un'interfaccia utente. In particolare, l'interfaccia utente sarà costituito da una serie di LinkButton per filtrare i risultati in base alla lettera inizia del nome utente e un controllo GridView per visualizzare gli utenti corrispondenti. Si inizierà elencando tutti gli account utente in un controllo GridView. Quindi, nel passaggio 3, si aggiungerà il filtro LinkButton. Passaggio 4 esamina i risultati filtrati di paging. L'interfaccia creato nei passaggi da 2 a 4 verrà considerato nelle esercitazioni successive per eseguire attività amministrative per un particolare account utente.

Iniziamo!

## <a name="step-1-adding-new-aspnet-pages"></a>Passaggio 1: Aggiunta di nuove pagine ASP.NET

In questa esercitazione e i due successivi è verrà esaminando le funzioni di amministrazione e le funzionalità. È necessario una serie di pagine ASP.NET per implementare gli argomenti esaminati in queste esercitazioni. Di seguito creare queste pagine e aggiornare la mappa del sito.

Iniziare creando una nuova cartella nel progetto denominato `Administration`. Successivamente, aggiungere due nuove pagine ASP.NET per la cartella, il collegamento di ogni pagina con il `Site.master` pagina master. Le pagine dei nomi:

- `ManageUsers.aspx`
- `UserInformation.aspx`

Aggiungere due pagine alla directory radice del sito Web: `ChangePassword.aspx` e `RecoverPassword.aspx`.

Queste quattro pagine, a questo punto, è necessario due controlli contenuto, uno per ogni ContentPlaceHolder pagina: `MainContent` e `LoginContent`.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample1.aspx)]

Si desidera mostrare il markup della pagina master predefinita per il `LoginContent` ContentPlaceHolder per tali pagine. Pertanto, rimuovere il markup dichiarativo per la `Content2` controllo contenuto. Al termine dell'operazione, il markup alla pagina deve contenere solo un controllo contenuto.

Pagine ASP.NET il `Administration` cartella sono destinati esclusivamente agli utenti amministratori. Abbiamo aggiunto un ruolo di amministratore per il sistema di <a id="_msoanchor_2"> </a> [ *creazione e gestione dei ruoli* ](../roles/creating-and-managing-roles-vb.md) esercitazione; limitare l'accesso a queste due pagine per questo ruolo. A tale scopo, aggiungere un `Web.config` file per il `Administration` cartella e configurare il `<authorization>` elemento comunque gli utenti del ruolo di amministratori e per impedire tutti gli altri.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample2.xml)]

Esplora soluzioni del progetto a questo punto dovrebbe essere simile alla schermata illustrata nella figura 1.


[![Quattro nuove pagine e un File Web. config sono stati aggiunti al sito Web](building-an-interface-to-select-one-user-account-from-many-vb/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image1.png)

**Figura 1**: quattro nuove pagine e un `Web.config` File sono stati aggiunti al sito Web ([fare clic per visualizzare l'immagine ingrandita](building-an-interface-to-select-one-user-account-from-many-vb/_static/image3.png))


Infine, aggiornare la mappa del sito (`Web.sitemap`) per includere una voce per il `ManageUsers.aspx` pagina. Aggiungere il seguente codice XML dopo la `<siteMapNode>` abbiamo aggiunto per le esercitazioni di ruoli.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample3.xml)]

Con la mappa del sito è stata aggiornata, visitare il sito tramite un browser. Come illustrato nella figura 2, la navigazione a sinistra ora include gli elementi per le esercitazioni di amministrazione.


[![Mappa del sito include un nodo denominato Amministrazione utenti](building-an-interface-to-select-one-user-account-from-many-vb/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image4.png)

**Figura 2**: mappa del sito include un'amministrazione utente denominato nodo ([fare clic per visualizzare l'immagine ingrandita](building-an-interface-to-select-one-user-account-from-many-vb/_static/image6.png))


## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>Passaggio 2: Elenco di tutti gli account utente in un controllo GridView.

L'obiettivo finale per questa esercitazione consiste nel creare una griglia di paging, filtrabile tramite cui un amministratore può selezionare un account utente da gestire. Per iniziare, elenco *tutti* gli utenti in un controllo GridView. Dopo questa procedura guidata, si aggiungerà il filtraggio e paging interfacce e le funzionalità.

Aprire il `ManageUsers.aspx` nella pagina il `Administration` cartella e aggiungere un controllo GridView, impostare il relativo `ID` per `UserAccounts` breve, si scriverà il codice per associare il set di account utente per l'utilizzo di GridView il `Membership` della classe `GetAllUsers` (metodo). Come illustrato nelle esercitazioni precedenti, il `GetAllUsers` metodo restituisce un `MembershipUserCollection` oggetto, che è una raccolta di `MembershipUser` oggetti. Ogni `MembershipUser` nella raccolta include le proprietà come `UserName`, `Email`, `IsApproved`e così via.

Per visualizzare le informazioni sull'account utente desiderato in GridView, impostare il controllo GridView `AutoGenerateColumns` la proprietà su False e aggiungere BoundField per il `UserName`, `Email`, e `Comment` proprietà e CheckBoxFields per il `IsApproved`, `IsLockedOut`, e `IsOnline` proprietà. Questa configurazione può essere applicata tramite markup dichiarativo del controllo o la finestra di dialogo campi. Figura 3 mostra una schermata dei campi la finestra di dialogo dopo la casella di controllo campi di generazione automatica è stata deselezionata e la BoundField e CheckBoxFields aggiunti e configurati.


[![Aggiungere tre BoundField e tre CheckBoxFields al controllo GridView.](building-an-interface-to-select-one-user-account-from-many-vb/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image7.png)

**Figura 3**: aggiungere tre BoundField e tre CheckBoxFields a GridView ([fare clic per visualizzare l'immagine ingrandita](building-an-interface-to-select-one-user-account-from-many-vb/_static/image9.png))


Dopo aver configurato il GridView, verificare che il markup dichiarativo è simile al seguente:

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample4.aspx)]

Successivamente, è necessario scrivere codice che associa gli account utente a GridView. Creare un metodo denominato `BindUserAccounts` per eseguire questa attività e quindi chiamare dal `Page_Load` gestore dell'evento nella prima visita pagina.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample5.vb)]

Richiedere qualche istante per testare la pagina tramite un browser. Come illustrato nella figura 4, il `UserAccounts` GridView Elenca il nome utente, indirizzo di posta elettronica e altre informazioni pertinenti account per tutti gli utenti nel sistema.


[![Vengono elencati gli account utente nel controllo GridView.](building-an-interface-to-select-one-user-account-from-many-vb/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image10.png)

**Figura 4**: l'account utente sono elencati in GridView ([fare clic per visualizzare l'immagine ingrandita](building-an-interface-to-select-one-user-account-from-many-vb/_static/image12.png))


## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>Passaggio 3: Filtrare i risultati per la prima lettera del nome utente

Attualmente il `UserAccounts` GridView Mostra *tutti* degli account utente. Per i siti Web con centinaia o migliaia di account utente, è fondamentale che sia in grado di ridurre rapidamente gli account visualizzati. Questa operazione può essere eseguita aggiungendo filtri LinkButton alla pagina. Aggiungere alla pagina 27 LinkButton: uno denominato tutti insieme uno LinkButton per ogni lettera dell'alfabeto. Se un visitatore fa clic sull'elemento LinkButton di tutti, GridView visualizzerà tutti gli utenti. Se si fa clic su una determinata lettera, verranno visualizzati solo agli utenti il cui nome utente inizia con la lettera selezionata.

La prima attività consiste nell'aggiungere i controlli LinkButton 27. Un'opzione, è possibile creare il 27 LinkButton in modo dichiarativo, uno alla volta. Un approccio più flessibile consiste nell'utilizzare un controllo Repeater con un `ItemTemplate` che esegue il rendering di un controllo LinkButton e quindi associa le opzioni di filtro Ripetitore come un `String` matrice.

Avvio tramite l'aggiunta di un controllo Repeater alla pagina precedente il `UserAccounts` GridView. Impostare il controllo Repeater `ID` proprietà `FilteringUI` configurare i modelli del ripetitore in modo che il relativo `ItemTemplate` esegue il rendering di un controllo LinkButton cui `Text` e `CommandName` proprietà sono associate all'elemento della matrice corrente. Come illustrato nel <a id="_msoanchor_3"> </a> [ *l'assegnazione di ruoli agli utenti* ](../roles/assigning-roles-to-users-vb.md) dell'esercitazione, questa può essere eseguita tramite il `Container.DataItem` sintassi di associazione dati. Utilizzare il controllo Repeater `SeparatorTemplate` per visualizzare una linea verticale tra ogni collegamento.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample6.aspx)]

Per popolare il controllo Repeater con le opzioni di filtro desiderate, creare un metodo denominato `BindFilteringUI`. Verificare di chiamare questo metodo il `Page_Load` gestore dell'evento il primo caricamento della pagina.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample7.vb)]

Questo metodo consente di specificare le opzioni di filtro come elementi nel `String` matrice `filterOptions` per ogni elemento nella matrice, Ripetitore verrà eseguito il rendering di un elemento LinkButton con il relativo `Text` e `CommandName` le proprietà assegnate al valore della matrice elemento.

La figura 5 mostra il `ManageUsers.aspx` pagina quando viene visualizzato tramite un browser.


[![Repeater elenca 27 LinkButton filtro](building-an-interface-to-select-one-user-account-from-many-vb/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image13.png)

**Figura 5**: il controllo Repeater elenca 27 filtro LinkButton ([fare clic per visualizzare l'immagine ingrandita](building-an-interface-to-select-one-user-account-from-many-vb/_static/image15.png))


> [!NOTE]
> I nomi utente possono iniziare con qualsiasi carattere, inclusi i numeri e segni di punteggiatura. Per visualizzare questi account, l'amministratore dovrà utilizzare l'opzione tutti LinkButton. In alternativa, è possibile aggiungere un controllo LinkButton per restituire tutti gli account utente che iniziano con un numero. Lasciare questo campo come esercizio per il lettore.


Fare clic su qualsiasi LinkButton il filtro provoca un postback e genera il Ripetitore `ItemCommand` evento, ma non è stata modificata nella griglia perché è stata ancora per scrivere codice per filtrare i risultati. Il `Membership` classe include un [ `FindUsersByName` metodo](https://technet.microsoft.com/en-us/library/system.web.security.membership.findusersbyname.aspx) che restituisce gli account utente il cui nome utente corrisponde a un criterio di ricerca specificati. È possibile utilizzare questo metodo per recuperare solo gli account utente i cui nomi utente iniziano con la lettera specificata dal `CommandName` del controllo LinkButton filtrati che è stato fatto clic.

Avviare aggiornando il `ManageUser.aspx` codice della pagina classe in modo che includa una proprietà denominata `UsernameToMatch` questa proprietà viene mantenuta la stringa di filtro di nome utente durante i postback:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample8.vb)]

Il `UsernameToMatch` proprietà archivia il valore viene assegnato nel `ViewState` raccolta utilizzando la chiave UsernameToMatch. Quando viene letto il valore di questa proprietà, viene verificato se esiste un valore nel `ViewState` raccolta; in caso contrario, viene restituito il valore predefinito, una stringa vuota. Il `UsernameToMatch` proprietà presenta un modello comune, vale a dire il mantenimento di un valore per lo stato di visualizzazione in modo che tutte le modifiche alla proprietà vengono rese persistenti tra i postback. Per altre informazioni su questo modello, vedere [informazioni sullo stato di visualizzazione ASP.NET](https://msdn.microsoft.com/en-us/library/ms972976.aspx).

Aggiornare quindi il `BindUserAccounts` metodo in modo che anziché chiamare `Membership.GetAllUsers`, chiama `Membership.FindUsersByName`, passando il valore della `UsernameToMatch` proprietà aggiunta con il carattere jolly SQL, %.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample9.vb)]

Per visualizzare solo gli utenti il cui nome utente inizia con la lettera A, impostare il `UsernameToMatch` proprietà su e quindi chiamare `BindUserAccounts` porterebbe alla creazione di una chiamata a `Membership.FindUsersByName("A%")`, che restituirà tutti gli utenti il cui nome utente inizia con A. Analogamente, per restituire *tutti* gli utenti, assegnare una stringa vuota per il `UsernameToMatch` proprietà in modo che il `BindUserAccounts` richiama il metodo `Membership.FindUsersByName("%")`, in tal modo la restituzione di tutti gli account utente.

Creare un gestore eventi per il controllo Repeater `ItemCommand` evento. Questo evento viene generato ogni volta che viene selezionato uno dei filtri LinkButton; viene passato sull'elemento LinkButton selezionato `CommandName` valore tramite il `RepeaterCommandEventArgs` oggetto. È necessario assegnare il valore appropriato per il `UsernameToMatch` proprietà e quindi chiamare il `BindUserAccounts` metodo. Se il `CommandName` è All, assegnare una stringa vuota per `UsernameToMatch` in modo che vengono visualizzati tutti gli account utente. In caso contrario, assegnare il `CommandName` valore`UsernameToMatch`

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample10.vb)]

Con questo codice, testare la funzionalità di filtro. Quando la pagina viene prima visitata, vengono visualizzati tutti gli account utente (vedere la figura 5). Facendo clic sull'elemento LinkButton A provoca un postback e filtra i risultati, la visualizzazione solo agli account utente che iniziano per a.


[![Utilizzare il filtro LinkButton per visualizzare gli utenti il cui nome utente inizia con una determinata lettera](building-an-interface-to-select-one-user-account-from-many-vb/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image16.png)

**Figura 6**: utilizzare LinkButton il filtro per visualizzare tali utenti il cui nome utente inizia con una lettera determinati ([fare clic per visualizzare l'immagine ingrandita](building-an-interface-to-select-one-user-account-from-many-vb/_static/image18.png))


## <a name="step-4-updating-the-gridview-to-use-paging"></a>Passaggio 4: Aggiornamento GridView per l'utilizzo di Paging

GridView illustrato nelle figure 5 e 6 sono elencati tutti i record restituiti dal `FindUsersByName` metodo. Se sono presenti centinaia o migliaia di account utente questo può causare all'overload di informazioni quando si visualizzano tutti gli account (come accade quando si fa clic sull'elemento LinkButton di tutte o quando inizialmente si visita la pagina). Per presentare gli account utente in blocchi più gestibile, configurazione GridView per visualizzare i 10 account utente alla volta.

Il controllo GridView offre due tipi di paging:

- **Paging predefinito** : facile da implementare, ma non efficiente. In breve, paging GridView predefinito prevede *tutti* dei record dall'origine dati. Quindi Visualizza solo la pagina di record appropriata.
- **Il paging personalizzato** -richiede maggiore impegno per implementare, ma è più efficiente rispetto all'impostazione predefinita il paging in quanto con paging dei dati personalizzata origine restituisce solo il preciso set di record da visualizzare.

La differenza nelle prestazioni tra l'impostazione predefinita e il paging personalizzato può rivelarsi piuttosto notevole paging migliaia di record. Poiché stiamo creando questa interfaccia, presupponendo che può essere centinaia o migliaia di account utente, è possibile utilizzare il paging personalizzato.

> [!NOTE]
> Per una discussione più approfondita sulle differenze tra l'impostazione predefinita e il paging personalizzato, nonché i problemi relativi all'implementazione del paging personalizzato, fare riferimento a [in modo efficiente il Paging tramite quantità di dati di grandi dimensioni](https://asp.net/learn/data-access/tutorial-25-vb.aspx). Per alcune analisi della differenza di prestazioni tra l'impostazione predefinita e il paging personalizzato, vedere [il Paging personalizzato in ASP.NET con SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx).


Per implementare il paging personalizzato è necessario innanzitutto un meccanismo dal quale recuperare il subset di record visualizzato dal controllo GridView preciso. Buone notizie consiste nel fatto che il `Membership` della classe `FindUsersByName` metodo presenta un overload che consente di specificare l'indice della pagina e le dimensioni di pagina e restituisce solo gli account utente che si trovano all'interno dell'intervallo di record.

In particolare, questo overload ha la firma seguente: [ `FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)` ](https://msdn.microsoft.com/en-us/library/fa5st8b2.aspx).

Il *pageIndex* parametro specifica la pagina dell'account utente da restituire. *pageSize* indica il numero di record da visualizzare per pagina. Il *totalRecords* parametro è un `ByRef` parametro che restituisce il numero di account utente totale nell'archivio dell'utente.

> [!NOTE]
> I dati restituiti da `FindUsersByName` sono ordinati per nome utente; non è possibile personalizzare i criteri di ordinamento.


Controllo GridView. può essere configurato per usare il paging personalizzato, ma solo quando è associato a un controllo ObjectDataSource. Per il controllo ObjectDataSource implementare il paging personalizzato, richiede due metodi: uno che viene passato un indice di riga iniziale e il numero massimo di record da visualizzare, e restituisce il subset preciso di record che si trovano all'interno di tale intervallo. e di cui esegue il paging tramite un metodo che restituisce il numero totale di record. Il `FindUsersByName` overload accetta un indice della pagina e le dimensioni di pagina e restituisce il numero totale di record mediante un `ByRef` parametro. È pertanto una mancata corrispondenza interfaccia qui.

Un'opzione, è possibile creare una classe proxy che espone l'interfaccia ObjectDataSource prevista e quindi chiama internamente il `FindUsersByName` metodo. Un'altra opzione e quella da usare per questo articolo, consiste nel creare la propria interfaccia di paging e utilizzare invece l'interfaccia di paging incorporata del controllo GridView.

### <a name="creating-a-first-previous-next-last-paging-interface"></a>Creazione di un primo, precedente, successivamente, ultimo interfaccia di Paging

Creare ora un'interfaccia di paging con prima, precedente, successivo e ultimo LinkButton. Il primo LinkButton, quando si fa clic, richiederà all'utente la prima pagina di dati, mentre la precedente quest'ultimo restituisce alla pagina precedente. Analogamente, successivo e ultimo passerà l'utente alla pagina successiva e l'ultima, rispettivamente. Aggiungere i quattro controlli LinkButton sotto il `UserAccounts` GridView.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample11.aspx)]

Successivamente, creare un gestore eventi per ogni sull'elemento LinkButton `Click` eventi.

Figura 7 illustra le quattro LinkButton quando viene visualizzato tramite la visualizzazione di progettazione di Visual Web Developer.


[![Aggiungere prima, precedente, in futuro, e l'ultimo LinkButton sotto controllo GridView.](building-an-interface-to-select-one-user-account-from-many-vb/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image19.png)

**Figura 7**: prima di aggiungere, precedente, successivo e ultimo LinkButton sotto il controllo GridView ([fare clic per visualizzare l'immagine ingrandita](building-an-interface-to-select-one-user-account-from-many-vb/_static/image21.png))


### <a name="keeping-track-of-the-current-page-index"></a>Tenere traccia dell'indice della pagina corrente

Quando un utente visita prima la `ManageUsers.aspx` pagina o fa clic su uno dei filtri del pulsante, si desidera visualizzare la prima pagina di dati in GridView. Quando l'utente fa clic su uno degli spostamenti LinkButton, tuttavia, è necessario aggiornare l'indice della pagina. Per mantenere l'indice della pagina e il numero di record da visualizzare per pagina, aggiungere le due proprietà seguenti nella classe code-behind della pagina:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample12.vb)]

Ad esempio il `UsernameToMatch` proprietà, il `PageIndex` proprietà mantiene il relativo valore per lo stato di visualizzazione. La proprietà di sola lettura `PageSize` proprietà restituisce un valore hardcoded, 10. Invitare il lettore interessato per aggiornare questa proprietà per l'utilizzo dello stesso modello `PageIndex`e quindi per potenziare la `ManageUsers.aspx` pagina in modo che l'utente visita la pagina è possibile specificare il numero di account utente da visualizzare per ogni pagina.

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>Il recupero solo i record della pagina corrente, di aggiornamento dell'indice, pagina e di abilitazione e disabilitazione di LinkButton di interfaccia di Paging

Con l'interfaccia di paging sul posto e `PageIndex` e `PageSize` aggiunta di proprietà, si è pronti per aggiornare il `BindUserAccounts` metodo in modo che utilizzi appropriata `FindUsersByName` rapporto di overload. Inoltre, è necessario abilitare o disabilitare l'interfaccia di paging a seconda di quale pagina viene visualizzata per questo metodo. Quando si visualizza la prima pagina di dati, i collegamenti del primo e precedente devono essere disabilitati; Successivamente e ultima deve essere disabilitata quando si visualizza l'ultima pagina.

Aggiornare il metodo `BindUserAccounts` con il codice seguente:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample13.vb)]

Si noti che il numero totale di record il paging tramite è determinato dal parametro ultimo del `FindUsersByName` metodo. Dopo la pagina specificata di account utente vengono restituiti i quattro LinkButton sono abilitato o disabilitato, a seconda se viene visualizzata prima o ultima pagina di dati.

L'ultimo passaggio consiste nello scrivere il codice per LinkButton quattro `Click` gestori eventi. Questi gestori eventi è necessario aggiornare il `PageIndex` proprietà e quindi riassociare i dati a GridView mediante una chiamata a `BindUserAccounts` il primo, indietro e Avanti gestori di eventi sono molto semplici. Il `Click` gestore eventi per l'ultimo LinkButton, tuttavia, è un po' più complesso in quanto è necessario determinare il numero di record è visualizzato per determinare l'ultimo indice della pagina.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample14.vb)]

Cifre 8 e 9 mostrano l'interfaccia di paging personalizzata in azione. Figura 8 viene illustrata la `ManageUsers.aspx` pagina quando si visualizzano la prima pagina di dati per tutti gli account utente. Si noti che vengono visualizzati solo 10 degli 13 account. Il collegamento successivo o ultimo provoca un postback, gli aggiornamenti di `PageIndex` su 1 e associa la seconda pagina utente account alla griglia (vedere Figura 9).


[![Vengono visualizzati gli account utente di primo 10](building-an-interface-to-select-one-user-account-from-many-vb/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image22.png)

**Figura 8**: vengono visualizzati il primo account di 10 ([fare clic per visualizzare l'immagine ingrandita](building-an-interface-to-select-one-user-account-from-many-vb/_static/image24.png))


[![Fare clic sul collegamento successivo consente di visualizzare la seconda pagina di account utente](building-an-interface-to-select-one-user-account-from-many-vb/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image25.png)

**Figura 9**: il collegamento successivo consente di visualizzare la seconda pagina di account utente ([fare clic per visualizzare l'immagine ingrandita](building-an-interface-to-select-one-user-account-from-many-vb/_static/image27.png))


## <a name="summary"></a>Riepilogo

Gli amministratori, spesso è necessario selezionare un utente dall'elenco di account. Nelle esercitazioni precedenti abbiamo esaminato tramite un elenco di riepilogo a discesa popolato con gli utenti, ma questo approccio non è facilmente scalabile. In questa esercitazione, è esplorare un'alternativa migliore: un'interfaccia filtrabile i cui risultati vengono visualizzati in un controllo GridView di paging. Con questa interfaccia utente, gli amministratori possono rapido ed efficiente individuare e selezionare un account utente tra le migliaia.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Paging personalizzato in ASP.NET con SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [In modo efficiente il Paging grandi quantità di dati](https://asp.net/learn/data-access/tutorial-25-vb.aspx)
- [Strumento Amministrazione sito Web in sequenza](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è  *[SAM insegna manualmente ASP.NET 2.0 nelle 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott può essere raggiunto al [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog all'indirizzo [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata Alicja Maziarz. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla

>[!div class="step-by-step"]
[Precedente](unlocking-and-approving-user-accounts-cs.md)
[Successivo](recovering-and-changing-passwords-vb.md)
