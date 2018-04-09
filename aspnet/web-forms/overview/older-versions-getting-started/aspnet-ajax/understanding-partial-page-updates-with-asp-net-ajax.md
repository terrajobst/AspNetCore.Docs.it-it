---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: Informazioni sulla pagina parziale degli aggiornamenti con ASP.NET AJAX | Documenti Microsoft
author: scottcate
description: Ad esempio la funzionalità di ASP.NET AJAX Extensions più visibile è la possibilità di eseguire gli aggiornamenti di una pagina parziale o incrementale senza eseguire un postback completo a t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 91a98bf1c9a71ae84c569f7ae40930422cb652e0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="understanding-partial-page-updates-with-aspnet-ajax"></a>Pagina parziale a conoscenza degli aggiornamenti con ASP.NET AJAX
====================
da [Scott categorie](https://github.com/scottcate)

[Scarica il PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> Ad esempio la funzionalità di ASP.NET AJAX Extensions più evidente è la possibilità di eseguire gli aggiornamenti di una pagina parziale o incrementale senza eseguire un postback completo al server, senza modifiche al codice e le modifiche minime markup. I vantaggi sono molte: non viene modificato lo stato di multimediali (ad esempio Adobe Flash o Windows Media), vengono ridotti i costi di larghezza di banda e il client non presenta lo sfarfallio in genere associato a un postback.


## <a name="introduction"></a>Introduzione

Tecnologia Microsoft per ASP.NET offre un modello di programmazione orientata a oggetti e sugli eventi e si unisce i vantaggi del codice compilato. Tuttavia, il relativo modello di elaborazione sul lato server presenta numerosi svantaggi inerenti la tecnologia:

- Gli aggiornamenti a pagina richiedono un round trip al server, che richiede un aggiornamento della pagina.
- Round trip non vengono mantenute effetti generati da Javascript o altre tecnologie lato client (ad esempio Adobe Flash)
- Durante il postback browser diverso da Microsoft Internet Explorer non supporta il ripristino automaticamente la posizione di scorrimento. E anche in Internet Explorer, è comunque sfarfallio la pagina viene aggiornata.
- Postback possono comportare una quantità elevata di larghezza di banda come il \_ \_campo modulo VIEWSTATE può raggiungere, in particolare quando si gestiscono i controlli, ad esempio il controllo GridView o controlli Repeater.
- È presente alcun modello unificato per l'accesso ai servizi Web tramite JavaScript o altre tecnologie lato client.

Immettere AJAX ASP.NET le estensioni Microsoft. AJAX, è l'acronimo di **A** sincrono **J** avaScript **A** nd **X** ML, è un framework integrato per fornisce pagina incrementale gli aggiornamenti tramite JavaScript multipiattaforma, composta da codice lato server che include il Framework di Microsoft AJAX e un componente di script denominato Script Microsoft AJAX Library. Le estensioni di ASP.NET AJAX offrono inoltre supporto multipiattaforma per l'accesso ai servizi Web ASP.NET tramite JavaScript.

Questo white paper esamina la funzionalità di aggiornamenti di pagina parziale delle estensioni AJAX di ASP.NET, che include il componente ScriptManager, il controllo UpdatePanel e il controllo UpdateProgress e considera gli scenari in cui deve essere o non deve essere utilizzate.

Questo white paper dipende dalla versione Beta 2 di Visual Studio 2008 e .NET Framework 3.5, che integra Estensioni AJAX di ASP.NET in libreria di classi Base (in cui era in precedenza un componente aggiuntivo disponibile per ASP.NET 2.0). Questo white paper si presuppone inoltre che si utilizza Visual Studio 2008 e non Visual Web Developer Express Edition; alcuni modelli di progetto a cui fa riferimento potrebbero non essere disponibile per gli utenti di Visual Web Developer Express.

## <a name="partial-page-updates"></a>Aggiornamenti a pagina parziale

Ad esempio la funzionalità di ASP.NET AJAX Extensions più evidente è la possibilità di eseguire gli aggiornamenti di una pagina parziale o incrementale senza eseguire un postback completo al server, senza modifiche al codice e le modifiche minime markup. I vantaggi sono numerose, non viene modificato lo stato di multimediali (ad esempio Adobe Flash o Windows Media), vengono ridotti i costi di larghezza di banda e il client non presenta lo sfarfallio in genere associato a un postback.

La possibilità di integrare il rendering parziale della pagina è integrata in ASP.NET con modifiche minime al progetto.

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>Procedura dettagliata: Integrare il Rendering parziale di un progetto esistente


1. In Microsoft Visual Studio 2008, creare un nuovo progetto di sito Web ASP.NET, passare a <em>File</em>  <em>- &gt; New</em>  <em>- &gt; sito Web</em> e selezionando la finestra di dialogo sito Web ASP.NET. È possibile specificare il nome scelto liberamente ed è possibile installare nel file System o in Internet Information Services (IIS).
2. Verrà visualizzata la pagina vuota predefinito con markup ASP.NET di base (un form lato server e un `@Page` (direttiva)). Eliminare un'etichetta denominata `Label1` e un pulsante denominato `Button1` nella pagina all'interno dell'elemento di formato. È possibile impostare le relative proprietà text su qualsiasi elemento desiderato.
3. In visualizzazione Progettazione fare doppio clic su `Button1` per generare un gestore eventi di codice. All'interno di questo gestore eventi, impostare `Label1.Text` per il pulsante. .

**Elenco 1: Markup per default. aspx prima che il rendering parziale è attivato**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**Elenco 2: Codebehind (tagliati) in default.aspx.cs**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. Premere F5 per avviare il sito web. Visual Studio verrà richiesto di aggiungere un file Web. config per abilitare il debug; a tale scopo. Quando si fa clic sul pulsante, si noti che l'aggiornamento della pagina per modificare il testo nell'etichetta, e che sia un breve sfarfallio come la pagina viene ridisegnata.
2. Dopo avere chiuso la finestra del browser, tornare a Visual Studio e alla pagina di markup. Scorrere verso il basso nella casella degli strumenti di Visual Studio e individuare la scheda Estensioni AJAX. (Se non si dispone in questa scheda perché si sta usando una versione precedente di Estensioni AJAX o Atlas, fare riferimento alla procedura dettagliata per la registrazione degli elementi Estensioni AJAX più avanti in questo white paper o installare la versione corrente con il download di Windows Installer dal sito Web).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([Fare clic per visualizzare l'immagine ingrandita](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))


1. <em>Problema noto:</em>se si installa Visual Studio 2008 in un computer che dispone già di Visual Studio 2005 installato con ASP.NET 2.0 AJAX Extensions, Visual Studio 2008 verranno importati gli elementi della casella degli strumenti AJAX Extensions. È possibile determinare se questo è il caso esaminando la descrizione comando di componenti. affermano versione 3.5.0.0. Se si dice versione 2.0.0.0, quindi aver importato gli elementi della casella degli strumenti precedenti e sarà necessario importare manualmente utilizzando la finestra di dialogo Scegli elementi della casella degli strumenti in Visual Studio. Non sarà possibile aggiungere controlli versione 2 tramite la finestra di progettazione.

2. Prima di `<asp:Label>` tag di inizio, creare una linea di spazi vuoti e fare doppio clic sul controllo UpdatePanel nella casella degli strumenti. Si noti che un nuovo `@Register` direttiva è inclusa nella parte superiore della pagina, che indica che i controlli nello spazio dei nomi System.Web.UI devono essere importati utilizzando il `asp:` prefisso.
3. Trascinare la chiusura `</asp:UpdatePanel>` tag oltre la fine dell'elemento Button, in modo che l'elemento è ben formata con i controlli etichetta e pulsante eseguito il wrapping.
4. Dopo l'apertura `<asp:UpdatePanel>` tag, iniziare un nuovo tag di apertura. Si noti che IntelliSense chiede con due opzioni. In questo caso, creare un `<ContentTemplate>` tag. Assicurarsi di eseguire il wrapping di questo tag l'etichetta e un pulsante in modo che il markup sia ben formato.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([Fare clic per visualizzare l'immagine ingrandita](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))


1. In un punto qualsiasi all'interno di `<form>` elemento, includere un controllo ScriptManager facendo doppio clic sul `ScriptManager` nella casella degli strumenti.
2. Modificare il `<asp:ScriptManager>` tag in modo che include l'attributo `EnablePartialRendering= true`.

**Elenco di 3: Markup per default. aspx con il rendering parziale abilitato**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. Aprire il file Web. config. Si noti che Visual Studio ha aggiunto automaticamente un riferimento di compilazione a System.Web.Extensions.dll.

1. Novità di Visual Studio 2008: il file Web. config che viene fornito automaticamente con i modelli di progetto di sito Web ASP.NET include tutti i riferimenti necessari per ASP.NET AJAX Extensions e impostata come commento le sezioni di informazioni di configurazione che possono essere non è commentato per abilitare funzionalità aggiuntive. Visual Studio 2005 era modelli simile quando sono stati installati ASP.NET 2.0 AJAX Extensions. Tuttavia, in Visual Studio 2008, le estensioni AJAX sono opt-out per impostazione predefinita (vale a dire fa riferimento per impostazione predefinita, ma può essere rimossa come riferimenti).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([Fare clic per visualizzare l'immagine ingrandita](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))


1. Premere F5 per avviare il sito Web. Si noti come alcuna modifica del codice sorgente sono stati necessari per supportare il rendering parziale, è stato modificato solo il markup.

Quando si avvia il sito Web, si noterà che il rendering parziale è abilitata, perché quando fa clic su pulsante non vi sarà alcuna lo sfarfallio, né ci saranno cambiamenti nella posizione di scorrimento della pagina (in questo esempio viene illustrato che). Se esamina l'origine viene eseguito il rendering della pagina dopo aver fatto clic sul pulsante, confermerà che non si è verificato in effetti un post-backup: il testo dell'etichetta originale è ancora parte del markup di origine e l'etichetta è stata modificata tramite JavaScript.

Visual Studio 2008 non sembra dotati di un modello predefinito per un sito web ASP.NET per AJAX. Tuttavia, tale modello è disponibile in Visual Studio 2005 se sono stati installati di Visual Studio 2005 e ASP.NET 2.0 AJAX Extensions. Di conseguenza, la configurazione di un sito web e che inizia con il modello di sito Web compatibile con AJAX saranno probabilmente anche più semplice, come il modello deve includere un file completamente configurato Web. config (che supporta tutte le estensioni AJAX di ASP.NET, incluso l'accesso a servizi Web e serializzazione JSON - JavaScript Object Notation) e include un UpdatePanel e ContentTemplate all'interno della pagina Web Form principale, per impostazione predefinita. L'abilitazione di rendering parziale con la pagina predefinita è semplice come rivedere passaggio 10 della procedura dettagliata e rilasciando controlli nella pagina.

## <a name="the-scriptmanager-control"></a>Il controllo ScriptManager

## <a name="scriptmanager-control-reference"></a>Riferimento per il controllo ScriptManager

Proprietà markup abilitato:

| **Nome della proprietà** | **Type** | **Descrizione** |
| --- | --- | --- |
| AllowCustomErrors-Redirect | Bool | Specifica se utilizzare la sezione di errori personalizzati del file Web. config per la gestione degli errori. |
| AsyncPostBackError-Message | Stringa | Ottiene o imposta il messaggio di errore inviato al client se viene generato un errore. |
| AsyncPostBack-Timeout | Int32 | Ottiene o imposta la quantità predefinita di un'ora in cui che un client deve attendere il completamento della richiesta asincrona. |
| EnableScript-Globalization | Bool | Ottiene o imposta l'abilitazione di globalizzazione di script. |
| EnableScript-Localization | Bool | Ottiene o imposta se la localizzazione dello script è abilitata. |
| ScriptLoadTimeout | Int32 | Determina il numero di secondi consentiti per il caricamento di script nel client |
| ScriptMode | Enum (Auto, eseguire il Debug, rilascio, ereditano) | Ottiene o imposta se eseguire il rendering delle versioni di script |
| ScriptPath | Stringa | Ottiene o imposta il percorso radice per il percorso dei file di script da inviare al client. |

Proprietà del solo codice:

| **Nome della proprietà** | **Type** | **Descrizione** |
| --- | --- | --- |
| AuthenticationService | AuthenticationService-Manager | Ottiene i dettagli relativi al proxy di servizio di autenticazione ASP.NET che verrà inviato al client. |
| IsDebuggingEnabled | Bool | Ottiene se creare script e il debug del codice è abilitata. |
| IsInAsyncPostback | Bool | Indica se la pagina è attualmente in una richiesta di postback asincrona. |
| ProfileService | ProfileService-Manager | Ottiene i dettagli sul proxy del servizio di profilatura ASP.NET che verrà inviato al client. |
| Script | Raccolta&lt;riferimento a Script&gt; | Ottiene una raccolta di riferimenti a script che verrà inviato al client. |
| Servizi | Raccolta&lt;riferimento al servizio&gt; | Ottiene una raccolta di riferimenti al proxy servizio Web che verrà inviato al client. |
| SupportsPartialRendering | Bool | Indica se il client corrente supporta il rendering parziale. Se questa proprietà restituisce **false**, tutte le richieste di pagina verrà postback standard. |

Metodi pubblici di codice:

| **Nome del metodo** | **Type** | **Descrizione** |
| --- | --- | --- |
| SetFocus(string) | Void | Imposta lo stato attivo del client a un particolare controllo quando la richiesta è stata completata. |

Markup discendenti:

| **tag** | **Descrizione** |
| --- | --- |
| &lt;AuthenticationService&gt; | Fornisce dettagli sul proxy per il servizio di autenticazione ASP.NET. |
| &lt;ProfileService&gt; | Fornisce dettagli sul proxy per il servizio di analisi di ASP.NET. |
| &lt;Script&gt; | Fornisce i riferimenti agli script aggiuntivi. |
| &lt;asp:ScriptReference&gt; | Indica un riferimento a script specifici. |
| &lt;Service&gt; | Riferimenti aggiuntivi del servizio Web che conterrà le classi proxy generate. |
| &lt;asp:ServiceReference&gt; | Indica un riferimento al servizio Web specifico. |

Il controllo ScriptManager è il nucleo fondamentale per ASP.NET AJAX Extensions. Fornisce l'accesso alla libreria di script (incluso il sistema di tipo completo dello script lato client), supporta il rendering parziale e fornisce un supporto completo per i servizi aggiuntivi di ASP.NET (ad esempio l'autenticazione e di profilatura, ma anche altri servizi Web). Il controllo ScriptManager offre inoltre globalizzazione e localizzazione di un supporto per gli script client.

## <a name="providing-alterative-and-supplemental-scripts"></a>Fornire script alternative e supplementare

Mentre Microsoft ASP.NET 2.0 AJAX Extensions includono il codice di script intera in entrambe le modalità di debug e rilascio edizioni come risorse incorporate nell'assembly di riferimento, gli sviluppatori hanno la possibilità di reindirizzare ScriptManager ai file di script personalizzato, nonché registrare script necessari aggiuntive.

Per eseguire l'override l'associazione predefinita per gli script inclusi in genere (ad esempio quelli che supportano lo spazio dei nomi WebForms e il sistema di tipizzazione personalizzato), è possibile registrare per il `ResolveScriptReference` evento della classe ScriptManager. Quando questo metodo viene chiamato, il gestore dell'evento ha la possibilità di modificare il percorso del file di script in questione. il gestore di script invierà quindi una copia diversa o personalizzata degli script per il client.

Inoltre, i riferimenti agli script (rappresentato dalla `ScriptReference` classe) possono essere incluse a livello di codice o tramite codice. A tale scopo, a livello di codice modificare il `ScriptManager.Scripts` insieme, o includere `<asp:ScriptReference>` tag sotto il `<Scripts>` tag, che è un figlio di primo livello del controllo ScriptManager.

## <a name="custom-error-handling-for-updatepanels"></a>La gestione per UpdatePanel errori personalizzata

Anche se gli aggiornamenti vengono gestiti dai trigger specificato dai controlli UpdatePanel, il supporto per la gestione degli errori e messaggi di errore personalizzati viene gestito dall'istanza di controllo ScriptManager della pagina. Questa operazione viene eseguita tramite l'esposizione di un evento, `AsyncPostBackError`, alla pagina che può quindi fornire la logica di gestione delle eccezioni personalizzata.

Utilizzando l'evento AsyncPostBackError, è possibile specificare il `AsyncPostBackErrorMessage` proprietà, che quindi genera un avviso da generare dopo il completamento del callback.

Personalizzazione sul lato client, è anche possibile anziché utilizzare la casella di avviso predefinito. ad esempio, si desidera visualizzare un oggetto personalizzato `<div>` elemento anziché la finestra di dialogo modale di predefinita del browser. In questo caso, è possibile gestire l'errore nello script client:

**Elenco 5: Uno script sul lato Client per visualizzare gli errori personalizzati**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

Lo script precedente in parole povere, registra un callback con il runtime di AJAX lato client per quando è stata completata la richiesta asincrona. Verifica se è stato segnalato un errore, quindi e in tal caso, elabora i dettagli di esso, infine che indica al runtime che l'errore è stato gestito in uno script personalizzato.

## <a name="globalization-and-localization-support"></a>Supporto di localizzazione e globalizzazione

Il controllo ScriptManager fornisce supporto completo per la localizzazione delle stringhe di script e i componenti dell'interfaccia utente; Tuttavia, tale argomento è esterno all'ambito di questo white paper. Per ulteriori informazioni, vedere il white paper, supporto per la globalizzazione in ASP.NET AJAX Extensions.

## <a name="the-updatepanel-control"></a>Il controllo UpdatePanel

## <a name="updatepanel-control-reference"></a>Riferimento per il controllo UpdatePanel

Proprietà markup abilitato:

| **Nome della proprietà** | **Type** | **Descrizione** |
| --- | --- | --- |
| ChildrenAsTriggers | bool | Specifica se i controlli figlio richiamano automaticamente l'aggiornamento nel postback. |
| RenderMode | enum (blocco, in linea) | Specifica che il modo il contenuto verrà visualizzato in modo visivo. |
| UpdateMode | enum (sempre, condizionale) | Specifica se l'UpdatePanel è sempre aggiornato durante un rendering parziale o se solo viene aggiornato quando viene raggiunto un trigger. |

Proprietà del solo codice:

| **Nome della proprietà** | **Type** | **Descrizione** |
| --- | --- | --- |
| IsInPartialRendering | bool | Indica se l'UpdatePanel supporta il rendering parziale per la richiesta corrente. |
| ContentTemplate | ITemplate | Ottiene il modello di markup per la richiesta di aggiornamento. |
| ContentTemplateContainer | Control | Ottiene il modello a livello di codice per la richiesta di aggiornamento. |
| Trigger | UpdatePanel- TriggerCollection | Ottiene l'elenco dei trigger associati UpdatePanel corrente. |

Metodi pubblici di codice:

| **Nome del metodo** | **Type** | **Descrizione** |
| --- | --- | --- |
| Update) | Void | Aggiorna l'UpdatePanel specificato a livello di codice. Consente a una richiesta di server attivare un rendering parziale di un altro processo UpdatePanel. |

Markup discendenti:

| **tag** | **Descrizione** |
| --- | --- |
| &lt;ContentTemplate&gt; | Specifica il codice da utilizzare per eseguire il rendering di risultato del rendering parziale. Elemento figlio di &lt;asp: UpdatePanel&gt;. |
| &lt;Trigger&gt; | Specifica una raccolta di *n* controlli associati con l'aggiornamento di questo UpdatePanel. Elemento figlio di &lt;asp: UpdatePanel&gt;. |
| &lt;asp:AsyncPostBackTrigger&gt; | Specifica un trigger che richiama un rendering a pagina parziale per l'UpdatePanel specificato. Può supportare o non può essere un controllo come un discendente dell'UpdatePanel in questione. Livello di granularità per il nome dell'evento. Elemento figlio di &lt;trigger&gt;. |
| &lt;asp:PostBackTrigger&gt; | Specifica un controllo che provoca l'intera pagina di aggiornamento. Può supportare o non può essere un controllo come un discendente dell'UpdatePanel in questione. Livello di granularità per l'oggetto. Elemento figlio di &lt;trigger&gt;. |

Il `UpdatePanel` è il controllo che delimita il contenuto sul lato server che eseguirà parte della funzionalità di rendering parziale di AJAX Extensions. Non sono previsti limiti al numero di controlli UpdatePanel che possono essere presenti nella pagina e possono essere annidate. Ogni UpdatePanel è isolato, in modo che ogni possibile lavorare in modo indipendente (è possibile avere due UpdatePanel in esecuzione nello stesso momento, il rendering di parti diverse della pagina, indipendente di postback della pagina).

Controllare l'UpdatePanel riguarda principalmente con i trigger di controllo: per impostazione predefinita, qualsiasi controllo contenuto all'interno di un UpdatePanel `ContentTemplate` che crea un postback viene registrato come trigger per l'UpdatePanel. Ciò significa che l'UpdatePanel può funzionare con i controlli con associazione a dati predefiniti (ad esempio GridView), i controlli utente e può essere programmati nello script.

Per impostazione predefinita, quando viene attivato un rendering parziale, tutti i controlli nella pagina UpdatePanel verranno aggiornati, o meno l'UpdatePanel controlli definiti trigger per tale azione. Ad esempio, se un UpdatePanel definisce un controllo Button e si fa clic su tale pulsante, tutti i controlli UpdatePanel nella pagina verranno aggiornati per impostazione predefinita. Questo avviene perché, per impostazione predefinita, il `UpdateMode` dell'UpdatePanel è impostata su `Always`. In alternativa, è possibile impostare la proprietà UpdateMode su `Conditional`, il che significa che l'UpdatePanel verrà aggiornato solo se viene raggiunto un trigger specifico.

## <a name="custom-control-notes"></a>Note del controllo personalizzato

Un UpdatePanel può essere aggiunti a qualsiasi controllo utente o un controllo personalizzato. Tuttavia, la pagina in cui questi controlli sono inclusi inoltre deve includere un controllo ScriptManager con la proprietà EnablePartialRendering è impostata su **true**.

Un modo in cui può tenere conto di questo utilizzando i controlli Web personalizzati consiste nell'eseguire l'override protetto `CreateChildControls()` metodo la `CompositeControl` classe. In questo modo, è possibile inserire un UpdatePanel tra gli elementi figlio del controllo e il mondo esterno se che la pagina supporta il rendering parziale; in caso contrario, è possibile creare livelli semplicemente i controlli figlio in un contenitore `Control` istanza.

## <a name="updatepanel-considerations"></a>Considerazioni sull'UpdatePanel

UpdatePanel funziona come un elemento di una casella nera wrapping postback ASP.NET all'interno del contesto di un JavaScript XMLHttpRequest. Tuttavia, esistono considerazioni significativo delle prestazioni da tenere presente, sia in termini di velocità e comportamento. Per comprendere come funziona l'UpdatePanel, in modo che è possibile decidere meglio quando il relativo uso è appropriato, è necessario esaminare lo scambio di AJAX. L'esempio seguente usa un sito esistente e, Mozilla Firefox con l'estensione Firebug (Firebug acquisisce dati XMLHttpRequest).

Si consideri un form, ad esempio, con una casella di testo CAP che dovrebbe per popolare un campo di città e stato di un form o controllo. Questo modulo infine raccoglie informazioni di appartenenza, inclusi nome, indirizzo e informazioni di contatto dell'utente. Ci sono molte considerazioni di progettazione per prendere in considerazione, in base ai requisiti di un progetto specifico.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([Fare clic per visualizzare l'immagine ingrandita](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([Fare clic per visualizzare l'immagine ingrandita](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))


Nell'iterazione originale dell'applicazione, è stato creato un controllo che l'intera i dati di registrazione utente, inclusi il codice postale, città e stato incorporata. L'intero controllo è stato eseguito il wrapping in un UpdatePanel e rilasciato in un Web Form. Quando il codice postale viene immesso dall'utente, l'UpdatePanel rileva l'evento (l'evento TextChanged corrispondente nel back-end, specificando i trigger o tramite il set di proprietà ChildrenAsTriggers su true). AJAX invia tutti i campi all'interno di UpdatePanel, acquisite dalla FireBug (vedere il diagramma a destra).

Come indica l'acquisizione dello schermo, vengono recapitati i valori di ogni controllo all'interno di UpdatePanel (in questo caso, sono vuoti), nonché il campo di ViewState. Ciò detto, più di 9kb di dati viene inviato, quando in realtà solo cinque byte di dati necessarie per rendere questa specifica richiesta. La risposta è ancora più piene: in totale, 57kb viene inviato al client, semplicemente per aggiornare un campo di testo e un campo dell'elenco a discesa.

Potrebbe anche essere interessati per visualizzare la modalità di aggiornamento della presentazione in ASP.NET AJAX. La parte di risposta della richiesta di aggiornamento dell'UpdatePanel viene visualizzata nella visualizzazione della console Firebug a sinistra. è formulata in modo speciale delimitati da pipe stringa separata da script client e quindi riassemblata nella pagina. In particolare, ASP.NET AJAX imposta il *innerHTML* proprietà dell'elemento HTML nel client che rappresenta l'UpdatePanel. Come il browser genera nuovamente il modello DOM, sussiste un leggero ritardo, a seconda della quantità di informazioni che devono essere elaborato.

La rigenerazione del DOM genera un numero di problemi aggiuntivi:


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([Fare clic per visualizzare l'immagine ingrandita](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))


- Se l'elemento HTML all'interno di UpdatePanel, perderà lo stato attivo. In tal caso, per gli utenti premuto il tasto Tab per uscire dalla casella di testo al codice postale, la destinazione successiva sarebbe stata casella di testo City. Tuttavia, quando l'UpdatePanel aggiornate per la visualizzazione, il modulo non avrebbero lo stato attivo e premendo il tasto Tab potuto avviare l'evidenziazione degli elementi di messa a fuoco (ad esempio collegamenti).
- Se è in uso qualsiasi tipo di script sul lato client personalizzato che gli elementi DOM accessi, i riferimenti mantenuti tramite funzioni potrebbero diventare inattivo dopo un postback parziale.

UpdatePanel non deve essere soluzioni catch-all. Piuttosto, fornire una soluzione rapida per alcune situazioni, incluse la creazione di prototipi, gli aggiornamenti di controllo di piccole dimensioni e fornire un'interfaccia familiare agli sviluppatori ASP.NET che potrebbero essere familiarità con il modello a oggetti .NET ma in modo meno con il modello DOM. Esistono numerose alternative che può comportare un miglioramento delle prestazioni, a seconda dello scenario di applicazione:

- È consigliabile usare PageMethods e JSON (JavaScript Object Notation) consente agli sviluppatori di richiamare metodi statici in una pagina, come se una chiamata al servizio web è stata richiamata. Poiché i metodi sono statici, lo stato non è necessario. il chiamante di script fornisce i parametri e il risultato viene restituito in modo asincrono.
- È consigliabile utilizzare un servizio Web e JSON, se un singolo controllo deve essere utilizzato in diverse posizioni in un'applicazione. Questo nuovo richiede un impegno minimo speciale e funziona in modo asincrono.

Inserimento di funzionalità tramite i metodi di pagina o servizi Web presenta anche svantaggi. In primo luogo, gli sviluppatori ASP.NET tendono in genere per compilare piccoli componenti della funzionalità nei controlli utente (ascx). I metodi di pagina non possono essere ospitati in questi file; e devono essere ospitati all'interno della classe di pagina. aspx effettivo. Servizi Web, in modo analogo, devono essere ospitati all'interno della classe con estensione asmx. A seconda dell'applicazione, questa architettura potrebbe violare il principio OCP, in quanto la funzionalità per un singolo componente è ora estese tra due o più componenti fisici che possono avere valori equivalenti coesiva minime o nessuna.

Infine, se un'applicazione richiede che vengano utilizzati UpdatePanel, le linee guida seguenti devono supporta la manutenzione e risoluzione dei problemi.

- **Annidare UpdatePanel minor, non solo all'interno di unità, ma anche tra le unità di codice.** Ad esempio, un UpdatePanel in una pagina che esegue il wrapping di un controllo, mentre il controllo contiene anche un UpdatePanel, che contiene un altro controllo che contiene un UpdatePanel, è la nidificazione tra unità. Questo consente di individuare con chiarezza quali elementi devono essere aggiornati e impedisce gli aggiornamenti imprevisti figlio UpdatePanel.
- **Mantenere il *ChildrenAsTriggers* proprietà impostata su false e impostato in modo esplicito l'attivazione di eventi.** Utilizzo di `<Triggers>` raccolta è un modo molto più semplice per gestire gli eventi e potrebbe impedire un comportamento imprevisto, contribuendo con attività di manutenzione e la forzatura di uno sviluppatore di registrarsi per un evento.
- **Per ottenere funzionalità, utilizzare l'unità più piccola possibile.** Come indicato nella descrizione del servizio, codice postale wrapping che solo il livello minimo riduce il tempo per il server di elaborazione totale e il footprint di exchange server a client, miglioramento delle prestazioni.

## <a name="the-updateprogress-control"></a>Il controllo UpdateProgress

## <a name="updateprogress-control-reference"></a>Riferimento al controllo UpdateProgress

Proprietà markup abilitato:

| **Nome della proprietà** | **Type** | **Descrizione** |
| --- | --- | --- |
| AssociatedUpdate-PanelID | Stringa | Specifica l'ID dell'UpdatePanel che deve segnalare l'UpdateProgress. |
| DisplayAfter | Int | Specifica il timeout in millisecondi prima che questo controllo viene visualizzato dopo l'inizio della richiesta asincrona. |
| DynamicLayout | bool | Specifica se lo stato di avanzamento viene rappresentato in modo dinamico. |

Markup discendenti:

| **tag** | **Descrizione** |
| --- | --- |
| &lt;ProgressTemplate&gt; | Contiene il modello di controllo imposta per il contenuto che verrà visualizzato con questo controllo. |

Il controllo UpdateProgress offre una misura di commenti e suggerimenti per mantenere l'interesse degli utenti durante le operazioni necessarie per il server di trasporto. Questo può informare gli utenti che si sta eseguendo un'operazione anche se potrebbe non essere evidente, soprattutto perché la maggior parte degli utenti vengono utilizzati per la pagina di aggiornamento e visualizzare la barra di stato di evidenziare.

Come nota, controlli UpdateProgress possono trovarsi in qualsiasi punto in una gerarchia di pagina. Tuttavia, nei casi in cui un postback parziale viene avviato da un elemento figlio UpdatePanel (in cui un UpdatePanel è annidato all'interno di un altro UpdatePanel), i postback trigger figlio UpdatePanel causerà UpdateProgress modelli da visualizzare per l'elemento figlio UpdatePanel nonché padre UpdatePanel. Tuttavia, se il trigger è un figlio diretto dell'elemento padre UpdatePanel, verranno visualizzati solo i modelli di UpdateProgress associati al padre.

## <a name="summary"></a>Riepilogo

Le estensioni Microsoft ASP.NET AJAX sono prodotti sofisticati progettati per assistere nella creazione di contenuto web più facilmente accessibile e fornire l'esperienza utente per le applicazioni web. Come parte delle estensioni AJAX di ASP.NET, i controlli per il rendering a pagina parziale, inclusi i controlli ScriptManager, UpdatePanel e UpdateProgress sono riportati alcuni dei componenti del toolkit più visibili.

Il componente ScriptManager si integra il provisioning del client JavaScript per le estensioni, nonché consente i vari componenti sul lato server e client interagire con un investimento minimo di sviluppo.

Il controllo UpdatePanel è la casella di magic apparente - markup all'interno di UpdatePanel possono avere il Codebehind sul lato server e non l'attivazione di un aggiornamento della pagina. Controlli UpdatePanel possono essere annidati e possono dipendere controlli in altri UpdatePanel. Per impostazione predefinita, UpdatePanel gestiscono tutti i postback richiamati dai controlli con i relativi discendenti, anche se questa funzionalità può essere ottimizzata, in modo dichiarativo o a livello di codice.

Quando si utilizza il controllo UpdatePanel, gli sviluppatori devono tenere l'impatto sulle prestazioni che potenzialmente potrebbe verificarsi. Le alternative potenziali includono servizi web e i metodi di pagina, anche se è necessario considerare la progettazione dell'applicazione.

L'UpdateProgress controllo consente di sapere che lei o ha non viene ignorato, e che la richiesta di background sta succedendo mentre la pagina in caso contrario non eseguire alcuna operazione per rispondere all'input dell'utente. Include inoltre la possibilità di interrompere i risultati di rendering parziale.

Questi strumenti insieme, semplificare la creazione di un'esperienza utente avanzata e trasparente creando server funzionano meno visibili all'utente e interrompere il flusso di lavoro meno.

## <a name="bio"></a>Bio

Categoria Scott lavora con tecnologie Web di Microsoft dal 1997 ed è il vicepresidente myKB.com ([www.myKB.com](http://www.myKB.com)) in cui si è specializzato nella scrittura ASP.NET basato su applicazioni con stato attivo sulle soluzioni Software Knowledge Base. Scott possano essere contattati tramite posta elettronica al [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) o il suo blog all'indirizzo [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [avanti](understanding-asp-net-ajax-updatepanel-triggers.md)
