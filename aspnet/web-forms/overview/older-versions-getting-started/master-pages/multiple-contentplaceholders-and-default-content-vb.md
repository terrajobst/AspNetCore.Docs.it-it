---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
title: "Più gli elementi ContentPlaceHolder e il contenuto predefinito (VB) | Documenti Microsoft"
author: rick-anderson
description: "Esamina come aggiungere più segnaposto di contenuto a una pagina master, nonché come specificare il contenuto predefinito nei segnaposto di contenuto."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 866a7177-6884-451e-88f4-c934b1dd1af5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
msc.type: authoredcontent
ms.openlocfilehash: ccb65f0b2f16e0c7a67787f7dfab14303daeca1d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="multiple-contentplaceholders-and-default-content-vb"></a>Più gli elementi ContentPlaceHolder e il contenuto predefinito (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_VB.pdf)

> Esamina come aggiungere più segnaposto di contenuto a una pagina master, nonché come specificare il contenuto predefinito nei segnaposto di contenuto.


## <a name="introduction"></a>Introduzione

Nell'esercitazione precedente abbiamo esaminato come abilitare le pagine master sviluppatori ASP.NET per creare un layout a livello di sito coerente. Pagine master definiscono markup che sono comuni a tutte le relative pagine di contenuto e le aree che possono essere personalizzate in base a una pagina per pagina. Nell'esercitazione precedente è stata creata una semplice pagina master (`Site.master`) e due pagine di contenuto (`Default.aspx` e `About.aspx`). La pagina master è costituito da due gli elementi ContentPlaceHolder denominato `head` e `MainContent`, che si trovano nel `<head>` elemento e Web Form, rispettivamente. Mentre le pagine di contenuto ogni aveva due controlli contenuto, è specificato solo il markup per quello corrispondente a `MainContent`.

Come evidenziato da due controlli ContentPlaceHolder in `Site.master`, una pagina master può contenere diversi ContentPlaceHolder. Inoltre, la pagina master può specificare il markup predefinito per i controlli ContentPlaceHolder. Una pagina di contenuto, quindi, può facoltativamente specificare il proprio codice o il markup predefinito. In questa esercitazione è esaminare l'utilizzo di più controlli contenuti nella pagina master e vedere come definire il markup predefinito nei controlli ContentPlaceHolder.

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>Passaggio 1: Aggiunta di controlli ContentPlaceHolder aggiuntive per la pagina Master

Molti progetti di sito Web contengono numerose aree dello schermo personalizzati in base a una pagina per pagina. `Site.master`, la pagina master creata nell'esercitazione precedente contiene un solo ContentPlaceHolder all'interno di Web Form denominato `MainContent`. In particolare, questo ContentPlaceHolder si trova all'interno di `mainContent` `<div>` elemento.

Figura 1 mostra `Default.aspx` quando viene visualizzato tramite un browser. L'area racchiuse in un cerchio rosso è il markup specifico della pagina corrispondente a `MainContent`.


[![L'area in un cerchio è visualizzata l'Area attualmente personalizzabile in base a una pagina per pagina](multiple-contentplaceholders-and-default-content-vb/_static/image2.png)](multiple-contentplaceholders-and-default-content-vb/_static/image1.png)

**Figura 01**: l'area con cerchio Mostra l'Area attualmente personalizzabile in base a una pagina per pagina ([fare clic per visualizzare l'immagine ingrandita](multiple-contentplaceholders-and-default-content-vb/_static/image3.png))


Si supponga che oltre ad area illustrato nella figura 1, è necessario aggiungere gli elementi specifici della pagina per la colonna a sinistra sotto le lezioni e notizie sezioni. A tale scopo, è aggiungere un altro controllo ContentPlaceHolder della pagina master. Per proseguire, aprire il `Site.master` in Visual Web Developer pagina master e quindi trascinare un controllo ContentPlaceHolder dalla casella degli strumenti nella finestra di progettazione dopo la sezione di notizie. Impostare il ContentPlaceHolder `ID` a `LeftColumnContent`.


[![Aggiungere un controllo ContentPlaceHolder alla colonna a sinistra della pagina Master](multiple-contentplaceholders-and-default-content-vb/_static/image5.png)](multiple-contentplaceholders-and-default-content-vb/_static/image4.png)

**Figura 02**: aggiungere un controllo ContentPlaceHolder alla colonna di sinistra della pagina Master ([fare clic per visualizzare l'immagine ingrandita](multiple-contentplaceholders-and-default-content-vb/_static/image6.png))


Con l'aggiunta del `LeftColumnContent` ContentPlaceHolder della pagina master, possiamo definire il contenuto per l'area in base a una pagina per pagina includendo un contenuto controllo nella pagina di cui `ContentPlaceHolderID` è impostato su `LeftColumnContent`. Esaminiamo il processo nel passaggio 2.

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>Passaggio 2: Definire il contenuto per il nuovo ContentPlaceHolder nelle pagine di contenuto

Quando si aggiunge una nuova pagina contenuta per il sito Web, Visual Web Developer viene creata automaticamente un contenuto controllo nella pagina per ogni controllo ContentPlaceHolder nella pagina master selezionata. Dopo aver aggiunto un il `LeftColumnContent` ContentPlaceHolder alla nostra pagina master nel passaggio 1, nuovo ASP.NET pagine verranno dispone di tre controlli del contenuto.

A questo scopo, aggiungere una nuova pagina contenuta nella directory radice denominato `MultipleContentPlaceHolders.aspx` associato per il `Site.master` pagina master. Visual Web Developer viene creata in questa pagina con il seguente markup dichiarativo:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample1.aspx)]

Immettere alcuni contenuti nel controllo contenuto che fa riferimento il `MainContent` gli elementi ContentPlaceHolder (`Content2`). Successivamente, aggiungere il markup seguente per il `Content3` controllo contenuto (che fa riferimento il `LeftColumnContent` ContentPlaceHolder):

[!code-html[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample2.html)]

Dopo aver aggiunto il markup, visitare la pagina tramite un browser. Come mostrato nella figura 3, il codice inserito nella `Content3` controllo contenuto viene visualizzato nella colonna a sinistra sotto la sezione Novità (un cerchio in rosso). Il codice inserito in `Content2` viene visualizzato nella parte destra della pagina (un cerchio blu).


[![Nella colonna sinistra ora include contenuto specifico della pagina sotto la sezione Novità](multiple-contentplaceholders-and-default-content-vb/_static/image8.png)](multiple-contentplaceholders-and-default-content-vb/_static/image7.png)

**Figura 03**: il sinistra colonna ora include specifici della pagina contenuto sotto la sezione Novità ([fare clic per visualizzare l'immagine ingrandita](multiple-contentplaceholders-and-default-content-vb/_static/image9.png))


### <a name="defining-content-in-existing-content-pages"></a>Definire il contenuto nelle pagine di contenuto esistente

La creazione automatica di una nuova pagina contenuta incorpora il controllo ContentPlaceHolder che abbiamo aggiunto nel passaggio 1. Ma le due pagine contenute esistente - `About.aspx` e `Default.aspx` -non dispone di un controllo contenuto per il `LeftColumnContent` ContentPlaceHolder. Per specificare il contenuto per questo ContentPlaceHolder in queste due pagine esistenti, è necessario aggiungere un controllo contenuto effettuata.

A differenza di maggior parte dei controlli Web ASP.NET, casella degli strumenti di Visual Web Developer non include un elemento del controllo contenuto. È possibile digitare manualmente nel markup dichiarativo del controllo contenuto nella visualizzazione origine, ma un approccio più semplice e rapido consiste nell'utilizzare l'area di progettazione. Aprire il `About.aspx` pagina e passare alla visualizzazione progettazione. Come illustrato nella figura 4, il `LeftColumnContent` ContentPlaceHolder viene visualizzato nella visualizzazione progettazione, se si passa il mouse su di esso, il titolo visualizzato legge: "LeftColumnContent (Master)." L'inclusione di "Master" nel titolo indica che è presente alcun controllo contenuto nella pagina definito per questo ContentPlaceHolder. Se è presente un controllo contenuto per il controllo ContentPlaceHolder, come nel caso di `MainContent`, verrà visualizzato il titolo: "*ContentPlaceHolderID* (personalizzato)."

Per aggiungere un controllo contenuto per il `LeftColumnContent` ContentPlaceHolder per `About.aspx`espandere smart tag del ContentPlaceHolder e fare clic sul collegamento Crea contenuto personalizzato.


[![La visualizzazione di progettazione per About Mostra LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image11.png)](multiple-contentplaceholders-and-default-content-vb/_static/image10.png)

**Figura 04**: la visualizzazione di progettazione per `About.aspx` Mostra il `LeftColumnContent` ContentPlaceHolder ([fare clic per visualizzare l'immagine ingrandita](multiple-contentplaceholders-and-default-content-vb/_static/image12.png))


Facendo clic sul collegamento Crea contenuto personalizzato che genera l'errore necessari controllo pagina e i set di contenuto relativo `ContentPlaceHolderID` proprietà per il controllo ContentPlaceHolder `ID`. Ad esempio, facendo clic sul collegamento Crea contenuto personalizzato per `LeftColumnContent` area `About.aspx` aggiunge il seguente markup dichiarativo alla pagina:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>L'omissione di controlli contenuto

ASP.NET non è necessario che tutte le pagine di contenuto includono i controlli del contenuto per ogni ContentPlaceHolder definiti nella pagina master. Se un controllo contenuto viene omesso, il motore ASP.NET utilizza il tag definito all'interno di ContentPlaceHolder nella pagina master. Questo tag viene considerato il ContentPlaceHolder *predefinito contenuto* ed è utile negli scenari in cui il contenuto per un'area è comune tra la maggior parte delle pagine, ma deve essere personalizzato per un numero ridotto di pagine. Passaggio 3 illustra che specifica il contenuto predefinito della pagina master.

Attualmente, `Default.aspx` contiene due controlli del contenuto per il `head` e `MainContent` ContentPlaceHolder; non dispone di un controllo contenuto per `LeftColumnContent`. Di conseguenza, quando `Default.aspx` viene eseguito il rendering di `LeftColumnContent` viene utilizzato il contenuto del ContentPlaceHolder predefinito. Poiché è ancora necessario definire il contenuto predefinito per questa ContentPlaceHolder, l'effetto finale è che non viene generato per questa area. Per verificare questo comportamento, visitare `Default.aspx` tramite un browser. Come illustrato nella figura 5, nella colonna a sinistra sotto la sezione Novità non viene generato alcun codice.


[![Nessun contenuto viene eseguito il rendering per il LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image14.png)](multiple-contentplaceholders-and-default-content-vb/_static/image13.png)

**Figura 05**: nessun contenuto viene eseguito il rendering per il `LeftColumnContent` ContentPlaceHolder ([fare clic per visualizzare l'immagine ingrandita](multiple-contentplaceholders-and-default-content-vb/_static/image15.png))


## <a name="step-3-specifying-default-content-in-the-master-page"></a>Passaggio 3: Specificare il contenuto predefinito della pagina Master

Alcuni progetti di sito Web includono un'area, il cui contenuto è lo stesso per tutte le pagine del sito, ad eccezione di uno o due eccezioni. Si consideri un sito Web che supporta gli account utente. Tale sito richiede una pagina di accesso in cui visitatori possono immettere le credenziali per accedere al sito. Per accelerare il processo di accesso, i progettisti di siti Web potrebbero includere nome utente e password nelle caselle di testo nell'angolo superiore sinistro di ogni pagina per consentire agli utenti di accedere senza dover in modo esplicito, visitare la pagina di accesso. Anche se queste caselle di testo Nome utente e password sono utili nella maggior parte delle pagine, sono ridondanti nella pagina di accesso, che già contiene caselle di testo per le credenziali dell'utente.

Per implementare questa soluzione, è possibile creare un controllo ContentPlaceHolder nell'angolo superiore sinistro della pagina master. Ogni pagina necessari per visualizzare il nome utente e password nelle caselle di testo nell'angolo superiore sinistro potrebbe creare un controllo contenuto per questo ContentPlaceHolder e aggiungere l'interfaccia necessaria. Pagina di accesso, d'altra parte, sarebbe di omettere l'aggiunta di un controllo contenuto per questo ContentPlaceHolder o creerebbe un contenuto controllo con alcun tag definiti. Lo svantaggio di questo approccio è che è necessario ricordarsi di aggiungere il nome utente e password nelle caselle di testo a ogni pagina che è stato aggiunto al sito (ad eccezione di pagina di accesso). Questo può comportare dei problemi. Si è probabilmente omesso di aggiungere queste caselle di testo a una pagina o due o, ancora peggio, è possibile non implementare l'interfaccia correttamente (ad esempio aggiungendo una sola casella di testo anziché due).

Una soluzione migliore consiste nel definire il nome utente e password nelle caselle di testo come contenuto predefinito del ContentPlaceHolder. In questo modo, è necessario solo eseguire l'override di questo contenuto predefiniti in tali alcune pagine che non vengono visualizzati il nome utente e password nelle caselle di testo (l'account di accesso di pagina, ad esempio). Per illustrare che specifica il contenuto predefinito per un controllo ContentPlaceHolder, consente di implementare lo scenario appena descritto.

> [!NOTE]
> Il resto di questa esercitazione consente di aggiornare il sito Web in modo da includere un'interfaccia di accesso nella colonna a sinistra per tutte le pagine, ma la pagina di accesso. Tuttavia, questa esercitazione non esamina come configurare il sito Web per supportare gli account utente. Per ulteriori informazioni su questo argomento, fare riferimento a my [autenticazione basata su form, autorizzazione, gli account utente e ruoli](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) esercitazioni.


### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>Aggiungere un controllo ContentPlaceHolder e specificare il contenuto predefinito

Aprire il `Site.master` pagina master e aggiungere il markup seguente per la colonna a sinistra tra le `DateDisplay` sezione etichetta e lezioni:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample4.aspx)]

Dopo aver aggiunto questo markup visualizzazione di progettazione della pagina master dovrebbe essere simile alla figura 6.


[![La pagina Master include un controllo di accesso](multiple-contentplaceholders-and-default-content-vb/_static/image17.png)](multiple-contentplaceholders-and-default-content-vb/_static/image16.png)

**Figura 06**: la pagina Master include un controllo di accesso ([fare clic per visualizzare l'immagine ingrandita](multiple-contentplaceholders-and-default-content-vb/_static/image18.png))


Questo ContentPlaceHolder, `QuickLoginUI`, dispone di un controllo di accesso Web come il contenuto predefinito. Il controllo di accesso consente di visualizzare un'interfaccia utente che richiede l'immissione di nome utente e password con un pulsante Accedi. Facendo clic sul pulsante Accedi, il controllo di accesso convalida internamente le credenziali dell'utente sull'API di appartenenza. Per utilizzare questo controllo di accesso in pratica, quindi, è necessario configurare il sito per utilizzare l'appartenenza. In questo argomento non rientra nell'ambito di questa esercitazione; Fare riferimento a my [autenticazione basata su form, autorizzazione, gli account utente e ruoli](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) esercitazioni per ulteriori informazioni sulla creazione di un'applicazione web che supporta gli account utente.

È possibile personalizzare il comportamento o l'aspetto del controllo di accesso. Sono stati impostati due proprietà: `TitleText` e `FailureAction`. Il `TitleText` valore della proprietà, che per impostazione predefinita "Accedi", viene visualizzato nella parte superiore dell'interfaccia utente del controllo. Questa proprietà è impostato in modo che venga visualizzato il testo "Accedi" come un `<h3>` elemento. Il `FailureAction` proprietà indica che cosa fare se le credenziali dell'utente non sono validi. Per impostazione predefinita il valore `Refresh`, lascia l'utente nella stessa pagina e viene visualizzato un messaggio di errore all'interno del controllo di accesso. Ho cambiato in `RedirectToLoginPage`, che invia l'utente alla pagina di accesso in caso di credenziali non valide. Preferisco inviare all'utente alla pagina di accesso quando un utente tenta di eseguire l'accesso da un'altra pagina, ma ha esito negativo, poiché la pagina di accesso può contenere istruzioni aggiuntive e le opzioni che non facilmente entra nella colonna sinistra. Ad esempio, la pagina di accesso potrebbe includere opzioni per recuperare una password dimenticata o per creare un nuovo account.

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>Creazione pagina di accesso e si esegue l'override di contenuto predefinito

Con la pagina master completa, il passaggio successivo consiste nel creare la pagina di accesso. Aggiungere una pagina ASP.NET nella directory radice del sito denominato `Login.aspx`, associarlo al `Site.master` pagina master. In questo modo verrà creata una pagina con quattro controlli contenuto, uno per ognuno del ContentPlaceHolder definiti in `Site.master`.

Aggiungere un controllo di accesso per il `MainContent` controllo contenuto. Analogamente, è possibile aggiungere qualsiasi contenuto per il `LeftColumnContent` area. Tuttavia, assicurarsi di lasciare il controllo contenuto per il `QuickLoginUI` ContentPlaceHolder vuoto. Questo garantisce che l'account di accesso controllo non viene visualizzato nella colonna sinistra della pagina di accesso.

Dopo aver definito il contenuto per il `MainContent` e `LeftColumnContent` aree, markup dichiarativo della pagina di accesso dovrebbe essere simile al seguente:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample5.aspx)]

Figura 7 illustra questa pagina quando viene visualizzato tramite un browser. Poiché questa pagina specifica di un controllo contenuto per il `QuickLoginUI` ContentPlaceHolder, sostituirà il contenuto predefinito specificato nella pagina master. L'effetto finale è che il controllo di accesso visualizzato nella finestra di progettazione della pagina master visualizzazione (vedere Figura 6) non viene eseguito in questa pagina.


[![Pagina di accesso Represses contenuto predefinito di QuickLoginUI ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image20.png)](multiple-contentplaceholders-and-default-content-vb/_static/image19.png)

**Figura 07**: la pagina di accesso Represses il `QuickLoginUI` predefinito contenuto del ContentPlaceHolder ([fare clic per visualizzare l'immagine ingrandita](multiple-contentplaceholders-and-default-content-vb/_static/image21.png))


### <a name="using-the-default-content-in-new-pages"></a>Utilizzando il contenuto predefinito nelle nuove pagine

Si vuole visualizzare il controllo di accesso nella colonna a sinistra per tutte le pagine tranne che nella pagina account di accesso. A tale scopo, tutte le pagine contenute ad eccezione di pagina di accesso non devono eseguire un controllo contenuto per il `QuickLoginUI` ContentPlaceHolder. Omettendo un controllo contenuto, il contenuto del ContentPlaceHolder predefinito verrà utilizzato.

Le pagine di contenuto esistente - `Default.aspx`, `About.aspx`, e `MultipleContentPlaceHolders.aspx` -non è incluso un controllo contenuto per `QuickLoginUI` perché sono state create prima che il controllo ContentPlaceHolder è stato aggiunto alla pagina master. Pertanto, queste pagine esistenti non è necessitano aggiornare. Tuttavia, le nuove pagine aggiunte al sito Web includono un controllo contenuto per il `QuickLoginUI` ContentPlaceHolder, per impostazione predefinita. Pertanto, è necessario ricordarsi di rimuovere questi controlli del contenuto ogni volta che si aggiunge una nuova pagina contenuta (a meno che non si desidera eseguire l'override di contenuto predefinito del ContentPlaceHolder, come nel caso di pagina di accesso).

Per rimuovere il controllo contenuto, è possibile eliminare manualmente il markup dichiarativo dalla vista origine oppure, dalla visualizzazione progettazione, scegliere il valore predefinito di collegamento al contenuto della pagina Master dal suo smart tag. Entrambi gli approcci rimuove il controllo contenuto dalla pagina e produce lo stesso effetto di net.

La figura 8 mostra `Default.aspx` quando viene visualizzato tramite un browser. Tenere presente che `Default.aspx` ha solo due controlli contenuto, incluso il markup dichiarativo - uno per `head` e uno per `MainContent`. Di conseguenza, il valore predefinito di contenuto per il `LeftColumnContent` e `QuickLoginUI` vengono visualizzati gli elementi ContentPlaceHolder.


[![Vengono visualizzati il contenuto predefinito per il LeftColumnContent e QuickLoginUI ContentPlaceHolders](multiple-contentplaceholders-and-default-content-vb/_static/image23.png)](multiple-contentplaceholders-and-default-content-vb/_static/image22.png)

**Figura 08**: il predefinito contenuto per il `LeftColumnContent` e `QuickLoginUI` vengono visualizzati gli elementi ContentPlaceHolder ([fare clic per visualizzare l'immagine ingrandita](multiple-contentplaceholders-and-default-content-vb/_static/image24.png))


## <a name="summary"></a>Riepilogo

Il modello di pagina master ASP.NET consente un numero arbitrario di ContentPlaceHolder nella pagina master. In particolare, gli elementi ContentPlaceHolder includono il contenuto predefinito, che viene generato nel caso che sia corrispondente nella pagina contenuto controllo contenuto. In questa esercitazione è stato illustrato come includere altri controlli ContentPlaceHolder nella pagina master e come definire i controlli del contenuto per questi elementi ContentPlaceHolder nuovo nelle pagine ASP.NET sia nuovi che esistenti. È inoltre stato esaminato specifica predefinita contenuti in un controllo ContentPlaceHolder, che è utile negli scenari in cui solo una minoranza degli pagine è necessario personalizzare l'in caso contrario standardizzati contenuto all'interno di una determinata area geografica.

Nella prossima esercitazione verrà esaminato il `head` ContentPlaceHolder in modo più dettagliato, vedere come definire in modo dichiarativo e a livello di codice il titolo, tag meta e altre intestazioni HTML in base a una pagina per pagina.

Buona programmazione!

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 3.5 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott può essere raggiunto al [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o tramite il suo blog all'indirizzo [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata Suchi Banerjee. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Precedente](creating-a-site-wide-layout-using-master-pages-vb.md)
[Successivo](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
