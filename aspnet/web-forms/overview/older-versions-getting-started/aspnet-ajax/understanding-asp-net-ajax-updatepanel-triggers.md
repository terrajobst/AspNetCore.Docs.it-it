---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: Informazioni sui trigger UpdatePanel ASP.NET AJAX | Documenti Microsoft
author: scottcate
description: Quando si utilizza l'editor di codice in Visual Studio, è possibile notare (da IntelliSense) che sono presenti due elementi figlio di un controllo UpdatePanel. Uno dei CO...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2008
ms.topic: article
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: f30f2ead402d2f49a89b2caf47cc30b6445d4cfb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890749"
---
<a name="understanding-aspnet-ajax-updatepanel-triggers"></a>Informazioni sui trigger UpdatePanel ASP.NET AJAX
====================
da [Scott categorie](https://github.com/scottcate)

[Scarica il PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> Quando si utilizza l'editor di codice in Visual Studio, è possibile notare (da IntelliSense) che sono presenti due elementi figlio di un controllo UpdatePanel. Uno dei quali è l'elemento di trigger, che specifica i controlli nella pagina (o il controllo utente, se si utilizza uno) che attiverà un rendering parziale del controllo UpdatePanel in cui si trova l'elemento.


## <a name="introduction"></a>Introduzione

Tecnologia Microsoft per ASP.NET offre un modello di programmazione orientata a oggetti e sugli eventi e si unisce i vantaggi del codice compilato. Tuttavia, il relativo modello di elaborazione sul lato server presenta numerosi svantaggi inerenti la tecnologia, molti dei quali è possibile risolvere le nuove funzionalità incluse in Microsoft ASP.NET 3.5 AJAX Extensions. Queste estensioni consentono a numerose nuove funzionalità rich client, inclusi il rendering parziale di pagine senza richiedere un aggiornamento completo di pagina, la possibilità di accedere ai servizi Web tramite lo script client (inclusi l'API di profilatura di ASP.NET) e un'API lato client estesa progettato per eseguire il mirroring molti degli schemi di controllo mostrati nel set di controllo sul lato server ASP.NET.

Questo white paper esamina la funzionalità XML trigger di ASP.NET AJAX `UpdatePanel` componente. Trigger XML offrono un controllo granulare i componenti che possono causare il rendering parziale per i controlli UpdatePanel specifici.

Questo white paper è basato sulla versione Beta 2 di .NET Framework 3.5 e Visual Studio 2008. Estensioni AJAX di ASP.NET, in precedenza un assembly di componenti aggiuntivi destinato a ASP.NET 2.0, ora sono integrate nella libreria di classe di Base di .NET Framework. Questo white paper anche presuppone che sia necessario lavorare con Visual Studio 2008, non Visual Web Developer Express e verrà illustrate le procedure dettagliate in base all'interfaccia utente di Visual Studio (sebbene listati di codice sarà completamente compatibile, indipendentemente dal valore ambiente di sviluppo).

## <a name="triggers"></a>*Trigger*

I trigger per un determinato UpdatePanel, per impostazione predefinita, includono automaticamente eventuali controlli figlio che richiamano un postback, inclusi, ad esempio, i controlli casella di testo contenenti le `AutoPostBack` proprietà impostata su **true**. Tuttavia, i trigger possono essere inclusi in modo dichiarativo utilizzando markup. Questa operazione viene eseguita all'interno di `<triggers>` sezione della dichiarazione controllo UpdatePanel. Anche se i trigger sono accessibili tramite il `Triggers` proprietà della raccolta, è consigliabile registrare i trigger di rendering parziale in fase di esecuzione (ad esempio, se un controllo non è disponibile in fase di progettazione) utilizzando il `RegisterAsyncPostBackControl(Control)` metodo il ScriptManager oggetto all'interno della pagina, il `Page_Load` evento. Tenere presente che le pagine sono senza state e pertanto è necessario registrare nuovamente questi controlli ogni volta che vengono creati.

Inclusione trigger figlio automatica può anche essere disabilitato (in modo che i controlli figlio che crea i postback non verrà automaticamente avviato il rendering parziale) impostando il `ChildrenAsTriggers` proprietà **false**. Ciò permette la massima flessibilità nell'assegnazione dei controlli specifici può richiamare un rendering della pagina ed è consigliata, in modo che uno sviluppatore verrà registrarsi per rispondere a un evento, anziché gestire gli eventi che possono verificarsi.

Si noti che quando sono annidati controlli UpdatePanel, quando il UpdateMode è impostato su **condizionale**, se l'elemento figlio UpdatePanel viene attivato, ma l'elemento padre, non è quindi figlio UpdatePanel verrà aggiornato. Tuttavia, se l'elemento padre UpdatePanel viene aggiornato, quindi l'elemento figlio UpdatePanel anche verranno aggiornato.

## <a name="the-lttriggersgt-element"></a>*Il &lt;trigger&gt; elemento*

Quando si utilizza l'editor di codice in Visual Studio, è possibile notare (da IntelliSense) che sono presenti due elementi figlio di un `UpdatePanel` controllo. L'elemento più frequenti è il `<ContentTemplate>` elemento, che essenzialmente incapsula il contenuto che verrà mantenuto dal Pannello di aggiornamento (il contenuto per cui si sta abilitazione per il rendering parziale). L'altro elemento è il `<Triggers>` elemento che specifica i controlli nella pagina (o il controllo utente, se si utilizza uno) che attiverà un rendering parziale del controllo UpdatePanel in cui il &lt;trigger&gt; contiene l'elemento.

Il `<Triggers>` elemento può contenere qualsiasi numero che ogni due nodi figlio: `<asp:AsyncPostBackTrigger>` e `<asp:PostBackTrigger>`. Accettano due attributi, `ControlID` e `EventName`e può specificare qualsiasi controllo all'interno dell'unità corrente di incapsulamento (ad esempio, se il controllo UpdatePanel si trova all'interno di un controllo utente Web, è consigliabile non tentare di fare riferimento a un controllo in la pagina in cui risiederà il controllo utente).

Il `<asp:AsyncPostBackTrigger>` elemento risulta particolarmente utile in quanto possono essere indirizzati a qualsiasi evento di un controllo che esiste come elemento figlio di *qualsiasi* controllo UpdatePanel nell'unità di incapsulamento, non solo l'UpdatePanel con cui il trigger è un elemento figlio . Di conseguenza, è possibile eseguire alcun controllo per attivare un aggiornamento parziale della pagina.

Analogamente, il `<asp:PostBackTrigger>` elemento può essere usato per i trigger, eseguire il rendering di una pagina parziale, ma che richiede un accesso completo al server. Questo elemento di trigger può essere usato anche per forzare un rendering a pagina intera quando un controllo in caso contrario genera normalmente un rendering a pagina parziale (ad esempio, quando un `Button` esiste nel controllo di `<ContentTemplate>` elemento di un controllo UpdatePanel). Nuovamente, l'elemento PostBackTrigger è possibile specificare qualsiasi controllo che è un figlio di qualsiasi controllo UpdatePanel nell'unità corrente di incapsulamento.

## <a name="lttriggersgt-element-reference"></a>*&lt;I trigger&gt; riferimento all'elemento*

*Markup discendenti:*

| **tag** | **Descrizione** |
| --- | --- |
| &lt;asp:AsyncPostBackTrigger&gt; | Specifica un controllo e l'evento che causa un aggiornamento parziale della pagina per l'UpdatePanel che contiene un riferimento al trigger. |
| &lt;asp:PostBackTrigger&gt; | Specifica un controllo e l'evento che causa un aggiornamento completo di pagina (un aggiornamento a pagina intera). Questo tag può essere utilizzato per forzare un aggiornamento completo quando un controllo in caso contrario verrà generato per il rendering parziale. |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*Procedura dettagliata: UpdatePanel tra trigger*

1. Creare una nuova pagina ASP.NET con un oggetto di ScriptManager impostata per attivare il rendering parziale. Aggiungere due UpdatePanel a questa pagina, nel primo, includere un controllo etichetta (Label1) e due controlli Button (Button1 e Button2). Button1 deve indicare fare clic per aggiornare i valori e Button2 deve indicare fare clic per aggiornare questo o un valore lungo le righe. In secondo UpdatePanel, includere solo un controllo etichetta (Label2), ma impostare la proprietà ForeColor su un valore diverso da quello predefinito per distinguerli.
2. Impostare la proprietà UpdateMode di entrambi i tag per UpdatePanel **condizionale**.

**Elenco 1: Markup per default. aspx:** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. Nel gestore dell'evento Click per Button1, impostare Label1.Text e Label2.Text qualcosa dipendenti dal tempo (ad esempio DateTime.Now.ToLongTimeString()). Per il gestore eventi Click per Button2, impostare Label1.Text solo il valore di dipendenti dal tempo.

**Elenco 2: Codebehind (tagliati) in default.aspx.cs:** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. Premere F5 per compilare ed eseguire il progetto. Si noti che, quando si fa clic su aggiornamento sia pannelli, entrambe le etichette modificare testo. Tuttavia, quando si fa clic su Pannello di questo aggiornamento, Label1 solo per gli aggiornamenti.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([Fare clic per visualizzare l'immagine ingrandita](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))


## <a name="under-the-hood"></a>*Dietro le quinte*

Utilizzando l'esempio che è appena costruite, è possibile esaminare svolte da ASP.NET AJAX e del funzionamento di questo trigger cross-pannello UpdatePanel. A tale scopo, si opererà con il codice generato pagina HTML, nonché l'estensione di Mozilla Firefox chiamato FireBug - con esso, è possibile esaminare facilmente i postback AJAX. Utilizziamo inoltre lo strumento .NET Reflector da Lutz Roeder. Entrambi gli strumenti sono disponibili gratuitamente e può essere trovati con una ricerca in internet.

Un esame del codice sorgente della pagina Mostra quasi alcuna; i controlli UpdatePanel vengono visualizzati come `<div>` contenitori ed è possibile osservare la risorsa di script include fornito per il `<asp:ScriptManager>`. Esistono inoltre alcune nuove chiamate specifico per AJAX per PageRequestManager che sono all'interno della libreria di script client AJAX. Infine, vedremo i due contenitori UpdatePanel - uno con il rendering `<input>` pulsanti con le due `<asp:Label>` controlli sottoposti a rendering come `<span>` contenitori. (Se si analizza l'albero del DOM in FireBug, si noterà che le etichette vengono visualizzate in grigio per indicare che non producono il contenuto visibile).

Fare clic sul pulsante Aggiorna il pannello e notare che superiore UpdatePanel verrà aggiornato con l'ora del server corrente. Apportare modifiche, scegliere la scheda Console in modo che è possibile esaminare la richiesta. Prima di verificare i parametri della richiesta POST:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([Fare clic per visualizzare l'immagine ingrandita](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))


Si noti che l'UpdatePanel ha indicato al codice AJAX lato server con precisione quali struttura ad albero di controllo è stato generato tramite il parametro ScriptManager1: `Button1` del `UpdatePanel1` controllo. A questo punto, fare clic sul pulsante di aggiornamento sia pannelli. Quindi, esaminando la risposta, si vede una serie delimitati da pipe di variabili impostate in una stringa. in particolare, è possibile notare l'UpdatePanel superiore, `UpdatePanel1`, con l'intera relativo HTML inviata al browser. La libreria di script client AJAX sostituisce HTML originale dell'UpdatePanel contenuto con il nuovo contenuto tramite il `.innerHTML` proprietà, quindi il server invia il contenuto modificato dal server in formato HTML.

A questo punto, fare clic sul pulsante di aggiornamento sia pannelli ed esaminare i risultati dal server. I risultati sono molto simili, entrambi UpdatePanel ricevere nuova pagina HTML del server. Come con il callback precedente, lo stato di pagine aggiuntive viene inviato.

Come possiamo vedere, perché nessun codice speciale viene utilizzato per eseguire un postback AJAX, la libreria di script client AJAX è in grado di intercettare i postback modulo senza codice aggiuntivo. I controlli server utilizzano JavaScript automaticamente in modo da non inviare automaticamente il form, ASP.NET inserisce automaticamente il codice per la convalida e stato già fatto, ottenuto principalmente con l'inclusione di risorse script automatico, la classe PostBackOptions e la classe ClientScriptManager.

Ad esempio, si consideri un controllo casella di controllo. Esaminare il disassembly di classe in Reflector .NET. A tale scopo, verificare che l'assembly System. Web sia aperta e passare al `System.Web.UI.WebControls.CheckBox` (classe), apertura di `RenderInputTag` metodo. Cercare un'istruzione condizionale che controlla il `AutoPostBack` proprietà:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([Fare clic per visualizzare l'immagine ingrandita](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))


Quando un postback automatico è abilitato in un `CheckBox` controllare (tramite un postback automatico proprietà che è true), i risultanti `<input>` tag viene pertanto eseguito il rendering e gestione degli script in un evento ASP.NET relativo `onclick` attributo. L'intercettazione dell'invio del form, quindi consente ASP.NET AJAX essere inseriti nella pagina nonintrusively, contribuendo ad per evitare qualsiasi potenziale ultime modifiche che potrebbero verificarsi utilizzando una sostituzione di stringa probabilmente imprecisa. Inoltre, in questo modo *qualsiasi* controllo ASP.NET personalizzato per utilizzare le funzioni di ASP.NET AJAX senza codice aggiuntivo per supportare l'utilizzo all'interno di un contenitore di UpdatePanel.

Il `<triggers>` funzionalità corrisponde ai valori inizializzati nella chiamata a PageRequestManager \_updateControls (si noti che la libreria di script client AJAX ASP.NET utilizza la convenzione di metodi, eventi e i nomi dei campi che iniziano con un carattere di sottolineatura sono contrassegnati come interni e non sono destinate all'uso all'esterno della libreria). Con, è possibile osservare i controlli che servono per provocano postback AJAX.

Ad esempio, aggiungere due controlli aggiuntivi per la pagina, lasciando un controllo all'esterno di UpdatePanel interamente e in uscita da uno all'interno di un UpdatePanel. Si verrà aggiungere un controllo casella di controllo all'interno di UpdatePanel superiore e rilasciare un DropDownList con un numero di colori definiti all'interno dell'elenco. Ecco il nuovo tag:

**Elenco di 3: Nuovo Markup**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

Di seguito è il nuovo code-behind:

**Listato 4: Codebehind**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

L'idea alla base di questa pagina è che l'elenco a discesa Seleziona uno dei tre colori per mostrare la seconda etichetta, che la casella di controllo determina se è in grassetto, sia se le etichette di visualizzano la data, nonché l'ora. La casella di controllo non dovrebbe causare un aggiornamento AJAX, ma è necessario l'elenco a discesa, anche se non si trova all'interno di un UpdatePanel.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([Fare clic per visualizzare l'immagine ingrandita](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))


Come è evidente nella schermata precedente, il pulsante più recente per essere selezionato è il pulsante destro del mouse pannello, questo aggiornamento aggiornato il database indipendentemente dal tempo superiore del tempo nella parte inferiore. La data è stata inoltre disattivata tra i clic, come la data è visibile nell'etichetta inferiore. Infine è il colore dell'etichetta nella parte inferiore di interesse: sia stato aggiornato più recentemente rispetto al testo dell'etichetta, che dimostra che lo stato del controllo è importante, e gli utenti si aspettano che deve essere mantenuto tramite postback AJAX. *Tuttavia*, l'ora non è stato aggiornato. Il tempo è stato ricompilato automaticamente tramite la persistenza del \_ \_campo VIEWSTATE della pagina venga interpretata dal runtime ASP.NET quando il controllo è stato eseguito il rendering nel server. Il codice server ASP.NET AJAX non riconosce i cui metodi i controlli di modifica stato; semplicemente ripopola dallo stato di visualizzazione e quindi esegue gli eventi appropriati.

È opportuno ricordare, tuttavia, che siano stati inizializzato il tempo all'interno della pagina\_evento di caricamento, l'ora sarebbe avere stato incrementato correttamente. Di conseguenza, gli sviluppatori devono fare attenzione che il codice appropriato è in esecuzione durante i gestori eventi appropriati ed evitare l'utilizzo della pagina\_caricare quando un gestore eventi di controllo sarebbe appropriato.

## <a name="summary"></a>Riepilogo

Il controllo UpdatePanel Estensioni AJAX di ASP.NET è versatile e può utilizzare diversi metodi per l'identificazione di eventi di controllo dovrebbero impedirne l'aggiornamento. Supporta verranno aggiornati automaticamente dai relativi controlli figlio, ma può anche rispondere agli eventi di controllo in un' posizione nella pagina.

Per ridurre il potenziale per il carico di elaborazione di server, è consigliabile che il `ChildrenAsTriggers` impostare proprietà di un UpdatePanel `false`, e che gli eventi scelto-nel anziché inclusi per impostazione predefinita. Ciò impedisce inoltre gli eventi non necessari causando potenzialmente indesiderati, incluse la convalida e le modifiche apportate ai campi di input. Questi tipi di bug potrebbero essere difficili isolare, perché la pagina Aggiorna in modo trasparente all'utente e la causa potrebbe pertanto non essere immediatamente evidente.

Esaminando il funzionamento interno del form di ASP.NET AJAX, registrare il modello di intercettazione, siamo in grado di determinare che utilizza il framework già fornito da ASP.NET. In questo modo, mantiene la massima compatibilità con i controlli progettato utilizzando lo stesso framework e intrusione minima su qualsiasi aggiuntive JavaScript scritta per la pagina.

## <a name="bio"></a>Bio

Rob Paveza è uno sviluppatore di applicazioni .NET senior presso Terralever ([www.terralever.com](http://www.terralever.com)), una società di marketing interattiva iniziale in Tempe, AZ Egli può essere raggiunto al [ robpaveza@gmail.com ](mailto:robpaveza@gmail.com), e si trova al suo blog [ http://geekswithblogs.net/robp/ ](http://geekswithblogs.net/robp/).

Categoria Scott lavora con tecnologie Web di Microsoft dal 1997 ed è il vicepresidente myKB.com ([www.myKB.com](http://www.myKB.com)) in cui si è specializzato nella scrittura ASP.NET basato su applicazioni con stato attivo sulle soluzioni Software Knowledge Base. Scott possano essere contattati tramite posta elettronica al [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) o il suo blog all'indirizzo [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Precedente](understanding-partial-page-updates-with-asp-net-ajax.md)
> [Successivo](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
