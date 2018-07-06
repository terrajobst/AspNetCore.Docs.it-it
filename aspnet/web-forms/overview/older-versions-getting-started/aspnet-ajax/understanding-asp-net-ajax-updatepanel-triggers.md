---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: Informazioni sui trigger UpdatePanel ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Quando si utilizza l'editor di markup in Visual Studio, è possibile notare (da IntelliSense) che sono presenti due elementi figlio di un controllo UpdatePanel. Uno dei eriore a...
ms.author: aspnetcontent
ms.date: 03/12/2008
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: 1b2e70a2b074d3c0a2fa4b669b3645ae5f7f790e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811931"
---
<a name="understanding-aspnet-ajax-updatepanel-triggers"></a>Informazioni sui trigger UpdatePanel ASP.NET AJAX
====================
da [Scott Cate](https://github.com/scottcate)

[Scaricare PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> Quando si utilizza l'editor di markup in Visual Studio, è possibile notare (da IntelliSense) che sono presenti due elementi figlio di un controllo UpdatePanel. Uno dei quali è l'elemento di trigger, che specifica i controlli nella pagina (o il controllo utente, se si usa uno) che attiverà un rendering parziale del controllo UpdatePanel in cui si trova l'elemento.


## <a name="introduction"></a>Introduzione

Tecnologia Microsoft per ASP.NET offre un modello di programmazione orientate a oggetti e guidati da eventi e si unisce i vantaggi del codice compilato. Tuttavia, il relativo modello di elaborazione sul lato server presenta alcuni inconvenienti intrinseche della tecnologia, molti dei quali è possibile risolvere le nuove funzionalità incluse nelle estensioni AJAX di Microsoft ASP.NET 3.5. Queste estensioni consentono molte nuove funzionalità client avanzate, tra cui il rendering parziale delle pagine senza richiedere un aggiornamento completo della pagina, la possibilità di accedere ai servizi Web tramite lo script client (inclusa l'API di profilatura ASP.NET) e un'API lato client estesa progettato per eseguire il mirroring di molti degli schemi di controllo visualizzati nell'insieme di controlli lato server ASP.NET.

Questo white paper vengono esaminate le funzionalità di trigger XML di ASP.NET AJAX `UpdatePanel` componente. I trigger XML offrono un controllo granulare i componenti che possono causare il rendering parziale per i controlli UpdatePanel specifici.

Questo white paper si basa sulla versione Beta 2 di .NET Framework 3.5 e Visual Studio 2008. ASP.NET AJAX Extensions, in precedenza un assembly di componenti aggiuntivi destinato a ASP.NET 2.0, adesso sono integrate nella libreria di classi .NET Framework Base. Questo white paper si presuppone inoltre che si userà Visual Studio 2008, non Visual Web Developer Express e forniscono procedure dettagliate in base all'interfaccia utente di Visual Studio (sebbene siano completamente compatibile indipendentemente listati di codice ambiente di sviluppo).

## <a name="triggers"></a>*Trigger*

I trigger per un determinato UpdatePanel, per impostazione predefinita, includono automaticamente tutti i controlli figlio che richiamano un postback, tra cui, ad esempio, i controlli casella di testo con loro `AutoPostBack` impostata su **true**. Tuttavia, i trigger possono essere inclusi in modo dichiarativo utilizzando markup. Questa operazione viene eseguita all'interno di `<triggers>` sezione della dichiarazione del controllo UpdatePanel. Anche se i trigger sono accessibili tramite il `Triggers` proprietà della raccolta, è consigliabile registrare qualsiasi trigger di rendering parziale in fase di esecuzione (ad esempio, se un controllo non è disponibile in fase di progettazione) usando il `RegisterAsyncPostBackControl(Control)` metodo per il ScriptManager dell'oggetto per la pagina, all'interno di `Page_Load` evento. Tenere presente che le pagine sono senza state e pertanto è necessario registrare di nuovo questi controlli ogni volta che vengono creati.

Inclusione di trigger automatici figlio può anche essere disabilitata (in modo che i controlli figlio che crea i postback non attivano automaticamente esegue il rendering parziale) impostando il `ChildrenAsTriggers` proprietà **false**. Ciò consente la massima flessibilità nell'assegnazione di controlli specifici che può richiamare un rendering di pagina ed è consigliata, in modo che uno sviluppatore sarà acconsenti esplicitamente per rispondere a un evento, anziché gestire tutti gli eventi che potrebbero verificarsi.

Si noti che quando vengono annidati i controlli UpdatePanel, quando il UpdateMode è impostata su **condizionale**, se l'elemento figlio UpdatePanel viene attivata, ma l'elemento padre, non è quindi solo l'elemento figlio UpdatePanel verrà aggiornato. Tuttavia, se l'elemento padre UpdatePanel viene aggiornato, quindi l'elemento figlio UpdatePanel anche verranno aggiornato.

## <a name="the-lttriggersgt-element"></a>*Il &lt;trigger&gt; elemento*

Quando si utilizza l'editor di markup in Visual Studio, è possibile notare (da IntelliSense) sono presenti due elementi figlio di un `UpdatePanel` controllo. L'elemento più frequenti è il `<ContentTemplate>` elemento, che essenzialmente incapsula il contenuto che verrà mantenuto dal Pannello di aggiornamento (il contenuto per i quali verrà abilitata per il rendering parziale). L'altro elemento è il `<Triggers>` elemento che specifica i controlli nella pagina (o il controllo utente, se si usa uno) che attiverà un rendering parziale del controllo UpdatePanel in cui la &lt;trigger&gt; contiene l'elemento.

Il `<Triggers>` elemento può contenere qualsiasi numero ogni di due nodi figlio: `<asp:AsyncPostBackTrigger>` e `<asp:PostBackTrigger>`. Entrambe accettano due attributi `ControlID` e `EventName`e può specificare qualsiasi controllo all'interno dell'unità corrente di incapsulamento (ad esempio, se il controllo UpdatePanel si trova all'interno di un controllo utente Web, è consigliabile non tentare fa riferimento a un controllo su la pagina in cui risiederà il controllo utente).

Il `<asp:AsyncPostBackTrigger>` elemento è particolarmente utile in quanto possono essere indirizzati a qualsiasi evento da un controllo che esiste come elemento figlio *qualsiasi* controllo UpdatePanel in unità di incapsulamento, non solo UpdatePanel in cui il trigger è un elemento figlio . Di conseguenza, qualsiasi controllo può essere reso per attivare un aggiornamento parziale della pagina.

Analogamente, il `<asp:PostBackTrigger>` elemento può essere usato per eseguire il rendering di una pagina parziale di trigger, ma che richiede un accesso completo al server. Questo elemento di trigger può essere utilizzato anche per forzare un rendering di pagina completo quando un controllo in genere in caso contrario, attiva un rendering parziale della pagina (ad esempio, quando un `Button` esiste nel controllo di `<ContentTemplate>` elemento di un controllo UpdatePanel). Anche in questo caso, l'elemento PostBackTrigger può specificare qualsiasi controllo figlio di qualsiasi controllo UpdatePanel nell'unità corrente di incapsulamento.

## <a name="lttriggersgt-element-reference"></a>*&lt;I trigger&gt; riferimento all'elemento*

*Discendenti di markup:*

| **Tag** | **Descrizione** |
| --- | --- |
| &lt;asp:AsyncPostBackTrigger&gt; | Specifica un controllo e l'evento che causa un aggiornamento parziale della pagina per UpdatePanel che contiene il riferimento di trigger. |
| &lt;asp:PostBackTrigger&gt; | Specifica un controllo e l'evento che causa un aggiornamento completo della pagina (un aggiornamento completo della pagina). Questo tag può essere utilizzato per forzare un aggiornamento completo quando un controllo attiva in caso contrario, il rendering parziale. |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*Procedura dettagliata: Cross-UpdatePanel trigger*

1. Creare una nuova pagina ASP.NET con un oggetto di ScriptManager impostato per attivare il rendering parziale. Aggiungere due UpdatePanel in questa pagina - nel primo, includere un controllo Label (Label1) e due controlli Button (Button1 e Button2). Button1 deve ad esempio fare clic per aggiornare entrambi e Button2 dovrebbe essere fare clic per aggiornare questo o qualcosa. In secondo UpdatePanel, includere solo un controllo Label (Label2), ma impostare la relativa proprietà ForeColor su un valore diverso da quello predefinito per distinguerli.
2. Impostare la proprietà UpdateMode di entrambi i tag per UpdatePanel **condizionale**.

**Listato 1: Markup per default. aspx:** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. Nel gestore eventi Click per Button1, impostare Label1.Text e Label2.Text a un elemento dipendente dal tempo (ad esempio DateTime.Now.ToLongTimeString()). Per il gestore eventi Click di Button2, impostare Label1.Text solo sul valore dipendenti dal tempo.

**Listato 2: Codebehind (tagliati) nel file default.aspx.cs:** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. Premere F5 per compilare ed eseguire il progetto. Si noti che, quando si fa clic pannelli di aggiornamento sia, entrambe le etichette modificare il testo. Tuttavia, quando si fa clic sul riquadro di questo aggiornamento, Label1 solo per gli aggiornamenti.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))


## <a name="under-the-hood"></a>*Dietro le quinte*

Usando l'esempio che sono appena costruite, è possibile dare un'occhiata svolte da ASP.NET AJAX e come funzionano i trigger UpdatePanel di cross-pannello. A tale scopo, Microsoft collaborerà con l'origine genera la pagina HTML, nonché l'estensione di Mozilla Firefox chiamato FireBug - con esso, è possibile esaminare con facilità i postback AJAX. Si userà anche strumento .NET Reflector di Lutz Roeder. Entrambi questi strumenti sono disponibili gratuitamente online e possono essere trovati con una ricerca in internet.

Un esame del codice sorgente della pagina Mostra quasi nulla fuori dell'ordinario; i controlli UpdatePanel vengono visualizzati come `<div>` contenitori e si noterà la risorsa di script include fornito dal `<asp:ScriptManager>`. Esistono anche alcune chiamate AJAX specifico nuovo da PageRequestManager che sono all'interno della libreria di script client AJAX. Infine, noteremo i due contenitori UpdatePanel - uno con il rendering `<input>` pulsanti con i due `<asp:Label>` sottoposto a rendering come controlli `<span>` contenitori. (Se si analizza l'albero DOM nonché di apportare modifiche, si noterà che le etichette vengono visualizzati in grigio per indicare che non producono il contenuto visibile).

Fare clic sul pulsante di aggiornamento questo pannello e notare che UpdatePanel superiore verrà aggiornato con l'ora corrente del server. Nonché di apportare modifiche, scegliere la scheda della Console, in modo da poter esaminare la richiesta. Esaminare innanzitutto i parametri della richiesta POST:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))


Si noti che UpdatePanel ha indicato al codice AJAX lato server con precisione quali struttura dei controlli è stata generata tramite il parametro ScriptManager1: `Button1` del `UpdatePanel1` controllo. A questo punto, fare clic sul pulsante con pannelli di aggiornamento. Quindi, eseguire l'analisi della risposta, è visibile una serie delimitato da barre verticali di variabili impostate in una stringa. in particolare, noteremo UpdatePanel superiore, `UpdatePanel1`, ha l'intero codice relativo HTML inviato al browser. La libreria di script client AJAX sostituisce HTML originale di UpdatePanel contenuto con il nuovo contenuto tramite il `.innerHTML` proprietà, e in modo che il server invia il contenuto modificato dal server in formato HTML.

A questo punto, fare clic sul pulsante con pannelli di aggiornamento sia ed esaminare i risultati dal server. I risultati sono molto simile: entrambi gli UpdatePanel ricevano nuova pagina HTML dal server. Come con il callback precedente, lo stato di pagine aggiuntive viene inviato.

Come si può notare, in quanto non viene usato alcun codice speciale per eseguire un postback AJAX, la libreria di script client AJAX è in grado di intercettare i postback form senza alcun codice aggiuntivo. I controlli server utilizzano automaticamente JavaScript in modo che non vengono automaticamente inviare il modulo - ASP.NET inserisce automaticamente il codice per la convalida e stato già fatto, principalmente a tale scopo inclusione risorse script automatico, la classe PostBackOptions e la classe ClientScriptManager.

Si consideri ad esempio un controllo casella di controllo. Esaminare il disassembly di classe in .NET Reflector. A tale scopo, verificare che l'assembly System. Web sia aperta e passare al `System.Web.UI.WebControls.CheckBox` classe, aprire il `RenderInputTag` (metodo). Cercare un'istruzione condizionale che controlla il `AutoPostBack` proprietà:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))


Quando postback automatico è abilitato in un `CheckBox` controllo (tramite la proprietà AutoPostBack che è true), il risultante `<input>` tag viene pertanto eseguito il rendering e la gestione degli script in un evento ASP.NET relativo `onclick` attributo. L'intercettazione dell'invio del form, quindi, consente di ASP.NET AJAX venga inserito nella pagina nonintrusively, aiutano a evitare qualsiasi rischio di modifiche che potrebbero verificarsi utilizzando una sostituzione di stringa possibilmente imprecisa di rilievo. Inoltre, in questo modo *qualsiasi* controllo ASP.NET personalizzato per utilizzare le potenzialità di ASP.NET AJAX senza alcun codice aggiuntivo per supportare l'uso all'interno di un contenitore di UpdatePanel.

Il `<triggers>` funzionalità corrisponde ai valori inizializzati nella chiamata a PageRequestManager \_updateControls (si noti che la libreria di script client ASP.NET AJAX utilizza la convenzione che metodi, eventi e i nomi dei campi che iniziano con un carattere di sottolineatura sono contrassegnate come interne e non sono progettate per l'utilizzo di fuori della stessa libreria). Con esso, è possibile osservare quali controlli sono progettati per fare in modo i postback AJAX.

Ad esempio, aggiungiamo altri due controlli alla pagina, lasciando un controllo all'esterno di UpdatePanel interamente e uscire da uno all'interno di un controllo UpdatePanel. Si verrà aggiungere una casella di controllo all'interno di UpdatePanel superiore e rilasciare un controllo DropDownList con un numero di colori definiti all'interno dell'elenco. Ecco il nuovo tag:

**Listato 3: Nuovo tag**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

Ed ecco il nuovo code-behind:

**Listato 4: Codebehind**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

L'idea alla base di questa pagina è che la casella di controllo determina se è in grassetto, sia se le etichette visualizzate la data, nonché l'ora, che consente di selezionare l'elenco a discesa scegliere una delle tre colori per mostrare la seconda etichetta. La casella di controllo non dovrebbero provocare un aggiornamento di AJAX, ma l'elenco a discesa dovrebbe, anche se non si trova all'interno di un controllo UpdatePanel.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([Fare clic per visualizzare l'immagine con dimensioni normali](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))


Come è evidente nello screenshot precedente, il pulsante più recente per essere selezionato è stato l'aggiornamento questo pannello, che aggiornato il database indipendentemente dal tempo superiore del tempo nella parte inferiore del pulsante destro. La data è stata inoltre disattivata tra i clic, la data è visibile nell'etichetta nella parte inferiore. Infine è il colore dell'etichetta nella parte inferiore di interesse: è stata aggiornata più recentemente rispetto al testo dell'etichetta, che dimostra che lo stato del controllo è importante, e gli utenti si aspettano che deve essere mantenuto attraverso i postback AJAX. *Tuttavia*, l'ora non è stato aggiornato. Il tempo è stato ripopolato automaticamente tramite la persistenza dei \_ \_campo VIEWSTATE della pagina venga interpretata dal runtime ASP.NET durante il controllo è stato nuovamente eseguito il rendering nel server. Il codice server ASP.NET AJAX non riconosce in cui i metodi i controlli di modifica stato; viene semplicemente ripopola dallo stato di visualizzazione e si esegue quindi gli eventi appropriati.

È opportuno ricordare, tuttavia, che avevo è inizializzato il tempo all'interno della pagina\_evento di caricamento, il tempo potrebbe avere stato incrementato correttamente. Di conseguenza, gli sviluppatori devono essere ben consapevoli che il codice appropriato è in esecuzione durante i gestori eventi appropriati ed evitare l'uso della pagina\_caricare quando un gestore di eventi di controllo potrebbe essere indicato.

## <a name="summary"></a>Riepilogo

Il controllo UpdatePanel di ASP.NET AJAX Extensions è versatile e può usare numerosi metodi per l'identificazione di eventi di controllo che devono indurla da aggiornare. Supporta l'aggiornamento automatico dai relativi controlli figlio, ma può anche rispondere agli eventi di controllo in un' posizione nella pagina.

Per ridurre il potenziale per il carico di elaborazione di server, è consigliabile che il `ChildrenAsTriggers` impostare proprietà di un UpdatePanel `false`, e che gli eventi di essere scelto in anziché incluso per impostazione predefinita. Ciò impedisce inoltre tutti gli eventi non necessari di provocare effetti potenzialmente indesiderati, incluse la convalida e le modifiche apportate ai campi di input. Questi tipi di bug possono essere difficili da localizzare, perché la pagina si aggiorna in modo trasparente all'utente e la causa potrebbe pertanto non essere immediatamente evidente.

Esaminando i meccanismi interni del modulo ASP.NET AJAX dopo il modello di intercettazione, siamo in grado di determinare che usa il framework già fornito da ASP.NET. In questo modo, conserva la massima compatibilità con i controlli progettata utilizzando lo stesso framework e accede minima su qualsiasi aggiuntive JavaScript scritta per la pagina.

## <a name="bio"></a>Informazioni biografiche

Rob Paveza è uno sviluppatore di applicazioni .NET senior presso Terralever ([www.terralever.com](http://www.terralever.com)), una società di marketing interattiva iniziale in Tempe, AZ È possibile contattarlo al [ robpaveza@gmail.com ](mailto:robpaveza@gmail.com), e il suo blog è disponibile all'indirizzo [ http://geekswithblogs.net/robp/ ](http://geekswithblogs.net/robp/).

Scott Cate ha collaborato con tecnologie Web di Microsoft dal 1997 ed è il presidente di myKB.com ([www.myKB.com](http://www.myKB.com)) ed è specializzato nella scrittura di ASP.NET basato su applicazioni incentrate su soluzioni Software di Knowledge Base. È possibile contattare Scott tramite posta elettronica all'indirizzo [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) o il suo blog all'indirizzo [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Precedente](understanding-partial-page-updates-with-asp-net-ajax.md)
> [Successivo](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
