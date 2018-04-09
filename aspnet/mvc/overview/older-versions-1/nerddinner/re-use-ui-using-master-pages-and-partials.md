---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Riutilizzo dell'interfaccia utente utilizzando le pagine Master e parziali | Documenti Microsoft
author: microsoft
description: Passaggio 7 esamina la modalità che è possibile applicare il principio secca entro i nostri modelli di visualizzazione per evitare la duplicazione del codice, usando i modelli di visualizzazione parziale e pagine master.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: ade655f3a4a429360b678d02fb564ac9cf255d42
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="re-use-ui-using-master-pages-and-partials"></a>Riutilizzo dell'interfaccia utente utilizzando le pagine Master e parziali
====================
by [Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Si tratta di passaggio 7 di una liberazione [esercitazione applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-through come compilare un piccolo, ma completa, un'applicazione web con ASP.NET MVC 1.
> 
> Passaggio 7 esamina la modalità che è possibile applicare il principio"secca" all'interno i nostri modelli di visualizzazione per evitare la duplicazione del codice, usando i modelli di visualizzazione parziale e pagine master.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire il [recupero avviato con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [negozio MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner passaggio 7: Parziali e pagine Master

Uno dei filosofie di progettazione che basata sul modello MVC ASP.NET è il principio "Non si ripetere manualmente" (considerato come "Sorgente"). Una progettazione secca consente di eliminare le duplicazioni di codice e logica, che in definitiva rende più veloce per compilare e facile da gestire le applicazioni.

Abbiamo già visto il principio secco applicato in diversi gli scenari NerdDinner. Alcuni esempi: viene implementata la logica di convalida all'interno di questo livello del modello, in modo da essere applicate in entrambi modifica e creare scenari in questo controller. usiamo nuovamente il modello di visualizzazione "NotFound" tra i metodi di azione di modifica, dettagli e l'eliminazione; si sta usando un modello di denominazione convenzione - con i modelli di visualizzazione, che elimina la necessità di specificare in modo esplicito il nome quando si chiama il metodo di supporto View(); e che vengono nuovamente utilizzando la classe di DinnerFormViewModel per entrambi modifica creare scenari di azione.

Ora esaminiamo modi che è possibile applicare il principio"secca" all'interno i nostri modelli di visualizzazione per eliminare anche la duplicazione del codice non esiste.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Nuovamente, visitare il nostro modifica e creare modelli di visualizzazione

Attualmente vengono utilizzati due modelli di visualizzazione diverse: "Edit" e "Create.aspx": per visualizzare il modulo Dinner dell'interfaccia utente. Un rapido confronto visual tra di essi evidenzia simile a come sono. Di seguito è il modulo Crea l'aspetto seguente:

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

Di seguito è il modulo "Edit" l'aspetto seguente:

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

Non c'è molto di una differenza è? Diverso dal testo del titolo e intestazione, i controlli di layout e l'input del form sono identici.

Se si apre il "Edit" e "Create.aspx" si noterà che i modelli di visualizzazione contengono codice di controllo di layout e l'input modulo identici. Questa duplicazione significa che è alla fine di apportare modifiche due volte ogni volta che si introducono o modificare una nuova proprietà Dinner - non è valida.

### <a name="using-partial-view-templates"></a>Utilizzo di modelli di visualizzazione parziale

ASP.NET MVC supporta la possibilità di definire i modelli di "visualizzazione parziale" che possono essere usati per incapsulare la logica di visualizzazione per il rendering di una parte di una pagina secondaria. "Parziali" forniscono un modo utile per definire la logica di visualizzazione per il rendering di una volta e quindi riutilizzare in più posizioni in un'applicazione.

Per "Sorgente-up" nostro Edit e Create.aspx la duplicazione del modello di visualizzazione, è possibile creare un modello di visualizzazione parziale denominato "DinnerForm.ascx" che incapsula il layout del form e i comuni a entrambi gli elementi di input. Faremo questo facendo clic sul nostro/viste o Dinners directory e scegliendo la "Add -&gt;visualizzazione" comando di menu:

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Verrà visualizzata la finestra di dialogo "Aggiungi visualizzazione". La nuova vista, si desidera creare "DinnerForm", selezionare la casella di controllo "Crea una visualizzazione parziale" nella finestra di dialogo e indicare che verrà passato è una classe DinnerFormViewModel sarà denominato:

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Quando si fa clic sul pulsante "Aggiungi", Visual Studio creerà un nuovo modello di visualizzazione "DinnerForm.ascx" per noi all'interno della directory "\Views\Dinners".

È possibile quindi copiare e incollare il layout del form duplicati / codice di controllo dai nostri modelli di visualizzazione Edit.aspx/ Create.aspx di input in questo nuovo modello di visualizzazione parziale "DinnerForm.ascx":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

È quindi possibile aggiornare i nostri modelli di visualizzazione di modifica e crea per chiamare il modello parziale DinnerForm ed eliminare le duplicazioni di modulo. È possibile eseguire questa operazione Html.RenderPartial("DinnerForm") chiamante entro i nostri modelli di visualizzazione:

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

È possibile qualificare in modo esplicito il percorso del modello parziale desiderato quando si chiama Html.RenderPartial (ad esempio: ~ Views/Dinners/DinnerForm.ascx "). Nel codice precedente, tuttavia, stiamo sfruttando il modello di denominazione basato sulle convenzioni all'interno di ASP.NET MVC e specificare solo "DinnerForm" come nome di cui eseguire il rendering parziale. Quando si esegue questa operazione, ASP.NET MVC viene cercato prima nella directory delle visualizzazioni basato sulle convenzioni (DinnersController sarebbe/visualizzazioni/Dinners). Se non viene trovato il modello parziale si verrà quindi eseguita una ricerca, nella directory /Views/Shared.

Quando Html.RenderPartial() viene chiamato con solo il nome della visualizzazione parziale, ASP.NET MVC verrà passato alla visualizzazione parziale gli oggetti di un dizionario modello e ViewData stesso utilizzati dal modello di visualizzazione del chiamante. In alternativa, sono disponibili le versioni di overload di Html.RenderPartial() che consentono di passare un oggetto modello alternativo e/o il dizionario ViewData per la visualizzazione parziale da utilizzare. Ciò è utile per scenari in cui si desidera solo un subset del completo modello/ViewModel passare.

| **Sul lato dell'argomento: Perché &lt;%&gt; anziché &lt;% = %&gt;?** |
| --- |
| Uno degli aspetti complessi è possibile aver notato con il codice precedente è che si sta usando un &lt;% %&gt; anziché il blocco una &lt;% = %&gt; blocco quando si chiama Html.RenderPartial(). &lt;% = %&gt; blocchi in ASP.NET indicano che uno sviluppatore desidera eseguire il rendering di un valore specificato (ad esempio: &lt;% = "Hello" %&gt; il rendering di "Hello"). &lt;% %&gt; blocchi invece indicano che lo sviluppatore desidera eseguire il codice e che qualsiasi output sottoposto a rendering all'interno di essi deve essere eseguita in modo esplicito (ad esempio: &lt;Response.Write("Hello") %&gt;. Il motivo che si sta usando un &lt;% %&gt; blocco con codice Html.RenderPartial è perché il metodo Html.RenderPartial() non restituisce una stringa e restituisce invece il contenuto direttamente al modello di visualizzazione del chiamante del flusso di output. Ciò avviene per motivi di efficienza di prestazioni e in tal modo evita la necessità di creare un oggetto stringa temporaneo (potenzialmente molto grande). Questo riduce l'utilizzo di memoria e consente di migliorare le prestazioni generali dell'applicazione. Un errore comune quando si utilizza Html.RenderPartial() è omesso di aggiungere un punto e virgola alla fine della chiamata quando è all'interno di un &lt;% %&gt; blocco. Ad esempio, questo codice causa un errore del compilatore: &lt;Html.RenderPartial("DinnerForm") %&gt; è invece necessario scrivere: &lt;Html.RenderPartial("DinnerForm") %; %&gt; infatti &lt;% %&gt; blocchi sono istruzioni di codice indipendente e quando si utilizza il codice c# istruzioni devono terminare con un punto e virgola. |

### <a name="using-partial-view-templates-to-clarify-code"></a>Utilizzo di modelli di visualizzazione parziale per chiarire il codice

È stato creato il modello di visualizzazione parziale "DinnerForm" per evitare la duplicazione di logica di visualizzazione per il rendering in più posizioni. Questo è il motivo più comune per creare modelli di visualizzazione parziale.

In alcuni casi è comunque utile per creare le visualizzazioni parziali, anche quando vengono chiamati solo in un'unica posizione. Modelli di visualizzazione molto complicata spesso possono diventare molto più facile da leggere quando la logica di visualizzazione per il rendering è estratto e partizionata in uno o più anche denominata modelli parziali.

Ad esempio, si consideri il seguente frammento di codice dal file Site. master nel progetto (che si visualizzeranno a breve). Il codice è relativamente semplice da leggere, in parte perché la logica per la visualizzazione di un'account di accesso/disconnessione collegamento nella parte superiore destra della schermata viene incapsulata all'interno di parziale "LogOnUserControl":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Ogni volta che si troverà confusione tentando di comprendere il markup html o codice all'interno di un modello di visualizzazione, prendere in considerazione se non sarebbe più chiaro se alcune di esse è stato estratto e sottoposta a refactoring in visualizzazioni parziali ben denominate.

### <a name="master-pages"></a>Pagine master

Oltre a supportare le visualizzazioni parziali, ASP.NET MVC supporta inoltre la possibilità di creare modelli "pagina master" che possono essere usati per definire il layout comuni e html di livello superiore di un sito. Segnaposto, quindi aggiungere i controlli della pagina master per identificare le aree sostituibili che possono essere sottoposto a override o "compilate" viste del contenuto. Ciò fornisce un modo molto efficace (e secco) per applicare un layout comune in un'applicazione.

Per impostazione predefinita, i nuovi progetti ASP.NET MVC dispongono di un modello di pagina master automaticamente aggiunto a questi ultimi. Questa pagina master viene denominata "Site. master" e risiede all'interno della cartella \Views\Shared\:

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

Il file Site. master predefinito è simile di sotto. Definisce il codice html esterno del sito, insieme a un menu per la navigazione nella parte superiore. Contiene due controlli segnaposto di contenuto sostituibili: uno per il titolo e l'altro in cui deve essere sostituito il contenuto di una pagina principale:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Tutti i modelli di visualizzazione che abbiamo creato per l'applicazione NerdDinner ("List", "Dettagli", "Modifica", "Crea", "NotFound", e così via) sono stati basati su questo modello Site. master. In questo caso mediante l'attributo "MasterPageFile" che è stato aggiunto per impostazione predefinita nella parte superiore &lt;% @ Page %&gt; direttiva quando abbiamo creato il nostro viste utilizzando la finestra di dialogo "Aggiungi visualizzazione":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

Ciò significa che è possibile modificare il contenuto Site. master e sono le modifiche automaticamente applicate e utilizzate quando si esegue il rendering di uno dei nostri modelli di visualizzazione.

Si aggiornare la sezione di intestazione del nostro Site. master in modo che l'intestazione dell'applicazione è "NerdDinner" anziché "My MVC Application". Di seguito aggiornare anche il menu di navigazione in modo che la prima scheda è "Trovare un Dinner" (gestito dal metodo di azione del HomeController Index ()) e aggiungere una nuova scheda denominata "Host a Dinner" (gestito dal metodo di azione di metodo di creazione del DinnersController):

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Quando si salva il file Site. master e aggiornare il browser si vedrà l'intestazione le modifiche Mostra fino a tutte le viste all'interno dell'applicazione. Ad esempio:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

E con il */Dinners/modifica / [id]* URL:

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>Passo successivo

Pagine master e parziali forniscono opzioni molto flessibile che consentono di organizzare correttamente le visualizzazioni. Sono disponibili che non si può evitare la duplicazione di visualizzazione del contenuto / codice e rendere più facile da leggere e gestire i modelli di visualizzazione.

Verrà ora accedere nuovamente lo scenario di elenco che è stato creato in precedenza e abilitare il supporto di paging scalabile.

> [!div class="step-by-step"]
> [Precedente](use-viewdata-and-implement-viewmodel-classes.md)
> [Successivo](implement-efficient-data-paging.md)
