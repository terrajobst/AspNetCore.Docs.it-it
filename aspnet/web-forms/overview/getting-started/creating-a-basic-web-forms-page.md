---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: Creazione di una base ASP.NET 4.5 pagina Web Form in Visual Studio 2013 | Documenti Microsoft
author: Erikre
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: 6b699cc939292b7ab0167dba7cfa6a00b681ef3a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-basic-aspnet-45-web-forms-page-in-visual-studio-2013"></a>Creazione di una base ASP.NET 4.5 pagina Web Form in Visual Studio 2013
====================
Da [Erik Reitan](https://github.com/Erikre)

Questa procedura dettagliata viene fornita un'introduzione all'ambiente di sviluppo Web in [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) e [Microsoft Visual Studio Express 2013 per Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Questa procedura dettagliata viene illustrata la creazione di una semplice pagina Web Form ASP.NET e vengono illustrate le tecniche di base di creazione di una nuova pagina, aggiunta di controlli e la scrittura di codice.

Le attività illustrate nella procedura dettagliata sono le seguenti:

- Creazione di un progetto di applicazione Web Form di file system.
- Acquisire familiarità con Visual Studio.
- Creazione di una pagina ASP.NET.
- Aggiunta di controlli.
- Aggiunta di gestori eventi.
- Esecuzione test di una pagina da Visual Studio.

## <a name="prerequisites"></a>Prerequisiti


Per completare questa procedura dettagliata, è necessario:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) o [Microsoft Visual Studio Express 2013 per Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework viene installato automaticamente. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 e Microsoft Visual Studio Express 2013 per Web verrà essere noto anche come Visual Studio in tutta questa serie di esercitazioni.  
    >   
    > Se si utilizza Visual Studio, questa procedura dettagliata si presuppone che sia selezionato il **lo sviluppo Web** raccolta di impostazioni la prima volta che viene avviato Visual Studio. Per ulteriori informazioni, vedere [come: Seleziona impostazioni ambiente di sviluppo Web](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="creating-a-web-application-project-and-a-page"></a>Creazione di un progetto di applicazione Web e una pagina

<a id="sectionToggle0"></a>

In questa parte della procedura dettagliata, si verrà creato un progetto di applicazione Web e aggiungere una nuova pagina. Vengono verrà inoltre aggiungere testo HTML e la pagina nel browser.

### <a name="to-create-a-web-application-project"></a>Per creare un progetto di applicazione Web

1. Aprire Microsoft Visual Studio.
2. Nel menu **File** selezionare **Nuovo progetto**.  
    ![Dal Menu file](creating-a-basic-web-forms-page/_static/image1.png)

    Verrà visualizzata la finestra di dialogo **Nuovo progetto** .
3. Selezionare il **modelli**  - &gt; **Visual c#**  - &gt; **Web** gruppo di modelli a sinistra.
4. Scegliere il **applicazione Web ASP.NET** modello nella colonna centrale.
5. Denominare il progetto ***BasicWebApp*** e fare clic su di **OK** pulsante.   
![Finestra di dialogo Nuovo progetto](creating-a-basic-web-forms-page/_static/image2.png)
6. Selezionare quindi il **Web Form** modello, quindi scegliere il **OK** pulsante per creare il progetto.  
![Finestra di dialogo Nuovo progetto ASP.NET](creating-a-basic-web-forms-page/_static/image3.png)  

    Visual Studio crea un nuovo progetto che include funzionalità predefinite in base al modello Web Form. Fornisce non solo con un *aspx* pagina, un *About* pagina, un *Contact.aspx* pagina, ma include anche funzionalità di appartenenza che registra gli utenti e Salva le credenziali in modo che l'accesso al sito Web. Quando viene creata una nuova pagina, per impostazione predefinita verrà visualizzata la pagina in **origine** visualizzazione, in cui è possibile visualizzare gli elementi HTML della pagina. Nella figura seguente viene illustrato quanto visualizzato nelle **origine** visualizzare se è stata creata una nuova pagina Web denominata *BasicWebApp.aspx*.  
    ![Visualizzazione Origine](creating-a-basic-web-forms-page/_static/image4.png)


### <a name="a-tour-of-the-visual-studio-web-development-environment"></a>Una panoramica dell'ambiente di sviluppo Web di Visual Studio


Prima di procedere modificando la pagina, è utile acquisire familiarità con l'ambiente di sviluppo di Visual Studio. Nella figura seguente mostra le finestre e gli strumenti disponibili in Visual Studio e Visual Studio Express per Web.

> [!NOTE] 
> 
> Questo diagramma mostra predefinito di windows e le posizioni delle finestre. Il **vista** menu consente di visualizzare le finestre aggiuntive per ridisporre e ridimensionare le finestre in base alle proprie preferenze. Se sono già state apportate modifiche a disposizione della finestra, viene visualizzato non corrisponderanno nella figura.


 Ambiente di Visual Studio

![Ambiente di Visual Studio](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a>Acquisire familiarità con la finestra di progettazione Web

Esaminare l'illustrazione precedente e il testo corrisponda nell'elenco seguente, che descrive più comune di windows e gli strumenti utilizzati. (Non tutte le finestre e gli strumenti visualizzati sono elencati di seguito, solo quelli contrassegnati nell'illustrazione precedente.)

- Barre degli strumenti. Specificare i comandi per la formattazione di testo, testo di ricerca e così via. Alcune barre degli strumenti sono disponibili solo quando si lavora in **progettazione** visualizzazione.
- **Esplora soluzioni** finestra. Consente di visualizzare i file e cartelle nell'applicazione Web.
- Finestra del documento. Consente di visualizzare i documenti in cui che si sta lavorando in finestre a schede. È possibile passare tra i documenti facendo clic sulle schede.
- **Proprietà** finestra. Consente di modificare le impostazioni per la pagina, gli elementi HTML, controlli e altri oggetti.
- Consente di visualizzare le schede. Presentare viste diverse dello stesso documento. **Progettazione** Vista è una superficie di modifica WYSIWYG. **Origine** visualizzazione è l'editor HTML per la pagina. **Split** Visualizza sia la **progettazione** Vista e **origine** visualizzazione del documento. Si utilizzerà il **progettazione** e **origine** viste, più avanti in questa procedura dettagliata. Se si vuole aprire le pagine Web in **progettazione** nella visualizzazione il **strumenti** menu, fare clic su **opzioni**, selezionare il **finestra di progettazione HTML** e modificarne il **Apri pagine In** opzione.
- **Casella degli strumenti**. Fornisce i controlli e gli elementi HTML che è possibile trascinare nella pagina. **Casella degli strumenti** gli elementi sono raggruppati per funzione.
- S **erver Esplora**. Consente di visualizzare le connessioni al database. Se Esplora Server non è visibile, dal menu Visualizza, fare clic su Esplora Server.


### <a name="creating-a-new-aspnet-web-forms-page"></a>Creazione di una nuova pagina ASP.NET Web Form


Quando si crea una nuova applicazione Web Form mediante la **applicazione Web ASP.NET** modello di progetto, Visual Studio aggiunge una pagina ASP.NET (pagina Web Form) denominata *Default.aspx*, nonché come numerosi altri file e le cartelle. È possibile utilizzare il *Default.aspx* pagina come pagina iniziale per l'applicazione Web. Tuttavia, per questa procedura dettagliata, verranno creare e lavorare con una nuova pagina.

### <a name="to-add-a-page-to-the-web-application"></a>Per aggiungere una pagina all'applicazione Web


1. Chiudi il *Default.aspx* pagina. A tale scopo, fare clic sulla scheda che visualizza il nome di file e quindi fare clic sull'opzione di chiusura.
2. In **Esplora soluzioni**, fare doppio clic sul nome dell'applicazione Web (in questa esercitazione è il nome dell'applicazione **BasicWebSite**), quindi fare clic su **Aggiungi**  - &gt; **Nuovo elemento**.   
Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
3. Selezionare il **Visual c#**  - &gt; **Web** gruppo di modelli a sinistra. Selezionare quindi **Web Form** dal centro elenco e denominarlo *FirstWebPage*.   
    ![Aggiungere la finestra di dialogo Nuovo elemento](creating-a-basic-web-forms-page/_static/image6.png)
4. Fare clic su **Aggiungi** per aggiungere la pagina web al progetto.  
Visual Studio crea la nuova pagina e viene aperto.


### <a name="adding-html-to-the-page"></a>Aggiunta di elementi HTML della pagina


In questa parte della procedura dettagliata, si aggiungerà un testo statico alla pagina.

### <a name="to-add-text-to-the-page"></a>Per aggiungere testo alla pagina


1. Nella parte inferiore della finestra del documento, fare clic su di **progettazione** tab per passare a **progettazione** visualizzazione.

    Visualizzazione Progettazione consente di visualizzare la pagina corrente in modo simile alla visualizzazione WYSIWYG. A questo punto, non è il testo o i controlli della pagina, pertanto la pagina è vuota, ad eccezione di una linea tratteggiata che descrive un rettangolo. Questo rettangolo rappresenta un **div** elemento nella pagina.
2. Fare clic all'interno del rettangolo specificato da una linea tratteggiata.
3. Tipo **Benvenuti a Visual Web Developer** e premere **invio** due volte.

    La figura seguente mostra il testo digitato in **progettazione** visualizzazione.

    ![Testo di benvenuto in visualizzazione Progettazione](creating-a-basic-web-forms-page/_static/image7.png "testo di benvenuto in visualizzazione Progettazione")
4. Passare a **origine** visualizzazione.

    È possibile visualizzare il codice HTML in **origine** vista creata quando si è digitato **progettazione** visualizzazione.  
    ![Pagina Web con testo statico](creating-a-basic-web-forms-page/_static/image8.png)


### <a name="running-the-page"></a>Esecuzione della pagina


Prima di procedere con l'aggiunta di controlli alla pagina, è possibile eseguirlo.

### <a name="to-run-the-page"></a>Per eseguire la pagina


1. In **Esplora**, fare doppio clic su *FirstWebPage* e selezionare **imposta come pagina iniziale**.
2. Premere **CTRL + F5** per eseguire la pagina.

    Verrà visualizzata la pagina nel browser. Anche se la pagina è stato creato con un'estensione di file di *aspx*, è attualmente in esecuzione come una qualsiasi pagina HTML.

    Per visualizzare una pagina nel browser è anche possibile fare doppio clic nella pagina **Esplora** e selezionare **Visualizza nel Browser**.
3. Chiudere il browser per arrestare l'applicazione Web.


## <a name="adding-and-programming-controls"></a>Aggiunta e programmazione di controlli


<a id="sectionToggle1"></a>

A questo punto si aggiungerà i controlli server alla pagina. Controlli di server, ad esempio pulsanti, etichette, caselle di testo e altri controlli comuni, forniscono funzionalità di elaborazione dei moduli tipico per le pagine Web Form. Tuttavia, è possibile programmare i controlli con il codice che viene eseguito sul server, invece del client.

Si aggiungerà un [pulsante](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) (controllo), un [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) (controllo) e un [etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) alla pagina di controllo e scrivere codice per gestire il [fare clic su](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) evento per il [pulsante](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) controllo.

### <a name="to-add-controls-to-the-page"></a>Per aggiungere controlli alla pagina


1. Fare clic su di **progettazione** tab per passare a **progettazione** visualizzazione.
2. Posizionare il punto di inserimento alla fine del **Benvenuti a Visual Web Developer** testo e premere **invio** cinque o più volte per creare spazio nel **div** casella dell'elemento.
3. Nel **della casella degli strumenti**, espandere il **Standard** gruppo se non è già espanso.  
Si noti che potrebbe essere necessario espandere il **della casella degli strumenti** finestra a sinistra per visualizzarlo.
4. Trascinare un [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) controllo nella pagina e rilasciarlo al centro del **div** casella elemento contenente **Benvenuti a Visual Web Developer** nella prima riga.
5. Trascinare un [pulsante](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) controllo nella pagina e rilasciarlo a destra del [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) controllo.
6. Trascinare un [etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) nella pagina e rilasciarlo su una riga separata sotto il [pulsante](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) controllo.
7. Posizionare il cursore sopra la [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) controllare e quindi digitare **immettere il nome:** .

    Il testo statico HTML rappresenta la didascalia per il [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) controllo. È possibile combinare HTML statico e i controlli server nella stessa pagina. La figura seguente mostra come i tre controlli visualizzati nel **progettazione** visualizzazione.

    ![Tre controlli nella visualizzazione Progettazione](creating-a-basic-web-forms-page/_static/image9.png "tre controlli nella visualizzazione Progettazione")


### <a name="setting-control-properties"></a>Impostazione delle proprietà di controllo


Visual Studio offre vari modi per impostare le proprietà dei controlli della pagina. In questa parte della procedura dettagliata, si imposteranno le proprietà in entrambi **progettazione** Vista e **origine** visualizzazione.

### <a name="to-set-control-properties"></a>Per impostare le proprietà del controllo


1. Prima di tutto, visualizzare il **proprietà** windows selezionando il **vista** menu -&gt; **altre finestre**  - &gt; **Finestra mostrarle**. In alternativa, è possibile selezionare **F4** per visualizzare il **proprietà** finestra.
2. Selezionare il [pulsante](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) (controllo) e quindi il **proprietà** finestra, impostare il valore di **testo** per **nome visualizzato**. Il testo immesso viene visualizzato sul pulsante nella finestra di progettazione, come illustrato nella figura seguente.

    ![Impostare il testo del pulsante](creating-a-basic-web-forms-page/_static/image10.png "testo del pulsante impostato")
3. Passare a **origine** visualizzazione.

    **Origine** consente di visualizzare il codice HTML della pagina, inclusi gli elementi creati tramite Visual Studio per i controlli server. I controlli vengono dichiarati utilizzando sintassi HTML, ad eccezione del fatto che i tag di utilizzano il prefisso **asp:** e includere l'attributo **runat =&quot;server&quot;**.

    Le proprietà del controllo vengono dichiarate come attributi. Ad esempio, quando si imposta la [testo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx) proprietà per il [pulsante](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) di controllo, nel passaggio 1, in effetti si imposta la **testo** attributo nel markup del controllo.

    > [!NOTE] 
    > 
    > Tutti i controlli siano all'interno di un **modulo** elemento che dispone anche dell'attributo **runat =&quot;server&quot;**. Il **runat =&quot;server&quot;**  attributo e **asp:** prefisso per il tag di controllo Contrassegna i controlli in modo che vengono elaborate da ASP.NET nel server all'esecuzione della pagina. Codice di fuori di  **&lt;modulo runat =&quot;server&quot; &gt;**  e  **&lt;script runat =&quot;server&quot; &gt;**  inviato inalterato al browser, motivo per cui il codice ASP.NET deve trovarsi all'interno di un elemento il cui tag di apertura contiene gli elementi di **runat =&quot;server&quot;**  attributo.
4. Successivamente, si aggiungerà una proprietà aggiuntiva per il [etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) controllo. Il punto di inserimento immediatamente dopo **asp: Label** nel  **&lt;asp: Label&gt;**  tag e quindi premere **barra spaziatrice**.

    Viene visualizzato un elenco di riepilogo a discesa che visualizza l'elenco delle proprietà disponibili, è possibile impostare per un [etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) controllo. Questa funzionalità, definita come **IntelliSense**, consente in **origine** visualizzazione con la sintassi di controlli server, gli elementi HTML e altri elementi nella pagina. La figura seguente mostra il **IntelliSense** elenco a discesa per la [etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) controllo.

    ![Attributi IntelliSense](creating-a-basic-web-forms-page/_static/image11.png "attributi IntelliSense")
5. Selezionare **ForeColor** e quindi digitare un segno di uguale.

    IntelliSense consente di visualizzare un elenco di colori.

    > [!NOTE] 
    > 
    > È possibile visualizzare un **IntelliSense** elenco a discesa in qualsiasi momento premendo **CTRL + J** durante la visualizzazione di codice.
6. Selezionare un colore per il  **[etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)**  testo del controllo. Assicurarsi di selezionare un colore scuro abbastanza da leggere su uno sfondo bianco.

    Il **ForeColor** attributo viene completato con il colore selezionato, incluse le virgolette di chiusura.


### <a name="programming-the-button-control"></a>Programmazione del controllo Button


Per questa procedura dettagliata, è necessario scrivere codice che legge il nome utente immesso nella casella di testo e quindi Visualizza il nome nel [etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) controllo.

### <a name="add-a-default-button-event-handler"></a>Aggiungere un gestore eventi del pulsante predefinito


1. Passare a **progettazione** visualizzazione.
2. Fare doppio clic su di [pulsante](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) controllo.

    Per impostazione predefinita, Visual Studio passa a un file code-behind e crea una struttura del gestore eventi per il [pulsante](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) evento predefinito del controllo, il [fare clic su](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) evento. Il file code-behind separa il markup dell'interfaccia utente (ad esempio HTML) dal codice server (ad esempio c#).   
Il cursore viene posizionato aggiungere codice per questo gestore eventi.

    > [!NOTE] 
    > 
    > Fare doppio clic su un controllo in **progettazione** Vista è solo uno dei modi diversi, è possibile creare gestori eventi.
3. All'interno di **Button1\_fare clic su** gestore eventi, digitare **Label1** seguiti da un punto (**.**).

    Quando si digita il periodo dopo il **ID** dell'etichetta (**Label1**), Visual Studio visualizza un elenco di membri disponibili per il [etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) controllare, come illustrato di seguito Figura. Un membro comunemente una proprietà, un metodo o un evento.

    ![IntelliSense nella visualizzazione codice](creating-a-basic-web-forms-page/_static/image12.png "IntelliSense nella visualizzazione codice")
4. Fine di **fare clic su** gestore eventi per il pulsante in modo che venga visualizzata come illustrato nell'esempio di codice seguente.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. Tornare alla visualizzazione la **origine** visualizzazione del codice HTML facendo clic *FirstWebPage* nel **Esplora** e selezionando **visualizzazione Markup**.
6. Scorrere verso il  **&lt;asp: Button&gt;**  elemento. Si noti che il  **&lt;asp: Button&gt;**  elemento ha l'attributo **onclick =&quot;Button1\_fare clic su&quot;**.

    Questo attributo consente di associare il pulsante [fare clic su](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) evento al metodo del gestore codificato nel passaggio precedente.

    I metodi del gestore eventi possono avere qualsiasi nome; il nome visualizzato è il nome predefinito creato da Visual Studio. L'aspetto importante è che il nome utilizzato per il **OnClick** attributo nel codice HTML deve corrispondere al nome di un metodo definito nel code-behind.


### <a name="running-the-page"></a>Esecuzione della pagina


È ora possibile testare i controlli server nella pagina.

### <a name="to-run-the-page"></a>Per eseguire la pagina


1. Premere **CTRL + F5** per eseguire la pagina nel browser. Se si verifica un errore, verificare di nuovo i passaggi precedenti.
2. Immettere un nome nella casella di testo e fare clic su di **nome visualizzato** pulsante.

    Il nome immesso viene visualizzato nel [etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) controllo. Si noti che quando si fa clic sul pulsante, la pagina viene registrata nel server Web. In ASP.NET viene ricreata la pagina, viene eseguito il codice (in questo caso, il [pulsante](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) del controllo [fare clic su](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) eseguito gestore eventi), quindi invia la nuova pagina nel browser. Osservando la barra di stato nel browser, si noterà che la pagina effettua un round trip al server Web ogni volta che si sceglie il pulsante.
3. Nel browser, visualizzare l'origine della pagina esecuzione facendo clic sulla pagina e selezionare **Visualizza origine**.

    Nel codice sorgente della pagina, vedrai HTML senza alcun codice server. In particolare, non viene visualizzato il  **&lt;asp:&gt;**  gli elementi che si sono lavorando con **origine** visualizzazione. Quando la pagina viene eseguita, ASP.NET elabora i controlli server e viene eseguito il rendering HTML per la pagina che eseguire le funzioni che rappresentano il controllo. Ad esempio, il  **&lt;asp: Button&gt;**  controllo viene eseguito il rendering come HTML  **&lt;tipo di input =&quot;inviare&quot; &gt;**  elemento.
4. Chiudere il browser.


## <a name="working-with-additional-controls"></a>Utilizzo dei controlli aggiuntivi

<a id="sectionToggle2"></a>

In questa parte della procedura dettagliata, si utilizzerà il [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controllo, che consente di visualizzare le date un mese. Il [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controllo è un controllo pulsante, casella di testo e di etichetta si lavora con più complesso e vengono illustrate ulteriori funzionalità dei controlli server.

In questa sezione si aggiungerà un [controllo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controllo alla pagina e formattarlo.

### <a name="to-add-a-calendar-control"></a>Per aggiungere un controllo di calendario


1. In Visual Studio, passare a **progettazione** visualizzazione.
2. Dal **Standard** sezione del **della casella degli strumenti**, trascinare un [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) nella pagina e rilasciarlo di sotto di **div** elemento che contiene gli altri controlli.

    Pannello smart tag del calendario viene visualizzato. Nel pannello vengono visualizzati i comandi che semplificano l'esecuzione di attività più comuni per il controllo selezionato. La figura seguente mostra il [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controllano il rendering in **progettazione** visualizzazione.

    ![Controllo nella visualizzazione Progettazione calendario](creating-a-basic-web-forms-page/_static/image13.png "controllo nella visualizzazione progettazione di calendario")
3. Nel pannello smart tag scegliere **formattazione automatica**.

    Il **formattazione automatica** viene visualizzata la finestra di dialogo, che consente di selezionare uno schema di formattazione per il calendario. La figura seguente mostra il **formattazione automatica** la finestra di dialogo per la [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controllo.

    ![Finestra di dialogo Formattazione automatica (controllo Calendar)](creating-a-basic-web-forms-page/_static/image14.png "la finestra di dialogo Formattazione automatica (controllo Calendar)")
4. Dal **selezionare uno schema** selezionare **semplice** e quindi fare clic su **OK**.
5. Passare a **origine** visualizzazione.

    È possibile visualizzare il  **&lt;asp: Calendar&gt;**  elemento. Questo elemento è molto più tempo rispetto agli elementi per i controlli semplici, creato in precedenza. Inoltre, include sottoelementi, ad esempio  **&lt;WeekEndDayStyle&gt;**, che rappresentano varie impostazioni di formattazione. Nella figura seguente il [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controllo **origine** visualizzazione. (Il markup esatto che viene visualizzato in **origine** vista potrebbe essere leggermente diverso rispetto a questa figura.)

    ![Controllo nella visualizzazione origine calendario](creating-a-basic-web-forms-page/_static/image15.png "controllo nella visualizzazione origine calendario")


### <a name="programming-the-calendar-control"></a>Programmazione del controllo di calendario


In questa sezione sarà programmato il [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controllo per visualizzare la data attualmente selezionata.

### <a name="to-program-the-calendar-control"></a>Programmare il controllo di calendario


1. In **progettazione** visualizzare, fare doppio clic su di [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controllo.

    Un nuovo gestore eventi viene creato e visualizzato nel file di codice denominato *FirstWebPage.aspx.cs*.
2. Fine di [SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx) gestore dell'evento con il codice seguente.


    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]


    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

 Il codice precedente imposta il testo del controllo etichetta per la data selezionata del controllo calendario.


### <a name="running-the-page"></a>Esecuzione della pagina


È ora possibile testare il calendario.

### <a name="to-run-the-page"></a>Per eseguire la pagina


1. Premere **CTRL + F5** per eseguire la pagina nel browser.
2. Fare clic su una data nel calendario.

    Viene visualizzata la data selezionata nel [etichetta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) controllo.
3. Nel browser, visualizzare il codice sorgente per la pagina.

    Si noti che il [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) è stato eseguito rendering del controllo alla pagina come un **tabella**, con ogni giorno come un **td** elemento.
4. Chiudere il browser.


## <a name="next-steps"></a>Passaggi successivi


<a id="nextStepsToggle"></a>

Questa procedura dettagliata è state illustrate le funzionalità di base della finestra di progettazione di pagina di Visual Studio. Ora che è comprendere come creare e modificare una pagina Web Form in Visual Studio, è consigliabile esplorare altre funzionalità. Potrebbe ad esempio, si desidera eseguire le operazioni seguenti:

- Per ulteriori informazioni su Web Form ASP.NET, seguendo la serie di esercitazioni dettagliate [Introduzione a Web Form ASP.NET 4.5 e Visual Studio 2013](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md).
- Ulteriori informazioni sui fogli di stile (CSS). Per informazioni dettagliate, vedere [Working with CSS Overview](https://msdn.microsoft.com/library/bb398931.aspx).
