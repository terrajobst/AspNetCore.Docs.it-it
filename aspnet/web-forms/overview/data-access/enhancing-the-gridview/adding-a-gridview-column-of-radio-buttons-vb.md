---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
title: Aggiunta di una colonna di GridView di pulsanti di opzione (VB) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione verrà illustrato come aggiungere una colonna di pulsanti di opzione a un controllo GridView per fornire all'utente un modo più intuitivo per la selezione di una singola riga di...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 2e31b60b-8723-4f14-b7ee-37859454dc3b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
msc.type: authoredcontent
ms.openlocfilehash: 39f6102e6b56e4bf258ca582633624d752bdc81f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889722"
---
<a name="adding-a-gridview-column-of-radio-buttons-vb"></a>Aggiunta di una colonna di GridView di pulsanti di opzione (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_VB.exe) o [Scarica il PDF](adding-a-gridview-column-of-radio-buttons-vb/_static/datatutorial51vb1.pdf)

> In questa esercitazione verrà illustrato come aggiungere una colonna di pulsanti di opzione a un controllo GridView per fornire all'utente un modo più intuitivo per la selezione di una singola riga di GridView.


## <a name="introduction"></a>Introduzione

Il controllo GridView offre una vasta gamma di funzionalità predefinite. Include un numero di campi diversi per la visualizzazione di testo, immagini, collegamenti ipertestuali e i pulsanti. Supporta i modelli per un'ulteriore personalizzazione. Con pochi clic del mouse, è possibile effettuare un controllo GridView in cui ogni riga può essere selezionato tramite un pulsante o per abilitare la modifica o eliminazione funzionalità s. Nonostante l'insieme di funzionalità fornite, spesso si situazioni in cui aggiuntive, sarà necessario aggiungere funzionalità non supportate. In questa esercitazione e le due successiva verrà esaminato come migliorare le funzionalità di s GridView per includere funzionalità aggiuntive.

In questa esercitazione e quello successivo concentrarsi su come migliorare il processo di selezione di riga. Come esaminato nel [Master/dettaglio con un controllo DetailView per i dettagli di Master GridView selezionabile](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md), è possibile aggiungere un CommandField a GridView che include un pulsante Seleziona. Quando si fa clic, viene utilizzata un postback e s GridView `SelectedIndex` proprietà viene aggiornata per l'indice della riga è stato fatto clic con il pulsante Seleziona. Nel [Master/dettaglio con un controllo DetailView per i dettagli di Master GridView selezionabile](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) dell'esercitazione, è stato illustrato come utilizzare questa funzionalità per visualizzare i dettagli per la riga selezionata di GridView.

Quando il pulsante Seleziona viene utilizzato in molti casi, potrebbe non funzionare anche per altri utenti. Invece di usare un pulsante, due altri elementi dell'interfaccia utente vengono comunemente utilizzati per la selezione: il pulsante di opzione e la casella di controllo. È possibile aumentare GridView in modo che anziché un pulsante di seleziona, ogni riga contiene un pulsante di opzione o una casella di controllo. In scenari in cui l'utente può solo selezionare uno dei record di GridView, il pulsante di opzione potrebbe essere preferito pulsante Seleziona. In situazioni in cui l'utente può selezionare più record, ad esempio in un'applicazione di posta elettronica basato sul web, in cui un utente potrebbe voler selezionare più messaggi per eliminare la casella di controllo offre funzionalità che non è disponibile dal pulsante Seleziona o pulsante di opzione interfacce utente.

In questa esercitazione verrà illustrato come aggiungere una colonna di pulsanti di opzione a GridView. L'esercitazione procedere viene illustrato l'utilizzo di caselle di controllo.

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>Passaggio 1: Creazione di miglioramento delle pagine Web di GridView

Prima di iniziare il GridView per includere una colonna di pulsanti di opzione di ottimizzazione, consentire s prima richiedere alcuni minuti per creare le pagine ASP.NET nel progetto sito Web che è necessario per questa esercitazione e i due successivi. Per iniziare, aggiungere una nuova cartella denominata `EnhancedGridView`. Successivamente, aggiungere le seguenti pagine ASP.NET a quella cartella, assicurandosi di associare ogni pagina con il `Site.master` pagina master:

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`


![Aggiungere le pagine ASP.NET per le esercitazioni relative SqlDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.gif)

**Figura 1**: aggiungere le pagine ASP.NET per le esercitazioni relative alla SqlDataSource


Come in altre cartelle, `Default.aspx` nel `EnhancedGridView` cartella elencherà le esercitazioni nella relativa sezione. Tenere presente che il `SectionLevelTutorialListing.ascx` controllo utente fornisce questa funzionalità. Pertanto, aggiungere il controllo utente `Default.aspx` trascinandolo da Esplora soluzioni nella pagina di visualizzazione della struttura s.


[![Aggiungere il controllo utente SectionLevelTutorialListing.ascx Default.aspx](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.png)

**Figura 2**: aggiungere il `SectionLevelTutorialListing.ascx` controllo utente al `Default.aspx` ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.png))


Infine, aggiungere questi quattro pagine come voci per il `Web.sitemap` file. In particolare, aggiungere il markup seguente dopo l'utilizzo del controllo SqlDataSource `<siteMapNode>`:


[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample1.xml)]

Dopo aver aggiornato `Web.sitemap`, dedicare alcuni minuti per visualizzare il sito Web esercitazioni tramite un browser. Il menu a sinistra include elementi per la modifica, inserimento ed eliminazione di esercitazioni.


![Mappa del sito include ora le voci per il miglioramento delle esercitazioni di GridView](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.gif)

**Figura 3**: mappa del sito include ora le voci per il miglioramento delle esercitazioni di GridView


## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>Passaggio 2: Visualizzare i fornitori in un controllo GridView.

Per questa esercitazione consente di s GridView che visualizza un elenco di fornitori dagli Stati Uniti, con ogni riga GridView che fornisce un pulsante di opzione di compilazione. Dopo aver selezionato un fornitore tramite il pulsante di opzione, l'utente può visualizzare i prodotti s fornitore facendo clic su un pulsante. Benché questa attività può sembrare semplice, sono disponibili un numero di sfumature che lo rendono particolarmente complessa. Prima di affrontare queste sfumature, consentire s innanzitutto ottenere un elenco di fornitori di GridView.

Aprire il `RadioButtonField.aspx` nella pagina di `EnhancedGridView` cartella mediante il trascinamento di un controllo GridView dalla casella degli strumenti nella finestra di progettazione. Impostare i dispositivi di GridView `ID` a `Suppliers` e dal suo smart tag, scegliere di creare una nuova origine dati. In particolare, creare un ObjectDataSource denominato `SuppliersDataSource` che estrae i dati di `SuppliersBLL` oggetto.


[![Creare un nuovo oggetto ObjectDataSource denominato SuppliersDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.png)

**Figura 4**: creare un nuovo denominato ObjectDataSource `SuppliersDataSource` ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.png))


[![Configurare ObjectDataSource per utilizzare la classe SuppliersBLL](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.png)

**Figura 5**: configurare ObjectDataSource per usare il `SuppliersBLL` classe ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.png))


Poiché si desidera solo elencare questi fornitori negli Stati Uniti, scegliere il `GetSuppliersByCountry(country)` metodo dall'elenco a discesa nella scheda Seleziona.


[![Configurare ObjectDataSource per utilizzare la classe SuppliersBLL](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.png)

**Figura 6**: configurare ObjectDataSource per usare il `SuppliersBLL` classe ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.png))


L'aggiornamento scheda, selezionare (nessuno) l'opzione e fare clic su Avanti.


[![Configurare ObjectDataSource per utilizzare la classe SuppliersBLL](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.png)

**Figura 7**: configurare ObjectDataSource per usare il `SuppliersBLL` classe ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.png))


Poiché il `GetSuppliersByCountry(country)` metodo accetta un parametro, la configurazione guidata origine dati richiede Microsoft per l'origine del parametro. Per specificare un valore hardcoded (USA, in questo esempio), mantenere il parametro di elenco a discesa origine impostata su None, immettere il valore predefinito nella casella di testo. Fare clic su Fine per completare la procedura guidata.


[![Utilizzare USA come valore predefinito per il paese parametro](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.png)

**Figura 8**: utilizzare USA come valore predefinito per il `country` parametro ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.png))


Dopo aver completato la procedura guidata, GridView includerà un BoundField per ognuno dei campi dati del fornitore. Rimuovere tutto tranne il `CompanyName`, `City`, e `Country` , BoundField e rinominare il `CompanyName` BoundField `HeaderText` proprietà al fornitore. Al termine dell'operazione, la sintassi dichiarativa GridView e ObjectDataSource dovrebbe essere simile al seguente.


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample2.aspx)]

Per questa esercitazione, lasciare s consente all'utente di visualizzare il fornitore selezionato prodotti s nella stessa pagina dell'elenco di fornitori o in una pagina diversa. A tale scopo, aggiungere due controlli Web pulsante alla pagina. I set ve il `ID` s di questi due pulsanti per `ListProducts` e `SendToProducts`, con il concetto che quando `ListProducts` si fa clic si verifica un postback e verranno elencati i prodotti selezionati fornitore s nella stessa pagina, ma quando `SendToProducts` viene fatto clic su l'utente verrà si visualizza un'altra pagina che elenca i prodotti.

Figura 9 è illustrato il `Suppliers` GridView e Web pulsante due controlli quando viene visualizzato tramite un browser.


[![Questi fornitori dagli Stati Uniti hanno nome, città che informazioni relative al paese elencati](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.png)

**Figura 9**: I fornitori del USA hanno Their nome, città e paese informazioni elencate ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.png))


## <a name="step-3-adding-a-column-of-radio-buttons"></a>Passaggio 3: Aggiunta di una colonna di pulsanti di opzione

A questo punto il `Suppliers` GridView ha tre BoundField visualizzando il nome della società, città e paese di ogni fornitore negli Stati Uniti. Comunque manca una colonna di pulsanti di opzione, tuttavia. Purtroppo, t GridView include un incorporato RadioButtonField, in caso contrario si potrebbe semplicemente aggiungere che alla griglia e di essere eseguite. In alternativa, è possibile aggiungere un TemplateField e configurare il relativo `ItemTemplate` per eseguire il rendering di un pulsante di opzione, risultante in un pulsante di opzione per ogni riga GridView.

Inizialmente, si potrebbe presuppone che l'interfaccia utente desiderato può essere implementata da un controllo RadioButton Web per l'aggiunta di `ItemTemplate` di un TemplateField. Mentre si aggiungerà effettivamente un singolo pulsante di opzione a ogni riga di GridView, i pulsanti di opzione non possono essere raggruppati e pertanto non si escludono a vicenda. Un utente finale è possibile selezionare più pulsanti di opzione contemporaneamente dal controllo GridView.

Anche se tramite un TemplateField controlli RadioButton Web non offre la funzionalità dobbiamo, s consentono di implementare questo approccio, come s utile per esaminare il motivo per cui non sono raggruppati i pulsanti di opzione risultanti. Per iniziare, aggiungere un TemplateField a GridView fornitori, rendendo il campo più a sinistra. Successivamente, da GridView s smart tag, fare clic sul collegamento di modifica modelli e trascinare un controllo RadioButton Web dalla casella degli strumenti in s TemplateField `ItemTemplate` (vedere la figura 10). Impostare i dispositivi di RadioButton `ID` proprietà `RowSelector` e `GroupName` proprietà `SuppliersGroup`.


[![Aggiungere un controllo RadioButton Web ItemTemplate](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.png)

**Figura 10**: aggiungere un controllo Web RadioButton per i `ItemTemplate` ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.png))


Dopo aver apportato queste aggiunte tramite la finestra di progettazione, nel markup di GridView s dovrebbe essere simile al seguente:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample3.aspx)]

I dispositivi di RadioButton [ `GroupName` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx) viene usata per raggruppare una serie di pulsanti di opzione. Tutti i controlli RadioButton con lo stesso `GroupName` valore sono considerati raggruppati; un solo pulsante di opzione selezionabile da un gruppo alla volta. Il `GroupName` proprietà specifica il valore per il pulsante di opzione visualizzabile s `name` attributo. Il browser esamina i pulsanti di opzione `name` gli attributi per determinare la radio pulsante raggruppamenti.

Con il controllo RadioButton Web aggiunto di `ItemTemplate`, visitare la pagina tramite un browser e fare clic sui pulsanti di opzione nelle righe della griglia s. Si noti come i pulsanti di opzione non sono raggruppati, che consente di selezionare tutte le righe, come nella figura 11 Mostra.


[![I pulsanti di opzione s GridView non sono raggruppati](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.png)

**Figura 11**: pulsanti di opzione s GridView non sono raggruppate ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.png))


Il motivo non sono raggruppati i pulsanti di opzione è che il rendering `name` gli attributi sono diversi, nonostante entrambe abbiano lo stesso `GroupName` l'impostazione della proprietà. Per visualizzare queste differenze, eseguire un'origine dal browser ed esaminare il markup di pulsante di opzione:


[!code-html[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample4.html)]

Si noti come il `name` e `id` attributi non sono i valori esatti, come specificato nella finestra Proprietà, ma sono preceduti da un numero di altri `ID` valori. Aggiuntiva `ID` valori aggiunti all'inizio dell'oggetto sottoposto a rendering `id` e `name` gli attributi sono il `ID` s della radio pulsanti controlli padre il `GridViewRow` s `ID` s, s GridView `ID`, Controllo s contenuto `ID`e il Web Form `ID`. Questi `ID` s vengono aggiunti in modo che viene eseguito il controllo Web in GridView è univoco `id` e `name` valori.

Ciascuno sottoposto a rendering un altro controllo `name` e `id` poiché si tratta come browser identifica in modo univoco ogni controllo lato client e come identifica al server web l'azione o è stata modificata durante il postback. Ad esempio, si supponga di voler eseguire il codice lato server ogni volta che un s RadioButton archiviato lo stato è stato modificato. Per eseguire questa operazione impostando s RadioButton `AutoPostBack` proprietà `True` e la creazione di un gestore eventi per il `CheckChanged` evento. Tuttavia, se il rendering `name` e `id` valori per tutti i pulsanti di opzione sono gli stessi in postback Impossibile determinare quali specifiche RadioButton è stato fatto clic.

Breve di esso è che è possibile creare una colonna di pulsanti di opzione in un controllo GridView mediante il controllo RadioButton Web. È necessario utilizzare invece Qoppa tecniche per garantire che il codice appropriato viene inserito in ogni riga GridView.

> [!NOTE]
> Ad esempio il controllo RadioButton Web, il controllo HTML, quando aggiunto a un modello di pulsante di opzione includerà univoci `name` attributo, rendendo i pulsanti di opzione nella griglia separata. Se non si ha familiarità con i controlli HTML, è possibile ignorare questa nota, come i controlli HTML vengono utilizzati raramente, soprattutto in ASP.NET 2.0. Ma se si è interessati in altre informazioni, vedere [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx) post di blog s [controlli Web e controlli HTML](http://www.odetocode.com/Articles/348.aspx).


## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>Utilizzo di un controllo valore letterale per inserire tag di pulsante di opzione

Per raggruppare correttamente tutti i pulsanti di opzione in GridView, è necessario inserire manualmente il markup di pulsanti di opzione nel `ItemTemplate`. Ogni pulsante di opzione è necessario lo stesso `name` attributo, ma deve essere univoco `id` attributo (nel caso in cui si desidera accedere a un pulsante di opzione tramite script sul lato client). Dopo che un utente seleziona un pulsante di opzione ed esegue il postback della pagina, il browser invierà il valore del pulsante di opzione selezionato s `value` attributo. Pertanto, sarà necessario univoca ogni pulsante di opzione `value` attributo. Infine, durante il postback è necessario assicurarsi di aggiungere il `checked` attributo al pulsante di opzione è selezionato, in caso contrario dopo che l'utente effettua una selezione e il post, i pulsanti di opzione verranno restituito lo stato predefinito (tutti non selezionati).

Sono disponibili due approcci che possono essere eseguiti per inserire codice di basso livello in un modello. Uno consiste nell'eseguire una combinazione di markup e chiamate ai metodi definiti nella classe code-behind di formattazione. Questa tecnica descritta prima nel [TemplateFields utilizzo nel controllo GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) esercitazione. In questo caso potrebbe essere simile al seguente:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample5.aspx)]

In questo caso, `GetUniqueRadioButton` e `GetRadioButtonValue` sarebbe metodi definiti con la classe code-behind che restituito appropriato `id` e `value` i valori per ogni pulsante di opzione di attributo. Questo approccio funziona bene per l'assegnazione di `id` e `value` gli attributi, ma non fornisce quando è necessario specificare il `checked` valore dell'attributo perché la sintassi di associazione dati viene eseguita solo quando i dati prima di tutto è associati a GridView. Pertanto, se il controllo GridView è abilitato lo stato di visualizzazione, i metodi di formattazione verranno generato solo quando la pagina viene caricata inizialmente o quando il controllo GridView è riassociato in modo esplicito all'origine dati e pertanto la funzione che imposta il `checked` attributo vinto t essere chiamato su postback. È un problema piuttosto sottile s e un po' esula dall'ambito di questo articolo, quindi verrà lasciarla questo. I si, tuttavia, è consigliabile provare a utilizzare l'approccio precedente e lavoro tramite il punto in cui verrà improvvisi. Mentre tale una t esercizio vinto ottenere qualsiasi più prossimo a una versione di lavoro, questo risulterà utile promuovere una comprensione più approfondita di GridView e il ciclo di vita di associazione dati.

Altri approcci per inserire personalizzato, markup di basso livello in un modello e l'approccio che verrà usato per questa esercitazione consiste nell'aggiungere un [controllo Literal](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx) al modello. Quindi, in GridView s `RowCreated` o `RowDataBound` gestore dell'evento, il controllo è possibile accedere a livello di codice e il relativo `Text` impostata per il markup da creare.

Iniziare a rimuovere il controllo RadioButton da s TemplateField `ItemTemplate`, sostituirlo con un controllo Literal. Impostare il valore letterale controllo s `ID` a `RadioButtonMarkup`.


[![Aggiungere un controllo Literal ItemTemplate](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.png)

**Figura 12**: aggiungere un valore letterale per il `ItemTemplate` ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.png))


Successivamente, creare un gestore eventi per s GridView `RowCreated` evento. Il `RowCreated` evento viene generato una volta per ogni riga aggiunta, o meno i dati sono da riassociati a GridView. Ciò significa che anche su un postback quando i dati viene ricaricati lo stato di visualizzazione, il `RowCreated` evento viene generato comunque e questo è il motivo viene usato in alternativa `RowDataBound` (che viene generato solo quando i dati sono associati in modo esplicito ai dati di controllo Web).

In questo gestore eventi, si vuole solo se è re la gestione di una riga di dati. Per ogni riga di dati si desidera fare riferimento a livello di codice il `RadioButtonMarkup` controllo Literal e impostare il relativo `Text` proprietà al markup da creare. Come mostrato nel codice seguente, il markup generato crea una radio pulsante la cui proprietà `name` attributo è impostato su `SuppliersGroup`, il cui `id` attributo è impostato su `RowSelectorX`, dove *X* è l'indice della riga GridView, e il cui `value` attributo è impostato per l'indice della riga GridView.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample6.vb)]

Quando è selezionata una riga GridView e si verifica un postback, si intende il `SupplierID` del fornitore selezionato. Pertanto, si pensa che il valore di ogni pulsante di opzione deve essere l'effettivo `SupplierID` (anziché l'indice della riga GridView). Mentre questo potrebbe funzionare in determinate circostanze, sarebbe un rischio per la sicurezza di ciecamente accettare ed elaborare un `SupplierID`. Il GridView, ad esempio, elenca solo i fornitori negli Stati Uniti. Tuttavia, se il `SupplierID` viene passato direttamente dal pulsante di opzione, una novità per impedire la modifica di un utente operazioni dannose di `SupplierID` valore inviato nuovamente durante il postback? Tramite l'indice di riga come il `value`e quindi ottenendo il `SupplierID` durante il postback dal `DataKeys` insieme, è possibile assicurarsi che l'utente utilizza solo uno del `SupplierID` valori associati a una delle righe GridView.

Dopo aver aggiunto il codice del gestore eventi, è opportuno testare la pagina in un browser. In primo luogo, si noti che solo un'opzione selezionabile in fase di un pulsante nella griglia. Tuttavia, quando la selezione di un pulsante di opzione e fare clic su uno dei pulsanti, si verifica un postback e ripristino tutti i pulsanti di opzione allo stato iniziale (ovvero, durante il postback, il pulsante di opzione selezionato non è più selezionato). Per risolvere questo problema, è necessario aumentare il `RowCreated` gestore dell'evento in modo che controlla se l'indice di pulsante di opzione selezionato inviato dal postback e aggiunge il `checked="checked"` attributo per il markup generato delle corrispondenze di indice di riga.

Quando si verifica un postback, il browser invia nuovamente il `name` e `value` del pulsante di opzione selezionato. Il valore può essere recuperato a livello di programmazione tramite `Request.Form("name")`. Il [ `Request.Form` proprietà](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx) fornisce un [ `NameValueCollection` ](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx) che rappresentano le variabili di form. Variabili dei form sono i nomi e valori dei campi nella pagina web form e vengono inviate nuovamente tramite il web browser ogni volta che viene utilizzata un postback. Poiché il rendering `name` attributo dei pulsanti di opzione in GridView `SuppliersGroup`, quando la pagina web viene eseguito il postback invierà il browser `SuppliersGroup=valueOfSelectedRadioButton` al server web (insieme ad altri campi di formato). Queste informazioni è possibile accedere dal `Request.Form` con proprietà: `Request.Form("SuppliersGroup")`.

Poiché si sarà necessario per determinare il pulsante di opzione selezionato di indice non solo nel `RowCreated` gestore eventi, mentre nel `Click` i gestori eventi per i controlli pulsante Web, s consentono di aggiungere un `SuppliersSelectedIndex` proprietà per la classe code-behind che restituisce `-1`se è stato selezionato alcun pulsante di opzione e l'indice selezionato, se uno dei pulsanti di opzione è selezionato.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample7.vb)]

Questa proprietà aggiunto, sappiamo per aggiungere il `checked="checked"` markup nel `RowCreated` gestore dell'evento quando `SuppliersSelectedIndex` è uguale a `e.Row.RowIndex`. Aggiornare il gestore dell'evento per includere tale logica:


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample8.vb)]

Con questa modifica, il pulsante di opzione selezionato rimane selezionato dopo un postback. Ora che è disponibile la possibilità di specificare il pulsante di opzione è selezionato, è possibile modificare il comportamento in modo che quando la pagina prima di tutto è stata visitata, è stato selezionato il primo pulsante di opzione s riga GridView (anziché non avere alcun pulsanti di opzione selezionati per impostazione predefinita, ovvero corrente comportamento). Per il primo pulsante di opzione selezionato per impostazione predefinita, è sufficiente modificare il `If SuppliersSelectedIndex = e.Row.RowIndex Then` indicato di seguito: `If SuppliersSelectedIndex = e.Row.RowIndex OrElse (Not Page.IsPostBack AndAlso e.Row.RowIndex = 0) Then`.

A questo punto è stata aggiunta una colonna di pulsanti di opzione raggruppati a GridView che consente una singola riga GridView selezionato e memorizzate durante i postback. I passaggi successivi sono per visualizzare i prodotti specificati dal fornitore selezionato. Nel passaggio 4 si vedrà come reindirizzare l'utente a un'altra pagina, l'invio lungo selezionato `SupplierID`. Nel passaggio 5, vedremo come visualizzare i prodotti s fornitore selezionato in un controllo GridView nella stessa pagina.

> [!NOTE]
> Anziché utilizzare un TemplateField (l'obiettivo di questo passaggio 3 di lunga durata), è possibile creare una classe personalizzata `DataControlField` classe che esegue il rendering dell'interfaccia utente appropriata e funzionalità. Il [ `DataControlField` classe](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx) è la classe base da cui derivano i BoundField, CheckBoxField, TemplateField e altri campi predefiniti di GridView e DetailsView. Creazione di un oggetto personalizzato `DataControlField` classe significa che la colonna di pulsanti di opzione è stato possibile aggiungere solo utilizzando la sintassi dichiarativa e renderebbe inoltre vengono replicate le funzionalità in altre pagine web e altre applicazioni web molto più semplice.


Se è già creato personalizzato, compilato controlli ASP.NET, tuttavia, è noto che tale pertanto richiede una notevole quantità di approfondito ed è caratterizzata da un host di sfumature e casi limite che devono essere gestiti con attenzione. Pertanto, si verrà evitare l'implementazione di una colonna di pulsanti di opzione come proprietà personalizzata `DataControlField` classe per il momento e continuare con l'opzione TemplateField. Ad esempio sono la possibilità di esplorare la creazione, l'utilizzo e distribuzione personalizzate `DataControlField` classi in un'esercitazione future!

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>Passaggio 4: Visualizzare i prodotti s fornitore selezionato in una pagina separata

Dopo che l'utente ha selezionato una riga GridView, è necessario visualizzare il fornitore selezionato prodotti s. In alcuni casi, potrebbe voler visualizzare questi prodotti in una pagina separata, in altri casi che potrebbe essere consigliabile eseguire questa operazione nella stessa pagina. Consente di esaminare innanzitutto illustrato come visualizzare i prodotti in una pagina separata; s nel passaggio 5 vedremo durante l'aggiunta di un controllo GridView per `RadioButtonField.aspx` per visualizzare i prodotti selezionati fornitore s.

Attualmente sono disponibili due controlli pulsante Web nella pagina `ListProducts` e `SendToProducts`. Quando il `SendToProducts` si fa clic sul pulsante, si desidera inviare all'utente di `~/Filtering/ProductsForSupplierDetails.aspx`. Questa pagina è stata creata la [Master-Details filtro tra due pagine](../masterdetail/master-detail-filtering-across-two-pages-vb.md) esercitazione e visualizza i prodotti per il fornitore il cui `SupplierID` viene passato tramite il campo querystring denominato `SupplierID`.

Per fornire questa funzionalità, creare un gestore eventi per il `SendToProducts` pulsante s `Click` evento. Nel passaggio 3 è stato aggiunto il `SuppliersSelectedIndex` proprietà, che restituisce l'indice della riga il cui pulsante di opzione è selezionata. Corrispondente `SupplierID` possono essere recuperati da s GridView `DataKeys` raccolta e l'utente può quindi essere inviati a `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` utilizzando `Response.Redirect("url")`.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample9.vb)]

Questo codice funziona estesamente come uno dei pulsanti di opzione è selezionato dal controllo GridView. Se inizialmente GridView non dispone dei pulsanti di opzione selezionati e l'utente fa clic il `SendToProducts` pulsante `SuppliersSelectedIndex` sarà `-1`, che genera un'eccezione generata dal `-1` è compreso nell'intervallo di indice del `DataKeys`insieme. L'operazione non è un problema, tuttavia, se si è deciso di aggiornare il `RowCreated` gestore eventi, come descritto nel passaggio 3 in modo che il primo pulsante di opzione in GridView inizialmente selezionato.

Per supportare un `SuppliersSelectedIndex` valore `-1`, aggiungere un controllo etichetta Web alla pagina sopra il controllo GridView. Impostare il `ID` proprietà `ChooseSupplierMsg`, le `CssClass` proprietà `Warning`, le `EnableViewState` e `Visible` proprietà `False`e il relativo `Text` proprietà Please scegliere un fornitore dalla griglia. Classe CSS `Warning` Visualizza il testo in un tipo di carattere rosso, corsivo, grassetto, grande e viene definito in `Styles.css`. Impostando il `EnableViewState` e `Visible` proprietà `False`, l'etichetta non viene eseguita, ad eccezione per i soli postback dove il controllo s `Visible` è impostata a livello di codice su `True`.


[![Aggiungere un controllo Web etichetta sopra il controllo GridView.](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image21.png)

**Figura 13**: aggiungere un'etichetta Web controllo sopra GridView ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-radio-buttons-vb/_static/image22.png))


Successivamente, aumentare il `Click` gestore eventi per visualizzare il `ChooseSupplierMsg` etichetta se `SuppliersSelectedIndex` è minore di zero e reindirizzare l'utente per `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` in caso contrario.


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample10.vb)]

Visitare la pagina in un browser e scegliere il `SendToProducts` pulsante prima di selezionare un fornitore dal controllo GridView. Come mostrato nella figura 14, verrà visualizzata la `ChooseSupplierMsg` etichetta. Successivamente, selezionare un fornitore e fare clic sul `SendToProducts` pulsante. Questo verrà whisk è a una pagina che elenca i prodotti offerti dal fornitore selezionato. Viene illustrato nella figura 15 il `ProductsForSupplierDetails.aspx` pagina quando è stato selezionato il fornitore birrerie Bigfoot.


[![L'etichetta ChooseSupplierMsg viene visualizzata se si seleziona No fornitore](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image23.png)

**Figura 14**: il `ChooseSupplierMsg` etichetta viene visualizzata se si seleziona No fornitore ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-radio-buttons-vb/_static/image24.png))


[![I prodotti s fornitore selezionato vengono visualizzati in ProductsForSupplierDetails.aspx](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image25.png)

**Figura 15**: i prodotti s il fornitore selezionato vengono visualizzati nella `ProductsForSupplierDetails.aspx` ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-radio-buttons-vb/_static/image26.png))


## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>Passaggio 5: Visualizzare i prodotti s fornitore selezionato nella stessa pagina

Nel passaggio 4 è stato illustrato come inviare all'utente a un'altra pagina web per visualizzare il fornitore selezionato prodotti s. In alternativa, è possono visualizzare i prodotti s fornitore selezionato nella stessa pagina. Per illustrare questo concetto, verrà aggiunto un altro controllo GridView per `RadioButtonField.aspx` per visualizzare i prodotti selezionati fornitore s.

Poiché si desidera solo questo GridView dei prodotti per la visualizzazione dopo aver selezionato un fornitore, aggiungere un controllo pannello Web sotto il `Suppliers` GridView, impostare il relativo `ID` per `ProductsBySupplierPanel` e il relativo `Visible` proprietà `False`. All'interno del pannello, aggiungere il testo prodotti per il fornitore selezionato, seguito da un controllo GridView denominato `ProductsBySupplier`. GridView s smart tag, scegliere di associarlo a un nuovo oggetto ObjectDataSource denominato `ProductsBySupplierDataSource`.


[![Associare un nuovo oggetto ObjectDataSource ProductsBySupplier GridView](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image27.png)

**Figura 16**: associare il `ProductsBySupplier` GridView per un nuovo oggetto ObjectDataSource ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-radio-buttons-vb/_static/image28.png))


A questo punto, configurare ObjectDataSource per utilizzare il `ProductsBLL` classe. Poiché si desidera solo recuperare i prodotti specificati dal fornitore selezionato, specificare che deve richiamare ObjectDataSource il `GetProductsBySupplierID(supplierID)` metodo per recuperare i dati. Selezionare (nessuno) dagli elenchi a discesa nell'aggiornamento, inserimento ed eliminare le schede.


[![Configurare ObjectDataSource per utilizzare il metodo GetProductsBySupplierID(supplierID)](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image29.png)

**Figura 17**: configurare ObjectDataSource per usare il `GetProductsBySupplierID(supplierID)` metodo ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-radio-buttons-vb/_static/image30.png))


[![Impostare gli elenchi di elenco a discesa su (nessuno) in UPDATE, INSERT ed eliminare le schede](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image31.png)

**Figura 18**: impostare l'elenco a discesa sono elencati su (nessuno) nelle schede DELETE, INSERT e UPDATE ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-radio-buttons-vb/_static/image32.png))


Dopo aver configurato l'istruzione SELECT, aggiornare, inserire, eliminare le schede, fare clic su Avanti. Poiché il `GetProductsBySupplierID(supplierID)` metodo prevede un parametro di input, la procedura guidata Crea origine dati richiede di specificare l'origine per il valore del parametro s.

Sono disponibili due opzioni qui in specificare l'origine del valore del parametro s. È possibile utilizzare l'oggetto del parametro predefinito e assegnare a livello di codice il valore della `SuppliersSelectedIndex` proprietà per il parametro s `DefaultValue` proprietà in s ObjectDataSource `Selecting` gestore dell'evento. Fare riferimento al [a livello di codice impostando i valori dei parametri di ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb.md) esercitazione per un aggiornamento per l'assegnazione a livello di programmazione di valori per i parametri di ObjectDataSource s.

In alternativa, è possibile utilizzare un ControlParameter e fare riferimento al `Suppliers` GridView s [ `SelectedValue` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx) (vedere Figura 19). S GridView `SelectedValue` proprietà restituisce il `DataKey` valore corrispondente al [ `SelectedIndex` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx). Affinché il corretto funzionamento di questa opzione, è necessario impostare a livello di programmazione s GridView `SelectedIndex` proprietà selezionato riga quando il `ListProducts` si fa clic sul pulsante. Un ulteriore vantaggio, impostando il `SelectedIndex`, assumerà il record selezionato il `SelectedRowStyle` definito nel `DataWebControls` tema (uno sfondo giallo).


[![Utilizzare un ControlParameter per specificare SelectedValue s GridView come origine del parametro](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image33.png)

**Figura 19**: usare un ControlParameter per specificare gli oggetti di GridView SelectedValue come origine del parametro ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-radio-buttons-vb/_static/image34.png))


Dopo avere completato la procedura guidata, Visual Studio aggiungerà automaticamente i campi per i campi di dati prodotto s. Rimuovere tutto tranne il `ProductName`, `CategoryName`, e `UnitPrice` , BoundField e modificare il `HeaderText` le proprietà per prodotto, categoria e prezzo. Configurare il `UnitPrice` BoundField in modo che il relativo valore viene formattato come valuta. Dopo aver apportato queste modifiche, il pannello di controllo GridView e ObjectDataSource s dichiarativo dovrebbe essere simile al seguente:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample11.aspx)]

Per completare questo esercizio, è necessario impostare i dispositivi di GridView `SelectedIndex` proprietà per il `SelectedSuppliersIndex` e `ProductsBySupplierPanel` pannello s `Visible` proprietà `True` quando il `ListProducts` si fa clic sul pulsante. A tale scopo, creare un gestore eventi per il `ListProducts` controllo Web Button s `Click` eventi e aggiungere il codice seguente:


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample12.vb)]

Se non è stato selezionato un fornitore da GridView, il `ChooseSupplierMsg` etichetta viene visualizzata e `ProductsBySupplierPanel` pannello nascosto. In caso contrario, se è stato selezionato un fornitore, il `ProductsBySupplierPanel` viene visualizzato e i dispositivi di GridView `SelectedIndex` proprietà viene aggiornata.

Figura 20 vengono visualizzati i risultati fornitore Bigfoot birrerie è stato selezionato e i prodotti visualizzare sul pulsante di pagina è stato fatto clic.


[![Prodotti forniti da Bigfoot birrerie sono elencati nella stessa pagina](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image35.png)

**Figura 20**: il prodotti forniti da Bigfoot birrerie sono elencati nella stessa pagina ([fare clic per visualizzare l'immagine ingrandita](adding-a-gridview-column-of-radio-buttons-vb/_static/image36.png))


## <a name="summary"></a>Riepilogo

Come descritto nel [Master/dettaglio con un controllo DetailView per i dettagli di Master GridView selezionabile](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) dell'esercitazione, i record possono essere selezionati da un GridView con utilizzo di un CommandField cui `ShowSelectButton` è impostata su `True`. Ma il CommandField consente di visualizzare i relativi pulsanti come pulsanti di comando normale, collegamenti o immagini. Un'interfaccia utente di selezione della riga alternativo consiste nel fornire un pulsante di opzione o una casella di controllo in ogni riga GridView. In questa esercitazione sono state esaminate come aggiungere una colonna di pulsanti di opzione.

Purtroppo, aggiungendo una colonna di t presenti i pulsanti di opzione come diretta o semplice, come ci si aspetterebbe. Non vi è alcun RadioButtonField incorporato che può essere aggiunto a un semplice clic del mouse e il controllo RadioButton Web all'interno di un TemplateField introduce una serie di problemi. Alla fine, per fornire tale interfaccia è necessario creare una classe personalizzata `DataControlField` classe o una risorsa per l'inserimento HTML appropriato in un TemplateField durante il `RowCreated` evento.

Avere esplorato come aggiungere una colonna di pulsanti di opzione farci come aggiunta di una colonna di caselle di controllo. Con una colonna di caselle di controllo, un utente può selezionare una o più righe di GridView e quindi eseguire alcune operazioni su tutte le righe selezionate (ad esempio la selezione di un set di messaggi di posta elettronica da un client di posta elettronica basato sul web e quindi scegliendo di eliminare tutti i messaggi di posta elettronica selezionati). Nella prossima esercitazione vedremo come aggiungere tale colonna.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata David Suru. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](inserting-a-new-record-from-the-gridview-s-footer-cs.md)
> [Successivo](adding-a-gridview-column-of-checkboxes-vb.md)
