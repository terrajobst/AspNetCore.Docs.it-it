---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs
title: Limitazione della funzionalità di modifica dei dati in base all'utente (c#) | Documenti Microsoft
author: rick-anderson
description: In un'applicazione web che consente agli utenti di modificare i dati, account utente diversi possono avere privilegi di modifica dei dati diversi. In questa esercitazione si esamineranno come t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 2b251c82-77cf-4e36-baa9-b648eddaa394
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs
msc.type: authoredcontent
ms.openlocfilehash: b056536eeaa86ef2c73debe23dd38861f41b2a69
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="limiting-data-modification-functionality-based-on-the-user-c"></a>Limitazione della funzionalità di modifica dei dati in base all'utente (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_CS.exe) o [Scarica il PDF](limiting-data-modification-functionality-based-on-the-user-cs/_static/datatutorial23cs1.pdf)

> In un'applicazione web che consente agli utenti di modificare i dati, account utente diversi possono avere privilegi di modifica dei dati diversi. In questa esercitazione si esamineranno come modificare dinamicamente le funzionalità di modifica di dati in base all'utente di accesso.


## <a name="introduction"></a>Introduzione

Un numero di applicazioni web supporta gli account utente e fornisca diverse opzioni, report e funzionalità in base all'utente connesso. Ad esempio, con le esercitazioni potrebbe essere opportuno consentire agli utenti di effettuare l'accesso per il sito e aggiornare le informazioni generali sui propri prodotti - il nome e la quantità per unità, ad esempio, insieme alle informazioni fornitore, ad esempio il nome della società, società fornitore indirizzo, informazioni di contatto s e così via. Inoltre, potrebbe essere opportuno includere alcuni account utente per persone della società, in modo che possano accedere e aggiornare le informazioni di prodotto, come unità in magazzino, livello di riordinamento e così via. L'applicazione web potrebbe anche consentire agli utenti anonimi di visitare (persone che non hanno effettuato l'accesso), ma sarà limitata al solo la visualizzazione dei dati. Con tale utente account sistema sul posto, vogliamo i controlli Web di dati in nostro pagine ASP.NET per offrire l'inserimento, modifica ed eliminazione di privilegi appropriati per l'utente attualmente connesso.

In questa esercitazione si esamineranno come modificare dinamicamente le funzionalità di modifica di dati in base all'utente di accesso. In particolare, si creerà una pagina che visualizza le informazioni di fornitori in un controllo DetailsView modificabile insieme a un controllo GridView in cui sono elencati i prodotti specificati dal fornitore. Se l'utente visita la pagina dalla società, è possibile: visualizzare le informazioni di qualsiasi fornitore s; modificare il proprio indirizzo; modificare le informazioni per i prodotti specificati dal fornitore. Se, tuttavia, l'utente da una determinata società, possano solo visualizzare e modificare le proprie informazioni sull'indirizzo e può essere modificata solo i prodotti che non sono stati contrassegnati come non più disponibile.


[![L'utente dalla società di modificare qualsiasi informazione s fornitore](limiting-data-modification-functionality-based-on-the-user-cs/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image1.png)

**Figura 1**: un utente dal nostro società possono modificare qualsiasi fornitore s informazioni ([fare clic per visualizzare l'immagine ingrandita](limiting-data-modification-functionality-based-on-the-user-cs/_static/image3.png))


[![Un utente da un fornitore particolare può solo visualizzare e modificare le relative informazioni](limiting-data-modification-functionality-based-on-the-user-cs/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image4.png)

**Figura 2**: un utente da un particolare fornitore possono solo visualizzare e modificare le relative informazioni ([fare clic per visualizzare l'immagine ingrandita](limiting-data-modification-functionality-based-on-the-user-cs/_static/image6.png))


Let s iniziare!

> [!NOTE]
> ASP.NET 2.0 il sistema di appartenenze s offre una piattaforma di standardizzato ed estendibile per la creazione, gestione e la convalida degli account utente. Poiché un esame di sistema di appartenenze non rientra nell'ambito di queste esercitazioni, in questa esercitazione invece "fakes" appartenenza consentendo ai visitatori anonimi di scegliere se da un particolare fornitore o dalla società. Per ulteriori informazioni sull'appartenenza, vedere il [esame ASP.NET 2.0 s appartenenza, ruoli e profilo](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx) articolo serie.


## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>Passaggio 1: Consente di specificare i diritti di accesso

In un'applicazione web del mondo reale, include informazioni su account utente s se hanno lavorato per l'azienda o per un particolare fornitore e tali informazioni saranno accessibili a livello di codice dalle pagine ASP.NET quando l'utente ha effettuato l'accesso al sito. Queste informazioni potrebbero essere acquisite tramite sistema ruoli di ASP.NET 2.0 s, come informazioni dell'account a livello di utente attraverso il sistema di profilo o tramite uno strumento personalizzato.

Poiché l'obiettivo di questa esercitazione è dimostrare regolando le funzionalità di modifica di dati in base all'utente connesso e non intende showcase ASP.NET 2.0 s appartenenza, ruoli e i sistemi di profilo, verrà usato un meccanismo molto semplice per determinare il funzionalità per l'utente visita la pagina - un DropDownList da cui l'utente può indicare se deve essere in grado di visualizzare e modificare le informazioni di fornitori o, in alternativa, le informazioni relative al fornitore particolare s possono visualizzare e modificare. Se l'utente indica che potrà visualizzare e modificare tutte le informazioni fornitore (impostazione predefinita), potrà scorrere tutti i fornitori, modificare le informazioni di indirizzo fornitore s e modificare il nome e la quantità per unità per i prodotti specificati dal fornitore selezionato. Se l'utente indica che può solo visualizzare e modificare un particolare fornitore, tuttavia, quindi Lei possono solo visualizzare i dettagli e i prodotti che un fornitore e possono solo aggiornare il nome e la quantità per informazioni sull'unità per i prodotti che sono *non* non più disponibile.

Il primo passaggio in questa esercitazione, è quindi, per creare questo DropDownList e popolarlo con i fornitori nel sistema. Aprire il `UserLevelAccess.aspx` nella pagina di `EditInsertDelete` cartella, aggiungere un controllo DropDownList cui `ID` è impostata su `Suppliers`e associare questa DropDownList a un nuovo oggetto ObjectDataSource denominato `AllSuppliersDataSource`.


[![Creare un nuovo oggetto ObjectDataSource denominato AllSuppliersDataSource](limiting-data-modification-functionality-based-on-the-user-cs/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image7.png)

**Figura 3**: creare un nuovo denominato ObjectDataSource `AllSuppliersDataSource` ([fare clic per visualizzare l'immagine ingrandita](limiting-data-modification-functionality-based-on-the-user-cs/_static/image9.png))


Poiché si desidera che questa DropDownList per includere tutti i fornitori, configurare ObjectDataSource per richiamare il `SuppliersBLL` classe s `GetSuppliers()` metodo. Verificare inoltre che i dispositivi ObjectDataSource `Update()` metodo viene eseguito il mapping per il `SuppliersBLL` classe s `UpdateSupplierAddress` metodo, come ObjectDataSource verrà inoltre utilizzato dal controllo DetailsView verranno aggiunte nel passaggio 2.

Dopo aver completato la procedura guidata ObjectDataSource, completare i passaggi configurando il `Suppliers` DropDownList in modo che mostra il `CompanyName` campo dei dati e viene utilizzato il `SupplierID` come valore per ogni campo dei dati `ListItem`.


[![Configurare DropDownList fornitori per utilizzare i campi di dati SupplierID e CompanyName](limiting-data-modification-functionality-based-on-the-user-cs/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image10.png)

**Figura 4**: configurare il `Suppliers` DropDownList per utilizzare il `CompanyName` e `SupplierID` campi dati ([fare clic per visualizzare l'immagine ingrandita](limiting-data-modification-functionality-based-on-the-user-cs/_static/image12.png))


A questo punto, DropDownList Elenca i nomi di società dei fornitori nel database. Tuttavia, è anche necessario includere un'opzione "Visualizza/Modifica tutti i fornitori" per DropDownList. A tale scopo, impostare il `Suppliers` s DropDownList `AppendDataBoundItems` proprietà `true` e quindi aggiungere un `ListItem` cui `Text` proprietà è "Visualizza/Modifica tutti i fornitori" e il cui valore è `-1`. Questo può essere aggiunto direttamente tramite markup dichiarativo o tramite la finestra di progettazione selezionando la finestra proprietà e fare clic sui puntini di sospensione in s DropDownList `Items` proprietà.

> [!NOTE]
> Fare riferimento al [ *Master-Details filtro con un DropDownList* ](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) esercitazione per informazioni più dettagliate sull'aggiunta di un elemento Seleziona tutto a un DropDownList con associazione a dati.


Dopo il `AppendDataBoundItems` proprietà è stata impostata e `ListItem` aggiunto, il markup dichiarativo s DropDownList dovrebbe essere simile:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample1.aspx)]

Figura 5 mostra una schermata dello stato di avanzamento corrente, quando viene visualizzato tramite un browser.


[![DropDownList Suppliers contiene una presentazione ListItem tutti, più uno per ogni fornitore](limiting-data-modification-functionality-based-on-the-user-cs/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image13.png)

**Figura 5**: il `Suppliers` DropDownList contiene Mostra tutto `ListItem`, più uno per ogni fornitore ([fare clic per visualizzare l'immagine ingrandita](limiting-data-modification-functionality-based-on-the-user-cs/_static/image15.png))


Poiché si desidera aggiornare l'interfaccia utente immediatamente dopo che l'utente ha modificato la selezione, impostare il `Suppliers` s DropDownList `AutoPostBack` proprietà `true`. Nel passaggio 2 si creerà un controllo DetailsView che verrà visualizzate le informazioni per i suoi fornitori in base alla selezione DropDownList. Quindi, nel passaggio 3, si creerà un gestore eventi per questo s DropDownList `SelectedIndexChanged` evento, in cui verrà aggiunto il codice che esegue l'associazione al controllo DetailsView informazioni sul fornitore appropriato in base al fornitore selezionato.

## <a name="step-2-adding-a-detailsview-control"></a>Passaggio 2: Aggiunta di un controllo DetailsView

Consente di utilizzare un controllo DetailsView per visualizzare informazioni relative al fornitore s. Per l'utente che è possibile visualizzare e modificare tutti i fornitori, DetailsView supporterà il paging, consentendo all'utente di scorrere il record di fornitore informazioni uno alla volta. Se l'utente appropriato per un particolare fornitore, tuttavia, controllo DetailsView verrà visualizzato solo quel particolare fornitore informazioni s e non include un'interfaccia di paging. In entrambi i casi, controllo DetailsView è necessario consentire all'utente di modificare il fornitore s indirizzo, città e campi del paese.

Aggiungere un controllo DetailsView sotto il `Suppliers` DropDownList, impostare il relativo `ID` proprietà `SupplierDetails`e associarlo al `AllSuppliersDataSource` ObjectDataSource creato nel passaggio precedente. Selezionare le caselle di controllo Abilita Paging e abilitare la modifica dallo smart tag s di DetailsView.

> [!NOTE]
> Se non si t viene visualizzata un'opzione Abilita modifica in s DetailsView smart tag s perché non è stato mappato s ObjectDataSource `Update()` metodo il `SuppliersBLL` classe s `UpdateSupplierAddress` metodo. Richiedere qualche istante per tornare indietro e modificare questa configurazione, dopo che l'opzione Abilita modifica dovrebbe essere visualizzato nello smart tag s DetailsView.


Poiché il `SuppliersBLL` classe s `UpdateSupplierAddress` metodo accetta solo quattro parametri - `supplierID`, `address`, `city`, e `country` -modificare gli oggetti DetailsView BoundField in modo che il `CompanyName` e `Phone` BoundField sono di sola lettura. Inoltre, rimuovere il `SupplierID` BoundField completamente. Infine, il `AllSuppliersDataSource` ObjectDataSource è attualmente relativo `OldValuesParameterFormatString` proprietà impostata su `original_{0}`. È opportuno rimuovere completamente la sintassi dichiarativa di impostazione di questa proprietà o impostarla sul valore predefinito, `{0}`.

Dopo aver configurato il `SupplierDetails` DetailsView e `AllSuppliersDataSource` ObjectDataSource, si avrà il seguente markup dichiarativo:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample2.aspx)]

A questo punto il controllo DetailsView possono essere trasferite tramite e le informazioni sull'indirizzo fornitore selezionato s può essere aggiornati, indipendentemente dalla selezione effettuata nel `Suppliers` DropDownList (vedere Figura 6).


[![Qualsiasi informazione di fornitori può essere visualizzati e il relativo indirizzo aggiornato](limiting-data-modification-functionality-based-on-the-user-cs/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image16.png)

**Figura 6**: qualsiasi Suppliers informazioni possono essere visualizzate e aggiornato l'indirizzo relativo ([fare clic per visualizzare l'immagine ingrandita](limiting-data-modification-functionality-based-on-the-user-cs/_static/image18.png))


## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>Passaggio 3: Visualizzando solo le informazioni di s fornitore selezionato

La pagina attualmente Visualizza le informazioni per tutti i fornitori, indipendentemente dal fatto che un particolare fornitore è stato selezionato dal `Suppliers` DropDownList. Per visualizzare solo le informazioni sul fornitore per il fornitore selezionato è necessario aggiungere un altro ObjectDataSource per la pagina, che recupera informazioni su un particolare fornitore.

Aggiungere un nuovo oggetto ObjectDataSource, denominarla `SingleSupplierDataSource`. Dal suo smart tag, fare clic sul collegamento Configura origine dati e fare in modo utilizzare i `SuppliersBLL` classe s `GetSupplierBySupplierID(supplierID)` metodo. Come con la `AllSuppliersDataSource` ObjectDataSource, hanno il `SingleSupplierDataSource` ObjectDataSource s `Update()` mapping del metodo di `SuppliersBLL` classe s `UpdateSupplierAddress` (metodo).


[![Configurare SingleSupplierDataSource ObjectDataSource per utilizzare il metodo GetSupplierBySupplierID(supplierID)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image19.png)

**Figura 7**: configurare il `SingleSupplierDataSource` ObjectDataSource per utilizzare il `GetSupplierBySupplierID(supplierID)` metodo ([fare clic per visualizzare l'immagine ingrandita](limiting-data-modification-functionality-based-on-the-user-cs/_static/image21.png))


Successivamente, abbiamo re richiesto di specificare l'origine del parametro per il `GetSupplierBySupplierID(supplierID)` metodo s `supplierID` parametro di input. Poiché si desidera visualizzare le informazioni per il fornitore selezionato da DropDownList, utilizzare il `Suppliers` s DropDownList `SelectedValue` proprietà come origine del parametro.


[![Utilizzare DropDownList Suppliers come supplierID origine parametro](limiting-data-modification-functionality-based-on-the-user-cs/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image22.png)

**Figura 8**: usare il `Suppliers` DropDownList come il `supplierID` parametro Source ([fare clic per visualizzare l'immagine ingrandita](limiting-data-modification-functionality-based-on-the-user-cs/_static/image24.png))


Anche con questo secondo ObjectDataSource aggiunto, il controllo DetailsView è attualmente configurato per utilizzare sempre il `AllSuppliersDataSource` ObjectDataSource. È necessario aggiungere la logica necessaria per modificare l'origine dati utilizzata dal controllo DetailsView a seconda di `Suppliers` DropDownList elemento selezionato. A tale scopo, creare un `SelectedIndexChanged` gestore eventi per DropDownList fornitori. È possibile creare più facilmente facendo doppio clic su DropDownList nella finestra di progettazione. Questo gestore eventi deve determinare l'origine dati da utilizzare e deve riassociare i dati di DetailsView. Questa operazione viene eseguita con il codice seguente:


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample3.cs)]

Il gestore dell'evento inizia determinando se è stata selezionata l'opzione "Visualizza/Modifica tutti i fornitori". Se è stato, imposta il `SupplierDetails` DetailsView s `DataSourceID` a `AllSuppliersDataSource` e restituisce l'utente al primo record nel set di fornitori impostando il `PageIndex` proprietà su 0. Se, tuttavia, l'utente ha selezionato un particolare fornitore da DropDownList, s DetailsView `DataSourceID` viene assegnato a `SingleSuppliersDataSource`. Indipendentemente dalle quali dati viene utilizzata l'origine, il `SuppliersDetails` modalità verrà ripristinata la modalità di sola lettura e i dati vengano riassociati al controllo DetailsView da una chiamata al `SuppliersDetails` controllo s `DataBind()` metodo.

A questo gestore eventi sul posto, il controllo DetailsView Mostra ora il fornitore selezionato, a meno che non è stata selezionata l'opzione "Visualizza/Modifica tutti i fornitori", nel qual caso tutti i fornitori possono essere visualizzati attraverso l'interfaccia di paging. Figura 9 è illustrata la pagina con l'opzione "Visualizza/Modifica tutti i fornitori" selezionata; Si noti che l'interfaccia di paging è presente, che consente all'utente a visitare e aggiornare qualsiasi fornitore. Figura 10 è illustrata la pagina con il fornitore Ma Maison selezionato. Solo le informazioni s Ma Maison visualizzabili e modificabili in questo caso sono.


[![Tutte le informazioni di fornitori possono essere visualizzate e modificate](limiting-data-modification-functionality-based-on-the-user-cs/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image25.png)

**Figura 9**: tutti i fornitori informazioni possono essere visualizzate e modificate ([fare clic per visualizzare l'immagine ingrandita](limiting-data-modification-functionality-based-on-the-user-cs/_static/image27.png))


[![Solo le informazioni del fornitore selezionato s possono essere visualizzate e modificate](limiting-data-modification-functionality-based-on-the-user-cs/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image28.png)

**Figura 10**: solo i dispositivi selezionati Supplier informazioni possono essere visualizzabile e modificato ([fare clic per visualizzare l'immagine ingrandita](limiting-data-modification-functionality-based-on-the-user-cs/_static/image30.png))


> [!NOTE]
> Per questa esercitazione, DropDownList sia il controllo DetailsView controllare s `EnableViewState` deve essere impostato su `true` (impostazione predefinita) perché s DropDownList `SelectedIndex` e s DetailsView `DataSourceID` le modifiche alle proprietà s devono essere memorizzate durante i postback.


## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>Passaggio 4: Elenco dei prodotti di fornitori in un controllo GridView. modificabile

Con controllo DetailsView completo, il passaggio successivo consiste nell'includere un GridView modificabile in cui sono elencati i prodotti specificati dal fornitore selezionato. Questo controllo GridView deve consentire le modifiche solo il `ProductName` e `QuantityPerUnit` campi. Inoltre, se l'utente visita la pagina proviene da un fornitore particolare, deve solo consentire gli aggiornamenti per i prodotti che sono *non* non più disponibile. A tale scopo è necessario aggiungere prima un overload di `ProductsBLL` classe s `UpdateProducts` metodo che accetta solo il `ProductID`, `ProductName`, e `QuantityPerUnit` campi come input. Si viene incrementata passo passo tramite questo processo in anticipo in numerose esercitazioni, è possibile s esaminare solo il codice, che deve essere aggiunto a `ProductsBLL`:


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample4.cs)]

Con questo overload creato, è nuovamente pronto per aggiungere il controllo GridView e ObjectDataSource associato. Aggiungere un nuovo GridView alla pagina, impostare il relativo `ID` proprietà `ProductsBySupplier`e configurarlo per l'utilizzo di un nuovo oggetto ObjectDataSource denominato `ProductsBySupplierDataSource`. Poiché si desidera che questa GridView per elencare i prodotti dal fornitore selezionato, utilizzare il `ProductsBLL` classe s `GetProductsBySupplierID(supplierID)` metodo. Anche eseguire il mapping di `Update()` il nuovo metodo `UpdateProduct` overload appena creato.


[![Configurare ObjectDataSource per utilizzare l'Overload UpdateProduct appena creato](limiting-data-modification-functionality-based-on-the-user-cs/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image31.png)

**Figura 11**: configurare ObjectDataSource per usare il `UpdateProduct` Overload appena creato ([fare clic per visualizzare l'immagine ingrandita](limiting-data-modification-functionality-based-on-the-user-cs/_static/image33.png))


È nuovamente richiesto di selezionare l'origine del parametro per il `GetProductsBySupplierID(supplierID)` metodo s `supplierID` parametro di input. Poiché si desidera visualizzare i prodotti per il fornitore selezionato nel controllo DetailsView, utilizzare il `SuppliersDetails` controllo DetailsView s `SelectedValue` proprietà come origine del parametro.


[![Utilizzare la proprietà SelectedValue di SuppliersDetails DetailsView s come origine del parametro](limiting-data-modification-functionality-based-on-the-user-cs/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image34.png)

**Figura 12**: usare il `SuppliersDetails` DetailsView s `SelectedValue` proprietà come origine del parametro ([fare clic per visualizzare l'immagine ingrandita](limiting-data-modification-functionality-based-on-the-user-cs/_static/image36.png))


Restituzione di GridView, rimuovere tutti i campi di GridView, ad eccezione di `ProductName`, `QuantityPerUnit`, e `Discontinued`, contrassegno di `Discontinued` CheckBoxField in sola lettura. Inoltre, selezionare l'opzione Abilita modifica dallo smart tag s di GridView. Dopo avere apportate queste modifiche, è possibile che il markup dichiarativo per GridView e ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample5.aspx)]

Come con il nostro ObjectDataSources precedente, si tratta di un `OldValuesParameterFormatString` è impostata su `original_{0}`, che si verificheranno problemi durante il tentativo di aggiornare un prodotto s nome o la quantità per unità. Rimuovere completamente la proprietà con la sintassi dichiarativa o impostare il valore predefinito, `{0}`.

Con questa configurazione completa, la pagina sono ora elencati i prodotti specificati dal fornitore selezionato in GridView (vedere Figura 13). Attualmente *qualsiasi* nome prodotto s o quantità per unità può essere aggiornata. Tuttavia, è necessario aggiornare la logica di pagina in modo che tale funzionalità non è consentito per i prodotti non più disponibili per gli utenti associati a un particolare fornitore. Che verranno affrontati questo finale nel passaggio 5.


[![Vengono visualizzati i prodotti specificati dal fornitore selezionato](limiting-data-modification-functionality-based-on-the-user-cs/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image37.png)

**Figura 13**: il prodotti specificati dal fornitore selezionato vengono visualizzati ([fare clic per visualizzare l'immagine ingrandita](limiting-data-modification-functionality-based-on-the-user-cs/_static/image39.png))


> [!NOTE]
> Con l'aggiunta di questo GridView modificabile il `Suppliers` s DropDownList `SelectedIndexChanged` gestore dell'evento deve essere aggiornato per restituire il controllo GridView in uno stato di sola lettura. In caso contrario, se un fornitore diverso viene selezionato mentre è in corso la modifica delle informazioni sul prodotto, l'indice corrispondente in GridView per il nuovo fornitore sarà anche modificabile. Per evitare questo problema, è sufficiente impostare s GridView `EditIndex` proprietà `-1` nel `SelectedIndexChanged` gestore dell'evento.


Inoltre, tenere presente che è importante che lo stato di visualizzazione s essere GridView abilitato (comportamento predefinito). Se si imposta la s GridView `EnableViewState` proprietà `false`, si corre il rischio di consentire agli utenti simultanei accidentalmente l'eliminazione o la modifica di record. Vedere [avviso: problema di concorrenza con ASP.NET 2.0 GridView/DetailsView/FormViews che supportano la modifica e/o l'eliminazione e il cui stato di visualizzazione è disabilitato](http://scottonwriting.net/sowblog/posts/10054.aspx) per ulteriori informazioni.

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>Passaggio 5: Non consentire modifiche per non più disponibili prodotti quando si visualizza o modifica tutti i fornitori non è selezionata

Mentre il `ProductsBySupplier` GridView è completamente funzionale, attualmente garantisce l'accesso eccessivo agli utenti che appartengono a un particolare fornitore. Per il nostro regole di business, tali utenti non devono essere in grado di aggiornare i prodotti non più supportati. Per applicare questo comportamento, è possibile nascondere o disabilitare il pulsante di modifica delle righe di GridView con i prodotti non più disponibili quando viene visitata la pagina da un utente da un fornitore.

Creare un gestore eventi per s GridView `RowDataBound` evento. In questo gestore eventi è necessario determinare se l'utente è associata a un particolare fornitore, per questa esercitazione, può essere determinata controllando il s DropDownList Suppliers `SelectedValue` - proprietà se è diverso da -1, l'utente è associata a un particolare fornitore. Per tali utenti, è necessario determinare se il prodotto non è più disponibile. È possibile acquisire un riferimento a effettivi `ProductRow` istanza associata alla riga GridView tramite il `e.Row.DataItem` proprietà, come descritto nel [ *la visualizzazione di informazioni di riepilogo in GridView s piè di pagina* ](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) in questa esercitazione. Se il prodotto non è più disponibile, è possibile scaricare un riferimento a livello di codice per il pulsante di modifica in GridView s CommandField utilizzando le tecniche descritte nell'esercitazione precedente, [ *aggiunta sul lato Client conferma quando l'eliminazione di* ](adding-client-side-confirmation-when-deleting-cs.md). Dopo aver ottenuto un riferimento è quindi possibile nascondere o disabilitare il pulsante.


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample6.cs)]

Con questo evento gestore sul posto, quando si visita questa pagina come un utente da un fornitore specifico i prodotti che non sono più disponibili non sono modificabili, come il pulsante Modifica è nascosto per questi prodotti. Ad esempio, s Chef Anton's Gumbo Mix è un prodotto non più disponibile per il fornitore Bolzano Cajun Delights. Durante la visita la pagina per questo particolare fornitore, il pulsante Modifica per questo prodotto è nascosto (vedere Figura 14). Tuttavia, durante la visita utilizzando "Visualizza/Modifica tutti i fornitori", il pulsante Modifica è disponibile (vedere Figura 15).


[![Per gli utenti specifici del fornitore è nascosto pulsante Modifica per s Chef Anton's Gumbo Mix](limiting-data-modification-functionality-based-on-the-user-cs/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image40.png)

**Figura 14**: per utenti specifici del fornitore è nascosto pulsante Modifica per s Chef Anton's Gumbo Mix ([fare clic per visualizzare l'immagine ingrandita](limiting-data-modification-functionality-based-on-the-user-cs/_static/image42.png))


[![Per visualizzare/modificare tutti gli utenti fornitori, viene visualizzato il pulsante Modifica per s Chef Anton's Gumbo Mix](limiting-data-modification-functionality-based-on-the-user-cs/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image43.png)

**Figura 15**: per mostrare/modifica tutti i fornitori gli utenti, il pulsante Modifica per s Chef Anton's Gumbo Mix viene visualizzato ([fare clic per visualizzare l'immagine ingrandita](limiting-data-modification-functionality-based-on-the-user-cs/_static/image45.png))


## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>Verifica per i diritti di accesso nel livello di logica di Business

In questa esercitazione ASP.NET pagina gestisce tutta la logica per quanto riguarda le informazioni che l'utente può visualizzare e quali prodotti è possibile aggiornare. In teoria, questa logica anche devono essere presente a livello di logica di Business. Ad esempio, il `SuppliersBLL` classe s `GetSuppliers()` (metodo), che restituisce tutti i fornitori, potrebbe includere un controllo per verificare che l'utente attualmente connesso è *non* associata a un fornitore specifico. Analogamente, il `UpdateSupplierAddress` metodo potrebbe includere un controllo per assicurarsi che l'utente attualmente connesso sia lavorato per l'azienda (e pertanto può aggiornare tutte le informazioni di indirizzo di fornitori) o con il fornitore i cui dati vengono aggiornati.

Non includere qui tali controlli livello Business LOGIC layer perché in questa esercitazione i diritti utente s sono determinati da un controllo DropDownList nella pagina, che non possono accedere le classi BLL. Quando si utilizza il sistema di appartenenze o di uno degli schemi di autenticazione fuori-predefinito forniti da ASP.NET (ad esempio l'autenticazione di Windows), attualmente connesso utente s informazioni e informazioni sui ruoli è possibile accedere da BLL, rendendo tale accesso diritti controlla possibili livelli BLL sia la presentazione.

## <a name="summary"></a>Riepilogo

La maggior parte dei siti che forniscono gli account utente di cui è necessario personalizzare l'interfaccia di modifica di dati in base all'utente connesso. Gli utenti amministratori potrebbero essere in grado di eliminare e modificare qualsiasi record, mentre gli utenti non amministratori possono essere limitati al solo l'aggiornamento o eliminazione di record, vengono creati da loro. Può essere qualsiasi scenario, i controlli Web, ObjectDataSource, dati e classi del livello di logica di Business possono essere estesa per aggiungere o negare determinate funzionalità in base all'utente connesso. In questa esercitazione è stato illustrato come limitare i dati visualizzabili e modificabili a seconda se l'utente è associata a un particolare fornitore o se hanno lavorato per l'azienda.

Questa esercitazione si conclude l'esame di inserimento, aggiornamento ed eliminazione di dati mediante i controlli di GridView, DetailsView e FormView. A partire dalla prossima esercitazione, si verranno prese in esame l'aggiunta di supporto per l'ordinamento e paging.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](adding-client-side-confirmation-when-deleting-cs.md)
> [Successivo](an-overview-of-inserting-updating-and-deleting-data-vb.md)
