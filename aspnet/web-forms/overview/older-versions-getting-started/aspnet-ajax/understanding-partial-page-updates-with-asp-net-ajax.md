---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: La comprensione di aggiornamenti parziali di pagine con ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Probabilmente la caratteristica più visibile di ASP.NET AJAX Extensions è la possibilità di eseguire gli aggiornamenti di una pagina parziale o incrementale senza eseguire un postback completo a t...
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 4883046aa16d5e67b7f0c92e15c897ef1a933b67
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098935"
---
<a name="understanding-partial-page-updates-with-aspnet-ajax"></a>Aggiornamenti di conoscenza parziali di pagine con ASP.NET AJAX
====================
da [Scott Cate](https://github.com/scottcate)

[Scaricare PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> Probabilmente la caratteristica più visibile di ASP.NET AJAX Extensions è la possibilità di eseguire gli aggiornamenti di una pagina parziale o incrementale senza eseguire un postback completo al server, senza modifiche al codice e modifiche minime del markup. I vantaggi sono disponibili numerose: non viene modificato lo stato dei tuoi file multimediali (ad esempio Adobe Flash o supporti di Windows), vengono ridotti i costi della larghezza di banda e il client non prevede lo sfarfallio in genere associato a un postback.


## <a name="introduction"></a>Introduzione

Tecnologia Microsoft per ASP.NET offre un modello di programmazione orientate a oggetti e guidati da eventi e si unisce i vantaggi del codice compilato. Il modello di elaborazione sul lato server presenta tuttavia numerosi svantaggi intrinseci della tecnologia:

- Gli aggiornamenti a pagina richiedono un round trip al server, che richiede un aggiornamento della pagina.
- Round trip non vengono mantenute qualsiasi effetto generato da Javascript o altre tecnologie lato client (ad esempio Adobe Flash)
- Durante il postback browser diversi da Microsoft Internet Explorer non supportano il ripristino automatico della posizione di scorrimento. E anche in Internet Explorer, non esiste ancora uno sfarfallio come la pagina viene aggiornata.
- Durante i postback possono comportare una quantità elevata di larghezza di banda come le \_ \_può raggiungere campo del form VIEWSTATE, soprattutto quando si usano controlli, ad esempio il controllo GridView o ripetitori.
- Non è disponibile alcun modello unificato per l'accesso ai servizi Web tramite JavaScript o altre tecnologie lato client.

Immettere le estensioni ASP.NET AJAX di Microsoft. AJAX, acronimo di **un'** sincrono **J** avaScript **oggetto** nd **X** Machine Learning, è un framework integrato per fornire pagina incrementale gli aggiornamenti tramite JavaScript multipiattaforma, è costituito da codice lato server che include il Framework AJAX di Microsoft e un componente di script denominato Script Microsoft AJAX Library. Le estensioni ASP.NET AJAX forniscono anche supporto multipiattaforma per l'accesso ai servizi Web ASP.NET tramite JavaScript.

Questo white paper vengono esaminate le funzionalità di aggiornamenti parziali di pagine di ASP.NET AJAX Extensions, che include il componente ScriptManager, il controllo UpdatePanel e il controllo UpdateProgress e prende in considerazione gli scenari in cui deve o non deve essere utilizzata.

Questo white paper si basa sulla versione Beta 2 di Visual Studio 2008 e .NET Framework 3.5, che integra ASP.NET AJAX Extensions in libreria di classi Base (in cui era in precedenza un componente aggiuntivo disponibile per ASP.NET 2.0). Questo white paper si suppone inoltre che si usa Visual Studio 2008 e non Visual Web Developer Express Edition; alcuni modelli di progetto a cui fanno riferimento potrebbero non essere disponibile per gli utenti di Visual Web Developer Express.

## <a name="partial-page-updates"></a>Aggiornamenti parziali di pagine

Probabilmente la caratteristica più visibile di ASP.NET AJAX Extensions è la possibilità di eseguire gli aggiornamenti di una pagina parziale o incrementale senza eseguire un postback completo al server, senza modifiche al codice e modifiche minime del markup. I vantaggi sono disponibili numerose: non viene modificato lo stato dei tuoi file multimediali (ad esempio Adobe Flash o supporti di Windows), vengono ridotti i costi della larghezza di banda e il client non prevede lo sfarfallio in genere associato a un postback.

La capacità di rendering parziale della pagina di integrazione è integrata in ASP.NET con modifiche minime all'interno del progetto.

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>Procedura dettagliata: L'integrazione di Rendering parziale in un progetto esistente


1. In Microsoft Visual Studio 2008, creare un nuovo progetto di sito Web ASP.NET visitando <em>File</em>  <em>- &gt; New</em>  <em>- &gt; sito Web</em> e selezionando la finestra di dialogo sito Web ASP.NET. È possibile specificare un nome qualsiasi, e può installarla nel file System o in Internet Information Services (IIS).
2. Verrà visualizzata la pagina vuota predefinita con markup di base ASP.NET (un form lato server e un `@Page` (direttiva)). Eliminare un'etichetta denominata `Label1` e un pulsante denominato `Button1` nella pagina all'interno dell'elemento di formato. È possibile impostare le relative proprietà di testo a proprio piacimento.
3. Nella visualizzazione Progettazione fare doppio clic su `Button1` per generare un gestore di eventi di code-behind. All'interno di questo gestore eventi, impostare `Label1.Text` a è stato scelto il pulsante. .

**Listato 1: Markup per default. aspx prima di abilitata il rendering parziale**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**Listato 2: CodeBehind (tagliati) in default.aspx.cs**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. Premere F5 per avviare il sito web. Visual Studio chiederà di aggiungere un file Web. config per abilitare il debug; eseguire questa operazione. Quando si fa clic sul pulsante, si noti che la pagina viene aggiornata per modificare il testo nell'etichetta, e c'è uno sfarfallio breve perché la pagina viene ridisegnata.
2. Dopo avere chiuso la finestra del browser, tornare a Visual Studio e alla pagina di markup. Scorrere verso il basso nella casella degli strumenti di Visual Studio e trovare la scheda con etichettata AJAX Extensions. (Se non è in questa scheda perché si sta usando una versione precedente delle estensioni AJAX o Atlas, fare riferimento alla procedura dettagliata per registrare gli elementi della casella degli strumenti AJAX Extensions più avanti in questo white paper oppure installare la versione corrente con il programma di installazione di Windows scaricabili dal sito Web).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))


1. <em>Problema noto:</em>se si installa Visual Studio 2008 in un computer che dispone già di Visual Studio 2005 installato con ASP.NET 2.0 AJAX Extensions, Visual Studio 2008 verranno importati gli elementi della casella degli strumenti AJAX Extensions. È possibile determinare se questo è il caso, esaminare la descrizione comando; i componenti dice versione version=3.5.0.0. Se si dice versione 2.0.0.0, quindi aver importato gli elementi della casella degli strumenti precedenti e sarà necessario importare manualmente usando la finestra di dialogo Scegli elementi della casella degli strumenti in Visual Studio. Non sarà possibile aggiungere controlli di versione 2 tramite la finestra di progettazione.

2. Prima di `<asp:Label>` tag di inizio, creare una linea di spazi vuoti e fare doppio clic sul controllo UpdatePanel nella casella degli strumenti. Si noti che un nuovo `@Register` direttiva è inclusa nella parte superiore della pagina, che indica che i controlli all'interno dello spazio dei nomi System.Web.UI devono essere importati utilizzando il `asp:` prefisso.
3. Trascinare la chiusura `</asp:UpdatePanel>` tag oltre la fine dell'elemento Button, in modo che l'elemento è ben formata con i controlli Label e pulsante eseguito il wrapping.
4. Dopo l'apertura `<asp:UpdatePanel>` tag, iniziare un nuovo tag di apertura. Si noti che IntelliSense suggerisce due opzioni. In questo caso, creare un `<ContentTemplate>` tag. Assicurarsi di eseguire il wrapping di questo tag di etichetta e un pulsante in modo che il markup è ben formato.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))


1. In un punto qualsiasi all'interno di `<form>` elemento, includono un controllo ScriptManager facendo doppio clic su di `ScriptManager` elemento della casella degli strumenti.
2. Modificare il `<asp:ScriptManager>` contrassegnare in modo che includa l'attributo `EnablePartialRendering= true`.

**Listato 3: Markup per default. aspx con il rendering parziale abilitato**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. Aprire il file Web. config. Si noti che Visual Studio ha aggiunto automaticamente un riferimento di compilazione a System.Web.Extensions.dll.

1. Quali sono le novità in Visual Studio 2008: Il file Web. config che viene fornito con il sito Web ASP.NET modelli di progetto automaticamente include tutti i riferimenti necessari per ASP.NET AJAX Extensions e impostata come commento le sezioni di informazioni di configurazione che può essere annullata impostata come commento per abilitare ulteriori funzionalità. Visual Studio 2005 era modelli simili quando ASP.NET 2.0 AJAX Extensions sono stati installati. In Visual Studio 2008, tuttavia, le estensioni AJAX sono opt-out per impostazione predefinita (vale a dire viene fatto riferimento per impostazione predefinita, ma può essere rimossa come riferimenti).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))


1. Premere F5 per avviare il sito Web. Si noti come senza modifiche al codice sorgente sono stati necessari per supportare il rendering parziale è stato modificato solo il markup.

Quando si avvia il sito Web, si dovrebbe vedere che il rendering parziale è ora abilitato, in quanto quando fa clic su pulsante presenti non sarà lo sfarfallio, né sarà disponibile qualsiasi modifica nella posizione di scorrimento della pagina (in questo esempio non illustra che). Se si intende esaminare l'origine viene eseguito il rendering della pagina dopo aver fatto clic sul pulsante, confermerà che non si è verificato in effetti un post-back: il testo dell'etichetta originale è ancora parte di markup di origine e l'etichetta è stata modificata tramite JavaScript.

Visual Studio 2008 non è dotata di un modello predefinito per un sito web ASP.NET per AJAX. Tuttavia, tale modello era disponibile in Visual Studio 2005 se sono state installate di Visual Studio 2005 e ASP.NET 2.0 AJAX Extensions. Di conseguenza, la configurazione di un sito web e l'avvio con il modello sito Web compatibile con AJAX probabilmente sarà ancora più semplice, come il modello deve includere un file Web. config completamente configurato (supportano tutte le estensioni ASP.NET AJAX, incluso l'accesso a servizi Web e serializzazione JSON - JavaScript Object Notation) e include un UpdatePanel e l'elemento ContentTemplate all'interno della pagina Web Form principale, per impostazione predefinita. L'abilitazione di rendering parziale con questa pagina predefinita è semplice come passaggio 10 della procedura dettagliata di aggiornamento ed eliminazione di controlli nella pagina.

## <a name="the-scriptmanager-control"></a>Il controllo ScriptManager

## <a name="scriptmanager-control-reference"></a>Riferimento al controllo ScriptManager

Proprietà del markup abilitati:

| **Nome proprietà** | **Type** | **Descrizione** |
| --- | --- | --- |
| AllowCustomErrors-Redirect | Bool | Specifica se usare la sezione degli errori personalizzati del file Web. config per la gestione degli errori. |
| AsyncPostBackError-Message | Stringa | Ottiene o imposta il messaggio di errore inviato al client se viene generato un errore. |
| AsyncPostBack-Timeout | Int32 | Ottiene o imposta la quantità predefinita di un'ora che un client deve attendere il completamento della richiesta asincrona. |
| EnableScript-Globalization | Bool | Ottiene o imposta l'attivazione della globalizzazione di script. |
| EnableScript-Localization | Bool | Ottiene o imposta se la localizzazione dello script è abilitata. |
| ScriptLoadTimeout | Int32 | Determina il numero di secondi consentito per il caricamento di script nel client |
| ScriptMode | Enum (Auto, eseguire il Debug, versione, eredita) | Ottiene o imposta se eseguire il rendering delle versioni degli script |
| ScriptPath | Stringa | Ottiene o imposta il percorso radice per il percorso dei file di script da inviare al client. |

Proprietà di solo codice:

| **Nome proprietà** | **Type** | **Descrizione** |
| --- | --- | --- |
| AuthenticationService | AuthenticationService-Manager | Ottiene informazioni dettagliate su proxy del servizio di autenticazione ASP.NET che verrà inviato al client. |
| IsDebuggingEnabled | Bool | Ottiene se creare script e il debug del codice è abilitata. |
| IsInAsyncPostback | Bool | Indica se la pagina è attualmente in una richiesta di postback asincrona. |
| ProfileService | ProfileService-Manager | Ottiene informazioni dettagliate su proxy del servizio di profilatura ASP.NET che verrà inviato al client. |
| Script | Raccolta&lt;Script-Reference&gt; | Ottiene una raccolta di riferimenti a script che verrà inviato al client. |
| Servizi | Raccolta&lt;riferimento al servizio&gt; | Ottiene una raccolta di riferimenti Web al servizio proxy che verrà inviato al client. |
| SupportsPartialRendering | Bool | Indica se il client corrente supporta il rendering parziale. Se questa proprietà restituisce **false**, tutte le richieste di pagina verranno utilizzato come standard durante i postback. |

Metodi pubblici di codice:

| **Nome del metodo** | **Type** | **Descrizione** |
| --- | --- | --- |
| SetFocus(string) | Void | Imposta lo stato attivo del client a un particolare controllo quando la richiesta è stata completata. |

Discendenti di markup:

| **Tag** | **Descrizione** |
| --- | --- |
| &lt;AuthenticationService&gt; | Fornisce informazioni dettagliate su proxy per il servizio di autenticazione ASP.NET. |
| &lt;ProfileService&gt; | Fornisce i dettagli relativi al proxy per il servizio di profilatura ASP.NET. |
| &lt;Script&gt; | Vengono forniti i riferimenti di script aggiuntivi. |
| &lt;asp:ScriptReference&gt; | Indica un riferimento a script specifici. |
| &lt;Service&gt; | Fornisce ulteriori riferimenti al servizio Web che conterrà le classi proxy generate. |
| &lt;asp:ServiceReference&gt; | Indica un riferimento al servizio Web specifico. |

Il controllo ScriptManager è la base essenziale per ASP.NET AJAX Extensions. Fornisce l'accesso alla libreria di script (incluso il sistema di tipo completo dello script lato client), supporta il rendering parziale e fornisce supporto completo per i servizi ASP.NET aggiuntivi (ad esempio, l'autenticazione e di profilatura, ma anche altri servizi Web). Il controllo ScriptManager fornisce anche il supporto di globalizzazione e localizzazione per gli script client.

## <a name="providing-alternative-and-supplemental-scripts"></a>Funzionalità offre script alternativi e supplementare

Mentre Microsoft ASP.NET 2.0 AJAX Extensions è incluso il codice intero script in entrambe le modalità di debug e rilascio edizioni come risorse incorporate nell'assembly di riferimento, gli sviluppatori sono liberi di reindirizzare ScriptManager in file di script personalizzati, nonché registrare script aggiuntivi necessari.

Per eseguire l'override l'associazione predefinita per gli script inclusi in genere (ad esempio quelli che supportano lo spazio dei nomi Sys. WebForms e il sistema di tipizzazione personalizzato), è possibile registrarsi per il `ResolveScriptReference` eventi della classe ScriptManager. Quando questo metodo viene chiamato, il gestore dell'evento ha la possibilità di modificare il percorso del file di script in questione. lo script manager invierà quindi una copia diversa o personalizzata degli script al client.

Inoltre, i riferimenti agli script (rappresentato dal `ScriptReference` classe) può essere incluso a livello di codice o tramite markup. A tale scopo, a livello di codice modificare la `ScriptManager.Scripts` insieme, o includere `<asp:ScriptReference>` tag sotto il `<Scripts>` tag, che è un figlio di primo livello del controllo ScriptManager.

## <a name="custom-error-handling-for-updatepanels"></a>Personalizzata degli errori per UpdatePanel

Anche se gli aggiornamenti vengono gestiti dai trigger specificato da tutti i controlli UpdatePanel, il supporto per la gestione degli errori e messaggi di errore personalizzato viene gestito dall'istanza del controllo ScriptManager della pagina. Questa operazione viene eseguita tramite l'esposizione di un evento, `AsyncPostBackError`, alla pagina che può quindi fornire la logica di gestione delle eccezioni personalizzata.

Utilizzando l'evento AsyncPostBackError, è possibile specificare il `AsyncPostBackErrorMessage` proprietà, causando quindi una finestra di avviso da generare dopo il completamento del callback.

Personalizzazione lato client è anche possibile invece di usare la finestra di avviso predefinito; ad esempio, è possibile visualizzare un oggetto personalizzato `<div>` elemento anziché il dialogo modale browser predefinito. In questo caso, è possibile gestire l'errore nello script client:

**Listato 5: Script sul lato client per visualizzare gli errori personalizzati**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

In parole povere, lo script precedente registra un callback con il runtime di AJAX lato client per quando è stata completata la richiesta asincrona. Verifica per vedere se è stato segnalato un errore, quindi e in questo caso, elabora i dettagli di esso infine che indica al runtime che l'errore è stato gestito in uno script personalizzato.

## <a name="globalization-and-localization-support"></a>Supporto di localizzazione e globalizzazione

Il controllo ScriptManager offre ampio supporto per la localizzazione di stringhe dello script e i componenti dell'interfaccia utente; Tuttavia, tale argomento è all'esterno dell'ambito di questo white paper. Per altre informazioni, vedere il white paper, supporto di globalizzazione in ASP.NET AJAX Extensions.

## <a name="the-updatepanel-control"></a>Il controllo UpdatePanel

## <a name="updatepanel-control-reference"></a>Riferimento al controllo UpdatePanel

Proprietà del markup abilitati:

| **Nome proprietà** | **Type** | **Descrizione** |
| --- | --- | --- |
| ChildrenAsTriggers | bool | Specifica se i controlli figlio richiamano automaticamente l'aggiornamento nel postback. |
| RenderMode | enum (blocco, in linea) | Specifica che il modo il contenuto verrà visualizzato in modo visivo. |
| UpdateMode | enum (Always, condizionale) | Specifica se UpdatePanel è sempre aggiornato durante un rendering parziale o se si viene aggiornato solo quando viene raggiunto un trigger. |

Proprietà di solo codice:

| **Nome proprietà** | **Type** | **Descrizione** |
| --- | --- | --- |
| IsInPartialRendering | bool | Determina se UpdatePanel è il supporto per il rendering parziale per la richiesta corrente. |
| Elemento ContentTemplate | ITemplate | Ottiene il modello di markup per la richiesta di aggiornamento. |
| ContentTemplateContainer | Control | Ottiene il modello a livello di codice per la richiesta di aggiornamento. |
| Trigger | UpdatePanel - TriggerCollection | Ottiene l'elenco dei trigger associati UpdatePanel corrente. |

Metodi pubblici di codice:

| **Nome del metodo** | **Type** | **Descrizione** |
| --- | --- | --- |
| Update) | Void | Aggiorna UpdatePanel specificato a livello di codice. Consente a una richiesta di server attivare un rendering parziale di un UpdatePanel in caso contrario,-senza trigger. |

Discendenti di markup:

| **Tag** | **Descrizione** |
| --- | --- |
| &lt;Elemento ContentTemplate&gt; | Specifica il markup da utilizzare per eseguire il rendering di risultato del rendering parziale. Elemento figlio &lt;asp: UpdatePanel&gt;. |
| &lt;Trigger&gt; | Specifica una raccolta di *n* controlli associati con l'aggiornamento di questo UpdatePanel. Elemento figlio &lt;asp: UpdatePanel&gt;. |
| &lt;asp:AsyncPostBackTrigger&gt; | Specifica un trigger che richiama un rendering parziale della pagina per UpdatePanel specificato. Ciò può o potrebbe non essere un controllo come un discendente del controllo UpdatePanel in questione. Granularità per il nome dell'evento. Elemento figlio &lt;trigger&gt;. |
| &lt;asp:PostBackTrigger&gt; | Specifica un controllo che causa l'aggiornamento della pagina intera. Ciò può o potrebbe non essere un controllo come un discendente del controllo UpdatePanel in questione. Granulari all'oggetto. Elemento figlio &lt;trigger&gt;. |

Il `UpdatePanel` è il controllo che delimita il contenuto sul lato server che diventerà parte nella funzionalità di rendering parziale delle estensioni AJAX. Non sono previsti limiti al numero di controlli UpdatePanel che possono essere in una pagina e possono essere annidati. Ogni UpdatePanel è isolato, in modo che ognuna possa funzionare in modo indipendente (è possibile avere due gli UpdatePanel in esecuzione nello stesso momento, il rendering di parti diverse della pagina, indipendenti di postback della pagina).

UpdatePanel controllare trattative principalmente con i trigger di controllo: per impostazione predefinita, qualsiasi controllo contenuto all'interno di un UpdatePanel `ContentTemplate` che crea un postback viene registrato come un trigger per UpdatePanel. Ciò significa che UpdatePanel possono funzionare con i controlli con associazione a dati predefiniti (ad esempio GridView), con i controlli utente e che può essere programmati nello script.

Per impostazione predefinita, quando viene attivato un rendering parziale della pagina, verranno aggiornati tutti i controlli UpdatePanel nella pagina, o meno controlli UpdatePanel definiti trigger per tale azione. Ad esempio, se un UpdatePanel definisce un controllo Button e si fa clic sul controllo pulsante, tutti i controlli UpdatePanel nella pagina verranno aggiornati per impostazione predefinita. Questo avviene perché, per impostazione predefinita, il `UpdateMode` proprietà del controllo UpdatePanel è impostata su `Always`. In alternativa, è possibile impostare la proprietà UpdateMode su `Conditional`, il che significa che UpdatePanel verranno aggiornati solo se viene raggiunto un trigger specifico.

## <a name="custom-control-notes"></a>Note del controllo personalizzato

UpdatePanel può essere aggiunti a qualsiasi controllo utente o un controllo personalizzato. Tuttavia, la pagina in cui questi controlli sono inclusi anche deve includere un controllo ScriptManager con la proprietà impostata EnablePartialRendering **true**.

Un modo in cui potrebbe considerare questo aspetto quando si usa controlli Web personalizzati è eseguire l'override protetto `CreateChildControls()` metodo del `CompositeControl` classe. In questo modo, è possibile inserire un controllo UpdatePanel tra gli elementi figlio del controllo e il mondo esterno, se si determina che la pagina supporta il rendering parziale; in caso contrario, è possibile creare livelli semplicemente i controlli figlio in un contenitore `Control` istanza.

## <a name="updatepanel-considerations"></a>Considerazioni di UpdatePanel

UpdatePanel opera come un elemento di una scatola nera, ritorno a capo dei postback ASP.NET all'interno del contesto di un JavaScript XMLHttpRequest. Tuttavia, esistono considerazioni significativi delle prestazioni da tenere presente, in termini di comportamento e velocità. Per comprendere il funzionamento di UpdatePanel, in modo che è più possibile decidere se è corretto, è necessario esaminare lo scambio di AJAX. L'esempio seguente usa un sito esistente e, Mozilla Firefox con l'estensione Firebug (Firebug consente di acquisire dati XMLHttpRequest).

Prendere in considerazione un form, tra le altre cose, con una casella di testo il codice postale cui devono essere per popolare un campo città e stato di un form o controllo. Questo modulo in definitiva raccoglie le informazioni sull'appartenenza, inclusi nome, indirizzo e informazioni di contatto dell'utente. Esistono molte considerazioni di progettazione da prendere in considerazione, in base ai requisiti di un progetto specifico.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))


Nell'iterazione originale di questa applicazione, è stato creato un controllo incorporato l'intera struttura i dati di registrazione utente, tra cui il codice postale, città e stato. L'intero controllo è stato eseguito il wrapping in un controllo UpdatePanel e rilasciato in un Web Form. Quando il codice postale immesso dall'utente, UpdatePanel viene rilevato l'evento (l'evento TextChanged corrispondente nel back-end, specificando i trigger oppure tramite la proprietà ChildrenAsTriggers impostata su true). AJAX invia tutti i campi all'interno di UpdatePanel, acquisita dalla FireBug (vedere il diagramma a destra).

Come indica l'acquisizione di schermate, i valori di ogni controllo all'interno di UpdatePanel vengono recapitati (in questo caso, sono tutti vuoti), nonché il campo di ViewState. Ciò detto, più di 9kb di dati quando viene inviato, in realtà solo cinque byte di dati sono stati necessari per effettuare questa richiesta specifica. La risposta è ancora più piene: in totale, 57kb viene inviato al client, è sufficiente per aggiornare un campo di testo e un campo elenco a discesa.

Potrebbe anche essere interessati per visualizzare la procedura di aggiornamento della presentazione in ASP.NET AJAX. La parte di risposta della richiesta di aggiornamento di UpdatePanel viene visualizzata nella visualizzazione della console Firebug a sinistra. è formulata in modo speciale delimitato da barre verticali stringa suddivisa in base allo script client e quindi riassemblare nella pagina. In particolare, ASP.NET AJAX imposta il *innerHTML* proprietà dell'elemento HTML nel client che rappresenta il UpdatePanel. Come il browser genera nuovamente il modello DOM, è presente un leggero ritardo, a seconda della quantità di informazioni che devono essere elaborati.

La rigenerazione del DOM attiva una serie di problemi aggiuntivi:


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))


- Se l'elemento HTML con lo stato attivo è all'interno di UpdatePanel, perderà lo stato attivo. Pertanto, per gli utenti premuto il tasto Tab per uscire dalla casella di testo il codice postale, la destinazione successiva sarebbe stato la casella di testo City. Tuttavia, una volta UpdatePanel aggiornati la visualizzazione, il modulo non è più sarebbe stato attivo e premendo Tab avviare evidenziando gli elementi dello stato attivo (ad esempio, i collegamenti).
- Se qualsiasi tipo di script personalizzato dal lato client è in uso che gli elementi DOM gli accessi, i riferimenti mantenuti dalle funzioni potrebbero diventare inattivo dopo un postback parziale.

UpdatePanel non dovranno essere soluzioni catch-all. Piuttosto, fornire una soluzione rapida per alcune situazioni, inclusi gli aggiornamenti di controllo di piccole dimensioni, la creazione di prototipi e fornire un'interfaccia familiare agli sviluppatori ASP.NET che possono essere familiarità con il modello a oggetti .NET, ma minore di-pertanto con il DOM. Sono disponibili numerose alternative che potrebbero comportare prestazioni migliori, a seconda dello scenario dell'applicazione:

- È consigliabile usare PageMethods e JSON (JavaScript Object Notation) consente allo sviluppatore di richiamare metodi statici in una pagina come se una chiamata al servizio web è stata richiamata. Poiché i metodi sono statici, lo stato non è necessario; il chiamante dello script fornisce i parametri e il risultato viene restituito in modo asincrono.
- È consigliabile usare un servizio Web e JSON, se un singolo controllo deve essere usato in diverse posizioni in un'applicazione. Questo nuovo richiede uno sforzo speciale e funziona in modo asincrono.

Inserimento di funzionalità tramite i servizi Web o i metodi di pagina ha anche degli svantaggi. Prima di tutto, gli sviluppatori ASP.NET è in genere tendono a compilare piccoli componenti delle funzionalità nei controlli utente (file con estensione ascx). I metodi di pagina non possono essere ospitati in questi file; devono essere ospitati all'interno della classe di pagina aspx effettiva. Servizi Web, in modo analogo, devono essere ospitati all'interno della classe con estensione asmx. A seconda dell'applicazione, questa architettura può violare il principio di singola responsabilità, in quanto la funzionalità per un singolo componente è ora estese tra due o più componenti fisici che potrebbero essere non offrivano ties cohesive.

Infine, se un'applicazione richiede che vengano utilizzati gli UpdatePanel, le linee guida seguenti devono supporto per la manutenzione e risoluzione dei problemi.

- **Annidare gli UpdatePanel minor, non solo all'interno di unità, ma anche tra le unità di codice.** Ad esempio, un UpdatePanel in una pagina che esegue il wrapping di un controllo, mentre tale controllo contiene inoltre un UpdatePanel, che contiene un altro controllo che contiene un controllo UpdatePanel, è nidificazione tra unità. Ciò aiuta a mantenere chiaro quali elementi devono essere aggiornati e impedisce gli aggiornamenti imprevisti figlio UpdatePanel.
- **Mantenere il *ChildrenAsTriggers* proprietà impostata su false e impostato in modo esplicito l'attivazione di eventi.** Utilizzo di `<Triggers>` raccolta è un modo molto più chiaro per gestire gli eventi e potrebbe impedire un comportamento imprevisto, contribuire con le attività di manutenzione, nonché dell'imposizione agli sviluppatori di acconsentire esplicitamente per un evento.
- **Usare l'unità più piccola possibile per ottenere una funzionalità.** Come indicato nella descrizione del servizio codice postale, ritorno a capo che solo il livello minimo riduce il tempo per il server di elaborazione totale e il footprint dello scambio client / server, migliorando le prestazioni.

## <a name="the-updateprogress-control"></a>Il controllo UpdateProgress

## <a name="updateprogress-control-reference"></a>Riferimento al controllo UpdateProgress

Proprietà del markup abilitati:

| **Nome proprietà** | **Type** | **Descrizione** |
| --- | --- | --- |
| AssociatedUpdate-PanelID | Stringa | Specifica l'ID del controllo UpdatePanel che devono comunicare questa UpdateProgress in. |
| DisplayAfter | Int | Specifica il timeout in millisecondi prima che questo controllo viene visualizzato dopo l'inizio della richiesta asincrona. |
| DynamicLayout | bool | Specifica se lo stato di avanzamento deve essere visualizzato in modo dinamico. |

Discendenti di markup:

| **Tag** | **Descrizione** |
| --- | --- |
| &lt;ProgressTemplate&gt; | Contiene il modello di controllo impostato per il contenuto che verrà visualizzato con questo controllo. |

Il controllo UpdateProgress ti offre una misura di commenti e suggerimenti per mantenere l'interesse degli utenti mentre si eseguono le operazioni necessarie per trasporto al server. Questo può consentire agli utenti di sapere che si esegue un'operazione anche se potrebbe non essere evidente, soprattutto perché la maggior parte degli utenti sono abituati alla pagina di aggiornamento e visualizzare la barra di stato di evidenziazione.

Come nota, controlli UpdateProgress possono trovarsi in qualsiasi punto in una gerarchia di pagina. Tuttavia, nei casi in cui un postback parziale viene avviato da un elemento figlio (in cui un controllo UpdatePanel è annidato all'interno di un altro metodo UpdatePanel) UpdatePanel, postback trigger figlio UpdatePanel causerà UpdateProgress modelli da visualizzare per l'elemento figlio UpdatePanel, nonché l'elemento padre di UpdatePanel. Tuttavia, se il trigger è un figlio diretto dell'elemento padre UpdatePanel, quindi verranno visualizzati solo i modelli di UpdateProgress associati al padre.

## <a name="summary"></a>Riepilogo

Le estensioni di Microsoft ASP.NET AJAX sono prodotti sofisticati progettati per assistere nella creazione di contenuto web più accessibili e forniscono un'esperienza utente avanzata alle applicazioni web. Come parte di ASP.NET AJAX Extensions, i controlli di rendering parziale della pagina, inclusi i controlli di ScriptManager, UpdatePanel e UpdateProgress sono alcuni dei componenti più visibili del toolkit.

Il componente ScriptManager si integra la fornitura di JavaScript sul lato client per le estensioni, nonché consente i vari componenti dal lato server e client interagire con investimenti minima dello sviluppo.

Il controllo UpdatePanel è la casella di magic evidente: markup all'interno di UpdatePanel possono avere Codebehind lato server e non attivano un aggiornamento della pagina. I controlli UpdatePanel possono essere annidati e possono essere dipendente da controlli in altri UpdatePanel. Per impostazione predefinita, gli UpdatePanel gestiscono qualsiasi postback richiamato per i controlli discendenti, anche se questa funzionalità può essere ottimizzata, in modo dichiarativo o a livello di codice.

Quando si usa il controllo UpdatePanel, gli sviluppatori devono essere consapevoli dell'impatto sulle prestazioni che potenzialmente può verificarsi. Alternative possibili includono servizi web e i metodi di pagina, anche se deve essere considerata la progettazione dell'applicazione.

Il UpdateProgress controllo consente all'utente di sapere che lei o non viene ignorato e che la richiesta di background sta succedendo mentre la pagina in caso contrario, non eseguire alcuna operazione per rispondere all'input dell'utente. Contiene inoltre la possibilità di interrompere i risultati di rendering parziale.

Insieme, questi strumenti facilita la creazione di un'esperienza utente avanzata e trasparente dal funzionamento del server meno evidenti all'utente e la conseguente interruzione del flusso di lavoro meno.

## <a name="bio"></a>Informazioni biografiche

Scott Cate ha collaborato con tecnologie Web di Microsoft dal 1997 ed è il presidente di myKB.com ([www.myKB.com](http://www.myKB.com)) ed è specializzato nella scrittura di ASP.NET basato su applicazioni incentrate su soluzioni Software di Knowledge Base. È possibile contattare Scott tramite posta elettronica all'indirizzo [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) o il suo blog all'indirizzo [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [avanti](understanding-asp-net-ajax-updatepanel-triggers.md)
