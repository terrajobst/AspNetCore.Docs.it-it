---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: Novità di ASP.NET e lo sviluppo Web in Visual Studio 2012 | Documenti Microsoft
author: rick-anderson
description: La nuova versione di Visual Studio introduce una serie di miglioramenti finalizzate soprattutto a migliorare l'esperienza e le prestazioni quando si lavora con tecnologie Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: f447dc0108dffb36ed6d627fb83b3117fd22c94c
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/18/2018
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a>Novità di ASP.NET e lo sviluppo Web in Visual Studio 2012
====================
Da [categorie Web Team](https://twitter.com/webcamps)

> La nuova versione di Visual Studio introduce una serie di miglioramenti finalizzate soprattutto a migliorare l'esperienza e le prestazioni quando si lavora con tecnologie Web. Editor di Visual Studio per CSS, JavaScript e HTML sono completamente rinnovato per includere numerose facilita il codice più richiesta, ad esempio IntelliSense e il rientro automatico. Relativamente alle prestazioni, aggregazione e minimizzazione ora sono integrati, come tempo di caricamento di funzionalità incorporate per ridurre facilmente la pagina.
> 
> Visual Studio consente di lavorare con le tecnologie del sito Web più recenti. È possibile utilizzare frammenti di codice CSS3 tra browser per verificare che il sito funziona indipendentemente dalla piattaforma client sfruttando anche i nuovi elementi HTML5 e le funzionalità.
> 
> Scrittura e la profilatura del codice JavaScript deve essere più semplice con questa versione di Visual Studio. Gli elenchi di IntelliSense, funzionalità di documentazione e navigazione XML integrate sono ora disponibili per il codice JavaScript. È ora disponibile il catalogo JavaScript a portata di mano. Inoltre, è possibile controllare la conformità di ECMAScript5 con gli script e rilevare gli errori di sintassi in fase iniziale.
> 
> Ultimo, ma non meno importante, questa versione di Visual Studio implementa minimizzazione e aggregazione predefiniti. Il file di script e fogli di stile verranno compressa e compressi in modo che il sito è più veloce.
> 
> Questa esercitazione illustra i miglioramenti e nuove funzionalità descritte in precedenza tramite l'applicazione di modifiche di lieve entità a un'applicazione Web di esempio fornita nella cartella di origine.
> 
> Tutto il codice di esempio e i frammenti di codice sono inclusi nel Web categorie Training Kit, disponibile all'indirizzo [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico, si apprenderà come:

- Utilizzare le nuove funzionalità e miglioramenti nell'editor CSS
- Utilizzare le nuove funzionalità e miglioramenti nell'editor HTML
- Usare le nuove funzionalità e miglioramenti nell'editor JavaScript
- Configurare l'aggregazione e riduzione

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

- [Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (lettura [appendice](#AppendixA) per istruzioni su come installarlo).
- [Windows PowerShell](https://support.microsoft.com/kb/968930/) (per gli script di installazione - già installati in Windows 8 e Windows Server 2008 R2)
- [Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - o un browser compatibile con HTML5

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questo laboratorio pratico include gli esercizi seguenti:

1. [Esercizio 1: Novità nell'Editor CSS](#Exercise1)
2. [Esercizio 2: Novità nell'Editor HTML](#Exercise2)
3. [Esercizio 3: Novità nell'Editor JavaScript](#Exercise3)
4. [Esercizio 4: Bundling and Minification](#Exercise4)

Tempo stimato per completare questa esercitazione: **60 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a>Esercizio 1: Novità nell'Editor CSS

Gli sviluppatori Web devono avere familiari con molte delle difficoltà relative alla modifica di CSS. Uno dei principali problemi di applicazione di stili CSS è la compatibilità browser. Accade spesso che, dopo l'applicazione di stili al sito, noterai che l'aspetto diverso se si apre in un altro browser o nel dispositivo. Pertanto, si potrebbe impiegano molto tempo in risolvere tali problemi visual per tenere presente che, quando si apportano infine funziona in un browser, viene suddiviso in altre.

Visual Studio ora include funzionalità che consentono agli sviluppatori di accedere, gestire e organizzare in modo efficace i fogli di stile CSS. In questa esercitazione, si soddisferà le nuove funzionalità per un'organizzazione efficace e l'edizione, nonché i frammenti di codice CSS3 per la compatibilità browser.

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a>Attività 1 - nuove funzionalità dell'Editor

In questa attività, sarà possibile osservare le nuove funzionalità dell'Editor CSS. Questo nuovo editor consente di aumentare la produttività, sfruttando il rientro smart nuovo, i commenti del codice migliore e l'elenco di IntelliSense migliorato.

1. Avviare **Visual Studio** e aprire il **WhatsNewASPNET.sln** soluzione si trova nel **Source\WhatsNewASPNET** cartella di questa esercitazione.
2. In Esplora soluzioni, aprire il **Site.css** file si trova sotto il **stili** cartella. Verificare che il **Editor di testo** strumenti sono visibili nella barra degli strumenti. A tale scopo, selezionare il **vista** | **barre degli strumenti** opzione di menu, quindi individuare il **Editor di testo** opzioni. Si noterà che, poiché questa nuova versione, il **commento** pulsante (![pulsante Commento](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) e **Rimuovi commento** pulsante (![rimuovere il commento-pulsante](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) sono abilitate anche per l'editor CSS.

    ![Abilitazione di Editor e strumenti CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "abilitazione Editor e strumenti CSS")

    *Abilitazione di Editor e strumenti CSS*
3. Scorrere il codice e selezionare qualsiasi definizione di classe CSS. Fare clic su di **commento** (![pulsante Commento](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) pulsante per commentare le righe selezionate. Quindi, fare clic su di **Rimuovi commento** (![pulsante rimuovere il commento](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) pulsante per annullare le modifiche.
4. Fare clic su di **comprimere** (![comprimere](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) e **Espandi** (![espandere](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) pulsanti si trovano sul margine sinistro del testo. Si noti che è ora possibile nascondere gli stili che non consentono di disporre di una visualizzazione più chiara.

    ![Compressione di classi CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "classi CSS compressione")

    *Compressione di classi CSS*
5. Assicurarsi che sia abilitata la funzionalità smart rientro. Selezionare il **strumenti** | **opzioni** opzione di menu e quindi selezionare il **Editor di testo** | **CSS**  |  **Formattazione** pagina nel riquadro a sinistra della schermata. Controllare il **rientro gerarchico** opzione.

    ![L'abilitazione di rientro gerarchico](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "abilitazione rientro gerarchico")

    *L'abilitazione di rientro gerarchico*
6. Individuare la definizione di classe principale (.main) e aggiungere uno stile per gli elementi div. Si noterà che il codice consente di allineare automaticamente, rendere gli utenti per trovare l'elemento padre, le classi a colpo d'occhio.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    ![Allineamento gerarchico in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "allineamento gerarchico in CSS")

    *Allineamento gerarchico in CSS*
7. All'interno di **.main div** classe, posizionare il cursore alla fine di **border: 0px;** e premere **invio** per visualizzare l'elenco di IntelliSense. Iniziare a digitare **top** e notare come l'elenco viene filtrato durante la digitazione. Nell'elenco vengono visualizzati gli elementi che contengono **top** in qualsiasi parte della parola (nelle versioni precedenti di Visual Studio, l'elenco viene filtrato per gli elementi che *iniziare* con il termine).

    ![Miglioramenti di IntelliSense in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "miglioramenti di IntelliSense in CSS")

    *Miglioramenti di IntelliSense in CSS*

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a>Attività 2: la selezione colori

In questa attività, sarà possibile osservare che la nuova selezione colori CSS integrato in Visual Studio IntelliSense.

1. In **Site,** individuare la definizione di classe di intestazione (.header) e posizionare il cursore accanto a **colore di sfondo** attributo, tra il &quot;:&quot; e &quot; # &quot; caratteri nella riga di codice **.**

    ![Individuazione del cursore](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "individuazione il cursore")

    *Individuazione del cursore*
2. Eliminare il **virgola** (:) e la scrittura di nuovo per visualizzare la selezione colori. Si noti che i colori di primo che verranno visualizzati i colori più frequenti del sito. Se si sceglie il colore bianco, il codice di colore HTML (#fff) sostituirà il codice colore corrente nel foglio di stile.

    ![Selezione dei colori](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "selezione dei colori")

    *Selezione dei colori*
3. Premere il **Espandi** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) pulsante sul selettore di colore per visualizzare la sfumatura di colore e quindi trascinare il cursore sfumato per selezionare un colore diverso. Successivamente, fare clic su di **contagocce** e selezionare qualsiasi colore dalla schermata. Si noti che il valore di colore di sfondo cambia dinamicamente mentre si sposta il cursore.

    ![Sfumatura di colore selezione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "sfumatura di colore selezione")

    *Sfumatura di colore selezione*
4. Nel **opacità** dispositivo di scorrimento, spostare il selettore al centro della barra per ridurre l'opacità. Si noti che il valore di colore di sfondo viene visualizzata la scala RGBA.

    ![Selezione dei colori opacità](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "selezione colori opacità")

    *Selezione dei colori opacità*

    > [!NOTE]
    > La definizione di colore RGBA (rosso, verde, blu e alfa) in CSS3 consente di definire il valore di opacità del colore per un singolo elemento. A differenza di **opacità -** un attributo CSS simile **-** colori RGBA sono inoltre compatibili con i browser più recenti.

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a>Attività 3 - frammenti di codice compatibile di CSS

In questa attività si apprenderà come usare i frammenti di codice CSS3 compatibile tra browser per implementare alcune funzionalità del sito Web.

1. Nel **Site.css** file, individuare il **intestazione** CSS (.header) di definizione di classe e posizionare il cursore sotto il **/ \*radius bordo\* /** segnaposto per aggiungere un nuovo frammento di codice. Premere **invio** per visualizzare l'elenco di IntelliSense e il tipo **radius** per filtrare l'elenco. Selezionare il **border-radius** opzione dall'elenco con doppio clic e quindi premere il **scheda** chiave per inserire il frammento di codice. Quindi, immettere una dimensione di radius in pixel e quindi premere **invio**. Ad esempio, digitare **15px**.

    Gli attributi CSS3 aggiunti per il frammento di codice verranno eseguito il rendering di bordi arrotondati nella maggior parte dei browser conformità HTML5, tra cui Mozilla e browser basato su WebKit.

    ![Usando un frammento di codice border-radius](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "usando un frammento di codice border-radius")

    *Usando un frammento di codice border-radius*
2. Applicare lo stesso **bordo** frammenti di codice lo stile della pagina (.page).

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. Premere **F5** per eseguire la soluzione. Si noti che ogni pagina ora arrotondati i bordi.

    ![Gli angoli arrotondati](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "gli angoli arrotondati")

    *Angoli arrotondati*
4. Chiudere il browser e tornare a Visual Studio.
5. Aprire il **Custom.css** file si trova sotto il **stili** cartella e posizionare il cursore all'interno di **div.images ul li img** definizione di classe.
6. Premere INVIO per visualizzare l'elenco di IntelliSense, tipo **ombreggiatura di casella** e premere il **scheda** tasto due volte per inserire il frammento di codice predefinito shadow nella definizione di classe. Impostare i valori di ombreggiatura **10px 10px 5px 888 #**. Digitare quindi **border-radius** e inserire il frammento di codice. Tipo **15px** per impostare dimensioni radius e premere **invio**.

    ![Arrotondare gli angoli con ombreggiatura](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "rinnovato con ombreggiatura")

    *Angoli arrotondati con ombreggiatura*

    > [!NOTE]
    > In questo momento, l'attributo shadow viene inserita con il prefisso corrispondente (moz, webkit, o) per supportare Mozilla e i browser Webkit (Chrome, Safari, Konkeror).
7. Creare una nuova classe **div.images ul li img:hover** sotto il **div.images ul li img** definizione di classe e posizionare il cursore all'interno delle parentesi **.**

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. Tipo **trasformare** e premere il **scheda** chiave due volte per inserire il frammento di trasformazione. Quindi, immettere **rotate(-15deg)** per modificare il valore dell'angolo di rotazione quando si passa le immagini.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. Premere **F5** per eseguire la soluzione e passare alla pagina di CSS3. Si noti che le immagini sono gli angoli arrotondati e casella ombreggiature. Posizionare il mouse sopra le immagini ed esaminarne le operazioni di rotazione.

    ![Trasformare la rotazione di un'immagine di frammento](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "frammento trasformazione la rotazione di un'immagine")

    *Trasformare la rotazione di un'immagine del frammento di codice*

    > [!NOTE]
    > Se si utilizza Internet Explorer 10 e non è possibile visualizzare le ombreggiature, verificare che sia impostata la modalità documento degli standard di Internet Explorer 10. Premere **F12** aprire Strumenti di sviluppo di Internet Explorer e fare clic su **modalità documento** per passare alla finestra standard di IE10.

    ![about-us](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a>Esercizio 2: Novità nell'Editor HTML

Visual Studio è un editor HTML migliorato. Alcuni dei miglioramenti inclusi in questa versione sono rientro intelligente nei documenti HTML, frammenti di codice HTML5, inizio HTML e corrispondenza dei tag di fine e la convalida HTML. In questa esercitazione, verrà visualizzato come queste modifiche migliorano la proprietà quando si utilizza il codice del sito Web.

Come l'editor CSS, editor HTML è stata migliorata. Molti di questi miglioramenti sono più piccole che semplificano la vita dello sviluppatore Web. Elementi come più frammenti di codice per HTML5, rientro smart tag di inizio e fine di corrispondenza quando si modifica e destinazione del documento HTML DOCTYPE convalida sono riportati alcuni di questi miglioramenti.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a>Attività 1 - convalida migliorate DOCTYPE

Editor HTML è ora la possibilità di controllare il tipo di documento della pagina, anche se potrebbe essere la definizione della pagina master. In base al tipo di documento della pagina, l'editor HTML verrà convalidato con il set corretto di regole e verrà filtrato l'elenco di IntelliSense, prendere in considerazione gli elementi di tipo di documento.

In questa attività si modificherà la dichiarazione DOCTYPE di una pagina per vedere come il comportamento dell'editor HTML cambia di conseguenza.

1. Se non è già aperto, avviare **Visual Studio** e aprire il **WhatsNewASPNET.sln** soluzione si trova nel **Source\WhatsNewASPNET** cartella di questa esercitazione.
2. Aprire il **Site. master** pagina.
3. Si noti lo Schema di destinazione per la convalida della barra degli strumenti. Il comportamento dell'editor HTML (convalida, IntelliSense, e così via) verrà modificato in modo corretto in base al tipo di documento selezionato.

    ![Utilizza Doctype nella barra degli strumenti modifica dell'origine HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Doctype utilizzare nella barra degli strumenti modifica dell'origine HTML")

    *Utilizza Doctype nella barra degli strumenti modifica dell'origine HTML*
4. Modificare lo Schema di destinazione in formato HTML 4.01.

    ![Modifica tipo di documento nella barra degli strumenti modifica dell'origine HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "modifica Doctype nella barra degli strumenti modifica dell'origine HTML")

    *Modifica tipo di documento nella barra degli strumenti modifica dell'origine HTML*
5. Posizionare il cursore sotto il **corpo** elemento e iniziare a digitare il nome di un elemento HTML5 (ad esempio, **video**). Si noti che l'elemento non è disponibile nell'elenco di IntelliSense.

    ![Elementi HTML5 non elencati](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "elementi HTML5 non elencati")

    *Elementi HTML5 non elencati*
6. Annullare le modifiche allo Schema di destinazione per la convalida della barra degli strumenti, selezione del tipo di documento: XHTML5 nell'elenco a discesa.

    ![Utilizza Doctype nella barra degli strumenti modifica dell'origine HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Doctype utilizzare nella barra degli strumenti modifica dell'origine HTML")

    *Reimpostare Doctype nella barra degli strumenti modifica dell'origine HTML*
7. Posizionare il cursore sotto il **corpo** elemento e iniziare a digitare di nuovo un elemento HTML5 (ad esempio, come **video**). Si noti che gli elementi di HTML5 sono ora disponibili nell'elenco di IntelliSense.

    ![Elementi HTML5 elencati](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "elementi HTML5 elencati")

    *Elementi HTML5 elencati*

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a>Attività 2: inizio/fine tag aggiornamento automatico

Ora, Visual Studio aggiorna il codice HTML di apertura o chiusura di tag dell'elemento che si sta modificando in modo che corrisponda a loro. Questa nuova funzionalità la produttività quando si modifica il tag HTML.

1. Nel **Default.aspx** pagina, aggiungere un **H3** elemento con un titolo (ad esempio, Visual Studio 2012 funziona).

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. Modifica il **H3** tag e il tipo **H2** o **H1.**

    Si noti che il tag di fine viene automaticamente aggiornato. È inoltre possibile modificare il tag di fine per verificare che il tag di inizio Aggiorna di conseguenza troppo.

    ![L'aggiornamento automatico del tag di fine](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "aggiornamento automatico del tag di fine")

    *Aggiornamento automatico del tag di fine*

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a>Attività 3 - nuovi frammenti di codice HTML5

Visual Studio ora include diversi frammenti di codice HTML5. In questa attività, si utilizzerà alcuni di questi frammenti di codice.

1. Aggiungere una nuova cartella denominata **audio** alla radice della cartella del sito web. Aprire Esplora risorse e copiare tutti i file audio nel **audio** cartella la **WhatsNewASPNET.sln** soluzione.
2. Nel **Default.aspx** pagina, posizionare il cursore sotto il Massi Web11!! Intestazione. Tipo **audio** e premere il tasto TAB.

    Il nuovo editor HTML include frammenti di codice per il contenuto di HTML5. Ricordarsi di utilizzare la definizione del tipo di documento appropriata per abilitare i frammenti di codice HTML5.

    ![Inserimento di frammenti di codice HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "inserimento di frammenti di codice HTML5")

    *Inserimento di frammenti di codice HTML5*
3. Aggiornare l'origine audio in modo che punti a un file audio esistente.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > È necessario aggiungere il file audio alla soluzione.
4. Premere **F5** per eseguire il sito e riprodurre l'audio.

    ![Esecuzione del controllo audio](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "esecuzione del controllo audio")

    *Esecuzione del controllo audio*

    > [!NOTE]
    > È anche possibile provare più frammenti di codice inclusi in Visual Studio, ad esempio video, nella figura, e così via.
5. A questo punto, provare a inserire un controllo in una parte della pagina. Ad esempio, il tentativo di inserire un **GridView** (controllo), ma anziché digitare  **&lt;Gri,** inizia a digitare  **&lt;GV**. Si noti che l'elenco di IntelliSense mostra il **asp: GridView** controllo.

    IntelliSense nell'Editor HTML offre ora ricerca iniziali maiuscole, nonché parziale corrispondente (il recupero di tutti gli elementi che contiene il termine).

    ![Inserimento di un controllo GridView con IntelliSense Elenca](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "inserimento di un controllo GridView con gli elenchi di IntelliSense")

    *Inserimento di un controllo GridView con gli elenchi di IntelliSense*

    Se si digita  **&lt;griglia** verranno visualizzati tutti gli elementi che corrispondono al termine, ma Visual Studio verrà suggerito il **gridview** controllo:

    ![Inserimento di un controllo GridView con gli elenchi di IntelliSense e di corrispondenza parziale](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "inserimento di un controllo GridView con corrispondenza parziale e gli elenchi di IntelliSense")

    *Inserimento di un controllo GridView con corrispondenza parziale e gli elenchi di IntelliSense*

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a>Attività 4: Editor HTML Smart tag

Un altro miglioramento all'Editor HTML è la funzionalità Smart tag. Gli smart tag rendono più semplice eseguire attività di sviluppo comuni o ripetitive in base a livello di controllo. Questa funzionalità era già disponibile nella finestra di progettazione HTML, ma non nell'Editor HTML.

1. Aprire **Site. master** e individuare il **asp: Menu** elemento. Posizionare il cursore sul tag di inizio e si noti che l'icona piccola visualizzata nella parte inferiore dell'elemento - fare clic per aprire il menu smart tag. Si noti che si può accedere rapidamente ad alcune attività correlate al controllo Menu.

    ![Attività per il controllo Menu Smart](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Smart attività per il controllo Menu")

    *Smart task per il controllo Menu*

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a>Attività 5 - rientri intelligenti

Una delle procedure consigliate in formato HTML è il rientro di elementi annidati per mantenere il codice come leggibili. In Visual Studio 2012, si noterà che l'editor rientra automaticamente gli elementi durante la scrittura di codice.

> [!NOTE]
> Nelle versioni precedenti di Visual Studio, rientri intelligenti erano disponibili nell'editor XML, ma non nell'editor HTML.


1. Assicurarsi che la configurazione di rientri in Editor HTML è impostata su rientri intelligenti. A tale scopo, selezionare il **strumenti | Opzioni** opzione di menu e quindi selezionare il **Editor di testo | HTML | Schede** pagina nel riquadro a sinistra della schermata. Selezionare l'opzione rientri intelligenti.

    ![Impostazioni dell'Editor HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "impostazioni dell'Editor HTML")

    *Impostazioni dell'Editor HTML*
2. Nel **Default.aspx** pagina, rimuovere tutto il contenuto sotto l'elemento audio.
3. Posizionare il cursore alla fine dell'apertura **audio** elemento e premere **invio**.

    Si noti che la nuova posizione del cursore è un livello di rientro aggiuntive.

    ![Rientro nell'Editor HTML per Smart](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "Smart rientro nell'Editor HTML")

    *Rientro smart nell'Editor HTML*
4. Ripristinare il tag con il contenuto è stato rimosso o chiudere audio **Default.aspx** senza salvare le modifiche.

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a>Attività 6 - Estrai nel controllo utente

Gli strumenti di Refactoring inclusi in Visual Studio, ad esempio l'estrazione di una parte di codice a una funzione, sono importanti funzionalità che facilitano l'analisi utilizzo software e il refactoring del codice esistente. La controparte per le pagine ASP.NET sarebbe l'estrazione del codice HTML a un controllo utente. Eseguire l'operazione manualmente implica alcuni passaggi, ad esempio la creazione di un nuovo controllo utente, lo spostamento di sezione di codice per il controllo utente, la registrazione di un prefisso di tag per il controllo utente e, infine, la creazione del controllo di utente nelle pagine. A questo punto, il nuovo *Estrai nel controllo utente* strumento esegue automaticamente tutti questi passaggi per l'utente.

In questa attività, si utilizzerà l'estrazione di nuovo all'operazione contesto controllo utente per generare un nuovo controllo utente dal codice selezionato.

1. Nel **Default.aspx** pagina, selezionare il **H2** e **audio** elementi.
2. Fare clic e selezionare **Estrai nel controllo utente**.

    ![Estrarre all'opzione di menu controllo utente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "estrarre all'opzione di menu controllo utente")

    *Estrarre all'opzione di menu controllo utente*
3. Digitare un nome per il nuovo controllo utente. Ad esempio, **Jukebox.ascx**, quindi fare clic su **OK**.

    ![Salvare il controllo utente estratti](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "il salvataggio del controllo utente estratti")

    *Salvare il controllo utente estratti*
4. Si noti che il codice selezionato è stato estratto in un controllo utente e il percorso originale del codice selezionato è stato sostituito con un'istanza del nuovo controllo utente.

    ![Pagina aggiornati automaticamente per usare il nuovo controllo utente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "pagina aggiornati automaticamente per usare il nuovo controllo utente")

    *Pagina aggiornati automaticamente per usare il nuovo controllo utente*
5. Premere **F5** per eseguire la pagina e verificare il corretto funzionamento di controllo.

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a>Esercizio 3: Novità nell'Editor JavaScript

La scrittura o modifica del codice JavaScript non è un compito facile, soprattutto quando l'avvio dell'applicazione per l'aumento delle dimensioni e si troverà la gestione di file lunghi e centinaia di funzioni. Gli sviluppatori di script in genere necessario eseguire un lavoro aggiuntivo per mantenere la leggibilità di codice e spostarsi tra i file. Con l'inclusione di librerie JavaScript come jQuery, spostamento di script è diventata una richiesta a causa della lunghezza del codice.

Visual Studio ha rinnovato l'editor JavaScript con la promessa affinché la modalità di codice accessibile e organizzati. Molte funzionalità di Visual Studio che esiste già nell'editor di codice c# o Visual Basic vengono ora implementate nell'editor JavaScript: Vai a definizione, il rientro automatico, documentazione e la convalida durante la scrittura. Con l'elenco di IntelliSense rinnovato è il catalogo della funzione JavaScript a portata di mano.

In questo esercizio, si apprenderà alcune delle nuove funzionalità e miglioramenti dell'editor JavaScript. Verrà sfogliare i file di esempio e individuare ognuna delle nuove caratteristiche che consentono la programmazione JavaScript più efficiente all'interno di Visual Studio 2012.

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a>Attività 1: nuove funzionalità di Editor JavaScript

Questa attività verrà presentate alcune delle nuove funzionalità JavaScript editor concentrarsi sull'organizzazione del codice e riportare una migliore esperienza utente.

1. Se non è già aperto, avviare **Visual Studio** e aprire il **WhatsNewASPNET.sln** soluzione si trova nel **Source\WhatsNewASPNET** cartella di questa esercitazione.
2. Premere **F5** per eseguire l'applicazione, quindi fare clic sul collegamento JavaScript nella barra di spostamento. Aggiornare la pagina più volte e verificare come l'incremento del contatore.

    ![Contatore di pagine](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "contatore di pagine")

    *Contatore di pagine*
3. Chiudere il browser e tornare a Visual Studio.
4. Aprire il **JavaScript.aspx** pagina e individuare il **&lt;script&gt;** blocco (mostrato sotto).

    Il codice seguente usa l'archiviazione locale HTML5 per archiviare un *pageLoadCount* variabile che archivia il numero di volte in cui la pagina è stata visitata dall'utente corrente. Archiviazione locale è un database di chiave-valore sul lato client introdotto con lo standard HTML5. I dati vengono salvati nel computer locale, all'interno del browser dell'utente.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > Verificare che il tipo di documento è impostato su XHTML5 prima di procedere con i passaggi successivi.
5. Modificare il codice e sono incluse le funzionalità di HTML5, come archiviazione locale e i relativi metodi interni che IntelliSense per JavaScript.

    ![Le funzionalità di HTML5 JavaScript di JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5 JavaScript le funzionalità di JavaScript")

    *Funzionalità di HTML5 JavaScript di JavaScript*
6. Fare clic su qualsiasi parentesi quadra aperta (**{**) da cui la creazione di script del codice e si noti che le parentesi quadre sono evidenziate.

    ![Le parentesi quadre sono evidenziate](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "tra parentesi vengono evidenziate")

    *Le parentesi quadre sono evidenziate*
7. Rimuovere la funzione **testAutoAlign()** (selezionare le tre righe, è possibile utilizzare **CTRL** + **K**; **CTRL** + **U**) e individuare il cursore all'interno del codice di funzione. Premere INVIO per aggiungere una seconda riga. Si noti che il codice è ora **allineato** e **rientrato automaticamente**.

    ![Il codice JavaScript viene automaticamente allineati](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "codice JavaScript viene allineato automaticamente")

    *Il codice JavaScript viene allineato automaticamente*

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a>Attività 2 - convalida JavaScript

In questa attività, sarà possibile osservare la convalida di JavaScript nuovi per lo standard di ECMAScript5. Questa funzionalità consente di scrivere codice JavaScript conforme, durante i problemi di scripting impedendo prima della distribuzione del sito.

> [!NOTE]
> Visual Studio 2010 implementate conformità ECMAStript3, mentre Visual Studio 2012 fornisce ECMAScript5 conformità.


1. Aprire **ECMA5script5.js** sotto il **Scripts\custom** cartella del progetto. Verrà ora testato convalida per ECMAScript5 standard.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    È possibile estrarre il &quot; **utilizzare strict** &quot; direzione nella prima riga del file, che consente di ECMAScript5 **modalità strict**. Questa modalità è composta da un sottoinsieme del linguaggio che si illustra le ambiguità dalla versione di precedente e aggiunge alcune nuove funzionalità, ad esempio getter e setter, supporto della libreria per JSON e più completa reflection alle proprietà dell'oggetto.
2. Aprire il **elenco errori** se non è ancora aperto (**vista** menu | **Elenco errori**). Si noti il **funzione** dichiarazione è sottolineata. Questo avviene perché in ECMA5 funzioni standard non possono essere annidate all'interno delle strutture di linguaggio. L'errore elenco riportato di seguito si visualizzeranno i dettagli dell'avviso.

    ![Messaggio di errore di convalida JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "messaggio di errore di convalida JavaScript")

    *Messaggio di errore di convalida JavaScript*
3. Impostare come commento il **&quot;utilizzare strict&quot;** direzione e notare che scompaiono errori, ma vengono mantenuti gli avvisi.
4. Nell'ultima riga del file, scrivere qualsiasi stringa come **&quot;test&quot;** (che includono le virgolette per indicare che è sotto forma di stringa). Scrivere un periodo accanto la stringa da visualizzare l'elenco di IntelliSense e selezionare il **trim** opzione.

    Nello standard ECMAScript5, variabili e i valori stringa dispongono anche di metodi di stringa definiti, ad esempio trim, lettere maiuscole, ricerca e sostituzione.

    ![Elenco di IntelliSense in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "elenco IntelliSense in JavaScript")

    *Elenco di IntelliSense in JavaScript*

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a>Attività 3 - documentazione XML per JavaScript

In questa attività verranno analizzate le funzionalità di Visual Studio per la documentazione XML in JavaScript. Verrà visualizzato che l'elenco di JavaScript IntelliSense Mostra ora la documentazione XML di ogni funzione. Inoltre, si noterà che la funzionalità di navigazione in JavaScript.

1. Aprire **XMLDoc.js** file si trova in **script/personalizzato** cartella del progetto. Questo file contiene la documentazione XML in tutte le funzioni JavaScript.

    ![Documentazione XML JavaScript integrato a IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML documentazione integrata in IntelliSense")

    *Documentazione XML JavaScript integrata in IntelliSense*
2. Di seguito **aggiungere** funzionare in **XMLDoc.js** file, creare una nuova funzione denominata **test**.
3. Nel **test** funzione, chiamare il **moltiplicare** funzione che riceve due parametri. Si noti la casella della descrizione comando viene visualizzato il **moltiplicare** funzione documentazione.

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    ![Documentazione XML per le funzioni JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "documentazione XML per le funzioni JavaScript")

    *Documentazione XML per le funzioni JavaScript*
4. Completare l'istruzione di chiamata di funzione e tipo di un *punto* per aprire l'elenco di IntelliSense nel valore restituito. Si noti che il rilevamento di Visual Studio il **restituire** valore nella documentazione, considerando il valore come un numero.

    ![Documentazione XML per i tipi restituiti](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "documentazione XML per i tipi restituiti")

    *Documentazione XML per i tipi restituiti*
5. A questo punto, inserire una chiamata di add (funzione). Si noti che l'editor JavaScript supporta ora gli overload della funzione. Quando si scrive un nome di funzione, sarà possibile selezionare uno degli overload disponibili specificato nella documentazione.

    ![Documentazione XML per gli overload](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "documentazione XML per gli overload")

    *Documentazione XML per gli overload*
6. Aprire **GotoDefinition.js** file e individuare il **$().html()** chiamata di funzione. Posizionare il cursore su **html**.
7. Premere **F12** e passare alla definizione. Nota è possibile accedere e individuare il codice JavaScript senza la **trovare** strumento.
8. Posizionare il cursore sull'istruzione jQuery prima il blocco della firma nella parte inferiore del file di codice. Premere **F12**. Passare il file della libreria jQuery. Si noti è inoltre possibile navigare in tutti i file di jQuery utilizzando **F12**.

    ![Passare alle definizioni di jQuery](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "spostarsi alle definizioni di jQuery")

    *Passare alle definizioni di jQuery*

> [!NOTE]
> Assicurarsi che GotoDefinition.js siano presenti errori di sintassi prima di salvare il file.


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a>Esercizio 4: Bundling and Minification

Quante volte si i siti Web include più JavaScript o CSS file? Si tratta di uno scenario molto comune in cui aggregazione e minimizzazione consentono di ridurre le dimensioni del file e rendere il sito risultano più veloci. La nuova funzionalità di aggregazione in ASP.NET 4.5 comprime un set di file JS o CSS in un singolo elemento e la riduzione del contenuto (ad esempio la rimozione degli spazi vuoti non necessari, rimozione di commenti, riducendo gli identificatori) riduce le dimensioni.

Come aggregare e riduzione in ASP.NET 4.5 viene eseguita in fase di esecuzione, in modo che il processo può identificare l'agente utente (ad esempio Internet Explorer, Mozilla e così via) e di conseguenza, migliorare la compressione includendo il browser utente (ad esempio, la rimozione ciò che è specifico di Mozilla Quando la richiesta proviene da Internet Explorer).

In questo esercizio, si apprenderà come abilitare e utilizzare i diversi tipi di aggregazione e riduzione in ASP.NET 4.5.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a>Attività 1: installare l'aggregazione e minimizzazione pacchetto NuGet

1. Se non è già aperto, avviare **Visual Studio** e aprire il **WhatsNewASPNET.sln** soluzione si trova nel **Source\WhatsNewASPNET** cartella di questa esercitazione.
2. Aprire la Console di gestione pacchetti NuGet. A tale scopo, utilizzare il menu **vista** | **altre finestre** | **Package Manager Console**.

    ![Apertura di file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole manager pacchetto](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "apertura della console di gestione pacchetti")

    *Aprire la console di gestione pacchetti*
3. Nel **Console di gestione pacchetti,** tipo **Install-Package Microsoft.Web.Optimization** e premere **invio**.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a>Attività 2: Bundle predefinita

Il modo più semplice da utilizzare come aggregare e riduzione è consentire di bundle predefinita. Questo metodo utilizza le convenzioni per consentono di fare riferimento alla versione minimizzata e aggregata per i file JS e CSS in una cartella.

In questa attività si apprenderà come abilitare e fare riferimento ai file in bundle e minimizzati JS e CSS e visualizzare l'output risultante.

1. Se non è già aperto, avviare **Visual Studio** e aprire il **WhatsNewASPNET.sln** soluzione si trova nel **Source\WhatsNewASPNET** cartella di questa esercitazione.
2. Nel **Esplora**, espandere il **stili**, **Scripts\custom** e **Scripts\bundle** cartelle.

    Si noti che l'applicazione utilizza i file più CSS e JS.

    ![I file più fogli di stile e JavaScript nell'applicazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "più fogli di stile e JavaScript file nell'applicazione")

    *Più file di JavaScript e fogli di stile nell'applicazione*
3. Aprire il **Global.asax.cs** file.

    Si noti che il nuovo **Microsoft.Web.Optimization** dello spazio dei nomi viene impostata come commento all'inizio del file. Rimuovere il commento di utilizzando una direttiva per includere le caratteristiche di aggregazione e la riduzione.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. Individuare il **applicazione\_avviare** metodo.

    In questo metodo, rimuovere il commento la chiamata di EnableDefaultBundles come illustrato nel frammento riportato di seguito. Ciò consente di fare riferimento a una raccolta di bundle di file CSS in una cartella utilizzando il percorso alla cartella, più il &quot;CSS&quot; o &quot;JS&quot; suffisso.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. Aprire il **Optimization.aspx** file e individuare il controllo contenuto di **HeadContent**.

    Si noti i file CSS e i file JS presentare un unico tag di cui si fa riferimento.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > Questo codice è a scopo dimostrativo. In teoria, si farà riferimento bundle nel file Site. master. In questo codice di esempio, si noterà che alcuni dei file in dotazione anche cui fa riferimento il file Site. master, rendendo l'ultimo riferimento ridondanti.
6. Si noti che i collegamenti utilizza le convenzioni di aggregazione di **href** attributo per ottenere i file di tutti i CSS o JS dagli stili e Scripts\custom cartella rispettivamente.

    È possibile utilizzare il percorso **personalizzato/script/JS** come illustrato di seguito per aggregare e minimizzare tutti i file JS all'interno di un **script/personalizzato** cartella. Questo è il comportamento predefinito con i bundle predefinita.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. Aprire il **Styles\Site.css** file.

    Si noti che il file CSS originale contiene rientro di codice, spazi vuoti e commenti che ampliare il file. (Anche il file JavaScript contiene spazi vuoti e commenti).

    ![Uno dei CSS originale file nella cartella Scripts](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "uno del foglio di stile CSS originale file nella cartella Scripts")

    *Uno dei file CSS originali nella cartella Scripts*
8. Premere **F5** per eseguire l'applicazione e spostarsi tra le **ottimizzazione** pagina.
9. Fare clic su di **Bundle CSS** collegamento per scaricare e aprire il file.

    Estrarre il file aggregato minimizzato. Si noterà che sono stati rimossi gli spazi vuoti, commenti e i caratteri di rientro, la generazione di un file più piccolo.

    ![I file CSS di bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "file CSS in bundle")

    *File di bundle CSS*
10. Fare clic su di **Bundle JS** collegamento per aprire il file JavaScript in bundle. È possibile ignorare l'avviso di explorer. Notare i file JavaScript sotto il **personalizzato** cartella sono anche in bundle e minimizzare.

    ![I file JavaScript di bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "file JavaScript in bundle")

    *File di bundle JavaScript*

    L'abilitazione della compressione per i file JS o CSS è molto più complessa in una versione precedente di ASP.NET. A questo punto, come si è visto, è sufficiente aggiungere una riga di *Global. asax* per abilitare la creazione di bundle di file e quindi fare riferimento ai file in dotazione dal sito.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a>Attività 3 - Bundle statici

L'approccio bundle statico consente di personalizzare il set di file di bundle, il riferimento e il metodo minimizzazione che verrà utilizzato.

In questa attività si configurerà un bundle statico per definire un set specifico di file da aggregare e minimizzare.

1. Chiudere il browser.
2. Aprire il **Global.asax.cs** file e individuare il **applicazione\_avviare** metodo.
3. Rimuovere il commento il codice di aggregazione statici come illustrato nel codice seguente.

    Si sta definendo un raggruppamento statico che verrà fatto riferimento con il &quot; **~/StaticBundle** &quot; percorso virtuale e utilizzare **JsMinify** per la riduzione di tutti i file specificati con il **AddFile** metodo. Infine, si aggiunge il bundle statico per il **BundleTable** e abilitarlo.

    Si noti che i file non si trovano nella stessa posizione; si tratta di un altro vantaggio rispetto l'aggregazione predefinita.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. Aprire il **Optimization.aspx** file.

    Si noti che il collegamento a **statico Bundle JS** sta utilizzando il percorso in cui è stato dichiarato quando è stato configurato il bundle statico nel file Global.asax.cs: **/StaticBundle**.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. Premere **F5** per eseguire l'applicazione e quindi passare il **ottimizzazione** pagina.
6. Fare clic su di **statico Bundle JS** collegamento per aprire il file.

    Si noti che il minimizzata in bundle file JavaScript sono riportato l'output per tutti i file JavaScript configurato nel file di bundle statico nel percorso &quot;/StaticBundle&quot;.

    ![Bundle di file statici JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "bundle di file JavaScript statico")

    *Aggregano i file JavaScript statici*
7. Chiudere il browser e tornare a Visual Studio.

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a>Attività 4: Bundle cartella dinamica

In questa attività si apprenderà come configurare bundle cartella dinamica. La potenza di aggregazione dinamica è che possibile includere statico JavaScript, nonché altri file in linguaggi che viene compilato in JavaScript e pertanto richiedono alcune operazioni di elaborazione prima che venga eseguita l'aggregazione.

In questo esempio, si apprenderà come usare il **DynamicFolderBundle** classe per creare un bundle dinamico per i file scritti in CofeeScript. CofeeScript è un linguaggio di programmazione che viene compilato in JavaScript e fornisce una sintassi più semplice per la scrittura di codice JavaScript, migliorando la leggibilità e brevità JavaScript.

1. Aprire il **Global.asax.cs** file e individuare il **applicazione\_avviare** metodo.
2. Rimuovere il commento di codice dinamico bundle come illustrato nel codice seguente.

    Si sta definendo un bundle della cartella dinamica che utilizzerà il **CoffeeMinify** processore minimizzazione personalizzato che si applica solo ai file con il &quot; **.coffee** &quot; (estensione CoffeeScript file). Si noti che è possibile utilizzare un criterio di ricerca per selezionare i file di bundle all'interno di una cartella, ad esempio '\*.coffee'.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. Aprire la Console di gestione pacchetti NuGet. A tale scopo, utilizzare il menu **vista** | **altre finestre** | **Package Manager Console**.
4. Nel **Console di gestione pacchetti,** tipo **Install-Package CoffeeSharp** e premere **invio**.
5. Fare clic su di **Mostra tutti i file** pulsante il **Esplora** finestra

    ![Visualizzazione di tutti i file](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "che mostra tutti i file")

    *Visualizzazione di tutti i file*
6. Fare clic con il pulsante destro il **CoffeeMinify.cs** del file nel **Esplora** e selezionare **Includi nel progetto**

    ![Includere il file CoffeeMinify.cs nel progetto](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "includere il file CoffeeMinify.cs nel progetto")

    *Includere il file CoffeeMinify.cs nel progetto*
7. Aprire il **CoffeeMinify.cs** file.

    Questa classe eredita da JsMinify da minimizzare l'output JavaScript risultante dalla compilazione di codice CoffeeScript. Chiama il compilatore di CoffeeScript per generare il codice JavaScript prima e quindi invia al metodo JsMinify.Process per minimizzare il codice risultante.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. Aprire il **Script1.coffee** e **Script2.coffee** i file dal **/bundle di script** cartella.

    Questi file includerà il codice CoffeScript da compilare durante l'esecuzione di aggregazione con la classe CoffeeMinify.

    Per motivi di semplicità, file CoffeeScript forniti sono inclusi solo il codice di esempio CoffeeScript. I commenti vengono esclusi dal processo di JsMinify.

    ![File CoffeeScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "file CoffeeScript")

    *File CoffeeScript*

    > [!NOTE]
    > [CofeeScript](https://github.com/jashkenas/coffeescript/) fornisce una sintassi più semplice per la scrittura di codice JavaScript, migliorando la leggibilità e brevità JavaScript, nonché l'aggiunta di altre funzionalità come matrice comprensione e criteri di ricerca.
9. Aprire il **Optimization.aspx** file e individuare i collegamenti di bundle.

    Si noti che il collegamento a **dinamica Bundle JS** fa riferimento il **/bundle di script** cartella utilizzando il **/caffè** suffisso è configurato per il bundle della cartella dinamica.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. Premere **F5** per eseguire l'applicazione e quindi passare il **ottimizzazione** pagina.
11. Fare clic su di **dinamica Bundle JS** collegamento per aprire il file generato.

    Si noti che il contenuto che è stato incluso in questo pacchetto contiene solo **.coffee** file. È anche possibile vedere che il codice di CoffeeScript è stato compilato in JavaScript e le righe di commento è stato rimosso.

    ![Aggregano il file JS dinamico](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "bundle file JS dinamica")

    *Aggregano il file JS dinamico*

> [!NOTE]
> Inoltre, è possibile distribuire l'applicazione per siti Web di Azure seguenti [pubblicazione appendice b: un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).


<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Questa esercitazione consente di comprendere cosa è nuovo in ASP.NET e lo sviluppo Web in Visual Studio 2012 e come sfruttare i numerosi miglioramenti a livello di Visual Studio 2012.

Completando questa pratica, apprese illustrato come utilizzare le nuove funzionalità e miglioramenti nell'editor di Visual Studio 2012 per CSS, JavaScript e HTML. Inoltre, apprese la modalità di implementazione di aggregazione incorporata e riduzione in Visual Studio 2012.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice a: installazione di Visual Studio Express 2012 per Web

È possibile installare **Microsoft Visual Studio Express 2012 per Web** o un altro &quot;Express&quot; versione utilizzando il **[installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web di Microsoft*.

1. Passare a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **installa**. Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.
3. Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.

    ![Installa Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "installa Visual Studio Express")

    *Installa Visual Studio Express*
4. Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.

    ![Accettare le condizioni di licenza](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    *Accettare le condizioni di licenza*
5. Attendere finché non viene completato il processo di download e l'installazione.

    ![Stato dell'installazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    *Installazione completata*
7. Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per Web, passare al **avviare** schermata e iniziare la scrittura &quot; **VS Express**&quot;, quindi fare clic su di **Visual Studio Express per Web** il riquadro.

    ![VS Express per il riquadro Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    *VS Express per il riquadro Web*

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Appendice b: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web

Questa appendice mostra come creare un nuovo sito web dal portale di gestione di Windows Azure e pubblicare l'applicazione, ottenute seguendo il lab, sfruttando la funzionalità di pubblicazione distribuzione Web fornita da Microsoft Azure.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Attività 1 - Creazione di un nuovo sito Web da Windows Azure Portal

1. Passare al [il portale di gestione di Windows Azure](https://manage.windowsazure.com/) e accedere utilizzando le credenziali di Microsoft associate alla sottoscrizione.

    > [!NOTE]
    > Con Windows Azure è possibile ospitare gratuito di 10 siti Web ASP.NET e quindi aumentare in base al traffico. È possibile iscriversi [qui](http://aka.ms/aspnet-hol-azure).

    ![Accedere al portale Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "accedere al portale Windows Azure")

    *Accedere al portale di gestione di Azure*
2. Fare clic su **nuovo** sulla barra dei comandi.

    ![Creazione di un nuovo sito Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "creando un nuovo sito Web")

    *Creazione di un nuovo sito Web*
3. Fare clic su **calcolo** | **sito Web**. Selezionare quindi **creazione rapida** opzione. Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.

    > [!NOTE]
    > Un sito Web di Windows Azure è l'host per un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire. L'opzione Creazione rapida consente di distribuire un'applicazione web completa per il sito Web Microsoft Azure dall'esterno al portale. Non include i passaggi per configurare un database.

    ![Creazione di un nuovo sito Web utilizzando Creazione rapida](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "creando un nuovo sito Web utilizzando Creazione rapida")

    *Creazione di un nuovo sito Web utilizzando Creazione rapida*
4. Attendere finché il nuovo **sito Web** viene creato.
5. Una volta creato il sito Web, fare clic sul collegamento sotto il **URL** colonna. Verificare il funzionamento del nuovo sito Web.

    ![Esplorazione per il nuovo sito web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "esplorazione per il nuovo sito web")

    *Esplorazione per il nuovo sito web*

    ![Sito Web in esecuzione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "sito Web in esecuzione")

    *Sito Web in esecuzione*
6. Tornare al portale e fare clic sul nome del sito web sotto il **nome** colonna per visualizzare le pagine di gestione.

    ![Aprire le pagine di gestione del sito web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "apertura delle pagine di gestione del sito web")

    *Aprire le pagine di gestione del sito Web*
7. Nel **Dashboard** nella pagina di **riepilogo rapido** fare clic su di **Scarica profilo di pubblicazione** collegamento.

    > [!NOTE]
    > Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in un sito Web di Windows Azure per ogni metodo di pubblicazione abilitato. Il profilo di pubblicazione contiene gli URL, le credenziali dell'utente e le stringhe di database necessari per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di queste applicazioni per pubblicazione di applicazioni web per siti Web di Windows Azure.

    ![Profilo di pubblicazione del sito web di download](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "profilo di pubblicazione del sito web di download")

    *Profilo di pubblicazione del sito Web di download*
8. Scaricare il file del profilo di pubblicazione in un percorso noto. In questo esercizio si ulteriormente verrà visualizzato come utilizzare questo file per pubblicare un'applicazione web a un siti Web di Windows Azure da Visual Studio.

    ![Salvare il file del profilo di pubblicazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "il salvataggio del profilo di pubblicazione")

    *Salvare il file del profilo di pubblicazione*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Attività 2: configurare il Server di Database

Se l'applicazione viene utilizzato SQL Server database, è necessario creare un server di Database SQL. Se si desidera distribuire un'applicazione semplice che non utilizza SQL Server è possibile ignorare questa attività.

1. È necessario un server di Database SQL per archiviare il database dell'applicazione. È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure all'indirizzo **database Sql** | **server** | **del Server Dashboard**. Se non si dispone di un server creato, è possibile creare tramite il **Aggiungi** pulsante sulla barra dei comandi. Annotare il **nome server e URL, il nome di accesso di amministratore e la password**, come verranno utilizzati in operazioni successive. Non creare il database, come verrà creato in una fase successiva.

    ![Dashboard del Server Database SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "Dashboard del Server Database SQL")

    *Dashboard del Server Database SQL*
2. Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server di **indirizzi IP consentiti**. A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP da **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo. Immettere un nome per la regola e fare clic su di ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) pulsante.

    ![Aggiunta indirizzo IP del Client](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    *Aggiunta indirizzo IP del Client*
3. Una volta il **indirizzo IP del Client** viene aggiunto agli indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.

    ![Confermare le modifiche](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    *Confermare le modifiche*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web

1. Tornare alla soluzione ASP.NET MVC 4. Nel **Esplora**, fare clic sul progetto sito web e selezionare **pubblica**.

    ![La pubblicazione dell'applicazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "la pubblicazione dell'applicazione")

    *Pubblicazione del sito web*
2. Importare il profilo di pubblicazione che è stato salvato nella prima attività.

    ![L'importazione del profilo di pubblicazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "l'importazione del profilo di pubblicazione")

    *L'importazione di profilo di pubblicazione*
3. Fare clic su **convalidare la connessione**. Una volta completata la convalida, fare clic su **Avanti**.

    > [!NOTE]
    > Dopo aver visualizzato un segno di spunta verde visualizzata accanto al pulsante convalida connessione, la convalida è stata completata.

    ![Convalida della connessione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "convalida della connessione")

    *Convalida della connessione*
4. Nel **impostazioni** nella pagina il **database** fare clic sul pulsante accanto casella di testo della connessione di database (ad esempio **DefaultConnection**).

    ![Configurazione della distribuzione Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "configurazione della distribuzione Web")

    *Configurazione della distribuzione Web*
5. Configurare la connessione al database come segue:

   - Nel **nome Server** digitare l'URL server di Database SQL utilizzando il *tcp:* prefisso.
   - In **nome utente** digitare il nome di accesso di amministratore di server.
   - In **Password** digitare la password dell'account di accesso amministratore server.
   - Digitare un nuovo nome di database, ad esempio: *MVC4SampleDB*.

     ![Configurazione di stringa di connessione di destinazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "configurazione stringa di connessione di destinazione")

     *Configurazione di stringa di connessione di destinazione*
6. Fare quindi clic su **OK**. Quando viene richiesto di creare il database fare clic su **Sì**.

    ![Creazione del database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "creazione della stringa di database")

    *Creazione del database*
7. La stringa di connessione che si utilizzerà per connettersi al Database SQL di Windows Azure viene visualizzata all'interno di casella di testo di connessione predefinito. Scegliere quindi **Avanti**.

    ![Stringa di connessione che punta al Database SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "stringa di connessione che punta al Database SQL")

    *Stringa di connessione che punta al Database SQL*
8. Nel **anteprima** pagina, fare clic su **pubblica**.

    ![Pubblicare l'applicazione web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "pubblicare l'applicazione web")

    *Pubblicare l'applicazione web*
9. Una volta terminato il processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.

    ![Applicazione pubblicata in Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "applicazione pubblicata in Windows Azure")

    *Applicazione pubblicata in Windows Azure*
