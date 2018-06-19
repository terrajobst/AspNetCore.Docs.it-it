---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: Con Page Inspector in Visual Studio 2012 | Documenti Microsoft
author: rick-anderson
description: In questo laboratorio pratico, sarà possibile osservare un nuovo strumento per individuare e correggere i problemi correlati alle pagine web in Visual Studio - controllo pagina. Controllo pagina è un nuovo strumento che b...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 052d29dba170d403c2b1c1667c55fc2c34045615
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2018
ms.locfileid: "30891243"
---
<a name="using-page-inspector-in-visual-studio-2012"></a>Con Page Inspector in Visual Studio 2012
====================
Da [categorie Web Team](https://twitter.com/webcamps)

> In questo laboratorio pratico, sarà possibile osservare un nuovo strumento per individuare e correggere i problemi correlati alle pagine web in Visual Studio - controllo pagina.
> 
> Controllo pagina è un nuovo strumento che fornisce gli strumenti di diagnostica del browser a Visual Studio e offre un'esperienza integrata tra il browser, ASP.NET e codice sorgente. Esegue il rendering di una pagina web (HTML, Web Form, ASP.NET MVC o Web Pages) direttamente all'interno di IDE di Visual Studio e consente di esaminare il codice sorgente sia l'output risultante. Controllo pagina consente di scomporre facilmente un sito Web, creare rapidamente pagine da zero e diagnosticare velocemente i problemi.
> 
> Oggi abbiamo un numero di Framework Web che creano siti Web flessibile e scalabile in modo tempestivo, ad esempio ASP.NET MVC e Web Form. D'altra parte, ottiene più difficile individuare i problemi nelle pagine perché l'IDE non supporta la visualizzazione di progettazione nelle pagine basate su modello e il contenuto dinamico. Di conseguenza, i seguenti siti Web devono essere aperte in un browser per vedere come vengono visualizzati a un utente.
> 
> Gli sviluppatori Web utilizzano strumenti esterni per individuare i problemi che eseguono regolarmente nel browser. Vengono quindi tornare all'IDE e la risoluzione. Questo avanti e indietro attività tra l'IDE, il browser e gli strumenti di profilatura può risultare inefficiente e talvolta richiede una nuova distribuzione e la cache ogni volta che si desidera riprodurre un problema di pulizia.
> 
> Page Inspector colma un divario nello sviluppo Web tra il client (strumenti browser) e il server (ASP.NET e il codice sorgente) inglobando il meglio di entrambi utilizzando un set combinato di funzionalità.
> 
> Con Page Inspector è possibile visualizzare gli elementi nei file di origine (incluso il codice lato server) che hanno prodotto il markup HTML per eseguire il rendering nel browser. Controllo pagina consente inoltre di modificare le proprietà CSS e attributi dell'elemento DOM per visualizzare le modifiche riflesse immediatamente nel browser.
> 
> Questa esercitazione pratica verrà illustrati le funzionalità di controllo pagina e mostrano come è possibile utilizzare per risolvere i problemi nelle applicazioni Web. **Questa esercitazione contiene due esercitazioni con flussi simili ma destinato a tecnologie diverse. Se si è uno sviluppatore di MVC ASP.NET, seguire esercizio. Se si è un esercizio di seguire developer WebForms due**.
> 
> Questa esercitazione illustra i miglioramenti e nuove funzionalità descritte in precedenza tramite l'applicazione di modifiche di lieve entità a un'applicazione Web di esempio fornita nella cartella di origine.
> 
> Tutto il codice di esempio e i frammenti di codice sono inclusi nel Web categorie Training Kit, disponibile all'indirizzo [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questa esercitazione pratica, si apprenderà come:

- Scomporre un sito Web con Page Inspector
- Controllare e visualizzare in anteprima le modifiche degli stili CSS con controllo pagina
- Rilevare e risolvere i problemi nelle pagine web con Page Inspector

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

È necessario che gli elementi seguenti per completare questa esercitazione:

- [Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (lettura [appendice](#AppendixA) per istruzioni su come installarlo).
- Internet Explorer 9 o versione successiva

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questa esercitazione include gli esercizi seguenti:

1. [Esercizio 1: Utilizzo di controllo pagina in progetti ASP.NET MVC](#Exercise1)
2. [Esercizio 2: Utilizzo di controllo pagina nei progetti di Web Form](#Exercise2)

> [!NOTE]
> Ogni esercizio è accompagnata da una soluzione inizia, che si trova nella cartella Begin dell'esercizio, che consente di seguire ogni esercizio indipendentemente dalle altre. All'interno del codice sorgente per un esercizio, si noterà anche una cartella di fine che contiene una soluzione di Visual Studio con il codice generato da completare i passaggi nell'esercizio corrispondente. Se è necessario ulteriore assistenza durante l'utilizzo tramite questa esercitazione pratica, è possibile usare queste soluzioni come guida.


Tempo stimato per completare questa esercitazione: **30 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>Esercizio 1: Utilizzo di controllo pagina in progetti ASP.NET MVC

In questo esercizio, si apprenderà come visualizzare in anteprima e il debug di un **ASP.NET MVC 4** soluzione usando **controllo pagina**. In primo luogo, si eseguirà una breve introduzione ad lo strumento per apprendere le funzioni che semplificano il processo di debug di Web. Quindi, verrà utilizzata in una pagina web contenente lo stile problemi. Si apprenderà come usare controllo pagina per trovare il codice sorgente che genera il problema e risolverlo.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Attività 1: esplorazione di controllo pagina

In questa attività si apprenderà come usare il controllo pagina nel contesto di un progetto ASP.NET MVC 4 che mostra una raccolta foto.

1. Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex1-MVC4/Begin/** cartella.

   1. È necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. In Esplora soluzioni, individuare **cshtml** visualizzare sotto la **/visualizzazioni/Home** cartella del progetto, pulsante destro del mouse e selezionare **Visualizza in controllo pagina**.

    ![Selezione di un file per visualizzare in anteprima in controllo pagina](using-page-inspector-in-visual-studio-2012/_static/image1.png "selezionando un file per visualizzare in anteprima in controllo pagina")

    *Selezione di un file per visualizzare in anteprima in controllo pagina*
3. Nella finestra di controllo pagina verrà visualizzato il */Home/Index* URL è mappato all'origine visualizzazione selezionata.

    ![ThefirstcontactwithPageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *Il primo contatto con controllo pagina*

    Lo strumento di controllo pagina è integrato nell'ambiente di Visual Studio. Il controllo contiene un browser incorporato, insieme a un profiler HTML potente. Si noti che non è necessario eseguire la soluzione per visualizzare l'aspetto delle pagine.

    > [!NOTE]
    > Quando la larghezza del browser di controllo pagina è inferiore alla larghezza della pagina aperto, non noterai la pagina correttamente. In tal caso, è possibile regolare la larghezza del controllo pagina.
4. Fare clic su di **file** scheda in controllo pagina.

    Si noterà che tutti i file di origine che si siano scrivendo la pagina di indice. Questa funzionalità consente di identificare tutti gli elementi a colpo d'occhio, soprattutto quando si lavora con le visualizzazioni parziali e i modelli. Si noti che è anche possibile aprire ogni file se si fa clic sui collegamenti.

    ![Scheda file](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *Scheda file*
5. Fare clic su di **modalità controllo Attiva/disattiva** pulsante, si trova a sinistra delle schede.

    Questo strumento consente di selezionare un elemento della pagina e visualizzarne il codice HTML e Razor.

    ![Pulsante modalità di controllo Toggle](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *Pulsante Attiva/Disattiva modalità controllo*
6. Nel browser di controllo pagina, spostare il puntatore del mouse sugli elementi della pagina. Mentre si sposta il puntatore del mouse su un punto qualsiasi della pagina rendering, viene visualizzato il tipo di elemento e il markup di origine corrispondente o il codice viene evidenziata nell'editor di Visual Studio.

    ![Inspectionmodeinaction](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *Modalità di controllo in azione*

    > [!NOTE]
    > Non ingrandire la finestra di controllo pagina o non sarà in grado di visualizzare la scheda di anteprima mostra il codice sorgente. Se si sceglie l'elemento in controllo pagina quando viene ingrandito, verrà visualizzato il codice sorgente della selezione, ma nasconde la finestra di controllo pagina.

    Se si presta attenzione per il **cshtml** file, si noterà che la parte del codice sorgente che genera l'elemento selezionato viene evidenziata. Questa funzionalità semplifica la modifica dei file di origine lungo, offrendo un modo rapido e diretto per accedere al codice.

    ![Inspectingelements](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *Esaminare gli elementi*
7. Fare clic sui **attiva/disattiva modalità di controllo** pulsante (![selezionare la scheda HTML per visualizzare il codice HTML eseguito il rendering nel browser di controllo pagina.] (using-page-inspector-in-visual-studio-2012/_static/image7.png "Selezionare la scheda HTML per visualizzare il codice HTML eseguito il rendering nel browser di controllo pagina.") ) per disabilitare il cursore.
8. Selezionare il **HTML** scheda per visualizzare il codice HTML eseguito il rendering nel browser di controllo pagina.
9. Nel markup HTML, individuare l'elemento di elenco con collegamento Koala e selezionarlo.

    Si noti che quando si seleziona il codice, l'output corrispondente viene automaticamente evidenziato nel browser. Questa funzionalità è utile per visualizzare la modalità di rendering di un blocco HTML della pagina.

    ![Se si seleziona elemento HTML nella pagina](using-page-inspector-in-visual-studio-2012/_static/image8.png "elemento di selezione HTML nella pagina")

    *Selezionare l'elemento HTML nella pagina*
10. Fare clic su di **modalità controllo Attiva/disattiva** pulsante per attivare *modalità controllo* e fare clic sulla barra di navigazione. A destra del codice HTML, nel riquadro stili, verrà visualizzato un elenco con gli stili CSS applicati all'elemento selezionato.

    > [!NOTE]
    > Poiché l'intestazione è una parte del layout del sito, controllo pagina verrà inoltre aperta \_file cshtml ed evidenziazione interessato nel segmento di codice.

    ![Discoveringstyles](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *L'individuazione di stili e i file di origine di un elemento selezionato*
11. Con il puntatore Attiva/Disattiva controllo abilitato, spostare il puntatore del mouse sotto la barra blu in primo piano e fare clic sul cerchio metà.

    ![Selezione di un elemento](using-page-inspector-in-visual-studio-2012/_static/image10.png "selezionando un elemento")

    *Selezione di un elemento*
12. Nel riquadro stili, individuare il **immagine di sfondo** elemento sotto il **.main contenuto** gruppo. **Deselezionare** il **immagine di sfondo** e osservare cosa accade. Si noterà che il browser rifletterà le modifiche immediatamente e il cerchio è nascosto.

    > [!NOTE]
    > Le modifiche applicate nella scheda stili di controllo pagina non influenzano il foglio di stile originale. È possibile deselezionare gli stili o modificare i relativi valori come tutte le volte che si desidera, ma essi verrà ripristinati dopo l'aggiornamento della pagina.

    ![Abilitazione e disabilitazione di stili CSS](using-page-inspector-in-visual-studio-2012/_static/image11.png "abilitazione e disabilitazione di stili CSS")

    *Abilitazione e disabilitazione di stili CSS*
13. A questo punto, fare clic su di '**inserire qui il logo**' testo nell'intestazione usando la modalità di controllo.
14. Nel **stili** , individuare il **font-size** CSS attributo sotto il **.site titolo** gruppo. Fare doppio clic sul valore dell'attributo e sostituire il valore di 2.3 em con **3 em**, quindi premere **invio**. Si noti che il titolo più grande.

    ![Modificare i valori CSS in controllo pagina](using-page-inspector-in-visual-studio-2012/_static/image12.png "valori modifica CSS in controllo pagina")

    *Modificare i valori CSS in controllo pagina*
15. Fare clic su di **stili traccia** scheda nel riquadro a destra del controllo pagina. Questo è un modo alternativo per vedere tutti gli stili applicati alla selezione, ordinati in base al nome dell'attributo.

    ![CSSstylestracing](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *Traccia di stili CSS dell'elemento selezionato*
16. Un'altra funzionalità di controllo pagina è il riquadro di Layout. Utilizzando la modalità di controllo, selezionare la barra di spostamento e quindi fare clic su di **Layout** scheda nel riquadro destro. Verrà visualizzata la dimensione esatta dell'elemento selezionato, nonché le dimensioni di offset, margini, spaziatura interna e bordo. Si noti che è inoltre possibile modificare i valori da questa visualizzazione.

    ![Layout dell'elemento in controllo pagina](using-page-inspector-in-visual-studio-2012/_static/image14.png "layout degli elementi in controllo pagina")

    *Layout dell'elemento in controllo pagina*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Attività 2: ricerca e risoluzione dei problemi di stile della raccolta foto

Modalità di sarebbe diagnosi dei problemi di pagine Web con le versioni precedenti di Visual Studio? Si è probabilmente con strumenti di debug che vengono eseguite all'esterno di IDE di Visual Studio, ad esempio strumenti di sviluppo di Internet Explorer o Firebug web. Browser comprendere solo HTML, script e gli stili, mentre un framework sottostante genera il codice HTML che verranno sottoposti a rendering. Per questo motivo, è spesso necessario distribuire l'intero sito per vedere come pagine web.

Probabile che ha eseguito questi passaggi quando si desidera rilevare e risolvere un problema nel sito web:

1. Eseguire la soluzione da Visual Studio o distribuire la pagina nel server web.
2. Nel browser, aprire Strumenti di sviluppo, si utilizza, o semplicemente aprire il codice sorgente e gli stili e tenterà di associare il problema. Per trovare i file interessati, è necessario utilizzare il &quot;ricerca&quot; o &quot;ricerca nei file&quot; funzionalità con il nome delle classi di stile.
3. Dopo aver rilevato l'errore, arrestare il Web browser e il server.
4. Cancellare la cache del browser.
5. Tornare a Visual Studio per applicare una correzione. Ripetere tutti i passaggi per eseguire il test.

Poiché non esiste alcun WYSIWYG reali in ASP.NET MVC 4, la maggior parte dei problemi di stile vengono rilevata in una fase successiva, dopo l'esecuzione o di distribuzione dell'applicazione web. A questo punto, con controllo pagina, è possibile visualizzare l'anteprima di qualsiasi pagina senza eseguire la soluzione.

In questa attività verrà utilizzato il controllo pagina ed risolvere alcuni problemi dell'applicazione Raccolta foto.

1. Con Page Inspector, individuare il **registrare** e **Accedi** collegamenti sul lato sinistro dell'intestazione.

    Si noti che i collegamenti non vengono visualizzati nella posizione prevista a destra e vengono visualizzati come un elenco puntato. Verrà ora allineare i collegamenti a destra e modificare lo stile in conseguenza.

    ![Individuazione del registro e del Log nei collegamenti](using-page-inspector-in-visual-studio-2012/_static/image15.png "individuazione il registro e Log nei collegamenti")

    *Individuazione del registro e del Log nei collegamenti*
2. Con modalità di controllo Toggle selezionato, fare clic su Chiudi per, ma non su, il collegamento di registro per aprire il relativo codice.

    Si noti che il codice sorgente dei collegamenti si trova nella  **\_LoginPartial.cshtml** file, non il cshtml né la \_cshtml, che sono le posizioni in cui è possibile esaminare in primo luogo. Sono stati inseriti direttamente nel file di origine corretti.
3. Nel **stili** individuare e fare clic su di **<section> #login</section>** elemento, che corrisponde al contenitore HTML per questi collegamenti.

    Si noti che il **#login** stile viene automaticamente posizionato **Site** dopo aver fatto clic. Inoltre, il codice viene evidenziato.

    ![Selezionare gli stili CSS](using-page-inspector-in-visual-studio-2012/_static/image16.png "selezionando gli stili CSS")

    *Selezionare gli stili CSS*
4. Rimuovere il commento di **text-align** attributo nel codice evidenziato rimuovendo i caratteri di apertura e chiusura e salvare il **Site** file.

    Controllo pagina è a conoscenza di tutti i file che costituiscono la pagina corrente e, è possibile rilevare quando uno di questi file vengono modificati. L'utente viene avvisato ogni volta che la pagina nel browser corrente non è sincronizzata con i file di origine.
5. Nel browser di controllo pagina, fare clic sulla barra che si trova sotto la barra degli indirizzi a ricaricare la pagina.

    ![Ricaricare la pagina](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *Ricaricare la pagina*

    I collegamenti sono ora a destra, ma vengano ancora visualizzate come un elenco puntato. Verrà ora, rimuovere i punti elenco e allineare i collegamenti in senso orizzontale.

    ![Pagina aggiornata](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *Pagina aggiornata*
6. Usando la modalità di controllo, selezionare una qualsiasi del **&lt;li&gt;** gli elementi che contengono il &quot;registrare&quot; e &quot;Accedi&quot; collegamenti. Quindi, fare clic su di  **&lt;sezione&gt; #login** elemento accesso **Styles. CSS** codice.

    ![Individuare lo stile](using-page-inspector-in-visual-studio-2012/_static/image19.png "lo stile di ricerca")

    *Lo stile di ricerca*
7. In **Style.css**, rimuovere il commento il codice per **#login li** elementi. Lo stile che si sta aggiungendo verrà al punto di nascondere e visualizzare gli elementi orizzontalmente.

    ![Modifica dello stile i collegamenti di accesso](using-page-inspector-in-visual-studio-2012/_static/image20.png "modifica dello stile i collegamenti di accesso")

    *Modifica dello stile i collegamenti di accesso*
8. Salvare **Style.css** file e fare clic su una sola volta sulla barra di cui si trova sotto l'indirizzo a ricaricare la pagina. Si noti che i collegamenti vengono visualizzati correttamente.

    ![Collegamenti allineato al lato destro](using-page-inspector-in-visual-studio-2012/_static/image21.png "collegamenti allineato a destra")

    *Collegamenti allineati a destra*
9. Infine, si modificherà il titolo di intestazione. Utilizzare la modalità di controllo fare clic su **inserire qui il logo** testo e ottenere il codice sorgente che genera.
10. È ora  **\_cshtml**, sostituire "**inserire qui il logo**'text con'**raccolta foto**'. Salvare e aggiornare il browser di controllo pagina.

    ![Assegnazione di un nuovo titolo](using-page-inspector-in-visual-studio-2012/_static/image22.png "assegnazione di un nuovo titolo")

    *Assegnazione di un nuovo titolo*

    ![PhotoGallerypage](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *Pagina Raccolta foto di aggiornamento*
11. Infine, selezionare il **PhotoGallery** progetto e premere **F5** per eseguire l'app. Controllare tutte le modifiche funzionano come previsto.

* * *

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>Esercizio 2: Utilizzo di controllo pagina nei progetti di Web Form

In questo esercizio, si apprenderà come visualizzare in anteprima ed eseguire il debug di una soluzione di Web Form con Page Inspector. Eseguire innanzitutto una breve introduzione ad lo strumento per apprendere le funzioni di controllo pagina che semplificano il processo di debug di Web. Quindi, verrà utilizzata in una pagina web contenente lo stile problemi. Si apprenderà come usare controllo pagina per trovare il codice sorgente che genera il problema e risolverlo.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Attività 1: esplorazione di controllo pagina

In questa attività si apprenderà come usare le funzionalità di controllo pagina nel contesto di un progetto di Web Form contenente una raccolta foto.

1. Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex2-WebForms/Begin/** cartella.

   1. È necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
2. In Esplora soluzioni, individuare **Default.aspx** pagina, pulsante destro del mouse e selezionare **Visualizza in controllo pagina**.

    ![Aprire Default. aspx con Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image24.png "aprire default. aspx con controllo pagina")

    *Apertura default. aspx con controllo pagina*
3. Nella finestra di controllo pagina verrà visualizzato Default.aspx.

    ![Visualizzazione di default. aspx in controllo pagina](using-page-inspector-in-visual-studio-2012/_static/image25.png "la visualizzazione di default. aspx in controllo pagina")

    *Visualizzazione di default. aspx in controllo pagina*

    Lo strumento di controllo pagina è integrato nell'ambiente di Visual Studio. Il controllo contiene un browser incorporato, insieme a un profiler HTML potente che mostra il codice selezionato. Si noti che non è necessario eseguire la soluzione per visualizzare l'aspetto delle pagine.

    > [!NOTE]
    > Quando la larghezza del browser di controllo pagina è inferiore alla larghezza della pagina aperto, non noterai la pagina correttamente. In tal caso, è possibile regolare la larghezza del controllo pagina.
4. Fare clic su di **file** scheda in controllo pagina.

    Si noterà che tutti i file di origine che si siano scrivendo la pagina predefinita. Questa è una funzionalità utile per identificare tutti gli elementi a colpo d'occhio, soprattutto quando si lavora con pagine Master e di controlli utente. Si noti che è anche possibile passare a ogni file.

    ![Scheda file](using-page-inspector-in-visual-studio-2012/_static/image26.png "il file (scheda)")

    *Scheda file*
5. Fare clic su di **modalità controllo Attiva/disattiva** pulsante, si trova a sinistra delle schede.

    Questo strumento consente di selezionare un elemento della pagina e vedere la relativa origine HTML, codice e aspx.

    ![Pulsante modalità controllo Mostra/Nascondi](using-page-inspector-in-visual-studio-2012/_static/image27.png "pulsante Attiva/Disattiva modalità di controllo")

    *Pulsante Attiva/Disattiva modalità controllo*
6. Nel browser di controllo pagina, spostare il puntatore del mouse sugli elementi della pagina. Mentre si sposta il puntatore del mouse su un punto qualsiasi della pagina rendering, viene visualizzato il tipo di elemento e il markup di origine corrispondente o il codice viene evidenziata nell'editor di Visual Studio.

    ![Modalità di controllo in azione](using-page-inspector-in-visual-studio-2012/_static/image28.png "modalità di controllo in azione")

    *Modalità di controllo in azione*

    > [!NOTE]
    > Non ingrandire la finestra di controllo pagina o non sarà in grado di visualizzare la scheda di anteprima mostra il codice sorgente. Se si sceglie l'elemento in controllo pagina quando viene ingrandito, verrà visualizzato il codice sorgente della selezione, ma nasconde la finestra di controllo pagina.

    Se si presta attenzione a **Default.aspx** file, si noterà che la parte del codice sorgente che genera l'elemento selezionato viene evidenziata. Questa funzionalità semplifica l'edizione dei file di origine lungo, offrendo un modo rapido e diretto per accedere al codice.

    ![Esaminare gli elementi](using-page-inspector-in-visual-studio-2012/_static/image29.png "controllando gli elementi")

    *Esaminare gli elementi*
7. Fare clic sui **attiva/disattiva modalità di controllo** pulsante (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.] (using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.") ), si trova nelle schede di controllo pagina, per disabilitare il cursore.
8. Selezionare il **HTML** scheda per visualizzare il codice HTML eseguito il rendering nel browser di controllo pagina.
9. Nel codice HTML, individuare l'elemento di elenco con collegamento Koala e selezionarlo.

    Si noti che quando si seleziona il codice, l'output corrispondente viene automaticamente evidenziato browser. Questa funzionalità è utile per visualizzare la modalità di rendering di un blocco HTML della pagina.

    ![Selezionare un elemento HTML nella pagina](using-page-inspector-in-visual-studio-2012/_static/image31.png "selezionando un elemento HTML nella pagina")

    *Selezionare un elemento HTML nella pagina*
10. Fare clic su di **modalità controllo Attiva/disattiva** pulsante per attivare *modalità controllo* e fare clic sulla barra di navigazione. A destra del codice HTML, nel riquadro stili, verrà visualizzato un elenco con gli stili CSS applicati all'elemento selezionato.

    > [!NOTE]
    > Poiché l'intestazione è una parte del layout del sito, controllo pagina verrà inoltre aprire file Site. master ed evidenziare il segmento di codice interessati.

    ![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "l'individuazione di stili e i file di origine di un elemento selezionato")

    *L'individuazione di stili e i file di origine di un elemento selezionato*
11. Con il puntatore Attiva/Disattiva controllo abilitato, spostare il puntatore del mouse sotto la barra dei menu e fare clic sul cerchio metà vuoto.

    ![Selezione di un elemento](using-page-inspector-in-visual-studio-2012/_static/image33.png "selezionando un elemento")

    *Selezione di un elemento*
12. Nel riquadro stili, individuare il **immagine di sfondo** elemento sotto il **.main contenuto** gruppo. **Deselezionare** il **immagine di sfondo** e osservare cosa accade. Si noterà che il browser rifletterà le modifiche immediatamente e il cerchio è nascosto.

    > [!NOTE]
    > Le modifiche applicate nella scheda stili di controllo pagina non influenzano il foglio di stile originale. È possibile deselezionare gli stili o modificare i relativi valori come tutte le volte che si desidera, ma essi verrà ripristinati dopo l'aggiornamento della pagina.

    ![Abilitazione e disabilitazione di CSS styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "abilitazione e disabilitazione di stili CSS")

    *Abilitazione e disabilitazione di stili CSS*
13. A questo punto, fare clic su di '**il** **qui il logo'** testo nell'intestazione usando la modalità di controllo.
14. Nel **stili** , individuare il **font-size** CSS attributo sotto il **.site titolo** gruppo. Fare doppio clic su una sola volta l'attributo per modificare il relativo valore. Il valore di sostituzione di 2.3em con **3em**, quindi premere INVIO. Si noti che il titolo più grande.

    ![Modificare i valori CSS nel Page Inspector2](using-page-inspector-in-visual-studio-2012/_static/image35.png "valori modifica CSS in controllo pagina")

    *Modificare i valori CSS in controllo pagina*
15. Fare clic su di **stili traccia** scheda nel riquadro a destra del controllo pagina. Questo è un modo alternativo per vedere tutti gli stili applicati alla selezione, ordinati in base al nome dell'attributo.

    ![La traccia di stili CSS dell'elemento selezionato](using-page-inspector-in-visual-studio-2012/_static/image36.png "la traccia di stili CSS dell'elemento selezionato")

    *Traccia di stili CSS dell'elemento selezionato*
16. Un'altra funzionalità di controllo pagina è il riquadro di Layout. Utilizzando la modalità di controllo, selezionare la barra di spostamento e quindi fare clic su di **Layout** scheda nel riquadro destro. Verrà visualizzata la dimensione esatta dell'elemento selezionato, nonché le dimensioni di offset, margini, spaziatura interna e bordo. Si noti che è inoltre possibile modificare i valori da questa visualizzazione.

    ![Layout dell'elemento in controllo pagina](using-page-inspector-in-visual-studio-2012/_static/image37.png "layout degli elementi in controllo pagina")

    *Layout dell'elemento in controllo pagina*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Attività 2: ricerca e risoluzione dei problemi di stile della raccolta foto

Modalità di sarebbe diagnosi dei problemi di pagine Web con le versioni precedenti di Visual Studio? Si è probabilmente con strumenti di debug che vengono eseguite all'esterno di IDE di Visual Studio, ad esempio strumenti di sviluppo di Internet Explorer o Firebug web. Browser comprendere solo HTML, script e gli stili, mentre un framework sottostante genera il codice HTML che verranno sottoposti a rendering. Per questo motivo, è spesso necessario distribuire l'intero sito per vedere come pagine web.

Probabile che ha eseguito questi passaggi quando si desidera rilevare e risolvere un problema nel sito web:

1. Eseguire la soluzione da Visual Studio o distribuire la pagina nel server web.
2. Nel browser, aprire Strumenti di sviluppo, si utilizza, o semplicemente aprire il codice sorgente e gli stili e tenterà di associare il problema. Per trovare i file interessati, è necessario utilizzare il &quot;ricerca&quot; o &quot;ricerca nei file&quot; funzionalità con il nome delle classi di stile.
3. Dopo aver rilevato l'errore, arrestare il Web browser e il server.
4. Cancellare la cache del browser.
5. Tornare a Visual Studio per applicare una correzione. Ripetere tutti i passaggi per eseguire il test.

Quando non è reale WYSIWYG in Web Form ASP.NET, vengono rilevati alcuni problemi di stile in una fase successiva, dopo l'esecuzione o l'implementazione. A questo punto, con controllo pagina, è possibile visualizzare l'anteprima di qualsiasi pagina senza eseguire la soluzione.

In questa attività, si utilizzerà il controllo pagina per risolvere alcuni problemi dell'applicazione Raccolta foto. Nei passaggi seguenti, verrà rilevato e risolvere rapidamente un problema di stile semplice nell'intestazione.

1. In controllo pagina, individuare il **registrare** e **Accedi** collegamenti sul lato sinistro dell'intestazione.

    Si noti che il collegamento non è visualizzato nella posizione prevista sulla destra. Verrà ora allineare il collegamento a destra e modificare lo stile, di conseguenza.

    ![Accedi al collegamento posizionato sulla sinistra](using-page-inspector-in-visual-studio-2012/_static/image38.png "Accedi al collegamento posizionata a sinistra")

    *Collegamento Accedi posizionata a sinistra*
2. Con modalità di controllo Toggle selezionato, selezionare il collegamento di accesso per aprire il relativo codice.

    Si noti che il codice sorgente di collegamento si trova nella **Site. master** file, non nella pagina Default.aspx che è il luogo potrebbe essere in primo luogo, vengono inseriti direttamente nel file di origine corretti.
3. Nel **stili** individuare e fare clic su di  **&lt;sezione&gt; #login** elemento, che corrisponde al contenitore HTML per questi collegamenti.

    Si noti che il **#login** stile viene automaticamente posizionato **Site** dopo aver fatto clic. Inoltre, il codice viene evidenziato.

    ![Selezionare gli stili CSS](using-page-inspector-in-visual-studio-2012/_static/image39.png "selezionando gli stili CSS")

    *Selezionare gli stili CSS*
4. Rimuovere il commento di **text-align** attributo nel codice evidenziato rimuovendo i caratteri di apertura e chiusura e salvare il **Site** file.

    Controllo pagina è a conoscenza di tutti i file che costituiscono la pagina corrente e, è possibile rilevare quando uno di questi file vengono modificati. L'utente viene avvisato ogni volta che la pagina nel browser corrente non è sincronizzata con i file di origine.
5. Nel browser di controllo pagina, fare clic sulla barra che si trova sotto la barra degli indirizzi per salvare le modifiche e ricaricare la pagina.

    ![Reloadingthepage](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *Ricaricare la pagina*

    I collegamenti sono ora a destra, ma vengano ancora visualizzate come un elenco puntato. Verrà ora, rimuovere i punti elenco e allineare i collegamenti in senso orizzontale.

    ![Pagina aggiornata](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *Pagina aggiornata*
6. Usando la modalità di controllo, selezionare una qualsiasi del **&lt;li&gt;** gli elementi che contengono il &quot;registrare&quot; e &quot;Accedi&quot; collegamenti. Quindi, fare clic su di  **&lt;sezione&gt; #login** elemento accesso **Styles. CSS** codice.

    ![Individuare lo stile](using-page-inspector-in-visual-studio-2012/_static/image42.png "lo stile di ricerca")

    *Lo stile di ricerca*
7. In **Style.css**, rimuovere il commento il codice per **#login li** elementi. Lo stile che si sta aggiungendo verrà al punto di nascondere e visualizzare gli elementi orizzontalmente.

    ![Modifica dello stile i collegamenti di accesso](using-page-inspector-in-visual-studio-2012/_static/image43.png "modifica dello stile i collegamenti di accesso")

    *Modifica dello stile i collegamenti di accesso*
8. Salvare **Style.css** file e fare clic su una sola volta sulla barra di cui si trova sotto l'indirizzo a ricaricare la pagina. Si noti che i collegamenti vengono visualizzati correttamente.

    ![Collegamenti allineato al lato destro](using-page-inspector-in-visual-studio-2012/_static/image44.png "collegamenti allineato a destra")

    *Collegamenti allineati a destra*
9. Infine, si modificherà il titolo di intestazione. Anziché dover cercare per il '**inserire qui il logo'** testo in tutti i file, utilizzare la modalità di controllo per selezionare il testo e ottenere al codice sorgente che genera.

    ![Trovare il titolo del sito](using-page-inspector-in-visual-studio-2012/_static/image45.png "ricerca il titolo del sito")

    *Trovare il titolo del sito*
10. È ora **Site. master**, sostituire il '**inserire qui il logo**'text con'**raccolta foto**'. Salvare e aggiornare il browser di controllo pagina.

    ![Pagina Raccolta foto di aggiornamento](using-page-inspector-in-visual-studio-2012/_static/image46.png "pagina Raccolta foto di aggiornamento")

    *Pagina Raccolta foto di aggiornamento*
11. Infine premere **F5** per eseguire l'app del check-out di tutte le modifiche funzionano come previsto.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Completando questa pratica, apprese come utilizza controllo pagina per visualizzare l'anteprima dell'applicazione Web senza dover ricompilare ed eseguire il sito Web in un browser. Inoltre, apprese come individuare e correggere i bug accedendo direttamente dall'output del rendering del codice sorgente rapidamente.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice a: installazione di Visual Studio Express 2012 per Web

È possibile installare **Microsoft Visual Studio Express 2012 per Web** o un altro &quot;Express&quot; versione utilizzando il **[installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web di Microsoft*.

1. Passare a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **installa**. Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.
3. Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.

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
7. Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per Web, passare al **avviare** schermata e iniziare la scrittura &quot; **VS Express**&quot;, quindi fare clic su di **Visual Studio Express per Web** il riquadro.

    ![VS Express per il riquadro Web](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *VS Express per il riquadro Web*
