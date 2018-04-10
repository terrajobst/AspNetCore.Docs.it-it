---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Lab pratico: Strumenti Web 2013 Visual Studio | Documenti Microsoft'
author: rick-anderson
description: Visual Studio è un ambiente di sviluppo eccellente per. Windows basata su rete e i progetti web. Include un editor di testo potenti che può essere facilmente usato per...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: ef8ab82f9043ef9da3a3e6a146a97f083149534d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2018
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a>Lab pratico: Strumenti Web 2013 Visual Studio
====================
Da [categorie Web Team](https://twitter.com/webcamps)

[Download Web categorie Kit di formazione](http://aka.ms/webcamps-training-kit)

> Visual Studio è un ambiente di sviluppo eccellente per. Windows basata su rete e i progetti web. Include un editor di testo potenti che può essere facilmente usato per modificare i file autonomi senza un progetto.
> 
> Visual Studio mantiene una struttura ad albero di analisi completa durante la modifica di ogni file. Ciò consente a Visual Studio fornire un completamento automatico e le azioni basate su documenti rendendo molto più veloce e ottimizzare l'esperienza di sviluppo. Queste funzionalità sono particolarmente efficace nei documenti HTML e CSS.
> 
> Questa capacità è anche disponibile per le estensioni, rendendo più semplice da estendere l'editor con nuove potenti funzionalità in base alle esigenze. Web Essentials è una raccolta di (principalmente) miglioramenti relativi al web in Visual Studio. Include un numero elevato di nuovo al completamento di IntelliSense (soprattutto per CSS), nuove funzionalità di collegamento del Browser, automatic file JSHint per JavaScript, nuovi avvisi per HTML e CSS e molte altre funzionalità che sono essenziali per lo sviluppo web moderne.
> 
> Tutto il codice di esempio e i frammenti di codice sono inclusi nel Web categorie Training Kit, disponibile all'indirizzo [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Panoramica

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questa esercitazione pratica, si apprenderà come:

- Utilizzare Essentials Web include, ad esempio i frammenti di codice completi HTML5 e codifica Zen nuove funzionalità dell'editor HTML
- Usare nuove funzionalità dell'editor CSS Web Essentials include, ad esempio la selezione dei colori e una descrizione comando di matrice di Browser
- Usare le nuove funzionalità di editor JavaScript incluse in Essentials Web, ad esempio l'estrazione di File e IntelliSense per tutti gli elementi HTML
- Scambiare i dati tra il browser e Visual Studio mediante collegamento Browser

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Di seguito è necessario per completare questa esercitazione pratica:

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) o versione successiva
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

Per eseguire gli esercizi di questa esercitazione pratica, è necessario configurare prima di tutto l'ambiente.

1. Aprire una finestra di Esplora risorse e individuare il lab **origine** cartella.
2. Fare doppio clic su **Setup.cmd** e selezionare **Esegui come amministratore** per avviare il processo di installazione che la configurazione dell'ambiente e installare i frammenti di codice di Visual Studio per questo ambiente lab.
3. Se viene visualizzata la finestra di dialogo controllo dell'Account utente, confermare l'azione per continuare.

> [!NOTE]
> Assicurarsi di avere selezionato tutte le dipendenze per questa esercitazione prima di eseguire il programma di installazione.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Utilizzo dei frammenti di codice

In tutto il documento lab, verrà invitati a inserire i blocchi di codice. Per praticità, la maggior parte di questo codice viene fornito come Visual Studio frammenti di codice che è possibile accedere da all'interno di Visual Studio 2013 per evitare di dover aggiungerla manualmente.

> [!NOTE]
> Ogni esercizio è accompagnata da una soluzione inizia all'interno di **iniziare** cartella dell'esercizio che consente di seguire ogni esercizio indipendentemente dalle altre. Tenere presente che i frammenti di codice aggiunti durante un esercizio non sono presenti queste soluzioni di avvio e potrebbero non funzionare fino a completare l'esercizio. All'interno del codice sorgente per un esercizio, si avrà inoltre un **fine** cartella che contiene una soluzione di Visual Studio con il codice generato da completare i passaggi nell'esercizio corrispondente. Se è necessario ulteriore assistenza durante l'utilizzo tramite questa esercitazione pratica, è possibile usare queste soluzioni come guida.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questa esercitazione include gli esercizi seguenti:

1. [Utilizzo di collegamento del Browser e Web Essentials](#Exercise1)
2. [Sfruttando la possibilità di IntelliSense e frammenti di codice](#Exercise2)

> [!NOTE]
> Al primo avvio di Visual Studio, è necessario selezionare una delle raccolte di impostazioni predefinite. Ogni raccolta predefinita è progettato in modo che corrisponda a un particolare stile di sviluppo e determina il layout di finestra, il comportamento dell'editor, frammenti di codice IntelliSense e finestre di dialogo. Le procedure descritte in questa esercitazione vengono descritte le azioni necessarie per eseguire una determinata attività in Visual Studio quando si utilizza il **impostazioni generali per lo sviluppo** insieme. Se si sceglie una raccolta di impostazioni diverse per l'ambiente di sviluppo, possono essere presenti differenze nei passaggi che è necessario prendere in considerazione.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>Esercizio 1: Utilizzo di collegamento del Browser e Web Essentials

**Web Essentials** è un'estensione di Visual Studio che aggiunge una serie di funzionalità utili per lo sviluppo web moderna, principalmente concentrato sulla semplificazione l'esperienza di sviluppo web molto più veloce e più piacevole. È possibile installare Essentials Web dalla raccolta di estensioni in Visual Studio.

**Collegamento del browser** è una nuova funzionalità inclusa in Visual Studio 2013 che fornisce un canale tra l'IDE di Visual Studio e qualsiasi browser aperto per lo scambio di dati tra l'applicazione web e Visual Studio. Essentials Web estende il collegamento del Browser con gli strumenti per modificare il modello a oggetti DOM e gli stili CSS delle pagine web direttamente dal browser.

In questo esercizio, è esplorare alcune delle funzionalità supportate da **Essentials Web** e **collegamento Browser** per migliorare una pagina semplice quiz.

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>Attività 1: esecuzione del progetto in più browser

In questa attività si configurerà l'applicazione web per l'esecuzione in più browser in una sola volta, che risulta utile per il test tra browser.

1. Aprire **Microsoft Visual Studio**.
2. Nel **File** dal menu **aprire | Progetto/soluzione...**  e passare a **Ex1 WorkingwithBrowserLinkandWebEssentials\Begin** nel **origine** cartella dell'esercitazione (C:\WebCampsTK\HOL\VSWebTooling\Source). Selezionare **Begin.sln** e fare clic su **aprire**.
3. Sulla barra degli strumenti di Visual Studio, espandere il menu di browser e selezionare **Esplora con...** .

    ![Esplora con opzione di menu](visual-studio-2013-web-tools/_static/image1.png "Sfoglia con... nel menu di browser")

    *Esplora con opzione di menu*
4. Nel **Esplora con** la finestra di dialogo, selezionare **Google Chrome** e **Internet Explorer** tenendo premuto il **CTRL** chiave e fare clic su  **Imposta come predefinito**.

    ![Selezionare la finestra di dialogo](visual-studio-2013-web-tools/_static/image2.png "Sfoglia con la finestra di dialogo")

    *Selezione di più browser predefinito*
5. Google Chrome e Internet Explorer verrà ora visualizzata come browser predefinito. Fare clic su **Annulla** per chiudere la finestra di dialogo.

    ![Internet Explorer come browser predefinito e Google Chrome](visual-studio-2013-web-tools/_static/image3.png "Internet Explorer e Google Chrome browser predefiniti")

    *Google Chrome e Internet Explorer come browser predefiniti*

    > [!NOTE]
    > Dopo aver configurato il browser predefinito, il **più browser** opzione nel menu del browser.
    > 
    > ![Più browser](visual-studio-2013-web-tools/_static/image4.png "più browser")
6. Premere **CTRL** + **F5** per eseguire l'applicazione senza eseguire il debug.
7. Quando entrambe le finestre del browser di aprono, inserire uno di essi sopra l'altro per visualizzare gli aggiornamenti in entrambi i browser contemporaneamente. Il browser dovrebbe visualizzare una pagina web con un rettangolo blu chiaro.

    ![Inserimento di un browser sopra l'altro](visual-studio-2013-web-tools/_static/image5.png "immissione uno browser sopra l'altro")

    *Inserimento di un browser sopra l'altro*
8. Non chiudere il browser. Utilizzare li nell'attività successiva.

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>Attività 2: Zen mediante la generazione di codice per creare elementi HTML

**Codifica Zen** è un editor di plug-in per HTML ad alta velocità, XML, XSL (o qualsiasi altro formato codice strutturato) codifica e la modifica. Il core di questo plug-in è un motore abbreviazione potente che consente di espandere le espressioni - simile a selettori CSS - nel codice HTML. Codifica Zen è un modo rapido per scrivere una sintassi del selettore di stile HTML tramite un CSS.

In questo esercizio, si utilizzerà la funzionalità di codifica Zen fornita da Essentials Web per generare i pulsanti HTML che rappresentano le opzioni della domanda.

1. Tornare a Visual Studio.
2. Aprire il **cshtml** file si trova nella **viste** | **Home** cartella.
3. Sostituire il **&lt;!: TODO: aggiungere qui - opzioni&gt;** commento con il codice seguente e premere **scheda**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. Il codice deve essere espanso in formato HTML.

    ![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")

    *Expanded HTML*

    > [!NOTE]
    > Per ulteriori informazioni sulla sintassi di codifica Zen, vedere gli argomenti seguenti [articolo](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).
5. Fare clic su di **aggiornare il browser collegato** per aggiornare entrambi i browser.

    ![Aggiornare il browser collegato](visual-studio-2013-web-tools/_static/image7.png "aggiornare browser collegato")

    *Aggiornare il browser collegato*

    ![Internet Explorer - pagina aggiornata con quattro pulsanti](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - pagina aggiornata con quattro pulsanti")

    *Internet Explorer - pagina aggiornata con quattro pulsanti*

    ![Google Chrome - pagina aggiornata con quattro pulsanti](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - pagina aggiornata con quattro pulsanti")

    *Google Chrome - pagina aggiornata con quattro pulsanti*
6. Tornare a Visual Studio.
7. I pulsanti aggiunti alla pagina, ma è comunque necessario aggiungere un modello di domanda. A tale scopo, si utilizzerà una nuova funzionalità in Essentials Web chiamato **generatore Lorem Ipsum**. Individuare il **div** elemento con la **classe** attributo **anteriore**.
8. Aggiungere il codice seguente come primo elemento figlio del **div**e premere **scheda**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. Il codice deve essere espanso in formato HTML.

    ![Generato automaticamente Lorem Ipsum](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum generato automaticamente")

    *Lorem Ipsum generato automaticamente*

    > [!NOTE]
    > Come parte della codifica Zen, è ora possibile generare codice Lorem Ipsum direttamente nell'editor HTML. Digitare semplicemente **lorem** e fare clic su **scheda** e un 30 word Lorem Ipsum verrà inserito il testo. Ad esempio, *lorem10* inserisce 10 Lorem Ipsum parole.
10. Si aggiungerà un logo nella parte superiore della domanda tramite un'altra nuova funzionalità in Essentials Web chiamato **generatore Lorem Pixel**. Aggiungere il codice seguente come primo elemento figlio del **div** elemento con **contenitore** come **classe** valore, quindi premere **scheda**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. È necessario espandere il codice HTML.

    ![Generato automaticamente lorem Pixel](visual-studio-2013-web-tools/_static/image11.png "generato automaticamente Lorem Pixel")

    *Generato automaticamente lorem Pixel*

    > [!NOTE]
    > Come parte della codifica Zen, è anche possibile generare codice del Lorem Pixel direttamente nell'editor HTML. Digitare semplicemente **pix-200 x 200-animali** e fare clic su **scheda** e **img** tag con un'immagine di 200 x 200 dell'animale verrà inserito. Per ulteriori informazioni, vedere [Lorem Pixel](http://www.lorempixel.com).
12. Fare clic su di **aggiornare il browser collegato** per aggiornare entrambi i browser.

    ![Internet Explorer - immagine generata automaticamente e il testo](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - generato automaticamente immagine e testo")

    *Internet Explorer - generato automaticamente immagine e testo*

    ![Google Chrome - immagine generata automaticamente e il testo](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - generato automaticamente immagine e testo")

    *Google Chrome - generato automaticamente immagine e testo*

    > [!NOTE]
    > Poiché l'immagine viene selezionato casualmente quando si aggiunge il frammento di codice, l'immagine visualizzata nel browser potrebbe differire.
13. Non chiudere il browser. Utilizzare li nell'attività successiva.

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>Attività 3: aggiornamento di una proprietà di stile

In questa attività, utilizzare il collegamento Browser **controllare la modalità** funzionalità per rilevare la posizione esatta in cui viene generato l'elemento DOM specifico e quindi aggiornare la proprietà color di tale elemento utilizzando un selettore di colore fornito dal Web Essentials.

1. Nel browser Internet Explorer, premere **CTRL** + **ALT** + **si** per abilitare la modalità di ispezionare.
2. Sposta il puntatore sul bordo blu chiaro e fare clic su.

    ![Spostando il puntatore sul bordo blu chiaro](visual-studio-2013-web-tools/_static/image14.png "spostando il puntatore sul bordo blu chiaro")

    *Spostando il puntatore sul bordo blu chiaro*
3. Tornare a Visual Studio. Si noti come l'elemento HTML selezionato nel browser viene anche selezionato nell'editor di Visual Studio HTML.

    ![Elemento HTML selezionato nell'editor di Visual Studio HTML](visual-studio-2013-web-tools/_static/image15.png "elemento HTML selezionato nell'editor di Visual Studio HTML")

    *Elemento HTML selezionato nell'editor di Visual Studio HTML*
4. Ora si aggiornerà il **anteriore** classe CSS per modificare lo stile dell'elemento selezionato. A tale scopo, premere **CTRL** + **,** per aprire la **passa a** casella di ricerca. Tipo **site.css** e premere **invio** per aprire il file.

    ![Apertura di file CSS](visual-studio-2013-web-tools/_static/image16.png "l'apertura del file Site. CSS")

    *Apertura del file Site. CSS*
5. Premere **CTRL** + **F** e tipo **un contenitore di .flip .front** per trovare il selettore CSS.
6. Fare clic sul quadrato di colore blu chiaro nella proprietà della classe per aprire il selettore di colore del bordo.

    ![Aprire il selettore di colore](visual-studio-2013-web-tools/_static/image17.png "aprire il selettore di colore")

    *Aprire il selettore di colore*
7. Espandere la selezione colore facendo clic sul pulsante freccia di espansione e selezionare un nuovo colore.

    ![Espandere la selezione colori](visual-studio-2013-web-tools/_static/image18.png "espandendo la selezione colori")

    *Espandere la selezione colori*
8. Premere **CTRL** + **ALT** + **invio** per aggiornare il browser collegato.
9. Passare a Internet Explorer e notare come è stato modificato il colore del bordo.

    ![Internet Explorer - colore del bordo aggiornato](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - colore del bordo aggiornato")

    *Internet Explorer - colore del bordo aggiornato*
10. Passa a Google Chrome e come è stato modificato il colore del bordo.

    ![Google Chrome - colore del bordo aggiornato](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - colore del bordo aggiornato")

    *Google Chrome - colore del bordo aggiornato*
11. Tornare a Visual Studio.
12. Passare alla fine del **Site.css** file e premere **CTRL** + **F** per individuare il **.btn** selettore.
13. Si noti che il **- webkit-border-radius** proprietà è sottolineata in verde.

    ![proprietà - webkit-border-radius del selettore di tasto](visual-studio-2013-web-tools/_static/image21.png "- webkit-border-radius proprietà del selettore di tasto")

    *proprietà - webkit-border-radius del selettore di tasto*
14. Posizionare il cursore nel **- webkit-border-radius** proprietà. Una linea blu dovrebbe essere visualizzato sotto la prima lettera della parola prima della proprietà. Si tratta di **smart tag**.
15. Premere **CTRL** + **.** Aprire il menu di suggerimenti e fare clic su **Aggiungi manca una proprietà standard (border-radius)**.

    ![Componente mancante suggerimento proprietà standard](visual-studio-2013-web-tools/_static/image22.png "Aggiungi mancante suggerimento proprietà standard")

    *Aggiungi mancanti suggerimento proprietà standard*
16. Il **border-radius** proprietà viene aggiunta automaticamente alla regola CSS.

    ![Manca una proprietà standard aggiunti](visual-studio-2013-web-tools/_static/image23.png "manca una proprietà standard aggiunta")

    *Manca una proprietà standard aggiunta*
17. Sposta il puntatore su di **border-radius** proprietà per visualizzare il **Browser matrice tooltip**. Il **Browser matrice tooltip** Mostra la disponibilità della proprietà in ogni browser.

    ![Descrizione comando matrice browser](visual-studio-2013-web-tools/_static/image24.png "Browser matrice tooltip")

    *Descrizione comando di una matrice di browser*
18. Si noti che il valore di **border-radius** proprietà è ancora sottolineato. Spostare il puntatore sul valore per visualizzare il messaggio di avviso.

    ![Avviso di valore di proprietà Border-radius](visual-studio-2013-web-tools/_static/image25.png "avviso valore di proprietà Border-radius")

    *Avviso di valore di proprietà Border-radius*
19. Rimuovere l'unità del **border-radius** valore della proprietà come indicato dalla descrizione comando.
20. Come **border-radius** è la proprietà standard per la definizione arrotondato come bordo angoli sono, è possibile rimuovere il **- webkit-border-radius** proprietà e il valore in base alla regola CSS.
21. Posizionare il cursore nel **ritorno a capo automatico** proprietà e notare che lo smart tag viene visualizzato anche sotto.
22. Aprire il menu e fare clic su **aggiungere specifiche del fornitore mancante**.

    ![Aggiungere mancanti suggerimento di specifiche del fornitore](visual-studio-2013-web-tools/_static/image26.png "aggiungere mancanti suggerimento di specifiche del fornitore")

    *Aggiungere mancanti suggerimento di specifiche del fornitore*
23. Il **-ms-word-wrap** proprietà viene aggiunta automaticamente alla regola CSS.

    ![Proprietà specifiche del fornitore aggiunta](visual-studio-2013-web-tools/_static/image27.png "aggiunta la proprietà specifica del fornitore")

    *Aggiunta delle proprietà specifiche del fornitore*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>Attività 4 - aggiornamento del codice HTML dal Browser

In questa attività, utilizzare il collegamento Browser **modalità progettazione** funzionalità per modificare l'oggetto DOM dal browser e trasferire le modifiche al file di origine HTML in Visual Studio.

1. Google Chrome, premere **CTRL** + **ALT** + **D** per abilitare la modalità di progettazione.
2. Sposta il puntatore su di **Lorem Ipsum dolor sit amet** etichetta e fare clic su.

    ![Modifica la domanda](visual-studio-2013-web-tools/_static/image28.png "modifica la domanda")

    *Modifica la domanda*
3. Dovrebbe essere visualizzato un cursore. Sostituire il testo originale con *does aspetto analogo al seguente quando si scrive una domanda più?*, quindi premere **ESC** per uscire dalla modalità progettazione.

    ![Domanda modificato](visual-studio-2013-web-tools/_static/image29.png "domanda modificato")

    *Domanda modificato*
4. Commutatore tornare a Visual Studio e aprire **cshtml**, se non è ancora aperto. Si noti che il testo interno del **&lt;p&gt;** elemento è stato aggiornato.

    ![Domanda aggiornate nella pagina HTML](visual-studio-2013-web-tools/_static/image30.png "domanda aggiornate nella pagina HTML")

    *Domanda aggiornata nella pagina HTML*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>Attività 5 - esaminare i risultati SEO avvisi correlati

**Ottimizzazione del motore di ricerca** (SEO) è il processo di creazione di una classificazione di sito Web superiore nell'elenco dei motori di ricerca dei risultati. Maggiore di classifica il sito e in modo più coerente è elencato, i visitatori più il sito verrà visualizzato da tale motore di ricerca. Essentials Web incorpora uno strumento di analisi che esamina l'HTML, i problemi di report trovato e fornisce assistenza per correggerli.

1. Passare al **vista** menu e fare clic su **elenco errori** per aprire la **elenco errori** finestra.

    ![Errore nella visualizzazione elenco menu](visual-studio-2013-web-tools/_static/image31.png "elenco errori nel menu Visualizza")

    *Errore nella visualizzazione elenco a menu*
2. Si noti che è un avviso SEO notifica che un **&lt;meta&gt;** tag per la descrizione della pagina è manca. Fare doppio clic sulla voce avviso SEO per risolvere il problema.

    ![Finestra Elenco errori](visual-studio-2013-web-tools/_static/image32.png "finestra Elenco errori")

    *Finestra Elenco errori*
3. Nel **Essentials Web** la finestra di dialogo, fare clic su **Sì** per inserire una descrizione &lt;meta&gt; tag.

    ![Finestra di dialogo Web Essentials](visual-studio-2013-web-tools/_static/image33.png "finestra di dialogo Web Essentials")

    *Finestra di dialogo Web Essentials*
4. L'editor per  **\_cshtml** apre e **&lt;meta&gt;** tag viene aggiunto automaticamente al **head** sezione del File HTML.

    ![Tag Meta aggiunti automaticamente nella pagina layout](visual-studio-2013-web-tools/_static/image34.png "tag Meta aggiunti automaticamente nella pagina layout")

    *Tag Meta aggiunti automaticamente al \_pagina Layout*
5. Modificare il valore della **contenuto** attributo *GeekQuiz* e salvare il file.

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>Esercizio 2: Sfruttando la possibilità di IntelliSense e frammenti di codice

Con Essentials Web, l'editor HTML è stato esteso con funzionalità aggiuntive. In questo esercizio, si noterà alcune nuove funzionalità che sono utili durante lo sviluppo di applicazioni web.

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>Attività 1: utilizzo di IntelliSense nei documenti HTML

La nuova funzionalità prima, verrà visualizzato in questa attività viene chiamata **IntelliSense dinamico**. IntelliSense dinamica legge altri tag e attributi di dedurre le possibili ID che verrà utilizzato.

In questa attività si creerà un nuovo elemento di form HTML che contiene un'etichetta e un campo di input. Aggiungerà un **per** attributo nell'etichetta e associarlo all'input, per visualizzare i suggerimenti di IntelliSense basati su ID di input nell'ambito.

1. Aprire **Visual Studio Express 2013 per Web** e **Begin.sln** soluzione si trova nel **origine/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/inizio** cartella. In alternativa, è possibile continuare con la soluzione ottenuta nell'esercizio precedente.
2. In **Esplora**, aprire il **cshtml** file si trova nella **viste** | **Home** cartella.
3. Aggiungere il form seguente all'interno di **&lt;sezione&gt;** elemento.

    (- Frammento di codice *VisualStudio2013WebTooling* - *Ex2* - *modulo*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. Il tag di input deve essere preceduto da un'etichetta con una descrizione del campo. Aggiungere la seguente etichetta prima del tag di input.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. Il **per** attributo di un **&lt;etichetta&gt;** specifica quale elemento modulo un'etichetta è associato. Il valore dell'attributo deve essere uguale all'id dell'elemento correlato. Aggiungere il **per** attributo la **&lt;etichetta&gt;** elemento. Come illustrato nella figura seguente, il &quot;nome&quot; valore viene visualizzata nella finestra di IntelliSense, in base all'id degli elementi all'interno dell'ambito stesso (che li racchiude  **&lt;modulo&gt;**).

    ![Con l'id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "che mostra l'id in IntelliSense")

    *Con l'id in IntelliSense*
6. Eliminare l'aggiunta di recente **&lt;modulo&gt;** elemento e il relativo contenuto.

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>Attività 2: utilizzare frammenti di codice HTML

HTML5 introdotte più di 25 tag semantici nuovo. Visual Studio è già stato supporto IntelliSense per questi tag, ma Visual Studio 2013 che rende più veloce e semplice scrivere tag mediante l'aggiunta di nuovi frammenti di codice. Anche se questi tag non sono più complessi, hanno alcune sfumature di piccole dimensioni, ad esempio l'aggiunta di fallback codec corretto per il *audio* tag. In questa attività, verranno visualizzati i frammenti di codice HTML per il tag audio.

1. Nel **cshtml** file, digitare  **&lt;aud** all'interno di **&lt;sezione&gt;** elemento, come illustrato nella figura seguente.

    ![Inserimento di un elemento audio](visual-studio-2013-web-tools/_static/image36.png "inserimento di un elemento audio")

    *Inserimento di un elemento audio*
2. Premere **scheda** due volte e si noti che il codice seguente viene aggiunto nella pagina e il cursore si trova il **src** attributo della prima origine.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > Premendo il **scheda** chiave due volte, viene inserito il frammento di codice. Nel frammento audio viene illustrato l'utilizzo di standard di *audio* tag, con due file di origine per un supporto migliorato.
3. Eliminare la seconda riga e aggiornare l'origine della prima riga con il seguente collegamento per lo stato di visualizzazione WebCampsTV Katana: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3). Il codice risultante è illustrato di seguito.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > Il file di origine *KatanaProject.mp3* viene utilizzato come esempio. Se si preferisce, è possibile utilizzare un'altra origine.
4. Premere **CTRL** + **S** per salvare il file.
5. Premere **CTRL** + **F5** per avviare l'applicazione.
6. Si noti che un lettore audio è stato aggiunto all'applicazione.

    ![Audio Windows Media player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "lettori Audio di Internet Explorer")

    *Audio Windows Media player in Internet Explorer*

    ![Lettore audio in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "lettori Audio in Google Chrome")

    *Lettore audio in Google Chrome*
7. Non chiudere il browser. Utilizzare li nell'attività successiva.

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>Attività 3: utilizzo di IntelliSense in JavaScript documenti

Con Web Essentials 2013, pagine HTML e fogli di stile producono un elenco di ID e i nomi di classe. In questa attività si apprenderà come tali elenchi migliorano il supporto IntelliSense per JavaScript in Web Essentials 2013.

1. Nel **cshtml** file, aggiungere il codice seguente per definire un **script** tag per il codice JavaScript.

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. Aggiungere il seguente codice all'interno di **script** tag per definire la funzione di callback pronto.

    (- Frammento di codice *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. Posizionare il cursore nel **script** tag, quindi premere **CTRL** + **.** Per aprire il menu di suggerimento.
4. Fare clic su **estrarre in File**.

    ![Estrarre suggerimento file JavaScript](visual-studio-2013-web-tools/_static/image39.png "JavaScript Estrai nel suggerimento di file")

    *JavaScript Estrai nel suggerimento di file*
5. Nel **Salva con nome** finestra, seleziona il **script** cartella, nome del file **init.js** e fare clic su **salvare**.

    ![Finestra Salva con nome](visual-studio-2013-web-tools/_static/image40.png "finestra Salva con nome")

    *Finestra Salva con nome*

    > [!NOTE]
    > Il **init.js** file viene creato e il file viene spostato il contenuto dello script.
    > 
    > ![File Init.js creato con il contenuto incluso](visual-studio-2013-web-tools/_static/image41.png "Init.js file creato con il contenuto incluso")
    > 
    > *File Init.js creato con il contenuto incluso*
6. Aprire il **cshtml** file e verificare che il tag di script è stato sostituito con un riferimento al **init.js** file.

    ![Riferimento html Init.js](visual-studio-2013-web-tools/_static/image42.png "riferimento html Init.js")

    *Riferimento html Init.js*
7. Passare al **Esplora** e notare che il **init.js** file è stato incluso automaticamente nella soluzione.

    ![File Init.js incluso nella soluzione](visual-studio-2013-web-tools/_static/image43.png "Init.js file incluso nella soluzione")

    *File Init.js incluso nella soluzione*
8. Tornare al **init.js** file per aggiornare il **pronto** funzione di callback.
9. All'interno di definizione della funzione di callback che viene passata a *pronto*, aggiungere il codice seguente per ottenere tutti gli elementi da un attributo di classe specifico.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. Premere **CTRL** + **spazio** tra le virgolette all'interno di **getElementsByClassName** chiamata di funzione.

    ![Visualizzazione IntelliSense per la funzione getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "visualizzare IntelliSense per la funzione getElementsByClassName")

    *Visualizzazione IntelliSense per la funzione getElementsByClassName*

    > [!NOTE]
    > Si noti che IntelliSense mostra le classi definite nei fogli di stile di progetto.
11. Sostituire la riga che è stato creato con il codice seguente.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. Posizionare il cursore dopo **Australia** all'interno delle virgolette nel **getElementsByTagName** (funzione) e premere **CTRL** + **spazio**.

    ![Visualizzazione IntelliSense per il metodo getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "IntelliSense che mostra il metodo getElementByTagName")

    *Visualizzazione IntelliSense per il metodo getElementsByTagName*
13. Selezionare **&quot;audio&quot;** nell'elenco e premere **invio**. Il risultato è illustrato nella figura seguente.

    ![Recupero di elementi Audio](visual-studio-2013-web-tools/_static/image46.png "recupero di elementi Audio")

    *Recupero di elementi Audio*
14. In **Esplora**, fare doppio clic sul **init.js** file nel **script** cartella e selezionare **file JavaScript minimizzare** dal **Web Essentials** menu.

    ![Minimizzare i file JavaScript](visual-studio-2013-web-tools/_static/image47.png "file minimizzare JavaScript")

    *Minimizzare i file JavaScript*
15. Quando viene richiesto di abilitare la riduzione automatica quando le modifiche al file di origine fare clic su **Sì**.

    ![L'abilitazione di avviso automatico minimizzazione](visual-studio-2013-web-tools/_static/image48.png "abilitazione avviso minimizzazione automatico")

    *L'abilitazione di avviso di riduzione automatica*

    > [!NOTE]
    > Il **init.min.js** viene creato e viene aggiunto come una dipendenza di **init.js** file.
    > 
    > ![File Init.min.js creato](visual-studio-2013-web-tools/_static/image49.png "file Init.min.js creato")
    > 
    > *File Init.min.js creato*
16. Aprire il **init.min.js** file e notare che il file è minimizzare.

    ![Contenuto del file Init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js contenuto del file")

    *Contenuto del file Init.min.js*
17. Nel **init.js** file, aggiungere il codice seguente sotto il **getElementsByTagName** chiamata di funzione per riprodurre tutti gli elementi audio.

    (- Frammento di codice *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. Fare clic su **CTRL** + **S** per salvare il file. Poiché il file minimizzato è già aperto, si verrà visualizzata una finestra di dialogo che informa che il file è stato modificato all'esterno dell'editor di origine. Scegliere **Sì**.

    ![Avviso di Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "avviso di Microsoft Visual Studio")

    *Avviso di Microsoft Visual Studio*
19. Tornare al **init.min.js** file per verificare che il file è stato aggiornato con il nuovo codice.

    ![File Init.min.js aggiornato](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file aggiornato")

    *File Init.min.js aggiornato*
20. Fare clic su di **aggiornamento del collegamento Browser** pulsante.
21. Dopo che entrambi i browser vengono aggiornati i lettori audio che è stato evidenziato nell'attività precedente verranno avviata la riproduzione automatica.

    ![Lettore audio incluso nella visualizzazione](visual-studio-2013-web-tools/_static/image53.png "lettori Audio incluso nella visualizzazione")

    *Lettore audio incluso nella visualizzazione*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa esercitazione si è appreso come:

- Utilizzare Essentials Web include, ad esempio i frammenti di codice completi HTML5 e codifica Zen nuove funzionalità dell'editor HTML
- Usare nuove funzionalità dell'editor CSS Web Essentials include, ad esempio la selezione dei colori e una descrizione comando di matrice di Browser
- Usare le nuove funzionalità di editor JavaScript incluse in Essentials Web, ad esempio l'estrazione di File e IntelliSense per tutti gli elementi HTML
- Scambiare i dati tra il browser e Visual Studio mediante collegamento Browser
