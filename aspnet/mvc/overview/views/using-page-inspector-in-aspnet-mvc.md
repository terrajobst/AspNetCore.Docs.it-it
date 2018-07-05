---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: Utilizzo di controllo pagina in ASP.NET MVC | Microsoft Docs
author: rick-anderson
description: Controllo pagina in Visual Studio 2012 è uno strumento di sviluppo web con un browser integrato. Selezionare qualsiasi elemento nel browser integrato e controllo pagina i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 032be78e744bdf2c74337c72afb4efb7471ae4f9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399143"
---
<a name="using-page-inspector-in-aspnet-mvc"></a>Utilizzo di Controllo pagina in ASP.NET MVC
====================
da Tim Ammann

> Controllo pagina in Visual Studio 2012 è uno strumento di sviluppo web con un browser integrato. Selezionare un elemento nel browser integrato e controllo pagina evidenzia immediatamente l'origine e CSS dell'elemento. È possibile esplorare tutte le visualizzazioni MVC, rapidamente trovare le origini di markup sottoposto a rendering e usare gli strumenti del browser direttamente all'interno dell'ambiente di Visual Studio.
> 
> [Guarda il Video](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> Questa esercitazione illustra come abilitare la modalità controllo e quindi individuare e modificare rapidamente CSS e markup all'interno del progetto web. L'esercitazione Usa un progetto MVC, ma è anche possibile utilizzare controllo pagina per [Web Form](https://go.microsoft.com/?linkid=9802001) e altre applicazioni ASP.NET.
> 
> L'esercitazione include le sezioni seguenti:
> 
> - [Prerequisiti](#_1_prerequisites)
> - [Creare un'applicazione Web](#_2_creating_a)
> - [Utilizzare controllo pagina per passare a una vista](#_3_using_page)
> - [Abilitare la modalità controllo](#_4_inspection_mode)
> - [Utilizzare controllo pagina per apportare modifiche al Markup](#_5_using_page)
> - [Modalità controllo e la finestra HTML](#_6_inspection_mode)
> - [Anteprima modifiche CSS nella finestra stili](#_7_previewing_css)
> - [Sincronizzazione automatica CSS](#css_auto_sync)
> - [Tramite la selezione colori CSS](#css_color_picker)
> - [Mapping di elementi della pagina dinamica a JavaScript](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Prerequisiti

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) oppure [Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Per ottenere la versione più recente di controllo pagina, usare [instalace Webové Platformy](https://go.microsoft.com/fwlink/?LinkId=255386) per installare Windows Azure SDK per .NET 2.0.


Page Inspector è abbinato a Microsoft Web Developer Tools. La versione più recente è 1.3x. Per verificare quale versione hanno, eseguire Visual Studio e selezionare **informazioni su Microsoft Visual Studio** dal **Guida** menu.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Creare un'applicazione Web

Innanzitutto, creare un'applicazione web che verrà utilizzato il controllo di pagina con. In Visual Studio, scegliere **File** &gt; **nuovo progetto**. A sinistra, espandere **Visual c#**, selezionare **Web**, quindi selezionare **applicazione Web di ASP.NET MVC4**.

![Nuova applicazione MVC ASP.NET](using-page-inspector-in-aspnet-mvc/_static/image2.png)

Fare clic su **OK**.

Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo **applicazione Internet**. Lasciare **Razor** come motore di visualizzazione predefinito.

![Nuovo progetto ASP.NET MVC - applicazione Internet](using-page-inspector-in-aspnet-mvc/_static/image4.png)

L'applicazione viene aperto in **origine** visualizzazione.

![Nuova applicazione MVC ASP.NET nella visualizzazione origine](using-page-inspector-in-aspnet-mvc/_static/image6.png)

Dopo aver creato un'applicazione di usare, è possibile utilizzare controllo pagina per esaminare e modificarlo.

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a>Utilizzare controllo pagina per passare a una vista

In Visual Studio 2012, è possibile fare doppio clic qualsiasi visualizzazione nel progetto, seleziona **Visualizza in controllo pagina**, controllo pagina verrà capire la route e visualizzare la pagina.

Nelle **Esplora soluzioni**, espandere il **viste** cartella e quindi il **Home** cartella. Fare clic con il pulsante destro il file index. cshtml e scegliere **Visualizza in controllo pagina**.

![Visualizzazione index. cshtml in controllo pagina](using-page-inspector-in-aspnet-mvc/_static/image8.png)

Per impostazione predefinita, controllo pagina è ancorato come finestra sul lato sinistro dell'ambiente di Visual Studio. Se si preferisce, è possibile ancorarlo altrove o disancorare la finestra. Visualizzare [procedura: disporre e ancorare Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).

Il riquadro superiore della finestra di controllo pagina Mostra la pagina corrente in una finestra del browser. Nel riquadro inferiore mostra la pagina nel markup HTML con alcune schede che consentono di controllare diversi aspetti della pagina. Il riquadro inferiore è simile al [strumenti di sviluppo F12](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.

![Applicazione ASP.NET MVC in controllo pagina](using-page-inspector-in-aspnet-mvc/_static/image10.png)

In questa esercitazione si userà il **HTML** e **stili** schede per passare rapidamente e apportare modifiche all'applicazione.

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a>Modalità EnableInspection

Per attivare la modalità controllo di controllo pagina, scegliere il **Inspect** pulsante. In modalità controllo, quando si posiziona il puntatore del mouse su un punto qualsiasi della pagina di cui è stato eseguito rendering, il codice o markup di origine corrispondente viene evidenziato.

![Modalità controllo Attiva/disattiva](using-page-inspector-in-aspnet-mvc/_static/image12.png)

A questo punto passa il mouse su parti diverse della pagina in controllo pagina. Come avviene, il puntatore del mouse diventa un segno più grande e viene evidenziato l'elemento di sotto:

![Posizionare il mouse su div.content wrapper](using-page-inspector-in-aspnet-mvc/_static/image14.png)

Quando si sposta il puntatore del mouse, Visual Studio evidenzia la sintassi Razor corrispondente nel file di origine. Se l'elemento HTML proviene da un altro file di origine, Visual Studio apre automaticamente il file.

In controllo pagina, il **HTML** scheda Mostra il codice HTML generato dalla sintassi Razor. Quando si sposta il puntatore del mouse, vengono evidenziati gli elementi HTML. Il **stili** scheda Mostra le regole CSS per l'elemento.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Utilizzare controllo pagina per apportare modifiche al Markup

Page Inspector consente di trovare markup la cui posizione potrebbe non essere evidente. È possibile modificare il markup e visualizzare le modifiche risultante.

Per verificarlo, fare clic su **Inspect** e quindi scorrere fino alla parte inferiore della pagina nella finestra di controllo pagina.

Quando si sposta il puntatore del mouse nell'area di piè di pagina, controllo pagina apre il \_file di layout. cshtml ed evidenzia la sezione della pagina di layout che è stata selezionata. Come si può notare, sono il piè di pagina è definita nel file di layout e non la visualizzazione stessa.

![Piè di pagina](using-page-inspector-in-aspnet-mvc/_static/image16.png)

Ora passare il puntatore del mouse sulla riga con il copyright <a id="a"> </a>notare. Nel \_pagina layout. cshtml, la riga corrispondente viene evidenziato.

![Riga di piè di pagina informazioni sul copyright evidenziata](using-page-inspector-in-aspnet-mvc/_static/image18.png)

Aggiungere testo alla fine della riga di \_file di layout. cshtml.

&lt;p&gt;&amp;duplicarlo; @DateTime.Now.Year -Applicazione ASP.NET MVC Rocks!  &lt; /p&gt;

A questo punto, premere Ctrl + Alt + INVIO o fare clic sulla barra di aggiornamento per visualizzare i risultati nella finestra del browser di controllo pagina.

![Funziona l'applicazione ASP.NET!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

Si potrebbe avrai pensato che il piè di pagina definite in Index. cshtml, ma si è scoperto nel \_layout. cshtml e controllo pagina ha rilevato che per l'utente.

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Modalità controllo e la finestra HTML

Successivamente, si avrà un'occhiata alla finestra HTML e il relativo mapping elementi per l'utente.

Fare clic su **Inspect** inserire controllo pagina in modalità controllo.

Fare clic sulla parte superiore della pagina con la dicitura "Il logohere". Se si sta esaminando un particolare elemento in modo più dettagliato, in modo che la visualizzazione nella finestra del browser non è più viene modificato quando si sposta il puntatore del mouse.

Ora passare il puntatore del mouse per il **HTML** finestra. Quando si sposta il puntatore del mouse, controllo pagina vengono illustrati l'elemento all'interno di **HTML** finestra ed evidenzia l'elemento corrispondente nella finestra del browser.

![Finestra HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

Come in precedenza, controllo pagina apre il \_file di layout. cshtml per l'utente in una scheda temporanea. Fare clic sui \_layout. cshtml scheda temporanea e il markup corrispondente verrà evidenziato nel &lt;intestazione&gt; sezione per l'utente:

![Markup evidenziato](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Anteprima modifiche CSS nella finestra stili

Successivamente, si userà la Page Inspector **stili** finestra per visualizzare in anteprima le modifiche a CSS.

Fare clic su **Inspect** inserire controllo pagina in modalità controllo.

Nella finestra del browser di controllo pagina, spostare il puntatore del mouse sopra la sezione "Home Page" finché il **div.content wrapper** etichetta viene visualizzata.

![Posizionare il mouse su div.content wrapper](using-page-inspector-in-aspnet-mvc/_static/image26.png)

Fare clic su una sola volta all'interno della sezione div.content wrapper, e quindi spostare il puntatore del mouse per il **stili** finestra. Il **stili** finestra Mostra tutte le regole CSS per questo elemento. Scorrere verso il basso il selettore di classe find .featured. Content-wrapper. A questo punto deselezionare la casella di controllo per la proprietà di colore di sfondo.

![Colore di sfondo chiaro](using-page-inspector-in-aspnet-mvc/_static/image28.png)

Si noti come la modifica include l'anteprima immediatamente nella finestra del browser di controllo pagina.

Selezionare la casella di controllo di nuovo, quindi fare doppio clic sul valore della proprietà e modificarlo in rosso. La modifica viene immediatamente:

![Colore di sfondo rosso](using-page-inspector-in-aspnet-mvc/_static/image30.png)

Il **stili** rende finestra è più facile testare e visualizzare in anteprima CSS viene modificato prima di procedere effettivamente le modifiche apportate allo stile della finestra stessa.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Sincronizzazione automatica CSS

> [!NOTE]
> Questa funzionalità richiede la versione 1.3 di controllo pagina.


La funzionalità di sincronizzazione automatica CSS consente di modificare direttamente un file CSS e visualizzare le modifiche immediatamente nel browser di controllo pagina.

Fare clic su **Inspect** inserire controllo pagina in modalità controllo.

Nel browser di controllo pagina, spostare il puntatore del mouse sopra la sezione "Home Page" finché il **div.content wrapper** etichetta viene visualizzata. Fare clic su una sola volta per selezionare questo elemento.

Il **stili** finestra Mostra tutte le regole CSS per questo elemento. Scorrere verso il basso il selettore di classe find .featured. Content-wrapper. Fare clic su .featured. Content-"wrapper". Controllo pagina apre il file CSS che definisce questo stile (CSS) ed evidenzia il CSS corrispondente.

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

A questo punto modificare il valore per `background-color` su "red". La modifica viene visualizzata immediatamente nel browser di controllo pagina.

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a>Tramite la selezione colori CSS

Editor CSS in Visual Studio 2012 è una selezione colori che consente di scegliere e inserire i colori. La selezione colori include una tavolozza di colori standard supporta i nomi dei colori standard, codici hash, i colori RGB, RGBA, HSL e HSLA e gestisce un elenco dei colori utilizzati più di recente nel documento.

Nella sezione precedente, è stato modificato il valore di `background-color` proprietà. Per richiamare la selezione colori, posizionare il cursore dopo il nome della proprietà e digitare **#** oppure **rgb (**.

![La barra di selezione colori CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

Fare clic su un colore per selezionarlo oppure premere il tasto freccia giù e quindi utilizzare i tasti freccia sinistra e destra per attraversare i colori. Quando si visita un colore, viene visualizzato in anteprima il corrispondente valore esadecimale:

![valore della proprietà di colore di sfondo visualizzata in anteprima](using-page-inspector-in-aspnet-mvc/_static/image38.png)

Se la barra dei colori non è il colore esatto desiderato, è possibile usare la selezione di colore pop-down. Per aprirlo, fare clic sulle virgolette doppie all'estremità destra della barra dei colori o premere una o due volte il tasto freccia giù sulla tastiera.

![Selezione di colore CSS Pop-Down](using-page-inspector-in-aspnet-mvc/_static/image40.png)

Fare clic su un altro colore nella barra verticale sul lato destro. Mostra una sfumatura di colore nella finestra principale. Scegliere un colore direttamente dalla barra degli strumenti verticale premendo INVIO oppure fare clic su qualsiasi punto nella finestra principale di scegliere con precisione maggiore.

Se è presente un colore sullo schermo del computer che si desidera utilizzare (non deve necessariamente essere all'interno dell'interfaccia utente di Visual Studio), è possibile acquisire il relativo valore con lo strumento contagocce in basso a destra.

È anche possibile modificare l'opacità di colore spostando il dispositivo di scorrimento nella parte inferiore della selezione colori. In questo modo le modifiche dei colori dei valori di valori RGBA, perché il formato RGBA può rappresentare l'opacità.

Dopo aver scelto un colore, premere INVIO e quindi digitare un punto e virgola per completare la voce di colore di sfondo nel *CSS* file.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>La barra di aggiornamento di controllo pagina

Controllo pagina immediatamente rileva la modifica per il *CSS* file e viene visualizzato un avviso in una barra di aggiornamento.

![Barra di aggiornamento](using-page-inspector-in-aspnet-mvc/_static/image42.png)

Per salvare tutti i file e aggiornare il browser di controllo pagina, premere Ctrl + Alt + INVIO o fare clic sulla barra di aggiornamento. La modifica il colore di evidenziazione viene visualizzata nel browser.

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a>Mapping di elementi della pagina dinamica a JavaScript

Nelle applicazioni web moderne, gli elementi della pagina vengono spesso generati in modo dinamico con JavaScript. Ciò significa che non sono presenti markup statici, HTML o Razor, che corrisponde a questi elementi di pagina.

Con la versione 1.3, controllo pagina può ora eseguire il mapping di elementi che sono stati aggiunti in modo dinamico alla pagina di tornare al codice JavaScript corrispondente. Per illustrare questa funzionalità, si userà il [modello di applicazione a pagina singola (SPA)](../../../single-page-application/overview/introduction/knockoutjs-template.md).

> [!NOTE]
> Il modello di applicazione a singola pagina richiede la [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) aggiornare.


In Visual Studio, scegliere **File** &gt; **nuovo progetto**. A sinistra, espandere **Visual c#**, selezionare **Web**, quindi selezionare **applicazione Web di ASP.NET MVC4**. Fare clic su **OK**.

Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo, seleziona **applicazione a pagina singola**.

In Esplora soluzioni espandere la **viste** cartella e quindi la **Home** cartella. Fare clic con il pulsante destro il file index. cshtml e scegliere **Visualizza in controllo pagina**.

Il primo elemento visualizzato nel browser di controllo pagina è una pagina di accesso. Fare clic su "Iscrizione" e creare un nome utente e password. Quando si effettua l'iscrizione, l'applicazione esegue l'accesso e crea un elenco di cose da fare con alcuni elementi di esempio.

Fare clic su **Inspect** inserire controllo pagina in modalità controllo. Nel browser di controllo pagina, fare clic su uno degli elementi di attività da eseguire. Si noti che invece viene evidenziato in blu, l'elemento viene evidenziato in arancione, con "JS" accanto al nome dell'elemento. Ciò indica che l'elemento è stato creato in modo dinamico tramite script.

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

Inoltre, in cui viene visualizzata una sottolineatura arancione nel **Stack di chiamate** scheda. Ciò indica che il **Stack di chiamate** riquadro contiene ulteriori informazioni sull'elemento.

Fare clic sui **Stack di chiamate** scheda. Il **Stack di chiamate** riquadro mostra lo stack di chiamate per la chiamata di JavaScript che ha creato l'elemento. Le chiamate a librerie esterne, ad esempio jQuery vengono compresse, in modo che è possibile visualizzare facilmente le chiamate per lo script di applicazione.

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

Per visualizzare lo stack completo, comprese chiamate alle librerie esterne, è possibile espandere i nodi con etichettati "Librerie esterne":

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

Se si fa clic su un elemento nello stack di chiamate, Visual Studio apre il file di codice e viene evidenziato lo script corrispondente.

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a>Vedere anche

[Introduzione ad ASP.NET MVC 4 con Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (sito Web ASP.net)

[Introduzione a controllo pagina](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (video di Channel 9)

[I messaggi di errore di controllo pagina](https://go.microsoft.com/?linkid=9813062) (MSDN)
