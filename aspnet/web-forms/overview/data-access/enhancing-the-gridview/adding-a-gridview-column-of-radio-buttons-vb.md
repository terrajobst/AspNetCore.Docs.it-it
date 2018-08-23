---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
title: Aggiunta di una colonna GridView di pulsanti di opzione (VB) | Microsoft Docs
author: rick-anderson
description: Questa esercitazione verrà illustrato come aggiungere un controllo GridView per fornire all'utente un modo più intuitivo per la selezione di una singola riga di una colonna di pulsanti di opzione...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 2e31b60b-8723-4f14-b7ee-37859454dc3b
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
msc.type: authoredcontent
ms.openlocfilehash: 76b3dbd502eff7c97f57fdacd120ac2312aaceae
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835850"
---
<a name="adding-a-gridview-column-of-radio-buttons-vb"></a>Aggiunta di una colonna GridView di pulsanti di opzione (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_VB.exe) o [Scarica il PDF](adding-a-gridview-column-of-radio-buttons-vb/_static/datatutorial51vb1.pdf)

> Questa esercitazione verrà illustrato come aggiungere una colonna di pulsanti di opzione a un controllo GridView per fornire all'utente un modo più intuitivo per la selezione di una singola riga di GridView.


## <a name="introduction"></a>Introduzione

Il controllo GridView offre una vasta gamma di funzionalità predefinite. Include un numero di campi diversi per la visualizzazione di testo, immagini, collegamenti ipertestuali e pulsanti. Supporta i modelli per un'ulteriore personalizzazione. Con pochi clic del mouse, è possibile eseguire un controllo GridView in cui ogni riga può essere selezionato tramite un pulsante o per abilitare la modifica o eliminazione di funzionalità. Nonostante il gran numero di funzionalità offerte, sono spesso sono situazioni in cui aggiuntive, funzionalità non supportate devono essere aggiunti. In questa esercitazione e le due successive esamineremo come migliorare le funzionalità di s GridView per includere funzionalità aggiuntive.

Questa esercitazione e quello successivo l'obiettivo di migliorare il processo di selezione di righe. Come esaminato nel [Master/dettaglio mediante un Master GridView selezionabile con un controllo DetailView per i dettagli](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md), possiamo aggiungere un CommandField a GridView che include un pulsante di selezione. Quando si fa clic, un postback previsioni e i dispositivi di GridView `SelectedIndex` proprietà viene aggiornata per l'indice della riga di cui Seleziona pulsante selezionato. Nel [Master/dettaglio mediante un Master GridView selezionabile con un controllo DetailView per i dettagli](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) dell'esercitazione, è stato illustrato come utilizzare questa funzionalità per visualizzare i dettagli per la riga selezionata di GridView.

Anche se il pulsante di selezione funziona nella maggior parte dei casi, potrebbe non funzionare anche per altri utenti. Invece di usare un pulsante, due altri elementi dell'interfaccia utente vengono utilizzate per la selezione: il pulsante di opzione e la casella di controllo. È possibile aumentare il GridView in modo che invece di un pulsante Seleziona, ogni riga contiene un pulsante di opzione o casella di controllo. Negli scenari in cui l'utente può selezionare solo uno dei record di controllo GridView, il pulsante di opzione potrebbe essere preferibile tramite il pulsante di selezione. In situazioni in cui l'utente può selezionare più record, ad esempio in un'applicazione di posta elettronica basato sul web, in cui un utente può essere necessario selezionare più messaggi per eliminare la casella di controllo offre funzionalità che non è disponibile dal pulsante di selezione o pulsante di opzione interfacce utente.

Questa esercitazione verrà illustrato come aggiungere una colonna di pulsanti di opzione a GridView. L'esercitazione procedere illustra l'uso di caselle di controllo.

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>Passaggio 1: Creazione di migliorare le pagine Web di GridView

Prima di iniziare miglioramento di GridView per includere una colonna di pulsanti di opzione, lasciare s prima di tutto si consiglia di creare le pagine ASP.NET nel progetto di sito Web che è necessario per questa esercitazione e i due successivi. Iniziare aggiungendo una nuova cartella denominata `EnhancedGridView`. Successivamente, aggiungere le seguenti pagine ASP.NET per quella cartella, assicurandosi di associare ogni pagina con il `Site.master` pagina master:

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`


![Aggiungere le pagine ASP.NET per le esercitazioni relative alla SqlDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.gif)

**Figura 1**: aggiungere le pagine ASP.NET per le esercitazioni relative alla SqlDataSource


In altre cartelle, analogo a `Default.aspx` nella `EnhancedGridView` cartella elencherà le esercitazioni nella relativa sezione. Si tenga presente che il `SectionLevelTutorialListing.ascx` controllo utente fornisce questa funzionalità. Pertanto, aggiungere questo controllo utente da `Default.aspx` trascinandolo da Esplora soluzioni nella pagina di visualizzazione della struttura s.


[![Aggiungere il controllo utente sectionleveltutoriallisting. ascx a default. aspx](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.png)

**Figura 2**: aggiungere i `SectionLevelTutorialListing.ascx` controllo utente da `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.png))


Infine, aggiungere questi quattro pagine come voci per il `Web.sitemap` file. In particolare, aggiungere il markup seguente dopo l'uso del controllo SqlDataSource `<siteMapNode>`:


[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample1.xml)]

Dopo aver aggiornato `Web.sitemap`, si consiglia di visualizzare il sito Web di esercitazioni tramite un browser. Il menu a sinistra ora include elementi per la modifica, inserimento ed eliminazione di esercitazioni.


![Mappa del sito include ora voci per il miglioramento delle esercitazioni di GridView](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.gif)

**Figura 3**: la mappa del sito include ora voci per il miglioramento delle esercitazioni di GridView


## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>Passaggio 2: Visualizzare i fornitori in un oggetto GridView

Per questa esercitazione lasciare s GridView che visualizza un elenco di fornitori degli Stati Uniti, con ogni riga GridView che fornisce un pulsante di opzione di compilazione. Dopo aver selezionato un fornitore tramite il pulsante di opzione, l'utente può visualizzare i prodotti di fornitore s facendo clic su un pulsante. Anche se questa attività può sembrare semplice, esistono una serie di sfumature che lo rendono particolarmente difficile. Prima di affrontare queste sfumature, consentire s prima di tutto ottenere un controllo GridView Elenca i fornitori.

Iniziare aprendo il `RadioButtonField.aspx` nella pagina di `EnhancedGridView` cartella mediante il trascinamento di un controllo GridView dalla casella degli strumenti nella finestra di progettazione. Impostare la s GridView `ID` a `Suppliers` e, dal suo smart tag, scegliere di creare una nuova origine dati. In particolare, creare un oggetto ObjectDataSource denominato `SuppliersDataSource` che vengono utilizzati i relativi dati di `SuppliersBLL` oggetto.


[![Creare un nuovo oggetto ObjectDataSource denominato SuppliersDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.png)

**Figura 4**: creare un nuovo oggetto ObjectDataSource denominato `SuppliersDataSource` ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.png))


[![Configurare ObjectDataSource per usare la classe SuppliersBLL](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.png)

**Figura 5**: configurare ObjectDataSource per usare il `SuppliersBLL` classe ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.png))


Poiché si vuole solo elencare tali fornitori negli Stati Uniti, scegliere il `GetSuppliersByCountry(country)` metodo nell'elenco a discesa della scheda selezionare.


[![Configurare ObjectDataSource per usare la classe SuppliersBLL](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.png)

**Figura 6**: configurare ObjectDataSource per usare il `SuppliersBLL` classe ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.png))


Dalla scheda Aggiorna, seleziona (nessuno) l'opzione e fare clic su Avanti.


[![Configurare ObjectDataSource per usare la classe SuppliersBLL](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.png)

**Figura 7**: configurare ObjectDataSource per usare il `SuppliersBLL` classe ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.png))


Poiché il `GetSuppliersByCountry(country)` metodo accetta un parametro, la procedura guidata Configura origine dati richiede l'origine di tale parametro. Per specificare un valore hardcoded (Stati Uniti, in questo esempio), lasciare il parametro di elenco di riepilogo a discesa origine impostata su None e immettere il valore predefinito nella casella di testo. Fare clic su Fine per completare la procedura guidata.


[![Usare USA come valore predefinito per il parametro di paese](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.png)

**Figura 8**: usare USA come valore predefinito per il `country` parametro ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.png))


Dopo aver completato la procedura guidata, il controllo GridView includerà un BoundField per ognuno dei campi di dati del fornitore. Rimuovi tutto tranne le `CompanyName`, `City`, e `Country` BoundField e rinominare il `CompanyName` BoundField `HeaderText` proprietà al fornitore. Al termine dell'operazione, la sintassi dichiarativa per il controllo GridView e ObjectDataSource dovrebbe essere simile al seguente.


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample2.aspx)]

Per questa esercitazione, lasciare s consentono all'utente di visualizzare il fornitore selezionato prodotti s nella stessa pagina dell'elenco di fornitori, o in una pagina diversa. Per risolvere questo problema, aggiungere due controlli Web pulsante alla pagina. Ho ve set il `ID` s di questi due pulsanti per `ListProducts` e `SendToProducts`, con l'idea che quando `ListProducts` si fa clic verrà eseguito un postback e i prodotti di fornitore selezionato s verranno elencati nella stessa pagina, ma quando `SendToProducts` viene fatto clic su l'utente verrà si visualizza una pagina di un altro che elenca i prodotti.

Figura 9 mostra la `Suppliers` GridView e due Web pulsante Controlla quando viene visualizzato tramite un browser.


[![Questi fornitori degli Stati Uniti sono il nome, città e paese informazioni elencate](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.png)

**Figura 9**: I fornitori del USA hanno Their nome, città e paese le informazioni elencate ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.png))


## <a name="step-3-adding-a-column-of-radio-buttons"></a>Passaggio 3: Aggiunta di una colonna di pulsanti di opzione

A questo punto il `Suppliers` GridView ha tre BoundField visualizzando il nome della società, città e paese del ogni fornitore negli Stati Uniti. Una colonna di pulsanti di opzione, è comunque assente tuttavia. Sfortunatamente, t GridView includono un incorporati RadioButtonField, in caso contrario, si può semplicemente aggiungere che nella griglia e di essere eseguite. In alternativa, possiamo aggiungere un TemplateField e configurare il `ItemTemplate` per eseguire il rendering di un pulsante di opzione, causando un pulsante di opzione per ogni riga GridView.

Inizialmente, si potrebbe essere presuppone che l'interfaccia utente desiderato può essere implementato aggiungendo un controllo RadioButton Web il `ItemTemplate` di un TemplateField. Mentre si aggiungerà effettivamente un singolo pulsante di opzione a ogni riga di GridView, i pulsanti di opzione non possono essere raggruppati e pertanto non si escludono a vicenda. Un utente finale, ovvero è possibile selezionare contemporaneamente più pulsanti di opzione da GridView.

Anche se tramite un TemplateField dei controlli Web RadioButton non offre la funzionalità è necessario, s ti permettono di implementare questo approccio, come s possono essere utili per esaminare il motivo per cui non sono raggruppati i pulsanti di opzione risultanti. Iniziare aggiungendo un TemplateField a GridView fornitori, rendendo il campo più a sinistra. GridView s nello smart tag, fare clic sul collegamento di modifica modelli quindi trascina un controllo RadioButton Web dalla casella degli strumenti in s TemplateField `ItemTemplate` (vedere la figura 10). Impostare la s RadioButton `ID` proprietà `RowSelector` e il `GroupName` proprietà `SuppliersGroup`.


[![Aggiungere un controllo Web RadioButton a ItemTemplate](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.png)

**Figura 10**: aggiungere un controllo Web RadioButton per i `ItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.png))


Dopo aver apportato queste aggiunte tramite la finestra di progettazione, il markup di GridView s dovrebbe essere simile al seguente:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample3.aspx)]

Le s RadioButton [ `GroupName` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx) viene usata per raggruppare una serie di pulsanti di opzione. Tutti i controlli RadioButton con lo stesso `GroupName` valore vengono considerati raggruppato; un solo pulsante di opzione selezionabile da un gruppo alla volta. Il `GroupName` proprietà specifica il valore per il pulsante di opzione sottoposto a rendering s `name` attributo. Il browser esamina i pulsanti di opzione `name` gli attributi per determinare la radio pulsante raggruppamenti.

Con il controllo RadioButton Web aggiunto al `ItemTemplate`, visitare questa pagina tramite un browser e fare clic sui pulsanti di opzione nelle righe della griglia s. Si noti come i pulsanti di opzione non sono raggruppati, rendendo possibile selezionare tutte le righe, come nella figura 11 Mostra.


[![I pulsanti di opzione s GridView non sono raggruppate](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.png)

**Figura 11**: pulsanti di opzione s di GridView non sono raggruppata ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.png))


Il motivo non sono raggruppati i pulsanti di opzione è che il rendering `name` gli attributi sono diversi, anche se lo stesso `GroupName` l'impostazione della proprietà. Per visualizzare queste differenze, è una vista o origine dal browser ed esaminare il markup di pulsante di opzione:


[!code-html[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample4.html)]

Si noti come entrambi i `name` e `id` attributi non sono i valori esatti come specificato nella finestra Proprietà, ma sono preceduti da un numero di altri `ID` valori. Aggiuntiva `ID` valori aggiunti all'inizio dell'oggetto sottoposto a rendering `id` e `name` gli attributi sono le `ID` s della radio pulsanti controlli padre il `GridViewRow` s `ID` s, la s GridView `ID`, il Controllo s contenuto `ID`e il Web Form s `ID`. Questi `ID` s vengono aggiunti in modo che ogni rendering controllo Web in GridView presenta un valore univoco `id` e `name` valori.

Ciascuno sottoposto a rendering un altro controllo esigenze `name` e `id` poiché si tratta di come il browser che identifica in modo univoco ogni controllo sul lato client e come identifica al server web l'azione oppure è stata modificata durante il postback. Ad esempio, si supponga di voler eseguire il codice lato server ogni volta che un pulsante di opzione s archiviato lo stato è stato modificato. Per eseguire questa operazione impostando la s RadioButton `AutoPostBack` proprietà `True` e la creazione di un gestore eventi per il `CheckChanged` evento. Tuttavia, se il rendering `name` e `id` i valori per tutti i pulsanti di opzione sono le stesse, sul postback Impossibile determinare quali specifico è stato fatto clic sul pulsante di opzione.

La breve di esso è che non è possibile creare una colonna di pulsanti di opzione in un controllo GridView utilizza il controllo RadioButton Web. In alternativa, è necessario usare le tecniche piuttosto antiquate per garantire che il markup appropriato viene inserito in ogni riga GridView.

> [!NOTE]
> Ad esempio il controllo RadioButton Web, il pulsante di opzione HTML se il controllo aggiunto a un modello, includerà l'univoco `name` attributo, rendendo i pulsanti di opzione della griglia sono separati. Se non ha familiarità con i controlli HTML, è possibile ignorare questa nota, come i controlli HTML vengono utilizzati raramente, soprattutto in ASP.NET 2.0. Ma se è interessati a ulteriori informazioni, vedere [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx) s post di blog [controlli Web e controlli HTML](http://www.odetocode.com/Articles/348.aspx).


## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>Utilizzo di un controllo valore letterale per l'inserimento di Markup di pulsante di opzione

Per raggruppare correttamente tutti i pulsanti di opzione all'interno di GridView, è necessario inserire manualmente il markup di pulsanti di opzione nel `ItemTemplate`. Ogni pulsante di opzione richiede lo stesso `name` dell'attributo, ma devono avere un valore univoco `id` attributo (nel caso in cui si vuole accedere a un pulsante di opzione tramite uno script lato client). Dopo che un utente seleziona un pulsante di opzione ed esegue il postback della pagina, il browser invierà di nuovo il valore del pulsante di opzione selezionato s `value` attributo. Pertanto, sarà necessario un valore univoco ogni pulsante di opzione `value` attributo. Infine, durante il postback è necessario assicurarsi di aggiungere il `checked` attributo di un pulsante di opzione selezionato, in caso contrario, dopo che l'utente effettua una selezione e torna post, i pulsanti di opzione restituirà lo stato predefinito (tutti non selezionati).

Esistono due approcci che possono essere eseguiti per inserire un modello di markup di basso livello. Uno consiste nell'eseguire una combinazione di markup e le chiamate ai metodi definiti nella classe code-behind di formattazione. Questa tecnica è stata illustrata per prima nella [uso di TemplateFields nel controllo GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) esercitazione. In questo caso potrebbe essere simile a:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample5.aspx)]

In questo caso, `GetUniqueRadioButton` e `GetRadioButtonValue` sarebbero metodi definiti nella classe code-behind che restituito appropriato `id` e `value` valori per ogni pulsante di opzione dell'attributo. Questo approccio funziona bene per assegnare il `id` e `value` attributi, ma è sufficiente in caso di necessità di specificare il `checked` valore dell'attributo perché la sintassi di associazione dati viene eseguita solo quando i dati prima di tutto sono associati a GridView. Pertanto, se il controllo GridView è abilitato lo stato di visualizzazione, i metodi di formattazione verranno generato solo quando la pagina viene caricata inizialmente o quando il controllo GridView è riassociato in modo esplicito all'origine dati e pertanto la funzione che imposta il `checked` attributo ha vinto t essere chiamata su postback. È un problema piuttosto complesso s e un po' esula dall'ambito di questo articolo, in modo che lo lascio in questo. , Tuttavia, è consigliabile provare a usare l'approccio precedente funzionano e tramite al punto in cui è possibile rimanere bloccati. Anche se tali una t esercizio ha vinto ottenere qualsiasi più prossimo a una versione di lavoro, e consentirà di promuovere una conoscenza più approfondita di GridView e il ciclo di vita di Data Binding.

L'altro approccio a inserire personalizzato, markup di basso livello in un modello e l'approccio che verrà usato per questa esercitazione consiste nell'aggiungere un [controllo letterale](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx) al modello. Quindi, in s GridView `RowCreated` o `RowDataBound` gestore eventi, controllo Literal accessibili a livello di codice e la relativa `Text` proprietà è impostata su markup a generare.

Iniziare rimuovendo il controllo RadioButton da s TemplateField `ItemTemplate`, sostituendolo con un controllo Literal. Impostare il controllo letterale 1!s `ID` a `RadioButtonMarkup`.


[![Aggiungere un controllo valore letterale a ItemTemplate](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.png)

**Figura 12**: aggiungere un controllo valore letterale per il `ItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.png))


Successivamente, creare un gestore eventi per s GridView `RowCreated` evento. Il `RowCreated` evento viene generato una volta per ogni riga aggiunta, se i dati sono in corso riassociati a GridView. Ciò significa che anche in un postback quando i dati viene ricaricati lo stato di visualizzazione, il `RowCreated` attiverà comunque eventi e questo è il motivo viene usato in alternativa `RowDataBound` (che viene attivato solo quando i dati in modo esplicito associati ai dati di controllo Web).

In questo gestore eventi, si vuole solo procedere se si ri affrontare una riga di dati. Per ogni riga di dati si desidera fare riferimento a livello di codice le `RadioButtonMarkup` controllo letterale e set relativo `Text` proprietà per il markup da creare. Come illustrato nel codice seguente, il markup generato crea un'opzione pulsante la cui proprietà `name` attributo è impostato su `SuppliersGroup`, la cui `id` attributo è impostato su `RowSelectorX`, dove *X* è l'indice della riga GridView, e il cui `value` attributo è impostato per l'indice della riga GridView.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample6.vb)]

Quando è selezionata una riga GridView e si verifica un postback, siamo interessati al `SupplierID` del fornitore selezionato. Pertanto, si potrebbe pensare che il valore di ogni pulsante di opzione deve essere l'effettivo `SupplierID` (piuttosto che l'indice della riga GridView). Sebbene questo approccio potrebbe funzionare in determinate circostanze, sarebbe un rischio per la sicurezza alla cieca accettare ed elaborare una `SupplierID`. Il controllo GridView, ad esempio, elenca solo i fornitori negli Stati Uniti. Tuttavia, se il `SupplierID` viene passato direttamente dal pulsante di opzione novità per impedire la modifica un utente dispettoso il `SupplierID` valore inviato nuovamente durante il postback? Tramite l'indice di riga come la `value`e quindi della configurazione il `SupplierID` durante il postback dal `DataKeys` raccolta, è possibile assicurarsi che l'utente sta usando solo uno del `SupplierID` valori associati a una delle righe GridView.

Dopo aver aggiunto questo codice del gestore eventi, è opportuno testare la pagina in un browser. In primo luogo, tenere presente tale opzione solo un pulsante nella griglia è possibile selezionare contemporaneamente. Tuttavia, quando la selezione di un pulsante di opzione e fare clic su uno dei pulsanti, si verifica un postback e tutti i pulsanti di opzione ripristino allo stato iniziale (vale a dire, durante il postback, il pulsante di opzione selezionato non è più selezionato). Per risolvere questo problema, è necessario aumentare la `RowCreated` gestore dell'evento in modo che non controlla se l'indice di pulsante di opzione selezionato inviato dal postback e aggiunge il `checked="checked"` il markup generato le corrispondenze di indice di riga dell'attributo.

Quando si verifica un postback, il browser invia di nuovo la `name` e `value` del pulsante di opzione selezionato. Il valore può essere recuperato a livello di programmazione tramite `Request.Form("name")`. Il [ `Request.Form` proprietà](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx) fornisce una [ `NameValueCollection` ](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx) che rappresentano le variabili di form. Variabili dei form sono i nomi e valori dei campi nella pagina web e inviate nuovamente da web browser ogni volta che un postback previsioni. Perché il rendering `name` attributo dei pulsanti di opzione in GridView `SuppliersGroup`, quando la pagina web viene pubblicato il browser invierà `SuppliersGroup=valueOfSelectedRadioButton` al server web (insieme ad altri campi modulo). Queste informazioni sono quindi possibile accedere dal `Request.Form` con proprietà: `Request.Form("SuppliersGroup")`.

Dopo sarà necessario per determinare il pulsante di opzione selezionato indicizzare non solo nel `RowCreated` gestore dell'evento, ma nel `Click` gestori eventi per i controlli pulsante Web, s ti permettono di aggiungere un `SuppliersSelectedIndex` proprietà alla classe code-behind che restituisce `-1`se è stato selezionato alcun pulsante di opzione e l'indice selezionato se uno dei pulsanti di opzione è selezionato.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample7.vb)]

Questa proprietà aggiunte, sappiamo per aggiungere il `checked="checked"` markup nel `RowCreated` gestore dell'evento quando `SuppliersSelectedIndex` è uguale a `e.Row.RowIndex`. Aggiornare il gestore eventi per includere la logica:


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample8.vb)]

Con questa modifica, pulsante di opzione selezionato rimane selezionato dopo un postback. Ora che abbiamo la possibilità di specificare quale pulsante di opzione è selezionato, è stato possibile modificare il comportamento in modo che quando la pagina è stata visitata prima di tutto, è stato selezionato il primo pulsante di opzione riga s GridView (anziché non avere alcuna pulsanti di opzione selezionati per impostazione predefinita, che è corrente comportamento). Per ottenere il primo pulsante di opzione selezionato per impostazione predefinita, è sufficiente modificare la `If SuppliersSelectedIndex = e.Row.RowIndex Then` come indicato di seguito: `If SuppliersSelectedIndex = e.Row.RowIndex OrElse (Not Page.IsPostBack AndAlso e.Row.RowIndex = 0) Then`.

A questo punto è stata aggiunta una colonna di pulsanti di opzione raggruppati a GridView che consente una singola riga GridView essere selezionato e memorizzati nella navigazione tra i postback. I passaggi successivi consistono all'esposizione dei prodotti specificati dal fornitore selezionato. Nel passaggio 4 si vedrà come reindirizzare l'utente a un'altra pagina, l'invio lungo selezionato `SupplierID`. Nel passaggio 5, vedremo come visualizzare i prodotti s fornitore selezionato in un controllo GridView nella stessa pagina.

> [!NOTE]
> Anziché utilizzare un TemplateField (l'obiettivo di questo passaggio 3 di lunga durata), è possibile creare un oggetto personalizzato `DataControlField` classe che esegue il rendering dell'interfaccia utente appropriata e funzionalità. Il [ `DataControlField` classe](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx) è la classe base da cui derivano i BoundField, CampoCasellaDiControllo, TemplateField e altri campi incorporati di GridView e DetailsView. Creazione di un oggetto personalizzato `DataControlField` classe significa che la colonna di pulsanti di opzione è stato possibile aggiungere solo con la sintassi dichiarativa e renderebbe anche replicate le funzionalità in altre pagine web e altre applicazioni web molto più semplice.


Se si sposta mai realizzato personalizzato, compilato controlli in ASP.NET, tuttavia, ciò indica che potrebbe richiede una notevole quantità di attività ed è caratterizzata da un host di sfaccettature e casi limite che devono essere gestiti con attenzione. Pertanto, si verrà rinunciano alla implementazione di una colonna di pulsanti di opzione come una classe personalizzata `DataControlField` classe per il momento e l'opzione TemplateField bisogna rispettarla. Probabilmente si avrà la possibilità di esplorare la creazione, uso e distribuzione personalizzate `DataControlField` classi in un'esercitazione futura.

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>Passaggio 4: Visualizzare i prodotti s fornitore selezionato in una pagina separata

Dopo che l'utente ha selezionato una riga GridView, è necessario visualizzare i prodotti s dal fornitore selezionato. In alcuni casi, si potrà visualizzare questi prodotti in una pagina separata, in altri casi che è potrebbe essere preferibile eseguire questa operazione nella stessa pagina. Let s esaminare innanzitutto illustrato come visualizzare i prodotti in una pagina separata. nel passaggio 5 esamineremo l'aggiunta di un controllo GridView a `RadioButtonField.aspx` all'esposizione dei prodotti di fornitore selezionato s.

Attualmente sono disponibili due controlli Web pulsante nella pagina `ListProducts` e `SendToProducts`. Quando la `SendToProducts` si fa clic sul pulsante, da inviare all'utente di `~/Filtering/ProductsForSupplierDetails.aspx`. Questa pagina è stata creata la [Master/Dettagli filtro in due pagine](../masterdetail/master-detail-filtering-across-two-pages-vb.md) esercitazione e consente di visualizzare i prodotti per il fornitore il cui `SupplierID` viene passato tramite il campo stringa di query denominato `SupplierID`.

Per fornire questa funzionalità, creare un gestore eventi per il `SendToProducts` pulsante s `Click` evento. In passaggio 3 è stata aggiunta la `SuppliersSelectedIndex` proprietà, che restituisce l'indice della riga di cui pulsante di opzione è selezionata. Corrispondente `SupplierID` possono essere recuperati da s GridView `DataKeys` raccolta e l'utente può quindi essere inviate a `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` usando `Response.Redirect("url")`.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample9.vb)]

Questo codice funziona incredibile fino a quando uno dei pulsanti di opzione Seleziona di GridView. Se, inizialmente, il controllo GridView non dispone dei pulsanti di opzione selezionati e l'utente fa clic il `SendToProducts` pulsante, `SuppliersSelectedIndex` saranno `-1`, che genererà un'eccezione generata dal `-1` non è compreso nell'intervallo di indici del `DataKeys`collection. Si tratta, tuttavia, non un problema, se si è deciso di aggiornare il `RowCreated` gestore eventi, come descritto nel passaggio 3 in modo da avere il primo pulsante di opzione in GridView inizialmente selezionato.

Per supportare un `SuppliersSelectedIndex` pari a `-1`, aggiungere un controllo etichetta Web alla pagina sopra il controllo GridView. Impostare relativo `ID` proprietà `ChooseSupplierMsg`, la relativa `CssClass` proprietà `Warning`, la relativa `EnableViewState` e `Visible` proprietà da `False`e il relativo `Text` proprietà per scegliere un fornitore dalla griglia. La classe CSS `Warning` Visualizza il testo in un tipo di carattere rosso, corsivo, grassetto, grande e viene definito in `Styles.css`. Impostando il `EnableViewState` e `Visible` delle proprietà per `False`, l'etichetta non viene eseguito, ad eccezione per solo i postback di dove il controllo s `Visible` viene impostata a livello di codice su `True`.


[![Aggiungere un controllo Web sopra il controllo GridView](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image21.png)

**Figura 13**: aggiungere un'etichetta Web controllo sopra il controllo GridView ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-radio-buttons-vb/_static/image22.png))


Successivamente, aumentare la `Click` gestore dell'evento per visualizzare il `ChooseSupplierMsg` etichettare se `SuppliersSelectedIndex` è minore di zero e reindirizzare l'utente `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` in caso contrario.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample10.vb)]

Visita la pagina in un browser e fare clic su di `SendToProducts` pulsante prima di selezionare un fornitore di GridView. Come illustrato nella figura 14, verrà visualizzata la `ChooseSupplierMsg` etichetta. Successivamente, selezionare dei fornitori e fare clic su di `SendToProducts` pulsante. Questo verrà whisk è a una pagina che elenca i prodotti offerti dal fornitore selezionato. Figura 15 viene mostrata la `ProductsForSupplierDetails.aspx` pagina quando è stato selezionato il fornitore birrerie Bigfoot.


[![L'etichetta ChooseSupplierMsg viene visualizzata se è selezionato nessun fornitore](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image23.png)

**Figura 14**: il `ChooseSupplierMsg` etichetta viene visualizzata se è selezionato nessun fornitore ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-radio-buttons-vb/_static/image24.png))


[![I prodotti s fornitore selezionato vengono visualizzati in ProductsForSupplierDetails.aspx](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image25.png)

**Figura 15**: i prodotti s il fornitore selezionato vengono visualizzati nella `ProductsForSupplierDetails.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-radio-buttons-vb/_static/image26.png))


## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>Passaggio 5: Visualizzare i prodotti di fornitore selezionato s nella stessa pagina

Nel passaggio 4 è stato illustrato come inviare all'utente a un'altra pagina web per visualizzare il fornitore selezionato prodotti s. In alternativa, i prodotti di fornitore selezionato s possono essere visualizzati nella stessa pagina. Per illustrare questo concetto, si aggiungerà un altro controllo GridView a `RadioButtonField.aspx` all'esposizione dei prodotti di fornitore selezionato s.

Poiché vogliamo solo questo controllo GridView di prodotti da visualizzare dopo aver selezionato un fornitore, aggiungere un controllo Web pannello sotto la `Suppliers` GridView, impostare relativo `ID` a `ProductsBySupplierPanel` e il relativo `Visible` proprietà `False`. All'interno del pannello, aggiungere il testo di prodotti per il fornitore selezionato, seguito da un controllo GridView denominato `ProductsBySupplier`. GridView s nello smart tag, scegliere da associare a un nuovo oggetto ObjectDataSource denominato `ProductsBySupplierDataSource`.


[![Associare il controllo ProductsBySupplier GridView a un nuovo oggetto ObjectDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image27.png)

**Figura 16**: associa il `ProductsBySupplier` GridView per un nuovo oggetto ObjectDataSource ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-radio-buttons-vb/_static/image28.png))


Successivamente, configurare ObjectDataSource per usare il `ProductsBLL` classe. Poiché si vuole solo recuperare tali prodotti specificati dal fornitore selezionato, specificare che ObjectDataSource deve richiamare il `GetProductsBySupplierID(supplierID)` metodo per recuperare i dati. Selezionare (nessuno) dagli elenchi a discesa nell'aggiornamento, inserimento ed eliminare le schede.


[![Configurare ObjectDataSource per usare il metodo GetProductsBySupplierID(supplierID)](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image29.png)

**Figura 17**: configurare ObjectDataSource per usare il `GetProductsBySupplierID(supplierID)` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-radio-buttons-vb/_static/image30.png))


[![Impostare gli elenchi di riepilogo a discesa su (nessuno) nell'aggiornamento, inserimento ed eliminare le schede](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image31.png)

**Figura 18**: impostare gli elenchi di riepilogo a discesa su (nessuno) nell'aggiornamento, inserimento ed eliminare schede ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-radio-buttons-vb/_static/image32.png))


Dopo aver configurato l'istruzione SELECT, aggiornare, inserire, eliminare le schede, fare clic su Avanti. Poiché il `GetProductsBySupplierID(supplierID)` metodo prevede un parametro di input, la procedura guidata Creazione di un'origine dati richiede di specificare l'origine per il valore del parametro s.

Abbiamo un paio di opzioni qui in specificando l'origine del valore s del parametro. È possibile usare l'oggetto di parametro predefinito e a livello di programmazione assegnare il valore dei `SuppliersSelectedIndex` proprietà per il parametro s `DefaultValue` proprietà in s ObjectDataSource `Selecting` gestore dell'evento. Fare riferimento al [a livello di codice impostando i valori dei parametri di ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb.md) esercitazione per un aggiornamento su a livello di codice l'assegnazione di valori per i parametri di ObjectDataSource s.

In alternativa, è possibile usare un ControlParameter e fare riferimento al `Suppliers` s GridView [ `SelectedValue` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx) (vedere la figura 19). S GridView `SelectedValue` proprietà restituisce il `DataKey` valore corrispondente per il [ `SelectedIndex` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx). Affinché questa opzione per funzionare, è necessario impostare a livello di codice la s GridView `SelectedIndex` proprietà al riga quando la `ListProducts` si fa clic sul pulsante. Un ulteriore vantaggio, impostando il `SelectedIndex`, richiederà il record selezionato nel `SelectedRowStyle` definito nel `DataWebControls` tema (uno sfondo giallo).


[![Usare un ControlParameter specificare SelectedValue s GridView come origine del parametro](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image33.png)

**Figura 19**: usare un ControlParameter per specificare gli oggetti di GridView SelectedValue come origine del parametro ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-radio-buttons-vb/_static/image34.png))


Dopo aver completato la procedura guidata, Visual Studio aggiungerà automaticamente i campi per i campi di dati prodotto s. Rimuovere tutto tranne le `ProductName`, `CategoryName`, e `UnitPrice` BoundField e cambiare il `HeaderText` le proprietà del prodotto, categoria e prezzo. Configurare il `UnitPrice` BoundField in modo che il relativo valore viene formattato come una valuta. Dopo aver apportato queste modifiche, pannello di controllo GridView e ObjectDataSource s markup dichiarativo dovrebbe essere simile al seguente:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample11.aspx)]

Per completare questo esercizio, è necessario impostare la s GridView `SelectedIndex` proprietà per il `SelectedSuppliersIndex` e il `ProductsBySupplierPanel` pannello s `Visible` proprietà `True` quando la `ListProducts` si fa clic sul pulsante. A questo scopo, creare un gestore eventi per il `ListProducts` controllo pulsante Web s `Click` eventi e aggiungere il codice seguente:


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample12.vb)]

Se un fornitore non è stato selezionato da GridView, il `ChooseSupplierMsg` etichetta viene visualizzata e il `ProductsBySupplierPanel` nascosto pannello. In caso contrario, se è stato selezionato un fornitore, il `ProductsBySupplierPanel` viene visualizzato e il controllo GridView s `SelectedIndex` proprietà viene aggiornata.

Figura 20 vengono visualizzati i risultati è stato selezionato il fornitore a Bigfoot birrerie e i prodotti visualizzare nel pulsante della pagina è stato fatto clic.


[![I prodotti forniti da un Bigfoot birrerie sono elencati nella stessa pagina](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image35.png)

**Figura 20**: The prodotti forniti da un Bigfoot birrerie sono elencati nella stessa pagina ([fare clic per visualizzare l'immagine con dimensioni normali](adding-a-gridview-column-of-radio-buttons-vb/_static/image36.png))


## <a name="summary"></a>Riepilogo

Come descritto nel [Master/dettaglio mediante un Master GridView selezionabile con un controllo DetailView per i dettagli](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) tutorial, i record possono essere selezionati da un controllo GridView mediante un CommandField cui `ShowSelectButton` è impostata su `True`. Ma il CommandField Visualizza i pulsanti come regolare i pulsanti di comando, i collegamenti o le immagini. Un'interfaccia utente selezione riga alternativo consiste nel fornire un pulsante di opzione o casella di controllo in ogni riga GridView. In questa esercitazione viene esaminato il modo aggiungere una colonna di pulsanti di opzione.

Sfortunatamente, aggiunge una colonna di radio pulsanti t come diretta o semplice, come ci si aspetterebbe. Non vi è alcun RadioButtonField predefiniti che possono essere aggiunti alla selezione di un pulsante e utilizza il controllo RadioButton Web all'interno di un TemplateField introduce una serie di problemi. Al termine, per fornire un'interfaccia di questo tipo è necessario creare una classe personalizzata `DataControlField` classe o ricorrere a inserire il codice HTML appropriato in un TemplateField durante il `RowCreated` evento.

Con stato illustrato come aggiungere una colonna di pulsanti di opzione, farci di concentrare l'attenzione all'aggiunta di una colonna di caselle di controllo. Con una colonna di caselle di controllo, un utente può selezionare uno o più righe di GridView e quindi eseguire alcune operazioni su tutte le righe selezionate (ad esempio la selezione di un set di messaggi di posta elettronica da un client di posta elettronica basato sul web e quindi scegliendo di eliminare tutti i messaggi di posta elettronica selezionati). Nella prossima esercitazione vedremo come aggiungere una colonna di questo tipo.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata David Suru. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](inserting-a-new-record-from-the-gridview-s-footer-cs.md)
> [Successivo](adding-a-gridview-column-of-checkboxes-vb.md)
