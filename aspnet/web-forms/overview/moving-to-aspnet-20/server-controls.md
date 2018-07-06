---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: I controlli server | Microsoft Docs
author: microsoft
description: ASP.NET 2.0 consente di migliorare i controlli server in molti modi. In questo modulo, illustreremo alcune delle modifiche dell'architettura per la modalità ASP.NET 2.0 e Visual Studio 200...
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: da06429f3949a47a02fccef45666d1220781e473
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837069"
---
<a name="server-controls"></a>Controlli server
====================
da [Microsoft](https://github.com/microsoft)

> ASP.NET 2.0 consente di migliorare i controlli server in molti modi. In questo modulo, illustreremo alcune delle modifiche dell'architettura alla modalità di ASP.NET 2.0 e Visual Studio 2005 gestisce i controlli server.


ASP.NET 2.0 consente di migliorare i controlli server in molti modi. In questo modulo, illustreremo alcune delle modifiche dell'architettura alla modalità di ASP.NET 2.0 e Visual Studio 2005 gestisce i controlli server.

## <a name="view-state"></a>Stato di visualizzazione

La modifica principale nello stato di visualizzazione in ASP.NET 2.0 è una notevole riduzione delle dimensioni. Si consideri una pagina con solo un controllo di calendario su di esso. Ecco lo stato di visualizzazione in ASP.NET 1.1.

[!code-css[Main](server-controls/samples/sample1.css)]

A questo punto è qui lo stato di visualizzazione in una pagina identica in ASP.NET 2.0.

[!code-css[Main](server-controls/samples/sample2.css)]

Che è una novità molto significativa, e considerando che lo stato di visualizzazione viene eseguito e viceversa in rete, questa modifica può offrire agli sviluppatori un aumento significativo delle prestazioni. La riduzione delle dimensioni dello stato di visualizzazione è dovuta principalmente il modo in cui gestiamo internamente. Tenere presente che in una stringa codificata che visualizza lo stato è Base64. Per comprendere meglio la modifica nello stato di visualizzazione in ASP.NET 2.0, si guardi in base ai valori decodificati dagli esempi precedenti.

Di seguito è riportato lo stato di 1.1 visualizzazione decodificato:

[!code-css[Main](server-controls/samples/sample3.css)]

Questo può apparire un po' come incomprensibile, ma è un modello di seguito. In ASP.NET 1.x, abbiamo utilizzato singoli caratteri per identificare i tipi di dati e delimitato da valori usando il &lt; &gt; caratteri. "T" nell'esempio di stato di visualizzazione precedente rappresenta una terna. La tripletta contiene una coppia di ArrayLists ("g" rappresenta un ArrayList). Uno di tali ArrayLists contiene un valore Int32 ("i") con un valore pari a 1 e l'altro contiene tre sezioni di un altro. La tripletta contiene una coppia di ArrayLists e così via. La cosa importante da ricordare è che usiamo Triplets che contengono coppie, si identificano i tipi di dati tramite una lettera e usiamo i &lt; e &gt; caratteri come delimitatori.

In ASP.NET 2.0, lo stato di visualizzazione decodificato ha un aspetto leggermente diverso.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Si dovrebbe notare una modifica nell'aspetto dello stato di visualizzazione decodificato. Questa modifica ha diversi fondamenti dell'architettura. Lo stato di visualizzazione in ASP.NET 1.x usato la LosFormatter per serializzare i dati. 2.0, utilizziamo la nuova classe ObjectStateFormatter. Questa classe è stato appositamente progettata per semplificare la serializzazione e deserializzazione dello stato di visualizzazione e lo stato del controllo. (Lo stato del controllo trattato nella sezione successiva.) Esistono diversi vantaggi ottenuti modificando il metodo mediante il quale la serializzazione e deserializzazione avvenire. Uno dei vantaggi più significativi consiste nel fatto che, a differenza di LosFormatter che usa un oggetto TextWriter, il ObjectStateFormatter Usa un oggetto BinaryWriter. In questo modo ASP.NET 2.0 archiviare lo stato di visualizzazione, una serie di byte anziché stringhe. Consideriamo, ad esempio, un numero intero. In ASP.NET 1.1, un numero intero necessari 4 byte dello stato di visualizzazione. In ASP.NET 2.0, tale integer stesso richiede solo 1 byte. Altri miglioramenti sono stati apportati per ridurre la quantità di stato di visualizzazione archiviato. I valori DateTime, ad esempio, vengono ora archiviati usando un TickCount anziché una stringa.

Come se tutto ciò non erano sufficienti, è stata pagata particolare attenzione al fatto che uno dei maggiori consumer dello stato di visualizzazione nella versione 1.x era il DataGrid e controlli simili. Un grave svantaggio dei controlli, ad esempio il controllo DataGrid in cui si occupa dello stato di visualizzazione consiste nel fatto che include spesso grandi quantità di informazioni ripetute. In ASP.NET 1.x, utilizzata per ripetere le informazioni semplicemente è state archiviate su e su causando in uno stato di visualizzazione troppo grandi. In ASP.NET 2.0, utilizziamo la nuova classe IndexedString per archiviare tali dati. Se si ripete una stringa, archiviamo solo il token per il IndexedString e l'indice all'interno di una tabella di oggetti IndexedString in esecuzione.

## <a name="control-state"></a>Lo stato del controllo

Uno delle lamentele principali che gli sviluppatori aveva lo stato di visualizzazione è la dimensione che aggiunto al payload HTTP. Come accennato in precedenza, uno dei maggiori consumer dello stato di visualizzazione è il controllo DataGrid. Per evitare le enormi quantità di stato di visualizzazione generato da un DataGrid, molti sviluppatori disabilitato semplicemente lo stato di visualizzazione per il controllo. Sfortunatamente, tale soluzione non è sempre una buona scelta. Lo stato di visualizzazione in ASP.NET 1.x contiene non solo i dati necessari per la funzionalità corretta del controllo. Include anche informazioni riguardanti lo stato dell'interfaccia utente del controllo. Questo significa che se si vuole consentire per l'impaginazione in un DataGrid, è necessario abilitare lo stato di visualizzazione anche se non sono necessarie tutte le informazioni dell'interfaccia utente che consente di visualizzare lo stato contiene. È uno scenario di tipo tutto o niente.

In ASP.NET 2.0, lo stato del controllo risolve il problema senza problemi tramite l'introduzione dello stato del controllo. Lo stato del controllo contiene i dati che sono assolutamente necessari per il corretto funzionamento di un controllo. A differenza di stato di visualizzazione, lo stato del controllo non può essere disabilitato. Pertanto, è importante che i dati archiviati in stato di controllo sono controllati con attenzione.

> [!NOTE]
> Lo stato di controllo viene mantenuto insieme allo stato di visualizzazione nel \_ \_campo del form nascosto VIEWSTATE.


In questo video è una procedura dettagliata dello stato di visualizzazione e lo stato del controllo.


![](server-controls/_static/image1.png)


[Aprirlo Video a schermo intero](server-controls/_static/state1.wmv)


Affinché un controllo server leggere e scrivere per controllare lo stato, è necessario eseguire tre passaggi.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>Passaggio 1: Chiamare il metodo RegisterRequiresControlState

Il metodo RegisterRequiresControlState informa ASP.NET che un controllo deve rendere persistente lo stato del controllo. Accetta un solo argomento di tipo di controllo che rappresenta il controllo in fase di registrazione.

È importante notare che la registrazione non viene mantenuto da richiesta a richiesta. Di conseguenza, questo metodo deve essere chiamato a ogni richiesta se un controllo consiste nel rendere persistente lo stato del controllo. È consigliabile che il metodo deve essere chiamato in OnInit.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>Passaggio 2: Eseguire l'Override SaveControlState

Il metodo SaveControlState controllo Salva le modifiche di stato per un controllo dopo l'ultimo postback. Restituisce un oggetto che rappresenta lo stato del controllo.

## <a name="step-3-override-loadcontrolstate"></a>Passaggio 3: Eseguire l'Override LoadControlState

Il metodo LoadControlState Carica lo stato salvato in un controllo. Il metodo accetta un argomento di tipo Object che contiene lo stato salvato per il controllo.

## <a name="full-xhtml-compliance"></a>Conformità XHTML completo

Qualsiasi sviluppatore Web conosce l'importanza di standard nelle applicazioni Web. Per mantenere un ambiente di sviluppo basato su standard, ASP.NET 2.0 è completamente conforme a XHTML. Pertanto, tutti i tag sono sottoposto a rendering in base agli standard XHTML nei browser che supportano HTML 4.0 o versione successiva.

La definizione del tipo di documento in ASP.NET 1.1 è come segue:

[!code-html[Main](server-controls/samples/sample6.html)]

In ASP.NET 2.0, la definizione di tipo di documento predefinita è come segue:

[!code-html[Main](server-controls/samples/sample7.html)]

Se si sceglie, è possibile modificare la conformità XHML predefinito tramite il nodo xhtmlConformance nel file di configurazione. Ad esempio, il nodo seguente nel file Web. config cambierà conformità XHTML per XHTML 1.0 Strict:

[!code-xml[Main](server-controls/samples/sample8.xml)]

Se si sceglie, è anche possibile configurare ASP.NET per usare la configurazione legacy utilizzata in ASP.NET 1.x come indicato di seguito:

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Esegue il Rendering tramite schede adattive

In ASP.NET 1.x, il file di configurazione contenuto un &lt;browserCaps&gt; sezione in cui è stato popolato un oggetto HttpBrowserCapabilities. Questo oggetto consentito agli sviluppatori di determinare quale dispositivo effettua una richiesta specifica ed eseguire il rendering di codice in modo appropriato. In ASP.NET 2.0, il modello ha migliorato e ora Usa la nuova classe ControlAdapter. La classe ControlAdapter esegue l'override di eventi del ciclo di vita del controllo e controlla il rendering dei controlli in base alle funzionalità dell'agente utente. Le funzionalità di un agente utente specifici vengono definite da un file di definizione del browser (un file con un'estensione di file con estensione) archiviato nel c:\windows\microsoft.net\framework\v2.0. \* \* \* \*\CONFIG\Browsers cartella.

> [!NOTE]
> La classe ControlAdapter è una classe astratta.


In modo analogo i &lt;browserCaps&gt; sezione nella versione 1.x, il file di definizione browser Usa un'espressione regolare per analizzare la stringa agente utente allo scopo di identificare il browser richiedente. Li definisce particolari funzionalità per l'agente utente. La ControlAdapter esegue il rendering il controllo tramite il metodo Render. Pertanto, se si esegue l'override del metodo Render, non chiamare rendering sulla classe di base. Questa operazione potrebbe causare il rendering che si verifichi due volte, una volta per l'adapter e una volta per il controllo stesso.

## <a name="developing-a-custom-adapter"></a>Sviluppa un Adapter personalizzato

È possibile sviluppare un adapter personalizzato ereditando da ControlAdapter. Inoltre, è possibile ereditare dalla classe astratta PageAdapter nei casi in cui un adapter è necessaria per una pagina. Mapping dei controlli per l'adapter personalizzato viene eseguito tramite il &lt;controlAdapters&gt; elemento nel file di definizione del browser. Ad esempio, il codice XML seguente da un file di definizione del browser viene eseguito il mapping del controllo Menu alla classe MenuAdapter:

[!code-html[Main](server-controls/samples/sample10.html)]

Usando questo modello, diventa molto facile per gli sviluppatori di controlli a un determinato dispositivo o un browser di destinazione. È anche molto semplice per uno sviluppatore di controllo completo sulla modalità di rendering delle pagine eseguito in tutti i dispositivi.

## <a name="per-device-rendering"></a>Per il Rendering per ogni dispositivo

Proprietà del controllo server ASP.NET 2.0 può essere specificato per ogni dispositivo usando un prefisso di specifiche del browser. Ad esempio, il codice seguente cambierà il testo di un'etichetta in base al quale dispositivo viene usato per esplorare la pagina.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Quando viene visualizzata la pagina contenenti questa etichetta da Internet Explorer, l'etichetta Visualizza il testo che indica che "L'esplorazione di Internet Explorer." Quando viene visualizzata la pagina da Firefox, l'etichetta Visualizza il testo "L'esplorazione da Firefox." Quando viene visualizzata la pagina da qualsiasi altro dispositivo, viene visualizzato "Esplorazione da un dispositivo sconosciuto." Qualsiasi proprietà può essere specificata usando questa sintassi speciale.

## <a name="setting-focus"></a>Impostazione dello stato attivo

Gli sviluppatori ASP.NET 1.x frequenti su come impostare lo stato attivo iniziale in un determinato controllo. Ad esempio, in una pagina di accesso, è utile disporre la casella di testo di ID utente di ottenere lo stato attivo al primo caricamento della pagina. In ASP.NET 1.x, questa operazione è necessario scrivere alcuni script lato client. Anche se uno script di questo tipo è un'attività banale, non è più necessario in ASP.NET 2.0 grazie al metodo di funzione SetFocus. Il metodo di funzione SetFocus accetta un argomento che indica il controllo che deve ricevere lo stato attivo. Questo argomento può essere l'ID client del controllo sotto forma di stringa o il nome del controllo Server come un oggetto di controllo. Ad esempio, per impostare lo stato attivo iniziale a un controllo TextBox denominato txtUserID al primo caricamento della pagina, aggiungere il codice seguente alla pagina\_carico:

[!code-csharp[Main](server-controls/samples/sample12.cs)]

- o

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2.0 usa il gestore WebResource. axd (illustrato in precedenza) per eseguire il rendering di una funzione sul lato client, che imposta lo stato attivo. Il nome della funzione lato client è WebForm\_AutoFocus come illustrato di seguito:

[!code-html[Main](server-controls/samples/sample14.html)]

In alternativa, è possibile usare il metodo lo stato attivo per un controllo per impostare lo stato attivo iniziale per tale controllo. Il metodo Focus deriva dalla classe Control ed è disponibile a tutti i controlli di ASP.NET 2.0. È anche possibile impostare lo stato attivo a un particolare controllo quando si verifica un errore di convalida. Che verrà descritta in un modulo successivo.

## <a name="new-server-controls-in-aspnet-20"></a>Nuovi controlli Server ASP.NET 2.0

Di seguito sono nuovi controlli server ASP.NET 2.0. Verranno esaminate più dettagliate su alcuni di essi nei moduli di versioni successive.

## <a name="imagemap-control"></a>Controllo ImageMap

Il controllo ImageMap consente di aggiungere le aree sensibili di un'immagine che può avviare un postback o passare a un URL. Sono disponibili tre tipi di aree sensibili; CircleHotSpot RectangleHotSpot e PolygonHotSpot. Le aree sensibili vengono aggiunti tramite un editor della raccolta in Visual Studio o a livello di programmazione nel codice. Non è disponibile per la creazione di aree sensibili su un'immagine di alcuna interfaccia utente. Le coordinate e dimensioni o il raggio di hotspot deve essere specificato in modo dichiarativo. Non è inoltre disponibile alcuna rappresentazione visiva di un punto attivo nella finestra di progettazione. Se un hotspot è configurato per passare a un URL, l'URL viene specificato tramite la proprietà NavigateUrl dell'area sensibile. Nel caso di un post di eseguire il punto attivo, il PostBackValue proprietà consente di passare una stringa nei postback che può essere recuperata nel codice lato server.


![Editor della raccolta hotSpot in Visual Studio](server-controls/_static/image1.jpg)

**Figura 1**: Editor della raccolta HotSpot in Visual Studio


## <a name="bulletedlist-control"></a>Controllo BulletedList

Il controllo BulletedList è un elenco puntato che può essere facilmente i dati associati. L'elenco può essere ordinato (numerati) o non ordinata tramite la proprietà BulletStyle. Ogni elemento nell'elenco è rappresentato da un oggetto ListItem.


![Controllo BulletedList in Visual Studio](server-controls/_static/image1.gif)

**Figura 2**: controllo BulletedList in Visual Studio


## <a name="hiddenfield-control"></a>Controllo HiddenField

Il controllo HiddenField aggiunge un campo del form nascosto a una pagina, il cui valore è disponibile nel codice lato server. Il valore del campo del form nascosto in genere dovrà rimangono invariate tra postback. Tuttavia, è possibile che un utente malintenzionato può modificare il precedente valore per postback. In questo caso, il controllo HiddenField verrà generato l'evento ValueChanged. Se si dispone di informazioni riservate nel controllo HiddenField e si desidera garantire che rimane invariato, è necessario gestire l'evento ValueChanged nel codice.

## <a name="fileupload-control"></a>Controllo fileUpload

Il controllo FileUpload in ASP.NET 2.0 rende possibile caricare file in un server Web tramite una pagina ASP.NET. Questo controllo è abbastanza simile alla classe ASP.NET 1.x HtmlInputFile con alcune eccezioni. In ASP.NET 1.x, era consigliabile che la proprietà PostedFile sottoporre i valori null per determinare se si disponesse di un file valido. Il controllo FileUpload in ASP.NET 2.0 aggiunge una nuova proprietà HasFile che è possibile usare per lo stesso scopo ed è un po' più efficiente.

La proprietà PostedFile è ancora disponibile per l'accesso a un oggetto HttpPostedFile, ma alcune delle funzionalità del HttpPostedFile è ora disponibile in modo intrinseco con il controllo FileUpload. Ad esempio, per salvare un file caricato in ASP.NET 1.x, si chiama il metodo SaveAs sull'oggetto HttpPostedFile. Utilizzo del controllo FileUpload in ASP.NET 2.0, chiamare il metodo SaveAs sul controllo FileUpload stesso.

Un altro cambiamento significativo nel comportamento 2.0 (e probabilmente il cambiamento più significativo) è che non è più necessario caricare un intero file caricato in memoria prima di salvarlo. Nella versione 1.x, viene salvato tutti i file che è stato caricato completamente in memoria prima della scrittura su disco. Questa architettura consente di evitare il caricamento del file di grandi dimensioni.

In ASP.NET 2.0, l'attributo requestLengthDiskThreshold dell'elemento httpRuntime consente di configurare quanti KB vengono mantenuti in un buffer in memoria prima della scrittura su disco.

**IMPORTANTE**: documentazione di MSDN (e la documentazione in un' posizione) specifica che questo valore è espresso in byte (non in kilobyte) e che il valore predefinito è 256. Il valore viene effettivamente specificato nel kilobyte e il valore predefinito è 80. Con valore predefinito è 80K, si assicura che il buffer non finire sull'heap oggetto grande.

## <a name="wizard-control"></a>Controllo della procedura guidata

È abbastanza comune che si verifichino gli sviluppatori ASP.NET difficoltà durante il tentativo di raccogliere informazioni in una serie di "pagine" usando i pannelli o il trasferimento tra le pagine. Molto spesso, l'attività è frustrante e può richiedere molto tempo. Il nuovo controllo Wizard risolve i problemi, consentendo per i passaggi lineari e lineare in un'interfaccia di procedura guidata che hanno familiari con gli utenti. Controllo della procedura guidata offre moduli di input in una serie di passaggi. Ogni passaggio è di un determinato tipo specificato dalla proprietà StepType del controllo. I tipi di passaggi disponibili sono i seguenti:

| **Tipo di passaggio** | **Spiegazione** |
| --- | --- |
| Auto | La procedura guidata rileva automaticamente il tipo di passaggio in base al relativo posizione all'interno della gerarchia di passaggio. |
| Inizia | Il primo passaggio, spesso usato per presentare un'istruzione introduttiva. |
| Passaggio | Un passaggio normale. |
| Fine | Il passaggio finale, in genere usato per presentare un pulsante per terminare la procedura guidata. |
| Completa | Visualizza un messaggio di comunicazione esito positivo o negativo. |

> [!NOTE]
> Il controllo Wizard tiene traccia di stato con lo stato di controllo ASP.NET. Pertanto, la proprietà EnableViewState può essere impostata su false senza alcun danno.


In questo video è una procedura dettagliata su controllo della procedura guidata.


![](server-controls/_static/image2.png)


[Aprirlo Video a schermo intero](server-controls/_static/wizard1.wmv)


## <a name="localize-control"></a>Localize (controllo)

Il controllo Localize è simile a un controllo Literal. Tuttavia, il controllo Localize ha un **modalità** proprietà che controlla la modalità con cui viene eseguito il rendering di markup che viene aggiunto a esso. La proprietà modalità supporta i valori seguenti:

| **Modalità** | **Spiegazione** |
| --- | --- |
| Transform | Markup viene trasformato secondo il protocollo del browser che effettua la richiesta. |
| Pass-through | Viene eseguito il rendering di markup come-è. |
| Codificare | Il markup che viene aggiunto al controllo è codificato con HtmlEncode. |

## <a name="multiview-and-view-controls"></a>MultiView e controlli di visualizzazione

Il controllo MultiView funge da contenitore per i controlli di visualizzazione e il controllo di visualizzazione funge da contenitore (analogamente a un controllo Panel) per altri controlli. Ogni vista in un controllo MultiView è rappresentato da un singolo controllo di visualizzazione. Il primo controllo di visualizzazione nella finestra di MultiView visualizzazione 0, il secondo è 1, visualizzazione e così via. Puoi cambiare visualizzazioni specificando ActiveViewIndex di MultiView controllo.

## <a name="substitution-control"></a>Controllo Substitution

Il controllo di sostituzione viene utilizzato in combinazione con la memorizzazione nella cache di ASP.NET. Nei casi in cui si desidera sfruttare i vantaggi della memorizzazione nella cache, ma si dispone di parti di una pagina che devono essere aggiornati a ogni richiesta (in altre parole, le parti di una pagina che non sono interessati dalla memorizzazione nella cache), il componente di sostituzione fornisce un'ottima soluzione. Il controllo non esegue effettivamente il rendering di alcun output di per sé. Al contrario, viene associato a un metodo nel codice lato server. Quando viene richiesta la pagina, viene chiamato il metodo e il markup restituito viene eseguito il rendering al posto del controllo di sostituzione.

Il metodo a cui è associato il controllo Substitution viene specificato tramite il **NomeMetodo** proprietà. Tale metodo deve soddisfare i criteri seguenti:

- Deve essere un metodo statico (condiviso in Visual Basic).
- Accetta un parametro di tipo HttpContext.
- Restituisce una stringa che rappresenta il markup che deve sostituire il controllo nella pagina.

Il controllo Substitution non hanno la possibilità di modificare qualsiasi altro controllo nella pagina, ma dispone di accesso per l'oggetto HttpContext corrente tramite il parametro.

## <a name="gridview-control"></a>Controllo GridView

Il controllo GridView è la sostituzione per il controllo DataGrid. Questo controllo verrà trattato più dettagliatamente in un modulo successivo.

## <a name="detailsview-control"></a>Controllo DetailsView

Il controllo DetailsView consente di visualizzare un singolo record da un'origine dati e di modificarlo o eliminarlo. Che viene trattato in modo più dettagliato in un modulo successivo.

## <a name="formview-control"></a>Controllo FormView

Il controllo FormView viene usato per visualizzare un singolo record da un'origine dati in un'interfaccia configurabile. Che viene trattato in modo più dettagliato in un modulo successivo.

## <a name="accessdatasource-control"></a>AccessDataSource (controllo)

Viene usato per il controllo AccessDataSource Associa dati di un database di Access. Che viene trattato in modo più dettagliato in un modulo successivo.

## <a name="objectdatasource-control"></a>Controllo ObjectDataSource

Il controllo ObjectDataSource viene usato per supportare un'architettura a tre livelli, in modo che i controlli possono essere con associazione dati a un oggetto business di livello intermedio anziché un modello a due livelli in cui i controlli vengono associati direttamente all'origine dati. Verrà illustrata in dettaglio in un modulo successivo.

## <a name="xmldatasource-control"></a>Controllo XmlDataSource

Il controllo XmlDataSource viene usato per associare i dati a un'origine dati XML. Che viene trattato in modo più dettagliato in un modulo successivo.

## <a name="sitemapdatasource-control"></a>Controllo SiteMapDataSource

Il controllo SiteMapDataSource fornisce un'associazione di dati per i controlli di spostamento sito basato su una mappa del sito. Verrà illustrata in dettaglio in un modulo successivo.

## <a name="sitemappath-control"></a>Controllo SiteMapPath

Il controllo SiteMapPath Visualizza una serie di collegamenti di navigazione noto come percorsi di navigazione. Che viene trattato in modo più dettagliato in un modulo successivo.

## <a name="menu-control"></a>Controllo Menu

Il controllo Menu consente di visualizzare menu dinamici tramite DHTML. Che viene trattato in modo più dettagliato in un modulo successivo.

## <a name="treeview-control"></a>Controllo TreeView

Consente di visualizzare una visualizzazione struttura ad albero gerarchica dei dati del controllo TreeView. Che viene trattato in modo più dettagliato in un modulo successivo.

## <a name="login-control"></a>Controllo degli accessi

Il controllo di accesso fornisce un meccanismo accedere a un sito Web. Che viene trattato in modo più dettagliato in un modulo successivo.

## <a name="loginview-control"></a>Controllo LoginView

Il controllo LoginView consente la visualizzazione di modelli diversi in base allo stato di accesso dell'utente. Che viene trattato in modo più dettagliato in un modulo successivo.

## <a name="passwordrecovery-control"></a>Controllo PassaggiwordRecovery

Il controllo PassaggiwordRecovery viene usato per recuperare le password dimenticate dagli utenti di un'applicazione ASP.NET. Che viene trattato in modo più dettagliato in un modulo successivo.

## <a name="loginstatus"></a>LoginStatus

Il controllo LoginStatus si visualizza lo stato di accesso dell'utente. Che viene trattato in modo più dettagliato in un modulo successivo.

## <a name="loginname"></a>LoginName

Il controllo LoginName Visualizza un nome utente dopo essere connessi a un'applicazione ASP.NET. Che viene trattato in modo più dettagliato in un modulo successivo.

## <a name="createuserwizard"></a>CreateUserWizard

CreateUserWizard è una procedura guidata configurabile che consente agli utenti la possibilità di creare un account di appartenenza ASP.NET per l'uso in un'applicazione ASP.NET. Che viene trattato in modo più dettagliato in un modulo successivo.

## <a name="changepassword"></a>ChangePassword

Controllo ChangePassword consente agli utenti di modificare la password per un'applicazione ASP.NET. Che viene trattato in modo più dettagliato in un modulo successivo.

## <a name="various-webparts"></a>Web part vari

ASP.NET 2.0 include diverse Web part. Questi verranno descritti nel dettaglio in un modulo successivo.
