---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: Creazione di una base ASP.NET 4.5 pagina Web Form in Visual Studio 2013 | Microsoft Docs
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: fda6922c0703ca442d4f1ebc5b39dabeb5ee58cd
ms.sourcegitcommit: 8bf4dff3069e62972c1b0839a93fb444e502afe7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2018
ms.locfileid: "46483022"
---
<a name="creating-a-basic-aspnet-45-web-forms-page-in-visual-studio-2013"></a>Creazione di una base ASP.NET 4.5 pagina Web Form in Visual Studio 2013
====================
da [Erik Reitan](https://github.com/Erikre)

[!INCLUDE[](~/includes/rp.md)]

Questa procedura dettagliata viene fornita un'introduzione all'ambiente di sviluppo Web in [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) e nella [Microsoft Visual Studio Express 2013 per Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Questa procedura dettagliata illustra come creare una semplice pagina Web Form ASP.NET e illustra le tecniche di base di creazione di una nuova pagina, aggiunta di controlli e la scrittura di codice.

Le attività illustrate nella procedura dettagliata sono le seguenti:

- Creazione di un progetto di applicazione Web Form di file system.
- Acquisire familiarità con Visual Studio.
- Creazione di una pagina ASP.NET.
- Aggiunta di controlli.
- Aggiunta di gestori eventi.
- In esecuzione e test di una pagina da Visual Studio.

## <a name="prerequisites"></a>Prerequisiti


Per completare questa procedura dettagliata, è necessario:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) oppure [Microsoft Visual Studio Express 2013 per Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework viene installato automaticamente. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 e Microsoft Visual Studio Express 2013 per Web verrà essere noto anche come Visual Studio in tutta questa serie di esercitazioni.  
    >   
    > Se si usa Visual Studio, questa procedura dettagliata si presume che il **lo sviluppo Web** raccolta di impostazioni la prima volta che è stato avviato Visual Studio. Per altre informazioni, vedere [procedura: selezionare impostazioni ambiente di sviluppo Web](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="creating-a-web-application-project-and-a-page"></a>Creazione di un progetto di applicazione Web e di una pagina

<a id="sectionToggle0"></a>

In questa parte della procedura dettagliata, si verrà creare un progetto di applicazione Web e aggiungere una nuova pagina a esso. È inoltre verrà aggiungere testo HTML ed eseguire la pagina nel browser.

### <a name="to-create-a-web-application-project"></a>Per creare un progetto di applicazione Web

1. Aprire Microsoft Visual Studio.
2. Nel menu **File** selezionare **Nuovo progetto**.  
    ![Menu file](creating-a-basic-web-forms-page/_static/image1.png)

    Verrà visualizzata la finestra di dialogo **Nuovo progetto** .
3. Selezionare il **modelli**  - &gt; **Visual c#**  - &gt; **Web** gruppo di modelli a sinistra.
4. Scegliere il **applicazione Web ASP.NET** modello nella colonna centrale.
5. Denominare il progetto ***BasicWebApp*** e fare clic sui **OK** pulsante.   
![Finestra di dialogo Nuovo progetto](creating-a-basic-web-forms-page/_static/image2.png)
6. Selezionare quindi il **Web Form** modello, quindi scegliere il **OK** pulsante per creare il progetto.  
![Finestra di dialogo Nuovo progetto ASP.NET](creating-a-basic-web-forms-page/_static/image3.png)  

    Visual Studio crea un nuovo progetto che include funzionalità predefinite in base al modello Web Form. Fornisce non solo con un *ridefinisce* pagina, un *About* pagina, una *aspx* pagina, ma include anche funzionalità di appartenenza che registra gli utenti e Salva le credenziali in modo che è possibile accedere al sito Web. Quando viene creata una nuova pagina, per impostazione predefinita Visual Studio visualizza la pagina **origine** visualizzazione, in cui è possibile visualizzare gli elementi HTML della pagina. La figura seguente illustra ciò che si vedrebbe nelle **origine** visualizzare se è stata creata una nuova pagina Web denominata *BasicWebApp.aspx*.  
    ![Visualizzazione Origine](creating-a-basic-web-forms-page/_static/image4.png)


### <a name="a-tour-of-the-visual-studio-web-development-environment"></a>Panoramica dell'ambiente di sviluppo Web Visual Studio


Prima di procedere, modificando la pagina, è utile acquisire familiarità con l'ambiente di sviluppo di Visual Studio. Nella figura seguente illustra gli strumenti disponibili in Visual Studio e Visual Studio Express per Web e windows.

> [!NOTE] 
> 
> Questo diagramma illustra windows predefinita e le posizioni delle finestre. Il **vista** menu consente di visualizzare le finestre aggiuntive e ridisporre e ridimensionare le finestre in base alle proprie preferenze. Se sono già state apportate modifiche per la disposizione delle finestre, ciò che viene visualizzato non corrisponderanno nell'illustrazione.


 Ambiente di Visual Studio

![Ambiente di Visual Studio](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a>Acquisire familiarità con la finestra di progettazione Web

Esaminare l'illustrazione precedente e corrisponde al testo per l'elenco seguente, che descrive più comunemente usati strumenti e finestre. (Non tutte le finestre e strumenti che noterete che sono elencati di seguito, solo quelli contrassegnati nella precedente illustrazione.)

- Barre degli strumenti. Specificare i comandi per la formattazione di testo, testo di ricerca e così via. Alcune barre degli strumenti sono disponibili solo quando si utilizza **progettazione** visualizzazione.
- **Esplora soluzioni** finestra. Consente di visualizzare i file e cartelle nell'applicazione Web.
- Finestra del documento. Consente di visualizzare i documenti che si sta lavorando in finestre a schede. È possibile passare tra i documenti facendo clic sulle schede.
- **Proprietà** finestra. Consente di modificare le impostazioni per la pagina, gli elementi HTML, controlli e altri oggetti.
- Consente di visualizzare le schede. Presentare visualizzazioni diverse dello stesso documento. **Progettazione** visualizzazione è un'area di modifica WYSIWYG. **Origine** visualizzazione è l'editor HTML della pagina. **Suddivisione** visualizzazione consente di visualizzare sia il **progettazione** visualizzazione e il **origine** visualizzazione del documento. Verrà usata la **Design** e **origine** viste più avanti in questa procedura dettagliata. Se si preferisce aprire le pagine Web **progettazione** visualizzare, scegliere il **strumenti** dal menu fare clic su **opzioni**, selezionare il **finestra di progettazione HTML** e modificarne il **Apri pagine In** opzione.
- **Casella degli strumenti**. Fornisce i controlli e gli elementi HTML che è possibile trascinare nella pagina. **Casella degli strumenti** gli elementi sono raggruppati dalla funzione comune.
- S **erver Esplora**. Consente di visualizzare le connessioni al database. Se Esplora Server non è visibile, nel menu Visualizza, fare clic su Esplora Server.


### <a name="creating-a-new-aspnet-web-forms-page"></a>Creazione di una nuova pagina ASP.NET Web Form


Quando si crea una nuova applicazione Web Form usando la **applicazione Web ASP.NET** modello di progetto, Visual Studio aggiunge una pagina ASP.NET (pagina Web Form) denominata *default. aspx*, nonché di numerosi altri file e le cartelle. È possibile usare la *default. aspx* pagina come pagina iniziale per l'applicazione Web. Tuttavia, per questa procedura dettagliata, crea e lavorare con una nuova pagina.

### <a name="to-add-a-page-to-the-web-application"></a>Per aggiungere una pagina per l'applicazione Web


1. Chiudi il *default. aspx* pagina. A tale scopo, fare clic sulla scheda che visualizza il nome di file e quindi fare clic sull'opzione chiude.
2. Nella **Esplora soluzioni**, fare clic sul nome dell'applicazione Web (in questa esercitazione è il nome dell'applicazione **BasicWebSite**), quindi fare clic su **Add**  - &gt; **Nuovo elemento**.   
Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
3. Selezionare il **Visual c#**  - &gt; **Web** gruppo di modelli a sinistra. Quindi, selezionare **Web Form** dalla parte centrale elencare e denominarlo *FirstWebPage*.   
    ![Aggiungi finestra di dialogo Nuovo elemento](creating-a-basic-web-forms-page/_static/image6.png)
4. Fare clic su **Add** per aggiungere la pagina web al progetto.  
Visual Studio crea la nuova pagina e lo apre.


### <a name="adding-html-to-the-page"></a>Aggiunta di elementi HTML della pagina


In questa parte della procedura dettagliata, si aggiungerà un testo statico alla pagina.

### <a name="to-add-text-to-the-page"></a>Per aggiungere testo alla pagina


1. Nella parte inferiore della finestra del documento, fare clic sui **Design** pressione di tab per passare alla **progettazione** visualizzazione.

    Visualizzazione progettazione viene visualizzata la pagina corrente in modo simile alla visualizzazione WYSIWYG. A questo punto, non è qualsiasi testo o i controlli nella pagina, in modo che la pagina è vuota, ad eccezione di una linea tratteggiata che descrive un rettangolo. Questo rettangolo rappresenta una **div** elemento della pagina.
2. Fare clic all'interno del rettangolo che è descritti da una linea tratteggiata.
3. Tipo di **Benvenuti a Visual Web Developer** , quindi premere **invio** due volte.

    La figura seguente mostra il testo digitato nel **progettazione** visualizzazione.

    ![Testo di benvenuto nella visualizzazione della struttura](creating-a-basic-web-forms-page/_static/image7.png "testo di benvenuto nella visualizzazione Progettazione")
4. Passare a **origine** visualizzazione.

    È possibile visualizzare il codice HTML nel **origine** vista in cui è stato creato quando è stato digitato nella **progettazione** visualizzazione.  
    ![Pagina Web con testo statico](creating-a-basic-web-forms-page/_static/image8.png)


### <a name="running-the-page"></a>Esecuzione della pagina


Prima di procedere con l'aggiunta di controlli alla pagina, è possibile eseguirlo.

### <a name="to-run-the-page"></a>Per eseguire la pagina


1. Nelle **Esplora soluzioni**, fare doppio clic su *FirstWebPage* e selezionare **imposta come pagina iniziale**.
2. Premere **CTRL+F5** per eseguire la pagina.

    La pagina viene visualizzata nel browser. Anche se la pagina creata ha un'estensione di nome file *aspx*, è attualmente in esecuzione come una qualsiasi pagina HTML.

    Per visualizzare una pagina nel browser è anche possibile fare doppio clic nella pagina **Esplora soluzioni** e selezionare **Visualizza nel Browser**.
3. Chiudere il browser per arrestare l'applicazione Web.


## <a name="adding-and-programming-controls"></a>Aggiunta e programmazione di controlli


<a id="sectionToggle1"></a>

A questo punto si aggiungerà i controlli server alla pagina. Controlli server, ad esempio pulsanti, etichette, caselle di testo e altri controlli comuni, forniscono funzionalità di elaborazione del form tipica per le pagine Web Form. Tuttavia, è possibile programmare i controlli con il codice che viene eseguito sul server, anziché il client.

Si aggiungerà un [sul pulsante](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) (controllo), un [nella casella di testo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) (controllo) e un [etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) alla pagina di controllo e scrivere codice per gestire il [fare clic su](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) eventi per il [pulsante](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) controllo.

### <a name="to-add-controls-to-the-page"></a>Per aggiungere controlli alla pagina


1. Fare clic sui **Design** pressione di tab per passare alla **progettazione** visualizzazione.
2. Inserire il punto di inserimento alla fine del **Benvenuti a Visual Web Developer** testo e premere **invio** cinque o più volte per liberare spazio nel **div** casella dell'elemento.
3. Nel **casella degli strumenti**, espandere il **Standard** gruppo se non è già espanso.  
Si noti che potrebbe essere necessario espandere la **casella degli strumenti** finestra a sinistra per visualizzarlo.
4. Trascinare un [casella di testo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) nella pagina e rilasciarlo al centro della **div** casella elemento che ha **Benvenuti a Visual Web Developer** nella prima riga.
5. Trascinare un [sul pulsante](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) nella pagina e rilasciarla a destra del [nella casella di testo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) controllo.
6. Trascinare un [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) nella pagina e rilasciarlo su una riga separata sotto il [pulsante](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) controllo.
7. Inserire il punto di inserimento precedente la [casella di testo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) controllare e quindi digitare **immettere il nome:** .

    Il testo HTML statico rappresenta la didascalia per il [casella di testo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) controllo. È possibile combinare codice HTML statico e controlli server nella stessa pagina. La figura seguente mostra come i tre controlli vengono visualizzati nella **progettazione** visualizzazione.

    ![Tre controlli nella visualizzazione della struttura](creating-a-basic-web-forms-page/_static/image9.png "tre controlli nella visualizzazione Progettazione")


### <a name="setting-control-properties"></a>Impostazione delle proprietà di controllo


Visual Studio offre diversi modi per impostare le proprietà dei controlli nella pagina. In questa parte della procedura dettagliata, si imposteranno le proprietà in entrambe **Design** visualizzazione e **origine** visualizzazione.

### <a name="to-set-control-properties"></a>Per impostare le proprietà del controllo


1. In primo luogo, visualizzare il **delle proprietà** windows tramite la selezione dal **vista** menu -&gt; **Other Windows**  - &gt; **Proprietà della finestra**. In alternativa, è possibile selezionare **F4** per visualizzare i **proprietà** finestra.
2. Selezionare il [pulsante](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) (controllo) e quindi il **proprietà** finestra, impostare il valore della **testo** a **nome visualizzato**. Il testo immesso verrà visualizzato sul pulsante nella finestra di progettazione, come illustrato nella figura seguente.

    ![Impostare il testo del pulsante](creating-a-basic-web-forms-page/_static/image10.png "testo pulsante Imposta")
3. Passare a **origine** visualizzazione.

    **Origine** Vista consente di visualizzare il codice HTML della pagina, inclusi gli elementi che Visual Studio ha creato per i controlli server. I controlli vengono dichiarati usando la sintassi simile a HTML, ad eccezione del fatto che i tag di usano il prefisso **asp:** e includere l'attributo **runat =&quot;server&quot;**.

    Le proprietà del controllo vengono dichiarate come attributi. Ad esempio, quando si imposta la [testo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx) proprietà per il [pulsante](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) controllo, nel passaggio 1, in effetti si imposta la **testo** attributo nel markup del controllo.

    > [!NOTE] 
    > 
    > Tutti i controlli sono all'interno di un **form** elemento che dispone anche dell'attributo **runat =&quot;server&quot;**. Il **runat =&quot;server&quot;**  attributo e il **asp:** prefisso per i tag di controllo contrassegnano i controlli in modo che vengono elaborati da ASP.NET nel server quando viene eseguita la pagina. Di fuori del codice **&lt;form runat =&quot;server&quot; &gt;** e **&lt;script runat =&quot;server&quot; &gt;** elementi viene inviato invariati al browser, motivo per cui il codice ASP.NET deve trovarsi all'interno di un elemento con tag di apertura contiene il **runat =&quot;server&quot;**  attributo.
4. Successivamente, si aggiungerà una proprietà aggiuntiva per i [etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) controllo. Il punto di inserimento immediatamente dopo aver **asp: Label** nel **&lt;asp: Label&gt;** tag e quindi premere **barra spaziatrice**.

    Viene visualizzato un elenco di riepilogo a discesa che visualizza l'elenco delle proprietà disponibili, è possibile impostare per un [etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) controllo. Questa funzionalità, definita come **IntelliSense**, consente in **origine** visualizzazione con la sintassi dei controlli server, gli elementi HTML e altri elementi nella pagina. La figura seguente mostra le **IntelliSense** elenco a discesa per il [etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) controllo.

    ![Attributi IntelliSense](creating-a-basic-web-forms-page/_static/image11.png "attributi IntelliSense")
5. Selezionare **ForeColor** e quindi digitare un segno di uguale.

    IntelliSense visualizza un elenco di colori.

    > [!NOTE] 
    > 
    > È possibile visualizzare un **IntelliSense** elenco a discesa in qualsiasi momento premendo **CTRL + J** durante la visualizzazione codice.
6. Selezionare un colore per il **[Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)** testo del controllo. Assicurarsi di che selezionare un colore che è sufficientemente scuro per lettura su uno sfondo bianco.

    Il **ForeColor** attributo è stato completato con il colore che è selezionato, incluse le virgolette di chiusura.


### <a name="programming-the-button-control"></a>Programmazione del controllo Button


Per questa procedura dettagliata, si scriverà codice che legge il nome che l'utente immette la casella di testo e quindi Visualizza il nome nel [etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) controllo.

### <a name="add-a-default-button-event-handler"></a>Aggiungere un gestore eventi predefinito


1. Passare a **progettazione** visualizzazione.
2. Fare doppio clic il [pulsante](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) controllo.

    Per impostazione predefinita, Visual Studio passa a un file code-behind e crea una struttura del gestore eventi per il [sul pulsante](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) evento predefinito del controllo, il [fare clic su](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) evento. Il file code-behind separa il markup dell'interfaccia utente (ad esempio HTML) dal codice server (ad esempio c#).   
   Il cursore viene posizionato aggiungere codice per questo gestore dell'evento.

    > [!NOTE] 
    > 
    > Fare doppio clic su un controllo nella **progettazione** visualizzazione è solo uno dei diversi modi per creare gestori eventi.
3. All'interno di **Button1\_fare clic su** gestore eventi, digitare **Label1** seguiti da un punto (**.**).

    Quando si digita il punto dopo il **ID** dell'etichetta (**Label1**), Visual Studio visualizza un elenco di membri disponibili per il [etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) controllare, come illustrato di seguito Nella figura. Un membro in genere una proprietà, metodo o evento.

    ![IntelliSense nella visualizzazione codice](creating-a-basic-web-forms-page/_static/image12.png "IntelliSense nella visualizzazione codice")
4. Completare la **fare clic su** gestore eventi per il pulsante in modo che venga letto come illustrato nell'esempio di codice seguente.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. Tornare alla visualizzazione la **origine** visualizzazione dei tag HTML facendo clic *FirstWebPage* nel **Esplora soluzioni** e selezionando **vista Markup**.
6. Scorrere fino al **&lt;asp: Button&gt;** elemento. Si noti che il **&lt;asp: Button&gt;** elemento ha l'attributo **onclick =&quot;Button1\_fare clic su&quot;**.

    Questo attributo associa il pulsante [fare clic su](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) evento al metodo del gestore di è codificato nel passaggio precedente.

    Metodi del gestore eventi possono avere qualsiasi nome; il nome visualizzato è il nome predefinito creato da Visual Studio. Il punto importante è che il nome utilizzato per il **OnClick** attributo nel codice HTML deve corrispondere al nome di un metodo definito nel code-behind.


### <a name="running-the-page"></a>Esecuzione della pagina


È ora possibile testare i controlli server nella pagina.

### <a name="to-run-the-page"></a>Per eseguire la pagina


1. Premere **CTRL+F5** per eseguirla nel browser. Se si verifica un errore, controllare di nuovo i passaggi precedenti.
2. Immettere un nome nella casella di testo e scegliere il **nome visualizzato** pulsante.

    Il nome immesso viene visualizzato nei [etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) controllo. Si noti che quando si fa clic sul pulsante, la pagina viene registrata nel server Web. In ASP.NET viene ricreata la pagina, viene eseguito il codice (in questo caso, il [sul pulsante](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) del controllo [fare clic su](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) esecuzione del gestore di eventi) e quindi invia la nuova pagina nel browser. Se si osserva la barra di stato nel browser, si noterà che la pagina è in corso un round trip al server Web ogni volta che si fa clic sul pulsante.
3. Nel browser, visualizzare l'origine della pagina si eseguono facendo clic sulla pagina e selezionando **Visualizza origine**.

    Nel codice sorgente della pagina, noterete HTML senza codice server. In particolare, non viene visualizzato il **&lt;asp:&gt;** elementi che si sono lavorando nel **origine** visualizzazione. Quando viene eseguita la pagina, ASP.NET elabora i controlli server ed esegue il rendering di elementi HTML alla pagina di cui eseguono le funzioni che rappresentano il controllo. Ad esempio, il **&lt;asp: Button&gt;** controllo viene eseguito il rendering come HTML **&lt;tipo di input =&quot;invio&quot; &gt;** elemento.
4. Chiudere il browser.


## <a name="working-with-additional-controls"></a>Uso di controlli aggiuntivi

<a id="sectionToggle2"></a>

In questa parte della procedura dettagliata, si utilizzeranno i [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controllo, che consente di visualizzare le date un mese. Il [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) di un controllo più complesso rispetto al pulsante, casella di testo e l'etichetta usano i e illustra alcune altre funzionalità dei controlli server.

In questa sezione si aggiungerà un [controllo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controllo alla pagina e formattarlo.

### <a name="to-add-a-calendar-control"></a>Per aggiungere un controllo di calendario


1. In Visual Studio, passare alla **progettazione** visualizzazione.
2. Dal **Standard** sezione del **della casella degli strumenti**, trascinare un [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) nella pagina e rilasciarlo sotto il **div** elemento che contiene altri controlli.

    Viene visualizzato il pannello di smart tag del calendario. Il pannello mostra i comandi che rendono più semplice per eseguire le attività più comuni per il controllo selezionato. La figura seguente mostra le [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controllano come viene eseguito il rendering nella **progettazione** visualizzazione.

    ![Controllo nella visualizzazione Progettazione calendario](creating-a-basic-web-forms-page/_static/image13.png "controllo nella visualizzazione della struttura di calendario")
3. Nel pannello smart tag, scegliere **formattazione automatica**.

    Il **formattazione automatica** viene visualizzata la finestra di dialogo, che consente di selezionare uno schema di formattazione per il calendario. La figura seguente mostra le **formattazione automatica** finestra di dialogo per il [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controllo.

    ![Finestra di dialogo Formattazione automatica (controllo Calendar)](creating-a-basic-web-forms-page/_static/image14.png "finestra di dialogo Formattazione automatica (controllo Calendar)")
4. Dal **Seleziona schema** , selezionare **semplice** e quindi fare clic su **OK**.
5. Passare a **origine** visualizzazione.

    È possibile vedere le **&lt;asp: calendario&gt;** elemento. Questo elemento è molto più tempo rispetto agli elementi per i controlli semplici creato in precedenza. Contiene inoltre i sottoelementi, come ad esempio  **&lt;WeekEndDayStyle&gt;**, che rappresentano varie impostazioni di formattazione. La figura seguente mostra la [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controllare nei **origine** visualizzazione. (Il markup esatto presenti **origine** vista potrebbe essere leggermente diverso rispetto a questa figura.)

    ![Controllo nella visualizzazione origine calendario](creating-a-basic-web-forms-page/_static/image15.png "controllo nella visualizzazione origine calendario")


### <a name="programming-the-calendar-control"></a>Programmazione del controllo di calendario


In questa sezione sarà programmato il [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controllo per visualizzare la data attualmente selezionata.

### <a name="to-program-the-calendar-control"></a>Programmare il controllo di calendario


1. In **Design** visualizzare, fare doppio clic il [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controllo.

    Un nuovo gestore di evento verrà creato e visualizzato nel file code-behind denominato *FirstWebPage.aspx.cs*.
2. Completare la [SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx) gestore dell'evento con il codice seguente.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]


    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

    Il codice precedente imposta il testo del controllo etichetta per la data selezionata del controllo calendario.


### <a name="running-the-page"></a>Esecuzione della pagina


È ora possibile testare il calendario.

### <a name="to-run-the-page"></a>Per eseguire la pagina


1. Premere **CTRL+F5** per eseguirla nel browser.
2. Fare clic su una data nel calendario.

    La data selezionata viene visualizzata nei [etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) controllo.
3. Nel browser, visualizzare il codice sorgente per la pagina.

    Si noti che il [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controllo è stato eseguito il rendering alla pagina come una **tabella**, con ogni giorno come un **td** elemento.
4. Chiudere il browser.


## <a name="next-steps"></a>Passaggi successivi


<a id="nextStepsToggle"></a>

Questa procedura dettagliata ha illustrato le funzionalità di base della finestra di progettazione pagina Visual Studio. Dopo avere appreso come creare e modificare una pagina Web Form in Visual Studio, è possibile esplorare altre funzionalità. Potrebbe ad esempio, si desidera eseguire le operazioni seguenti:

- Altre informazioni sui Web Form ASP.NET, seguendo la serie di esercitazioni dettagliata [Introduzione a Web Form ASP.NET 4.5 e Visual Studio 2013](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md).
- Altre informazioni sui fogli di stile (CSS). Per informazioni dettagliate, vedere [utilizzo di CSS Overview](https://msdn.microsoft.com/library/bb398931.aspx).
