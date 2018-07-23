---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: Utilizzo di controllo pagina in Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: In questo laboratorio pratico, si noterà un nuovo strumento per individuare e correggere problemi correlati alle pagine web in Visual Studio - controllo pagina. Page Inspector è un nuovo strumento che b...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: ac945a23dc6ef060340320d047f13c8e81057138
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833672"
---
<a name="using-page-inspector-in-visual-studio-2012"></a>Utilizzo di controllo pagina in Visual Studio 2012
====================
da [Camp Web Team](https://twitter.com/webcamps)

> In questo laboratorio pratico, si noterà un nuovo strumento per individuare e correggere problemi correlati alle pagine web in Visual Studio - controllo pagina.
> 
> Page Inspector è un nuovo strumento che offre strumenti di diagnostica del browser a Visual Studio e offre un'esperienza integrata tra il browser, ASP.NET e codice sorgente. Esegue il rendering di una pagina web (HTML, Web Form, ASP.NET MVC o Web Pages) direttamente nell'IDE di Visual Studio e ti permette di esaminare il codice sorgente sia l'output risultante. Page Inspector consente di scomporre facilmente un sito Web, creare rapidamente le pagine da zero e diagnosticare rapidamente i problemi.
> 
> Oggi abbiamo un numero di Framework Web che creano siti Web flessibili e scalabili in modo tempestivo, ad esempio MVC ASP.NET e Web Form. D'altra parte, raggiunge più difficile individuare i problemi nelle pagine perché l'IDE non supporta la visualizzazione di progettazione nelle pagine basate su modelli e il contenuto dinamico. Di conseguenza, questi siti Web devono essere aperte in un browser per verificare come vengono visualizzate a un utente.
> 
> Gli sviluppatori Web utilizzano strumenti esterni per trovare i problemi che eseguito regolarmente nel browser. Vengono quindi tornare all'IDE e iniziare a risolvere. Questo e viceversa attività tra l'IDE, il browser e gli strumenti di profilatura può risultare inefficiente e talvolta richiede una nuova distribuzione e della cache ogni volta che si vuole riprodurre un problema di pulizia.
> 
> Page Inspector colma un divario nello sviluppo Web tra il client (strumenti browser) e il server (ASP.NET e codice sorgente) inglobando il meglio di entrambi i mondi usando un set combinato di funzionalità.
> 
> Con Page Inspector è possibile visualizzare quali elementi nei file di origine (incluso il codice lato server) hanno prodotto il markup HTML per eseguire il rendering nel browser. Page Inspector consente inoltre di modificare le proprietà CSS e attributi dell'elemento DOM per visualizzare le modifiche apportate immediatamente nel browser.
> 
> Questo laboratorio pratico verrà illustrano le funzionalità di controllo pagina e illustrano come è possibile usarli per risolvere i problemi nelle applicazioni Web. **Questa esercitazione contiene due esercizi utilizzando flussi simile ma destinate a tecnologie diverse. Se sei uno sviluppatore di ASP.NET MVC, seguono esercizio uno. Se si è un esercizio di seguire per gli sviluppatori Web Form due**.
> 
> Questa esercitazione illustra i miglioramenti e nuove funzionalità descritte in precedenza applicando modifiche minime a un'applicazione Web di esempio fornita nella cartella di origine.
> 
> Tutto il codice di esempio e frammenti di codice sono inclusi nel Web Camp Kit di formazione, disponibile all'indirizzo [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico, si apprenderà come:

- Scomporre un sito Web utilizzando controllo pagina
- Controllare e visualizzare in anteprima le modifiche degli stili CSS con Page Inspector
- Rilevare e risolvere i problemi nelle pagine web utilizzo di controllo pagina

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Sono necessari gli elementi seguenti per completare questa esercitazione:

- [Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (leggere [appendice A](#AppendixA) per istruzioni su come installarlo).
- Internet Explorer 9 o versione successiva

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questo laboratorio pratico include gli esercizi seguenti:

1. [Esercizio 1: Uso di controllo pagina in progetti ASP.NET MVC](#Exercise1)
2. [Esercizio 2: Uso di controllo pagina nei progetti Web Form](#Exercise2)

> [!NOTE]
> Ogni esercizio è accompagnata da una soluzione inizia, che si trova nella cartella Begin dell'esercizio, che consente di seguire ogni esercizio indipendentemente dagli altri. All'interno del codice sorgente per un esercizio, si noterà anche una cartella finale contenente una soluzione di Visual Studio con il codice che scaturisce da completare i passaggi nell'esercizio corrispondente. È possibile usare queste soluzioni come prassi consigliata se ti serve assistenza aggiuntiva durante l'esecuzione in questo laboratorio pratico.


Tempo stimato per completare questa esercitazione: **30 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>Esercizio 1: Uso di controllo pagina in progetti ASP.NET MVC

In questo esercizio, si apprenderà come visualizzare in anteprima e il debug di un' **ASP.NET MVC 4** soluzione usando **Page Inspector**. In primo luogo, si eseguirà una breve panoramica dello strumento per informazioni sulle funzionalità che semplificano il processo di debug Web. Quindi, si utilizzeranno in una pagina web che contiene i problemi di stile. Si apprenderà come utilizzare controllo pagina per trovare il codice sorgente che genera il problema e risolverlo.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Attività 1: esplorare controllo pagina

In questa attività si apprenderà come usare il controllo pagina nel contesto di un progetto ASP.NET MVC 4 che mostra una raccolta di foto.

1. Aprire il **Begin** soluzione disponibile all'indirizzo **inizio/origine/Ex1-MVC4/** cartella.

   1. È necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. In Esplora soluzioni, individuare **index. cshtml** visualizzare sotto il **/visualizzazioni/Home** cartella del progetto, pulsante destro del mouse e selezionare **Visualizza in controllo pagina**.

    ![Selezione di un file per visualizzare in anteprima in controllo pagina](using-page-inspector-in-visual-studio-2012/_static/image1.png "selezionando un file da visualizzare in anteprima in controllo pagina")

    *Selezione di un file per visualizzare in anteprima in controllo pagina*
3. Nella finestra di controllo pagina verrà visualizzato il */Home/Index* URL è mappato per la visualizzazione selezionata di origine.

    ![ThefirstcontactwithPageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *Il primo contatto con Page Inspector*

    Lo strumento di controllo pagina è integrato nell'ambiente Visual Studio. Il controllo contiene un browser incorporato, insieme a un potente profiler HTML. Si noti che non è necessario eseguire la soluzione per visualizzare l'aspetto delle pagine.

    > [!NOTE]
    > Quando la larghezza del browser di controllo pagina è minore rispetto alla larghezza della pagina aperta, non vedrete la pagina in modo corretto. In tal caso, è possibile regolare la larghezza del controllo pagina.
4. Scegliere il **file** scheda in controllo pagina.

    Si noterà che tutti i file di origine che siano scrivendo la pagina di indice. Questa funzionalità consente di identificare tutti gli elementi in modo immediato, in particolare quando si lavora con le visualizzazioni parziali e i modelli. Si noti che è anche possibile aprire ogni file se si fa clic sui collegamenti.

    ![Scheda file](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *La scheda file*
5. Scegliere il **modalità controllo Attiva/disattiva** pulsante, che si trova a sinistra delle schede.

    Questo strumento consente di selezionare qualsiasi elemento della pagina e visualizzare il codice HTML e Razor.

    ![Pulsante Mostra/Nascondi-controllo-modalità](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *Pulsante modalità controllo Attiva/disattiva*
6. Nel browser di controllo pagina, spostare il puntatore del mouse sugli elementi della pagina. Mentre si sposta il puntatore del mouse su un punto qualsiasi della pagina sottoposta a rendering, viene visualizzato il tipo di elemento e il corrispondente codice o markup di origine viene evidenziato nell'editor di Visual Studio.

    ![Inspectionmodeinaction](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *Modalità di controllo in azione*

    > [!NOTE]
    > Non per ingrandire la finestra di controllo pagina oppure non sarà in grado di visualizzare la scheda di anteprima che mostra il codice sorgente. Se si sceglie l'elemento in controllo pagina quando viene ingrandito, verrà visualizzato il codice sorgente della selezione ma nasconde la finestra di controllo pagina.

    Se si presta attenzione per il **index. cshtml** file, si noterà che la parte del codice sorgente che genera l'elemento selezionato viene evidenziata. Questa funzionalità semplifica la modifica dei file di origine lungo, offrendo un modo rapido e diretto per accedere al codice.

    ![Inspectingelements](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *Esaminare gli elementi*
7. Scegliere il **attiva/disattiva modalità di controllo** pulsante (![selezionare la scheda HTML per visualizzare il codice HTML sottoposto a rendering nel browser di controllo pagina.] (using-page-inspector-in-visual-studio-2012/_static/image7.png "Selezionare la scheda HTML per visualizzare il codice HTML sottoposto a rendering nel browser di controllo pagina.") ) per disabilitare il cursore.
8. Selezionare il **HTML** scheda per visualizzare il codice HTML sottoposto a rendering nel browser di controllo pagina.
9. Nel markup HTML, individuare l'elemento di elenco con collegamento Koala e selezionarlo.

    Si noti che quando si seleziona il codice, l'output corrispondente viene automaticamente evidenziato nel browser. Questa funzionalità è utile per visualizzare la modalità di rendering di un blocco di codice HTML della pagina.

    ![Se si seleziona elemento HTML nella pagina](using-page-inspector-in-visual-studio-2012/_static/image8.png "elemento di selezione HTML nella pagina")

    *Selezionare l'elemento HTML nella pagina*
10. Scegliere il **modalità controllo Attiva/disattiva** pulsante per abilitare *modalità controllo* e fare clic sulla barra di spostamento. A destra del codice HTML, nel riquadro stili, verrà visualizzato un elenco con gli stili CSS applicati all'elemento selezionato.

    > [!NOTE]
    > Poiché l'intestazione è una parte del layout del sito, controllo pagina apre \_file di layout. cshtml ed evidenziare il segmento di codice interessato.

    ![Discoveringstyles](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *L'individuazione degli stili e i file di origine di un elemento selezionato*
11. Con il puntatore di controllo Attiva/Disattiva abilitato, spostare il puntatore del mouse sotto la barra blu in primo piano e fare clic sul cerchio metà.

    ![Selezione di un elemento](using-page-inspector-in-visual-studio-2012/_static/image10.png "selezionando un elemento")

    *Selezione di un elemento*
12. Nel riquadro stili, individuare il **immagine di sfondo** elemento sotto il **.main-content** gruppo. **Deselezionare l'opzione** il **immagine di sfondo** e vedere cosa succede. Si noterà che il browser rifletteranno le modifiche immediatamente e il cerchio è nascosto.

    > [!NOTE]
    > Le modifiche applicate nella scheda stili di controllo pagina non influenzano il foglio di stile originale. È possibile deselezionare gli stili o modificare i relativi valori come tutte le volte che si desidera, ma queste verranno ripristinate dopo aver aggiornato la pagina.

    ![Abilitazione e disabilitazione di stili CSS](using-page-inspector-in-visual-studio-2012/_static/image11.png "abilitazione e disabilitazione di stili CSS")

    *Abilitazione e disabilitazione di stili CSS*
13. A questo punto, fare clic su di '**inserire qui il logo**' testo nell'intestazione usando la modalità di controllo.
14. Nel **stili** , individuare il **font-size** CSS attributo sotto il **.site-titolo** gruppo. Fare doppio clic sul valore dell'attributo e sostituire il valore di 2.3 em con **3 em**, quindi premere **invio**. Si noti che il titolo è più grande.

    ![Modificare i valori CSS in controllo pagina](using-page-inspector-in-visual-studio-2012/_static/image12.png "valori modifica CSS in controllo pagina")

    *Modificare i valori CSS in controllo pagina*
15. Scegliere il **stili traccia** scheda, che si trova nel riquadro di destra del controllo pagina. Questo è un modo alternativo per visualizzare tutti gli stili applicati alla selezione, ordinati in base al nome dell'attributo.

    ![CSSstylestracing](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *Traccia di stili CSS dell'elemento selezionato*
16. Un'altra funzionalità di controllo pagina è il riquadro di Layout. Usa la modalità di controllo, selezionare la barra di spostamento, quindi scegliere il **Layout** scheda nel riquadro destro. Si noterà la dimensione esatta dell'elemento selezionato, nonché le dimensioni di offset, dei margini, spaziatura interna e bordo. Si noti che è anche possibile modificare i valori da questa visualizzazione.

    ![Layout degli elementi in controllo pagina](using-page-inspector-in-visual-studio-2012/_static/image14.png "layout degli elementi in controllo pagina")

    *Layout degli elementi in controllo pagina*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Attività 2: individuare e correggere i problemi di stile nella raccolta di foto

Come si sarebbero diagnosticare i problemi di pagine Web con le versioni precedenti di Visual Studio? Sono probabilmente familiari con gli strumenti che vengono eseguiti all'esterno di IDE di Visual Studio, ad esempio strumenti per sviluppatori di Internet Explorer o Firebug di debug web. Browser comprendere solo HTML, stili, mentre un framework sottostante genera il codice HTML che verranno sottoposti a rendering e scripting. Per questo motivo, è spesso necessario distribuire l'intero sito per visualizzare l'aspetto delle pagine web, ad esempio.

Probabile che aveva eseguito questi passaggi quando si vuole rilevare e risolvere un problema nel sito web:

1. Eseguire la soluzione da Visual Studio o distribuire la pagina sul server web.
2. Nel browser, aprire gli strumenti di sviluppo si usano, o semplicemente aprire il codice sorgente e gli stili e tentano di trovare il problema. Per trovare i file interessati, è necessario utilizzare il &quot;ricerca&quot; oppure &quot;ricerca nei file&quot; funzionalità con il nome delle classi di stile.
3. Quando viene rilevato l'errore, arrestare il Web browser e il server.
4. Cancellare la cache del browser.
5. Tornare a Visual Studio per applicare una correzione. Ripetere tutti i passaggi per eseguire il test.

Poiché non esiste alcun reale WYSIWYG in ASP.NET MVC 4, la maggior parte dei problemi di stile vengono rilevata in una fase successiva, dopo l'esecuzione o distribuzione dell'applicazione web. A questo punto, con controllo pagina, è possibile visualizzare l'anteprima di qualsiasi pagina senza eseguire la soluzione.

In questa attività verrà utilizzare controllo pagina e correggere alcuni problemi applicazione Raccolta foto.

1. Utilizzo di controllo pagina, individuare il **registrare** e il **Accedi** collegamenti sul lato sinistro dell'intestazione.

    Si noti che i collegamenti non vengono visualizzati nella posizione prevista a destra e vengono visualizzati come un elenco puntato. Verrà ora allineare i collegamenti a destra e modificare lo stile li di conseguenza.

    ![Individuazione del registratore di cassa e del Log nei collegamenti](using-page-inspector-in-visual-studio-2012/_static/image15.png "individuazione del registratore di cassa e del Log nei collegamenti")

    *Individuazione del registratore di cassa e del Log nei collegamenti*
2. Con modalità di controllo Attiva/Disattiva selezionato, fare clic su Chiudi per, ma non sul collegamento Registra per aprire il relativo codice.

    Si noti che il codice sorgente dei collegamenti è disponibile nel  **\_loginpartial. cshtml** del file, non index. cshtml né la \_layout. cshtml, quali sono le posizioni potrebbe apparire in primo luogo. Sono stati posizionati direttamente nel file di origine corretta.
3. Nel **stili** scheda, individuare e fare clic il **<section> #login</section>** elemento, che è il contenitore HTML per i collegamenti seguenti.

    Si noti che il **#login** stile di visualizzazione si trova automaticamente nella **CSS** dopo aver fatto clic. Inoltre, ora è evidenziato il codice.

    ![Selezionando gli stili CSS](using-page-inspector-in-visual-studio-2012/_static/image16.png "selezionando gli stili CSS")

    *Selezionando gli stili CSS*
4. Rimuovere il commento il **text-align** dell'attributo nel codice evidenziato rimuovendo i caratteri di apertura e chiusura e salvare il **CSS** file.

    Page Inspector è compatibile con tutti i file che costituiscono la pagina corrente e può individuare se uno di questi file viene modificato. L'utente viene avvisato ogni volta che la pagina corrente nel browser non è sincronizzata con i file di origine.
5. Nel browser di controllo pagina, fare clic sulla barra che si trova sotto la barra degli indirizzi per ricaricare la pagina.

    ![Ricaricare la pagina](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *Ricaricare la pagina*

    I collegamenti sono ora a destra, ma vengono ancora visualizzate come un elenco puntato. A questo punto, si rimuoverà gli elenchi puntati e allineare i collegamenti in senso orizzontale.

    ![Pagina aggiornata](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *Pagina aggiornata*
6. Usando la modalità di controllo, selezionare una qualsiasi delle **&lt;li&gt;** gli elementi che contengono il &quot;registrare&quot; e &quot;Accedi&quot; collegamenti. Scegliere il  **&lt;sezione&gt; #login** elemento per l'accesso **Styles. CSS** codice.

    ![Ricerca dello stile](using-page-inspector-in-visual-studio-2012/_static/image19.png "ricerca dello stile")

    *Ricerca dello stile*
7. Nelle **Style. CSS**, rimuovere il commento il codice per **li #login** elementi. Lo stile che si sta aggiungendo verrà al punto di nascondere e visualizzare gli elementi orizzontalmente.

    ![Modifica dello stile di collegamenti di accesso](using-page-inspector-in-visual-studio-2012/_static/image20.png "modifica dello stile di collegamenti di accesso")

    *Modifica dello stile di collegamenti di accesso*
8. Salvare **Style. CSS** file e fare clic su una sola volta nella barra dei che si trova sotto l'indirizzo a ricaricare la pagina. Si noti che i collegamenti vengano visualizzati correttamente.

    ![I collegamenti allineato al lato destro](using-page-inspector-in-visual-studio-2012/_static/image21.png "collegamenti allineato a destra")

    *Collegamenti allineati a destra*
9. Infine, si modificherà il titolo dell'intestazione. Usare la modalità di controllo per fare clic su **inserire qui il logo** testo e ottenere il codice sorgente che lo genera.
10. A questo punto è nella  **\_layout. cshtml**, sostituire '**inserire qui il logo**'text con'**raccolta foto**'. Salvare e aggiornare il browser di controllo pagina.

    ![Assegnazione di un nuovo titolo](using-page-inspector-in-visual-studio-2012/_static/image22.png "assegnazione di un nuovo titolo")

    *Assegnazione di un nuovo titolo*

    ![PhotoGallerypage](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *Pagina Raccolta foto aggiornato*
11. Infine, selezionare la **PhotoGallery** progetti e premere **F5** per eseguire l'app. Scopri tutte le modifiche funzionino come previsto.

* * *

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>Esercizio 2: Uso di controllo pagina nei progetti Web Form

In questo esercizio, si apprenderà come visualizzare in anteprima ed eseguire il debug di una soluzione di Web Form tramite controllo pagina. Prima di tutto si eseguirà una breve panoramica dello strumento per apprendere le funzioni di controllo pagina che facilitano il processo di debug Web. Quindi, si utilizzeranno in una pagina web che contiene i problemi di stile. Si apprenderà come utilizzare controllo pagina per trovare il codice sorgente che genera il problema e risolverlo.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Attività 1: esplorare controllo pagina

In questa attività si apprenderà come usare le funzionalità di controllo pagina nel contesto di un progetto di Web Form che mostra una raccolta di foto.

1. Aprire il **Begin** soluzione disponibile all'indirizzo **inizio/origine/Ex2-WebForms/** cartella.

   1. È necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. In Esplora soluzioni, individuare **default. aspx** pagina, pulsante destro del mouse e selezionare **Visualizza in controllo pagina**.

    ![Aprire Default. aspx con Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image24.png "aprire default. aspx con controllo pagina")

    *Default. aspx apertura con controllo pagina*
3. Nella finestra di controllo pagina verrà visualizzato default. aspx.

    ![Visualizzazione di default. aspx in controllo pagina](using-page-inspector-in-visual-studio-2012/_static/image25.png "visualizzazione default. aspx in controllo pagina")

    *Visualizzazione di default. aspx in controllo pagina*

    Lo strumento di controllo pagina è integrato nell'ambiente Visual Studio. Il controllo contiene un browser incorporato, insieme a un potente profiler HTML che mostra il codice selezionato. Si noti che non è necessario eseguire la soluzione per visualizzare l'aspetto delle pagine.

    > [!NOTE]
    > Quando la larghezza del browser di controllo pagina è minore rispetto alla larghezza della pagina aperta, non vedrete la pagina in modo corretto. In tal caso, è possibile regolare la larghezza del controllo pagina.
4. Scegliere il **file** scheda in controllo pagina.

    Si noterà che tutti i file di origine che siano scrivendo la pagina predefinita viene eseguito il rendering. Questa è una funzionalità utile per identificare tutti gli elementi in modo immediato, in particolare quando si lavora con i controlli utente e le pagine Master. Si noti che è anche possibile passare a ogni file.

    ![Scheda dei file](using-page-inspector-in-visual-studio-2012/_static/image26.png "The file (scheda)")

    *La scheda file*
5. Scegliere il **modalità controllo Attiva/disattiva** pulsante, che si trova a sinistra delle schede.

    Questo strumento consente di selezionare qualsiasi elemento della pagina e visualizzare la relativa origine. aspx e codice HTML.

    ![Pulsante modalità controllo Attiva/disattiva](using-page-inspector-in-visual-studio-2012/_static/image27.png "pulsante Attiva/Disattiva modalità di controllo")

    *Pulsante modalità controllo Attiva/disattiva*
6. Nel browser di controllo pagina, spostare il puntatore del mouse sugli elementi della pagina. Mentre si sposta il puntatore del mouse su un punto qualsiasi della pagina sottoposta a rendering, viene visualizzato il tipo di elemento e il corrispondente codice o markup di origine viene evidenziato nell'editor di Visual Studio.

    ![Modalità di controllo in azione](using-page-inspector-in-visual-studio-2012/_static/image28.png "modalità di controllo in azione")

    *Modalità di controllo in azione*

    > [!NOTE]
    > Non per ingrandire la finestra di controllo pagina oppure non sarà in grado di visualizzare la scheda di anteprima che mostra il codice sorgente. Se si sceglie l'elemento in controllo pagina quando viene ingrandito, verrà visualizzato il codice sorgente della selezione ma nasconde la finestra di controllo pagina.

    Se si presta attenzione al **default. aspx** file, si noterà che la parte del codice sorgente che genera l'elemento selezionato viene evidenziata. Questa funzionalità semplifica l'edizione dei file di origine lungo, offrendo un modo rapido e diretto per accedere al codice.

    ![Esaminare gli elementi](using-page-inspector-in-visual-studio-2012/_static/image29.png "esaminare gli elementi")

    *Esaminare gli elementi*
7. Scegliere il **attiva/disattiva modalità di controllo** pulsante (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.](using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.") ), che si trova nelle schede di controllo pagina, per disabilitare il cursore.
8. Selezionare il **HTML** scheda per visualizzare il codice HTML sottoposto a rendering nel browser di controllo pagina.
9. Nel codice HTML, individuare l'elemento di elenco con collegamento Koala e selezionarlo.

    Si noti che quando si seleziona il codice, l'output corrispondente viene automaticamente evidenziato browser. Questa funzionalità è utile per visualizzare la modalità di rendering di un blocco di codice HTML della pagina.

    ![Selezione di un elemento HTML nella pagina](using-page-inspector-in-visual-studio-2012/_static/image31.png "selezionando un elemento HTML nella pagina")

    *Selezione di un elemento HTML nella pagina*
10. Scegliere il **modalità controllo Attiva/disattiva** pulsante per abilitare *modalità controllo* e fare clic sulla barra di spostamento. A destra del codice HTML, nel riquadro stili, verrà visualizzato un elenco con gli stili CSS applicati all'elemento selezionato.

    > [!NOTE]
    > Poiché l'intestazione è una parte del layout del sito, controllo pagina verrà anche aprire il file Site. master ed evidenziare il segmento di codice interessata.

    ![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "l'individuazione degli stili e i file di origine di un elemento selezionato")

    *L'individuazione degli stili e i file di origine di un elemento selezionato*
11. Con il puntatore di controllo Attiva/Disattiva abilitato, spostare il puntatore del mouse sotto la barra dei menu e fare clic sul cerchio metà vuoto.

    ![Selezione di un elemento](using-page-inspector-in-visual-studio-2012/_static/image33.png "selezionando un elemento")

    *Selezione di un elemento*
12. Nel riquadro stili, individuare il **immagine di sfondo** elemento sotto il **.main-content** gruppo. **Deselezionare l'opzione** il **immagine di sfondo** e vedere cosa succede. Si noterà che il browser rifletteranno le modifiche immediatamente e il cerchio è nascosto.

    > [!NOTE]
    > Le modifiche applicate nella scheda stili di controllo pagina non influenzano il foglio di stile originale. È possibile deselezionare gli stili o modificare i relativi valori come tutte le volte che si desidera, ma queste verranno ripristinate dopo aver aggiornato la pagina.

    ![Abilitazione e disabilitazione di CSS styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "abilitazione e disabilitazione di stili CSS")

    *Abilitazione e disabilitazione di stili CSS*
13. A questo punto, fare clic su di '**le** **qui il logo'** testo nell'intestazione usando la modalità di controllo.
14. Nel **stili** , individuare il **font-size** CSS attributo sotto il **.site-titolo** gruppo. Fare doppio clic su una sola volta l'attributo per modificare il relativo valore. Sostituire il 2.3em il valore con **3em**, quindi premere INVIO. Si noti che il titolo è più grande.

    ![Modificare i valori CSS nel Page Inspector2](using-page-inspector-in-visual-studio-2012/_static/image35.png "valori modifica CSS in controllo pagina")

    *Modificare i valori CSS in controllo pagina*
15. Scegliere il **stili traccia** scheda, che si trova nel riquadro di destra del controllo pagina. Questo è un modo alternativo per visualizzare tutti gli stili applicati alla selezione, ordinati in base al nome dell'attributo.

    ![Traccia di stili CSS dell'elemento selezionato](using-page-inspector-in-visual-studio-2012/_static/image36.png "traccia degli stili CSS dell'elemento selezionato")

    *Traccia di stili CSS dell'elemento selezionato*
16. Un'altra funzionalità di controllo pagina è il riquadro di Layout. Usa la modalità di controllo, selezionare la barra di spostamento, quindi scegliere il **Layout** scheda nel riquadro destro. Si noterà la dimensione esatta dell'elemento selezionato, nonché le dimensioni di offset, dei margini, spaziatura interna e bordo. Si noti che è anche possibile modificare i valori da questa visualizzazione.

    ![Layout degli elementi in controllo pagina](using-page-inspector-in-visual-studio-2012/_static/image37.png "layout degli elementi in controllo pagina")

    *Layout degli elementi in controllo pagina*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Attività 2: individuare e correggere i problemi di stile nella raccolta di foto

Come si sarebbero diagnosticare i problemi di pagine Web con le versioni precedenti di Visual Studio? Sono probabilmente familiari con gli strumenti che vengono eseguiti all'esterno di IDE di Visual Studio, ad esempio strumenti per sviluppatori di Internet Explorer o Firebug di debug web. Browser comprendere solo HTML, stili, mentre un framework sottostante genera il codice HTML che verranno sottoposti a rendering e scripting. Per questo motivo, è spesso necessario distribuire l'intero sito per visualizzare l'aspetto delle pagine web, ad esempio.

Probabile che aveva eseguito questi passaggi quando si vuole rilevare e risolvere un problema nel sito web:

1. Eseguire la soluzione da Visual Studio o distribuire la pagina sul server web.
2. Nel browser, aprire gli strumenti di sviluppo si usano, o semplicemente aprire il codice sorgente e gli stili e tentano di trovare il problema. Per trovare i file interessati, è necessario utilizzare il &quot;ricerca&quot; oppure &quot;ricerca nei file&quot; funzionalità con il nome delle classi di stile.
3. Quando viene rilevato l'errore, arrestare il Web browser e il server.
4. Cancellare la cache del browser.
5. Tornare a Visual Studio per applicare una correzione. Ripetere tutti i passaggi per eseguire il test.

Come è no real WYSIWYG in Web Form ASP.NET, alcuni problemi di stile vengono rilevati in una fase successiva, dopo l'esecuzione o la distribuzione. A questo punto, con controllo pagina, è possibile visualizzare l'anteprima di qualsiasi pagina senza eseguire la soluzione.

In questa attività si userà il controllo pagina per risolvere alcuni problemi dell'applicazione Raccolta foto. Nei passaggi seguenti, si rileverà e correggere rapidamente un problema semplice applicazione di stili nell'intestazione.

1. In controllo pagina, individuare il **registrare** e il **Accedi** collegamenti sul lato sinistro dell'intestazione.

    Si noti che il collegamento non è visualizzato nella posizione prevista a destra. Verrà ora allineare il collegamento a destra e modificare di conseguenza lo stile.

    ![Accedi al collegamento posizionato a sinistra](using-page-inspector-in-visual-studio-2012/_static/image38.png "Accedi collegamento posizionato a sinistra")

    *Collegamento Accedi posizionato a sinistra*
2. Attiva/Disattiva modalità di controllo selezionata, selezionare il collegamento Accedi per aprire il relativo codice.

    Si noti che il codice di origine di collegamento è disponibile nel **Site. master** file, non nella pagina default. aspx che è il posto potrebbe apparire in primo luogo; inserita direttamente nel file di origine corretta.
3. Nel **stili** scheda, individuare e fare clic il  **&lt;sezione&gt; #login** elemento, che è il contenitore HTML per i collegamenti seguenti.

    Si noti che il **#login** stile di visualizzazione si trova automaticamente nella **CSS** dopo aver fatto clic. Inoltre, ora è evidenziato il codice.

    ![Selezionando gli stili CSS](using-page-inspector-in-visual-studio-2012/_static/image39.png "selezionando gli stili CSS")

    *Selezionando gli stili CSS*
4. Rimuovere il commento il **text-align** dell'attributo nel codice evidenziato rimuovendo i caratteri di apertura e chiusura e salvare il **CSS** file.

    Page Inspector è compatibile con tutti i file che costituiscono la pagina corrente e può individuare se uno di questi file viene modificato. L'utente viene avvisato ogni volta che la pagina corrente nel browser non è sincronizzata con i file di origine.
5. Nel browser di controllo pagina, fare clic sulla barra che si trova sotto la barra degli indirizzi per salvare le modifiche e ricaricare la pagina.

    ![Reloadingthepage](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *Ricaricare la pagina*

    I collegamenti sono ora a destra, ma vengono ancora visualizzate come un elenco puntato. A questo punto, si rimuoverà gli elenchi puntati e allineare i collegamenti in senso orizzontale.

    ![Pagina aggiornata](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *Pagina aggiornata*
6. Usando la modalità di controllo, selezionare una qualsiasi delle **&lt;li&gt;** gli elementi che contengono il &quot;registrare&quot; e &quot;Accedi&quot; collegamenti. Scegliere il  **&lt;sezione&gt; #login** elemento per l'accesso **Styles. CSS** codice.

    ![Ricerca dello stile](using-page-inspector-in-visual-studio-2012/_static/image42.png "ricerca dello stile")

    *Ricerca dello stile*
7. Nelle **Style. CSS**, rimuovere il commento il codice per **li #login** elementi. Lo stile che si sta aggiungendo verrà al punto di nascondere e visualizzare gli elementi orizzontalmente.

    ![Modifica dello stile di collegamenti di accesso](using-page-inspector-in-visual-studio-2012/_static/image43.png "modifica dello stile di collegamenti di accesso")

    *Modifica dello stile di collegamenti di accesso*
8. Salvare **Style. CSS** file e fare clic su una sola volta nella barra dei che si trova sotto l'indirizzo a ricaricare la pagina. Si noti che i collegamenti vengano visualizzati correttamente.

    ![I collegamenti allineato al lato destro](using-page-inspector-in-visual-studio-2012/_static/image44.png "collegamenti allineato a destra")

    *Collegamenti allineati a destra*
9. Infine, si modificherà il titolo dell'intestazione. Invece di cercare di '**inserire qui il logo'** testo in tutti i file, usare la modalità di controllo per fare clic sul testo e ottenere al codice sorgente che lo genera.

    ![Trovare il titolo del sito](using-page-inspector-in-visual-studio-2012/_static/image45.png "ricerca il titolo del sito")

    *Trovare il titolo del sito*
10. A questo punto è nella **Site. master**, sostituire il '**inserire qui il logo**'text con'**raccolta foto**'. Salvare e aggiornare il browser di controllo pagina.

    ![Pagina Raccolta foto aggiornata](using-page-inspector-in-visual-studio-2012/_static/image46.png "pagina Raccolta foto aggiornato")

    *Pagina Raccolta foto aggiornato*
11. Infine, selezionare **F5** per eseguire l'app di check-out di tutte le modifiche funzionino come previsto.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa pratica, si è appresa come utilizzare controllo pagina per visualizzare l'anteprima dell'applicazione Web senza dover ricompilare ed eseguire il sito Web in un browser. Inoltre, si è appresa come trovare e correggere i bug accedendo direttamente dall'output del rendering del codice sorgente rapidamente.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice a: installazione di Visual Studio Express 2012 per Web

È possibile installare **Microsoft Visual Studio Express 2012 per Web** o da un'altra &quot;Express&quot; versione utilizzando il **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web Microsoft*.

1. Passare a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e cercare il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **Installa ora**. Se non hai **instalace Webové Platformy** si verrà reindirizzati per scaricarlo e installarlo prima di tutto.
3. Una volta **instalace Webové Platformy** è aperto, fare clic su **installare** per avviare il programma di installazione.

    ![Installa Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "installa Visual Studio Express")

    *Installa Visual Studio Express*
4. Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.

    ![Accettare le condizioni di licenza](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *Accettare le condizioni di licenza*
5. Attendere finché non viene completato il processo di download e l'installazione.

    ![Stato dell'installazione](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *Installazione completata*
7. Fare clic su **Exit** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per Web, fare clic per il **avviare** schermata e iniziare la scrittura &quot; **Visual Studio Express**&quot;, quindi fare clic sul **Visual Studio Express per Web** il riquadro.

    ![Visual Studio Express per il riquadro Web](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *Visual Studio Express per il riquadro Web*
