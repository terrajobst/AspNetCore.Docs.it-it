---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Laboratorio pratico: Visual Studio 2013 Web Tools | Microsoft Docs'
author: rick-anderson
description: Visual Studio è un ambiente di sviluppo straordinaria per. Windows basata su rete e i progetti web. Include un editor di testo potente che può essere utilizzato con facilità per...
ms.author: aspnetcontent
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 9553d3129ff4c942eacbba628d1daaf6c4e33635
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807528"
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a>Laboratorio pratico: Visual Studio 2013 Web Tools
====================
da [Camp Web Team](https://twitter.com/webcamps)

[Download Web Camp Kit di formazione](http://aka.ms/webcamps-training-kit)

> Visual Studio è un ambiente di sviluppo straordinaria per. Windows basata su rete e i progetti web. Include un editor di testo potente che può essere facilmente utilizzato per modificare i file autonomi senza un progetto.
> 
> Visual Studio gestisce un albero di analisi complete durante la modifica di ogni file. In questo modo Visual Studio per fornire durante l'esecuzione molto più veloce e più piacevole l'esperienza di sviluppo senza precedenti il completamento automatico e le azioni in base al documento. Queste funzionalità sono particolarmente efficace in documenti HTML e CSS.
> 
> Tutta questa capacità è disponibile anche per le estensioni, rendendo più semplice estendere l'editor con nuove potenti funzionalità in base alle esigenze. Web Essentials è una raccolta (principalmente) miglioramenti relativi al web per Visual Studio. Include numerose nuove completamenti IntelliSense (soprattutto per CSS), nuove funzionalità di collegamento del Browser, automatic file JSHint per JavaScript, nuovi avvisi per HTML e CSS e molte altre funzionalità che sono essenziali per lo sviluppo web moderno.
> 
> Tutto il codice di esempio e frammenti di codice sono inclusi nel Web Camp Kit di formazione, disponibile all'indirizzo [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Panoramica

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico, si apprenderà come:

- Usare nuove funzionalità dell'editor HTML incluse in Web Essentials, ad esempio i frammenti di codice HTML5 avanzati e codifica Zen
- Usare nuove funzionalità dell'editor CSS incluse in Web Essentials, ad esempio la selezione dei colori e una descrizione comando di matrice di Browser
- Usare nuove funzionalità dell'editor JavaScript inclusa in Web Essentials, ad esempio l'estrazione di File e IntelliSense per tutti gli elementi HTML
- Scambiare i dati tra browser e Visual Studio usando il collegamento del Browser

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Di seguito è necessario per completare questo laboratorio pratico:

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) o versione successiva
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

Per eseguire gli esercizi in questo laboratorio pratico, è necessario configurare prima di tutto l'ambiente.

1. Aprire una finestra di Windows Explorer e passare a del lab **origine** cartella.
2. Fare doppio clic su **Setup. cmd** e selezionare **Esegui come amministratore** per avviare il processo di installazione che la configurazione dell'ambiente e installare i frammenti di codice di Visual Studio per questo ambiente lab.
3. Se viene visualizzata la finestra di dialogo controllo dell'Account utente, confermare l'azione per continuare.

> [!NOTE]
> Assicurarsi che tutte le dipendenze per questo ambiente lab che sono stati verificati prima di eseguire il programma di installazione.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Usando i frammenti di codice

In tutto il documento di laboratorio, verrà invitati a inserire blocchi di codice. Per praticità, la maggior parte di questo codice viene fornito come Visual Studio frammenti di codice che è possibile accedere da all'interno di Visual Studio 2013, per evitare che sia necessario aggiungerla manualmente.

> [!NOTE]
> Ogni esercizio è accompagnata da una soluzione inizia che si trova nel **iniziare** cartella dell'esercizio che consente di seguire ogni esercizio indipendentemente dagli altri. Tenere presente che i frammenti di codice aggiunti durante un esercizio non sono presenti queste soluzioni di avvio e potrebbero non funzionare fino a quando non si have completato l'esercizio. All'interno del codice sorgente per un esercizio, si noterà anche un **End** cartella che contiene una soluzione di Visual Studio con il codice che scaturisce da completare i passaggi nell'esercizio corrispondente. È possibile usare queste soluzioni come prassi consigliata se ti serve assistenza aggiuntiva durante l'esecuzione in questo laboratorio pratico.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questo laboratorio pratico include gli esercizi seguenti:

1. [Uso di Browser Link e Web Essentials](#Exercise1)
2. [Sfruttando i vantaggi di IntelliSense e frammenti di codice](#Exercise2)

> [!NOTE]
> Al primo avvio di Visual Studio, è necessario selezionare una delle raccolte di impostazioni predefinite. Ogni raccolta predefinita è pensata per associare un particolare stile di sviluppo e determina i layout delle finestre, il comportamento dell'editor, frammenti di codice IntelliSense e opzioni della finestra di dialogo. Le procedure descritte in questa esercitazione vengono descritte le azioni necessarie per eseguire una determinata attività in Visual Studio quando si usa la **delle impostazioni di sviluppo generale** raccolta. Se si sceglie una raccolta di impostazioni diverse per l'ambiente di sviluppo, possono essere presenti differenze nei passaggi che è necessario tenere conto.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>Esercizio 1: Uso di Browser Link e Web Essentials

**Web Essentials** è un'estensione di Visual Studio che aggiunge numerose funzionalità utili per lo sviluppo web moderno, principalmente incentrato sul rendere l'esperienza di sviluppo web molto più veloce e più piacevole. È possibile installare Web Essentials dalla raccolta di estensioni in Visual Studio.

**Collegamento del browser** è una nuova funzionalità inclusa in Visual Studio 2013 che fornisce un canale tra l'IDE di Visual Studio e qualsiasi browser aperto per lo scambio di dati tra l'applicazione web e Visual Studio. Web Essentials estende il collegamento del Browser con gli strumenti per modificare il modello a oggetti DOM e gli stili CSS delle pagine web direttamente dal browser.

In questo esercizio, verrà illustrato alcune delle funzionalità supportate da **Web Essentials** e **Browser Link** per migliorare una pagina semplice quiz.

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>Attività 1: esecuzione del progetto in più browser

In questa attività si configurerà l'applicazione web per l'esecuzione in più browser in una sola volta, questo è utile per i test tra browser.

1. Aprire **Microsoft Visual Studio**.
2. Nel **File** dal menu **Open | Progetto/soluzione...**  e passare a **Ex1 WorkingwithBrowserLinkandWebEssentials\Begin** nel **origine** cartella del lab (C:\WebCampsTK\HOL\VSWebTooling\Source). Selezionare **Begin.sln** e fare clic su **Open**.
3. Sulla barra degli strumenti di Visual Studio, espandere il menu di scelta del browser e selezionare **Esplora con...** .

    ![Esplora con opzione di menu](visual-studio-2013-web-tools/_static/image1.png "Sfoglia con... nel menu browser")

    *Esplora con opzione di menu*
4. Nel **Esplora con** finestra di dialogo, selezionare **Google Chrome** e **Internet Explorer** tenendo premuto il **CTRL** della chiave e fare clic su  **Imposta come predefinito**.

    ![Esplorare la finestra di dialogo](visual-studio-2013-web-tools/_static/image2.png "Sfoglia con la finestra di dialogo")

    *Selezione di più browser predefiniti*
5. Google Chrome sia Internet Explorer deve essere visualizzata come browser predefinito. Fare clic su **annullare** per chiudere la finestra di dialogo.

    ![Google Chrome e Internet Explorer come browser predefiniti](visual-studio-2013-web-tools/_static/image3.png "Internet Explorer e Google Chrome browser predefiniti")

    *Google Chrome e Internet Explorer come browser predefiniti*

    > [!NOTE]
    > Dopo aver configurato il browser predefinito, il **più browser** opzione è selezionata nel menu di scelta del browser.
    > 
    > ![Più browser](visual-studio-2013-web-tools/_static/image4.png "più browser")
6. Premere **CTRL** + **F5** per eseguire l'applicazione senza eseguire il debug.
7. Quando entrambe le finestre del browser aprono, posizionati uno sopra l'altro per poterlo visualizzare gli aggiornamenti in entrambi i browser contemporaneamente. Il browser dovrebbe visualizzare una pagina web con un rettangolo blu chiaro.

    ![Inserimento di un browser sopra l'altro](visual-studio-2013-web-tools/_static/image5.png "inserimento uno browser sopra l'altro")

    *Inserimento di un browser sopra l'altro*
8. Non chiudere il browser. Verranno usati nell'attività successiva.

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>Attività 2: Zen usando la codifica per creare elementi HTML

**Scrittura di codice Zen** è un editor di plug-in per HTML ad alta velocità, XML, XSL (o qualsiasi altro formato codice strutturato) codifica e la modifica. Alla base di questo plug-in è un motore di abbreviazione potente che consente di espandere le espressioni - simile a selettori CSS - nel codice HTML. Il codice Zen rappresenta un modo rapido per scrivere sintassi del selettore di stile HTML tramite un CSS.

In questo esercizio si userà la funzionalità di codifica Zen fornita da Web Essentials per generare i pulsanti HTML che rappresentano le opzioni della domanda.

1. Tornare a Visual Studio.
2. Aprire il **index. cshtml** file che si trova nel **viste** | **Home** cartella.
3. Sostituire il **&lt;!-TODO: aggiungere qui, le opzioni&gt;** commento con il codice seguente e premere **scheda**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. Il codice deve essere espanso in formato HTML.

    ![HTML espanso](visual-studio-2013-web-tools/_static/image6.png "espanso HTML")

    *HTML espanso*

    > [!NOTE]
    > Per altre informazioni sulla sintassi di codifica Zen, vedere gli argomenti seguenti [articolo](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).
5. Scegliere il **Aggiorna browser collegati** pulsante per aggiornare entrambi i browser.

    ![Aggiorna browser collegati](visual-studio-2013-web-tools/_static/image7.png "Aggiorna browser collegati")

    *Aggiorna browser collegati*

    ![Internet Explorer - pagina aggiornata con quattro pulsanti](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - pagina aggiornata con quattro pulsanti")

    *Internet Explorer - pagina aggiornata con quattro pulsanti*

    ![Google Chrome - pagina aggiornata con quattro pulsanti](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - pagina aggiornata con quattro pulsanti")

    *Google Chrome - pagina aggiornata con quattro pulsanti*
6. Tornare a Visual Studio.
7. Si sono aggiunti i pulsanti alla pagina, ma è necessario aggiungere un modello di domanda. A tale scopo, si userà una nuova funzionalità in Web Essentials chiamato **generatore di Lorem Ipsum**. Individuare il **div** elemento con la **classe** attributo **front**.
8. Aggiungere il codice seguente come primo elemento figlio del **div**, quindi premere **scheda**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. Il codice deve essere espanso in formato HTML.

    ![Generato automaticamente di Lorem Ipsum](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum generato automaticamente")

    *Lorem Ipsum generato automaticamente*

    > [!NOTE]
    > Come parte della scrittura di codice Zen, è ora possibile generare codice Lorem Ipsum direttamente nell'editor HTML. È sufficiente digitare **lorem** e fare clic su **scheda** e un 30 word Lorem Ipsum verrà inserito il testo. Ad esempio, *lorem10* inserisce 10 parole Lorem Ipsum.
10. Si aggiungerà un logo nella parte superiore della domanda con un'altra nuova funzionalità in Web Essentials chiamato **generatore di Lorem Pixel**. Aggiungere il codice seguente come primo elemento figlio del **div** elemento con **contenitore** come **classe** valore, quindi premere **scheda**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. Il codice deve espandere in formato HTML.

    ![Generato automaticamente lorem Pixel](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel generato automaticamente")

    *Lorem Pixel generato automaticamente*

    > [!NOTE]
    > Come parte della scrittura di codice Zen, è anche possibile generare codice Lorem Pixel direttamente nell'editor HTML. È sufficiente digitare **pix-200x200-animali** e fare clic su **della scheda** e un **img** verrà inserito il tag con un'immagine di 200x200 dell'animale. Per altre informazioni, consultare [Lorem Pixel](http://www.lorempixel.com).
12. Scegliere il **Aggiorna browser collegati** pulsante per aggiornare entrambi i browser.

    ![Internet Explorer - generato automaticamente immagini e testo](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - generato automaticamente immagini e testo")

    *Internet Explorer - generato automaticamente immagini e testo*

    ![Google Chrome - generato automaticamente immagini e testo](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - generato automaticamente immagini e testo")

    *Google Chrome - generato automaticamente immagini e testo*

    > [!NOTE]
    > L'immagine viene selezionato casualmente quando si aggiunge il frammento di codice, l'immagine visualizzata nel browser può differire.
13. Non chiudere il browser. Verranno usati nell'attività successiva.

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>Attività 3: aggiornamento di una proprietà di stile

In questa attività si userà il collegamento Browser **ispezionare modalità** funzionalità per rilevare la posizione esatta in cui viene generato l'elemento DOM specifico e quindi aggiornare la proprietà color di quell'elemento con un selettore di colore fornito dal Web Essentials.

1. Nel browser Internet Explorer, premere **CTRL** + **ALT** + **ho** per abilitare la modalità di ispezionare.
2. Spostare il puntatore sul bordo blu chiaro e fare clic su.

    ![Spostare il puntatore del mouse sul bordo blu chiaro](visual-studio-2013-web-tools/_static/image14.png "spostando il puntatore sul bordo blu chiaro")

    *Spostare il puntatore del mouse sul bordo blu chiaro*
3. Tornare a Visual Studio. Si noti come l'elemento HTML selezionato nel browser viene anche selezionato nell'editor HTML di Visual Studio.

    ![Elemento HTML selezionato nell'editor di Visual Studio HTML](visual-studio-2013-web-tools/_static/image15.png "elemento HTML selezionato nell'editor HTML di Visual Studio")

    *Elemento HTML selezionato nell'editor HTML di Visual Studio*
4. A questo punto si aggiornerà il **front** classe CSS per modificare lo stile dell'elemento selezionato. A tale scopo, premere **CTRL** + **,** per aprire la **passa a** casella di ricerca. Tipo di **CSS** , quindi premere **invio** per aprire il file.

    ![Apertura file Site. CSS](visual-studio-2013-web-tools/_static/image16.png "apertura del file CSS")

    *Apertura del file CSS*
5. Premere **CTRL** + **F** e il tipo **.flip containter .front** per trovare il selettore CSS.
6. Fare clic sul quadrato blu chiaro nella proprietà del bordo della classe per aprire la selezione colori.

    ![Aprire la selezione colori](visual-studio-2013-web-tools/_static/image17.png "aprendo la selezione colori")

    *Aprire la selezione colori*
7. Espandere la selezione colori facendo clic sul pulsante freccia di espansione e selezionare un nuovo colore.

    ![Espandere la selezione colori](visual-studio-2013-web-tools/_static/image18.png "espandendo la selezione colori")

    *Espandere la selezione colori*
8. Premere **CTRL** + **ALT** + **invio** per aggiornare i browser collegati.
9. Passare a Internet Explorer e notare come è stato modificato il colore del bordo.

    ![Internet Explorer - colore del bordo aggiornato](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - colore del bordo aggiornato")

    *Internet Explorer - colore del bordo aggiornato*
10. Passare a Google Chrome e notare come è stato modificato il colore del bordo.

    ![Google Chrome: colore del bordo aggiornato](visual-studio-2013-web-tools/_static/image20.png "Google Chrome: colore del bordo aggiornato")

    *Google Chrome: colore del bordo aggiornato*
11. Tornare a Visual Studio.
12. Passare alla fine della **CSS** file e premere **CTRL** + **F** per individuare il **.btn** selettore.
13. Si noti che il **- webkit-border-radius** proprietà sottolineata in verde.

    ![proprietà - webkit-border-radius del selettore btn](visual-studio-2013-web-tools/_static/image21.png "proprietà - webkit-border-radius del selettore btn")

    *proprietà - webkit-border-radius del selettore btn*
14. Posizionare il cursore nel **- webkit-border-radius** proprietà. Una linea blu verrà visualizzato sotto la prima lettera della parola prima della proprietà. Questo è il **smart tag**.
15. Premere **CTRL** + **.** Aprire il menu di suggerimenti e fare clic su **manca una proprietà standard (raggio bordo) Aggiungi**.

    ![Componente mancante suggerimento di proprietà standard](visual-studio-2013-web-tools/_static/image22.png "componente mancante suggerimento di proprietà standard")

    *Aggiungi mancanti suggerimento di proprietà standard*
16. Il **border-radius** proprietà viene aggiunta automaticamente alla regola CSS.

    ![Manca una proprietà standard aggiunta](visual-studio-2013-web-tools/_static/image23.png "manca una proprietà standard aggiunta")

    *Manca una proprietà standard aggiunta*
17. Spostare il puntatore sul **border-radius** proprietà per visualizzare i **Browser matrice tooltip**. Il **Browser matrice tooltip** Mostra la disponibilità della proprietà in ogni browser.

    ![Browser matrice tooltip](visual-studio-2013-web-tools/_static/image24.png "Browser matrice tooltip")

    *Descrizione comando matrice browser*
18. Si noti che il valore della **border-radius** proprietà è ancora sottolineato. Spostare il puntatore sul valore per visualizzare il messaggio di avviso.

    ![Avviso di valore di proprietà Border-radius](visual-studio-2013-web-tools/_static/image25.png "avviso valore di proprietà Border-radius")

    *Avviso di valore di proprietà Border-radius*
19. Rimuovere l'unità dei **border-radius** valore della proprietà come indicato dalla descrizione comando.
20. Come **border-radius** è la proprietà standard per la definizione di bordo arrotondato come sono angoli, è possibile rimuovere il **- webkit-border-radius** proprietà e il valore in base alla regola CSS.
21. Posizionare il cursore nel **ritorno a capo automatico** proprietà e si noti che lo smart tag viene visualizzato anche sotto.
22. Aprire il menu di scelta e fare clic su **aggiungere le specifiche di fornitore mancanti**.

    ![Aggiungere mancante fornitore specifiche suggerimento](visual-studio-2013-web-tools/_static/image26.png "aggiungere mancante fornitore specifiche suggerimento")

    *Aggiungere mancante fornitore specifiche suggerimento*
23. Il **-ms-ritorno a capo automatico** proprietà viene aggiunta automaticamente alla regola CSS.

    ![Aggiunta la proprietà specifica fornitore](visual-studio-2013-web-tools/_static/image27.png "proprietà specifico fornitore aggiunta")

    *Proprietà specifico fornitore aggiunta*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>Attività 4: aggiornamento del codice HTML dal Browser

In questa attività si userà il collegamento Browser **modalità di progettazione** questa funzionalità per modificare l'oggetto DOM dal browser e trasferire le modifiche al file di origine HTML in Visual Studio.

1. In Google Chrome, premere **CTRL** + **ALT** + **1!d** per abilitare la modalità di progettazione.
2. Spostare il puntatore sul **Lorem Ipsum dolor sit amet** assegnare un'etichetta e fare clic su.

    ![Modifica la domanda](visual-studio-2013-web-tools/_static/image28.png "modifica la domanda")

    *Modifica la domanda*
3. Dovrebbe essere visualizzato un cursore. Sostituire il testo originale con *ciò che simile quando scrive una domanda più lungo?*, quindi premere **ESC** per uscire dalla modalità progettazione.

    ![Domanda modificato](visual-studio-2013-web-tools/_static/image29.png "domanda modificato")

    *Domanda modificato*
4. Commutatore torna a Visual Studio e aprire **index. cshtml**, se non è già aperto. Si noti che il testo interno del **&lt;p&gt;** elemento è stato aggiornato.

    ![Domanda aggiornate nella pagina HTML](visual-studio-2013-web-tools/_static/image30.png "domanda aggiornate nella pagina HTML")

    *Domanda aggiornato nella pagina HTML*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>Attività 5: revisione SEO avvisi correlati

**Ottimizzazione motore di ricerca** (SEO) è il processo di creazione di una classificazione di sito Web successiva nell'elenco dei motori di ricerca di risultati. Maggiore sarà il sito classifica e in modo più coerente è elencato, i visitatori ulteriori otterrà il sito da tale motore di ricerca. Web Essentials include uno strumento di analisi che esamina l'HTML, i report i problemi trovati e offre assistenza per risolverli.

1. Passare al **visualizzazione** dal menu **elenco errori** per aprire il **elenco errori** finestra.

    ![Errore nella visualizzazione elenco dal menu](visual-studio-2013-web-tools/_static/image31.png "elenco errori nel menu Visualizza")

    *Menu elenco nella visualizzazione errore*
2. Si noti che vi sia un avviso di SEO che informa che un **&lt;meta&gt;** tag per la descrizione della pagina è manca. Fare doppio clic la voce di avviso SEO per risolvere il problema.

    ![Finestra Elenco errori](visual-studio-2013-web-tools/_static/image32.png "finestra Elenco errori")

    *Finestra Elenco errori*
3. Nel **Web Essentials** della finestra di dialogo fare clic su **Yes** inserire una descrizione &lt;meta&gt; tag.

    ![Finestra di dialogo Web Essentials](visual-studio-2013-web-tools/_static/image33.png "nella finestra di dialogo Web Essentials")

    *Finestra di dialogo Web Essentials*
4. L'editor per  **\_layout. cshtml** apre e il **&lt;meta&gt;** tag viene aggiunto automaticamente al **head** sezione del File HTML.

    ![Tag Meta aggiunti automaticamente nella pagina layout](visual-studio-2013-web-tools/_static/image34.png "Meta tag aggiunti automaticamente nella pagina layout")

    *Tag Meta aggiunto automaticamente a \_pagina Layout*
5. Modificare il valore della **contenuti** dell'attributo *GeekQuiz* e salvare il file.

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>Esercizio 2: Sfruttando i vantaggi di IntelliSense e frammenti di codice

Web Essentials, l'editor HTML è stato esteso con funzionalità aggiuntive. In questo esercizio, si noterà alcune nuove funzionalità che sono utili durante lo sviluppo di applicazioni web.

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>Attività 1: uso di IntelliSense nei documenti HTML

La prima nuova funzionalità verrà visualizzato in questa attività viene chiamata **IntelliSense dinamica**. IntelliSense dinamica legge altri tag e attributi di dedurre gli ID possibili che si userà.

In questa attività si creerà un nuovo elemento di form HTML che contiene un'etichetta e un campo di input. Aggiungerà un **per** attributo per l'etichetta per associarlo all'input, si noterà i suggerimenti di IntelliSense basati agli ID degli input nell'ambito.

1. Aprire **Visual Studio Express 2013 per il Web** e il **Begin.sln** soluzione che si trova nel **inizio/origine/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense** cartella. In alternativa, è possibile continuare con la soluzione ottenuta nell'esercizio precedente.
2. Nelle **Esplora soluzioni**, aprire il **index. cshtml** file che si trova nel **viste** | **Home** cartella.
3. Aggiungere il seguente formato all'interno di **&lt;sezione&gt;** elemento.

    (Code - Snippet *VisualStudio2013WebTooling* - *Ex2* - *Form*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. Il tag di input deve essere preceduto da un'etichetta con una descrizione del campo. Aggiungere l'etichetta seguente prima del tag di input.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. Il **per** attributo di un **&lt;etichetta&gt;** specifica quale elemento del form un'etichetta è associato. Il valore dell'attributo deve essere uguale all'id dell'elemento correlato. Aggiungere il **per** attributo il **&lt;etichetta&gt;** elemento. Come illustrato nella figura seguente, il &quot;name&quot; valore viene visualizzata nella finestra di IntelliSense, in base all'id degli elementi all'interno dell'ambito stesso (che li racchiude  **&lt;form&gt;**).

    ![Che mostra l'id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "che mostra l'id in IntelliSense")

    *Che mostra l'id in IntelliSense*
6. Eliminare le aggiunte di recente **&lt;form&gt;** elemento e il relativo contenuto.

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>Attività 2 - con frammenti di codice HTML

HTML5 introdotte più di 25 nuovi tag semantici. Visual Studio ha già il supporto IntelliSense per questi tag, ma Visual Studio 2013 rendono più veloce e più semplici da scrivere markup mediante l'aggiunta di nuovi frammenti di codice. Anche se questi tag non sono complicati, presentano alcune sottigliezze piccole, ad esempio aggiungendo i fallback codec corretto per il *audio* tag. In questa attività, verranno visualizzati i frammenti di codice HTML per i tag audio.

1. Nel **index. cshtml** file, digitare  **&lt;aud** all'interno di **&lt;sezione&gt;** elemento, come illustrato nella figura seguente.

    ![Inserimento di un elemento audio](visual-studio-2013-web-tools/_static/image36.png "inserimento di un elemento audio")

    *Inserimento di un elemento audio*
2. Premere **della scheda** due volte e notare come il codice seguente viene aggiunta nella pagina e il cursore viene posizionato sulle **src** attributo della prima origine.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > Premendo il **scheda** chiave due volte, viene inserito il frammento di codice. Il frammento di audio Mostra l'utilizzo standard delle *audio* tag, con due file di origine per il supporto migliorato.
3. Eliminare la seconda riga e aggiornare l'origine della prima riga con il collegamento seguente per la presentazione WebCampsTV Katana: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3). Seguito è riportato il codice risultante.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > Il file di origine *KatanaProject.mp3* viene usato come esempio. Se si preferisce, è possibile usare un'altra origine.
4. Premere **CTRL** + **S** per salvare il file.
5. Premere **CTRL** + **F5** per avviare l'applicazione.
6. Si noti che l'applicazione è stato aggiunto un lettore audio.

    ![Lettore audio in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "lettore Audio in Internet Explorer")

    *Lettore audio in Internet Explorer*

    ![Lettore audio in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "lettore Audio in Google Chrome")

    *Lettore audio in Google Chrome*
7. Non chiudere il browser. Verranno usati nell'attività successiva.

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>Attività 3: uso di IntelliSense in JavaScript documenti

Web Essentials 2013, i fogli di stile e pagine HTML producono un elenco di ID e i nomi di classe. In questa attività si apprenderà come tali elenchi migliorano il supporto IntelliSense per JavaScript in Web Essentials 2013.

1. Nel **index. cshtml** , aggiungere il codice seguente per definire una **script** tag per il codice JavaScript.

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. Aggiungere il codice seguente all'interno di **script** tag per definire la funzione di callback pronto.

    (Code - Snippet *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. Posizionare il cursore nel **script** tag e premere **CTRL** + **.** Per aprire il menu di suggerimento.
4. Fare clic su **estrarre in File**.

    ![Estrarre suggerimento file JavaScript](visual-studio-2013-web-tools/_static/image39.png "JavaScript Estrai nel suggerimento di file")

    *JavaScript Estrai nel suggerimento di file*
5. Nel **Salva con nome** finestra, seleziona la **script** cartella, nome del file **init.js** e fare clic su **Salva**.

    ![Salva con nome finestra](visual-studio-2013-web-tools/_static/image40.png "finestra Salva con nome")

    *Salva con nome finestra*

    > [!NOTE]
    > Il **init.js** file viene creato e viene spostato il contenuto dello script nel file.
    > 
    > ![File Init.js creato con il contenuto incluso](visual-studio-2013-web-tools/_static/image41.png "Init.js file creato con il contenuto disponibile")
    > 
    > *File Init.js creato con il contenuto disponibile*
6. Aprire il **index. cshtml** del file e verificare che il tag di script è stato sostituito con un riferimento al **init.js** file.

    ![Riferimento html Init.js](visual-studio-2013-web-tools/_static/image42.png "riferimento html Init.js")

    *Riferimento html Init.js*
7. Andare alla **Esplora soluzioni** e notare che le **init.js** file è stato incluso automaticamente nella soluzione.

    ![File Init.js inclusi nella soluzione](visual-studio-2013-web-tools/_static/image43.png "Init.js file inclusi nella soluzione")

    *File Init.js inclusi nella soluzione*
8. Tornare al **init.js** file per aggiornare il **pronto** funzione di callback.
9. All'interno della definizione di funzione callback che viene passata a *pronti*, aggiungere il codice seguente per ottenere tutti gli elementi da un attributo di classe specifica.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. Premere **CTRL** + **spazio** tra le virgolette all'interno di **getElementsByClassName** chiamata di funzione.

    ![Visualizzazione di IntelliSense per la funzione getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "IntelliSense che mostra per la funzione getElementsByClassName")

    *Visualizzazione di IntelliSense per la funzione getElementsByClassName*

    > [!NOTE]
    > Si noti che IntelliSense mostra le classi definite nei fogli di stile progetto.
11. Sostituire la riga che è stato creato con il codice seguente.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. Posizionare il cursore dopo **au** all'interno delle virgolette nel **getElementsByTagName** (funzione) e premere **CTRL** + **spazio**.

    ![Visualizzazione di IntelliSense per il metodo getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "IntelliSense che mostra il metodo getElementByTagName")

    *Visualizzazione di IntelliSense per il metodo getElementsByTagName*
13. Selezionare **&quot;audio&quot;** dall'elenco, quindi premere **invio**. Il risultato è illustrato nella figura seguente.

    ![Recupero di elementi Audio](visual-studio-2013-web-tools/_static/image46.png "il recupero degli elementi Audio")

    *Recupero di elementi Audio*
14. In **Esplora soluzioni**, fare doppio clic sul **init.js** del file nei **script** cartella e selezionare **file JavaScript Minify** dal **Web Essentials** menu.

    ![I file JavaScript di minimizzare](visual-studio-2013-web-tools/_static/image47.png "file Minify JavaScript")

    *Minimizzare i file JavaScript*
15. Quando viene richiesto di abilitare la riduzione automatica quando le modifiche ai file di origine fare clic su **Sì**.

    ![Abilitazione di avviso automatico minimizzazione](visual-studio-2013-web-tools/_static/image48.png "abilitazione avviso automatico minimizzazione")

    *Abilitazione di avviso di minimizzazione automatica*

    > [!NOTE]
    > Il **init.min.js** viene creato e viene aggiunto come dipendenza per il **init.js** file.
    > 
    > ![File Init.min.js creato](visual-studio-2013-web-tools/_static/image49.png "file Init.min.js creato")
    > 
    > *File Init.min.js creato*
16. Aprire il **init.min.js** file e notare che il file è minimizzato.

    ![Contenuto del file Init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js contenuto del file")

    *Contenuto del file Init.min.js*
17. Nel **init.js** , aggiungere il codice seguente sotto il **getElementsByTagName** per riprodurre tutti gli elementi audio chiamata di funzione.

    (Code - Snippet *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. Fare clic su **CTRL** + **S** per salvare il file. Poiché il file minimizzato è già aperto, si verrà visualizzata una finestra di dialogo che informa che il file è stato modificato all'esterno dell'editor di origine. Scegliere **Sì**.

    ![Avviso di Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "avviso di Microsoft Visual Studio")

    *Avviso di Microsoft Visual Studio*
19. Tornare alla **init.min.js** file per verificare che il file è stato aggiornato con il nuovo codice.

    ![Il file Init.min.js aggiornato](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file aggiornato")

    *File Init.min.js aggiornato*
20. Scegliere il **aggiornamento del collegamento Browser** pulsante.
21. Dopo che entrambi i browser vengono aggiornati i lettori audio che si è visto nell'attività precedente verranno avviata la riproduzione automatica.

    ![Lettore audio incluso nella visualizzazione](visual-studio-2013-web-tools/_static/image53.png "lettore Audio incluso nella visualizzazione")

    *Lettore audio incluso nella visualizzazione*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa esercitazione pratica si è appreso come:

- Usare nuove funzionalità dell'editor HTML incluse in Web Essentials, ad esempio i frammenti di codice HTML5 avanzati e codifica Zen
- Usare nuove funzionalità dell'editor CSS incluse in Web Essentials, ad esempio la selezione dei colori e una descrizione comando di matrice di Browser
- Usare nuove funzionalità dell'editor JavaScript inclusa in Web Essentials, ad esempio l'estrazione di File e IntelliSense per tutti gli elementi HTML
- Scambiare i dati tra browser e Visual Studio usando il collegamento del Browser
