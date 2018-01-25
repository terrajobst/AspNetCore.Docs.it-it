---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Con Page Inspector per Visual Studio 2012, in ASP.NET Web Forms | Documenti Microsoft
author: rick-anderson
description: "Controllo pagina per Visual Studio 2012 è uno strumento di sviluppo web con un browser integrato. Selezionare un elemento nel browser integrato e controllo pagina..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: ca8a3c194577766e56d0604323fef567d539316c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>Con Page Inspector per Visual Studio 2012 in Web Form ASP.NET
====================
da Tim Ammann

> Controllo pagina per Visual Studio 2012 è uno strumento di sviluppo web con un browser integrato. Selezionare un elemento nel browser integrato e controllo pagina evidenzia immediatamente l'origine dell'elemento e CSS. È possibile passare qualsiasi pagina dell'applicazione, rapidamente trovare le origini di markup sottoposto a rendering e gli strumenti del visualizzatore a destra all'interno dell'ambiente di Visual Studio.
> 
> Questa esercitazione shwos come abilitare la modalità di controllo e quindi individuare e modificare rapidamente le regole CSS e testo all'interno del progetto web. L'esercitazione Usa un progetto di applicazione Web Form, ma è anche possibile utilizzare controllo pagina per i progetti di sito Web e [MVC](https://go.microsoft.com/?linkid=9802002) applicazioni.
> 
> L'esercitazione include le sezioni seguenti:
> 
> [Prerequisiti](#_1_prerequisites)
> 
> [Creare un'applicazione Web](#_2_creating_a)
> 
> [Utilizza controllo pagina per visualizzare l'applicazione](#_3_using_page)
> 
> [Abilitare la modalità di controllo](#_4_inspection_mode)
> 
> [Utilizza controllo pagina per apportare modifiche al Markup](#_5_using_page)
> 
> [Modalità di ispezione e la finestra di HTML](#_6_inspection_mode)
> 
> [Anteprima modifiche CSS nella finestra stili](#_7_previewing_css)
> 
> [Sincronizzazione automatica CSS](#css_auto_sync)
> 
> [Mediante il selettore del colore CSS](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Prerequisiti

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) o [Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Per ottenere la versione più recente di controllo pagina, utilizzare [installazione guidata piattaforma Web](https://go.microsoft.com/fwlink/?LinkId=255386) per installare Azure SDK per .NET 2.0.


Controllo pagina viene fornito con Microsoft Web Developer Tools. La versione più recente è 1.3. Per verificare la versione hanno, eseguire Visual Studio e selezionare **su Microsoft Visual Studio** dal **Guida** menu.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Creare un'applicazione Web

Creare innanzitutto un'applicazione web che verrà utilizzato il controllo pagina con. In Visual Studio, scegliere **File** &gt; **nuovo progetto**. A sinistra, espandere **Visual c#**selezionare **Web**, quindi selezionare **applicazioni Web Form ASP.NET**.

![Nuova applicazione Web Form](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

Fare clic su **OK**.

L'applicazione verrà visualizzata in **origine** visualizzazione.

![Nuova applicazione di Web Form in visualizzazione origine](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

Dopo aver creato un'applicazione da utilizzare, è possibile utilizzare controllo pagina per esaminare e modificare.

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>Utilizza controllo pagina per visualizzare l'applicazione

Successivamente, si visualizzeranno l'applicazione con controllo pagina. In **Esplora**, fare clic con il pulsante destro del progetto e quindi scegliere **Visualizza in controllo pagina**.

![Visualizza in controllo pagina](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

Per impostazione predefinita, quando viene avviata controllo pagina per la prima volta, è ancorata come una piccola finestra sul lato sinistro dell'ambiente Visual Studio. Lasciarla ancorata a sinistra e impostarla su una larghezza che secondo necessità per l'utente o in una delle aree strumento ancorarla nella parte superiore, inferiore o destro:

![Posizione di ancoraggio di controllo pagina](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

Se si disancorare la finestra di controllo pagina, è possibile inserirlo all'esterno di Visual Studio o in un secondo monitor se si dispone di uno. Tuttavia, in ordine a ALT + TAB tra Visual Studio e controllo pagina quando la finestra di controllo pagina è ancorata, passare a **strumenti** &gt; **opzioni** &gt;  **Ambiente** &gt; **schede e finestre**e in **scheda anche**, deselezionare la casella di controllo denominata **sempre le finestre degli strumenti mobile in cima il finestra principale**:

![Deselezionare la casella di windows mobile strumento per ALT + TAB tra Visual Studio e la finestra di controllo pagina non ancorata](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

Nel riquadro superiore della finestra di controllo pagina Mostra la pagina corrente in una finestra del browser. Nel riquadro inferiore mostra la pagina nel markup HTML a sinistra e alcune schede a destra che consentono di controllare aspetti diversi della pagina. Il riquadro inferiore è simile al [strumenti di sviluppo F12](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer. (Tuttavia, diversamente da strumenti di sviluppo, è possibile utilizzare il controllo pagina destra all'interno di Visual Studio.)

![Controllo pagina](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

In questa esercitazione si utilizzerà il riquadro del browser di controllo pagina e **HTML** e **stili** schede che consentono di effettuare rapidamente passare e apportare modifiche all'applicazione.

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>Abilitare la modalità di controllo

Successivamente, verrà visualizzato come modalità di controllo di controllo pagina funziona. Nella finestra di controllo pagina, fare clic su di **controlla** pulsante.

![Controllare l'elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

Per visualizzare le modalità di controllo in azione, spostare il puntatore del mouse su parti diverse della pagina nella finestra del browser di controllo pagina. Quando si esegue, il puntatore del mouse diventa un segno più grande e viene evidenziato l'elemento sotto:

![Mouse div.content wrapper](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

Quando si sposta il puntatore del mouse, si noti che

- Il contenuto di **origine** visualizzare cambia per mostrare il markup corrispondente all'elemento selezionato nella pagina. Il markup pertinente viene evidenziato. Se l'origine è in un altro file, il file è aperto nella visualizzazione origine con il markup pertinente evidenziato.

- Il markup visualizzato nel **HTML** scheda in controllo pagina viene modificata anche in modo che corrisponda all'elemento selezionato nella pagina. Nel **HTML** scheda, viene indicato il codice pertinente.

- Il **stili** scheda Visualizza le regole CSS pertinenti per la selezione corrente.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Utilizza controllo pagina per apportare modifiche al Markup

Verrà visualizzata come è possibile utilizzare controllo pagina per trovare e apportare modifiche al markup o testo la cui posizione potrebbe non essere immediatamente evidente.

Inserire il controllo pagina in modalità controllo e quindi scorrere fino alla fine della home page.

Non appena si immette l'area di piè di pagina, vengono aperti il *Site. master* file di layout in **origine** vista in una scheda temporanea a destra delle altre schede ed evidenzia la sezione del master pagina che si stato selezionato. Verrà visualizzata come controllo pagina è possibile trovare e visualizzare il contenuto in una pagina che effettivamente potrebbe provenire da un file diverso da quello che aperto originariamente.

![Caratteristiche salienti di piè di pagina in modalità controllo](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

Nella finestra del browser di controllo pagina, spostare il puntatore del mouse sulla linea con il copyright <a id="a"> </a>nota.

Nel *Site. master* pagina, la riga corrispondente viene evidenziato.

![Riga di piè di pagina copyright evidenziata](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

Aggiungere testo alla fine della riga di *Site. master* file.

&lt;p&gt;&amp;copiare; &lt;%: % DateTime.Now.Year&gt; -My Massi applicazione ASP.NET!&lt; / p&gt;

A questo punto, premere Ctrl + Alt + Invio oppure fare clic sulla barra di aggiornamento per visualizzare i risultati nella finestra del browser di controllo pagina.

![L'applicazione ASP.NET funziona](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

Si potrebbe pensare che il piè di pagina è stato nel *Default.aspx* pagina, ma non è risultato per essere nella pagina di layout master e controllo pagina è stato trovato per l'utente.

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Modalità di ispezione e la finestra di HTML

Successivamente, si avrà un rapido controllo di finestra di HTML e come viene eseguito il mapping elementi per l'utente.

Inserire il controllo pagina in modalità controllo.

![Controllare l'elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

Fare clic sulla parte superiore della pagina su cui è indicato "inserire qui il logo". Si sta esaminando un particolare elemento in modo più dettagliato, pertanto la visualizzazione nella finestra del browser non cambierà quando si sposta il puntatore del mouse.

Spostare il puntatore del mouse per il **HTML** finestra. Quando si sposta il puntatore del mouse, controllo pagina vengono descritti l'elemento all'interno di **HTML** finestra ed evidenziare l'elemento corrispondente nella finestra del browser.

![Finestra HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

Come in precedenza, vengono aperti il *Site. master* file in una scheda temporanea. Fare clic sulla scheda Site. master, e il markup corrispondente viene evidenziato nel &lt;intestazione&gt; sezione:

![Markup evidenziato](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Anteprima modifiche CSS nella finestra stili

Successivamente, verrà visualizzato come è possibile utilizzare il controllo pagina **stili** finestra per visualizzare in anteprima le modifiche a CSS.

Fare clic su di **controlla** pulsante da inserire controllo pagina in modalità di controllo.

Nella finestra del browser di controllo pagina, spostare il puntatore del mouse sopra la sezione "Home Page" finché il **div.content wrapper** etichetta viene visualizzata.

![Passaggio del mouse sugli elementi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

Fare clic una volta all'interno della sezione div.content wrapper, e quindi spostare il puntatore del mouse per il **stili** finestra. Nel selettore di classe .featured. Content wrapper, deselezionare e selezionare la casella di controllo per la proprietà del colore di sfondo.

![Colore di sfondo cancella](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

Si noti come la modifica immediata anteprime nella finestra del browser di controllo pagina.

Selezionare la casella di controllo di nuovo, quindi fare doppio clic sul valore della proprietà e impostarla su `red`. La modifica viene immediatamente:

![Colore di sfondo rosso](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

Il **stili** rende finestra è più facile di test e visualizzare in anteprima CSS modifiche prima di applicare le modifiche allo stile di finestra stessa.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Sincronizzazione automatica CSS

> [!NOTE]
> Questa funzionalità richiede la versione 1.3 del controllo pagina.


La funzionalità di sincronizzazione automatica CSS consente di modificare direttamente un file CSS e visualizzare le modifiche immediatamente nel browser di controllo pagina.

Fare clic su **controlla** inserire controllo pagina in modalità controllo.

Nel browser di controllo pagina, spostare il puntatore del mouse sopra la sezione "Home Page" finché il **div.content wrapper** etichetta viene visualizzata. Fare clic per selezionare questo elemento.

Il **stili** finestra Visualizza tutte le regole CSS per questo elemento. Scorrere fino a individuare il wrapper. Content .featured selettore di classe. Fare clic su ".featured. Content-wrapper". Controllo pagina consente di aprire il file CSS che definisce questo stile (Site.css) ed evidenzia il CSS corrispondente.

![File CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

Modificare il valore per `background-color` per "red". La modifica viene visualizzata immediatamente nel browser di controllo pagina.

![Browser di controllo pagina](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>Mediante il selettore del colore CSS

Successivamente, si apprenderà come usare controllo pagina per trovare rapidamente e modificare il codice CSS per il testo evidenziato nell'applicazione predefinita. In questo esempio, si è deciso di non desidera che l'evidenziazione blu e modificarla in un altro colore.

Fare clic su di **controlla** pulsante.

![Controllare l'elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

Nella finestra del browser di controllo pagina, spostare il puntatore del mouse su evidenziata "video, esercitazioni ed esempi" viene visualizzato il testo in modo che il codice CSS "contrassegna" etichetta.

![Passare il mouse sopra l'elemento contrassegno](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

Fare clic sul testo per selezionarlo. Il selettore di contrassegno CSS corrispondente viene visualizzato in fondo il **stili** finestra.

![selettore di spunta nella finestra stili](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

Fare clic sul selettore di contrassegno. Verrà visualizzata la *Site* file per l'applicazione web. Fare clic sulla scheda Site e per il selettore CSS corrispondente viene evidenziato:

![Contrassegna selezione nel foglio di stile](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

Selezionare e rimuovere la riga con la proprietà del colore di sfondo.

Verrà utilizzata la nuova selezione colori di Visual Studio 2012 CSS per scegliere un nuovo colore per il **contrassegnare** proprietà colore di sfondo.

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>Mediante il selettore di colore CSS di Visual Studio 2012

L'editor CSS in Visual Studio 2012 è una selezione di colori che rende più semplice scegliere e inserire i colori. Dispone di una semplice barra dei colori e un selettore "pop-down" che offre un controllo più preciso.

La selezione colori include una tavolozza di colori standard supporta nomi di colori standard, codici hash, i colori RGB, RGBA, HSL e HSLA e gestisce un elenco dei colori utilizzati più di recente nel documento.

Nella riga in cui è stata la proprietà del colore di sfondo, digitare "bc" e premere la freccia rivolta verso il basso di una volta.

Quando si digita il primo carattere di ogni parola in una proprietà separati da trattini, ad esempio "background-color", IntelliSense consente di filtrare l'elenco per visualizzare solo le proprietà che corrispondono a:

![IntelliSense filtrato i valori](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

Digitare i due punti. Quando si esegue l'operazione, viene inserito il nome completo del colore di sfondo della proprietà. Tipo  **#**  o **rgb (**, e viene visualizzata la barra di selezione colore:

![Barra di selezione dei colori CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

Per verificare il funzionamento di barra di selezione dei colori, fare clic sui colori con il puntatore del mouse o premere il tasto freccia giù e quindi utilizzare i tasti freccia sinistra e destra per attraversare i colori. Quando si visita un colore, il valore corrispondente per la proprietà del colore di sfondo viene visualizzato in anteprima:

![valore della proprietà colore di sfondo visualizzato in anteprima](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

A questo punto, è possibile premere INVIO per selezionare il valore e quindi un punto e virgola (;) per completare la voce CSS. Per il momento, andare alla sezione successiva, in modo da poter visualizzare il funzionamento di selezione colore pop-down.

#### <a name="using-the-color-picker-pop-down"></a>Tramite la selezione colore Pop-Down

Quando la barra dei colori non è il colore esatto che si sta cercando, è possibile utilizzare il selettore di colore pop-down.

Per aprirlo, fare clic sulla doppia freccia di espansione all'estremità destra della barra dei colori o premere una o due volte il tasto freccia giù sulla tastiera.

![Selezione colori CSS Pop-Down](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

Fare clic su un colore dalla barra verticale a destra. Nella finestra principale indica una sfumatura per il colore specifico. Scegliere un colore direttamente dalla barra verticale premendo INVIO oppure fare clic su qualsiasi punto nella finestra principale di scegliere con maggiore precisione.

Se è presente un colore sullo schermo del computer che si desidera utilizzare (non è necessario all'interno dell'interfaccia utente di Visual Studio), è possibile acquisire il relativo valore utilizzando il contagocce in basso a destra.

È inoltre possibile modificare l'opacità di un colore spostando il dispositivo di scorrimento nella parte inferiore della selezione colori. In tal modo le modifiche dei colori valori RGBA perché il formato RGBA può rappresentare l'opacità.

Dopo aver scelto un colore, premere INVIO e quindi digitare un punto e virgola per completare l'immissione di colore di sfondo nel *Site* file.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>La barra di aggiornamento del controllo pagina

Controllo pagina rileva immediatamente la modifica di *Site* file (o a qualsiasi file nell'applicazione) e viene visualizzato un avviso in una barra di aggiornamento.

![Barra di aggiornamento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

Per salvare tutti i file e aggiornare il browser di controllo pagina, premere Ctrl + Alt + Invio oppure fare clic sulla barra di aggiornamento. La modifica il colore di evidenziazione viene visualizzato nel browser:

![Colore di evidenziazione modificato](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>Si noti che è facilmente aggiornato il browser di controllo pagina direttamente da all'interno dell'ambiente di Visual Studio. Con Page Inspector anziché un browser esterno consente di rimanere nell'editor, quando si sviluppano le applicazioni web.

## <a name="see-also"></a>Vedere anche

[Introduzione a controllo pagina](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (video di Channel 9)

[Messaggi di errore di Page Inspector](https://go.microsoft.com/?linkid=9813062) (MSDN)
