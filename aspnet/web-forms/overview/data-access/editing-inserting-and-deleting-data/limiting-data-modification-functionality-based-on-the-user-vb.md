---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
title: Limitazione della funzionalità di modifica dei dati in base all'utente (VB) | Microsoft Docs
author: rick-anderson
description: In un'applicazione web che consente agli utenti di modificare i dati, account utente diversi possono avere privilegi di modifica dei dati diversi. In questa esercitazione verrà esaminato come t...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 9dc264a6-feb8-474b-8b91-008c50708065
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
msc.type: authoredcontent
ms.openlocfilehash: 8633d6ba5d82dfa2ebbea6858eb89433573a775f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836783"
---
<a name="limiting-data-modification-functionality-based-on-the-user-vb"></a>Limitazione della funzionalità di modifica dei dati in base all'utente (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_VB.exe) o [Scarica il PDF](limiting-data-modification-functionality-based-on-the-user-vb/_static/datatutorial23vb1.pdf)

> In un'applicazione web che consente agli utenti di modificare i dati, account utente diversi possono avere privilegi di modifica dei dati diversi. In questa esercitazione verrà esaminato come modificare dinamicamente le funzionalità di modifica dei dati in base all'utente visita.


## <a name="introduction"></a>Introduzione

Un numero di applicazioni web che supporta gli account utente e offre diverse opzioni, report e funzionalità di base dell'utente connesso. Ad esempio, le esercitazioni illustrano potrebbe essere necessario consentire agli utenti da società il fornitore per l'accesso al sito e aggiornare le informazioni generali sui prodotti - il nome e la quantità per unità, ad esempio, insieme alle informazioni di fornitore, ad esempio il nome della società, indirizzo, le informazioni di contatto s e così via. Inoltre, si potrebbe essere opportuno includere alcuni account utente per gli utenti dalla nostra azienda in modo che possano accedere e aggiornare le informazioni sul prodotto, ad esempio unità in magazzino, livello di riordinamento e così via. L'applicazione web potrebbe anche consentire agli utenti anonimi da visitare (persone che non hanno effettuato l'accesso), ma sarà limitata a visualizzare solo i dati. Con tale utente account sistema posto, sarebbe opportuno i controlli Web dei dati in nostro pagine ASP.NET per offrire l'inserimento, la modifica e l'eliminazione di privilegi appropriati per l'utente attualmente connesso.

In questa esercitazione verrà esaminato come modificare dinamicamente le funzionalità di modifica dei dati in base all'utente visita. In particolare, si creerà una pagina che visualizza le informazioni di fornitori in un controllo DetailsView modificabile insieme a un controllo GridView in cui sono elencati i prodotti specificati dal fornitore. Se l'utente visitando la pagina dalla nostra società, è possibile: visualizzare le informazioni di qualsiasi fornitore s; modificare l'indirizzo; e modificare le informazioni per tutti i prodotti specificati dal fornitore. Se, tuttavia, l'utente proviene da una particolare azienda, questi possono solo visualizzare e modificare le proprie informazioni di indirizzo e potranno essere modificati solo i prodotti che non sono stati contrassegnati come non più disponibile.


[![Un utente dalla nostra azienda può modificare eventuali informazioni s fornitore](limiting-data-modification-functionality-based-on-the-user-vb/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image1.png)

**Figura 1**: un utente la nostra società possono modificare qualsiasi fornitore s informazioni ([fare clic per visualizzare l'immagine con dimensioni normali](limiting-data-modification-functionality-based-on-the-user-vb/_static/image3.png))


[![Un utente da un particolare fornitore possono solo visualizzare e modificare le relative informazioni](limiting-data-modification-functionality-based-on-the-user-vb/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image4.png)

**Figura 2**: un utente da un particolare fornitore possono solo visualizzare e modificare le relative informazioni ([fare clic per visualizzare l'immagine con dimensioni normali](limiting-data-modification-functionality-based-on-the-user-vb/_static/image6.png))


Introduzione a ti permettono di s.

> [!NOTE]
> ASP.NET 2.0 il sistema di appartenenze s offre una piattaforma standardizzata ed estendibile per la creazione, la gestione e la convalida degli account utente. Poiché un esame del sistema di appartenenze è oltre l'ambito delle esercitazioni seguenti, in questa esercitazione invece "fakes" appartenenza in quanto consente visitatori anonimi scegliere se sono da un particolare fornitore o dalla nostra azienda. Per altre informazioni sull'appartenenza, fare riferimento alla mia [analisi ASP.NET 2.0 s appartenenza, ruoli e profilo](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx) serie di articoli.


## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>Passaggio 1: Consentire all'utente di specificare i diritti di accesso

In un'applicazione web reale, informazioni su account utente s includerà se hanno collaborato per la nostra azienda o per un particolare fornitore e tali informazioni saranno accessibili a livello di codice da alle pagine ASP.NET dopo che l'utente ha effettuato l'accesso al sito. Queste informazioni può essere acquisite tramite ASP.NET 2.0 sistema ruoli, come informazioni dell'account a livello di utente attraverso il sistema di profilo, o tramite alcuni metodi personalizzati.

Poiché l'obiettivo di questa esercitazione è illustrare modificando le funzionalità di modifica dei dati in base all'utente connesso e non è destinato a ASP.NET 2.0 in evidenza s appartenenza, ruoli e i sistemi di profilo, si userà un meccanismo molto semplice per determinare il funzionalità per l'utente visitando la pagina - un controllo DropDownList da cui l'utente può indicare se deve essere in grado di visualizzare e modificare tutte le informazioni di fornitori o, in alternativa, quali informazioni particolare fornitore s possono visualizzare e modificare. Se l'utente indica che potrà quindi visualizzare e modificare tutte le informazioni di fornitore (predefinito), Anna può spostarsi tra tutti i fornitori, modificare le informazioni sugli indirizzi s fornitore e modificare il nome e la quantità per unità per i prodotti specificati dal fornitore selezionato. Se l'utente indica che può solo visualizzare e modificare un particolare fornitore, tuttavia, Anna può solo visualizzare i dettagli e i prodotti per tale uno fornitore e quindi può solo aggiornare il nome e una quantità per informazioni sulle unità per i prodotti *non* non più disponibile.

Il primo passaggio in questa esercitazione, è quindi, per creare questo DropDownList e popolarla con i fornitori nel sistema. Aprire il `UserLevelAccess.aspx` nella pagina la `EditInsertDelete` cartella, aggiungere un controllo DropDownList cui `ID` è impostata su `Suppliers`e associare questo controllo DropDownList per un nuovo oggetto ObjectDataSource denominato `AllSuppliersDataSource`.


[![Creare un nuovo oggetto ObjectDataSource denominato AllSuppliersDataSource](limiting-data-modification-functionality-based-on-the-user-vb/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image7.png)

**Figura 3**: creare un nuovo oggetto ObjectDataSource denominato `AllSuppliersDataSource` ([fare clic per visualizzare l'immagine con dimensioni normali](limiting-data-modification-functionality-based-on-the-user-vb/_static/image9.png))


Poiché si desidera che questo controllo DropDownList per includere tutti i fornitori, configurare ObjectDataSource per richiamare il `SuppliersBLL` classe s `GetSuppliers()` (metodo). Assicurarsi inoltre che gli oggetti ObjectDataSource `Update()` metodo viene eseguito il mapping per il `SuppliersBLL` classe s `UpdateSupplierAddress` metodo, come ObjectDataSource verrà anche essere usato da DetailsView verranno aggiunte nel passaggio 2.

Dopo aver completato la procedura guidata ObjectDataSource, completare i passaggi configurando il `Suppliers` DropDownList tale che mostra il `CompanyName` campo dati e il `SupplierID` il valore per ogni campo di dati `ListItem`.


[![Configurare il controllo DropDownList fornitori per usare il CompanyName e campi dati SupplierID](limiting-data-modification-functionality-based-on-the-user-vb/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image10.png)

**Figura 4**: configurare il `Suppliers` DropDownList per utilizzare il `CompanyName` e `SupplierID` campi dati ([fare clic per visualizzare l'immagine con dimensioni normali](limiting-data-modification-functionality-based-on-the-user-vb/_static/image12.png))


A questo punto, il controllo DropDownList Elenca i nomi società dei fornitori nel database. Tuttavia, è anche necessario includere un'opzione "Mostra o modifica tutti i fornitori" per il controllo DropDownList. A tale scopo, impostare il `Suppliers` s DropDownList `AppendDataBoundItems` proprietà `true` e quindi aggiungere un `ListItem` cui `Text` proprietà è "Mostra o modifica tutti i fornitori" e il cui valore è `-1`. Questo può essere aggiunto direttamente tramite markup dichiarativo o tramite la finestra di progettazione accedendo alla finestra proprietà e facendo clic sui puntini di sospensione in s DropDownList `Items` proprietà.

> [!NOTE]
> Vedere la [ *Master/Dettagli filtro con un controllo DropDownList* ](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) esercitazione per informazioni più dettagliate sull'aggiunta di un elemento Seleziona tutto a un controllo DropDownList con associazione a dati.


Dopo il `AppendDataBoundItems` proprietà è stata impostata e `ListItem` aggiunto, il markup dichiarativo s DropDownList dovrebbe essere, ad esempio:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample1.aspx)]

Figura 5 mostra una schermata di stato di avanzamento corrente, quando viene visualizzato tramite un browser.


[![DropDownList Suppliers contiene una presentazione ListItem tutti, più uno per ogni fornitore](limiting-data-modification-functionality-based-on-the-user-vb/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image13.png)

**Figura 5**: il `Suppliers` DropDownList contiene Mostra tutto `ListItem`, oltre a uno per ogni fornitore ([fare clic per visualizzare l'immagine con dimensioni normali](limiting-data-modification-functionality-based-on-the-user-vb/_static/image15.png))


Poiché si vuole aggiornare l'interfaccia utente immediatamente dopo che l'utente ha modificato la selezione, impostare il `Suppliers` s DropDownList `AutoPostBack` proprietà `true`. Nel passaggio 2 si creerà un controllo DetailsView che mostra le informazioni per i suoi fornitori in base alla selezione DropDownList. Quindi, nel passaggio 3, creeremo un gestore eventi per questo s DropDownList `SelectedIndexChanged` evento, in cui verrà aggiunto codice che associa le informazioni appropriate supplier a DetailsView in base al fornitore selezionato.

## <a name="step-2-adding-a-detailsview-control"></a>Passaggio 2: Aggiungere un controllo DetailsView

Consentire s di usare un controllo DetailsView per mostrare informazioni relative al fornitore. Per l'utente che è possibile visualizzare e modificare tutti i fornitori, DetailsView supporterà il paging, consentendo all'utente di scorrere la supplier informazioni al record alla volta. Se l'utente appropriato per un particolare fornitore, tuttavia, DetailsView verrà visualizzato solo quel particolare fornitore informazioni s e non include un'interfaccia di paging. In entrambi i casi, deve consentire all'utente di modificare l'indirizzo fornitore, città e paese campi DetailsView.

Aggiungere un controllo DetailsView alla pagina sotto il `Suppliers` DropDownList, impostare relativo `ID` proprietà `SupplierDetails`e associarlo al `AllSuppliersDataSource` ObjectDataSource creato nel passaggio precedente. Successivamente, controllare le caselle di controllo Abilita modifica e attiva Paging nello smart tag s DetailsView.

> [!NOTE]
> Se non si t visualizzata un'opzione Abilita modifica in s DetailsView smart tag s perché non è stato mappato s ObjectDataSource `Update()` metodo per il `SuppliersBLL` classe s `UpdateSupplierAddress` (metodo). Si consiglia di tornare indietro e modificare questa configurazione, dopo che l'opzione Abilita modifica dovrebbe essere visualizzato nello smart tag s DetailsView.


Poiché il `SuppliersBLL` classe s `UpdateSupplierAddress` metodo accetti solo quattro parametri: `supplierID`, `address`, `city`, e `country` -modificare i BoundField di DetailsView s in modo che il `CompanyName` e `Phone` BoundField sono di sola lettura. Inoltre, rimuovere il `SupplierID` BoundField completamente. Infine, il `AllSuppliersDataSource` ObjectDataSource è attualmente relativo `OldValuesParameterFormatString` impostata su `original_{0}`. Si consiglia di rimuovere questa impostazione di proprietà da completamente di sintassi dichiarativa o impostarlo sul valore predefinito, `{0}`.

Dopo aver configurato il `SupplierDetails` DetailsView e `AllSuppliersDataSource` ObjectDataSource, si avrà il seguente markup dichiarativo:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample2.aspx)]

A questo punto DetailsView possono essere inviati tramite cercapersone tramite e possono essere aggiornate le informazioni sull'indirizzo fornitore selezionato s, indipendentemente dalla selezione effettuata nella `Suppliers` DropDownList (vedere la figura 6).


[![Qualsiasi informazione di fornitori può essere visualizzati e il relativo indirizzo aggiornato](limiting-data-modification-functionality-based-on-the-user-vb/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image16.png)

**Figura 6**: Any fornitori informazioni possono essere visualizzate e aggiornato l'indirizzo relativo ([fare clic per visualizzare l'immagine con dimensioni normali](limiting-data-modification-functionality-based-on-the-user-vb/_static/image18.png))


## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>Passaggio 3: Visualizzando solo le informazioni di s fornitore selezionato

La pagina attualmente Visualizza le informazioni per tutti i suoi fornitori indipendentemente dal fatto che un fornitore particolare è stato selezionato dal `Suppliers` DropDownList. Per visualizzare solo le informazioni sui fornitori per il fornitore selezionato è necessario aggiungere un altro oggetto ObjectDataSource alla pagina, che recupera informazioni su un particolare fornitore.

Aggiungere un nuovo oggetto ObjectDataSource alla pagina, denominarlo `SingleSupplierDataSource`. Dal suo smart tag, fare clic sul collegamento Configura origine dati e che usi il `SuppliersBLL` classe s `GetSupplierBySupplierID(supplierID)` (metodo). Come con le `AllSuppliersDataSource` ObjectDataSource, hanno il `SingleSupplierDataSource` s ObjectDataSource `Update()` metodo di cui è stato eseguito il mapping al `SuppliersBLL` classe s `UpdateSupplierAddress` (metodo).


[![Configurare SingleSupplierDataSource ObjectDataSource per usare il metodo GetSupplierBySupplierID(supplierID)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image19.png)

**Figura 7**: configurare il `SingleSupplierDataSource` ObjectDataSource per utilizzare il `GetSupplierBySupplierID(supplierID)` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](limiting-data-modification-functionality-based-on-the-user-vb/_static/image21.png))


Successivamente, viene nuovamente richiesto di specificare l'origine del parametro per il `GetSupplierBySupplierID(supplierID)` metodo s `supplierID` parametro di input. Poiché si vogliono visualizzare le informazioni per il fornitore selezionato da DropDownList, usare il `Suppliers` DropDownList s `SelectedValue` proprietà come origine del parametro.


[![Usare il controllo DropDownList Suppliers come origine del parametro supplierID](limiting-data-modification-functionality-based-on-the-user-vb/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image22.png)

**Figura 8**: usare il `Suppliers` DropDownList come le `supplierID` origine del parametro ([fare clic per visualizzare l'immagine con dimensioni normali](limiting-data-modification-functionality-based-on-the-user-vb/_static/image24.png))


Anche con questo secondo ObjectDataSource aggiunto, il controllo DetailsView è attualmente configurato per usare sempre la `AllSuppliersDataSource` ObjectDataSource. È necessario aggiungere la logica necessaria per modificare l'origine dati utilizzata dal controllo DetailsView. a seconda di `Suppliers` DropDownList elemento selezionato. A questo scopo, creare un `SelectedIndexChanged` gestore eventi per il controllo DropDownList Suppliers. Ciò è possibile creare più facilmente facendo doppio clic su DropDownList nella finestra di progettazione. Questo gestore eventi deve determinare l'origine dati da usare e deve riassociare i dati in DetailsView. Questa operazione viene eseguita con il codice seguente:


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample3.vb)]

Il gestore dell'evento inizia determinando se è stata selezionata l'opzione "Mostra o modifica tutti i fornitori". Se si tratta, imposta il `SupplierDetails` s DetailsView `DataSourceID` a `AllSuppliersDataSource` e restituisce all'utente per il primo record nel set di fornitori impostando il `PageIndex` proprietà su 0. Se, tuttavia, l'utente ha selezionato un particolare fornitore da DropDownList, s DetailsView `DataSourceID` viene assegnato a `SingleSuppliersDataSource`. Indipendentemente dal fatto che i dati viene usata l'origine, il `SuppliersDetails` modalità viene ripristinata la modalità di sola lettura e la data è riassociata per DetailsView da una chiamata ai `SuppliersDetails` controllo s `DataBind()` (metodo).

A questo gestore eventi posto, il controllo DetailsView Mostra ora il fornitore selezionato, a meno che non è stata selezionata l'opzione "Mostra o modifica tutti i fornitori", nel qual caso tutti i fornitori possono essere visualizzati tramite l'interfaccia di paging. Figura 9 mostra la pagina con l'opzione "Mostra o modifica tutti i fornitori" selezionata; Si noti che l'interfaccia di paging è presente, che consente all'utente di visitare e aggiornare qualsiasi fornitore. Figura 10 Mostra la pagina con il fornitore Ma Maison selezionato. Solo le informazioni s Ma Maison sono visualizzabile e modificabile in questo caso.


[![Tutte le informazioni di fornitori possono essere visualizzate e modificate](limiting-data-modification-functionality-based-on-the-user-vb/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image25.png)

**Figura 9**: tutti i suoi fornitori informazioni possono essere visualizzate e modificate ([fare clic per visualizzare l'immagine con dimensioni normali](limiting-data-modification-functionality-based-on-the-user-vb/_static/image27.png))


[![Solo le informazioni selezionate Supplier s possono essere visualizzate e modificate](limiting-data-modification-functionality-based-on-the-user-vb/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image28.png)

**Figura 10**: solo la s fornitore selezionato le informazioni può essere visualizzabile e modificata ([fare clic per visualizzare l'immagine con dimensioni normali](limiting-data-modification-functionality-based-on-the-user-vb/_static/image30.png))


> [!NOTE]
> Per questa esercitazione, il controllo DropDownList e DetailsView controllare s `EnableViewState` deve essere impostata su `true` (predefinito) perché la s DropDownList `SelectedIndex` e gli oggetti DetailsView `DataSourceID` le modifiche alle proprietà s devono essere memorizzati nella navigazione tra i postback.


## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>Passaggio 4: Elenco dei prodotti di fornitori in un controllo GridView modificabile

Con DetailsView completa, il passaggio successivo consiste nell'includere un GridView modificabile in cui sono elencati i prodotti specificati dal fornitore selezionato. Questo controllo GridView deve consentire le modifiche solo per i `ProductName` e `QuantityPerUnit` campi. Inoltre, se l'utente visitando la pagina proviene da un fornitore particolare, deve consentire solo gli aggiornamenti per i prodotti *non* non più disponibile. A tale scopo è necessario innanzitutto aggiungere un overload del `ProductsBLL` classe s `UpdateProducts` metodo che accetta solo la `ProductID`, `ProductName`, e `QuantityPerUnit` campi come input. Abbiamo ve una alla volta questo processo in anticipo nelle numerose esercitazioni sulla, ma vediamo s just esaminare il codice in questo caso, che deve essere aggiunto al `ProductsBLL`:


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample4.vb)]

Con questo overload creato, sono pronti per aggiungere il controllo GridView e relativo ObjectDataSource associata. Aggiungere un controllo GridView di nuovo alla pagina, impostare relativi `ID` proprietà `ProductsBySupplier`e configurarlo per usare un nuovo oggetto ObjectDataSource denominato `ProductsBySupplierDataSource`. Poiché si desidera che questo controllo GridView per visualizzare l'elenco di tali prodotti dal fornitore selezionato, usare il `ProductsBLL` classe s `GetProductsBySupplierID(supplierID)` (metodo). Anche eseguire il mapping di `Update()` metodo al nuovo `UpdateProduct` overload appena creato.


[![Configurare ObjectDataSource per usare l'Overload UpdateProduct appena creato](limiting-data-modification-functionality-based-on-the-user-vb/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image31.png)

**Figura 11**: configurare ObjectDataSource per usare il `UpdateProduct` Overload appena creato ([fare clic per visualizzare l'immagine con dimensioni normali](limiting-data-modification-functionality-based-on-the-user-vb/_static/image33.png))


Abbiamo re viene richiesto di selezionare l'origine del parametro per il `GetProductsBySupplierID(supplierID)` metodo s `supplierID` parametro di input. Poiché si vogliono visualizzare i prodotti per il fornitore selezionato nel controllo DetailsView, usare il `SuppliersDetails` controllo DetailsView s `SelectedValue` proprietà come origine del parametro.


[![Usare la proprietà SelectedValue s SuppliersDetails DetailsView come origine del parametro](limiting-data-modification-functionality-based-on-the-user-vb/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image34.png)

**Figura 12**: usare il `SuppliersDetails` s DetailsView `SelectedValue` proprietà come origine del parametro ([fare clic per visualizzare l'immagine con dimensioni normali](limiting-data-modification-functionality-based-on-the-user-vb/_static/image36.png))


Restituzione di GridView, rimuovere tutti i campi di GridView, ad eccezione di `ProductName`, `QuantityPerUnit`, e `Discontinued`, contrassegno di `Discontinued` CampoCasellaDiControllo in sola lettura. Inoltre, selezionare l'opzione Abilita modifica dallo smart tag s GridView. Dopo aver apportate queste modifiche, il markup dichiarativo per il controllo GridView e ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample5.aspx)]

Come con i nostri ObjectDataSource precedente, si tratta di uno `OldValuesParameterFormatString` è impostata su `original_{0}`, che si verificheranno problemi durante il tentativo di aggiornare un prodotto s nome o la quantità per unità. Rimuovere completamente questa proprietà con la sintassi dichiarativa o impostarlo sul valore predefinito, `{0}`.

Con questa configurazione è completata, la pagina sono ora elencati i prodotti specificati dal fornitore selezionato nel GridView (vedere la figura 13). Attualmente *qualsiasi* quantity per ogni unità o il nome del prodotto s possono essere aggiornati. Tuttavia, è necessario aggiornare la logica della pagina in modo che tale funzionalità non è consentito per i prodotti non più disponibili per gli utenti associati a un particolare fornitore. Che verranno affrontati in questo componente finale nel passaggio 5.


[![Vengono visualizzati i prodotti specificati dal fornitore selezionato](limiting-data-modification-functionality-based-on-the-user-vb/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image37.png)

**Figura 13**: The prodotti specificati dal fornitore selezionato vengono visualizzati ([fare clic per visualizzare l'immagine con dimensioni normali](limiting-data-modification-functionality-based-on-the-user-vb/_static/image39.png))


> [!NOTE]
> Con l'aggiunta di questo controllo GridView modificabile il `Suppliers` DropDownList s `SelectedIndexChanged` gestore dell'evento deve essere aggiornato per restituire il controllo GridView a uno stato di sola lettura. In caso contrario, se un fornitore diverso viene selezionato mentre è in corso la modifica delle informazioni sul prodotto, l'indice corrispondente in GridView per il nuovo fornitore sarà anche modificabile. Per evitare questo problema, è sufficiente impostare la s GridView `EditIndex` proprietà `-1` nel `SelectedIndexChanged` gestore dell'evento.


Inoltre, è importante ricordare che è importante che il controllo GridView lo stato di visualizzazione s essere abilitato (comportamento predefinito). Se si imposta la s GridView `EnableViewState` proprietà `false`, si corre il rischio di consentire agli utenti simultanei involontariamente l'eliminazione o modifica di record. Visualizzare [avviso: problema di concorrenza con ASP.NET 2.0 GridViews/DetailsView/FormViews tale supporto di modifica e/o l'eliminazione e il cui stato di visualizzazione è disabilitato](http://scottonwriting.net/sowblog/posts/10054.aspx) per altre informazioni.

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>Passaggio 5: Non consentire la modifica per non più disponibili prodotti quando Show o modificare tutti i fornitori non è selezionato

Mentre il `ProductsBySupplier` GridView è completamente funzionale, attualmente concede accesso eccessivo agli utenti che provengono da un fornitore particolare. Per le regole di business, tali utenti non devono essere in grado di aggiornare i prodotti non più utilizzati. Per applicare questo comportamento, è possibile nascondere o disabilitare il pulsante di modifica delle righe di GridView con i prodotti non più disponibili quando viene visitata la pagina da un utente da un fornitore.

Creare un gestore eventi per s GridView `RowDataBound` evento. In questo gestore eventi è necessario determinare se l'utente è associato a un particolare fornitore, che, per questa esercitazione, può essere determinato mediante il controllo DropDownList fornitori dispositivi `SelectedValue` proprietà - se si s diverso da -1, quindi l'utente è associato a un particolare fornitore. Per tali utenti, è necessario quindi determinare se il prodotto non è più disponibile. È possibile ottenere un riferimento a effettivi `ProductRow` istanza associato alla riga GridView tramite il `e.Row.DataItem` proprietà, come descritto nel [ *visualizzazione di riepilogo delle informazioni in GridView s piè di pagina* ](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) esercitazione. Se il prodotto non è più disponibile, è possibile ottenere un riferimento a livello di codice per il pulsante Modifica in GridView s CommandField utilizzando le tecniche descritte nell'esercitazione precedente, [ *aggiunta lato Client conferma quando l'eliminazione* ](adding-client-side-confirmation-when-deleting-vb.md). Dopo aver ottenuto un riferimento può essere quindi nascondere o disabilitare il pulsante.


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample6.vb)]

Con questo evento gestore sul posto, quando si visita questa pagina come un utente da un determinato fornitore tali prodotti che non sono più disponibili non sono modificabili, come il pulsante Modifica è nascosto per questi prodotti. Ad esempio, s Chef Anton's Gumbo combinazione è un prodotto non più disponibile per il fornitore di New Orleans Cajun coinvolgente. Quando si visita la pagina per questo particolare fornitore, il pulsante Modifica per questo prodotto è nascosto (vedere la figura 14). Tuttavia, quando si visita usando "Show o modificare tutti i fornitori", il pulsante Modifica è disponibile (vedere Figura 15).


[![Per utenti specifici per fornitore pulsante Modifica per s Chef Anton's Gumbo Mix è nascosta](limiting-data-modification-functionality-based-on-the-user-vb/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image40.png)

**Figura 14**: per utenti specifici per fornitore pulsante Modifica per s Chef Anton's Gumbo Mix viene nascosta ([fare clic per visualizzare l'immagine con dimensioni normali](limiting-data-modification-functionality-based-on-the-user-vb/_static/image42.png))


[![Per visualizzare o modificare tutti gli utenti di fornitori, viene visualizzato il pulsante Modifica per s Chef Anton's Gumbo Mix](limiting-data-modification-functionality-based-on-the-user-vb/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image43.png)

**Figura 15**: per visualizzare o modificare tutti i fornitori gli utenti, il pulsante Modifica per s Chef Anton's Gumbo Mix viene visualizzata ([fare clic per visualizzare l'immagine con dimensioni normali](limiting-data-modification-functionality-based-on-the-user-vb/_static/image45.png))


## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>Controllo dei diritti di accesso nel livello di logica di Business

In questa esercitazione ASP.NET pagina consente di gestire tutta la logica per quanto riguarda le informazioni che l'utente può visualizzare e quali prodotti è possibile aggiornare. In teoria, questa logica anche sarebbe stata disponibile a livello di logica di Business. Ad esempio, il `SuppliersBLL` classe s `GetSuppliers()` metodo, che restituisce tutti i fornitori, potrebbe includere un controllo per assicurarsi che l'utente attualmente connesso sia *non* associata a un determinato fornitore. Analogamente, il `UpdateSupplierAddress` metodo potrebbe includere un controllo per assicurarsi che l'utente attualmente connesso sia lavorato per la nostra azienda (e pertanto può aggiornare tutte le informazioni sull'indirizzo di fornitori) o è associato il fornitore i cui dati vengono aggiornati.

Tali controlli a livello BLL qui non ho incluso perché in questa esercitazione i diritti utente s sono determinati da un controllo DropDownList nella pagina, che non possono accedere le classi BLL. Quando si usa il sistema di appartenenze o uno degli schemi di autenticazione out-pronte forniti da ASP.NET (ad esempio l'autenticazione di Windows), attualmente connesso in assegnare agli utenti informazioni e delle informazioni sui ruoli è possibile accedere dal livello BLL, in modo da ottenere autorizzazioni di accesso diritti controlla possibili nel server di presentazione e livelli BLL.

## <a name="summary"></a>Riepilogo

La maggior parte dei siti che forniscono gli account utente necessario personalizzare l'interfaccia di modifica dei dati in base all'utente connesso. Utenti con privilegi amministrativi potrebbero essere possibile eliminare e modificare qualsiasi record, mentre gli utenti non amministratori possono essere limitati solo l'aggiornamento o eliminazione dei record creati da loro. Potrebbe essere insomma, i controlli Web, ObjectDataSource, dati e le classi di livello per la logica di Business possono essere esteso per aggiungere o impedire determinate funzionalità basate sull'utente attualmente connesso al computer. In questa esercitazione è stato illustrato come limitare i dati visualizzabili e modificabili a seconda che l'utente sia stato associato a un particolare fornitore o se hanno collaborato per la nostra azienda.

Questa esercitazione è terminata l'esame di inserimento, aggiornamento ed eliminazione dei dati tramite i controlli GridView, DetailsView e FormView. Iniziando con l'esercitazione successiva, si sarà concentrare l'attenzione all'aggiunta di supporto per l'ordinamento e paging.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](adding-client-side-confirmation-when-deleting-vb.md)
