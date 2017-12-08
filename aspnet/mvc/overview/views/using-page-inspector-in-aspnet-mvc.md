---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: Utilizzo di controllo pagina in ASP.NET MVC | Documenti Microsoft
author: rick-anderson
description: "Controllo pagina in Visual Studio 2012 è uno strumento di sviluppo web con un browser integrato. Selezionare un elemento nel browser integrato e controllo pagina i..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 6aa9f16f166ecf5529ae33a17951eb5ea425e7af
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="using-page-inspector-in-aspnet-mvc"></a>Utilizzo di Controllo pagina in ASP.NET MVC
====================
da Tim Ammann

> Controllo pagina in Visual Studio 2012 è uno strumento di sviluppo web con un browser integrato. Selezionare un elemento nel browser integrato e controllo pagina evidenzia immediatamente l'origine dell'elemento e CSS. È possibile passare qualsiasi visualizzazione MVC, rapidamente trovare le origini di markup sottoposto a rendering e gli strumenti del visualizzatore a destra all'interno dell'ambiente di Visual Studio.
> 
> [Guardare il Video](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> In questa esercitazione viene illustrato come abilitare la modalità di controllo e quindi individuare e modificare rapidamente markup e CSS all'interno del progetto web. L'esercitazione Usa un progetto MVC, ma è anche possibile usare il controllo pagina per [Web Form](https://go.microsoft.com/?linkid=9802001) e altre applicazioni ASP.NET.
> 
> L'esercitazione include le sezioni seguenti:
> 
> - [Prerequisiti](#_1_prerequisites)
> - [Creare un'applicazione Web](#_2_creating_a)
> - [Utilizza controllo pagina per selezionare una vista](#_3_using_page)
> - [Abilitare la modalità di controllo](#_4_inspection_mode)
> - [Utilizza controllo pagina per apportare modifiche al Markup](#_5_using_page)
> - [Modalità di ispezione e la finestra di HTML](#_6_inspection_mode)
> - [Anteprima modifiche CSS nella finestra stili](#_7_previewing_css)
> - [Sincronizzazione automatica CSS](#css_auto_sync)
> - [Mediante il selettore del colore CSS](#css_color_picker)
> - [Mapping di elementi della pagina dinamica per JavaScript](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Prerequisiti

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11/en-us) o [Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web).

> [!NOTE]
> Per ottenere la versione più recente di controllo pagina, utilizzare [installazione guidata piattaforma Web](https://go.microsoft.com/fwlink/?LinkId=255386) per installare Windows Azure SDK per .NET 2.0.


Controllo pagina viene fornito con Microsoft Web Developer Tools. La versione più recente è 1.3. Per verificare la versione hanno, eseguire Visual Studio e selezionare **su Microsoft Visual Studio** dal **Guida** menu.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Creare un'applicazione Web

Innanzitutto, creare un'applicazione web che verrà utilizzato il controllo pagina con. In Visual Studio, scegliere **File** &gt; **nuovo progetto**. A sinistra, espandere **Visual c#**selezionare **Web**, quindi selezionare **applicazione Web di ASP.NET MVC4**.

![Nuova applicazione MVC ASP.NET](using-page-inspector-in-aspnet-mvc/_static/image2.png)

Fare clic su **OK**.

Nel **nuovo progetto ASP.NET MVC 4** nella finestra di dialogo **applicazione Internet**. Lasciare **Razor** come motore di visualizzazione predefinito.

![Nuovo progetto ASP.NET MVC - applicazione Internet](using-page-inspector-in-aspnet-mvc/_static/image4.png)

L'applicazione verrà visualizzata in **origine** visualizzazione.

![Nuova applicazione MVC ASP.NET nella visualizzazione origine](using-page-inspector-in-aspnet-mvc/_static/image6.png)

Dopo aver creato un'applicazione da utilizzare, è possibile utilizzare controllo pagina per esaminare e modificare.

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a>Utilizza controllo pagina per selezionare una vista

In Visual Studio 2012, è possibile fare doppio clic qualsiasi visualizzazione nel progetto, seleziona **Visualizza in controllo pagina**, controllo pagina verrà scoprire la route e visualizzare la pagina.

In **Esplora**, espandere il **viste** cartella e quindi la **Home** cartella. Fare clic con il pulsante destro del file cshtml e scegliere **Visualizza in controllo pagina**.

![Visualizzare cshtml in controllo pagina](using-page-inspector-in-aspnet-mvc/_static/image8.png)

Per impostazione predefinita, controllo pagina è ancorato come finestra sul lato sinistro dell'ambiente Visual Studio. Se si preferisce, è possibile ancorare la finestra in un' posizione o disancorare la finestra. Vedere [procedura: disporre e ancorare le finestre](https://msdn.microsoft.com/en-us/library/z4y0hsax.aspx).

Nel riquadro superiore della finestra di controllo pagina Mostra la pagina corrente in una finestra del browser. Nel riquadro inferiore mostra la pagina nel markup HTML, con alcune schede che consentono di controllare aspetti diversi della pagina. Il riquadro inferiore è simile al [strumenti di sviluppo F12](https://msdn.microsoft.com/en-us/ie/aa740478) in Internet Explorer.

![Applicazione ASP.NET MVC in controllo pagina](using-page-inspector-in-aspnet-mvc/_static/image10.png)

In questa esercitazione si utilizzerà il **HTML** e **stili** schede per passare rapidamente e apportare modifiche all'applicazione.

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a>Modalità EnableInspection

Per impostare controllo pagina in modalità di controllo, fare clic su di **controlla** pulsante. In modalità di controllo, quando si posiziona il puntatore del mouse su qualsiasi parte della pagina di cui è stato eseguito rendering, il markup di origine corrispondente o il codice viene evidenziata.

![Attiva/Disattiva modalità di controllo](using-page-inspector-in-aspnet-mvc/_static/image12.png)

Spostare il puntatore del mouse su parti diverse della pagina in controllo pagina. Quando si esegue, il puntatore del mouse diventa un segno più grande e viene evidenziato l'elemento sotto:

![Mouse div.content wrapper](using-page-inspector-in-aspnet-mvc/_static/image14.png)

Quando si sposta il puntatore del mouse, Visual Studio consente di evidenziare la sintassi Razor corrispondente nel file di origine. Se l'elemento HTML proviene da un altro file di origine, Visual Studio apre automaticamente il file.

In controllo pagina, il **HTML** scheda Mostra il codice HTML che è stato generato dalla sintassi Razor. Quando si sposta il puntatore del mouse, vengono evidenziati gli elementi HTML. Il **stili** scheda Visualizza le regole CSS per l'elemento.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Utilizza controllo pagina per apportare modifiche al Markup

Controllo pagina consente di trovare markup il cui percorso potrebbe non essere evidente. È quindi possibile modificare il markup e visualizzare le modifiche risultante.

Per verificarlo, fare clic su **controlla** e quindi scorrere fino alla fine della pagina nella finestra di controllo pagina.

Quando si sposta il puntatore del mouse nell'area di piè di pagina, vengono aperti il \_file cshtml ed evidenzia la sezione della pagina di layout che è stato selezionato. Come si può notare, il piè di pagina sono è definito nel file di layout e non la vista stessa.

![Piè di pagina](using-page-inspector-in-aspnet-mvc/_static/image16.png)

Spostare il puntatore del mouse sulla linea con il copyright <a id="a"> </a>nota. Nel \_cshtml pagina, la riga corrispondente viene evidenziato.

![Riga di piè di pagina copyright evidenziata](using-page-inspector-in-aspnet-mvc/_static/image18.png)

Aggiungere testo alla fine della riga di \_file cshtml.

&lt;p&gt;&amp;copiare; @DateTime.Now.Year -Applicazione ASP.NET MVC Massi!  &lt; /p&gt;

A questo punto, premere Ctrl + Alt + Invio oppure fare clic sulla barra di aggiornamento per visualizzare i risultati nella finestra del browser di controllo pagina.

![L'applicazione ASP.NET funziona](using-page-inspector-in-aspnet-mvc/_static/image20.png)

Si potrebbe pensare che il piè di pagina definite nelle cshtml, ma è stato eseguito nel \_cshtml e controllo pagina ha rilevato che per l'utente.

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Modalità di ispezione e la finestra di HTML

Successivamente, si avrà un rapido controllo di finestra di HTML e come viene eseguito il mapping elementi per l'utente.

Fare clic su **controlla** inserire controllo pagina in modalità controllo.

Fare clic sulla parte superiore della pagina su cui è indicato "Il logohere". Si sta esaminando un particolare elemento in modo più dettagliato, pertanto la visualizzazione nella finestra del browser non cambierà quando si sposta il puntatore del mouse.

Spostare il puntatore del mouse per il **HTML** finestra. Quando si sposta il puntatore del mouse, controllo pagina vengono descritti l'elemento all'interno di **HTML** finestra ed evidenziare l'elemento corrispondente nella finestra del browser.

![Finestra HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

Come in precedenza, vengono aperti il \_file cshtml automaticamente in una scheda temporanea. Fare clic su di \_scheda temporanea cshtml e il markup corrispondente verrà evidenziato nel &lt;intestazione&gt; sezione automaticamente:

![Markup evidenziato](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Anteprima modifiche CSS nella finestra stili

Successivamente, si utilizzerà il controllo pagina **stili** finestra per visualizzare in anteprima le modifiche a CSS.

Fare clic su **controlla** inserire controllo pagina in modalità controllo.

Nella finestra del browser di controllo pagina, spostare il puntatore del mouse sopra la sezione "Home Page" finché il **div.content wrapper** etichetta viene visualizzata.

![Mouse div.content wrapper](using-page-inspector-in-aspnet-mvc/_static/image26.png)

Fare clic una volta all'interno della sezione div.content wrapper, e quindi spostare il puntatore del mouse per il **stili** finestra. Il **stili** finestra Visualizza tutte le regole CSS per questo elemento. Scorrere fino a individuare il wrapper. Content .featured selettore di classe. A questo punto deselezionare la casella di controllo per la proprietà del colore di sfondo.

![Colore di sfondo cancella](using-page-inspector-in-aspnet-mvc/_static/image28.png)

Si noti come la modifica immediata anteprime nella finestra del browser di controllo pagina.

Selezionare di nuovo la casella di controllo, quindi fare doppio clic sul valore della proprietà e modificarla in rosso. La modifica viene immediatamente:

![Colore di sfondo rosso](using-page-inspector-in-aspnet-mvc/_static/image30.png)

Il **stili** rende finestra è più facile di test e visualizzare in anteprima CSS modifiche prima di applicare le modifiche allo stile di finestra stessa.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Sincronizzazione automatica CSS

> [!NOTE]
> Questa funzionalità richiede la versione 1.3 del controllo pagina.


La funzionalità di sincronizzazione automatica CSS consente di modificare direttamente un file CSS e visualizzare le modifiche immediatamente nel browser di controllo pagina.

Fare clic su **controlla** inserire controllo pagina in modalità controllo.

Nel browser di controllo pagina, spostare il puntatore del mouse sopra la sezione "Home Page" finché il **div.content wrapper** etichetta viene visualizzata. Fare clic per selezionare questo elemento.

Il **stili** finestra Visualizza tutte le regole CSS per questo elemento. Scorrere fino a individuare il wrapper. Content .featured selettore di classe. Fare clic su ".featured. Content-wrapper". Controllo pagina consente di aprire il file CSS che definisce questo stile (Site.css) ed evidenzia il CSS corrispondente.

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

Modificare il valore per `background-color` per "red". La modifica viene visualizzata immediatamente nel browser di controllo pagina.

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a>Mediante il selettore del colore CSS

L'editor CSS in Visual Studio 2012 è una selezione di colori che rende più semplice scegliere e inserire i colori. La selezione colori include una tavolozza di colori standard supporta nomi di colori standard, codici hash, i colori RGB, RGBA, HSL e HSLA e gestisce un elenco dei colori utilizzati più di recente nel documento.

Nella sezione precedente, è stato modificato il valore di `background-color` proprietà. Per richiamare il selettore di colore, posizionare il cursore dopo il nome della proprietà e il tipo  **#**  o **rgb (**.

![Barra di selezione dei colori CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

Fare clic su un colore per selezionarlo oppure premere il tasto freccia giù e quindi utilizzare i tasti freccia sinistra e destra per attraversare i colori. Quando si visita un colore, il valore esadecimale corrispondente viene visualizzato in anteprima:

![valore della proprietà colore di sfondo visualizzato in anteprima](using-page-inspector-in-aspnet-mvc/_static/image38.png)

Se la barra dei colori non è il colore esatto desiderato, è possibile utilizzare il colore selezione pop verso il basso. Per aprirlo, fare clic sulla doppia freccia di espansione all'estremità destra della barra dei colori o premere una o due volte il tasto freccia giù sulla tastiera.

![Selezione colori CSS Pop-Down](using-page-inspector-in-aspnet-mvc/_static/image40.png)

Fare clic su un colore dalla barra verticale a destra. Nella finestra principale indica una sfumatura per il colore specifico. Scegliere un colore direttamente dalla barra verticale premendo INVIO oppure fare clic su qualsiasi punto nella finestra principale di scegliere con maggiore precisione.

Se è presente un colore sullo schermo del computer che si desidera utilizzare (non è necessario all'interno dell'interfaccia utente di Visual Studio), è possibile acquisire il relativo valore utilizzando il contagocce in basso a destra.

È inoltre possibile modificare l'opacità di un colore spostando il dispositivo di scorrimento nella parte inferiore della selezione colori. In tal modo le modifiche dei colori valori RGBA, perché il formato RGBA può rappresentare l'opacità.

Dopo aver scelto un colore, premere INVIO e quindi digitare un punto e virgola per completare l'immissione di colore di sfondo nel *Site* file.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>La barra di aggiornamento del controllo pagina

Controllo pagina rileva immediatamente la modifica di *Site* file e visualizza un avviso in una barra di aggiornamento.

![Barra di aggiornamento](using-page-inspector-in-aspnet-mvc/_static/image42.png)

Per salvare tutti i file e aggiornare il browser di controllo pagina, premere Ctrl + Alt + Invio oppure fare clic sulla barra di aggiornamento. La modifica il colore di evidenziazione viene visualizzato nel browser.

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a>Mapping di elementi della pagina dinamica per JavaScript

Nelle applicazioni web moderne, gli elementi della pagina sono spesso generati dinamicamente con JavaScript. Ciò significa che non sono presenti markup statico (HTML o Razor) che corrisponde a questi elementi di pagina.

Con la versione 1.3, controllo pagina può ora mappare gli elementi che sono stati aggiunti dinamicamente alla pagina al codice JavaScript corrispondente. Per illustrare questa funzionalità, si userà il [modello di applicazione a pagina singola (SPA)](../../../single-page-application/overview/introduction/knockoutjs-template.md).

> [!NOTE]
> Il modello di SPA richiede il [ASP.NET e Web strumenti 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) aggiornare.


In Visual Studio, scegliere **File** &gt; **nuovo progetto**. A sinistra, espandere **Visual c#**selezionare **Web**, quindi selezionare **applicazione Web di ASP.NET MVC4**. Fare clic su **OK**.

Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo Seleziona **applicazione a pagina singola**.

In Esplora soluzioni, espandere il **viste** cartella e quindi la **Home** cartella. Fare clic con il pulsante destro del file cshtml e scegliere **Visualizza in controllo pagina**.

In primo luogo che è visualizzato nel browser di controllo pagina è una pagina di accesso. Fare clic su "Sign Up" e creare un nome utente e una password. Quando si esegue l'iscrizione, l'applicazione esegue l'accesso e crea un elenco di attività con alcuni elementi di esempio.

Fare clic su **controlla** inserire controllo pagina in modalità controllo. Nel browser di controllo pagina, fare clic su uno degli elementi di attività da eseguire. Si noti che invece viene evidenziato in blu, l'elemento viene evidenziato in arancione, con "JS" accanto al nome dell'elemento. Indica che l'elemento è stato creato in modo dinamico tramite script.

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

Inoltre, è presente un carattere di sottolineatura arancione il **Stack di chiamate** scheda. Ciò indica che il **Stack di chiamate** riquadro è disponibili altre informazioni sull'elemento.

Fare clic su di **Stack di chiamate** scheda. Il **Stack di chiamate** riquadro mostra lo stack di chiamate per la chiamata di JavaScript che ha creato l'elemento. Chiamate a librerie esterne, ad esempio jQuery sono compressi, in modo che è possibile visualizzare facilmente le chiamate per lo script dell'applicazione.

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

Per visualizzare lo stack completo, incluse le chiamate a librerie esterne, è possibile espandere i nodi con etichettati "Librerie esterne":

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

Se si fa clic su un elemento nello stack di chiamate, Visual Studio apre il file di codice ed evidenzia script corrispondenti.

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a>Vedere anche

[Introduzione ad ASP.NET MVC 4 con Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (sito Web ASP.net)

[Introduzione a controllo pagina](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (video di Channel 9)

[Messaggi di errore di Page Inspector](https://go.microsoft.com/?linkid=9813062) (MSDN)
