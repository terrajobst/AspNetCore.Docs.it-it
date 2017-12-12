---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
title: Specifica la pagina Master a livello di codice (VB) | Documenti Microsoft
author: rick-anderson
description: Esamina il contenuto della pagina master a livello di codice tramite il gestore dell'evento PreInit impostazione.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 0edcd653-f24a-41aa-aef4-75f868fe5ac2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
msc.type: authoredcontent
ms.openlocfilehash: 090d0777b9d541003c3115d0da7cd974820c2939
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="specifying-the-master-page-programmatically-vb"></a>Specifica la pagina Master a livello di codice (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.pdf)

> Esamina il contenuto della pagina master a livello di codice tramite il gestore dell'evento PreInit impostazione.


## <a name="introduction"></a>Introduzione

Dopo il primo esempio [ *la creazione di un Layout a livello di sito utilizzando le pagine Master*](creating-a-site-wide-layout-using-master-pages-vb.md), tutto il contenuto di pagine di riferimento la pagina master in modo dichiarativo tramite la `MasterPageFile` attributo la `@Page`direttiva. Ad esempio, `@Page` direttiva collega la pagina di contenuto della pagina master `Site.master`:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample1.aspx)]

Il [ `Page` classe](https://msdn.microsoft.com/en-us/library/system.web.ui.page.aspx) nel `System.Web.UI` spazio dei nomi include un [ `MasterPageFile` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.page.masterpagefile.aspx) che restituisce il percorso per il contenuto della pagina master, ma è anche questa proprietà per l'impostazione è la `@Page` direttiva. Questa proprietà può essere utilizzata anche per specificare a livello di codice il contenuto della pagina master. Questo approccio è utile se si desidera assegnare in modo dinamico la pagina master in base a fattori esterni, ad esempio l'utente visita la pagina.

In questa esercitazione è aggiungere una seconda pagina master per il sito Web e decidere quale pagina master da utilizzare in fase di esecuzione.

## <a name="step-1-a-look-at-the-page-lifecycle"></a>Passaggio 1: Esaminare il ciclo di vita di pagina

Ogni volta che arriva una richiesta nel server web per una pagina ASP.NET che è una pagina di contenuto, è necessario associare il motore ASP.NET il della pagina contenuto controlli ContentPlaceHolder corrispondente del controlli nella pagina master. Questo fusione crea una gerarchia singolo controllo che può passare attraverso il ciclo di vita di pagina tipiche.

La figura 1 illustra questo fusion. Passaggio 1 nella figura 1 mostra le gerarchie di controllo pagina master e il contenuto iniziale. Alla fine della parte finale della fase PreInit il contenuto della pagina vengono aggiunti al ContentPlaceHolder corrispondente nella pagina master (passaggio 2). Dopo questo fusion, la pagina master funge da radice della gerarchia del controllo fusibili. Questo controllo con fusibile gerarchia viene quindi aggiunto alla pagina per produrre la gerarchia del controllo finalizzato (passaggio 3). Il risultato è che la gerarchia dei controlli della pagina include la gerarchia dei controlli fusibili.


[![La pagina Master e le gerarchie di controllo del contenuto della pagina sono fusibile insieme durante la fase PreInit](specifying-the-master-page-programmatically-vb/_static/image2.png)](specifying-the-master-page-programmatically-vb/_static/image1.png)

**Figura 01**: la pagina Master e contenuto pagina gerarchie di controllo sono fusibile insieme durante la fase PreInit ([fare clic per visualizzare l'immagine ingrandita](specifying-the-master-page-programmatically-vb/_static/image3.png))


## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>Passaggio 2: Configurazione di`MasterPageFile`proprietà dal codice

La pagina master partakes in questo fusione dipende dal valore del `Page` dell'oggetto `MasterPageFile` proprietà. Impostazione di `MasterPageFile` attributo il `@Page` direttiva ha l'effetto dell'assegnazione il `Page`del `MasterPageFile` proprietà durante la fase di inizializzazione, che è la prima fase del ciclo di vita della pagina. In alternativa è possibile impostare questa proprietà a livello di codice. Tuttavia, è fondamentale che questa proprietà deve essere impostata prima che venga eseguita di fusione nella figura 1.

All'inizio della fase PreInit il `Page` oggetto genera relativo [ `PreInit` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.page.preinit.aspx) e chiama il relativo [ `OnPreInit` metodo](https://msdn.microsoft.com/en-us/library/system.web.ui.page.onpreinit.aspx). Per impostare a livello di codice della pagina master, quindi, è possibile creare un gestore eventi per il `PreInit` evento o eseguire l'override di `OnPreInit` metodo. Diamo un'occhiata entrambi gli approcci.

Avvia aprendo `Default.aspx.vb`, il file di classe code-behind per la home page del sito. Aggiungere un gestore eventi per la pagina `PreInit` evento digitando il codice seguente:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample2.vb)]

Da qui è possibile impostare il `MasterPageFile` proprietà. Aggiornare il codice in modo che assegna il valore "~ / Site. master" per il `MasterPageFile` proprietà.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample3.vb)]

Se si imposta un punto di interruzione e avvia il debug si noterà che ogni volta che il `Default.aspx` viene visitata una pagina o ogni volta che è presente un postback a questa pagina, il `Page_PreInit` gestore eventi viene eseguito e `MasterPageFile` proprietà viene assegnata a "~ / Site. master".

In alternativa, è possibile eseguire l'override di `Page` della classe `OnPreInit` metodo e impostare il `MasterPageFile` proprietà non esiste. Per questo esempio, non si impostare la pagina master in una pagina specifica, ma piuttosto da `BasePage`. Tenere presente che è stata creata una classe di pagina base personalizzata (`BasePage`) nei [ *che specifica il titolo, tag Meta e altre intestazioni HTML della pagina Master* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) esercitazione. Attualmente `BasePage` esegue l'override di `Page` della classe `OnLoadComplete` (metodo), in cui imposta la pagina `Title` proprietà basate sui dati della mappa del sito. Consente di aggiornare `BasePage` eseguire anche l'override di `OnPreInit` per specificare a livello di codice della pagina master.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample4.vb)]

Poiché tutte le pagine contenute derivano da `BasePage`, tutti gli elementi hanno ora la pagina master a livello di codice assegnata. A questo punto il `PreInit` gestore dell'evento in `Default.aspx.vb` è superflua, è possibile rimuoverlo.

### <a name="what-about-thepagedirective"></a>Per quanto riguarda la`@Page`direttiva?

Che è possibile confusione è che le pagine contenute `MasterPageFile` proprietà ora vengono specificate in due posizioni: a livello di codice nel `BasePage` della classe `OnPreInit` metodo anche mediante il `MasterPageFile` attributo in ogni pagina di contenuto `@Page` direttiva.

La prima fase del ciclo di vita di pagina è la fase di inizializzazione. Durante questa fase il `Page` dell'oggetto `MasterPageFile` proprietà viene assegnato il valore della `MasterPageFile` attributo il `@Page` (direttiva) (se fornito). La fase PreInit segue la fase di inizializzazione ed è a questo punto in cui è impostato a livello di codice il `Page` dell'oggetto `MasterPageFile` proprietà, in tal modo la sovrascrittura del valore assegnato dal `@Page` direttiva. Perché si sta impostando il `Page` dell'oggetto `MasterPageFile` proprietà a livello di codice è stato possibile rimuovere il `MasterPageFile` dall'attributo di `@Page` direttiva senza influire sull'esperienza dell'utente finale. Per convincere manualmente di questo, vado avanti e rimuovere il `MasterPageFile` dall'attributo di `@Page` direttiva `Default.aspx` e quindi visitare la pagina tramite un browser. Come prevedibile, l'output è identico a prima che l'attributo è stato rimosso.

Se il `MasterPageFile` viene impostata tramite il `@Page` direttiva o a livello di codice non è rilevante per l'esperienza dell'utente finale. Tuttavia, il `MasterPageFile` attributo la `@Page` direttiva viene utilizzata da Visual Studio in fase di progettazione per produrre il WYSIWYG visualizzazione nella finestra di progettazione. Se si torna alla `Default.aspx` in Visual Studio e passare alla finestra di progettazione verrà visualizzato il messaggio "errore di pagina Master: la pagina contiene i controlli che richiedono un riferimento alla pagina Master, ma non è specificato" (vedere la figura 2).

In breve, è necessario lasciare il `MasterPageFile` attributo la `@Page` direttiva usufruire esperienze in fase di progettazione in Visual Studio.


[![Visual Studio Usa il @Page attributo MasterPageFile della direttiva per il rendering della visualizzazione Progettazione](specifying-the-master-page-programmatically-vb/_static/image5.png)](specifying-the-master-page-programmatically-vb/_static/image4.png)

**Figura 02**: Visual Studio Usa il `@Page` della direttiva `MasterPageFile` attributo per il rendering, la visualizzazione di progettazione ([fare clic per visualizzare l'immagine ingrandita](specifying-the-master-page-programmatically-vb/_static/image6.png))


## <a name="step-3-creating-an-alternative-master-page"></a>Passaggio 3: Creazione di una pagina Master alternativa

Poiché un contenuto della pagina master può essere impostata a livello di codice in fase di esecuzione è possibile caricare dinamicamente una pagina master specifica in base ad alcuni criteri esterni. Questa funzionalità può essere utile nelle situazioni in cui il layout del sito deve variano in base all'utente. Ad esempio, un'applicazione web di motore di blog consentono agli utenti di scegliere un layout per i blog, dove ogni layout è associata a un'altra pagina master. In fase di esecuzione, quando un visitatore visualizza blog di un utente, l'applicazione web era necessario determinare il layout del blog e associare in modo dinamico la pagina master corrispondente alla pagina contenuto.

Esaminiamo come caricare dinamicamente una pagina master in fase di esecuzione in base ad alcuni criteri esterni. Attualmente, il sito Web contiene una sola pagina master (`Site.master`). Abbiamo bisogno di un'altra pagina master per illustrare la scelta di una pagina master in fase di esecuzione. Questo passaggio è incentrata sulla creazione e la configurazione della nuova pagina master. Passaggio 4: esamina determinare quale pagina master da utilizzare in fase di esecuzione.

Creare una nuova pagina master nella cartella radice denominata `Alternate.master`. Inoltre, aggiungere un nuovo foglio di stile per il sito Web denominato `AlternateStyles.css`.


[![Aggiungere un altro File pagina Master e CSS al sito Web](specifying-the-master-page-programmatically-vb/_static/image8.png)](specifying-the-master-page-programmatically-vb/_static/image7.png)

**Figura 03**: aggiungere un'altra pagina Master e il File CSS al sito Web ([fare clic per visualizzare l'immagine ingrandita](specifying-the-master-page-programmatically-vb/_static/image9.png))


È stato progettato il `Alternate.master` pagina master per il titolo visualizzato nella parte superiore della pagina, centrata e su uno sfondo blu. Ho distribuito della colonna a sinistra e spostare tale contenuto di sotto di `MainContent` controllo ContentPlaceHolder, che si estende per l'intera larghezza della pagina. Inoltre, si nixed l'elenco non ordinato di lezioni e sostituito da un elenco orizzontale `MainContent`. Aggiornato anche i tipi di carattere e colori utilizzati dalla pagina master (e, di conseguenza, le pagine di contenuto). La figura 4 mostra `Default.aspx` quando si utilizza il `Alternate.master` pagina master.

> [!NOTE]
> ASP.NET include la possibilità di definire *temi*. Un tema è una raccolta di immagini, file CSS e relative allo stile Web proprietà impostazioni di controllo possono essere applicate a una pagina in fase di esecuzione. I temi sono il modo se il layout del sito sono diversi solo in immagini visualizzate e le regole CSS. Se i layout diversi più significativo, ad esempio utilizzando i controlli Web diversi o con un layout di completamente diverso, è necessario utilizzare pagine master separate. Alla fine di questa esercitazione per ulteriori informazioni su temi, consultare la sezione di approfondimento.


[![Le pagine contenute ora è possono usare un nuovo aspetto](specifying-the-master-page-programmatically-vb/_static/image11.png)](specifying-the-master-page-programmatically-vb/_static/image10.png)

**Figura 04**: le pagine contenute ora è possono usare un nuovo aspetto ([fare clic per visualizzare l'immagine ingrandita](specifying-the-master-page-programmatically-vb/_static/image12.png))


Quando il server principale e il markup di pagine contenuto sono fusibile, la `MasterPage` classe controlli per assicurarsi che ogni contenuto controllo nella pagina di contenuto fa riferimento a un controllo ContentPlaceHolder nella pagina master. Se viene trovato un controllo contenuto che fa riferimento a un controllo ContentPlaceHolder inesistente, viene generata un'eccezione. In altre parole, è fondamentale che la pagina master da assegnare alla pagina di contenuto include un ContentPlaceHolder per ogni controllo nella pagina di contenuto del contenuto.

Il `Site.master` pagina master include quattro controlli ContentPlaceHolder:

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

Alcune delle pagine di contenuto nel sito includono i controlli contenuti solo uno o due. altri includono un controllo contenuto per ogni il ContentPlaceHolder disponibili. Se la pagina master (`Alternate.master`) può mai essere assegnato a tali pagine di contenuto che dispone di controlli contenuto per tutti i elementi ContentPlaceHolder in `Site.master` è essenziale che `Alternate.master` anche includere gli stessi controlli ContentPlaceHolder di `Site.master`.

Per ottenere il `Alternate.master` pagina master per renderli simili per estrarre (vedere la figura 4), per iniziare, definire gli stili della pagina master di `AlternateStyles.css` foglio di stile. Aggiungere le regole seguenti in `AlternateStyles.css`:


[!code-css[Main](specifying-the-master-page-programmatically-vb/samples/sample5.css)]

Successivamente, aggiungere il seguente markup dichiarativo per `Alternate.master`. Come si può notare, `Alternate.master` contiene quattro controlli ContentPlaceHolder con lo stesso `ID` i valori dei controlli ContentPlaceHolder `Site.master`. Inoltre, include un controllo ScriptManager, che è necessario per le pagine il sito Web che utilizzano il framework ASP.NET AJAX.


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>Test della nuova pagina Master

Per testare questo nuovo aggiornamento della pagina master di `BasePage` della classe `OnPreInit` metodo in modo che il `MasterPageFile` proprietà viene assegnato il valore `"~/Alternate.maser"` e quindi visitare il sito Web. Ogni pagina dovrebbe funzionare senza errori, ad eccezione di due: `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx`. Aggiunta di un prodotto al controllo DetailsView in `~/Admin/AddProduct.aspx` comporta un `NullReferenceException` dalla riga di codice che tenta di impostare la pagina master `GridMessageText` proprietà. Durante la visita `~/Admin/Products.aspx` un `InvalidCastException` viene generata al caricamento della pagina con il messaggio: "Impossibile eseguire il cast dell'oggetto di tipo ' ASP.alternate\_master' nel tipo ' ASP.site\_master'."

Questi errori si verificano perché le `Site.master` classe code-behind include gli eventi pubblici, proprietà e metodi che non sono definiti in `Alternate.master`. La parte di markup di queste due pagine hanno un `@MasterType` direttiva che fa riferimento il `Site.master` pagina master.


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample7.aspx)]

Inoltre, di DetailsView `ItemInserted` gestore dell'evento in `~/Admin/AddProduct.aspx` include il codice che esegue il cast di fortemente tipizzato `Page.Master` proprietà a un oggetto di tipo `Site`. Il `@MasterType` (utilizzata in questo modo) e cast nel `ItemInserted` gestore dell'evento è strettamente collegato il `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` delle pagine di `Site.master` pagina master.

Per interrompere il possibilità di installare l'accoppiamento stretto `Site.master` e `Alternate.master` derivano da una classe di base comune che contiene le definizioni per i membri pubblici. Successivamente, è possibile aggiornare il `@MasterType` direttiva per fare riferimento a questo tipo di base comune.

### <a name="creating-a-custom-base-master-page-class"></a>Creazione di una classe di pagina Master Base personalizzata

Aggiungere un nuovo file di classe per il `App_Code` cartella denominata `BaseMasterPage.vb` e fare in modo che derivano da `System.Web.UI.MasterPage`. È necessario definire il `RefreshRecentProductsGrid` (metodo) e il `GridMessageText` proprietà `BaseMasterPage`, ma non è semplicemente possibile spostare le da `Site.master` poiché questi membri funzionano con i controlli Web che sono specifici per il `Site.master` pagina master (il `RecentProducts` GridView e `GridMessage` etichetta).

È necessario eseguire viene configurata `BaseMasterPage` in modo che questi membri sono definiti, ma in realtà sono implementati da `BaseMasterPage`di classi derivate (`Site.master` e `Alternate.master`). Questo tipo di ereditarietà è possibile contrassegnare la classe come `MustInherit` e i relativi membri come `MustOverride`. In breve, l'aggiunta di queste parole chiave per la classe e i relativi due membri annuncia che `BaseMasterPage` non è implementato `RefreshRecentProductsGrid` e `GridMessageText`, ma che verranno le relative classi derivate.

È anche necessario definire il `PricesDoubled` evento in `BaseMasterPage` e forniscono un mezzo dalle classi derivate per generare l'evento. Il modello utilizzato in .NET Framework per semplificare questo comportamento consiste nel creare un evento pubblico nella classe base e aggiungere un metodo sottoponibile a override, protetto denominato `OnEventName`. Le classi derivate possono quindi chiamare questo metodo per generare l'evento o possono eseguirne l'override per eseguire codice immediatamente prima o dopo l'evento viene generato.

Aggiornamento del `BaseMasterPage` classe in modo che contenga il codice seguente:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample8.vb)]

Passare quindi al `Site.master` codice classe e fare in modo che derivano da `BaseMasterPage`. Poiché `BaseMasterPage` contiene membri contrassegnati `MustOverride` è necessario eseguire l'override di tali membri in `Site.master`. Aggiungere il `Overrides` (parola chiave) per le definizioni di metodo e proprietà. Aggiornare anche il codice che genera il `PricesDoubled` evento nel `DoublePrice` del pulsante `Click` gestore dell'evento con una chiamata per la classe base `OnPricesDoubled` metodo.

Dopo queste modifiche di `Site.master` classe code-behind deve contenere il codice seguente:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample9.vb)]

È anche necessario aggiornare `Alternate.master`della classe code-behind da cui derivare `BaseMasterPage` ed eseguire l'override di due `MustOverride` membri. Tuttavia, poiché `Alternate.master` non contiene un controllo GridView che elenca i prodotti più recenti né un'etichetta che viene visualizzato un messaggio dopo un nuovo prodotto viene aggiunto al database, questi metodi non è necessario eseguire alcuna operazione.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample10.vb)]

### <a name="referencing-the-base-master-page-class"></a>La classe delle pagine Master di Base di riferimento

Ora che è stata completata la `BaseMasterPage` classe e i due pagine master estende, il passaggio finale consiste nell'aggiornare il `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` pagine per fare riferimento a questo tipo comune. Iniziare modificando il `@MasterType` direttiva in entrambe le pagine da:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample11.aspx)]

A:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample12.aspx)]

Invece di riferimento a un percorso di file, il `@MasterType` proprietà fa riferimento a questo punto il tipo di base (`BaseMasterPage`). Di conseguenza, fortemente tipizzata `Master` le classi di codice delle due pagine di proprietà è di tipo `BaseMasterPage` (anziché di tipo `Site`). Con questa modifica sul posto rivedere `~/Admin/Products.aspx`. In precedenza, questo ha comportato un errore di cast perché la pagina è configurata per utilizzare il `Alternate.master` pagina master, ma la `@MasterType` direttiva a cui fa riferimento il `Site.master` file. Ma ora il rendering della pagina senza errori. In questo modo il `Alternate.master` pagina master può essere convertito in un oggetto di tipo `BaseMasterPage` (dal momento che si estende).

È una piccola modifica che deve essere impostato `~/Admin/AddProduct.aspx`. Il controllo DetailsView `ItemInserted` gestore eventi utilizza entrambi fortemente tipizzata `Master` proprietà e il fortemente tipizzato `Page.Master` proprietà. Il problema è risolto il riferimento fortemente tipizzato quando è stato aggiornato il `@MasterType` direttiva, ma è comunque necessario aggiornare il riferimento fortemente tipizzato. Sostituire la riga di codice seguente:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample13.vb)]

Con il comando seguente, che esegue il cast `Page.Master` al tipo di base:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample14.vb)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>Passaggio 4: Determinare quale pagina Master per associare le pagine di contenuto

Il nostro `BasePage` classe attualmente imposta tutte le pagine di contenuto `MasterPageFile` proprietà su un valore hardcoded nella fase PreInit del ciclo di vita di pagina. È possibile aggiornare il codice di base della pagina master in alcuni fattori esterni. Ad esempio la pagina master per caricare dipende le preferenze dell'utente attualmente connesso. In tal caso, è necessario scrivere codice nel `OnPreInit` metodo `BasePage` che ricerca le preferenze della pagina principale dell'utente attualmente visita.

Creare una pagina web che consente all'utente di scegliere la pagina master da utilizzare - `Site.master` o `Alternate.master` - e salvare questa scelta in una variabile di sessione. Iniziare creando una nuova pagina web nella directory radice denominata `ChooseMasterPage.aspx`. Durante la creazione di questa pagina (o qualsiasi altro contenuto pagine ormai) non è necessario associarlo a una pagina master, perché la pagina master è impostata a livello di codice `BasePage`. Tuttavia, se non è possibile associare la nuova pagina a una pagina master quindi markup dichiarativo di valore predefinito della nuova pagina contiene un Web Form e altro contenuto fornito dalla pagina master. È necessario sostituire manualmente questo markup con i controlli contenuto appropriati. Per questo motivo, facile associare la nuova pagina ASP.NET a una pagina master.

> [!NOTE]
> Poiché `Site.master` e `Alternate.master` hanno lo stesso insieme di controlli ContentPlaceHolder non è importante indicare la pagina master desiderato quando si crea una nuova pagina contenuto. Per coerenza, consigliamo di utilizzare `Site.master`.


[![Aggiungere una nuova pagina contenuta per il sito Web](specifying-the-master-page-programmatically-vb/_static/image14.png)](specifying-the-master-page-programmatically-vb/_static/image13.png)

**Figura 05**: aggiungere una nuova pagina contenuto per il sito Web ([fare clic per visualizzare l'immagine ingrandita](specifying-the-master-page-programmatically-vb/_static/image15.png))


Aggiornamento di `Web.sitemap` file da includere una voce per questa lezione. Aggiungere il markup seguente sotto il `<siteMapNode>` della lezione pagine Master e ASP.NET AJAX:


[!code-xml[Main](specifying-the-master-page-programmatically-vb/samples/sample15.xml)]

Prima di aggiungere il contenuto per il `ChooseMasterPage.aspx` pagina richiedere qualche istante per l'aggiornamento di classe code-behind della pagina in modo che derivi da `BasePage` (anziché `System.Web.UI.Page`). Successivamente, aggiungere un controllo DropDownList alla pagina, impostare il relativo `ID` proprietà `MasterPageChoice`, e aggiungere due elementi ListItem con il `Text` valori di "~ / Site. master" e "~ / Alternate.master".

Aggiungere un controllo pulsante Web alla pagina e impostare il relativo `ID` e `Text` proprietà `SaveLayout` e "Salvare Layout scelta", rispettivamente. Markup dichiarativo della pagina a questo punto dovrebbe essere simile al seguente:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample16.aspx)]

Quando viene prima visitata la pagina è necessario visualizzare la scelta dell'utente attualmente selezionato pagina master. Creare un `Page_Load` gestore dell'evento e aggiungere il codice seguente:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample17.vb)]

Il codice precedente viene eseguita solo nella prima visita pagina (e non nei postback successivi). Verifica innanzitutto se la variabile di sessione `MyMasterPage` esiste. In caso affermativo, tenta di individuare l'elemento ListItem corrispondenti nel `MasterPageChoice` DropDownList. Se viene trovato un corrispondenza ListItem, il relativo `Selected` è impostata su `True`.

È anche necessario codice che consente di salvare la scelta dell'utente nel `MyMasterPage` variabile di sessione. Creare un gestore eventi per il `SaveLayout` del pulsante `Click` eventi e aggiungere il codice seguente:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample18.vb)]

> [!NOTE]
> Una volta il `Click` gestore eventi viene eseguito durante il postback, la pagina master è già stata selezionata. Pertanto, la selezione dell'utente elenco a discesa sarà attivo fino a quando non visitare la pagina successiva. Il `Response.Redirect` forza il browser per richiedere nuovamente `ChooseMasterPage.aspx`.


Con il `ChooseMasterPage.aspx` pagina completa, l'attività finale è che `BasePage` assegnare il `MasterPageFile` proprietà in base al valore del `MyMasterPage` variabile di sessione. Se non è impostata la variabile di sessione `BasePage` per impostazione predefinita `Site.master`.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample19.vb)]

> [!NOTE]
> Quando si sposta il codice che assegna il `Page` dell'oggetto `MasterPageFile` proprietà fuori il `OnPreInit` gestore dell'evento e in due metodi distinti. Il primo metodo, `SetMasterPageFile`, assegna il `MasterPageFile` proprietà sul valore restituito dal metodo secondo `GetMasterPageFileFromSession`. È stata contrassegnata la `SetMasterPageFile` metodo `Overridable` in modo che le future classi che estendono `BasePage` possibile eseguire l'override per implementare la logica personalizzata, se necessario. Possiamo vedere un esempio di override `BasePage`del `SetMasterPageFile` proprietà nella prossima esercitazione.


Con questo codice, visitare il `ChooseMasterPage.aspx` pagina. Inizialmente, il `Site.master` pagina master è selezionato (vedere Figura 6), ma l'utente può scegliere una pagina master diversa dall'elenco a discesa.


[![Pagine di contenuto vengono visualizzate utilizzando la pagina Master Site. master](specifying-the-master-page-programmatically-vb/_static/image17.png)](specifying-the-master-page-programmatically-vb/_static/image16.png)

**Figura 06**: le pagine di contenuto vengono visualizzati utilizzando il `Site.master` pagina Master ([fare clic per visualizzare l'immagine ingrandita](specifying-the-master-page-programmatically-vb/_static/image18.png))


[![Pagine di contenuto vengono visualizzate tramite la pagina Master Alternate.master](specifying-the-master-page-programmatically-vb/_static/image20.png)](specifying-the-master-page-programmatically-vb/_static/image19.png)

**Figura 07**: le pagine di contenuto vengono ora visualizzati utilizzando il `Alternate.master` pagina Master ([fare clic per visualizzare l'immagine ingrandita](specifying-the-master-page-programmatically-vb/_static/image21.png))


## <a name="summary"></a>Riepilogo

Quando viene visitata una pagina di contenuto, i controlli contenuti sono fusibile con controlli ContentPlaceHolder della pagina master. Il contenuto della pagina master è indicata il `Page` della classe `MasterPageFile` proprietà, che viene assegnato al `@Page` della direttiva `MasterPageFile` attributo durante la fase di inizializzazione. Come in questa esercitazione è stata illustrata, è possibile assegnare un valore per il `MasterPageFile` proprietà purché avviene prima della fine della fase PreInit. La possibilità di specificare a livello di codice della pagina master apre le porte per scenari più avanzati, ad esempio l'associazione dinamica di una pagina di contenuto a una pagina master in base a fattori esterni.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Diagramma del ciclo di vita di pagina ASP.NET](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [Panoramica del ciclo di vita di pagina ASP.NET](https://msdn.microsoft.com/en-us/library/ms178472.aspx)
- [Panoramica di interfacce e temi ASP.NET](https://msdn.microsoft.com/en-us/library/ykzx33wh.aspx)
- [Pagine master: Suggerimenti, consigli e trap](http://www.odetocode.com/articles/450.aspx)
- [Temi in ASP.NET](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 3.5 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott può essere raggiunto al [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o tramite il suo blog all'indirizzo [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata Suchi Banerjee. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](master-pages-and-asp-net-ajax-vb.md)
[Successivo](nested-master-pages-vb.md)
