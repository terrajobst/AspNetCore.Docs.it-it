---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Riutilizzo dell'interfaccia utente tramite pagine Master e visualizzazioni parziali | Microsoft Docs
author: microsoft
description: Passaggio 7 esamina i metodi che è possibile applicare il principio DRY entro i modelli di vista per eliminare la duplicazione del codice, usando modelli di visualizzazione parziale e le pagine master.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: 0da32e6ac38f10df6e581517989b3b1fd2f2328c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828096"
---
<a name="re-use-ui-using-master-pages-and-partials"></a>Riutilizzo dell'interfaccia utente tramite pagine Master e visualizzazioni parziali
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Si tratta di passaggio 7 di una liberazione [esercitazione sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-dettaglio come compilare una piccola, ma completa, applicazione web con ASP.NET MVC 1.
> 
> Passaggio 7 vengono esaminati modi che sevice del "principio DRY" all'interno dei nostri modelli di vista per eliminare la duplicazione del codice, usando modelli di visualizzazione parziale e le pagine master.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le [Guida introduttiva con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oppure [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner passaggio 7: Righe parzialmente eseguite e pagine Master

Una delle filosofie di progettazione che ASP.NET MVC implementa è il principio di "Eseguire operazioni non ' t Repeat Yourself" (noto come "Prova"). Una progettazione DRY consente di eliminare la duplicazione del codice e per la logica, che in definitiva rende più veloce per compilare e maggiore facile di gestione di applicazioni.

Abbiamo già visto il principio DRY applicato in molti gli scenari di NerdDinner. Alcuni esempi: entro il livello del modello, in modo da essere applicato a entrambe le modifica e creare scenari nel nostro controller; viene implementata la logica di convalida Utilizziamo nuovamente il modello di vista "NotFound" tra i metodi di azione di modifica, dettagli e l'eliminazione; si sta usando un modello di denominazione convenzione - con i modelli di visualizzazione, che elimina la necessità di specificare in modo esplicito il nome quando si chiama il metodo helper View(); e viene nuovamente Usa la classe DinnerFormViewModel per entrambe le modifica e creare scenari di azione.

Verranno ora esaminate modi che sevice del "principio DRY" all'interno dei nostri modelli di vista per eliminare la duplicazione del codice sono anche.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Riesaminare la modifica e creazione di modelli di visualizzazione

Utilizziamo attualmente due modelli di visualizzazione diverse: "Edit" e "Tornarci": per visualizzare il form Dinner dell'interfaccia utente. Un confronto visivo rapido di essi viene evidenziata la similitudine sono. Ecco come appare il modulo di creazione:

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

Ed ecco come appare il modulo "Modifica":

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

Non molto una differenza significativa è presente? Diversi dal testo del titolo e l'intestazione, i controlli di layout e l'input del form sono identici.

Se si apre il "Edit" e "Tornarci" modelli di vista, che si noterà che contengono codice di controllo di layout e l'input identici form. Questa duplicazione significa che si finisce la necessità di apportare modifiche due volte ogni volta che si introducono o modificare una proprietà di Dinner nuovo - che non è valida.

### <a name="using-partial-view-templates"></a>Usando i modelli di visualizzazione parziale

ASP.NET MVC supporta la possibilità di definire i modelli di "visualizzazione parziale" che possono essere usati per incapsulare la logica per il rendering di visualizzazione per una parte di una pagina secondaria. "Righe parzialmente eseguite" forniscono un modo utile per definire la logica di visualizzazione per il rendering di una volta e quindi riutilizzare in più posizioni in un'applicazione.

Per "DRY-up" nostro Edit. aspx e tornarci la duplicazione del modello di visualizzazione, è possibile creare un modello di visualizzazione parziale denominato "DinnerForm.ascx" che incapsula il layout del form e comune a entrambi gli elementi di input. È possibile eseguire questa operazione facendo clic sul nostro/viste o Dinners directory e scegliendo la "Add -&gt;Visualizza" comando di menu:

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Verrà visualizzata la finestra di dialogo "Aggiungi visualizzazione". La nuova vista si desidera creare "DinnerForm", selezionare la casella di controllo "Crea una visualizzazione parziale" all'interno della finestra di dialogo e indicare che viene passata è una classe DinnerFormViewModel denominiamo:

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Quando si fa clic sul pulsante "Aggiungi", Visual Studio creerà un nuovo modello di visualizzazione "DinnerForm.ascx" per noi all'interno della directory "\Views\Dinners".

È possibile quindi copiare e incollare il layout del form duplicati / input di codice di controllo dai nostri modelli di vista Edit.aspx/ tornarci in nostro nuovo modello di visualizzazione parziale "DinnerForm.ascx":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

È quindi possibile aggiornare i modelli di visualizzazione creazione e modifica per chiamare il modello parziale DinnerForm ed eliminare la duplicazione di form. È possibile eseguire questa operazione Html.RenderPartial("DinnerForm") chiamante entro i nostri modelli di visualizzazione:

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

È possibile qualificare in modo esplicito il percorso del modello parziale desiderata quando si chiama Html.RenderPartial (ad esempio: ~ Views/Dinners/DinnerForm.ascx "). Nel codice precedente, tuttavia, stiamo sfruttando il modello di denominazione basata sulle convenzioni all'interno di ASP.NET MVC e specificare solo "DinnerForm" come nome di eseguire il rendering parziale. Quando si esegue questa operazione, ASP.NET MVC cercherà prima nella directory delle visualizzazioni basato sulle convenzioni (DinnersController sarebbe/viste/Dinners). Se non viene trovato il modello parziale vi quindi cercherà lo nella directory /Views/Shared.

Quando viene chiamato Html.RenderPartial() utilizzando solo il nome della visualizzazione parziale, ASP.NET MVC passerà alla visualizzazione parziale gli oggetti di un dizionario ViewData e modello stesso utilizzati dal modello di vista del chiamante. In alternativa, esistono versioni di overload di Html.RenderPartial() che consentono di passare un oggetto modello alternativo e/o il dizionario ViewData per la visualizzazione parziale da utilizzare. Ciò è utile per scenari in cui si desidera solo passare un sottoinsieme di modello/ViewModel completo.

| **Sul lato dell'argomento: Il motivo per cui &lt;% %&gt; invece di &lt;% = %&gt;?** |
| --- |
| Uno degli aspetti meno evidenti si potrebbe aver notato con il codice precedente è che si sta usando un &lt;% %&gt; block invece di un &lt;% = %&gt; bloccare quando si chiama Html.RenderPartial(). &lt;% = %&gt; blocchi in ASP.NET indicano che uno sviluppatore desidera eseguire il rendering di un valore specificato (ad esempio: &lt;% = "Hello" %&gt; consentirà di visualizzare "Hello"). &lt;% %&gt; blocchi indicano invece che lo sviluppatore desidera eseguire il codice e che qualsiasi output sottoposto a rendering all'interno di essi deve essere eseguita in modo esplicito (ad esempio: &lt;Response.Write("Hello") %&gt;. Il motivo si sta usando un &lt;% %&gt; blocco con il nostro codice Html.RenderPartial sopra è perché il metodo Html.RenderPartial() non restituisce una stringa e restituisce invece il contenuto direttamente con il modello di vista chiamante del flusso di output. Ciò avviene per motivi di efficienza di prestazioni e in tal modo evita la necessità di creare un oggetto stringa temporaneo (potenzialmente molto grande). Questo riduce l'utilizzo di memoria e migliorare la velocità effettiva complessiva dell'applicazione. Un errore comune quando si utilizza Html.RenderPartial() è omesso di aggiungere un punto e virgola alla fine della chiamata quando è all'interno di un &lt;% %&gt; blocco. Ad esempio, questo codice causa un errore del compilatore: &lt;% Html.RenderPartial("DinnerForm")&gt; è invece necessario scrivere: &lt;Html.RenderPartial("DinnerForm") %; %&gt; infatti &lt;% %&gt; blocchi sono istruzioni di codice indipendente e quando si utilizza codice c# necessario istruzioni terminare con un punto e virgola. |

### <a name="using-partial-view-templates-to-clarify-code"></a>Usando i modelli di visualizzazione parziale per chiarire codice

È stato creato il modello di visualizzazione parziale "DinnerForm" per evitare di duplicare la logica di visualizzazione per il rendering in più posizioni. Questo è il motivo più comune per creare modelli di visualizzazione parziale.

In alcuni casi è comunque opportuno per creare le visualizzazioni parziali, anche quando vengono chiamati solo in un'unica posizione. I modelli di vista molto complicata spesso possono diventare molto più semplici da leggere quando la logica di visualizzazione per il rendering verrà estratti e partizionata in uno o più anche denominata templates parziale.

Ad esempio, si consideri il seguente frammento di codice dal file Site. master nel nostro progetto (che esamineremo a breve). Il codice è relativamente semplice da leggere, in parte perché la logica per la visualizzazione di un'account di accesso/disconnessione collegamento nella parte superiore destra della schermata viene incapsulata all'interno di "LogOnUserControl" parziale:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Ogni volta che ci si ritroverà a confusione cercando di capire il markup html/codice all'interno di un modello di vista, prendere in considerazione se non sarebbe più chiaro se alcune di esse è stato estratto e sottoposta a refactoring in visualizzazioni parziali ben identificabili.

### <a name="master-pages"></a>Pagine master

Oltre a supportare le visualizzazioni parziali, ASP.NET MVC supporta inoltre la possibilità di creare modelli "pagina master" che possono essere usati per definire il layout comuni e html principale di un sito. I controlli possono quindi essere aggiunti alla pagina master per identificare aree sostituibili che possono essere sottoposto a override o "compilate" viste segnaposto di contenuto. Ciò fornisce un modo molto efficace (e DRY) per applicare un layout comune in un'applicazione.

Per impostazione predefinita, i nuovi progetti ASP.NET MVC hanno un modello di pagina master a essi aggiunti automaticamente. Questa pagina master è denominata "Site" e si trova all'interno della cartella \Views\Shared\:

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

Il file Site. master predefinito è la seguente. Definisce il codice html esterno del sito, oltre a un menu per la navigazione nella parte superiore. Contiene due controlli sostituibili segnaposto di contenuto: uno per il titolo e l'altro per la quale il principale contenuto di una pagina deve essere sostituito:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Tutti i modelli di vista, che abbiamo creato per la nostra applicazione NerdDinner ("List", "Dettagli", "Modifica", "Crea", "NotFound", e così via) sono basati su questo modello Site. master. Questa caratteristica è indicata tramite l'attributo "MasterPageFile" che è stato aggiunto per impostazione predefinita nella parte superiore &lt;% @ % pagina&gt; direttiva quando abbiamo creato viste utilizzando la finestra di dialogo "Aggiungi visualizzazione":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

Ciò significa che è possibile modificare il contenuto Site. master, e hanno le modifiche automaticamente essere applicate e usate quando si esegue il rendering di uno dei nostri modelli di visualizzazione.

Aggiorniamo la sezione di intestazione del nostro Site. master, in modo che l'intestazione dell'applicazione è "NerdDinner" anziché "My MVC Application". È possibile anche aggiornare il menu di navigazione in modo che nella prima scheda è "Trovare un Dinner" (gestito dal metodo di azione Index () di HomeController) e aggiungiamo una nuova scheda denominata "Host a Dinner" (gestito dal metodo di azione del DinnersController create ()):

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Quando si salva il file Site. master e aggiornare il browser si vedrà l'intestazione modifiche vengano visualizzate in tutte le visualizzazioni all'interno dell'applicazione. Ad esempio:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

E con il */Dinners/modifica / [id]* URL:

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>Passo successivo

Righe parzialmente eseguite e le pagine master forniscono opzioni molto flessibili che consentono di organizzare normalmente le visualizzazioni. Troverai che consentono di evitare la duplicazione di visualizzazione del contenuto / codice e rendere più semplice da leggere e gestire i modelli di visualizzazione.

È possibile a questo punto rivedere lo scenario di elenco che è stato creato in precedenza e abilitare il supporto di paging scalabile.

> [!div class="step-by-step"]
> [Precedente](use-viewdata-and-implement-viewmodel-classes.md)
> [Successivo](implement-efficient-data-paging.md)
