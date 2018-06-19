---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: I controlli server | Documenti Microsoft
author: microsoft
description: ASP.NET 2.0 consente di ottimizzare i controlli server in molti modi. In questo modulo verranno descritte alcune delle modifiche dell'architettura per la modalità di ASP.NET 2.0 e 200 di Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: 72e9cac7cf9a01791c30783fa56ad7ea205a5a11
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
ms.locfileid: "28885193"
---
<a name="server-controls"></a>Controlli server
====================
by [Microsoft](https://github.com/microsoft)

> ASP.NET 2.0 consente di ottimizzare i controlli server in molti modi. In questo modulo verranno descritte alcune delle modifiche dell'architettura per la modalità di ASP.NET 2.0 e Visual Studio 2005 riguarda i controlli server.


ASP.NET 2.0 consente di ottimizzare i controlli server in molti modi. In questo modulo verranno descritte alcune delle modifiche dell'architettura per la modalità di ASP.NET 2.0 e Visual Studio 2005 riguarda i controlli server.

## <a name="view-state"></a>Stato di visualizzazione

La principale modifica nello stato di visualizzazione in ASP.NET 2.0 è una significativa riduzione delle dimensioni. Si consideri una pagina con solo un controllo di calendario su di esso. Ecco lo stato di visualizzazione in ASP.NET 1.1.

[!code-css[Main](server-controls/samples/sample1.css)]

Ora è qui lo stato di visualizzazione in una pagina identica in ASP.NET 2.0.

[!code-css[Main](server-controls/samples/sample2.css)]

Che è una modifica significativa molto e si considera che lo stato di visualizzazione viene trasferito avanti e indietro nella rete, questa modifica può consentire agli sviluppatori un aumento significativo delle prestazioni. La riduzione delle dimensioni dello stato di visualizzazione è dovuta principalmente il modo in cui viene gestito internamente. Tenere presente che visualizza lo stato è Base64 stringa codificata. Per comprendere meglio la modifica dello stato di visualizzazione in ASP.NET 2.0, si hanno un aspetto in base ai valori decodificati dagli esempi precedenti.

Di seguito è riportato lo stato di 1.1 visualizzazione decodificato:

[!code-css[Main](server-controls/samples/sample3.css)]

Ciò potrebbe sembrare un po' incomprensibile, ma in questo caso un modello. In ASP.NET 1. x, è utilizzato singoli caratteri per identificare tipi di dati e delimitato da valori utilizzando il &lt; &gt; caratteri. "T" nell'esempio di stato di visualizzazione precedente rappresenta una terna. Tre contiene una coppia di ArrayLists ("g" rappresenta un ArrayList). Uno dei tali ArrayLists contiene un valore Int32 ("i") con un valore pari a 1 e l'altro contiene un altro gruppo. Tre contiene una coppia di ArrayLists e così via. La cosa importante da ricordare è che utilizziamo ovvero che contengono coppie, si identificano i tipi di dati tramite una lettera e utilizziamo la &lt; e &gt; caratteri come delimitatori.

In ASP.NET 2.0, lo stato di visualizzazione decodificato è leggermente diverso.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Si noterà una modifica dell'aspetto dello stato di visualizzazione decodificato. Questa modifica ha più di base dell'architettura. Lo stato di visualizzazione in ASP.NET 1. x usato il LosFormatter per serializzare i dati. 2.0, utilizziamo la nuova classe di ObjectStateFormatter. Questa classe è stata progettata specificamente per facilitare la serializzazione e deserializzazione dello stato di visualizzazione e lo stato del controllo. (Lo stato del controllo verrà descritta nella sezione successiva.) Esistono numerosi vantaggi ottenuti modificando il metodo mediante il quale la serializzazione e deserializzazione avvenire. Uno dei vantaggi più significativi consiste nel fatto che, a differenza di LosFormatter che utilizza un oggetto TextWriter, di ObjectStateFormatter utilizza un oggetto BinaryWriter. In questo modo ASP.NET 2.0 archiviare lo stato di visualizzazione, una serie di byte invece di stringhe. Si consideri, ad esempio, un numero intero. In ASP.NET 1.1, un numero intero necessari 4 byte, dello stato di visualizzazione. In ASP.NET 2.0, tale integer stesso richiede solo 1 byte. Altri miglioramenti sono stati apportati per ridurre la quantità di stato di visualizzazione archiviato. I valori DateTime, ad esempio, vengono memorizzati utilizzando un TickCount anziché una stringa.

Come se tutti che non sono sufficienti, è stata pagata particolare attenzione al fatto che uno degli utenti massimo dello stato di visualizzazione in 1. x è il DataGrid controlli simili. Uno svantaggio principale dei controlli, ad esempio il controllo DataGrid in cui è attualmente preoccupato per lo stato di visualizzazione è che spesso contiene grandi quantità di informazioni ripetute. In ASP.NET 1. x, che è ripetuta informazioni semplicemente archiviate su e su nuovamente risultante in uno stato di visualizzazione troppo grandi. In ASP.NET 2.0, utilizziamo la nuova classe IndexedString per archiviare tali dati. Se si ripete una stringa, il token è semplicemente archiviati per il IndexedString e dell'indice all'interno di una tabella di oggetti IndexedString in esecuzione.

## <a name="control-state"></a>Lo stato del controllo

Uno dei principali lamentele, riprendendo che gli sviluppatori dovevano con lo stato di visualizzazione è la dimensione che aggiunto al payload HTTP. Come menzionato in precedenza, il consumer dello stato di visualizzazione maggiore è il controllo DataGrid. Per evitare l'enormi quantità di stato di visualizzazione generati da un controllo DataGrid, molti sviluppatori disabilitato semplicemente lo stato di visualizzazione per tale controllo. Sfortunatamente, tale soluzione non è sempre un buon. Lo stato di visualizzazione in ASP.NET 1. x contiene non solo i dati necessari per il corretto funzionamento del controllo. Contiene inoltre informazioni riguardanti lo stato dell'interfaccia utente del controllo. Ciò significa che se si desidera consentire per l'impaginazione di un controllo DataGrid, è necessario abilitare lo stato di visualizzazione, anche se non sono necessarie tutte le informazioni dell'interfaccia utente che consente di visualizzare lo stato contiene. È uno scenario radicale.

In ASP.NET 2.0, lo stato del controllo come risolvere il problema correttamente tramite l'introduzione di stato del controllo. Lo stato del controllo contiene i dati che sono assolutamente necessari per il corretto funzionamento di un controllo. A differenza di stato di visualizzazione, lo stato del controllo non può essere disabilitato. Pertanto, è importante che i dati archiviati in stato di controllo sono controllati con attenzione.

> [!NOTE]
> Persistenza dello stato di controllo con lo stato di visualizzazione nel \_ \_campo modulo nascosto VIEWSTATE.


In questo video è una procedura dettagliata lo stato di visualizzazione e lo stato del controllo.


![](server-controls/_static/image1.png)


[Aprire Video a schermo intero](server-controls/_static/state1.wmv)


Affinché un controllo server leggere e scrivere per controllare lo stato, è necessario eseguire tre passaggi.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>Passaggio 1: Chiamare il metodo RegisterRequiresControlState

Il metodo RegisterRequiresControlState informa ASP.NET che un controllo deve mantenere lo stato del controllo. Accetta un solo argomento di tipo di controllo che è il controllo da registrare.

È importante notare che la registrazione non viene mantenuto da una richiesta a richiesta. Pertanto, questo metodo deve essere chiamato a ogni richiesta se un controllo consiste nel rendere persistente lo stato di controllo. È consigliabile che il metodo deve essere chiamato in OnInit.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>Passaggio 2: Eseguire l'Override SaveControlState

Il metodo SaveControlState Salva le modifiche dello stato per un controllo di un controllo dall'ultimo postback. Restituisce un oggetto che rappresenta lo stato del controllo.

## <a name="step-3-override-loadcontrolstate"></a>Passaggio 3: Eseguire l'Override LoadControlState

Il metodo LoadControlState Carica lo stato salvato in un controllo. Il metodo accetta un argomento di tipo di oggetto che contiene lo stato salvato per il controllo.

## <a name="full-xhtml-compliance"></a>Conformità a XHTML completo

Gli sviluppatori Web conosce l'importanza di standard in applicazioni Web. Per mantenere un ambiente di sviluppo basato su standard, ASP.NET 2.0 è completamente conforme XHTML. Pertanto, tutti i tag sono sottoposto a rendering in base agli standard XHTML nei browser che supportano HTML 4.0 o versione successiva.

La definizione del tipo di documento in ASP.NET 1.1 è indicato di seguito:

[!code-html[Main](server-controls/samples/sample6.html)]

In ASP.NET 2.0, la definizione del tipo di documento predefinito è il seguente:

[!code-html[Main](server-controls/samples/sample7.html)]

Se si sceglie, è possibile modificare la conformità XHML predefinito tramite il nodo xhtmlConformance nel file di configurazione. Ad esempio, il nodo seguente nel file Web. config verrà visualizzata l'indicazione conformità XHTML XHTML 1.0 Strict:

[!code-xml[Main](server-controls/samples/sample8.xml)]

Se si sceglie, è inoltre possibile configurare ASP.NET per utilizzare la configurazione legacy utilizzata in ASP.NET 1. x come indicato di seguito:

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Esegue il Rendering tramite schede adattivo

In ASP.NET 1. x, il file di configurazione contenuto un &lt;browserCaps&gt; sezione in cui è stato popolato un oggetto HttpBrowserCapabilities. Questo oggetto è consentito uno sviluppatore di determinare quali dispositivi sono una particolare richiesta ed eseguire il rendering di codice in modo appropriato. In ASP.NET 2.0, il modello è migliorato e ora utilizza la nuova classe ControlAdapter. La classe ControlAdapter esegue l'override di eventi del ciclo di vita del controllo e controlla il rendering dei controlli in base alle funzionalità dell'agente utente. Le funzionalità di un agente utente specifico sono definite da un file di definizione del browser (un file con un'estensione di file con estensione browser) archiviato nel c:\windows\microsoft.net\framework\v2.0. \* \* \* \*\CONFIG\Browsers cartella.

> [!NOTE]
> La classe ControlAdapter è una classe astratta.


Analogamente il &lt;browserCaps&gt; sezione in 1. x, il file di definizione del browser utilizzata un'espressione regolare per analizzare la stringa agente utente per identificare il browser richiedente. Li definisce particolari funzionalità per l'agente utente. La ControlAdapter esegue il rendering il controllo tramite il metodo Render. Pertanto, se si esegue l'override del metodo Render, è necessario chiamare non eseguire il rendering nella classe base. Questa operazione potrebbe causare il rendering per due volte, una volta per l'adapter e una volta per il controllo stesso.

## <a name="developing-a-custom-adapter"></a>Sviluppo di un Adapter personalizzato

È possibile sviluppare un adapter personalizzato ereditando dalla classe ControlAdapter. Inoltre, è possibile ereditare dalla classe astratta PageAdapter nei casi in cui un adapter è necessaria per una pagina. Mapping di controlli per l'adapter personalizzato viene eseguito tramite il &lt;controlAdapters&gt; elemento nel file di definizione del browser. Il seguente codice XML da un file di definizione del browser, ad esempio, esegue il mapping del controllo Menu alla classe MenuAdapter:

[!code-html[Main](server-controls/samples/sample10.html)]

Utilizzando questo modello, diventa molto semplice per gli sviluppatori di controlli a un determinato dispositivo o browser di destinazione. Risulta inoltre più semplice per uno sviluppatore dispone del controllo completo su come viene eseguito il rendering di pagine su ogni dispositivo.

## <a name="per-device-rendering"></a>Per il Rendering per ogni dispositivo

Proprietà del controllo server ASP.NET 2.0 può essere specificata al dispositivo utilizzando un prefisso specifico del browser. Ad esempio, il codice riportato di seguito verrà modificare il testo di un'etichetta in base al quale dispositivo utilizzato per passare alla pagina.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Durante l'esplorazione della pagina che contiene l'etichetta da Internet Explorer, l'etichetta Visualizza il testo "Per l'esplorazione di Internet Explorer." Quando viene visualizzata la pagina da Firefox, l'etichetta verrà visualizzato il testo "Si sta utilizzando Firefox." Quando viene visualizzata la pagina da qualsiasi altro dispositivo, verrà visualizzato "Si sta utilizzando una periferica sconosciuta." Qualsiasi proprietà può essere specificato utilizzando questa sintassi speciale.

## <a name="setting-focus"></a>Impostando lo stato attivo

Gli sviluppatori di 1. x ASP.NET frequenti su come impostare lo stato attivo iniziale in un controllo specifico. Ad esempio, in una pagina di accesso, è utile ottenere lo stato attivo al primo caricamento della pagina nella casella di testo ID utente. In ASP.NET 1. x, in questo modo, è necessario scrivere alcuni script sul lato client. Anche se lo script è un'attività semplice, non è più necessario in ASP.NET 2.0 grazie al metodo SetFocus. Il metodo SetFocus accetta un argomento che indica il controllo che deve ricevere lo stato attivo. Questo argomento può essere l'ID client del controllo sotto forma di stringa o il nome del controllo Server come un oggetto di controllo. Ad esempio, per impostare lo stato attivo iniziale a un controllo casella di testo denominato txtUserID al primo caricamento della pagina, aggiungere il codice seguente alla pagina\_carico:

[!code-csharp[Main](server-controls/samples/sample12.cs)]

- o

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2.0 utilizza il gestore Webresource.axd (descritto in precedenza) per eseguire il rendering di una funzione sul lato client che imposta lo stato attivo. Il nome della funzione sul lato client è WebForm\_AutoFocus come illustrato di seguito:

[!code-html[Main](server-controls/samples/sample14.html)]

In alternativa, è possibile utilizzare il metodo lo stato attivo per un controllo per impostare lo stato attivo iniziale a tale controllo. Il metodo lo stato attivo deriva dalla classe Control ed è disponibile a tutti i controlli di ASP.NET 2.0. È inoltre possibile impostare lo stato attivo a un particolare controllo, quando si verifica un errore di convalida. Che verrà trattato in un modulo successivo.

## <a name="new-server-controls-in-aspnet-20"></a>Nuovi controlli Server ASP.NET 2.0

Di seguito sono nuovi controlli server ASP.NET 2.0. Alcuni di essi in moduli successivi verranno esaminate in dettaglio.

## <a name="imagemap-control"></a>Controllo ImageMap

Il controllo ImageMap consente di aggiungere aree sensibili a un'immagine che è possibile avviare un postback o passare a un URL. Sono disponibili tre tipi di aree sensibili; CircleHotSpot, RectangleHotSpot e PolygonHotSpot. Aree sensibili vengono aggiunti tramite un editor della raccolta in Visual Studio o a livello di programmazione nel codice. Non è disponibile per la creazione di aree sensibili in un'immagine senza interfaccia utente. Le coordinate e dimensioni o il raggio dell'area sensibile deve essere specificato in modo dichiarativo. Non vi è alcuna rappresentazione visiva di un punto attivo nella finestra di progettazione. Se un hotspot è configurato per passare a un URL, l'URL viene specificato tramite la proprietà NavigateUrl dell'area sensibile. Nel caso di un post di eseguire il punto attivo, il PostBackValue proprietà consente di passare una stringa nel nuovo post di che può essere recuperata nel codice sul lato server.


![Editor della raccolta hotSpot in Visual Studio](server-controls/_static/image1.jpg)

**Figura 1**: Editor della raccolta HotSpot in Visual Studio


## <a name="bulletedlist-control"></a>Controllo BulletedList

Il controllo di BulletedList è un elenco puntato che può essere facilmente i dati associati. L'elenco può essere ordinato (numerato) o non ordinata tramite la proprietà BulletStyle. Ogni elemento nell'elenco è rappresentato da un oggetto ListItem.


![Controllo BulletedList in Visual Studio](server-controls/_static/image1.gif)

**Figura 2**: controllo BulletedList in Visual Studio


## <a name="hiddenfield-control"></a>Controllo HiddenField

Il controllo HiddenField aggiunge un campo modulo nascosto nella pagina, il cui valore è disponibile nel codice sul lato server. Il valore di un campo nascosto del modulo avviene in genere rimangono invariate tra esegue il post. Tuttavia, è possibile che un utente malintenzionato di modificare il precedente valore per il postback. In questo caso, il controllo HiddenField genererà l'evento ValueChanged. Se si dispone di informazioni riservate nel controllo HiddenField e si desidera garantire che rimane invariato, è necessario gestire l'evento ValueChanged nel codice.

## <a name="fileupload-control"></a>Controllo fileUpload

Il controllo FileUpload in ASP.NET 2.0 consente di caricare file in un server Web tramite una pagina ASP.NET. Questo controllo è simile alla classe ASP.NET 1. x HtmlInputFile con alcune eccezioni. In ASP.NET 1. x, è consigliabile che la proprietà PostedFile essere controllati i valori null per determinare se è necessario un file valido. Il controllo FileUpload in ASP.NET 2.0 aggiunge una nuova proprietà HasFile che è possibile utilizzare per lo stesso scopo e risulta più efficiente.

La proprietà PostedFile è ancora disponibile per l'accesso a un oggetto HttpPostedFile, ma alcune delle funzionalità del HttpPostedFile è ora disponibile in modo intrinseco con il controllo FileUpload. Ad esempio, per salvare un file caricato in ASP.NET 1. x, si chiama il metodo SaveAs sull'oggetto HttpPostedFile. Utilizzo del controllo FileUpload in ASP.NET 2.0, chiamare il metodo SaveAs sul controllo FileUpload stesso.

Un altro cambiamento significativo nel comportamento 2.0 (e probabilmente la modifica più significativa) è che non è più necessario caricare un intero file caricato in memoria prima di salvarlo. In 1. x, viene salvato qualsiasi file che è stato caricato completamente in memoria prima della scrittura su disco. Questa architettura impedisce il caricamento del file di grandi dimensioni.

In ASP.NET 2.0, l'attributo dell'elemento httpRuntime requestLengthDiskThreshold consente di configurare il numero di kilobyte contenuti in un buffer in memoria prima della scrittura su disco.

**IMPORTANTE**: documentazione MSDN (e documentazione altrove) specifica che questo valore è espresso in byte (non in kilobyte) e che il valore predefinito è 256. In realtà è specificato il valore in kilobyte e il valore predefinito è 80. Con il valore predefinito 80K, è di assicurare che il buffer non terminare nell'heap degli oggetti di grandi dimensioni.

## <a name="wizard-control"></a>Controllo della procedura guidata

È abbastanza comune che si verifichino gli sviluppatori ASP.NET avrà difficoltà con tentativo di raccogliere informazioni in una serie di "pagine" pannelli o il trasferimento da una pagina. Molto spesso, il complicata uno frustrante e richiedere molto tempo. Controllo della procedura guidata nuovo risolve i problemi, consentendo lineari e non lineari passaggi tramite un'interfaccia che hanno familiari con gli utenti. Controllo della procedura guidata presenta i moduli di input in una serie di passaggi. Ogni passaggio è di un particolare tipo specificato dalla proprietà StepType del controllo. I tipi di passaggi disponibili sono come segue:

| **Tipo di passaggio** | **Spiegazione** |
| --- | --- |
| Auto | La procedura guidata determina automaticamente il tipo di passaggio in base alla relativa posizione all'interno della gerarchia di passaggio. |
| Inizia | Il primo passaggio, spesso usato per presentare un'istruzione introduttiva. |
| Passaggio | Un passaggio normale. |
| Fine | Il passaggio finale, in genere utilizzato per presentare un pulsante per terminare la procedura guidata. |
| Completa | Visualizza un messaggio di comunicazione esito positivo o negativo. |

> [!NOTE]
> Controllo della procedura guidata tiene traccia del relativo stato mediante lo stato del controllo ASP.NET. Pertanto, è possibile impostare la proprietà EnableViewState su false, senza alcun dannosa.


In questo video è una procedura dettagliata di controllo della procedura guidata.


![](server-controls/_static/image2.png)


[Aprire Video a schermo intero](server-controls/_static/wizard1.wmv)


## <a name="localize-control"></a>Localize (controllo)

Il controllo Localize è simile a un controllo Literal. Tuttavia, il controllo Localize ha un **modalità** proprietà che controlla la modalità con cui viene eseguito il rendering di markup che viene aggiunto. La proprietà Mode supporta i seguenti valori:

| **Modalità** | **Spiegazione** |
| --- | --- |
| Transform | Markup viene trasformato in base al protocollo del browser che effettua la richiesta. |
| PassThrough | Viene eseguito il rendering di markup come-è. |
| Codificare | Codice che viene aggiunto al controllo viene codificato tramite HtmlEncode. |

## <a name="multiview-and-view-controls"></a>MultiView e controlli di visualizzazione

Il controllo MultiView funge da contenitore per i controlli di visualizzazione e il controllo di visualizzazione funge da contenitore (molto simile a un pannello di controllo) per altri controlli. Ogni vista in un controllo MultiView è rappresentato da un singolo controllo di visualizzazione. Il primo controllo di visualizzazione di MultiView è 0, il secondo è 1, visualizzazione e così via. È possibile passare visualizzazioni specificando ActiveViewIndex del controllo MultiView.

## <a name="substitution-control"></a>Controllo Substitution

Il controllo di sostituzione è utilizzato in combinazione con la memorizzazione nella cache di ASP.NET. Nei casi in cui si desidera sfruttare i vantaggi della memorizzazione nella cache, ma si dispone di parti di una pagina che devono essere aggiornati a ogni richiesta (in altre parole, le parti di una pagina che non sono interessati dalla memorizzazione nella cache), il componente di sostituzione offre una soluzione ottima. Il rendering del controllo non viene effettivamente alcun output autonomamente. In alternativa, è associato a un metodo nel codice sul lato server. Quando la pagina viene richiesta, il metodo viene chiamato e il markup restituito viene eseguito il rendering al posto del controllo di sostituzione.

Il metodo a cui è associato il controllo di sostituzione viene specificato tramite il **NomeMetodo** proprietà. Tale metodo deve soddisfare i criteri seguenti:

- Deve essere un metodo statico (condiviso in VB).
- Accetta un parametro di tipo HttpContext.
- Restituisce una stringa che rappresenta il markup che deve sostituire il controllo nella pagina.

Il controllo di sostituzione ha la possibilità di modificare qualsiasi altro controllo nella pagina, ma dispone di accesso per l'oggetto HttpContext corrente tramite il relativo parametro.

## <a name="gridview-control"></a>Controllo GridView

Il controllo GridView è la sostituzione per il controllo DataGrid. Questo controllo verrà descritta in dettaglio in un modulo successivo.

## <a name="detailsview-control"></a>Controllo DetailsView

Il controllo DetailsView consente di visualizzare un singolo record da un'origine dati e di modificare o eliminare. È incluso in modo più dettagliato in un modulo successivo.

## <a name="formview-control"></a>Controllo FormView

Il controllo FormView è utilizzato per visualizzare un singolo record da un'origine dati in un'interfaccia configurabile. È incluso in modo più dettagliato in un modulo successivo.

## <a name="accessdatasource-control"></a>Controllo AccessDataSource

Il controllo AccessDataSource viene utilizzato per associare i dati un database di Access. È incluso in modo più dettagliato in un modulo successivo.

## <a name="objectdatasource-control"></a>Controllo ObjectDataSource

Il controllo ObjectDataSource viene utilizzato per supportare un'architettura a tre livelli in modo che controlli possono essere associato a un oggetto business di livello intermedio anziché un modello a due livelli in cui i controlli vengono associati direttamente all'origine dati. Verrà illustrata in dettaglio in un modulo successivo.

## <a name="xmldatasource-control"></a>XmlDataSource Control

Il controllo XmlDataSource viene utilizzato per associare dati a un'origine dati XML. È incluso in modo più dettagliato in un modulo successivo.

## <a name="sitemapdatasource-control"></a>Controllo SiteMapDataSource

Il controllo SiteMapDataSource fornisce un'associazione di dati per i controlli di navigazione del sito in base a una mappa del sito. Verrà illustrata in dettaglio in un modulo successivo.

## <a name="sitemappath-control"></a>SiteMapPath Control

Visualizza una serie di collegamenti di navigazione, comunemente noti come controlli di navigazione. È incluso in modo più dettagliato in un modulo successivo.

## <a name="menu-control"></a>Controllo Menu

Il controllo Menu consente di visualizzare menu dinamici con DHTML. È incluso in modo più dettagliato in un modulo successivo.

## <a name="treeview-control"></a>Controllo TreeView

Il controllo TreeView viene utilizzato per visualizzare una visualizzazione struttura ad albero gerarchica dei dati. È incluso in modo più dettagliato in un modulo successivo.

## <a name="login-control"></a>Controllo di accesso

Il controllo di accesso fornisce un meccanismo accedere a un sito Web. È incluso in modo più dettagliato in un modulo successivo.

## <a name="loginview-control"></a>Controllo LoginView

Il controllo LoginView consente la visualizzazione di modelli diversi in base al momento lo stato di accesso dell'utente. È incluso in modo più dettagliato in un modulo successivo.

## <a name="passwordrecovery-control"></a>Controllo PasswordRecovery

Il controllo PasswordRecovery viene utilizzato per recuperare le password dimenticate da parte degli utenti di un'applicazione ASP.NET. È incluso in modo più dettagliato in un modulo successivo.

## <a name="loginstatus"></a>LoginStatus

Il controllo LoginStatus Visualizza lo stato di accesso dell'utente. È incluso in modo più dettagliato in un modulo successivo.

## <a name="loginname"></a>LoginName

Il controllo LoginName Visualizza il nome dell'utente dopo aver registrato in un'applicazione ASP.NET. È incluso in modo più dettagliato in un modulo successivo.

## <a name="createuserwizard"></a>CreateUserWizard

CreateUserWizard è una procedura guidata configurabile che consente agli utenti la possibilità di creare un account di appartenenza di ASP.NET per l'utilizzo in un'applicazione ASP.NET. È incluso in modo più dettagliato in un modulo successivo.

## <a name="changepassword"></a>ChangePassword

Il controllo ChangePassword consente agli utenti di modificare la password per un'applicazione ASP.NET. È incluso in modo più dettagliato in un modulo successivo.

## <a name="various-webparts"></a>Web part vari

ASP.NET 2.0 viene fornito con diverse Web part. Questi verrà descritta in dettaglio in un modulo successivo.
