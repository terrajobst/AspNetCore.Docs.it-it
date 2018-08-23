---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
title: Più elementi ContentPlaceHolder e contenuto predefinito (VB) | Microsoft Docs
author: rick-anderson
description: Esamina come aggiungere più segnaposto di contenuto a una pagina master e come specificare il contenuto predefinito i segnaposto del contenuto.
ms.author: riande
ms.date: 05/21/2008
ms.assetid: 866a7177-6884-451e-88f4-c934b1dd1af5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
msc.type: authoredcontent
ms.openlocfilehash: 18e2dfdf5cf3d5802bf94d797795fc0f12bdc95b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835298"
---
<a name="multiple-contentplaceholders-and-default-content-vb"></a>Più elementi ContentPlaceHolder e contenuto predefinito (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_VB.pdf)

> Esamina come aggiungere più segnaposto di contenuto a una pagina master e come specificare il contenuto predefinito i segnaposto del contenuto.


## <a name="introduction"></a>Introduzione

Nell'esercitazione precedente abbiamo esaminato come enable delle pagine master ASP.NET agli sviluppatori di creare un layout coerente a livello di sito. Pagine master definiscono markup che sono comuni a tutte le relative pagine di contenuto e aree personalizzabili in base a una pagina per pagina. Nell'esercitazione precedente abbiamo creato una semplice pagina master (`Site.master`) e due pagine di contenuto (`Default.aspx` e `About.aspx`). Questa pagina master è costituita da due elementi ContentPlaceHolder denominato `head` e `MainContent`, che si trovavano nel `<head>` elemento e Web Form, rispettivamente. Mentre le pagine di contenuto ogni aveva due controlli contenuto, viene specificata solo markup per quella corrispondente `MainContent`.

Come evidenziato da due controlli ContentPlaceHolder in `Site.master`, una pagina master può contenere diversi ContentPlaceHolder. Inoltre, la pagina master può specificare il markup predefinito per i controlli ContentPlaceHolder. Una pagina di contenuto, quindi, può facoltativamente specificare il relativo markup oppure usare il markup predefinito. In questa esercitazione è esaminiamo l'uso di più controlli contenuti nella pagina master e vedere come definire i tag predefiniti nei controlli ContentPlaceHolder.

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>Passaggio 1: Aggiunta di controlli ContentPlaceHolder aggiuntive alla pagina Master

Molte progettazioni di sito Web contengono diverse aree dello schermo personalizzati in base a una pagina per pagina. `Site.master`, la pagina master è stato creato nell'esercitazione precedente contiene un solo ContentPlaceHolder all'interno di Web Form denominato `MainContent`. In particolare, questo controllo ContentPlaceHolder si trova all'interno di `mainContent` `<div>` elemento.

La figura 1 mostra `Default.aspx` quando viene visualizzato tramite un browser. L'area cerchiata in rosso è indicato il markup specifico della pagina corrispondente `MainContent`.


[![L'area cerchiata è visualizzata l'Area attualmente personalizzabili in base a una pagina per pagina](multiple-contentplaceholders-and-default-content-vb/_static/image2.png)](multiple-contentplaceholders-and-default-content-vb/_static/image1.png)

**Figura 01**: l'area con cerchio viene illustrata l'Area attualmente personalizzabili in base a una pagina per pagina ([fare clic per visualizzare l'immagine con dimensioni normali](multiple-contentplaceholders-and-default-content-vb/_static/image3.png))


Si supponga che oltre a quella illustrata nella figura 1, è anche necessario aggiungere elementi specifici della pagina per la colonna a sinistra sotto le lezioni e le notizie sezioni. A tale scopo, aggiungiamo un altro controllo ContentPlaceHolder della pagina master. Per seguire la procedura, aprire il `Site.master` pagina in Visual Web Developer master e quindi trascinare un controllo ContentPlaceHolder dalla casella degli strumenti nella finestra di progettazione dopo la sezione Novità. Impostare il controllo ContentPlaceHolder `ID` a `LeftColumnContent`.


[![Aggiungere un controllo ContentPlaceHolder alla colonna a sinistra della pagina Master](multiple-contentplaceholders-and-default-content-vb/_static/image5.png)](multiple-contentplaceholders-and-default-content-vb/_static/image4.png)

**Figura 02**: aggiungere un controllo ContentPlaceHolder alla colonna di sinistra della pagina Master ([fare clic per visualizzare l'immagine con dimensioni normali](multiple-contentplaceholders-and-default-content-vb/_static/image6.png))


Con l'aggiunta del `LeftColumnContent` ContentPlaceHolder della pagina master, possiamo definire contenuto per l'area in base a una pagina per pagina includendo un contenuto controllo nella pagina la cui `ContentPlaceHolderID` è impostata su `LeftColumnContent`. Esaminiamo questo processo nel passaggio 2.

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>Passaggio 2: Definizione del contenuto per il nuovo controllo ContentPlaceHolder nelle pagine di contenuto

Quando si aggiunge una nuova pagina contenuta per il sito Web, Visual Web Developer crea automaticamente un contenuto controllo nella pagina di ogni controllo ContentPlaceHolder nella pagina master selezionata. Dopo aver aggiunto un il `LeftColumnContent` ContentPlaceHolder alla nostra pagina master nel passaggio 1, nuovo ASP.NET le pagine verranno ora hanno tre controlli del contenuto.

Per illustrare questo concetto, aggiungere una nuova pagina di contenuto nella directory radice denominato `MultipleContentPlaceHolders.aspx` associato ai `Site.master` pagina master. Visual Web Developer viene creata in questa pagina con il seguente markup dichiarativo:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample1.aspx)]

Immettere alcuni contenuti nel controllo contenuto che fa riferimento il `MainContent` elementi ContentPlaceHolder (`Content2`). Successivamente, aggiungere il markup seguente per il `Content3` controllo contenuto (che fa riferimento il `LeftColumnContent` ContentPlaceHolder):

[!code-html[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample2.html)]

Dopo aver aggiunto questo markup, visitare la pagina tramite un browser. Come illustrato nella figura 3, il codice inserito nel `Content3` controllo contenuto viene visualizzato nella colonna sinistra sotto la sezione Novità (con un cerchio rosso). Il codice inserito in `Content2` viene visualizzato nella parte destra della pagina (con cerchiata blu).


[![Nella colonna sinistra ora include il contenuto specifico della pagina di sotto la sezione Novità](multiple-contentplaceholders-and-default-content-vb/_static/image8.png)](multiple-contentplaceholders-and-default-content-vb/_static/image7.png)

**Figura 03**: la sinistra colonna ora include specifico della pagina contenuto di sotto la sezione Novità ([fare clic per visualizzare l'immagine con dimensioni normali](multiple-contentplaceholders-and-default-content-vb/_static/image9.png))


### <a name="defining-content-in-existing-content-pages"></a>Definizione del contenuto nelle pagine di contenuto esistente

La creazione automatica di una nuova pagina contenuta incorpora il controllo ContentPlaceHolder che abbiamo aggiunto nel passaggio 1. Le pagine di contenuto esistente due - ma `About.aspx` e `Default.aspx` -non dispone di un controllo contenuto per il `LeftColumnContent` ContentPlaceHolder. Per specificare il contenuto per questo controllo ContentPlaceHolder in queste due pagine esistenti, è necessario aggiungere un controllo contenuto noi stessi.

A differenza di maggior parte dei controlli Web ASP.NET, casella degli strumenti di Visual Web Developer non include un elemento del controllo contenuto. È possibile digitare manualmente nel markup dichiarativo del controllo del contenuto nella visualizzazione origine, ma un approccio più semplice e rapido consiste nell'usare la visualizzazione di progettazione. Aprire il `About.aspx` pagina e passare alla visualizzazione progettazione. Come illustrato nella figura 4, il `LeftColumnContent` ContentPlaceHolder viene visualizzato nella visualizzazione progettazione, se si passa il mouse su di esso, il titolo visualizzato legge: "LeftColumnContent (Master)". L'inclusione di "Master" nel titolo non indica che è presente alcun controllo contenuto definito nella pagina per questo controllo ContentPlaceHolder. Se è presente un controllo contenuto per il controllo ContentPlaceHolder, come nel caso per `MainContent`, verrà letto il titolo: "*ContentPlaceHolderID* (personalizzato)."

Per aggiungere un controllo contenuto per il `LeftColumnContent` ContentPlaceHolder per `About.aspx`espandere tag intelligente del ContentPlaceHolder e fare clic sul collegamento creare contenuto personalizzato.


[![La visualizzazione di progettazione per About Mostra il controllo LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image11.png)](multiple-contentplaceholders-and-default-content-vb/_static/image10.png)

**Figura 04**: la visualizzazione di progettazione per `About.aspx` Mostra le `LeftColumnContent` ContentPlaceHolder ([fare clic per visualizzare l'immagine con dimensioni normali](multiple-contentplaceholders-and-default-content-vb/_static/image12.png))


Facendo clic sul collegamento creare contenuto personalizzato che genera l'errore necessari controllo pagina e i set di contenuto relativo `ContentPlaceHolderID` proprietà per il controllo ContentPlaceHolder `ID`. Ad esempio, facendo clic sul collegamento creare contenuto personalizzato per `LeftColumnContent` area nella `About.aspx` aggiunge il seguente markup dichiarativo alla pagina:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>L'omissione di controlli contenuto

ASP.NET non è necessario che tutte le pagine di contenuto includono i controlli del contenuto per ogni controllo ContentPlaceHolder definiti nella pagina master. Se un controllo contenuto viene omesso, il motore ASP.NET usa il tag definito all'interno di ContentPlaceHolder nella pagina master. Questo markup viene definito il controllo ContentPlaceHolder *predefinito contenuto* ed è utile negli scenari in cui il contenuto per alcune aree sono comuni tra la maggior parte delle pagine, ma deve essere personalizzato per un numero ridotto di pagine. Passaggio 3 illustra il contenuto predefinito specificando nella pagina master.

Attualmente `Default.aspx` contiene due controlli contenuti per il `head` e `MainContent` elementi ContentPlaceHolder; che non è un controllo contenuto per `LeftColumnContent`. Di conseguenza, quando `Default.aspx` viene eseguito il rendering di `LeftColumnContent` viene usato il contenuto predefinito del controllo ContentPlaceHolder. Poiché è ancora necessario definire qualsiasi contenuto predefinito per questo controllo ContentPlaceHolder, il risultato finale è che non viene generato alcun codice per questa area. Per verificare questo comportamento, visitare `Default.aspx` tramite un browser. Come illustrato nella figura 5, non viene generato alcun markup nella colonna sinistra sotto la sezione Novità.


[![Non viene eseguito il rendering di alcun contenuto per il LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image14.png)](multiple-contentplaceholders-and-default-content-vb/_static/image13.png)

**Figura 05**: nessun contenuto viene sottoposto a rendering per il `LeftColumnContent` ContentPlaceHolder ([fare clic per visualizzare l'immagine con dimensioni normali](multiple-contentplaceholders-and-default-content-vb/_static/image15.png))


## <a name="step-3-specifying-default-content-in-the-master-page"></a>Passaggio 3: Specificare il contenuto predefinito nella pagina Master

Alcuni progetti di sito Web includono un'area il cui contenuto è lo stesso per tutte le pagine del sito fatta eccezione per una o due eccezioni. Prendere in considerazione un sito Web che supporta gli account utente. Tale sito richiede una pagina di accesso in cui i visitatori possono immettere le credenziali per accedere al sito. Per accelerare il processo di accesso, i progettisti di siti Web potrebbero includere nome utente e password nelle caselle di testo nell'angolo superiore sinistro di ogni pagina per consentire agli utenti di accedere senza dover accedere in modo esplicito la pagina di accesso. Anche se queste caselle di testo Nome utente e password sono utili nella maggior parte delle pagine, sono ridondanti nella pagina di accesso, che già contiene caselle di testo per le credenziali dell'utente.

Per implementare questa progettazione, è possibile creare un controllo ContentPlaceHolder nell'angolo superiore sinistro della pagina master. Ogni pagina che alle esigenze per visualizzare il nome utente e password nelle caselle di testo nell'angolo superiore sinistro sarebbe creare un controllo contenuto per questo controllo ContentPlaceHolder e aggiungere l'interfaccia necessaria. La pagina di accesso, d'altra parte, ovvero sarebbe omettere l'aggiunta di un controllo contenuto per questo controllo ContentPlaceHolder o creerebbe un contenuto controllo con alcun markup definito. Lo svantaggio di questo approccio è che è necessario ricordarsi di aggiungere il nome utente e password nelle caselle di testo per ogni pagina che viene aggiunto al sito (ad eccezione di pagina di accesso). Ciò può comportare dei problemi. Potrebbero dimenticare di aggiungere queste caselle di testo a una pagina o a due o, peggio ancora, si potrebbe essere non implementano l'interfaccia correttamente (ad esempio aggiungendo una sola casella di testo anziché due).

Una soluzione migliore consiste nel definire il nome utente e password nelle caselle di testo come contenuto predefinita del controllo ContentPlaceHolder. In questo modo, è sufficiente eseguire l'override di questo contenuto predefinito in tali pagine alcuni che non vengono visualizzati il nome utente e password nelle caselle di testo (l'account di accesso di pagina, ad esempio). Per illustrare che specifica il contenuto predefinito per un controllo ContentPlaceHolder, è possibile implementare lo scenario appena descritto.

> [!NOTE]
> Nella parte restante di questa esercitazione consente di aggiornare il sito Web Microsoft per includere un'interfaccia di accesso nella colonna a sinistra per tutte le pagine, ma la pagina di accesso. Tuttavia, questa esercitazione non esamina come configurare il sito Web per supportare gli account utente. Per altre informazioni su questo argomento, fare riferimento a mio [autenticazione basata su form, autorizzazione, gli account utente e ruoli](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) esercitazioni.


### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>Aggiunta di un controllo ContentPlaceHolder e specificando il relativo contenuto predefinito

Aprire il `Site.master` pagina master e aggiungere il markup seguente alla colonna a sinistra tra le `DateDisplay` sezione etichetta e lezioni:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample4.aspx)]

Dopo aver aggiunto il markup dovrebbe essere simile alla figura 6 visualizzazione di progettazione della pagina master.


[![La pagina Master include un controllo di accesso](multiple-contentplaceholders-and-default-content-vb/_static/image17.png)](multiple-contentplaceholders-and-default-content-vb/_static/image16.png)

**Figura 06**: la pagina Master include un controllo di accesso ([fare clic per visualizzare l'immagine con dimensioni normali](multiple-contentplaceholders-and-default-content-vb/_static/image18.png))


Questo controllo ContentPlaceHolder, `QuickLoginUI`, dispone di un controllo di accesso Web come proprio contenuto predefinito. Il controllo Login consente di visualizzare un'interfaccia utente che richiede l'immissione di nome utente e password con un pulsante Accedi. Facendo clic sul pulsante Accedi, il controllo di accesso convalida internamente le credenziali dell'utente con l'API di appartenenza. Per usare questo controllo di accesso in pratica, quindi, è necessario configurare il sito per utilizzare l'appartenenza. In questo argomento non rientra nell'ambito di questa esercitazione. Fare riferimento alla mia [autenticazione basata su form, autorizzazione, gli account utente e ruoli](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) esercitazioni per altre informazioni sulla creazione di un'applicazione web che supporta gli account utente.

È possibile personalizzare il comportamento o l'aspetto del controllo di accesso. Sono stati impostati due proprietà: `TitleText` e `FailureAction`. Il `TitleText` valore della proprietà, impostazione predefinita è "Accedi", viene visualizzato nella parte superiore dell'interfaccia utente del controllo. Questa proprietà è impostato in modo che venga visualizzato il testo "Sign In" come un `<h3>` elemento. Il `FailureAction` proprietà indica cosa fare se le credenziali dell'utente non sono validi. Il valore predefinito è un valore di `Refresh`, lascia l'utente nella stessa pagina e viene visualizzato un messaggio di errore all'interno del controllo di accesso. È stato modificato per `RedirectToLoginPage`, che invia l'utente alla pagina di accesso in caso di credenziali non valide. È preferibile inviare l'utente alla pagina di accesso quando un utente prova a eseguire l'accesso da un'altra pagina, ma ha esito negativo, poiché la pagina di accesso può contenere istruzioni aggiuntive e opzioni che non sarebbero facilmente sufficienti nella colonna a sinistra. Ad esempio, la pagina di accesso potrebbe includere opzioni per recuperare una password dimenticata o per creare un nuovo account.

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>Creazione della pagina di accesso e si esegue l'override del contenuto predefinito

Con la pagina master completa, il passaggio successivo consiste nel creare la pagina di accesso. Aggiungere una pagina ASP.NET alla directory radice del sito denominate `Login.aspx`, associarla al `Site.master` pagina master. In questo modo verrà creata una pagina con quattro controlli contenuti, uno per ognuno degli elementi di ContentPlaceHolder definiti in `Site.master`.

Aggiungere un controllo di accesso per il `MainContent` controllo contenuto. Analogamente, è possibile aggiungere qualsiasi contenuto per il `LeftColumnContent` area. Tuttavia, assicurarsi di lasciare il controllo contenuto per il `QuickLoginUI` ContentPlaceHolder vuoto. Questo garantisce che l'account di accesso controllo non viene visualizzato nella colonna sinistra della pagina di accesso.

Dopo aver definito il contenuto per il `MainContent` e `LeftColumnContent` aree, markup dichiarativo di pagina di accesso dovrebbe essere simile al seguente:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample5.aspx)]

Figura 7 Mostra questa pagina quando viene visualizzato tramite un browser. Poiché questa pagina consente di specificare un controllo contenuto per il `QuickLoginUI` ContentPlaceHolder, viene eseguito l'override di contenuto predefinito specificato nella pagina master. Il risultato finale è che il controllo di accesso visualizzato nella progettazione della pagina master visualizzazione (vedere la figura 6) non viene eseguita in questa pagina.


[![La pagina di accesso Represses contenuto predefinito di QuickLoginUI ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image20.png)](multiple-contentplaceholders-and-default-content-vb/_static/image19.png)

**Figura 07**: la pagina di accesso Represses il `QuickLoginUI` contenuto del ContentPlaceHolder predefinito ([fare clic per visualizzare l'immagine con dimensioni normali](multiple-contentplaceholders-and-default-content-vb/_static/image21.png))


### <a name="using-the-default-content-in-new-pages"></a>Usando il contenuto predefinito in nuove pagine

Si vuole mostrare il controllo di accesso presenti nella colonna a sinistra per tutte le pagine, ad eccezione di pagina di accesso. A tale scopo, tutte le pagine contenute ad eccezione di pagina di accesso non devono eseguire un controllo contenuto per il `QuickLoginUI` ContentPlaceHolder. Omettendo un controllo contenuto, il contenuto predefinito del controllo ContentPlaceHolder verrà utilizzato.

Le pagine di contenuto esistente - `Default.aspx`, `About.aspx`, e `MultipleContentPlaceHolders.aspx` -non includere un controllo contenuto per `QuickLoginUI` perché sono stati creati prima che subentrasse tale controllo ContentPlaceHolder della pagina master. Pertanto, queste pagine esistenti non sono necessario essere aggiornati. Tuttavia, le nuove pagine aggiunte al sito Web includono un controllo contenuto per il `QuickLoginUI` ContentPlaceHolder, per impostazione predefinita. Pertanto, è necessario ricordarsi di rimuovere questi controlli di contenuto ogni volta che si aggiunge una nuova pagina contenuta (a meno che non si vuole eseguire l'override di contenuto predefinito del controllo ContentPlaceHolder, come nel caso la pagina di accesso).

Per rimuovere il controllo contenuto, è possibile eliminare manualmente il markup dichiarativo dalla vista origine oppure, dalla visualizzazione progettazione, scegliere l'impostazione predefinita al collegamento al contenuto dello schema dal suo smart tag. Entrambi gli approcci rimuove il controllo contenuto dalla pagina e produce lo stesso effetto di net.

Figura 8 mostra `Default.aspx` quando viene visualizzato tramite un browser. È importante ricordare che `Default.aspx` ha solo due controlli di contenuto specificati nel relativo markup dichiarativo - uno per `head` e uno per `MainContent`. Di conseguenza, il valore predefinito di contenuto per il `LeftColumnContent` e `QuickLoginUI` vengono visualizzati elementi ContentPlaceHolder.


[![Il contenuto predefinito per il LeftColumnContent e QuickLoginUI ContentPlaceHolders vengono visualizzati](multiple-contentplaceholders-and-default-content-vb/_static/image23.png)](multiple-contentplaceholders-and-default-content-vb/_static/image22.png)

**Figura 08**: The Default contenuto per il `LeftColumnContent` e `QuickLoginUI` vengono visualizzati elementi ContentPlaceHolder ([fare clic per visualizzare l'immagine con dimensioni normali](multiple-contentplaceholders-and-default-content-vb/_static/image24.png))


## <a name="summary"></a>Riepilogo

Il modello di pagina master ASP.NET consente un numero arbitrario di elementi ContentPlaceHolder nella pagina master. Inoltre, elementi ContentPlaceHolder comprendere contenuto predefinito, che viene generato nel caso che non vi sia alcun corrispondente nella pagina di contenuto controllo contenuto. In questa esercitazione è stato illustrato come includere altri controlli ContentPlaceHolder nella pagina master e come definire i controlli del contenuto per questi nuovi elementi ContentPlaceHolder nelle pagine ASP.NET sia nuovi che esistenti. Abbiamo anche esaminato impostazione del valore predefinito contenuti in un controllo ContentPlaceHolder, che risulta utile negli scenari in cui solo una minoranza degli esigenze di pagine per personalizzare l'in caso contrario standardizzato contenuto all'interno di una determinata area.

Nella prossima esercitazione verrà esaminato il `head` ContentPlaceHolder in modo più dettagliato, ottenere informazioni su come definire in modo dichiarativo e a livello di codice il titolo, metatag e altre intestazioni HTML in base a una pagina per pagina.

Buona programmazione!

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri e fondatore di 4GuysFromRolla.com, ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach manualmente ASP.NET 3.5 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata Suchi Banerjee. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Precedente](creating-a-site-wide-layout-using-master-pages-vb.md)
> [Successivo](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
