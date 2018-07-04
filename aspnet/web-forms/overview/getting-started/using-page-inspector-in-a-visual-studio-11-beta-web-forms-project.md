---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Utilizzo di controllo pagina per Visual Studio 2012 in ASP.NET Web Forms | Microsoft Docs
author: rick-anderson
description: Controllo pagina per Visual Studio 2012 è uno strumento di sviluppo web con un browser integrato. Selezionare qualsiasi elemento nel browser integrato e controllo pagina...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 1f1ac1072d33c85ed3e64c493b9cf7970695cea6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384475"
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>Utilizzo di controllo pagina per Visual Studio 2012 in Web Form ASP.NET
====================
da Tim Ammann

> Controllo pagina per Visual Studio 2012 è uno strumento di sviluppo web con un browser integrato. Selezionare un elemento nel browser integrato e controllo pagina evidenzia immediatamente l'origine e CSS dell'elemento. È possibile esplorare tutte le pagine nell'applicazione, rapidamente trovare le origini di markup sottoposto a rendering e usare gli strumenti del browser direttamente all'interno dell'ambiente di Visual Studio.
> 
> Questa esercitazione shwos come abilitare la modalità controllo rapidamente individuare e modificare le regole CSS e il testo all'interno del progetto web. L'esercitazione Usa un progetto di applicazione Web Form, ma è anche possibile utilizzare controllo pagina per progetti di siti Web e [MVC](https://go.microsoft.com/?linkid=9802002) applicazioni.
> 
> L'esercitazione include le sezioni seguenti:
> 
> [Prerequisiti](#_1_prerequisites)
> 
> [Creare un'applicazione Web](#_2_creating_a)
> 
> [Utilizzare controllo pagina per visualizzare l'applicazione](#_3_using_page)
> 
> [Abilitare la modalità controllo](#_4_inspection_mode)
> 
> [Utilizzare controllo pagina per apportare modifiche al Markup](#_5_using_page)
> 
> [Modalità controllo e la finestra HTML](#_6_inspection_mode)
> 
> [Anteprima modifiche CSS nella finestra stili](#_7_previewing_css)
> 
> [Sincronizzazione automatica CSS](#css_auto_sync)
> 
> [Tramite la selezione colori CSS](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Prerequisiti

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) oppure [Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Per ottenere la versione più recente di controllo pagina, usare [instalace Webové Platformy](https://go.microsoft.com/fwlink/?LinkId=255386) per installare Azure SDK per .NET 2.0.


Page Inspector è abbinato a Microsoft Web Developer Tools. La versione più recente è 1.3x. Per verificare quale versione hanno, eseguire Visual Studio e selezionare **informazioni su Microsoft Visual Studio** dal **Guida** menu.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Creare un'applicazione Web

In primo luogo, si creerà un'applicazione web che verrà utilizzato il controllo di pagina con. In Visual Studio, scegliere **File** &gt; **nuovo progetto**. A sinistra, espandere **Visual c#**, selezionare **Web**, quindi selezionare **applicazione Web Form ASP.NET**.

![Nuova applicazione Web Form](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

Fare clic su **OK**.

L'applicazione viene aperto in **origine** visualizzazione.

![Nuova applicazione Web Form nella visualizzazione origine](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

Dopo aver creato un'applicazione di usare, è possibile utilizzare controllo pagina per esaminare e modificarlo.

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>Utilizzare controllo pagina per visualizzare l'applicazione

Successivamente, si visualizzeranno l'applicazione con controllo pagina. Nelle **Esplora soluzioni**, fare clic con il pulsante destro del progetto e quindi scegliere **Visualizza in controllo pagina**.

![Visualizza in controllo pagina](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

Per impostazione predefinita, quando si avvia controllo pagina per la prima volta, è ancorata come una piccola finestra sul lato sinistro dell'ambiente di Visual Studio. Lasciarla ancorata al lato sinistro e impostarla su una larghezza che ritiene adatto per l'utente oppure ancorarla in una delle aree degli strumenti in alto, nella parte inferiore o destro:

![Posizioni di ancoraggio di controllo pagina](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

Se si disancorare la finestra di controllo pagina, è possibile inserirlo all'esterno di Visual Studio o anche in un secondo monitor se presente. Tuttavia, in ordine ALT + TAB tra controllo pagina e Visual Studio quando la finestra di controllo pagina è ancorata, passare a **degli strumenti** &gt; **opzioni** &gt;  **Ambiente** &gt; **schede e Windows**, quindi in **scheda anche**, deselezionare la casella di controllo denominato **finestre degli strumenti finestra mobile restano sempre in cima il finestra principale**:

![Deselezionare la casella di controllo di windows degli strumenti a virgola mobile per ALT + TAB tra Visual Studio e la finestra di controllo pagina non ancorata](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

Il riquadro superiore della finestra di controllo pagina Mostra la pagina corrente in una finestra del browser. Nel riquadro inferiore mostra la pagina nel markup HTML a sinistra e alcune schede a destra che consentono di controllare diversi aspetti della pagina. Il riquadro inferiore è simile al [strumenti di sviluppo F12](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer. (Tuttavia, diversamente da strumenti di sviluppo, è possibile utilizzare controllo pagina direttamente da Visual Studio.)

![Controllo pagina](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

In questa esercitazione si userà il riquadro del browser di controllo pagina e il **HTML** e **stili** schede che consentono rapidamente passare e apportare modifiche all'applicazione.

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>Abilitare la modalità controllo

Successivamente, si noterà come modalità di controllo di controllo pagina funziona. Nella finestra di controllo pagina, scegliere il **Inspect** pulsante.

![Esaminare l'elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

Per visualizzare le modalità di controllo in azione, spostare il puntatore del mouse su parti diverse della pagina nella finestra del browser di controllo pagina. Come avviene, il puntatore del mouse diventa un segno più grande e viene evidenziato l'elemento di sotto:

![Posizionare il mouse su div.content wrapper](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

Quando si sposta il puntatore del mouse, tenere presente che

- Il contenuto in **origine** visualizzazione cambia per mostrare il markup corrispondente all'elemento selezionato nella pagina. Il markup pertinente viene evidenziato. Se l'origine è in un altro file, tale file viene aperto nella visualizzazione origine con il markup pertinente evidenziato.

- Il markup visualizzato nei **HTML** consente anche di passare in controllo pagina in modo da corrispondere all'elemento selezionato nella pagina. Nel **HTML** scheda, viene illustrato il markup pertinente.

- Il **stili** scheda Mostra le regole CSS pertinenti per la selezione corrente.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Utilizzare controllo pagina per apportare modifiche al Markup

A questo punto verrà visualizzato come è possibile utilizzare controllo pagina per trovare e apportare modifiche al markup o la cui posizione potrebbe non essere immediatamente ovvia come testo.

PUT Page Inspector in modalità controllo e quindi scorrere fino alla fine della home page.

Non appena si immette l'area di piè di pagina, controllo pagina apre il *Site. master* file di layout nella **origine** vista in una scheda temporanea a destra delle altre schede ed evidenzia la sezione del master pagina che si Hai selezionato. Vengono visualizzati come controllo pagina è possibile trovare e visualizzare il contenuto in una pagina che in realtà potrebbe provenire da un file diverso da quello che è stata aperta originariamente.

![Piè di pagina vengono evidenziate in modalità controllo](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

Nella finestra del browser di controllo pagina, spostare il puntatore del mouse sulla riga con il copyright <a id="a"> </a>notare.

Nel *Site. master* pagina, la riga corrispondente viene evidenziato.

![Riga di piè di pagina informazioni sul copyright evidenziata](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

Aggiungere testo alla fine della riga nel *Site. master* file.

&lt;p&gt;&amp;duplicarlo; &lt;%: % DateTime.Now.Year&gt; -Rocks l'applicazione ASP.NET!&lt; / p&gt;

A questo punto, premere Ctrl + Alt + INVIO o fare clic sulla barra di aggiornamento per visualizzare i risultati nella finestra del browser di controllo pagina.

![Funziona l'applicazione ASP.NET!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

Si potrebbe avrai pensato che era il piè di pagina sul *default. aspx* pagina, ma si è dimostrata nella pagina di layout master e controllo pagina è stato trovato per te.

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Modalità controllo e la finestra HTML

Successivamente, si avrà un'occhiata alla finestra HTML e il relativo mapping elementi per l'utente.

Inserire controllo pagina in modalità controllo.

![Esaminare l'elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

Fare clic sulla parte superiore della pagina con la dicitura "inserire qui il logo". Se si sta esaminando un particolare elemento in modo più dettagliato, in modo che la visualizzazione nella finestra del browser non è più viene modificato quando si sposta il puntatore del mouse.

Ora passare il puntatore del mouse per il **HTML** finestra. Quando si sposta il puntatore del mouse, controllo pagina vengono illustrati l'elemento all'interno di **HTML** finestra ed evidenzia l'elemento corrispondente nella finestra del browser.

![Finestra HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

Come in precedenza, controllo pagina apre il *Site. master* file per l'utente in una scheda temporanea. Fare clic sulla scheda Site. master, e il markup corrispondente viene evidenziato nel &lt;intestazione&gt; sezione:

![Markup evidenziato](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Anteprima modifiche CSS nella finestra stili

Successivamente, si vedrà come usare il controllo pagina **stili** finestra per visualizzare in anteprima le modifiche a CSS.

Scegliere il **Inspect** pulsante per attivare controllo pagina in modalità controllo.

Nella finestra del browser di controllo pagina, spostare il puntatore del mouse sopra la sezione "Home Page" finché il **div.content wrapper** etichetta viene visualizzata.

![Passare il mouse sugli elementi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

Fare clic su una sola volta all'interno della sezione div.content wrapper, e quindi spostare il puntatore del mouse per il **stili** finestra. Nel selettore della classe wrapper. Content .featured, deselezionare e selezionare la casella di controllo per la proprietà di colore di sfondo.

![Colore di sfondo chiaro](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

Si noti come la modifica include l'anteprima immediatamente nella finestra del browser di controllo pagina.

Selezionare la casella di controllo di nuovo, quindi fare doppio clic sul valore della proprietà e modificarlo in base ai `red`. La modifica viene immediatamente:

![Colore di sfondo rosso](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

Il **stili** rende finestra è più facile testare e visualizzare in anteprima CSS viene modificato prima di procedere effettivamente le modifiche apportate allo stile della finestra stessa.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Sincronizzazione automatica CSS

> [!NOTE]
> Questa funzionalità richiede la versione 1.3 di controllo pagina.


La funzionalità di sincronizzazione automatica CSS consente di modificare direttamente un file CSS e visualizzare le modifiche immediatamente nel browser di controllo pagina.

Fare clic su **Inspect** inserire controllo pagina in modalità controllo.

Nel browser di controllo pagina, spostare il puntatore del mouse sopra la sezione "Home Page" finché il **div.content wrapper** etichetta viene visualizzata. Fare clic su una sola volta per selezionare questo elemento.

Il **stili** finestra Mostra tutte le regole CSS per questo elemento. Scorrere verso il basso il selettore di classe find .featured. Content-wrapper. Fare clic su .featured. Content-"wrapper". Controllo pagina apre il file CSS che definisce questo stile (CSS) ed evidenzia il CSS corrispondente.

![File CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

A questo punto modificare il valore per `background-color` su "red". La modifica viene visualizzata immediatamente nel browser di controllo pagina.

![Browser di controllo pagina](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>Tramite la selezione colori CSS

Successivamente, imparerai a utilizzare controllo pagina per trovare rapidamente e modificare il CSS relativo testo evidenziato nell'applicazione predefinita. In questo esempio, si è deciso di non desidera che l'evidenziazione blu e per modificarlo in un altro colore.

Scegliere il **Inspect** pulsante.

![Esaminare l'elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

Nella finestra del browser di controllo pagina, spostare il puntatore del mouse sulla evidenziata "video, esercitazioni ed esempi di" testo in modo che il file CSS "mark" etichetta viene visualizzata.

![Passare il mouse sull'elemento mark](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

Fare clic sul testo per selezionarlo. Il selettore di mark CSS corrispondente viene visualizzato in fondo il **stili** finestra.

![selettore di Mark nella finestra stili](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

Fare clic sul selettore di mark. Verrà visualizzata la *CSS* file per l'applicazione web. Fare clic sulla scheda Site. CSS e viene evidenziato il CSS corrispondente per il selettore:

![selettore di Mark nel foglio di stile](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

Selezionare e rimuovere la riga con la proprietà di colore di sfondo.

Si userà ora la nuova selezione di colori di Visual Studio 2012 CSS per scegliere un nuovo colore per il **contrassegnare** proprietà background-color.

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>Tramite la selezione colori CSS di Visual Studio 2012

Editor CSS in Visual Studio 2012 è una selezione colori che consente di scegliere e inserire i colori. Include una semplice barra dei colori e una selezione di "pop-down" che offre un controllo più preciso.

La selezione colori include una tavolozza di colori standard supporta i nomi dei colori standard, codici hash, i colori RGB, RGBA, HSL e HSLA e gestisce un elenco dei colori utilizzati più di recente nel documento.

Nella riga in cui è stata la proprietà di colore di sfondo, digitare "bc" e premere la freccia verso il basso una sola volta.

Quando si digita il primo carattere di ogni parola in una proprietà separati da trattini, ad esempio "background-color", IntelliSense Filtra l'elenco per visualizzare solo le proprietà che corrispondono a:

![Valori di IntelliSense filtrati](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

A questo punto digitare due punti. Quando esegue questa operazione, viene inserito il nome della proprietà completa del colore di sfondo. Tipo di **#** oppure **rgb (**, e viene visualizzata la barra di selezione colore:

![La barra di selezione colori CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

Per vedere come funziona la barra di selezione colore, fare clic sul relativo colori con il puntatore del mouse o premere il tasto freccia giù e quindi utilizzare i tasti freccia sinistra e destra per attraversare i colori. Quando si visita un colore, viene visualizzato in anteprima il valore corrispondente per la proprietà background-color:

![valore della proprietà di colore di sfondo visualizzata in anteprima](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

A questo punto, è possibile premere INVIO per selezionare il valore e quindi un punto e virgola (;) per completare la voce di CSS. Per il momento, andare nella sezione successiva in modo che è possibile vedere come funziona il pop-down di selezione colore.

#### <a name="using-the-color-picker-pop-down"></a>Tramite la selezione di colore Pop-Down

Quando la barra dei colori non dispone del colore esatto che si sta cercando, è possibile usare la selezione colori popup a discesa.

Per aprirlo, fare clic sulle virgolette doppie all'estremità destra della barra dei colori o premere una o due volte il tasto freccia giù sulla tastiera.

![Selezione di colore CSS Pop-Down](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

Fare clic su un altro colore nella barra verticale sul lato destro. Mostra una sfumatura di colore nella finestra principale. Scegliere un colore direttamente dalla barra degli strumenti verticale premendo INVIO oppure fare clic su qualsiasi punto nella finestra principale di scegliere con precisione maggiore.

Se è presente un colore sullo schermo del computer che si desidera utilizzare (non deve necessariamente essere all'interno dell'interfaccia utente di Visual Studio), è possibile acquisire il relativo valore con lo strumento contagocce in basso a destra.

È anche possibile modificare l'opacità di colore spostando il dispositivo di scorrimento nella parte inferiore della selezione colori. In questo modo le modifiche dei colori dei valori di valori RGBA perché il formato RGBA può rappresentare l'opacità.

Dopo aver scelto un colore, premere INVIO e quindi digitare un punto e virgola per completare la voce di colore di sfondo nel *CSS* file.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>La barra di aggiornamento di controllo pagina

Controllo pagina immediatamente rileva la modifica per il *CSS* file (o a qualsiasi file nell'applicazione) e viene visualizzato un avviso in una barra di aggiornamento.

![Barra di aggiornamento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

Per salvare tutti i file e aggiornare il browser di controllo pagina, premere Ctrl + Alt + INVIO o fare clic sulla barra di aggiornamento. La modifica il colore di evidenziazione viene visualizzata nel browser:

![Colore dei lati illuminati modificato](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>Si noti che facilmente aggiornato browser di controllo pagina direttamente dall'interno dell'ambiente Visual Studio. Utilizzo di controllo pagina invece di un browser esterno consente di rimanere nell'editor, quando si sviluppano le applicazioni web.

## <a name="see-also"></a>Vedere anche

[Introduzione a controllo pagina](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (video di Channel 9)

[I messaggi di errore di controllo pagina](https://go.microsoft.com/?linkid=9813062) (MSDN)
