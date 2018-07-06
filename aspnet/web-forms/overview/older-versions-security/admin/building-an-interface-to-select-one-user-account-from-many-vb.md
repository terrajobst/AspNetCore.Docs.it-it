---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
title: Creazione di un'interfaccia per selezionare un Account utente da molti (Visual Basic) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione si compilerà un'interfaccia utente con una griglia di paging, filtrabile. In particolare, l'interfaccia utente sarà costituito da una serie di LinkButton per...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: da53380c-a16b-41c7-a20d-24343c735c52
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
msc.type: authoredcontent
ms.openlocfilehash: ec257b09209cb1a377f1ae93b58db4469f438a46
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373191"
---
<a name="building-an-interface-to-select-one-user-account-from-many-vb"></a>Creazione di un'interfaccia per selezionare un Account utente da molti (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.12.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_vb.pdf)

> In questa esercitazione si compilerà un'interfaccia utente con una griglia di paging, filtrabile. In particolare, l'interfaccia utente sarà costituito da una serie di LinkButton per filtrare i risultati in base alla lettera inizia del nome utente e un controllo GridView per mostrare gli utenti corrispondenti. Si inizierà elencando tutti gli account utente in un controllo GridView. Nel passaggio 3, quindi, si aggiungerà il filtro LinkButton. Passaggio 4 esamina il paging dei risultati filtrati. L'interfaccia creato nei passaggi da 2 a 4 verrà considerato nelle esercitazioni successive per eseguire attività amministrative per un particolare account utente.


## <a name="introduction"></a>Introduzione

Nel <a id="_msoanchor_1"> </a> [ *assegnando ruoli agli utenti* ](../roles/assigning-roles-to-users-vb.md) esercitazione, abbiamo creato un'interfaccia rudimentale per un amministratore di selezionare un utente e gestire i ruoli di suo. In particolare, l'interfaccia visualizzata all'amministratore un elenco di riepilogo a discesa di tutti gli utenti. Tale interfaccia è adatto quando sono presenti ma una decina utente degli account, ma è difficile da gestire per i siti con centinaia o migliaia di account. Una griglia di paging, filtrabile è più adatto dell'interfaccia utente per i siti Web con basi utente di grandi dimensioni.

In questa esercitazione si compilerà un'interfaccia utente. In particolare, l'interfaccia utente sarà costituito da una serie di LinkButton per filtrare i risultati in base alla lettera inizia del nome utente e un controllo GridView per mostrare gli utenti corrispondenti. Si inizierà elencando tutti gli account utente in un controllo GridView. Nel passaggio 3, quindi, si aggiungerà il filtro LinkButton. Passaggio 4 esamina il paging dei risultati filtrati. L'interfaccia creato nei passaggi da 2 a 4 verrà considerato nelle esercitazioni successive per eseguire attività amministrative per un particolare account utente.

Iniziamo!

## <a name="step-1-adding-new-aspnet-pages"></a>Passaggio 1: Aggiunta di nuove pagine ASP.NET

In questa esercitazione e i successivi due è verrà esaminando le funzioni relative all'amministrazione e le funzionalità. È necessario una serie di pagine ASP.NET per implementare gli argomenti esaminati in queste esercitazioni. È possibile creare queste pagine e aggiornare la mappa del sito.

Iniziare creando una nuova cartella nel progetto denominato `Administration`. Successivamente, aggiungere due nuove pagine ASP.NET nella cartella, ogni pagina con il collegamento di `Site.master` pagina master. Denominare le pagine:

- `ManageUsers.aspx`
- `UserInformation.aspx`

Aggiungere anche due pagine alla directory radice del sito Web: `ChangePassword.aspx` e `RecoverPassword.aspx`.

Questi quattro pagine, a questo punto, è necessario due controlli contenuto, uno per ognuno degli elementi ContentPlaceHolder della pagina master: `MainContent` e `LoginContent`.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample1.aspx)]

Si vuole visualizzare il markup predefinito della pagina master per il `LoginContent` ContentPlaceHolder per queste pagine. Pertanto, rimuovere il markup dichiarativo per la `Content2` controllo contenuto. Al termine dell'operazione, il markup alla pagina deve contenere solo un controllo contenuto.

Pagine di ASP.NET il `Administration` cartella sono destinati esclusivamente agli utenti amministratori. Abbiamo aggiunto un ruolo di amministratore per il sistema dei <a id="_msoanchor_2"> </a> [ *creazione e gestione dei ruoli* ](../roles/creating-and-managing-roles-vb.md) esercitazione; limitare l'accesso a queste due pagine a questo ruolo. A tale scopo, aggiungere un `Web.config` file per il `Administration` cartella e configurare relativo `<authorization>` elemento ammettere che gli utenti nel ruolo Administrators e negare tutte le altre.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample2.xml)]

Esplora soluzioni del progetto a questo punto dovrebbe essere simile allo screenshot illustrato nella figura 1.


[![Sono stati aggiunti quattro nuove pagine e un File Web. config per il sito Web](building-an-interface-to-select-one-user-account-from-many-vb/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image1.png)

**Figura 1**: quattro nuove pagine e un' `Web.config` File sono stati aggiunti al sito Web ([fare clic per visualizzare l'immagine con dimensioni normali](building-an-interface-to-select-one-user-account-from-many-vb/_static/image3.png))


Infine, aggiornare la mappa del sito (`Web.sitemap`) per includere una voce per il `ManageUsers.aspx` pagina. Aggiungere il codice XML seguente dopo il `<siteMapNode>` è stato aggiunto per le esercitazioni di ruoli.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample3.xml)]

Con la mappa del sito aggiornata, visitare il sito tramite un browser. Come illustrato nella figura 2, la navigazione a sinistra ora include elementi per le esercitazioni di amministrazione.


[![Mappa del sito include un nodo denominato Amministrazione utenti](building-an-interface-to-select-one-user-account-from-many-vb/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image4.png)

**Figura 2**: la mappa del sito include un nodo denominato utente amministrazione ([fare clic per visualizzare l'immagine con dimensioni normali](building-an-interface-to-select-one-user-account-from-many-vb/_static/image6.png))


## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>Passaggio 2: Elencare tutti gli account utente in un oggetto GridView

L'obiettivo finale per questa esercitazione consiste nel creare una griglia di paging, filtrabile attraverso il quale un amministratore può selezionare un account utente da gestire. Iniziamo pubblicando un'inserzione *tutti* gli utenti in un controllo GridView. Al termine, verrà aggiunto il filtro e paging interfacce e funzionalità.

Aprire il `ManageUsers.aspx` nella pagina la `Administration` cartella e aggiungere un controllo GridView, impostando relativo `ID` a `UserAccounts` In qualche secondo, si scriverà il codice per associare il set di account utente per l'uso di GridView il `Membership` della classe `GetAllUsers` (metodo). Come illustrato nelle esercitazioni precedenti, il `GetAllUsers` metodo restituisce un `MembershipUserCollection` oggetto, ovvero una raccolta di `MembershipUser` oggetti. Ciascuna `MembershipUser` nella raccolta sono incluse proprietà come `UserName`, `Email`, `IsApproved`e così via.

Per visualizzare le informazioni dell'account utente desiderato in GridView, impostare il controllo GridView `AutoGenerateColumns` la proprietà su False e aggiungere i BoundField per il `UserName`, `Email`, e `Comment` le proprietà e CheckBoxFields per il `IsApproved`, `IsLockedOut`, e `IsOnline` proprietà. Questa configurazione può essere applicata tramite il tag del controllo dichiarativa o la finestra di dialogo campi. Figura 3 mostra una schermata dei campi nella finestra di dialogo dopo che è stata deselezionata la casella di controllo delle genera automaticamente i campi e i BoundField e CheckBoxFields sono stati aggiunti e configurati.


[![Aggiungere tre BoundField e tre CheckBoxFields a GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image7.png)

**Figura 3**: aggiungere tre BoundField e CheckBoxFields tre per il controllo GridView ([fare clic per visualizzare l'immagine con dimensioni normali](building-an-interface-to-select-one-user-account-from-many-vb/_static/image9.png))


Dopo aver configurato il controllo GridView, assicurarsi che il markup dichiarativo simile al seguente:

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample4.aspx)]

Successivamente, è necessario scrivere codice che associa gli account utente a GridView. Creare un metodo denominato `BindUserAccounts` per eseguire questa attività e chiamarlo successivamente dal `Page_Load` gestore dell'evento nella prima visita pagina.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample5.vb)]

Si consiglia di testare la pagina tramite un browser. Come illustrato nella figura 4, il `UserAccounts` GridView sono elencati il nome utente, indirizzo di posta elettronica e altre informazioni sugli account pertinenti per tutti gli utenti nel sistema.


[![Vengono elencati gli account utente in GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image10.png)

**Figura 4**: l'account utente sono elencati in GridView ([fare clic per visualizzare l'immagine con dimensioni normali](building-an-interface-to-select-one-user-account-from-many-vb/_static/image12.png))


## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>Passaggio 3: Filtrare i risultati in base alla prima lettera del nome utente

Attualmente le `UserAccounts` controllo GridView sono visualizzate *tutto* degli account utente. Per i siti Web con centinaia o migliaia di account utente, è fondamentale che essere in grado di ridurre rapidamente gli account visualizzati quell'utente. È possibile aggiungendo filtri LinkButton alla pagina. È possibile aggiungere i controlli 27 LinkButton alla pagina: uno denominato tutte insieme a un controllo LinkButton per ogni lettera dell'alfabeto. Se un visitatore sceglie il LinkButton tutti, il controllo GridView mostrerà tutti gli utenti. Se si fa clic su una determinata lettera, verranno visualizzati solo agli utenti il cui nome utente inizi con la lettera selezionata.

La prima attività consiste nell'aggiungere i controlli LinkButton 27. Sarebbe un'opzione per creare il 27 LinkButton in modo dichiarativo, uno alla volta. Un approccio più flessibile consiste nell'usare un controllo Repeater con un `ItemTemplate` che esegue il rendering di un controllo LinkButton e si associa quindi le opzioni di filtro per il controllo Repeater come un `String` matrice.

Avvio tramite l'aggiunta di un controllo Repeater alla pagina precedente il `UserAccounts` GridView. Impostare il controllo Repeater `ID` proprietà `FilteringUI` configurare i modelli del controllo Repeater, in modo che relativo `ItemTemplate` esegue il rendering di un controllo LinkButton cui `Text` e `CommandName` proprietà sono associate all'elemento della matrice corrente. Come abbiamo visto nel <a id="_msoanchor_3"> </a> [ *assegnando ruoli agli utenti* ](../roles/assigning-roles-to-users-vb.md) tutorial, ciò è possibile usare il `Container.DataItem` sintassi di associazione dati. Usare il controllo Repeater `SeparatorTemplate` per visualizzare una linea verticale tra ogni collegamento.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample6.aspx)]

Per popolare il Repeater con le opzioni di filtro desiderati, creare un metodo denominato `BindFilteringUI`. Assicurarsi di chiamare questo metodo dal `Page_Load` gestore dell'evento il primo caricamento della pagina.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample7.vb)]

Questo metodo consente di specificare le opzioni di filtro come elementi nel `String` matrice `filterOptions` per ogni elemento nella matrice, il controllo Repeater verrà eseguito il rendering di un elemento LinkButton con il relativo `Text` e `CommandName` proprietà assegnata al valore della matrice elemento.

Figura 5 mostra il `ManageUsers.aspx` pagina quando viene visualizzato tramite un browser.


[![Il controllo Repeater elenca 27 LinkButton filtro](building-an-interface-to-select-one-user-account-from-many-vb/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image13.png)

**Figura 5**: il controllo Repeater elenca 27 filtro LinkButton ([fare clic per visualizzare l'immagine con dimensioni normali](building-an-interface-to-select-one-user-account-from-many-vb/_static/image15.png))


> [!NOTE]
> I nomi utente possono iniziare con qualsiasi carattere, inclusi i numeri e segni di punteggiatura. Per visualizzare questi account, l'amministratore dovrà usare l'opzione LinkButton tutti. In alternativa, è possibile aggiungere un controllo LinkButton per restituire tutti gli account utente che iniziano con un numero. Lasciare come esercizio per il lettore.


Facendo clic su uno qualsiasi del LinkButton filtro determina un postback e genera il Repeater `ItemCommand` evento, ma non viene modificata nella griglia, perché dobbiamo ancora scrivere codice per filtrare i risultati. Il `Membership` classe include una [ `FindUsersByName` metodo](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) che restituisce gli account utente il cui nome utente corrisponde a un criterio di ricerca specificati. È possibile usare questo metodo per recuperare solo gli account utente i cui nomi utente iniziano con la lettera specificata dal `CommandName` del controllo LinkButton filtrato su cui è stato fatto clic.

Per iniziare, aggiornare il `ManageUser.aspx` code-behind della pagina classe in modo che includa una proprietà denominata `UsernameToMatch` questa proprietà viene mantenuta la stringa di filtro di nome utente durante i postback:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample8.vb)]

Il `UsernameToMatch` proprietà archivia il relativo valore viene assegnato nel `ViewState` raccolta utilizzando la chiave UsernameToMatch. Quando questo valore della proprietà viene letta, controlla se un valore presente nel `ViewState` insieme; in caso contrario, viene restituito il valore predefinito, una stringa vuota. Il `UsernameToMatch` proprietà presenta un modello comune, vale a dire rendere persistente un valore per visualizzare lo stato in modo che tutte le modifiche alla proprietà sono mantenute tra i postback. Per altre informazioni su questo modello, leggere [informazioni sullo stato di visualizzazione ASP.NET](https://msdn.microsoftn-us/library/ms972976.aspx).

A questo punto, aggiornare il `BindUserAccounts` metodo in modo che anziché chiamare il metodo `Membership.GetAllUsers`, chiama `Membership.FindUsersByName`, passando il valore della `UsernameToMatch` proprietà accodata con il carattere jolly SQL, %.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample9.vb)]

Per visualizzare solo gli utenti il cui nome utente inizi con la lettera A, impostare il `UsernameToMatch` proprietà ad A, quindi chiamare `BindUserAccounts` questo porterebbe a una chiamata a `Membership.FindUsersByName("A%")`, che restituirà tutti gli utenti il cui nome utente inizi con A. Analogamente, per restituire *tutti i* gli utenti, assegnare una stringa vuota per il `UsernameToMatch` proprietà in modo che il `BindUserAccounts` metodo richiama il `Membership.FindUsersByName("%")`, in tal modo la restituzione di tutti gli account utente.

Creare un gestore eventi per il controllo Repeater `ItemCommand` evento. Questo evento viene generato ogni volta che viene selezionato uno dei filtri LinkButton; viene passato il LinkButton selezionato `CommandName` valore attraverso la `RepeaterCommandEventArgs` oggetto. È necessario assegnare il valore appropriato per il `UsernameToMatch` proprietà e chiamare quindi il `BindUserAccounts` (metodo). Se il `CommandName` è All, assegnare una stringa vuota per `UsernameToMatch` in modo che vengono visualizzati tutti gli account utente. In caso contrario, assegnare il `CommandName` valore `UsernameToMatch`

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample10.vb)]

Con questo codice, testare la funzionalità di filtro. Quando la pagina viene prima visitata, vengono visualizzati tutti gli account utente (vedere la figura 5). Facendo clic il LinkButton A causa un postback e di filtrare i risultati, visualizzando solo gli account utente che iniziano per a.


[![Usare i controlli LinkButton i filtri per visualizzare tali utenti il cui nome utente inizi con una lettera](building-an-interface-to-select-one-user-account-from-many-vb/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image16.png)

**Figura 6**: usare i controlli LinkButton i filtri per visualizzare tali utenti il cui nome utente inizi con una lettera Certain ([fare clic per visualizzare l'immagine con dimensioni normali](building-an-interface-to-select-one-user-account-from-many-vb/_static/image18.png))


## <a name="step-4-updating-the-gridview-to-use-paging"></a>Passaggio 4: Aggiornamento di GridView per l'uso di Paging

Il controllo GridView illustrato nelle figure 5 e 6 sono elencati tutti i record restituiti dai `FindUsersByName` (metodo). Se sono presenti centinaia o migliaia di account utente di questo può causare all'overload di informazioni quando si visualizzano tutti gli account (come avviene quando si fa clic il LinkButton tutti o inizialmente gli utenti in visita la pagina). Per presentare gli account utente in base a blocchi più gestibili, è possibile configurare il controllo GridView per visualizzare i 10 account utente alla volta.

Il controllo GridView offre due tipi di paging:

- **Il paging predefinito** - facile da implementare, ma non efficiente. In breve, con spostamento tra pagine GridView predefinito prevede *tutti* dei record dall'origine dati. Quindi Visualizza solo la pagina dei record appropriata.
- **Il paging personalizzato** -richiede maggiore impegno per implementare, ma è più efficiente il paging predefinito perché Custom paging dei dati l'origine restituisce solo lo specifico set di record da visualizzare.

La differenza nelle prestazioni tra l'impostazione predefinita e il paging personalizzato può essere piuttosto notevole quando il paging attraverso migliaia di record. Poiché stiamo creando questa interfaccia presupponendo che potrebbero essere centinaia o migliaia di account utente, è possibile usare il paging personalizzato.

> [!NOTE]
> Per una discussione più approfondita sulle differenze tra l'impostazione predefinita e il paging personalizzato, nonché i problemi relativi all'implementazione del paging personalizzato, fare riferimento alla [in modo efficiente il Paging attraverso quantità di dati di grandi dimensioni](https://asp.net/learn/data-access/tutorial-25-vb.aspx). Per alcune analisi della differenza di prestazioni tra l'impostazione predefinita e il paging personalizzato, vedere [Paging personalizzato in ASP.NET con SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx).


Per implementare il paging personalizzato è innanzitutto necessario un meccanismo dal quale recuperare il subset di record visualizzati GridView preciso. La buona notizia è che il `Membership` della classe `FindUsersByName` metodo ha un overload che consente di specificare l'indice della pagina e dimensioni di pagina e restituisce solo gli account utente che si trovano all'interno dell'intervallo di record.

In particolare, questo overload ha la firma seguente: [ `FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)` ](https://msdn.microsoft.com/library/fa5st8b2.aspx).

Il *pageIndex* parametro specifica la pagina degli account utente da restituire. *pageSize* indica il numero di record per visualizzare ogni pagina. Il *totalRecords* parametro è un `ByRef` parametro che restituisce il numero di account utente totale nell'archivio dell'utente.

> [!NOTE]
> I dati restituiti da `FindUsersByName` è ordinato per nome utente; non è possibile personalizzare i criteri di ordinamento.


Il controllo GridView può essere configurato per usare il paging personalizzato, ma solo quando è associato a un controllo ObjectDataSource. Per il controllo ObjectDataSource implementare il paging personalizzato richiede due metodi: uno che viene passato un indice di riga iniziale e il numero massimo di record da visualizzare, e restituisce il sottoinsieme preciso dei record che rientrano in tale intervallo. e Page tramite un metodo che restituisce il numero totale di record. Il `FindUsersByName` overload accetta un indice della pagina e dimensioni di pagina e restituisce il numero totale di record mediante un `ByRef` parametro. Ecco di seguito una mancata corrispondenza dell'interfaccia.

Sarebbe un'opzione per creare una classe proxy che espone l'interfaccia prevede che ObjectDataSource e quindi chiama internamente il `FindUsersByName` (metodo). Un'altra opzione - e quella da usare per questo articolo - è per creare la propria interfaccia di paging e usarlo invece di interfaccia di paging predefinito del controllo GridView.

### <a name="creating-a-first-previous-next-last-paging-interface"></a>Creazione di un primo, precedente, successivamente, ultima interfaccia di Paging

Creare ora un'interfaccia di paging con First, precedente, successivo e ultima LinkButton. Il primo LinkButton, quando si fa clic, richiederà all'utente la prima pagina di dati, mentre la precedente quest'ultimo restituisce alla pagina precedente. Analogamente, successivo e ultimo passerà l'utente alla pagina successiva e l'ultima, rispettivamente. Aggiungere i quattro controlli LinkButton di sotto di `UserAccounts` GridView.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample11.aspx)]

Successivamente, creare un gestore eventi per ognuno del LinkButton `Click` gli eventi.

Figura 7 mostra il quattro LinkButton quando vengono visualizzate tramite la visualizzazione di progettazione di Visual Web Developer.


[![Aggiungere prima, precedente, in futuro, e l'ultimo LinkButton sotto il controllo GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image19.png)

**Figura 7**: prima di aggiungere, precedente, successivo e ultima LinkButton di sotto di GridView ([fare clic per visualizzare l'immagine con dimensioni normali](building-an-interface-to-select-one-user-account-from-many-vb/_static/image21.png))


### <a name="keeping-track-of-the-current-page-index"></a>Tenere traccia di indice della pagina corrente

Quando un utente visita prima la `ManageUsers.aspx` pagina o fa clic su uno dei filtri del pulsante, si desidera visualizzare la prima pagina di dati nel GridView. Quando l'utente fa clic su uno degli spostamenti LinkButton, tuttavia, è necessario aggiornare l'indice della pagina. Per mantenere l'indice della pagina e il numero di record da visualizzare per pagina, aggiungere le due proprietà seguenti alla classe code-behind della pagina:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample12.vb)]

Ad esempio il `UsernameToMatch` proprietà, il `PageIndex` proprietà mantiene il relativo valore allo stato di visualizzazione. Di sola lettura `PageSize` proprietà restituisce un valore hardcoded, 10. Invita il lettore interessato per aggiornare questa proprietà per utilizzare lo stesso modello `PageIndex`e quindi di ampliare la `ManageUsers.aspx` pagina in modo che l'utente visitando la pagina è possibile specificare il numero di account utente da visualizzare per ogni pagina.

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>Recuperare solo i record della pagina corrente, l'aggiornamento dell'indice, pagina e abilitazione e disabilitazione di LinkButton di interfaccia di Paging

Con l'interfaccia di paging in posizione e il `PageIndex` e `PageSize` aggiunte proprietà, siamo pronti aggiornare il `BindUserAccounts` metodo in modo che utilizzi appropriati `FindUsersByName` overload. Inoltre, è necessario affinché questo metodo abilita o disabilita l'interfaccia di paging in base alla quale pagina viene visualizzata. Quando si visualizzano la prima pagina di dati, i collegamenti prima and precedente devono essere disabilitati; Successivamente e ultima deve essere disabilitata quando si visualizza l'ultima pagina.

Aggiornare il metodo `BindUserAccounts` con il codice seguente:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample13.vb)]

Si noti che il numero totale di record in fase di paging tramite è determinato dall'ultimo parametro della `FindUsersByName` (metodo). Dopo la pagina specificata degli account utente vengono restituiti, i controlli LinkButton i quattro sono abilitato o disabilitato, a seconda del fatto che viene visualizzata prima o ultima pagina di dati.

L'ultimo passaggio consiste nello scrivere il codice per i quattro i controlli LinkButton `Click` gestori eventi. Questi gestori eventi necessario aggiornare il `PageIndex` proprietà e quindi associare di nuovo i dati a GridView mediante una chiamata a `BindUserAccounts` il primo, indietro e Avanti gestori di eventi sono molto semplici. Il `Click` gestore eventi per l'ultimo LinkButton, tuttavia, è un po' più complesso perché è necessario determinare il numero di record viene visualizzato per determinare l'ultimo indice di pagina.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample14.vb)]

Figure 8 e 9 di visualizzare l'interfaccia di paging personalizzato in azione. Figura 8 mostra la `ManageUsers.aspx` pagina quando si visualizzano la prima pagina di dati per tutti gli account utente. Si noti che vengono visualizzati solo 10 degli account di 13. Selezionando il collegamento successivo o ultima causa un postback, gli aggiornamenti di `PageIndex` su 1 e associa la seconda pagina dell'utente account alla griglia (vedere la figura 9).


[![Vengono visualizzati il primo account utente di 10](building-an-interface-to-select-one-user-account-from-many-vb/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image22.png)

**Figura 8**: vengono visualizzati il primo account di utente 10 ([fare clic per visualizzare l'immagine con dimensioni normali](building-an-interface-to-select-one-user-account-from-many-vb/_static/image24.png))


[![Selezione del collegamento viene visualizzata la pagina secondo degli account utente](building-an-interface-to-select-one-user-account-from-many-vb/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image25.png)

**Figura 9**: selezione del collegamento viene visualizzato la seconda pagina di account utente ([fare clic per visualizzare l'immagine con dimensioni normali](building-an-interface-to-select-one-user-account-from-many-vb/_static/image27.png))


## <a name="summary"></a>Riepilogo

Gli amministratori spesso necessario selezionare un utente dall'elenco di account. Nelle esercitazioni precedenti abbiamo esaminato tramite un elenco di riepilogo a discesa popolato con gli utenti, ma questo approccio non è facilmente scalabile. In questa esercitazione è stata esaminata un'alternativa migliore: un'interfaccia filtrabile i cui risultati vengono visualizzati in un controllo GridView di paging. Con questa interfaccia utente, gli amministratori possono rapido ed efficiente individuare e selezionare un account utente tra migliaia.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Paging personalizzato in ASP.NET con SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [Paging efficiente in grandi quantità di dati](https://asp.net/learn/data-access/tutorial-25-vb.aspx)
- [In sequenza il proprio strumento Amministrazione sito Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla.com, ha lavorato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola  *[Sams Teach Yourself ASP.NET 2.0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. È possibile contattarlo Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata Alicja Maziarz. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami un messaggio nella

> [!div class="step-by-step"]
> [Precedente](unlocking-and-approving-user-accounts-cs.md)
> [Successivo](recovering-and-changing-passwords-vb.md)
