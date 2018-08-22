---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: What ' s New in ASP.NET e sviluppo Web in Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: La nuova versione di Visual Studio introduce una serie di miglioramenti finalizzate soprattutto a migliorare l'esperienza e le prestazioni quando si lavora con tecnologie Web...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 263a6e0aed51a681193333b53eff8f03847fc3aa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823822"
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a>What ' s New in ASP.NET e sviluppo Web in Visual Studio 2012
====================
da [Camp Web Team](https://twitter.com/webcamps)

> La nuova versione di Visual Studio introduce una serie di miglioramenti finalizzate soprattutto a migliorare l'esperienza e le prestazioni quando si lavora con tecnologie Web. Editor di Visual Studio per CSS, JavaScript e HTML sono stata completamente rinnovato per includere molti dei riquadri di codice più ricercate, ad esempio IntelliSense e il rientro automatico. Relativamente alle prestazioni, creazione di bundle e minimizzazione adesso sono integrate, come il tempo di caricamento di funzionalità incorporate per ridurre facilmente la pagina.
> 
> Visual Studio consente di lavorare con le tecnologie più recenti di sito Web. È possibile usare frammenti di codice CSS3 multibrowser per assicurarsi che il sito funziona indipendentemente dalla piattaforma client, sfruttando i vantaggi delle nuove funzionalità e gli elementi HTML5.
> 
> La scrittura e la profilatura del codice JavaScript deve essere più semplice con questa versione di Visual Studio. Gli elenchi di IntelliSense, funzionalità di navigazione e la documentazione XML integrate sono ora disponibili per il codice JavaScript. Il catalogo di JavaScript è ora disponibile a tua disposizione. Inoltre, è possibile controllare la conformità di ECMAScript5 con gli script e rilevare gli errori di sintassi in una fase iniziale.
> 
> Ultimo, ma non meno importante, questa versione di Visual Studio implementa incorporati e minimizzazione. I file di script e fogli di stile verranno compressi e compressi in modo che il sito è più veloce.
> 
> Questa esercitazione illustra i miglioramenti e nuove funzionalità descritte in precedenza applicando modifiche minime a un'applicazione Web di esempio fornita nella cartella di origine.
> 
> Tutto il codice di esempio e frammenti di codice sono inclusi nel Web Camp Kit di formazione, disponibile all'indirizzo [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico, si apprenderà come:

- Usare le nuove funzionalità e miglioramenti nell'editor CSS
- Usare le nuove funzionalità e miglioramenti nell'editor HTML
- Usare le nuove funzionalità e miglioramenti nell'editor JavaScript
- Configurare e usare aggregazione e minimizzazione

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

- [Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (leggere [appendice A](#AppendixA) per istruzioni su come installarlo).
- [Windows PowerShell](https://support.microsoft.com/kb/968930/) (per gli script di installazione - già installati in Windows 8 e Windows Server 2008 R2)
- [Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - o un browser compatibile con HTML5

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questo laboratorio pratico include gli esercizi seguenti:

1. [Esercizio 1: Quali sono le novità nell'Editor CSS](#Exercise1)
2. [Esercizio 2: Quali sono le novità dell'Editor HTML](#Exercise2)
3. [Esercizio 3: Quali sono le novità nell'Editor JavaScript](#Exercise3)
4. [Esercizio 4: Creazione di bundle e minimizzazione](#Exercise4)

Tempo stimato per completare questa esercitazione: **60 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a>Esercizio 1: Quali sono le novità nell'Editor CSS

Gli sviluppatori Web devono essere familiari con molti le difficoltà correlate alla modalità di modifica CSS. Uno dei problemi più gravi di applicazione di stili CSS è la compatibilità tra browser. Spesso, si verifica che, dopo l'applicazione di stili per il sito, noterete che ha un aspetto diverso se viene aperto in un altro browser o dal dispositivo. Pertanto, si può impiegare molto tempo nel risolvere tali problemi visual per rendersi conto che, quando si infine consentire il funzionamento in un browser, questa viene suddivisa in altri.

Visual Studio include ora le funzionalità che consentono agli sviluppatori di accedere a, lavoro e organizzare i fogli di stile CSS in modo efficace. In questo esercizio, si soddisferà le nuove funzionalità per un'organizzazione effettiva e l'edizione, nonché i frammenti di codice CSS3 per garantire la compatibilità tra browser.

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a>Attività 1 - nuove funzionalità dell'Editor

In questa attività, scoprirai le nuove funzionalità dell'Editor CSS. Questo nuovo editor consentirà di aumentare la produttività, sfruttando i vantaggi del nuovo rientro intelligente, i commenti del codice migliorata e l'elenco di IntelliSense migliorata.

1. Avviare **Visual Studio** e aprire il **WhatsNewASPNET.sln** soluzione che si trova nel **Source\WhatsNewASPNET** cartella della presente esercitazione.
2. In Esplora soluzioni aprire il **CSS** file che si trova sotto il **stili** cartella. Assicurarsi che il **Editor di testo** sono visibili sulla barra degli strumenti. A tale scopo, selezionare il **View** | **barre degli strumenti** opzione di menu e controllare il **Editor di testo** opzioni. Si noterà che, poiché questa nuova versione, il **commento** pulsante (![comment-pulsante](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) e il **Rimuovi commento** pulsante (![rimuovere il commento-pulsante](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) vengono inoltre abilitate per l'editor CSS.

    ![Abilitazione dell'Editor e strumenti CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "abilitazione Editor e strumenti CSS")

    *Abilitazione dell'Editor e strumenti CSS*
3. Scorrere verso il codice e selezionare qualsiasi definizione di classe CSS. Scegliere il **commento** (![commento (pulsante)](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) pulsante per commentare le righe selezionate. Scegliere il **Rimuovi commento** (![pulsante di rimuovere il commento](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) pulsante per annullare le modifiche.
4. Fare clic sui **Comprimi** (![Comprimi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) e **Espandi** (![espandere](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) pulsanti che si trovano sul margine sinistro del testo. Si noti che è ora possibile nascondere gli stili che non si usa per avere una visualizzazione più chiara.

    ![Compressione delle classi CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "classi CSS di compressione")

    *Compressione delle classi CSS*
5. Assicurarsi che la funzionalità di rientro intelligente è abilitata. Selezionare il **degli strumenti** | **opzioni** opzione di menu e quindi selezionare il **Editor di testo** | **CSS**  |  **Formattazione** pagina nel riquadro sinistro della schermata. Verificare i **rientro gerarchico** opzione.

    ![Abilitazione di rientro gerarchico](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "abilitazione rientro gerarchico")

    *Abilitazione di rientro gerarchico*
6. Trovare la definizione della classe principale (.main) e aggiungere uno stile agli elementi div. Si noterà che il codice consente di allineare automaticamente, aiutare gli utenti per trovare l'elemento padre le classi in modo immediato.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    ![Allineamento gerarchico in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "allineamento gerarchico in CSS")

    *Allineamento gerarchico in CSS*
7. All'interno **.main div** classe, individuare il cursore alla fine del **bordo: 0px;** , quindi premere **invio** per visualizzare l'elenco di IntelliSense. Iniziare a digitare **top** e notare come l'elenco viene filtrato durante la digitazione. Nell'elenco vengono visualizzati gli elementi che contengono **superiore** in qualsiasi parte della parola (nelle versioni precedenti di Visual Studio, l'elenco viene filtrato per gli elementi che *begin* con il termine).

    ![Miglioramenti di IntelliSense in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "miglioramenti di IntelliSense CSS")

    *Miglioramenti di IntelliSense CSS*

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a>Attività 2: la selezione colori

In questa attività consente di individuare che la nuova selezione colori CSS integrato in Visual Studio IntelliSense.

1. Nelle **CSS,** individuare la definizione di classe di intestazione (.header) e posizionare il cursore accanto a **colore di sfondo** attributo, tra il &quot;:&quot; e &quot; # &quot; caratteri nella riga di codice **.**

    ![Individuazione del cursore](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "individuando il cursore")

    *Individuazione del cursore*
2. Eliminare il **virgola** (:) e scrivere nuovamente per visualizzare la selezione colori. Si noti che i colori di primo che verranno visualizzati i colori più frequenti del sito. Se si sceglie il colore bianco, al codice colore HTML (#fff) sostituirà il codice colore corrente nel foglio di stile.

    ![Selezione colori](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "selezione colori")

    *Selezione colori*
3. Premere il **Expand** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) pulsante nella selezione colori per visualizzare la sfumatura di colore e quindi trascinare il cursore sfumatura per selezionare un colore diverso. Successivamente, fare clic sui **contagocce** e selezionare un colore dalla schermata. Si noti che il valore di colore di sfondo cambia dinamicamente mentre si sposta il cursore.

    ![Sfumatura di colore della selezione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "sfumatura di colore della selezione")

    *Sfumatura di colore della selezione*
4. Nel **opacità** dispositivo di scorrimento, spostare il selettore al centro della barra per ridurre l'opacità. Si noti che il valore di colore di sfondo cambia ora la scala in RGBA.

    ![Selezione colori opacità](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "barev opacità")

    *Selezione colori opacità*

    > [!NOTE]
    > La definizione di colore RGBA (rosso, verde, blu e alfa) in CSS3 consente di definire il valore di opacità di colore per un singolo elemento. A differenza **opacità -** un simile attributo CSS **-** colori RGBA sono compatibili anche con i browser più recenti.

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a>Attività 3: i frammenti di codice compatibile CSS

In questa attività si apprenderà come usare frammenti di codice CSS3 compatibile tra browser per implementare alcune funzionalità nel tuo sito Web.

1. Nel **CSS** del file, individuare il **intestazione** CSS (.header) definizione di classe e posizionare il cursore sotto il **/ \*raggio bordo\* /** segnaposto per aggiungere un nuovo frammento di codice. Premere **invio** per visualizzare l'elenco di IntelliSense e tipo **radius** per filtrare l'elenco. Selezionare il **border-radius** opzione dall'elenco con un doppio clic e quindi premere il **scheda** chiave per inserire il frammento di codice. Quindi, digitare raggio dimensioni espresse in pixel e quindi premere **invio**. Ad esempio, digitare **15px**.

    Gli attributi di CSS3 aggiunti dal frammento forniranno i bordi arrotondati nella maggior parte dei browser conformità HTML5, tra cui Mozilla e nei browser basati su WebKit.

    ![Uso di un frammento border-radius](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "usando un frammento di codice border-radius")

    *Uso di un frammento border-radius*
2. Applicare le stesse **bordo** frammenti di codice nello stile di pagina (.page).

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. Premere **F5** per eseguire la soluzione. Si noti che ogni pagina ora è arrotondato bordi.

    ![Gli angoli arrotondati](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "gli angoli arrotondati")

    *Angoli arrotondati*
4. Chiudere il browser e tornare a Visual Studio.
5. Aprire il **Custom. CSS** file che si trova sotto il **stili** cartella e posizionare il cursore all'interno **div.images ul li img** definizione di classe.
6. Premere INVIO per visualizzare l'elenco di IntelliSense, digitare **ombreggiatura di casella** e premere il **scheda** chiave due volte per inserire il frammento di codice shadow predefinito nella definizione di classe. Impostazioni per i valori di shadow **10px 10px 5px #888**. Quindi, digitare **border-radius** e inserire il frammento di codice. Tipo di **15px** per impostare le dimensioni di radius e premere **invio**.

    ![Arrotondare gli angoli con ombreggiatura](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "arrotondati gli angoli con ombreggiatura")

    *Angoli arrotondati con ombreggiatura*

    > [!NOTE]
    > In questo momento, l'attributo shadow viene inserita con il prefisso corrispondente (moz, webkit, o) per supportare Mozilla e i browser Webkit (Chrome, Safari, Konkeror).
7. Creare una nuova classe **div.images ul li img:hover** sotto il **div.images ul li img** definizione di classe e posizionare il cursore all'interno delle parentesi **.**

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. Tipo di **trasformare** e premere il **scheda** chiave due volte per inserire il frammento di codice di trasformazione. Quindi, immettere **rotate(-15deg)** per modificare il valore dell'angolo di rotazione quando le immagini sono passaggio del mouse.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. Premere **F5** per eseguire la soluzione e passare alla pagina di CSS3. Si noti che le immagini hanno angoli arrotondati e casella ombreggiature. Posizionare il mouse sopra le immagini e guardarli ruotare.

    ![Frammento di codice la rotazione di un'immagine di trasformare](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "frammento trasformazione ruotare un'immagine")

    *Trasformare frammento ruotare un'immagine*

    > [!NOTE]
    > Se si usa Internet Explorer 10 e non è possibile vedere le ombreggiature, assicurarsi che la modalità documento è impostata su standard di IE10. Premere **F12** per aprire Strumenti di sviluppo di Internet Explorer e fare clic su **modalità documento** modificare a standard di IE10.

    ![about-us](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a>Esercizio 2: Quali sono le novità dell'Editor HTML

Visual Studio include un editor HTML migliorato. Alcuni dei miglioramenti inclusi in questa versione sono rientri in documenti HTML, frammenti di codice HTML5, iniziale HTML e corrispondenza tag di fine e la convalida HTML. In questo esercizio, verranno visualizzate come queste modifiche migliorano la necessaria la padronanza quando si lavora nel markup di sito Web.

Come l'editor CSS, editor HTML è stato anche migliorato. La maggior parte di questi miglioramenti sono più piccoli che semplifica la vita dello sviluppatore Web. Elementi come altri frammenti di codice per HTML5, rientri, corrispondenti ai tag di inizio e fine quando modifica e convalida come destinazione del documento HTML DOCTYPE sono riportati alcuni di questi miglioramenti.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a>Attività 1: convalida di DOCTYPE migliorata

L'editor HTML include ora la possibilità di controllare il tipo di documento della pagina, anche se la definizione potrebbe essere nella pagina master. A seconda del tipo di documento della pagina, l'editor HTML convaliderà il set corretto delle regole e verrà filtrato l'elenco di IntelliSense, prendere in considerazione gli elementi di tipo di documento.

In questa attività si modificherà il tipo di documento di una pagina per vedere come il comportamento dell'editor HTML cambia di conseguenza.

1. Se non è già aperto, avviare **Visual Studio** e aprire il **WhatsNewASPNET.sln** soluzione che si trova nel **Source\WhatsNewASPNET** cartella della presente esercitazione.
2. Aprire il **Site. master** pagina.
3. Si noti che lo Schema di destinazione per la convalida della barra degli strumenti. Il comportamento dell'editor HTML (convalida, IntelliSense e così via) verrà modificato in modo corretto per adattare il tipo di documento selezionata.

    ![Utilizza Doctype nella barra degli strumenti Modifica origine HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Doctype di utilizzo nella barra degli strumenti Modifica origine HTML")

    *Utilizza Doctype nella barra degli strumenti Modifica origine HTML*
4. Modificare lo Schema di destinazione in formato HTML 4.01.

    ![Modifica tipo di documento nella barra degli strumenti Modifica origine HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "Doctype modifica nella barra degli strumenti Modifica origine HTML")

    *Modifica tipo di documento nella barra degli strumenti Modifica origine HTML*
5. Posizionare il cursore sotto il **corpo** elemento e iniziare a digitare il nome di un elemento HTML5 (ad esempio **video**). Si noti che l'elemento non è disponibile nell'elenco di IntelliSense.

    ![Gli elementi HTML5 non elencati](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "elementi HTML5 non elencati")

    *Elementi HTML5 non elencati*
6. Annullare le modifiche allo Schema di destinazione per la convalida della barra degli strumenti, DOCTYPE di prelievo: XHTML5 nell'elenco a discesa.

    ![Utilizza Doctype nella barra degli strumenti Modifica origine HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Doctype di utilizzo nella barra degli strumenti Modifica origine HTML")

    *Reimpostare Doctype nella barra degli strumenti Modifica origine HTML*
7. Posizionare il cursore sotto il **corpo** elemento e iniziare a digitare nuovamente un elemento HTML5 (ad esempio, come **video**). Si noti che gli elementi HTML5 sono ora disponibili nell'elenco di IntelliSense.

    ![Gli elementi HTML5 essere elencati](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "elementi HTML5 essere elencati")

    *Elementi HTML5 essere elencati*

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a>Attività 2: inizio/fine aggiornamento automatico dei tag

A questo punto, Visual Studio aggiorna il codice HTML di apertura o chiusura di tag dell'elemento che si sta modificando in modo da corrispondere tra loro. Questa nuova funzionalità migliorerà la produttività durante la modifica di tag HTML.

1. Nel **default. aspx** pagina, aggiungere un' **H3** elemento con un titolo (ad esempio, Visual Studio 2012 Rocks!).

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. Modifica il **H3** tag e il tipo **H2** o **H1.**

    Si noti che il tag di fine viene aggiornato automaticamente. È inoltre possibile modificare il tag di fine per verificare che il tag di inizio Aggiorna di conseguenza troppo.

    ![L'aggiornamento automatico del tag di fine](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "aggiornamento automatico del tag di fine")

    *Aggiornamento automatico del tag di fine*

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a>Attività 3: nuovi frammenti di codice HTML5

Visual Studio ora include diversi frammenti di codice HTML5. In questa attività si userà alcuni di questi frammenti di codice.

1. Aggiungere una nuova cartella denominata **audio** alla radice della cartella del sito web. Aprire l'Explorer di Windows e copiare i file audio nel **audio** cartella della **WhatsNewASPNET.sln** soluzione.
2. Nel **default. aspx** pagina, posizionare il cursore sotto il Web11 Rocks!! Intestazione. Tipo di **audio** e premere il tasto TAB.

    Il nuovo editor HTML include frammenti di codice per il contenuto HTML5. Ricordarsi di usare la definizione del tipo di documento appropriata per abilitare i frammenti HTML5.

    ![Inserimento di frammenti di codice HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "inserimento di frammenti di codice HTML5")

    *Inserimento di frammenti di codice HTML5*
3. Aggiornare l'origine audio in modo che punti a un file audio esistente.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > È necessario aggiungere il file audio alla soluzione.
4. Premere **F5** per eseguire il sito e riprodurre l'audio.

    ![Esecuzione del controllo audio](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "che esegue il controllo audio")

    *Esecuzione del controllo audio*

    > [!NOTE]
    > È anche possibile provare più frammenti di codice inclusi in Visual Studio, ad esempio video, nella figura, e così via.
5. A questo punto, provare a inserire un controllo in una parte della pagina. Ad esempio, provare a inserire un **GridView** (controllo), ma anziché digitare  **&lt;datta,** inizia a digitare  **&lt;GV**. Si noti che l'elenco di IntelliSense mostra le **asp: GridView** controllo.

    IntelliSense nell'Editor HTML offre ora ricerca titolo le maiuscole/minuscole, nonché parziale corrispondente (il recupero di tutti gli elementi che contiene il termine).

    ![Inserimento di un controllo GridView con IntelliSense Elenca](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "inserimento di un controllo GridView con elenchi di IntelliSense")

    *Inserimento di un controllo GridView con elenchi di IntelliSense*

    Se si digita  **&lt;griglia** verranno visualizzati tutti gli elementi che corrispondono al termine, ma Visual Studio suggerisce il **gridview** controllo:

    ![Inserimento di un controllo GridView con elenchi di IntelliSense e individuare corrispondenze parziali](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "inserimento di un controllo GridView con elenchi di IntelliSense e individuare corrispondenze parziali")

    *Inserimento di un controllo GridView con elenchi di IntelliSense e individuare corrispondenze parziali*

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a>Attività 4: Editor HTML Smart tag

Un altro miglioramento nell'Editor HTML è la funzionalità Smart tag. Gli smart tag rendono più semplice eseguire attività di sviluppo comuni o ripetitive in base al controllo. Questa funzionalità era già disponibile nella finestra di progettazione HTML, ma non nell'Editor HTML.

1. Aprire **Site. master** e individuare le **asp: Menu** elemento. Posizionare il cursore sul tag di inizio e notare che l'icona piccola visualizzata nella parte inferiore dell'elemento - fare clic per aprire il menu smart tag. Si noti che occorre accedere rapidamente ad alcune attività correlate al controllo Menu.

    ![Attività per il controllo Menu Smart](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "intelligente attività per il controllo Menu")

    *Smart task per il controllo Menu*

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a>Attività 5: rientro intelligente

Una delle procedure consigliate in formato HTML è il rientro di elementi annidati per mantenere il codice leggibile. In Visual Studio 2012, si noterà che l'editor rientra automaticamente gli elementi durante la scrittura di codice.

> [!NOTE]
> Nella versione precedente di Visual Studio, erano disponibili rientri nell'editor XML, ma non nell'editor HTML.


1. Assicurarsi che la configurazione di rientri sull'Editor HTML è impostata su rientro intelligente. A tale scopo, selezionare il **strumenti | Le opzioni** l'opzione di menu e quindi selezionare il **Editor di testo | HTML | Schede** pagina nel riquadro sinistro della schermata. Selezionare l'opzione rientro intelligente.

    ![Le impostazioni dell'Editor HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "le impostazioni dell'Editor HTML")

    *Impostazioni dell'Editor HTML*
2. Nel **default. aspx** pagina, rimuovere tutto il contenuto nell'elemento audio.
3. Posizionare il cursore alla fine dell'apertura **audio** elemento e premere **invio**.

    Si noti che la nuova posizione del cursore ha un livello aggiuntivo di rientro.

    ![Rientro automatico nell'Editor HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "rientro automatico nell'Editor HTML")

    *Rientro intelligente nell'Editor HTML*
4. Ripristinare i tag audio con il contenuto è stato rimosso oppure chiudere **default. aspx** senza salvare le modifiche.

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a>Attività 6: Estrai nel controllo utente

Gli strumenti di Refactoring in Visual Studio, inclusi, ad esempio l'estrazione di una parte di codice per una funzione, sono eccezionali funzionalità che facilitano l'analisi utilizzo software e il refactoring del codice esistente. La controparte per le pagine ASP.NET sarebbe l'estrazione del codice HTML a un controllo utente. Tale operazione manualmente comporta diversi passaggi, ad esempio la creazione di un nuovo controllo utente, lo spostamento di sezione di codice al controllo utente, la registrazione di un prefisso di tag per il controllo utente e, infine, un'istanza del controllo utente nelle pagine. A questo punto, il nuovo *Estrai nel controllo utente* strumento esegue automaticamente tutti i passaggi per l'utente.

In questa attività si userà l'estrazione di nuovo all'operazione contesto controllo utente per generare un nuovo controllo utente dal codice selezionato.

1. Nel **default. aspx** pagina, selezionare la **H2** e **audio** elementi.
2. Fare clic e selezionare **Estrai nel controllo utente**.

    ![Estrarre all'opzione di menu controllo utente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "estrarre all'opzione di menu controllo utente")

    *Estrarre all'opzione di menu controllo utente*
3. Digitare un nome per il nuovo controllo utente. Ad esempio, **Jukebox.ascx**, quindi fare clic su **OK**.

    ![Salvataggio del controllo utente estratti](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "salvando il controllo utente estratti")

    *Salvataggio del controllo utente estratti*
4. Si noti che il codice selezionato è stato estratto in un controllo utente e nel percorso originale del codice selezionato è stato sostituito con un'istanza del nuovo controllo utente.

    ![Pagina aggiornati automaticamente per usare il nuovo controllo utente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "pagina aggiornati automaticamente per usare il nuovo controllo utente")

    *Pagina aggiornati automaticamente per usare il nuovo controllo utente*
5. Premere **F5** per eseguire la pagina e verificare il corretto funzionamento di controllo.

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a>Esercizio 3: Quali sono le novità nell'Editor JavaScript

La scrittura o modifica del codice JavaScript non è un compito facile, in particolare quando l'applicazione inizia a crescere in dimensioni e si ritrova di gestione di file lunghi e centinaia di funzioni. Gli sviluppatori di script in genere necessario eseguire operazioni aggiuntive per mantenere la leggibilità di codice e spostarsi tra i file. Con l'inclusione delle librerie JavaScript come jQuery, navigazione dello script è diventato un problema a causa della lunghezza di codice.

Visual Studio ha rinnovato l'editor JavaScript con il suggerimento per rendere la modalità di codice accessibile e organizzate. Molte funzionalità di Visual Studio che esisteva già nell'editor di codice c# o Visual Basic ora sono implementate nell'editor JavaScript: Vai a definizione, il rientro automatico, documentazione e la convalida quando si scrive. Con l'elenco di IntelliSense rinnovato sarà il catalogo di funzione JavaScript a tua disposizione.

In questo esercizio, si apprenderà alcune delle nuove funzionalità e miglioramenti dell'editor JavaScript. Verrà sfogliare i file di esempio e individuare ognuna delle nuove caratteristiche che consentono la programmazione JavaScript più efficiente all'interno di Visual Studio 2012.

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a>Attività 1: nuove funzionalità dell'Editor JavaScript

Questa attività verrà presentate alcune delle nuove funzionalità dell'editor JavaScript, concentrandosi sulla organizzare il codice e garantendo un'esperienza utente migliore.

1. Se non è già aperto, avviare **Visual Studio** e aprire il **WhatsNewASPNET.sln** soluzione che si trova nel **Source\WhatsNewASPNET** cartella della presente esercitazione.
2. Premere **F5** per eseguire l'applicazione, quindi fare clic sul collegamento JavaScript nella barra di spostamento. Aggiornare la pagina più volte e controllare come l'incremento del contatore.

    ![Contatore di pagine](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "contatore di pagine")

    *Contatore di pagine*
3. Chiudere il browser e tornare a Visual Studio.
4. Aprire il **JavaScript.aspx** pagina e individuare le **&lt;script&gt;** blocco (mostrato sotto).

    Il codice seguente usa l'archiviazione locale HTML5 per archiviare una *pageLoadCount* variabile che archivia il numero di volte in cui la pagina è stata visitata dall'utente corrente. Archiviazione locale è un database chiave-valore sul lato client, introdotto con lo standard HTML5. I dati vengono salvati nel computer locale, all'interno del browser dell'utente.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > Verificare che la dichiarazione DOCTYPE è impostato su XHTML5 prima di procedere con i passaggi successivi.
5. Modificare il codice e si noti che IntelliSense per JavaScript include le funzionalità HTML5, come archiviazione locale e i relativi metodi interni.

    ![Le funzionalità HTML5 JavaScript di JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5 JavaScript le funzionalità di JavaScript")

    *Funzionalità HTML5 JavaScript di JavaScript*
6. Fare clic su qualsiasi parentesi quadra aperta (**{**) dalla creazione di script di codice e notare che le parentesi quadre sono evidenziate.

    ![Le parentesi vengono evidenziate](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "parentesi vengono evidenziate")

    *Le parentesi vengono evidenziate*
7. Rimuovere il commento la funzione **testAutoAlign()** (selezionare le tre righe ed è possibile usare **CTRL** + **K**; **CTRL** + **U**) e individuare il cursore all'interno del codice della funzione. Premere INVIO per aggiungere una seconda riga. Si noti che il codice è ora **allineato** e **rientrato automaticamente**.

    ![Il codice JavaScript viene automaticamente allineato](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "codice JavaScript viene automaticamente allineato")

    *Il codice JavaScript viene automaticamente allineato*

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a>Attività 2 - durante la convalida di JavaScript

In questa attività consente di individuare la nuova convalida di JavaScript per lo standard di ECMAScript5. Questa funzionalità consente di scrivere codice JavaScript conforme, mentre i problemi di scripting impedendo prima distribuzione del sito.

> [!NOTE]
> Visual Studio 2010 implementato ECMAStript3 conformità, anche se Visual Studio 2012 offre la conformità di ECMAScript5.


1. Aprire **ECMA5script5.js** sotto il **Scripts\custom** cartella del progetto. A questo punto si testerà la convalida di ECMAScript5 standard.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    È possibile consultare il &quot; **utilizzare strict** &quot; direzione nella prima riga del file, che consente di ECMAScript5 **modalità strict**. Questa modalità consiste in un subset del linguaggio che chiarisce ambiguità dall'edizione precedente e aggiunge alcune nuove funzionalità, ad esempio getter e setter, supporto della libreria per JSON e reflection più complete sulle proprietà degli oggetti.
2. Aprire il **elenco errori** se non è già aperta (**visualizzazione** menu | **Elenco errori**). Si noti che il **funzione** dichiarazione è sottolineata. Questo avviene perché in ECMA5 funzioni standard non possono essere annidate all'interno di strutture di linguaggio. Nel messaggio di errore elenco riportato di seguito si vedranno i dettagli di avviso.

    ![Messaggio di errore di convalida di JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "messaggio di errore di convalida di JavaScript")

    *Messaggio di errore di convalida di JavaScript*
3. Impostare come commento il **&quot;utilizzare strict&quot;** direzione e notare che non vengono più visualizzati errori, ma vengono mantenuti gli avvisi.
4. Nell'ultima riga del file, qualsiasi stringa, ad esempio scrivere **&quot;testare&quot;** (incluse le virgolette per indicare è sotto forma di stringa). Scrivere un periodo accanto alla stringa da visualizzare l'elenco di IntelliSense e selezionare il **trim** opzione.

    Nello standard ECMAScript5, variabili e i valori stringa dispongono anche di metodi di stringa definiti, ad esempio trim, lettere maiuscole, ricerca e sostituzione.

    ![Elenco di IntelliSense in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "elenco IntelliSense in JavaScript")

    *Elenco di IntelliSense in JavaScript*

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a>Attività 3: la documentazione XML per JavaScript

In questa attività verranno analizzate le funzionalità di Visual Studio per la documentazione XML in JavaScript. Verrà visualizzato che l'elenco di IntelliSense per JavaScript Mostra ora la documentazione XML di ogni funzione. Inoltre, si scoprirà che la funzionalità di navigazione in JavaScript.

1. Aprire **XMLDoc.js** file che si trova **gli scriptpersonalizzati/** cartella del progetto. Questo file contiene la documentazione XML in ognuna delle funzioni JavaScript.

    ![Documentazione XML JavaScript integrato di IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "integrata della documentazione XML JavaScript IntelliSense")

    *Documentazione XML JavaScript integrata di IntelliSense*
2. Di seguito **aggiungere** funzionare in **XMLDoc.js** del file, creare una nuova funzione denominata **test**.
3. Nel **testare** function, chiamare il **moltiplicare** funzione che riceve due parametri. Si noti che la casella della descrizione comando viene visualizzato il **moltiplicare** documentazione della funzione.

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    ![Documentazione XML per le funzioni JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "documentazione XML per le funzioni JavaScript")

    *Documentazione XML per le funzioni JavaScript*
4. Completare l'istruzione di chiamata di funzione e il tipo di un *dot* per aprire l'elenco di IntelliSense per il valore restituito. Si noti che Visual Studio sta rilevando i **restituire** valore nella documentazione, considerando il valore sotto forma di numero.

    ![Documentazione XML per i tipi restituiti](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "documentazione XML per i tipi restituiti")

    *Documentazione XML per i tipi restituiti*
5. A questo punto, inserire una chiamata per aggiungere una funzione. Si noti che l'editor JavaScript supporta ora gli overload della funzione. Quando si scrive un nome di funzione, sarà possibile selezionare uno qualsiasi degli overload disponibili specificato nella documentazione.

    ![Documentazione XML per gli overload](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "documentazione XML per gli overload")

    *Documentazione XML per gli overload*
6. Aprire **GotoDefinition.js** del file e individuare le **$().html()** chiamata di funzione. Individuare il cursore sul **html**.
7. Premere **F12** e passare alla definizione. Si noti che è ora possibile accedere ed esplorare il codice JavaScript senza usare la **trovare** dello strumento.
8. Individuare il cursore sull'istruzione jQuery prima del blocco di firme nella parte inferiore del file di codice. Premere **F12**. Passerà a file di libreria jQuery. Si noti che è anche possibile spostarsi tra i file di jQuery utilizzando **F12**.

    ![Passare alle definizioni di jQuery](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "passando alle definizioni di jQuery")

    *Passare alle definizioni di jQuery*

> [!NOTE]
> Assicurarsi che GotoDefinition.js siano presenti errori di sintassi prima di salvare il file.


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a>Esercizio 4: Creazione di bundle e minimizzazione

Quante volte si dei siti Web include file CSS o JavaScript di più? Si tratta di uno scenario molto comune in cui creazione di bundle e minimizzazione consente di ridurre le dimensioni del file e rendere il sito prestazioni più rapide. La nuova funzionalità di creazione di bundle in ASP.NET 4.5 comprime un set di file JS o CSS in un singolo elemento e minimizzazione (rimozione di spazi vuoti non necessari, rimuovendo i commenti, riducendo gli identificatori) contenuto riduce le dimensioni.

Creazione di bundle e minimizzazione in ASP.NET 4.5 viene eseguita in fase di esecuzione, in modo che il processo può identificare l'agente utente (ad esempio Internet Explorer, Mozilla e così via) e di conseguenza, migliorare la compressione indicando come destinazione dal browser dell'utente (ad esempio, la rimozione e comunque Mozilla specifico Quando la richiesta proviene da Internet Explorer).

In questo esercizio, si apprenderà come abilitare e usare i diversi tipi di aggregazione e minimizzazione in ASP.NET 4.5.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a>Attività 1: installare la creazione di bundle e minimizzazione pacchetto da NuGet

1. Se non è già aperto, avviare **Visual Studio** e aprire il **WhatsNewASPNET.sln** soluzione che si trova nel **Source\WhatsNewASPNET** cartella della presente esercitazione.
2. Aprire la Console di gestione pacchetti NuGet. A questo scopo, usare il menu di scelta **View** | **Other Windows** | **Package Manager Console**.

    ![Apertura di file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole Gestione pacchetti](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "aprendo la console di gestione pacchetti")

    *Aprire la console di gestione pacchetti*
3. Nel **Console di gestione pacchetti** tipo **Install-Package Microsoft.Web.Optimization** , quindi premere **invio**.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a>Attività 2: Bundle predefiniti

Il modo più semplice per usare aggregazione e minimizzazione è abilitare bundle predefiniti. Questo metodo Usa le convenzioni per consentire di fare riferimento alla versione in bundle e minimizzata per i file JS e CSS in una cartella.

In questa attività si apprenderà come abilitare e fare riferimento ai file JS e CSS in bundle e minimizzati e visualizzare l'output risultante.

1. Se non è già aperto, avviare **Visual Studio** e aprire il **WhatsNewASPNET.sln** soluzione che si trova nel **Source\WhatsNewASPNET** cartella della presente esercitazione.
2. Nel **Esplora soluzioni**, espandere il **stili**, **Scripts\custom** e **Scripts\bundle** cartelle.

    Si noti che l'applicazione usa più CSS e JS il file.

    ![I file più fogli di stile e JavaScript dell'applicazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "JavaScript e fogli di stile più file nell'applicazione")

    *Più file JavaScript e fogli di stile nell'applicazione*
3. Aprire il **Global.asax.cs** file.

    Si noti che il nuovo **Microsoft.Web.Optimization** dello spazio dei nomi viene impostata come commento all'inizio del file. Rimuovere il commento di utilizzo della direttiva da includere le funzionalità di creazione di bundle e minimizzazione.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. Individuare il **Application\_avviare** (metodo).

    In questo metodo, rimuovere il commento dalla chiamata EnableDefaultBundles come illustrato nel frammento di codice riportato di seguito. Ciò consente di fare riferimento a una raccolta di bundle di file CSS in una cartella usando il percorso alla cartella, oltre al &quot;CSS&quot; o nella &quot;JS&quot; suffisso.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. Aprire il **Optimization.aspx** del file e individuare il controllo contenuto per **HeadContent**.

    Si noti che i file CSS e i file JS hanno un singolo tag di cui viene fatto riferimento.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > Questo codice è a scopo dimostrativo. In teoria, si farà riferimento i bundle nel file Site. master. In questo codice di esempio, si noterà che alcuni dei file aggregati anche viene fatto riferimento dal file Site. master, rendendo questo ultimo riferimento ridondante.
6. Si noti che i collegamenti Usa le convenzioni di creazione di bundle il **href** attributo per ottenere i file di tutti i CSS o JS dagli stili e Scripts\custom cartella rispettivamente.

    È possibile usare il percorso **script/custom/JS** come illustrato di seguito per aggregare e minimizzare tutti i file JS all'interno di un **gli scriptpersonalizzati/** cartella. Questo è il comportamento predefinito con i bundle predefiniti.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. Aprire il **Styles\Site.css** file.

    Si noti che il file CSS originale contiene codice rientrato, gli spazi vuoti e commenti che aumentare le dimensioni del file. (Anche i file JavaScript contiene spazi vuoti e commenti).

    ![Uno di foglio di stile CSS originale file nella cartella Scripts](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "uno di foglio di stile CSS originale file nella cartella Scripts")

    *Uno dei file CSS originali nella cartella Scripts*
8. Premere **F5** per eseguire l'applicazione e spostarsi tra le **ottimizzazione** pagina.
9. Fare clic sui **Bundle CSS** collegamento per scaricare e aprire il file.

    Estrarre il file nel bundle minimizzato. Si noterà che sono stati rimossi gli spazi vuoti, commenti e i caratteri di rientro, la generazione di un file più piccolo.

    ![I file CSS di bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "file CSS in bundle")

    *File CSS in bundle*
10. Fare ora clic la **Bundle JS** collegamento per aprire il file JavaScript in bundle. È possibile ignorare tranquillamente l'explorer di avviso. Si noti che i file JavaScript con il **personalizzato** cartella vengono inoltre in bundle e minimizzati.

    ![I file JavaScript di bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "file JavaScript in bundle")

    *File di bundle JavaScript*

    L'abilitazione della compressione per i file CSS o JS era molto più complessa nella versione precedente di ASP.NET. A questo punto, come si è visto, è sufficiente aggiungere una riga nel *Global. asax* per abilitare la creazione di bundle di file e quindi fare riferimento a file aggregati dal sito.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a>Attività 3: aggregazioni statici

L'approccio di aggregazione statici consente di personalizzare il set di file di bundle, il riferimento e il metodo di minimizzazione che verrà utilizzato.

In questa attività si configurerà un bundle statico per definire un set specifico di file da aggregare e minimizzare.

1. Chiudere il browser.
2. Aprire il **Global.asax.cs** del file e individuare il **applicazione\_avviare** (metodo).
3. Rimuovere il commento il codice di aggregazione statici come illustrato nel codice seguente.

    Si sta definendo un bundle statico che verrà fatto riferimento con il &quot; **~/StaticBundle** &quot; percorso virtuale e usare **JsMinify** per minimizzazione di tutti i file specificati con il **AddFile** (metodo). Infine, si aggiunge il bundle statico per il **BundleTable** e abilitarlo.

    Si noti che i file non si trovano nella stessa posizione; si tratta di un altro vantaggio rispetto la creazione di bundle predefinita.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. Aprire il **Optimization.aspx** file.

    Si noti che il collegamento a **statici JS Bundle** Usa il percorso è stato dichiarato quando è stato configurato il bundle statico nel file Global.asax.cs: **/StaticBundle**.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. Premere **F5** per eseguire l'applicazione, quindi passare per il **ottimizzazione** pagina.
6. Fare clic sui **Bundle JS statico** collegamento per aprire il file.

    Si noti che il minimizzata in bundle file JavaScript sono riportato l'output per tutti i file JavaScript configurato nel file di bundle statici nel percorso &quot;/StaticBundle&quot;.

    ![Bundle di file statici JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "bundle di file JavaScript statico")

    *Aggregare i file statici di JavaScript*
7. Chiudere il browser e tornare a Visual Studio.

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a>Attività 4: bundle di cartelle dinamici

In questa attività si apprenderà come configurare il bundle di cartelle dinamici. La potenza di creazione di bundle dinamiche è che è possibile includere static JavaScript, nonché altri file nei linguaggi che viene compilato in JavaScript e richiedono pertanto, alcune operazioni di elaborazione prima che venga eseguita la creazione di bundle.

In questo esempio, si apprenderà come usare il **DynamicFolderBundle** classe per creare un bundle dinamico per i file scritti in CofeeScript. CofeeScript è un linguaggio di programmazione che viene compilato in JavaScript e fornisce una sintassi più semplice per la scrittura di codice JavaScript, migliorando la leggibilità e brevità di JavaScript.

1. Aprire il **Global.asax.cs** del file e individuare il **applicazione\_avviare** (metodo).
2. Rimuovere il commento il codice di aggregazione dinamica, come illustrato nel codice seguente.

    Si sta definendo un bundle di cartelle dinamici che verrà usato il **CoffeeMinify** processore minimizzazione personalizzato che verrà applicato solo ai file con il &quot; **. Coffee** &quot; (estensione File CoffeeScript). Si noti che è possibile usare un criterio di ricerca per selezionare i file di bundle all'interno di una cartella, ad esempio '\*. Coffee '.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. Aprire la Console di gestione pacchetti NuGet. A questo scopo, usare il menu di scelta **View** | **Other Windows** | **Package Manager Console**.
4. Nel **Console di gestione pacchetti** tipo **Install-Package CoffeeSharp** , quindi premere **invio**.
5. Fare clic sui **Mostra tutti i file** pulsante il **Esplora soluzioni** finestra

    ![Visualizzando tutti i file](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "visualizzando tutti i file")

    *Visualizzando tutti i file*
6. Fare clic il **CoffeeMinify.cs** del file nei **Esplora soluzioni** e selezionare **Includi nel progetto**

    ![Includere il file CoffeeMinify.cs nel progetto](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "includere il file CoffeeMinify.cs nel progetto")

    *Includere il file CoffeeMinify.cs nel progetto*
7. Aprire il **CoffeeMinify.cs** file.

    Questa classe eredita da JsMinify per minimizzare l'output JavaScript risultante dalla compilazione di codice CoffeeScript. Chiama il compilatore CoffeeScript per generare il codice JavaScript prima e quindi lo invia al metodo JsMinify.Process per minimizzare il codice risultante.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. Aprire il **Script1.coffee** e **Script2.coffee** dei file dal **bundle discript/** cartella.

    Questi file includerà il codice CoffeScript devono essere elaborate durante la creazione di bundle con la classe CoffeeMinify.

    Per motivi di semplicità, i file CoffeeScript forniti sono includendo solo il codice di esempio CoffeeScript. I commenti vengono esclusi dal processo JsMinify.

    ![File CoffeeScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "file CoffeeScript")

    *File CoffeeScript*

    > [!NOTE]
    > [CofeeScript](https://github.com/jashkenas/coffeescript/) fornisce una sintassi più semplice per la scrittura di codice JavaScript, migliorando la leggibilità e brevità di JavaScript, nonché l'aggiunta di altre funzionalità come la comprensione di matrice e criteri di ricerca.
9. Aprire il **Optimization.aspx** file e individuare i collegamenti di bundle.

    Si noti che il collegamento a **dinamica JS Bundle** a cui fa riferimento il **bundle di script/** cartella utilizzando il **/caffè** suffisso è stato configurato per il bundle della cartella dinamica.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. Premere **F5** per eseguire l'applicazione, quindi passare per il **ottimizzazione** pagina.
11. Fare clic sui **dinamica JS Bundle** collegamento per aprire il file generato.

    Si noti che il contenuto che era incluso in questo bundle contiene solo **. Coffee** file. È anche possibile vedere che il codice CoffeeScript è stato compilato in JavaScript e le righe di commento è stato rimosso.

    ![File JS dinamici del bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "bundle file JS dinamica")

    *File JS dinamici del bundle*

> [!NOTE]
> Inoltre, è possibile distribuire questa applicazione per siti Web di Azure seguenti [appendice b: pubblicazione un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixB).


<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Questa esercitazione consente di comprendere che cos'è ASP.NET e sviluppo Web in Visual Studio 2012 e su come sfruttare la vasta gamma di miglioramenti in Visual Studio 2012.

Completando questa pratica, si è appresa come usare le nuove funzionalità e miglioramenti nell'editor di Visual Studio 2012 per CSS, JavaScript e HTML. Inoltre, si è appresa come Visual Studio 2012 implementa incorporati e minimizzazione.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice a: installazione di Visual Studio Express 2012 per Web

È possibile installare **Microsoft Visual Studio Express 2012 per Web** o da un'altra &quot;Express&quot; versione utilizzando il **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web Microsoft*.

1. Passare a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e cercare il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **Installa ora**. Se non hai **instalace Webové Platformy** si verrà reindirizzati per scaricarlo e installarlo prima di tutto.
3. Una volta **instalace Webové Platformy** è aperto, fare clic su **installare** per avviare il programma di installazione.

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
7. Fare clic su **Exit** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per Web, fare clic per il **avviare** schermata e iniziare la scrittura &quot; **Visual Studio Express**&quot;, quindi fare clic sul **Visual Studio Express per Web** il riquadro.

    ![Visual Studio Express per il riquadro Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    *Visual Studio Express per il riquadro Web*

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Appendice b: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web

In questa appendice spiega come creare un nuovo sito web dal portale di gestione di Azure e pubblicare l'applicazione che è stato ottenuto seguendo il lab, sfruttando la funzionalità di pubblicazione di distribuzione Web fornita da Azure.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Attività 1: creazione di un nuovo sito Web di Windows portale di Azure

1. Andare alla [portale di gestione di Windows Azure](https://manage.windowsazure.com/) e accedere usando le credenziali di Microsoft associate alla sottoscrizione.

    > [!NOTE]
    > Con Windows Azure Puoi ospitare gratuitamente 10 siti Web ASP.NET e la scalabilità orizzontale man mano che aumenta il traffico. È possibile effettuare l'iscrizione [qui](http://aka.ms/aspnet-hol-azure).

    ![Accedere al portale di Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "accedere al portale di Azure")

    *Accedere al portale di gestione di Azure*
2. Fare clic su **New** sulla barra dei comandi.

    ![Creazione di un nuovo sito Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "creando un nuovo sito Web")

    *Creazione di un nuovo sito Web*
3. Fare clic su **Compute** | **sito Web**. Quindi selezionare **creazione rapida** opzione. Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.

    > [!NOTE]
    > Un sito Web di Microsoft Azure è l'host per un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire. L'opzione Creazione rapida consente di distribuire un'applicazione web completa per il sito Web Microsoft Azure all'esterno del portale. Non include i passaggi per la configurazione di un database.

    ![Creazione di un nuovo sito Web utilizzando Creazione rapida](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "creando un nuovo sito Web mediante Creazione rapida")

    *Creazione di un nuovo sito Web utilizzando Creazione rapida*
4. Attendere finché il nuovo **sito Web** viene creato.
5. Dopo aver creato il sito Web fare clic sul collegamento sotto i **URL** colonna. Verificare che il nuovo sito Web sia in funzione.

    ![Passare al nuovo sito web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "passare al nuovo sito web")

    *Passare al nuovo sito web*

    ![Sito Web in esecuzione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "sito Web in esecuzione")

    *Sito Web in esecuzione*
6. Tornare al portale e fare clic sul nome del sito web con il **nome** colonna per visualizzare le pagine di gestione.

    ![Aprire le pagine di gestione sito web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "aprire le pagine di gestione sito web")

    *Aprire le pagine di gestione sito Web*
7. Nel **Dashboard** nella pagina il **riepilogo rapido** fare clic sui **Scarica profilo di pubblicazione** collegamento.

    > [!NOTE]
    > Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in un sito Web di Azure per ogni metodo di pubblicazione abilitato. Il profilo di pubblicazione contiene l'URL, le credenziali dell'utente e stringhe di database necessarie per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di questi programmi per pubblicazione di applicazioni web in siti Web di Windows Azure.

    ![Download del sito web di profilo di pubblicazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "scaricando il sito web di profilo di pubblicazione")

    *Profilo di pubblicazione scaricato il sito Web*
8. Scaricare il file di profilo di pubblicazione in una posizione nota. In questo esercizio ulteriormente si vedrà come usare questo file per pubblicare un'applicazione web a un siti Web di Windows Azure da Visual Studio.

    ![Salvare il file di profilo di pubblicazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "salvataggio del profilo di pubblicazione")

    *Salvare il file di profilo di pubblicazione*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Attività 2: configurare il Server di Database

Se l'applicazione Usa SQL Server database è necessario creare un Database di SQL server. Se si vuole distribuire una semplice applicazione che non Usa SQL Server è possibile ignorare questa attività.

1. È necessario un server di Database SQL per archiviare il database dell'applicazione. È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Microsoft Azure all'indirizzo **database Sql** | **server** | **del Server Dashboard**. Se non si dispone di un server creato, è possibile crearne una usando il **Add** pulsante sulla barra dei comandi. Annotare il **nome server e URL, nome dell'account di accesso amministratore e la password**, come verranno usati nelle attività successiva. Non creare il database, come verrà creato in una fase successiva.

    ![Dashboard di Server di Database SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "Dashboard di Server di Database SQL")

    *Dashboard di Server di Database SQL*
2. Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server del **indirizzi IP consentiti**. A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP dal **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo. Immettere un nome per la regola, quindi scegliere il ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) pulsante.

    ![Aggiunta indirizzo IP del Client](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    *Aggiunta indirizzo IP del Client*
3. Una volta il **indirizzo IP del Client** viene aggiunto a indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.

    ![Confermare le modifiche](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    *Confermare le modifiche*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web

1. Tornare alla soluzione ASP.NET MVC 4. Nel **Esplora soluzioni**, fare clic sul progetto sito web e selezionare **Publish**.

    ![Pubblicazione dell'applicazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "pubblicazione dell'applicazione")

    *Pubblicazione del sito web*
2. Importare il profilo di pubblicazione che è stato salvato nella prima attività.

    ![L'importazione del profilo di pubblicazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "l'importazione del profilo di pubblicazione")

    *Importazione del profilo di pubblicazione*
3. Fare clic su **convalidare la connessione**. Dopo aver completata la convalida fare clic su **successivo**.

    > [!NOTE]
    > La convalida è stata completata una volta che visualizzato un segno di spunta verde accanto al pulsante convalida connessione.

    ![La convalida connessione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "convalida della connessione")

    *Convalida della connessione*
4. Nel **impostazioni** nella pagina il **database** sezione, fare clic sul pulsante accanto alla casella di testo della connessione di database (vale a dire **DefaultConnection**).

    ![Configurazione della distribuzione Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "configurazione della distribuzione Web")

    *Configurazione della distribuzione Web*
5. Configurare la connessione al database come segue:

   - Nel **nome Server** digitare l'URL server di Database SQL tramite il *tcp:* prefisso.
   - Nelle **nome utente** digitare il nome dell'account di accesso amministratore server.
   - Nelle **Password** digitare la password dell'account di accesso amministratore server.
   - Digitare un nuovo nome di database, ad esempio: *MVC4SampleDB*.

     ![Configurazione di stringa di connessione di destinazione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "configurazione stringa di connessione di destinazione")

     *Configurazione di stringa di connessione di destinazione*
6. Fare quindi clic su **OK**. Quando viene richiesto di creare il database, fare clic su **Sì**.

    ![Creazione del database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "creazione della stringa di database")

    *Creazione del database*
7. La stringa di connessione che si userà per connettersi al Database SQL di Windows Azure viene visualizzata all'interno di connessione predefinita nella casella di testo. Scegliere quindi **Avanti**.

    ![Stringa di connessione che punta al Database SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "stringa di connessione che punta al Database SQL")

    *Stringa di connessione che punta al Database SQL*
8. Nel **Preview** pagina, fare clic su **Publish**.

    ![Pubblicazione dell'applicazione web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "pubblicazione dell'applicazione web")

    *Pubblicazione dell'applicazione web*
9. Al termine del processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.

    ![Applicazione pubblicata in Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "applicazione pubblicata in Windows Azure")

    *Pubblicata dell'applicazione in Windows Azure*
