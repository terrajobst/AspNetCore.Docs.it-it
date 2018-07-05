---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
title: Specificazione della pagina Master a livello di codice (VB) | Microsoft Docs
author: rick-anderson
description: Esamina l'impostazione di pagina master alla pagina di contenuto a livello di programmazione tramite il gestore eventi PreInit.
ms.author: aspnetcontent
ms.date: 07/28/2008
ms.assetid: 0edcd653-f24a-41aa-aef4-75f868fe5ac2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
msc.type: authoredcontent
ms.openlocfilehash: 0c84462487426fc50888708baeedd2fb50983ce3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814841"
---
<a name="specifying-the-master-page-programmatically-vb"></a>Specificazione della pagina Master a livello di codice (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.pdf)

> Esamina l'impostazione di pagina master alla pagina di contenuto a livello di programmazione tramite il gestore eventi PreInit.


## <a name="introduction"></a>Introduzione

Dall'esempio inaugurale [ *creazione di un Layout a livello di sito tramite le pagine Master*](creating-a-site-wide-layout-using-master-pages-vb.md), tutto il contenuto di pagine sono fatto riferimento a loro pagina master in modo dichiarativo tramite la `MasterPageFile` attributo nel `@Page`della direttiva. Ad esempio, il seguente `@Page` direttiva collega la pagina di contenuto della pagina master `Site.master`:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample1.aspx)]

Il [ `Page` classe](https://msdn.microsoft.com/library/system.web.ui.page.aspx) nel `System.Web.UI` dello spazio dei nomi include una [ `MasterPageFile` proprietà](https://msdn.microsoft.com/library/system.web.ui.page.masterpagefile.aspx) che restituisce il percorso per il contenuto della pagina master, ma questa proprietà impostata dal `@Page` direttiva. Questa proprietà può essere utilizzata anche per specificare a livello di codice il contenuto della pagina master. Questo approccio è utile se si desidera assegnare in modo dinamico la pagina master dipende da fattori esterni, ad esempio l'utente visitando la pagina.

In questa esercitazione è aggiungere una seconda pagina master per il sito Web Microsoft e decidere in modo dinamico la pagina master da usare in fase di esecuzione.

## <a name="step-1-a-look-at-the-page-lifecycle"></a>Passaggio 1: Esaminare il ciclo di vita di pagina

Ogni volta che arriva una richiesta nel server web per una pagina ASP.NET che è una pagina di contenuto, il motore ASP.NET deve fuse della pagina contenuto controlli ContentPlaceHolder corrispondente del controlli nella pagina master. Questa fusione consente di creare una gerarchia di solo controllo che può quindi procedere attraverso il ciclo di vita di pagina tipiche.

Figura 1 illustra questa fusion. Passaggio 1 nella figura 1 mostra il contenuto iniziale e le gerarchie di controllo pagina master. Nella parte finale della fase PreInit il contenuto nella pagina vengono aggiunti di elementi ContentPlaceHolder corrispondente nella pagina master (passaggio 2). Dopo questo fusion, la pagina master viene utilizzato come la radice della gerarchia del controllo congiunta. Questa istruzione fused controllo gerarchia viene quindi aggiunto alla pagina per produrre la gerarchia dei controlli finalizzato (passaggio 3). Il risultato finale è che la gerarchia dei controlli della pagina include la gerarchia dei controlli congiunta.


[![Della pagina Master e le gerarchie di controllo della pagina contenuto vengono combinati insieme durante la fase di PreInit](specifying-the-master-page-programmatically-vb/_static/image2.png)](specifying-the-master-page-programmatically-vb/_static/image1.png)

**Figura 01**: la pagina Master e del contenuto della pagina gerarchie di controllo sono Fused insieme durante la fase di PreInit ([fare clic per visualizzare l'immagine con dimensioni normali](specifying-the-master-page-programmatically-vb/_static/image3.png))


## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>Passaggio 2: Configurazione di`MasterPageFile`proprietà dal codice

Quale pagina master partakes in questa fusione dipende dal valore della `Page` dell'oggetto `MasterPageFile` proprietà. Impostando il `MasterPageFile` attributo la `@Page` direttiva ha l'effetto finale di assegnazione il `Page`del `MasterPageFile` proprietà durante la fase di inizializzazione, che è la prima fase del ciclo di vita della pagina. In alternativa è possibile impostare questa proprietà a livello di codice. Tuttavia, è fondamentale che questa proprietà deve essere impostata prima che venga eseguita la fusione nella figura 1.

All'inizio della fase PreInit il `Page` oggetto genera relativi [ `PreInit` evento](https://msdn.microsoft.com/library/system.web.ui.page.preinit.aspx) e chiama relativo [ `OnPreInit` metodo](https://msdn.microsoft.com/library/system.web.ui.page.onpreinit.aspx). Per impostare la pagina master a livello di codice, quindi, è possibile creare un gestore eventi per il `PreInit` evento o eseguire l'override di `OnPreInit` (metodo). Diamo un'occhiata entrambi gli approcci.

Iniziare aprendo `Default.aspx.vb`, il file di classe code-behind per la home page del sito. Aggiungere un gestore eventi per la pagina `PreInit` eventi digitando il codice seguente:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample2.vb)]

Da qui è possibile impostare il `MasterPageFile` proprietà. Aggiornare il codice in modo che assegna il valore "~ / Site. master" per il `MasterPageFile` proprietà.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample3.vb)]

Se si imposta un punto di interruzione e avvio con il debug si noterà che ogni volta che il `Default.aspx` viene visitata oppure ogni volta che è un postback a questa pagina, il `Page_PreInit` gestore dell'evento e il `MasterPageFile` proprietà viene assegnata a "~ / Site. master".

In alternativa, è possibile eseguire l'override di `Page` della classe `OnPreInit` metodo e impostare il `MasterPageFile` proprietà non esiste. Per questo esempio, è possibile non impostare la pagina master in una pagina specifica, ma piuttosto da `BasePage`. È importante ricordare che abbiamo creato una classe di base di pagine personalizzate (`BasePage`) nella [ *specificazione di titolo, metatag e altre intestazioni HTML nella pagina Master* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) esercitazione. Attualmente `BasePage` esegue l'override di `Page` della classe `OnLoadComplete` metodo, in cui imposta la pagina `Title` proprietà basate sui dati della mappa del sito. Aggiorniamo `BasePage` per eseguire l'override anche di `OnPreInit` metodo per specificare a livello di codice della pagina master.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample4.vb)]

Poiché tutte le pagine di contenuto derivano da `BasePage`, tutti gli elementi hanno ora la pagina master a livello di codice assegnata. A questo punto il `PreInit` gestore dell'evento in `Default.aspx.vb` è superfluo, è possibile rimuoverlo.

### <a name="what-about-thepagedirective"></a>Per quanto riguarda il`@Page`direttiva?

Ciò che può essere un po' di confusione è che le pagine del contenuto `MasterPageFile` le proprietà vengono ora specificate in due posizioni: a livello di codice nel `BasePage` della classe `OnPreInit` metodo nonché mediante il `MasterPageFile` attributo in ogni pagina di contenuto `@Page` direttiva.

La prima fase del ciclo di vita di pagina è la fase di inizializzazione. Durante questa fase il `Page` dell'oggetto `MasterPageFile` proprietà viene assegnato il valore del `MasterPageFile` attributo di `@Page` (direttiva) (se fornito). La fase di PreInit segue la fase di inizializzazione, ed è qui in cui è impostato a livello di codice il `Page` dell'oggetto `MasterPageFile` proprietà, in tal modo sovrascrivendo quello assegnato dal `@Page` direttiva. Perché si sta impostando il `Page` dell'oggetto `MasterPageFile` proprietà a livello di codice è stato possibile rimuovere il `MasterPageFile` dell'attributo dal `@Page` direttiva senza influire sull'esperienza dell'utente finale. Per indurre manualmente di questo, proseguire e rimuovere i `MasterPageFile` dell'attributo dal `@Page` direttiva `Default.aspx` e quindi accedere alla pagina tramite un browser. Come si può immaginare, l'output è identico a prima che l'attributo è stato rimosso.

Se il `MasterPageFile` viene impostata tramite il `@Page` direttiva o a livello di codice non è rilevante per l'esperienza dell'utente finale. Tuttavia, il `MasterPageFile` attributo il `@Page` direttiva viene utilizzata da Visual Studio durante la fase di progettazione per produrre il WYSIWYG visualizzazione nella finestra di progettazione. Se si torna alla `Default.aspx` in Visual Studio e passare alla finestra di progettazione verrà visualizzato il messaggio, "errore di pagina Master: la pagina contiene i controlli che richiedono un riferimento alla pagina Master, ma non è specificato" (vedere la figura 2).

In breve, è necessario lasciare il `MasterPageFile` attributo la `@Page` direttiva per usufruire di esperienze in fase di progettazione in Visual Studio.


[![Visual Studio Usa il @Page attributo MasterPageFile della direttiva per il rendering della visualizzazione di progettazione](specifying-the-master-page-programmatically-vb/_static/image5.png)](specifying-the-master-page-programmatically-vb/_static/image4.png)

**Figura 02**: Visual Studio Usa il `@Page` della direttiva `MasterPageFile` per il rendering dell'attributo di visualizzazione di progettazione ([fare clic per visualizzare l'immagine con dimensioni normali](specifying-the-master-page-programmatically-vb/_static/image6.png))


## <a name="step-3-creating-an-alternative-master-page"></a>Passaggio 3: Creazione di una pagina Master alternativa

Poiché un contenuto della pagina master può essere impostata a livello di codice in fase di esecuzione è possibile caricare dinamicamente una particolare pagina master in base ad alcuni criteri esterni. Questa funzionalità può essere utile nelle situazioni in cui il layout del sito deve variare in base all'utente. Ad esempio, un'applicazione web del motore di blog può consentire agli utenti di scegliere un layout per i blog, dove ogni layout è associata a un'altra pagina master. In fase di esecuzione, quando un visitatore accede alla post di blog di un utente, l'applicazione web dovrà determinare il layout del blog e associare in modo dinamico la pagina master corrispondente con la pagina contenuta.

Verrà ora esaminato come caricare in modo dinamico una pagina master in fase di esecuzione in base ad alcuni criteri esterni. Il sito Web Microsoft attualmente contiene solo una pagina master (`Site.master`). Abbiamo bisogno di un'altra pagina master per illustrare la scelta di una pagina master in fase di esecuzione. Questo passaggio si concentra sulla creazione e la configurazione della nuova pagina master. Passaggio 4 è simile a determinare quale pagina master da usare in fase di esecuzione.

Creare una nuova pagina master nella cartella radice denominata `Alternate.master`. Aggiungere anche un nuovo foglio di stile per il sito Web denominato `AlternateStyles.css`.


[![Aggiungere un altro File pagina Master e CSS al sito Web](specifying-the-master-page-programmatically-vb/_static/image8.png)](specifying-the-master-page-programmatically-vb/_static/image7.png)

**Figura 03**: aggiungere un'altra pagina Master e File CSS per il sito Web ([fare clic per visualizzare l'immagine con dimensioni normali](specifying-the-master-page-programmatically-vb/_static/image9.png))


Ho ho progettato il `Alternate.master` pagina master per il titolo visualizzato nella parte superiore della pagina, centrata e su uno sfondo blu. Ho erogate della colonna a sinistra e spostare tale contenuto di sotto di `MainContent` controllo ContentPlaceHolder, che si estende per l'intera larghezza della pagina. Inoltre, ho nixed l'elenco non ordinato di lezioni e sostituito con un elenco orizzontale precedente `MainContent`. Ho aggiornato anche i tipi di carattere e colori utilizzati dalla pagina master (e, di conseguenza, le pagine di contenuto). La figura 4 illustra `Default.aspx` quando si usa il `Alternate.master` pagina master.

> [!NOTE]
> ASP.NET include la possibilità di definire *temi*. Un tema è una raccolta di immagini, file CSS e correlate allo stile proprietà impostazioni di controlli Web che possono essere applicate a una pagina in fase di esecuzione. I temi sono il modo per vedere se i layout del sito differiscono solo nelle immagini visualizzate e le regole CSS. Se il layout si distinguono più significativo, ad esempio usando i controlli Web diversi o impostare un layout radicalmente diverso, è necessario usare pagine master separate. Alla fine di questa esercitazione per altre informazioni sui temi, consultare la sezione altre informazioni.


[![Le pagine di contenuto possono ora usare un nuovo aspetto grafico](specifying-the-master-page-programmatically-vb/_static/image11.png)](specifying-the-master-page-programmatically-vb/_static/image10.png)

**Figura 04**: le pagine di contenuto possono ora usare un nuovo aspetto ([fare clic per visualizzare l'immagine con dimensioni normali](specifying-the-master-page-programmatically-vb/_static/image12.png))


Quando il server master e markup di pagine contenuto sono fusibile, il `MasterPage` classe controlli per verificare che il contenuto ogni controllo nella pagina di contenuto fa riferimento a un controllo ContentPlaceHolder nella pagina master. Se viene trovato un elemento ContentControl che fa riferimento a un controllo ContentPlaceHolder inesistente, viene generata un'eccezione. In altre parole, è fondamentale che la pagina master da assegnare alla pagina di contenuto include un controllo ContentPlaceHolder per ogni controllo nella pagina di contenuto del contenuto.

Il `Site.master` pagina master include quattro controlli ContentPlaceHolder:

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

Alcune delle pagine di contenuto nel nostro sito Web includono solo una o due controlli contenuto; altri utenti includono un controllo contenuto per ogni di elementi disponibile ContentPlaceHolder. Se la nuova pagina master (`Alternate.master`) può essere assegnato per le pagine del contenuto con i controlli del contenuto per tutti gli elementi ContentPlaceHolder nella `Site.master` , quindi è essenziale che `Alternate.master` anche includere gli stessi controlli ContentPlaceHolder come `Site.master`.

Per ottenere il `Alternate.master` pagina master per renderli simili per eseguire il data mining (vedere la figura 4), iniziare definendo gli stili della pagina master di `AlternateStyles.css` foglio di stile. Aggiungere le regole seguenti in `AlternateStyles.css`:


[!code-css[Main](specifying-the-master-page-programmatically-vb/samples/sample5.css)]

Successivamente, aggiungere il seguente markup dichiarativo per `Alternate.master`. Come si vede `Alternate.master` contiene quattro controlli ContentPlaceHolder con lo stesso `ID` i valori dei controlli ContentPlaceHolder `Site.master`. Inoltre, include un controllo ScriptManager, che è necessario per le pagine nel nostro sito Web che usano il framework ASP.NET AJAX.


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>Test della nuova pagina Master

Per testare questo nuovo aggiornamento pagina master le `BasePage` della classe `OnPreInit` metodo in modo che il `MasterPageFile` proprietà viene assegnato il valore `"~/Alternate.maser"` e quindi visitare il sito Web. Ogni pagina dovrebbe funzionare senza errori tranne due: `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx`. Aggiunta di un prodotto in DetailsView nella `~/Admin/AddProduct.aspx` genera una `NullReferenceException` dalla riga di codice che tenta di impostare la pagina master `GridMessageText` proprietà. Quando si visita `~/Admin/Products.aspx` un' `InvalidCastException` generata al caricamento della pagina con il messaggio: "Impossibile eseguire il cast dell'oggetto di tipo ' ASP.alternate\_master' nel tipo ' ASP.site\_master'."

Questi errori si verificano perché le `Site.master` classe code-behind include gli eventi pubblici, le proprietà e metodi che non sono definiti nel `Alternate.master`. La parte di markup di queste due pagine hanno una `@MasterType` direttiva che fa riferimento il `Site.master` pagina master.


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample7.aspx)]

Inoltre, DetailsView `ItemInserted` gestore dell'evento in `~/Admin/AddProduct.aspx` include codice che esegue il cast di non fortemente tipizzate `Page.Master` proprietà a un oggetto di tipo `Site`. Il `@MasterType` direttiva (usato in questo modo) e il cast nel `ItemInserted` gestore di è strettamente collegato il `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` pagine per il `Site.master` pagina master.

Per interrompere questo accoppiamento stretto possiamo `Site.master` e `Alternate.master` derivano da una classe di base comune che contiene le definizioni per i membri pubblici. In seguito, è possibile aggiornare il `@MasterType` direttiva per fare riferimento a questo tipo di base comune.

### <a name="creating-a-custom-base-master-page-class"></a>Creazione di una classe di pagina Master di Base personalizzata

Aggiungere un nuovo file di classe per il `App_Code` cartella denominata `BaseMasterPage.vb` e fare in modo che derivano da `System.Web.UI.MasterPage`. È necessario definire il `RefreshRecentProductsGrid` metodo e il `GridMessageText` proprietà in `BaseMasterPage`, ma non è semplicemente possibile spostare essi da `Site.master` poiché questi membri funzionano con i controlli Web specifiche per il `Site.master` pagina master (il `RecentProducts` GridView e `GridMessage` etichetta).

Che cosa dobbiamo fare è configurare `BaseMasterPage` in modo che questi membri vengono definiti non esiste, ma vengono effettivamente implementati da `BaseMasterPage`di classi derivate (`Site.master` e `Alternate.master`). Questo tipo di ereditarietà è possibile contrassegnare la classe come `MustInherit` e i relativi membri come `MustOverride`. In breve, l'aggiunta di queste parole chiave per la classe e i relativi due membri annuncia che `BaseMasterPage` non è implementato `RefreshRecentProductsGrid` e `GridMessageText`, ma che verranno relative classi derivate.

È inoltre necessario definire il `PricesDoubled` eventi in `BaseMasterPage` e forniscono un mezzo per le classi derivate per generare l'evento. Il modello usato in .NET Framework per semplificare questo comportamento consiste nel creare un evento pubblico nella classe di base e aggiungere un metodo protetto, sottoponibile a override denominato `OnEventName`. Le classi derivate possono quindi chiamare questo metodo per generare l'evento o possano eseguirne l'override per eseguire codice immediatamente prima o dopo l'evento viene generato.

Aggiornamento di `BaseMasterPage` classe in modo che contenga il codice seguente:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample8.vb)]

Passare quindi ad il `Site.master` code-behind classe e fare in modo che derivano da `BaseMasterPage`. In quanto `BaseMasterPage` contiene i membri contrassegnati `MustOverride` è necessario eseguire l'override di tali membri in `Site.master`. Aggiungere il `Overrides` una parola chiave per le definizioni di metodo e proprietà. Aggiornare anche il codice che genera la `PricesDoubled` eventi nel `DoublePrice` del pulsante `Click` gestore dell'evento con una chiamata per la classe di base `OnPricesDoubled` (metodo).

Dopo queste modifiche di `Site.master` classe code-behind deve contenere il codice seguente:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample9.vb)]

È anche necessario aggiornare `Alternate.master`della classe code-behind da cui derivare `BaseMasterPage` ed eseguire l'override di due `MustOverride` membri. Tuttavia, poiché `Alternate.master` non contiene un controllo GridView che elenca i prodotti più recenti né un'etichetta che visualizza un messaggio dopo un nuovo prodotto viene aggiunto al database, questi metodi non sono necessario eseguire alcuna operazione.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample10.vb)]

### <a name="referencing-the-base-master-page-class"></a>La classe delle pagine Master di Base di riferimento

Ora che è stata completata la `BaseMasterPage` classi e le due pagine master estenderlo, il passaggio finale consiste nell'aggiornare il `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` pagine per fare riferimento a questo tipo comune. Iniziare cambiando il `@MasterType` direttiva in entrambe le pagine da:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample11.aspx)]

A:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample12.aspx)]

Invece che fanno riferimento a un percorso di file, il `@MasterType` proprietà fa riferimento a questo punto il tipo di base (`BaseMasterPage`). Di conseguenza, fortemente tipizzato `Master` proprietà sia alla pagina code-behind per le classi ora è di tipo `BaseMasterPage` (anziché di tipo `Site`). Con questa modifica posto rivisitata `~/Admin/Products.aspx`. In precedenza, questo ha comportato un errore di cast perché la pagina è configurata per usare la `Alternate.master` pagina master, ma la `@MasterType` direttiva fa riferimento il `Site.master` file. Ma ora il rendering della pagina senza errori. Infatti, il `Alternate.master` pagina master può essere convertita in un oggetto di tipo `BaseMasterPage` (dal momento che si estende).

È una piccola modifica che deve essere inviata `~/Admin/AddProduct.aspx`. Il controllo DetailsView `ItemInserted` gestore dell'evento Usa entrambi fortemente tipizzato `Master` proprietà e il non fortemente tipizzate `Page.Master` proprietà. È stato risolto il riferimento fortemente tipizzato quando sono stati aggiornati i `@MasterType` direttiva, ma è comunque necessario aggiornare il riferimento non fortemente tipizzato. Sostituire la riga di codice seguente:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample13.vb)]

Con il comando seguente, che viene eseguito il cast `Page.Master` al tipo di base:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample14.vb)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>Passaggio 4: Determinare quale pagina Master a cui associare le pagine di contenuto

Nostri `BasePage` classe imposta attualmente tutte le pagine di contenuto `MasterPageFile` proprietà su un valore hardcoded nella fase del ciclo di vita pagina PreInit. È possibile aggiornare il codice di base della pagina master in fattori esterni. Forse la pagina master per caricare dipende dalle preferenze dell'utente attualmente connesso. In tal caso, è necessario scrivere codice nel `OnPreInit` metodo `BasePage` che ricerca le preferenze della pagina master dell'utente attualmente visita.

È possibile creare una pagina web che consente all'utente di scegliere quale pagina master da utilizzare - `Site.master` o `Alternate.master` - e salvare questa scelta in una variabile di sessione. Iniziare creando una nuova pagina web nella directory radice denominata `ChooseMasterPage.aspx`. Durante la creazione di questa pagina (o qualsiasi altre pagine di contenuto da questo momento) non è necessario associarlo a una pagina master perché la pagina master è impostata a livello di codice `BasePage`. Tuttavia, se non è possibile associare la nuova pagina a una pagina master quindi markup dichiarativo predefinito della nuova pagina contiene un modulo Web e altri contenuti forniti dalla pagina master. È necessario sostituire manualmente questo markup con i controlli di contenuto appropriati. Per questo motivo, trovo più semplice associare la nuova pagina ASP.NET a una pagina master.

> [!NOTE]
> In quanto `Site.master` e `Alternate.master` hanno lo stesso set di controlli ContentPlaceHolder non importa quale pagina master scelto durante la creazione della nuova pagina contenuta. Per coerenza, è preferibile usando `Site.master`.


[![Aggiungere una nuova pagina contenuta per il sito Web](specifying-the-master-page-programmatically-vb/_static/image14.png)](specifying-the-master-page-programmatically-vb/_static/image13.png)

**Figura 05**: aggiungere una nuova pagina contenuto per il sito Web ([fare clic per visualizzare l'immagine con dimensioni normali](specifying-the-master-page-programmatically-vb/_static/image15.png))


Aggiornamento di `Web.sitemap` file da includere una voce per questa lezione. Aggiungere il markup seguente sotto il `<siteMapNode>` per la lezione pagine Master e ASP.NET AJAX:


[!code-xml[Main](specifying-the-master-page-programmatically-vb/samples/sample15.xml)]

Prima di aggiungere qualsiasi contenuto per il `ChooseMasterPage.aspx` pagina si consiglia di aggiornare classe code-behind della pagina in modo che deriva da `BasePage` (anziché `System.Web.UI.Page`). Successivamente, aggiungere un controllo DropDownList alla pagina, impostare relativi `ID` proprietà `MasterPageChoice`, e aggiungere due ListItems con il `Text` i valori di "~ / Site. master" e "~ / Alternate.master".

Aggiungere un controllo pulsante Web alla pagina e impostare relativi `ID` e `Text` delle proprietà per `SaveLayout` e "Salva Layout scelta", rispettivamente. A questo punto markup dichiarativo della pagina dovrebbe essere simile al seguente:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample16.aspx)]

Quando la pagina viene visitata prima di tutto è necessario visualizzare la scelta dell'utente attualmente selezionato pagina master. Creare un `Page_Load` gestore dell'evento e aggiungere il codice seguente:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample17.vb)]

Il codice riportato sopra esegue solo nella prima visita pagina (e non nei postback successivi). Viene innanzitutto verificato se la variabile di sessione `MyMasterPage` esiste. In caso affermativo, tenta di trovare l'elemento ListItem corrispondente nel `MasterPageChoice` DropDownList. Se viene trovato un elemento ListItem corrispondenti, relative `Selected` è impostata su `True`.

È necessario anche il codice che consente di salvare la scelta dell'utente nel `MyMasterPage` variabile di sessione. Creare un gestore eventi per il `SaveLayout` del pulsante `Click` eventi e aggiungere il codice seguente:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample18.vb)]

> [!NOTE]
> Entro l'ora di `Click` gestore eventi viene eseguita durante il postback, la pagina master è già stata selezionata. Selezione elenco a discesa dell'utente, pertanto non avranno effetto fino a quando non visitare la pagina successiva. Il `Response.Redirect` forza il browser per richiedere nuovamente `ChooseMasterPage.aspx`.


Con il `ChooseMasterPage.aspx` pagina completa, l'attività finale consiste per avere `BasePage` assegnare il `MasterPageFile` proprietà in base al valore della `MyMasterPage` variabile di sessione. Se non è impostata la variabile di sessione hanno `BasePage` per impostazione predefinita `Site.master`.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample19.vb)]

> [!NOTE]
> Quando si sposta il codice che assegna il `Page` dell'oggetto `MasterPageFile` proprietà fuori il `OnPreInit` gestore dell'evento e in due metodi distinti. Il primo metodo, `SetMasterPageFile`, assegna la `MasterPageFile` sul valore restituito dal metodo secondo, `GetMasterPageFileFromSession`. È stata contrassegnata la `SetMasterPageFile` metodo `Overridable` in modo che i futuri nelle classi che estendono `BasePage` può facoltativamente eseguire l'override per implementare logica personalizzata, se necessario. Vedremo un esempio di override `BasePage`del `SetMasterPageFile` proprietà nella prossima esercitazione.


Con questo codice, visitare il `ChooseMasterPage.aspx` pagina. Inizialmente, il `Site.master` pagina master è selezionato (vedere la figura 6), ma l'utente può selezionare un'altra pagina master nell'elenco a discesa.


[![Le pagine di contenuto vengono visualizzate utilizzando la pagina Master Site. master](specifying-the-master-page-programmatically-vb/_static/image17.png)](specifying-the-master-page-programmatically-vb/_static/image16.png)

**Figura 06**: le pagine di contenuto vengono visualizzati usando la `Site.master` pagina Master ([fare clic per visualizzare l'immagine con dimensioni normali](specifying-the-master-page-programmatically-vb/_static/image18.png))


[![Le pagine di contenuto vengono ora visualizzate utilizzando la pagina Master Alternate.master](specifying-the-master-page-programmatically-vb/_static/image20.png)](specifying-the-master-page-programmatically-vb/_static/image19.png)

**Figura 07**: le pagine di contenuto vengono ora visualizzati utilizzando il `Alternate.master` pagina Master ([fare clic per visualizzare l'immagine con dimensioni normali](specifying-the-master-page-programmatically-vb/_static/image21.png))


## <a name="summary"></a>Riepilogo

Quando viene visitata una pagina di contenuto, i controlli contenuti vengono aggregati con controlli ContentPlaceHolder relativa della pagina master. Pagina master alla pagina di contenuto è identificata dal `Page` della classe `MasterPageFile` proprietà, che sono state assegnate le `@Page` della direttiva `MasterPageFile` attributo durante la fase di inizializzazione. Come in questa esercitazione è stata illustrata, è possibile assegnare un valore per il `MasterPageFile` proprietà fino a quando eseguire questa operazione prima della fine della fase PreInit. La possibilità di specificare a livello di codice della pagina master apre le porte per gli scenari più avanzati, ad esempio l'associazione dinamica di una pagina di contenuto a una pagina master dipende da fattori esterni.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Diagramma del ciclo di vita di pagina ASP.NET](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [Panoramica del ciclo di vita di pagina ASP.NET](https://msdn.microsoft.com/library/ms178472.aspx)
- [Cenni preliminari su interfacce e temi ASP.NET](https://msdn.microsoft.com/library/ykzx33wh.aspx)
- [Pagine master: Suggerimenti, trucchi e trabocchetti](http://www.odetocode.com/articles/450.aspx)
- [Temi in ASP.NET](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri e fondatore di 4GuysFromRolla.com, ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach manualmente ASP.NET 3.5 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). È possibile contattarlo Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata Suchi Banerjee. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami un messaggio nella [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](master-pages-and-asp-net-ajax-vb.md)
> [Successivo](nested-master-pages-vb.md)
